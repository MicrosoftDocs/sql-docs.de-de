---
title: Verwalten von Servern mit der richtlinienbasierten Verwaltung
description: Erfahren Sie, wie Sie die richtlinienbasierte Verwaltung für die Verwaltung einer oder mehrerer Instanzen von SQL Server verwenden.
ms.custom: seo-lt-2019
ms.date: 08/12/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- facet See facets
- Declarative Management Framework See Policy-Based Management
- surface area configuration [SQL Server], Policy-Based Management
- Policy-Based Management
- facets [Policy-Based Management]
- Policy-Based Management, administering
- conditions [Policy-Based Management]
- facets [Policy-Based Management], about facets
- PolicyAdministratorRole role
ms.assetid: ef2a7b3b-614b-405d-a04a-2464a019df40
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 721f2d692d95f8bdccbdf351193fb63c36ad146b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345521"
---
# <a name="administer-servers-by-using-policy-based-management"></a>Verwalten von Servern mit der richtlinienbasierten Verwaltung
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
   Die richtlinienbasierte Verwaltung ist ein richtlinienbasiertes System zum Verwalten einer oder mehrerer Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Verwenden Sie diese, um Bedingungen zu erstellen, die Bedingungsausdrücke enthalten. Erstellen Sie dann Richtlinien, die die Bedingungen für Datenbankzielobjekte übernehmen.  

Beispielsweise könnte es sein, dass Sie als Datenbankadministrator sicherstellen möchten, dass für bestimmte Server Datenbank-E-Mail nicht aktiviert ist. Also erstellen Sie eine Bedingung und eine Richtlinie, die diese Serveroption festgelegt. 
   
 > **WICHTIG!** Richtlinien können die Funktionsweise einiger Funktionen beeinflussen. Beispielsweise verwenden Change Data Capture und die Transaktionsreplikation beide die systranschemas-Tabelle, die keinen Index hat. Wenn Sie eine Richtlinie aktivieren, die festlegt, dass alle Tabellen einen Index aufweisen müssen, bewirkt die Erzwingung der Einhaltung der Richtlinie das Fehlschlagen dieser Funktionen.  
  
 Verwenden Sie SQL Server Management Studio, um Richtlinien für folgenden Aufgaben zu erstellen und zu verwalten:
  
1.  Wählen Sie ein Facet der richtlinienbasierten Verwaltung aus, das die zu konfigurierenden Eigenschaften enthält.  
  
2.  Definieren Sie eine Bedingung, die den Status eines Verwaltungsfacets angibt.  
  
3.  Definieren Sie eine Richtlinie, die die Bedingung, zusätzliche Bedingungen zum Filtern der Zielsätze und den Auswertungsmodus enthält.  
  
4.  Überprüfen Sie, ob eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in Übereinstimmung mit der Richtlinie ist.  
  
 Für Fehler bei Richtlinien wird im Objekt-Explorer eine kritische Zustandswarnung in Form eines roten Symbols neben dem Ziel und den übergeordneten Knoten in der Strukturansicht des Objekt-Explorers angezeigt.  
  
> **HINWEIS:** Wenn das System den Objektsatz für eine Richtlinie berechnet, werden die Systemobjekte standardmäßig ausgeschlossen.  Falls der Objektsatz der Richtlinie z. B. auf alle Tabellen verweist, gilt die Richtlinie nicht für Systemtabellen. Wenn Benutzer eine Richtlinie in Verbindung mit Systemobjekten auswerten möchten, können sie dem Objektsatz Systemobjekte explizit hinzufügen. Obwohl alle Richtlinien für den Auswertungsmodus **Zeitplan prüfen** unterstützt werden, werden aus Leistungsgründen jedoch nicht alle Richtlinien mit beliebigen Objektsätzen für den Auswertungsmodus **Änderungen prüfen** unterstützt. Weitere Informationen finden Sie unter [https://docs.microsoft.com/archive/blogs/sqlpbm/policy-evaluation-modes](/archive/blogs/sqlpbm/policy-evaluation-modes).  
  
## <a name="three-policy-based-management-components"></a>Drei Komponenten für richtlinienbasierte Verwaltung  
 Die richtlinienbasierte Verwaltung besteht aus drei Komponenten:  
  
-   Richtlinienverwaltung. Richtlinienadministratoren erstellen Richtlinien.  
  
-   Explizite Verwaltung. Administratoren wählen ein oder mehrere verwaltete Ziele aus. Für diese Ziele überprüfen sie explizit, ob sie eine bestimmte Richtlinie einhalten oder veranlassen explizit, dass die Ziele eine Richtlinie einhalten.  
  
-   Auswertungsmodi. Es gibt vier Auswertungsmodi, von denen drei automatisiert werden können:  
  
    -   **Bedarfsgesteuert**. Dieser Modus wertet die Richtlinie aus, wenn sie vom Benutzer direkt angegeben wird.  
  
    -   **Bei Änderung - Verhindern**. Dieser automatisierte Modus verwendet DDL-Trigger, um Richtlinienverstöße zu verhindern.  
  
        > **WICHTIG!** Wenn die Serverkonfigurationsoption für geschachtelte Trigger deaktiviert ist, funktioniert **Bei Änderung: Verhindern** nicht ordnungsgemäß. Die richtlinienbasierte Verwaltung basiert auf DDL-Triggern, um DDL-Vorgänge zu erkennen und dafür ein Rollback auszuführen, falls diese Vorgänge nicht mit Richtlinien übereinstimmen, die diesen Auswertungsmodus verwenden. Das Entfernen der DDL-Trigger für die richtlinienbasierte Verwaltung oder das Deaktivieren geschachtelter Trigger bewirkt das Fehlschlagen oder ein unerwartetes Ausführungsverhalten dieses Auswertungsmodus.  
  
    -   **Bei Änderung: Nur protokollieren**. Dieser automatisierte Modus verwendet die Ereignisbenachrichtigung, um eine Richtlinie auszuwerten, wenn eine relevante Änderung vorgenommen wurde.  
  
    -   **Nach Zeitplan**. Dieser automatisierte Modus verwendet einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agentauftrag, um eine Richtlinie in regelmäßigen Abständen auszuwerten.  
  
     Wenn keine automatisierten Richtlinien aktiviert sind, hat die richtlinienbasierte Verwaltung keine Auswirkungen auf die Systemleistung.  
  
## <a name="terms"></a>Begriffe  
 **Verwaltetes Ziel der richtlinienbasierten Verwaltung**: Entitäten, die durch die richtlinienbasierte Verwaltung verwaltet werden, beispielsweise eine [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]-Instanz, eine Datenbank, eine Tabelle oder ein Index Alle Ziele in einer Serverinstanz bilden eine Zielhierarchie. Ein Zielsatz ist der Satz der Ziele, der sich aus der Anwendung eines Satzes von Zielfiltern auf die Zielhierarchie ergibt, z. B. alle Tabellen in der Datenbank, die im Besitz des HumanResources-Schemas sind.  
  
 **Facet der richtlinienbasierten Verwaltung**: Eine Menge logischer Eigenschaften, die das Verhalten oder die Eigenschaften bestimmter Typen von verwalteten Zielen modellieren Die Anzahl und die Merkmale der Eigenschaften sind in das Facet integriert und können nur durch den Ersteller des Facets hinzugefügt oder entfernt werden. Ein Zieltyp kann ein oder mehrere Verwaltungsfacets implementieren, und ein Verwaltungsfacet kann von einem oder mehreren Zieltypen implementiert werden. Bestimmte Eigenschaften eines Facets können nur für eine bestimmte Version gelten.  
  
 **Bedingung der richtlinienbasierten Verwaltung**  
 Ein boolescher Ausdruck, der einen Satz zulässiger Zustände eines durch die richtlinienbasierte Verwaltung verwalteten Ziels für ein Verwaltungsfacet angibt. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , Sortierungen beizubehalten. Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sortierungen den Windows-Sortierungen nicht genau entsprechen, testen Sie die Bedingung, um zu ermitteln, wie Konflikte vom Algorithmus aufgelöst werden.  
  
 **Richtlinie der richtlinienbasierten Verwaltung**  
 Eine Bedingung der richtlinienbasierten Verwaltung und das erwartete Verhalten, wie z. B. Auswertungsmodus, Zielfilter und Zeitplan. Eine Richtlinie kann nur eine einzige Bedingung enthalten. Richtlinien können aktiviert oder deaktiviert sein. Richtlinien werden in der msdb-Datenbank gespeichert.  
  
 **Kategorie der richtlinienbasierten Verwaltung**  
 Eine benutzerdefinierte Kategorie, die das Verwalten von Richtlinien unterstützt. Benutzer können Richtlinien in verschiedene Richtlinienkategorien klassifizieren. Eine Richtlinie gehört immer nur zu einer Richtlinienkategorie. Richtlinienkategorien gelten für Datenbanken und Server. Auf der Datenbankebene gelten folgende Bedingungen:  
  
-   Datenbankbesitzer können einen Satz von Richtlinienkategorien für eine Datenbank abonnieren.  
  
-   Nur Richtlinien aus Kategorien, die eine Datenbank abonniert hat, können diese Datenbank steuern.  
  
-   Alle Datenbanken abonnieren implizit die Standardrichtlinienkategorie.  
  
 Auf Serverebene können Richtlinienkategorien auf alle Datenbanken angewendet werden.  
  
 **Effektive Richtlinie**  
 Die effektiven Richtlinien für ein Ziel sind die Richtlinien, die dieses Ziel steuern. Eine Richtlinie ist nur dann für ein Ziel effektiv, wenn alle folgenden Bedingungen erfüllt werden:  
  
-   Die Richtlinie ist aktiviert.  
  
-   Das Ziel gehört zum Zielsatz der Richtlinie.  
  
-   Für das Ziel oder einen der Vorgänger des Ziels ist die Richtliniengruppe abonniert, die diese Richtlinie enthält.  
  
## <a name="links-to-specific-tasks"></a>Links zu bestimmten Aufgaben 

 - [Speichern von Richtlinien zur richtlinienbasierten Verwaltung](policy-based-management-storage.md)
 - [Konfigurieren von Warnungen zur Benachrichtigung von Richtlinienadministratoren bei Richtlinienfehlern](../../relational-databases/policy-based-management/configure-alerts-to-notify-policy-administrators-of-policy-failures.md)
 - [Erstellen einer neuen Bedingung der richtlinienbasierten Verwaltung](../../relational-databases/policy-based-management/create-a-new-policy-based-management-condition.md)
 - [Löschen einer Bedingung der richtlinienbasierten Verwaltung](../../relational-databases/policy-based-management/delete-a-policy-based-management-condition.md)
 - [Anzeigen oder Ändern der Eigenschaften einer Bedingung der richtlinienbasierten Verwaltung](../../relational-databases/policy-based-management/view-or-modify-the-properties-of-a-policy-based-management-condition.md)
 - [Exportieren einer Richtlinie der richtlinienbasierten Verwaltung](../../relational-databases/policy-based-management/export-a-policy-based-management-policy.md)
 - [Importieren einer Richtlinie der richtlinienbasierten Verwaltung](../../relational-databases/policy-based-management/import-a-policy-based-management-policy.md)
 - [Auswerten einer Richtlinie der richtlinienbasierten Verwaltung von einem Objekt aus](../../relational-databases/policy-based-management/evaluate-a-policy-based-management-policy-from-an-object.md)
 - [Arbeiten mit Facets der richtlinienbasierten Verwaltung](../../relational-databases/policy-based-management/working-with-policy-based-management-facets.md)
 - [Überwachen und Erzwingen von Best Practices mit der richtlinienbasierten Verwaltung](../../relational-databases/policy-based-management/monitor-and-enforce-best-practices-by-using-policy-based-management.md)

## <a name="see-also"></a>Weitere Informationen  
 
 - [Tutorial: Erstellen und Anwenden der Richtlinie „Standardmäßig aus“](lesson-1-create-and-apply-an-off-by-default-policy.md)
 - [Tutorial: Erstellen und Anwenden einer Richtlinie für Benennungsstandards](lesson-2-create-and-apply-a-naming-standards-policy.md)
 - [Sichten der richtlinienbasierten Verwaltung &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)