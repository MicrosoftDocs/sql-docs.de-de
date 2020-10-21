---
description: Benutzerdefinierte Eigenschaften von ADO.NET
title: Benutzerdefinierte Eigenschaften von ADO.NET | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: e062a9ab-1e6b-4061-845a-4f8a0552b09d
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 557b9c04bf89dfe4d2667689857c48102009c560
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92194882"
---
# <a name="ado-net-custom-properties"></a>Benutzerdefinierte Eigenschaften von ADO.NET

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  **Benutzerdefinierte Eigenschaften von Quellen**  
  
 Die ADO NET-Quelle verfügt sowohl über benutzerdefinierte Eigenschaften als auch über die Eigenschaften, die allen Datenflusskomponenten gemeinsam sind.  
  
 In der folgenden Tabelle werden die benutzerdefinierten Eigenschaften der ADO NET-Quelle beschrieben. Alle Eigenschaften weisen Lese-/Schreibzugriff auf.  
  
|Eigenschaftenname|Datentyp|BESCHREIBUNG|  
|-------------------|---------------|-----------------|  
|CommandTimeout|String|Ein Wert, der die Anzahl der Sekunden angibt, bevor für den SQL-Befehl ein Timeout eintritt. Der Wert 0 gibt an, dass bei dem Befehl nie ein Timeout eintritt.|  
|SqlCommand|String|Die SQL-Anweisung, die die ADO NET-Quelle zum Extrahieren von Daten verwendet.<br /><br /> Während das Paket geladen wird, können Sie diese Eigenschaft dynamisch mit der SQL-Anweisung aktualisieren, die die ADO NET-Quelle verwendet. Weitere Informationen finden Sie unter [Integration Services-Ausdrücke &#40;SSIS&#41;](../../integration-services/expressions/integration-services-ssis-expressions.md) und [Verwenden von Eigenschaftsausdrücken in Paketen](../../integration-services/expressions/use-property-expressions-in-packages.md).|  
|AllowImplicitStringConversion|Boolean|Ein Wert, der angibt, ob Folgendes eintritt:<br /><br /> – Es wird kein Validierungsfehler generiert, wenn eine Unstimmigkeit zwischen externen Metadatentypen und Ausgabespaltentypen in Form von Zeichenfolgen auftritt (DT_WSTR oder DT_NTEXT).<br /><br /> – Implizite Konvertierung externer Metadatentypen in den string-Datentyp, den die Ausgabespalte verwendet.<br /><br /> <br /><br /> Der Standardwert ist TRUE.<br /><br /> Weitere Informationen finden Sie unter [ADO NET Source](../../integration-services/data-flow/ado-net-source.md).|  
  
 Die Ausgabe und die Ausgabespalten der ADO NET-Quelle verfügen nicht über benutzerdefinierte Eigenschaften.  
  
 Weitere Informationen finden Sie unter [ADO NET Source](../../integration-services/data-flow/ado-net-source.md).  
  
 **Benutzerdefinierte Eigenschaften von Zielen**  
  
 Das [!INCLUDE[vstecado](../../includes/vstecado-md.md)] -Ziel verfügt sowohl über benutzerdefinierte Eigenschaften als auch über die Eigenschaften, die allen Datenflusskomponenten gemeinsam sind.  
  
 In der folgenden Tabelle werden die benutzerdefinierten Eigenschaften des [!INCLUDE[vstecado](../../includes/vstecado-md.md)] -Ziels beschrieben. Alle Eigenschaften weisen Lese-/Schreibzugriff auf. Diese Eigenschaften sind nicht im **ADO.NET-Ziel-Editor**verfügbar, können jedoch mit dem **Erweiterten Editor**festgelegt werden.  
  
|Eigenschaft|Datentyp|BESCHREIBUNG|  
|--------------|---------------|-----------------|  
|BatchSize|Integer|Die Anzahl von Zeilen in einem Batch, die an den Server gesendet wurden. Der Wert **0** gibt an, dass die Batchgröße mit der internen Puffergröße übereinstimmt. Der Standardwert dieser Eigenschaft ist **0**.|  
|CommandTimeOut|Integer|Die maximale Ausführungsdauer in Sekunden, bevor ein Timeout für den SQL-Befehl eintritt. Der Wert **0** gibt einen unbegrenzten Zeitraum an. Der Standardwert dieser Eigenschaft ist **0**.|  
|TableOrViewName|String|Name der Zieltabelle oder -sicht.|  
  
 Weitere Informationen finden Sie unter [ADO NET Destination](../../integration-services/data-flow/ado-net-destination.md).  
  
## <a name="see-also"></a>Siehe auch  
 [Common Properties](./set-the-properties-of-a-data-flow-component.md)  
  
