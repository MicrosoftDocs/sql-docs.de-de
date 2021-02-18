---
title: Database-Element zur Arbeitsauslastung (DTA)
description: Im dta-Hilfsprogramm gibt das Database-Element für Workload die Datenbank an, in der sich die Ablaufverfolgungstabelle der Arbeitsauslastung befindet.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Database element
ms.assetid: 112fca2a-37e5-4162-b2e7-b56eb8ab0c6f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: c772a91b39917acd3e009009c44123fc31510eca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342017"
---
# <a name="database-element-for-workload-dta"></a>Database-Element zur Arbeitsauslastung (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Gibt die Datenbank an, in der sich die Ablaufverfolgungstabelle der Arbeitsauslastung befindet.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
<Workload>  
  <Database>  
   ...code removed here...  
  </Database>  
```  
  
## <a name="element-characteristics"></a>Elementmerkmale  
  
|Merkmal|BESCHREIBUNG|  
|--------------------|-----------------|  
|**Datentyp und -länge**|Keine.|  
|**Standardwert**|Keine.|  
|**Vorkommen**|Einmalig erforderlich, wenn kein anderer Arbeitsauslastungstyp angegeben ist. Sie müssen ein untergeordnetes **EventString**-, **File**- oder **Database** -Element für das übergeordnete **Workload** -Element angeben. Es kann jedoch nur ein Typ verwendet werden. Wenn Sie beispielsweise eine Arbeitsauslastung mit dem **Database** -Element angeben, können Sie in dieser XML-Eingabedatei keine Arbeitsauslastung mit dem **File** -Element angeben.|  
  
## <a name="element-relationships"></a>Elementbeziehungen  
  
|Beziehung|Elemente|  
|------------------|--------------|  
|**Übergeordnetes Element**|[Workload-Element &#40;DTA&#41;](../../tools/dta/workload-element-dta.md)|  
|**Untergeordnete Elemente**|[Name-Element für Datenbank &#40;DTA&#41;](../../tools/dta/name-element-for-database-dta.md)<br /><br /> [Schema-Element für Datenbank &#40;DTA&#41;](../../tools/dta/schema-element-for-database-dta.md)|  
  
## <a name="remarks"></a>Bemerkungen  
 Dieses Element hat den Namen **DatabaseDetailsTypecomplexType** im XML-Schema des Datenbankoptimierungsratgebers. Dieses **Database** -Element ist nicht mit dem identisch, dessen übergeordnetes Stammelement das **Configuration** -Element ist. (Weitere Informationen finden Sie unter [Database-Element für Konfiguration &#40;DTA&#41;](../../tools/dta/database-element-for-configuration-dta.md).)  
  
## <a name="example"></a>Beispiel  
 Ein Beispiel für die Verwendung dieses **Database** -Elements finden Sie im Codebeispiel unter [Workload-Element &#40;DTA&#41;](../../tools/dta/workload-element-dta.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [XML-Eingabedateireferenz &#40;Datenbankoptimierungsratgeber&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
