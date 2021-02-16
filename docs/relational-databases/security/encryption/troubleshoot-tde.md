---
title: Häufige Fehler bei von kundenseitig verwalteten Schlüsseln in Azure Key Vault
description: Hier erfahren Sie, wie Sie Probleme und gängige Fehler mit TDE (Transparent Data Encryption) und kundenseitig verwalteten Schlüsseln in Azure Key Vault identifizieren und lösen.
ms.custom: seo-lt-2019
helpviewer_keywords:
- troublshooting, tde akv
- tde akv configuration, troubleshooting
- tde troubleshooting
author: jaszymas
ms.prod: sql
ms.technology: security
ms.reviewer: vanto
ms.topic: conceptual
ms.date: 11/06/2019
ms.author: jaszymas
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 033346cf1d00d1f2acbd6b8de0796027c80814a7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345377"
---
# <a name="common-errors-for-transparent-data-encryption-with-customer-managed-keys-in-azure-key-vault"></a>Häufige Fehler bei Transparent Data Encryption (TDE) mit vom Kunden verwalteten Schlüsseln in Azure Key Vault

[!INCLUDE[asdb-asdbmi-asa](../../../includes/applies-to-version/asdb-asdbmi-asa.md)]

In diesem Artikel wird beschrieben, wie Sie Probleme mit dem Azure Key Vault-Schlüsselzugriff identifizieren und beheben, die dazu geführt haben, dass eine für die Verwendung von [Transparent Data Encryption (TDE) mit kundenseitig verwalteten Schlüsseln in Azure Key Vault](/azure/sql-database/transparent-data-encryption-byok-azure-sql) konfigurierte Datenbank nicht mehr zugänglich ist.

## <a name="introduction"></a>Einführung
Wenn TDE zur Verwendung eines kundenseitig verwalteten Schlüssels in Azure Key Vault konfiguriert ist, muss fortlaufender Zugriff auf diesen TDE-Schutz gewährleistet sein, damit die Datenbank online bleibt.  Wenn die logische SQL Server-Instanz den Zugriff auf den kundenseitig verwalteten TDE-Schutz in Azure Key Vault verliert, beginnt eine Datenbank alle Verbindungen mit der entsprechenden Fehlermeldung abzulehnen. Zudem wird im Azure-Portal der Status in *Nicht zugänglich* geändert.

Wenn das zugrunde liegende Azure Key Vault-Schlüsselzugriffsproblem innerhalb der ersten 8 Stunden behoben wird, wird die Datenbank automatisch repariert und wieder online geschaltet. Dies bedeutet, dass bei zeitweiligen und vorübergehenden Netzwerkausfällen keine Benutzeraktion erforderlich ist und die Datenbank automatisch wieder online geschaltet wird. In den meisten Fällen ist jedoch ein Benutzereingriff erforderlich, um das zugrunde liegende Azure Key Vault-Schlüsselzugriffsproblem zu lösen. 

Wenn eine unzugängliche Datenbank nicht länger benötigt wird, kann sie sofort gelöscht werden, um Kosten zu vermeiden. Alle weiteren Aktionen für die Datenbank sind erst dann erlaubt, wenn der Zugriff auf den Azure Key Vault-Schlüssel wiederhergestellt wurde und die Datenbank wieder online ist. Das Ändern der TDE-Option von kundenseitig verwalteten Schlüsseln zu dienstseitig verwalteten Schlüsseln ist ebenfalls nicht möglich, wenn eine mit kundenseitig verwalteten Schlüsseln verschlüsselte Datenbank nicht zugänglich ist. Dies ist erforderlich, um die Daten vor einem nicht autorisierten Zugriff zu schützen, wenn die Berechtigungen für den TDE-Schutz widerrufen wurden. 

Wenn eine Datenbank länger als 8 Stunden nicht zugänglich ist, ist eine automatische Reparatur nicht mehr möglich. Wenn der erforderliche Azure Key Vault-Schlüsselzugriff erst danach wiederhergestellt wurde, müssen Sie den Zugriff auf den Schlüssel manuell noch mal validieren, um die Datenbank wieder online zu schalten. Das Onlineschalten der Datenbank kann in diesem Fall je nach Größe der Datenbank sehr lange dauern. Sobald die Datenbank wieder online ist, gehen zuvor konfigurierte Einstellungen wie die [Failovergruppe](/azure/sql-database/sql-database-auto-failover-group), der PITR-Verlauf und Tags **verloren**. Daher wird empfohlen, ein Benachrichtigungssystem mit [Aktionsgruppen](/azure/azure-monitor/platform/action-groups) zu implementieren, mit dem zugrunde liegende Probleme mit dem Azure Key Vault-Schlüsselzugriff erkannt und gelöst werden können. 

## <a name="common-errors-causing-databases-to-become-inaccessible"></a>Häufige Fehler, die zu unzugänglichen Datenbanken führen

Die meisten Probleme, die auftreten, wenn Sie TDE mit Key Vault verwenden, werden durch einen der folgenden Konfigurationsfehler verursacht:

### <a name="the-key-vault-is-unavailable-or-doesnt-exist"></a>Der Schlüsseltresor ist nicht verfügbar oder nicht vorhanden.

- Der Schlüsseltresor wurde versehentlich gelöscht.
- Die Firewall wurde für Azure Key Vault konfiguriert, ohne den Zugriff auf Microsoft-Dienste zuzulassen.
- Ein zeitweiliger Netzwerkfehler führt dazu, dass der Schlüsseltresor nicht verfügbar ist.

### <a name="no-permissions-to-access-the-key-vault-or-the-key-doesnt-exist"></a>Es liegen keine Berechtigungen für den Zugriff auf den Schlüsseltresor vor, oder der Schlüssel ist nicht vorhanden.

- Der Schlüssel wurde versehentlich gelöscht oder deaktiviert, oder der Schlüssel ist abgelaufen.
- Der AppId der logischen SQL Server-Instanz wurde versehentlich gelöscht.
- Die logische SQL Server-Instanz wurde in ein anderes Abonnement verschoben. Es muss eine neue AppId erstellt werden, wenn der logische Server in ein anderes Abonnement verschoben wird.
- Die der AppId für die Schlüssel erteilten Berechtigungen sind nicht ausreichend (sie beinhalten nicht Get, Wrap und Unwrap).
- Die Berechtigungen für die AppId der logischen SQL Server-Instanz wurden widerrufen.

## <a name="identify-and-resolve-common-errors"></a>Identifizieren und Beheben von häufigen Fehlern

Im diesem Abschnitt werden die Schritte zur Problembehandlung für die häufigsten Fehler aufgelistet.

### <a name="missing-server-identity"></a>Serveridentität fehlt

**Fehlermeldung**

_401 AzureKeyVaultNoServerIdentity: Die Serveridentität ist auf dem Server nicht korrekt konfiguriert. Wenden Sie sich an den Support._

**Erkennung**

Verwenden Sie das folgende Cmdlet oder den folgenden Befehl, um sicherzustellen, dass der logischen SQL Server-Instanz eine Identität zugewiesen wurde:

- Azure PowerShell: [Get-AzureRMSqlServer](/powershell/module/AzureRM.Sql/Get-AzureRmSqlServer) 

- Azure CLI: [az-sql-server-show](/cli/azure/sql/server#az-sql-server-show)

**Abhilfe**

Verwenden Sie das folgende Cmdlet oder den folgenden Befehl, um eine Azure AD-Identität (eine AppId) für die logische SQL Server-Instanz zu konfigurieren:

- Azure PowerShell: [Set-AzureRmSqlServer](/powershell/module/azurerm.sql/set-azurermsqlserver) mit der Option `-AssignIdentity`.

- Azure CLI: [az sql server update](/cli/azure/sql/server#az-sql-server-update) mit der Option `--assign_identity`.

Navigieren Sie im Azure-Portal zum Schlüsseltresor und anschließend zu **Zugriffsrichtlinien**. Führen Sie die folgenden Schritte aus: 

 1. Verwenden Sie die Schaltfläche **Neues Element hinzufügen**, um die im vorherigen Schritt erstellte AppId für den Server hinzuzufügen. 
 1. Weisen Sie die folgenden Schlüsselberechtigungen zu: „Get (Abrufen)“, „Wrap (Packen)“ und „Unwrap (Entpacken)“. 

Weitere Informationen finden Sie unter [Zuweisen einer Azure AD-Identität zu einem Server](/azure/sql-database/transparent-data-encryption-byok-azure-sql-configure#assign-an-azure-ad-identity-to-your-server).

> [!IMPORTANT]
> Wurde die logische SQL Server-Instanz nach der TDE-Erstkonfiguration mit Key Vault in einen neuen Mandanten verschoben, wiederholen Sie den Schritt zum Konfigurieren der Azure AD-Identität, um eine neue AppId zu erstellen. Fügen Sie die AppId dann dem Schlüsseltresor hinzu, und weisen Sie dem Schlüssel die richtigen Berechtigungen zu. 
>

### <a name="missing-key-vault"></a>Schlüsseltresor fehlt

**Fehlermeldung**

_503 AzureKeyVaultConnectionFailed: Der Vorgang konnte auf dem Server nicht ausgeführt werden, weil beim Versuch der Verbindungsherstellung mit Azure Key Vault Fehler aufgetreten sind._

**Erkennung**

So ermitteln Sie den Schlüssel-URI und den Schlüsseltresor:

1. Verwenden Sie das folgende Cmdlet oder den folgenden Befehl, um den Schlüssel-URI einer bestimmten logischen SQL Server-Instanz abzurufen:

    - Azure PowerShell: [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey)

    - Azure CLI: [az-sql-server-tde-key-show](/cli/azure/sql/server/tde-key#az-sql-server-tde-key-show) 

1. Verwenden Sie den Schlüssel-URI, um den Schlüsseltresor zu identifizieren.

    - Azure PowerShell: Sie können die Eigenschaften von $MyServerKeyVaultKey untersuchen, um die Details zum Schlüsseltresor abzurufen.

    - Azure CLI: Untersuchen Sie den zurückgegebenen Serververschlüsselungsschutz, um die Details zum Schlüsseltresor abzurufen.

**Abhilfe**

Vergewissern Sie sich, dass der Schlüsseltresor verfügbar ist.

- Stellen Sie sicher, dass der Schlüsseltresor verfügbar ist und die logische SQL Server-Instanz darauf zugreifen kann.
- Befindet sich der Schlüsseltresor hinter einer Firewall, stellen Sie sicher, dass das Kontrollkästchen aktiviert ist, mit dem Microsoft-Diensten der Zugriff auf den Schlüsseltresor erlaubt wird.
- Wurde der Schlüsseltresor versehentlich gelöscht, muss die vollständige Konfiguration erneut ausgeführt werden.


### <a name="missing-key"></a>Schlüssel fehlt

**Fehlermeldungen**

_404 ServerKeyNotFound: Der angeforderte Serverschlüssel wurde im aktuellen Abonnement nicht gefunden._ 

_409 ServerKeyDoesNotExists: Der Serverschlüssel ist nicht vorhanden._

**Erkennung**

So ermitteln Sie den Schlüssel-URI und den Schlüsseltresor:

- Verwenden Sie das Cmdlet oder die Befehle unter [Schlüsseltresor fehlt](#missing-key-vault), um den Schlüssel-URI zu identifizieren, der der logischen SQL Server-Instanz hinzugefügt wird. Die Ausführung der Befehle gibt die Liste der Schlüssel zurück.

**Abhilfe**

Bestätigen Sie, dass der TDE-Schutz in Key Vault vorhanden ist:

1. Identifizieren Sie den Schlüsseltresor, und navigieren Sie dann im Azure-Portal zum Schlüsseltresor.
1. Stellen Sie sicher, dass der durch den Schlüssel-URI identifizierte Schlüssel vorhanden ist.

### <a name="missing-permissions"></a>Berechtigungen fehlen

**Fehlermeldung**

_401 AzureKeyVaultMissingPermissions: Erforderliche Serverberechtigungen für Azure Key Vault fehlen._

**Erkennung**

So identifizieren Sie den Schlüssel-URI und den Schlüsseltresor: 

- Verwenden Sie das Cmdlet oder die Befehle unter [Schlüsseltresor fehlt](#missing-key-vault), um den Schlüsseltresor zu identifizieren, den die logische SQL Server-Instanz verwendet.

**Abhilfe**

Vergewissern Sie sich, dass die logische SQL Server-Instanz über Berechtigungen für den Schlüsseltresor und über die richtigen Zugriffsberechtigungen für den Schlüssel verfügt:

- Navigieren Sie im Azure-Portal zum Schlüsseltresor und dann zu **Zugriffsrichtlinien**. Suchen Sie nach der AppId der logischen SQL Server-Instanz.  
- Ist die AppId vorhanden, vergewissern Sie sich, dass sie über die folgenden Schlüsselberechtigungen verfügt: „Get (Abrufen)“, „Wrap (Packen)“ und „Unwrap (Entpacken)“.
- Fehlt die AppId, fügen Sie sie über die Schaltfläche **Neues Element hinzufügen** hinzu. 

## <a name="getting-tde-status-from-the-activity-log"></a>Abrufen des TDE-Status aus dem Aktivitätsprotokoll

Um die Überwachung des Datenbankstatus im Hinblick auf Azure Key Vault-Schlüsselzugriffsprobleme zu ermöglichen, werden die folgenden Ereignisse im [Aktivitätsprotokoll](/azure/service-health/alerts-activity-log-service-notifications) protokolliert. Die Ressourcen-ID basiert auf der Azure Resource Manager-URL und Abonnement+Ressourcengruppe+Servername+Datenbankname: 

**Ereignis, wenn der Dienst den Zugriff auf den Azure Key Vault-Schlüssel verliert**

EventName: MakeDatabaseInaccessible 

Status: Gestartet 

Beschreibung: Die Datenbank hat den Zugriff auf den Azure Key Vault-Schlüssel verloren und ist jetzt unzugänglich: <error message>   

 

**Ereignis, wenn die Wartezeit von 8 Stunden für die automatische Reparatur beginnt** 

EventName: MakeDatabaseInaccessible 

Status: InProgress 

Beschreibung: Die Datenbank wartet darauf, dass der Azure Key Vault-Schlüsselzugriff vom Benutzer innerhalb von 8 Stunden wiederhergestellt wird.   

 

**Ereignis, wenn die Datenbank automatisch wieder online geschaltet wurde**

EventName: MakeDatabaseAccessible 

Status: Erfolgreich 

Beschreibung: Der Datenbankzugriff auf Azure Key Vault Key wurde wiederhergestellt, und die Datenbank ist nun online. 

 

**Ereignis, wenn das Problem nicht innerhalb von 8 Stunden behoben wurde und der Azure Key Vault-Schlüsselzugriff manuell validiert werden muss** 

EventName: MakeDatabaseInaccessible 

Status: Erfolgreich 

Beschreibung: Die Datenbank ist nicht zugänglich, und der Benutzer muss den Azure Key Vault-Fehler beheben und den Zugriff auf Azure Key Vault durch eine erneute Validierung des Schlüssels wiederherstellen. 

 

**Ereignis, wenn die Datenbank nach der manuellen erneuten Validierung des Schlüssels wieder online geschaltet wird**

EventName: MakeDatabaseAccessible 

Status: Erfolgreich 

Beschreibung: Der Datenbankzugriff auf Azure Key Vault Key wurde wiederhergestellt, und die Datenbank ist nun online. 

 

**Ereignis, wenn die erneute Validierung des Azure Key Vault-Schlüsselzugriffs erfolgreich war und die Datenbank wieder online geschaltet wird**

EventName: MakeDatabaseAccessible 

Status: Gestartet 

Beschreibung: Die Wiederherstellung des Datenbankzugriffs auf den Azure Key Vault-Schlüssel wurde gestartet. 

 

**Ereignis, wenn die erneute Validierung des Azure Key Vault-Schlüsselzugriffs nicht erfolgreich war**

EventName: MakeDatabaseAccessible 

Status: Fehler 

Beschreibung: Bei der Wiederherstellung des Datenbankzugriffs auf den Azure Key Vault-Schlüssel ist es zu einem Fehler gekommen. 


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Azure Resource Health](/azure/service-health/resource-health-overview).
- Richten Sie [Aktionsgruppen](/azure/azure-monitor/platform/action-groups) ein, um Benachrichtigungen und Warnungen auf die von Ihnen bevorzugte Weise zu erhalten, z. B. per E-Mail/SMS/Push/Voice, Logik-App, Webhook, ITSM oder Automation Runbook.