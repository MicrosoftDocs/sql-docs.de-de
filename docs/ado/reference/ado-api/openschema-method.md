---
description: OpenSchema-Methode
title: OpenSchema-Methode | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::OpenSchema
- Connection15::raw_OpenSchema
helpviewer_keywords:
- OpenSchema method [ADO]
ms.assetid: 850cf3ce-f18f-4e7c-8597-96c1dc504866
author: rothja
ms.author: jroth
ms.openlocfilehash: b764f9a28665d31667e9c502c7ad8bdf0b9bbcad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041310"
---
# <a name="openschema-method"></a>OpenSchema-Methode
Ruft Datenbankschema Informationen vom Anbieter ab.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
Set recordset = connection.OpenSchema(QueryType, Criteria, SchemaID)  
```  
  
## <a name="return-value"></a>Rückgabewert  
 Gibt ein [Recordset](./recordset-object-ado.md) -Objekt zurück, das Schema Informationen enthält. Das **Recordset** wird als Schreib geschützter, statischer Cursor geöffnet. Der *QueryType* bestimmt, welche Spalten im **Recordset** angezeigt werden.  
  
#### <a name="parameters"></a>Parameter  
 *QueryType*  
 Ein beliebiger [SchemaEnum](./schemaenum.md) -Wert, der den Typ der zu testenden Schema Abfrage darstellt.  
  
 *Kriterien*  
 Optional. Ein Array von Abfrage Einschränkungen für jede *QueryType* -Option, wie in [SchemaEnum](./schemaenum.md)aufgeführt.  
  
 *SchemaID*  
 Die GUID für eine Anbieter Schema Abfrage, die nicht durch die OLE DB Spezifikation definiert ist. Dieser Parameter ist erforderlich, wenn *QueryType* auf **adSchemaProviderSpecific** festgelegt ist. Andernfalls wird Sie nicht verwendet.  
  
## <a name="remarks"></a>Bemerkungen  
 Die **OpenSchema** -Methode gibt selbst beschreibende Informationen über die Datenquelle zurück, z. b. die Tabellen in der Datenquelle, die Spalten in den Tabellen und die unterstützten Datentypen.  
  
 Das *QueryType* -Argument ist eine GUID, die die zurückgegebenen Spalten (Schemas) angibt. Die OLE DB Spezifikation verfügt über eine vollständige Liste der Schemas.  
  
 Das *Kriterienargument* schränkt die Ergebnisse einer Schema Abfrage ein. *Kriterien* geben ein Array von Werten an, die in einer entsprechenden Teilmenge der Spalten (Einschränkungs Spalten) im resultierenden **Recordset** auftreten müssen.  
  
 Die Konstante **adSchemaProviderSpecific** wird für das *QueryType* -Argument verwendet, wenn der Anbieter seine eigenen nicht standardmäßigen Schema Abfragen außerhalb der zuvor aufgeführten definiert. Wenn diese Konstante verwendet wird, ist das *SchemaID* -Argument erforderlich, um die GUID der auszuführenden Schema Abfrage zu übergeben. Wenn *QueryType* auf **adSchemaProviderSpecific** festgelegt ist, aber *SchemaID* nicht angegeben wird, tritt ein Fehler auf.  
  
 Anbieter sind nicht erforderlich, um alle OLE DB Standardschema Abfragen zu unterstützen. Insbesondere werden für die OLE DB Spezifikation nur **adSchemaTables**, **adschemacolenumns** und **adschemaprovidertypes** benötigt. Es ist jedoch nicht erforderlich, dass der Anbieter die zuvor aufgeführten *kriterieneinschränkungen* für diese Schema Abfragen unterstützt.  
  
> [!NOTE]
>  **Verwendung von Remote Datendiensten** Die **OpenSchema** -Methode ist für ein Client seitiges [Verbindungs](./connection-object-ado.md) Objekt nicht verfügbar.  
  
> [!NOTE]
>  In Visual Basic können Spalten, die eine 4-Byte-Ganzzahl ohne Vorzeichen (DbType UI4) im **Recordset** aufweisen, die von der **OpenSchema** -Methode für das **Verbindungs** Objekt zurückgegeben wurde, nicht mit anderen Variablen verglichen werden. Weitere Informationen zu OLE DB-Datentypen finden Sie unter [Datentypen in OLE DB (OLE DB)](/previous-versions/windows/desktop/ms714931(v=vs.85)) und [Anhang A: Datentypen](/previous-versions/windows/desktop/ms723969(v=vs.85)) in der Microsoft OLE DB Programmer es Reference.  
  
> [!NOTE]
>  **Visual C/C++-Benutzer** Wenn keine Client seitigen Cursor verwendet werden, wird beim Abrufen des "ORDINAL_POSITION" eines Spalten Schemas in ADO eine Variante des Typs VT_R8 in MDAC 2,7, MDAC 2,8 und Windows Data Access Components (Windows DAC) 6,0 zurückgegeben, während der in MDAC 2,6 verwendete Typ VT_I4 ist. Für MDAC 2,6 geschriebene Programme, die nur nach einer Variante suchen, die vom Typ "VT_I4" zurückgegeben wird, erhalten eine NULL für jede Ordinalzahl, wenn Sie unter MDAC 2,7, MDAC 2,8 und Windows DAC 6,0 ohne Änderung ausgeführt wird. Diese Änderung wurde vorgenommen, weil der Datentyp, den OLE DB zurückgibt, DBTYPE_UI4 ist und im signierten VT_I4-Typ nicht genügend Platz für alle möglichen Werte verfügbar ist, ohne dass möglicherweise abgeschnitten werden und dadurch Daten verloren gehen.  
  
## <a name="applies-to"></a>Gilt für  
 [Connection-Objekt (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [OpenSchema-Methode (Beispiel) (VB)](./openschema-method-example-vb.md)   
 [OpenSchema-Methode (Beispiel) (VC + +)](./openschema-method-example-vc.md)   
 [Open-Methode (ADO-Verbindung)](./open-method-ado-connection.md)   
 [Open-Methode (ADO-Datensatz)](./open-method-ado-record.md)   
 [Open-Methode (ADO-Recordset)](./open-method-ado-recordset.md)   
 [Open-Methode (ADO-Stream)](./open-method-ado-stream.md)   
 [Anhang A: Anbieter](../../guide/appendixes/appendix-a-providers.md)