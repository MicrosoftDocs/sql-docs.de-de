---
title: Übersicht über die CLR-Integration | Microsoft-Dokumentation
description: Microsoft SQL Server Hosting von CLR wird als CLR-Integration bezeichnet. Die Erstellung in verwaltetem Code kann die Leistung verbessern. SQL Server verwendet CAS zum Sichern von verwaltetem Code.
ms.custom: ''
ms.date: 04/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- common language runtime [SQL Server], about CLR integration
- extended stored procedures [SQL Server], vs. managed code
- objects [CLR integration]
- Transact-SQL vs. managed code
- managed code [SQL Server], vs. Transact-SQL
- managed code [SQL Server], vs. extended stored procedures
- execution at client vs. execution at server [CLR integration]
ms.assetid: 5aa176da-3652-4afa-a742-4c40c77ce5c3
author: rothja
ms.author: jroth
ms.openlocfilehash: 667ded44c653f71902893f160b6231ad8b751789
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100341455"
---
# <a name="clr-integration---overview"></a>CLR-Integration: Übersicht
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  CLR (Common Language Runtime) steht im Mittelpunkt von Microsoft .NET Framework und stellt die Ausführungsumgebung für sämtlichen .NET Framework-Code bereit. In CLR geschriebener Code wird als verwalteter Code bezeichnet. CLR stellt verschiedene Funktionen und Dienste für die Programmausführung bereit, unter anderem Just-In-Time-Kompilierung (JIT), Speicherzuordnung und -verwaltung, Erzwingen der Typsicherheit, Ausnahmebehandlung, Threadverwaltung und Sicherheit.  Informationen hierzu finden Sie im .NET Framework-SDK.  
  
 Mit der von Microsoft SQL Server gehosteten CLR ("CLR-Integration" genannt) können Sie gespeicherte Prozeduren, Trigger, benutzerdefinierte Funktionen, benutzerdefinierte Typen (UDT) und benutzerdefinierte Aggregate in verwaltetem Code erstellen. Da verwalteter Code vor der Ausführung in systemeigenen Code kompiliert wird, können Sie in manchen Fällen einen erheblichen Leistungsanstieg erreichen.  
  
 In verwaltetem Code wird Codezugriffssicherheit (Code Access Sicherheit, CAS) verwendet, um zu verhindern, dass Assemblys bestimmte Vorgänge ausführen. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet CAS, um die Absicherung von verwaltetem Code zu unterstützen und eine Gefährdung des Betriebssystems oder des Datenbankservers zu verhindern.  
  
## <a name="advantages-of-clr-integration"></a>Vorteile der CLR-Integration  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] ist ausdrücklich für direkten Datenzugriff und die direkte Datenbearbeitung in der Datenbank vorgesehen. Während [!INCLUDE[tsql](../../includes/tsql-md.md)] für den Datenzugriff und die Datenverwaltung ideal geeignet ist, handelt es sich dabei nicht um eine vollständige Programmiersprache. Beispielsweise unterstützt [!INCLUDE[tsql](../../includes/tsql-md.md)] keine Arrays, Auflistungen, For-Each-Schleifen, Bit-Verschiebungen oder Klassen. Bestimmte dieser Konstrukte können in [!INCLUDE[tsql](../../includes/tsql-md.md)]simuliert werden; verwalteter Code verfügt über eine integrierte Unterstützung dieser Konstrukte. Je nach Szenario können diese Funktionen einen zwingenden Grund darstellen, bestimmte Datenbankfunktionalitäten in verwaltetem Code zu implementieren.  
  
 Microsoft Visual Basic .NET und Microsoft Visual C# bieten objektorientierte Funktionen wie z. B. Kapselung, Vererbung und Polymorphie. Damit verbundener Code kann jetzt leicht in Klassen und Namespaces organisiert werden. Wenn Sie mit großen Datenmengen an Servercode arbeiten, können Sie auf diese Weise Ihren Code leichter organisieren und instand halten.  
  
 Verwalteter Code eignet sich besser als [!INCLUDE[tsql](../../includes/tsql-md.md)] für Berechnungen und komplizierte Ausführungslogik und bietet weitreichende Unterstützung für zahlreiche komplexe Tasks wie Zeichenfolgenbehandlung und reguläre Ausdrücke. Mit der Funktionalität, die in der .NET Framework -Bibliothek zur Verfügung steht, haben Sie Zugriff auf Tausende von vorgefertigten Klassen und Routinen. Auf diese Klassen und Routinen können Sie von jeder gespeicherten Prozedur, jedem Trigger und jeder benutzerdefinierten Funktion aus zugreifen. Die Basisklassenbibliothek (Base Class Library, BCL) enthält u. a. Klassen mit Funktionen zur Zeichenfolgenbearbeitung, für erweiterte mathematische Vorgänge, den Dateizugriff, die Kryptografie.  
  
> [!NOTE]  
>  Viele dieser Klassen können ausgehend vom CLR-Code in SQL Server verwendet werden. Klassen, die nicht für eine serverseitige Verwendung geeignet sind (beispielsweise Window-Klassen) sind auf diese Weise nicht verfügbar. Weitere Informationen finden Sie [unter Supported .NET Framework Libraries](../../relational-databases/clr-integration/database-objects/supported-net-framework-libraries.md).  
  
 Einer der Vorteile von verwaltetem Code besteht in der Typsicherheit, d. h. die Gewissheit, dass der Code nur in einer genau definierten und zulässigen Weise auf Typen zugreift. Bevor verwalteter Code ausgeführt wird, überprüft die CLR, dass der Code sicher ist. Beispielsweise wird der Code dahingehend überprüft, dass kein Speicher gelesen wird, der nicht zuvor geschrieben wurde. Die CLR kann auch dazu beitragen, sicherzustellen, dass der Code nicht verwalteten Speicher bearbeitet.  
  
 Die CLR-Integration bietet das Potenzial zu verbesserter Leistung. Weitere Informationen finden Sie unter [Performance of CLR Integration](../../relational-databases/clr-integration/clr-integration-architecture-performance.md).  
 
> [!WARNING]
>  CLR verwendet die Codezugriffssicherheit (Code Access Security, CAS) im .NET Framework, die nicht länger als Sicherheitsbegrenzung unterstützt wird. Eine CLR-Assembly, die mit `PERMISSION_SET = SAFE` erstellt wurde, kann womöglich auf externe Systemressourcen zugreifen, nicht verwalteten Code aufrufen und sysadmin-Privilegien erwerben. Ab [!INCLUDE[sssql17](../../includes/sssql17-md.md)] wird eine `sp_configure`-Option mit der Bezeichnung `clr strict security` eingeführt, um die Sicherheit von CLR-Assemblys zu erhöhen. `clr strict security` ist standardmäßig aktiviert und behandelt `SAFE`- und `EXTERNAL_ACCESS`-Assemblys so, als wären Sie als `UNSAFE` gekennzeichnet. Die Option `clr strict security` kann für die Abwärtskompatibilität deaktiviert werden, es wird jedoch nicht empfohlen. Microsoft empfiehlt, dass alle Assemblys durch ein Zertifikat oder einen asymmetrischen Schlüssel mit einem entsprechenden Anmeldenamen signiert werden, dem `UNSAFE ASSEMBLY`-Berechtigung für die Masterdatenbank gewährt wurde. Weitere Informationen finden Sie unter [CLR Strict Security](../../database-engine/configure-windows/clr-strict-security.md). 
  
## <a name="choosing-between-transact-sql-and-managed-code"></a>Transact-SQL oder verwalteter Code?  
 Beim Erstellen von gespeicherten Prozeduren, Triggern und benutzerdefinierten Funktionen müssen Sie zunächst entscheiden, ob Sie dazu das traditionelle [!INCLUDE[tsql](../../includes/tsql-md.md)]oder eine .NET Framework-Programmiersprache wie Visual Basic .NET oder Visual C# einsetzen möchten. Verwenden Sie [!INCLUDE[tsql](../../includes/tsql-md.md)] , wenn der Code hauptsächlich Datenzugriffe mit wenig oder keiner prozeduralen Logik ausführen soll. Verwenden Sie verwalteten Code für CPU-intensive Funktionen und Prozeduren, die eine komplexe Logik aufweisen, oder in Fällen, in denen Sie auf die Basisklassenbibliothek (Base Class Library, BCL) von .NET Framework zurückgreifen möchten.  
  
### <a name="choosing-between-execution-in-the-server-and-execution-in-the-client"></a>Ausführen im Server oder im Client?  
 Ein weiterer Entscheidungsfaktor zwischen [!INCLUDE[tsql](../../includes/tsql-md.md)] und verwaltetem Code ist der Speicherort des Codes: auf dem Servercomputer oder auf dem Clientcomputer. Sowohl [!INCLUDE[tsql](../../includes/tsql-md.md)] als auch verwalteter Code können auf dem Server ausgeführt werden. Dies hält Code und Daten näher zusammen und ermöglicht Ihnen außerdem, die Verarbeitungskapazität des Servers auszunutzen. Andererseits möchten Sie möglicherweise vermeiden, prozessorintensive Tasks auf dem Datenbankserver zu platzieren. Die meisten Clientcomputer sind heute ebenfalls sehr leistungsfähig, was Sie ausnutzen könnten, indem Sie soviel Code wie möglich auf einem Clientcomputer ablegen. Verwalteter Code kann auf einem Clientcomputer ausgeführt werden, nicht aber [!INCLUDE[tsql](../../includes/tsql-md.md)] -Code.  
  
## <a name="choosing-between-extended-stored-procedures-and-managed-code"></a>Erweiterte gespeicherte Prozeduren oder verwalteter Code?  
 Erweiterte gespeicherte Prozeduren werden erstellt, um Funktionen auszuführen, die mit gespeicherten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Prozeduren nicht möglich sind. Erweiterte gespeicherte Prozeduren können jedoch die Integrität des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Prozesses beeinträchtigen, während verwalteter Code auf Typsicherheit überprüft wird und daher keine Gefährdung darstellt. Darüber hinaus sind Speicherverwaltungsfunktionen, Planung von Threads und Fibers und Synchronisierungsdienste des verwalteten Codes der CLR und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]besser integriert. Die CLR-Integration bietet mehr Sicherheit als erweiterte gespeicherte Prozeduren beim Erstellen der gespeicherten Prozeduren, die Sie benötigen, um Tasks auszuführen, die in [!INCLUDE[tsql](../../includes/tsql-md.md)]nicht möglich sind. Weitere Informationen zu CLR-Integration und erweiterte gespeicherte Prozeduren finden Sie unter [Performance of CLR Integration](../../relational-databases/clr-integration/clr-integration-architecture-performance.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Installieren des .NET Framework](https://technet.microsoft.com/library/ms166014\(v=SQL.105\).aspx)   
 [Architektur der CLR-Integration](./clr-integration-architecture-clr-hosted-environment.md)   
 [Datenzugriff von CLR-Datenbankobjekten](../../relational-databases/clr-integration/data-access/data-access-from-clr-database-objects.md)   
 [Erste Schritte mit der CLR-Integration](../../relational-databases/clr-integration/database-objects/getting-started-with-clr-integration.md)  
  
