---
description: T-SQL-Anweisung ausführen (Task)
title: T-SQL-Anweisung ausführen (Task) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.executetsqlstatementtask.f1
helpviewer_keywords:
- Transact-SQL statements, SSIS
- statements [Integration Services]
- Execute T-SQL Statement task [Integration Services]
ms.assetid: 7e9086ca-d27e-46c0-bfad-d61333ebd55e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 060fa6ad9faae0fa6159eba2591623af57a41c5b
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2020
ms.locfileid: "92196544"
---
# <a name="execute-t-sql-statement-task"></a>T-SQL-Anweisung ausführen (Task)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Mit dem Task T-SQL-Anweisung ausführen werden Transact-SQL-Anweisungen ausgeführt. Weitere Informationen finden Sie unter [Transact-SQL-Referenz &#40;Datenbank-Engine&#41;](../../t-sql/language-reference.md) und [Integration Services-Abfragen &#40;SSIS&#41;](../../integration-services/integration-services-ssis-queries.md).  
  
 Dieser Task ist mit dem Task SQL ausführen vergleichbar. Der Task T-SQL-Anweisung ausführen unterstützt jedoch nur die Transact-SQL-Version der SQL-Sprache und kann nicht zum Ausführen von Anweisungen auf Servern verwendet werden, die andere Dialekte der SQL-Sprache verwenden. Falls Sie parametrisierte Abfragen ausführen müssen, speichern Sie die Abfrageergebnisse in Variablen, oder verwenden Sie Eigenschaftsausdrücke. Sie sollten den Task SQL ausführen anstelle des Tasks T-SQL-Anweisung ausführen verwenden. Weitere Informationen finden Sie unter [Execute SQL Task](../../integration-services/control-flow/execute-sql-task.md).  
  
## <a name="configuration-of-the-execute-t-sql-task"></a>Konfiguration des Tasks "T-SQL-Anweisung ausführen"  
 Eigenschaften können Sie mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer festlegen. Dieser Task ist im **-Designer in der** Toolbox **im Abschnitt** Wartungsplantasks [!INCLUDE[ssIS](../../includes/ssis-md.md)] enthalten.  
  
 Klicken Sie auf das folgende Thema, um weitere Informationen zu den Eigenschaften zu erhalten, die Sie im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer festlegen können:  
  
-   [Task „T-SQL-Anweisung ausführen“ &#40;Wartungsplan&#41;](../../relational-databases/maintenance-plans/execute-t-sql-statement-task-maintenance-plan.md)  
  
 Klicken Sie auf das folgende Thema, um weitere Informationen zum Festlegen dieser Eigenschaften im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer zu erhalten:  
  
-   [Festlegen der Eigenschaften eines Tasks oder Containers](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Tasks](../../integration-services/control-flow/integration-services-tasks.md)   
 [Ablaufsteuerung](../../integration-services/control-flow/control-flow.md)   
 [MERGE in Integration Services-Paketen](../../integration-services/control-flow/merge-in-integration-services-packages.md)  
  
