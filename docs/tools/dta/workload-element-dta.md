---
title: Workload-Element (DTA)
description: Im dta-Hilfsprogramm gibt das Workload-Element die Arbeitsauslastung an, die für eine Optimierungssitzung verwendet werden soll. In diesem Artikel wird dieses Element beschrieben.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Workload element
ms.assetid: 68ffd473-6546-4015-98d0-3763165de65c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 60e673cf32849203c921afa41fa4af6df8f2023e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349479"
---
# <a name="workload-element-dta"></a>Workload-Element (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Gibt die für eine Optimierungssitzung zu verwendende Arbeitsauslastung an.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
<DTAInput>  
    <Server>  
...code removed...  
    <Workload>...</Workload>  
```  
  
## <a name="element-characteristics"></a>Elementmerkmale  
  
|Merkmal|BESCHREIBUNG|  
|--------------------|-----------------|  
|**Datentyp und -länge**|Keine.|  
|**Standardwert**|Keine.|  
|**Vorkommen**|Für jedes **DTAInput** -Element einmal erforderlich.|  
  
## <a name="element-relationships"></a>Elementbeziehungen  
  
|Beziehung|Elemente|  
|------------------|--------------|  
|**Übergeordnetes Element**|[Starten und Verwenden des Datenbankoptimierungsratgebers](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)|  
|**Untergeordnete Elemente**|[File-Element &#40;DTA&#41;](../../tools/dta/file-element-dta.md)<br /><br /> [Database-Element zur Arbeitsauslastung &#40;DTA&#41;](../../tools/dta/database-element-for-workload-dta.md)<br /><br /> [EventString-Element &#40;DTA&#41;](../../tools/dta/eventstring-element-dta.md)|  
  
## <a name="remarks"></a>Bemerkungen  
 Die Arbeitsauslastung besteht aus einer Reihe von [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen, die für eine oder mehrere Datenbanken ausgeführt werden, die Sie optimieren möchten. Der Datenbankoptimierungsratgeber kann [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skripts, Ablaufverfolgungsdateien und Ablaufverfolgungstabellen als Arbeitsauslastung verwenden.  
  
 Wenn Sie sowohl in einer XML-Eingabedatei als auch mit dem Tool **dta** in der Befehlszeile eine Arbeitsauslastung angeben, wird die in der Befehlszeile angegebene Arbeitsauslastung für die Optimierung verwendet. Alle Optimierungsoptionen, die in der Befehlszeile angegeben sind, überschreiben die in XML-Eingabedateien angegebenen Optionen. Die einzige Ausnahme hiervon sind benutzerdefinierte Konfigurationen, die im Auswertungsmodus in der XML-Eingabedatei eingegeben werden. Wenn z. B. eine Konfiguration in das **Configuration** -Element der XML-Eingabedatei eingegeben wird und auch das **EvaluateConfiguration** -Element als eine der Optimierungsoptionen angegeben ist, überschreiben die in der XML-Eingabedatei angegebenen Optimierungsoptionen die in der Befehlszeile eingegebenen Optimierungsoptionen.  
  
 Die Angabe einer Arbeitsauslastung ist für jede Optimierungssitzung erforderlich.  
  
## <a name="example"></a>Beispiel  
 Das folgende Codebeispiel gibt die **MyDatabase.MyDBOwner.TuningTable001** -Ablaufverfolgungstabelle für das **Workload** -Element an. Die **TuningTable001** -Tabelle wurde mithilfe der Optimierungsvorlage von SQL Server Profiler und Speichern der Ablaufverfolgungsausgabe als Tabelle erstellt.  
  
```  
<DTAXML ...>  
  <DTAInput>  
    <Server>  
...code removed here...  
    </Server>  
    <Workload>  
      <Database>  
        <Name>MyDatabase</Name>  
        <Schema>  
          <Name>MyDBOwner</Name>  
            <Table>  
              <Name>TuningTable001</Name>  
            </Table>  
        </Schema>  
      </Database>  
    </Workload>  
...code removed here...  
  </DTAInput>  
</DTAXML>  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [XML-Eingabedateireferenz &#40;Datenbankoptimierungsratgeber&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
