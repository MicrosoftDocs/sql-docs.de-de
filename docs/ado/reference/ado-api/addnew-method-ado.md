---
description: AddNew-Methode (ADO)
title: AddNew-Methode (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::AddNew
- Recordset15::raw_AddNew
helpviewer_keywords:
- AddNew method [ADO]
ms.assetid: a9f54be9-5763-45d0-a6eb-09981b03bc08
author: rothja
ms.author: jroth
ms.openlocfilehash: 1dd85acf2d16ad998294bae5ee6490d227e8ede2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031452"
---
# <a name="addnew-method-ado"></a>AddNew-Methode (ADO)
Erstellt einen neuen Datensatz für ein Aktualisier bares [Recordset](./recordset-object-ado.md) -Objekt.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
recordset.AddNew FieldList, Values  
```  
  
#### <a name="parameters"></a>Parameter  
 *Recordset*  
 Ein **Recordset** -Objekt.  
  
 *FieldList*  
 Optional. Ein einzelner Name oder ein Array von Namen oder Ordinalpositionen der Felder im neuen Datensatz.  
  
 *Werte*  
 Optional. Ein einzelner Wert oder ein Array von Werten für die Felder im neuen Datensatz. Wenn *FieldList* ein Array ist, müssen die *Werte* auch ein Array mit derselben Anzahl von Membern sein. Andernfalls tritt ein Fehler auf. Die Reihenfolge der Feldnamen muss der Reihenfolge der Feldwerte in jedem Array entsprechen.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die **AddNew** -Methode, um einen neuen Datensatz zu erstellen und zu initialisieren. Verwenden Sie die Methode [unterstützt](./supports-method.md) mit **adAddNew** (ein [Cursor](./cursoroptionenum.md) Wert), um zu überprüfen, ob dem aktuellen **Recordset** -Objektdaten Sätze hinzugefügt werden können.  
  
 Nachdem Sie die **AddNew** -Methode aufgerufen haben, wird der neue Datensatz zum aktuellen Datensatz und bleibt nach dem Abrufen der [Update](./update-method.md) -Methode aktuell. Da der neue Datensatz an das **Recordset** angehängt wird, wird ein **MoveNext** -Befehl nach dem Update nach dem Ende des **Recordsets** verschoben, sodass **EOF** true ist. Wenn das **Recordset** -Objekt keine Lesezeichen unterstützt, können Sie möglicherweise nicht mehr auf den neuen Datensatz zugreifen, wenn Sie zu einem anderen Datensatz wechseln. Abhängig vom Cursortyp müssen Sie möglicherweise die [Requery](./requery-method.md) -Methode aufzurufen, um den neuen Datensatz zugänglich zu machen.  
  
 Wenn Sie beim Bearbeiten des aktuellen Datensatzes oder beim Hinzufügen eines neuen Datensatzes **AddNew** aufrufen, ruft ADO die **Update** -Methode auf, um alle Änderungen zu speichern und dann den neuen Datensatz zu erstellen.  
  
 Das Verhalten der **AddNew** -Methode hängt vom Aktualisierungs Modus des **Recordset** -Objekts und davon ab, ob die Argumente *FieldList* und *Values* übergeben werden.  
  
 Im *sofortigen Update Modus* (in dem der Anbieter Änderungen in die zugrunde liegende Datenquelle schreibt, nachdem Sie die **Update** -Methode aufgerufen haben), wird durch Aufrufen der **AddNew** -Methode ohne Argumente die [EditMode](./editmode-property.md) -Eigenschaft auf **adEditAdd** (ein [EditModeEnum](./editmodeenum.md) -Wert) festgelegt. Der Anbieter speichert alle Feldwert Änderungen lokal zwischen. Durch Aufrufen der **Update** -Methode wird der neue Datensatz in der Datenbank bereitgestellt, und die **EditMode** -Eigenschaft wird auf " **adEditNone** " ( **EditModeEnum** -Wert) zurückgesetzt. Wenn Sie die Argumente *FieldList* und *Values* übergeben, sendet ADO den neuen Datensatz sofort an die Datenbank (kein **Update** -Befehl erforderlich); der **EditMode** -Eigenschafts Wert ändert sich nicht (**adEditNone**).  
  
 Im *Batch Aktualisierungs Modus* (in dem der Anbieter mehrere Änderungen zwischenspeichert und Sie nur dann in die zugrunde liegende Datenquelle schreibt, wenn Sie die [UpdateBatch](./updatebatch-method.md) -Methode aufrufen), wird durch Aufrufen der **AddNew** -Methode ohne Argumente die **EditMode** -Eigenschaft auf **adEditAdd** festgelegt. Der Anbieter speichert alle Feldwert Änderungen lokal zwischen. Durch den Aufruf der **Update** -Methode wird der neue Datensatz dem aktuellen **Recordset** hinzugefügt, aber der Anbieter sendet die Änderungen nicht an die zugrunde liegende Datenbank oder setzt **EditMode** auf **adEditNone** zurück, bis Sie die **UpdateBatch** -Methode aufrufen. Wenn Sie die Argumente *FieldList* und *Values* übergeben, sendet ADO den neuen Datensatz zum Speichern in einem Cache an den Anbieter und legt **EditMode** auf **adEditAdd**; fest. Sie müssen die **UpdateBatch** -Methode aufrufen, um den neuen Datensatz in der zugrunde liegenden Datenbank zu veröffentlichen.  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird gezeigt, wie die AddNew-Methode mit der Liste der Felder und Werte verwendet wird, um zu erfahren, wie Sie die Feldliste und Werteliste als Arrays einschließen.  
  
```  
create table aa1 (intf int, charf char(10))  
insert into aa1 values (2, 'aa')  
  
Dim cn As New adodb.Connection  
Dim rs As New adodb.Recordset  
Dim cmd As New adodb.Command  
  
cn.ConnectionString = "Provider=SQLOLEDB;Data Source=alexverb2;uid=sa;pwd=foo$bar00;"  
  
cn.Open  
rs.Open "select * from xxx..aa1", cn, adOpenKeyset, adLockOptimistic  
  
Dim fieldsArray(1) As Variant  
fieldsArray(0) = "intf"  
fieldsArray(1) = "charf"  
Dim values(1) As Variant  
values(0) = 4  
values(1) = "as"  
rs.AddNew fieldsArray, values  
rs.Update  
```  
  
## <a name="applies-to"></a>Gilt für  
 [Recordset-Objekt (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [AddNew-Methode (Beispiel) (VB)](./addnew-method-example-vb.md)   
 [AddNew-Methode (Beispiel) (VBScript)](./addnew-method-example-vbscript.md)   
 [AddNew-Methode (Beispiel) (VC + +)](./addnew-method-example-vc.md)   
 [CancelUpdate-Methode (ADO)](./cancelupdate-method-ado.md)   
 [EditMode-Eigenschaft](./editmode-property.md)   
 [Requery-Methode](./requery-method.md)   
 [Unterstützt Methode](./supports-method.md)   
 [Update-Methode](./update-method.md)   
 [UpdateBatch-Methode](./updatebatch-method.md)