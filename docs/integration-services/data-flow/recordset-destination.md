---
description: Recordsetziel
title: Recordsetziel | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.recordsetdest.f1
helpviewer_keywords:
- Recordset destination
- ADO recordsets [Integration Services]
- destinations [Integration Services], Recordset
- in-memory ADO recordsets [Integration Services]
ms.assetid: be973cf1-c4ff-49f8-987e-314c08ef98e4
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 75027180045921b52377567bd90c0fa10c5fed1a
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88484571"
---
# <a name="recordset-destination"></a>Recordsetziel

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Das Recordsetziel erstellt ein ADO-Recordset im Arbeitsspeicher und füllt es auf. Die Form des Recordsets wird durch die Eingabe im Recordsetziel zur Entwurfszeit definiert.  
  
## <a name="configuration-of-the-recordset-destination"></a>Konfiguration des Recordsetziels  
 Zum Konfigurieren des Recordsetziels geben Sie die Variable an, die das ADO-Recordset speichert.  
  
 Zur Laufzeit wird ein ADO-Recordset in die Variable des Typs „Object“ geschrieben, die in der „VariableName“-Eigenschaft des „Recordset“-Ziels angegeben ist. Die Variable stellt dann das Recordset außerhalb des Datenflusses zur Verfügung, wo es von Skripts und sonstigen Paketelementen verwendet werden kann.  
  
 Diese Quelle hat nur eine Eingabe. Eine Fehlerausgabe wird nicht unterstützt.  
  
 Sie können Eigenschaften mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen.  
  
 Das Dialogfeld **Erweiterter Editor** enthält die Eigenschaften, die programmgesteuert festgelegt werden können. Klicken Sie auf eines der folgenden Themen, um weitere Informationen zu den Eigenschaften zu erhalten, die Sie im Dialogfeld **Erweiterter Editor** oder programmgesteuert festlegen können:  
  
-   [Common Properties](https://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Benutzerdefinierte Eigenschaften des Recordsetziels](../../integration-services/data-flow/recordset-destination-custom-properties.md)  
  
 Informationen zum Festlegen der Eigenschaften finden Sie unter [Festlegen der Eigenschaften einer Datenflusskomponente](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="related-tasks"></a>Related Tasks  
 [Verwenden eines Recordsetziels](../../integration-services/data-flow/use-a-recordset-destination.md)  
  
  
