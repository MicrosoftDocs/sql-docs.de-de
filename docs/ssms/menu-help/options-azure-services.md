---
title: 'SQL Server-Optionsseite: Azure-Dienste'
description: Optionen (Azure-Dienste)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Azure_Services.Azure_Cloud
dev_langs:
- TSQL
author: markingmyname
ms.author: maghan
ms.date: 01/15/2021
ms.openlocfilehash: 7e4bb93f4a3a992c5033061ca8a2e0209125a5fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076621"
---
# <a name="options-azure-services"></a>Optionen (Azure-Dienste)

Auf dieser Seite geben Sie Optionen für Azure-Clouddienste an. Greifen Sie über **Tools > Optionen > Azure-Dienste** auf der oberen Menüleiste auf dieses Dialogfeld zu.

## <a name="miscellaneous"></a>Verschiedenes

| Option | Information | BESCHREIBUNG |
|--------|-------------|-------------|
| Ablaufverfolgungsebene für ADAL-Ausgabefenster | **Information** <br> **Keine** <br> **Ausführlich** <br> **Warning** | Die Ebene der Azure Active Directory-Anmeldeablaufverfolgung (AAD), die an das Ausgabefenster gesendet werden soll. |
| Azure Data Factory-Portal-URL | `https://adf.azure.com` | Gibt die URL für das Azure Data Factory-Portal an. |
| Bedingten Zugriff auf Azure SQL-Datenbank aktivieren (EXPERIMENTELL) | **Wahr** <br> **False** | EXPERIMENTELL: Wenn diese Option TRUE lautet, werden Zugriffstoken für Azure SQL-Datenbank angefordert, die einen Anspruch für den Zugriff auf den ausgewählten Server enthalten. Möglicherweise muss SSMS neu gestartet werden, damit diese Einstellung wirksam wird. |
| Katalogendpunkt | `https://gallery.azure.com` | Gibt den Endpunkt für den Resource Manager-Katalog mit Bereitstellungsvorlagen an. |
| Graph-Zielgruppe | `https://graph.windows.net` | Die Ressourcen-ID für den Graph-Endpunkt. |
| Graph-Endpunkt | `https://graph.windows.net` | Gibt die URL für Azure Active Directory Graph-Anforderungen an. |
| Verwaltungsportal-URL | `https://portal.azure.com` | Gibt die URL für das Verwaltungsportal an. |
| URL zur Datei mit Veröffentlichungseinstellungen | `https://go.microsoft.com/fwlink/?LinkID=335839` | Gibt die URL an, von der die `.publishsettings`-Datei heruntergeladen werden kann. |
| Dienstprinzipalname für SQL-Datenbank | `https://database.windows.net/` | Der Azure SQL-Datenbank-Dienstprinzipalname, für den bei Verwendung der AAD-Authentifizierung ein Token abgerufen werden soll. Außerdem die Zielgruppe des JSON Web Token (JWT) für die serverseitige Analyse/Validierung des JSON Web Token (JWT). |

## <a name="resource-management"></a>Ressourcenverwaltung

| Option | Information | BESCHREIBUNG |
|--------|-------------|-------------|
| Active Directory-Autorität | `https://login.microsoftonline.com` | Gibt die Basisautorität für die Azure Active Directory-Authentifizierung (AAD) an. |
| Ressourcen-ID des Active Directory-Dienstendpunkts | `https://management.core.windows.net` | Gibt die Zielgruppe für Token an, die Anforderungen an Azure Resource Manager-Endpunkte (ARM) oder Dienstverwaltungsendpunkte (RDFE) authentifizieren. |
| Resource Manager-Endpunkt | `https://management.azure.com` | Gibt die URL für Ressourcenverwaltungsanforderungen an. |
| Dienstendpunkt | `https://management.core.windows.net` | Gibt den Endpunkt für Dienstverwaltungsanforderungen an. |

## <a name="select-an-azure-cloud"></a>Azure-Cloud auswählen

| Option | Information | BESCHREIBUNG |
|--------|-------------|-------------|
| Name | **Azure-Cloud China** <br><br> **Azure Cloud** <br><br> **Azure German Cloud** <br><br> **Azure US Government** <br><br> **Benutzerdefiniert** | Wählen Sie die Azure-Cloud aus, mit der Management Studio eine Verbindung herstellen soll, um Ressourcen zu entdecken und zu verwalten. Klicken Sie auf **Benutzerdefiniert**, um Ihre eigenen URLs und DNS-Suffixe bereitzustellen. |

## <a name="service-addresses"></a>Dienstadressen

| Option | Information | BESCHREIBUNG |
|--------|-------------|-------------|
| Azure Data Lake-Dienstendpunkt | `azuredatalakeanalytics.net` | Gibt den Azure Data Lake Analytics-Katalog und das Suffix für den Auftragsendpunkt an. |
| Azure Data Lake Store-Dateisystemendpunkt | `azuredatalakestore.net` | Gibt das Suffix für den Endpunkt des Azure Data Lake Store-Dateisystems an. | 
| Azure Key Vault-Zielgruppe | `https://vault.azure.net` | Gibt die Zielgruppe für Zugriffstoken an, die Anforderungen für Key Vault-Dienste autorisieren. |
| Azure Key Vault-DNS-Suffix | `vault.azure.net` | Gibt das Domänennamensuffix für Azure Key Vault-Server an. |
| SQL-Datenbank-DNS-Suffix | `database.windows.net` | Gibt das Domänennamensuffix für Azure SQL-Datenbank-Server an. |
| Speicherendpunkt | `core.windows.net` | Gibt den Endpunkt für den Speicherzugriff an. Hierbei kann es sich um Blob-, Tabellen-, Warteschlangen- und Dateispeicher handeln. |
| Traffic Manager-DNS-Suffix | `trafficmanager.net` | Gibt das Domänennamensuffix für Azure Traffic Manager-Dienste an. |