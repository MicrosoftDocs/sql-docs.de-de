---
description: Überprüfen von Daten (MDS-Add-In für Excel)
title: Überprüfen von Daten
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 71eda98f-01a4-4fff-8246-be3133782523
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 27e6cc619206c108e17aa62c907cdea083526b48
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352965"
---
# <a name="validating-data-mds-add-in-for-excel"></a>Überprüfen von Daten (MDS-Add-In für Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Beim Veröffentlichen von Daten werden in [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]zwei Arten von Überprüfungen ausgeführt:  
  
-   Alle definierten Geschäftsregeln werden auf die Daten angewendet.  
  
-   Daten werden auf zulässige Attributwerte hin überprüft (z. B. die Anzahl der Zeichen oder der Typ der Daten).  
  
 Gültige Daten werden in jedem Fall im MDS-Repository veröffentlicht. Ungültige Daten werden hervorgehoben, und Details des Fehlers können in Statusspalten angezeigt werden.  
  
## <a name="when-validation-occurs"></a>Zeitpunkt der Überprüfungen  
 Beim Veröffentlichen von Daten werden in [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]wird die Überprüfung ausgeführt, wenn Sie neue oder geänderte Daten veröffentlichen oder wenn Sie Geschäftsregeln manuell anwenden.  
  
 Wenn für die Geschäftsregeln ein Fehler auftritt, werden die Daten trotzdem im MDS-Repository veröffentlicht. Wenn die Überprüfung der Eingabe fehlschlägt, werden die Daten nicht im Repository veröffentlicht.  
  
## <a name="validation-statuses"></a>Überprüfungsstatus  
 Beim Veröffentlichen von Daten werden in [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]sind die folgenden Überprüfungsstatus möglich.  
  
 Weitere Informationen zu zusätzlichen Status finden Sie unter [Überprüfungsstatus &#40;Master Data Services&#41;](../../master-data-services/validation-statuses-master-data-services.md).  
  
|Status|BESCHREIBUNG|  
|------------|-----------------|  
|Fehler bei der Überprüfung|Für einen oder mehrere Werte in der Zeile ist bei der Überprüfung auf Grundlage der Geschäftsregeln, die von einem MDS-Administrator definiert wurden, ein Fehler aufgetreten.|  
|Die Überprüfung war erfolgreich|Alle Werte in der Zeile haben die Überprüfung auf Grundlage der Geschäftsregeln bestanden.|  
  
## <a name="input-statuses"></a>Eingabestatus  
 Beim Veröffentlichen von Daten werden in [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)]sind die folgenden Eingabestatus möglich.  
  
|Status|BESCHREIBUNG|  
|------------|-----------------|  
|Fehler|Ein oder mehrere Werte in der Zeile erfüllen die Systemanforderungen nicht, z. B. Länge oder Datentyp. Der Wert wird im MDS-Repository nicht aktualisiert.|  
|Neue Zeile|Die Werte in der Zeile wurden noch nicht im MDS-Repository veröffentlicht.|  
|Nur Leseberechtigung|Der angemeldete Benutzer verfügt nur über die Leseberechtigung für einen oder mehrere Werte in der Zeile, und der Wert bzw. die Werte können nicht aktualisiert werden.|  
|Unverändert|Kein Wert in der Zeile wurde im Arbeitsblatt geändert. Dies bedeutet nicht, dass sich die Werte im Repository nicht geändert haben. Klicken Sie zum Abrufen der aktuellen Daten im Blatt in der Gruppe **Verbinden und Laden** auf **Laden oder Aktualisieren**.<br /><br /> Dies ist die Standardeinstellung für jede Zeile.|  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Taskbeschreibung|Thema|  
|----------------------|-----------|  
|Bestimmen Sie, welche Werte die definierten Geschäftsregeln nicht bestehen.|[Anwenden von Geschäftsregeln &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/apply-business-rules-mds-add-in-for-excel.md)|  
|Zeigen Sie als Unterstützung beim Korrigieren von Überprüfungsfehlern alle Transaktionen an, die für ein Element ausgeführt wurden.|[Anzeigen aller Anmerkungen oder Transaktionen für ein Element &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/view-all-annotations-or-transactions-for-a-member-mds-add-in-for-excel.md)|  
  
## <a name="related-content"></a>Verwandte Inhalte  
  
-   [Übersicht: Importieren von Daten aus Excel &#40;MDS-Add-In für Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)  
  
  
