---
title: Versionshinweise für Microsoft ODBC Driver for SQL Server für Windows
description: In diesem Artikel mit Versionshinweisen werden die Änderungen in jedem Release von Microsoft ODBC Driver for SQL Server unter Windows beschrieben.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: b8459ed8-625e-4d8b-891c-e7e78c9977cc
ms.reviewer: v-daenge
author: v-chojas
ms.author: v-chojas
manager: kenvh
ms.openlocfilehash: 2c7ea508a6f0f79a000de1ae42f5e7eb3de09f5a
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464768"
---
# <a name="release-notes-for-microsoft-odbc-driver-for-sql-server-on-windows"></a>Versionshinweise für Microsoft ODBC Driver for SQL Server für Windows

In diesem Artikel mit Versionshinweisen werden die Neuerungen für Microsoft ODBC Driver for SQL Server unter Windows beschrieben.

<!--
PLEASE USE THE STANDARD 2-COLUMN TABLE FORMAT!

For all our Release Notes articles (What's New too?), we are standardizing on the 2-column format that you see here for version "## 17.3".

Going forward, all new additions to this article must use the 2-column format.

Also, use the shorter ## H2 title format, which eliminates all the redundant constants, and appends the date-added.
One beneift of shortness is the avoidance of the annoying wrapping of unnecessarily long H2 titles in the rightNav.
- OLD H2:  ## What's New in the [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 17.3 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] on Windows
- NEW H2:  ## 17.3, February 2019

By the way, in GitHub, the file name is changing today 2019/03/30:
- FROM:  docs/connect/odbc/windows/release-notes.md
- TO  :  docs/connect/odbc/windows/release-notes-odbc-sql-server-windows.md

Thank you.
GeneMi (and CraigG).  2019/03/30.
-->

## <a name="177"></a>17,7

![Download](../../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2153471)  
![Download](../../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2153469)  

Versionsnummer: 17.7.1.1  
Veröffentlichung: 29. Januar 2021

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x40a)

| Neues Feature | Details |
| :------- | :------ |
| Interaktive Azure Key Vault-Authentifizierung | Weitere Informationen finden Sie unter [Verwenden von Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md). |
| Dienstprinzipalauthentifizierung | Siehe [Schlüsselwörter und Attribute von DSN- und Verbindungszeichenfolgen](../dsn-connection-string-attribute.md). |
| Einfügen in verschlüsselte money- und smallmoney-Spalten | Weitere Informationen finden Sie unter [Verwenden von Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md). |
| Fehlerbehebungen. | Siehe [Fehlerbehebungen](../bug-fixes.md) |
| &nbsp; | &nbsp; |

## <a name="previous-releases"></a>Vorgängerversionen

## <a name="176"></a>17.6

![Download](../../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2137027)  
![Download](../../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2137028)  

Versionsnummer: 17.6.1.1  
Veröffentlichung: 31. Juli 2020

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2137027&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2137028&clcid=0x40a)

| Neues Feature | Details |
| :------- | :------ |
| Zwischenspeichern von Metadaten für vorbereitete Anweisungen | Weitere Informationen finden Sie unter [Verwenden von Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md). |
| SQL_COPT_SS_AUTOBEGINTXN-Verbindungsattribut, das steuert, ob die automatische BEGIN TRANSACTION-Anweisung nach ROLLBACK oder COMMIT auftritt | Siehe [Schlüsselwörter und Attribute von DSN- und Verbindungszeichenfolgen](../dsn-connection-string-attribute.md). |
| Fehlerbehebungen. | Siehe [Fehlerbehebungen](../bug-fixes.md) |
| &nbsp; | &nbsp; |

## <a name="1752"></a>17.5.2

![Download](../../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120137)  
![Download](../../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120140)  

Versionsnummer: 17.5.2.1  
Veröffentlichung: 6. März 2020

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120137&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120140&clcid=0x40a)  

### <a name="features-added-in-1752"></a>In 17.5.2 hinzugefügte Features

| Neues Feature | Details |
| :------------ | :------ |
| Unterstützung für die Authentifizierung mit einer verwalteten Identität bei Azure Key Vault | Siehe [Verwenden von Always Encrypted mit dem ODBC-Treiber](../using-always-encrypted-with-the-odbc-driver.md). |
| Unterstützung für zusätzliche Azure Key Vault-Endpunkte | Siehe [Verwenden von Always Encrypted mit dem ODBC-Treiber](../using-always-encrypted-with-the-odbc-driver.md). |
| Fehlerbehebungen. | Siehe [Fehlerbehebungen](../bug-fixes.md) |
| &nbsp; | &nbsp; |

Laden Sie frühere Versionen des ODBC-Treibers herunter, indem Sie auf die Downloadlinks in den folgenden Abschnitten klicken:

## <a name="175"></a>17,5

![Download](../../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120248)  
![Download](../../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120353)  

Versionsnummer: 17.5.1.1  
Veröffentlichung: 31. Januar 2020

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120248&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120353&clcid=0x40a)  

### <a name="features-added-in-175"></a>In 17.5 hinzugefügte Features

| Neues Feature | Details |
| :------------ | :------ |
| SQL_COPT_SS_SPID-Verbindungsattribut zum Abrufen der SPID ohne Roundtrip zum Server | Siehe [Schlüsselwörter und Attribute von DSN- und Verbindungszeichenfolgen](../dsn-connection-string-attribute.md). |
| Fehlerbehebungen. | Siehe [Fehlerbehebungen](../bug-fixes.md) |
| &nbsp; | &nbsp; |

## <a name="1742"></a>17.4.2

![Download](../../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120354)  
![Download](../../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120249)  

Versionsnummer: 17.4.2.1  
Veröffentlichung: Oktober 2019

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120354&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120249&clcid=0x40a)  

### <a name="features-added-in-1742"></a>In 17.4.2 hinzugefügte Features

| Neues Feature | Details |
| :------------ | :------ |
| Unterstützung für zusätzliche Azure Key Vault-Endpunkte | Siehe [Verwenden von Always Encrypted mit dem ODBC-Treiber](../using-always-encrypted-with-the-odbc-driver.md). |
| Unterstützung für das Festlegen der Datenklassifizierungsversion | Siehe [Data Classification (Datenklassifizierung)](../data-classification.md#bkmk-version) |
| Die Active Directory-Authentifizierungsbibliothek (adal.dll) wurde zum Installer hinzugefügt | Dieses Feature ist nun Teil der Installation des Basistreibers. Das ODBC-Installationsprogramm sorgt dafür, dass ein Upgrade für bestehende Installationen der Microsoft Active Directory-Authentifizierungsbibliothek für SQL Server durchgeführt und die Bibliothek aus der Liste mit installierten Anwendungen in Windows entfernt wird. |
| Fehlerbehebungen. | Siehe [Fehlerbehebungen](../bug-fixes.md) |
| &nbsp; | &nbsp; |

## <a name="174"></a>17.4

![Download](../../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120149)  
![Download](../../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120150)  

Versionsnummer: 17.4.1.1  
Veröffentlichung: Juli 2019

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120149&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120150&clcid=0x40a)  

### <a name="features-added-in-174"></a>In 17.4 hinzugefügte Features

| Neues Feature | Details |
| :------------ | :------ |
| Always Encrypted mit Secure Enclaves | Siehe [Verwenden von Always Encrypted mit dem ODBC-Treiber](../using-always-encrypted-with-the-odbc-driver.md). |
| Konfigurierbare TCP-Keep-Alive-Einstellungen. | Siehe [Herstellen einer Verbindung mit SQL Server](../linux-mac/connection-string-keywords-and-data-source-names-dsns.md). |
| Fehlerbehebungen. | Siehe [Fehlerbehebungen](../bug-fixes.md) |
| &nbsp; | &nbsp; |

## <a name="173"></a>17.3

![Download](../../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120355)  
![Download](../../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120356)  

Versionsnummer: 17.3.1.1  
Veröffentlichung: Februar 2019

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120355&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120356&clcid=0x40a)  

### <a name="features-added-in-173"></a>In 17.3 hinzugefügte Features

| Neues Feature | Details |
| :------------ | :------ |
| Modus für die Azure Active Directory-Authentifizierung mit einer (systemweit und benutzerseitig zugewiesenen) verwalteten Identität | Siehe [Using Azure Active Directory with the ODBC Driver (Verwenden von Azure Active Directory mit dem ODBC-Treiber)](../using-azure-active-directory.md) |
| Übermitteln von Eingabeparametern für Always Encrypted-Spalten | Siehe [Limitations of the ODBC driver when using Always Encrypted (Einschränkungen des ODBC-Treibers bei Verwendung von Always Encrypted)](../using-always-encrypted-with-the-odbc-driver.md#limitations-of-the-odbc-driver-when-using-always-encrypted) |
| Verteilte XA-Transaktionen | Siehe [Using XA Transactions (Verwenden von XA-Transaktionen)](../use-xa-with-dtc.md) |
| Aktualisierte Version des Visual C++ Redistributable | Die Runtimeabhängigkeit wurde auf das Visual C++ 2017 Redistributable ([x64-Download](https://aka.ms/vs/15/release/vc_redist.x64.exe), [x86-Download](https://aka.ms/vs/15/release/vc_redist.x86.exe)) aktualisiert. |
| Fehlerbehebungen. | Siehe [Fehlerbehebungen](../bug-fixes.md) |
| &nbsp; | &nbsp; |

## <a name="172"></a>17.2

![Download](../../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120250)  
![Download](../../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120357)  

Versionsnummer: 17.2.0.1  
Veröffentlichung: Juli 2018

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120250&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120357&clcid=0x40a)  

### <a name="features-added-in-172"></a>In 17.2 hinzugefügte Features

| Neues Feature | Details |
| :------------ | :------ |
| Datenklassifizierung für Azure SQL-Datenbank und SQL Server | Siehe [Data Classification (Datenklassifizierung)](../data-classification.md) |
| Unterstützung der UTF-8-Servercodierung | &nbsp; |
| Fehlerbehebungen. | Siehe [Fehlerbehebungen](../bug-fixes.md) |
| &nbsp; | &nbsp; |

## <a name="171"></a>17.1

![Download](../../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120151)  
![Download](../../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120443)  

Versionsnummer: 17.1.0.1  
Veröffentlichung: März 2018

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120151&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120443&clcid=0x40a)  

### <a name="features-added-in-171"></a>In 17.1 hinzugefügte Features

| Neues Feature | Details |
| :------------ | :------ |
| Unterstützung der Verbindungsattribute `SQL_COPT_SS_CEKCACHETTL` und `SQL_COPT_SS_TRUSTEDCMKPATHS` | &bull; &nbsp; `SQL_COPT_SS_CEKCACHETTL`<br/>Ermöglicht das Steuern und Leeren der Zeit, für die der lokale Cache von Spaltenverschlüsselungsschlüsseln vorhanden ist<br/><br/>&bull; &nbsp; `SQL_COPT_SS_TRUSTEDCMKPATHS`<br/>Ermöglicht der Anwendung das Einschränken von Always Encrypted-Vorgängen, sodass diese nur die festgelegte Liste von Spaltenhauptschlüsseln nutzen<br/><br/> Weitere Informationen finden Sie unter [Using Always Encrypted with the ODBC Driver for SQL Server (Verwenden von Always Encrypted mit dem ODBC Driver for SQL Server)](../using-always-encrypted-with-the-odbc-driver.md). |
| Unterstützung der interaktiven Azure Active Directory-Authentifizierung | &nbsp; |
| Fehlerbehebungen. | Siehe [Fehlerbehebungen](../bug-fixes.md) |
| &nbsp; | &nbsp; |

## <a name="170"></a>17.0

![Download](../../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120444)  
![Download](../../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120152)  

Versionsnummer: 17.0.1.1  
Veröffentlichung: Februar 2018

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120444&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120152&clcid=0x40a)  

### <a name="features-added-in-170"></a>In 17.0 hinzugefügte Features

| Neues Feature | Details |
| :------------ | :------ |
| Always Encrypted-Unterstützung für die BCP-API | &nbsp; |
| Neues Verbindungszeichenfolgenattribut `UseFMTOnly` | Bewirkt, dass der Treiber in speziellen Fällen ältere Metadaten verwendet, die temporäre Tabellen erfordern |
| Unterstützung verwalteter Azure SQL-Datenbank-Instanzen | Weitere Informationen finden Sie in der folgenden [Liste der Unterschiede bei der Verwendung verwalteter Instanzen (ODBC-Version 17)](#diffs-managed-instance-17) |
| &nbsp; | &nbsp; |

| Geänderte Abhängigkeit | Details |
| :------------ | :------ |
| Der Anmeldeassistent für Microsoft-Onlinedienste wurde entfernt | Die Abhängigkeit wurde entfernt |
| &nbsp; | &nbsp; |

### <a name="differences-when-using-managed-instance-odbc-version-17"></a><a name="diffs-managed-instance-17"></a> Unterschiede bei der Verwendung verwalteter Instanzen (ODBC-Version 17)

Diese Version von ODBC umfasst Unterstützung für verwaltete Azure SQL-Instanzen. In der folgenden Liste werden die Unterschiede bei der Verwendung von verwalteten Instanzen aufgeführt.

> [!NOTE]
> Bei der Verwendung verwalteter Instanzen gibt es einige Unterschiede:
>
> - FILESTREAM wird nicht unterstützt
> - Der Zugriff auf das lokale Dateisystem wird nicht unterstützt, ist jedoch für Dinge wie Ablaufverfolgungsdateien erforderlich.
> - Das Erstellen von benutzerdefinierten Typen aus lokalen Pfaden wird nicht unterstützt
> - Die integrierte Windows-Authentifizierung wird nicht unterstützt
> - DTC wird nicht unterstützt
> - Das `sa`-Konto ist nicht vorhanden (Standardkonto: `cloudSA`).
> - Fehler beim TDS-Token (0xAA) gibt einen falschen Servernamen zurück
> - Sonderzeichen werden im Datenbanknamen nicht unterstützt
> - ALTER DATABASE [dbname1] MODIFY NAME = [dbname2] wird nicht unterstützt
> - Fehlermeldungen werden unabhängig von Ihren Spracheinstellungen immer in englischer Sprache angezeigt (wie bei Azure)

## <a name="131"></a>Version 13.1

![Download](../../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2121020)  
![Download](../../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120923)  

Versionsnummer: Version 13.1  

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2121020&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120923&clcid=0x40a)  

[Microsoft Befehlszeilenprogramme 13.1 für SQL Server](https://www.microsoft.com/download/details.aspx?id=53591)

### <a name="features-added-in-131"></a>In 13.1 hinzugefügte Features

| Neues Feature | Details |
| :------------ | :------ |
| Mit dem ODBC Driver 13.1 for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] wurden [Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md) und [Azure Active Directory](../using-azure-active-directory.md) hinzugefügt | Diese hinzugefügten Unterstützungen stehen zur Verfügung, wenn eine Verbindung mit Microsoft SQL Server 2016 oder einer höheren Version hergestellt wird |
| Es wurden Schlüsselwörter und Attribute für das Verbindungspooling hinzugefügt, die den Unterstützungen für Always Encrypted und Azure Active Directory entsprechen | Diese Schlüsselwörter und Attribute werden unter [Treiberfähiges Verbindungspooling im ODBC-Treiber für SQL Server](driver-aware-connection-pooling-in-the-odbc-driver-for-sql-server.md) beschrieben. |
| &nbsp; | &nbsp; |

## <a name="13"></a>13

![Download](../../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2121118)  
![Download](../../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2120924)  

Versionsnummer: 13  

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2121118&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2120924&clcid=0x40a)  

[Download: Microsoft Befehlszeilenprogramme 13 für SQL Server](https://www.microsoft.com/download/details.aspx?id=52680)

### <a name="features-added-in-13"></a>In 13 hinzugefügte Features

| Neues Feature | Details |
| :------------ | :------ |
| Die Unterstützung von Microsoft SQL Server 2016 wurde hinzugefügt | Die Funktionalität der Version 11 des ODBC-Treibers wird beibehalten |
| &nbsp; | &nbsp; |

## <a name="11"></a>11

![Download](../../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2121206)  
![Download](../../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2121021)  

Versionsnummer: 11  

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2121206&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2121021&clcid=0x40a)  

[Download: Microsoft Befehlszeilenprogramme 11 für SQL Server](https://www.microsoft.com/download/details.aspx?id=36433)  

### <a name="features-added-in-11"></a>In 11 hinzugefügte Features

| Neues Feature | Details |
| :------------ | :------ |
| Enthält neue Features | Siehe [Features of the Microsoft ODBC Driver for SQL Server on Windows (Features des Microsoft ODBC Driver for SQL Server unter Windows)](features-of-the-microsoft-odbc-driver-for-sql-server-on-windows.md) |
| Enthält alle Features des nativen Clients für ODBC in SQL Server 2012 | &nbsp; |
| &nbsp; | &nbsp; |
