---
description: Entwickeln von Anwendungen mithilfe von Always Encrypted mit Secure Enclaves
title: Entwickeln von Anwendungen mithilfe von Always Encrypted mit Secure Enclaves | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
dev_langs:
- CSharp
ms.assetid: 9595eb66-284c-4474-828f-8961a05ce989
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 14bfbdd1a9c6bd176c89513674c5680388ef1452
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838923"
---
# <a name="develop-applications-using-always-encrypted-with-secure-enclaves"></a>Entwickeln von Anwendungen mithilfe von Always Encrypted mit Secure Enclaves
[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted mit Secure Enclaves](always-encrypted-enclaves.md) ist eine Erweiterung von [Always Encrypted](always-encrypted-database-engine.md), die umfangreichere Funktionen für Anwendungsabfragen in verschlüsselten vertraulichen Datenbankspalten bietet. Dabei kommen Secure Enclave-Technologien zum Einsatz, mit deren Hilfe der Abfrageexecutor in [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] Berechnungen mit verschlüsselten Spalten an eine Secure Enclave im [!INCLUDE[ssde-md](../../../includes/ssde-md.md)]-Prozess delegieren kann.

## <a name="prerequisites"></a>Voraussetzungen

- Die [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Instanz oder die Datenbank und der Server in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] müssen ordnungsgemäß für die Unterstützung von Enclaves und Nachweisen konfiguriert werden. Weitere Informationen finden Sie unter [Einrichten von Secure Enclaves und Nachweisen](configure-always-encrypted-enclaves.md#set-up-the-secure-enclave-and-attestation).
- Sie benötigen für Ihre Umgebung eine Nachweis-URL vom Dienstadministrator, der für Nachweise zuständig ist.

  - Wenn Sie [!INCLUDE[ssnoversion-md](../../../includes/ssnoversion-md.md)] und den Host-Überwachungsdienst verwenden, finden Sie weitere Informationen unter [Ermitteln und Freigeben der HGS-Nachweis-URL](../../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Wenn Sie [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] und Microsoft Azure Attestation verwenden, finden Sie weitere Informationen unter [Ermitteln der Nachweis-URL für Ihre Nachweisrichtlinie](./always-encrypted-enclaves.md?view=sql-server-ver15#secure-enclave-attestation).

- Die Anwendung muss eine Version des SQL-Clienttreibers verwenden, die Secure Enclaves unterstützt. Weitere Informationen finden Sie in den folgenden Abschnitten.

- Sie müssen ein Nachweisprotokoll und eine Nachweis-URL für eine Datenverbindung konfigurieren. Die Details zum Konfigurieren des Nachweisprotokolls und der Nachweis-URL sind vom verwendeten Clienttreiber abhängig.

## <a name="client-drivers-for-always-encrypted-with-secure-enclaves"></a>Clienttreiber für Always Encrypted mit Secure Enclaves

Zum Entwickeln von Anwendungen mit Always Encrypted mit Secure Enclaves benötigen Sie eine Version des SQL-Clienttreibers, die Secure Enclaves unterstützt. Der Clienttreiber spielt die folgende wichtige Rolle:

- Bevor Sie eine Abfrage, für die eine Secure Enclave verwendet wird, zur Ausführung an [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] senden, initiiert der Treiber einen Enclave-Nachweis, um zu prüfen, ob die Secure Enclave vertrauenswürdig ist und ohne Risiko zur Verarbeitung vertraulicher Daten verwendet werden kann. Weitere Informationen zum Nachweis finden Sie unter [Secure Enclave-Nachweis](always-encrypted-enclaves.md#secure-enclave-attestation).
- Wenn der Nachweis erfolgreich geführt werden kann, richtet der Clienttreiber eine sichere Sitzung mit der Enclave ein, indem er ein gemeinsames Geheimnis aushandelt.
- Der Treiber verwendet das gemeinsame Geheimnis zur Verschlüsselung der Spaltenverschlüsselungsschlüssel, die von der Enclave zum Verarbeiten der Abfrage benötigt werden, und sendet die Schlüssel an [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], von wo aus sie zur Entschlüsselung an die Secure Enclave weitergeleitet werden. 
- Abschließend sendet der Treiber die Abfrage zur Ausführung, wodurch in der Secure Enclave Berechnungen ausgelöst werden.

## <a name="next-steps"></a>Nächste Schritte

Always Encrypted mit Secure Enclaves wird von folgenden Clienttreibern unterstützt:

- Microsoft .NET-Datenanbieter für SQL Server in .NET Framework 4.6 oder höher und .NET Core 2.1 oder höher 
    - Weitere Informationen finden Sie unter [Verwenden von Always Encrypted mit dem Microsoft .NET-Datenanbieter für SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md).
    - Ein Schritt-für-Schritt-Tutorial finden Sie unter [Tutorial: Entwickeln einer .NET-Anwendung mithilfe von Always Encrypted mit Secure Enclaves](../../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md).
    - Weitere Informationen finden Sie unter [Beispiel zur Veranschaulichung der Verwendung von Always Encrypted mit Secure Enclaves mit einem Azure Key Vault-Anbieter](../../../connect/ado-net/sql/azure-key-vault-enclave-example.md).
- Microsoft ODBC Driver for SQL Server, Version 17.4 oder höher. 
    - Weitere Informationen finden Sie unter [Using Always Encrypted with the ODBC Driver (Verwenden von Always Encrypted mit dem ODBC-Treiber)](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md). 
    - Informationen zum Aktivieren von Enclave-Berechnungen für eine Datenbankverbindung mithilfe von ODBC finden Sie im Abschnitt [Aktivieren von Always Encrypted mit Secure Enclaves](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-with-secure-enclaves).
- Microsoft JDBC-Treiber für SQL Server, Version 8.2 oder höher
    - Weitere Informationen finden Sie unter [Verwenden von Always Encrypted mit Secure Enclaves mit dem JDBC-Treiber](../../../connect/jdbc/using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver.md).
- .NET Framework-Datenanbieter für SQL Server in .NET Framework 4.7.2 oder höher. 
    - Weitere Informationen finden Sie unter [Verwenden von Always Encrypted mit dem .NET Framework-Datenanbieter für SQL Server](../../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)
    - Ein Schritt-für-Schritt-Tutorial finden Sie unter [Tutorial: Entwickeln einer .NET Framework-Anwendung mithilfe von Always Encrypted mit Secure Enclaves](../tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)

## <a name="see-also"></a>Weitere Informationen

- [Behandeln von häufig auftretenden Problemen bei Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-troubleshooting.md)