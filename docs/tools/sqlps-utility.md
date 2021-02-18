---
title: sqlps (Hilfsprogramm)
description: Das Hilfsprogramm „sqlps“ startet eine Windows PowerShell-Sitzung mit geladenem und registriertem SQL Server PowerShell-Anbieter sowie geladenen und registrierten Cmdlets.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- sqlps utility
- PowerShell [SQL Server], sqlps utility
ms.assetid: 4b2515a6-12c3-44fb-b263-1c567681cd2b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 87fbc1760cecc26f98b4c6e6f610f32683015e66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338574"
---
# <a name="sqlps-utility"></a>sqlps (Hilfsprogramm)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Das Hilfsprogramm **sqlps** startet eine Windows PowerShell-Sitzung mit geladenem und registriertem [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -PowerShell-Anbieter sowie geladenen und registrierten Cmdlets. Sie können PowerShell-Befehle oder -Skripts eingeben, die die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -PowerShell-Komponenten verwenden, sodass Instanzen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] und ihre Objekte verwendet werden können.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../includes/ssnotedepfutureavoid-md.md)] Verwenden Sie stattdessen das PowerShell-Modul **sqlps**. Weitere Informationen zum **sqlps** -Modul finden Sie unter [Importieren des SQLPS-Moduls](../powershell/download-sql-server-ps-module.md).  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sqlps   
[ [ [ -NoLogo ][ -NoExit ][ -NoProfile ]  
    [ -OutPutFormat { Text | XML } ] [ -InPutFormat { Text | XML } ]  
  ]  
  [ -Command { -  
             | script_block [ -args argument_array ]  
             | string [ command_parameters ]  
             }  
  ]  
]  
[ -? | -Help ]  
```  
  
## <a name="arguments"></a>Argumente  
 **-NoLogo**  
 Gibt an, dass beim Start des Hilfsprogramms **sqlps** die Copyrightinformationen ausgeblendet werden.  
  
 **-NoExit**  
 Gibt an, dass das Hilfsprogramm **sqlps** weiter ausgeführt wird, nachdem die Startbefehle abgeschlossen wurden.  
  
 **-NoProfile**  
 Gibt an, dass das Hilfsprogramm **sqlps** kein Benutzerprofil lädt. In Benutzerprofilen werden häufig verwendete Aliase, Funktionen und Variablen zur Verwendung in mehreren PowerShell-Sitzungen aufgezeichnet.  
  
 **-OutPutFormat** { **Text** | **XML** }  
 Gibt an, dass die Ausgabe des Hilfsprogramms **sqlps** entweder als Textzeichenfolgen (**Text**) oder in einem serialisierten CLIXML-Format (**XML**) formatiert werden soll.  
  
 **-InPutFormat** { **Text** | **XML** }  
 Gibt an, dass die Eingabe in das Hilfsprogramm **sqlps** entweder als Textzeichenfolgen (**Text**) oder in einem serialisierten CLIXML-Format (**XML**) formatiert ist.  
  
 **-Command**  
 Gibt den Befehl an, der vom Hilfsprogramm **sqlps** ausgeführt werden soll. Das Hilfsprogramm **sqlps** führt den Befehl aus und wird dann beendet, es sei denn, **-NoExit** ist ebenfalls angegeben. Geben Sie nach **-Command** keine anderen Schalter an, denn diese werden als Befehlsparameter gelesen.  
  
 **-**  
 **-Command-** gibt an, dass das Hilfsprogramm **sqlps** die Eingabe aus der Standardeingabe lesen soll.  
  
 *script_block* [ **-args**_argument\_array_ ]  
 Gibt einen Block von PowerShell-Befehlen an, die ausgeführt werden sollen. Der Block muss in geschweifte Klammern ({}) eingeschlossen werden. *Script_block* kann nur angegeben werden, wenn das Hilfsprogramm **sqlps** über **PowerShell** oder über eine andere Sitzung des Hilfsprogramms **sqlps** aufgerufen wird. *Argument_array* ist ein Array von PowerShell-Variablen, das die Argumente für die PowerShell-Befehle in *script_block* enthält.  
  
 *string* [ *command_parameters* ]  
 Gibt eine Zeichenfolge an, die die auszuführenden PowerShell-Befehle enthält. Verwenden Sie das Format **"&{** _command_ **}"** . Die Anführungszeichen geben eine Zeichenfolge an, und der Aufrufoperator (&) bewirkt, dass das Hilfsprogramm **sqlps** den Befehl ausführt.  
  
 [ **-?** |  **-Help** ]  
 Zeigt eine Syntaxzusammenfassung der Optionen des Hilfsprogramms **sqlps** an.  
  
## <a name="remarks"></a>Bemerkungen  
 Das Hilfsprogramm **sqlps** startet die PowerShell-Umgebung (PowerShell.exe) und lädt das [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -PowerShell-Modul. Das Modul, das auch **sqlps** heißt, lädt und registriert diese [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -PowerShell-Snap-Ins:  
  
-   Microsoft.SqlServer.Management.PSProvider.dll  
  
     Implementiert den [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -PowerShell-Anbieter und zugeordnete Cmdlets, z. B. **Encode-SqlName** und **Decode-SqlName**.  
  
-   Microsoft.SqlServer.Management.PSSnapin.dll  
  
     Implementiert die Cmdlets **Invoke-Sqlcmd** und **Invoke-PolicyEvaluation** .  
  
 Das Hilfsprogramm **sqlps** kann für die folgenden Tasks verwendet werden:  
  
-   Interaktives Ausführen von PowerShell-Befehlen  
  
-   Ausführen von PowerShell-Skriptdateien  
  
-   Ausführen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Cmdlets  
  
-   Verwenden Sie die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Anbieterpfade, um durch die Hierarchie der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Objekte zu navigieren.  
  
 Standardmäßig ist für die Ausführung des Hilfsprogramms **sqlps** die Skriptausführungsrichtlinie auf **Restricted** festgelegt. Dadurch wird die Ausführung von PowerShell-Skripts verhindert. Mit dem Cmdlet **Set-ExecutionPolicy** können Sie die Ausführung signierter Skripts oder beliebiger anderer Skripts ermöglichen. Führen Sie nur Skripts aus vertrauenswürdigen Quellen aus, und sichern Sie alle Eingabe- und Ausgabedateien, indem Sie die geeigneten NTFS-Berechtigungen verwenden. Weitere Informationen zum Aktivieren von PowerShell-Skripts finden Sie unter [Ausführen der Windows PowerShell-Skripts](/previous-versions/system-center/virtual-machine-manager-2008-r2/cc917925(v=technet.10)).  
  
 Die Version des Hilfsprogramms **sqlps** in [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] und [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)] wurde als Windows PowerShell 1.0-Mini-Shell implementiert. Mini-Shells weisen bestimmte Einschränkungen auf; Benutzer können beispielsweise keine anderen als die von der Mini-Shell geladenen Snap-Ins laden. Diese Einschränkungen gelten nicht für die [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] -Version und höhere Versionen des Hilfsprogramms, die dahingehend geändert wurden, dass sie das **sqlps** -Modul verwenden.  
  
## <a name="examples"></a>Beispiele  
 **A. Ausführen des sqlps-Hilfsprogramms im interaktiven Standardmodus ohne Copyrightinformationen**  
  
```  
sqlps -NoLogo  
```  
  
 **B. Ausführen eines SQL Server PowerShell-Skripts über die Eingabeaufforderung**  
  
```  
sqlps -Command "&{.\MyFolder.MyScript.ps1}"  
```  
  
 **C. Ausführen eines SQL Server PowerShell-Skripts über die Eingabeaufforderung und weitere Ausführung nach Abschluss des Skripts**  
  
```  
sqlps -NoExit -Command "&{.\MyFolder.MyScript.ps1}"  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Aktivieren oder Deaktivieren eines Servernetzwerkprotokolls](../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)   
 [SQL Server-PowerShell](../powershell/sql-server-powershell.md)  
  
