---
title: SQLXML 4.0-Programmierkonzepte
description: Anzeigen von Informationen zu den in SQLXML 4,0 verwendeten Programmier Konzepten.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXML, about SQLXML
- SQLXML
ms.assetid: 5a11cda2-b8a3-4453-848f-641afdaa7024
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2699c938d13750e954a4171da610c6a40b74f141
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97429583"
---
# <a name="sqlxml-40-programming-concepts"></a>SQLXML 4.0-Programmierkonzepte
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  SQLXML 3.0 wurde als Webversion angeboten, um zusätzliche clientseitige XML-Funktionalitäten und Erweiterungen zu bestehenden Funktionen wie XSD-Schemas mit Anmerkungen, XML-Massenladen, Unterstützung für Webdienste (SOAP) und Updategrams bereitzustellen.  
  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] führt Version 4.0 von SQLXML ein, die dieselbe Funktionalität wie SQLXML 3.0 bereitstellt. Darüber hinaus bietet sie zusätzliche Updates, um mit neuen Funktionen, die in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] eingeführt wurden, zu funktionieren.  
  
 Dieser Abschnitt enthält Informationen zur SQLXML 4.0.  
  
 [SQLXML ist in SQL Server nicht installiert](../../relational-databases/sqlxml/sqlxml-is-not-installed-in-sql-server.md)  
 Beschreibt die Installation von SQLXML 4.0.  
  
 [Neues in SQLXML 4.0 SP1](../../relational-databases/sqlxml/what-s-new-in-sqlxml-4-0-sp1.md)  
 Beschreibt die Updates und die Erweiterungen in SQLXML 4.0 und stellt Links zu relevanten Themen in dieser Dokumentation bereit.  
  
 [Verwenden von ADO zum Ausführen von SQLXML 4.0-Abfragen](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
 Beschreibt, wie ADO für SQLXML-Abfragen verwendet wird. ADO nimmt in SQLXML 4.0 einen wichtigeren Platz ein als in vorherigen Versionen.  
  
 [XML-Datentypunterstützung für SQLXML 4.0](../../relational-databases/sqlxml/xml-data-type-support-in-sqlxml-4-0.md)  
 Beschreibt die Unterstützung für den XML-Datentyp, der in SQLXML 4.0 hinzugefügt wurde.  
  
 [Anforderungen zum Ausführen von SQLXML-Beispielen](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)  
 Beschreibt die Anforderungen zum Erstellen von funktionstüchtigen Beispielen aus den bereitgestellten SQLXML-Beispielen.  
  
 [Client seitige und Server seitige Formatierung &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml/formatting/client-side-and-server-side-formatting-sqlxml-4-0.md)  
 Liefert Informationen über die clientseitige Formatierung im Vergleich zur serverseitigen Formatierung sowie den FOR XML-Befehl zum Konstruieren von XML-Dokumenten.  
  
 [XSD-Schemas mit Anmerkungen in SQLXML 4.0](../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xsd-schemas-in-sqlxml-4-0.md)  
 Enthält Informationen zum Verwenden von XSD-Schemas mit Anmerkungen in SQLXML 4.0. Stellt außerdem Informationen über XDR-Schemas mit Anmerkungen zur Verwendung in älteren Anwendungen bereit.  
  
 [Verwenden von XPath-Abfragen in SQLXML 4.0](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/using-xpath-queries-in-sqlxml-4-0.md)  
 Beschreibt, wie mit einer Teilmenge der Xpath-Sprache die von dem mit Anmerkungen versehenen XSD-Schema erstellten XML-Sichten abgefragt werden können und liefert Beispiele dazu.  
  
 [Verwenden von Updategrams zum Ändern von Daten in SQLXML 4.0](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/using-updategrams-to-modify-data-in-sqlxml-4-0.md)  
 Stellt Informationen über Updategrams bereit, die Daten in einer Datenbank ändern, indem sie mit den XML-Sichten verwendet werden, die von den mit Anmerkungen versehenen XSD- oder XDR-Schemas erzeugt wurden.  
  
 [Ausführen von Massenladen von XML-Daten &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md)  
 Beschreibt den XML-Massenladevorgang in SQLXML 4.0.  
  
 [SQLXML 4.0 Data Access Components](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/sqlxml-4-0-data-access-components-sqlxmloledb-provider.md)  
 Dokumentiert den SQLXMLOLEDB-Anbieter und stellt Links zu anderen SQLXML 4.0-Datenzugriffskomponenten bereit.  
  
 [SQLXML 4.0 .NET Framework-Unterstützung](../sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-4-0-net-framework-support-managed-classes.md)  
 Beschreibt die SQLXML 4.0-Unterstützung für .NET Framework.  
  
 [Zwischenspeichern von Vorlagen, XSL und Schemas &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/caching-templates-xsl-and-schemas-sqlxml-4-0.md)  
 Beschreibt die in SQLXML zum Optimieren der Leistung bereitgestellte Zwischenspeicherfunktionalität.  
  
 [Sicherheitsüberlegungen zu SQLXML 4.0](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/sqlxml-4-0-security-considerations.md)  
 Erläutert Sicherheitsprobleme in Bezug auf SQLXML 4.0.  
  
 [Richtlinien und Einschränkungen von SQLXML 4.0](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/guidelines-and-limitations-of-sqlxml-4-0.md)  
 Listet Punkte auf, die bei der Arbeit mit SQLXML 4.0 berücksichtigt werden sollten.  
  
## <a name="see-also"></a>Weitere Informationen  
 [XML-Daten &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
