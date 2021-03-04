---
title: Versionshinweise zum OLE DB-Treiber
description: In diesem Artikel mit Versionshinweisen werden die Änderungen in jedem Release des Microsoft OLE DB-Treibers für SQL Server beschrieben.
ms.date: 12/01/2020
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: genemi
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dfc66e7c1f1aa787ccc366c7dc50613741724a1f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837441"
---
# <a name="release-notes-for-the-microsoft-ole-db-driver-for-sql-server"></a>Anmerkungen zu dieser Version vom OLE DB-Treiber für SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Auf dieser Seite wird erläutert, was in den einzelnen Versionen des Microsoft OLE DB-Treibers für SQL Server hinzugefügt wurde.

<!--
USE THE TABLE FORMAT!
Hello, from now on, please use the table-based format standard for all new Release Notes content.
See section "## 18.2.1" for a live example in this article.
Thank you. For questions, contact GeneMi. (2019/03/16)
-->

## <a name="1850"></a>18.5.0
![Download](../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2135577)  
![Download](../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2135722)  

Veröffentlichung: 1. Dezember 2020

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
    Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2135577&clcid=0x40a)  
    Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2135722&clcid=0x40a)  

### <a name="features-added"></a>Neue Features:

| Neues Feature | Details |
| :------------ | :------ |
| Unterstützung für die [SQL-Datenermittlung und -klassifizierung](../../relational-databases/security/sql-data-discovery-and-classification.md) | [Verwenden der Datenklassifizierung](features/using-data-classification.md) |
| Unterstützung der Azure Active Directory-Authentifizierung mit einem Dienstprinzipal (`ActiveDirectoryServicePrincipal`) | [Verwendung von Azure Active Directory](features/using-azure-active-directory.md) |

### <a name="bugs-fixed"></a>Behobene Probleme

| Behobene Fehler | Details |
| :-------- | :------ |
| Problem mit eingebetteten NUL-Zeichen behoben. | Problem behoben, bei dem der Treiber eine falsche Länge von Zeichenfolgen mit eingebetteten NUL-Zeichen zurückgegeben hat. |
| Problem des Arbeitsspeicherverlusts bei der [IBCPSession](ole-db-interfaces/ibcpsession-ole-db.md)-Schnittstelle behoben. | Problem des Arbeitsspeicherverlusts bei der [IBCPSession](ole-db-interfaces/ibcpsession-ole-db.md)-Schnittstelle behoben, wenn Massenkopiervorgänge für den `sql_variant`-Datentyp ausgeführt wurden. |
| Probleme behoben, aufgrund derer falsche Werte für die Eigenschaften `SSPROP_INTEGRATEDAUTHENTICATIONMETHOD` und `SSPROP_MUTUALLYAUTHENTICATED` zurückgegeben wurden. | Bei früheren Versionen des Treibers wurden abgeschnittene Werte der Eigenschaft `SSPROP_INTEGRATEDAUTHENTICATIONMETHOD` zurückgegeben. Im Fall der `ActiveDirectoryIntegrated`-Authentifizierung lautete der zurückgegebene Wert der `SSPROP_MUTUALLYAUTHENTICATED`-Eigenschaft außerdem selbst dann `VARIANT_FALSE`, wenn für beide Seiten eine gegenseitige Authentifizierung erfolgte.|
| Problem bei einem Einfügevorgang für eine Verbindungsserver-Remotetabelle behoben. | Problem behoben, aufgrund dessen bei einem Einfügevorgang für eine Verbindungsserver-Remotetabelle ein Fehler aufgetreten ist, wenn die [NOCOUNT-Serverkonfigurationsoption](../../database-engine/configure-windows/configure-the-user-options-server-configuration-option.md) aktiviert wurde. |

## <a name="previous-releases"></a>Vorgängerversionen

## <a name="1840"></a>18.4.0
![Download](../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2129954)  
![Download](../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2131003)  

Veröffentlichung: Mai 2020

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2129954&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2131003&clcid=0x40a)  

### <a name="features-added"></a>Neue Features:

| Neues Feature | Details |
| :------------ | :------ |
| Unterstützung für transparente Netzwerk-IP-Auflösung (TNIR) |[Transparente Netzwerk-IP-Auflösung (TNIR)](features/using-transparent-network-ip-resolution.md)|
| Unterstützung für UTF-8-Clientcodierung | [UTF-8-Unterstützung im OLE DB-Treiber für SQL Server](features/utf-8-support-in-oledb-driver-for-sql-server.md) |

### <a name="bugs-fixed"></a>Behobene Probleme

| Behobene Fehler | Details |
| :-------- | :------ |
| Verschiedene Fehler der Schnittstelle [ISequentialStream](/previous-versions/windows/desktop/ms718035(v=vs.85)) wurden behoben. | Einige Fehler, die sich auf Multibytecodepages auswirken, ergaben, dass die Schnittstelle das Ende des Streams während des Lesevorgangs vorzeitig gemeldet hat.|
| Ein Arbeitsspeicherverlust der Schnittstelle [IopenRowset::OpenRowset](/previous-versions/windows/desktop/ms716724(v=vs.85)) wurde behoben. | Ein Arbeitsspeicherverlust der Schnittstelle [IopenRowset::OpenRowset](/previous-versions/windows/desktop/ms716724(v=vs.85)) wurde behoben, als die Eigenschaft `SSPROP_IRowsetFastLoad` aktiviert war. |
| Ein Fehler in Szenarios mit einem Datentyp `sql_variant` und ASCII-Zeichenfolgen wurde behoben. | Das Ausführen bestimmter Szenarios mit einem Datentyp `sql_variant` und ASCII-Zeichenfolgen kann zu einer Datenbeschädigung führen. Einzelheiten dazu finden Sie unter: [Bekannte Probleme](ole-db-data-types/ssvariant-structure.md#known-issues). |
| Probleme mit der Schaltfläche *Verbindung testen* im Dialogfeld [UDL-Konfiguration](help-topics/data-link-pages.md) behoben. | Die Schaltfläche *Verbindung testen* im Dialogfeld [UDL-Konfiguration](help-topics/data-link-pages.md) berücksichtigt nun auf der Registerkarte *Alle* festgelegte Initialisierungseigenschaften. |
| Die Standardwertbehandlung der Eigenschaft `SSPROP_INIT_PACKETSIZE` wurde korrigiert. | Ein unerwarteter Fehler beim Festlegen der Eigenschaft `SSPROP_INIT_PACKETSIZE` auf den Standardwert `0` wurde behoben. Weitere Informationen zu dieser Eigenschaft finden Sie unter [Initialisierungs- und Autorisierungseigenschaften](ole-db-data-source-objects/initialization-and-authorization-properties.md). |
| Probleme mit dem Pufferüberlauf in [IBCPSession](ole-db-interfaces/ibcpsession-ole-db.md) behoben. | Probleme mit dem Pufferüberlauf beim Verwenden von fehlerhaften Datendateien wurden behoben. |
| Probleme bei der Barrierefreiheit behoben. | Probleme bei der Barrierefreiheit auf der Benutzeroberfläche des Installationsprogramms und im [Anmeldedialogfeld von SQL Server](help-topics/sql-server-login-dialog.md) (Lesen von Inhalten, Tabstopps) wurden behoben. |

## <a name="1830"></a>18.3.0

![Download](../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2117515)  
![Download](../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2117517)  

Veröffentlichung: Oktober 2019

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2117515&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2117517&clcid=0x40a)  

### <a name="features-added"></a>Neue Features:

| Neues Feature | Details |
| :------------ | :------ |
| Unterstützung der Azure Active Directory-Authentifizierung (`ActiveDirectoryInteractive`, `ActiveDirectoryMSI`) | [Verwendung von Azure Active Directory](features/using-azure-active-directory.md) |
| Die Active Directory-Authentifizierungsbibliothek (adal.dll) wurde zum Installer hinzugefügt | Dieses Feature ist nun Teil der Installation des Basistreibers. Das OLE DB-Installationsprogramm sorgt dafür, dass ein Upgrade für bestehende Installationen der Microsoft Active Directory-Authentifizierungsbibliothek für SQL Server durchgeführt und die Bibliothek aus der Liste mit installierten Anwendungen in Windows entfernt wird. |
| &nbsp; | &nbsp; |

### <a name="bugs-fixed"></a>Behobene Probleme

| Behobene Fehler | Details |
| :-------- | :------ |
| Probleme mit der DROP INDEX-Logik in [IIndexDefinition::DropIndex](/previous-versions/windows/desktop/ms722733(v=vs.85)) wurden behoben. | Frühere Versionen des OLE DB-Treibers können keinen Primärschlüsselindex ablegen, wenn die Schema- und die Benutzer-ID des Indexbesitzers nicht gleich sind. |
| &nbsp; | &nbsp; |

Laden Sie frühere Versionen des OLE DB Treibers herunter, indem Sie auf die Downloadlinks in den folgenden Abschnitten klicken:

## <a name="1823"></a>18.2.3.0

![Download](../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2119554)  
![Download](../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2119738)  

Veröffentlichung: Juni 2019

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2119554&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2119738&clcid=0x40a)  

### <a name="features-added-in-1823"></a>In 18.2.3 hinzugefügte Features

| Neues Feature | Details |
| :------------ | :------ |
| Unterstützung von Treiberupgrades über SQL Server-Wechselmedien | Durch diese Verbesserungen können Treiberupgrades direkt über SQL Server-Wechselmedien durchgeführt werden. |
| &nbsp; | &nbsp; |

## <a name="1822"></a>18.2.2

![Download](../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2118512)  
![Download](../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2118415)  

Veröffentlichung: Mai 2019

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2118512&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2118415&clcid=0x40a)  

### <a name="bugs-fixed-in-1822"></a>In 18.2.2 behobene Fehler

| Behobene Fehler | Details |
| :-------- | :------ |
| Die nicht interaktive Azure Active Directory-Authentifizierung im Multithread-Apartment-Modus (MTA) wurde behoben. | Der OLE DB-Treiber Version 18.2.1 versucht fälschlicherweise das COM-Parallelitätsmodell für ein Apartment zu ändern, das zuvor als Multithread-Apartment (MTA) initialisiert wurde. Daher kann der Treiber in einer Anwendung, die vor dem Aufruf der Schnittstelle [IDBInitialize::Initialize](/previous-versions/windows/desktop/ms718026(v=vs.85)) mehr als einen nachfolgenden Aufruf von [CoInitialize](/windows/win32/api/objbase/nf-objbase-coinitialize) oder [CoInitializeEx](/windows/win32/api/combaseapi/nf-combaseapi-coinitializeex) durchführt, keine Verbindung herstellen, wenn ein Azure Active Directory-Authentifizierungsmodus verwendet wird. |
| &nbsp; | &nbsp; |

## <a name="1821"></a>18.2.1

![Download](../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2118511)  
![Download](../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2118278)  

Veröffentlichung: Februar 2019

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2118511&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2118278&clcid=0x40a)  

### <a name="features-added-in-1821"></a>In 18.2.1 hinzugefügte Features

| Neues Feature | Details |
| :------------ | :------ |
| Unterstützung der UTF-8-Servercodierung | [UTF-8-Unterstützung im OLE DB-Treiber für SQL Server](features/utf-8-support-in-oledb-driver-for-sql-server.md) |
| Unterstützung der Azure Active Directory-Authentifizierung | [Verwendung von Azure Active Directory](features/using-azure-active-directory.md) |
| &nbsp; | &nbsp; |

## <a name="1810"></a>18.1.0

![Download](../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2118506)  
![Download](../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2118509)  

Veröffentlichung: Juli 2018

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2118506&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2118509&2118509=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2118509&clcid=0x40a)  

### <a name="features-added-in-1810"></a>In 18.1.0 hinzugefügte Features

| Neues Feature | Details |
| :------------ | :------ |
| Unterstützung für das Schlüsselwort der `UseFMTONLY`-Verbindungszeichenfolge sowie für die Initialisierungseigenschaft `SSPROP_INIT_USEFMTONLY` | `UseFMTONLY` steuert, wie Metadaten abgerufen wird, wenn eine Verbindung mit [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher hergestellt wird.<br/><br/>Weitere Informationen finden Sie unter [Verwenden von Schlüsselwörtern für Verbindungszeichenfolgen mit dem OLE DB-Treiber für SQL Server.](applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md) |
| &nbsp; | &nbsp; |

### <a name="bugs-fixed-in-1810"></a>In 18.1.0 behobene Fehler

| Behobene Fehler | Details |
| :-------- | :------ |
| Die falsche Version der BCP-Formatdatei wurde korrigiert. | Der OLE DB-Treiber 18.0 legt fälschlicherweise die Version der BCP-Formatdatei auf 18.0 statt auf 11.0 fest.<br/>Die Formatdateien, die vom OLE DB-Treiber 18.0 generiert wurden, können nicht vom OLE DB-Treiber 18.1 gelesen werden.<br/>Wenn Sie mit dem neuen Treiber Formatdateien verwenden müssen, die von der vorherigen Treiberversion generiert wurden, können Sie diese Dateien manuell bearbeiten, um die Version in 11.0 zu ändern. |
| &nbsp; | &nbsp; |

## <a name="1802"></a>18.0.2

![Download](../../ssms/media/download-icon.png) [Herunterladen des x64-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2118504)  
![Download](../../ssms/media/download-icon.png) [Herunterladen des x86-Installationsprogramms](https://go.microsoft.com/fwlink/?linkid=2118277)  

Veröffentlichung: März 2018

Wenn Sie das Installationsprogramm in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Für den x64-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2118504&clcid=0x40a)  
Für den x86-Treiber: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2118277&clcid=0x40a)  

### <a name="features-added-in-1802"></a>In 18.0.2 hinzugefügte Features

| Neues Feature | Details |
| :------------ | :------ |
| Unterstützung für das Schlüsselwort der `MultiSubnetFailover`-Verbindungszeichenfolge sowie für die Initialisierungseigenschaft `SSPROP_INIT_MULTISUBNETFAILOVER`. | Weitere Informationen finden Sie unter<br/>&bull; &nbsp; [OLE DB-Treiber für SQL Server-Unterstützung für Hochverfügbarkeit und Notfallwiederherstellung](features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md) und<br/>&bull; &nbsp; [Verwenden von Schlüsselwörtern für Verbindungszeichenfolgen mit dem OLE DB-Treiber für SQL Server](applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

## <a name="see-also"></a>Weitere Informationen

[Microsoft OLE DB-Treiber für SQL Server](oledb-driver-for-sql-server.md)
