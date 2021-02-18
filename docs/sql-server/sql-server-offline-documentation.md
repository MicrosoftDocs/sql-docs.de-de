---
title: Installieren der SQL Server-Dokumentation für die Offlineanzeige
description: Erfahren Sie, wie Sie Offlinedokumentation für SQL Server 2019, 2017, 2016, 2014 und 2012 installieren können. Verwenden Sie SQL Server Management Studio (SSMS) zum Anzeigen der Offlineinhalte.
ms.prod: sql
ms.technology: install
ms.topic: conceptual
ms.assetid: 51f8a08c-51d0-41d8-8bc5-1cb4d42622fb
author: markingmyname
ms.author: maghan
ms.date: 08/12/2020
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: ab5ee55bf91e1a9287f22584da83ca7112dadece
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336775"
---
# <a name="install-sql-server-documentation-to-view-offline-in-ssms"></a>Installieren der SQL Server-Dokumentation für die Offlineanzeige in SSMS

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

In diesem Artikel wird beschrieben, wie Sie SQL Server-Offlineinhalte herunterladen und in [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) anzeigen. Mithilfe von Offlineinhalten können Sie ohne bestehende Internetverbindung auf die Dokumentation zugreifen (allerdings ist für den Download anfangs eine Internetverbindung erforderlich).

Die Offlinedokumentation ist für Versionen ab SQL Server 2012 verfügbar. Zwar lassen sich [Inhalte für frühere Versionen online](/previous-versions/sql/) anzeigen, eine Offlineoption bietet jedoch ein komfortables Zugriffsverfahren für die älteren Inhalte.

- [SQL Server 2016 und höher](#sql-server-2016-and-later-offline-content)
- [SQL Server 2014](#sql-server-2014-offline-content)
- [SQL Server 2012](#sql-server-2012-offline-content)

## <a name="sql-server-2016-and-later-offline-content"></a>Offlineinhalte für SQL Server 2016 und höher

In den folgenden Schritten wird erläutert, wie Offlineinhalte für SQL Server 2016 und höher geladen werden.

1. Wählen Sie in SSMS im Menü „Hilfe“ **Hilfeinhalt hinzufügen und entfernen** aus.

   ![Screenshot zur Option „Hilfeinhalt hinzufügen und entfernen“](../sql-server/media/sql-server-offline-documentation/add-remove-content.png)

   Help Viewer öffnet die Registerkarte „Inhalt verwalten“.

2. Die neuesten Hilfsinhalte für SQL Server 2016 und höher finden Sie, indem Sie auf der Registerkarte **Inhalt verwalten** die Installationsquelle **Online** auswählen und dann *SQL Server* in die Suchleiste eingeben.

   ![SQL Server-Büchersuche](../sql-server/media/sql-server-offline-documentation/sql-online-search.png)

   > [!Note]
   > Der lokale Speicherpfad auf der Registerkarte „Inhalt verwalten“ zeigt an, wo der Inhalt auf dem lokalen Computer installiert ist. Wenn Sie den Speicherort ändern möchten, wählen Sie **Verschieben** aus, geben Sie einen anderen Ordnerpfad im Feld **Nach** ein, und wählen Sie anschließend **OK** aus. Wenn die Installation der Hilfe nach dem Ändern des lokalen Speicherpfads fehlschlägt, müssen Sie Help Viewer schließen und anschließend wieder öffnen. Stellen Sie sicher, dass der neue Speicherort im lokalen Speicherpfad angezeigt wird, und versuchen Sie dann noch mal, die Installation durchzuführen.

3. Klicken Sie neben jedem Inhaltspaket (Buch), das Sie installieren möchten, auf **Hinzufügen**, und klicken Sie dann in der unteren rechten Ecke auf **Aktualisieren**, um die neuesten Hilfsinhalte für SQL Server 2016 und höher zu installieren.

   ![SQL Server-Onlinebücher hinzufügen und aktualisieren](../sql-server/media/sql-server-offline-documentation/sql-add-update.png)

   > [!NOTE]
   > Wenn Help Viewer beim Hinzufügen von Inhalt nicht mehr reagieren sollte, ändern Sie die Cachezeile „LastRefreshed="\<mm/dd/yyyy> 00:00:00“ in den Dateien „%LOCALAPPDATA%\Microsoft\HelpViewer2.x\HlpViewer_SSMSx_en-US.settings“ oder „HlpViewer_VisualStudiox_en-US.settings“ in ein Datum, das in der Zukunft liegt. Weitere Informationen zu diesem Problem finden Sie unter [Visual Studio Help Viewer friert beim Begrüßungsbildschirm ein](/visualstudio/welcome-to-visual-studio).

4. Sie können überprüfen, ob die Inhalte von SQL Server 2016 oder höher geladen wurden, indem Sie im Inhaltsbereich links nach *SQL Server 2016* suchen.

   ![Automatische Aktualisierung von SQL Server 2016-Büchern](../sql-server/media/sql-server-offline-documentation/sql-2016-content.png)

## <a name="sql-server-2014-offline-content"></a>Offlineinhalte für SQL Server 2014

> [!IMPORTANT]
> SQL 2014 Transact-SQL-Inhalte sind nur offline verfügbar.

In den folgenden Schritten wird erläutert, wie Offlineinhalte für SQL Server 2014 geladen werden.

1. Laden Sie den Inhalt [Produktdokumentation für Microsoft SQL Server 2014 für firewall- und proxygeschützte Umgebungen](https://www.microsoft.com/download/details.aspx?id=42557) aus dem Downloadcenter herunter, und speichern Sie ihn in einem Ordner.

2. Entpacken Sie die Datei, um die *.msha*-Datei anzuzeigen.

   ![Setupdatei der SQL Server 2014-Hilfedokumentation](../sql-server/media/sql-server-offline-documentation/sql-2014-help-content-setup-msha.png)

3. Wählen Sie in SSMS im Menü „Hilfe“ **Hilfeinhalt hinzufügen und entfernen** aus.

   ![Hinzufügen und Entfernen von Inhalten in Help Viewer](../sql-server/media/sql-server-offline-documentation/add-remove-content.png)

   Help Viewer öffnet die Registerkarte „Inhalt verwalten“.

4. Wählen Sie unter „Installationsquelle“ **Datenträger**, um die neuesten Hilfeinhalte zu installieren, und anschließend die Auslassungspunkte (...) aus.

   ![Quelle „Datenträger“ in Help Viewer – Inhalte verwalten](../sql-server/media/sql-server-offline-documentation/install-source-disk.png)

   > [!NOTE]
   > Der lokale Speicherpfad auf der Registerkarte „Inhalt verwalten“ zeigt an, wo sich der Inhalt auf dem lokalen Computer befindet. Wenn Sie den Speicherort ändern möchten, wählen Sie **Verschieben** aus, geben Sie einen anderen Ordnerpfad im Feld **Nach** ein, und wählen Sie anschließend **OK** aus.
   Wenn die Installation der Hilfe nach dem Ändern des lokalen Speicherpfads fehlschlägt, müssen Sie Help Viewer schließen und anschließend wieder öffnen. Stellen Sie sicher, dass der neue Speicherort im lokalen Speicherpfad angezeigt wird, und versuchen Sie dann noch mal, die Installation durchzuführen.

5. Navigieren Sie zu dem Ordner, in dem Sie die Inhalte entzippt haben. Wählen Sie die Datei **HelpContentSetup.msha** im Ordner und anschließend **Öffnen** aus.

   ![Öffnen der Datei „SQL Server 2014 Help Content Setup.msha“](../sql-server/media/sql-server-offline-documentation/sql-2014-open-msha.png)

6. Geben Sie in der Suchleiste *sql server 2014* ein. Wenn Sie die verfügbaren 2014-Inhalte sehen, wählen Sie neben jedem einzelnen Inhaltspaket (Buch), das Sie in Help Viewer installieren möchten, **Hinzufügen**, und wählen Sie dann **Aktualisieren** aus.

   ![Suche nach SQL Server 2014-Büchern in Help Viewer](../sql-server/media/sql-server-offline-documentation/sql-2014-search.png)

   ![Hinzufügen und Aktualisieren von SQL Server 2014-Büchern in Help Viewer](../sql-server/media/sql-server-offline-documentation/sql-2014-add-update.png)

    > [!NOTE]
    > Wenn Help Viewer beim Hinzufügen von Inhalt nicht mehr reagieren sollte, ändern Sie die Cachezeile „LastRefreshed="\<mm/dd/yyyy> 00:00:00“ in den Dateien „%LOCALAPPDATA%\Microsoft\HelpViewer2.x\HlpViewer_SSMSx_en-US.settings“ oder „HlpViewer_VisualStudiox_en-US.settings“ in ein Datum, das in der Zukunft liegt. Weitere Informationen zu diesem Problem finden Sie unter [Visual Studio Help Viewer friert beim Begrüßungsbildschirm ein](/visualstudio/welcome-to-visual-studio).

7. Sie können überprüfen, ob die SQL Server 2014-Inhalte installiert wurden, indem Sie im Inhaltsbereich links nach *sql server 2014* suchen.

   ![Automatische Aktualisierung von SQL Server 2014-Büchern](../sql-server/media/sql-server-offline-documentation/sql-2014-content.png)

## <a name="sql-server-2012-offline-content"></a>Offlineinhalte für SQL Server 2012

In den folgenden Schritten wird erläutert, wie Offlineinhalte für SQL Server 2012 geladen werden.

1. Laden Sie den Inhalt [Produktdokumentation für Microsoft SQL Server 2012 für firewall- und proxygeschützte Umgebungen](https://www.microsoft.com/download/details.aspx?id=35750) aus dem Downloadcenter herunter, und speichern Sie ihn in einem Ordner.

2. Entpacken Sie die Datei, um die *.msha*-Datei anzuzeigen.

   ![Setupdatei für SQL Server 2012-Hilfeinhalte](../sql-server/media/sql-server-offline-documentation/sql-2012-help-content-setup-msha.png)

3. Wählen Sie in SSMS im Menü „Hilfe“ **Hilfeinhalt hinzufügen und entfernen** aus.

   ![Hinzufügen und Entfernen von Inhalten in Help Viewer](../sql-server/media/sql-server-offline-documentation/add-remove-content.png)

   Help Viewer öffnet die Registerkarte „Inhalt verwalten“.

4. Wählen Sie unter „Installationsquelle“ **Datenträger**, um die neuesten Hilfeinhalte zu installieren, und anschließend die Auslassungspunkte (...) aus.

   ![Quelle „Datenträger“ in Help Viewer – Inhalte verwalten](../sql-server/media/sql-server-offline-documentation/install-source-disk.png)

   > [!NOTE]
   > Der lokale Speicherpfad auf der Registerkarte „Inhalt verwalten“ zeigt an, wo sich der Inhalt auf dem lokalen Computer befindet. Wenn Sie den Speicherort ändern möchten, wählen Sie **Verschieben** aus, geben Sie einen anderen Ordnerpfad im Feld **Nach** ein, und wählen Sie anschließend **OK** aus.
   Wenn die Installation der Hilfe nach dem Ändern des lokalen Speicherpfads fehlschlägt, müssen Sie Help Viewer schließen und anschließend wieder öffnen. Stellen Sie sicher, dass der neue Speicherort im lokalen Speicherpfad angezeigt wird, und versuchen Sie dann noch mal, die Installation durchzuführen.

5. Navigieren Sie zu dem Ordner, in dem Sie die Inhalte entzippt haben. Wählen Sie die Datei **HelpContentSetup.msha** im Ordner und anschließend **Öffnen** aus.

   ![Öffnen der Datei „SQL Server 2012 Help Content Setup.msha“](../sql-server/media/sql-server-offline-documentation/sql-2012-open-msha.png)

6. Geben Sie in der Suchleiste *sql server 2012* ein. Wenn Sie die verfügbaren 2012-Inhalte sehen, wählen Sie neben jedem einzelnen Inhaltspaket (Buch), das Sie in Help Viewer installieren möchten, **Hinzufügen**, und wählen Sie dann **Aktualisieren** aus.

   ![Suche nach SQL Server 2012-Büchern in Help Viewer](../sql-server/media/sql-server-offline-documentation/sql-2012-search.png)

   ![Hinzufügen und Aktualisieren von SQL Server 2012-Büchern in Help Viewer](../sql-server/media/sql-server-offline-documentation/sql-2012-add-update.png)

    > [!NOTE]
    > Wenn Help Viewer beim Hinzufügen von Inhalt nicht mehr reagieren sollte, ändern Sie die Cachezeile „LastRefreshed="\<mm/dd/yyyy> 00:00:00“ in den Dateien „%LOCALAPPDATA%\Microsoft\HelpViewer2.x\HlpViewer_SSMSx_en-US.settings“ oder „HlpViewer_VisualStudiox_en-US.settings“ in ein Datum, das in der Zukunft liegt. Weitere Informationen zu diesem Problem finden Sie unter [Visual Studio Help Viewer friert beim Begrüßungsbildschirm ein](/visualstudio/welcome-to-visual-studio).

7. Sie können überprüfen, ob die SQL Server 2012-Inhalte geladen wurden, indem Sie im Inhaltsbereich links nach *sql server 2012* suchen.

   ![Automatisch aktualisierte SQL Server 2012-Dokumentation](../sql-server/media/sql-server-offline-documentation/sql-2012-content.png)

## <a name="view-offline-documentation"></a>Anzeigen der Offlinedokumentation

Sie können SQL Server-Hilfeinhalte über das Menü **HILFE** in der letzten Version von [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) anzeigen.

### <a name="view-offline-help-content-in-ssms"></a>Anzeigen von Offline-Hilfeinhalten in SSMS

Um die installierte Hilfe in SSMS anzuzeigen, wählen Sie im Menü „Hilfe“ **In Help Viewer starten** aus, um den Help Viewer zu starten.

   ![In Help Viewer starten](../sql-server/media/sql-server-offline-documentation/helpviewer-view-offline.png)  

Help Viewer öffnet sich auf der Registerkarte „Inhalt verwalten“ mit dem installierten Inhaltsverzeichnis für die Hilfe im linken Bereich. Wählen Sie im Inhaltsverzeichnis Artikel aus, um diese auf der rechten Seite anzuzeigen.

> [!Important]
> Wenn der Inhaltsbereich nicht angezeigt wird, wählen Sie auf der linken Seite „Inhalt“ aus. Wählen Sie das Reißzweckensymbol aus, damit der Inhaltsbereich geöffnet bleibt.  

   ![Help Viewer mit Inhalt](../sql-server/media/sql-server-offline-documentation/view-offline-all.png)

## <a name="life-cycle-policy"></a>Lebenszyklusrichtlinie

Lesen Sie den Microsoft-Produktlebenszyklus durch, um Informationen zum Support für ein bestimmtes Produkt, einen Dienst oder eine Technologie zu erhalten:

- [Microsoft-Lebenszyklusrichtlinie](https://support.microsoft.com/lifecycle/selectindex)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu archivierten Inhalten und zu Help Viewer finden Sie unter den folgenden Links.

- [Onlinedokumentation für SQL Server](../sql-server/index.yml?view=sql-server-2016&preserve-view=true)
- [Onlinedokumentation für SQL Server 2014](/previous-versions/sql/2014)
- [Frühere Versionen der Onlinedokumentation für SQL Server](previous-versions-sql-server.md)
- [Versionsverwaltungssystem für die SQL-Dokumentation](../sql-server/versioning-system-monikers-ui-sql-server.md?view=sql-server-2016&preserve-view=true)