---
description: Cancel-Methode – Beispiel (VBScript)
title: Cancel-Methode (Beispiel) (VBScript) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Cancel method [ADO], VBScript example
ms.assetid: 4ade106d-063d-486e-bc4d-a1a6b6e0bea9
author: rothja
ms.author: jroth
ms.openlocfilehash: ccdd34512ba797aa5de8da14ac03011da00bf54e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163916"
---
# <a name="cancel-method-example-vbscript"></a>Cancel-Methode – Beispiel (VBScript)
> [!IMPORTANT]
>  Ab Windows 8 und Windows Server 2012 sind RDS-Server Komponenten nicht mehr im Windows-Betriebssystem enthalten (weitere Details finden Sie unter Windows 8 und [Windows Server 2012 Compatibility Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). RDS-Client Komponenten werden in einer zukünftigen Version von Windows entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Anwendungen, die RDS verwenden, sollten zu [WCF Data Service](/dotnet/framework/wcf/)migriert werden.  
  
 Im folgenden Beispiel wird gezeigt, wie die [Cancel](../ado-api/cancel-method-ado.md) -Methode zur Laufzeit gelesen wird. Schneiden Sie den folgenden Code aus, und fügen Sie ihn in Editor oder einen anderen Text-Editor ein, und speichern Sie ihn als cancelvsb. ASP. Sie können das Ergebnis in einem beliebigen Client Browser anzeigen.  
  
```  
<!-- BeginCancelVBS -->  
<Script Language="VBScript">  
<!--  
Sub cmdCancelAsync_OnClick  
   ' Terminates currently running AsyncExecute,  
   ' ReadyState property set to adcReadyStateLoaded,  
   ' Recordset set to Nothing  
  ADC.Cancel  
End Sub  
  
Sub cmdRefreshTable_OnClick  
   ADC.Refresh  
End Sub  
-->  
</Script>  
  
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="ADC">  
.  
   <PARAM NAME="SQL" VALUE="Select FirstName, LastName from Employees">  
   <PARAM NAME="CONNECT" VALUE="Provider='sqloledb';Integrated Security='SSPI';Initial Catalog='Northwind'">  
   <PARAM NAME="Server" VALUE="https://<%=Request.ServerVariables("SERVER_NAME")%>">  
.  
</OBJECT>  
  
<TABLE DATASRC=#ADC>  
<TBODY>  
  <TR>  
    <TD><SPAN DATAFLD="FirstName"></SPAN></TD>  
    <TD><SPAN DATAFLD="LastName"></SPAN></TD>  
  </TR>  
</TBODY>  
</TABLE>  
  
<FORM>  
<INPUT type="button" value="Refresh" id=cmdRefreshTable name=cmdRefreshTable>  
<INPUT type="button" value="Cancel" id=cmdCancelAsync name=cmdCancelAsync>  
</FORM>  
<!-- EndCancelVBS -->  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Cancel-Methode (ADO)](../ado-api/cancel-method-ado.md)