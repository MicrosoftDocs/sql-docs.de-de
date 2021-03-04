---
description: Verwenden der Datenklassifizierung mit dem Microsoft ODBC Driver for SQL Server
title: Verwenden der Datenklassifizierung mit dem Microsoft ODBC-Treiber für SQL Server | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
author: v-makouz
ms.author: v-makouz
manager: kenvh
ms.openlocfilehash: 3b97e6407f541657acee2b15097d3fbb877854a9
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837414"
---
# <a name="data-classification"></a>Datenklassifizierung
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

## <a name="overview"></a>Übersicht
Für die Arbeit mit vertraulichen Daten wurde in SQL Server und Azure SQL Server die Möglichkeit eingeführt, Datenbankspalten mit Vertraulichkeitsmetadaten anzugeben. Dadurch kann die Clientanwendung unterschiedliche Arten von vertraulichen Daten (z.B. Gesundheits- oder Finanzdaten) in Übereinstimmung mit den jeweiligen Datenschutzrichtlinien behandeln und verarbeiten.

Weitere Informationen zum Zuweisen von Spaltenklassifizierungen finden Sie unter [SQL-Datenermittlung und -klassifizierung](../../relational-databases/security/sql-data-discovery-and-classification.md).

Mit dem Microsoft ODBC-Treiber 17.2 können diese Metadaten mithilfe von SQLGetDescField und dem Feldbezeichner SQL_CA_SS_DATA_CLASSIFICATION abgerufen werden.

## <a name="format"></a>Format
SQLGetDescField weist die folgende Syntax auf:

```  
SQLRETURN SQLGetDescField(  
     SQLHDESC        DescriptorHandle,  
     SQLSMALLINT     RecNumber,  
     SQLSMALLINT     FieldIdentifier,  
     SQLPOINTER      ValuePtr,  
     SQLINTEGER      BufferLength,  
     SQLINTEGER *    StringLengthPtr);  
```
*DescriptorHandle*  
 [Eingabe] Handle des Implementierungszeilendeskriptors. Kann durch einen Aufruf von SQLGetStmtAttr mit dem Anweisungsattribut SQL_ATTR_IMP_ROW_DESC abgerufen werden.
  
 *RecNumber*  
 [Eingabe] 0
  
 *FieldIdentifier*  
 [Eingabe] SQL_CA_SS_DATA_CLASSIFICATION
  
 *ValuePtr*  
 [Ausgabe] Ausgabepuffer
  
 *BufferLength*  
 [Eingabe] Der Länge des Ausgabepuffers in Bytes

 *StringLengthPtr* [Ausgabe] Zeiger auf den Puffer, an den die Gesamtzahl der zur Rückgabe in *ValuePtr* verfügbaren Bytes zurückgegeben wird.
 
> [!NOTE]
> Wenn die Größe des Puffers unbekannt ist, kann sie durch einen Aufruf von SQLGetDescField mit *ValuePtr* als NULL bestimmt werden. Sehen Sie sich dazu den Wert von *StringLengthPtr* an.
 
Wenn keine Datenklassifizierungsinformationen verfügbar sind, wird der Fehler *Ungültiges Deskriptorfeld* zurückgegeben.

Bei einem erfolgreichen Aufruf von SQLGetDescField enthält der Puffer, auf den *ValuePtr* verweist, die folgenden Daten:

 `nn nn [n sensitivitylabels] tt tt [t informationtypes] cc cc [c columnsensitivitys]`

> [!NOTE]
> `nn nn`, `tt tt` und `cc cc` sind ganzzahlige Werte mit mehreren Bytes, die mit dem niederwertigsten Byte an der niedrigsten Adresse gespeichert werden.

*`sensitivitylabel`* und *`informationtype`* haben dieses Format

 `nn [n bytes name] ii [i bytes id]`

*`columnsensitivity`* hat folgendes Format

 `nn nn [n sensitivityprops]`

Für jede Spalte *(c)* sind *n* *`sensitivityprops`* mit 4 Byte vorhanden:

 `ss ss tt tt`

s – Index des *`sensitivitylabels`* -Arrays, `FF FF`, falls keine Bezeichnung vorhanden

t – Index des *`informationtypes`* -Arrays, `FF FF`, falls keine Bezeichnung vorhanden


<br><br>
Das Format der Daten lässt sich mit folgenden Pseudostrukturen ausdrücken:

```
struct IDnamePair {
 BYTE nameLen;
 USHORT name[nameLen];
 BYTE idLen;
 USHORT id[idLen];
};

struct SensitivityProp {
 USHORT labelIdx;
 USHORT infoTypeIdx;
};

USHORT nLabels;
struct IDnamePair labels[nLabels];
USHORT nInfoTypes;
struct IDnamePair infotypes[nInfoTypes];
USHORT nColumns;
struct {
 USHORT nProps;
 struct SensitivityProp[nProps];
} columnClassification[nColumns];
```


## <a name="code-sample"></a>Codebeispiel
Testanwendung zur Veranschaulichung, wie die Metadaten einer Datenklassifizierung gelesen werden. Unter Windows kann sie mit `cl /MD dataclassification.c /I (directory of msodbcsql.h) /link odbc32.lib` kompiliert und mit einer Verbindungszeichenfolge sowie einer SQL-Abfrage (zur Rückgabe klassifizierter Spalten) als Parametern ausgeführt werden:

```
#ifdef _WIN32
#include <windows.h>
#endif
#include <sql.h>
#include <sqlext.h>
#include <msodbcsql.h>
#include <stdio.h>
SQLHANDLE env, dbc, stmt;
void checkRC_exit(SQLRETURN rc, SQLHANDLE hand, SQLSMALLINT htype, int retcode, char *action)
{
    if ((rc == SQL_ERROR || rc == SQL_SUCCESS_WITH_INFO) && hand)
    {
        char msg[1024], state[6];
        int i = 0;
        SQLRETURN rc2;
        SQLINTEGER err;
        SQLSMALLINT lenout;
        while ((rc2 = SQLGetDiagRec(htype, hand, ++i, state, &err, msg, sizeof(msg), &lenout)) == SQL_SUCCESS ||
            rc2 == SQL_SUCCESS_WITH_INFO)
            printf("%d (%d)[%s]%s\n", i, err, state, msg);
    }
    if (rc == SQL_ERROR && retcode)
    {
        printf("Error occurred%s%s\n", action ? " upon " : "", action ? action : "");
        exit(retcode);
    }
}
void printLabelInfo(char *type, char **pptr)
{
    char *ptr = *pptr;
    unsigned short nlabels;
    printf("----- %s(%u) -----\n", type, nlabels = *(unsigned short*)ptr);
    ptr += sizeof(unsigned short);
    while (nlabels--)
    {
        int namelen, idlen;
        char *nameptr, *idptr;
        namelen = *ptr++;
        nameptr = ptr;
        ptr += namelen * 2;
        idlen = *ptr++;
        idptr = ptr;
        ptr += idlen * 2;
        wprintf(L"Name: \"%.*s\" Id: \"%.*s\"\n", namelen, nameptr, idlen, idptr);
    }
    *pptr = ptr;
}
int main(int argc, char **argv)
{
    unsigned char *dcbuf;
    unsigned int dclen = 0;
    SQLRETURN rc;
    SQLHANDLE ird;
    if (argc < 3)
    {
        fprintf(stderr, "usage: dataclassification connstr query\n");
        return 1;
    }
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_ENV, 0, &env), 0, 0,
        2, "allocate environment");
    checkRC_exit(SQLSetEnvAttr(env, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, 0), env, SQL_HANDLE_ENV,
        3, "set ODBC version");
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_DBC, env, &dbc), env, SQL_HANDLE_ENV,
        4, "allocate connection");
    checkRC_exit(SQLDriverConnect(dbc, 0, argv[1], SQL_NTS, 0, 0, 0, SQL_DRIVER_NOPROMPT), dbc, SQL_HANDLE_DBC,
        5, "connect to server");
    checkRC_exit(SQLAllocHandle(SQL_HANDLE_STMT, dbc, &stmt), dbc, SQL_HANDLE_DBC,
        6, "allocate statement");
    checkRC_exit(SQLExecDirect(stmt, argv[2], SQL_NTS), stmt, SQL_HANDLE_STMT,
        7, "execute query");
    checkRC_exit(SQLGetStmtAttr(stmt, SQL_ATTR_IMP_ROW_DESC, (SQLPOINTER)&ird, SQL_IS_POINTER, 0), stmt, SQL_HANDLE_STMT,
        8, "get IRD handle");
    rc = SQLGetDescFieldW(ird, 0, SQL_CA_SS_DATA_CLASSIFICATION, dcbuf, 0, &dclen);
    if (rc != SQL_SUCCESS && rc != SQL_SUCCESS_WITH_INFO)
    {
        checkRC_exit(rc, ird, SQL_HANDLE_DESC, 0, 0);
        printf("Error reading SQL_CA_SS_DATA_CLASSIFICATION IRD field. Ensure that the driver and\n"
            "server both support the Data Classification feature, and that the query returns columns\n"
            "with classification information.\n");
    }
    else
    {
        SQLINTEGER dclenout;
        unsigned char *dcptr;
        unsigned short ncols;
        printf("Data Classification information (%u bytes):\n", dclen);
        if (!(dcbuf = malloc(dclen)))
        {
            printf("Memory Allocation Error");
            return 9;
        }
        checkRC_exit(SQLGetDescFieldW(ird, 0, SQL_CA_SS_DATA_CLASSIFICATION, dcbuf, dclen, &dclenout),
            ird, SQL_HANDLE_DESC, 10, "reading SQL_CA_SS_DATA_CLASSIFICATION");
        dcptr = dcbuf;
        printLabelInfo("Labels", &dcptr);
        printLabelInfo("Information Types", &dcptr);
        printf("----- Column Sensitivities(%u) -----\n", ncols = *(unsigned short*)dcptr);
        dcptr += sizeof(unsigned short);
        while (ncols--)
        {
            unsigned short nprops = *(unsigned short*)dcptr;
            dcptr += sizeof(unsigned short);
            while (nprops--)
            {
                unsigned short labelidx, typeidx;
                labelidx = *(unsigned short*)dcptr; dcptr += sizeof(unsigned short);
                typeidx = *(unsigned short*)dcptr; dcptr += sizeof(unsigned short);
                printf(labelidx == 0xFFFF ? "(none) " : "%u ", labelidx);
                printf(typeidx == 0xFFFF ? "(none)\n" : "%u\n", typeidx);
            }
            printf("-----\n");
        }
        if (dcptr != dcbuf + dclen)
        {
            printf("Error: unexpected parse of DATACLASSIFICATION data\n");
            return 11;
        }
        free(dcbuf);
    }
    return 0;
}
```

## <a name="supported-version"></a><a name="bkmk-version"></a>Unterstützte Version
Der Microsoft ODBC-Treiber 17.2 ermöglicht das Abrufen von Datenklassifizierungsinformationen über `SQLGetDescField`, wenn `FieldIdentifier` auf `SQL_CA_SS_DATA_CLASSIFICATION` festgelegt ist (1237). 

Ab Microsoft ODBC-Treiber 17.4.1.1 kann die von einem Server unterstütze Version der Datenklassifizierung mithilfe von `SQLGetDescField` unter Verwendung des Feldbezeichners `SQL_CA_SS_DATA_CLASSIFICATION_VERSION` (1238) abgerufen werden. In 17.4.1.1 ist die unterstützte Datenklassifizierungsversion auf 2 festgelegt.

 

Mit 17.4.2.1 wurde die Standardversion der Datenklassifizierung eingeführt, die auf 1 festgelegt ist. Diese Version wird (sofern unterstützt) vom Treiber an SQL Server gemeldet. Mit dem neuen Verbindungsattribut `SQL_COPT_SS_DATACLASSIFICATION_VERSION` (1400) können Anwendungen die unterstützte Version der Datenklassifizierung von 1 in den maximal unterstützten Wert ändern.  

Beispiel: 

Zum Festlegen der Version sollte dieser Aufruf unmittelbar vor dem Aufruf von SQLConnect oder SQLDriverConnect erfolgen:
```
ret = SQLSetConnectAttr(dbc, SQL_COPT_SS_DATACLASSIFICATION_VERSION, (SQLPOINTER)2, SQL_IS_INTEGER);
```

Der Wert der aktuell unterstützten Version der Datenklassifizierung kann durch einen Aufruf von SQLGetConnectAttr abgerufen werden: 
```
ret = SQLGetConnectAttr(dbc, SQL_COPT_SS_DATACLASSIFICATION_VERSION, (SQLPOINTER)&dataClassVersion, SQL_IS_INTEGER, 0);
```