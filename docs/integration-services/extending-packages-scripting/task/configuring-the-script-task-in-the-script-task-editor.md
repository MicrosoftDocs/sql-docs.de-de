---
description: Konfigurieren des Skripttasks im Skripttask-Editor
title: Konfigurieren des Skripttasks im Skripttask-Editor | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Script task [Integration Services], configuring
- Script Task Editor
- SSIS Script task, configuring
ms.assetid: 232de0c9-b24d-4c38-861d-6c1f4a75bdf3
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8b4a052ea59e3c72adda887c3084bca278059f0b
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96122927"
---
# <a name="configuring-the-script-task-in-the-script-task-editor"></a>Konfigurieren des Skripttasks im Skripttask-Editor

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Bevor Sie benutzerdefinierten Code in den Skripttask schreiben, konfigurieren Sie auf den drei Seiten im **Skripttask-Editor** seine Prinzipaleigenschaften. Mithilfe des Eigenschaftsfensters können Sie zusätzliche Taskeigenschaften, die nicht nur für den Skripttask vorhanden sind, konfigurieren.  
  
> [!NOTE]  
>  Anders als in früheren Versionen, in denen Sie angeben konnten, ob die Skripts vorkompiliert sind, werden ab [!INCLUDE[ssISversion10](../../../includes/ssisversion10-md.md)] alle Skripts vorkompiliert.  
  
## <a name="general-page-of-the-script-task-editor"></a>Seite 'Allgemein' des Skripttask-Editors  
 Auf der Seite **Allgemein** im **Skripttask-Editor** können Sie dem Skripttask einen eindeutigen Namen und eine Beschreibung zuweisen.  
  
## <a name="script-page-of-the-script-task-editor"></a>Seite 'Skript' des Skripttask-Editors  
 Auf der Seite **Skript** im **Skripttask-Editor** werden die benutzerdefinierten Eigenschaften des Skripttasks angezeigt.  
  
### <a name="scriptlanguage-property"></a>ScriptLanguage-Eigenschaft  
 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] Tools for Applications (VSTA) unterstützt die Programmiersprachen [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic oder [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C#. Nach der Erstellung eines Skripts im Skripttask können Sie den Wert der **ScriptLanguage**-Eigenschaft nicht mehr ändern.  
  
 Um die Standardskriptsprache für Skripttasks und Skriptkomponenten festzulegen, verwenden Sie im Dialogfeld **Optionen** auf der Seite **Allgemein** die **ScriptLanguage**-Eigenschaft. Weitere Informationen finden Sie unter [General Page](../../general-page-of-integration-services-designers-options.md).  
  
### <a name="entrypoint-property"></a>EntryPoint-Eigenschaft  
 Die **EntryPoint**-Eigenschaft gibt die Methode für die **ScriptMain**-Klasse im VSTA-Projekt an, die die [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)]-Laufzeit als Einstiegspunkt in den Code des Skripttasks aufruft. Die **ScriptMain**-Klasse ist die Standardklasse, die von den Skriptvorlagen generiert wird.  
  
 Wenn Sie den Namen der Methode im VSTA-Projekt geändert haben, müssen Sie den Wert der **EntryPoint** -Eigenschaft ändern.  
  
### <a name="readonlyvariables-and-readwritevariables-properties"></a>Eigenschaften 'ReadOnlyVariables' und 'ReadWriteVariables'  
 Sie können kommagetrennte Listen von vorhandenen Variablen als Werte dieser Eigenschaften eingeben, um die Variablen für schreibgeschützten oder Lese-/Schreibzugriff im Code des Skripttasks verfügbar zu machen. Auf Variablen beider Typen wird im Code über die <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A>-Eigenschaft des **Dts**-Objekts zugegriffen. Weitere Informationen finden Sie unter [Using Variables in the Script Task](../../../integration-services/extending-packages-scripting/task/using-variables-in-the-script-task.md).  
  
> [!NOTE]  
>  Bei Variablennamen wird nach Groß-/Kleinschreibung unterschieden.  
  
 Klicken Sie auf die Schaltfläche mit den Auslassungspunkten ( **…** ) neben dem Eigenschaftenfeld, um Variablen auszuwählen. Weitere Informationen finden Sie unter [Select Variables Page](../../../integration-services/control-flow/select-variables-page.md) (Seite „Variablen auswählen“).  
  
### <a name="edit-script-button"></a>Schaltfläche 'Skript bearbeiten'  
 Über die Schaltfläche **Skript bearbeiten** wird die VSTA-Entwicklungsumgebung gestartet, in der Sie das benutzerdefinierte Skript schreiben. Weitere Informationen siehe [Coding and Debugging the Script Task](../../../integration-services/extending-packages-scripting/task/coding-and-debugging-the-script-task.md) (Codieren und Debuggen des Skripttasks).  
  
## <a name="expressions-page-of-the-script-task-editor"></a>Seite 'Ausdrücke' des Skripttask-Editors  
 Auf der Seite **Ausdrücke** im **Skripttask-Editor** können Sie Ausdrücke verwenden, um Werte für die Eigenschaften des oben aufgeführten Skripttasks und für viele weitere Taskeigenschaften bereitzustellen. Weitere Informationen finden Sie unter [Integration Services-Ausdrücke &#40;SSIS&#41;](../../../integration-services/expressions/integration-services-ssis-expressions.md)ausgewertet wird.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Codieren und Debuggen des Skripttasks](../../../integration-services/extending-packages-scripting/task/coding-and-debugging-the-script-task.md)  
  
  
