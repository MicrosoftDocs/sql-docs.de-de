---
description: Benutzerdefinierte Eigenschaften des DataReader-Ziels
title: Benutzerdefinierte Eigenschaften des DataReader-Ziels | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: f151c3e8-3811-457d-a3d3-6158ca65a646
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0cf82f80425dcc00cb3129d051ca233af55ad783
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92194846"
---
# <a name="datareader-destination-custom-properties"></a>Benutzerdefinierte Eigenschaften des DataReader-Ziels

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Das DataReader-Ziel verfügt sowohl über benutzerdefinierte Eigenschaften als auch über die Eigenschaften, die allen Datenflusskomponenten gemeinsam sind.  
  
 Die folgende Tabelle beschreibt die benutzerdefinierten Eigenschaften des DataReader-Ziels. Alle Eigenschaften außer **DataReader** weisen Lese-/Schreibzugriff auf.  
  
|Eigenschaftenname|Datentyp|BESCHREIBUNG|  
|-------------------|---------------|-----------------|  
|DataReader|String|Der Klassenname des DataReader-Ziels.|  
|FailOnTimeout|Boolean|Gibt an, ob ein Fehler ausgegeben wird, wenn ein **ReadTimeout** auftritt. Der Standardwert dieser Eigenschaft ist **False**.|  
|ReadTimeout|Integer|Die Anzahl von Millisekunden, bevor ein Timeout auftritt. Der Standardwert dieser Eigenschaft beträgt 30000 (30 Sekunden).|  
  
 Die Eingabe und die Eingabespalten des DataReader-Ziels verfügen nicht über benutzerdefinierte Eigenschaften.  
  
 Weitere Informationen finden Sie unter [DataReader Destination](../../integration-services/data-flow/datareader-destination.md).  
  
## <a name="see-also"></a>Siehe auch  
 [Common Properties](./set-the-properties-of-a-data-flow-component.md)  
  
