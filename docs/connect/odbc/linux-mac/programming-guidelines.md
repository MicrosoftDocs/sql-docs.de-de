---
title: Programmierrichtlinien (ODBC Driver)
description: Die Features für die Programmierung des Microsoft ODBC Driver for SQL Server unter macOS und Linux basieren auf ODBC in SQL Server Native Client (ODBC).
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
author: v-makouz
ms.author: v-daenge
ms.openlocfilehash: 6d602db9c189b6e7fce8b767b60204253ccb7f67
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464393"
---
# <a name="programming-guidelines"></a>Programmierrichtlinien

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Die Features für die Programmierung des [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] unter macOS und Linux basieren auf ODBC in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ([SQL Server Native Client (ODBC)](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)). [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client basiert auf ODBC in Windows Data Access Components ([Referenz für ODBC-Programmierer](../../../odbc/reference/odbc-programmer-s-reference.md)).  

Eine ODBC-Anwendung kann Multiple Active Result Sets (MARS) und andere [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-spezifische Features verwenden, indem `/usr/local/include/msodbcsql.h` nach den unixODBC-Headern (`sql.h`, `sqlext.h`, `sqltypes.h` und `sqlucode.h`) eingefügt wird. Verwenden Sie anschließend die gleichen symbolischen Namen für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-spezifische Elemente, die Sie auch in Ihrer Windows-ODBC-Anwendung verwenden würden.

## <a name="available-features"></a>Verfügbare Funktionen  
Die folgenden Abschnitte aus der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client Dokumentation für ODBC ([SQL Server Native Client (ODBC)](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)) sind gültig, wenn der ODBC-Treiber unter macOS und Linux verwendet wird:  

-   [Kommunikation mit SQL Server (ODBC)](../../../relational-databases/native-client-odbc-communication/communicating-with-sql-server-odbc.md)  
-   [Verbindungs- und Abfragetimeout-Unterstützung](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)  
-   [Cursor](../../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md)  
-   [Datums-/Uhrzeitverbesserungen (ODBC)](../../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)  
-   [Ausführen von Abfragen (ODBC)](../../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
-   [Behandlung von Fehlern und Meldungen](../../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
-   [Kerberos-Authentifizierung](../../../relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections.md)  
-   [Große benutzerdefinierte CLR-Typen (ODBC)](../../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)  
-   [Ausführen von Transaktionen (ODBC) (mit Ausnahme der verteilten Transaktionen)](../../../relational-databases/native-client/odbc/performing-transactions-in-odbc.md)  
-   [Verarbeiten von Ergebnissen (ODBC)](../../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
-   [Ausführen gespeicherter Prozeduren](../../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)
-   [Unterstützung für Spalten mit geringer Dichte (ODBC)](../../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md)
-   [Verwenden von Verschlüsselung ohne Überprüfung](../../../relational-databases/native-client/features/using-encryption-without-validation.md)
-   [Table valued parameters (Tabellenwertparameter)](../../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)
-   [UTF-8 und UTF-16 für die Befehls- und Daten-API](../../../relational-databases/native-client/features/utf-16-support-in-sql-server-native-client-11-0.md)
-   [Verwenden von Katalogfunktionen](../../../relational-databases/native-client/odbc/using-catalog-functions.md)  

## <a name="unsupported-features"></a>Nicht unterstützte Funktionen

Die Funktionsweise der folgenden Features wurde in diesem Release des ODBC-Treibers für macOS und Linux überprüft:

-   Failoverclusterverbindung
-   [Transparente Netzwerk-IP-Adressauflösung](../using-transparent-network-ip-resolution.md) (vor ODBC-Treiber 17)
-   [Erweiterte Treiberablaufverfolgung](/archive/blogs/mattn/enabling-advanced-driver-tracing-for-the-sql-native-client-odbc-drivers)

Die folgenden Features sind nicht in dieser Version des ODBC-Treibers für macOS und Linux verfügbar: 

-   Verteilte Transaktionen (das Attribut SQL_ATTR_ENLIST_IN_DTC wird nicht unterstützt)  
-   Datenbankspiegelung  
-   FILESTREAM  
-   Leistungsprofilerstellung des ODBC-Treibers, erläutert in [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md), sowie die folgenden leistungsbezogenen Verbindungsattribute:  
    -   SQL_COPT_SS_PERF_DATA  
    -   SQL_COPT_SS_PERF_DATA_LOG  
    -   SQL_COPT_SS_PERF_DATA_LOG_NOW  
    -   SQL_COPT_SS_PERF_QUERY  
    -   SQL_COPT_SS_PERF_QUERY_INTERVAL  
    -   SQL_COPT_SS_PERF_QUERY_LOG  
-   SQLBrowseConnect (vor Version 17.2)
-   C-Intervall-Typen, wie z.B. SQL_C_INTERVAL_YEAR_TO_MONTH (dokumentiert in [-Datentypbezeichnungen und Deskriptoren](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md)), werden derzeit nicht unterstützt.
-   Der Wert SQL_CUR_USE_ODBC des Attributs SQL_ATTR_ODBC_CURSORS der Funktion SQLSetConnectAttr.

## <a name="character-set-support"></a>Zeichensatzunterstützung

Für ODBC-Treiber 13 und 13.1 müssen die SQLCHAR-Daten die UTF-8-Codierung aufweisen. Es werden keine weiteren Codierungen unterstützt.

Für ODBC-Treiber 17 werden die folgenden Zeichensätze bzw. Codierungen für SQLCHAR-Daten unterstützt:

> [!NOTE]  
> Aufgrund von Unterschieden bei `iconv` in `musl` und `glibc` werden viele dieser Gebietsschemas unter Alpine Linux nicht unterstützt.
>
> Weitere Informationen finden Sie unter [Functional differences from glibc](https://wiki.musl-libc.org/functional-differences-from-glibc.html) (Funktionsbezogene Unterschiede zu glibc).

|Name|BESCHREIBUNG|
|-|-|
|UTF-8|Unicode|
|CP437|MS-DOS-Latin-US|
|CP850|MS-DOS-Latin-1|
|CP874|Lateinisch/Thailändisch|
|CP932|Japanisch (Shift_JIS)|
|CP936|Chinesisch (vereinfacht) (GBK)|
|CP949|Koreanisch, EUC-KR|
|CP950|Chinesisch (traditionell) (Big5)|
|CP1251|Kyrillisch|
|CP1253|Griechisch|
|CP1256|Arabisch|
|CP1257|Baltisch|
|CP1258|Vietnamesisch|
|ISO-8859-1 / CP1252|Latin-1|
|ISO-8859-2 / CP1250|Latin-2|
|ISO-8859-3|Latin-3|
|ISO-8859-4|Latin-4|
|ISO-8859-5|Lateinisch/Kyrillisch|
|ISO-8859-6|Lateinisch/Arabisch|
|ISO-8859-7|Lateinisch/Griechisch|
|ISO-8859-8 / CP1255|Hebräisch|
|ISO-8859-9 / CP1254|Türkisch|
|ISO-8859-13|Latin-7|
|ISO-8859-15|Latin-9|

Wenn die Verbindung hergestellt wird, ermittelt der Treiber das aktuelle Gebietsschema des Prozesses, in dem er geladen wurde. Wenn eine der oben aufgeführten Codierungen verwendet wird, verwendet der Treiber diese Codierung für SQLCHAR-Daten (schmale Zeichen). Andernfalls wird standardmäßig die UTF-8-Codierung verwendet. Da alle Prozesse standardmäßig im Gebietsschema „C“ gestartet werden (und der Treiber daher standardmäßig die UTF-8-Codierung verwendet), sollte die Funktion **setlocale** zum ordnungsgemäßen Festlegen des Gebietsschemas verwendet werden, bevor die Verbindung hergestellt wird. Hierzu muss entweder das Gebietsschema explizit festgelegt oder eine leere Zeichenfolge (z. B. `setlocale(LC_ALL, "")`) verwendet werden, um die Gebietsschemaeinstellungen der Umgebung zu verwenden.

Daher werden Benutzern des ODBC-Treibers 17, die ein Upgrade von Version 13 oder 13.1 ausgeführt haben, in herkömmlichen Linux- oder macOS-Umgebungen keine Unterschiede auffallen, wenn die UTF-8-Codierung verwendet wird. Allerdings müssen Anwendungen, für die mithilfe von `setlocale()` eine andere Codierung aus der obigen Liste festgelegt wurde, diese Codierung anstelle der UTF-8-Codierung für Daten für und aus dem Treiber verwenden.

SQLWCHAR-Daten müssen UTF-16LE (Little Endian) sein.

Wenn ein SQL-Typ mit schmalen Zeichen wie SQL_VARCHAR festgelegt ist und Eingabeparameter mit SQLBindParameter eingebunden werden, konvertiert der Treiber die bereitgestellten Daten aus der Codierung des Clients in die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Standardcodierung (in der Regel Codepage 1252). Für Ausgabeparameter konvertiert der Treiber die in den Sortierungsinformationen festgelegte Codierung, die den Daten zugeordnet ist, in die Codierung des Clients. Es ist jedoch Datenverlust möglich. Zeichen der Quellcodierung, die nicht in der Zielcodierung angezeigt werden können, werden in Fragezeichen („?“) konvertiert.

Legen Sie einen Unicode-SQL-Zeichentyp wie SQL_NVARCHAR fest, um diese Datenverluste beim Einbinden von Eingabeparametern zu vermeiden. In diesem Fall konvertiert der Treiber die Codierung des Clients in die UTF-16-Codierung, die alle Unicode-Zeichen darstellen kann. Darüber hinaus muss die Zielspalte oder der Zielparameter auf dem Server auch entweder einem Unicode-Typ (**nchar**, **nvarchar**, **ntext**) oder einer Sortierung bzw. Codierung entsprechen, die alle Zeichen der ursprünglichen Quelldaten darstellen kann. Legen Sie einen Unicode-SQL-Typ und entweder einen Unicode-C-Typ (SQL_C_WCHAR), wodurch der Treiber die Daten in der UTF-16-Codierung zurückgibt, oder einen schmalen C-Typ fest, und stellen Sie sicher, dass die Codierung des Clients alle Zeichen der Quelldaten darstellen kann (dies wird mit der UTF-8-Codierung immer gewährleistet), um Datenverlust bei Ausgabeparametern zu vermeiden.

Weitere Informationen zur Sortierung und Codierung finden Sie unter [Sortierung und Unicode-Unterstützung](../../../relational-databases/collations/collation-and-unicode-support.md).

Zwischen Windows und mehreren Versionen der iconv-Bibliothek unter Linux und macOS gibt es einige Unterschiede bei der Konvertierung der Codierung. Textdaten in der Codepage 1255 (Hebräisch) verfügen über einen Codepunkt (0xCA), der sich bei der Konvertierung in Unicode anders verhält. Unter Windows wird das Zeichen in den UTF-16-Codepunkt 0x05BA konvertiert. Unter macOS und Linux wird es bei libiconv-Versionen vor Version 1.15 in 0x00CA konvertiert. Zeichen, die unter Linux mit iconv-Bibliotheken hinzugefügt werden, die die Revision von Big5/CP950 (namens `BIG5-2003`) von 2003 nicht unterstützen, werden nicht ordnungsgemäß konvertiert. In der Codepage 932 (Japanisch, Shift_JIS) unterscheidet sich das Ergebnis der Decodierung von Zeichen, die nicht ursprünglich im Codierungsstandard definiert wurden, ebenfalls. Zum Beispiel wird das Byte 0x80 unter Windows in U+0080 konvertiert, jedoch kann es je nach iconv-Version unter Linux und macOS auch in U+30FB konvertiert werden.

Im ODBC-Treiber 13 und 13.1 werden Daten beschädigt, wenn UTF-8-Mehrbytezeichen oder UTF-16-Ersatzzeichen auf SQLPutData-Puffer aufgeteilt werden. Für das Streamen von SQLPutData, verwenden Sie Puffer, die nicht in partiellen Zeichencodierungen enden. Diese Einschränkung wurde mit dem Version 17 des ODBC-Treibers entfernt.

## <a name="openssl"></a><a name="bkmk-openssl"></a>OpenSSL
Ab Version 17.4 lädt der Treiber OpenSSL dynamisch, sodass diese Software auf Systemen mit Version 1.0 oder 1.1 ausgeführt werden kann, ohne dass separate Treiberdateien erforderlich sind. Wenn mehrere OpenSSL-Versionen vorhanden sind, versucht der Treiber, die neueste zu laden. Der Treiber unterstützt derzeit OpenSSL 1.0.x und 1.1.x.

> [!NOTE]  
> Es können Konflikte auftreten, wenn die Anwendung, die den Treiber verwendet (oder eine ihrer Komponenten), mit einer anderen Version von OpenSSL verknüpft ist oder dynamisch eine andere Version von OpenSSL lädt. Wenn im System mehrere OpenSSL-Versionen vorhanden sind und eine Anwendung OpenSSL verwendet, müssen Sie unbedingt sicherstellen, dass die von der Anwendung und vom Treiber geladene Version übereinstimmen. Fehler könnten den Arbeitsspeicher beeinträchtigen, daher macht sich ein solcher Konflikt möglicherweise nicht auf offensichtliche oder konsistente Weise bemerkbar.

## <a name="alpine-linux"></a><a name="bkmk-alpine"></a>Alpine Linux
Zum Zeitpunkt der Erstellung dieser Dokumentation beträgt die Standardstapelgröße in MUSL 128K. Dies ist für die grundlegenden ODBC-Treiberfunktionen ausreichend, aber je nach Zweck der Anwendung kann dieser Grenzwert schnell überschritten werden – insbesondere dann, wenn der Treiber aus mehreren Threads aufgerufen wird. Es empfiehlt sich, eine ODBC-Anwendung unter Alpine Linux mit `-Wl,-z,stack-size=<VALUE IN BYTES>` zu kompilieren, um die Stapelgröße zu erhöhen. Zur Referenz: Die Standardstapelgröße in den meisten GLIBC-Systemen beträgt 2 MB.

## <a name="additional-notes"></a>Weitere Hinweise  

1.  Sie können eine dedizierte Administratorverbindung (DAC) herstellen, indem Sie die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Authentifizierung und **host,port** verwenden. Ein Mitglied der Sysadmin-Rolle muss zunächst den DAC-Port ermitteln. Weitere Informationen dazu finden Sie unter [Diagnoseverbindung für Datenbankadministratoren](../../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md#dac-port). Wenn beispielsweise der DAC-Port „33000“ ist, können Sie mit `sqlcmd` wie folgt eine Verbindung herstellen:  

    ```
    sqlcmd -U <user> -P <pwd> -S <host>,33000
    ```

    > [!NOTE]  
    > DAC-Verbindungen müssen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Authentifizierung verwenden.  
    
2.  Der UnixODBC-Treiber-Manager gibt „ Attribut-/Optionsbezeichner ungültig“ für alle Anweisungsattribute zurück, wenn sie über SQLSetConnectAttr übergeben werden. Wenn SQLSetConnectAttr unter Windows einen Anweisungsattributwert erhält, verursacht dieser, dass der Treiber diesen Wert für alle aktiven Anweisungen festlegt, die dem Verbindungshandle untergeordnet sind.  

3.  Bei Verwendung des Treibers mit Anwendungen mit sehr vielen Threads kann die Handlevalidierung von unixODBC zu einem Leistungsengpass führen. In solchen Szenarien lässt sich durch Kompilieren von unixODBC mit der Option `--enable-fastvalidate` eine wesentlich bessere Leistung erzielen. Beachten Sie jedoch, dass dies bei Anwendungen, die ungültige Handles an ODBC-APIs übergeben, dazu führen kann, dass diese Anwendungen abstürzen, anstatt `SQL_INVALID_HANDLE`-Fehler zurückzugeben.

## <a name="see-also"></a>Weitere Informationen  
[Häufig gestellte Fragen](frequently-asked-questions-faq-for-odbc-linux.yml)

[Bekannte Probleme in dieser Version des Treibers](known-issues-in-this-version-of-the-driver.md)

[Versionsanmerkungen](release-notes-odbc-sql-server-linux-mac.md)
