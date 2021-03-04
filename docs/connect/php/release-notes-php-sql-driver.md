---
title: Anmerkungen zu dieser Version für die Microsoft-Treiber für PHP
description: Auf dieser Seite wird erläutert, was in den einzelnen Versionen des Microsoft-Treibers für PHP für SQL Server geändert wurde.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- what's new in version 1.1
ms.assetid: 91cca3d2-ba99-4a6d-b0de-beb9699cb3f8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0df2acfa00507f42d3ee03a0a65ff2ae25aafbd1
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837184"
---
# <a name="release-notes-for-the-microsoft-drivers-for-php-for-sql-server"></a>Versionshinweise für die Microsoft-Treiber für PHP für SQL Server

Auf dieser Seite wird erläutert, welche Funktionen in den verschiedenen Versionen der [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] hinzugefügt wurden.  

<!--
Hello, We are standardizing the format of content inside our Release Notes (or What's New) articles.
Instead of bullets (or paragraphs), we have shifted to the 2-column format you see for H2 **What's New in Version 5.6**.
It is not necessary to reformat all the older H2 sections in this Release Notes file, but.....

Going forward, please be sure to use the 2-column format.

Also, all Release Notes .md file names now must begin with 'release-notes-*.md'.  And no filler words.
The 5.6 edition of this file is being renamed.....
FROM:  'release-notes-for-the-php-sql-driver.md'
TO  :  'release-notes-php-sql-driver.md'

For any questions, ask GeneMi or CraigG.
Thanks a lot.  2019-03-28  (DevO= 1467988)
-->

## <a name="59"></a>5.9

![Download](../../ssms/media/download-icon.png) [Windows-Paket herunterladen](https://go.microsoft.com/fwlink/?linkid=?????)
[GitHub-Releasetag (Linux- und macOS-Pakete hier verfügbar)](https://github.com/Microsoft/msphpsql/releases/tag/v5.9.0)

### <a name="version-information"></a>Versionsinformationen

- Releasenummer: 5.9.0<sup>1</sup>
- Veröffentlichung: 29. Januar 2021

### <a name="whats-new-in-59"></a>Neuerungen in 5.9

| Neues Element | Details |
| :------- | :------ |
| Unterstützung für PHP 8.0 hinzugefügt | &nbsp; |
| Unterstützung für PHP 7.2 eingestellt | &nbsp; |
| Unterstützung für den Microsoft ODBC-Treiber 17.7 auf allen Plattformen hinzugefügt | &nbsp; |
| Unterstützung für macOS Big Sur, Ubuntu 20.04, Ubuntu 20.10 und Alpine 3.12 hinzugefügt | Einige erfordern den ODBC-Treiber 17.5 oder höher |
| Unterstützung für macOS High Sierra, Debian 8 und Ubuntu 19.10 eingestellt | &nbsp; |
| Unterstützung für das Gebietsschema GB18030 | &nbsp; |
| PDO-Fehlerinformationen um zusätzliche ODBC-Meldungen erweitert, falls verfügbar | &nbsp; |
| Unterstützung der Datenklassifizierung durch Ranginformationen | Für diese Funktion ist SQL Server 2019 und der ODBC-Treiber 17.4.2 oder höher erforderlich. |
| Unterstützung der Azure Active Directory-Authentifizierung mit einem Dienstprinzipal hinzugefügt | Erfordert ODBC-Treiber 17.7 oder höher |
| Leistung bei der Verarbeitung von Dezimalzahlen als Ein- oder Ausgaben verbessert und unnötige Konvertierungen von numerischen Werten entfernt | &nbsp; |
| Verbesserte Leistung beim Abrufen von Zahlen mithilfe von Clientpuffern | &nbsp; |
| Festlegen des Abfragetimeouts ohne LOCK TIMEOUT, wodurch ein zusätzlicher Trip zum Server gespart wird | &nbsp; |
| &nbsp; | &nbsp; |

<sup>1</sup> Dieses Release erfordert den ODBC-Treiber 17.4.2 oder höher. Andernfalls wird eine Warnung mit dem Hinweis angezeigt, dass ein Attribut nicht festgelegt werden kann. Diese Warnung kann unterdrückt werden, wenn ein älterer ODBC-Treiber verwendet wird. Wenn Sie SQLSRV verwenden, lesen Sie [Vorgehensweise: Konfigurieren der Behandlung von Fehlern und Warnungen unter Verwendung des SQLSRV-Treibers](./how-to-configure-error-and-warning-handling-using-the-sqlsrv-driver.md). Wenn Sie PDO_SQLSRV verwenden, werden Warnungen standardmäßig unterdrückt. Sie können jedoch protokolliert werden. Weitere Informationen finden Sie unter [Protokollieraktivität](./logging-activity.md).

## <a name="581"></a>5.8.1

Diese Version gilt nur für Linux und macOS.

[GitHub-Releasetag (hier sind Pakete für Linux und macOS verfügbar)](https://github.com/Microsoft/msphpsql/releases/tag/v5.8.1)

### <a name="version-information"></a>Versionsinformationen

- Releasenummer: 5.8.1
- Veröffentlichung: 15. April 2020

### <a name="whats-new-in-581"></a>Neuerungen in 5.8.1

| Neues Element | Details |
| :------- | :------ |
| Fehlerbehebung | Die Standardgebietsschemaprobleme in Alpine Linux wurden korrigiert. |
| Fehlerbehebung | Unnötige Datenstruktur zur Unterstützung des Features der clientseitigen Cursor in Alpine Linux wurde entfernt. |
| Fehlerbehebung | Es wurden Protokollierungsprobleme behoben, die auftraten, wenn beide Treiber in Alpine Linux aktiviert waren. |
| &nbsp; | &nbsp; |

## <a name="58"></a>5.8

![Download](../../ssms/media/download-icon.png) [Windows-Paket herunterladen](https://go.microsoft.com/fwlink/?linkid=2120362)  
[GitHub-Releasetag (hier sind Pakete für Linux und macOS verfügbar)](https://github.com/Microsoft/msphpsql/releases/tag/v5.8.0)

### <a name="version-information"></a>Versionsinformationen

- Releasenummer: 5.8.0
- Veröffentlichung: 31. Januar 2020

### <a name="whats-new-in-58"></a>Neuerungen in 5.8

| Neues Element | Details |
| :------- | :------ |
| Unterstützung für PHP 7.4 hinzugefügt. | &nbsp; |
| Unterstützung für PHP 7.1 eingestellt. | &nbsp; |
| Unterstützung für Microsoft ODBC-Treiber 17.5 auf allen Plattformen hinzugefügt. | &nbsp; |
| Unterstützung für Debian 10 und Red Hat 8 hinzugefügt. | Für beide Systeme ist der ODBC-Treiber 17.4 oder höher erforderlich. |
| Unterstützung für macOS Catalina, Alpine Linux 3.11<sup>1</sup> und Ubuntu 19.10 hinzugefügt. | Für alle Systeme ist der ODBC-Treiber 17.5 oder höher erforderlich. |
| Unterstützung für SQL Server 2008 R2, macOS Sierra, Ubuntu 18.10 und Ubuntu 19.04 wurde eingestellt. | &nbsp; |
| Unterstützung für die Sprachoption bei Verbindung mit SQL Server. | &nbsp; |
| Unterstützung für erweiterte PHP-Zeichenfolgentypen, die in PHP 7.2 eingeführt wurden. | &nbsp; |
| Unterstützung für den Abruf von Sensitivitätsmetadaten der Datenklassifizierung. | Für diese Funktion ist SQL Server 2019 und der ODBC-Treiber 17.4.2 oder höher erforderlich. |
| Unterstützung für Always Encrypted mit Secure Enclaves | Für diese Funktion ist der ODBC-Treiber 17.4 oder höher erforderlich. |
| Unterstützung von konfigurierbaren Optionen für Gebietsschemaeinstellungen in Linux und macOS. |
| Verbesserte Leistung durch das Zwischenspeichern von Metadaten beim Abruf und den Wegfall redundanter Aufrufe. | &nbsp; |
| &nbsp; | &nbsp; |

<sup>1</sup> Experimentelle Unterstützung von Alpine Linux in Version 5.8.

## <a name="previous-releases"></a>Vorgängerversionen

## <a name="561"></a>5.6.1

![Download](../../ssms/media/download-icon.png) [Windows-Paket herunterladen](https://go.microsoft.com/fwlink/?linkid=2120446)  
[GitHub-Releasetag (hier sind Pakete für Linux und macOS verfügbar)](https://github.com/Microsoft/msphpsql/releases/tag/v5.6.1)

### <a name="version-information"></a>Versionsinformationen

- Releasenummer: 5.6.1
- Veröffentlichung: 19. März 2019

### <a name="whats-new-in-561"></a>Neuerungen in 5.6.1

| Neues Element | Details |
| :------- | :------ |
| Fehlerbehebung | Annahmen beim Berechnen von Feld- oder Spaltenmetadaten wurden korrigiert, die möglicherweise zur Beendigung der Anwendung geführt haben. |
| Fehlerbehebung | Die SQL Server-Konfigurationsdatei wurde so geändert, dass sie unabhängig von „pdo_sqlsrv“ kompiliert werden kann. |
| Fehlerbehebung | „PDOStatement::getColumnMeta()“ wurde korrigiert, sodass bei einem Fehler „false“ zurückgegeben wird. |
| &nbsp; | &nbsp; |

## <a name="56"></a>5.6

![Download](../../ssms/media/download-icon.png) [Windows-Paket herunterladen](https://go.microsoft.com/fwlink/?linkid=2120450)  
[GitHub-Releasetag (hier sind Pakete für Linux und macOS verfügbar)](https://github.com/Microsoft/msphpsql/releases/tag/v5.6.0)

### <a name="version-information"></a>Versionsinformationen

- Releasenummer: 5.6.0
- Veröffentlichung: 21. Februar 2019

### <a name="whats-new-in-56"></a>Neuerungen in 5.6

| Neues Element | Details |
| :------- | :------ |
| Unterstützung für PHP 7.3. | &nbsp; |
| Unterstützung für PHP 7.0 eingestellt. | &nbsp; |
| Unterstützung für Microsoft ODBC-Treiber 17.3 auf allen Plattformen. | &nbsp; |
| Unterstützung für macOS Mojave. | Für diese Funktion ist der ODBC-Treiber 17.3 oder höher erforderlich. |
| Unterstützung für Ubuntu 18.10 und Suse Linux 15. | Für beide Systeme ist der ODBC-Treiber 17.3 oder höher erforderlich. |
| Die Unterstützung für Linux Ubuntu 17.10 und macOS El Capitan wurde eingestellt. | &nbsp; |
| Unterstützung für Azure AD-Zugriffstoken. | Unter Linux und macOS ist der ODBC-Treiber 17.2 oder höher sowie unixODBC 2.3.6 oder höher erforderlich. |
| Unterstützung für die Authentifizierung mit Azure AD unter Verwendung verwalteter Identitäten für Azure-Ressourcen. | Für diese Funktion ist der ODBC-Treiber 17.3 oder höher erforderlich. |
| Neue Abruffunktionen | &bull; &nbsp; Neues Kennzeichen PDO::SQLSRV_ATTR_FETCHES_DATETIME_TYPE, damit pdo_sqlsrv datetime als Objekt zurückgibt.<br/><br/>&bull; &nbsp; Neue ReturnDatesAsStrings-Option auf Anweisungsebene für sqlsrv.<br/><br/>&bull; &nbsp; Neue Optionen auf Verbindungs- und Anweisungsebene für beide Treiber, um Dezimalwerte in den abgerufenen Ergebnissen zu formatieren. |
| Unterstützung für die statische Kompilierung von Treibern, wenn Benutzer die Erstellung aus der Quelle auswählen. | &nbsp; |
| Verbesserte Leistung durch das Zwischenspeichern von Metadaten beim Abruf und eine schnellere Konvertierung von Unicode-Zeichenfolgen. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="53"></a>5.3

![Download](../../ssms/media/download-icon.png) [Windows-Paket herunterladen](https://go.microsoft.com/fwlink/?linkid=2120447)  
[GitHub-Releasetag (hier sind Pakete für Linux und macOS verfügbar)](https://github.com/Microsoft/msphpsql/releases/tag/v5.3.0)

### <a name="version-information"></a>Versionsinformationen

- Releasenummer: 5.3.0
- Veröffentlichung: 20. Juli 2018

### <a name="whats-new-in-53"></a>Neuerungen in 5.3

- Unterstützung für Microsoft ODBC-Treiber 17.2 auf allen Plattformen
- Unterstützung für macOS High Sierra (erfordert ODBC-Treiber 17 und höher)
- Unterstützung von Azure Key Vault für Always Encrypted für grundlegende CRUD-Funktionalität, sodass die Always Encrypted-Funktion für alle unterstützten Windows-, Linux- oder macOS-Plattformen verfügbar ist. [Verwenden von Always Encrypted mit den PHP-Treibern für SQL Server](using-always-encrypted-php-drivers.md)
- Unterstützung für Ubuntu 18.04 LTS (ODBC-Treiber 17.2 erforderlich)
- Unterstützung für Verbindungsresilienz unter Linux oder macOS (ODBC-Treiber 17.2 erforderlich)

## <a name="52"></a>5,2

![Download](../../ssms/media/download-icon.png) [Windows-Paket herunterladen](https://go.microsoft.com/fwlink/?linkid=2120451)  
[GitHub-Releasetag (hier sind Pakete für Linux und macOS verfügbar)](https://github.com/Microsoft/msphpsql/releases/tag/v5.2.0)

### <a name="version-information"></a>Versionsinformationen

- Releasenummer: 5.2.0
- Veröffentlichung: 23. März 2018

### <a name="whats-new-in-52"></a>Neuerungen in 5.2

- Unterstützung für PHP 7.2.1 und höher unter Windows sowie Version 7.2.0 und höher auf allen anderen Plattformen
- Unterstützung für Microsoft ODBC-Treiber 17
  - Version 17 ist nun der Standard auf allen Plattformen
- Unterstützung für Ubuntu 17.10, Debian 9 und Suse Enterprise Linux 12
- Unterstützung für Ubuntu 15.10 eingestellt
- Unterstützung für Always Encrypted mit CRUD-Funktionalität unter Windows. Weitere Informationen finden Sie unter [Verwenden von Always Encrypted mit den PHP-Treibern für SQL Server](using-always-encrypted-php-drivers.md).
  - Unterstützung für den Windows-Zertifikatspeicher
  - Always Encrypted wird nur mit Microsoft ODBC-Treiber 17 und höher unterstützt
- Unterstützung für Nicht-UTF-8-Gebietsschemas unter Linux und macOS
  - Nicht-UTF-8-Gebietsschemas werden unter Linux und macOS nur mit Microsoft ODBC-Treiber 17 und höher unterstützt
- Unterstützung für Azure Synapse Analytics
- Unterstützung für verwaltete Azure SQL-Datenbank-Instanzen

## <a name="43"></a>4.3

![Download](../../ssms/media/download-icon.png) [Windows-Paket herunterladen](https://go.microsoft.com/fwlink/?linkid=2120616)  
[GitHub-Releasetag (hier sind Pakete für Linux und macOS verfügbar)](https://github.com/Microsoft/msphpsql/releases/tag/v4.3.0)

### <a name="version-information"></a>Versionsinformationen

- Releasenummer: 4.3.0
- Veröffentlichung: 6. Juli 2017

### <a name="whats-new-in-43"></a>Neuerungen in 4.3

- Unterstützung für PHP 7.1
- Unterstützung für macOS Sierra und macOS El Capitan
- Unterstützung für Ubuntu 15.10 und Debian 8
- Unterstützung für Ubuntu 15.04 eingestellt
- Unterstützung für Always On-Verfügbarkeitsgruppen über die transparente Netzwerk-IP-Adressauflösung. Weitere Informationen finden Sie unter [Connection Options](connection-options.md).
- Eingeschränkte Unterstützung für den sql_variant-Datentyp hinzugefügt.
- Unterstützung für Resilienz von Verbindungen im Leerlauf unter Windows. Weitere Informationen finden Sie unter [Connection Options](connection-options.md).
- Unterstützung für Verbindungspooling unter Linux und macOS. Weitere Informationen finden Sie unter [Verbindungspooling](connection-pooling-microsoft-drivers-for-php-for-sql-server.md).
- Unterstützung für Azure Active Directory-Authentifizierung mit ActiveDirectoryPassword und SqlPassword. Weitere Informationen finden Sie unter [Connection Options](connection-options.md).

## <a name="40"></a>4,0

![Download](../../ssms/media/download-icon.png) [Windows-Paket herunterladen](https://go.microsoft.com/fwlink/?linkid=2120448)  
[GitHub-Releasetag](https://github.com/microsoft/msphpsql/releases/tag/v4.0-RTW)

### <a name="version-information"></a>Versionsinformationen

- Releasenummer: 4,0
- Veröffentlichung: 1. Juli 2016

### <a name="whats-new-in-40"></a>Neuerungen in 4.0

- Unterstützung für PHP 7.0  
- Vollständige 64-Bit-Unterstützung
- Unterstützung für Ubuntu 15.04, Ubuntu 16.04 und RedHat 7

## <a name="32"></a>3.2

![Download](../../ssms/media/download-icon.png) [Windows-Paket herunterladen](https://go.microsoft.com/fwlink/?linkid=2120449)  
[GitHub-Releasetag](https://github.com/microsoft/msphpsql/releases/tag/v3.2.0.0)

### <a name="version-information"></a>Versionsinformationen

- Releasenummer: 3.2
- Veröffentlichung: 9. März 2015

### <a name="whats-new-in-32"></a>Neuerungen in 3.2

- Unterstützung für PHP 5.6  
- Enthält die neuesten Updates für vorherige PHP-Versionen 5.5 und 5.4  
- Erfordert Microsoft ODBC Driver 11 for SQL Server  

## <a name="31"></a>3.1

![Download](../../ssms/media/download-icon.png) [Windows-Paket herunterladen](https://go.microsoft.com/fwlink/?linkid=2143027)  
[GitHub-Releasetag](https://github.com/microsoft/msphpsql/releases/tag/v3.1.0.0)

### <a name="version-information"></a>Versionsinformationen

- Releasenummer: 3.1
- Veröffentlichung: 12. Dezember 2014

### <a name="whats-new-in-31"></a>Neuerungen in 3.1

- Unterstützung für PHP 5.5  
- Erfordert Microsoft ODBC Driver 11 for SQL Server Frühere Versionen benötigten SQL Native Client.  

## <a name="30"></a>3.0

![Download](../../ssms/media/download-icon.png) [Windows-Paket herunterladen](https://go.microsoft.com/fwlink/?linkid=2143026)  

### <a name="whats-new-in-30"></a>Neuerungen in 3.0  

- Unterstützung für PHP 5.4  PHP 5.2 wird in Version 3 von [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]nicht unterstützt.  
- Verbindungsoption AttachDBFileName wurde hinzugefügt. Weitere Informationen finden Sie unter [Connection Options](connection-options.md).  
- Unterstützung für LocalDB, (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]). Weitere Informationen finden Sie unter [Unterstützung für LocalDB](php-driver-for-sql-server-support-for-localdb.md).
- Verbindungsoption AttachDBFileName wurde hinzugefügt. Weitere Informationen finden Sie unter [Connection Options](connection-options.md).  
- Unterstützung für Hochverfügbarkeit und Notfallwiederherstellung Weitere Informationen finden Sie unter [Unterstützung für hohe Verfügbarkeit, Notfallwiederherstellung](php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).
- Unterstützung für clientseitige Cursor (Zwischenspeichern eines Resultsets im Arbeitsspeicher). Weitere Informationen finden Sie unter [Cursortypen &#40;SQLSRV-Treiber&#41;](cursor-types-sqlsrv-driver.md) und [Cursortypen &#40;PDO_SQLSRV-Treiber&#41;](cursor-types-pdo-sqlsrv-driver.md).
- Das Attribut PDO::ATTR_EMULATE_PREPARES- wurde hinzugefügt. Weitere Informationen finden Sie unter [PDO::prepare](pdo-prepare.md).  

## <a name="20"></a>2.0

### <a name="whats-new-in-20"></a>Neuerungen in 2.0

In Version 2.0 wurde Unterstützung für den PDO_SQLSRV-Treiber hinzugefügt. Weitere Informationen finden Sie in der [PDO_SQLSRV-Treiberreferenz](pdo-sqlsrv-driver-reference.md).  

## <a name="see-also"></a>Weitere Informationen

[Overview of the Microsoft Drivers for PHP for SQL Server (Übersicht über die Microsoft-Treiber für PHP für SQL Server)](overview-of-the-php-sql-driver.md)