---
title: Ausführen von Paketen in SSIS Scale Out (SQL Server Integration Services) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie SQL Server Integration Services-Pakete (SSIS) in Scale Out mithilfe verschiedener Methoden ausführen.
ms.custom: performance
ms.date: 12/13/2017
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
author: HaoQian-MS
ms.author: haoqian
ms.reviewer: maghan
f1_keywords:
- sql13.ssis.ssms.ispackageexecuteinscaleout.f1
ms.openlocfilehash: 25f550f711364760de9cc07608b46532cfa34219
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839004"
---
# <a name="run-packages-in-integration-services-ssis-scale-out"></a>Ausführen von Paketen in SSIS Scale Out (SQL Server Integration Services)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Nachdem Sie Pakete auf dem Integration Services-Server bereitgestellt haben, können Sie sie über eine der folgenden Methoden in Scale Out ausführen:

-   [Dialogfeld „Paket in Scale Out ausführen“](#scale_out_dialog)

-   [Gespeicherten Prozeduren](#stored_proc)

-   [Aufträge des SQL Server-Agents](#sql_agent)

## <a name="run-packages-with-the-execute-package-in-scale-out-dialog-box"></a><a name="scale_out_dialog"></a> Ausführen von Paketen über das Dialogfeld „Paket in Scale Out ausführen“

1. Öffnen Sie das Dialogfeld „Paket in Scale Out ausführen“.

    Stellen Sie in [!INCLUDE[ssManStudioFull_md](../../includes/ssmanstudiofull-md.md)]eine Verbindung mit dem Integration Services-Server her. Erweitern Sie im Objekt-Explorer die Struktur, um die Knoten unter **Integration Services-Kataloge** anzuzeigen. Klicken Sie mit der rechten Maustaste auf den **SSISDB**-Knoten oder auf das Projekt oder Paket, das Sie ausführen möchten, und klicken Sie dann auf **In Scale Out ausführen**.

2. Wählen Sie Pakete aus, und legen Sie die Optionen fest.

    Wählen Sie auf der Seite **Paketauswahl** mindestens ein Paket aus, das ausgeführt werden soll. Legen Sie für jedes Paket die Umgebung, Parameter, Verbindungs-Manager und erweiterte Optionen fest. Klicken Sie auf ein Paket, um diese Optionen festzulegen.
    
    Legen Sie auf der Registerkarte **Erweitert** die Aufskalierungsoption **Wiederholungsanzahl** fest, um zu bestimmen, wie oft versucht werden soll, ein Paket auszuführen, wenn ein Fehler auftritt.

    > [!NOTE]
    > Die Option **Bei Fehler Abbild sichern** funktioniert nur, wenn das Konto, unter dem der Scale Out-Workerdienst ausgeführt wird, einem Administrator auf dem lokalen Computer zugeordnet ist.

3. Wählen Sie Workercomputer aus.

    Wählen Sie auf der Seite **Computerauswahl** die Scale Out-Workercomputer aus, auf denen die Pakete ausgeführt werden sollen. Standardmäßig können die Pakete auf jedem Computer ausgeführt werden. 

   > [!NOTE] 
   > Die Pakete werden mit den Anmeldeinformationen der Benutzerkonten der Dienste für Scale Out-Worker ausgeführt. Diese Anmeldeinformationen finden Sie auf der Seite **Computerauswahl**. Standardmäßig lautet das Konto `NT Service\SSISScaleOutWorker140`.

   > [!WARNING]
   > Die von verschiedenen Benutzern im selben Worker ausgelösten Paketausführungen werden mit denselben Anmeldeinformationen ausgeführt. Es gibt keine Sicherheitsbegrenzung zwischen diesen. 

4. Führen Sie die Pakete aus, und zeigen Sie Berichte an.

    Klicken Sie auf **OK** , um die Paketausführungen zu starten. Um den Ausführungsbericht für ein Paket anzuzeigen, klicken Sie im Objekt-Explorer mit der rechten Maustaste auf das Paket, klicken Sie auf **Berichte**, klicken Sie auf **Alle Ausführungen**, und suchen Sie nach der Ausführung.
    
## <a name="run-packages-with-stored-procedures"></a><a name="stored_proc"></a> Ausführen von Paketen mit gespeicherten Prozeduren

1.  Erstellen Sie Ausführungen.

    Rufen Sie für jedes Paket `[catalog].[create_execution]` auf. Legen Sie den Parameter **\@runinscaleout** auf `True` fest. Sind nicht alle Scale Out-Workercomputer berechtigt, das Paket auszuführen, legen Sie den Parameter **\@useanyworker** auf `False` fest. Weitere Informationen zu dieser gespeicherten Prozedur und dem Parameter **\@useanyworker** finden Sie unter [catalog.create_execution](../system-stored-procedures/catalog-create-execution-ssisdb-database.md). 

2. Legen Sie Ausführungsparameter fest.

    Rufen Sie für jede Ausführung `[catalog].[set_execution_parameter_value]` auf.

3. Legen Sie die Scale Out-Worker fest.

    Rufen Sie `[catalog].[add_execution_worker]` auf. Sind alle Computer berechtigt, das Paket auszuführen, müssen Sie diese gespeicherte Prozedur nicht aufrufen. 

4. Starten Sie die Ausführungen.

    Rufen Sie `[catalog].[start_execution]` auf. Legen Sie den Parameter **\@retry_count** fest, um anzugeben, wie oft versucht werden soll, ein Paket auszuführen, wenn ein Fehler auftritt.
    
### <a name="example"></a>Beispiel
Im folgenden Beispiel werden zwei Pakete in Scale Out mit einem Scale Out-Worker ausgeführt: `package1.dtsx` und `package2.dtsx`.  

```sql
Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'package1.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'folder1', @project_name=N'project1', @use32bitruntime=False, @reference_id=Null, @useanyworker=False, @runinscaleout=True
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0
EXEC [SSISDB].[catalog].[add_execution_worker] @execution_id,  @workeragent_id=N'64c020e2-f819-4c2d-a22f-efb31a91e70a'
EXEC [SSISDB].[catalog].[start_execution] @execution_id,  @retry_count=0
GO

Declare @execution_id bigint
EXEC [SSISDB].[catalog].[create_execution] @package_name=N'package2.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'folder2', @project_name=N'project2', @use32bitruntime=False, @reference_id=Null, @useanyworker=False, @runinscaleout=True
Select @execution_id
DECLARE @var0 smallint = 1
EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0
EXEC [SSISDB].[catalog].[add_execution_worker] @execution_id,  @workeragent_id=N'64c020e2-f819-4c2d-a22f-efb31a91e70a'
EXEC [SSISDB].[catalog].[start_execution] @execution_id,  @retry_count=0
GO
```

### <a name="permissions"></a>Berechtigungen
Eine der folgenden Berechtigungen ist erforderlich, um die Pakete in Scale Out auszuführen:

-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  

-   Mitgliedschaft in der Datenbankrolle **ssis_cluster_executor**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  

## <a name="set-default-execution-mode"></a>Festlegen des Standardausführungsmodus
Führen Sie die folgenden Schritte aus, um den Standardausführungsmodus für Pakete auf **Scale Out** festzulegen:

1.  Klicken Sie in SSMS in Objekt-Explorer mit der rechten Maustaste auf den Knoten **SSISDB**, und wählen Sie **Eigenschaften** aus.

2.  Legen Sie im Dialogfeld **Katalogeigenschaften** für den **serverweiten Standardausführungsmodus** die Option **Scale Out** fest.

Nachdem Sie diesen Standardausführungsmodus festgelegt haben, müssen Sie den **\@runinscaleout**-Parameter nicht mehr angeben, wenn Sie die gespeicherte Prozedur `[catalog].[create_execution]` aufrufen. Pakete werden in Scale Out automatisch ausgeführt. 

![Ausführungsmodus](media/exe-mode.PNG)

Um den Standardausführungsmodus wieder auf den Modus ohne Scale Out zurückzusetzen, damit die Pakete nicht mehr standardmäßig im Scale Out-Modus ausgeführt werden, legen Sie den **serverweiten Standardausführungsmodus** auf **Server** fest.

## <a name="run-package-in-sql-server-agent-job"></a><a name="sql_agent"></a> Ausführen eines Pakets in einem SQL Server-Agent-Auftrag
In einem SQL Server-Agent-Auftrag können Sie im Rahmen des Auftrags ein SSIS-Paket ausführen. Legen Sie den Standardausführungsmodus auf **Scale Out** fest, um das Paket in Scale Out auszuführen. Nachdem der Standardausführungsmodus auf **Scale Out** festgelegt wurde, werden Pakete in SQL-Agent-Aufträgen in Scale Out ausgeführt.

> [!NOTE]
> Sie können die Ausführung von Scale Out-Paketen beenden, indem Sie den SQL Server-Agent-Auftrag abbrechen. Um die Ausführung von Scale Out zu beenden, empfehlen wir, die gespeicherte Prozedur catalog.stop_operation oder den Bereich **Aktive Vorgänge** zu verwenden. 

## <a name="next-steps"></a>Nächste Schritte
-   [Problembehandlung in Scale Out](troubleshooting-scale-out.md)
