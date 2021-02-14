---
title: Herstellen einer Verbindung mit SQL Server (accesstosql) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine Verbindung mit einer Ziel Instanz von SQL-Datenbank herstellen, um Zugriffs Datenbanken zu migrieren SSMA Ruft Metadaten zu Datenbanken in SQL-Datenbank ab.
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- authentication
- instance of SQL Server
- metadata, refreshing
- ports
- refreshing metadata
- spaces in database names
- special characters
- SQL Server
- SQL Server, connecting
- SQL Server, connecting to
- SQL Server, reconnecting
ms.assetid: f84cf007-ddf1-4396-a07c-3e0729abc769
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 59583057641bb167aae197dd9fae3bf6e5833302
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076191"
---
# <a name="connecting-to-sql-server-accesstosql"></a>Herstellen einer Verbindung mit SQL Server (accesstosql)

Wenn Sie Access-Datenbanken zu migrieren möchten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , müssen Sie eine Verbindung mit der-Ziel Instanz von herstellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Wenn Sie eine Verbindung herstellen, ruft SSMA Metadaten zu den Datenbanken in der Instanz von ab [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und zeigt Daten Bank Metadaten in **SQL Server Metadaten-Explorer** an. SSMA speichert Informationen zu der Instanz von, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit der Sie verbunden sind, speichert aber keine Kenn Wörter.

Die Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bleibt aktiv, bis Sie das Projekt schließen. Wenn Sie das Projekt erneut öffnen, müssen Sie erneut eine Verbindung mit herstellen, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Wenn Sie eine aktive Verbindung mit dem Server herstellen möchten. Sie können offline arbeiten, bis Sie Datenbankobjekte in Laden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und Daten migrieren.

Metadaten über die Instanz von werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht automatisch synchronisiert. Stattdessen müssen Sie die Metadaten manuell aktualisieren, um die Metadaten in SQL Server Metadaten-Explorer zu aktualisieren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Weitere Informationen finden Sie im Abschnitt "Synchronisieren von SQL Server Metadaten" weiter unten in diesem Thema.

## <a name="required-sql-server-permissions"></a>Erforderliche SQL Server Berechtigungen

Das Konto, das verwendet wird, um eine Verbindung mit herzustellen, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erfordert abhängig von den Aktionen, die das Konto ausführt, unterschiedliche Berechtigungen:

- [!INCLUDE[tsql](../../includes/tsql-md.md)]Das Konto muss über die Berechtigung verfügen, sich bei der Instanz von anzumelden, um Zugriffs Objekte in die-Syntax zu konvertieren, um Metadaten aus zu aktualisieren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder um konvertierte Syntax in Skripts zu speichern [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Das Konto muss ein Mitglied der **db_ddladmin** Daten Bank Rolle sein, um Datenbankobjekte in laden zu können.

- Zum Migrieren von Daten zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muss das Konto Mitglied der **db_owner** Daten Bank Rolle sein.

## <a name="establishing-a-sql-server-connection"></a>Einrichten einer SQL Server Verbindung

Bevor Sie Access-Datenbankobjekte in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Syntax konvertieren, müssen Sie eine Verbindung mit der Instanz von herstellen, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in der Sie die Access-Datenbanken migrieren möchten.

Wenn Sie die Verbindungs Eigenschaften definieren, geben Sie auch die Datenbank an, in die Objekte und Daten migriert werden. Sie können diese Zuordnung auf der Ebene der Zugriffs Datenbank anpassen, nachdem Sie eine Verbindung mit hergestellt haben [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Weitere Informationen finden Sie unter [Mapping von Quell-und Ziel Datenbanken](mapping-source-and-target-databases-accesstosql.md).

> [!IMPORTANT]
> Bevor Sie eine Verbindung mit herstellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , stellen Sie sicher, dass die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird und Verbindungen akzeptieren kann.

So stellen Sie eine Verbindung mit dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] her

1. Wählen Sie im Menü **Datei** die Option **mit SQL Server verbinden** aus.
   Wenn Sie zuvor eine Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hergestellt haben, wird der Befehls Name **erneut mit SQL Server** verbunden.

2. Geben Sie im Feld **Server Name** den Namen der Instanz von ein, oder wählen Sie ihn aus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
   - Wenn Sie eine Verbindung mit der Standard Instanz auf dem lokalen Computer herstellen, können Sie `localhost` oder einen Punkt ( `.` ) eingeben.
   - Wenn Sie eine Verbindung mit der Standard Instanz auf einem anderen Computer herstellen, geben Sie den Namen des Computers ein.
   - Wenn Sie eine Verbindung mit einer benannten Instanz herstellen, geben Sie den Computernamen, einen umgekehrten Schrägstrich und den Instanznamen ein. Beispiel: `MyServer\MyInstance`.
   - Stellen Sie eine Verbindung mit einer aktiven Benutzer Instanz von her [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)] , indem Sie das Named Pipes-Protokoll verwenden und den Pipenamen angeben, z `\\.\pipe\sql\query` . b.. Weitere Informationen finden Sie in der Dokumentation zu [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)].

3. Wenn die Instanz von für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Annahme von Verbindungen an einem nicht standardmäßigen Port konfiguriert ist, geben Sie die Portnummer ein, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] im Feld **Serverport** für Verbindungen verwendet wird. Die Standard Portnummer für die Standard Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist 1433. Bei benannten Instanzen wird von SSMA versucht, die Portnummer vom- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser Dienst abzurufen.

4. Geben Sie im Feld **Datenbank** den Namen der Zieldatenbank für die Objekt-und Datenmigration ein.
   Diese Option ist nicht verfügbar, wenn Sie eine erneute Verbindung mit herstellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
   Der Name der Zieldatenbank darf keine Leerzeichen oder Sonderzeichen enthalten. Beispielsweise können Sie Access-Datenbanken zu einer Datenbank mit dem Namen migrieren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `abc` . Access-Datenbanken können jedoch nicht zu einer Datenbank namens migriert werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `a b-c` .
   Nachdem Sie eine Verbindung hergestellt haben, können Sie diese Zuordnung pro Datenbank anpassen. Weitere Informationen finden Sie unter [Zuordnung von Quell-und Ziel Datenbanken](mapping-source-and-target-databases-accesstosql.md) .

5. Wählen Sie im Dropdown Menü **Authentifizierung** den Authentifizierungstyp aus, der für die Verbindung verwendet werden soll. Um das aktuelle Windows-Konto zu verwenden, wählen Sie **Windows-Authentifizierung** aus. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Wählen Sie **SQL Server Authentifizierung** aus, und geben Sie einen Benutzernamen und ein Kennwort ein, um einen Anmelde Namen zu verwenden.

6. Für eine sichere Verbindung werden zwei Steuerelemente hinzugefügt, das Kontrollkästchen **Verbindung verschlüsseln** und das Kontrollkästchen **TrustServerCertificate** . Nur wenn das Kontrollkästchen **Verbindung verschlüsseln** aktiviert ist, wird das Kontrollkästchen **TrustServerCertificate** angezeigt. Wenn **Verbindung verschlüsseln** (true) und **TrustServerCertificate** deaktiviert (false) ist, überprüft das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SSL-Zertifikat. Das Überprüfen des Serverzertifikats ist Teil des SSL-Handshakes und stellt sicher, dass es sich bei dem Server tatsächlich um den Server handelt, mit dem eine Verbindung hergestellt werden soll. Um dies zu gewährleisten, muss ein Zertifikat sowohl auf Clientseite als auch auf Serverseite installiert werden.

7. Klicken Sie auf **Verbinden**.

> [!IMPORTANT]
> Obwohl Sie möglicherweise eine Verbindung mit einer höheren Version von herstellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , wird die Konvertierung der-Datenbankobjekte von der Zielversion des Projekts und nicht von der Version von, mit der Sie verbunden sind, festgelegt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

## <a name="synchronizing-sql-server-metadata"></a>Synchronisieren von SQL Server-Metadaten

Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Schemas geändert werden, nachdem Sie eine Verbindung hergestellt haben, können Sie die Metadaten mit dem Server synchronisieren.

Um SQL Server Metadaten zu synchronisieren, **SQL Server Metadaten-Explorer**, klicken Sie mit der rechten Maustaste auf **Datenbanken**, und wählen Sie dann **mit Datenbank synchronisieren**

## <a name="reconnecting-to-sql-server"></a>Erneutes Herstellen einer Verbindung mit SQL Server

Die Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bleibt aktiv, bis Sie das Projekt schließen. Wenn Sie das Projekt erneut öffnen, müssen Sie erneut eine Verbindung mit herstellen, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Wenn Sie eine aktive Verbindung mit dem Server herstellen möchten. Sie können offline arbeiten, bis Sie Datenbankobjekte in Laden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und Daten migrieren.

Die Vorgehensweise für das erneute Herstellen einer Verbindung mit entspricht dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verfahren zum Herstellen einer Verbindung.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie die Zuordnung zwischen Quell-und Ziel Datenbanken anpassen möchten, finden Sie weitere Informationen unter [Zuordnung von Quell-und Ziel Datenbanken](mapping-source-and-target-databases-accesstosql.md) . andernfalls ist der nächste Schritt das Konvertieren von Datenbankobjekten in eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Syntax mithilfe von [Convert Database Objects](converting-access-database-objects-accesstosql.md).

## <a name="see-also"></a>Weitere Informationen

[Migration von Access-Datenbanken zu SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)
