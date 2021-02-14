---
description: Herstellen einer Verbindung mit SQL Server (DB2ToSQL)
title: Herstellen einer Verbindung mit SQL Server (DB2ToSQL) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: b59803cb-3cc6-41cc-8553-faf90851410e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 1109bd083476bf9be2421b06637751c34fc6405c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081636"
---
# <a name="connecting-to-sql-server-db2tosql"></a>Herstellen einer Verbindung mit SQL Server (DB2ToSQL)

Wenn Sie DB2-Datenbanken zu migrieren möchten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , müssen Sie eine Verbindung mit der-Ziel Instanz von herstellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Wenn Sie eine Verbindung herstellen, erhält SSMA Metadaten zu allen Datenbanken in der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und zeigt Daten Bank Metadaten im **SQL Server Metadaten-Explorer** an. SSMA speichert Informationen zu der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , mit der Sie verbunden sind, speichert aber keine Kenn Wörter.

Die Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bleibt aktiv, bis Sie das Projekt schließen. Wenn Sie das Projekt erneut öffnen, müssen Sie erneut eine Verbindung mit herstellen, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Wenn Sie eine aktive Verbindung mit dem Server herstellen möchten. Sie können offline arbeiten, bis Sie Datenbankobjekte in Laden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und Daten migrieren.

Metadaten über die Instanz von werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht automatisch synchronisiert. Stattdessen müssen Sie die Metadaten manuell aktualisieren, um die Metadaten in **SQL Server Metadaten-Explorer** zu aktualisieren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Weitere Informationen finden Sie im Abschnitt "Synchronisieren von SQL Server Metadaten" weiter unten in diesem Thema.

## <a name="required-sql-server-permissions"></a>Erforderliche SQL Server Berechtigungen

Das Konto, das verwendet wird, um eine Verbindung mit herzustellen, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erfordert abhängig von den Aktionen, die das Konto ausführt, unterschiedliche Berechtigungen:

- Zum Konvertieren von DB2-Objekten in [!INCLUDE[tsql](../../includes/tsql-md.md)] Syntax, zum Aktualisieren von Metadaten aus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder zum Speichern konvertierter Syntax in Skripts muss das Konto über die Berechtigung verfügen, sich bei der Instanz von anzumelden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Das Konto muss ein Mitglied der **db_ddladmin** Server Rolle sein, um Datenbankobjekte in laden zu können.

- Zum Migrieren von Daten zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muss das Konto Mitglied der **db_owner** Daten Bank Rolle sein.

- Zum Ausführen des von SSMA generierten Codes muss das Konto über `EXECUTE` Berechtigungen für alle benutzerdefinierten Funktionen im **ssma_db2** Schema der Zieldatenbank verfügen. Diese Funktionen bieten äquivalente Funktionalität von DB2-Systemfunktionen und werden von konvertierten Objekten verwendet.

## <a name="establishing-a-sql-server-connection"></a>Einrichten einer SQL Server Verbindung

Bevor Sie DB2-Datenbankobjekte in die- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Syntax konvertieren, müssen Sie eine Verbindung mit der Instanz von herstellen, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in der Sie die DB2-Datenbank oder-Datenbanken migrieren möchten.

Wenn Sie die Verbindungs Eigenschaften definieren, geben Sie auch die Datenbank an, in die Objekte und Daten migriert werden. Sie können diese Zuordnung auf DB2-Schema Ebene anpassen, nachdem Sie eine Verbindung mit hergestellt haben [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Weitere Informationen finden Sie unter [Mapping von DB2-Schemas zu SQL Server Schemas &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-schemas-to-sql-server-schemas-db2tosql.md).

> [!IMPORTANT]
> Bevor Sie versuchen, eine Verbindung mit herzustellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , stellen Sie sicher, dass die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird und Verbindungen akzeptieren kann.  
  
So stellen Sie eine Verbindung mit her [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :

1. Wählen Sie im Menü **Datei** die Option **mit SQL Server verbinden** aus.
   Wenn Sie zuvor eine Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hergestellt haben, wird der Befehls Name **erneut mit SQL Server** verbunden.

2. Geben Sie im Dialogfeld Verbindung den Namen der Instanz von ein, oder wählen Sie ihn aus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
   - Wenn Sie eine Verbindung mit der Standard Instanz auf dem lokalen Computer herstellen, können Sie `localhost` oder einen Punkt ( `.` ) eingeben.
   - Wenn Sie eine Verbindung mit der Standard Instanz auf einem anderen Computer herstellen, geben Sie den Namen des Computers ein.
   - Wenn Sie eine Verbindung mit einer benannten Instanz auf einem anderen Computer herstellen, geben Sie den Computernamen gefolgt von einem umgekehrten Schrägstrich und dem Instanznamen ein, z `MyServer\MyInstance` . b..

3. Wenn die Instanz von für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Annahme von Verbindungen an einem nicht standardmäßigen Port konfiguriert ist, geben Sie die Portnummer ein, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] im Feld **Serverport** für Verbindungen verwendet wird. Die Standard Portnummer für die Standard Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist 1433. Bei benannten Instanzen wird von SSMA versucht, die Portnummer vom- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser Dienst abzurufen.

4. Geben Sie im Feld **Datenbank** den Namen der Zieldatenbank ein.
   Diese Option ist nicht verfügbar, wenn Sie erneut eine Verbindung mit herstellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

5. Wählen Sie im Feld **Authentifizierung** den Authentifizierungstyp aus, der für die Verbindung verwendet werden soll. Um das aktuelle Windows-Konto zu verwenden, wählen Sie **Windows-Authentifizierung** aus. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Wählen Sie **SQL Server Authentifizierung** aus, und geben Sie dann den Anmelde Namen und das Kennwort ein, um eine Anmeldung zu verwenden.

6. Für eine sichere Verbindung werden zwei-Steuerelemente hinzugefügt: die Kontrollkästchen **Verbindung verschlüsseln** und **TrustServerCertificate** . Nur wenn **Verbindung verschlüsseln** aktiviert ist, ist das Kontrollkästchen **TrustServerCertificate** sichtbar. Wenn **Verbindung verschlüsseln** (true) und **TrustServerCertificate** deaktiviert (false) ist, wird das SQL Server SSL-Zertifikat überprüft. Das Überprüfen des Serverzertifikats ist Teil des SSL-Handshakes und stellt sicher, dass es sich bei dem Server tatsächlich um den Server handelt, mit dem eine Verbindung hergestellt werden soll. Um dies zu gewährleisten, muss ein Zertifikat sowohl auf Clientseite als auch auf Serverseite installiert werden.

7. Klicken Sie auf **Verbinden**.

> [!IMPORTANT]
> Obwohl Sie möglicherweise eine Verbindung mit einer höheren Version von herstellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , wird die Konvertierung der-Datenbankobjekte von der Zielversion des Projekts und nicht von der Version von, mit der Sie verbunden sind, festgelegt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

## <a name="synchronizing-sql-server-metadata"></a>Synchronisieren von SQL Server-Metadaten

Metadaten zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbanken werden nicht automatisch aktualisiert. Bei den Metadaten in **SQL Server Metadaten-Explorer** handelt es sich um eine Momentaufnahme der Metadaten, wenn Sie zum ersten Mal eine Verbindung mit hergestellt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] haben oder wenn Sie die Metadaten zuletzt manuell aktualisiert haben. Sie können Metadaten für alle Datenbanken oder für eine einzelne Datenbank oder ein Datenbankobjekt manuell aktualisieren. So synchronisieren Sie Metadaten:

1. Stellen Sie sicher, dass Sie mit verbunden sind [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
  
2. Aktivieren Sie in **SQL Server metadatenexplorer** das Kontrollkästchen neben dem zu aktualisierenden Datenbankschema oder Datenbankschema.
   Wenn Sie z. b. die Metadaten für alle Datenbanken aktualisieren möchten, aktivieren Sie das Kontrollkästchen neben **Datenbanken**.

3. Klicken Sie mit der rechten Maustaste auf **Datenbanken** oder die einzelne Datenbank oder das Datenbankschema, und wählen Sie dann **mit Datenbank synchronisieren** aus.

## <a name="next-step"></a>Nächster Schritt

Der nächste Schritt der Migration hängt von Ihren Projektanforderungen ab:

- Informationen zum Anpassen der Zuordnung zwischen DB2-Schemas und- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbanken und-Schemas finden Sie unter [Mapping von DB2-Schemas zu SQL Server Schemas &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-schemas-to-sql-server-schemas-db2tosql.md).
- Informationen zum Anpassen der Konfigurationsoptionen für die Projekte finden Sie unter [Project Settings &#40;Conversion&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-conversion-db2tosql.md) und Related Abschnitts.
- Informationen zum Anpassen der Zuordnung von Quell-und Ziel Datentypen finden Sie unter [Mapping von DB2-und SQL Server-Datentypen &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md).
- Wenn Sie diese Aufgaben nicht ausführen müssen, können Sie die Objekt Definitionen der DB2-Datenbank in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Objekt Definitionen konvertieren. Weitere Informationen finden Sie unter [umstellen von DB2-Schemas &#40;DB2ToSQL&#41;](../../ssma/db2/converting-db2-schemas-db2tosql.md).

## <a name="see-also"></a>Weitere Informationen

[Migrieren von DB2-Datenbanken zu SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)
