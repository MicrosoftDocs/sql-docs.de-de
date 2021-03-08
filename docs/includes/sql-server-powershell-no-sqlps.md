---
title: Verwenden von NOSQLPS für SQL Server-Agent mit PowerShell
description: Meldung zur Erläuterung der Verwendung des PowerShell-Cmdlets „sqlserver“ anstelle des Cmdlet „sqlps“ mit SQL Server-Agent
ms.topic: include
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier
ms.openlocfilehash: bf161bd583f3d48dc389cea6b69f55c32e2cce59
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839404"
---
Ab SQL Server 2019 können Sie SQLPS deaktivieren. Fügen Sie hierzu der ersten Zeile eines Auftragsschritts des PowerShell-Moduls `#NOSQLPS` hinzu, wodurch der SQL-Agent aufhört, das SQLPS-Modul automatisch zu laden. Nun führt der SQL-Agent-Auftrag die auf dem Computer installierte PowerShell-Version aus. Sie können dann auch ein beliebiges anderes PowerShell-Modul verwenden.

Um das Modul [**SqlServer**](https://www.powershellgallery.com/packages/Sqlserver/21.1.18235) in Ihrem SQL-Agent-Auftragsschritt zu verwenden, können Sie diesen Code in den ersten beiden Skriptzeilen platzieren.

```powershell
#NOSQLPS
Import-Module -Name SqlServer
```