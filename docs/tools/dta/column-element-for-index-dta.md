---
title: Column-Element für Index (DTA)
description: Im dta-Hilfsprogramm werden mit dem Column-Element für Index die Spalten angegeben, für die der Index für eine benutzerdefinierte Konfiguration erstellt wird.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Column element
ms.assetid: ba9fac20-26bd-4333-940e-842c15241b46
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/09/2017
ms.openlocfilehash: ea803914ac69a51663b6d489600a7c3719dd5ee0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342077"
---
# <a name="column-element-for-index-dta"></a>Column-Element für Index (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Gibt die Spalten an, für die der Index für eine benutzerspezifische Konfiguration erstellt wird.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
<Create>  
  <Index>  
    <Name>...</Name>  
    <Column [Type | SortOrder]>  
     ...code removed here...  
     </Column>  
```  
  
## <a name="element-attributes"></a>Elementattribute  
  
 **Typ:** Optional. Gibt den Indexspaltentyp an. Mit einem **string** -Datentyp können Sie dieses Attribut mit einem der folgenden zulässigen Werte angeben:  
  
-   **KeyColumn**  
  
     Gibt an, dass durch einen Indexschlüssel auf die Spalte verwiesen wird. Verwenden Sie die folgende Syntax, um dieses Attribut festzulegen:  
  
    ```  
    <Column Type="KeyColumn">  
    ```  
  
     Weitere Informationen zu Schlüsselspalten finden Sie unter [Beschreibung von gruppierten und nicht gruppierten Indizes](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md).  
  
-   **IncludedColumn**  
  
     Gibt an, dass die Spalte eine eingeschlossene Spalte ist (statt einer Schlüsselspalte). Verwenden Sie die folgende Syntax, um dieses Attribut festzulegen:  
  
    ```  
    <Column Type="IncludedColumn">  
    ```  
  
     Weitere Informationen zu eingeschlossenen Spalten finden Sie unter [Erstellen von Indizes mit eingeschlossenen Spalten](../../relational-databases/indexes/create-indexes-with-included-columns.md).  
  
 **SortOrder:** Optional. Gibt die Sortierreihenfolge der Spalte an. Mit einem **string** -Datentyp können Sie die Sortierreihenfolge wie folgt als **Aufsteigend** oder **Absteigend** angeben:  
  
```  
<Column SortOrder="Ascending">  
```  
  
## <a name="element-characteristics"></a>Elementmerkmale  
  
|Merkmal|BESCHREIBUNG|  
|--------------------|-----------------|  
|**Datentyp und -länge**|Keine.|  
|**Standardwert**|Keine.|  
|**Vorkommen**|Angabe von bis zu 1024 Spalten für das **Index** -Element möglich.|  
  
## <a name="element-relationships"></a>Elementbeziehungen  
  
|Beziehung|Elemente|  
|------------------|--------------|  
|**Übergeordnetes Element**|[Index-Element &#40;DTA&#41;](../../tools/dta/index-element-dta.md)|  
|**Untergeordnete Elemente**|[Name-Element für Spalte &#40;DTA&#41;](../../tools/dta/name-element-for-column-dta.md)|  
  
## <a name="example"></a>Beispiel  
 Ein Beispiel für die Verwendung dieses Elements finden Sie unter [Beispiel für eine XML-Eingabedatei mit benutzerdefinierter Konfiguration &#40;DTA&#41;](../../tools/dta/xml-input-file-sample-with-user-specified-configuration-dta.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [XML-Eingabedateireferenz &#40;Datenbankoptimierungsratgeber&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
