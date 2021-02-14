---
description: Berechtigungen für Hierarchieelemente (Master Data Services)
title: Hierarchieelementberechtigungen
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- members [Master Data Services], permissions
- permissions [Master Data Services], members
ms.assetid: b3880eed-1bf6-4f65-ab23-b08c194cc858
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 58c354eae5dadcc34d160b756e936dd82c3844e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347738"
---
# <a name="hierarchy-member-permissions-master-data-services"></a>Berechtigungen für Hierarchieelemente (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Berechtigungen für Hierarchieelemente sind optional und sollten nur verwendet werden, wenn ein Benutzer beschränkten Zugriff auf bestimmte Elemente erhalten soll. Wenn Sie keine Berechtigungen auf der Registerkarte **Hierarchieelemente** zuweisen, basieren die Berechtigungen eines Benutzers ausschließlich auf den Berechtigungen, die auf der Registerkarte **Modelle** zugewiesen wurden.  
  
 Berechtigungen für Hierarchie Elemente werden in der [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] Benutzeroberfläche (UI) im Funktionsbereich **Benutzer-und Gruppenberechtigungen** auf der Registerkarte **Hierarchie** Elemente zugewiesen. Diese Berechtigungen legen fest, auf welche Elemente ein Benutzer im Funktionsbereich **Explorer** der Benutzeroberfläche zugreifen kann.  
  
 Auf der Registerkarte **Hierarchieelemente** wird jede Hierarchie als Baumstruktur dargestellt. Wenn Sie eine Berechtigung für einen Knoten in der Struktur zuweisen, erben alle untergeordneten Elemente diese Berechtigung, sofern sie nicht auf einer niedrigeren Ebene explizit zugewiesen wurde.  
  
> [!NOTE]  
>  Wenn Sie einem Hierarchieknoten eine Berechtigung zuweisen, werden alle Elemente in den anderen Knoten auf gleicher oder höherer Ebene implizit verweigert.  
  
 Im **Explorer** werden die Elementberechtigungen überall dort angewendet, wo das Element angezeigt wird. Ein Element mit **Leseberechtigung** kann beispielsweise alle Entitäten, Hierarchien und Sammlungen lesen, denen es angehört.  
  
 Berechtigungen für Hierarchieelemente gelten für die Modellversion, denen sie zugewiesen werden, sowie für alle zukünftigen Kopien der Version. Sie gelten nicht für Vorgängerversionen der Version, der sie zugewiesen wurden.  
  
|Berechtigung|BESCHREIBUNG|  
|----------------|-----------------|  
|**Lesen**|Die Elemente werden angezeigt.<br /><br /> <br /><br /> Hinweis: Wenn Sie nur dem **Stamm** die **Leseberechtigung** zuweisen, sind die Elemente unter **Stamm** schreibgeschützt. In expliziten Hierarchien und Sammlungen kann der Benutzer jedoch Elemente in den **Stamm** verschieben und dem **Stamm** neue Elemente hinzufügen.|  
|**Erstellen**|Die Hierarchieelementberechtigung enthält keine Berechtigung zum Erstellen.|  
|**Aktualisieren**|Die Elemente werden angezeigt und können vom Benutzer geändert werden. Der Benutzer kann die Elemente außerdem in beliebigen expliziten Hierarchien oder Auflistungen verschieben, denen die Elemente angehören.|  
|**Löschen**|Die Elemente werden angezeigt und können vom Benutzer gelöscht werden.|  
|**Deny**|Die Elemente werden nicht angezeigt.|  
  
 Auf der Registerkarte **Hierarchieelemente** werden die zugewiesenen Berechtigungen nicht sofort wirksam. Wie häufig die Berechtigungen angewendet werden, hängt von der Einstellung **Member security processing interval setting** (Intervall für die Verarbeitung der Mitgliedersicherheit) ab, die Sie in der Tabelle „Systemeinstellungen“ der [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -Datenbank vornehmen. Eine Anleitung für die sofortige Anwendung von Elementberechtigungen finden Sie unter [Sofortiges Anwenden von Elementberechtigungen &#40;Master Data Services&#41;](../master-data-services/immediately-apply-member-permissions-master-data-services.md)neue Elemente hinzufügen.  
  
> [!NOTE]  
>  Sie können rekursiven Hierarchien, abgeleiteten Hierarchien mit expliziten Abschlüssen und abgeleiteten Hierarchien mit ausgeblendeten Ebenen keine Hierarchieelementberechtigungen zuweisen.  
  
## <a name="possible-overlapping-permissions"></a>Mögliche Überlappungen bei Berechtigungen  
 Wenn Sie Elementen eine Berechtigung zuweisen, müssen möglicherweise überlappende Berechtigungen aufgelöst werden.  
  
### <a name="when-a-member-belongs-to-multiple-hierarchies"></a>Wenn ein Element mehreren Hierarchien angehört  
 Das gleiche Element kann in zwei oder mehr Hierarchien enthalten sein.  
  
-   Wenn einem Hierarchieknoten die Berechtigung **Aktualisieren** und einem anderen die **Leseberechtigung** zugewiesen wird, sind die Elemente im Knoten **leseberechtigt**.  
  
-   Wenn einem Hierarchieknoten die Berechtigungen **Aktualisieren** und **Erstellen** und einem anderen die Berechtigungen **Aktualisieren** und **Löschen** zugewiesen sind, können die Elemente im Knoten aktualisiert werden.  
  
-   Wenn einem Hierarchie Knoten eine beliebige Kombination aus **Create** / **Read** / **Update** / **Delete** -Berechtigungen und einem anderen Knoten die Berechtigung **verweigern** zugewiesen wird, wird der Zugriff auf die Elemente im Knoten verweigert.  
  
## <a name="external-resources"></a>Externe Ressourcen  
 Blogbeitrag [Sicherheitsverbesserungen](/archive/blogs/e7/improvements-to-autoplay), auf msdn.com.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Hierarchie Element Berechtigungen &#40;Master Data Services zuweisen&#41;](../master-data-services/assign-hierarchy-member-permissions-master-data-services.md)   
 [Wie Berechtigungen &#40;Master Data Services bestimmt werden&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md)   
 [Mitglieder &#40;Master Data Services&#41;](../master-data-services/members-master-data-services.md)   
 [Hierarchien &#40;Master Data Services&#41;](../master-data-services/hierarchies-master-data-services.md)   
 [Sofortiges Anwenden von Elementberechtigungen &#40;Master Data Services&#41;](../master-data-services/immediately-apply-member-permissions-master-data-services.md)  
  
