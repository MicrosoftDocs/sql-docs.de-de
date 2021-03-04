---
description: Verwenden von Always Encrypted mit Secure Enclaves mit dem JDBC-Treiber
title: Verwenden von Always Encrypted mit Secure Enclaves mit dem JDBC-Treiber | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 271c0438-8af1-45e5-b96a-4b1cabe32707
author: reneye
ms.author: v-reye
ms.openlocfilehash: 46e812a4234098e49a272be503e52f34d4c78733
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837537"
---
# <a name="using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver"></a>Verwenden von Always Encrypted mit Secure Enclaves mit dem JDBC-Treiber
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Dieser Artikel enthält Informationen zum Entwickeln von Java-Anwendungen mithilfe von [Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md) und des Microsoft JDBC-Treibers 8.2 (oder höher) für SQL Server.

Secure Enclaves sind eine Erweiterung des bereits vorhandenen [Always Encrypted-Features](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Mit Secure Enclaves sollen Einschränkungen bei der Arbeit mit Always Encrypted-Daten beseitigt werden. Bisher konnten Benutzer für Always Encrypted-Daten nur Gleichheitsvergleiche durchführen und mussten die Daten abrufen und entschlüsseln, um andere Vorgänge durchzuführen. Secure Enclaves beseitigen diese Einschränkungen, indem Berechnungen für Klartextdaten innerhalb einer Secure Enclave auf Serverseite zugelassen werden. Eine Secure Enclave ist eine geschützte Arbeitsspeicherregion im SQL Server-Prozess und fungiert als vertrauenswürdige Ausführungsumgebung für die Verarbeitung vertraulicher Daten in der SQL Server-Engine. Eine Secure Enclave wird dem Rest von SQL Server und anderen Prozessen auf dem Hostcomputer als Blackbox angezeigt. Es gibt keine Möglichkeit, Daten oder Code innerhalb der Enclave von außen anzuzeigen, auch nicht mit einem Debugger.

## <a name="prerequisites"></a>Voraussetzungen
- Stellen Sie sicher, dass der Microsoft JDBC-Treiber 8.2 (oder höher) für SQL Server auf dem Entwicklungscomputer installiert ist.
- Überprüfen Sie, ob sich die Umgebungsabhängigkeiten, z. B. DLLs oder Keystores, jeweils unter dem richtigen Pfad befinden. Always Encrypted mit Secure Enclaves ist ein Add-On für das bereits vorhandene [Always Encrypted](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md)-Feature und verfügt über ähnliche Voraussetzungen.

> [!Note]
> Wenn Sie eine ältere Version von JDK 8 verwenden, müssen Sie möglicherweise die Richtliniendateien „Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction“ herunterladen und installieren. Achten Sie darauf, die in der ZIP-Datei enthaltene Infodatei zu lesen, um Installationsanweisungen und relevante Details zu möglichen Export-/Importproblemen zu erhalten.  
>
> Diese Richtliniendateien können unter [Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy Files 8 Download](https://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html) heruntergeladen werden.

## <a name="setting-up-secure-enclaves"></a>Einrichten von Secure Enclaves
Arbeiten Sie das [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in SQL Server](../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) oder das [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank](/azure/azure-sql/database/always-encrypted-enclaves-getting-started) durch, um mit der Verwendung von Secure Enclaves zu beginnen. Weitere Informationen finden Sie unter [Always Encrypted mit Secure Enclaves](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

## <a name="connection-string-properties"></a>Verbindungszeichenfolgen-Eigenschaften

Um Enclaveberechnungen für eine Datenbankverbindung zu ermöglichen, müssen Sie zusätzlich zur Aktivierung von Always Encrypted die folgenden Schlüsselwörter für die Verbindungszeichenfolge festlegen.

- **enclaveAttestationProtocol**: gibt ein Nachweisprotokoll an. 
  - Wenn Sie [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] und den Host-Überwachungsdienst (Host Guardian Service, HGS) verwenden, sollte der Wert dieses Schlüsselworts `HGS` lauten.
  - Wenn Sie [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und Microsoft Azure Attestation verwenden, sollte der Wert dieses Schlüsselworts `AAS` lauten.

- **enclaveAttestationUrl**: gibt eine Nachweis-URL (einen Endpunkt für den Nachweisdienst) an. Sie benötigen für Ihre Umgebung eine Nachweis-URL von dem Dienstadministrator, der für Nachweise zuständig ist.
  - Wenn Sie [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] und den Host-Überwachungsdienst verwenden, finden Sie weitere Informationen unter [Ermitteln und Freigeben der HGS-Nachweis-URL](../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Wenn Sie [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und Microsoft Azure Attestation verwenden, finden Sie weitere Informationen unter [Ermitteln der Nachweis-URL für Ihre Nachweisrichtlinie](../../relational-databases/security/encryption/always-encrypted-enclaves.md?view=sql-server-ver15#secure-enclave-attestation).

Benutzer müssen **columnEncryptionSetting** aktivieren und **beide** der oben genannten Verbindungszeichenfolgen-Eigenschaften richtig festlegen, um Always Encrypted mit Secure Enclaves aus dem [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] zu aktivieren.

## <a name="working-with-secure-enclaves"></a>Arbeiten mit Secure Enclaves
Wenn die Verbindungseigenschaften der Enclaves richtig festgelegt wurden, arbeitet das Feature transparent. Der Treiber stellt automatisch fest, ob für die Abfrage Secure Enclaves erforderlich sind. Enclaveberechnungen werden beispielsweise bei den folgenden Abfragen ausgelöst. Um sich über das Setup von Datenbank und Tabelle zu informieren, lesen Sie das [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in SQL Server](../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) oder das [Tutorial: Erste Schritte mit Always Encrypted mit Secure Enclaves in Azure SQL-Datenbank](/azure/azure-sql/database/always-encrypted-enclaves-getting-started).


Umfangreiche Abfragen lösen Enclave-Berechnungen aus:
```java
private static final String URL = "jdbc:sqlserver://<server>:<port>;user=<username>;password=<password>;databaseName=ContosoHR;columnEncryptionSetting=enabled;enclaveAttestationUrl=<attestation-url>;enclaveAttestationProtocol=<attestation-protocol>;";
try (Connection c = DriverManager.getConnection(URL)) {
    try (PreparedStatement p = c.prepareStatement("SELECT * FROM Employees WHERE SSN LIKE ?")) {
        p.setString(1, "%6818");
        try (ResultSet rs = p.executeQuery()) {
            while (rs.next()) {
                // Do work with data
            }
        }
    }
    
    try (PreparedStatement p = c.prepareStatement("SELECT * FROM Employees WHERE SALARY > ?")) {
        ((SQLServerPreparedStatement) p).setMoney(1, new BigDecimal(0));
        try (ResultSet rs = p.executeQuery()) {
            while (rs.next()) {
                // Do work with data
            }
        }
    }
}
```

Das Verschlüsseln von Spalten löst ebenfalls Enclave-Berechnungen aus:
```java
private static final String URL = "jdbc:sqlserver://<server>:<port>;user=<username>;password=<password>;databaseName=ContosoHR;columnEncryptionSetting=enabled;enclaveAttestationUrl=<attestation-url>;enclaveAttestationProtocol=<attestation-protocol>;";
try (Connection c = DriverManager.getConnection(URL);Statement s = c.createStatement()) {
    s.executeUpdate("ALTER TABLE Employees ALTER COLUMN SSN CHAR(11) NULL WITH (ONLINE = ON)");
}
```

## <a name="java-8-users"></a>Benutzer von Java 8
Für dieses Feature ist der Signaturalgorithmus RSASSA-PSA erforderlich. Dieser Algorithmus wurde in JDK 11 hinzugefügt, aber nicht auf JDK 8 zurückportiert. Benutzer, die dieses Feature mit der Version des [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] verwenden möchten, müssen entweder einen eigenen Anbieter laden, der den Signaturalgorithmus RSASSA-PSA unterstützt, oder die optionale BouncyCastleProvider-Abhängigkeit integrieren. Diese Abhängigkeit wird später entfernt, wenn der Signaturalgorithmus auf JDK 8 zurückportiert wird oder der Support-Lebenszyklus von JDK 8 endet.

## <a name="see-also"></a>Weitere Informationen
[Verwenden von Always Encrypted mit dem JDBC-Treiber](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md)