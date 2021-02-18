---
title: Beispiel für eine XML-Eingabedatei mit benutzerdefinierter Konfiguration
description: Dieser Artikel enthält ein Beispiel für eine XML-Eingabedatei mit benutzerdefinierter Konfiguration, die zum Optimieren von Arbeitsauslastungen mit dem Datenbankoptimierungsratgeber verwendet wird.
titleSuffix: DTA
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: b29c9716-e5c3-4003-9efb-3ade2197b630
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: fc4e0f7e4816a6aec1cdbe41d2d4c84c8af55711
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340000"
---
# <a name="xml-input-file-sample-with-user-specified-configuration-dta"></a>Beispiel für eine XML-Eingabedatei mit benutzerdefinierter Konfiguration (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Kopieren Sie dieses Beispiel für eine XML-Eingabedatei, die eine benutzerdefinierte Konfiguration mit dem **Configuration** -Element angibt, und fügen Sie sie in Ihren XML-Editor oder Text-Editor ein. Dadurch können Sie Was-wäre-wenn-Analysen ausführen. Bei Was-wäre-wenn-Analysen wird mit dem **Configuration** -Element eine Gruppe von hypothetischen physischen Entwurfsstrukturen für die Datenbank angegeben, die Sie optimieren möchten. Anschließend analysieren Sie mit dem Datenbankoptimierungsratgeber, welche Auswirkungen das Ausführen einer Arbeitsauslastung für diese hypothetische Konfiguration hat, um herauszufinden, ob dadurch die Abfrageverarbeitungsleistung verbessert werden kann. Diese Art einer Analyse hat den Vorteil, dass die neue Konfiguration ausgewertet werden kann, ohne dass eine Implementierung erforderlich ist. Wenn die hypothetische Konfiguration nicht die gewünschten Leistungssteigerungen erzielt, lässt sie sich auf einfache Weise ändern und erneut analysieren, bis Sie die Konfiguration haben, die die gewünschten Ergebnisse erzielt.  
  
 Nach dem Kopieren dieses Beispiels in Ihr Bearbeitungstool ersetzen Sie die Werte für das **Server**-, **Database**-, **Schema**-, **Table**-, **Workload**-, **TuningOptions**- und **Configuration** -Element durch die Werte für Ihre Optimierungssitzung. Weitere Informationen zu sämtlichen Attributen und untergeordneten Elementen, die Sie zusammen mit diesen Elementen verwenden können, finden Sie unter [XML-Eingabedateireferenz &#40;Datenbankoptimierungsratgeber&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md). Im folgenden Beispiel wird nur eine Teilmenge der verfügbaren Optionen für Attribute und untergeordnete Elemente verwendet.  
  
## <a name="code"></a>Code  
  
```  
<?xml version="1.0" encoding="utf-16" ?>  
<DTAXML xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="https://schemas.microsoft.com/sqlserver/2004/07/dta">  
  <DTAInput>  
    <Server>  
      <Name>MyServerName</Name>  
<!-- To tune multiple databases, list them and their associated tables in the following section. -->  
      <Database>  
        <Name>MyDatabaseName</Name>  
        <Schema>  
          <Name>MyDatabaseSchemaName</Name>  
<!-- You can list as many tables as necessary in the following section. -->  
          <Table>  
            <Name>MyTableName1</Name>  
          </Table>  
          <Table>  
            <Name>MyTableName2</Name>  
          </Table>  
        </Schema>  
      </Database>  
    </Server>  
    <Workload>  
<!-- The following element specifies a workload file, which can be a trace file or a Transact-SQL script file. -->  
      <File>c:\PathToYourWorkloadFile</File>  
    </Workload>  
    <TuningOptions>  
      <TuningTimeInMin>180</TuningTimeInMin>  
      <StorageBoundInMB>10000</StorageBoundInMB>  
      <FeatureSet>IDX_IV</FeatureSet>  
      <Partitioning>NONE</Partitioning>  
      <KeepExisting>NONE</KeepExisting>  
      <OnlineIndexOperation>OFF</OnlineIndexOperation>  
    </TuningOptions>  
    <Configuration SpecificationMode="Absolute">  
      <Server>  
        <Name>MyServerName</Name>  
          <Database>  
            <Name>MyDatabaseName</Name>  
            <Schema>  
              <Name>MyDatabaseSchemaName</Name>  
                <Table>  
                  <Name>MyTableName1</Name>  
                  <Recommendation>  
                    <Create>  
                      <Index Clustered="true" Unique="false" Online="false" IndexSizeInMB="873.75">  
                        <Name>MyIndexName</Name>  
                        <Column Type="KeyColumn" SortOrder="Ascending">  
                          <Name>MyColumnName1</Name>  
                        </Column>  
                        <Filegroup>MyFileGroupName1</FileGroup>  
                      </Index>  
                    </Create>  
                  </Recommendation>  
                </Table>  
            </Schema>  
          </Database>  
      </Server>  
    </Configuration>  
  </DTAInput>  
</DTAXML>  
```  
  
## <a name="comments"></a>Kommentare  
  
-   Das Löschen von physischen Entwurfsstrukturen wird nicht unterstützt, wenn Sie den **Absolute** -Modus für das **Configuration** -Element angeben (`Configuration SpecificationMode="Absolute"`).  
  
-   Sie können die gleiche physische Entwurfsstruktur nicht in beiden Modi (**Relative** oder **Absolute**) des **Configuration** -Elements erstellen und löschen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Starten und Verwenden des Datenbankoptimierungsratgebers](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)   
 [Anzeigen und Verwenden der Ausgabe des Datenbankoptimierungsratgebers](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)   
 [XML-Eingabedateireferenz &#40;Datenbankoptimierungsratgeber&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
