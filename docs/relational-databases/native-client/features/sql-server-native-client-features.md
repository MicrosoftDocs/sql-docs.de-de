---
title: Features
description: Zusätzlich zum verfügbar machen von Features der Windows-Datenzugriffs Komponenten implementiert SQL Server Native Client andere Funktionen, um SQL Server Funktionalität verfügbar zu machen.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- MDAC [SQL Server]
- SQL Server Native Client ODBC driver, about SQL Server Native Client ODBC driver
- SQLNCLI, about SQL Server Native Client
- data access [SQL Server Native Client], features
ms.assetid: 7bb32865-5afb-41ab-98b4-3fa545ee8953
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d192f469759ae57d01f4789aef887f4cbb60adca
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462101"
---
# <a name="sql-server-native-client-features"></a>SQL Server Native Client-Funktionen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client macht nicht nur Funktionen der Windows (früher Microsoft) Data Access Components (WDAC) verfügbar, sondern implementiert zudem viele weitere Funktionen, um die Funktionalität von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] verfügbar zu machen.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
 [Verhaltensänderungen des ODBC-Treibers bei der Behandlung von Zeichenkonvertierungen](../../../relational-databases/native-client/features/odbc-driver-behavior-change-when-handling-character-conversions.md)  
 Erläutert das geänderte Verhalten ab [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 2012 Native Client.  
  
 [Verwenden der Datenbankspiegelung](../../../relational-databases/native-client/features/using-database-mirroring.md)  
 Erläutert [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , wie Native Client die Verwendung von gespiegelten Datenbanken unterstützt. Dies ist die Möglichkeit, eine Kopie oder Spiegelung einer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Datenbank auf einem Standbyserver zu speichern.  
  
 [Ausführen asynchroner Vorgänge](../../../relational-databases/native-client/features/performing-asynchronous-operations.md)  
 Erläutert, auf welche Weise [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client asynchrone Vorgänge unterstützt. Das ist die Fähigkeit, Rückgaben unverzüglich zu übermitteln, ohne den aufrufenden Thread zu blockieren.  
  
 [Verwenden von Multiple Active Result Sets &#40;MARS&#41;](../../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md)  
 Erläutert, auf welche Weise [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client Multiple Active Result Sets (MARS) unterstützt. MARS ermöglichen es Ihnen, mehrere Resultsets mithilfe einer einzigen Datenbankverbindung auszuführen und zu empfangen.  
  
 [Verwenden von XML-Datentypen](../../../relational-databases/native-client/features/using-xml-data-types.md)  
 Erläutert, auf welche Weise [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client den XML-Datentyp unterstützt. Dieser XML-basierte Datentyp kann als Spaltentyp, Variablentyp, Parametertyp oder Funktionsrückgabetyp verwendet werden.  
  
 [Verwenden von benutzerdefinierten Typen](../../../relational-databases/native-client/features/using-user-defined-types.md)  
 Erläutert [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , wie Native Client User-Defined Typen (UDT) unterstützt, die das SQL-Typsystem erweitern, indem Sie Objekte und benutzerdefinierte Datenstrukturen in einer-Datenbank speichern können [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 [Verwenden von Datentypen mit umfangreichen Werten](../../../relational-databases/native-client/features/using-large-value-types.md)  
 Erläutert, auf welche Weise [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client Datentypen mit großen Werten unterstützt, bei denen es sich um LOB-Datentypen handelt.  
  
 [Programmgesteuertes Ändern von Kennwörtern](../../../relational-databases/native-client/features/changing-passwords-programmatically.md)  
 Erläutert, auf welche Weise [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client die Handhabung abgelaufener Kennwörter unterstützt und es ermöglicht, dass Kennwörter jetzt auf dem Client ohne Eingreifen eines Administrators geändert werden können.  
  
 [Arbeiten mit der Momentaufnahmeisolation](../../../relational-databases/native-client/features/working-with-snapshot-isolation.md)  
 Erläutert, auf welche Weise [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client die Verbesserung der Zeilenversionsverwaltung unterstützt. Sie erhöht die Datenbankleistung, indem Leser-/Schreiberblockierungsszenarien vermieden werden.  
  
 [Arbeiten mit Abfragebenachrichtigungen](../../../relational-databases/native-client/features/working-with-query-notifications.md)  
 Erläutert, auf welche Weise [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client die Benachrichtigung von Consumern bei Rowsetänderungen unterstützt.  
  
 [Durchführen von Massenkopiervorgängen](../../../relational-databases/native-client/features/performing-bulk-copy-operations.md)  
 Erläutert [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , wie Native Client Massen Kopiervorgänge unterstützt, mit denen große Datenmengen in eine oder aus einer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Tabelle oder Sicht übertragen werden können.  
  
 [Verwenden von Verschlüsselung ohne Überprüfung](../../../relational-databases/native-client/features/using-encryption-without-validation.md)  
 Erläutert, wie [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client zur Verschlüsselung an den Server gesendeter Daten ohne Prüfung des Zertifikats verwendet wird.  
  
 [Tabellenwert Parameter &#40;SQL Server Native Client&#41;](../../../relational-databases/native-client/features/table-valued-parameters-sql-server-native-client.md)  
 Erläutert, auf welche Weise [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client die Tabellenwertparameter unterstützt.  
  
 [Große benutzerdefinierte CLR-Typen](../../../relational-databases/native-client/features/large-clr-user-defined-types.md)  
 Erläutert die Unterstützung für große CLR-benutzerdefinierte Typen (Common Language Runtime).  
  
 [FILESTREAM-Unterstützung](../../../relational-databases/native-client/features/filestream-support.md)  
 Erläutert [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] die Native Client-Unterstützung für die erweiterte FILESTREAM-Funktion.  
  
 [Unterstützung von Dienstprinzipalnamen &#40;SPN&#41; in Clientverbindungen](../../../relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections.md)  
 Erläutert, auf welche Weise die Unterstützung für Dienstprinzipalnamen (Service Principal Names, SPN) erweitert wurde, damit die gegenseitige Authentifizierung über alle Protokolle hinweg möglich ist.  
  
 [Unterstützung für Spalten mit geringer Dichte in SQL Server Native Client](../../../relational-databases/native-client/features/sparse-columns-support-in-sql-server-native-client.md)  
 Erläutert, auf welche Weise [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client Sparsespalten unterstützt.  
  
 [Verbesserungen bei Datum und Zeit](../../../relational-databases/native-client/features/date-and-time-improvements.md)  
 Erläutert die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client hinzugefügte Unterstützung für die Datums- und Uhrzeitdatentypen.  
  
 [Metadatenermittlung](../../../relational-databases/native-client/features/metadata-discovery.md)  
 Erläutert Verbesserungen der Metadatenermittlung in [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)].  
  
 [Unterstützung für UTF-16 in SQL Server Native Client 11.0](../../../relational-databases/native-client/features/utf-16-support-in-sql-server-native-client-11-0.md)  
 Erläutert eine mit [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] eingeführte Verhaltensänderung. Wenn Sie beim Binden eines Spalten Ergebnisses oder Ausgabe Parameters einen Puffer fester Länge angeben und das **WCHAR** -Zeichen, das vor dem abschließenden Zeichen in den Puffer geschrieben wurde, ein hoher Ersatz Zeichencode Punkt eines Ersatz Zeichen Paars ist und das nächste **WCHAR** -Zeichen ein niedriger Ersatz Zeichencode Punkt ist, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] fügt Native Client dem Puffer keinen hohen Ersatz Zeichencode Punkt hinzu.  
  
 [SQL Server Native Client-Unterstützung für hohe Verfügbarkeit, Notfallwiederherstellung](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)  
 Erläutert, wie die Anwendung konfiguriert werden kann, um von den Funktionen für Hochverfügbarkeit und Notfallwiederherstellung zu profitieren, die in [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] hinzugefügt wurden.  
  
 [Zugreifen auf Diagnoseinformationen im Protokoll der erweiterten Ereignisse](../../../relational-databases/native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md)  
 Erläutert Erweiterungen zu [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client und -Datenablaufverfolgung, die Ihnen Zugriff auf Diagnoseinformationen im Ringpuffer und XEvents-Protokoll geben.  
  
 [SQL Server Native Client-Unterstützung für LocalDB](../../../relational-databases/native-client/features/sql-server-native-client-support-for-localdb.md)  
 Erläutert, auf welche Weise [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client die verbesserte LocalDB-Funktion unterstützt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Native Client Programmierung](../../../relational-databases/native-client/sql-server-native-client-programming.md)   
 [ODBC-Themen zur Vorgehensweise](../../../relational-databases/native-client-odbc-how-to/odbc-how-to-topics.md)   
 [Vorgehensweisen für OLE DB](../../../relational-databases/native-client-ole-db-how-to/ole-db-how-to-topics.md)   
 [Installieren von SQL Server Native Client](../../../relational-databases/native-client/applications/installing-sql-server-native-client.md)  
  
  
