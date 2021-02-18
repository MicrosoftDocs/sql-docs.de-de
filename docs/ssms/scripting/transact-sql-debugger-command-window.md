---
title: Befehlsfenster
description: Hier erfahren Sie, wie Sie das Befehlsfenster des Transact-SQL-Debuggers verwenden, um Debugbefehle auszuführen und Befehle im Code zu bearbeiten, den Sie gerade debuggen.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Command Window [Transact-SQL]
ms.assetid: e567ebf9-0793-451b-92c7-26193a02d9da
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1921e37b51728d9d460082ecbd73643ef80f1a92
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339368"
---
# <a name="transact-sql-debugger---command-window"></a>Transact-SQL-Debugger – Befehlsfenster

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Im **Befehlsfenster** können Sie für den Code im gerade gedebuggten [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]-Abfrage-Editor-Fenster Befehle ausführen, wie z. B. Debug- und Bearbeitungsbefehle. Um das **Befehlsfenster** zu verwenden, müssen Sie sich im Debugmodus befinden. Der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Debugger unterstützt zahlreiche Befehle, die auch im **Befehlsfenster** von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] unterstützt werden. Weitere Informationen finden Sie unter [Visual Studio-Befehlsfenster](/previous-versions/visualstudio/visual-studio-2015/ide/reference/command-window).  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="task-list"></a>Aufgabenliste

**So greifen Sie auf das Befehlsfenster zu**

- Klicken Sie im Menü **Debuggen** auf **Debuggen starten**.

**So geben Sie den Wert einer Variablen aus**

- Geben Sie im **Befehlsfenster** die Zeichenfolge **Debug.Print \<VariableName>** ein, und drücken Sie dann die EINGABETASTE.

**So listen Sie Informationen zum aktuellen Thread auf**

- Geben Sie im **Befehlsfenster** die Zeichenfolge **Debug.ListThread** ein, und drücken Sie dann die EINGABETASTE.

**So fügen Sie dem Fenster Schnellüberwachung eine Variable hinzu**

- Geben Sie im **Befehlsfenster** **Debug.QuickWatch \<VariableName>** ein, und drücken Sie dann die EINGABETASTE.

## <a name="see-also"></a>Weitere Informationen

[Transact-SQL-Debugger](./transact-sql-debugger.md)