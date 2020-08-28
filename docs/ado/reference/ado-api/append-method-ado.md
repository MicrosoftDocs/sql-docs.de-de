---
description: Append-Methode (ADO)
title: Append-Methode (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- _DynaCollection::Append
helpviewer_keywords:
- Append method [ADO]
ms.assetid: f8a9bbed-ba9c-4698-945d-317ad22d2e92
author: rothja
ms.author: jroth
ms.openlocfilehash: 84969b95751726579bdc7d4a61aee311b95b6108
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88976081"
---
# <a name="append-method-ado"></a>Append-Methode (ADO)
Fügt ein Objekt an eine Auflistung an. Wenn es sich bei der Auflistung um [Felder](./fields-collection-ado.md)handelt, kann ein neues [Feld](./field-object.md) Objekt erstellt werden, bevor es an die Auflistung angefügt wird.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
collection.Append object  
fields.Append Name, Type, DefinedSize, Attrib, FieldValue  
```  
  
#### <a name="parameters"></a>Parameter  
 *Ausstellung*  
 Ein Auflistungs Objekt.  
  
 *fields*  
 Eine **Fields** -Auflistung.  
  
 *object*  
 Eine Objekt Variable, die das angefügte Objekt darstellt.  
  
 *Name*  
 Ein **Zeichen** folgen Wert, der den Namen des neuen **Feld** Objekts enthält und nicht denselben Namen wie ein beliebiges anderes Objekt in *Feldern*aufweisen darf.  
  
 *Type*  
 Ein [datatyetenum](./datatypeenum.md) -Wert, dessen Standardwert " **adempty**" ist, der den Datentyp des neuen Felds angibt. Die folgenden Datentypen werden nicht von ADO unterstützt und sollten nicht verwendet werden, wenn neue Felder an ein [Recordset-Objekt (ADO)](./recordset-object-ado.md)angehängt werden: **adidispatch**, **adiunknown**, **advariant**.  
  
 *DefinedSize*  
 Optional. Ein **Long** -Wert, der die definierte Größe (in Zeichen oder Bytes) des neuen Felds darstellt. Der Standardwert für diesen Parameter wird vom- *Typ*abgeleitet. Felder, die eine *DefinedSize* aufweisen, die größer als 255 Bytes ist, werden als Spalten variabler Länge behandelt. Der Standardwert für *DefinedSize* ist nicht angegeben.  
  
 *Attrib*  
 Optional. Ein [FieldAttributeEnum](./fieldattributeenum.md) -Wert, dessen Standardwert **adFldDefault**ist und Attribute für das neue Feld angibt. Wenn dieser Wert nicht angegeben wird, enthält das Feld Attribute, die vom *Typ*abgeleitet werden.  
  
 *Feldwert*  
 Optional. Eine **Variante** , die den Wert für das neue Feld darstellt. Wenn kein Wert angegeben wird, wird dem Feld ein NULL-Wert angefügt.  
  
## <a name="remarks"></a>Bemerkungen  
  
## <a name="parameters-collection"></a>Parameters-Auflistung  
 Sie müssen die [Type](./type-property-ado.md) -Eigenschaft eines [Parameter](./parameter-object.md) Objekts festlegen, bevor es an die [Parameter](./parameters-collection-ado.md) Auflistung angehängt wird. Wenn Sie einen Datentyp mit variabler Länge auswählen, müssen Sie auch die [size](./size-property-ado-parameter.md) -Eigenschaft auf einen Wert größer 0 (null) festlegen.  
  
 Durch das Beschreiben von Parametern selbst werden Aufrufe des Anbieters minimiert und somit die Leistung verbessert, wenn Sie gespeicherte Prozeduren oder parametrisierte Abfragen verwenden. Sie müssen jedoch die Eigenschaften der Parameter kennen, die der gespeicherten Prozedur oder der parametrisierten Abfrage zugeordnet sind, die aufgerufen werden soll.  
  
 Verwenden Sie die Methode " [kreateparameter](./createparameter-method-ado.md) ", um **Parameter** Objekte mit den entsprechenden Eigenschaften Einstellungen zu erstellen, und fügen Sie Sie mithilfe der **Append** -Methode der [Parameter](./parameters-collection-ado.md) Auflistung hinzu. Dies ermöglicht es Ihnen, Parameterwerte festzulegen und zurückzugeben, ohne den Anbieter für die Parameterinformationen aufrufen zu müssen. Wenn Sie in einen Anbieter schreiben, der keine Parameterinformationen bereitstellt, müssen Sie diese Methode verwenden, um die **Parameter** Auflistung manuell aufzufüllen, um Parameter zu verwenden.  
  
## <a name="fields-collection"></a>Fields-Auflistung  
 Der *FieldValue* -Parameter ist nur gültig, wenn ein **Feld** Objekt zu einem [Datensatz](./record-object-ado.md) -Objekt hinzugefügt wird, nicht zu einem **Recordset** -Objekt. Mit einem **Datensatz** -Objekt können Sie Felder anfügen und Werte gleichzeitig angeben. Mit einem **Recordset** -Objekt müssen Sie Felder erstellen, während das **Recordset** geschlossen ist, und dann das **Recordset** öffnen und den Feldern Werte zuweisen.  
  
> [!NOTE]
>  Bei neuen **Feld** Objekten, die an die **Fields** -Auflistung eines **Datensatz** -Objekts angehängt wurden, muss die [value](./value-property-ado.md) -Eigenschaft festgelegt werden, bevor andere **Feld** Eigenschaften angegeben werden können. Zuerst muss ein bestimmter Wert für die **value** -Eigenschaft zugewiesen und in der **Fields** -Auflistung mit dem Namen [aktualisiert](./update-method.md) werden. Anschließend können Sie auf andere Eigenschaften, z. b. den [Typ](./type-property-ado.md) oder die [Attribute](./attributes-property-ado.md) , zugreifen. **Feld** Objekte der folgenden Datentypen (**datatyetenum**) können nicht an die **Fields** -Auflistung angehängt werden und führen zu einem Fehler: **adarray**, **adChapter**, **adempty**, **adpropvariant**und **aduserdefined**. Außerdem werden die folgenden Datentypen nicht von ADO unterstützt: **adidispatch**, **adiunknown**und **adivariant**. Für diese Typen tritt kein Fehler auf, wenn Sie angefügt werden, die Verwendung kann jedoch zu unvorhersehbaren Ergebnissen, einschließlich Speicher Verlusten, führen.  
  
## <a name="recordset"></a>Recordset  
 Wenn Sie die Eigenschaft " [Cursor Location](./cursorlocation-property-ado.md) " nicht vor dem Aufrufen der **Append** -Methode festlegen, wird " **Cursor Location** " automatisch auf **adUseClient** (ein [Cursor locationenum](./cursorlocationenum.md) -Wert) festgelegt, wenn die [Open](./open-method-ado-recordset.md) -Methode des [Recordset](./recordset-object-ado.md) -Objekts aufgerufen wird.  
  
 Ein Laufzeitfehler tritt auf, wenn die **Append** -Methode für die **Fields** -Auflistung eines geöffneten **Recordsets**oder für ein **Recordset** aufgerufen wird, in dem die [ActiveConnection](./activeconnection-property-ado.md) -Eigenschaft festgelegt wurde. Sie können nur Felder an ein **Recordset** anfügen, das nicht geöffnet ist und noch keine Verbindung mit einer Datenquelle hergestellt wurde. Dies ist in der Regel der Fall, wenn ein **Recordset** -Objekt mit der Methode " [kreaterecordset](../rds-api/createrecordset-method-rds.md) " oder einer Objektvariablen zugewiesen wird.  
  
## <a name="record"></a>Datensatz  
 Ein Laufzeitfehler tritt nicht auf, wenn die **Append** -Methode für die **Fields** -Auflistung eines geöffneten **Datensatzes**aufgerufen wird. Das neue Feld wird der **Fields** -Auflistung des **Datensatz** -Objekts hinzugefügt. Wenn der **Datensatz** von einem **Recordset**abgeleitet wurde, wird das neue Feld nicht in der **Fields** -Auflistung des **Recordset** -Objekts angezeigt.  
  
 Ein nicht vorhandenes Feld kann erstellt und an die **Fields** -Auflistung angehängt werden, indem dem Field-Objekt ein Wert zugewiesen wird, als wäre es bereits in der Auflistung vorhanden. Die Zuweisung löst die automatische Erstellung und das Anhängen des **Feld** Objekts aus, und anschließend wird die Zuweisung abgeschlossen.  
  
 Nachdem Sie ein **Feld** an die **Fields** -Auflistung eines **Datensatz** -Objekts angehängt haben, müssen Sie die **Update** -Methode der **Fields** -Auflistung abrufen, um die Änderung zu speichern.  
  
## <a name="applies-to"></a>Gilt für  
  
- [Fields-Collection (ADO)](./fields-collection-ado.md)  
- [Parameters-Collection (ADO)](./parameters-collection-ado.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für Append und die Methode "Anfüge Parameter" (VB)](./append-and-createparameter-methods-example-vb.md)   
 [Beispiel für Append und die Methode "append Parameter" (VC + +)](./append-and-createparameter-methods-example-vc.md)   
 [Kreateparameter-Methode (ADO)](./createparameter-method-ado.md)   
 [Delete-Methode (ADO Fields-Auflistung)](./delete-method-ado-fields-collection.md)   
 [Delete-Methode (ADO Parameters-Sammlung)](./delete-method-ado-parameters-collection.md)   
 [Delete-Methode (ADO-Recordset)](./delete-method-ado-recordset.md)   
 [Update-Methode](./update-method.md)