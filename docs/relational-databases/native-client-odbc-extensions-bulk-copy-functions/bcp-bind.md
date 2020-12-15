---
description: bcp_bind
title: bcp_bind | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_bind
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_bind function
ms.assetid: 6e335a5c-64b2-4bcf-a88f-35dc9393f329
author: markingmyname
ms.author: maghan
ms.custom: ''
ms.reviewer: ''
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: edff3154a4385ee87bf7686cf5e2954e026e495f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473661"
---
# <a name="bcp_bind"></a>bcp_bind

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Bindet Daten einer Programmvariablen an eine Tabellenspalte im Hinblick auf einen Massenkopiervorgang in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="syntax"></a>Syntax

```  
  
RETCODE bcp_bind (  
        HDBC hdbc,
        LPCBYTE pData,  
        INT cbIndicator,  
        DBINT cbData,  
        LPCBYTE pTerm,  
        INT cbTerm,  
        INT eDataType,  
        INT idxServerCol);  
```  
  
## <a name="arguments"></a>Argumente

 *hdbc*  
 Das für den Massenkopiervorgang aktivierte ODBC-Verbindungshandle.  
  
 *pData*  
 Ein Zeiger auf die kopierten Daten. Wenn *eDataType* SQLTEXT, SQLNTEXT, SQLXML, SQLUDT, SQLCHARACTER, SQLVARCHAR, SQLVARBINARY, SqlBinary, SQLNCHAR oder SQLIMAGE ist, kann *pData* NULL sein. Ein *pData* -Wert von NULL gibt an, dass lange Datenwerte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe von [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md)in Blöcken an gesendet werden. Der Benutzer sollte *pData* nur auf NULL festlegen, wenn die Spalte, die dem Benutzer gebundenen Feld entspricht, eine BLOB-Spalte ist, andernfalls **bcp_bind** fehlschlagen.  
  
 Falls Indikatoren in den Daten vorhanden sind, stehen sie im Speicher direkt vor den Daten. Der *pData* -Parameter verweist in diesem Fall auf die Indikator Variable, und die Breite des Indikators, der *cbIndicator* -Parameter, wird vom Massen Kopiervorgang verwendet, um die Benutzerdaten ordnungsgemäß zu adressieren.  
  
 *cbIndicator*  
 Die Länge eines Längen- oder NULL-Indikators für die Spaltendaten in Byte. Gültige Indikatorlängenwerte sind 0 (wenn kein Indikator verwendet wird), 1, 2, 4 oder 8. Indikatoren stehen im Speicher direkt vor allen Daten. Beispielsweise könnte die folgende Strukturtypdefinition verwendet werden, um für einen Massenkopiervorgang Ganzzahlwerte in eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Tabelle einzufügen:  
  
```
typedef struct tagBCPBOUNDINT  
    {  
    int iIndicator;  
    int Value;  
    } BCPBOUNDINT;  
```  
  
 Im Beispiel Fall wird der *pData* -Parameter auf die Adresse einer deklarierten Instanz der Struktur festgelegt, die Adresse des bcpboundint- *iIndicator* -Strukturmembers. Der *cbIndicator* -Parameter wird auf die Größe einer ganzen Zahl (sizeof (int)) festgelegt, und der *cbData* -Parameter würde erneut auf die Größe einer ganzen Zahl (sizeof (int)) festgelegt werden. Um einen Massen Kopiervorgang für eine Zeile auf den Server mit einem NULL-Wert für die gebundene Spalte durchzusetzen, sollte der Wert des *iIndicator* -Members der Instanz auf SQL_NULL_DATA festgelegt werden.  
  
 *cbData*  
 Die Anzahl der Datenbytes in der Programmvariablen ohne die Länge eventuell vorhandener Längenindikatoren, NULL-Indikatoren oder Abschlusszeichen.  
  
 Wenn *cbData* auf SQL_NULL_DATA festgelegt wird, bedeutet dies, dass alle auf den Server kopierten Zeilen einen NULL-Wert für die Spalte enthalten.  
  
 Wenn *cbData* auf SQL_VARLEN_DATA festgelegt wird, wird angegeben, dass das System ein Zeichen folgen Abschluss Zeichen oder eine andere Methode verwendet, um die Länge der kopierten Daten zu bestimmen.  
  
 Bei Datentypen mit fester Länge wie ganzen Zahlen gibt der Datentyp dem System die Länge der Daten an. Daher können *cbData* für Datentypen mit fester Länge sicher SQL_VARLEN_DATA oder die Länge der Daten sein.  
  
 Für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Zeichen-und Binär Datentypen können *cbData* SQL_VARLEN_DATA, SQL_NULL_DATA, ein positiver Wert oder 0 sein. Wenn *cbData* SQL_VARLEN_DATA ist, verwendet das System entweder einen Längen-/NULL-Indikator (sofern vorhanden) oder eine Abschluss Zeichen Sequenz, um die Länge der Daten zu bestimmen. Wenn beide Indikatoren bereitgestellt werden, verwendet das System beim Massenkopieren den Wert, der zu der kleineren zu kopierenden Datenmenge führt. Wenn *cbData* SQL_VARLEN_DATA ist, der Datentyp der Spalte ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Zeichen-oder Binärtyp ist und weder ein Längen Indikator noch eine Abschluss Zeichen Sequenz angegeben ist, gibt das System eine Fehlermeldung zurück.  
  
 Wenn *cbData* 0 oder ein positiver Wert ist, verwendet das System *cbData* als Daten Länge. Wenn jedoch zusätzlich zu einem positiven *cbData* -Wert ein Längen Indikator oder eine Abschluss Zeichen Sequenz angegeben ist, bestimmt das System die Daten Länge mithilfe der Methode, die zu der geringsten zu kopierenden Datenmenge führt.  
  
 Der *cbData* -Parameterwert stellt die Anzahl der Daten Bytes dar. Wenn Zeichendaten durch Unicode-breit Zeichen dargestellt werden, stellt ein positiver *cbData* -Parameterwert die Anzahl der Zeichen multipliziert mit der Größe in Bytes jedes Zeichens dar.  
  
 *pTerm*  
 Ein Zeiger auf das Bytemuster, falls vorhanden, das das Ende dieser Programmvariablen markiert. Beispielsweise weisen ANSI- und MBCS-C-Zeichenfolgen normalerweise ein 1-Byte-Abschlusszeichen (\0) auf.  
  
 Wenn kein Abschluss Zeichen für die Variable vorhanden ist, legen Sie *pterm* auf NULL fest.  
  
 Sie können eine leere Zeichenfolge ("") verwenden, um das C-NULL-Abschlusszeichen als Programmvariablen-Abschlusszeichen festzulegen. Da die NULL-terminierte leere Zeichenfolge ein einzelnes Byte (das Abschluss Zeichen selbst) bildet, legen Sie *cbTerm* auf 1 fest. Um z. b. anzugeben, dass die Zeichenfolge in *szName* NULL-terminiert ist und das Abschluss Zeichen verwendet werden soll, um die Länge anzugeben:  
  
```
bcp_bind(hdbc, szName, 0,  
   SQL_VARLEN_DATA, "", 1,  
   SQLCHARACTER, 2)  
```

 Eine nicht beendete Form dieses Beispiels könnte darauf hindeuten, dass 15 Zeichen aus der *szName* -Variablen in die zweite Spalte der gebundenen Tabelle kopiert werden:  

```
bcp_bind(hdbc, szName, 0, 15,
   NULL, 0, SQLCHARACTER, 2)  
```

 Die API für das Massenkopieren führt nach Bedarf eine Zeichenkonvertierung von Unicode in MBCS aus. Stellen Sie sicher, dass sowohl die Bytezeichenfolge des Abschlusszeichens als auch die Länge der Bytezeichenfolge richtig festgelegt sind. Um beispielsweise anzugeben, dass die Zeichenfolge in *szName* eine Unicode-Zeichenfolge mit breit Zeichen ist, die durch den Unicode-Wert des NULL-Abschluss Zeichens beendet wird  
  
```  
bcp_bind(hdbc, szName, 0,
   SQL_VARLEN_DATA, L"",  
   sizeof(WCHAR), SQLNCHAR, 2)  
```  
  
 Wenn die gebundene [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Spalte breit Zeichen ist, wird keine Konvertierung auf [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md)ausgeführt. Wenn die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Spalte vom Typ MBCS-Zeichen ist, wird eine Konvertierung von Doppelbytezeichen in Multibytezeichen durchgeführt, wenn die Daten an [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gesendet werden.  
  
 *cbTerm*  
 Anzahl von Bytes im Abschlusszeichen für die Programmvariable, falls vorhanden. Wenn kein Abschluss Zeichen für die Variable vorhanden ist, legen Sie *cbTerm* auf 0 fest.  

*eDataType* Der C-Datentyp der Programmvariablen. Die Daten in der Programmvariablen werden in den Typ der Datenbankspalte konvertiert. Wenn dieser Parameter 0 ist, wird keine Konvertierung ausgeführt.  

Der *eDataType* -Parameter wird durch die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datentyp Token in sqlncli. h, nicht die ODBC-C-Datentyp Enumeratoren, aufgelistet. Beispielsweise können Sie eine 2-Byte-Ganzzahl, ODBC-Typ SQL_C_SHORT, angeben, indem Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-spezifischen Typ SQLINT2 verwenden.  

[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] in wurde die Unterstützung für die Datentyp Token SQLXML und SQLUDT im **_eDataType_** -Parameter eingeführt.  

Die folgende Tabelle führt gültige enumerierte Datentypen und die entsprechenden ODBC-C-Datentypen auf.

|eDataType|C-Typ|
|-----------------------|------------|  
|SQLTEXT|char *|  
|SQLNTEXT|wchar_t *|  
|SQLCHARACTER|char *|  
|SQLBIGCHAR|char *|  
|SQLVARCHAR|char *|  
|SQLBIGVARCHAR|char *|  
|SQLNCHAR|wchar_t *|  
|SQLNVARCHAR|wchar_t *|  
|SQLBINARY|unsigned char *|  
|SQLBIGBINARY|unsigned char *|  
|SQLVARBINARY|unsigned char *|  
|SQLBIGVARBINARY|unsigned char *|  
|SQLBIT|char|  
|SQLBITN|char|  
|SQLINT1|char|  
|SQLINT2|short int|  
|SQLINT4|INT|  
|SQLINT8|_int64|  
|SQLINTN|*cbIndicator*<br /> 1: SQLINT1<br /> 2: SQLINT2<br /> 4: SQLINT4<br /> 8: SQLINT8|  
|SQLFLT4|float|  
|SQLFLT8|float|  
|SQLFLTN|*cbIndicator*<br /> 4: SQLFLT4<br /> 8: SQLFLT8|  
|SQLDECIMALN|SQL_NUMERIC_STRUCT|  
|SQLNUMERICN|SQL_NUMERIC_STRUCT|  
|SQLMONEY|DBMONEY|  
|SQLMONEY4|DBMONEY4|  
|SQLMONEYN|*cbIndicator*<br /> 4: SQLMONEY4<br /> 8: SQLMONEY|  
|SQLTIMEN|SQL_SS_TIME2_STRUCT|  
|SQLDATEN|SQL_DATE_STRUCT|  
|SQLDATETIM4|DBDATETIM4|  
|SQLDATETIME|DBDATETIME|  
|SQLDATETIMN|*cbIndicator*<br /> 4: SQLDATETIM4<br /> 8: SQLDATETIME|  
|SQLDATETIME2N|SQL_TIMESTAMP_STRUCT|  
|SQLDATETIMEOFFSETN|SQL_SS_TIMESTAMPOFFSET_STRUCT|  
|SQLIMAGE|unsigned char *|  
|SQLUDT|unsigned char *|  
|SQLUNIQUEID|SQLGUID|  
|SQLVARIANT|*Jeder Datentyp außer:*<br />-   text<br />-   ntext<br />-   image<br />-   varchar(max)<br />-   varbinary(max)<br />-   nvarchar(max)<br />-   xml<br />-   timestamp|  
|SQLXML|*Unterstützte C-Datentypen:*<br />-   char*<br />-   wchar_t *<br />-   unsigned char *|  

*idxServerCol* Die Ordinalposition der Spalte in der Datenbanktabelle, in die die Daten kopiert werden. Die erste Spalte einer Tabelle ist die Spalte 1. Die Ordnungsposition einer Spalte wird von [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md)ausgegeben.  
  
## <a name="returns"></a>Gibt zurück

 SUCCEED oder FAIL.

## <a name="remarks"></a>Hinweise

Verwenden Sie **bcp_bind** für eine schnelle, effiziente Methode zum Kopieren von Daten aus einer Programmvariablen in eine Tabelle in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

Rufen Sie [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md) auf, bevor Sie diese oder eine andere Massen Kopierfunktion aufrufen. Durch Aufrufen von **bcp_init** wird die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Ziel Tabelle für das Massen kopieren festgelegt. Wenn **bcp_init** für die Verwendung mit **bcp_bind** und [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md)aufgerufen wird, wird der **bcp_init** _szDataFile_ -Parameter, der die Datendatei angibt, auf NULL festgelegt. der **bcp_init**_eDirection_ -Parameter ist auf DB_IN festgelegt.  

Erstellen Sie für jede Spalte in der Tabelle, in die kopiert werden soll, einen separaten **bcp_bind** -aufzurufen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Nachdem die erforderlichen **bcp_bind** Aufrufe durchgeführt wurden, rufen Sie **bcp_sendrow** auf, um eine Daten Zeile aus den Programmvariablen an zu senden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Das erneute Binden einer Spalte wird nicht unterstützt.

Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die bereits empfangenen Zeilen übertragen möchten, wenden Sie [bcp_batch](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-batch.md)an. Beispielsweise können Sie **bcp_batch** einmal für alle 1000 Zeilen, die eingefügt werden, oder in einem beliebigen anderen Intervall abrufen.  

Wenn keine weiteren Zeilen eingefügt werden müssen, wenden Sie [bcp_done](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-done.md)an. Andernfalls wird ein Fehler ausgelöst.

Die Parametereinstellungen für Steuerelemente, die mit [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md)angegeben werden, haben keine Auswirkung auf **bcp_bind** Zeilen Übertragungen.  

Wenn *pData* für eine Spalte auf NULL festgelegt ist, da der Wert durch Aufrufe von [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md)bereitgestellt wird, alle nachfolgenden Spalten mit *eDataType* , die auf SQLTEXT, SQLNTEXT, SQLXML, SQLUDT, SQLCHARACTER, SQLVARCHAR, SQLVARBINARY, SqlBinary **,** SQLNCHAR oder SQLIMAGE festgelegt sind, müssen ebenfalls mit *pData auf* NULL festgelegt werden bcp_moretext  

Für neue große Werttypen, wie z. b. **varchar (max)**, **varbinary (max)** oder **nvarchar (max)**, können Sie SQLCHARACTER, SQLVARCHAR, SQLVARBINARY, SqlBinary und SQLNCHAR als Typindikatoren im *eDataType* -Parameter verwenden.  

Wenn *cbTerm* nicht 0 ist, ist jeder Wert (1, 2, 4 oder 8) für das Präfix (*cbIndicator*) gültig. In dieser Situation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sucht Native Client nach dem Abschluss Zeichen, berechnet die Daten Länge in Bezug auf das Abschluss Zeichen (*i*) und legt die *cbData* auf den kleineren Wert von i und den Wert des Präfixes fest.  

Wenn *cbTerm* den Wert 0 hat und *cbIndicator* (das Präfix) nicht 0 ist, muss *cbIndicator* 8 lauten. Das 8-Byte-Präfix kann die folgenden Werte annehmen:  

- 0xFFFFFFFFFFFFFFFF bedeutet einen NULL-Wert für das Feld.  

- 0xFFFFFFFFFFFFFFFE wird als spezieller Präfix Wert behandelt, der zum effizienten Senden von Daten in Blöcken an den Server verwendet wird. Die Daten mit diesem speziellen Präfix weisen das folgende Format auf:  

- <SPECIAL_PREFIX> \<0 or more  DATA CHUNKS> <ZERO_CHUNK>, wobei:  

- SPECIAL_PREFIX 0xFFFFFFFFFFFFFFFE entspricht.  

- DATA_CHUNK ist ein 4-Byte-Präfix mit der Länge des Blocks, gefolgt von den eigentlichen Daten, deren Länge im 4-Byte-Präfix angegeben ist.  

- ZERO_CHUNK ist ein 4-Byte-Wert, der alle Nullen (00000000) enthält, die das Ende der Daten angeben.  

- Jede andere gültige Länge von 8 Bytes wird als reguläre Daten Länge behandelt.  

 Das Aufrufen von [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md) , wenn **bcp_bind** verwendet wird, führt zu einem Fehler.  
  
## <a name="bcp_bind-support-for-enhanced-date-and-time-features"></a>bcp_bind-Unterstützung für erweiterte Funktionen für Datum und Uhrzeit

Informationen zu den Typen, die mit dem *eDataType* -Parameter für Datums-/Uhrzeittypen verwendet werden, finden Sie unter [Massen Kopier Änderungen für verbesserte Datums-und Uhrzeit Typen &#40;OLE DB und ODBC-&#41;](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md).  

Weitere Informationen finden Sie unter [Verbesserungen bei Datum und Uhrzeit &#40;ODBC-&#41;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  

## <a name="example"></a>Beispiel
  
```
#include sql.h  
#include sqlext.h  
#include odbcss.h  
// Variables like henv not specified.  
HDBC      hdbc;  
char         szCompanyName[MAXNAME];  
DBINT      idCompany;  
DBINT      nRowsProcessed;  
DBBOOL      bMoreData;  
char*      pTerm = "\t\t";  
  
// Application initiation, get an ODBC environment handle, allocate the  
// hdbc, and so on.  
...
  
// Enable bulk copy prior to connecting on allocated hdbc.  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_BCP, (SQLPOINTER) SQL_BCP_ON,  
   SQL_IS_INTEGER);  
  
// Connect to the data source; return on error.  
if (!SQL_SUCCEEDED(SQLConnect(hdbc, _T("myDSN"), SQL_NTS,  
   _T("myUser"), SQL_NTS, _T("myPwd"), SQL_NTS)))  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Initialize bcp.
if (bcp_init(hdbc, "comdb..accounts_info", NULL, NULL  
   DB_IN) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Bind program variables to table columns.
if (bcp_bind(hdbc, (LPCBYTE) &idCompany, 0, sizeof(DBINT), NULL, 0,  
   SQLINT4, 1)    == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
if (bcp_bind(hdbc, (LPCBYTE) szCompanyName, 0, SQL_VARLEN_DATA,  
   (LPCBYTE) pTerm, strnlen(pTerm, sizeof(pTerm)), SQLCHARACTER, 2) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
while (TRUE)  
   {  
   // Retrieve and process program data.
   if ((bMoreData = getdata(&idCompany, szCompanyName)) == TRUE)  
      {  
      // Send the data.
      if (bcp_sendrow(hdbc) == FAIL)  
         {  
         // Raise error and return.  
         return;  
         }  
      }  
   else  
      {  
      // Break out of loop.  
      break;  
      }  
   }  
  
// Terminate the bulk copy operation.  
if ((nRowsProcessed = bcp_done(hdbc)) == -1)  
   {  
   printf_s("Bulk-copy unsuccessful.\n");  
   return;  
   }  
  
printf_s("%ld rows copied.\n", nRowsProcessed);  
```  
  
## <a name="see-also"></a>Weitere Informationen

 [Bulk Copy Functions](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)
