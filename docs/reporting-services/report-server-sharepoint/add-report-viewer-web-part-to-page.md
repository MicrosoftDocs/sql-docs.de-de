---
title: Hinzufügen des Webparts des Berichts-Viewers für SQL Server Reporting Services zu einer SharePoint-Seite | Microsoft-Dokumentation
description: Zeigen Sie einen Bericht von SQL Server Reporting Services oder Power BI-Berichtsserver an, indem Sie das Berichts-Viewer-Webpart zu einer SharePoint-Seite hinzufügen.
ms.date: 11/26/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 7102f8cd25e3d0bdc2176308cb749b8d73290e0a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060595"
---
# <a name="add-sql-server-reporting-services-report-viewer-web-part-to-a-sharepoint-page"></a>Hinzufügen des Webparts des Berichts-Viewers für SQL Server Reporting Services zu einer SharePoint-Seite

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

Zeigen Sie einen Bericht von SQL Server Reporting Services oder Power BI-Berichtsserver an, indem Sie das Berichts-Viewer-Webpart zu einer SharePoint-Seite hinzufügen.

![Berichts-Viewer-Webpart auf einer SharePoint-Website](media/sharepoint-report-viewer-web-part-on-page.png)

## <a name="prerequisites"></a>Voraussetzungen

* Damit Berichte erfolgreich geladen werden können, muss für Claims to Windows Token Service (C2WTS) die eingeschränkte Kerberos-Delegierung konfiguriert werden. Weitere Informationen zu C2WTS finden Sie unter [Claims to Windows Token Service (C2WTS) and Reporting Services (Forderungen an den Windows-Tokendienst (C2WTS) und Reporting Services)](../install-windows/claims-to-windows-token-service-c2wts-and-reporting-services.md).

* Das Berichts-Viewer-Webpart muss für Ihre SharePoint-Farm bereitgestellt werden. Weitere Informationen zur Bereitstellung des Berichts-Viewer-Webpart-Projektmappenprojekts finden Sie unter [Deploy the Report Viewer web part on a SharePoint site (Bereitstellen des Berichts-Viewer-Webparts auf einer SharePoint-Website)](deploy-report-viewer-web-part.md).

* Sie müssen über eine Berechtigung zum Hinzufügen und Anpassen von Seiten auf Websiteebene verfügen, um einer Webseite ein Webpart hinzufügen zu können. Wenn Sie Standardsicherheitseinstellungen verwenden, wird diese Berechtigung Mitgliedern der Gruppe **Besitzer** erteilt, die über Berechtigungen für den Vollzugriff verfügen.

## <a name="add-web-part"></a>Hinzufügen eines Webparts

1. Klicken Sie auf Ihrer SharePoint-Website auf das **Zahnradsymbol** in der oberen linken Ecke und anschließend auf **Seite hinzufügen**.

    ![Fügen Sie über das Zahnradsymbol eine Seite zu einer SharePoint-Website hinzu.](media/sharepoint-add-a-page.png)

2. Benennen Sie Ihre Seite, und klicken Sie auf **Erstellen**.

3. Klicken Sie im Seiten-Designer im Menüband auf die Registerkarte **Einfügen**. Klicken Sie anschließend im Abschnitt **Parts** auf **Webpart**.

    ![Fügen Sie über das Menüband ein Webpart ein.](media/sharepoint-insert-web-part.png)

4. Klicken Sie unter **Kategorien** auf **SQL Server Reporting Services (einheitlicher Modus)** . Klicken Sie unter **Parts** auf **Berichts-Viewer**. Wählen Sie anschließend **Hinzufügen**.

    ![Fügen Sie das Berichts-Viewer-Webpart hinzu.](media/sharepoint-report-viewer-web-part.png)

    Möglicherweise wird Ihnen anfangs ein Fehler angezeigt. Dieser Fehler tritt auf, weil die Standard-URL auf *https://localhost* festgelegt und an diesem Speicherort möglicherweise nicht verfügbar ist.

## <a name="configure-the-report-viewer-web-part"></a>Konfigurieren des Berichts-Viewer-Webparts

Führen Sie folgende Schritte aus, um das Webpart zu konfigurieren, das auf einen bestimmten Bericht zeigen soll.

1. Wenn Sie die SharePoint-Seite bearbeiten, klicken Sie auf den Pfeil nach unten in der oberen rechten Ecke des Webparts, und wählen Sie **Webpart bearbeiten** aus.

    ![Bearbeiten Sie die Webseite über die Dropdownliste des Webparts.](media/sharepoint-edit-web-part.png)

2. Geben Sie die **Berichtsserver-URL** des Berichtsserver ein, der Ihren Bericht hostet. Die URL sollte in etwa wie folgt aussehen: *https://myrsserver/reportserver* .

3. Geben Sie den Pfad und den Namen des Berichts an, den Sie im Webpart anzeigen möchten. Dies sollte in etwa wie folgt aussehen: */AdventureWorks Sample Reports/Company Sales*. In diesem Beispiel befindet sich der Bericht *Company Sales* (Firmenumsatz) in einem Ordner mit der Bezeichnung *AdventureWorks Sample Reports* (AdventureWorks-Beispielberichte).

4. Wenn für Ihren Bericht Parameter benötigt werden, nachdem Sie die Berichtsserver-URL und den Namen des Berichts eingegeben haben, klicken Sie auf **Parameter laden** im Abschnitt **Parameter**.

5. Klicken Sie auf **OK**, um Ihre Änderungen der Webpart-Konfiguration zu speichern.

6. Klicken Sie im Office-Menüband auf **Speichern**, um die Änderungen an der SharePoint-Seite zu speichern.

![Berichts-Viewer-Webpart auf einer SharePoint-Website](media/sharepoint-report-viewer-web-part-on-page.png)

## <a name="considerations-and-limitations"></a>Überlegungen und Einschränkungen

* Das Berichts-Viewer-Webpart kann in SharePoint nicht auf modernen Seiten verwendet werden.
* Power BI-Berichte können mit dem Berichts-Viewer-Webpart nicht verwendet werden.
* Wenn Ihnen das Berichts-Viewer-Webpart nicht angezeigt wird und ihn zu Ihrer Seite hinzufügen möchten, stellen Sie sicher, dass Sie den [Berichts-Viewer-Webpart bereitgestellt haben](deploy-report-viewer-web-part.md).

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
