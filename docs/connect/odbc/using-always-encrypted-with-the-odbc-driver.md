---
title: Verwenden von Always Encrypted mit dem ODBC Driver
description: Erfahren Sie, wie Sie ODBC-Anwendungen mithilfe von Always Encrypted und dem Microsoft ODBC Driver for SQL Server entwickeln.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 02e306b8-9dde-4846-8d64-c528e2ffe479
ms.reviewer: v-daenge
ms.author: v-chojas
author: v-chojas
ms.openlocfilehash: 7da6c0d9864f514d757d0872fd32ce1e0219f01d
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464778"
---
# <a name="using-always-encrypted-with-the-odbc-driver-for-sql-server"></a>Verwenden von Always Encrypted mit ODBC Driver for SQL Server

[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

### <a name="applicable-to"></a>Anwendbar auf

- ODBC Driver 13.1 for SQL Server
- ODBC Driver 17 for SQL Server

### <a name="introduction"></a>Einführung

Dieser Artikel bietet Informationen zum Entwickeln von ODBC-Anwendungen mithilfe von [Always Encrypted (Datenbank-Engine)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) oder [Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md) und dem [ODBC-Treiber für SQL Server](microsoft-odbc-driver-for-sql-server.md).

Always Encrypted ermöglicht Clientanwendungen das Verschlüsseln von vertraulichen Daten in einer Weise, dass weder die Daten noch die Verschlüsselungsschlüssel zu irgendeinem Zeitpunkt gegenüber SQL Server oder Azure SQL-Datenbank offengelegt werden. Bei einem Treiber, bei dem Always Encrypted aktiviert ist, z. B. ODBC-Treiber für SQL Server, wird dies durch die transparente Ver- und Entschlüsselung sensibler Daten in der Clientanwendung erreicht. Der Treiber ermittelt automatisch, welche Abfrageparameter vertraulichen Datenbankspalten (mit Always Encrypted geschützt) entsprechen. Die Werte dieser Parameter werden dann vor der Übergabe an SQL Server oder Azure SQL-Datenbank verschlüsselt. Auf ähnliche Weise entschlüsselt der Treiber die Daten transparent, die von verschlüsselten Datenbankspalten in Abfrageergebnissen empfangen werden. *Always Encrypted mit Secure Enclaves* erweitert dieses Feature um umfangreichere Funktionen für vertrauliche Daten und sorgt gleichzeitig dafür, dass diese Daten vertraulich bleiben.

Weitere Informationen finden Sie unter [Always Encrypted (Datenbank-Engine)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) und [Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

### <a name="prerequisites"></a>Voraussetzungen

Konfigurieren Sie Always Encrypted in Ihrer Datenbank. Dieser Vorgang beinhaltet die Bereitstellung von Always Encrypted-Schlüsseln und die Einrichtung der Verschlüsselung für ausgewählte Datenbankspalten. Wenn Sie nicht bereits über eine Datenbank verfügen, für die Always Encrypted konfiguriert ist, befolgen Sie die Anweisungen in [Erste Schritte mit Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted). Insbesondere sollte Ihre Datenbank die Metadatendefinitionen für einen Spaltenhauptschlüssel (Column Master Key, CMK), einen Spaltenverschlüsselungsschlüssel (Column Encryption Key, CEK) und eine Tabelle mit mindestens einer Spalte enthalten, die mit diesem CEK verschlüsselt ist.

Wenn Sie Always Encrypted mit Secure Enclaves verwenden, finden Sie weitere Voraussetzungen unter [Entwickeln von Anwendungen mithilfe von Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves-client-development.md).

### <a name="enabling-always-encrypted-in-an-odbc-application"></a>Aktivieren von Always Encrypted in einer ODBC-Anwendung

Die einfachste Möglichkeit zum Aktivieren von sowohl Parameterverschlüsselung als auch Spaltenentschlüsselung in verschlüsselten Resultsets besteht darin, den Wert für das Schlüsselwort der Verbindungszeichenfolge `ColumnEncryption` auf **Aktiviert** festzulegen. Im Folgenden finden Sie ein Beispiel für eine Verbindungszeichenfolge, die „Always Encrypted“ aktiviert:

```cpp
SQLWCHAR *connString = L"Driver={ODBC Driver 17 for SQL Server};Server={myServer};Trusted_Connection=yes;ColumnEncryption=Enabled;";
```

Always Encrypted kann auch in der DSN-Konfiguration mit demselben Schlüssel und demselben Wert (die durch die Einstellung der Verbindungszeichenfolge, falls vorhanden, überschrieben werden) oder programmgesteuert über das Attribut `SQL_COPT_SS_COLUMN_ENCRYPTION` zur Verbindungsherstellung aktiviert werden. Wird es auf diese Weise festgelegt, wird der in der Verbindungszeichenfolge oder im DSN festgelegte Wert überschrieben:

```cpp
 SQLSetConnectAttr(hdbc, SQL_COPT_SS_COLUMN_ENCRYPTION, (SQLPOINTER)SQL_COLUMN_ENCRYPTION_ENABLE, 0);
```

Nachdem Always Encrypted für die Verbindung aktiviert wurde, kann das Verhalten für einzelne Abfragen angepasst werden. Weitere Informationen finden Sie weiter unten im Abschnitt [Kontrollieren der Auswirkungen von Always Encrypted auf die Leistung](#controlling-the-performance-impact-of-always-encrypted).

Das Aktivieren von Always Encrypted für eine erfolgreiche Verschlüsselung oder Entschlüsselung ist nicht ausreichend. Sie müssen außerdem Folgendes sicherstellen:

- Die Anwendung verfügt über die Datenbankberechtigungen *VIEW ANY COLUMN MASTER KEY DEFINITION* und *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* , die für den Zugriff auf die Metadaten in der Datenbank über Always Encrypted-Schlüssel erforderlich sind. Einzelheiten hierzu finden Sie unter [Datenbankberechtigungen](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).

- Die Anwendung muss auf den CMK zugreifen können, der die CEKs für die abgefragten verschlüsselten Spalten schützt. Dieses Verhalten hängt von dem Keystore-Anbieter ab, der den CMK speichert. Weitere Informationen finden Sie im Abschnitt [Arbeiten mit Spaltenhauptschlüsselspeichern](#working-with-column-master-key-stores).

### <a name="enabling-always-encrypted-with-secure-enclaves"></a>Aktivieren von Always Encrypted mit Secure Enclaves

> [!NOTE]
> Unter Linux und macOS ist zur Verwendung von Always Encrypted mit Secure Enclaves die OpenSSL-Version 1.0.1 oder höher erforderlich.

Ab Version 17.4 unterstützt der Treiber Always Encrypted mit Secure Enclaves. Um die Verwendung der Enclave beim Herstellen einer Verbindung mit einer Datenbank zu ermöglichen, legen Sie den DSN-Schlüssel `ColumnEncryption`, das Schlüsselwort für die Verbindungszeichenfolge oder das Verbindungsattribut auf den folgenden Wert fest: `<attestation protocol>\<attestation URL>`. Dabei gilt Folgendes:

- `<attestation protocol>`: gibt ein für den Enclavenachweis verwendetes Protokoll an.
  - Wenn Sie [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)] und den Host-Überwachungsdienst (Host Guardian Service, HGS) verwenden, sollte `<attestation protocol>` auf `VBS-HGS` festgelegt werden.
  - Wenn Sie [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und Microsoft Azure Attestation verwenden, sollte `<attestation protocol>` auf `SGX-AAS` festgelegt werden.

- `<attestation URL>`: gibt eine Nachweis-URL (einen Endpunkt für den Nachweisdienst) an. Sie benötigen für Ihre Umgebung eine Nachweis-URL von dem Dienstadministrator, der für Nachweise zuständig ist.

  - Wenn Sie [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)] und den Host-Überwachungsdienst verwenden, finden Sie weitere Informationen unter [Ermitteln und Freigeben der HGS-Nachweis-URL](../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Wenn Sie [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und Microsoft Azure Attestation verwenden, finden Sie weitere Informationen unter [Ermitteln der Nachweis-URL für Ihre Nachweisrichtlinie](../../relational-databases/security/encryption/always-encrypted-enclaves.md#secure-enclave-attestation).

Beispiele für Verbindungszeichenfolgen, die Enclaveberechnungen für eine Datenbankverbindung ermöglichen:

- [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)]:
  
   ```
   Driver=ODBC Driver 17 for SQL Server;Server=myServer.myDomain;Database=myDataBase;Trusted_Connection=Yes;ColumnEncryption=VBS-HGS,http://myHGSServer.myDomain/Attestation
   ```

- [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]:
  
   ```
   Driver=ODBC Driver 17 for SQL Server;Server=myServer.database.windows.net;Database=myDataBase;Uid=myUsername;Pwd=myPassword;Encrypt=yes;ColumnEncryption=SGX-AAS,https://myAttestationProvider.uks.attest.azure.net/attest/SgxEnclave
   ```

Wenn der Server und der Nachweisdienst sowie die für Enclaves aktivierten CMKs und CEKs für die gewünschten Spalten ordnungsgemäß konfiguriert sind, sollten Sie jetzt in der Lage sein, zusätzlich zur vorhandenen Always Encrypted-Funktionalität Abfragen mit Enclave-Verwendung auszuführen. Hierzu gehören beispielsweise die direkte Verschlüsselung und umfangreiche Berechnungen. Weitere Informationen finden Sie unter [Konfigurieren von Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/configure-always-encrypted-enclaves.md).

### <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Abrufen und Ändern von Daten in verschlüsselten Spalten

Sobald Sie Always Encrypted für eine Verbindung aktiviert haben, können Sie standardmäßige ODBC-APIs verwenden. Die ODBC-APIs können Daten in verschlüsselten Datenbankspalten abrufen oder ändern. Die folgenden Dokumentationsmaterialien sind möglicherweise hilfreich:

- [ODBC-Beispielcode](cpp-code-example-app-connect-access-sql-db.md)
- [ODBC-Programmierreferenz](../../odbc/reference/odbc-programmer-s-reference.md)

Ihre Anwendung muss über die erforderlichen Datenbankberechtigungen verfügen und auf den Spaltenhauptschlüssel zugreifen können. Dann verschlüsselt der Treiber alle Abfrageparameter für verschlüsselte Spalten. Der Treiber entschlüsselt auch Daten, die aus verschlüsselten Spalten abgerufen werden. Der Treiber führt sämtliche Verschlüsselungs- und Entschlüsselungsvorgänge ohne Unterstützung durch den Quellcode durch. Für Ihr Programm stellt sich dies so dar, als wären die Spalten nicht verschlüsselt.

Wenn Always Encrypted nicht aktiviert ist, tritt bei Abfragen mit Parametern, die auf verschlüsselte Spalten ausgerichtet sind, ein Fehler auf. Daten können weiterhin aus verschlüsselten Spalten abgerufen werden, solange die Abfrage keine Parameter für verschlüsselte Spalten enthält. Der Treiber versucht jedoch nicht, die Daten zu entschlüsseln. Daher erhält die Anwendung die verschlüsselten Binärdaten (als Bytearrays).

In der folgenden Tabelle wird das Verhalten von Abfragen in Abhängigkeit davon zusammengefasst, ob Always Encrypted aktiviert ist:

|Abfragemerkmal | Always Encrypted ist aktiviert und die Anwendung kann auf die Schlüssel und Schlüsselmetadaten zugreifen.|Always Encrypted ist aktiviert und die Anwendung kann nicht auf die Schlüssel oder Schlüsselmetadaten zugreifen. | Always Encrypted ist deaktiviert|
|:---|:---|:---|:---|
| Parameter für verschlüsselte Spalten | Parameterwerte werden transparent verschlüsselt. | Fehler | Fehler|
| Daten von verschlüsselten Spalten ohne Parameter abrufen, die auf verschlüsselte Spalten ausgerichtet sind.| Ergebnisse von verschlüsselten Spalten werden transparent entschlüsselt. Die Anwendung erhält Klartextspaltenwerte. | Fehler | Ergebnisse von verschlüsselten Spalten werden nicht entschlüsselt. Die Anwendung erhält verschlüsselte Werte als Bytearrays.

Die folgenden Beispiele veranschaulichen das Abrufen und Ändern von Daten in verschlüsselten Spalten. Dabei wird eine Tabelle mit dem folgenden Schema angenommen: Die Spalten „SSN“ und „BirthDate“ werden verschlüsselt.

```tsql
CREATE TABLE [dbo].[Patients](
 [PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] )
 GO
```

#### <a name="data-insertion-example"></a>Beispiel für das Einfügen von Daten

In diesem Beispiel wird eine Zeile in die Tabelle „Patients“ eingefügt. Beachten Sie Folgendes:

- Es erfolgt keine spezielle Verschlüsselung im Beispielcode. Der Treiber erkennt und verschlüsselt automatisch die Werte der SSN- und Datumsparameter für verschlüsselte Spalten. Durch dieses Verhalten wird die Verschlüsselung für die Anwendung transparent.

- Die in die Datenbankspalten eingefügten Werte, einschließlich der verschlüsselten Spalten, werden als gebundene Parameter übergeben (siehe [SQLBindParameter-Funktion](../../odbc/reference/syntax/sqlbindparameter-function.md)). Während die Verwendung von Parametern optional ist, wenn Werte an nicht verschlüsselte Spalten gesendet werden (obwohl es dringend empfohlen wird, da es dabei hilft, eine Einschleusung von SQL-Befehlen zu verhindern), ist sie für Werte erforderlich, die auf verschlüsselte Spalten ausgerichtet sind. Werden die in die Spalten „SSN“ oder „BirthDate“ eingefügten Werte als Literale übergeben, die in die Abfrageanweisung eingebettet sind, tritt ein Fehler auf, da der Treiber nicht versucht, Literale in Abfragen zu verschlüsseln oder anderweitig zu verarbeiten. Daher würde der Server sie zurückweisen, da sie mit den verschlüsselten Spalten inkompatibel sind.

- Der in die SSN-Spalte eingefügte SQL-Typ des Parameters wird auf „SQL_CHAR“ festgelegt, der dem SQL Server-Datentyp **char** zugeordnet wird (`rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);`). Wird der Typ des Parameters auf „SQL_WCHAR“ festgelegt, der **nchar** zugeordnet wird, tritt bei der Abfrage ein Fehler auf, da Always Encrypted keine serverseitigen Konvertierungen von verschlüsselten nchar-Werten in verschlüsselte char-Werte unterstützt. Unter [ODBC-Programmierreferenz – Anhang D: Datentypen](../../odbc/reference/appendixes/appendix-d-data-types.md) finden Sie weitere Informationen zu den Datentypzuordnungen.

```cpp
    SQL_DATE_STRUCT date;
    SQLLEN cbdate;   // size of date structure  

    SQLCHAR SSN[12];
    strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

    SQLWCHAR* firstName = L"Catherine";
    SQLWCHAR* lastName = L"Abel";
    SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

    // Initialize the date structure  
    date.day = 10;
    date.month = 9;
    date.year = 1996;

    // Size of structures   
    cbdate = sizeof(SQL_DATE_STRUCT);

    SQLRETURN rc = 0;

    string queryText = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?) ";

    rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

    //SSN
    rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);
    //FirstName
    rc = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)firstName, 0, &cbFirstName);
    //LastName
    rc = SQLBindParameter(hstmt, 3, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)lastName, 0, &cbLastName);
    //BirthDate
    rc = SQLBindParameter(hstmt, 4, SQL_PARAM_INPUT, SQL_C_TYPE_DATE, SQL_TYPE_DATE, 10, 0, (SQLPOINTER)&date, 0, &cbdate);

    rc = SQLExecute(hstmt);
```

#### <a name="plaintext-data-retrieval-example"></a>Beispiel für das Abrufen von Klartextdaten

Im folgenden Beispiel wird das Filtern von Daten auf Basis verschlüsselter Werte und das Abrufen von Klartextdaten aus verschlüsselten Spalten veranschaulicht. Beachten Sie Folgendes:

- Der in der WHERE-Klausel zum Filtern der Spalte „SSN“ verwendete Wert muss mit dem SQLBindParameter-Parameter übergeben werden, damit ihn der Treiber vor dem Senden an die Datenbank transparent verschlüsseln kann.

- Alle Werte werden vom Programm als Klartext ausgegeben, da der Treiber die aus den Spalten „SSN“ und „BirthDate“ abgerufenen Daten transparent entschlüsselt.

> [!NOTE]
> Abfragen können Übereinstimmungsvergleiche für verschlüsselte Spalten nur dann ausführen, wenn die Verschlüsselung deterministisch erfolgt oder Secure Enclave aktiviert ist. Weitere Informationen finden Sie im Abschnitt [Auswählen der deterministischen oder zufälligen Verschlüsselung](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

```cpp
SQLCHAR SSN[12];
strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

SQLWCHAR* firstName = L"Catherine";
SQLWCHAR* lastName = L"Abel";
SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

SQLRETURN rc = 0;
string empty = "";
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] " + empty +
    "FROM  [dbo].[Patients]" +
    "WHERE " +
    "[SSN] = ? ";

rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

//SSN
rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);

rc = SQLExecute(hstmt);
HandleDiagnosticRecord(hstmt, SQL_HANDLE_STMT, rc);

SQL_DATE_STRUCT dateVal;
SQLWCHAR firstNameVal[50];
SQLWCHAR lastNameVal[50];
SQLCHAR SSNVal[12];
SQLLEN cbdate;   // size of date structure  

int rowcount = 0;
while (SQL_SUCCEEDED(SQLFetch(hstmt)))
{
    rowcount++;
    SQLGetData(hstmt, 1, SQL_C_CHAR, &SSNVal, 11, &cbSSN);
    SQLGetData(hstmt, 2, SQL_C_WCHAR, &firstNameVal, 50, &cbFirstName);
    SQLGetData(hstmt, 3, SQL_C_WCHAR, &lastNameVal, 50, &cbLastName);
    SQLGetData(hstmt, 4, SQL_C_TYPE_DATE, &dateVal, 10, &cbdate);        
}
```

#### <a name="ciphertext-data-retrieval-example"></a>Beispiel für das Abrufen von Chiffretextdaten

Wenn Always Encrypted nicht aktiviert ist, können Abfragen weiterhin Daten aus verschlüsselten Spalten abrufen, so lange die Abfrage keine Parameter für verschlüsselte Spalten enthält.

In den folgenden Beispielen wird das Abrufen von binär verschlüsselten Daten aus verschlüsselten Spalten veranschaulicht. Beachten Sie Folgendes:

- Da Always Encrypted in der Verbindungszeichenfolge nicht aktiviert ist, gibt die Abfrage verschlüsselte Werte von „SSN“ und „BirthDate“ als Bytearrays zurück (das Programm konvertiert die Werte in Zeichenfolgen).
- Eine Abfrage, die Daten aus verschlüsselten Spalten mit deaktiviertem Always Encrypted abruft, kann Parameter aufweisen, so lange keiner der Parameter auf eine verschlüsselte Spalte ausgerichtet ist. Die obige Abfrage filtert nach der Spalte „LastName“, die in der Datenbank nicht verschlüsselt ist. Wenn die Abfrage nach „SSN“ oder „BirthDate“ filtert, würde ein Fehler auftreten.

```cpp
SQLCHAR SSN[12];
strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

SQLWCHAR* firstName = L"Catherine";
SQLWCHAR* lastName = L"Abel";
SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

SQLRETURN rc = 0;
string empty = "";
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] " + empty +
    "FROM  [dbo].[Patients]" +
    "WHERE " +
    "[LastName] = ?";

rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

//LastName
rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)lastName, 0, &cbLastName);

rc = SQLExecute(hstmt);
HandleDiagnosticRecord(hstmt, SQL_HANDLE_STMT, rc);

SQL_DATE_STRUCT dateVal;
SQLWCHAR firstNameVal[50];
SQLWCHAR lastNameVal[50];
SQLCHAR SSNVal[12];
SQLLEN cbdate;   // size of date structure  

int rowcount = 0;
while (SQL_SUCCEEDED(SQLFetch(hstmt)))
{
    rowcount++;
    SQLGetData(hstmt, 1, SQL_C_CHAR, &SSNVal, 11, &cbSSN);
    SQLGetData(hstmt, 2, SQL_C_WCHAR, &firstNameVal, 50, &cbFirstName);
    SQLGetData(hstmt, 3, SQL_C_WCHAR, &lastNameVal, 50, &cbLastName);
    SQLGetData(hstmt, 4, SQL_C_TYPE_DATE, &dateVal, 10, &cbdate);        
}
```

#### <a name="moneysmallmoney-encryption"></a>Money-/SmallMoney-Verschlüsselung

Ab der Treiberversion 17.7 ist es möglich, Always Encrypted mit MONEY und SMALLMONEY zu verwenden. Es sind jedoch noch einige zusätzliche Schritte erforderlich.
Bei Einfügevorgängen in verschlüsselte MONEY- oder SMALLMONEY-Spalten sollten Sie einen der folgenden C-Typen verwenden:

```cpp
SQL_C_CHAR
SQL_C_WCHAR
SQL_C_SHORT
SQL_C_LONG
SQL_C_FLOAT
SQL_C_DOUBLE
SQL_C_BIT
SQL_C_TINYINT
SQL_C_SBIGINT
SQL_C_NUMERIC
```

Außerdem sollten Sie den SQL-Typ `SQL_NUMERIC` oder `SQL_DOUBLE` (Genauigkeit kann bei Verwendung dieses Typs verloren gehen) einsetzen.

##### <a name="binding-the-variable"></a>Binden der Variablen

Bei jedem Binden einer MONEY-/SMALLMONEY-Variablen in einer verschlüsselten Spalte müssen die folgenden Deskriptorfelder festgelegt werden:

```cpp
// n is the descriptor record of the MONEY/SMALLMONEY parameter
// the type is assumed to be SMALLMONEY if isSmallMoney is true and MONEY otherwise

SQLHANDLE ipd = 0;
SQLGetStmtAttr(hStmt, SQL_ATTR_IMP_PARAM_DESC, (SQLPOINTER)&ipd, SQL_IS_POINTER, NULL);
SQLSetDescField(ipd, n, SQL_CA_SS_SERVER_TYPE, isSmallMoney ? (SQLPOINTER)SQL_SS_TYPE_SMALLMONEY :
                                                              (SQLPOINTER)SQL_SS_TYPE_MONEY, SQL_IS_INTEGER);

// If the variable is bound as SQL_NUMERIC, additional descriptor fields have to be set
// var is SQL_NUMERIC_STRUCT containing the value to be inserted

SQLHDESC   hdesc = NULL;
SQLGetStmtAttr(hStmt, SQL_ATTR_APP_PARAM_DESC, &hdesc, 0, NULL);
SQLSetDescField(hdesc, n, SQL_DESC_PRECISION, (SQLPOINTER)(var.precision), 0);
SQLSetDescField(hdesc, n, SQL_DESC_SCALE, (SQLPOINTER)(var.scale), 0);
SQLSetDescField(hdesc, n, SQL_DESC_DATA_PTR, &var, 0);
```

#### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Vermeiden allgemeiner Probleme beim Abfragen von verschlüsselter Spalten

In diesem Abschnitt werden die allgemeinen Fehlerkategorien bei der Abfrage verschlüsselter Spalten über ODBC-Anwendungen sowie einige Grundsätze zum Vermeiden dieser Fehler beschrieben.

##### <a name="unsupported-data-type-conversion-errors"></a>Konvertierungsfehler durch nicht unterstützte Datentypen

Always Encrypted unterstützt einige Konvertierungen für verschlüsselte Datentypen. Eine ausführliche Liste der unterstützten Typkonvertierungen finden Sie unter [Always Encrypted (Datenbank-Engine)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Beachten Sie die folgenden Punkte, wenn Sie „SQLBindParameter“ mit Parametern für verschlüsselte Spalten verwenden, um Fehler bei der Datentypkonvertierung zu vermeiden:

- Entweder ist der SQL-Typ des Parameters identisch mit dem Typ der Zielspalte oder die Konvertierung vom SQL-Typ in den Typ der Spalte wird unterstützt.

- Die Genauigkeit und Dezimalstellenanzahl von Parametern, die auf Spalten der SQL Server-Datentypen `decimal` und `numeric` ausgerichtet sind, ist mit der für die Zielspalte konfigurierten Genauigkeit und Dezimalstellenanzahl identisch.

- Die Genauigkeit von Parametern, die auf Spalten der SQL Server-Datentypen `datetime2`, `datetimeoffset` oder `time` ausgerichtet sind, ist in Abfragen, in denen die Zielspalte geändert wird, nicht größer als die Genauigkeit für die Zielspalte.  

##### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Fehler aufgrund der Übergabe von Klartext anstelle von verschlüsselten Werten

Jeder Wert für verschlüsselte Spalten muss vor dem Senden an den Server verschlüsselt werden. Ein Versuch, einen Klartextwert einzufügen, zu ändern oder nach einem Klartextwert für eine verschlüsselte Spalte zu filtern, führt zu einem Fehler. Stellen Sie Folgendes sicher, um solche Fehler zu vermeiden:

- Always Encrypted muss aktiviert sein (im DSN, in der Verbindungszeichenfolge, vor der Verbindungsherstellung durch Festlegen des Verbindungsattributs `SQL_COPT_SS_COLUMN_ENCRYPTION` für eine bestimmte Verbindung oder durch Festlegen des Anweisungsattributs `SQL_SOPT_SS_COLUMN_ENCRYPTION` für eine bestimmte Anweisung).

- Sie verwenden „SQLBindParameter“ zum Senden von Daten, die auf verschlüsselte Spalten ausgerichtet sind. Das folgende Beispiel zeigt eine Abfrage, in der auf falsche Weise nach einem Literal bzw. einer Konstante einer verschlüsselten Spalte (SSN) gefiltert wird, anstatt das Literal als Argument an SQLBindParameter zu übergeben.

```cpp
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
```

### <a name="precautions-when-using-sqlsetpos-and-sqlmoreresults"></a>Vorsichtsmaßnahmen bei der Verwendung von „SQLSetPos“ und „SQLMoreResults“

#### <a name="sqlsetpos"></a>SQLSetPos

Mit der API `SQLSetPos` kann eine Anwendung Zeilen in einem Resultset mithilfe von Puffern aktualisieren, die mit „SQLBindCol“ gebunden sind und in die zuvor Zeilendaten abgerufen wurden. Aufgrund des asymmetrischen Verhaltens von verschlüsselten Typen mit fester Länge beim Auffüllen mit Leerstellen ist es möglich, die Daten in diesen Spalten unerwartet zu ändern, während andere Spalten in der Zeile aktualisiert werden. Mit Always Encrypted werden Zeichenwerte mit fester Länge mit Leerzeichen aufgefüllt, wenn der Wert kleiner als die Puffergröße ist.

Wenn Sie dieses Verhalten verhindern möchten, verwenden Sie das Flag `SQL_COLUMN_IGNORE` zum Ignorieren von Spalten, die nicht als Teil von `SQLBulkOperations` und bei Verwendung von `SQLSetPos` für cursorbasierte Updates aktualisiert werden.  Sie sollten alle Spalten ignorieren, die nicht direkt von der Anwendung geändert werden. Damit verhindern Sie Leistungseinbußen und die Kürzung von Spalten, die an einen Puffer gebunden sind, der *kleiner* als die tatsächliche Größe (der Datenbank) ist. Weitere Informationen finden Sie in der [SQLSetPos-Funktionsreferenz](../../odbc/reference/syntax/sqlsetpos-function.md).

#### <a name="sqlmoreresults--sqldescribecol"></a>SQLMoreResults und SQLDescribeCol

Anwendungsprogramme können [SQLDescribeCol](../../odbc/reference/syntax/sqldescribecol-function.md) aufrufen, um Metadaten zu Spalten in vorbereiteten Anweisungen zurückzugeben.  Wenn Always Encrypted aktiviert ist, wird durch Aufrufen von `SQLMoreResults` *vor* dem Aufrufen von `SQLDescribeCol` stattdessen [sp_describe_first_result_set](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) aufgerufen. Damit werden die Klartextmetadaten für verschlüsselte Spalten nicht korrekt zurückgegeben. Rufen Sie daher `SQLDescribeCol` für vorbereitete Anweisungen *vor* dem Aufrufen von `SQLMoreResults` auf, wenn Sie dies vermeiden möchten.

## <a name="controlling-the-performance-impact-of-always-encrypted"></a>Kontrollieren der Auswirkungen von Always Encrypted auf die Leistung

Da Always Encrypted eine clientseitige Verschlüsselungstechnologie ist, werden die meisten Leistungseinbußen auf der Clientseite und nicht in der Datenbank beobachtet. Neben den Kosten für Verschlüsselungs- und Entschlüsselungsvorgänge ergeben sich die folgenden anderen Quellen für Leistungseinbußen auf der Clientseite:

- Zusätzliche Roundtrips zur Datenbank zum Abrufen von Metadaten für Abfrageparameter.

- Aufrufe an einen Spaltenhauptschlüsselspeicher für den Zugriff auf einen Spaltenhauptschlüssel.

In diesem Abschnitt werden die integrierten Leistungsoptimierungen in ODBC Driver for SQL Server und die Steuerung der Auswirkung der beiden oben genannten Faktoren auf die Leistung beschrieben.

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>Kontrollieren von Roundtrips zum Abrufen von Metadaten für Abfrageparameter

Wenn Always Encrypted für eine Verbindung aktiviert ist, ruft der Treiber standardmäßig [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) für jede parametrisierte Abfrage auf, wobei die Abfrageanweisung (ohne Parameterwerte) an SQL Server übergeben wird. Die Abfrageanweisung wird von dieser gespeicherten Prozedur analysiert, um zu ermitteln, ob Parameter verschlüsselt werden müssen. In diesem Fall werden verschlüsselungsbezogene Informationen für jeden Parameter zurückgegeben, die der Treiber dann verschlüsseln kann. Das oben beschriebene Verhalten stellt einen hohen Grad an Transparenz für die Clientanwendung sicher: Die Anwendung (und der Anwendungsentwickler) muss nicht beachten, welche Abfragen auf verschlüsselte Spalten zugreifen, so lange die Werte für verschlüsselte Spalten in Parametern an den Treiber übergeben werden.

Ab Version 17.6 werden vom Treiber auch die Verschlüsselungsmetadaten für vorbereitete Anweisungen zwischengespeichert. So wird die Leistung verbessert, da nachfolgende Aufrufe von `SQLExecute` keinen zusätzlichen Roundtrip zum Abrufen der Verschlüsselungsmetadaten erfordern.

### <a name="per-statement-always-encrypted-behavior"></a>Verhalten von Always Encrypted für einzelne Anweisungen

Sie können das Verhalten von Always Encrypted für einzelne Abfragen ändern, wenn Sie es für die Verbindung aktiviert haben, um beim Abrufen von Verschlüsselungsmetadaten für parametrisierte Abfragen die Auswirkungen auf die Leistung zu steuern. Auf diese Weise können Sie sicherstellen, dass `sys.sp_describe_parameter_encryption` nur für Abfragen aufgerufen wird, bei denen Ihnen bekannt ist, dass sie über Parameter für verschlüsselte Spalten verfügen. Beachten Sie jedoch, dass Sie auf diese Weise die Transparenz der Verschlüsselung reduzieren: Wenn Sie zusätzliche Spalten in Ihrer Datenbank verschlüsseln, müssen Sie möglicherweise den Code der Anwendung ändern, um ihn an die Schemaänderungen anzupassen.

Rufen Sie zum Steuern des Verhaltens von Always Encrypted für eine Anweisung „SQLSetStmtAttr“ auf, um das Anweisungsattribut `SQL_SOPT_SS_COLUMN_ENCRYPTION` auf einen der folgenden Werte festzulegen:

|Wert|BESCHREIBUNG|
|-|-|
|`SQL_CE_DISABLED` (0)|Always Encrypted ist für die Anweisung deaktiviert.|
|`SQL_CE_RESULTSETONLY` (1)|Nur Entschlüsselung. Resultsets und Rückgabewerte werden entschlüsselt, Parameter werden nicht verschlüsselt.|
|`SQL_CE_ENABLED` (3) | Always Encrypted ist aktiviert und wird für Parameter und Ergebnisse verwendet.|

Neue Anweisungshandles, die über eine Verbindung mit aktiviertem Always Encrypted erstellt wurden, sind standardmäßig auf „SQL_CE_ENABLED“ festgelegt. Wurden die Handles über eine Verbindung mit deaktivierter Always Encrypted-Option erstellt, ist „SQL_CE_DISABLED“ die Standardeinstellung (und Always Encrypted kann nicht aktiviert werden).

Greifen die meisten Abfragen einer Clientanwendung auf verschlüsselte Spalten zu, wird die Beachtung folgender Punkte empfohlen:

- Legen Sie das Schlüsselwort für die `ColumnEncryption`-Verbindungszeichenfolge auf `Enabled` fest.

- Legen Sie für Anweisungen, die nicht auf verschlüsselte Spalten zugreifen, das Attribut `SQL_SOPT_SS_COLUMN_ENCRYPTION` auf `SQL_CE_DISABLED` fest. Durch diese Einstellung wird der Aufruf von `sys.sp_describe_parameter_encryption` deaktiviert, und es werden auch keine Werte im Resultset entschlüsselt.

- Legen Sie für Anweisungen ohne Parameter, für die eine Verschlüsselung erforderlich wäre, und die Daten aus verschlüsselten Spalten abrufen, das Attribut `SQL_SOPT_SS_COLUMN_ENCRYPTION` auf `SQL_CE_RESULTSETONLY` fest. Durch diese Einstellung werden der Aufruf von `sys.sp_describe_parameter_encryption` und die Parameterverschlüsselung deaktiviert. Ergebnisse mit verschlüsselten Spalten werden weiterhin entschlüsselt.

- Verwenden Sie vorbereitete Anweisungen für Abfragen, die mehrmals ausgeführt werden. Bereiten Sie die Abfrage mit `SQLPrepare` vor, speichern Sie das Anweisungshandle, und verwenden Sie es bei jeder Ausführung mit `SQLExecute` erneut. Diese Methode ist der bevorzugte leistungsorientierte Ansatz, auch wenn keine verschlüsselten Spalten vorhanden sind. Außerdem profitiert der Treiber von zwischengespeicherten Metadaten.

## <a name="always-encrypted-security-settings"></a>Always Encrypted-Sicherheitseinstellungen

### <a name="force-column-encryption"></a>Erzwingen von Spaltenverschlüsselung

Wenn Sie die Verschlüsselung eines Parameters erzwingen möchten, legen Sie das IPD-Feld `SQL_CA_SS_FORCE_ENCRYPT` (Implementierungsparameterdeskriptor) fest, indem Sie die Funktion „SQLSetDescField“ aufrufen. Bei einem Wert ungleich 0 (null) wird ein Fehler zurückgegeben, wenn für den zugeordneten Parameter keine Verschlüsselungsmetadaten zurückgegeben werden.

```cpp
SQLHDESC ipd;
SQLGetStmtAttr(hStmt, SQL_ATTR_IMP_PARAM_DESC, &ipd, 0, 0);
SQLSetDescField(ipd, paramNum, SQL_CA_SS_FORCE_ENCRYPT, (SQLPOINTER)TRUE, SQL_IS_SMALLINT);   
```

Wenn SQL Server den Treiber informiert, dass der Parameter nicht verschlüsselt werden muss, tritt bei Abfragen, die diesen Parameter verwenden, ein Fehler auf. Dieses Verhalten bietet zusätzlichen Schutz vor Angriffen, bei denen ein kompromittierter SQL Server falsche Verschlüsselungsmetadaten für den Client bereitstellt, was zur Offenlegung von Daten führen kann.

### <a name="column-encryption-key-caching"></a>Zwischenspeicherung von Spaltenverschlüsselungsschlüsseln

Der Treiber speichert die Spaltenverschlüsselungsschlüssel (CEKs) im Klartext im Zwischenspeicher, um die Anzahl der Aufrufe an einen Spaltenhauptschlüsselspeicher zum Entschlüsseln von CEKs zu verringern. Der CEK-Cache ist für den Treiber global und keiner Verbindung zugeordnet. Nach dem Erhalt des verschlüsselten CEK (ECEK) aus den Datenbankmetadaten versucht der Treiber zunächst, den CEK im Klartext zu finden, der dem verschlüsselten Schlüsselwert im Cache entspricht. Der Treiber ruft den Schlüsselspeicher, der den CMK enthält, nur auf, wenn der entsprechende CEK im Klartext im Cache nicht gefunden werden kann.

> [!NOTE]
> Bei ODBC Driver for SQL Server werden die Einträge im Cache nach einem Zeitlimit von zwei Stunden entfernt. Dieses Verhalten bedeutet, dass der Treiber den Schlüsselspeicher für einen bestimmten ECEK während der Lebensdauer der Anwendung oder alle zwei Stunden (je nachdem, welches Intervall kürzer ist) nur einmal kontaktiert.

Ab ODBC Driver 17.1 for SQL Server kann das Zeitlimit für den CEK-Cache mithilfe des Verbindungsattributs `SQL_COPT_SS_CEKCACHETTL` angepasst werden. Dabei wird die Anzahl der Sekunden angegeben, die ein CEK im Cache bleiben soll. Da der Cache global ist, kann das Attribut von jedem für den Treiber gültigen Verbindungshandle angepasst werden. Wird die Gültigkeitsdauer des Caches verringert, werden auch vorhandene CEKs, die die neue Gültigkeitsdauer überschreiten würden, entfernt. Bei einer Gültigkeitsdauer von 0 (null) werden keine CEKs zwischengespeichert.

### <a name="trusted-key-paths"></a>Vertrauenswürdige Schlüsselpfade

Ab ODBC Driver 17.1 for SQL Server ist es mithilfe des Verbindungsattributs `SQL_COPT_SS_TRUSTEDCMKPATHS` zulässig, dass eine Anwendung für Always Encrypted-Vorgänge die ausschließliche Verwendung einer angegebenen Liste mit CMKs anfordert, die anhand ihrer Schlüsselpfade identifiziert werden. Die Standardeinstellung für dieses Attribut ist NULL, d. h. der Treiber akzeptiert jeden beliebigen Schlüsselpfad. Wenn Sie diese Funktion verwenden möchten, legen Sie `SQL_COPT_SS_TRUSTEDCMKPATHS` so fest, dass auf eine durch NULL getrennte und mit NULL endende Zeichenfolge mit Breitzeichen verwiesen wird, die den bzw. die zulässigen Schlüsselpfad(e) auflistet. Der Speicher, auf den das Attribut zeigt, muss mithilfe des Verbindungshandles, für das es festgelegt wurde, während der Verschlüsselungs- oder Entschlüsselungsvorgänge gültig bleiben. Auf diesem Weg überprüft der Treiber ohne Beachtung von Groß-/Kleinschreibung, ob der CMK-Pfad wie in den Servermetadaten angegeben in dieser Liste zu finden ist. Befindet sich der CMK-Pfad nicht in der Liste, tritt ein Fehler auf. Der Inhalt des Speichers, auf den das Attribut zeigt, kann durch die Anwendung geändert werden. So kann die Liste der vertrauenswürdigen CMKs geändert werden, ohne das Attribut erneut festzulegen.

## <a name="working-with-column-master-key-stores"></a>Arbeiten mit Spaltenhauptschlüsselspeichern

Der Treiber benötigt zum Verschlüsseln oder Entschlüsseln von Daten einen CEK, der für die Zielspalte konfiguriert ist. CEKs werden in verschlüsselter Form (als ECEKs) in den Datenbankmetadaten gespeichert. Jeder CEK besitzt einen entsprechenden CMK, mit dem er verschlüsselt wurde. In den [Datenbankmetadaten](../../t-sql/statements/create-column-master-key-transact-sql.md) wird nicht der CMK selbst gespeichert, sondern nur der Name des Schlüsselspeichers sowie Informationen für die Suche nach dem CMK.

Wenn der Treiber den Klartextwert eines ECEK abrufen möchten, benötigt er zunächst die Metadaten für den CEK und den entsprechenden CMK. Mit diesen Informationen kontaktiert er den Schlüsselspeicher, der den CMK enthält, und fordert die Entschlüsselung des ECEK an. Die Kommunikation zwischen Treiber und Schlüsselspeicher erfolgt über einen Schlüsselspeicheranbieter.

### <a name="built-in-keystore-providers"></a>Integrierte Schlüsselspeicheranbieter

ODBC Driver for SQL Server verfügt über die folgenden integrierten Schlüsselspeicheranbieter:

| Name | BESCHREIBUNG | Name des Anbieters (Metadaten) |Verfügbarkeit|
|:---|:---|:---|:---|
|Azure-Schlüsseltresor |Speichert CMKs im Azure Key Vault. | `AZURE_KEY_VAULT` |Windows, macOS, Linux|
|Windows-Zertifikatspeicher|Speichert CMKs lokal im Windows-Schlüsselspeicher.| `MSSQL_CERTIFICATE_STORE`|Windows|

- Sie (oder der Datenbankadministrator) müssen sicherstellen, dass der in den Metadaten des Spaltenhauptschlüssels konfigurierte Anbietername richtig ist und der Pfad des Spaltenhauptschlüssels dem Schlüsselpfadformat für den angegebenen Anbieter entspricht. Es wird empfohlen, dass Sie die Schlüssel mithilfe von Tools wie SQL Server Management Studio konfigurieren, die die gültigen Anbieternamen und Schlüsselpfade automatisch generieren, wenn die Anweisung [CREATE COLUMN MASTER KEY (Transact-SQL)](../../t-sql/statements/create-column-master-key-transact-sql.md) ausgegeben wird.

- Stellen Sie sicher, dass die Anwendung auf den Schlüssel im Schlüsselspeicher zugreifen kann. Dieser Vorgang kann das Gewähren des Zugriffs auf den Schlüssel und/oder Schlüsselspeicher für die Anwendung (abhängig vom Schlüsselspeicher) oder das Ausführen anderer wichtiger schlüsselspeicherspezifischer Konfigurationsschritte beinhalten. Zum Zugreifen auf Azure Key Vault müssen Sie beispielsweise im Keystore die korrekten Anmeldeinformationen angeben.

### <a name="using-the-azure-key-vault-provider"></a>Verwenden des Azure Key Vault-Anbieters

Azure Key Vault (AKV) ist eine praktische Möglichkeit zum Speichern und Verwalten von Spaltenhauptschlüsseln für Always Encrypted (insbesondere, wenn Ihre Anwendungen in Azure gehostet werden). Unter Linux, macOS und Windows enthält ODBC Driver for SQL Server einen integrierten CMK-Speicheranbieter für den Azure Key Vault. Weitere Informationen zum Konfigurieren von Azure Key Vault für Always Encrypted finden Sie unter [Ausführliche Anleitung zu Azure Key Vault](/archive/blogs/kv/azure-key-vault-step-by-step), [Erste Schritte mit Azure Key Vault](/azure/key-vault/general/overview) und [Erstellen und Speichern von Spaltenhauptschlüsseln in Azure Key Vault](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md#creating-column-master-keys-in-azure-key-vault).

> [!NOTE]
> Der ODBC-Treiber unterstützt die Azure Key Vault-Authentifizierung nur direkt für Azure Active Directory. Wenn Sie die Azure Active Directory-Authentifizierung bei Azure Key Vault verwenden und Ihre Active Directory-Konfiguration die Authentifizierung für einen Active Directory-Verbunddienstendpunkt erfordert, kann bei der Authentifizierung ein Fehler auftreten.
> Unter Linux und macOS ist ab ODBC Driver 17.2 for SQL Server für die Verwendung dieses Anbieters `libcurl` erforderlich. Dabei handelt es sich jedoch nicht um eine explizite Abhängigkeit, da es für andere Vorgänge mit dem Treiber nicht erforderlich ist. Wenn im Bezug zu `libcurl` ein Fehler auftritt, überprüfen Sie ob es installiert ist.

Der Treiber unterstützt die Authentifizierung beim Azure Key Vault mithilfe der folgenden Typen von Anmeldeinformationen:

- Benutzername/Kennwort: Bei dieser Methode bestehen die Anmeldeinformationen aus dem Namen eines Azure Active Directory-Benutzers und dessen Kennwort.

- Client-ID/Geheimnis: Bei dieser Methode bestehen die Anmeldeinformationen aus einer Anwendungsclient-ID und einem Anwendungsgeheimnis.

- Verwaltete Identität (17.5.2 und höher): entweder vom System oder Benutzer zugewiesen (siehe[Verwaltete Identitäten für Azure-Ressourcen](/azure/active-directory/managed-identities-azure-resources/)).

- Interaktive Azure Key Vault-Authentifizierung (Windows-Treiber mit Version 17.7 und höher): Bei dieser Methode werden die Anmeldeinformationen über Azure Active Directory mit der Anmelde-ID authentifiziert.

Verwenden Sie die folgenden Schlüsselwörter bestehend aus Verbindungszeichenfolgen, um dem Treiber für die Spaltenverschlüsselung die Verwendung der im Azure Key Vault gespeicherten CMKs zu erlauben:

|Anmeldeinformationen|<code>KeyStoreAuthentication</code>|<code>KeyStorePrincipalId</code>|<code>KeyStoreSecret</code>|
|-|-|-|-|
|Benutzername/Kennwort| `KeyVaultPassword`|Benutzerprinzipalname|Kennwort|
|Client-ID/Geheimnis| `KeyVaultClientSecret`|Client-ID|`Secret`|
|Verwaltete Identität|`KeyVaultManagedIdentity`|Objekt-ID (optional, nur bei benutzerseitiger Zuweisung)|(nicht angegeben)|
|Interaktive AKV-Authentifizierung|`KeyVaultInteractive`|(nicht festgelegt)|(nicht festgelegt)|

#### <a name="example-connection-strings"></a>Exemplarische Verbindungszeichenfolgen

Die folgenden Verbindungszeichenfolgen veranschaulichen die Authentifizierung beim Azure Key Vault mit den beiden Anmeldeinformationstypen:

**Client-ID/geheimer Schlüssel**:

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultClientSecret;KeyStorePrincipalId=<clientId>;KeyStoreSecret=<secret>
```

**Benutzername/Kennwort**:

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultPassword;KeyStorePrincipalId=<username>;KeyStoreSecret=<password>
```

**Verwaltete Identität (systemseitig zugewiesen)**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultManagedIdentity
```

**Verwaltete Identität (benutzerseitig zugewiesen)**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultManagedIdentity;KeyStorePrincipalId=<objectID>
```

**Interaktive AKV-Authentifizierung**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultInteractive;UID=<userID>;PWD=<password>
```

Zum Speichern von CMKs im Azure Key Vault sind keine weiteren Änderungen der ODBC-Anwendung erforderlich.

> [!NOTE]
> Der Treiber enthält eine Liste von Azure Key Vault-Endpunkten, denen der Treiber vertraut. Ab der Treiberversion 17.5.2 kann diese Liste konfiguriert werden: Legen Sie die `AKVTrustedEndpoints`-Eigenschaft im Treiber oder im DSN-Registrierungsschlüssel ODBCINST.INI oder ODBC.INI (Windows) oder in den Dateiabschnitten `odbcinst.ini` oder `odbc.ini` (Linux/macOS) auf eine durch Semikolons getrennte Liste fest. Wenn Sie die Eigenschaft im DSN festlegen, hat diese Einstellung Vorrang vor einer Einstellung im Treiber. Wenn der Wert mit einem Semikolon beginnt, wird die Standardliste erweitert. Andernfalls wird die Standardliste ersetzt. Die Standardliste (ab 17.5) ist `vault.azure.net;vault.azure.cn;vault.usgovcloudapi.net;vault.microsoftazure.de`. Ab Version 17.7 enthält die Liste auch `managedhsm.azure.net;managedhsm.azure.cn;managedhsm.usgovcloudapi.net;managedhsm.microsoftazure.de`.

### <a name="using-the-windows-certificate-store-provider"></a>Verwenden des Windows-Zertifikatspeicheranbieters

Unter Windows enthält ODBC Driver for SQL Server einen integrierten CMK-Speicheranbieter für den Windows-Zertifikatspeicher namens `MSSQL_CERTIFICATE_STORE`. (Dieser Anbieter ist nicht unter macOS oder Linux verfügbar.) Mit diesem Anbieter wird der CMK lokal auf dem Clientcomputer gespeichert. Für dessen Verwendung mit dem Treiber ist keine weitere Konfiguration durch die Anwendung erforderlich. Die Anwendung muss jedoch auf das Zertifikat und den privaten Schlüssel im Speicher zugreifen können. Weitere Informationen finden Sie unter [Create and Store Column Master Keys (Always Encrypted)](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)(Erstellen und Speichern von Spaltenhauptschlüsseln (Always Encrypted)).

### <a name="using-custom-keystore-providers"></a>Verwenden benutzerdefinierter Keystore-Anbieter

Über die CEKeystoreProvider-Oberfläche unterstützt ODBC Driver for SQL Server auch benutzerdefinierte Schlüsselspeicheranbieter von Drittanbietern. Mit dieser Funktion kann eine Anwendung Schlüsselspeicheranbieter laden, abfragen und konfigurieren, damit sie vom Treiber zum Zugreifen auf verschlüsselte Spalten verwendet werden können. Anwendungen können auch direkt mit einem Schlüsselspeicheranbieter interagieren, um CEKs für die Speicherung in SQL Server zu verschlüsseln und weitere Aufgaben auszuführen, die über das Zugreifen auf verschlüsselte Spalten mit ODBC hinausgehen. Weitere Informationen finden Sie unter [Custom Keystore Providers (Benutzerdefinierte Schlüsselspeicheranbieter)](custom-keystore-providers.md).

Zur Interaktion mit benutzerdefinierten Schlüsselspeicheranbietern werden zwei Verbindungsattribute verwendet. Sie lauten wie folgt:

- `SQL_COPT_SS_CEKEYSTOREPROVIDER`

- `SQL_COPT_SS_CEKEYSTOREDATA`

Das erste Attribut wird zum Laden und Aufzählen geladener Schlüsselspeicheranbieter verwendet, das zweite für die Kommunikation zwischen Anwendung und Anbieter. Diese Verbindungsattribute können jederzeit, also vor und nach dem Herstellen einer Verbindung, verwendet werden, da die Interaktion zwischen Anwendung und Anbieter keine Kommunikation mit SQL Server beinhaltet. Da der Treiber jedoch noch nicht geladen wurde, werden die Attribute durch das Festlegen und Abrufen vor einer Verbindungsherstellung vom Treiber-Manager verarbeitet, was möglicherweise nicht zu den erwarteten Ergebnissen führt.

#### <a name="loading-a-keystore-provider"></a>Laden eines Schlüsselspeicheranbieters

Durch das Festlegen des Verbindungsattributs `SQL_COPT_SS_CEKEYSTOREPROVIDER` kann eine Clientanwendung eine Anbieterbibliothek laden und die darin enthaltenen Schlüsselspeicheranbieter bereitstellen.

```cpp
SQLRETURN SQLSetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER StringLength);
```

| Argument | BESCHREIBUNG |
|:---|:---|
|`ConnectionHandle`|[Eingabe] Verbindungshandle. Es muss sich um ein gültiges Verbindungshandle handeln, jedoch kann auf Anbieter, die über ein Verbindungshandle geladen werden, von jedem anderen Handle im selben Vorgang aus zugegriffen werden.|
|`Attribute`|[Eingabe] Festzulegendes Attribut: die Konstante `SQL_COPT_SS_CEKEYSTOREPROVIDER`.|
|`ValuePtr`|[Eingabe] Zeiger auf eine Zeichenfolge, die mit NULL endet und den Dateinamen der Anbieterbibliothek angibt. Bei „SQLSetConnectAttrA“ ist dieser Wert eine ANSI (Multibyte)-Zeichenfolge. Bei „SQLSetConnectAttrW“ ist dieser Wert eine Unicode-Zeichenfolge (vom Typ „wchar_t“).|
|`StringLength`|[Eingabe] Länge der Zeichenfolge „ValuePtr“ oder von „SQL_NTS“.|

Der Treiber versucht, mithilfe des von der Plattform definierten Lademechanismus für dynamische Bibliotheken (`dlopen()` unter Linux und macOS, `LoadLibrary()` unter Windows) die vom ValuePtr-Parameter identifizierte Bibliothek zu laden. Anschließend werden alle darin definierten Anbieter der Liste mit Anbietern hinzugefügt, die dem Treiber bekannt sind. Die folgenden Fehler können auftreten:

| Fehler | BESCHREIBUNG |
|:--|:--|
|`CE203`|Die dynamische Bibliothek konnte nicht geladen werden.|
|`CE203`|Das exportierte Symbol „CEKeyStoreProvider“ wurde in der Bibliothek nicht gefunden.|
|`CE203`|Ein oder mehrere Anbieter in der Bibliothek wurden bereits geladen.|

`SQLSetConnectAttr` gibt die üblichen Fehler- oder Erfolgswerte zurück. Weitere Informationen sind zu allen aufgetretenen Fehlern sind über den ODBC-Standarddiagnosemechanismus verfügbar.

> [!NOTE]
> Der Programmierer der Anwendung muss sicherstellen, dass alle benutzerdefinierten Anbieter geladen werden, bevor über eine beliebige Verbindung eine Abfrage gesendet wird, mit der die Anbieter angefordert werden. Andernfalls wird folgender Fehler ausgelöst:

| Fehler | BESCHREIBUNG |
|:--|:--|
|`CE200`|Schlüsselspeicheranbieter %1 wurde nicht gefunden. Stellen Sie sicher, dass die entsprechende Bibliothek für Schlüsselspeicheranbieter geladen wurde.|

> [!NOTE]
> Ausführende des Schlüsselspeicheranbieters sollten es vermeiden, `MSSQL` im Namen der benutzerdefinierten Anbieter zu verwenden. Dieser Begriff ist für die ausschließliche Verwendung mit Microsoft reserviert, und seine Verwendung könnte zu Konflikten mit künftigen integrierten Anbietern führen. Die Verwendung des Begriffs im Namen eines benutzerdefinierten Anbieters könnte eine ODBC-Warnung verursachen.

#### <a name="getting-the-list-of-loaded-providers"></a>Abrufen der Liste mit geladenen Anbietern

Durch Abrufen des Verbindungsattributs kann eine Clientanwendung die aktuell im Treiber geladenen Schlüsselspeicheranbieter (einschließlich der integrierten Anbieter) bestimmen. Dieser Vorgang ist erst nach dem Herstellen einer Verbindung möglich.

```cpp
SQLRETURN SQLGetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER BufferLength, SQLINTEGER * StringLengthPtr);
```

| Argument | BESCHREIBUNG |
|:---|:---|
|`ConnectionHandle`|[Eingabe] Verbindungshandle. Es muss sich um ein gültiges Verbindungshandle handeln, jedoch kann auf Anbieter, die über ein Verbindungshandle geladen werden, von jedem anderen Handle im selben Vorgang aus zugegriffen werden.|
|`Attribute`|[Eingabe] Abzurufendes Attribut: die Konstante `SQL_COPT_SS_CEKEYSTOREPROVIDER`.|
|`ValuePtr`|[Ausgabe] Zeiger auf den Speicher, an den der nächste geladene Anbietername zurückgegeben werden soll.|
|`BufferLength`|[Eingabe] Länge des Puffers „ValuePtr“.|
|`StringLengthPtr`|[Ausgabe] Zeiger auf einen Puffer, an den die Gesamtzahl der zur Rückgabe in \*ValuePtr verfügbaren Bytes (ausgenommen das mit NULL endende Zeichen) zurückgegeben wird. Handelt es sich bei „ValuePtr“ um einen NULL-Zeiger, wird keine Länge zurückgegeben. Handelt es sich bei dem Attributwert um eine Zeichenfolge und ist die Anzahl der zur Rückgabe verfügbaren Bytes größer als die Pufferlänge abzüglich der Länge des mit NULL endenden Zeichens, werden die Daten in \*ValuePtr auf diese Länge gekürzt und vom Treiber mit NULL beendet.|

Damit die gesamte Liste abgerufen werden kann, wird bei jedem GET-Vorgang der Name des aktuellen Anbieters zurückgegeben und der interne Zähler auf den nächsten Anbieter vorgerückt. Ist das Ende der Liste erreicht, wird eine leere Zeichenfolge ("") zurückgegeben und der Zähler zurückgesetzt. Nachfolgende GET-Vorgänge beginnen dann wieder am Anfang der Liste.

### <a name="communicating-with-keystore-providers"></a>Kommunizieren mit Schlüsselspeicheranbietern

Mit dem Verbindungsattribut `SQL_COPT_SS_CEKEYSTOREDATA` kann eine Clientanwendung mit geladenen Schlüsselspeicheranbietern bezüglich der Konfiguration von zusätzlichen Parametern, Schlüsselerstellungsmaterial usw. kommunizieren. Die Kommunikation zwischen einer Clientanwendung und einem Anbieter folgt einem einfachen Protokoll aus Anforderung und Antwort basierend auf GET- und SET-Anforderungen, die dieses Verbindungsattribut verwenden. Die Kommunikation geht immer von der Clientanwendung aus.

> [!NOTE]
> Aufgrund der Art der Antwort von ODBC-Aufrufen über „CEKeyStoreProvider“ auf (SQLGet/SetConnectAttr) unterstützt die ODBC-Oberfläche nur das Festlegen von Daten bei der Auflösung des Verbindungskontexts.

Die Kommunikation zwischen Anwendung und Schlüsselspeicheranbietern erfolgt über den Treiber mithilfe der CEKeystoreData-Struktur:

```cpp
typedef struct CEKeystoreData {
wchar_t *name;
unsigned int dataSize;
char data[];
} CEKEYSTOREDATA;
```

| Argument | BESCHREIBUNG |
|:---|:---|
|`name`|[Eingabe] Bei SET-Vorgängen: Name des Anbieters, an den die Daten gesendet werden. Wird bei GET-Vorgängen ignoriert. Auf NULL endende Zeichenfolge für breite Zeichen.|
|`dataSize`|[Eingabe] Größe des Datenarrays gemäß der Struktur.|
|`data`|[Eingabe/Ausgabe] Bei SET-Vorgängen: Daten, die an den Anbieter gesendet werden sollen. Dabei kann es sich um willkürliche Daten handeln. Der Treiber versucht nicht, die Daten zu interpretieren. Bei GET-Vorgängen: Puffer, der die gelesenen Daten vom Anbieter empfängt.|

#### <a name="writing-data-to-a-provider"></a>Schreiben von Daten an einen Anbieter

Bei einem mithilfe des Attributs `SQL_COPT_SS_CEKEYSTOREDATA` ausgeführten Aufrufs von `SQLSetConnectAttr` wird ein Datenpaket an den angegebenen Schlüsselspeicheranbieter geschrieben.

```cpp
SQLRETURN SQLSetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER StringLength);
```

| Argument | BESCHREIBUNG |
|:---|:---|
|`ConnectionHandle`| [Eingabe] Verbindungshandle. Es muss sich um ein gültiges Verbindungshandle handeln, jedoch kann auf Anbieter, die über ein Verbindungshandle geladen werden, von jedem anderen Handle im selben Vorgang aus zugegriffen werden.|
|`Attribute`|[Eingabe] Festzulegendes Attribut: die Konstante `SQL_COPT_SS_CEKEYSTOREDATA`.|
|`ValuePtr`|[Eingabe] Zeiger auf eine CEKeystoreData-Struktur. Der Anbieter, für den die Daten vorgesehen sind, wird vom Namensfeld der Struktur identifiziert.|
|`StringLength`|[Eingabe] Konstante „SQL_IS_POINTER“.|

Weitere detaillierte Fehlerinformationen können über [SQLGetDiacRec](../../odbc/reference/syntax/sqlgetdescrec-function.md) abgerufen werden.

> [!NOTE]
> Der Anbieter kann mithilfe des Verbindungshandles, sofern gewünscht, die geschriebenen Daten einer bestimmten Verbindung zuordnen. Diese Funktion ist beim Implementieren von Konfigurationen für einzelne Verbindungen nützlich. Außerdem kann er den Verbindungskontext ignorieren und alle Daten identisch verarbeiten, unabhängig davon, welche Verbindung zum Senden der Daten verwendet wurde. Weitere Informationen finden Sie unter [Kontextzuordnung](custom-keystore-providers.md#context-association).

#### <a name="reading-data-from-a-provider"></a>Lesen von Daten von einem Anbieter

Bei einem mithilfe des Attributs `SQL_COPT_SS_CEKEYSTOREDATA` ausgeführten Aufrufs von `SQLGetConnectAttr` wird ein Datenpaket vom Anbieter gelesen, *an den zuletzt geschrieben wurde*. Ist kein solcher Anbieter vorhanden, tritt ein Fehler in der Funktionsreihenfolge auf. Für Ausführende des Schlüsselspeicheranbieters ist es sinnvoll, Pseudoschreibvorgänge mit 0 Bytes zu unterstützen. Damit kann der Anbieter ohne weitere Auswirkungen für Schreibvorgänge ausgewählt werden.

```cpp
SQLRETURN SQLGetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER BufferLength, SQLINTEGER * StringLengthPtr);
```

| Argument | BESCHREIBUNG |
|:---|:---|
|`ConnectionHandle`|[Eingabe] Verbindungshandle. Es muss sich um ein gültiges Verbindungshandle handeln, jedoch kann auf Anbieter, die über ein Verbindungshandle geladen werden, von jedem anderen Handle im selben Vorgang aus zugegriffen werden.|
|`Attribute`|[Eingabe] Abzurufendes Attribut: die Konstante `SQL_COPT_SS_CEKEYSTOREDATA`.|
|`ValuePtr`|[Ausgabe] Zeiger auf eine CEKeystoreData-Struktur, in die die vom Anbieter gelesenen Daten platziert werden.|
|`BufferLength`|[Eingabe] Konstante „SQL_IS_POINTER“.|
|`StringLengthPtr`|[Ausgabe] Zeiger auf einen Puffer, an den die Pufferlänge zurückgegeben werden soll. Handelt es sich bei *ValuePtr um einen NULL-Zeiger, wird keine Länge zurückgegeben.|

Der Aufrufer muss sicherstellen, dass dem Anbieter zum Schreiben der Daten ein Puffer mit ausreichender Länge gemäß der CEKEYSTOREDATA-Struktur zur Verfügung steht. Bei der Rückgabe wird das Feld „dataSize“ mit der tatsächlichen Länge der vom Anbieter gelesenen Daten aktualisiert. Weitere detaillierte Fehlerinformationen können über [SQLGetDiacRec](../../odbc/reference/syntax/sqlgetdescrec-function.md) abgerufen werden.

Die Oberfläche stellt keine weiteren Anforderungen an das Format der zwischen Anwendung und Schlüsselspeicheranbieter übertragenen Daten. Jeder Anbieter kann je nach Anforderungen ein eigenes Protokoll bzw. Datenformat definieren.

Ein Beispiel für das Implementieren eines eigenen Schlüsselspeicheranbieters finden Sie unter [Custom Keystore Providers (Benutzerdefinierte Schlüsselspeicheranbieter)](custom-keystore-providers.md).

## <a name="limitations-of-the-odbc-driver-when-using-always-encrypted"></a>Einschränkungen des ODBC-Treibers bei Verwendung von Always Encrypted

### <a name="asynchronous-operations"></a>Asynchrone Vorgänge

Zwar lässt der Treiber die Verwendung von [asynchronen Vorgängen](../../relational-databases/native-client/odbc/creating-a-driver-application-asynchronous-mode-and-sqlcancel.md) mit Always Encrypted zu, doch kommt es zu Leistungsauswirkungen auf die Vorgänge, wenn Always Encrypted aktiviert ist. Der Aufruf von `sys.sp_describe_parameter_encryption` zum Bestimmen von Verschlüsselungsmetadaten für die Anweisung blockiert, weshalb der Treiber auf die Rückgabe der Metadaten vom Server warten muss, bevor er `SQL_STILL_EXECUTING` zurückgibt.

### <a name="retrieve-data-in-parts-with-sqlgetdata"></a>Abrufen von Teildaten mit „SQLGetData“

Vor ODBC Driver 17 for SQL Server konnten verschlüsselte Zeichen und Binärspalten mit „SQLGetData“ nicht in Teilen abgerufen werden. Es kann nur ein Aufruf von „SQLGetData“ vorgenommen werden, der einen Puffer mit ausreichender Länge für die gesamten Daten der Spalte enthält.

### <a name="send-data-in-parts-with-sqlputdata"></a>Senden von Teildaten mit „SQLPutData“

Vor ODBC Driver 17.3 for SQL Server war es nicht möglich, Teildaten mit „SQLPutData“ zum Einfügen oder Vergleichen zu senden. Es kann nur ein Aufruf von „SQLPutData“ vorgenommen werden, der einen Puffer mit allen Daten enthält. Wenn Sie Long-Daten in verschlüsselte Spalten einfügen möchten, verwenden Sie die API für das Massenkopieren mit einer Eingabedatendatei, die im nächsten Abschnitt beschrieben wird.

### <a name="encrypted-money-and-smallmoney"></a>Verschlüsselte Daten vom Typ „money“ und „smallmoney“

Parameter können nicht auf verschlüsselte Spalten vom Typ **money** oder **smallmoney** ausgerichtet sein, da es keinen bestimmten ODBC-Datentyp gibt, der diesen Typen zugeordnet ist. Daher treten in diesem Fall Fehler vom Typ „Operandentypkollision“ auf.

## <a name="bulk-copy-of-encrypted-columns"></a>Massenkopieren von verschlüsselten Spalten

Ab ODBC Driver 17 for SQL Server wird die Verwendung der [SQL-Massenkopierfunktionen](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md) und des **BCP-Hilfsprogramms** mit Always Encrypted unterstützt. Mit den APIs für das Massenkopieren (bcp_&#42;) und dem **BCP-Hilfsprogramm** können Sie sowohl Klartext (beim Einfügen in verschlüsselter Form, beim Abrufen entschlüsselt) als auch Chiffretext (im genauen Wortlaut übertragen) einfügen und abrufen.

- Wenn Sie Chiffretext im Format „varbinary(max)“ abrufen möchten (z. B. beim Massenladen in eine andere Datenbank), stellen Sie ohne die Option `ColumnEncryption` eine Verbindung her (oder legen Sie sie auf `Disabled` fest), und führen Sie einen BCP OUT-Vorgang aus.

- Wenn Sie Klartext einfügen und abrufen möchten und den Treiber die erforderliche Verschlüsselung und Entschlüsselung transparent ausführen lassen, ist es ausreichend, `ColumnEncryption` auf `Enabled` festzulegen. Die sonstigen Funktionen der BCP-API bleiben unverändert.

- Wenn Sie Chiffretext im Format „varbinary(max)“ einfügen möchten (z. B. wie beim Abrufen oben), legen Sie die Option `BCPMODIFYENCRYPTED` auf TRUE fest, und führen Sie einen BCP IN-Vorgang aus. Damit die resultierenden Daten entschlüsselt werden können, vergewissern Sie sich, dass der CEK der Zielspalte und der CEK der ursprünglichen Spalte, aus der der Chiffretext abgerufen wurde, identisch sind.

Gehen Sie bei Verwendung des **bcp**-Hilfsprogramms folgendermaßen vor: Um die Einstellung `ColumnEncryption` zu steuern, verwenden Sie die Option „-D“, und geben Sie einen DSN mit dem gewünschten Wert an. Vergewissern Sie sich, dass zum Einfügen von Chiffretext die Einstellung `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` des Benutzers aktiviert ist.

In der folgenden Tabelle werden die Aktionen beim Verarbeiten einer verschlüsselten Spalte zusammengefasst:

|<code>ColumnEncryption</code>|BCP-Richtung|BESCHREIBUNG|
|----------------|-------------|-----------|
|`Disabled`|OUT (zum Client)|Ruft Chiffretext ab. Der observierte Datentyp lautet **varbinary(max)** .|
|`Enabled`|OUT (zum Client)|Ruft Klartext ab. Die Spaltendaten werden vom Treiber entschlüsselt.|
|`Disabled`|IN (zum Server)|Fügt Chiffretext ein. Diese Einstellung wird für sichtbar verschobene verschlüsselte Daten verwendet, die nicht entschlüsselt werden müssen. Es tritt dabei ein Fehler auf, wenn die Option `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` beim Benutzer nicht festgelegt ist oder „BCPMODIFYENCRYPTED“ beim Verbindungshandle nicht festgelegt ist. Weitere Informationen finden Sie unten.|
|`Enabled`|IN (zum Server)|Fügt Klartext ein. Die Spaltendaten werden vom Treiber verschlüsselt.|

### <a name="the-bcpmodifyencrypted-option"></a>Option „BCPMODIFYENCRYPTED“

Damit die Daten nicht beschädigt werden, lässt der Server normalerweise das direkte Einfügen von Chiffretext in eine verschlüsselte Spalte nicht zu, und bei einem solchen Versuch tritt ein Fehler auf. Beim Massenladen von verschlüsselten Daten mithilfe der BCP-API kann Chiffretext jedoch direkt eingefügt werden, wenn die [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md)-Option `BCPMODIFYENCRYPTED` auf „true“ festgelegt wird. Damit ist das Risiko einer Beschädigung von verschlüsselten Daten geringer als beim Festlegen der Option `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` für das Benutzerkonto. Trotzdem müssen die Schlüssel mit den Daten übereinstimmen, weshalb es sinnvoll ist, nach dem Masseneinfügen und vor der weiteren Verwendung schreibgeschützte Überprüfungen der eingefügten Daten vorzunehmen.

Weitere Informationen finden Sie unter [Migrieren von durch Always Encrypted geschützten sensiblen Daten](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="always-encrypted-api-summary"></a>Zusammenfassung der Always Encrypted-API

### <a name="connection-string-keywords"></a>Schlüsselwörter für Verbindungszeichenfolgen

|Name|BESCHREIBUNG|  
|----------|-----------------|  
|`ColumnEncryption`|Zulässige Werte sind `Enabled`/`Disabled`.<br>`Enabled`: aktiviert Always Encrypted-Funktionen für die Verbindung.<br>`Disabled`: deaktiviert Always Encrypted-Funktionen für die Verbindung.<br>*Nachweisprotokoll*,*Nachweis-URL* (Version 17.4 und höher): bietet Always Encrypted mit Secure Enclave die Möglichkeit, das angegebene Nachweisprotokoll und die Nachweis-URL zu verwenden. <br><br>Der Standardwert lautet `Disabled`.|
|`KeyStoreAuthentication` | Gültige Werte:`KeyVaultPassword`, `KeyVaultClientSecret`, `KeyVaultInteractive` |
|`KeyStorePrincipalId` | Wenn `KeyStoreAuthentication` = `KeyVaultPassword`, legen Sie diesen Wert auf einen gültigen Benutzerprinzipalnamen in Azure Active Directory fest. <br>Wenn `KeyStoreAuthetication` = `KeyVaultClientSecret`, legen Sie diesen Wert auf eine gültige Anwendungsclient-ID in Azure Active Directory fest. |
|`KeyStoreSecret` | Wenn `KeyStoreAuthentication` = `KeyVaultPassword`, legen Sie diesen Wert auf das Kennwort für den entsprechenden Benutzernamen fest. <br>Wenn `KeyStoreAuthentication` = `KeyVaultClientSecret`, legen Sie diesen Wert auf das Anwendungsgeheimnis fest, das einer gültigen Anwendungsclient-ID in Azure Active Directory zugeordnet wird. |

### <a name="connection-attributes"></a>Verbindungsattribute

|Name|Typ|BESCHREIBUNG|  
|----------|-------|----------|  
|`SQL_COPT_SS_COLUMN_ENCRYPTION`|Vor dem Herstellen einer Verbindung|`SQL_COLUMN_ENCRYPTION_DISABLE` (0): Always Encrypted wird deaktiviert. <br>`SQL_COLUMN_ENCRYPTION_ENABLE` (1): Always Encrypted wird aktiviert.<br> Zeiger auf die Zeichenfolge *Nachweisprotokoll*,*Nachweis-URL* (Version 17.4 und höher) aktiviert die Verwendung mit Secure Enclave|
|`SQL_COPT_SS_CEKEYSTOREPROVIDER`|Nach dem Herstellen einer Verbindung|[SET]: Es wird versucht, „CEKeystoreProvider“ zu laden.<br>[GET]: Ein CEKeystoreProvider-Name wird zurückgegeben.|
|`SQL_COPT_SS_CEKEYSTOREDATA`|Nach dem Herstellen einer Verbindung|[SET]: Daten werden in „CEKeystoreProvider“ geschrieben.<br>[GET]: Daten werden aus „CEKeystoreProvider“ gelesen.|
|`SQL_COPT_SS_CEKCACHETTL`|Nach dem Herstellen einer Verbindung|[SET]: Gültigkeitsdauer des CEK-Caches wird festgelegt.<br>[GET]: Gültigkeitsdauer des aktuellen CEK-Caches wird abgerufen.|
|`SQL_COPT_SS_TRUSTEDCMKPATHS`|Nach dem Herstellen einer Verbindung|[SET]: Zeiger auf vertrauenswürdige CMK-Pfade wird festgelegt.<br>[GET]: Zeiger auf aktuelle vertrauenswürdige CMK-Pfade wird abgerufen.|

### <a name="statement-attributes"></a>Anweisungsattribute

|Name|BESCHREIBUNG|  
|----------|-----------------|  
|`SQL_SOPT_SS_COLUMN_ENCRYPTION`|`SQL_CE_DISABLED` (0): Always Encrypted ist für die Anweisung deaktiviert. <br>`SQL_CE_RESULTSETONLY` (1): Nur Entschlüsselung. Resultsets und Rückgabewerte werden entschlüsselt, Parameter werden nicht verschlüsselt. <br>`SQL_CE_ENABLED` (3): Always Encrypted ist aktiviert und wird für Parameter und Ergebnisse verwendet.|

### <a name="descriptor-fields"></a>Deskriptorfelder

|IPD-Feld|Größe/Typ|Standardwert|BESCHREIBUNG|
|-|-|-|-|  
|`SQL_CA_SS_FORCE_ENCRYPT` (1236)|WORD (2 Bytes)|0|Bei 0 (Standardeinstellung): Entscheidung, ob dieser Parameter verschlüsselt wird, basiert auf der Verfügbarkeit von Verschlüsselungsmetadaten.<br><br>Bei ungleich 0: Parameter wird verschlüsselt, wenn Verschlüsselungsmetadaten dafür verfügbar sind. Andernfalls verursacht die Anforderung den Fehler [CE300] [Microsoft][ODBC Driver 17 for SQL Server]: Die obligatorische Verschlüsselung wurde für einen Parameter angegeben, es wurden vom Server jedoch keine Verschlüsselungsmetadaten bereitgestellt.|

### <a name="bcp_control-options"></a>Optionen für „bcp_control“

|Optionsname|Standardwert|BESCHREIBUNG|
|-|-|-|
|`BCPMODIFYENCRYPTED` (21)|FALSE|Bei TRUE können Werte vom Typ „varbinary(max)“ in eine verschlüsselte Spalte eingefügt werden. Bei FALSE wird das Einfügen verhindert, solange die korrekten Typ- und Verschlüsselungsmetadaten nicht bereitgestellt werden.|

## <a name="troubleshooting"></a>Problembehandlung

Wenn bei der Verwendung von Always Encrypted Probleme auftreten, überprüfen Sie zunächst folgende Punkte:

- Der CEK, der die gewünschte Spalte verschlüsselt, ist auf dem Server vorhanden und zugänglich.

- Der CMK, der den CEK verschlüsselt, verfügt über zugängliche Metadaten auf dem Server, und der Client kann auf den CMK zugreifen.

- `ColumnEncryption` ist im DSN, in der Verbindungszeichenfolge oder im Verbindungsattribut aktiviert und weist bei Verwendung von Secure Enclave das richtige Format auf.

Darüber hinaus identifizieren Nachweisfehler bei Verwendung von Secure Enclave den Schritt im Nachweisprozess, in dem der Fehler aufgetreten ist. Weitere Informationen finden Sie in der folgenden Tabelle:

|Schritt|BESCHREIBUNG|
|----|-----------|
|0–99| Ungültige Nachweisantwort oder Fehler bei der Signaturüberprüfung. |
|100–199| Fehler beim Abrufen von Zertifikaten aus der Nachweis-URL. Stellen Sie sicher, dass `<attestation URL>/v2.0/signingCertificates` gültig und zugänglich ist. |
|200–299| Unerwartetes oder falsches Format der Identität der Enclave. |
|300–399| Fehler beim Einrichten eines sicheren Kanals mit einer Enclave. |

## <a name="see-also"></a>Weitere Informationen

- [„Immer verschlüsselt“ (Datenbank-Engine)](../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md)
