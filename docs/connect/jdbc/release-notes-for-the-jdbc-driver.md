---
title: Versionshinweise für den JDBC-Treiber
description: In diesem Artikel werden die Releases des Microsoft JDBC-Treibers für SQL Server aufgeführt. Für jede Version werden die Änderungen benannt und beschrieben.
ms.custom: ''
ms.date: 03/02/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 074f211e-984a-4b76-bb15-ee36f5946f12
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c394eaeb6ec8361dd429b944b128d19a0cf33319
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837527"
---
# <a name="release-notes-for-the-microsoft-jdbc-driver-for-sql-server"></a>Versionshinweise zum Microsoft JDBC-Treiber für SQL Server

In diesem Artikel werden die Versionen des _Microsoft JDBC-Treibers für SQL Server_ aufgeführt. Für jede Version werden die Änderungen benannt und beschrieben.

## <a name="92"></a><a id="92"></a> 9.2

**[![Herunterladen](../../ssms/media/download-icon.png) Microsoft JDBC-Treiber 9.2.1 für SQL Server (ZIP-Datei) herunterladen](https://go.microsoft.com/fwlink/?linkid=2155948)**  
**[![Herunterladen](../../ssms/media/download-icon.png) Microsoft JDBC-Treiber 9.2.1 für SQL Server (tar.gz) herunterladen](https://go.microsoft.com/fwlink/?linkid=2155949)**  

Versionsnummer: 9.2.1  
Veröffentlichung: 02.03.2021

Wenn Sie den Treiber in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Treiber in einer ZIP-Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x40a)  
Treiber in einer tar.gz-Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x40a)  

### <a name="compliance"></a>Kompatibilität

| Kompatibilitäts-/Konformitätsänderung | Details |
| :---------------- | :------ |
| Download der neuesten Updates für den JDBC-Treiber 9.2 | &bull; &nbsp; [GitHub, 9.2.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v9.2.1)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Vollständig konform mit JDBC-API-Spezifikation 4.2. | Die JAR-Dateien im 9.2-Paket sind der Java-Versionskompatibilität entsprechend benannt.<br/><br/>Beispielsweise muss mit Java 15 die Datei „mssql-jdbc-9.2.1.jre15.jar“ aus dem Paket „9.2“ verwendet werden. |
| Kompatibel mit dem Java Development Kit (JDK) der Versionen 15.0, 11.0 und 1.8 | Der Microsoft JDBC-Treiber 9.2 für SQL Server ist jetzt nicht nur mit JDK 11.0 und 1.8, sondern auch mit Version 15.0 kompatibel. |
| &nbsp; | &nbsp; |

### <a name="releases"></a>Releases

Versionsnummer: 9.2.1  
Veröffentlichung: 02.03.2021

Behobene Probleme in 9.2.1:

Problem mit einem leeren Clientgeheimnis während der ActiveDirectoryServicePrincipal-Authentifizierung in der Azure-Umgebung behoben.

Versionsnummer: 9.2.0  
Veröffentlichung: 29. Januar 2021

**[![Herunterladen](../../ssms/media/download-icon.png) Microsoft JDBC-Treiber 9.2.0 für SQL Server (ZIP-Datei) herunterladen](https://go.microsoft.com/fwlink/?linkid=2153622)**  
**[![Herunterladen](../../ssms/media/download-icon.png) Microsoft JDBC-Treiber 9.2.0 für SQL Server (tar.gz) herunterladen](https://go.microsoft.com/fwlink/?linkid=2153521)**  

ZIP-Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x40a)  
Tar.gz-Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x40a)  

### <a name="support-for-jdk-15"></a>Unterstützung für JDK 15

Der Microsoft JDBC-Treiber 9.2 für SQL Server ist jetzt nicht nur mit JDK 11.0 und 1.8, sondern auch mit Version 15.0 kompatibel.

### <a name="added-support-for-azure-active-directory-interactive-authentication"></a>Unterstützung für die interaktive Azure Active Directory-Authentifizierung hinzugefügt
| Zusätzlicher Authentifizierungstyp | Details |
| :---------- | :------ |
| Der Microsoft JDBC-Treiber 9.2 für SQL Server unterstützt jetzt die Authentifizierung bei Azure Key Vault über die interaktive Authentifizierung. | Weitere Informationen finden Sie unter [Herstellen einer Verbindung mithilfe der Azure Active Directory-Authentifizierung](../../connect/jdbc/connecting-using-azure-active-directory-authentication.md). |
| &nbsp; | &nbsp; |

### <a name="added-support-for-azure-active-directory-service-principal-authentication"></a>Unterstützung der Azure Active Directory-Authentifizierung mit einem Dienstprinzipal hinzugefügt
| Zusätzlicher Authentifizierungstyp | Details |
| :---------- | :------ |
| Der Microsoft JDBC-Treiber 9.2 für SQL Server unterstützt jetzt die Authentifizierung bei Azure Key Vault mit der Client-ID und dem Geheimnis einer Dienstprinzipalidentität. | Weitere Informationen finden Sie unter [Herstellen einer Verbindung mithilfe der Azure Active Directory-Authentifizierung](../../connect/jdbc/connecting-using-azure-active-directory-authentication.md). |
| &nbsp; | &nbsp; |

### <a name="updated-azure-key-vault-library-to-use-modern-azure-key-vault-library"></a>Update auf die Verwendung einer moderneren Azure Key Vault-Bibliothek
| Bibliotheksänderungen für Azure Key Vault | Details |
| :------------------- | :------ |
| Der Microsoft JDBC-Treiber 9.2 wurde von der Azure Key Vault-Bibliothek der früheren Generation und von ADAL-Bibliotheken auf die moderneren Varianten von Azure Key Vault und der Azure-Identität migriert. | **ABHÄNGIGKEITSÄNDERUNG:** Achten Sie darauf, Ihre Anwendungsabhängigkeiten zu aktualisieren, wenn Sie die Azure Active Directory-Authentifizierung oder Azure Key Vault nutzen. Weitere Informationen finden Sie in den Abhängigkeitsanforderungen beim [Arbeiten mit dem Azure Key Vault-Anbieter](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md#working-with-the-azure-key-vault-provider) oder der [Azure Active Directory-Authentifizierung](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md#working-with-azure-active-directory-authentication) |
| &nbsp; | &nbsp; |

### <a name="enabled-usebulkcopyforbatchinsert-for-non-azure-synapse-analytics-servers"></a>useBulkCopyForBatchInsert für Server außerhalb von Azure Synapse Analytics aktiviert
| Änderungen an useBulkCopyForBatchInsert für Server außerhalb von Azure Synapse Analytics | Details |
| :------------------- | :------ |
| Der Microsoft JDBC-Treiber 9.2 erlaubt jetzt die API für das Massenkopieren bei Batcheinfügungsvorgängen für Server, die nicht in Azure Synapse Analytics gehostet werden. | Weitere Informationen finden Sie unter [Verwenden der API für das Massenkopieren für Batcheinfügungsvorgänge](../../connect/jdbc/use-bulk-copy-api-batch-insert-operation.md). |
| &nbsp; | &nbsp; |


## <a name="previous-releases"></a>Vorgängerversionen

## <a name="84"></a><a id="84"></a> 8.4

**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 8.4 für SQL Server (zip)](https://go.microsoft.com/fwlink/?linkid=2137600)**  
**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 8.4 für SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2137502)**  

Versionsnummer: 8.4.1  
Veröffentlichung: 27. August 2020

Wenn Sie den Treiber in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Treiber in einer ZIP-Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x40a)  
Treiber in einer tar.gz-Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x40a)  

### <a name="compliance"></a>Kompatibilität

| Kompatibilitäts-/Konformitätsänderung | Details |
| :---------------- | :------ |
| Laden Sie die neuesten Updates für den JDBC-Treiber 8.4 herunter. | &bull; &nbsp; [GitHub, 8.4.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v8.4.1)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Vollständig konform mit JDBC-API-Spezifikation 4.2. | Die JAR-Dateien im 8.4-Paket sind gemäß Java-Versionskompatibilität benannt.<br/><br/>Beispielsweise sollte die Datei „mssql-jdbc-8.4.1.jre14.jar“ aus dem 8.4-Paket mit Java 14 verwendet werden. |
| Kompatibel mit dem Java Development Kit (JDK), Version 14.0, 11.0 und 1.8 | Der Microsoft JDBC-Treiber 8.4 für SQL Server ist jetzt zusätzlich zum JDK 11.0 (Java Development Kit) und 1.8 mit der JDK-Version 14.0 kompatibel. |
| &nbsp; | &nbsp; |

### <a name="releases"></a>Releases

Versionsnummer: 8.4.1  
Veröffentlichung: 27. August 2020  
Behobene Probleme:  

- Ein Problem mit der Inkompatibilität von `SQLServerConnectionPoolProxy` mit `delayLoadingLobs` wurde behoben.
- Ein potenzielles Problem von `NullPointerException` mit `delayLoadingLobs` wurde behoben.
- Ein Problem mit der Entschlüsselung von Spaltenverschlüsselungsschlüsseln bei Verwendung des Windows-Zertifikatspeichers wurde behoben.

Versionsnummer: 8.4.0  
Veröffentlichung: 31. Juli 2020  

### <a name="support-for-jdk-14"></a>Unterstützung für JDK 14

Der Microsoft JDBC-Treiber 8.4 für SQL Server ist jetzt zusätzlich zum JDK 11.0 (Java Development Kit) und 1.8 mit der JDK-Version 14.0 kompatibel.

### <a name="added-support-for-authentication-to-azure-key-vault-using-managed-identity"></a>Die Authentifizierung bei Azure Key Vault mit einer verwalteten Identität wird jetzt unterstützt.

| Zusätzlicher Authentifizierungstyp | Details |
| :---------- | :------ |
| Der Microsoft JDBC-Treiber 8.4 für SQL Server unterstützt jetzt die Authentifizierung bei Azure Key Vault mit einer verwalteten Identität. | Weitere Informationen finden Sie unter [Verwenden von Always Encrypted mit dem JDBC-Treiber](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md). |
| &nbsp; | &nbsp; |

### <a name="extended-support-for-bulk-copy-for-azure-data-warehouse"></a>Erweiterte Unterstützung für das Massenkopieren für Azure Data Warehouse

| Änderungen am Massenkopiervorgang für Azure Data Warehouse | Details |
| :------------------- | :------ |
| Mit dem Microsoft JDBC-Treiber 8.4 wird die neue Verbindungseigenschaft `sendTemporalDataTypesAsStringForBulkCopy` hinzugefügt. Diese boolesche Eigenschaft ist standardmäßig auf TRUE festgelegt. | Weitere Informationen finden Sie unter [Verwenden von Massenkopieren mit dem JDBC-Treiber](../../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md). |
| &nbsp; | &nbsp; |

### <a name="added-support-for-azure-sql-dns-caching"></a>Die Azure SQL-DNS-Zwischenspeicherung wird jetzt unterstützt.

| DNS-Zwischenspeicherung | Details |
| :------------------- | :------ |
| Der Microsoft JDBC-Treiber 8.4 für SQL Server unterstützt jetzt die DNS-Zwischenspeicherung für Azure SQL-Server. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="added-backwards-compatibility-for-streaming-lob-objects"></a>Das Streaming von LOB-Objekten ist jetzt abwärtskompatibel.

| LOB-Streaming | Details |
| :------------------- | :------ |
| Dem Microsoft JDBC-Treiber 8.4 für SQL Server wurde die neue Verbindungseigenschaft `delayLoadingLobs` hinzugefügt. | Wenn `delayLoadingLobs` auf FALSE festgelegt wird, werden keine vom ResultSet abgerufenen LOB-Objekte gestreamt. Dies bedeutet, dass das gesamte LOB-Objekt auf einmal vom Treiber in den Arbeitsspeicher geladen wird, ähnlich wie der Treiber vor Version 6.4 funktioniert hat. |
| &nbsp; | &nbsp; |

### <a name="added-support-for-client-certificate-authentication-for-loopback-scenarios"></a>Die Clientzertifikatauthentifizierung für Loopbackszenarios wird jetzt unterstützt.

| Clientzertifikatsauthentifizierung | Details |
| :------------------- | :------ |
| Mit dem Microsoft JDBC-Treiber 8.4 für SQL Server wurde eine neue Authentifizierungsmethode, die so genannte Clientzertifikatauthentifizierung für Loopbackszenarios, eingeführt. | Weitere Informationen finden Sie unter [Clientzertifikatauthentifizierung für Loopbackszenarios](../../connect/jdbc/client-certification-authentication-for-loopback-scenarios.md). |

## <a name="82"></a><a id="82"></a> 8.2

**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 8.2 für SQL Server (ZIP-Datei)](https://go.microsoft.com/fwlink/?linkid=2122433)**  
**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 8.2 für SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122536)**  

Versionsnummer: In Version 8.2.2. veröffentlicht: 24. März 2020

Wenn Sie den Treiber in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Treiber in einer ZIP-Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x40a)  
Treiber in einer tar.gz-Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x40a)  

### <a name="compliance"></a>Kompatibilität

| Kompatibilitäts-/Konformitätsänderung | Details |
| :---------------- | :------ |
| Laden Sie die neuesten Updates für den JDBC-Treiber 8.2 herunter. | &bull; &nbsp; [GitHub, 8.2.2](https://github.com/Microsoft/mssql-jdbc/releases/tag/v8.2.2)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Vollständig konform mit JDBC-API-Spezifikation 4.2. | Die JAR-Dateien im 8.2-Paket sind gemäß Java-Versionskompatibilität benannt.<br/><br/>Beispielsweise sollte die Datei „mssql-jdbc-8.2.2.jre11.jar“ aus dem 8.2-Paket mit Java 11 verwendet werden. |
| Kompatibel mit dem Java Development Kit (JDK), Version 13.0, 11.0 und 1.8 | Der Microsoft JDBC-Treiber 8.2 für SQL Server ist jetzt zusätzlich zu JDK 11.0 und 1.8 mit der JDK-Version 13.0 (Java Development Kit) kompatibel. |
| &nbsp; | &nbsp; |

### <a name="releases"></a>Releases

Versionsnummer: 8.2.2  
Veröffentlichung: 24. März 2020  
Behobene Probleme:  

- Es wurde eine Option zum Konfigurieren der Liste vertrauenswürdiger Azure Key Vault-Endpunkte hinzugefügt.

Versionsnummer: 8.2.1  
Veröffentlichung: 26. Februar 2020  
Behobene Probleme:  

- Ein potenzielles Problem von `NullPointerException` wurde behoben, das beim Abrufen von Daten als `java.time.LocalTime`- oder `java.time.LocalDate`-Typ mit `SQLServerResultSet.getObject()` auftrat.

Versionsnummer: 8.2.0  
Veröffentlichung: 31. Januar 2020  

### <a name="support-for-jdk-13"></a>Unterstützung für JDK 13

Der Microsoft JDBC-Treiber 8.2 für SQL Server ist jetzt zusätzlich zu JDK 11.0 und 1.8 mit der JDK-Version 13.0 (Java Development Kit) kompatibel.

### <a name="always-encrypted-with-secure-enclaves"></a>Always Encrypted mit Secure Enclaves

| Änderungen an Always Encrypted | Details |
| :--------- | :------ |
| Der Microsoft JDBC-Treiber 8.2 für SQL Server unterstützt jetzt Always Encrypted mit Secure Enclaves. Weitere Informationen finden Sie hier: Always Encrypted mit Secure Enclaves |
| Weitere Informationen und Beispielcode | Weitere Informationen finden Sie unter [Always Encrypted mit Secure Enclaves](../../connect/jdbc/using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver.md). |
| &nbsp; | &nbsp; |

### <a name="performance-improvement-when-retrieving-temporal-datatypes-from-sql-server-sup1sup"></a>Leistungsverbesserung beim Abrufen temporaler Datentypen aus SQL Server<sup>1</sup>

| Änderungen an temporalen Datentypen | Details |
| :---------- | :------ |
| Die Leistung des Microsoft JDBC-Treibers 8.2 für SQL Server beim Abrufen temporaler Datentypen aus SQL Server wurde verbessert. | Durch diese Änderung wird die unnötige Konvertierung temporaler Datentypen beseitigt, indem die Verwendung von „java.util.Calender“ nach Möglichkeit vermieden wird. |
| Im Folgenden finden Sie eine Liste der temporalen Datentypen, die von dieser Leistungsverbesserung betroffen sind. Hierbei steht zunächst der SQL Server-Datentyp und dann die entsprechende Java-Zuordnung. | date (java.sql.Date), datetime (java.sql.Timestamp), datetime2 (java.sql.Timestamp), smalldatetime (java.sql.Timestamp) und time (java.sql.Time) |
| &nbsp; | &nbsp; |

<sup>1</sup> Da Zeitzonen in der java.util.Calendar- und java.time.LocalDateTime-API unterschiedlich verarbeitet werden, gilt diese Verbesserung nicht für temporale Datentypen mit einem vom Benutzer bereitgestellten und zugeordneten java.util.Calendar-Objekt und nicht für microsoft.sql.DateTimeOffset-Datentypen.

### <a name="deployment-of-mssql-jdbc_auth-version-archdll-previously-sqljdbc_authdll-to-maven-repository"></a>Bereitstellung von mssql-jdbc_auth-\<version>-\<arch>.dll (ehemals sqljdbc_auth.dll) im Maven-Repository

| Änderungen an „sqljdbc_auth.dll“ | Details |
| :------------------- | :------ |
| Ab dem Microsoft JDBC-Treiber 8.2 für SQL Server ist der Treiber für die Verwendung des Authentifizierungsfeatures von Azure Active Directory von „mssql-jdbc_auth-\<version>-\<arch>.dll“ anstatt von „sqljdbc_auth.dll“ abhängig. | &nbsp; |
| Die DLL-Datei wurde auch in das Maven-Repository hochgeladen, um den Zugriff zu vereinfachen. | Öffnen Sie [diese Seite](https://search.maven.org/artifact/com.microsoft.sqlserver/mssql-jdbc_auth). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Bekannte Probleme

| Bekannte Probleme | Details |
| :----------- | :------ |
| Verwendung von Always Encrypted mit Secure Enclaves mit Java 8 | Benutzer müssen den BouncyCastle-Anbieter als Abhängigkeit abgeben oder einen Sicherheitsanbieter zuweisen oder laden, der den RSASSA-PSS-Signaturalgorithmus unterstützt. |
| &nbsp; | &nbsp; |

## <a name="a-id74-741"></a><a id="74"> 7.4.1

**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 7.4.1 für SQL Server (selbstextrahierende EXE-Datei)](https://go.microsoft.com/fwlink/?linkid=2122712)**  
**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 7.4.1 für SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122613)**  

Versionsnummer: 7.4.1  
Veröffentlichung: 2. August 2019

Wenn Sie den Treiber in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Treiber in einer selbstextrahierenden Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x40a)  
Treiber in einer tar.gz-Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x40a)  

### <a name="compliance"></a>Kompatibilität

| Kompatibilitäts-/Konformitätsänderung | Details |
| :---------------- | :------ |
| Laden Sie die neuesten Updates für den JDBC-Treiber 7.4 herunter. | &bull; &nbsp; [GitHub, 7.4.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v7.4.1)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Vollständig konform mit JDBC-API-Spezifikation 4.2. | Die JAR-Dateien im 7.4-Paket sind gemäß Java-Versionskompatibilität benannt.<br/><br/>Beispielsweise muss die Datei „mssql-jdbc-7.4.1.jre11.jar“ aus dem 7.4-Paket mit Java 11 verwendet werden. |
| Kompatibel mit dem Java Development Kit (JDK), Version 12.0, 11.0 und 1.8. | Der Microsoft JDBC-Treiber 7.4 für SQL Server ist jetzt zusätzlich zu JDK 11.0 und 1.8 mit der JDK-Version 12.0 (Java Development Kit) kompatibel. |
| &nbsp; | &nbsp; |

### <a name="releases"></a>Releases

Versionsnummer: 7.4.1  
Veröffentlichung: 2. August 2019  
Behobene Probleme:  

- Neue `hashCode()`- und `equals()`-API-Implementierungen von `SQLServerDataTable` und `SQLServerDataColumn` wurden rückgängig gemacht, weil die Abwärtskompatibilität durch die API-Änderung beeinträchtigt wurde.

Versionsnummer: 7.4.0  
Veröffentlichung: 31. Juli 2019  

### <a name="support-for-jdk-12"></a>Unterstützung für JDK 12

Der Microsoft JDBC-Treiber 7.4 für SQL Server ist jetzt zusätzlich zu JDK 11.0 und 1.8 mit der JDK-Version 12.0 (Java Development Kit) kompatibel.

### <a name="introduces-ntlm-authentication"></a>NTLM-Authentifizierung

| NTLM-Änderung | Details |
| :--------- | :------ |
| Unterstützt den NTLM-Authentifizierungsmodus | Mit diesem Authentifizierungsmodus können Windows- und Nicht-Windows-Clients sich mithilfe von Domänenbenutzern bei SQL Server authentifizieren. |
| Weitere Details und eine Beispielanwendung zum Verwenden dieses Authentifizierungsmodus. | Weitere Informationen finden Sie unter [Herstellen von Verbindungen mit SQL Server mit NTLM-Authentifizierung](using-ntlm-authentication-to-connect-to-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="introduces-querying-parametermetadata-via-_usefmtonly_"></a>Abfragen von ParameterMetaData über _useFmtOnly_

| useFmtOnly-Änderung | Details |
| :---------- | :------ |
| Die Verbindungseigenschaft **useFmtOnly** wurde hinzugefügt. | Dieses Feature ermöglicht es Benutzern, ParameterMetaData optional über die `SET FMTONLY ON`-Legacy-API abzufragen. Es ist nützlich für Szenarios, in denen `sp_describe_undeclared_parameters` nicht die erwartete Leistung bringt. |
| Weitere Details und Einschränkungen | Siehe [Verwenden von useFmtOnly](using-usefmtonly.md) |
| &nbsp; | &nbsp; |

### <a name="updated-_microsoft-azure-key-vault-sdk-for-java_-version-121"></a>Aktualisiert: _Microsoft Azure Key Vault SDK für Java_, Version 1.2.1

| Key Vault SDK-Änderung | Details |
| :------------------- | :------ |
| Die Maven-Abhängigkeit von _Microsoft Azure Key Vault SDK für Java_ wurde auf Version 1.2.1 aktualisiert. | &nbsp; |
| Entfernt _Microsoft Azure SDK für Key Vault WebKey_ als Maven-Abhängigkeit. | &nbsp; |
| Zusätzliche Details. | Informationen hierzu finden Sie unter [Featureabhängigkeiten des Microsoft JDBC-Treibers für SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Bekannte Probleme

| Bekannte Probleme | Details |
| :----------- | :------ |
| Bei Verwendung der NTLM-Authentifizierung. | Die zeitgleiche Aktivierung des erweiterten Schutzes und von verschlüsselten Verbindungen wird derzeit nicht unterstützt. |
| Bei Verwendung von useFmtOnly. | Es gibt einige Probleme mit diesem Feature. Diese sind auf Mängel in der SQL-Parserlogik zurückzuführen. Weitere Informationen und Vorschläge zu Problemumgehungen finden Sie unter [Abrufen von ParameterMetaData über useFmtOnly](using-usefmtonly.md). |
| &nbsp; | &nbsp; |

## <a name="a-id72-722"></a><a id="72"> 7.2.2

**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 7.2.2 für SQL Server (selbstextrahierende EXE-Datei)](https://go.microsoft.com/fwlink/?linkid=2122435)**  
**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 7.2.2 für SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122434)**  

Versionsnummer: 7.2.2  
Veröffentlichung: 16. April 2019

Wenn Sie den Treiber in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Treiber in einer selbstextrahierenden Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x40a)  
Treiber in einer tar.gz-Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x40a)  

### <a name="compliance"></a>Kompatibilität

| Kompatibilitäts-/Konformitätsänderung | Details |
| :---------------- | :------ |
| Laden Sie die neuesten Updates für JDBC-Treiber 7.2 herunter. | &bull; &nbsp; [GitHub, 7.2.2](https://github.com/Microsoft/mssql-jdbc/releases/tag/v7.2.2)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Vollständig konform mit JDBC-API-Spezifikation 4.2. | Die JAR-Dateien im 7.2-Paket sind gemäß der Java-Versionskompatibilität benannt.<br/><br/>Beispielsweise sollte die Datei „mssql-jdbc-7.2.2.jre11.jar“ aus dem 7.2-Paket mit Java 11 verwendet werden. |
| Kompatibel mit Java Development Kit (JDK), Version 11.0, zusätzlich zu JDK 1.8. | Der Microsoft JDBC-Treiber 7.2 für SQL Server ist jetzt mit Java Development Kit (JDK) Version 11.0 zusätzlich zu JDK 1.8 kompatibel. |
| &nbsp; | &nbsp; |

### <a name="releases"></a>Releases

Versionsnummer: 7.2.2  
Veröffentlichung: 16. April 2019  
Behobene Probleme:  

- Probleme mit nicht ordnungsgemäß bereinigten Aktivitäts-IDs wurden behoben.

Versionsnummer: 7.2.1  
Veröffentlichung: 11. Februar 2019  
Behobene Probleme:  

- Fehler beim Analysieren bestimmter parametrisierter Abfragen wurden behoben.

Versionsnummer: 7.2.0  
Veröffentlichung: 31. Januar 2019  

### <a name="active-directory-_managed-identity_-msi-authentication"></a>Active Directory-Authentifizierung mit einer _verwalteten Identität_ (MSI)

| MSI-Änderung | Details |
| :--------- | :------ |
| Unterstützt den Active Directory-Authentifizierungsmodus mit einer verwalteten Identität (MSI). | Dieser Authentifizierungsmodus gilt für Azure-Ressourcen, bei denen die Unterstützung der Identitätsfunktion aktiviert ist.<br/><br/>Beide Typen von verwalteten Identitäten (MSI) werden vom Treiber unterstützt, um **Zugriffstoken** (accessToken) zum Herstellen sicherer Verbindungen abzurufen. |
| Weitere Details und eine Beispielanwendung zum Verwenden dieses Authentifizierungsmodus. | Informationen hierzu finden Sie unter [Herstellen einer Verbindung mithilfe der Azure Active Directory-Authentifizierung](connecting-using-azure-active-directory-authentication.md). |
| &nbsp; | &nbsp; |

### <a name="introduces-_open-service-gateway-initiative_-osgi-support"></a>Einführung der Unterstützung von _Open Service Gateway Initiative_ (OSGi)

| OSGi-Änderung | Details |
| :---------- | :------ |
| Die **DataSourceFactory**-Implementierung wurde hinzugefügt. | &bull; &nbsp; `org.osgi.service.jdbc.DataSourceFactory`<br/>&bull; &nbsp; `com.microsoft.sqlserver.jdbc.osgi.SQLServerDataSourceFactory` |
| Die **Activator**-Implementierung wurde hinzugefügt. | &bull; &nbsp; `org.osgi.framework.BundleActivator`<br/>&bull; &nbsp; `com.microsoft.sqlserver.jdbc.osgi.Activator` |
| &nbsp; | &nbsp; |

### <a name="introduces-_sqlservererror_-apis"></a>Einführung von _SQLServerError_-APIs

| Fehler-API-Änderung | Details |
| :--------------- | :------ |
| Die SQLServerError-API wurde eingeführt. | Getter-APIs zum Abrufen zusätzlicher Details zu dem vom Server generierten Fehler.<br/><br/>&bull; &nbsp; `SQLServerException.getSQLServerError()`<br/>&bull; &nbsp; `SQLServerError` |
| Zusätzliche Details. | Informationen hierzu finden Sie unter [Behandlung von Fehlern](handling-errors.md). |
| &nbsp; | &nbsp; |

### <a name="updated-_microsoft-azure-active-directory-authentication-library-adal4j-for-java_-version-163"></a>Die _Microsoft Azure Active Directory-Authentifizierungsbibliothek für Java (ADAL4J)_ , Version 1.6.3, wurde aktualisiert.

| ADAL4J-Änderung | Details |
| :------------ | :------ |
| Die Maven-Abhängigkeit von ADAL4J wurde auf Version 1.6.3 aktualisiert. | &nbsp; |
| Führt _Java-Clientlaufzeit für AutoRest_ als Maven-Abhängigkeit ein (Version 1.6.5). | &nbsp; |
| Zusätzliche Details. | Informationen hierzu finden Sie unter [Featureabhängigkeiten des Microsoft JDBC-Treibers für SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="updated-_microsoft-azure-key-vault-sdk-for-java_-version-120"></a>Aktualisiert: _Microsoft Azure Key Vault SDK für Java_, Version 1.2.0

| Key Vault SDK-Änderung | Details |
| :------------------- | :------ |
| Die Maven-Abhängigkeit von _Microsoft Azure Key Vault SDK für Java_ wurde auf Version 1.2.0 aktualisiert. | &nbsp; |
| Führt _Microsoft Azure SDK für Key Vault WebKey_ als Maven-Abhängigkeit ein (Version 1.2.0). | &nbsp; |
| Zusätzliche Details. | Informationen hierzu finden Sie unter [Featureabhängigkeiten des Microsoft JDBC-Treibers für SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Bekannte Probleme

| Bekannte Probleme | Details |
| :----------- | :------ |
| Parametrisierte Abfragen (in bestimmten Fällen). | Im Februar 2019 wurde ein Update der Version 7.2.0, v7.2.1, zur Lösung dieses Problems veröffentlicht. |
| Bereinigung von Aktivitäts-IDs. | Im April 2019 wurde ein Update der Version 7.2.1, v7.2.2, zur Lösung dieses Problems veröffentlicht. |
| &nbsp; | &nbsp; |

## <a name="70"></a>7.0

**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 7.0 für SQL Server (selbstextrahierende EXE-Datei)](https://go.microsoft.com/fwlink/?linkid=2122713)**  
**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 7.0 für SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122614)**  

Versionsnummer: 7.0.0  
Veröffentlichung: 31. Juli 2018

Wenn Sie den Treiber in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Treiber in einer selbstextrahierenden Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x40a)  
Treiber in einer tar.gz-Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x40a)  

Microsoft JDBC-Treiber 7.0 für SQL Server ist mit JDBC-API-Spezifikation 4.2 vollständig konform. Die JAR-Dateien im 7.0-Paket sind gemäß der Java-Versionskompatibilität benannt. Beispielsweise sollte die Datei „mssql-jdbc-7.0.0.jre10.jar“ aus dem 7.0-Paket mit Java 10 verwendet werden.

### <a name="support-for-jdk-10"></a>Unterstützung für JDK 10

Der Microsoft JDBC-Treiber 7.0 für SQL Server ist jetzt mit Java Development Kit (JDK) Version 10.0 zusätzlich zu JDK 1.8 kompatibel. Dieses Update zeigt auch den `Automatic-Module-Name` des Treibers als `com.microsoft.sqlserver.jdbc` über die Manifestdatei an.

### <a name="support-for-spatial-datatypes"></a>Unterstützung für räumliche Datentypen

Microsoft JDBC-Treiber 7.0 für SQL Server bietet jetzt Unterstützung für die räumlichen SQL Server-Datentypen „Geography“ und „Geometry“. Weitere Informationen zu APIs für räumliche Datentyp und deren Verwendung finden Sie unter [Verwendung von räumlichen Datentypen](use-spatial-datatypes.md).

### <a name="implementation-for-jdbc-43-introduced-javasqlconnection-apis-beginrequest-and-endrequest"></a>Implementierung der in JDBC 4.3 eingeführten java.sql.Connection-APIs beginRequest() und endRequest()

Microsoft JDBC-Treiber 7.0 für SQL Server implementiert jetzt die APIs `beginRequest()` und `endRequest()` der `java.sql.Connection`-Klasse. Diese APIs wurden in JDBC-Spezifikation 4.3 und JDK 9 eingeführt. Weitere Informationen zur Implementierung dieser APIs im Treiber finden Sie unter [JDBC 4.3-Kompatibilität für den JDBC-Treiber](jdbc-4-3-compliance-for-the-jdbc-driver.md).

### <a name="support-for-sql-data-discovery-and-classification"></a>Unterstützung für die SQL-Datenermittlung und -klassifizierung

Microsoft JDBC-Treiber 7.0 für SQL Server bietet Unterstützung für die SQL-Datenermittlung und -klassifizierung in jeder Zieldatenbank, die diese Funktion unterstützt. Der Treiber stellt jetzt `SQLServerResultSet.getSensitivityClassification()`-APIs zum Extrahieren dieser Informationen aus dem abgerufenen `ResultSet` bereit.

Weitere Informationen zum Verwenden dieser Funktion mit dem JDBC-Treiber finden Sie im Beispiel unter [SQL-Datenermittlung und -klassifizierung](data-discovery-classification-sample.md).

### <a name="added-connection-property-usebulkcopyforbatchinsert"></a>Zusätzliche Verbindungseigenschaft: useBulkCopyForBatchInsert

In Microsoft JDBC-Treiber 7.0 für SQL Server wird die neue Verbindungseigenschaft `useBulkCopyForBatchInsert` eingeführt. Diese Eigenschaft wird nur für Azure Synapse Analytics unterstützt.

Diese Eigenschaft ist standardmäßig deaktiviert. Sie können sie aktivieren, um die Leistung von Benutzeranwendungen zu erhöhen, wenn Sie große Mengen von Daten in Azure Synapse Analytics übertragen. Wenn Sie diese Eigenschaft aktivieren, ändert sich das Verhalten von Batcheinfügungsvorgängen, um zu Massenkopiervorgängen mit vom Benutzer bereitgestellten Daten zu wechseln. Weitere Informationen zu dieser Eigenschaft und ihren Einschränkungen finden Sie unter [Verwenden der Massenkopierungs-API für den Batcheinfügungsvorgang](use-bulk-copy-api-batch-insert-operation.md).

### <a name="added-connection-property-cancelquerytimeout"></a>Zusätzliche Verbindungseigenschaft: cancelQueryTimeout

In Microsoft JDBC-Treiber 7.0 für SQL Server wird die neue Verbindungseigenschaft `cancelQueryTimeout` eingeführt, um `queryTimeout` für `java.sql.Connection`- und `java.sql.Statement`-Objekte abzubrechen.

### <a name="added-azure-key-vault-provider-constructors"></a>Zusätzliche Azure Key Vault-Anbieterkonstruktoren

In Microsoft JDBC-Treiber 7.0 für SQL Server wird ein zuvor entfernter Konstruktor für `SQLServerColumnEncryptionAzureKeyVaultProvider` wieder hinzugefügt. Er ließ die Authentifizierung über eine benutzerdefinierte Methode zu, die über `SQLServerKeyVaultAuthenticationCallback` implementiert wurde, um ein Zugriffstoken abzurufen.

Die Definition für die neuen Konstruktoren lautet wie folgt:

```java
/* This constructor is added to provide backward compatibility with 6.0
* version of the driver. It is marked deprecated for removal in the next
* stable release.
*/
@Deprecated
public SQLServerColumnEncryptionAzureKeyVaultProvider(
        SQLServerKeyVaultAuthenticationCallback authenticationCallback,
        ExecutorService executorService) throws SQLServerException;

/*New constructor to replace the above constructor*/
public SQLServerColumnEncryptionAzureKeyVaultProvider(
            SQLServerKeyVaultAuthenticationCallback authenticationCallback) throws SQLServerException;
```

### <a name="updated-microsoft-azure-active-directory-authentication-library-adal4j-for-java-version-160"></a>Aktualisiert: „Microsoft Azure Active Directory-Authentifizierungsbibliothek für Java (ADAL4J)“, Version 1.6.0

In Microsoft JDBC-Treiber 7.0 für SQL Server wurde die Maven-Abhängigkeit von der Microsoft Azure Active Directory-Authentifizierungsbibliothek (ADAL4J) für Java auf Version 1.6.0 aktualisiert. Weitere Informationen zu Abhängigkeiten finden Sie unter [Featureabhängigkeiten des Microsoft JDBC-Treibers für SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md).

## <a name="64"></a>6.4

**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 6.4 für SQL Server (selbstextrahierende EXE-Datei)](https://go.microsoft.com/fwlink/?linkid=2122436)**  
**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 6.4 für SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122537)**  

Versionsnummer: 6.4.0  
Veröffentlichung: 27. Februar 2018

Wenn Sie den Treiber in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Treiber in einer selbstextrahierenden Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x40a)  
Treiber in einer tar.gz-Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x40a)  

Microsoft JDBC-Treiber 6.4 für SQL Server ist mit den JDBC-Spezifikationen 4.1 und 4.2 vollständig konform. Die JAR-Dateien im 6.4-Paket sind gemäß der Java-Versionskompatibilität benannt. Beispielsweise sollte die Datei „mssql-jdbc-6.4.0.jre8.jar“ aus dem 6.4-Paket mit Java 8 verwendet werden.

### <a name="support-for-jdk-9"></a>Unterstützung für JDK 9

Der Treiber unterstützt jetzt die JDK-Version 9.0 zusätzlich zu JDK 8.0 und 7.0.

### <a name="jdbc-43-compliance"></a>JDBC 4.3-Konformität

Der Treiber unterstützt die Java Database Connectivity-API-Spezifikation 4.3, zusätzlich zu 4.1 und 4.2. Die JDBC 4.3-API-Methoden wurden hinzugefügt, aber noch nicht implementiert. Weitere Informationen finden Sie unter [JDBC 4.3-Kompatibilität für den JDBC-Treiber](jdbc-4-3-compliance-for-the-jdbc-driver.md).

### <a name="added-connection-property-sslprotocol"></a>Zusätzliche Verbindungseigenschaft: sslProtocol

Mit einer neuen Verbindungseigenschaft können Benutzer das TLS-Protokollschlüsselwort angeben. Mögliche Werte: TLS, TLSv1, TLSv1.1 und TLSv1.2. Weitere Informationen finden Sie unter [SSLProtocol](https://github.com/Microsoft/mssql-jdbc/wiki/SSLProtocol).

### <a name="deprecated-connection-property-fipsprovider"></a>Veraltete Verbindungseigenschaft: fipsProvider

Die Verbindungseigenschaft `fipsProvider` wird aus der Liste der zulässigen Verbindungseigenschaften entfernt. Weitere Informationen finden Sie in der zugehörigen [GitHub Pull Request](https://github.com/Microsoft/mssql-jdbc/pull/460).

### <a name="added-connection-properties-for-specifying-a-custom-trustmanager"></a>Zusätzliche Verbindungseigenschaften für das Angeben eines benutzerdefinierten TrustManagers

Der Treiber unterstützt jetzt das Angeben eines benutzerdefinierten TrustManagers mit den hinzugefügten Verbindungseigenschaften `trustManagerClass` und `trustManagerConstructorArg`. Sie können eine Gruppe von Zertifikaten, die pro Verbindung als vertrauenswürdig eingestuft werden, dynamisch angeben, ohne die globalen Einstellungen der Java Virtual Machine-Umgebung (JVM) ändern zu müssen.

### <a name="added-support-for-datetimesmalldatetime-in-table-valued-parameters"></a>Hinzugefügt: Unterstützung für die Tabellenwertparameter „datetime“ und „smallDatetime“

Der Treiber unterstützt jetzt die Datentypen `datetime` und `smallDatetime` bei der Verwendung von Tabellenwertparametern (Table-Valued Parameters, TVPs).

### <a name="added-support-for-the-sql_variant-datatype"></a>Hinzugefügt: Unterstützung für den Datentyp „sql_variant“

Der JDBC-Treiber unterstützt jetzt `sql_variant`-Datentypen für die Verwendung mit SQL Server. Der Datentyp `sql_variant` wird auch in Verbindung mit Features wie TVPs und Massenkopieren mit folgenden Einschränkungen unterstützt:

* **Für Datumswerte**:

  Wenn Sie einen TVP zum Auffüllen einer Tabelle verwenden, die gespeicherte `datetime`-, `smalldatetime`- oder `date`-Werte in einer `sql_variant`-Spalte enthält, funktioniert das Aufrufen der `getDateTime()`-, `getSmallDateTime()`- oder `getDate()`-Methode für das Resultset nicht, und es wird die folgende Ausnahme ausgelöst:

  `java java.lang.String cannot be cast to java.sql.Timestamp`

  Verwenden Sie als Problemumgehung stattdessen die `getString()`- oder `getObject()`-Methode.

* **Verwenden von TVP mit sql_variant für Nullwerte**:
  
  Wenn Sie einen TVP zum Auffüllen einer Tabelle verwenden und einen NULL-Wert an den Spaltentyp `sql_variant` senden, tritt eine Ausnahme auf. Das Einfügen eines NULL-Werts mit dem Spaltentyp `sql_variant` in TVPs wird derzeit nicht unterstützt.

### <a name="implemented-prepared-statement-metadata-caching"></a>Implementiert: Zwischenspeicherung von Metadaten einer vorbereiteten Anweisung

Im JDBC-Treiber wurde die Zwischenspeicherung von Metadaten einer vorbereiteten Anweisung zwecks Leistungsverbesserung implementiert. Der Treiber unterstützt jetzt das Zwischenspeichern von Metadaten einer vorbereiteten Anweisung im Treiber mit den Verbindungseigenschaften `disableStatementPooling` und `statementPoolingCacheSize`. Dieses Feature ist standardmäßig deaktiviert. Weitere Informationen finden Sie unter [Zwischenspeichern von Metadaten einer vorbereiteten Anweisung für den JDBC-Treiber](prepared-statement-metadata-caching-for-the-jdbc-driver.md).

### <a name="added-support-for-azure-ad-integrated-authentication-on-linuxmacos"></a>Hinzugefügt: Unterstützung für die integrierte Azure AD-Authentifizierung unter Linux/macOS

Der JDBC-Treiber unterstützt nun auch die integrierte Azure Active Directory-Authentifizierung (Azure AD) unter allen Betriebssystemen (Windows, Linux und macOS) mit Kerberos. Alternativ können sich Benutzer unter Windows-Betriebssystemen mit „mssql-jdbc_auth-\<version>-\<arch>.dll“ authentifizieren.

### <a name="updated-microsoft-azure-active-directory-authentication-library-adal4j-for-java-version-140"></a>Aktualisiert: „Microsoft Azure Active Directory-Authentifizierungsbibliothek für Java (ADAL4J)“, Version 1.4.0

Für den JDBC-Treiber wurde die Maven-Abhängigkeit von der Microsoft Azure Active Directory-Authentifizierungsbibliothek (ADAL4J) für Java auf Version 1.4.0 aktualisiert. Weitere Informationen zu Abhängigkeiten finden Sie unter [Featureabhängigkeiten des Microsoft JDBC-Treibers für SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md).

## <a name="62"></a>6.2

**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 6.2 für SQL Server (selbstextrahierende EXE-Datei)](https://go.microsoft.com/fwlink/?linkid=2122616)**  
**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 6.2 für SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122615)**  

Versionsnummer: 6.2.2  
Veröffentlichung: 29. September 2017

Wenn Sie den Treiber in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Treiber in einer selbstextrahierenden Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x40a)  
Treiber in einer tar.gz-Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x40a)  

Microsoft JDBC-Treiber 6.2 für SQL Server ist mit den JDBC-Spezifikationen 4.1 und 4.2 vollständig konform. Die JAR-Dateien im 6.2-Paket sind gemäß der Java-Versionskompatibilität benannt. Beispielsweise sollte die Datei „mssql-jdbc-6.2.2.jre8.jar“ aus dem 6.2-Paket mit Java 8 verwendet werden.

### <a name="releases"></a>Releases

Versionsnummer: 6.2.2  
Veröffentlichung: 3. Oktober 2017  
Behobene Probleme:  

- Die ADAL4J-Abhängigkeit wurde auf Version 1.2.0 und die Azure Key Vault-Abhängigkeit auf Version 1.0.0 aktualisiert.

Versionsnummer: 6.2.1  
Veröffentlichung: 14. Juli 2017  
Behobene Probleme:  

- Ein Problem bei der Ausführung von Abfragen ohne Parameter unter Verwendung von `preparedStatement` wurde behoben.

Versionsnummer: 6.2.0  
Veröffentlichung: 30. Juni 2017  

> [!NOTE]  
> In der am 29. Juni 2017 veröffentlichten Treiberversion JDBC 6.2 RTW wurde ein Problem bei der Verbesserung der Zwischenspeicherung von Metadaten gefunden. Die Verbesserung wurde zurückgesetzt, und am 17. Juli 2017 wurden neue JAR-Dateien (Version 6.2.1) veröffentlicht.
>
> Durch eine weitere Verbesserung wurde die abhängige Bibliotheksversion von Azure Key Vault auf 1.0.0 aktualisiert, und am 19. Oktober 2017 wurden neue JAR-Dateien (Version 6.2.2) veröffentlicht.
>
> Über die folgenden Links können Sie die aktuellen Updates für den JDBC-Treiber 6.2 herunterladen: [GitHub](https://github.com/Microsoft/mssql-jdbc/releases/tag/v6.2.2) oder [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver). Aktualisieren Sie Ihre Projekte zur Verwendung der JAR-Dateien der Version 6.2.2. Weitere Informationen finden Sie in den Versionshinweisen zu [6.2.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v6.2.1) und [6.2.2](https://github.com/Microsoft/mssql-jdbc/releases/tag/v6.2.2).

### <a name="azure-ad-support-for-linux"></a>Azure AD-Unterstützung für Linux

Verbinden Sie Ihre Linux-Anwendungen mit Azure SQL-Datenbank mithilfe der Azure AD-Authentifizierung über Benutzername/Kennwort- und Zugriffstoken-Methoden.

### <a name="fips-enabled-jvms"></a>FIPS-aktivierte JVMs

Der JDBC-Treiber kann jetzt auf JVMs verwendet werden, die im Federal Information Processing Standard 140-Kompatibilitätsmodus (FIPS 140-Kompatibilitätsmodus) ausgeführt werden, um Bundesnormen für Compliance zu erfüllen.

### <a name="kerberos-authentication-improvements"></a>Verbesserungen der Kerberos-Authentifizierung

Der JDBC-Treiber bietet jetzt Unterstützung für Folgendes:

* Prinzipal/Kennwort-Methode für Anwendungen, bei denen die Kerberos-Konfiguration nicht geändert werden kann oder kein neues Token oder keine Schlüsseltabelle abgerufen werden kann. Diese Methode kann für die Authentifizierung bei einer SQL Server-Instanz verwendet werden, die nur die Kerberos-Authentifizierung zulässt.
* Bereichsübergreifende Authentifizierung mit integrierter Kerberos-Authentifizierung ohne explizites Festlegen des Server-SPNs. Der Treiber berechnet jetzt automatisch den Bereich, auch wenn er nicht angegeben ist.
* Eingeschränkte Kerberos-Delegierung durch das Akzeptieren von Benutzeranmeldeinformationen mit Identitätswechsel als Objekt mit GSS-Anmeldeinformationen über die Datenquelle. Diese Anmeldeinformationen mit Identitätswechsel werden dann zum Herstellen einer Kerberos-Verbindung verwendet.

### <a name="added-timeouts"></a>Zusätzliche Timeouts

Der JDBC-Treiber unterstützt jetzt die folgenden konfigurierbare Timeouts. Sie können sie basierend auf den Anforderungen Ihrer Anwendung ändern.

* Abfragetimeout zum Steuern der Anzahl von Sekunden, die beim Ausführen einer Abfrage verstreichen sollen, bevor ein Timeout auftritt.
* Sockettimeout zum Angeben der Anzahl von Millisekunden, die verstreichen sollen, bevor ein Timeout für einen Socketlesevorgang oder eine Socketannahme auftritt.

## <a name="61"></a>6.1

Versionsnummer: 6.1.0  
Veröffentlichung: 17. November 2016  

Microsoft JDBC-Treiber 6.1 für SQL Server ist mit den JDBC-Spezifikationen 4.1 und 4.2 vollständig konform. Dies ist die erste Open-Source-Version des JDBC-Treibers. Den Quellcode finden Sie unter dem [GitHub v6.1.0-Tag](https://github.com/microsoft/mssql-jdbc/releases/tag/v6.1.0). Dieser erstellt die JAR-Dateien „mssql-jdbc-6.1.0.jre8.jar“ und „mssql-jdbc-6.1.0.jre7.jar“, die der Java-Versionskompatibilität entsprechen.

## <a name="60"></a>6.0

**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 6.0 für SQL Server (selbstextrahierende EXE-Datei)](https://go.microsoft.com/fwlink/?linkid=2122617)**  
**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 6.0 für SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122714)**  

Versionsnummer: 6.0.8112  
Veröffentlichung: 17. Januar 2017

Wenn Sie den Treiber in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Treiber in einer selbstextrahierenden Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x40a)  
Treiber in einer tar.gz-Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x40a)  

Microsoft JDBC-Treiber 6.0 für SQL Server ist mit den JDBC-Spezifikationen 4.1 und 4.2 vollständig konform. Die JAR-Dateien im 6.0-Paket sind nach ihrer Kompatibilität mit der JDBC-API-Version benannt. Beispielsweise ist die Datei „sqljdbc42.jar“ aus dem 6.0-Paket mit JDBC-API 4.2 kompatibel. Auf ähnliche Weise ist die Datei „sqljdbc41.jar“ mit JDBC-API 4.1 kompatibel.

Führen Sie die folgenden Codezeilen aus, um sicherzustellen, dass Sie über die richtige Datei „sqljdbc42.jar“ oder „sqljdbc41.jar“ verfügen. Wenn die Ausgabe „Treiberversion: 6.0.7507.100“ lautet, verfügen Sie über das JDBC-Treiberpaket 6.0.

```java
Connection conn = DriverManager.getConnection("jdbc:sqlserver://<server>;user=<user>;password=<password>;");
System.out.println("Driver version: " + conn.getMetaData().getDriverVersion());
```

### <a name="always-encrypted"></a>Always Encrypted

Der Treiber unterstützt das Feature Always Encrypted in SQL Server 2016. Mit diesem Feature wird sichergestellt, dass vertrauliche Daten in einer SQL Server-Instanz niemals in Klartext angezeigt werden. Always Encrypted arbeitet mit transparenter Verschlüsselung der Daten in der Anwendung, sodass SQL Server nur die verschlüsselten Daten verarbeitet, aber keine Klartextwerte. Selbst wenn die Integrität der SQL Server-Instanz oder des Hostcomputers beschädigt ist, kann ein Angreifer sensible Daten nur als chiffrierten Text sehen. Einzelheiten finden Sie unter [Using Always Encrypted with the JDBC Driver (Verwenden von Always Encrypted mit dem JDBC-Treiber)](using-always-encrypted-with-the-jdbc-driver.md).

### <a name="internationalized-domain-names"></a>Internationale Domänennamen

Der Treiber unterstützt internationalisierte Domänennamen (Internationalized Domain Names, IDNs) für Servernamen. Weitere Informationen finden Sie unter „Verwenden internationaler Domänennamen“ im Artikel [Internationale Funktionen des JDBC-Treibers](international-features-of-the-jdbc-driver.md).

### <a name="parameterized-queries"></a>Parametrisierte Abfragen

Der Treiber unterstützt jetzt das Abrufen von Parametermetadaten mit vorbereiteten Anweisungen für komplexe Abfragen wie Teilabfragen und/oder Joins. Beachten Sie, dass diese Verbesserung nur bei Verwendung von SQL Server 2012 und neueren Versionen verfügbar ist.

### <a name="azure-active-directory"></a>Azure Active Directory

Die Azure AD-Authentifizierung ist ein Mechanismus zum Herstellen einer Verbindung mit Azure SQL-Datenbank v12 mithilfe von Identitäten in Azure AD. Verwenden Sie Azure AD-Authentifizierung zur zentralen Verwaltung von Identitäten von Datenbankbenutzern und als Alternative zur SQL Server-Authentifizierung.

Mit JDBC-Treiber 6.0 können Sie Ihre Azure AD-Anmeldeinformationen in der JDBC-Verbindungszeichenfolge angeben, um die Verbindung mit Azure SQL-Datenbank herzustellen. Weitere Informationen finden Sie in der Beschreibung der Authentifizierungseigenschaft im Artikel [Festlegen der Verbindungseigenschaften](setting-the-connection-properties.md).

### <a name="table-valued-parameters"></a>Tabellenwertparameter

TVPs bieten eine einfache Möglichkeit zum Marshallen mehrerer Datenzeilen aus einer Clientanwendung nach SQL Server, ohne dass mehrere Roundtrips oder eine spezielle serverseitige Logik für die Verarbeitung der Daten erforderlich sind. Sie können TVPs verwenden, um Datenzeilen in einer Clientanwendung zu kapseln und die Daten in einem einzigen parametrisierten Befehl an den Server zu senden. Die eingehenden Datenzeilen werden in einer Tabellenvariablen gespeichert, die Sie dann mithilfe von Transact-SQL bearbeiten können. Weitere Informationen finden Sie unter [Verwenden von Tabellenwertparametern](using-table-valued-parameters.md).

### <a name="always-on-availability-groups"></a>Always On-Verfügbarkeitsgruppen

Der Treiber unterstützt jetzt transparente Verbindungen mit Always On-Verfügbarkeitsgruppen. Der Treiber ermittelt schnell die aktuelle Always On-Topologie Ihrer Serverinfrastruktur und stellt transparent eine Verbindung mit dem derzeit aktiven Server her.

## <a name="42"></a>4,2

**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 4.2 für SQL Server (selbstextrahierende EXE-Datei)](https://go.microsoft.com/fwlink/?linkid=2122538)**  
**[![Download](../../ssms/media/download-icon.png) Herunterladen des Microsoft JDBC-Treibers 4.2 für SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2122437)**  

Versionsnummer: 4.2.8112  
Veröffentlichung: 24. August 2015

Wenn Sie den Treiber in einer anderen Sprache als der für Sie erkannten herunterladen möchten, können Sie hierfür einen der folgenden direkten Links verwenden.  
Treiber in einer selbstextrahierenden Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x40a)  
Treiber in einer tar.gz-Datei: [Chinesisch (vereinfacht)](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x804) | [Chinesisch (traditionell)](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x404) | [Englisch (Vereinigte Staaten)](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x409) | [Französisch](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x40c) | [Deutsch](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x407) | [Italienisch](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x410) | [Japanisch](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x411) | [Koreanisch](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x412) | [Portugiesisch (Brasilien)](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x416) | [Russisch](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x419) | [Spanisch](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x40a)  

Microsoft JDBC-Treiber 4.2 für SQL Server ist mit den JDBC-Spezifikationen 4.1 und 4.2 vollständig konform. Die JAR-Dateien im 4.2-Paket sind nach ihrer Kompatibilität mit der JDBC-API-Version benannt. Beispielsweise ist die Datei „sqljdbc42.jar“ aus dem 4.2-Paket mit JDBC-API 4.2 kompatibel. Auf ähnliche Weise ist die Datei „sqljdbc41.jar“ mit JDBC-API 4.1 kompatibel.

Führen Sie die folgenden Codezeilen aus, um sicherzustellen, dass Sie über die richtige Datei „sqljdbc42.jar“ oder „sqljdbc41.jar“ verfügen. Wenn die Ausgabe „Treiberversion: 4.2.6420.100“ lautet, verfügen Sie über das JDBC-Treiberpaket 4.2.

```java
Connection conn = DriverManager.getConnection("jdbc:sqlserver://<server>;user=<user>;password=<password>;");
System.out.println("Driver version: " + conn.getMetaData().getDriverVersion());
```

### <a name="support-for-jdk-8"></a>Unterstützung für JDK 8

Der Treiber unterstützt jetzt die JDK-Version 8.0 zusätzlich zu JDK 7.0, 6.0 und 5.0.

### <a name="jdbc-41-and-42-compliance"></a>JDBC 4.1- und 4.2-Kompatibilität

Der Treiber unterstützt die Java Database Connectivity-API-Spezifikationen 4.1 und 4.2, zusätzlich zu 4.0. Weitere Informationen finden Sie unter [JDBC 4.1-Kompatibilität für den JDBC-Treiber](jdbc-4-1-compliance-for-the-jdbc-driver.md) und [JDBC 4.2-Kompatibilität für den JDBC-Treiber](jdbc-4-2-compliance-for-the-jdbc-driver.md).

### <a name="bulk-copy"></a>Massenkopieren

Die Massenkopierfunktion wird verwendet, um schnell große Datenmengen in Tabellen oder Ansichten in SQL Server-Datenbanken zu kopieren. Weitere Informationen finden Sie unter [Verwenden von Massenkopieren mit dem JDBC-Treiber](using-bulk-copy-with-the-jdbc-driver.md).

### <a name="xa-transaction-rollback-option"></a>Rollbackoption für XA-Transaktionen

Der Treiber hat neue Timeoutoptionen für das vorhandene automatische Rollback nicht vorbereiteter Transaktionen. Weitere Informationen finden Sie unter [Grundlegendes zu XA-Transaktionen](understanding-xa-transactions.md).

### <a name="new-kerberos-principal-connection-property"></a>Neue Kerberos-Prinzipalverbindungseigenschaft

Der Treiber verwendet neue Verbindungseigenschaft, um mehr Flexibilität für Kerberos-Verbindungen zu ermöglichen. Details finden Sie unter [Verwenden der integrierten Kerberos-Authentifizierung für Verbindungen mit SQL Server](using-kerberos-integrated-authentication-to-connect-to-sql-server.md).

## <a name="41"></a>4,1

Versionsnummer: 4.1.8112  
Veröffentlichung: 12. Dezember 2014

### <a name="support-for-jdk-7"></a>Unterstützung für JDK 7

Der Treiber unterstützt jetzt die JDK-Version 7.0 zusätzlich zu JDK 6.0 und 5.0.

## <a name="itanium-not-supported-for-jdbc-driver-applications"></a>Keine Itanium-Unterstützung für JDBC Driver-Anwendungen

Die Ausführung des Microsoft JDBC-Treibers für SQL Server-Anwendungen auf einem Itanium-Computer wird nicht unterstützt.

## <a name="see-also"></a>Weitere Informationen

[Overview of the JDBC Driver (Übersicht über den JDBC-Treiber)](overview-of-the-jdbc-driver.md)
