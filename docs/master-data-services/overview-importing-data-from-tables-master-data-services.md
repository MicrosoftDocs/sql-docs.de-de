---
title: Importieren von Daten aus Tabellen
description: Importieren Sie Daten aus Tabellen, und nehmen Sie Änderungen an den Daten vor, nachdem Sie ein Modell für Ihre Daten in Master Data Services erstellt haben.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- staging process [Master Data Services], about staging process
- importing data [Master Data Services]
- staging process [Master Data Services]
ms.assetid: 181d1e22-379c-45d1-b03c-e1e22ff14164
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ca89907d14bdac1667a84be98a6cff53a44b4034
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340268"
---
# <a name="overview-importing-data-from-tables-master-data-services"></a>Übersicht: Importieren von Daten aus Tabellen (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Nach der Erstellung eines Modells für Ihre Daten in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]können Sie beginnen, Daten hinzufügen und Änderungen an den Daten vorzunehmen.   Sie verwenden [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -Stagingtabellen, gespeicherte Prozeduren und Master Data Manager.  
  
 Informationen zum Hinzufügen und Ändern von Daten finden Sie unter [Importieren von Daten aus Tabellen &#40;Master Data Services&#41;](../master-data-services/import-data-from-tables-master-data-services.md).  
  
> [!NOTE]
>  Ferner können Sie [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)][!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)]verwenden, um dem MDS-Repository ([!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]-Datenbank) Daten aus Excel hinzuzufügen. Weitere Informationen finden Sie unter [Übersicht: Importieren von Daten aus Excel &#40;MDS-Add-In für Excel&#41;](../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md).  
  
 Beim Hinzufügen und Ändern von Daten können Sie Folgendes tun.  
  
-   Laden und Aktualisieren von Elementen und Aktualisieren von Attributwerten  
  
-   Deaktivieren und Löschen von Elementen  
  
-   Verschieben von Elementen der expliziten Hierarchie  
  
 Das Hinzufügen und Aktualisieren von Daten besteht hauptsächlich in den folgenden Aufgaben.  
  
1.  Laden von Daten in die Stagingtabellen in der [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -Datenbank.  
  
2.  Laden von Daten aus den Stagingtabellen in die entsprechenden [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -Tabellen.  
  
     Zum Laden der Daten verwenden Sie gespeicherte Prozeduren oder [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] .  
  
> [!NOTE]  
>  In [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]ist die Unterstützung für die [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)] -Stagingprozesse veraltet.  
  
## <a name="deactivating-and-deleting-members-mds"></a>Deaktivieren und Löschen von Elementen (MDS)  
 Deaktivieren bedeutet, dass das Element erneut aktiviert werden kann. Wenn Sie ein Element erneut aktivieren, werden seine Attribute und Mitgliedschaft in Hierarchien und Auflistungen wiederhergestellt. Alle vorherigen Transaktionen sind intakt. Deaktivierungstransaktionen werden Administratoren im Funktionsbereich **Versionsverwaltung** von Master Data Manager angezeigt.  
  
 Löschen bedeutet, das Element dauerhaft vom System zu entfernen. Alle Transaktionen für das Element, alle Beziehungen und alle Attribute werden dauerhaft gelöscht.  
  
> [!NOTE]  
>  Per Staging können Sie keine Elemente erneut aktivieren. Diesen Vorgang müssen Sie manuell in Master Data Manager ausführen. Weitere Informationen finden Sie unter [Reaktivieren eines Elements oder einer Sammlung &#40;Master Data Services&#41](../master-data-services/reactivate-a-member-or-collection-master-data-services.md).  
>   
>  Per Staging können Sie keine Auflistungen löschen oder deaktivieren. Weitere Informationen zum manuellen Deaktivieren von Sammlungen finden Sie unter [Löschen eines Elements oder einer Sammlung &#40;Master Data Services&#41;](../master-data-services/delete-a-member-or-collection-master-data-services.md).  
  
## <a name="moving-explicit-hierarchy-members-mds"></a>Verschieben von Elementen der expliziten Hierarchie (MDS)  
 Beim Massenverschieben der Position von Elementen der expliziten Hierarchie können Sie die folgenden Festlegungen treffen.  
  
-   Ein konsolidiertes Element als übergeordnetes Element eines konsolidierten Elements  
  
-   Ein konsolidiertes Element als übergeordnetes Element eines Blattelements  
  
-   Ein Blattelement als ein gleichgeordnetes Element eines Blattelements oder eines konsolidierten Elements  
  
-   Ein konsolidiertes Element als ein gleichgeordnetes Element eines Blattelements oder eines konsolidierten Elements  
  
## <a name="staging-tables-and-stored-procedures-mds"></a>Stagingtabellen und gespeicherte Prozeduren (MDS)  
 Die [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -Datenbank enthält die folgenden Typen von Stagingtabellen, die Sie mit Ihren Daten auffüllen können.  
  
-   [Stagingtabelle für Blattelemente &#40;Master Data Services&#41;](../master-data-services/leaf-member-staging-table-master-data-services.md)  
  
-   [Konsolidierte Elementstagingtabelle &#40;Master Data Services&#41;](../master-data-services/consolidated-member-staging-table-master-data-services.md)  
  
-   [Stagingtabelle für Beziehungen &#40;Master Data Services&#41;](../master-data-services/relationship-staging-table-master-data-services.md)  
  
 Für jede Entität im Modell gibt es eine Stagingtabelle. Der Tabellenname gibt die entsprechende Entität und den Entitätstyp an, wie etwa ein Blattelement. In der folgenden Abbildung sind die Stagingtabellen für die Entitäten „Währung“, „Kunde“ und „Produkt“ dargestellt.  
  
 ![Stagingtabellen in der MDS-Datenbank](../master-data-services/media/mds-staging-tables.png "Stagingtabellen in der MDS-Datenbank")  
  
 Der Name der Tabelle wird beim Erstellen einer Entität angegeben und kann nicht geändert werden. Wenn der Stagingtabellenname eine _1 oder eine andere Zahl enthält, war eine andere Tabelle dieses Namens bereits vorhanden, als die Entität erstellt wurde.  
  
 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] beinhaltet die folgenden Typen von gespeicherten Stagingprozeduren.  
  
-   STG.udp_ \<name> _Leaf  
  
-   STG.udp_ \<name> _Consolidated  
  
-   STG.udp_ \<name> _Relationship  
  
 Für jede Entität im Modell gibt es drei gespeicherte Prozeduren, die dem Blattelement, dem konsolidierten Element und der Stagingtabelle für Beziehungen entsprechen.  In der folgenden Abbildung sind die gespeicherten Stagingprozeduren für die Entitäten „Währung“, „Kunde“ und „Produkt“ dargestellt.  
  
 ![Staging gespeicherter Prozeduren in der MDS-Datenbank](../master-data-services/media/mds-staging-storedprocedures.png "Staging gespeicherter Prozeduren in der MDS-Datenbank")  
  
 Weitere Informationen zu den gespeicherten Prozeduren finden Sie unter [Gespeicherte Stagingprozedur &#40;Master Data Services&#41;](../master-data-services/staging-stored-procedure-master-data-services.md).  
  
## <a name="logging-transactions-mds"></a>Protokollieren von Transaktionen (MDS)  
 Alle Transaktionen, die auftreten, wenn Daten oder Beziehungen importiert oder aktualisiert werden, können protokolliert werden. Eine Option in der gespeicherten Prozedur ermöglicht diese Protokollierung. Wenn Sie den Stagingprozess mithilfe von [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]einleiten, erfolgt keine Protokollierung.  
  
 In [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]wird die Einstellung **Protokollieren aller Stagingtransaktionen** nicht für diese Methode zum Bereitstellen von Daten angewendet.  
  
## <a name="related-content"></a>Verwandte Inhalte  
  
-   [Überprüfung &#40;Master Data Services&#41;](../master-data-services/validation-master-data-services.md)  
  
-   [Geschäftsregeln &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)  
  
  
