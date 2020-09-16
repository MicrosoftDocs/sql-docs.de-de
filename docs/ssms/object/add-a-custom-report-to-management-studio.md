---
description: Hinzufügen eines benutzerdefinierten Berichts zu Management Studio
title: Hinzufügen eines benutzerdefinierten Berichts zu Management Studio
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], custom reports
ms.assetid: 3cf8d726-0a90-4f80-98d0-352a2a59be0f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: aa920b620cfa0045228fcdb5675c1e88bc7ab035
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88480163"
---
# <a name="add-a-custom-report-to-management-studio"></a>Hinzufügen eines benutzerdefinierten Berichts zu Management Studio
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
In diesem Thema wird beschrieben, wie ein einfacher, als RDL-Datei gespeicherter [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Bericht erstellt und anschließend [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] diese RDL-Datei als benutzerdefinierter Bericht hinzugefügt wird. [!INCLUDE[ssRS](../../includes/ssrs.md)] kann eine Vielzahl komplexer Berichte erstellen. Zum Erstellen eines Berichts mithilfe dieses Themas muss auf dem Computer [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull_md.md)] installiert sein. Sie müssen [!INCLUDE[ssRS](../../includes/ssrs.md)] nicht auf einem Computer mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installieren, um einen benutzerdefinierten Bericht mithilfe von [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]auszuführen.  
  
 
### <a name="to-create-a-simple-report-saved-as-an-rdl-file"></a>So erstellen Sie einen als RDL-Datei gespeicherten, einfachen Bericht  
  
1.  Klicken Sie auf **Start**, zeigen sie auf **Alle Programme**und dann auf **Microsoft SQL Server**, und klicken Sie auf **SQL Server Data Tools**.  
  
2.  Zeigen Sie im Menü **Datei** auf **Neu**, und klicken Sie dann auf **Projekt**.  
  
3.  Klicken Sie in der Liste **Projekttypen** auf **Business Intelligence-Projekte**.  
  
4.  Klicken Sie in der Liste **Vorlagen** auf **Berichtsserverprojekt-Assistent**.  
  
5.  Geben Sie unter **Name****ConnectionsReport**ein, und klicken Sie anschließend auf **OK**.  
  
6.  Klicken Sie auf der Einführungsseite des **Berichts-Assistenten** auf **Weiter**.  
  
7.  Geben Sie auf der Seite **Datenquelle auswählen** im Feld „Name“ einen Namen für diese Verbindung mit [!INCLUDE[ssDE](../../includes/ssde_md.md)]ein, und klicken Sie anschließend auf **Bearbeiten**.  
  
8.  Geben Sie im Dialogfeld **Verbindungseigenschaften** im Feld **Servername** den Namen Ihrer Instanz von [!INCLUDE[ssDE](../../includes/ssde_md.md)]ein.  
  
9. Geben Sie im Feld **Datenbanknamen eingeben oder auswählen** den Namen einer beliebigen Datenbank auf Ihrem Computer mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ein (z. B. [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]), und klicken Sie dann auf **OK**.  
  
10. Klicken Sie auf der Seite **Datenquelle auswählen** auf **Weiter**.  
  
11. Geben Sie auf der Seite **Abfrage entwerfen** im Feld **Abfragezeichenfolge** die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung ein, mit der die aktuellen Verbindungen mit [!INCLUDE[ssDE](../../includes/ssde_md.md)]aufgelistet werden, und klicken Sie dann auf **Weiter**. Im Feld "Abfragezeichenfolge" des Berichts-Assistenten können keine Berichtsparameter eingegeben werden. Komplexere benutzerdefinierte Berichte müssen manuell erstellt werden.  
  
    **SELECT session_id, net_transport FROM sys.dm_exec_connections;**  
  
12. Wählen Sie auf der Seite **Berichtstyp auswählen** die Option **Tabellarisch**aus, und klicken Sie dann auf **Fertig stellen**.  
  
13. Geben Sie auf der Seite **Assistenten abschließen** im Feld **Berichtsname** die Zeichenfolge **ConnectionsReport**ein, und klicken Sie anschließend zum Erstellen und Speichern des Berichts auf **Fertig stellen** .  
  
14. Schließen Sie [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)].  
  
15. Kopieren Sie die Datei **ConnectionsReport.rdl** in einen Ordner, den Sie auf Ihrem Datenbankserver für benutzerdefinierte Berichte erstellt haben.  
  
### <a name="to-add-a-report-to-management-studio"></a>So fügen Sie Management Studio einen Bericht hinzu  
  
-   Klicken Sie in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]mit der rechten Maustaste auf einen Knoten im Objekt-Explorer, zeigen Sie auf **Berichte**, und klicken Sie auf **Benutzerdefinierte Berichte**. Suchen Sie im Dialogfeld **Datei öffnen** den Ordner mit den benutzerdefinierten Berichten, wählen Sie die Datei **ConnectionsReport.rdl** aus, und klicken Sie dann auf **Öffnen**.  
  
    Wenn ein neuer benutzerdefinierter Bericht erstmalig von einem Knoten des Objekt-Explorers aus geöffnet wird, wird der Liste zuletzt verwendeter Objekte im Kontextmenü dieses Knotens unter **Benutzerdefinierte Berichte** dieser benutzerdefinierte Bericht hinzugefügt. Wenn ein Standardbericht erstmalig geöffnet wird, wird er auch in der Liste zuletzt verwendeter Objekte unter **Benutzerdefinierte Berichte**angezeigt. Wenn eine benutzerdefinierte Berichtsdatei gelöscht wird, wird beim nächsten Auswählen des Elements eine Aufforderung angezeigt, das Element aus der Liste zuletzt verwendeter Objekte zu löschen.  
  
    1.  Klicken Sie im Menü **Extras** auf **Optionen** , erweitern Sie den Ordner **Umgebung** , und klicken Sie dann auf **Allgemein**, um die Anzahl der in der Liste zuletzt verwendeter Objekte angezeigten Dateien zu ändern.  
  
    2.  Passen Sie die Anzahl für **Dateien in „Zuletzt geöffnet“-Liste angezeigt**an.  
  
## <a name="see-also"></a>Weitere Informationen  
[Benutzerdefinierte Berichte in Management Studio](../../ssms/object/custom-reports-in-management-studio.md)  
[Verwenden benutzerdefinierter Berichte mit Eigenschaften von Objekt-Explorer-Knoten](../../ssms/object/use-custom-reports-with-object-explorer-node-properties.md)  
[Aufheben der Unterdrückung von Warnungen für das Ausführen von benutzerdefinierten Berichten](../../ssms/object/unsuppress-run-custom-report-warnings.md)  
[SQL Server Reporting Services](../../reporting-services/create-deploy-and-manage-mobile-and-paginated-reports.md)  
  
