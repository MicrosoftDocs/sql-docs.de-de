---
title: Herstellen einer Verbindung mit der DB2-Datenbank (DB2ToSQL) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Verbindung mit einer DB2-Zieldatenbank herstellen, um DB2-Datenbanken zu migrieren SSMA Ruft Metadaten zu allen DB2-Schemas ab.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 5eb5801d-f0c3-4127-97c0-0b1ef49f4844
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b7a2bd0434e865e3748861b94b21d44615e07ed5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081611"
---
# <a name="connecting-to-db2-database-db2tosql"></a>Herstellen einer Verbindung mit der DB2-Datenbank (DB2ToSQL)

Um DB2-Datenbanken zu zu migrieren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , müssen Sie eine Verbindung mit der DB2-Datenbank herstellen, die Sie migrieren möchten. Wenn Sie eine Verbindung herstellen, ruft SSMA Metadaten zu allen DB2-Schemas ab und zeigt Sie dann im DB2-metadatenexplorer-Bereich an. SSMA speichert Informationen über den Datenbankserver, speichert aber keine Kenn Wörter.

Die Verbindung mit der Datenbank bleibt aktiv, bis Sie das Projekt schließen. Wenn Sie das Projekt erneut öffnen, müssen Sie erneut eine Verbindung herstellen, wenn Sie eine aktive Verbindung mit der Datenbank herstellen möchten.

Metadaten über die DB2-Datenbank werden nicht automatisch aktualisiert. Wenn Sie die Metadaten im DB2-metadatenexplorer aktualisieren möchten, müssen Sie Sie stattdessen manuell aktualisieren. Weitere Informationen finden Sie im Abschnitt "Aktualisieren von DB2-Metadaten" weiter unten in diesem Thema.

## <a name="required-db2-permissions"></a>Erforderliche DB2-Berechtigungen

Die Benutzer Autorisierung definiert die Liste der Befehle und Objekte, die für einen Benutzer verfügbar sind. Diese Liste steuert Benutzeraktionen. In DB2 gibt es vordefinierte Gruppen von Berechtigungen für die Autorisierung, sowohl auf Instanzebene als auch auf der Ebene einer DB2-Datenbank. Dies ermöglicht SSMA das Abrufen von Metadaten aus Schemas im Besitz des Benutzers, der eine Verbindung herstellt. Zum Abrufen von Metadaten für Objekte in anderen Schemas und zum anschließenden Konvertieren von Objekten in diesen Schemas muss das Konto über die folgenden Berechtigungen verfügen:

- Der Schema Zugriff für die Schema Migration wird öffentlich erteilt, es sei denn, das Einschränkungs Schlüsselwort wurde in CREATE verwendet
- Der Datenzugriff für die Datenmigration erfordert DataAccess

## <a name="establishing-a-connection-to-db2"></a>Herstellen einer Verbindung mit DB2

Wenn Sie eine Verbindung mit einer Datenbank herstellen, liest SSMA die Metadaten der Datenbank und fügt diese Metadaten dann der Projektdatei hinzu. Diese Metadaten werden von SSMA verwendet, wenn Objekte in die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Syntax konvertiert werden und Daten zu migriert werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Sie können diese Metadaten im Bereich DB2-metadatenexplorer durchsuchen und die Eigenschaften einzelner Datenbankobjekte überprüfen.  

> [!IMPORTANT]
> Bevor Sie versuchen, eine Verbindung herzustellen, stellen Sie sicher, dass der Datenbankserver ausgeführt wird und Verbindungen akzeptieren kann.

**Zum Herstellen einer Verbindung mit DB2**

1. Wählen Sie im Menü **Datei** die Option **mit DB2 verbinden** aus.

   Wenn Sie zuvor eine Verbindung mit DB2 hergestellt haben, wird der Befehls Name **erneut eine Verbindung mit DB2 herstellen**.

2. Im Feld **Anbieter** wird der **OLE DB Anbieter** angezeigt, der zurzeit der einzige DB2-Client Zugriffs Anbieter ist.

3. Im Feld " **Manager** " können Sie entweder **DB2 für zOS**, **DB2 für LUW** oder **DB2 für i** auswählen.

4. Wählen Sie im Feld **Modus** entweder den **Standard Modus** oder den **Verbindungs Zeichen folgen Modus** aus.

   Verwenden Sie den Standardmodus, um den Servernamen und den Port anzugeben. Verwenden Sie den Dienstnamen Modus, um den DB2-Dienstnamen manuell anzugeben. Verwenden Sie den Verbindungs Zeichen folgen Modus, um eine vollständige Verbindungs Zeichenfolge

5. Wenn Sie den **Standard Modus** auswählen, geben Sie die folgenden Werte an:

   - Geben Sie im Feld **Server Name** den Namen oder die IP-Adresse des Datenbankservers ein, oder wählen Sie ihn aus.
   - Wenn der Datenbankserver nicht für die Annahme von Verbindungen über den Standardport (1521) konfiguriert ist, geben Sie im Feld **Serverport** die Portnummer ein, die für DB2-Verbindungen verwendet wird.
   - Geben Sie im Feld **Serverport** die TCP/IP-Port Nummer ein.
   - Geben Sie im Feld **anfangs Katalog** den Datenbanknamen ein.
   - Geben Sie im Feld **Benutzername** ein DB2-Konto ein, das über die erforderlichen Berechtigungen verfügt.
   - Geben Sie im Feld **Kennwort** das Kennwort für den angegebenen Benutzernamen ein.

6. Wenn Sie den **Verbindungs Zeichen folgen Modus** auswählen, geben Sie im Feld **Verbindungs Zeichenfolge** eine Verbindungs Zeichenfolge an.

   Das folgende Beispiel zeigt eine OLE DB Verbindungs Zeichenfolge:

   `Provider=OraOLEDB.DB2;Data Source=MyDB2DB;User Id=myUsername;Password=myPassword;`

   Das folgende Beispiel zeigt eine DB2-Client Verbindungs Zeichenfolge, die die integrierte Sicherheit verwendet:
  
   `Data Source=MyDB2DB;Integrated Security=yes;`

   Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit Oracle &#40;oracleto SQL&#41;](../../ssma/oracle/connect-to-oracle-oracletosql.md).
  
## <a name="reconnecting-to-db2"></a>Erneutes Herstellen einer Verbindung mit DB2

Die Verbindung mit dem Datenbankserver bleibt aktiv, bis Sie das Projekt schließen. Wenn Sie das Projekt erneut öffnen, müssen Sie erneut eine Verbindung herstellen, wenn Sie eine aktive Verbindung mit der Datenbank herstellen möchten. Sie können offline arbeiten, bis Sie Metadaten aktualisieren, Datenbankobjekte in Laden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und Daten migrieren möchten.

## <a name="refreshing-db2-metadata"></a>Aktualisieren von DB2-Metadaten

Metadaten über die DB2-Datenbank werden nicht automatisch aktualisiert. Bei den Metadaten im DB2-metadatenexplorer handelt es sich um eine Momentaufnahme der Metadaten, wenn Sie zum ersten Mal eine Verbindung hergestellt haben, oder wenn Sie das letzte Mal manuell Sie können Metadaten für alle Schemas, ein einzelnes Schema oder einzelne Datenbankobjekte manuell aktualisieren.

**So aktualisieren Sie Metadaten**

1. Stellen Sie sicher, dass Sie mit der Datenbank verbunden sind.
2. Aktivieren Sie im DB2-metadatenexplorer das Kontrollkästchen neben jedem Schema oder Datenbankobjekt, das Sie aktualisieren möchten.
3. Klicken Sie mit der rechten Maustaste auf **Schemas** oder das einzelne Schema oder Datenbankobjekt, und wählen Sie dann **aus Datenbank aktualisieren aus**.

   Wenn Sie nicht über eine aktive Verbindung verfügen, zeigt SSMA das Dialogfeld **mit DB2 verbinden an** , sodass Sie eine Verbindung herstellen können.
  
4. Geben Sie im Dialogfeld aus Datenbank aktualisieren an, welche Objekte aktualisiert werden sollen.
   - Um ein Objekt zu aktualisieren, klicken Sie auf das **aktive** Feld neben dem Objekt, bis ein Pfeil angezeigt wird.
   - Um zu verhindern, dass ein Objekt aktualisiert wird, klicken Sie auf das **aktive** Feld neben dem Objekt, bis ein **X** angezeigt wird.
   - Um eine Kategorie von Objekten zu aktualisieren oder abzulehnen, klicken Sie auf das **aktive** Feld neben dem Kategorieordner.

     Um die Definitionen der Farbcodierung anzuzeigen, klicken Sie auf die Schaltfläche **Legende** .

5. [!INCLUDE[click OK](../../includes/clickok-md.md)]

## <a name="next-step"></a>Nächster Schritt

- Der nächste Schritt des Migrations Vorgangs besteht darin, eine [Verbindung mit SQL Server](./connecting-to-sql-server-db2tosql.md)herzustellen.

## <a name="see-also"></a>Weitere Informationen

- [Migrieren von DB2-Datenbanken zu SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)