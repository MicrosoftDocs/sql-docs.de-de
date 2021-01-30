---
description: ActiveConnection-Eigenschaft (ADO)
title: ActiveConnection-Eigenschaft (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::ActiveConnection
- Recordset15::get_ActiveConnection
- _Record::ActiveConnection
helpviewer_keywords:
- ActiveConnection property [ADO]
ms.assetid: 52d0a96c-14fb-4ad9-b004-4d821bc0a6db
author: rothja
ms.author: jroth
ms.openlocfilehash: efcb376c9ad27dff5a0c85d73f70ae4000385e3d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159240"
---
# <a name="activeconnection-property-ado"></a>ActiveConnection-Eigenschaft (ADO)
Gibt an, zu welchem [Verbindungs](./connection-object-ado.md) Objekt der angegebene [Befehl](./command-object-ado.md), das [Recordset](./recordset-object-ado.md)oder das [Datensatz](./record-object-ado.md) -Objekt derzeit gehört.  
  
## <a name="settings-and-return-values"></a>Einstellungen und Rückgabewerte  
 Legt einen Zeichen folgen Wert fest, der eine Definition für eine Verbindung enthält, wenn die Verbindung geschlossen wird, oder **gibt einen** **Zeichen** folgen Wert zurück, der das aktuelle **Verbindungs** Objekt enthält, wenn die Verbindung geöffnet ist. Der Standardwert ist ein NULL-Objekt Verweis. Siehe die [ConnectionString](./connectionstring-property-ado.md) -Eigenschaft.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die **ActiveConnection** -Eigenschaft, um das **Verbindungs** Objekt zu ermitteln, über das das angegebene **Befehls** Objekt ausgeführt wird oder das angegebene **Recordset** geöffnet wird.  
  
## <a name="command"></a>Get-Help  
 Für **Befehls** Objekte ist die **ActiveConnection** -Eigenschaft Lese-/Schreibzugriff.  
  
 Wenn Sie versuchen, die [Execute](./execute-method-ado-command.md) -Methode für ein **Befehls** Objekt aufzurufen, bevor Sie diese Eigenschaft auf ein offenes **Verbindungs** Objekt oder eine gültige Verbindungs Zeichenfolge festlegen, tritt ein Fehler auf.  
  
 Wenn der **ActiveConnection** -Eigenschaft ein **Verbindungs** Objekt zugewiesen ist, muss das-Objekt geöffnet werden. Das Zuweisen eines geschlossenen Verbindungs Objekts verursacht einen Fehler.  
  
### <a name="note"></a>Hinweis  
 **Microsoft-Visual Basic** Wenn die **ActiveConnection** -Eigenschaft auf *Nothing* festgelegt wird, wird das **Befehls** Objekt der aktuellen **Verbindung** aufgehoben, und der Anbieter wird veranlasst, alle zugeordneten Ressourcen für die Datenquelle freizugeben. Anschließend können Sie das **Befehls** Objekt dem gleichen oder einem anderen **Verbindungs** Objekt zuordnen. Einige Anbieter ermöglichen es Ihnen, die Eigenschafts Einstellung von einer **Verbindung** zu einer anderen zu ändern, ohne dass die Eigenschaft zuerst auf " *Nothing*" festgelegt werden muss.  
  
 Wenn die [Parameters](./parameters-collection-ado.md) -Auflistung des **Command** -Objekts Parameter enthält, die vom Anbieter bereitgestellt werden, wird die Auflistung gelöscht, wenn Sie die **ActiveConnection** -Eigenschaft auf " *Nothing* " oder ein anderes **Verbindungs** Objekt festlegen. Wenn Sie [Parameter](./parameter-object.md) Objekte manuell erstellen und diese zum Auffüllen der **Parameter** Auflistung des **Befehls** Objekts verwenden, lässt das Festlegen der **ActiveConnection** -Eigenschaft auf " *Nothing* " oder auf ein anderes **Verbindungs** Objekt die **Parameter** Auflistung unverändert.  
  
 Wenn Sie das **Verbindungs** Objekt schließen, dem ein **Befehls** Objekt zugeordnet ist, wird die **ActiveConnection** -Eigenschaft auf " *Nothing*" festgelegt. Wenn diese Eigenschaft auf ein geschlossenes **Verbindungs** Objekt festgelegt wird, wird ein Fehler generiert.  
  
## <a name="recordset"></a>Recordset  
 Für geöffnete **Recordsetobjekte** oder für **Recordset** -Objekte, deren [Source](./source-property-ado-recordset.md) -Eigenschaft auf ein gültiges **Befehls** Objekt festgelegt ist, ist die **ActiveConnection** -Eigenschaft schreibgeschützt. Andernfalls ist der Lese-/Schreibzugriff.  
  
 Sie können diese Eigenschaft auf ein gültiges **Verbindungs** Objekt oder auf eine gültige Verbindungs Zeichenfolge festlegen. In diesem Fall erstellt der Anbieter mithilfe dieser Definition ein neues **Verbindungs** Objekt und öffnet die Verbindung. Außerdem kann der Anbieter diese Eigenschaft auf das neue **Verbindungs** Objekt festlegen, um Ihnen eine Möglichkeit zum Zugreifen auf das **Verbindungs** Objekt für erweiterte Fehlerinformationen oder zum Ausführen anderer Befehle zu erhalten.  
  
 Wenn Sie das *ActiveConnection* -Argument der [Open](./open-method-ado-recordset.md) -Methode verwenden, um ein **Recordset** -Objekt zu öffnen, erbt die **ActiveConnection** -Eigenschaft den Wert des-Arguments.  
  
 Wenn Sie die **Source** -Eigenschaft des **Recordset** -Objekts auf eine gültige **Befehls** Objekt Variable festlegen, erbt die **ActiveConnection** -Eigenschaft des **Recordsets** die Einstellung der **ActiveConnection** -Eigenschaft des **Befehls** Objekts.  
  
> [!NOTE]
>  **Verwendung von Remote Datendiensten** Wenn diese Eigenschaft auf einem Client seitigen  Recordsetobjekt verwendet wird, kann Sie nur auf eine Verbindungs Zeichenfolge oder (in Microsoft Visual Basic oder Visual Basic, Scripting Edition) auf *Nothing* festgelegt werden.  
  
## <a name="record"></a>Record  
 Diese Eigenschaft ist Lese-/Schreibzugriff, wenn das **Datensatz** -Objekt geschlossen ist, und kann eine Verbindungs Zeichenfolge oder einen Verweis auf ein offenes **Verbindungs** Objekt enthalten. Diese Eigenschaft ist schreibgeschützt, wenn das **Datensatz** -Objekt geöffnet ist, und enthält einen Verweis auf ein geöffnetes **Verbindungs** Objekt.  
  
 Ein **Verbindungs** Objekt wird implizit erstellt, wenn das **Datensatz** -Objekt über eine URL geöffnet wird. Öffnen Sie den **Datensatz** mit einem vorhandenen geöffneten **Verbindungs** Objekt, indem Sie das **Verbindungs** Objekt dieser Eigenschaft zuweisen, oder verwenden Sie das **Verbindungs** Objekt als Parameter im [Open](./open-method-ado-record.md) -Methoden-aufrufen. Wenn der **Datensatz** von einem vorhandenen **Datensatz** oder [Recordset](./recordset-object-ado.md)aus geöffnet wird, wird er automatisch dem **Verbindungs** Objekt dieses **Datensatzes** oder des **Recordset** -Objekts zugeordnet.  
  
> [!NOTE]
>  URLs, die das http-Schema verwenden, rufen automatisch den [Microsoft OLE DB-Anbieter für die Internet Veröffentlichung auf](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Weitere Informationen finden Sie unter [absolute und relative URLs](../../guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>Gilt für  

:::row:::
    :::column:::
        [Command-Objekt (ADO)](./command-object-ado.md)  
    :::column-end:::
    :::column:::
        [Record-Objekt (ADO)](./record-object-ado.md)  
    :::column-end:::
    :::column:::
        [Recordset-Objekt (ADO)](./recordset-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für ActiveConnection, CommandText, CommandTimeout, CommandType, Size und Direction Properties (VB)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [Beispiel für ActiveConnection, CommandText, CommandTimeout, CommandType, Size und Direction Properties (VC + +)](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [Beispiel für ActiveConnection, CommandText, CommandTimeout, CommandType, Size und Direction Properties (JScript)](./activeconnection-commandtext-timeout-type-size-example-jscript.md)   
 [Verbindungs Objekt (ADO)](./connection-object-ado.md)   
 [ConnectionString-Eigenschaft (ADO)](./connectionstring-property-ado.md)