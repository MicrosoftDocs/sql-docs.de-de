---
title: Escape-Bezeichner für SQL Server
description: Einige Zeichen können in den Begrenzungsbezeichnern von SQL Server enthalten sein, die in Windows PowerShell-Pfaden nicht unterstützt werden. Hier erfahren Sie, wie einige dieser Zeichen mit einem Escapezeichen versehen werden können.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 105acf1e3e67d558ccbb0ae450afba9468de3a5f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838029"
---
# <a name="escape-sql-server-identifiers"></a>Escape-Bezeichner für SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Sie können Zeichen, die zwar in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Begrenzungsbezeichnern zulässig sind, in Windows PowerShell-Pfadnamen jedoch nicht, oftmals mit dem Escapezeichen (`) umwandeln. Einige Zeichen können jedoch nicht mit Escapezeichen versehen werden. Zum Beispiel können Sie das Doppelpunktzeichen (:) in Windows PowerShell nicht mit Escapezeichen versehen. Bezeichner mit diesem Zeichen müssen codiert werden. Codierung ist zuverlässiger als das Umwandeln mit Escapezeichen, da das Codieren für alle Zeichen funktioniert.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Das Graviszeichen (`) befindet sich i. d. R. oben rechts auf der Tastatur, links von der RÜCKTASTE.  

## <a name="examples"></a>Beispiele

Dies ist ein Beispiel für das Umwandeln eines #-Zeichens mittels Escapezeichen:  

```powershell
cd SQLSERVER:\SQL\MyComputer\MyInstance\MyDatabase\MySchema\`#MyTempTable  
```

Dies ist ein Beispiel für das Maskieren der Klammer mit dem Escapezeichen beim Angeben von "(lokal)" als Computername:  

```powershell
Set-Location SQLSERVER:\SQL\`(local`)\DEFAULT  
```

## <a name="see-also"></a>Weitere Informationen

- [SQL Server-Bezeichnern in PowerShell](sql-server-identifiers-in-powershell.md)
- [SQL Server PowerShell-Anbieter](sql-server-powershell-provider.md)
- [SQL Server-PowerShell](sql-server-powershell.md)