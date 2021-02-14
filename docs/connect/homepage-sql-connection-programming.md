---
title: Homepage für die SQL-Clientprogrammierung | Microsoft-Dokumentation
description: Seite mit kommentierten Links zu Downloads und Dokumentationsmaterial zum Herstellen einer Verbindung mit SQL Server oder Azure SQL-Datenbank für verschiedene Sprachen und Betriebssysteme.
author: David-Engel
ms.date: 11/07/2018
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: v-daveng
ms.author: v-daenge
ms.openlocfilehash: 10d04306621722abd9c9a170028ba632a8d95c9e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045500"
---
# <a name="homepage-for-client-programming-to-microsoft-sql-server"></a>Startseite für die Clientprogrammierung in Microsoft SQL Server


Willkommen auf unserer Homepage für die Clientprogrammierung zum Interagieren mit Microsoft SQL Server und Azure SQL-Datenbank in der Cloud. Dieser Artikel bietet die folgenden Informationen:

- Die verfügbaren Kombinationen aus Sprache und Treiber werden aufgeführt und beschrieben.
  - Es werden Informationen zu den Betriebssystemen von Linux (Ubuntu und weitere), macOS und Windows bereitgestellt.
- Der Artikel stellt Links zur detaillierten Dokumentation für jede Kombination bereit.
- Die über- und untergeordneten Bereiche der hierarchischen Dokumentation für bestimmte Sprachen werden angezeigt, sofern vorhanden.


#### <a name="azure-sql-database"></a>Azure SQL-Datenbank

In allen Sprachen ist der Code, mit dem eine Verbindung mit SQL Server hergestellt wird, nahezu identisch mit dem Code zum Herstellen einer Verbindung mit Azure SQL-Datenbank.

Details zu den Verbindungszeichenfolgen zum Herstellen einer Verbindung mit Azure SQL-Datenbank finden Sie hier:

- [Abfragen einer Azure SQL-Datenbank mithilfe von .NET Core (C#)](/azure/sql-database/sql-database-connect-query-dotnet-core).
- Weitere Artikel zu Azure SQL-Datenbank über andere Sprachen, die sich im Inhaltsverzeichnis in der Nähe des oben genannten Artikels befinden. Beispiel: [Abfragen einer Azure SQL-Datenbank mithilfe von PHP](/azure/sql-database/sql-database-connect-query-php).


#### <a name="build-an-app-webpages"></a>Webseiten zum Erstellen einer App

Unsere Webseiten zum *Erstellen einer App* enthalten Codebeispiele sowie Konfigurationsinformationen in einem alternativen Format. Weitere Informationen finden Sie weiter unten in diesem Artikel im [Abschnitt *Website zum Erstellen einer App*](#an-204-aka-ms-sqldev).



<a name="an-050-languages-clients" />

## <a name="languages-and-drivers-for-client-programs"></a>Sprachen und Treiber für Clientprogramme


In der folgenden Tabelle enthält jedes Bild einer Sprache einen Link zu weiteren Informationen zur Verwendung der Sprache mit SQL Server. Jeder Link führt zu einem Abschnitt weiter unten in diesem Artikel.

:::row:::
    :::column:::
        [![C#-Logo][image-ref-320-csharp]](#an-110-ado-net-docu)  

        [![Node.js-Logo][image-ref-340-node]](#an-140-node-js-docu)  

        [![Python-Logo][image-ref-370-python]](#an-180-python-docu)  
    :::column-end:::
    :::column:::
        [![ORM Entity Framework von .NET Framework][image-ref-333-ef]](#an-116-csharp-ef-orm)  

        [**`ODBC for C++`**](#an-160-odbc-cpp-docu)<br/>[![cpp-big-plus][image-ref-322-cpp]](#an-160-odbc-cpp-docu)  

        [![Ruby-Logo][image-ref-380-ruby]](#an-190-ruby-docu)  
    :::column-end:::
    :::column:::
        [![Java-Logo][image-ref-330-java]](#an-130-jdbc-docu)  

        [![PHP-Logo][image-ref-360-php]](#an-170-php-docu)  
    :::column-end:::
:::row-end:::

#### <a name="downloads-and-installs"></a>Downloads und Installationen

Der folgende Artikel beschreibt den Download und die Installation verschiedener SQL-Treiber zum Herstellen von Verbindungen, zur Verwendung mit verschiedenen Programmiersprachen:

- [SQL Server-Treiber](./sql-connection-libraries.md)



<a name="an-110-ado-net-docu" />

## <a name="c-logoimage-ref-320-csharp-c-using-adonet"></a>![C#-Logo][image-ref-320-csharp] C# unter Verwendung von ADO.NET

Die .NET-verwalteten Sprachen wie C# und Visual Basic sind die häufigsten Einsatzgebiete von ADO.NET. *ADO.NET* ist ein informeller Name für eine Teilmenge von .NET Framework-Klassen.

#### <a name="code-examples"></a>Codebeispiele

| Beispiel | BESCHREIBUNG |
| :-- | :-- |
| [Proof of concept connecting to SQL using ADO.NET (Proof of Concept für Verbindungen mit SQL mithilfe von ADO.NET)](./ado-net/step-3-connect-sql-ado-net.md) | Ein kleines Codebeispiel zum Herstellen einer Verbindung mit und Abfragen von SQL Server. |
| [Connect resiliently to SQL with ADO.NET (Herstellen stabiler SQL-Verbindungen mit ADO.NET)](./ado-net/step-4-connect-resiliently-sql-ado-net.md) | Wiederholungslogik in einem Codebeispiel, da bei Verbindungen gelegentlich kurze Unterbrechungen auftreten können.<br /><br />Die Wiederholungslogik lässt sich gut auf Verbindungen mit Clouddatenbanken über das Internet anwenden, beispielsweise mit Azure SQL-Datenbank. |
| [Azure SQL-Datenbank: Demonstration zur Verwendung von .NET Core unter Windows/Linux/macOS zum Erstellen eines C#-Programms zum Herstellen einer Verbindung und Abfragen von Datenbanken](/azure/sql-database/sql-database-connect-query-dotnet-core) | Beispiel für Azure SQL-Datenbank. |
| [Erstellen einer App: C#, ADO.NET, Windows](https://www.microsoft.com/sql-server/developer-get-started/csharp/win/) | Konfigurationsinformationen sowie Codebeispiele. |
| &nbsp; | <br /> |

#### <a name="documentation"></a>Dokumentation

| Bereich | BESCHREIBUNG |
| :-- | :-- |
| [C# unter Verwendung von ADO.NET](./ado-net/microsoft-ado-net-sql-server.md)| Hauptseite unserer Dokumentation. |
| [Namespace: System.Data](/dotnet/api/system.data) | Eine Reihe von Klassen, die für ADO.NET verwendet werden. |
| [Namespace: Microsoft.Data.SqlClient](/dotnet/api/microsoft.data.SqlClient) | Die Klassen, die für den Microsoft-.NET-Datenanbieter für SQL Server verwendet werden. |
| &nbsp; | <br /> |



<a name="an-116-csharp-ef-orm" />

## <a name="entity-framework-logoimage-ref-333-ef-entity-framework-ef-with-cx23"></a>![Entity Framework-Logo][image-ref-333-ef] Entity Framework (EF) mit C&#x23;

Das Entity Framework (EF) stellt eine objektrelationale Zuordnung (Object-Relational Mapping, ORM) bereit. Mit dieser Zuordnung kann Quellcode, der in einer objektorientierten Programmiersprache (Object-Oriented Programming, OOP) geschrieben wurde, aus einer relationalen SQL-Datenbank abgerufene Daten leichter ändern.

Das EF verfügt über direkte oder indirekte Beziehungen zu den folgenden Technologien:

- .NET Framework
- [LINQ to SQL](/dotnet/framework/data/adonet/sql/linq/) oder [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities)
- Erweiterungen der Sprachsyntax, wie z. B. der **=>** -Operator in C#.
- Praktische Programme, die Quellcode für Klassen generieren, die sich den Tabellen in Ihrer SQL-Datenbank-Instanz zuordnen lassen. Beispiel: [EdmGen.exe](/dotnet/framework/data/adonet/ef/edm-generator-edmgen-exe).


#### <a name="original-ef-and-new-ef"></a>Ursprüngliches EF und neues EF

Die [Startseite für das Entity Framework](/ef/) bietet eine Einführung mit einer ähnlichen Beschreibung wie folgt:

- Das Entity Framework (EF) ist eine objektrelationale Zuordnung (Object-Relational Mapper, O/RM), die .NET-Entwicklern mithilfe von .NET-Objekten das Arbeiten mit einer Datenbank ermöglicht. Im EF ist der Großteil des Datenzugriffscodes, den Entwickler in der Regel schreiben müssen, nicht mehr erforderlich.

*Entity Framework* ist der Name zweier separater Quellcodeversionen. Eine der beiden Versionen ist älter, und der zugehörige Quellcode kann jetzt öffentlich verwaltet werden. Das andere EF ist neu. Im Folgenden werden die beiden Frameworks beschrieben:

| Version | BESCHREIBUNG |
| :-- | :-- |
| [EF 6.x](/ef/ef6/) | Microsoft brachte das EF im August 2008 heraus. Im März 2015 kündigte Microsoft an, dass EF 6.x die letzte von Microsoft entwickelte Version sei. Microsoft stellte den Quellcode für die Öffentlichkeit zur Verfügung.<br /><br />Ursprünglich war das EF Teil von .NET Framework. EF 6.x wurde jedoch aus .NET Framework herausgelöst.<br /><br />[EF 6.x-Quellcode auf GitHub im Repository *aspnet/EntityFramework6*](https://github.com/aspnet/EntityFramework6) |
| [EF Core](/ef/core/) | Im Juni 2016 veröffentlichte Microsoft das neu entwickelte EF Core. EF Core wurde im Hinblick auf mehr Flexibilität und Portierbarkeit konzipiert. EF Core kann auf anderen Betriebssystemen als Microsoft Windows ausgeführt werden. Und EF Core kann mit Datenbanken über Microsoft SQL Server und andere relationale Datenbanken hinaus interagieren.<br /><br />**C&#x23;-Codebeispiele:**<br />[Erste Schritte mit Entity Framework Core](/ef/core/get-started/index)<br />[Erste Schritte mit EF Core auf .NET Framework mit einer vorhandenen Datenbank](/ef/core/get-started/full-dotnet/existing-db) |
| &nbsp; | <br /> |

EF und verwandte Technologien sind ausgesprochen leistungsstark, und Entwickler, die den gesamten Themenbereich beherrschen möchten, müssen viel lernen.

&nbsp;



<a name="an-130-jdbc-docu" />

## <a name="java-logoimage-ref-330-java-java-and-jdbc"></a>![Java-Logo][image-ref-330-java] Java und JDBC

Microsoft stellt einen JDBC-Treiber (Java Database Connectivity) für die Verwendung mit SQL Server (oder Azure SQL-Datenbank) bereit. Dabei handelt es sich um einen JDBC-Treiber vom Typ 4, der über die Standardanwendungsprogrammierschnittstellen für JDBC Database Connectivity zur Verfügung stellt.

#### <a name="code-examples"></a>Codebeispiele

| Beispiel | BESCHREIBUNG |
| :-- | :-- |
| [Codebeispiele](./jdbc/sample-jdbc-driver-applications.md) | Codebeispiele, die Informationen zu Datentypen, Resultsets und umfangreichen Datenmengen vermitteln. |
| [Verbindungs-URL – Beispiel](./jdbc/connection-url-sample.md) | Beschreibt die Verwendung einer Verbindungs-URL zum Herstellen einer Verbindung mit SQL Server. Hierbei verwenden Sie eine SQL-Anweisung zum Abrufen von Daten. |
| [Beispiel für Datenquellen](./jdbc/data-source-sample.md) | Beschreibt die Verwendung einer Datenquelle zum Herstellen einer Verbindung mit SQL Server. Hierbei verwenden Sie eine gespeicherte Prozedur zum Abrufen von Daten. |
| [Abfragen einer Azure SQL-Datenbank mithilfe von Java](/azure/sql-database/sql-database-connect-query-java) | Beispiel für Azure SQL-Datenbank. |
| [Erstellen von Java-Apps mithilfe von SQL Server unter Ubuntu](https://www.microsoft.com/sql-server/developer-get-started/java/ubuntu/) | Konfigurationsinformationen sowie Codebeispiele. |
| &nbsp; | <br /> |

#### <a name="documentation"></a>Dokumentation

Die JDBC-Dokumentation umfasst die folgenden Hauptthemen:

| Bereich | BESCHREIBUNG |
| :-- | :-- |
| [Java Database Connectivity (JDBC)](./jdbc/microsoft-jdbc-driver-for-sql-server.md) | Hauptseite unserer JDBC-Dokumentation. |
| [Referenz](./jdbc/reference/jdbc-driver-api-reference.md) | Schnittstellen, Klassen und Member. |
| [Programmierhandbuch für den JDBC-SQL-Treiber](./jdbc/programming-guide-for-jdbc-sql-driver.md) | Konfigurationsinformationen sowie Codebeispiele. |
| &nbsp; | <br /> |



<a name="an-140-node-js-docu" />

## <a name="nodejs-logoimage-ref-340-node-nodejs"></a>![Node.js-Logo][image-ref-340-node] Node.js

Mithilfe von Node.js können Sie unter Windows, Linux oder macOS eine Verbindung mit SQL Server herstellen. Die Hauptseite unserer Node.js-Dokumentation befindet sich [hier](./node-js/node-js-driver-for-sql-server.md).

Der Node.js-Verbindungstreiber für SQL Server ist in JavaScript implementiert. Der Treiber verwendet das TDS-Protokoll, das von allen modernen SQL Server-Versionen unterstützt wird. Der Treiber ist ein Open-Source-Projekt und [auf GitHub verfügbar](https://tediousjs.github.io/tedious/).

#### <a name="code-examples"></a>Codebeispiele

| Beispiel | BESCHREIBUNG |
| :-- | :-- |
| [Proof of concept connecting to SQL using Node.js (Proof of Concept für Verbindungen mit SQL mithilfe von Node.js)](./node-js/step-3-proof-of-concept-connecting-to-sql-using-node-js.md) | Reiner Quellcode zum Herstellen einer Verbindung mit SQL Server und Ausführen einer Abfrage. |
| [Azure SQL-Datenbank: Verwenden von Node.js zum Abfragen](/azure/sql-database/sql-database-connect-query-nodejs) | Beispiel für Azure SQL-Datenbank in der Cloud. |
| [Erstellen von Node.js-Apps zur Verwendung von SQL Server unter macOS](https://www.microsoft.com/sql-server/developer-get-started/node/mac/) | Konfigurationsinformationen sowie Codebeispiele. |
| &nbsp; | <br /> |



<a name="an-160-odbc-cpp-docu" />

## <a name="odbc-for-c"></a>ODBC für C++

![ODBC-Logo][image-ref-350-odbc] ![cpp-big-plus][image-ref-322-cpp]

ODBC (Open Database Connectivity) wurde in den 90er-Jahren entwickelt, noch vor .NET Framework. ODBC ist sowohl vom Datenbanksystem als auch vom Betriebssystem unabhängig.

Im Lauf der Jahre wurde von verschiedenen Gruppen innerhalb und außerhalb von Microsoft eine Vielzahl von ODBC-Treibern entwickelt. In diese Treiber sind verschiedene Clientprogrammiersprachen involviert. Die Liste der Datenziele geht weit über SQL Server hinaus.

Einige andere Konnektivitätstreiber verwenden intern ODBC.

#### <a name="code-example"></a>Codebeispiel

- [C++-Codebeispiel unter Verwendung von ODBC](../odbc/reference/sample-odbc-program.md)

#### <a name="documentation-outline"></a>Übersicht über die Dokumentation

Bei den ODBC-Inhalten in diesem Abschnitt geht es hauptsächlich um den Zugriff auf SQL Server oder Azure SQL-Datenbank über C++. In der folgenden Tabelle finden Sie eine allgemeine Übersicht über die wichtigsten Dokumentationsmaterialien für ODBC.


| Bereich | Unterbereich | BESCHREIBUNG |
| :--- | :------ | :---------- |
| [ODBC für C++](./odbc/microsoft-odbc-driver-for-sql-server.md) | Hauptseite unserer Dokumentation. |
| [Linux-macOS](./odbc/linux-mac/system-requirements.md) | &nbsp; | Informationen zur Verwendung von ODBC auf den Linux- oder macOS-Betriebssystemen. |
| [Windows](./odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows.md)     | &nbsp; | Informationen zur Verwendung von ODBC auf den Windows-Betriebssystemen. |
| [Verwaltung](../odbc/admin/odbc-data-source-administrator.md) | &nbsp; | Das Tool zur Verwaltung von ODBC-Datenquellen. |
| [Microsoft](../odbc/microsoft/microsoft-supplied-odbc-drivers.md)  | &nbsp; | Verschiedene ODBC-Treiber, die von Microsoft erstellt und bereitgestellt werden. |
| [Konzepte und Referenzen](../odbc/reference/introduction-to-odbc.md) | &nbsp; | Konzeptionelle Informationen zur ODBC-Schnittstelle sowie herkömmliche Referenzmaterialien. |
| &nbsp; " | [Anhänge](../odbc/reference/appendixes/odbc-appendixes.md)    | Tabellen für Statusübergänge, ODBC-Cursorbibliothek und vieles mehr. |
| &nbsp; " | [Entwickeln von Apps](../odbc/reference/develop-app/checking-feature-support-and-variability.md)  | Funktionen, Handles und vieles mehr. |
| &nbsp; " | [Entwickeln von Treibern](../odbc/reference/develop-driver/developing-an-odbc-driver.md) | Informationen zum Entwickeln eigener ODBC-Treiber, wenn eine spezialisierte Datenquelle vorhanden ist. |
| &nbsp; " | [Installieren](../odbc/reference/install/odbc-subkey.md) | ODBC-Installation, Unterschlüssel und vieles mehr. |
| &nbsp; " | [Syntax](../odbc/reference/syntax/odbc-reference.md)   | APIs für Setup, Installationsprogramm, Übersetzung und Datenzugriff. |
| &nbsp; | &nbsp; | <br /> |



<a name="an-170-php-docu" />

## <a name="php-logoimage-ref-360-php-php"></a>![PHP-Logo][image-ref-360-php] PHP

Sie können PHP für die Interaktion mit SQL Server verwenden. Die Hauptseite unserer PHP-Dokumentation befindet sich [hier](./php/microsoft-php-driver-for-sql-server.md).

#### <a name="code-examples"></a>Codebeispiele

| Beispiel | BESCHREIBUNG |
| :-- | :-- |
| [Proof of concept connecting to SQL using PHP (Proof of Concept für Verbindungen mit SQL mithilfe von PHP)](./php/step-3-proof-of-concept-connecting-to-sql-using-php.md) | Ein kleines Codebeispiel zum Herstellen einer Verbindung mit und Abfragen von SQL Server. |
| [Connect resiliently to SQL with PHP (Herstellen stabiler SQL-Verbindungen mit PHP)](./php/step-4-connect-resiliently-to-sql-with-php.md) | Wiederholungslogik in einem Codebeispiel, da bei Verbindungen über das Internet und die Cloud gelegentlich kurze Unterbrechungen auftreten können. |
| [Azure SQL-Datenbank: Verwenden von PHP für Abfragen](/azure/sql-database/sql-database-connect-query-php) | Beispiel für Azure SQL-Datenbank. |
| [Erstellen von PHP-Apps zur Verwendung von SQL Server unter RHEL](https://www.microsoft.com/sql-server/developer-get-started/php/rhel/) | Konfigurationsinformationen sowie Codebeispiele. |
| &nbsp; | <br /> |



<a name="an-180-python-docu" />

## <a name="python-logoimage-ref-370-python-python"></a>![Python-Logo][image-ref-370-python] Python


Sie können Python für die Interaktion mit SQL Server verwenden.

#### <a name="code-examples"></a>Codebeispiele

| Beispiel | BESCHREIBUNG |
| :-- | :-- |
| [Proof of Concept für Verbindungen mit SQL mithilfe von Python: pyodbc](./python/pyodbc/step-3-proof-of-concept-connecting-to-sql-using-pyodbc.md) | Ein kleines Codebeispiel zum Herstellen einer Verbindung mit und Abfragen von SQL Server. |
| [Azure SQL-Datenbank: Verwenden von Python für Abfragen](/azure/sql-database/sql-database-connect-query-python) | Beispiel für Azure SQL-Datenbank. |
| [Erstellen von PHPs-Apps zur Verwendung von SQL Server unter SLES](https://www.microsoft.com/sql-server/developer-get-started/python/sles/) | Konfigurationsinformationen sowie Codebeispiele. |
| &nbsp; | <br /> |

#### <a name="documentation"></a>Dokumentation

| Bereich | BESCHREIBUNG |
| :--- | :---------- |
| [Python für SQL Server](./python/python-driver-for-sql-server.md) | Hauptseite unserer Dokumentation. |
| [pymssql-Treiber](./python/pymssql/python-sql-driver-pymssql.md) | Microsoft wartet und testet den pymssql-Treiber nicht.<br /><br />Der pymssql-Verbindungstreiber ist eine einfache Schnittstelle für SQL-Datenbanken, die in Python-Programmen verwendet wird. pymssql setzt auf FreeTDS auf, um eine Python-Datenbank-API-Schnittstelle (PEP-249) zu Microsoft SQL Server bereitzustellen. |
| [pyodbc-Treiber](./python/pyodbc/python-sql-driver-pyodbc.md)   | Der pyodbc-Verbindungstreiber ist ein Open-Source-Python-Modul, das den Zugriff auf ODBC-Datenbanken vereinfacht. Er implementiert die DB API 2.0-Spezifikation und enthält darüber hinaus viele weitere Python-bezogene praktische Funktionen. |
| &nbsp; | <br /> |


<a name="an-190-ruby-docu" />

## <a name="ruby-logoimage-ref-380-ruby-ruby"></a>![Ruby-Logo][image-ref-380-ruby] Ruby

Sie können Ruby für die Interaktion mit SQL Server verwenden. Die Hauptseite unserer Ruby-Dokumentation befindet sich [hier](./ruby/ruby-driver-for-sql-server.md).

#### <a name="code-examples"></a>Codebeispiele

| Beispiel | BESCHREIBUNG |
| :-- | :-- |
| [Proof of Concept für Verbindungen mit SQL mithilfe von PHP](./ruby/step-3-proof-of-concept-connecting-to-sql-using-ruby.md) | Ein kleines Codebeispiel zum Herstellen einer Verbindung mit und Abfragen von SQL Server. |
| [Azure SQL-Datenbank: Verwenden von Ruby für Abfragen](/azure/sql-database/sql-database-connect-query-ruby) | Beispiel für Azure SQL-Datenbank. |
| [Erstellen von Ruby-Apps zur Verwendung von SQL Server unter macOS](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/) | Konfigurationsinformationen sowie Codebeispiele. |
| &nbsp; | <br /> |



<a name="an-204-aka-ms-sqldev" />

## <a name="build-an-app-website-for-sql-client-development"></a>[Website zum Erstellen einer App – für die SQL-Cliententwicklung](https://www.microsoft.com/sql-server/developer-get-started/)


Auf unseren Webseiten zum [*Erstellen einer App*](https://www.microsoft.com/sql-server/developer-get-started/) können Sie aus zahlreichen Programmiersprachen zum Herstellen einer Verbindung mit SQL Server auswählen. Ihr Clientprogramm kann unter einer Vielzahl von Betriebssystemen ausgeführt werden.

Bei *Erstellen einer App* liegt der Fokus auf Einfachheit und Vollständigkeit – für Entwickler, die noch nicht über sehr viel Erfahrung verfügen. Die Schritte erläutern die folgenden Aufgaben:

1. Installieren von Microsoft SQL Server
2. Herunterladen und Installieren von Tools und Treibern
3. Durchführen von erforderlichen Konfigurationen für das von Ihnen ausgewählte Betriebssystem
4. Kompilieren des bereitgestellten Quellcodes
5. Ausführen des Programms.

Im Folgenden finden Sie eine Übersicht über die auf der Website bereitgestellten Informationen:

#### <a name="java-on-ubuntu"></a>Java unter Ubuntu

1. Einrichten Ihrer Umgebung
    - Schritt 1.1: Installieren von SQL Server
    - Schritt 1.2: Installieren von Java
    - Schritt 1.3: Installieren des Java Development Kit (JDK)
    - Schritt 1.4: Installieren von Maven
2. Erstellen einer Java-Anwendung mit SQL Server
    - Schritt 2.1: Erstellen einer Java-App, die eine Verbindung mit SQL Server herstellt und Abfragen ausführt
    - Schritt 2.2: Erstellen einer Java-App, die über das beliebte Framework Hibernate eine Verbindung mit SQL Server herstellt
3. Beschleunigen Ihrer Java-App auf die bis zu 100-fache Geschwindigkeit
    - Schritt 3.1: Erstellen einer Java-App zum Veranschaulichen von Columnstore-Indizes

#### <a name="python-on-windows"></a>Python unter Windows

1. Einrichten Ihrer Umgebung
    - Schritt 1.1: Installieren von SQL Server
    - Schritt 1.2: Installieren von Python
    - Schritt 1.3 Installieren des ODBC-Treibers und des SQL-Befehlszeilen-Hilfsprogramms für SQL Server
2. Erstellen einer Python-Anwendung mit SQL Server
    - Schritt 2.1: Installieren des Python-Treibers für SQL Server
    - Schritt 2.2: Erstellen einer Datenbank für Ihre Anwendung
    - Schritt 2.3: Erstellen einer Python-App, die eine Verbindung mit SQL Server herstellt und Abfragen ausführt
3. Beschleunigen Ihrer Python-App auf die bis zu 100-fache Geschwindigkeit
    - Schritt 3.1: Erstellen einer neuen Tabelle mit 5 Millionen Zeilen mithilfe von sqlcmd
    - Schritt 3.2: Erstellen einer Python-App, die diese Tabelle abfragt und die benötigte Zeit misst
    - Schritt 3.3: Messen, wie lange die Ausführung der Abfrage dauert
    - Schritt 3.4: Hinzufügen eines Columnstore-Indexes zur Tabelle
    - Schritt 3.5: Messen, wie lange die Ausführung der Abfrage mit einem Columnstore-Index dauert

Die folgenden Screenshot zeigen, wie unsere Website mit der Dokumentation für die SQL-Entwicklung aussieht.

#### <a name="choose-a-language"></a>Wählen Sie eine Sprache

![Website für die SQL-Entwicklung, erste Schritte][image-ref-390-aka-ms-sqldev-choose-language]

&nbsp;

#### <a name="choose-an-operating-system"></a>Wählen Sie ein Betriebssystem aus

![Website für die SQL-Entwicklung, Java Ubuntu][image-ref-400-aka-ms-sqldev-java-ubuntu]

&nbsp;



## <a name="other-development"></a>Weitere Entwicklungsoptionen


Dieser Abschnitt enthält Links zu anderen Entwicklungsoptionen. Diese umfassen dieselben Sprachen für die allgemeine Azure-Entwicklung. Diese Informationen gehen über die Entwicklung für Azure SQL-Datenbank und Microsoft SQL Server hinaus.

#### <a name="developer-hub-for-azure"></a>Entwicklerhub für Azure

- [Entwicklerhub für Azure](/azure/)
- [Azure für .NET-Entwickler](/dotnet/azure/)
- [Azure für Java-Entwickler](/java/azure/)
- [Azure für Node.js-Entwickler](/nodejs/azure/)
- [Azure für Python-Entwickler](/python/azure/)
- [Erstellen einer PHP-Web-App in Azure](/azure/app-service-web/app-service-web-get-started-php)

#### <a name="other-languages"></a>Andere Sprachen

- [Erstellen von Go-Apps mithilfe von SQL Server unter Windows](https://www.microsoft.com/sql-server/developer-get-started/go/windows/)



<!-- Image references. -->

[image-ref-322-cpp]: ./media/homepage-sql-connection-drivers/gm-cpp-4point-p61f.png
[image-ref-320-csharp]: ./media/homepage-sql-connection-drivers/gm-csharp-c10c.png
[image-ref-333-ef]: ./media/homepage-sql-connection-drivers/gm-entity-framework-ef20d.png
[image-ref-330-java]: ./media/homepage-sql-connection-drivers/gm-java-j18c.png
[image-ref-340-node]: ./media/homepage-sql-connection-drivers/gm-node-n30.png
[image-ref-350-odbc]: ./media/homepage-sql-connection-drivers/gm-odbc-ic55826-o35.png
[image-ref-360-php]: ./media/homepage-sql-connection-drivers/gm-php-php60.png
[image-ref-370-python]: ./media/homepage-sql-connection-drivers/gm-python-py72.png
[image-ref-380-ruby]: ./media/homepage-sql-connection-drivers/gm-ruby-un-r82.png
[image-ref-390-aka-ms-sqldev-choose-language]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-choose-language-g21.png
[image-ref-400-aka-ms-sqldev-java-ubuntu]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-java-ubuntu-c31.png