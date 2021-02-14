---
description: Projekteinstellungen (Typzuordnung) (accesstosql)
title: Projekteinstellungen (Typzuordnung) (accesstosql) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access data types
- data types, default mappings
- default data type mappings
- Project Settings dialog box, Type Mapping
- SQL Server data types
- Type Mapping settings
ms.assetid: b87b9683-abed-4677-8c50-18bdba704655
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2fc9bbd8092c98d21263d3972ab04d90888fcbae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066406"
---
# <a name="project-settings-type-mapping-accesstosql"></a>Projekteinstellungen (Typzuordnung) (accesstosql)
Mit den Projekteinstellungen für die Typzuordnung können Sie Standardtypmappings für das SSMA-Projekt festlegen. Sie können auch Typzuordnungen für einzelne Datenbankobjekte angeben. Weitere Informationen finden Sie unter [Zuordnung von Quell-und Ziel Datentypen](mapping-source-and-target-data-types-accesstosql.md).  
  
Die Typzuordnung ist in den Dialogfeldern **Projekteinstellungen** und **Standard Projekteinstellungen** verfügbar:  
  
-   Verwenden Sie das Dialogfeld **Projekteinstellungen** , um Konfigurationsoptionen für das aktuelle Projekt festzulegen. Um auf die typzuordnungseinstellungen zuzugreifen, wählen Sie **im Menü Extras** die Option **Projekteinstellungen** aus, und klicken Sie dann im linken Bereich auf **Typzuordnung** .  
  
-   Verwenden Sie das Dialogfeld **Standard Projekteinstellungen** , um Konfigurationsoptionen für alle Projekte festzulegen. Um auf die typzuordnungseinstellungen zuzugreifen, wählen Sie **im Menü Extras** die Option **Standard Projekteinstellungen** aus, wählen Sie den Migrations Projekttyp, für den die Einstellungen angezeigt werden müssen/Changed in der Dropdown Liste **Migrations Ziel Version** aus, und klicken Sie im linken Bereich auf **Typzuordnung** .  
  
## <a name="options"></a>Tastatur  
**Quellentyp**  
Der zuzuordnende Zugriffs Datentyp.  
  
**Zieltyp**  
Der Ziel- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure Datentyp für den angegebenen Zugriffs Datentyp.  
  
Die folgende Tabelle zeigt die Standard Zuordnung zwischen Quell-und Ziel Datentypen.  
  
|Access-Datentyp|SQL Server-Datentyp|  
|--------------------|------------------------|  
|**Binär [ \* .. \* ]**|**varbinary [ \* ]**|  
|**boolean**|**bit**|  
|**byte**|**tinyint**|  
|**Währungs**|**money**|  
|**date**|**datetime**|  
|**decimal**|**float**|  
|**double**|**float**|  
|**guid**|**uniqueidentifier**|  
|**integer**|**smallint**|  
|**long**|**int**|  
|**LONGBINARY**|**varbinary(max)**|  
|**Anruf**|**nvarchar(max)**|  
|**Memo** für Zugriff 97|**varchar(max)**|  
|**single**|**real**|  
|**Text [ \* .. \* ]**|**nvarchar [ \* ]**|  
|**Text [ \* .. \* ]** : für Access 97|**varchar [ \* ]**|  
  
**Add (Hinzufügen)**  
Klicken Sie hier, um der Zuordnungsliste einen Datentyp hinzuzufügen.  
  
**Bearbeiten**  
Klicken Sie hierauf, um einen Datentyp in der Liste Zuordnung zu bearbeiten.  
  
**Remove**  
Klicken Sie hierauf, um die ausgewählte Datentyp Zuordnung aus der Liste Zuordnung zu entfernen.  
  
**Standard wiederherstellen**  
Klicken Sie hier, um alle Datentyp Zuordnungen auf die SSMA-Standardwerte zurückzusetzen.  
  
## <a name="see-also"></a>Weitere Informationen  
[Zuordnen von Quell- und Zieldatentypen](mapping-source-and-target-data-types-accesstosql.md)  
[Referenz zur Benutzeroberfläche (Zugriff)](./user-interface-reference-accesstosql.md)  
