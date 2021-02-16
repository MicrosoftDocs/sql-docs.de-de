---
title: Rotieren von Always Encrypted-Schlüsseln mithilfe von SQL Server Management Studio | Microsoft-Dokumentation
description: Erfahren Sie mehr über das Rotieren von Always Encrypted-Spaltenhauptschlüsseln und -Spaltenverschlüsselungsschlüsseln mit SQL Server Management Studio.
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.COLUMNMASTERKEY.ROTATION.F1
- sql13.SWB.COLUMNMASTERKEY.CLEANUP.F1
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 249e0d2bc53083839787543723fc16dc11a4ff57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340129"
---
# <a name="rotate-always-encrypted-keys-using-sql-server-management-studio"></a>Rotieren von Always Encrypted-Schlüsseln mithilfe von SQL Server Management Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

In diesem Artikel werden Aufgaben zum Rotieren von Always Encrypted-Spaltenhauptschlüsseln und -Spaltenverschlüsselungsschlüsseln mit [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md) beschrieben.

Einen Überblick über die Always Encrypted-Schlüsselverwaltung einschließlich bewährter Methoden und wichtiger Sicherheitsüberlegungen finden Sie unter [Übersicht der Schlüsselverwaltung für Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).

<a name="rotatecmk"></a>
## <a name="rotate-column-master-keys"></a>Rotieren von Spaltenhauptschlüsseln 

Die Rotation eines Spaltenhauptschlüssels ist der Prozess des Ersetzens eines vorhandenen Spaltenhauptschlüssels durch einen neuen. Sie müssen einen Schlüssel möglicherweise rotieren, wenn er kompromittiert wurde, oder um die Richtlinien und Kompatibilitätsbestimmungen Ihrer Organisation einzuhalten, die die regelmäßige Rotation kryptografischer Schlüssel vorschreiben. Die Rotation eines Spaltenhauptschlüssels umfasst die Entschlüsselung von Spaltenverschlüsselungsschlüsseln, die mit dem aktuellen Spaltenhauptschlüssel geschützt werden, deren Neuverschlüsselung mithilfe des neuen Spaltenhauptschlüssels und das Aktualisieren der Schlüsselmetadaten. 

### <a name="step-1-provision-a-new-column-master-key"></a>Schritt 1: Bereitstellen eines neuen Spaltenhauptschlüssels

Führen Sie die Schritte unter [Bereitstellen von Spaltenhauptschlüsseln mit dem Dialogfeld „Neuer Spaltenhauptschlüssel“](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog) aus.

### <a name="step-2-encrypt-column-encryption-keys-with-the-new-column-master-key"></a>Schritt 2: Verschlüsseln von Spaltenverschlüsselungsschlüsseln mit dem neuen Spaltenhauptschlüssel

Ein Spaltenhauptschlüssel schützt in der Regel mindestens einen Spaltenverschlüsselungsschlüssel. Jeder Spaltenverschlüsselungsschlüssel verfügt über einen verschlüsselten Wert, der in der Datenbank gespeichert wird und das Produkt der Verschlüsselung des Spaltenverschlüsselungsschlüssels mithilfe des Spaltenhauptschlüssels ist.
In diesem Schritt müssen Sie jeden Spaltenverschlüsselungsschlüssel, der mit dem aktuellen (zu rotierenden) Spaltenhauptschlüssel geschützt ist, mit dem neuen Spaltenhauptschlüssel verschlüsseln, und den neuen verschlüsselten Wert in der Datenbank speichern. Daher verfügt jeder von der Rotation betroffene Spaltenverschlüsselungsschlüssel über zwei verschlüsselte Werte: ein Wert, der mit dem vorhandenen Spaltenhauptschlüssel verschlüsselt wurde, und ein neuer Wert, der mit dem neuen Spaltenhauptschlüssel verschlüsselt wurde.

1.  Navigieren Sie über den **Objekt-Explorer** zu dem Ordner **Sicherheit > Always Encrypted-Schlüssel > Spaltenhauptschlüssel**, und suchen Sie den Spaltenhauptschlüssel, den Sie rotieren möchten.
2.  Klicken Sie mit der rechten Maustaste auf den Spaltenhauptschlüssel, und wählen Sie **Rotieren** aus.
3.  Wählen Sie im Dialogfeld **Column Master Key Rotation** (Rotation der Spaltenhauptschlüssel) den Namen Ihres neuen Spaltenhauptschlüssels im Feld **Ziel** aus, den Sie in Schritt 1 erstellt haben.
4.  Überprüfen Sie die Liste der Spaltenverschlüsselungsschlüssel, die durch den vorhanden Spaltenhauptschlüssel geschützt sind. Die Rotation wirkt sich auf diese Schlüssel aus.
5.  Klicken Sie auf **OK**.

SQL Server Management Studio erhält die Metadaten der Spaltenverschlüsselungsschlüssel, die mit dem alten Spaltenhauptschlüssel geschützt werden, und die Metadaten der alten und neuen Spaltenhauptschlüssel. SSMS verwendet die Metadaten des Spaltenhauptschlüssels anschließend, um auf den Schlüsselspeicher mit dem alten Spaltenhauptschlüssel zuzugreifen und den bzw. die Spaltenverschlüsselungsschlüssel zu entschlüsseln. SSMS greift daraufhin auf den Schlüsselspeicher mit dem neuen Spaltenhauptschlüssel zu, um einen neuen Satz von verschlüsselten Werten zu erzeugen und diese anschließend zu den Metadaten hinzuzufügen (durch Generieren und Ausgeben von [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md) -Anweisungen).

> [!NOTE]
> Stellen Sie sicher, dass jeder der Spaltenverschlüsselungsschlüssel, der mit dem alten Spaltenhauptschlüssel verschlüsselt wurde, mit keinem anderen Spaltenhauptschlüssel verschlüsselt wird. Das bedeutet, dass jeder durch die Drehung betroffene Spaltenverschlüsselungsschlüssel genau einen verschlüsselten Wert in der Datenbank enthalten muss. Wenn ein betroffener Spaltenverschlüsselungsschlüssel über mehr als einen verschlüsselten Wert verfügt, müssen Sie den Wert entfernen, bevor Sie die Rotation fortsetzen können (Informationen zum Entfernen eines verschlüsselten Werts eines Spaltenverschlüsselungsschlüssels finden Sie in *Schritt 4* ).

### <a name="step-3-configure-your-applications-with-the-new-column-master-key"></a>Schritt 3: Konfigurieren Ihrer Anwendungen mit dem neuen Spaltenhauptschlüssel

In diesem Schritt geht es um alle Ihre Clientanwendungen, die Datenbankspalten abfragen, die mit dem zu rotierenden Spaltenhauptschlüssel geschützt werden (d.h. mit einem Spaltenverschlüsselungsschlüssel verschlüsselt sind, der wiederum mit dem zu rotierenden Spaltenhauptschlüssel verschlüsselt ist). Sie müssen sicherstellen, dass diese Clientanwendungen auf den neuen Spaltenhauptschlüssel zugreifen können. Ihr Vorgehen in diesem Schritt hängt vom Typ des Zertifikatspeichers ab, in dem sich Ihr neuer Spaltenhauptschlüssel befindet. Beispiel:

- Wenn der neue Spaltenhauptschlüssel ein im Windows-Zertifikatspeicher gespeichertes Zertifikat ist, müssen Sie das Zertifikat an dem Zertifikatspeicherort speichern (*Aktueller Benutzer* oder *Lokaler Computer*), der auch im Schlüsselpfad Ihres Spaltenhauptschlüssels in der Datenbank angegeben ist. Die Anwendung muss auf das Zertifikat zugreifen können:
  - Wenn das Zertifikat am Zertifikatspeicherort *Aktueller Benutzer* gespeichert wird, muss das Zertifikat in den Speicherort „Aktueller Benutzer“ der Windows-Identität (Benutzer) der Anwendung importiert werden.
  - Wenn das Zertifikat am Zertifikatspeicherort *Lokaler Computer* gespeichert wird, benötigt die Windows-Identität der Anwendung die Berechtigung zum Zugriff auf das Zertifikat.
- Wenn der neue Spaltenhauptschlüssel im Microsoft Azure Key Vault gespeichert wird, muss die Anwendung implementiert werden, damit sie für Azure authentifiziert werden kann und die Berechtigung zum Zugriff auf den Schlüssel erhält.

Ausführliche Informationen finden Sie unter [Erstellen und Speichern von Spaltenhauptschlüsseln für Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

> [!NOTE]
> Zu diesem Zeitpunkt des Rotationsverfahrens sind sowohl der alte als auch der neue Spaltenhauptschlüssel gültig und können für den Datenzugriff verwendet werden.

### <a name="step-4-clean-up-column-encryption-key-values-encrypted-with-the-old-column-master-key"></a>Schritt 4: Bereinigen der mit dem alten Spaltenhauptschlüssel verschlüsselten Werte des Spaltenhauptschlüssels

Wenn Sie alle Ihre Anwendungen für die Verwendung des neuen Spaltenhauptschlüssels konfiguriert haben, müssen Sie die Werte der Spaltenverschlüsselungsschlüssel aus der Datenbank entfernen, die mit dem *alten* Spaltenhauptschlüssel verschlüsselt wurden. Das Entfernen der alten Werte stellt sicher, dass Sie bereit für den nächsten Rotationsvorgang sind (denken Sie daran, dass jeder Spaltenverschlüsselungsschlüssel, der mit einem zu rotierenden Spaltenhauptschlüssel geschützt wird, über genau einen verschlüsselten Wert verfügen muss).

Ein weiterer Grund dafür, den alten Wert vor dem Archivieren oder Entfernen des alten Spaltenhauptschlüssels zu bereinigen, ist die Leistung: Beim Abfragen einer verschlüsselten Spalte muss ein für Always Encrypted aktivierter Clienttreiber möglicherweise versuchen, zwei Werte zu entschlüsseln – den alten und den neuen Wert. Dem Treiber ist nicht bekannt, welcher der beiden Spaltenhauptschlüssel in der Umgebung der Anwendung gültig ist, weswegen der Treiber beide verschlüsselten Werte vom Server abruft. Wenn bei der Entschlüsselung eines der Werte ein Fehler auftritt, da er mit dem nicht verfügbaren Spaltenhauptschlüssel geschützt ist (wenn es z. B. der alte Spaltenhauptschlüssel ist, der aus dem Speicher entfernt wurde), versucht der Treiber einen anderen Wert mithilfe des neuen Spaltenhauptschlüssels zu entschlüsseln.

> [!WARNING]
> Wenn Sie den Wert eines Spaltenverschlüsselungsschlüssels entfernen, bevor der zugehörige Spaltenhauptschlüssel für eine Anwendung verfügbar gemacht wurde, kann die Anwendung die Datenbankspalte nicht mehr entschlüsseln.

1.  Navigieren Sie über den **Objekt-Explorer** zu dem Ordner **Sicherheit > Always Encrypted-Schlüssel**, und suchen Sie den vorhandenen Spaltenhauptschlüssel, den Sie ersetzen möchten.
2.  Klicken Sie mit der rechten Maustaste auf den vorhandenen Spaltenhauptschlüssel, und wählen Sie **Cleanup** aus.
3.  Überprüfen Sie die Liste der zu entfernenden Werte für den Spaltenhauptschlüssel.
4.  Klicken Sie auf **OK**.

SQL Server Management Studio gibt [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md) -Anweisungen aus, um verschlüsselte Werte von Spaltenverschlüsselungsschlüsseln zu löschen, die mit dem alten Spaltenhauptschlüssel verschlüsselt wurden.

### <a name="step-5-delete-metadata-for-your-old-column-master-key"></a>Schritt 5: Löschen von Metadaten Ihres alten Spaltenhauptschlüssels

Wenn Sie die Definition des alten Spaltenhauptschlüssels aus der Datenbank entfernen möchten, befolgen Sie diese Schritte.

1. Navigieren Sie über den **Objekt-Explorer** zu dem Ordner **Sicherheit > Always Encrypted-Schlüssel > Spaltenhauptschlüssel**, und suchen Sie den alten Spaltenhauptschlüssel, der aus der Datenbank entfernt werden soll.
2. Klicken Sie mit der rechten Maustaste auf den alten Spaltenhauptschlüssel, und wählen Sie **Löschen** aus. (Dadurch wird eine [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md) -Anweisung generiert und ausgegeben, die die Metadaten des Spaltenhauptschlüssels entfernt.)
3. Klicken Sie auf **OK**.

> [!NOTE]
> Es wird dringend empfohlen, dass Sie den alten Spaltenhauptschlüssel nach der Rotation nicht dauerhaft löschen. Stattdessen sollten Sie den alten Spaltenhauptschlüssel im aktuellen Schlüsselspeicher oder an einem anderen sicheren Ort archivieren. Wenn Sie die Datenbank aus einer Sicherungsdatei auf einen Zeitpunkt vor der Konfiguration des neuen Spaltenhauptschlüssels wiederherstellen, benötigen Sie den alten Schlüssel für den Datenzugriff.

### <a name="permissions-for-rotating-column-master-key"></a>Berechtigung für das Rotieren des Spaltenhauptschlüssels

Die Rotation eines Spaltenhauptschlüssels erfordert die folgenden Berechtigungen:

- **ALTER ANY COLUMN MASTER KEY** – Für die Erstellung von Metadaten für den neuen Spaltenhauptschlüssel und die Löschung der Metadaten des alten Spaltenhauptschlüssels erforderlich.
- **ALTER ANY COLUMN ENCRYPTION KEY** – Für die Änderung der Metadaten von Spaltenverschlüsselungsschlüsseln erforderlich (neue verschlüsselte Werte hinzufügen).

Sie müssen sowohl auf den alten als auch auf den neuen Spaltenhauptschlüssel in ihren jeweiligen Schlüsselspeichern zugreifen können. Sie benötigen möglicherweise Berechtigungen für Schlüsselspeicher oder/und den Schlüssel, um auf einen Schlüsselspeicher zugreifen und einen Spaltenhauptschlüssel verwenden zu können:
- **Zertifikatspeicher – Lokaler Computer**: Sie benötigen Lesezugriff auf das Zertifikat, das als Spaltenhauptschlüssel verwendet wird, oder Administratorrechte auf dem Computer.
- **Azure Key Vault**: Sie benötigen die Berechtigungen *create*, *get*, *unwrapKey*, *wrapKey*, *sign* und *verify* für den Tresor, der den bzw. die Spaltenhauptschlüssel enthält.
- **Schlüsselspeicheranbieter (CNG)** : Bei der Verwendung eines Schlüsselspeichers oder Schlüssels werden Sie möglicherweise aufgefordert, die erforderlichen Berechtigungen und Anmeldeinformationen anzugeben, welche von der Konfiguration des Speichers und des KSP abhängen.
- **Kryptografiedienstanbieter (Kryptografie-API)** : Bei der Verwendung eines Schlüsselspeichers oder Schlüssels werden Sie möglicherweise aufgefordert, die erforderlichen Berechtigungen und Anmeldeinformationen anzugeben, welche von der Konfiguration des Speichers und des CSP abhängen.

Weitere Informationen finden Sie unter [Erstellen und Speichern von Spaltenhauptschlüsseln für Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

<a name="rotatecek"></a> 
## <a name="rotate-column-encryption-keys"></a>Rotieren von Spaltenverschlüsselungsschlüsseln

Die Rotation eines Spaltenverschlüsselungsschlüssels umfasst das Entschlüsseln der Daten in allen Spalten, die mit dem zu rotierenden Schlüssel verschlüsselt wurden, und die Neuverschlüsselung der Daten mithilfe des neuen Spaltenverschlüsselungsschlüssels.

>[!NOTE]
> Die Rotation eines Spaltenverschlüsselungsschlüssels kann sehr viel Zeit in Anspruch nehmen, wenn die Tabellen mit den Spalten, die mit dem zu rotierenden Schlüssel verschlüsselt wurden, groß ist. Während die Daten neu verschlüsselt werden, können Ihre Anwendungen nichts in die betroffenen Tabellen schreiben. Daher muss Ihre Organisation eine Rotation der Spaltenverschlüsselungsschlüssel sehr sorgfältig planen.
Verwenden Sie den Always Encrypted-Assistenten, um einen Spaltenverschlüsselungsschlüssel zu rotieren.

1.  Öffnen Sie den Assistenten für Ihre Datenbank: Klicken Sie mit der rechten Maustaste auf Ihre Datenbank, bewegen Sie den Mauszeiger zu **Aufgaben**, und klicken Sie auf **Spalten verschlüsseln**.
2.  Lesen Sie die Seite **Einführung** , und klicken Sie dann auf **Weiter**.
3.  Erweitern Sie auf der Seite **Spaltenauswahl** die Tabellen, und suchen Sie alle Spalten, die Sie ersetzen möchten und die derzeit mit dem alten Spaltenverschlüsselungsschlüssel verschlüsselt sind.
4.  Legen Sie **Verschlüsselungsschlüssel** für jede mit dem alten Verschlüsselungsschlüssel verschlüsselte Spalte auf einen neuen, automatisch generierten Schlüssel fest. **Hinweis:** Alternativ können Sie einen neuen Spaltenverschlüsselungsschlüssel erstellen, bevor Sie den Assistenten ausführen. Informationen dazu finden Sie unter [Bereitstellen von Spaltenverschlüsselungsschlüsseln mit dem Dialogfeld „Neuer Spaltenverschlüsselungsschlüssel“](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog).
5.  Wählen Sie auf der Seite **Konfiguration des Hauptschlüssels** einen Speicherort für den neuen Schlüssel aus, wählen Sie eine Hauptschlüsselquelle aus, und klicken Sie anschließend auf **Weiter**. **Hinweis:** Wenn Sie einen vorhandenen Spaltenverschlüsselungsschlüssel verwenden (keinen automatisch generierten), können Sie diesen Schritt überspringen.
6.  Wählen Sie auf der **Überprüfungsseite** aus, ob das Skript sofort ausgeführt oder ob ein PowerShell-Skript erstellt werden soll, und klicken Sie anschließend auf **Weiter**.
7.  Überprüfen Sie die ausgewählten Optionen auf der Seite **Zusammenfassung**, klicken Sie anschließend auf **Fertig stellen**, und schließen Sie den Assistenten, wenn Sie alle Schritte ausgeführt haben.
8.  Navigieren Sie über den **Objekt-Explorer** zu dem Ordner **Sicherheit &gt; Always Encrypted-Schlüssel &gt; Spaltenverschlüsselungsschlüssel** , und suchen Sie Ihren alten Spaltenverschlüsselungsschlüssel, der aus der Datenbank entfernt werden soll. Klicken Sie mit der rechten Maustaste auf den Schlüssel, und wählen Sie anschließend **Löschen** aus.

### <a name="permissions-for-rotating-column-encryption-keys"></a>Berechtigung zum Rotieren von Spaltenverschlüsselungsschlüsseln

Die Rotation eines Spaltenhauptschlüssels erfordert die folgenden Berechtigungen: **ALTER ANY COLUMN MASTER KEY:** ist erforderlich, wenn Sie einen neuen, automatisch generierten Spaltenverschlüsselungsschlüssel verwenden (ein neuer Spaltenhauptschlüssel und die neuen, dazugehörigen Metadaten werden ebenfalls generiert).
**ALTER ANY COLUMN ENCRYPTION KEY** – Für das Hinzufügen von Metadaten für den neuen Spaltenverschlüsselungsschlüssel erforderlich.

Sie müssen sowohl für den alten als auch für den neuen Spaltenverschlüsselungsschlüssel auf den Spaltenhauptschlüssel zugreifen können. Sie benötigen möglicherweise Berechtigungen für Schlüsselspeicher oder/und den Schlüssel, um auf einen Schlüsselspeicher zugreifen und einen Spaltenhauptschlüssel verwenden zu können:
- **Zertifikatspeicher – Lokaler Computer**: Sie benötigen einen Lesezugriff auf das Zertifikat, das als Spaltenhauptschlüssel verwendet wird, oder Administratorrechte auf dem Computer.
- **Azure Key Vault**: Sie benötigen die Berechtigungen „get“, „unwrapKey“, und „verify“ für den Tresor, der den Spaltenhauptschlüssel enthält.
- **Schlüsselspeicheranbieter (CNG)** : Bei der Verwendung eines Schlüsselspeichers oder Schlüssels werden Sie möglicherweise aufgefordert, die erforderlichen Berechtigungen und Anmeldeinformationen anzugeben, welche von der Konfiguration des Speichers und des KSP abhängen.
- **Kryptografiedienstanbieter (Kryptografie-API)** : Bei der Verwendung eines Schlüsselspeichers oder Schlüssels werden Sie möglicherweise aufgefordert, die erforderlichen Berechtigungen und Anmeldeinformationen anzugeben, welche von der Konfiguration des Speichers und des CSP abhängen.

Weitere Informationen finden Sie unter [Erstellen und Speichern von Spaltenhauptschlüsseln für Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="next-steps"></a>Nächste Schritte
- [Abfragen von Spalten mit Always Encrypted und SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Entwickeln von Anwendungen mit Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>Weitere Informationen
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Übersicht über die Schlüsselverwaltung für Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
- [Konfigurieren von Always Encrypted mithilfe von SQL Server Management Studio](configure-always-encrypted-using-sql-server-management-studio.md)
- [Konfigurieren von Always Encrypted mithilfe von PowerShell](configure-always-encrypted-using-powershell.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)
