---
title: Herunterladen des ODBC-Treibers für SQL Server
description: Laden Sie den Microsoft ODBC Driver for SQL Server herunter, um Anwendungen mit nativem Code zu entwickeln, die eine Verbindung mit SQL Server und Azure SQL-Datenbank herstellen.
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 53b09784-bb9d-4fd4-99d3-0492b3308ac4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 778d8f2f53ba56436047eee53d17dd827c58f3eb
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837571"
---
# <a name="download-odbc-driver-for-sql-server"></a>Herunterladen des ODBC-Treibers für SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Microsoft ODBC Driver for SQL Server ist eine einzelne Dynamic-Link Library (DLL), die Laufzeitunterstützung für Anwendungen bietet, die APIs mit nativem Code verwenden, um eine Verbindung zu SQL Server herzustellen. Microsoft ODBC Driver 17 for SQL Server sollte für die Erstellung neuer Anwendungen oder zur Verbesserung vorhandener Anwendungen eingesetzt werden, die die neueren SQL Server-Features nutzen.

> [!Important]
> Wenn Sie die Azure Active Directory-Authentifizierung mit verwalteter Identität (`authentication=ActiveDirectoryMSI`) mit dem Microsoft ODBC-Treiber für SQL Server verwenden, führen Sie kein Upgrade auf Version 17.7 durch. In 17.7 gibt es ein Problem, das bei Verwendung dieses Authentifizierungsmodus zu einem Fehler führt.

## <a name="download-for-windows"></a>Herunterladen für Windows

Das weitervertreibbare Installationsprogramm für Microsoft ODBC Driver 17 for SQL Server installiert die Clientkomponenten, die während der Laufzeit erforderlich sind, um neuere SQL Server-Features nutzen zu können. Optional werden auch die Headerdateien installiert, die erforderlich sind, um eine Anwendung zu entwickeln, die die ODBC-API verwendet. Ab der Version 17.4.2 schließt das Installationsprogramm auch die Microsoft Active Directory-Authentifizierungsbibliothek (ADAL.dll) mit ein und installiert sie.

Version 17.7.1 ist die neueste allgemein verfügbare Version. Wenn Sie eine frühere Version des Microsoft ODBC Driver 17 for SQL Server installiert haben, können Sie durch Installieren von Version 17.7.1 ein Upgrade auf Version 17.7.1 durchführen.

**[![Download](../../ssms/media/download-icon.png) Herunterladen von Microsoft OLE DB Driver 17 for SQL Server (x64)](https://go.microsoft.com/fwlink/?linkid=2153471)**  
**[![Download](../../ssms/media/download-icon.png) Herunterladen von Microsoft OLE DB Driver 17 for SQL Server (x86)](https://go.microsoft.com/fwlink/?linkid=2153469)**  

### <a name="version-information"></a>Versionsinformationen

- Releasenummer: 17.7.1.1
- Veröffentlichung: 29. Januar 2021

> [!Note]
> Wenn Sie von einer nicht englischsprachigen Version auf diese Seite zugreifen und den neuesten Inhalt anzeigen möchten, besuchen Sie die [US-englische Version der Website](). Sie können verschiedene Sprachen von der US-englischen Website herunterladen, indem Sie [available languages](#available-languages) (verfügbare Sprachen) auswählen.

## <a name="available-languages"></a>Verfügbare Sprachen

Dieses Release von Microsoft ODBC Driver for SQL Server kann in den folgenden Sprachen installiert werden:

Microsoft ODBC Driver 17.7.1 for SQL Server (x64):  
[Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x40a)

Microsoft ODBC Driver 17.7.1 for SQL Server (x86):  
[Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x40a)

### <a name="release-notes-for-windows"></a>Versionshinweise für Windows

Weitere Informationen zu diesem Release unter Windows finden Sie unter [Versionshinweise zu ODBC für SQL Server unter Windows](windows\release-notes-odbc-sql-server-windows.md).

### <a name="previous-releases-for-windows"></a>Ältere Releases für Windows

Wenn Sie ältere Releases für Windows herunterladen möchten, finden Sie unter [Ältere Releases](windows\release-notes-odbc-sql-server-windows.md#previous-releases) weitere Informationen.

## <a name="download-for-linux-and-macos"></a>Download für Linux und macOS

Microsoft ODBC Driver for SQL Server kann mithilfe der Paket-Manager für Linux und macOS mithilfe der entsprechenden Installationsanweisungen heruntergeladen und installiert werden:  
[Installieren von Microsoft ODBC Driver for SQL Server unter Linux und macOS](linux-mac\installing-the-microsoft-odbc-driver-for-sql-server.md) (Linux)  
[Installieren von Microsoft ODBC Driver for SQL Server](linux-mac\install-microsoft-odbc-driver-sql-server-macos.md) (macOS)  

Wenn Sie die Pakete für eine Offlineinstallation herunterladen möchten, stehen alle Versionen auch über die untenstehenden Links zur Verfügung.

> [!Note]
> Pakete namens `msodbcsql17-*` enthalten die aktuelle Version. Pakete namens `msodbcsql-*` enthalten die Version 13 des Treibers.

### <a name="alpine"></a>Alpine

- [Alpine-Paket 17.7.1.1](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.1.1-1_amd64.apk) ([PGP-Signatur](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.1.1-1_amd64.sig))
- [Alpine-Paket 17.6.1.1](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.6.1.1-1_amd64.apk) ([PGP-Signatur](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.6.1.1-1_amd64.sig))
- [Alpine-Paket 17.5.2.2](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.2.2-1_amd64.apk) ([PGP-Signatur](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.2.2-1_amd64.sig))
- [Alpine-Paket 17.5.2.1](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.2.1-1_amd64.apk) ([PGP-Signatur](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.2.1-1_amd64.sig))
- [Alpine-Paket 17.5.1.1](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.1.1-1_amd64.apk) ([PGP-Signatur](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.1.1-1_amd64.sig))

### <a name="debian"></a>Debian

- [Debian 10 .deb-Pakete](https://packages.microsoft.com/debian/10/prod/pool/main/m/msodbcsql17/)
- [Debian 9 .deb-Pakete](https://packages.microsoft.com/debian/9/prod/pool/main/m/msodbcsql17/)
- [Debian 8 .deb-Pakete](https://packages.microsoft.com/debian/8/prod/pool/main/m/msodbcsql17/)
- [Debian 8 .deb-Pakete (msodbcsql 13.x)](https://packages.microsoft.com/debian/8/prod/pool/main/m/msodbcsql/)

### <a name="redhat"></a>RedHat

- [RedHat 8 .rpm-Pakete](https://packages.microsoft.com/rhel/8/prod/)
- [RedHat 7 .rpm-Pakete](https://packages.microsoft.com/rhel/7/prod/)
- [RedHat 6 .rpm-Pakete](https://packages.microsoft.com/rhel/6/prod/)

### <a name="suse"></a>Suse

- [SuSE 15 .rpm-Pakete](https://packages.microsoft.com/sles/15/prod/)
- [SuSE 12 .rpm-Pakete](https://packages.microsoft.com/sles/12/prod/)
- [SuSE 11 .rpm-Pakete](https://packages.microsoft.com/sles/11/prod/)

### <a name="ubuntu"></a>Ubuntu

- [Ubuntu 20.10-DEB-Pakete](https://packages.microsoft.com/ubuntu/20.10/prod/pool/main/m/msodbcsql17/)
- [Ubuntu 20.04 .deb-Pakete](https://packages.microsoft.com/ubuntu/20.04/prod/pool/main/m/msodbcsql17/)
- [Ubuntu 18.04 .deb-Pakete](https://packages.microsoft.com/ubuntu/18.04/prod/pool/main/m/msodbcsql17/)
- [Ubuntu 16.04 .deb-Pakete](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql17/)
- [Ubuntu 14.04 .deb-Pakete](https://packages.microsoft.com/ubuntu/14.04/prod/pool/main/m/msodbcsql17/)
- [Ubuntu 16.04 .deb-Pakete (msodbcsql 13.x)](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/)
- [Ubuntu 14.04 .deb-Pakete (msodbcsql 13.x)](https://packages.microsoft.com/ubuntu/14.04/prod/pool/main/m/msodbcsql/)

Weitere Informationen finden Sie unter [Installieren von Microsoft ODBC Driver for SQL Server unter Linux und macOS](linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

### <a name="macos"></a>macOS

- Weitere Informationen finden Sie unter [Homebrew-Formel](https://github.com/Microsoft/homebrew-mssql-release).

Weitere Informationen finden Sie unter [Installieren von Microsoft ODBC Driver for SQL Server](linux-mac/install-microsoft-odbc-driver-sql-server-macos.md).

### <a name="older-linux-releases"></a>Ältere Linux-Releases

- **Red Hat Enterprise Linux 5 und 6 (64-Bit)**  - [Download: Microsoft ODBC Driver 11 for SQL Server – Red Hat Linux](https://go.microsoft.com/fwlink/?LinkId=267321)  
- **SUSE Linux Enterprise 11 Service Pack 2 (64-Bit)**  - [Download: Microsoft ODBC Driver 11 Preview for SQL Server – SUSE Linux](https://go.microsoft.com/fwlink/?LinkId=264916)

### <a name="release-notes-for-linux-and-macos"></a>Versionshinweise für Linux und macOS

Weitere Informationen zu Releases für Linux und macOS finden Sie unter [Versionshinweise zu Microsoft ODBC Driver for SQL Server für Linux und macOS](linux-mac\release-notes-odbc-sql-server-linux-mac.md).
