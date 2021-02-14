---
title: Projektorientierte Datenbankentwicklung mit Befehlszeilentools
description: Zeigen Sie verfügbare Ressourcen in Befehlszeilentools an, die SQL Server Data Tools zum Arbeiten mit DACPAC-Dateien bereitstellen (z. B. „SQLPackage.exe“ und „dbSqlPackage“).
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 9a26def9-8fbd-43e4-9e57-414840b73ed8
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 04/26/2017
ms.openlocfilehash: 1439673e40faf8da8cf65da673f0145374b48abd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081891"
---
# <a name="project-oriented-database-development-using-command-line-tools"></a>Projektorientierte Datenbankentwicklung mit Befehlszeilentools

SQL Server Data Tools stellt Befehlszeilentools bereit, die eine Reihe von Szenarios für die projektorientierte Datenbankentwicklung ermöglichen.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
  
|Thema|BESCHREIBUNG|  
|-|-|  
|[SqlPackage.exe](../tools/sqlpackage/sqlpackage.md)|In diesem Thema wird das Hilfsprogramm SQLPackage.exe beschrieben, das für die folgenden Aufgaben verwendet wird:<br /><br />– Extrahieren einer DACPAC-Datei aus einer SQL Server-Livedatenbank<br />– Veröffentlichen einer DACPAC-Datei in einer SQL Server-Livedatenbank, um das Livedatenbankschema inkrementell so zu aktualisieren, dass es mit der DACPAC-Datei übereinstimmt<br />– Vergleichen einer DACPAC-Datei mit einer SQL Server-Livedatenbank und Generieren eines Transact\-SQL-Skripts für inkrementelle Upgrades, ohne dass die Livedatenbank aktualisiert wird<br />– Vergleichen von zwei DACPAC-Dateien, um ein Transact\-SQL-Skript für inkrementelle Upgrades zu generieren<br />– Generieren eines XML-Berichts mit einer Übersicht über die Änderungen durch inkrementelle Upgrades, die bei inkrementellen Upgrades der Datenbank auftreten würden|  
|[Verwenden von MSDeploy mit dem dbSqlPackage-Anbieter](../ssdt/using-msdeploy-with-dbsqlpackage-provider.md)|In diesem Thema wird der in SSDT enthaltene [Webbereitstellungstool](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd568996(v=ws.10))-Anbieter „dbSqlPackage“ beschrieben. Dieser wird zusammen mit dem Webbereitstellungstool der Microsoft-Internetinformationsdienste (IIS) (MSDeploy.exe) für folgende Aufgaben verwendet:<br /><br />– Extrahieren einer DACPAC-Datei aus einer lokalen SQL Server- oder Azure SQL-Datenbank oder SQL Server- bzw. Azure SQL-Remotedatenbank<br />– Veröffentlichen einer DACPAC-Datei in einer lokalen SQL Server- oder Azure SQL-Datenbank oder SQL Server- bzw. Azure SQL-Remotedatenbank, um inkrementelle Upgrades durchzuführen<br />– Veröffentlichen aus einer lokalen SQL Server-Datenbank in einer SQL Server- oder Azure SQL-Remotedatenbank, um inkrementelle Upgrades für die Remotedatenbank durchzuführen<br />– Vergleichen einer DACPAC-Datei mit einer lokalen SQL Server- oder Azure SQL-Datenbank oder SQL Server- bzw. Azure SQL-Remotedatenbank, um ein Transact\-SQL-Skript für inkrementelle Upgrades zu generieren, ohne dass die Livedatenbank aktualisiert wird.<br />– Generieren eines XML-Berichts mit einer Übersicht über die Änderungen durch inkrementelle Upgrades, die bei inkrementellen Upgrades der Datenbank auftreten würden|  
  
## <a name="related-sections"></a>Verwandte Abschnitte  
[Projektorientierte Offlinedatenbankentwicklung](../ssdt/project-oriented-offline-database-development.md)  
