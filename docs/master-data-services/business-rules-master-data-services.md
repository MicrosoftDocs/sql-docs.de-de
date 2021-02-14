---
title: Geschäftsregeln
description: Erfahren Sie mehr über Geschäftsregeln in Master Data Services, die Daten aktualisieren, e-Mails senden oder einen Geschäftsprozess oder Workflow starten können.
ms.custom: ''
ms.date: 03/18/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], about business rules
- business rules [Master Data Services]
ms.assetid: a9f9e41a-2461-4845-b947-58b3a205543f
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4869875867f7b1cad99780b1ec82664c9d569201
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341481"
---
# <a name="business-rules-master-data-services"></a>Geschäftsregeln (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]ist eine Geschäftsregel ist eine Regel, mit der Sie die Qualität und Genauigkeit der Masterdaten sicherstellen. Sie können eine Geschäftsregel zum automatischen Aktualisieren von Daten, zum Senden von E-Mails oder zum Starten eines Geschäftsprozesses oder Workflows verwenden.  
  
 Beispiele für Geschäftsregeln finden Sie unter [Beispiele für Geschäftsregeln &#40;Master Data Services&#41;](../master-data-services/business-rule-examples-master-data-services.md).  
  
## <a name="create-and-publish-business-rules"></a>Erstellen und Veröffentlichen von Geschäftsregeln  
 Geschäftsregeln sind **If/Then/Else** -Anweisungen, die Sie in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]erstellen. Wenn ein Attributwert eine angegebene Bedingung erfüllt, wird eine Aktion ausgeführt. Andernfalls erfolgt eine Else-Aktion. Zu den möglichen Aktionen zählen das Festlegen eines Standardwerts oder Ändern eines Werts. Diese Aktionen können mit dem Senden einer E-Mail-Benachrichtigung kombiniert werden.  
  
 Geschäftsregeln können auf bestimmten Attributwerten (z. B. Aktion, wenn Color=Blue) oder Attributwertänderungen basieren (z. B. Aktion, wenn der Wert des Color-Attributs geändert wird). Weitere Informationen zur Ablaufverfolgung für nicht spezifische Änderungen finden Sie unter [Änderungsnachverfolgung &#40;Master Data Services&#41;](../master-data-services/change-tracking-master-data-services.md).  
  
 Um Geschäftsregeln zu verwenden, müssen Sie zuerst die Regeln erstellen und veröffentlichen und dann die veröffentlichten Regeln auf die Daten anwenden. Sie können Regeln auf eine Teilmenge der Daten oder auf alle Daten für eine Version anwenden, indem Sie die Version überprüfen. Ein Commit kann erst dann für eine Version ausgeführt werden, wenn alle Attribute die Geschäftsregelüberprüfung bestanden haben.  
  
 Wenn ein Benutzer versucht, einen Attributwert hinzuzufügen, der die Geschäftsregelüberprüfung nicht bestanden hat, kann der Wert dennoch gespeichert werden. Sie können die Überprüfungsprobleme, die in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]angezeigt werden, analysieren und korrigieren.  
  
 Wenn Sie ein Modellbereitstellungspaket erstellen und Geschäftsregeln einschließen möchten, müssen Sie Daten aus der Version in das Paket einschließen.  
  
 Wenn Sie eine Geschäftsregel erstellen, die den **OR** -Operator verwendet, sollten Sie eine separate Regel für jede Bedingungsanweisung erstellen, die unabhängig ausgewertet werden kann. Sie können dann Regeln nach Bedarf ausschließen und so mehr Flexibilität und eine einfachere Problembehandlung bereitstellen.  
  
## <a name="how-business-rules-are-applied"></a>So werden Geschäftsregeln übernommen  
 Sie können die Priorität der Regelausführung festlegen, indem Sie Geschäftsregeln nach oben oder unten verschieben. Bevor die Priorität jedoch berücksichtigt wird, werden Geschäftsregeln auf Grundlage des Aktionstyps der Regel angewendet. Die Reihenfolge ist die Folgende:  
  
1.  **Standardwert**  
  
2.  **Wert ändern**  
  
3.  **Überprüfung**  
  
4.  **Externe Aktion**  
  
5.  **Benutzerdefiniertes Aktionsskript**  
  
 Innerhalb dieser Gruppen werden Aktionen in der Prioritätsreihenfolge übernommen, von der niedrigsten zur höchsten. Vier separate Regeln könnten so z. B. **Standardwertaktionen** aufweisen. Die **Standardwertaktion** , die zuerst erfolgt, hängt von der Prioritätsreihenfolge ab, die auf Web-Benutzeroberfläche angegeben wurde.  
  
 Andere wichtige Hinweise zum Anwenden von Regeln:  
  
-   Wenn eine Geschäftsregel ausgeschlossen oder nicht mit dem Status **Aktiv** veröffentlicht wurde, ist die Regel zwar verfügbar, wird aber beim Anwenden von Geschäftsregeln nicht eingeschlossen.  
  
-   Geschäftsregeln werden auf die Attributwerte für alle Blattelemente oder alle konsolidierten Elemente angewendet, jedoch nicht auf beide.  
  
-   Geschäftsregeln können auf eine beliebige Version eines Modells angewendet werden, das **Offen** oder **Gesperrt** ist.  
  
-   Änderungen an Daten bei der Anwendung von Geschäftsregeln werden nicht als Transaktionen protokolliert.  
  
-   Eine Geschäftsregel kann nicht mehr als eine Aktion vom Typ **Workflow starten** enthalten.  
  
## <a name="system-settings"></a>Systemeinstellungen  
 Es gibt zwei Einstellungen in [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] , die sich auf Geschäftsregeln auswirken. Sie können diese Einstellungen in [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] oder direkt in der Tabelle Systemeinstellungen anpassen. Weitere Informationen finden Sie unter [Systemeinstellungen &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Taskbeschreibung|Thema|  
|----------------------|-----------|  
|Erstellen und veröffentlichen Sie eine neue Geschäftsregel.|[Erstellen und Veröffentlichen einer Geschäftsregel &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md)|  
|Fügen Sie einer Geschäftsregel mehrere Bedingungen hinzu.|[Hinzufügen mehrerer Bedingungen zu einer Geschäftsregel &#40;Master Data Services&#41;](../master-data-services/add-multiple-conditions-to-a-business-rule-master-data-services.md)|  
|Erstellen Sie eine Geschäftsregel, damit Attribute über Werte verfügen müssen.|[Erfordern von Attributwerten &#40;Master Data Services&#41;](../master-data-services/require-attribute-values-master-data-services.md)|  
|Erstellen Sie eine Geschäftsregel, um eine Aktion auf der Grundlage der Änderungen an Attributwerten auszuführen.|[Initiieren von Aktionen auf der Grundlage von Attributwertänderungen &#40;Master Data Services&#41;](../master-data-services/initiate-actions-based-on-attribute-value-changes-master-data-services.md)|  
|Erstellen Sie eine Geschäftsregel, um ein benutzerdefiniertes Skript als Bedingung zu verwenden.|[Geschäftsregelerweiterung &#40;Master Data Services&#41;](../master-data-services/business-rules-extension-master-data-services.md)|  
|Erstellen Sie eine Geschäftsregel, um ein benutzerdefiniertes Skript als Aktion zu verwenden.|[Geschäftsregelerweiterung &#40;Master Data Services&#41;](../master-data-services/business-rules-extension-master-data-services.md)|  
|Ändern Sie den Namen einer vorhandenen Geschäftsregel.|[Ändern des Namens einer Geschäftsregel &#40;Master Data Services&#41;](../master-data-services/change-a-business-rule-name-master-data-services.md)|  
|Konfigurieren Sie [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , um Benachrichtigungen zu senden, wenn Geschäftsregeln angewendet werden.|[Konfigurieren von Geschäftsregeln für das Senden von Benachrichtigungen &#40;Master Data Services&#41;](../master-data-services/configure-business-rules-to-send-notifications-master-data-services.md)|  
|Wenden Sie Geschäftsregeln auf bestimmte Elemente an.|[Überprüfen von bestimmten Elementen auf Geschäftsregeln &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)|  
|Schließen Sie eine Geschäftsregel aus, damit sie nicht verwendet wird.|[Ausschließen einer Geschäftsregel &#40;Master Data Services&#41;](../master-data-services/exclude-a-business-rule-master-data-services.md)|  
|Löschen Sie eine vorhandene Geschäftsregel.|[Löschen einer Geschäftsregel &#40;Master Data Services&#41;](../master-data-services/delete-a-business-rule-master-data-services.md)|  
  
## <a name="related-content"></a>Verwandte Inhalte  
  
-   [Übersicht über Master Data Services &#40;MDS&#41;](../master-data-services/master-data-services-overview-mds.md)  
  
-   [Versionen &#40;Master Data Services&#41;](../master-data-services/versions-master-data-services.md)  
  
-   [Überprüfung &#40;Master Data Services&#41;](../master-data-services/validation-master-data-services.md)  
  
-   [Änderungsnachverfolgung &#40;Master Data Services&#41;](../master-data-services/change-tracking-master-data-services.md)  
  
  
