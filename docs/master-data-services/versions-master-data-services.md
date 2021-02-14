---
title: Versionen
description: Sie können in Master Data Services mehrere Versionen der Master Daten innerhalb eines Modells erstellen. Erfahren Sie mehr über Versionen und wann Sie zu verwenden sind.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- version flags [Master Data Services], about version flags
- versions [Master Data Services]
- version flags [Master Data Services]
- versions [Master Data Services], version flags
ms.assetid: 752ec96d-53d7-4160-8ed2-92e0324645f3
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 54c250aa3b6a6e803f90522218b1eef854af7903
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348847"
---
# <a name="versions-master-data-services"></a>Versionen (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]können Sie mehrere Versionen der Masterdaten innerhalb eines Modells erstellen. Versionen können gesperrt werden, während Sie die Daten überprüfen, und für Versionen kann ein Commit ausgeführt werden, nachdem die Daten überprüft wurden. Versionen mit ausgeführtem Commit bilden einen überwachbaren Datensatz mit Änderungen. Jede erstellte Version umfasst sämtliche Elemente, Attributwerte, Hierarchielemente, Hierarchiebeziehungen und Auflistungen für das Modell.  
  
## <a name="when-to-use-versions"></a>Verwenden von Versionen  
 Sie verwenden Versionen für folgende Zwecke:  
  
-   Verwalten eines überwachungsfähigen Datensatzes der Masterdaten, während sich dieser im Zeitverlauf ändert.  
  
-   Verhindern von Benutzeränderungen, bis alle Daten auf der Grundlage vor Geschäftsregeln erfolgreich überprüft wurden.  
  
-   Sperren eines Modells für die Verwendung durch Abonnementsysteme  
  
-   Testen verschiedener Hierarchien, ohne sie sofort zu implementieren  
  
> [!NOTE]  
>  Wenn Sie die Struktur des Modells ändern, z. B. eine neue Entität oder ein neues domänenbasiertes Attribut erstellen, gilt die Änderung für alle Versionen. Wenn Sie eine frühere Version des Modells anzeigen, wird die Entität oder das Attribut angezeigt, es sind aber keine Daten vorhanden.  
  
## <a name="version-flags"></a>Versionsflags  
 Sobald eine Version für Benutzer oder ein Abonnementsystem freigegeben wird, können Sie ein Identifizierungsflag für die Version festlegen. Sie können dieses Flag bei Bedarf versionsübergreifend verwenden. Anhand von Flags können Benutzer und Abonnementsysteme erkennen, welche Modellversion verwendet werden soll.  
  
## <a name="workflow-for-version-management"></a>Workflow zur Versionsverwaltung  
 Verwenden Sie den folgenden Workflow für die Versionsverwaltung:  
  
1.  Eine Anfangsversion wird automatisch erstellt, wenn Sie ein Modell anlegen und die [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] -Datenbank mit den Masterdaten des Unternehmens auffüllen. Auf der Grundlage von Berechtigungen können Benutzer dann bei Bedarf Änderungen an dieser Version vornehmen.  
  
2.  Wenn Sie ein Commit für eine Modellversion ausführen möchten, sperren Sie die Version, sodass nur Modelladministratoren die Daten aktualisieren können. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md). Wenn Benachrichtigungen konfiguriert sind, wird jedes Mal eine E-Mail-Benachrichtigung an Modelladministratoren gesendet, wenn sich der Status der Version ändert. Weitere Informationen finden Sie unter [Konfigurieren von E-Mail-Benachrichtigungen &#40;Master Data Services&#41;](../master-data-services/configure-email-notifications-master-data-services.md).  
  
3.  Wenden Sie Geschäftsregeln auf die Daten der gesperrten Version an und analysieren Sie alle Überprüfungsprobleme. Falls erforderlich können Sie fehlende Informationen einfügen oder die Transaktion, die das Problem verursacht hat, rückgängig machen. Darüber hinaus können Sie die Version entsperren, damit Benutzer Änderungen vornehmen können.  
  
4.  Nachdem alle Daten erfolgreich überprüft wurden, führen Sie ein Commit der Version aus und kennzeichnen Sie die Version mit einem Flag für die Nutzung durch Abonnementsysteme. Durch ein Commit bestätigte Versionen können nicht geändert werden.  
  
5.  Kopieren Sie die durch ein Commit bestätigte Version, und teilen Sie den Benutzern mit, dass sie die Arbeit mit der neuen Modellversion aufnehmen können.  
  
## <a name="sequential-or-simultaneous-versions"></a>Sequenzielle oder gleichzeitige Versionen  
 Sie können sequenzielle oder gleichzeitige Versionen des Modells erstellen.  
  
-   **Sequenzielle Versionen.** Jedes Mal, wenn Sie ein Commit für eine Version ausführen, erstellen Sie eine neue Kopie und weisen der Version die nächste laufende Nummer zu. Beispielsweise können Sie **Version 7** des Modells kopieren und die Kopie **Version 8** nennen.  
  
-   **Gleichzeitige Versionen.** Erstellen Sie gleichzeitige Versionen des Modells, wenn Sie mit mindestens zwei Versionen der Daten gleichzeitig arbeiten möchten. Dies ist hilfreich bei Umstrukturierungen oder Fusionen des Unternehmens, die mit dem normalen Geschäftsgang zusammenfallen, und Sie feststellen möchten, wie sich die neuen Masterdaten in vorhandene Strukturen einfügen.  
  
    > [!NOTE]  
    >  Eine Einstellung in [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] bestimmt, ob alle oder nur die durch ein Commit bestätigten Versionen kopiert werden können. Um gleichzeitige Versionen zu erstellen, müssen Sie [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] konfigurieren, um es Ihnen zu ermöglichen, alle Versionen zu kopieren. Diese Einstellung ist auch in der Tabelle Systemeinstellungen verfügbar. Weitere Informationen finden Sie unter [Systemeinstellungen &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Taskbeschreibung|Thema|  
|----------------------|-----------|  
|Ändern Sie den Namen einer vorhandenen Version.|[Ändern eines Versionsnamens &#40;Master Data Services&#41;](../master-data-services/change-a-version-name-master-data-services.md)|  
|Sperren Sie eine Version, sodass nur Administratoren die Daten bearbeiten können.|[Sperren einer Version &#40;Master Data Services&#41;](../master-data-services/lock-a-version-master-data-services.md)|  
|Entsperren Sie eine Version, damit Benutzer die Daten bearbeiten können.|[Entsperren einer Version &#40;Master Data Services&#41;](../master-data-services/unlock-a-version-master-data-services.md)|  
|Führen Sie für eine Version einen Commit aus, nachdem alle Daten überprüft wurden.|[Durchführen eines Commits für eine Version &#40;Master Data Services&#41;](../master-data-services/commit-a-version-master-data-services.md)|  
|Erstellen Sie ein neues Flag, um eine Version zu markieren.|[Erstellen eines Versionsflags &#40;Master Data Services&#41;](../master-data-services/create-a-version-flag-master-data-services.md)|  
|Ändern Sie den Namen eines vorhandenen Versionsflags.|[Ändern des Namens eines Versionsflags &#40;Master Data Services&#41;](../master-data-services/change-a-version-flag-name-master-data-services.md)|  
|Weisen Sie einer Version ein vorhandenes Flag zu.|[Zuweisen eines Flags zu einer Version &#40;Master Data Services&#41;](../master-data-services/assign-a-flag-to-a-version-master-data-services.md)|  
|Erstellen Sie eine neue Kopie einer vorhandenen Version.|[Kopieren einer Version &#40;Master Data Services&#41;](../master-data-services/copy-a-version-master-data-services.md)|  
|Löschen Sie eine vorhandene Version.|[Löschen einer Version &#40;Master Data Services&#41;](../master-data-services/delete-a-version-master-data-services.md)|  
|Löschen Sie vorläufig gelöschte Elemente aus einer Version.|[Endgültiges Löschen von Versionselementen &#40;Master Data Services&#41;](../master-data-services/purge-version-members-master-data-services.md)|  
  
## <a name="related-content"></a>Verwandte Inhalte  
  
-   [Umkehren einer Transaktion &#40;Master Data Services&#41;](../master-data-services/reverse-a-transaction-master-data-services.md)  
  
-   [Benachrichtigungen &#40;Master Data Services&#41;](../master-data-services/notifications-master-data-services.md)  
  
-   [Geschäftsregeln &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)  
  
  
