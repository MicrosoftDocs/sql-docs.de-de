---
title: Herstellen einer Verbindung mit MySQL (mysqlto SQL) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Verbindung mit einer Ziel-imysql-Datenbank herstellen, um eine MySQL-Datenbank SSMA Ruft Metadaten zu Datenbanken in Azure SQL-Datenbank ab.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connecting to MySQL, MySQL permission
- Connecting to MySQL,reconnecting
ms.assetid: 084c7020-f729-4f91-90e0-143f85fa68d1
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d7f3f46a58fccafa68164d9335c66df10c5b7487
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068895"
---
# <a name="connecting-to-mysql-mysqltosql"></a>Herstellen einer Verbindung mit MySQL (MySqlToSql)
Um MySQL-Datenbanken zu SQL Server oder SQL Azure zu migrieren, müssen Sie eine Verbindung mit der MySQL-Datenbank herstellen, die Sie migrieren möchten. Wenn Sie eine Verbindung herstellen, werden von SSMA Metadaten zu allen MySQL-Schemas abgerufen und anschließend im Bereich MySQL-metadatenexplorer angezeigt. SSMA speichert Informationen über den Datenbankserver, speichert aber keine Kenn Wörter.  
  
Die Verbindung mit der Datenbank bleibt aktiv, bis Sie das Projekt schließen. Wenn Sie das Projekt erneut öffnen, müssen Sie erneut eine Verbindung herstellen, wenn Sie eine aktive Verbindung mit der Datenbank herstellen möchten.  
  
Metadaten über die MySQL-Datenbank werden nicht automatisch aktualisiert. Wenn Sie die Metadaten im MySQL-metadatenexplorer aktualisieren möchten, müssen Sie Sie stattdessen manuell aktualisieren. Weitere Informationen finden Sie im Abschnitt "Aktualisieren von MySQL-Metadaten" weiter unten in diesem Thema.  
  
## <a name="required-mysql-permissions"></a>Erforderliche MySQL-Berechtigungen  
Das Konto, das zum Herstellen einer Verbindung mit der MySQL-Datenbank verwendet wird, muss mindestens über **Connect** -Berechtigungen verfügen. Dies ermöglicht SSMA das Abrufen von Metadaten aus Schemas im Besitz des Benutzers, der eine Verbindung herstellt. Zum Abrufen von Metadaten für Objekte in anderen Schemas und zum anschließenden Konvertieren von Objekten in diesen Schemas muss das Konto über die folgenden Berechtigungen verfügen:  
  
-   "Show"-Berechtigungen für Datenbankobjekte  
  
-   SELECT-Berechtigung für "Information_schema"  
  
-   SELECT-Berechtigung für MySQL (für UDFs)  
  
## <a name="establishing-a-connection-to-mysql"></a>Herstellen einer Verbindung mit MySQL  
Wenn Sie eine Verbindung mit einer Datenbank herstellen, liest SSMA die Metadaten der Datenbank und fügt diese Metadaten dann der Projektdatei hinzu. Diese Metadaten werden von SSMA verwendet, wenn Objekte in SQL Server-oder SQL Azure-Syntax konvertiert und Daten zu SQL Server oder SQL Azure migriert werden. Sie können diese Metadaten im Bereich MySQL-metadatenexplorer durchsuchen und die Eigenschaften einzelner Datenbankobjekte überprüfen.  
  
> [!IMPORTANT]  
> Bevor Sie versuchen, eine Verbindung herzustellen, stellen Sie sicher, dass der Datenbankserver ausgeführt wird und Verbindungen akzeptieren kann.  
  
**So stellen Sie eine Verbindung mit MySQL her**  
  
1.  Wählen Sie im Menü **Datei** die Option **mit MySQL verbinden** aus. (diese Option wird nach der Projekt Erstellung aktiviert.)  
  
    Wenn Sie zuvor eine Verbindung mit MySQL hergestellt haben, wird der Befehls Name **erneut mit MySQL** verbunden.  
  
2.  Wählen Sie im Feld **Anbieter** die Option MySQL ODBC 5,1-Treiber (vertrauenswürdig) aus. Dies ist der Standardanbieter im Standardmodus.  
  
3.  Wählen Sie im Feld **Modus** die Option **Standard Modus** aus. Es handelt sich hierbei um den Standardmodus.  
  
    Verwenden Sie den Standardmodus, um den Servernamen und den Port anzugeben.  
  
4.  Geben Sie im **Standard Modus** die folgenden Werte an:  
  
    1.  Geben Sie im Feld **Server Name** den Namen des MySQL-Servers ein. Geben Sie im Feld **Serverport** die Portnummer 3306 ein. Dies ist der Standardport.  
  
    2.  Geben Sie im Feld **Benutzername** ein MySQL-Konto ein, das über die erforderlichen Berechtigungen verfügt.  
  
    3.  Geben Sie im Feld **Kennwort** das Kennwort für den angegebenen Benutzernamen ein.  
  
5.  **SSL:** Wenn Sie eine sichere Verbindung mit MySQL herstellen möchten, verwenden Sie SSL (Secure Socket Layer), indem Sie das Kontrollkästchen **SSL** aktivieren.  
  
6.  **Konfigurieren:** Es bietet eine Option zum Konfigurieren der Verbindung mit MySQL über Secure Socket Layer (SSL).  
  
    > [!NOTE]  
    > Zum Aktivieren von **configure** muss SSL auf **true** festgelegt werden.  
  
    Wenn Sie auf die Schaltfläche "Konfigurieren" klicken, wird ein Dialogfeld angezeigt. Wenn Sie beim Herstellen einer Verbindung mit einer MySQL-Datenbank die Verschlüsselung verwenden möchten, müssen Sie den Pfad zu den drei folgenden im Dialogfeld vorhandenen Zertifikat Dateien definieren [Privacy Enhanced Mail Zertifikate (PEM)]:  
  
    -   **SSL-Zertifizierungsstelle:** Gibt den Pfad zu einer Datei mit einer Liste mit vertrauenswürdigen SSL-Zertifizierungsstellen an.  
  
    -   **SSL-Zertifikat:** Gibt den Namen der SSL-Zertifikatsdatei an, die zum Herstellen einer sicheren Verbindung verwendet werden soll.  
  
    -   **SSL-Schlüssel:** Gibt den Namen der SSL-Schlüsseldatei an, die zum Herstellen einer sicheren Verbindung verwendet werden soll.  
  
    > [!NOTE]  
    > -   Die Schaltfläche **OK** ist aktiviert, wenn die erforderlichen Informationen angegeben wurden. Wenn einer der Dateipfade ungültig ist, bleibt die Schaltfläche "OK" deaktiviert.  
    > -   Mit der Schaltfläche **Abbrechen** wird das Dialogfeld geschlossen, und die Option SSL **wird** aus dem Haupt Verbindungs Formular deaktiviert.  
  
7.  Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit MySQL &#40;mysqlto SQL&#41;](../../ssma/mysql/connect-to-mysql-mysqltosql.md)  
  
## <a name="reconnecting-to-mysql"></a>Erneutes Herstellen einer Verbindung mit MySQL  
Die Verbindung mit dem Datenbankserver bleibt aktiv, bis Sie das Projekt schließen. Wenn Sie das Projekt erneut öffnen, müssen Sie erneut eine Verbindung herstellen, wenn Sie eine aktive Verbindung mit der Datenbank herstellen möchten. Sie können offline arbeiten, bis Sie Metadaten aktualisieren, Datenbankobjekte in SQL Server oder SQL Azure laden und Daten migrieren möchten.  
  
## <a name="refreshing-mysql-metadata"></a>Aktualisieren von MySQL-Metadaten  
Metadaten über die MySQL-Datenbank werden nicht automatisch aktualisiert. Bei den Metadaten im MySQL-metadatenexplorer handelt es sich um eine Momentaufnahme der Metadaten, wenn Sie zum ersten Mal eine Verbindung hergestellt haben, oder wenn Sie das letzte Mal manuell Sie können Metadaten für alle Schemas, ein einzelnes Schema oder einzelne Datenbankobjekte manuell aktualisieren.  
  
**So aktualisieren Sie Metadaten**  
  
1.  Stellen Sie sicher, dass Sie mit der Datenbank verbunden sind.  
  
2.  Aktivieren Sie im MySQL-metadatenexplorer das Kontrollkästchen neben jedem Schema oder Datenbankobjekt, das Sie aktualisieren möchten.  
  
3.  Klicken Sie mit der rechten Maustaste auf **Schemas** oder das einzelne Schema oder Datenbankobjekt, und wählen Sie dann **aus Datenbank aktualisieren aus**.  
  
    Wenn Sie nicht über eine aktive Verbindung verfügen, wird das Dialogfeld **mit MySQL Verbinden von** SSMA angezeigt, sodass Sie eine Verbindung herstellen können.  
  
4.  Geben Sie im Dialogfeld aus Datenbank aktualisieren an, welche Objekte aktualisiert werden sollen.  
  
    -   Um ein Objekt zu aktualisieren, klicken Sie auf das **aktive** Feld neben dem Objekt, bis ein Pfeil angezeigt wird.  
  
    -   Um zu verhindern, dass ein Objekt aktualisiert wird, klicken Sie auf das **aktive** Feld neben dem Objekt, bis ein **X** angezeigt wird.  
  
    -   Um eine Kategorie von Objekten zu aktualisieren oder abzulehnen, klicken Sie auf das **aktive** Feld neben dem Kategorieordner.  
  
    -   Um die Definitionen der Farbcodierung anzuzeigen, klicken Sie auf die Schaltfläche **Legende** .  
  
5.  Klicken Sie auf **OK**.  
  
## <a name="next-step"></a>Nächster Schritt  
Der nächste Schritt des Migrations Vorgangs ist das [Herstellen einer Verbindung mit SQL Server &#40;mysqldesql&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
## <a name="see-also"></a>Weitere Informationen  
[Migrieren von MySQL-Datenbanken zu SQL Server Azure SQL-Datenbank &#40;mysqlto SQL&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
