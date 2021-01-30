---
description: DataTypeEnum
title: Datatypum | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- DataTypeEnum
helpviewer_keywords:
- DataTypeEnum enumeration [ADO]
ms.assetid: 2c57eca6-9336-4b06-ba10-9fef5926b1d0
author: rothja
ms.author: jroth
ms.openlocfilehash: 700f7aa4053a1e99bcae5cdabe01a5fc6c440258
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171317"
---
# <a name="datatypeenum"></a>DataTypeEnum
Gibt den Datentyp eines [Felds](../../../ado/reference/ado-api/field-object.md), eines [Parameters](../../../ado/reference/ado-api/parameter-object.md)oder einer [Eigenschaft](../../../ado/reference/ado-api/property-object-ado.md)an. Der entsprechende OLE DB Typindikator wird in Klammern in der Beschreibungs Spalte der folgenden Tabelle angezeigt.  
  
|Konstante|Wert|BESCHREIBUNG|  
|--------------|-----------|-----------------|  
|**Adarray**|0x2000|Ein Flagwert, der immer mit einer anderen Datentyp Konstante kombiniert wird und ein Array des anderen Datentyps angibt. Gilt nicht für ADOX.|  
|**adBigInt**|20|Gibt eine 8-Byte-Ganzzahl mit Vorzeichen (DBTYPE_I8) an.|  
|**adBinary**|128|Gibt einen binären Wert an (DBTYPE_BYTES).|  
|**adBoolean**|11|Gibt einen **booleschen** Wert an (DBTYPE_BOOL).|  
|**adBSTR**|8|Gibt eine NULL-terminierte Zeichenfolge (Unicode) (DBTYPE_BSTR) an.|  
|**adChapter**|136|Gibt einen vier Byte-Kapitel Wert an, der Zeilen in einem untergeordneten Rowset (DBTYPE_HCHAPTER) identifiziert.|  
|**adChar**|129|Gibt einen Zeichen folgen Wert (DBTYPE_STR) an.|  
|**adCurrency**|6|Gibt einen Währungswert an (DBTYPE_CY). Währung ist eine Festkomma Zahl mit vier Ziffern auf der rechten Seite des Dezimal Trennzeichens. Sie wird in einer 8-Byte-Ganzzahl mit Vorzeichen gespeichert, die durch 10.000|  
|**adDate**|7|Gibt einen Datumswert an (DBTYPE_DATE). Ein Datum wird als Double gespeichert, wobei der gesamte Teil die Anzahl von Tagen seit dem 30. Dezember 1899 und der Bruchteil eines Tages ist.|  
|**adDBDate**|133|Gibt einen Datumswert an (YYYYMMDD) (DBTYPE_DBDATE).|  
|**adDBTime**|134|Gibt einen Uhrzeitwert (HHMMSS) an (DBTYPE_DBTIME).|  
|**adDBTimestamp**|135|Gibt einen Datums-/Uhrzeitstempel an (YYYYMMDDHHMMSS plus einen Bruchteil in Milliardstel) (DBTYPE_DBTIMESTAMP).|  
|**adDecimal**|14|Gibt einen genauen numerischen Wert mit fester Genauigkeit und Dezimalstelle (DBTYPE_DECIMAL) an.|  
|**adDouble**|5|Gibt einen Gleit Komma Wert mit doppelter Genauigkeit (DBTYPE_R8) an.|  
|**adempty**|0|Gibt keinen Wert an (DBTYPE_EMPTY).|  
|**adError**|10|Gibt einen 32-Bit-Fehlercode (DBTYPE_ERROR) an.|  
|**adFileTime**|64|Gibt einen 64-Bit-Wert an, der die Anzahl von 100-Nanosekunden-Intervallen seit dem 1. Januar 1601 (DBTYPE_FILETIME) darstellt.|  
|**adguid**|72|Gibt eine Globally Unique Identifier (GUID) an (DBTYPE_GUID).|  
|**adIDispatch**|9|Gibt einen Zeiger auf eine **IDispatch** -Schnittstelle in einem COM-Objekt an (DBTYPE_IDISPATCH).<br /><br /> **Hinweis** Dieser Datentyp wird derzeit nicht von ADO unterstützt. Die Verwendung kann zu unvorhersehbaren Ergebnissen führen.|  
|**adInteger**|3|Gibt eine 4-Byte-Ganzzahl mit Vorzeichen (DBTYPE_I4) an.|  
|**adIUnknown**|13|Gibt einen Zeiger auf eine **IUnknown** -Schnittstelle in einem COM-Objekt an (DBTYPE_IUNKNOWN).<br /><br /> **Hinweis** Dieser Datentyp wird derzeit nicht von ADO unterstützt. Die Verwendung kann zu unvorhersehbaren Ergebnissen führen.|  
|**adLongVarBinary**|205|Gibt einen langen Binärwert an.|  
|**adLongVarChar**|201|Gibt einen langen Zeichen folgen Wert an.|  
|**adLongVarWChar**|203|Gibt einen langen, auf NULL endenden Unicode-Zeichen folgen Wert an.|  
|**adNumeric**|131|Gibt einen genauen numerischen Wert mit fester Genauigkeit und Dezimalstelle (DBTYPE_NUMERIC) an.|  
|**adPropVariant**|138|Gibt eine automatisierungspropvariant (DBTYPE_PROP_VARIANT) an.|  
|**adSingle**|4|Gibt einen Gleit Komma Wert mit einfacher Genauigkeit an (DBTYPE_R4).|  
|**adSmallInt**|2|Gibt eine 2-Byte-Ganzzahl mit Vorzeichen (DBTYPE_I2) an.|  
|**adTinyInt**|16|Gibt eine 1-Byte-Ganzzahl mit Vorzeichen (DBTYPE_I1) an.|  
|**adUnsignedBigInt**|21|Gibt eine 8-Byte-Ganzzahl ohne Vorzeichen an (DBTYPE_UI8).|  
|**adUnsignedInt**|19|Gibt eine 4-Byte-Ganzzahl ohne Vorzeichen an (DBTYPE_UI4).|  
|**adUnsignedSmallInt**|18|Gibt eine 2-Byte-Ganzzahl ohne Vorzeichen an (DBTYPE_UI2).|  
|**adUnsignedTinyInt**|17|Gibt eine 1-Byte-Ganzzahl ohne Vorzeichen an (DBTYPE_UI1).|  
|**adUserDefined**|132|Gibt eine benutzerdefinierte Variable (DBTYPE_UDT) an.|  
|**adVarBinary**|204|Gibt einen binären Wert an.|  
|**adVarChar**|200|Gibt einen Zeichen folgen Wert an.|  
|**adVariant**|12|Gibt eine Automatisierungs **Variante** an (DBTYPE_VARIANT).<br /><br /> **Hinweis** Dieser Datentyp wird derzeit nicht von ADO unterstützt. Die Verwendung kann zu unvorhersehbaren Ergebnissen führen.|  
|**adVarNumeric**|139|Gibt einen numerischen Wert an.|  
|**adVarWChar**|202|Gibt eine NULL-terminierte Unicode-Zeichenfolge an.|  
|**adWChar**|130|Gibt eine NULL-terminierte Unicode-Zeichenfolge (DBTYPE_WSTR) an.|  
  
## <a name="adowfc-equivalent"></a>ADO/WFC-Entsprechung  
 Paket: **com. ms. wfc. Data**  
  
|Konstante|  
|--------------|  
|Adoumums. DataType. Array|  
|Adoumums. DataType. bigint|  
|Adoumums. DataType. Binary|  
|Adoerums. DataType. Boolean|  
|Adoerums. DataType. BSTR|  
|Adoumums. DataType. Chapter|  
|Adoumums. DataType. Char|  
|Adoumums. DataType. Currency|  
|Adoumums. DataType. Date|  
|Adoumums. DataType. DBDate|  
|Adoumums. DataType. dbtime|  
|Adoumums. DataType. DBTIMESTAMP|  
|Adotenums. DataType. Decimal|  
|Adoerums. DataType. Double|  
|Adoumums. DataType. Empty|  
|Adoerums. DataType. Error|  
|Adoumums. DataType. FILETIME|  
|Adoerums. DataType. GUID|  
|Adoumums. DataType. IDispatch|  
|Adoumums. DataType. Integer|  
|Adoumums. DataType. IUnknown|  
|Adoumums. DataType. LONGVARBINARY|  
|Adoumums. DataType. LONGVARCHAR|  
|Adoumums. DataType. longvarwchar|  
|Adoumums. DataType. numeric|  
|Adoumums. DataType. PROPVARIANT|  
|Adoumums. DataType. Single|  
|Adoteums. DataType. smallint|  
|AdoEnums. DataType. tinyint|  
|Adoumums. DataType. UnsignedBigInt|  
|Adoumums. DataType. unsignetdint|  
|Adoteums. DataType. UnsignedSmallInt|  
|AdoEnums. DataType. UnsignedTinyInt|  
|Adoumums. DataType. UserDefined|  
|Adoumums. DataType. varbinary|  
|Adoumums. DataType. varchar|  
|Adoumums. DataType. Variant|  
|Adoumums. DataType. VarNumeric|  
|Adoumums. DataType. VarWChar|  
|Adoumums. DataType. WCHAR|  
  
## <a name="applies-to"></a>Gilt für  

:::row:::
    :::column:::
        [Append-Methode (ADO)](../../../ado/reference/ado-api/append-method-ado.md)  
        [CreateParameter-Methode (ADO)](../../../ado/reference/ado-api/createparameter-method-ado.md)  
    :::column-end:::
    :::column:::
        [CreateRecordset-Methode (RDS)](../../../ado/reference/rds-api/createrecordset-method-rds.md)  
        [Type-Eigenschaft (ADO)](../../../ado/reference/ado-api/type-property-ado.md)  
    :::column-end:::
:::row-end:::
