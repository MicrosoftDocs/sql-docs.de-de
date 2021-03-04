---
title: Behandeln von häufig auftretenden Problemen bei Always Encrypted mit Secure Enclaves
description: Behandeln von häufig auftretenden Problemen bei Always Encrypted mit Secure Enclaves
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: how-to
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: dc50006d17228bf3b5ec03c2a672ed113260bf3b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839260"
---
# <a name="troubleshoot-common-issues-for-always-encrypted-with-secure-enclaves"></a>Behandeln von häufig auftretenden Problemen bei Always Encrypted mit Secure Enclaves

In diesem Artikel wird beschrieben, wie Sie häufige Probleme identifizieren und beheben, die bei der Ausführung von Transact-SQL-Anweisungen (T-SQL) mit [Always Encrypted mit Secure Enclaves](always-encrypted-enclaves.md) auftreten können.

Informationen zum Ausführen von Abfragen mit Secure Enclaves finden Sie unter [Ausführen von Transact-SQL-Anweisungen mit Secure Enclaves](always-encrypted-enclaves-query-columns.md).

## <a name="database-connection-errors"></a>Datenbankverbindungsfehler

Zum Ausführen von Anweisungen mit Secure Enclaves müssen Sie Always Encrypted aktivieren und eine Nachweis-URL für die Datenbankverbindung angeben wie unter [Voraussetzungen für das Ausführen von Anweisungen mit Secure Enclaves](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves) erläutert. Bei der Verbindung tritt jedoch ein Fehler auf, wenn Sie eine Nachweis-URL angeben, aber die Datenbank in [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] oder die [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Zielinstanz unterstützt Secure Enclaves nicht oder ist falsch konfiguriert.

- Wenn Sie [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] verwenden, überprüfen Sie, ob die Datenbank die Hardwarekonfiguration der [DC-Serie](/azure/azure-sql/database/service-tiers-vcore?tabs=azure-portal#dc-series) verwendet. Weitere Informationen finden Sie unter [Aktivieren von Intel SGX für die Azure SQL-Datenbank](/azure/azure-sql/database/always-encrypted-enclaves-enable-sgx).
- Wenn Sie [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] verwenden, überprüfen Sie, ob Secure Enclaves für die Instanz richtig konfiguriert ist. Weitere Informationen finden Sie unter [Konfigurieren von Secure Enclaves in SQL Server](always-encrypted-enclaves-configure-enclave-type.md).

## <a name="attestation-errors-when-using-microsoft-azure-attestation"></a>Nachweisfehler bei der Verwendung von Microsoft Azure Attestation

> [!NOTE]
> Dieser Abschnitt gilt nur für [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

Bevor ein Clienttreiber eine T-SQL-Anweisung zur Ausführung an den logischen Azure SQL-Server sendet, löst der Treiber mithilfe von Microsoft Azure Attestation den folgenden Enclave-Nachweisworkflow aus.

1. Der Clienttreiber übergibt die in der Datenbankverbindung angegebene Nachweis-URL an den logischen Azure SQL-Server.
2. Der logische Azure SQL-Server sammelt die Beweise für die Enclave, die Hostingumgebung und den Code, der innerhalb der Enclave ausgeführt wird. Der Server sendet dann eine Nachweisanforderung an den Nachweisanbieter, auf den in der Nachweis-URL verwiesen wird.
3. Der Nachweisanbieter überprüft den Beweis anhand der konfigurierten Richtlinie und übergibt ein Nachweistoken an den logischen Azure SQL-Server. Der Nachweisanbieter signiert das Nachweistoken mit seinem privaten Schlüssel.
4. Der logische Azure SQL-Server sendet das Nachweistoken an den Clienttreiber.
5. Der Client kontaktiert den Nachweisanbieter an der angegebenen Nachweis-URL, um seinen öffentlichen Schlüssel abzurufen. Dieser überprüft die Signatur im Nachweistoken.

Aufgrund von Fehlkonfigurationen können in verschiedenen Schritten des obigen Workflows Fehler auftreten. Häufige Nachweisfehler, die zugehörigen Grundursachen und die empfohlenen Schritte zur Problembehandlung sind hier aufgeführt:

- Der logische Azure SQL-Server kann keine Verbindung mit dem Nachweisanbieter in Azure Attestation (Schritt 2 des obigen Workflows) herstellen, der in der Nachweis-URL angegeben ist. Mögliche Ursachen sind:
  - Die Nachweis-URL ist falsch oder unvollständig. Weitere Informationen finden Sie unter [Ermitteln der Nachweis-URL für die Nachweisrichtlinie](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation#determine-the-attestation-url-for-your-attestation-policy).
  - Der Nachweisanbieter wurde versehentlich gelöscht.
  - Die Firewall wurde für den Nachweisanbieter konfiguriert, lässt aber keinen Zugriff auf Microsoft-Dienste zu.
  - Ein zeitweiliger Netzwerkfehler bewirkt, dass der Nachweisanbieter nicht verfügbar ist.
- Der logische Azure SQL-Server darf keine Nachweisanforderungen an den Nachweisanbieter senden. Stellen Sie sicher, dass der Administrator Ihres Nachweisanbieters den Datenbankserver zur Nachweisrolle „Leser“ hinzugefügt hat. Weitere Informationen finden Sie unter [Gewähren des Zugriffs auf den Nachweisanbieter für den Azure SQL-Datenbank-Server](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation#grant-your-azure-sql-database-server-access-to-your-attestation-provider).
- Die Überprüfung der Nachweisrichtlinie schlägt fehl (in Schritt 3 des obigen Workflows).
  - Eine falsche Nachweisrichtlinie ist wahrscheinlich die Grundursache. Stellen Sie sicher, dass Sie die von Microsoft empfohlene Richtlinie verwenden. Weitere Informationen finden Sie unter [Erstellen und Konfigurieren eines Nachweisanbieters](/azure/azure-sql/database/always-encrypted-enclaves-configure-attestation#create-and-configure-an-attestation-provider).
  - Die Überprüfung der Richtlinie kann auch aufgrund einer Sicherheitsverletzung, die die serverseitige Enclave kompromittiert, fehlschlagen.
- Die Clientanwendung kann sich nicht mit dem Nachweisanbieter verbinden und den öffentlichen Signaturschlüssel abrufen (in Schritt 5). Mögliche Ursachen sind:
  - Die Konfiguration von Firewalls zwischen der Anwendung und dem Nachweisanbieter kann die Verbindungen blockieren. Vergewissern Sie sich, dass Sie eine Verbindung mit dem OpenID-Endpunkt Ihres Nachweisanbieters herstellen können, um Probleme mit der blockierten Verbindung zu beheben. Verwenden Sie beispielsweise einen Webbrowser auf dem Computer, der Ihre Anwendung hostet, um zu überprüfen, ob Sie eine Verbindung mit dem OpenID-Endpunkt herstellen können. Weitere Informationen finden Sie unter [Metadatenkonfiguration – Get](/rest/api/attestation/metadataconfiguration/get).

## <a name="attestation-errors-when-using-host-guardian-service"></a>Nachweisfehler bei der Verwendung des Host-Überwachungsdienstes

> [!NOTE]
> Dieser Abschnitt gilt nur für [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)].

Bevor ein Clienttreiber eine T-SQL-Anweisung zur Ausführung an [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] sendet, löst der Treiber mithilfe des Host-Überwachungsdienstes den folgenden Enclave-Nachweisworkflow aus.

1. Der Clienttreiber ruft [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] auf, um den Nachweis einzuleiten.
2. [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] sammelt die Beweise für die Enclave, die Hostingumgebung und den Code, der innerhalb der Enclave ausgeführt wird. [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] fordert ein Integritätszertifikat von der Host-Überwachungsdienstinstanz an, mit der der Computer, der [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] hostet, registriert wurde. Weitere Informationen finden Sie unter [Registrieren des Computers beim Host-Überwachungsdienst](always-encrypted-enclaves-host-guardian-service-register.md).
3. Der Host-Überwachungsdienst überprüft den Beweis und übergibt das Integritätszertifikat an [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Der Host-Überwachungsdienst signiert das Integritätszertifikat mit dem privaten Schlüssel.
4. [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] sendet das Integritätszertifikat an den Clienttreiber.
5. Der Clienttreiber kontaktiert den Host-Überwachungsdienst bei der Nachweis-URL, die für die Datenbankverbindung angegeben ist, um den öffentlichen Schlüssel des Host-Überwachungsdienstes abzurufen. Der Clienttreiber überprüft die Signatur im Integritätszertifikat.

Aufgrund von Fehlkonfigurationen können in verschiedenen Schritten des obigen Workflows Fehler auftreten. Häufige Nachweisfehler, die zugehörigen Grundursachen und die empfohlenen Schritte zur Problembehandlung werden nachfolgend aufgeführt:

- [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] kann aufgrund eines vorübergehenden Netzwerkfehlers nicht mit dem Host-Überwachungsdienst (Schritt 2 des obigen Workflows) verbunden werden. Der Administrator des [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computers sollte überprüfen, dass der Computer sich mit dem Computer des Host-Überwachungsdienstes verbinden kann, um die Verbindungsprobleme zu beheben.
- Die Validierung in Schritt 3 schlägt fehl. So beheben Sie Validierungsprobleme:
  - Der [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computeradministrator sollte mit dem Administrator der Clientanwendung zusammenarbeiten, um zu überprüfen, ob der [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer bei derselben Host-Überwachungsdienstinstanz wie die Instanz, auf die in der Nachweis-URL auf der Clientseite verwiesen wird, registriert ist.
  - Der [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computeradministrator sollte prüfen, ob der [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]-Computer erfolgreich beglaubigen kann, indem er die Anweisungen unter [Schritt 5: Bestätigen, dass der Host erfolgreich beglaubigen kann](always-encrypted-enclaves-host-guardian-service-register.md#step-5-confirm-the-host-can-attest-successfully) befolgt.
- Ihre Clientanwendung kann keine Verbindung mit dem Host-Überwachungsdienst herstellen und den öffentlichen Signaturschlüssel nicht abrufen (in Schritt 5). Die wahrscheinlichste Ursache ist Folgende:
  - Die Konfiguration einer der Firewalls zwischen Ihrer Anwendung und dem Nachweisanbieter kann die Verbindungen blockieren. Vergewissern Sie sich, dass der Computer, der die App hostet, eine Verbindung mit dem Computer des Host-Überwachungsdienstes hat.

## <a name="in-place-encryption-errors"></a>Direkte Verschlüsselungsfehler

In diesem Abschnitt werden häufige Fehler aufgeführt, die bei der Verwendung von `ALTER TABLE`/`ALTER COLUMN` für die direkte Verschlüsselung auftreten können (zusätzlich zu den in den anderen Abschnitten beschriebenen Nachweisfehlern). Weitere Informationen hierzu finden Sie unter [Konfigurieren einer direkten Spaltenverschlüsselung mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-configure-encryption.md).

- Der Spaltenverschlüsselungsschlüssel, den Sie zum Verschlüsseln, Entschlüsseln oder erneuten Verschlüsseln von Daten verwenden möchten, ist kein Enclave-fähiger Schlüssel. Weitere Informationen zu den Voraussetzungen für die direkte Verschlüsselung finden Sie unter [Voraussetzungen](always-encrypted-enclaves-configure-encryption.md#prerequisites). Weitere Informationen zum Bereitstellen von Enclave-fähigen Schlüsseln finden Sie unter [Bereitstellen Enclave-fähiger Schlüssel](always-encrypted-enclaves-provision-keys.md).
- Sie haben Always Encrypted und Enclave-Berechnungen für die Datenbankverbindung nicht aktiviert. Weitere Informationen finden Sie unter [Voraussetzungen für das Ausführen von Anweisungen mit Secure Enclaves](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves)
- Die `ALTER TABLE`/`ALTER COLUMN`-Anweisung löst einen kryptographischen Vorgang aus und ändert den Datentyp der Spalte oder legt eine Sortierung mit einer anderen Codepage fest, die von der aktuellen Sortierungscodepage abweicht. Das Kombinieren kryptografischer Vorgänge mit Änderungen des Datentyps oder der Sortierungsseiten ist nicht zulässig. Verwenden Sie separate Anweisungen, um das Problem zu beheben: Eine Anweisung zum Ändern des Datentyps oder der Sortierungscodepage und eine andere Anweisung für die direkte Verschlüsselung.

## <a name="errors-when-running-confidential-dml-queries-using-secure-enclaves"></a>Fehler beim Ausführen von vertraulichen DML-Abfragen mit Secure Enclaves

In diesem Abschnitt werden häufige Fehler aufgeführt, die auftreten können, wenn Sie vertrauliche DML-Abfragen mit Secure Enclaves ausführen (zusätzlich zu den in den anderen Abschnitten beschriebenen Nachweisfehlern).

- Der für die Spalte konfigurierte Spaltenverschlüsselungsschlüssel, den Sie abfragen, ist kein Enclave-fähiger Schlüssel.
- Sie haben Always Encrypted und Enclave-Berechnungen für die Datenbankverbindung nicht aktiviert. Weitere Informationen finden Sie unter [Voraussetzungen für das Ausführen von Anweisungen mit Secure Enclaves](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves).
- Die Spalte, die Sie abfragen, verwendet die deterministische Verschlüsselung. Vertrauliche DML-Abfragen mit Secure Enclaves werden bei der deterministischen Verschlüsselung nicht unterstützt. Weitere Informationen zum Ändern des Verschlüsselungstyps in „Zufällig“ finden Sie unter [Aktivieren von Always Encrypted mit Secure Enclaves für vorhandene verschlüsselte Spalten](always-encrypted-enclaves-enable-for-encrypted-columns.md).
- Die Zeichenfolgenspalte, die Sie abfragen, verwendet eine andere Sortierung als die BIN2- oder UTF-8-Sortierung. Ändern Sie die Sortierung in BIN2 oder UTF-8. Weitere Informationen finden Sie unter [DML-Anweisungen mit Secure Enclaves](always-encrypted-enclaves-query-columns.md#dml-statements-using-secure-enclaves).
- Die Abfrage löst einen nicht unterstützten Vorgang aus. Eine Liste der Vorgänge, die innerhalb der Enclaves unterstützt werden, finden Sie unter [DML-Anweisungen mit Secure Enclaves](always-encrypted-enclaves-query-columns.md#dml-statements-using-secure-enclaves).
## <a name="next-steps"></a>Nächste Schritte

- [Entwickeln von Anwendungen mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>Weitere Informationen

- [Ausführen von Transact-SQL-Anweisungen mit Secure Enclaves](always-encrypted-enclaves-query-columns.md)
- [Konfigurieren einer direkten Spaltenverschlüsselung mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-configure-encryption.md)
- [Erstellen und Verwenden von Indizes in Spalten mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-create-use-indexes.md)