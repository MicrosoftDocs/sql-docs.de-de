---
title: Codierung und Decodierung von SQL Server-Bezeichnern
description: Einige Zeichen können in den Begrenzungsbezeichnern von SQL Server enthalten sein, die in Windows PowerShell-Pfaden nicht unterstützt werden. Hier erfahren Sie, wie Sie diese einschließen, indem Sie sie mit ihren hexadezimalen Werten darstellen.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 13b32a62791aaa932311223f4f648d8006eb33e7
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838034"
---
# <a name="encode-and-decode-sql-server-identifiers"></a>Codierung und Decodierung von SQL Server-Bezeichnern

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Begrenzungsbezeichner von SQL Server können Zeichen enthalten, die in Windows PowerShell-Pfaden nicht unterstützt werden. Diese Zeichen können angegeben werden, indem ihre Hexadezimalwerte codiert werden.

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Zeichen, die nicht in Windows PowerShell-Pfadnamen unterstützt werden, können als "%"-Zeichen gefolgt vom Hexadezimalwert des Bitmusters, das das Zeichen darstellt, dargestellt oder codiert werden (Beispiel: **%** xx). Codierung kann immer zur Verarbeitung von Zeichen verwendet werden, die in Windows PowerShell-Pfaden nicht unterstützt werden.

Das Cmdlet **Encode-SqlName** verwendet als Eingabe einen [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Bezeichner. Es gibt eine Zeichenfolge mit allen nicht von der Windows PowerShell-Sprache unterstützten Zeichen, die mit "%xx" codiert sind, aus. Das Cmdlet **Decode-SqlName** verwendet einen codierten [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Bezeichner als Eingabe und gibt den ursprünglichen Bezeichner zurück.  

## <a name="limitations-and-restrictions"></a>Einschränkungen

Die Cmdlets **Encode-Sqlname** und **Decode-Sqlname** codieren oder decodieren nur die Zeichen, die in SQL Server-Begrenzungsbezeichnern zulässig sind, aber in PowerShell-Pfaden nicht unterstützt werden. Im Folgenden werden die Zeichen aufgeführt, die von **Encode-SqlName** codiert und von **Decode-SqlName** decodiert werden:

|||||||||||||
|-|-|-|-|-|-|-|-|-|-|-|-|
|**Zeichen**|\ |/|:|%|\<|>|*|?|[|]|&#124;|  
|**Hexadezimale Codierung**|%5C|%2F|%3A|%25|%3C|%3E|%2A|%3F|%5B|%5D|%7C|

## <a name="encoding-an-identifier"></a>Codieren eines Bezeichners  

### <a name="to-encode-a-sql-server-identifier-in-a-powershell-path"></a>So codieren Sie einen SQL Server-Bezeichner in einem PowerShell-Pfad

- Codieren Sie mithilfe einer der folgenden beiden Methoden einen SQL Server-Bezeichner:
    - Geben Sie den Hexadezimalcode für das nicht unterstützte Zeichen an, indem Sie die Syntax % XX verwenden, wobei XX der Hexadezimalcode ist.
    - Übergeben Sie den Bezeichner als Zeichenfolge in Anführungszeichen an das Cmdlet **Encode-Sqlname** .

### <a name="examples-encoding"></a>Beispiele (Codierung)

In diesem Beispiel wird die codierte Version des Zeichens ":" (%3A) dargestellt:

```powershell
Set-Location Table%3ATest
```

Alternativ können Sie **Encode-SqlName** verwenden, um einen von Windows PowerShell unterstützten Namen zu erstellen:

```powershell
Set-Location (Encode-SqlName "Table:Test")
```

## <a name="decoding-an-identifier"></a>Decodieren eines Bezeichners

### <a name="to-decode-a-sql-server-identifier-from-a-powershell-path"></a>So decodieren Sie einen SQL Server-Bezeichner aus einem PowerShell-Pfad

Verwenden Sie das Cmdlet **Decode-Sqlname** , um die Hexadezimalcodierungen durch die von der Codierung dargestellten Zeichen zu ersetzen.

### <a name="examples-decoding"></a>Beispiele (Decodierung)

Dieses Beispiel gibt „Table:Test“ zurück:

```powershell
Decode-SqlName "Table%3ATest"
```

## <a name="see-also"></a>Weitere Informationen

- [SQL Server-Bezeichnern in PowerShell](sql-server-identifiers-in-powershell.md)
- [SQL Server PowerShell-Anbieter](sql-server-powershell-provider.md)
- [SQL Server-PowerShell](sql-server-powershell.md)  
