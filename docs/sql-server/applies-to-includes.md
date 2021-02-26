---
title: 'SQL Server-Dokumentation: Includedateien | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
author: MashaMSFT
ms.author: mathoma
ms.topic: conceptual
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: e23fdd773cce72583f561569e106912d8b77db5c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341301"
---
# <a name="sql-server-include-files-for-versioning-and-applies-to"></a>SQL Server-Includedateien für die Versionsverwaltung und „Applies-to“

Mithilfe von Includedateien in Markdown können Verweise in der Dokumentation auf einfache Weise geändert werden, ohne den tatsächlichen Text der jeweiligen Artikel zu ändern. Es gibt drei Arten von Includedateien für SQL-Inhalte: SQL-Version, „Applies-to“ und Referenztext. Includedateien für die **SQL Server-Version** werden verwendet, um anzugeben, von welcher SQL-Version (z. B. SQL Server 2016 oder 2017) gesprochen wird. Die **applies-to**-Includedateien geben an, für welche SQL-Produkte und -Dienste die Dokumentation gilt (z. B. SQL Server für Linux oder Azure SQL-Datenbank). **Referenztexte** sind Includedateien, die nicht in die anderen beiden Kategorien fallen, z. B. die Includedatei „Get Help“, eine Liste mit Links, die Kunden verwenden können, um Hilfe zu SQL Server zu erhalten.

Dieser Artikel soll als Anhaltspunkt für die ersten beiden Arten von Includedateien dienen. Sie können die vollständige Liste der Includedateien im Repository [sql-docs](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs/includes) durchsuchen.

## <a name="sql-server-version-include-files"></a>Includedateien für die SQL Server-Version

Autoren von SQL-Inhalten müssen häufig den Produktnamen und die Version von SQL Server per Include einschließen. Auf diese Weise wird bei einer Namensänderung die Includedatei geändert, statt den Wert in jedem einzelnen Artikel ändern zu müssen. Diese Includedateien werden als Platzhalter für Produktnamen eingesetzt, wurden aber nicht konsistent in der gesamten SQL-Dokumentation verwendet. SQL Server vNext bezieht sich auf eine zukünftige Version von SQL Server, die noch keine Versionsnummer besitzt, und stellt eine Ausnahme dar.  

|SQL-Version| Dateiname| Markdownbeispiel |Text|
| :------------  | :-------------| :----------| :-------------------|
| SQL | ssnoversion-md.md | `[!INCLUDE[ssSQL11](../includes/ssnoversion-md.md)]` | SQL Server |
| SQL 2000 | ssversion2000-md.md | `[!INCLUDE[ssSQL11](../includes/ssversion2000-md.md)]` | SQL Server 2000 (8.x) |
| SQL 2005 | ssversion2005-md.md | `[!INCLUDE[ssSQL11](../includes/ssversion2005-md.md)]` | SQL Server 2005 (9.x) |
| SQL 2012 | sssql11-md.md | `[!INCLUDE[ssSQL11](../includes/sssql11-md.md)]` | SQL Server 2012 (11.x) |
| SQL 2012 SP1 | sssql11sp1-md.md | `[!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)]` | SQL Server 2012 SP1 (11.0.3x) |
| SQL 2014 | sssql14-md.md | `[!INCLUDE[ssSQL14](../includes/sssql14-md.md)]` | SQL Server 2014 (12.x) |
| SQL 2016 | sssql16-md.md | `[!INCLUDE[sssql15-md](../includes/sssql16-md.md)]` | SQL Server 2016 (13.x) |
| SQL 2017 | sssql17-md.md | `[!INCLUDE[sssql17-md](../includes/sssql17-md.md)]` | SQL Server 2017 (14.x) |
| SQL 2017 | sssql17-md.md | `[!INCLUDE[sssql14](../includes/sssql17-md.md)]` | SQL Server 2017 (14.x) |
| SQL vNext | sssql19-md.md | `[!INCLUDE[sssql19-md](../includes/sssql19-md.md)]` | SQL Server vNext |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; |  

## <a name="sql-server-applies-to-non-version-specific"></a>SQL Server: „applies-to“ (nicht versionsspezifisch)

Bei diesen „applies-to“-Includedateien wird die SQL Server-Version ausgelassen.

| Dateiname| Markdownbeispiel |Image|
| :-------------| :----------| :-------------------|
| appliesto-ss-asdb-asdw-xxx-md.md | `[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]` | [!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)] |
| appliesto-ss-asdb-asdw-pdw-md.md | `[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]` | [!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)] |
| appliesto-ss-asdb-xxxx-pdw-md.md | `[!INCLUDE[appliesto-ss-asdb-xxxx-pdw-md.md](../includes/appliesto-ss-asdb-xxxx-pdw-md.md)]` | [!INCLUDE[appliesto-ss-asdb-xxxx-pdw-md.md](../includes/appliesto-ss-asdb-xxxx-pdw-md.md)] |
| appliesto-ss-asdb-xxxx-xxx-md.md | `[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md.md](../includes/applies-to-version/sql-asdb.md)]` | [!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md.md](../includes/applies-to-version/sql-asdb.md)] |
| applies-to-version/sql-asdbmi.md | `[!INCLUDE[applies-to-version/sql-asdbmi.md](../includes/applies-to-version/sql-asdbmi.md)]` | [!INCLUDE[applies-to-version/sql-asdbmi.md](../includes/applies-to-version/sql-asdbmi.md)] |
| appliesto-ss-xxxx-asdw-pdw-md.md | `[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md.md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]` | [!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md.md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)] |
| appliesto-ss-xxxx-xxxx-pdw-md.md | `[!INCLUDE[appliesto-ss-xxxx-xxxx-pdw-md.md](../includes/appliesto-ss-xxxx-xxxx-pdw-md.md)]` | [!INCLUDE[appliesto-ss-xxxx-xxxx-pdw-md.md](../includes/appliesto-ss-xxxx-xxxx-pdw-md.md)] |
| appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md | `[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]` | [!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)] |
| appliesto-ss-xxxx-xxxx-xxx-md-winonly.md | `[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly.md](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]` | [!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly.md](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)] |
| applies-to-version/_ssnoversion.md | `[!INCLUDE[applies-to-version/_ssnoversion.md](../includes/applies-to-version/sqlserver.md)]` | [!INCLUDE[applies-to-version/_ssnoversion.md](../includes/applies-to-version/sqlserver.md)] |
| appliesto-xx-asdb-asdw-xxx-md.md | `[!INCLUDE[appliesto-xx-asdb-asdw-xxx-md.md](../includes/appliesto-xx-asdb-asdw-xxx-md.md)]` | [!INCLUDE[appliesto-xx-asdb-asdw-xxx-md.md](../includes/appliesto-xx-asdb-asdw-xxx-md.md)] |
| appliesto-xx-asdb-xxxx-xxx-md.md | `[!INCLUDE[appliesto-xx-asdb-xxxx-xxx-md.md](../includes/appliesto-xx-asdb-xxxx-xxx-md.md)]` | [!INCLUDE[appliesto-xx-asdb-xxxx-xxx-md.md](../includes/appliesto-xx-asdb-xxxx-xxx-md.md)] |
| appliesto-xx-xxxx-asdw-pdw-md.md | `[!INCLUDE[appliesto-xx-xxxx-asdw-pdw-md.md](../includes/appliesto-xx-xxxx-asdw-pdw-md.md)]` | [!INCLUDE[appliesto-xx-xxxx-asdw-pdw-md.md](../includes/appliesto-xx-xxxx-asdw-pdw-md.md)] |
| appliesto-xx-xxxx-asdw-xxx-md.md | `[!INCLUDE[appliesto-xx-xxxx-asdw-xxx-md.md](../includes/appliesto-xx-xxxx-asdw-xxx-md.md)]` | [!INCLUDE[appliesto-xx-xxxx-asdw-xxx-md.md](../includes/appliesto-xx-xxxx-asdw-xxx-md.md)] |
|&nbsp; | &nbsp; | &nbsp; |  
 
## <a name="sql-server-applies-to-version-specific"></a>SQL Server: „applies-to“ (versionsspezifisch)

Diese „applies-to“-Includedateien geben an, für welche Versionen von SQL die Dokumentation gilt.

| Dateiname| Markdownbeispiel |Image|
| :-------------| :----------| :-------------------|
| tsql-appliesto-ss2008-all-md.md | `[!INCLUDE[tsql-appliesto-ss2008-all-md.md](../includes/tsql-appliesto-ss2008-all-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-all-md.md](../includes/tsql-appliesto-ss2008-all-md.md)] |
| tsql-appliesto-ss2008-all-md.md | `[!INCLUDE[tsql-appliesto-ss2008-all-md.md](../includes/tsql-appliesto-ss2008-all-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-all-md.md](../includes/tsql-appliesto-ss2008-all-md.md)] |
| tsql-appliesto-ss2008-asdb-asdw-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-xxx-md.md](../includes/tsql-appliesto-ss2008-asdb-asdw-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-xxx-md.md](../includes/tsql-appliesto-ss2008-asdb-asdw-xxx-md.md)] |
| tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2008-asdbmi-xxxx-pdw-md.md | `[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-pdw-md.md](../includes/tsql-appliesto-ss2008-asdbmi-xxxx-pdw-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-pdw-md.md](../includes/tsql-appliesto-ss2008-asdbmi-xxxx-pdw-md.md)] |
| tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md | `[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md](../includes/tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md](../includes/tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md)] |
| tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md | `[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md](../includes/tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md](../includes/tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md)] |
| tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md](../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md](../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)] |
| tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md](../includes/applies-to-version/sqlserver.md)]` | [!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md](../includes/applies-to-version/sqlserver.md)] |
| tsql-appliesto-ss2012-all-md.md | `[!INCLUDE[tsql-appliesto-ss2012-all-md.md](tsql-appliesto-ss2012-all-md.md)]` | [!INCLUDE[../includes/tsql-appliesto-ss2012-all-md.md](../includes/tsql-appliesto-ss2012-all-md.md)] |
| tsql-appliesto-ss2012-asdb-asdw-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2012-asdb-asdw-xxx-md.md](../includes/tsql-appliesto-ss2012-asdb-asdw-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2012-asdb-asdw-xxx-md.md](../includes/tsql-appliesto-ss2012-asdb-asdw-xxx-md.md)] |
| tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2012-asdb-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md | `[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md)]` | [!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md)] |
| tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2014-asdb-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md)] |
| tsql-appliesto-2014sp2-asdb-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-2014sp2-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-2014sp2-asdb-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-2014sp2-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-2014sp2-asdb-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2016-all-md.md | `[!INCLUDE[tsql-appliesto-ss2016-all-md.md](tsql-appliesto-ss2016-all-md.md)]` | [!INCLUDE[tsql-appliesto-ss2016-all-md.md](../includes/tsql-appliesto-ss2016-all-md.md)] |
| tsql-appliesto-ss2016-asdb-asdw-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2016-asdb-asdw-xxx-md.md](../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2016-asdb-asdw-xxx-md.md](../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)] |
| tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2016-xxxx-asdw-pdw-md.md | `[!INCLUDE[tsql-appliesto-ss2016-xxxx-asdw-pdw-md.md](../includes/tsql-appliesto-ss2016-xxxx-asdw-pdw-md.md)]` | [!INCLUDE[tsql-appliesto-ss2016-xxxx-asdw-pdw-md.md](../includes/tsql-appliesto-ss2016-xxxx-asdw-pdw-md.md)] |
| tsql-appliesto-ss2016-xxxx-asdw-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2016-xxxx-asdw-xxx-md.md](../includes/tsql-appliesto-ss2016-xxxx-asdw-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2016-xxxx-asdw-xxx-md.md](../includes/tsql-appliesto-ss2016-xxxx-asdw-xxx-md.md)] |
| tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md](../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2016-xxxx-xxxx-xxx-md-winonly.md | `[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md-winonly.md](../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md-winonly.md)]` | [!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md-winonly.md](../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md-winonly.md)] |
| tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)] |
| tsql-appliesto-ss2017-all-md.md | `[!INCLUDE[tsql-appliesto-ss2017-all-md.md](tsql-appliesto-ss2017-all-md.md)]` | [!INCLUDE[tsql-appliesto-ss2017-all-md.md](../includes/tsql-appliesto-ss2017-all-md.md)] |
| tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md](../includes/applies-to-version/sqlserver2017.md)]` | [!INCLUDE[tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md](../includes/applies-to-version/sqlserver2017.md)] |
| sqlserver2017-asdb.md | `[!INCLUDE[sqlserver2017-asdb.md](../includes/applies-to-version/sqlserver2017-asdb.md)]` | [!INCLUDE[sqlserver2017-asdb.md](../includes/applies-to-version/sqlserver2017-asdb.md)] |
| tsql-appliesto-xxxxxx-asdb-asdw-xxx-md.md | `[!INCLUDE[tsql-appliesto-xxxxxx-asdb-asdw-xxx-md.md](../includes/tsql-appliesto-xxxxxx-asdb-asdw-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-xxxxxx-asdb-asdw-xxx-md.md](../includes/tsql-appliesto-xxxxxx-asdb-asdw-xxx-md.md)] |
| tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md | `[!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md](../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)] |
| applies-to-version/asa-pdw.md | `[!INCLUDE[applies-to-version/asa-pdw.md](../includes/applies-to-version/asa-pdw.md)]` | [!INCLUDE[applies-to-version/asa-pdw.md](../includes/applies-to-version/asa-pdw.md)] |
| tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md.md | `[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md.md](../includes/tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md.md)]` | [!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md.md](../includes/tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md.md)] |
| tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md | `[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md](../includes/tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md)]` | [!INCLUDE[tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md](../includes/tsql-appliesto-xxxxxx-xxxx-xxxx-pdw-md.md)] |
|&nbsp; | &nbsp; | &nbsp; |  

## <a name="analysis-services-applies-to"></a>Analysis Services: „applies-to“

Diese „applies-to“-Includedateien werden mit der Analysis Services-Dokumentation verwendet.

| Dateiname| Markdownbeispiel |Image|
| :-------------| :----------| :-------------------|
| ssas-appliesto-sql2016.md | `[!INCLUDE[ssas-appliesto-sql2016.md](../includes/ssas-appliesto-sql2016.md)]` | [!INCLUDE[ssas-appliesto-sql2016.md](../includes/ssas-appliesto-sql2016.md)] |
| ssas-appliesto-sql2016-later.md | `[!INCLUDE[ssas-appliesto-sql2016-later.md](../includes/ssas-appliesto-sql2016-later.md)]` | [!INCLUDE[ssas-appliesto-sql2016-later.md](../includes/ssas-appliesto-sql2016-later.md)] |
| ssas-appliesto-sql2016-later-aas.md | `[!INCLUDE[ssas-appliesto-sql2016-later-aas.md](../includes/ssas-appliesto-sql2016-later-aas.md)]` | [!INCLUDE[ssas-appliesto-sql2016-later-aas.md](../includes/ssas-appliesto-sql2016-later-aas.md)] |
| ssas-appliesto-sql2017.md | `[!INCLUDE[ssas-appliesto-sql2017.md](../includes/ssas-appliesto-sql2017.md)]` | [!INCLUDE[ssas-appliesto-sql2017.md](../includes/ssas-appliesto-sql2017.md)] |
| ssas-appliesto-sql2017-later-aas.md | `[!INCLUDE[ssas-appliesto-sql2017-later-aas.md](../includes/ssas-appliesto-sql2017-later-aas.md)]` | [!INCLUDE[ssas-appliesto-sql2017-later-aas.md](../includes/ssas-appliesto-sql2017-later-aas.md)] |
| ssas.md | `[!INCLUDE[ssas.md](../includes/applies-to-version/ssas.md)]` | [!INCLUDE[ssas.md](../includes/applies-to-version/ssas.md)] |
| ssas-appliesto-sqlas-aas.md | `[!INCLUDE[ssas-aas.md](../includes/ssas-appliesto-sqlas-aas.md)]` | [!INCLUDE[ssas-appliesto-sqlas-aas.md](../includes/ssas-appliesto-sqlas-aas.md)] |
| ssas-appliesto-sqlas-all.md | `[!INCLUDE[ssas-all.md](../includes/ssas-appliesto-sqlas-all.md)]` | [!INCLUDE[ssas-appliesto-sqlas-all.md](../includes/ssas-appliesto-sqlas-all.md)] |
| ssas-appliesto-sqlas-all-aas.md | `[!INCLUDE[ssas-all-aas.md](../includes/ssas-appliesto-sqlas-all-aas.md)]` | [!INCLUDE[ssas-appliesto-sqlas-all-aas.md](../includes/ssas-appliesto-sqlas-all-aas.md)] |
|&nbsp; | &nbsp; | &nbsp; |  

## <a name="reporting-services-applies-to"></a>Reporting Services: „applies-to“

Diese „applies-to“-Includedateien werden mit der Reporting Services-Dokumentation verwendet.

| Dateiname| Markdownbeispiel |Image|
| :-------------| :----------| :-------------------|
| ssrs-appliesto-2017-and-later.md | `[!INCLUDE[ssrs-appliesto-2017-and-later.md](../includes/ssrs-appliesto-2017-and-later.md)]` | [!INCLUDE[ssrs-appliesto-2017-and-later.md](../includes/ssrs-appliesto-2017-and-later.md)] |
| ssrs-appliesto-not-pbirs.md | `[!INCLUDE[ssrs-appliesto-not-pbirs.md](../includes/ssrs-appliesto-not-pbirs.md)]` | [!INCLUDE[ssrs-appliesto-not-pbirs.md](../includes/ssrs-appliesto-not-pbirs.md)] |
| ssrs-appliesto-pbirs.md | `[!INCLUDE[ssrs-appliesto-pbirs.md](../includes/ssrs-appliesto-pbirs.md)]` | [!INCLUDE[ssrs-appliesto-pbirs.md](../includes/ssrs-appliesto-pbirs.md)] |
| ssrs-appliesto-sharepoint-2013-2016.md | `[!INCLUDE[ssrs-appliesto-sharepoint-2013-2016.md](../includes/ssrs-appliesto-sharepoint-2013-2016.md)]` | [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016.md](../includes/ssrs-appliesto-sharepoint-2013-2016.md)] |
| ssrs-appliesto-sql2016-preview.md | `[!INCLUDE[ssrs-appliesto-sql2016-preview.md](../includes/ssrs-appliesto-sql2016-preview.md)]` | [!INCLUDE[ssrs-appliesto-sql2016-preview.md](../includes/ssrs-appliesto-sql2016-preview.md)] |
|&nbsp; | &nbsp; | &nbsp; |  

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung dieser Includedateien finden Sie unter [Applies-to includes](sql-server-docs-contribute.md#applies-to-includes).

Dies ist ein Test.
