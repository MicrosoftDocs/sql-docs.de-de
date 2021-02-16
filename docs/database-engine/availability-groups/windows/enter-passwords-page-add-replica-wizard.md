---
title: 'Assistent zum Hinzufügen von Replikaten: Seite zum Eingeben von Kennwörtern für Verfügbarkeitsgruppen'
description: Eine Beschreibung der Einstellungen auf der „Seite zum Eingeben der Kennwörter“ im „Assistenten zum Hinzufügen von Replikaten“ in SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.addreplicawizard.enterpasswords.f1
ms.assetid: e69207a0-c5c4-44e4-ae9a-4afbb67251d1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8bef426da309598674848d42dfee473439cfb318
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348479"
---
# <a name="enter-passwords-page-add-replica-wizard-for-always-on-availability-groups"></a>Seite zum Eingeben der Kennwörter (Assistent zum Hinzufügen von Replikaten) für Always On-Verfügbarkeitsgruppen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Hilfethema werden die Optionen der Seite **Kennwörter eingeben** beschrieben. Dieses Thema gilt für den [!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)] von [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
 Wenn die Replikate, die Sie auf der Seite **Replikate angeben** ausgewählt haben, Datenbanken enthalten, die über einen Datenbank-Hauptschlüssel verfügen, wird die Seite zum Eingeben der Kennwörter angezeigt.  
  
## <a name="enter-passwords-options"></a>Optionen der Kennworteingabe  
 Im Raster **Benutzerdatenbanken auf dieser SQL Server-Instanz** wird jede lokale Benutzerdatenbank aufgelistet. Es gibt folgende Spalten:  
  
 **Name**  
 Zeigt den Namen einer lokalen Benutzerdatenbank an.  
  
 **Größe**  
 Zeigt die Datenbankgröße an, wenn die Größe für den Assistenten verfügbar ist.  
  
 **Status**  
 Gibt an, dass für Datenbanken, die über einen Datenbank-Hauptschlüssel verfügen, ein **Kennwort erforderlich** ist. Klicken Sie nach dem Eingeben der Kennwörter für die Datenbank-Hauptschlüssel in der **Kennwörter** -Spalte auf **Aktualisieren**. Wenn Sie die Kennwörter richtig eingegeben haben, wird in der **Status** -Spalte **Kennwort eingegeben** angezeigt.  
  
 Die Spalte **Status** gibt **Kein Kennwort erforderlich** für Datenbanken an, die über keinen Datenbank-Hauptschlüssel verfügen.  
  
 **Kennwort**  
 Wenn die Spalte **Status** **Kennwort erforderlich** angibt, geben Sie das Kennwort für den Datenbank-Hauptschlüssel ein.  
  
 **Aktualisieren**  
 Klicken Sie, um das Raster zu aktualisieren. Dies ist nützlich, nachdem Sie die erforderlichen Kennwörter eingegeben haben.  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [Verwenden des Assistenten zum Hinzufügen von Replikaten zu Verfügbarkeitsgruppen &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
  
