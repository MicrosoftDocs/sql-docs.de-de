---
description: Erstellen eines Visual C#-SMO-Projekts in Visual Studio .NET
title: Erstellen eines Visual C# SMO-Projekts in Visual Studio.NET
ms.custom: seo-dt-2019
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- Visual C# [SMO]
ms.assetid: 1e7abb16-23a0-4a18-91ad-253261e6bf84
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 943cfd787cccff42973862b12f0f52feae24410c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467191"
---
# <a name="how-to-create-a-visual-c-smo-project-in-visual-studio-net"></a>Erstellen eines Visual C#-SMO-Projekts in Visual Studio .NET
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  In diesem Abschnitt wird beschrieben, wie eine einfache SMO-Konsolenanwendung erstellt wird.  
  
 In diesem Beispiel werden Namespaces importiert. Hierdurch kann das Programm auf SMO-Typen verweisen. Der Import des **Agent** -Namespace ist optional. Verwenden Sie es, wenn Sie ein Programm schreiben, das den- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent verwendet. Der **gemeinsame** Namespace ist erforderlich, um eine sichere Verbindung mit der Instanz von herzustellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Der **SqlClient** -Namespace wird verwendet, um SQL-Ausnahme Fehler zu verarbeiten.  
  
### <a name="creating-a-visual-c-smo-project-in-visual-studionet"></a>Erstellen eines Visual c# SMO-Projekts in Visual Studio.net  
  
1. Starten von VisualStudio
  
2. Klicken Sie im Menü **Datei** auf **neu** und dann auf **Projekt**.  Das Dialogfeld **Neues Projekt** wird angezeigt.   
  
3. Navigieren Sie im Bereich [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] **installiert** zu **Vorlagen** \\ **Visual c#** \\ **Windows** , und wählen Sie **Konsolenanwendung** aus.  
  
4. Optionale Geben Sie im Textfeld **Name** den Namen der neuen Anwendung ein.  

5. Klicken Sie auf **OK** , um die Vorlage Konsolenanwendung zu laden.  

6. Befolgen Sie die Anweisungen unter [Installieren von SMO](installing-smo.md) , um das Paket zu installieren, auf das Ihr Projekt verweisen soll.
  
7. Klicken Sie im Menü **Ansicht** auf **Code**.
    
8. Geben Sie im Code vor der Namespace-Anweisung die folgenden **using** -Anweisungen ein, um die Typen im SMO-Namespace zu qualifizieren:
  
    ```  
    using Microsoft.SqlServer.Management.Smo;  
    using Microsoft.SqlServer.Management.Common;  
    ```  
  
15. SMO verfügt über verschiedene Namespaces unter Microsoft.SqlServer.Management.Smo, z. B. Microsoft.SqlServer.Management.Smo.Agent. Fügen Sie diese Namespaces nach Bedarf hinzu.  
  
16. Sie können jetzt den SMO-Code hinzufügen.  

