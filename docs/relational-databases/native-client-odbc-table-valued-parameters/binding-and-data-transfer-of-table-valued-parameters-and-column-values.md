---
title: Datenübertragung von Table-Valued Parametern
description: Beschreiben Datenübertragung von Table-Valued Parametern
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: native-client
ms.topic: reference
ms.assetid: 0a2ea462-d613-42b6-870f-c7fa086a6b42
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: ''
ms.date: 07/01/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 19a7f22cd26ea4988364d51ff70300cdbf42d365
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97436154"
---
# <a name="binding-and-data-transfer-of-table-valued-parameters-and-column-values"></a>Bindung und Datenübertragung von Tabellenwertparametern und Spaltenwerten

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Tabellenwert Parameter (TVP) müssen wie andere Parameter gebunden werden, bevor Sie an den Server übermittelt werden. Die Anwendung bindet Tabellenwert Parameter auf dieselbe Weise, wie andere Parameter gebunden werden: mithilfe von SQLBindParameter oder gleichwertigen Aufrufen von SQLSetDescField oder SQLSetDescRec. Tabellenwertparameter haben den Serverdatentyp SQL_SS_TABLE. Der C-Typ kann entweder als SQL_C_DEFAULT oder SQL_C_BINARY angegeben werden.  

In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] oder höher werden nur Eingabe-Tabellenwertparameter unterstützt. Daher wird bei jedem Versuch, SQL_DESC_PARAMETER_TYPE auf einen anderen Wert als SQL_PARAM_INPUT festzulegen, SQL_ERROR mit SQLSTATE = HY105 und der Meldung "Ungültiger Parametertyp" zurückgegeben.  

Gesamten Tabellenwertparameter-Spalten können mit dem Attribut SQL_CA_SS_COL_HAS_DEFAULT_VALUE Standardwerte zugewiesen werden. Einzelnen Tabellenwert Parameter-Spaltenwerten können allerdings keine Standardwerte zugewiesen werden, indem SQL_DEFAULT_PARAM in *StrLen_or_IndPtr* mit SQLBindParameter verwendet wird. Tabellenwert Parameter als Ganzes können nicht auf einen Standardwert festgelegt werden, indem SQL_DEFAULT_PARAM in *StrLen_or_IndPtr* mit SQLBindParameter verwendet wird. Wenn diese Regeln nicht befolgt werden, gibt SQLExecute oder SQLExecDirect SQL_ERROR zurück. Es wird ein Diagnosedaten Satz mit SQLSTATE = 07s01 und der Meldung "Ungültige Verwendung des Standard Parameters für \<p> den Parameter" generiert, wobei \<p> die Ordinalzahl des TVP in der Abfrage Anweisung ist.  

> [!Note]
> Tabellenwert Parameter verfügen über keinen Standardwert, der festgelegt werden kann, da SQL_DEFAULT_PARAM keine Zeilen angibt. Wenn keine Zeilen vorhanden sind, sind keine zu bindenden Spalten vorhanden.

Nachdem die Anwendung den Tabellenwertparameter gebunden hat, muss sie jede einzelne Tabellenwertparameter-Spalte binden. Hierzu ruft die Anwendung zuerst SQLSetStmtAttr auf, um SQL_SOPT_SS_PARAM_FOCUS auf die Ordnungszahl eines Tabellenwert Parameters festzulegen. Die Anwendung bindet die Spalten des Tabellenwert Parameters durch Aufrufe der folgenden Routinen: SQLBindParameter, SQLSetDescRec und SQLSetDescField. Wenn Sie SQL_SOPT_SS_PARAM_FOCUS auf 0 festlegen, werden die üblichen Auswirkungen von SQLBindParameter, SQLSetDescRec und SQLSetDescField beim Betrieb auf reguläre Parameter der obersten Ebene wieder hergestellt.

> [!Note]
> Für Linux-und Mac-ODBC-Treiber mit unixODBC 2.3.1 bis 2.3.4 wird beim Festlegen des TVP-namens über SQLSetDescField mit dem Feld SQL_CA_SS_TYPE_NAME Deskriptor von unixodbc nicht automatisch zwischen ANSI-und Unicode-Zeichen folgen konvertiert, abhängig von der exakten Funktion (sqlsetdescfielda/sqlsetdescfieldw). Zum Festlegen des TVP-namens muss entweder immer SQLBindParameter oder sqlsetdescfieldw mit einer Unicode-Zeichenfolge (UTF-16) verwendet werden.

Für den Tabellenwertparameter an sich werden keine Daten gesendet oder empfangen, die Daten werden für die einzelnen Spalten, aus denen er sich zusammensetzt, gesendet und empfangen. Da der Tabellenwert Parameter eine Pseudo Spalte ist, verweisen die Parameter für SQLBindParameter auf unterschiedliche Attribute als andere Datentypen, wie im folgenden dargestellt:  

| Parameter | Verknüpftes Attribut für nicht-Tabellenwert Parameter-Typen, einschließlich Spalten | Verknüpftes Attribut für Tabellenwertparameter |
|-----------|---------------------------------------------------------------------------|-----------------------------------------------|
|*InputOutputType*|SQL_DESC_PARAMETER_TYPE in IPD<br /><br /> Für Tabellenwertparameter-Spalten muss diese Angabe der Angabe für den Tabellenwertparameter selbst entsprechen.|SQL_DESC_PARAMETER_TYPE in IPD<br /><br /> Hier muss SQL_PARAM_INPUT angegeben werden.|  
|*ValueType*|SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE in APD|SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE in APD<br /><br /> Hier muss SQL_C_DEFAULT oder SQL_C_BINARY angegeben werden.|  
|*ParameterType*|SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE in IPD|SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE in IPD<br /><br /> Hier muss SQL_SS_TABLE angegeben werden.|  
|*ColumnSize*|SQL_DESC_LENGTH oder SQL_DESC_PRECISION in IPD<br /><br /> Dies hängt vom Wert von *ParameterType* ab.|SQL_DESC_ARRAY_SIZE<br /><br /> Kann auch mit SQL_ATTR_PARAM_SET_SIZE festgelegt werden, wenn der Parameterfokus auf den Tabellenwertparameter festgelegt wurde.<br /><br /> Bei einem Tabellenwertparameter ist dies die Anzahl von Zeilen in den Tabellenwertparameter-Spaltenpuffern.|  
|*DecimalDigits*|SQL_DESC_PRECISION oder SQL_DESC_SCALE in IPD|Nicht verwendet. Diese Angabe muss 0 sein.<br /><br /> Wenn dieser Parameter nicht 0 ist, gibt SQLBindParameter SQL_ERROR zurück, und es wird ein Diagnosedaten Satz mit SQLSTATE = HY104 und der Meldung "ungültige Genauigkeit oder Skala" generiert.|  
|*ParameterValuePtr*|SQL_DESC_DATA_PTR in APD|SQL_CA_SS_TYPE_NAME<br /><br /> Dies ist für Aufrufe gespeicherter Prozeduren optional, und NULL kann angegeben werden, wenn dies nicht erforderlich ist. Es muss für SQL-Anweisungen angegeben werden, die keine Prozedur Aufrufe sind.<br /><br /> Dieser Parameter dient als eindeutiger Wert, anhand dessen die Anwendung den betreffenden Tabellenwertparameter bei Verwendung einer variablen Zeilenbindung identifizieren kann. Weitere Informationen finden Sie im Abschnitt "Variable Tabellenwertparameter-Zeilenbindung" weiter unten in diesem Thema.<br /><br /> Wenn ein Tabellenwert Parameter-Typname beim Aufrufen von SQLBindParameter angegeben wird, muss er auch in Anwendungen, die als ANSI-Anwendungen erstellt wurden, als Unicode-Wert angegeben werden. Der für den Parameter *StrLen_or_IndPtr* verwendete Wert muss entweder SQL_NTS oder die Zeichen folgen Länge des Namens multipliziert mit der Größe von (WChar) sein.|  
|*BufferLength*|SQL_DESC_OCTET_LENGTH in APD|Die Länge des Typnamens des Tabellenwertparameters in Byte<br /><br /> Dies kann SQL_NTS werden, wenn der Typname NULL-terminiert ist, oder 0, wenn der Typname des Tabellenwert Parameters nicht erforderlich ist.|  
|*StrLen_or_IndPtr*|SQL_DESC_OCTET_LENGTH_PTR in APD|SQL_DESC_OCTET_LENGTH_PTR in APD<br /><br /> Für Tabellenwertparameter wird hier die Zeilenanzahl statt die Datenlänge angegeben.|  
||||

Zwei Datenübertragungsmodi werden für Tabellenwertparameter unterstützt: feste Zeilenbindung und variable Zeilenbindung.  

## <a name="fixed-table-valued-parameter-row-binding"></a>Feste Tabellenwertparameter-Zeilenbindung

Bei der festen Zeilenbindung ordnet die Anwendung Puffer (oder Pufferarrays) zu, die groß genug sind, um alle Eingabespaltenwerte aufnehmen zu können. Die Anwendung führt Folgendes aus:  

1. Bindet alle Parameter mithilfe von SQLBindParameter-, SQLSetDescRec-oder SQLSetDescField-aufrufen.  

    1. Sie legt SQL_DESC_ARRAY_SIZE auf die maximale Anzahl von Zeilen fest, die für jeden Tabellenwertparameter übertragen werden können. Dies kann im SQLBindParameter-Befehl durchgeführt werden.  

2. Ruft SQLSetStmtAttr auf, um SQL_SOPT_SS_PARAM_FOCUS auf die Ordnungszahl der einzelnen Tabellenwert Parameter festzulegen.  

    1. Für jeden Tabellenwert Parameter bindet Tabellenwert Parameter-Spalten mithilfe von SQLBindParameter-, SQLSetDescRec-oder SQLSetDescField-aufrufen.  

    2. Für jede Tabellenwert Parameter-Spalte, die über Standardwerte verfügen soll, wird SQLSetDescField aufgerufen, um SQL_CA_SS_COL_HAS_DEFAULT_VALUE auf 1 festzulegen.  

3. Ruft SQLSetStmtAttr auf, um SQL_SOPT_SS_PARAM_FOCUS auf 0 festzulegen. Dies muss erfolgen, bevor SQLExecute oder SQLExecDirect aufgerufen wird. Andernfalls wird SQL_ERROR zurückgegeben, und es wird ein Diagnosedaten Satz mit SQLSTATE = HY024 und der Meldung "Ungültiger Attribut Wert, SQL_SOPT_SS_PARAM_FOCUS (muss zur Ausführungszeit NULL sein)" generiert.  

4. Legt *StrLen_or_IndPtr* oder SQL_DESC_OCTET_LENGTH_PTR auf SQL_DEFAULT_PARAM für einen Tabellenwert Parameter ohne Zeilen oder die Anzahl der Zeilen fest, die beim nächsten Aufrufen von SQLExecute oder SQLExecDirect übertragen werden sollen, wenn der Tabellenwert Parameter über Zeilen verfügt. *StrLen_or_IndPtr* oder SQL_DESC_OCTET_LENGTH_PTR kann nicht auf SQL_NULL_DATA für einen Tabellenwert Parameter festgelegt werden, da Tabellenwert Parameter nicht auf NULL festgelegt werden können (obwohl Tabellenwert Parameter-Spalten möglicherweise NULL-Werte zulassen). Wenn dieser Wert auf einen ungültigen Wert festgelegt ist, gibt SQLExecute oder SQLExecDirect SQL_ERROR zurück, und es wird ein Diagnosedaten Satz mit SQLSTATE = HY090 und der Meldung "ungültige Zeichen folgen-oder Pufferlänge für Parameter" generiert, \<p> wobei p für die Parameter Nummer steht.

5. Ruft SQLExecute oder SQLExecDirect auf.

    Die Spaltenwerte der Eingabe-Tabellenwert Parameter können in Teilen übergeben werden, wenn *StrLen_or_IndPtr* für die Spalte auf SQL_LEN_DATA_AT_EXEC (*length*) oder SQL_DATA_AT_EXEC festgelegt ist. Dies gleicht der Übergabe einzelner Werte bei Verwendung von Parameterarrays. Wie bei allen Data-at-Execution-Parametern gibt SQLParamData nicht an, für welche Zeile des Arrays der Treiber Daten anfordert. Dies muss von der Anwendung übernommen werden. Die Anwendung kann keine Annahmen über die Reihenfolge treffen, in der der Treiber Werte anfordert.  

## <a name="variable-table-valued-parameter-row-binding"></a>Variable Tabellenwertparameter-Zeilenbindung  

Bei einer Variablen Zeilen Bindung werden Zeilen zur Ausführungszeit in Batches übertragen, und die Anwendung übergibt bei Bedarf Zeilen an den Treiber. Dies ähnelt der Übergabe einzelner Werte für Data-at-Execution-Parameter zur Laufzeit. Bei einer variablen Zeilenbindung geht die Anwendung wie folgt vor:  

1. Bindet Parameter und Tabellenwert Parameter-Spalten, wie in den Schritten 1 bis 3 des vorherigen Abschnitts "korrigieren Table-Valued Parameter Zeilen Bindung" beschrieben.  

2. Legt *StrLen_or_IndPtr* oder SQL_DESC_OCTET_LENGTH_PTR für Tabellenwert Parameter fest, die zur Ausführungszeit an SQL_DATA_AT_EXEC übermittelt werden. Wenn keines von beiden festgelegt ist, wird der-Parameter wie im vorherigen Abschnitt beschrieben verarbeitet.  

3. Ruft SQLExecute oder SQLExecDirect auf. Dadurch wird SQL_NEED_DATA zurückgegeben, wenn SQL_PARAM_INPUT oder SQL_PARAM_INPUT_OUTPUT Parameter als Data-at-Execution-Parameter behandelt werden. In diesem Fall geht die Anwendung wie folgt vor:  

    - Ruft SQLParamData auf. Dadurch wird der *ParameterValuePtr* -Wert für einen Data-at-Execution-Parameter und der Rückgabecode SQL_NEED_DATA zurückgegeben. Wenn alle Parameterdaten an den Treiber übergeben wurden, gibt SQLParamData SQL_SUCCESS, SQL_SUCCESS_WITH_INFO oder SQL_ERROR zurück. Bei Data-at-Execution-Parametern kann *ParameterValuePtr*, das dem Deskriptorfeld SQL_DESC_DATA_PTR entspricht, als Token betrachtet werden, um einen Parameter zu identifizieren, für den ein Wert eindeutig erforderlich ist. Dieses "Token" wird beim Binden von der Anwendung an den Treiber übergeben und während der Ausführung dann wieder an die Anwendung übergeben.  
  
4. Um Tabellenwert Parameter-Zeilendaten für NULL-Tabellenwert Parameter zu senden, ruft eine Anwendung SQLPutData auf, wenn der Tabellenwert Parameter keine Zeilen enthält, und *StrLen_Or_Ind* auf SQL_DEFAULT_PARAM festgelegt ist.  

    Bei Tabellenwertparametern, die nicht NULL sind, geht die Anwendung wie folgt vor:

    - Legt *Str_Len_or_Ind* für alle Tabellenwert Parameter-Spalten auf entsprechende Werte fest und füllt Datenpuffer für Tabellenwert Parameter-Spalten auf, die keine Data-at-Execution-Parameter sind. Tabellenwertparameter-Spalten können Daten zur Laufzeit auf ähnliche Weise übergeben werden wie dem Treiber gewöhnliche Parameter schrittweise übergeben werden.  

    - Ruft SQLPutData auf, wobei *Str_Len_or_Ind* auf die Anzahl der an den Server zu sendenden Zeilen festgelegt ist. Jeder Wert außerhalb des Bereichs 0 bis SQL_DESC_ARRAY_SIZE oder SQL_DEFAULT_PARAM ist ein Fehler und gibt SQLSTATE HY090 mit der Meldung "ungültige Zeichen folgen-oder Pufferlänge" zurück. der Wert 0 gibt an, dass alle Zeilen gesendet wurden und keine weiteren Daten für einen Tabellenwert Parameter vorhanden sind (wie im zweiten Aufzählungs Zeichen in dieser Liste vermerkt). SQL_DEFAULT_PARAM kann nur verwendet werden, wenn der Treiber zum ersten Mal Daten für einen Tabellenwertparameter anfordert (wie im ersten Aufzählungspunkt dieser Auflistung beschrieben).  

5. Wenn alle Zeilen gesendet wurden, wird SQLPutData für den Tabellenwert Parameter mit einem *Str_Len_or_Ind* Wert von 0 aufgerufen, und fahren Sie mit Schritt 3a fort.  

6. Ruft SQLParamData erneut auf. Wenn unter den Tabellenwert Parameter-Spalten Data-at-Execution-Parameter vorhanden sind, werden diese durch den Wert *ValuePtrPtr* identifiziert, der von SQLParamData zurückgegeben wurde. Wenn alle Spaltenwerte verfügbar sind, gibt SQLParamData den *ParameterValuePtr* -Wert für den Tabellenwert Parameter zurück, und die Anwendung beginnt erneut.  

## <a name="next-steps"></a>Nächste Schritte

[Tabellenwert Parameter (ODBC)](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)