---
title: Frühere Releases von SQL Server Data Tools (SSDT)
description: Hier erfahren Sie, welche Versionen von SSDT und SSDT-BI mit welchen Versionen von Visual Studio funktionieren. Außerdem wird gezeigt, wie Sie verschiedene Versionen von SSDT und SSDT-BI installieren.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 5d32e301-0f44-4916-b0db-76e8322c0ab7
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 06/17/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=azuresqldb-mi-current'
ms.openlocfilehash: 3e2a3e607b561ede827652f61032b90fc7efa336
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353995"
---
# <a name="previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi"></a>Vorgängerversionen von SQL Server Data Tools (SSDT und SSDT-BI)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SQL Server Data Tools (SSDT) stellt Projektvorlagen und Entwurfsoberflächen zum Erstellen von SQL Server-Inhaltstypen bereit: relationale Datenbanken, Analysis Services-Modelle, Reporting Services-Berichte und Integration Services-Pakete.

SSDT ist abwärtskompatibel, d.h., Sie können jederzeit [das neueste SSDT](download-sql-server-data-tools-ssdt.md) verwenden, um Datenbanken, Modelle, Berichte und Pakete zu entwerfen und bereitzustellen, die für ältere Versionen von SQL Server vorgesehen sind.

Die Visual Studio-Shell, mit der SQL Server-Inhaltstypen erstellt werden, wurde unter verschiedenen Namen veröffentlicht, etwa **SQL Server Data Tools**, **SQL Server Data Tools - Business Intelligence** und **Business Intelligence Development Studio**. Zum Lieferumfang von früheren Versionen gehörten unterschiedliche Sätze von Projektvorlagen. Möchten Sie alle Projektvorlagen zusammen in einem SSDT haben, benötigen Sie [die neueste Version](download-sql-server-data-tools-ssdt.md). Andernfalls müssen Sie wahrscheinlich mehrere frühere Versionen installieren, um alle in SQL Server verwendeten Vorlagen zu erhalten. Pro Version von Visual Studio ist nur eine Shell installiert. Wird ein zweites SSDT installiert, werden nur die fehlenden Vorlagen hinzugefügt.

## <a name="previous-ssdt-releases"></a>Frühere SSDT-Releases

Laden Sie frühere SSDT-Versionen herunter, indem Sie auf den Downloadlink im entsprechenden Abschnitt klicken.

| SSDT-Version | Visual Studio-Version |
|--------------|-----------------------|
| [15.8](#ssdt-for-visual-studio-vs-2017) | 2017 |
| [17.4](#ssdt-for-visual-studio-vs-2015) | 2015 |
| [16.5](#ssdt-for-visual-studio-vs-2013) | 2013 |
| [11.1.50727.1](#ssdt-for-visual-studio-vs-2012) | 2012 |

### <a name="ssdt-for-visual-studio-vs-2017"></a>SSDT für Visual Studio (VS) 2017

**[SSDT für Visual Studio 2017 (15.8) herunterladen](https://go.microsoft.com/fwlink/?linkid=2124319)**

Dieses Release von **SSDT für Visual Studio 2017** kann in den folgenden Sprachen installiert werden:

[Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x40a)

### <a name="ssdt-for-visual-studio-vs-2015"></a>SSDT für Visual Studio (VS) 2015

Sie müssen ein ISO-Image herunterladen, um diese Version von SSDT zu installieren. Hierbei handelt es sich um eine eigenständige Datei, die alle für SSDT erforderlichen Komponenten enthält und mit einem Download-Manager heruntergeladen werden kann, der neu gestartet werden kann. Dies ist hilfreich bei begrenzter oder weniger zuverlässiger Netzwerkbandbreite. Nach dem Download kann das ISO-Image als Laufwerk eingebunden werden.

Installationsschritte:

1. **[SSDT für Visual Studio 2015 (17.4) herunterladen](https://go.microsoft.com/fwlink/?linkid=2132817)**

2. Öffnen Sie das ISO-Image.

3. Führen Sie die Datei *SSDTSetup.exe* aus.

    ![ISO-Image](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

Dieses Release von **SSDT für Visual Studio 2015** kann in den folgenden Sprachen installiert werden:

| Sprache | SHA256-Hash |
|----------|-------------|
| [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x804) | 79A958B122DDEC2857F1F4B9F0272A6BD2106BB17B4DA94CC68CACFCDDC16EAE |
| [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x404) | 8F9661883F2D2D28D6928AE66242DB465DBB25696EDFE0348D222421CEAB000A |
| [Englisch (USA)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x409) | 7727BA227A9E49C2DFCC1E9F2A10924CC472E3425653DC7DE8E4B830712B302E |
| [Französisch](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x40c) | 2DF6655819F604E8D20A396CA9FDFEE279C5A9E50776FFC143A5BA4C3E2F1849 |
| [Deutsch](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x407) | 5C44502DEE8B31675D0B10B4CE8CA6F5D96A2692CC498B9859A77C24F30EF870 |
| [Italienisch](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x410) | 6A616F6E3A1C7DD52457FB8C8692E5C1C551032FF46AD8C5112CF9364F17C631 |
| [Japanisch](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x411) | 1244A5EADF673E4BD36E9967A2008F65CA2A1D40E8E7DD4D17640A6FC1EACA9A |
| [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x412) | 6E0E057A853F54AB87F3F8984954F590FCCD3BE2EC2139F02EFA085FEA6D3E61 |
| [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x416) | 24122C092464B299F41A7644814F375F0CC2786877BCAE0282221FF8D73BD100 |
| [Russisch](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x419) | 2CDE208C241C3F13D2EC37294C10503C7A9D1222ED33F6FE54849169F30BE697 |
| [Spanisch](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x40a) | BD12844E6F0A0ECFD5815D01EDFB8018CE9B4A1044DE8DF96AC740D85E800FD6 |

### <a name="ssdt-for-visual-studio-vs-2013"></a>SSDT für Visual Studio (VS) 2013

Sie müssen ein ISO-Image herunterladen, um diese Version von SSDT zu installieren. Hierbei handelt es sich um eine eigenständige Datei, die alle für SSDT erforderlichen Komponenten enthält und mit einem Download-Manager heruntergeladen werden kann, der neu gestartet werden kann. Dies ist hilfreich bei begrenzter oder weniger zuverlässiger Netzwerkbandbreite. Nach dem Download kann das ISO-Image als Laufwerk eingebunden werden.

Installationsschritte:

1. **[SSDT für Visual Studio 2013 (16.5) herunterladen](https://go.microsoft.com/fwlink/?linkid=832312)**

2. Öffnen Sie das ISO-Image.

3. Führen Sie die Datei *SSDTSetup.exe* aus.

    ![ISO-Image](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

Dieses Release von **SSDT für Visual Studio 2013** kann in den folgenden Sprachen installiert werden:

| Sprache | SHA256-Hash |
|----------|-------------|
| [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x804) | 79A958B122DDEC2857F1F4B9F0272A6BD2106BB17B4DA94CC68CACFCDDC16EAE |
| [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x404) | 8F9661883F2D2D28D6928AE66242DB465DBB25696EDFE0348D222421CEAB000A |
| [Englisch (USA)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x409) | 7727BA227A9E49C2DFCC1E9F2A10924CC472E3425653DC7DE8E4B830712B302E |
| [Französisch](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x40c) | 2DF6655819F604E8D20A396CA9FDFEE279C5A9E50776FFC143A5BA4C3E2F1849 |
| [Deutsch](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x407) | 5C44502DEE8B31675D0B10B4CE8CA6F5D96A2692CC498B9859A77C24F30EF870 |
| [Italienisch](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x410) | 6A616F6E3A1C7DD52457FB8C8692E5C1C551032FF46AD8C5112CF9364F17C631 |
| [Japanisch](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x411) | 1244A5EADF673E4BD36E9967A2008F65CA2A1D40E8E7DD4D17640A6FC1EACA9A |
| [Koreanisch](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x412) | 6E0E057A853F54AB87F3F8984954F590FCCD3BE2EC2139F02EFA085FEA6D3E61 |
| [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x416) | 24122C092464B299F41A7644814F375F0CC2786877BCAE0282221FF8D73BD100 |
| [Russisch](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x419) | 2CDE208C241C3F13D2EC37294C10503C7A9D1222ED33F6FE54849169F30BE697 |
| [Spanisch](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x40a) | BD12844E6F0A0ECFD5815D01EDFB8018CE9B4A1044DE8DF96AC740D85E800FD6 |

### <a name="ssdt-for-visual-studio-vs-2012"></a>SSDT für Visual Studio (VS) 2012

Sie müssen ein ISO-Image herunterladen, um diese Version von SSDT zu installieren. Hierbei handelt es sich um eine eigenständige Datei, die alle für SSDT erforderlichen Komponenten enthält und mit einem Download-Manager heruntergeladen werden kann, der neu gestartet werden kann. Dies ist hilfreich bei begrenzter oder weniger zuverlässiger Netzwerkbandbreite. Nach dem Download kann das ISO-Image als Laufwerk eingebunden werden.

Installationsschritte:

1. **[SSDT für Visual Studio 2012 (11.1.50727.1) herunterladen](https://go.microsoft.com/fwlink/?linkid=518814)**

2. Öffnen Sie das ISO-Image.

3. Führen Sie die Datei *SSDTSetup.exe* aus.

    ![ISO-Image](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

Dieses Release von **SSDT für Visual Studio 2012** kann in den folgenden Sprachen installiert werden:

| Sprache | SHA256-Hash |
|----------|-------------|
| [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x804) | 78F20325648CFF49D9C58A26186E0AC199D104B3CF634E5663B4B262BEC69C07 |
| [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x404) | A2722380AF2EE1E8BB52B4FA54A61BAB411E5E5FD5B050108F81ED23DC87366D |
| [Englisch (USA)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x409) | 748EF78D3F9CC6FE360432C378EA690DE51AEB2C747E87D43E08448D26F93A91 |
| [Französisch](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x40c) | 40E6504169BF618EDC7EB5B62D1215DC96726D6EEC3CA8EC3EEB49044E4B6FB7 |
| [Deutsch](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x407) | C45C974E6B8F9611BA2AC1EE90C5C507992BCE5693BF46F6C7C138591ED6A123 |
| [Italienisch](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x410) | C1B20CDB41C7B1B5DE76A71F9A091A6FDF5186B12F6AA269DA6338B3CB2D91A6 |
| [Japanisch](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x411) | BF56958B904C1C5F28084BD0B16928B6CBFEC83EB3F0C913EC364FA335241943 |
| [Koreanisch](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x412) | 9023B0656785C357A67E39EB76A2806B923C2BD17342D7226A8ADA384A9F4E28 |
| [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x416) | ACAD9FE03729E289ECE821D92C56CFB1D7FCCEA8423ABF1E164BF3C67ABEFEFB |
| [Russisch](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x419) | E2191D787BA833DF4A85B064C5373DC44099E76214FBF9505728702D4D6B83F0 |
| [Spanisch](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x40a) | 6D81FB572A7003C54C29D2ACF076D2CED4A1CA80F329BFF9D41A806920D64EEE |

> [!Note]
> SSDT unterstützt die beiden neuesten Versionen von Visual Studio. Mit der Veröffentlichung von Visual Studio 2019 werden SSDT-Versionen für Visual Studio 2015 und ältere Versionen nicht mehr aktualisiert. SSDT für Visual Studio 2010 ist nicht mehr verfügbar. Weitere Informationen finden Sie im Abschnitt *Häufig gestellte Fragen* [dieses Blogbeitrags des SSDT-Teams](/archive/blogs/ssdt/sql-server-data-tools-17-0-rc-and-ssdt-in-vs2017).

## <a name="sql-bi-analysis-services-reporting-services-integration-services"></a>SQL BI: Analysis Services, Reporting Services und Integration Services

BI-Vorlagen werden verwendet, um SSAS-Modelle, SSRS-Berichte und SSIS-Pakete zu erstellen. BI-Designer sind an bestimmte Versionen von SQL Server gebunden. Damit Sie die neueren BI-Funktionen nutzen können, installieren Sie den BI-Designer in einer neueren Version.

## <a name="bi-designers"></a>BI-Designer

[Herunterladen von SSDT-BI für Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313) (SQL Server 2014, SQL Server 2012, SQL Server 2008 und 2008 R2)

[Herunterladen von SSDT-BI für Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843) (SQL Server 2014, SQL Server 2012, SQL Server 2008 und 2008 R2)

Business Intelligence Development Studio (BIDS) wird über SQL Server-Setup installiert. Es steht kein Webdownload zur Verfügung (SQL Server 2008 und 2008 R2).

Für SQL Server 2012 oder 2014 können Sie entweder **SSDT-BI für Visual Studio 2012** oder **SSDT-BI foder Visual Studio 2013**. Der einzige Unterschied zwischen den beiden besteht in der Visual Studio-Version.

## <a name="next-steps"></a>Nächste Schritte

- [Download der neuesten SQL Server-Datatools](../ssdt/download-sql-server-data-tools-ssdt.md)
- [Versionshinweise für SQL Server Data Tools (SSDT)](release-notes-ssdt.md)
- [Herunterladen von SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)
- [Herunterladen und Installieren von Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
- [SQL Tools and Utilities (Tools und Hilfsprogramme für SQL)](../tools/overview-sql-tools.md)