---
title: Programmierung
description: Verwenden Sie diese Ressourcen, um SQL Server Native Client zu verstehen, eine eigenständige Datenzugriffs-API, die sowohl für OLE DB als auch für ODBC verwendet wird.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLNCLI, about SQL Server Native Client
- SQL Server Native Client, about SQL Server Native Client
- data access [SQL Server Native Client], about SQL Server Native Client
- data access [SQL Server Native Client]
- SQL Server Native Client
- SQLNCLI
- native data access [SQL Server Native Client]
ms.assetid: 14ba2cb1-a424-4e4d-b224-0bf1015ab801
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: eb4fa99aa8b67f27f75746f6c1e094ba12ba8e27
ms.sourcegitcommit: 783b35f6478006d654491cb52f6edf108acf2482
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91891100"
---
# <a name="sql-server-native-client-programming"></a>Programmierung für SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ist eine eigenständige Datenzugriffs-API (Application Programming Interface), die sowohl für OLE DB als auch für ODBC verwendet wird und in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] eingeführt wurde. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client (SQL Native Client) ist eine DLL (Dynamic Link Library), die den SQL-OLE DB-Anbieter und den SQL-ODBC-Treiber enthält. Sie stellt auch neue Funktionen bereit, die weit über die von Windows Data Access Components (Windows DAC, früher Microsoft Data Access Components oder MDAC genannt) bereitgestellten Funktionalität hinausgehen. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client kann zur Erstellung neuer Anwendungen oder zur Erweiterung vorhandener Anwendungen verwendet werden, die in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] neu eingeführte Funktionen nutzen müssen, wie Multiple Active Result Sets (MARS), benutzerdefinierte Datentypen (UDT), Abfragebenachrichtigungen, Momentaufnahmenisolation und Unterstützung für XML-Datentypen.  
  
> [!NOTE]  
>  Eine Liste der Unterschiede zwischen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client und Windows DAC sowie Informationen zu Problemen, die vor dem Aktualisieren einer Windows DAC-Anwendung auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client zu berücksichtigen sind, finden Sie unter [Aktualisieren einer Anwendung in SQL Server Native Client von MDAC](../../relational-databases/native-client/applications/updating-an-application-to-sql-server-native-client-from-mdac.md).  
  
 Der ODBC-Treiber von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client wird stets in Verbindung mit dem ODBC-Treiber-Manager verwendet, der zum Lieferumfang von Windows DAC gehört. Der OLE DB-Anbieter von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client kann in Verbindung mit den OLE DB-Basisdiensten von Windows DAC verwendet werden, dies wird jedoch nicht vorausgesetzt. Ob die Basisdienste verwendet werden oder nicht, hängt von den Anforderungen der jeweiligen Anwendung ab (beispielsweise wenn Verbindungspooling erforderlich ist).  
  
 ADO-Anwendungen (ActiveX Data Object) verwenden möglicherweise den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client-OLE DB-Anbieter, aber es wird empfohlen, ADO in Verbindung mit dem Schlüsselwort für die Verbindungs Zeichenfolge der **datatyetcompatibility** (oder der entsprechenden **DataSource** -Eigenschaft) zu verwenden. Beim Einsatz des OLE DB-Anbieters von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client können ADO-Anwendungen die neuen Funktionen nutzen, die in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] eingeführt wurden und für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client über die Verbindungszeichenfolgen-Schlüsselwörter oder OLE DB-Eigenschaften oder [!INCLUDE[tsql](../../includes/tsql-md.md)] verfügbar sind. Weitere Informationen zur Verwendung dieser Funktionen mit ADO finden Sie unter Verwenden von [ADO mit SQL Server Native Client](../../relational-databases/native-client/applications/using-ado-with-sql-server-native-client.md).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client wurde entwickelt, um eine einfache Methode für den systemeigenen Datenzugriff auf SQL Server über OLE DB oder ODBC zur Verfügung zu stellen. Er ist einfach, weil OLE DB- und ODBC-Technologien in einer Bibliothek integriert sind, und er bietet eine Möglichkeit, Datenzugriffsfunktionen zu optimieren und weiterzuentwickeln, ohne die aktuellen Windows DAC-Komponenten zu ändern, die jetzt Teil der Microsoft Windows-Plattform sind.  
  
 Während [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client Komponenten in Windows DAC verwendet, ist er nicht explizit von einer bestimmten Version von Windows DAC abhängig. Sie können [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client mit der Version von Windows DAC verwenden, die zusammen mit dem von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client unterstützten Betriebssystem installiert wird.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
 [SQL Server Native Client](../../relational-databases/native-client/sql-server-native-client.md)  
 Listet wichtige neue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client-Funktionen auf.  
  
 [Einsatzbedingungen für SQL Server Native Client](../../relational-databases/native-client/when-to-use-sql-server-native-client.md)  
 Erläutert, welche Rolle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client unter den Datenzugriffstechnologien von Microsoft spielt und welche Unterschiede gegenüber Windows DAC und ADO.NET bestehen, und bietet Entscheidungshilfen für die Auswahl einer Datenzugriffstechnologie.  
  
 [SQL Server Native Client-Funktionen](../../relational-databases/native-client/features/sql-server-native-client-features.md)  
 Beschreibt die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client unterstützten Funktionen.  
  
 [Erstellen von Anwendungen mit SQL Server Native Client](../../relational-databases/native-client/applications/building-applications-with-sql-server-native-client.md)  
 Bietet einen Überblick über die Entwicklung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client sowie die Unterschiede gegenüber Windows DAC, die verwendeten Komponenten und darüber, wie ADO in Verbindung damit verwendet werden kann.  
  
 In diesem Abschnitt wird außerdem die Installation und Bereitstellung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client einschließlich der Weiterverteilung der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client-Bibliothek erläutert.  
  
 [Systemanforderungen für SQL Server Native Client](../../relational-databases/native-client/system-requirements-for-sql-server-native-client.md)  
 Erläutert die zur Nutzung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client erforderlichen Systemressourcen.  
  
 [SQL Server Native Client &#40;OLE DB&#41;](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
 Bietet Informationen zur Verwendung des OLE DB-Anbieter von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
 [SQL Server Native Client &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)  
 Bietet Informationen zur Verwendung des ODBC-Treibers von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
 [Finden weiterer SQL Server Native Client-Informationen](../../relational-databases/native-client/finding-more-sql-server-native-client-information.md)  
 Stellt zusätzliche Ressourcen über [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client bereit, einschließlich Links zu externen Ressourcen und zum Abrufen weiterer Hilfe.  
  
 [SQL Server Native Client-Fehler](./sql-server-native-client-error-mssqlserver-50000.md)  
 Enthält Themen zu Laufzeitfehlern, die dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client zugeordnet sind.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Aktualisieren einer Anwendung von SQL Server 2005 Native Client](../../relational-databases/native-client/applications/updating-an-application-from-sql-server-2005-native-client.md)   
 [ODBC-Themen zur Vorgehensweise](../../relational-databases/native-client-odbc-how-to/odbc-how-to-topics.md)   
 [Vorgehensweisen für OLE DB](../../relational-databases/native-client-ole-db-how-to/ole-db-how-to-topics.md)  
  
