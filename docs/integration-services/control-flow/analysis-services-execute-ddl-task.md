---
description: DDL ausführen (Analysis Services-Task)
title: DDL ausführen (Analysis Services-Task) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.asexecuteddltask.f1
- sql13.dts.designer.asexecuteddltask.general.f1
- sql13.dts.designer.asexecuteddltask.ddl.f1
helpviewer_keywords:
- Analysis Services Execute DDL task
- DDL
ms.assetid: 7f25c8c6-b601-41f2-9553-be0a2ee0751a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6a2b82bede4fd6c04bd1d72f24b9cdb2a0e6fe34
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91727587"
---
# <a name="analysis-services-execute-ddl-task"></a>DDL ausführen (Analysis Services-Task)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Der [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Task DDL ausführen führt DLL-Anweisungen (Data Definition Language, Datendefinitionssprache) aus, mit denen Miningmodelle und mehrdimensionale Objekte, wie z. B. Cubes und Dimensionen, erstellt, gelöscht oder geändert werden können. Beispielsweise kann mit einer DDL-Anweisung eine Partition im **Adventure Works** -Cube erstellt oder eine Dimension in [!INCLUDE[ssAWDWsp](../../includes/ssawdwsp-md.md)], der im Lieferumfang von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] enthaltenen Beispieldatenbank von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], gelöscht werden.  
  
 Der [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Task DDL ausführen stellt mithilfe eines Verbindungs-Managers von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] eine Verbindung mit einer Instanz von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] oder mit einem Projekt von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] her. Weitere Informationen finden Sie unter [Analysis Services Connection Manager](../../integration-services/connection-manager/analysis-services-connection-manager.md).  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] schließt eine Reihe von Tasks ein, die Business Intelligence-Vorgänge ausführen, wie z. B. das Verarbeiten analytischer Objekte und das Ausführen von Data Mining-Vorhersageabfragen.  
  
 Klicken Sie auf eines der folgenden Themen, um weitere Informationen zu verwandten Business Intelligence-Tasks zu erhalten:  
  
-   [Analysis Services-Verarbeitungstask](../../integration-services/control-flow/analysis-services-processing-task.md)  
  
-   [Data Mining-Abfragetask](../../integration-services/control-flow/data-mining-query-task.md)  
  
## <a name="ddl-statements"></a>DDL-Anweisungen  
 Die DDL-Anweisungen werden als [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Scripting Language (ASSL) dargestellt und in einen XMLA-Befehl (XML for Analysis) eingebunden.  
  
-   Mit ASSL werden eine Instanz von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] und die darin enthaltenen Datenbanken und Datenbankobjekte definiert und beschrieben. Weitere Informationen finden Sie unter [Analysis Services Scripting Language &#40;ASSL für XMLA&#41;](/analysis-services/assl/analysis-services-scripting-language-assl-for-xmla).  
  
-   Bei XMLA handelt es sich um eine Befehlssprache, mit der Aktionsbefehle, wie z. B. Create, Alter oder Process, an eine Instanz von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]gesendet werden. Weitere Informationen finden Sie in der [XML for Analysis-Referenz &#40;XMLA&#41;](/analysis-services/xmla/xml-for-analysis-xmla-reference).  
  
 Wenn der DDL-Code in einer separaten Datei gespeichert ist, gibt der [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Task DDL ausführen mithilfe eines Dateiverbindungs-Manager den Dateipfad an. Weitere Informationen finden Sie unter [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md).  
  
 DDL-Anweisungen können Kennwörter und sonstige vertrauliche Informationen enthalten. Deshalb sollte für ein Paket, das [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Tasks „DDL ausführen“ enthält, die Paketschutzebene **EncryptAllWithUserKey** oder **EncryptAllWithPassword**verwendet werden. Weitere Informationen finden Sie unter [Integration Services-Pakete &#40;SSIS&#41;](../../integration-services/integration-services-ssis-packages.md).  
  
### <a name="ddl-examples"></a>DDL-Beispiele  
 Die folgenden drei DDL-Anweisungen wurden von Skripterstellungsobjekten in [!INCLUDE[ssAWDWsp](../../includes/ssawdwsp-md.md)], der im Lieferumfang von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] enthaltenen Beispieldatenbank von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], generiert.  
  
 Mit der folgenden DDL-Anweisung wird die **Höherstufungs** -Dimension gelöscht.  
  
```  
<Delete xmlns="https://schemas.microsoft.com/analysisservices/2003/engine">  
    <Object>  
        <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
        <DimensionID>Dim Promotion</DimensionID>  
    </Object>  
</Delete>  
  
```  
  
 Mit der folgenden DDL-Anweisung wird der [!INCLUDE[ssAWDWsp](../../includes/ssawdwsp-md.md)] -Cube verarbeitet.  
  
```  
<Batch xmlns="https://schemas.microsoft.com/analysisservices/2003/engine">  
  <Parallel>  
    <Process xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
      <Object>  
        <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
      </Object>  
      <Type>ProcessFull</Type>  
      <WriteBackTableCreation>UseExisting</WriteBackTableCreation>  
    </Process>  
  </Parallel>  
</Batch>  
  
```  
  
 Mit der folgenden DDL-Anweisung wird das **Forecasting** -Miningmodell erstellt.  
  
```  
<Create xmlns="https://schemas.microsoft.com/analysisservices/2003/engine">  
    <ParentObject>  
        <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
        <MiningStructureID>Forecasting</MiningStructureID>  
    </ParentObject>  
    <ObjectDefinition>  
        <MiningModel xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
            <ID>Forecasting</ID>  
            <Name>Forecasting</Name>  
            <Algorithm>Microsoft_Time_Series</Algorithm>  
            <AlgorithmParameters>  
                <AlgorithmParameter>  
                    <Name>PERIODICITY_HINT</Name>  
                    <Value xsi:type="xsd:string">{12}</Value>  
                </AlgorithmParameter>  
            </AlgorithmParameters>  
            <Columns>  
                <Column>  
                    <ID>Amount</ID>  
                    <Name>Amount</Name>  
                    <SourceColumnID>Amount</SourceColumnID>  
                    <Usage>Predict</Usage>  
                </Column>  
                <Column>  
                    <ID>Model Region</ID>  
                    <Name>Model Region</Name>  
                    <SourceColumnID>Model Region</SourceColumnID>  
                    <Usage>Key</Usage>  
                </Column>  
                <Column>  
                    <ID>Quantity</ID>  
                    <Name>Quantity</Name>  
                    <SourceColumnID>Quantity</SourceColumnID>  
                    <Usage>Predict</Usage>  
                </Column>  
                <Column>  
                    <ID>Time Index</ID>  
                    <Name>Time Index</Name>  
                    <SourceColumnID>Time Index</SourceColumnID>  
                    <Usage>Key</Usage>  
                </Column>  
            </Columns>  
            <Collation>Latin1_General_CS_AS_KS</Collation>  
        </MiningModel>  
    </ObjectDefinition>  
</Create>  
  
```  
  
 Die folgenden drei DDL-Anweisungen wurden von Skripterstellungsobjekten in [!INCLUDE[ssAWDWsp](../../includes/ssawdwsp-md.md)], der im Lieferumfang von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] enthaltenen Beispieldatenbank von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], generiert.  
  
 Mit der folgenden DDL-Anweisung wird die **Höherstufungs** -Dimension gelöscht.  
  
```  
<Delete xmlns="https://schemas.microsoft.com/analysisservices/2003/engine">  
    <Object>  
        <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
        <DimensionID>Dim Promotion</DimensionID>  
    </Object>  
</Delete>  
  
```  
  
 Mit der folgenden DDL-Anweisung wird der [!INCLUDE[ssAWDWsp](../../includes/ssawdwsp-md.md)] -Cube verarbeitet.  
  
```  
<Batch xmlns="https://schemas.microsoft.com/analysisservices/2003/engine">  
  <Parallel>  
    <Process xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
      <Object>  
        <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
      </Object>  
      <Type>ProcessFull</Type>  
      <WriteBackTableCreation>UseExisting</WriteBackTableCreation>  
    </Process>  
  </Parallel>  
</Batch>  
  
```  
  
 Mit der folgenden DDL-Anweisung wird das **Forecasting** -Miningmodell erstellt.  
  
```  
<Create xmlns="https://schemas.microsoft.com/analysisservices/2003/engine">  
    <ParentObject>  
        <DatabaseID>Adventure Works DW Multidimensional 2012</DatabaseID>  
        <MiningStructureID>Forecasting</MiningStructureID>  
    </ParentObject>  
    <ObjectDefinition>  
        <MiningModel xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
            <ID>Forecasting</ID>  
            <Name>Forecasting</Name>  
            <Algorithm>Microsoft_Time_Series</Algorithm>  
            <AlgorithmParameters>  
                <AlgorithmParameter>  
                    <Name>PERIODICITY_HINT</Name>  
                    <Value xsi:type="xsd:string">{12}</Value>  
                </AlgorithmParameter>  
            </AlgorithmParameters>  
            <Columns>  
                <Column>  
                    <ID>Amount</ID>  
                    <Name>Amount</Name>  
                    <SourceColumnID>Amount</SourceColumnID>  
                    <Usage>Predict</Usage>  
                </Column>  
                <Column>  
                    <ID>Model Region</ID>  
                    <Name>Model Region</Name>  
                    <SourceColumnID>Model Region</SourceColumnID>  
                    <Usage>Key</Usage>  
                </Column>  
                <Column>  
                    <ID>Quantity</ID>  
                    <Name>Quantity</Name>  
                    <SourceColumnID>Quantity</SourceColumnID>  
                    <Usage>Predict</Usage>  
                </Column>  
                <Column>  
                    <ID>Time Index</ID>  
                    <Name>Time Index</Name>  
                    <SourceColumnID>Time Index</SourceColumnID>  
                    <Usage>Key</Usage>  
                </Column>  
            </Columns>  
            <Collation>Latin1_General_CS_AS_KS</Collation>  
        </MiningModel>  
    </ObjectDefinition>  
</Create>  
  
```  
  
## <a name="configuration-of-the-analysis-services-execute-ddl-task"></a>Konfiguration des Analysis Services-Tasks "DDL ausführen"  
 Sie können Eigenschaften mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen.  
  
 Klicken Sie auf das folgende Thema, um weitere Informationen zu den Eigenschaften zu erhalten, die Sie im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer festlegen können:  
  
-   [Seite Ausdrücke](../../integration-services/expressions/expressions-page.md)  
  
 Klicken Sie auf das folgende Thema, um weitere Informationen zum Festlegen dieser Eigenschaften im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer zu erhalten:  
  
-   [Festlegen der Eigenschaften eines Tasks oder Containers](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="programmatic-configuration-of-the-analysis-services-execute-ddl-task"></a>Programmgesteuerte Konfiguration des Analysis Services-Tasks "DDL ausführen"  
 Klicken Sie auf das folgende Thema, um weitere Informationen zum programmgesteuerten Festlegen dieser Eigenschaften anzuzeigen:  
  
-   <xref:Microsoft.DataTransformationServices.Tasks.DTSProcessingTask.ASExecuteDDLTask>  
  
## <a name="analysis-services-execute-ddl-task-editor-general-page"></a>Editor für den Analysis Services-Task 'DDL ausführen' (Seite Allgemein)
  Auf der Seite **Allgemein** des Dialogfelds **Editor für den Analysis Services-Task „DDL ausführen“** können Sie den Namen und eine Beschreibung des [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Tasks „DDL ausführen“ angeben.  
  
### <a name="options"></a>Tastatur  
 **Name**  
 Geben Sie einen eindeutigen Namen für den [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Task „DDL ausführen“ an. Dieser Name wird im Tasksymbol als Bezeichnung verwendet.  
  
> [!NOTE]  
>  Tasknamen müssen innerhalb eines Pakets eindeutig sein.  
  
 **Beschreibung**  
 Geben Sie eine Beschreibung des [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Tasks 'DDL ausführen' ein.  
  
## <a name="analysis-services-execute-ddl-task-editor-ddl-page"></a>Editor für den Analysis Services-Task 'DDL ausführen' (Seite DDL)
  Auf der Seite **DDL** des Dialogfelds **Editor für den Analysis Services-Task „DDL ausführen“** können Sie eine Verbindung mit einem [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Projekt oder einer [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Datenbank angeben sowie Informationen zur Quelle der DDL-Anweisungen (Data Definition Language) angeben.  
  
### <a name="static-options"></a>Statische Optionen  
 **Connection**  
 Wählen Sie ein [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Projekt oder einen [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Verbindungs-Manager aus der Liste aus, oder klicken Sie auf \<**New connection...**>, und erstellen Sie im Dialogfeld **Analysis Services-Verbindungs-Manager hinzufügen** eine neue Verbindung.  
  
 **Verwandte Themen:** [Referenz zur Benutzeroberfläche des Dialogfelds Analysis Services-Verbindungs-Manager hinzufügen](../../integration-services/connection-manager/add-analysis-services-connection-manager-dialog-box-ui-reference.md), [Analysis Services-Verbindungs-Manager](../../integration-services/connection-manager/analysis-services-connection-manager.md)  
  
 **SourceType**  
 Geben Sie den Quelltyp der DDL-Anweisung an. Für diese Eigenschaft sind die in der folgenden Tabelle aufgeführten Optionen verfügbar:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Direct Input**|Legen Sie die Quelle der im Textfeld **SourceDirect** gespeicherten DDL-Anweisung fest. Wenn Sie diesen Wert auswählen, werden im folgenden Abschnitt die dynamischen Optionen angezeigt.|  
|**File Connection**|Legen Sie die Quelle für eine Datei fest, in der die DDL-Anweisung enthalten ist. Wenn Sie diesen Wert auswählen, werden im folgenden Abschnitt die dynamischen Optionen angezeigt.|  
|**Variable**|Legen Sie die Quelle für eine Variable fest. Wenn Sie diesen Wert auswählen, werden im folgenden Abschnitt die dynamischen Optionen angezeigt.|  
  
### <a name="dynamic-options"></a>Dynamische Optionen  
  
#### <a name="sourcetype--direct-input"></a>SourceType = Direkteingabe  
 **Quelle**  
 Geben Sie die DDL-Anweisungen ein, oder klicken Sie auf die Schaltfläche mit den **Auslassungspunkten (...)** , und geben Sie dann die Anweisungen im Dialogfeld **DDL-Anweisungen** ein.  
  
#### <a name="sourcetype--file-connection"></a>SourceType = File Connection  
 **Quelle**  
 Wählen Sie eine Dateiverbindung aus der Liste aus, oder klicken Sie auf \<**New connection...**>, und erstellen Sie im Dialogfeld **Dateiverbindungs-Manager** eine neue Verbindung.  
  
 **Verwandte Themen:** [Dateiverbindungs-Manager](../../integration-services/connection-manager/file-connection-manager.md)  
  
#### <a name="sourcetype--variable"></a>SourceType = Variable  
 **Quelle**  
 Wählen Sie eine Variable aus der Liste aus, oder klicken Sie auf \<**New variable...**>, und erstellen Sie im Dialogfeld **Variable hinzufügen** eine neue Variable.  
  
 **Verwandte Themen:** [Integration Services-Variablen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md)  
