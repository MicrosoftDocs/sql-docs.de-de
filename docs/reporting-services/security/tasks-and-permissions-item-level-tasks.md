---
description: 'Tasks und Berechtigungen: Tasks auf Elementebene'
title: Tasks auf Elementebene | Microsoft-Dokumentation
ms.date: 02/04/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- item-level tasks [Reporting Services]
ms.assetid: fdeb7bc3-167a-4342-84e3-32e3faa1fa39
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 82101577abe86914aff2ac1d5296d7dc176a60b0
ms.sourcegitcommit: 6f4fb9cfd0cad06127a6328adc745e2ba7c191d1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2021
ms.locfileid: "99570434"
---
# <a name="tasks-and-permissions---item-level-tasks"></a>Tasks und Berechtigungen: Tasks auf Elementebene
  
  [!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]
  
  Eine Aufgabe auf Elementebene ist eine Auflistung von Berechtigungen, die sich auf einen Bericht, einen Ordner, ein Berichtsmodell, eine Ressource oder eine freigegebene Datenquelle beziehen. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] enthält auch Aufgaben auf Systemebene, die sich auf die Berichtsserversite als Ganzes beziehen. Weitere Informationen finden Sie unter [Aufgaben auf Systemebene](../../reporting-services/security/tasks-and-permissions-system-level-tasks.md). Weitere Informationen zu Aufgaben und Berechtigungen im Allgemeinen finden Sie unter [Tasks and Permissions](../../reporting-services/security/tasks-and-permissions.md).  
  
> [!NOTE]  
>  Wenn Sie mit diesen Aufgaben programmgesteuert arbeiten, müssen Sie Methoden verwenden, die Aufgaben auf Elementebene unterstützen. Weitere Informationen finden Sie unter <xref:ReportService2010.ReportingService2010.ListTasks%2A> und <xref:ReportService2010.ReportingService2010.ListRoles%2A>.  
  
## <a name="permissions-in-item-level-tasks"></a>Berechtigungen in Aufgaben auf Elementebene  
 In der folgenden Tabelle sind die Aufgaben auf Elementebene, die in jeder Aufgabe enthaltenen Berechtigungen sowie die Elemente aufgelistet, für die diese Berechtigungen gelten. Die Berechtigungen sind nur zu Informationszwecken aufgeführt, um eine genauere Beschreibung der über die verschiedenen Aufgaben verfügbaren Funktionalität bereitzustellen.  
  
 Freigegebene Datasets verwenden den gleichen Satz von Berechtigungen wie Berichte. Berichtsteile verwenden den gleichen Satz von Berechtigungen wie Ressourcen.  
  
|Aufgabe|Gilt für Element|Berechtigungen|  
|----------|---------------------|-----------------|  
|Kommentieren von Berichten<br />(SSRS 2017 und höher, Power BI-Berichtsserver)|Berichte|Lesen von Eigenschaften<br /><br /> Kommentare erstellen<br /><br /> Kommentare löschen<br /><br /> Kommentare lesen<br /><br /> Kommentare aktualisieren|  
|Berichte lesen|Berichte|Lesen von Inhalt<br /><br /> Lesen von Berichtsdefinitionen<br /><br /> Lesen von Eigenschaften|  
|Berichte lesen|Freigegebene Datasets|Lesen von Inhalt<br /><br /> Lesen von Berichtsdefinitionen<br /><br /> Lesen von Eigenschaften|  
|Verknüpfte Berichte erstellen|Berichte|Erstellen von Links<br /><br /> Lesen von Eigenschaften|  
|Alle Abonnements verwalten|Berichte|Lesen von Eigenschaften<br /><br /> Lesen eines Abonnements<br /><br /> Erstellen eines Abonnements<br /><br /> Löschen eines Abonnements<br /><br /> Aktualisieren eines Abonnements|  
|Kommentare verwalten<br />(SSRS 2017 und höher, Power BI-Berichtsserver)|Berichte|Lesen von Eigenschaften<br /><br />Kommentare löschen|  
|Datenquellen verwalten|Ordner|Erstellen von Datenquellen|  
|Datenquellen verwalten|Datenquellen|Aktualisieren von Eigenschaften<br /><br /> Löschen von Updateinhalt<br /><br /> Lesen von Eigenschaften|  
|Ordner verwalten|Ordner|Erstellen eines Ordners<br /><br /> Löschen von Updateeigenschaften<br /><br /> Lesen von Eigenschaften|  
|Einzelne Abonnements verwalten|Berichte|Lesen von Eigenschaften<br /><br /> Erstellen von Abonnements<br /><br /> Löschen des Abonnements<br /><br /> Lesen von Abonnements<br /><br /> Aktualisieren des Abonnements|  
|Verwalten von Modellen|Ordner|Erstellen eines Modells|  
|Verwalten von Modellen|Modelle|Lesen von Eigenschaften<br /><br /> Lesen von Inhalt<br /><br /> Löschen von Updateinhalt<br /><br /> Lesen von Datenquellen<br /><br /> Aktualisieren von Datenquellen<br /><br /> Lesen von Autorisierungsrichtlinien für Modellelemente<br /><br /> Aktualisieren von Autorisierungsrichtlinien für Modellelemente<br /><br /> Löschen von Updateeigenschaften|  
|Berichtsverlauf verwalten|Berichte|Lesen von Eigenschaften<br /><br /> Erstellen eines Berichtsverlaufs<br /><br /> Löschen eines Berichtsverlaufs<br /><br /> Ausführen von Leserichtlinien<br /><br /> Aktualisieren von Richtlinien<br /><br /> Auflisten des Berichtsverlaufs|  
|Berichte verwalten|Ordner|Erstellen von Berichten<br /><br /> gilt auch für die Erstellung freigegebener Datasets|  
|Berichte verwalten|Berichte|Lesen von Eigenschaften<br /><br /> Löschen von Updateeigenschaften<br /><br /> Aktualisieren von Parametern<br /><br /> Lesen von Datenquellen<br /><br /> Aktualisieren von Datenquellen<br /><br /> Lesen von Berichtsdefinitionen<br /><br /> Aktualisieren von Berichtsdefinitionen<br /><br /> Ausführen von Leserichtlinien<br /><br /> Aktualisieren von Richtlinien|  
|Berichte verwalten|Freigegebene Datasets|Lesen von Eigenschaften<br /><br /> Löschen von Updateeigenschaften<br /><br /> Aktualisieren von Parametern<br /><br /> Lesen von Datenquellen<br /><br /> Aktualisieren von Datenquellen<br /><br /> Lesen von Berichtsdefinitionen<br /><br /> Aktualisieren von Berichtsdefinitionen<br /><br /> Ausführen von Leserichtlinien<br /><br /> Aktualisieren von Richtlinien|  
|Ressourcen verwalten|Ordner|Resource erstellen|  
|Ressourcen verwalten|Ressourcen|Aktualisieren von Eigenschaften<br /><br /> Löschen von Updateinhalt<br /><br /> Lesen von Eigenschaften|  
|Ressourcen verwalten|Berichtsteile|Aktualisieren von Eigenschaften<br /><br /> Löschen von Updateinhalt<br /><br /> Lesen von Eigenschaften|  
|Sicherheit für einzelne Elemente festlegen|Berichte, Ressourcen, Datenquellen, freigegebene Datasets, Ordner|Lesen von Sicherheitsrichtlinien - Aktualisieren von Sicherheitsrichtlinien|  
|Datenquellen anzeigen|Datenquellen|Lesen von Inhalt<br /><br /> Lesen von Eigenschaften|  
|Ordner anzeigen|Ordner|Lesen von Eigenschaften<br /><br /> Ausführen und Anzeigen<br /><br /> Auflisten des Berichtsverlaufs|  
|Modelle anzeigen|Berichtsmodelle|Lesen von Eigenschaften<br /><br /> Lesen von Inhalt<br /><br /> Lesen von Datenquellen|  
|Anzeigen von Berichten|Berichte|Lesen von Inhalt<br /><br /> Lesen von Eigenschaften|  
|Anzeigen von Berichten|Freigegebene Datasets|Lesen von Inhalt<br /><br /> Lesen von Eigenschaften|  
|Ressourcen anzeigen|Ressourcen|Lesen von Inhalt<br /><br /> Lesen von Eigenschaften|  
|Ressourcen anzeigen|Berichtsteile|Lesen von Inhalt<br /><br /> Lesen von Eigenschaften|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Granting Permissions on a Native Mode Report Server (Erteilen von Berechtigungen für einen Berichtsserver im einheitlichen Modus)](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)  
  
  
