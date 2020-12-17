---
title: Bearbeiten einer Datenwarnung im Warnungs-Designer | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie eine Datenwarnung bearbeiten. Sie können Datenwarnungen bearbeiten, indem Sie über den Datenwarnungs-Manager auf diese zugreifen.
ms.date: 07/02/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- editing, data alerts
- updating, data alerts
- editing, alerts
- updating, alerts
ms.assetid: dde3664d-90b5-4b12-969e-39152c86e58a
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: b97a4a51a8edbce4172b92fee7a418b38ce42bc4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472541"
---
# <a name="edit-a-data-alert-in-alert-designer"></a>Bearbeiten einer Datenwarnung im Warnungs-Designer

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016](../includes/ssrs-appliesto-2016.md)] [!INCLUDE [ssrs-appliesto-not-2017](../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../includes/ssrs-previous-versions.md)]

Öffnen Sie die mit dem Datenwarnungs-Manager zu bearbeitende Datenwarnungsdefinition. Nur der Benutzer, der die Warnungsdefinition erstellt hat, kann sie bearbeiten. Weitere Informationen zum Öffnen des Datenwarnungs-Managers finden Sie unter [Verwalten meiner Datenwarnungen im Datenwarnungs-Manager](../reporting-services/manage-my-data-alerts-in-data-alert-manager.md).

> [!NOTE]
> Die Integration von Reporting Services in SharePoint ist nach SQL Server 2016 nicht mehr möglich.

 Das folgende Bild zeigt das Kontextmenü einer Datenwarnung im Datenwarnungs-Manager.  
  
 ![Öffnen des Datenwarnungs-Designers durch Klicken auf „Edit“ (Bearbeiten)](../reporting-services/media/rs-alertmanageriwopendesigner.gif "Öffnen des Datenwarnungs-Designers durch Klicken auf „Edit“ (Bearbeiten)")  
  
 Die folgende Prozedur enthält die Schritte zum Öffnen der Warnungsdefinition im Datenwarnungs-Designer über den Datenwarnungs-Manager, um sie zu bearbeiten.  
  
### <a name="to-edit-a-data-alert-definition-in-data-alert-designer"></a>So bearbeiten Sie eine Datenwarnungsdefinition mit dem Datenwarnungs-Designer  
  
1.  Klicken Sie im Datenwarnungs-Manager mit der rechten Maustaste auf die zu bearbeitende Datenwarnungsdefinition, und klicken Sie dann auf **Bearbeiten**.  
  
     Die Warnungsdefinition wird im Datenwarnungs-Designer geöffnet.  
  
2.  Aktualisieren Sie die Regeln, Zeitplaneinstellungen und E-Mail-Einstellungen. Weitere Informationen finden Sie unter [Datenwarnungs-Designer](../reporting-services/data-alert-designer.md) und [Erstellen einer Datenwarnung im Datenwarnungs-Designer](../reporting-services/create-a-data-alert-in-data-alert-designer.md).  
  
    > [!NOTE]  
    >  Sie können keinen anderen Datenfeed auswählen. Um einen anderen Datenfeed zu verwenden, erstellen Sie eine neue Datenwarnungsdefinition.  
  
3.  Klicken Sie auf **Speichern**.  
  
    > [!NOTE]  
    >  Wenn der Bericht und die aus dem Bericht generierten Datenfeeds geändert wurden, ist die Warnungsdefinition möglicherweise nicht mehr gültig. Dies tritt auf, wenn eine Spalte, auf die die Warnungsdefinition in ihren Regeln verweist, vom Bericht gelöscht wird, den Datentyp ändert, oder wenn der Bericht gelöscht oder verschoben wird. Sie können eine ungültige Warnungsdefinition öffnen. Sie können sie jedoch nicht erneut speichern, sofern sie nicht gemäß der aktuellen Version des Berichtsdatenfeeds gültig ist, auf dem sie basiert. Weitere Informationen dazu, wie Datenfeeds aus Berichten generiert werden, finden Sie unter [Generieren von Datenfeeds aus Berichten (Berichts-Generator und SSRS)](../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md).  

## <a name="see-also"></a>Weitere Informationen

[Datenwarnungs-Manager für Warnungsadministratoren](../reporting-services/data-alert-manager-for-alerting-administrators.md)   
[Reporting Services-Datenwarnungen](../reporting-services/reporting-services-data-alerts.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
