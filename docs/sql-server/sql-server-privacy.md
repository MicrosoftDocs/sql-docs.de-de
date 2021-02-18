---
description: Ergänzende Datenschutzbestimmungen zu SQL Server
title: Ergänzende Datenschutzbestimmungen zu SQL Server | Microsoft-Dokumentation
ms.date: 11/11/2020
ms.prod: sql
ms.technology: release-landing
ms.reviewer: wopeter
ms.custom: ''
ms.topic: conceptual
f1_keywords: ''
helpviewer_keywords: ''
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 1821ebdd7c52d306a34be98f6e506de24c31539e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341670"
---
# <a name="sql-server-privacy-supplement"></a>Ergänzende Datenschutzbestimmungen zu SQL Server

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Dieser Artikel beschreibt internetfähige Features, die anonyme Featurenutzungs- und Diagnosedaten sammeln und an Microsoft senden können. SQL Server sammelt möglicherweise Standardinformationen zu Ihrem Computer, und Daten zur Nutzung und Leistung werden möglicherweise an Microsoft übermittelt und analysiert, um die Qualität, Sicherheit und Zuverlässigkeit des Produkts zu optimieren. Wenn Sie SQL Server auf einem virtuellen Computer (VM) in Microsoft Azure installieren, werden möglicherweise Umgebungsinformationen an Microsoft gesendet, damit Microsoft die SQL Server-IaaS-Agent-Erweiterung auf Ihrer VM installieren und Ihre SQL-VM-Ressource beim SQL VM-Ressourcenanbieter registrieren kann, wie hier [ beschrieben](/azure/azure-sql/virtual-machines/windows/sql-vm-resource-provider-register).

Dieser Artikel ist ein Nachtrag zu den [Microsoft-Datenschutzbestimmungen](https://go.microsoft.com/fwlink/?LinkId=521839). Die Datenklassifizierung in diesem Artikel gilt nur für lokale Versionen von SQL Server. Sie gilt nicht für folgende Produkte:

- Azure SQL-Datenbank
- [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-telemetry-ssms.md)
- SQL Server Data Tools (SSDT)
- Azure Data Studio
- Datenbankmigrations-Assistent
- SQL Server Migration Assistant
- MS-SQL-Erweiterung

Definition von *zugelassenen Verwendungsszenarios*. Im Rahmen dieses Artikels werden „zugelassene Verwendungsszenarios“ als Aktionen oder Aktivitäten definiert, die von Microsoft initiiert werden.

## <a name="access-control"></a>Zugriffssteuerung

Informationen, die auf die Anmeldeinformationen bezogen sind, die zum Sichern von Anmeldungen, Benutzern oder Konten in einer Installation von SQL Server verwendet werden.

### <a name="examples-of-access-control"></a>Beispiele für die Zugriffssteuerung

- Kennwörter
- Zertifikate

### <a name="permitted-usage-scenarios"></a>Szenarios für die zulässige Verwendung

|Szenario |Zugriffsbeschränkungen |Aufbewahrungsanforderungen |
|---------|---------|---------|
|Diese Anmeldeinformationen verlassen den Computer des Benutzers nie über Nutzungs- und Diagnosedaten. |- |- |
|Absturzabbilder können Zugriffssteuerungsdaten enthalten. |- |Absturzabbilder: Maximal 30 Tage. |
|Diese Anmeldeinformationen verlassen den Computer des Benutzers nie über das Nutzungsfeedback, es sei denn, der Kunde fügt sie manuell ein. |Der Zugriff ist auf den internen Gebrauch von Microsoft ohne Zugriff von Drittanbietern begrenzt. |Benutzerfeedback: Maximal 1 Jahr.|
|&nbsp;|&nbsp;|&nbsp;|

## <a name="customer-data"></a>Kundendaten

Mit Kundendaten sind Daten gemeint, die direkt oder indirekt in Benutzertabellen gespeichert sind. Diese Daten schließen Statistiken und Benutzerliterale in Abfragetexten ein, die in Benutzertabellen gespeichert werden können.

### <a name="examples-of-customer-data"></a>Beispiele für Kundendaten

- Datenwerte, die in den Zeilen einer Benutzertabelle gespeichert werden.
- Statistikobjekte, die Kopien von Werten in den Zeilen einer Benutzertabelle enthalten.
- Abfragetexte, die Literalwerte enthalten.

### <a name="permitted-usage-scenarios"></a>Szenarios für die zulässige Verwendung

|Szenario  |Zugriffsbeschränkungen  |Aufbewahrungsanforderungen |
|---------|---------|---------|
|Diese Daten verlassen den Computer des Benutzers nie über Nutzungs- und Diagnosedaten. |- |- |
|Absturzabbilder können Kundendaten enthalten und an Microsoft gesendet werden. |- |Absturzabbilder: Maximal 30 Tage. |
|Benutzerfeedback, das Kundendaten enthält, kann mit Zustimmung der Kunden an Microsoft gesendet werden. |Der Zugriff ist auf den internen Gebrauch von Microsoft ohne Zugriff von Drittanbietern begrenzt. Microsoft kann die Daten für den ursprünglichen Kunden verfügbar machen. |Benutzerfeedback: Maximal 1 Jahr. |

## <a name="personal-data"></a>Personenbezogene Daten

Daten, die von einem Benutzer stammen oder durch die Verwendung des Produkts erstellt wurden.
- Auf einen individuellen Benutzer zurückführbar.
- Enthalten keine Kundendaten.

### <a name="examples-of-personal-data"></a>Beispiele zu persönlichen Daten

- Schnittstellenidentifikation Die vollständige IP-Adresse
- Computername
- Anmelde- oder Benutzernamen
- Lokaler Teil der E-Mail-Adresse (joe@contoso.com)
- Informationen zum Speicherort
- Kundenidentifikation

### <a name="permitted-usage-scenarios"></a>Szenarios für die zulässige Verwendung

|Szenario  |Zugriffsbeschränkungen  |Aufbewahrungsanforderungen|
|---------|---------|---------|
|Diese Daten verlassen den Computer des Benutzers nie über Nutzungs- und Diagnosedaten. |- |- |
|Absturzabbilder können personenbezogene Daten enthalten und an Microsoft gesendet werden. |- |Absturzabbilder: Maximal 30 Tage. |
|Die Kunden-ID kann an Microsoft ausgegeben werden, um neue Hybrid- und Cloudfeatures bereitzustellen, die Benutzer abonniert haben. |- |Zurzeit existieren solche Hybrid- oder Cloudfeatures nicht.|
|Benutzerfeedback, das Kundendaten enthält, kann mit Zustimmung der Kunden an Microsoft gesendet werden.|Der Zugriff ist auf den internen Gebrauch von Microsoft ohne Zugriff von Drittanbietern begrenzt. Microsoft kann die Daten für den ursprünglichen Kunden verfügbar machen. |Benutzerfeedback: Maximal 1 Jahr. |

## <a name="internet-based-services-data"></a>Daten zu internetbasierten Diensten

Daten, die gemäß der SQL Server-Lizenzbedingungen für die Bereitstellung von internetbasierten Diensten benötigt werden.

### <a name="examples-of-internet-based-services-data"></a>Beispiele für internetbasierte Dienstdaten

- Informationen über die Computerspezifikationen
- Name oder Version des Browsers
- SQL Server-Version
- Sprachcode
- Eine IP-Adresse mit bestimmten entfernten Oktetten
- Kartendaten

### <a name="permitted-usage-scenarios"></a>Szenarios für die zulässige Verwendung

|Szenario  |Zugriffsbeschränkungen  |Aufbewahrungsanforderungen|
|---------|---------|---------| 
|Können von Microsoft verwendet werden, um Features zu verbessern und bzw. oder Fehler in aktuellen Features zu beheben. |Der Zugriff ist auf den internen Gebrauch von Microsoft ohne Zugriff von Drittanbietern begrenzt. Microsoft kann die Daten für den ursprünglichen Kunden verfügbar machen.  Zum Beispiel Dashboards. |Mindestens 90 Tage bis maximal 3 Jahre. |
|Benutzerfeedback, das Kundendaten enthält, kann mit Zustimmung der Kunden an Microsoft gesendet werden. |Der Zugriff ist auf den internen Gebrauch von Microsoft ohne Zugriff von Drittanbietern begrenzt. |Benutzerfeedback, das Kundendaten enthält, kann mit Zustimmung der Kunden an Microsoft gesendet werden. |
|Power View und Kartenelemente von SQL Reporting Services können Daten für die Verwendung von Bing Maps freigeben. |Der Zugriff ist auf Sitzungsdaten begrenzt. |- |

## <a name="non-personal-data"></a>Keine personenbezogenen Daten

1. Daten, die von einer Organisation stammen oder durch die Verwendung des Produkts erstellt wurden. Sie können auf eine Organisation zurückgeführt werden und enthalten keine Kundendaten.

   - Beispiel
     - Name der Organisation (Beispiel: Microsoft Corp.)

   - Szenarios für die zulässige Verwendung

     |Szenario  |Zugriffsbeschränkungen  |Aufbewahrungsanforderungen|
     |---------|---------|---------|
     | Microsoft kann allgemeine Nutzungsdaten von SQL Server-Instanzen sammeln, die in Azure Virtual Machines ausgeführt werden, um Kunden innerhalb von Azure optionale Vorteile für die Verwendung von SQL Server in Azure Virtual Machines zu bieten. | Microsoft kann Daten für den Kunden z.B. über das Azure-Portal verfügbar machen, damit Kunden, die SQL Server in Azure Virtual Machines ausführen, die Vorteile von SQL Server in Azure nutzen können. </br></br>Microsoft verwendet diese Daten ohne vorherige Zustimmung des Kunden nicht für Lizenzierungsüberprüfungen. | Mindestens 90 Tage bis maximal 3 Jahre. |

2. Daten, die zum Beschreiben oder Konfigurieren von Servern, Datenbanken, Tabellen und anderen Ressourcen verwendet werden, die von Kunden erstellt oder bereitgestellt werden. Sie umfassen Datenbanktabellen- und Spaltennamen, aber nicht die Inhalte von Datenbankzeilen oder andere Kundendaten. Kunden sollten keine personenbezogenen Daten in solchen Feldern platzieren oder Anwendungen erstellen, mit denen personenbezogene Daten in diesen Feldern gespeichert werden. Für die unten aufgeführten Szenarios für die zulässige Verwendung wird nur die Hashform verwendet, um Verwendungsmuster zum Verbessern des Produkts zu bestimmen.

   - Beispiel
      - SQL Server-Datenbanknamen
      - Tabellen- und Spaltennamen
      - Namen von Statistiken

    - Szenarios für die zulässige Verwendung

      > [!NOTE]
      > Alle Metadatenwerte werden vor der Sammlung mit einem Hashwert versehen.
      >

      |Szenario  |Zugriffsbeschränkungen  |Aufbewahrungsanforderungen|
      |---------|---------|---------|
      |Können von Microsoft verwendet werden, um Features zu verbessern und bzw. oder Fehler in aktuellen Features zu beheben. |Der Zugriff ist auf den internen Gebrauch von Microsoft ohne Zugriff von Drittanbietern begrenzt. |Mindestens 90 Tage bis maximal 3 Jahre.|

3. Daten, die generiert werden, während der Server ausgeführt wird. Sie enthalten keine Kundendaten, keine personenbezogen Daten, wie in Punkt 1 oder 2 (weiter oben) aufgeführt, keine Kundenzugriffssteuerungsdaten und keine personenbezogenen Daten.

   - Beispiel
     - Datenbank-GUID
     - Hash des Computernamens
     - Hash des Instanznamens
     - Anwendungsname
     - Verhaltens- oder Nutzungsdaten
     - Daten vom SQL-Programm zur Verbesserung der Benutzerfreundlichkeit (SQLCEIP)
     - Serverkonfigurationsdaten, zum Beispiel Einstellungen von „sp_configure“
     - Featurekonfigurationsdaten
     - Ereignisnamen und Fehlercodes
     - Hardwareeinstellungen und -identifizierung, z.B. OEM-Hersteller

   Microsoft untersucht die Werte von Anwendungsnamen, die von anderen Programmen, die SQL Server verwenden, festgelegt wurden (z. B. SharePoint oder von Drittanbietern zusammengestellte Programme), und fügt diese Informationen in Metadatenfelder ein, die an Microsoft gesendet werden, wenn die Nutzungsdaten aktiviert sind. Kunden sollten keine personenbezogenen Daten in solchen Metadatenfeldern platzieren oder Anwendungen erstellen, mit denen personenbezogene Daten in diesen Feldern gespeichert werden.

   - Szenarios für die zulässige Verwendung

     |Szenario  |Zugriffsbeschränkungen  |Aufbewahrungsanforderungen|
     |---------|---------|---------|
     |Können von Microsoft verwendet werden, um Features zu verbessern und bzw. oder Fehler in aktuellen Features zu beheben.|Der Zugriff ist auf den internen Gebrauch von Microsoft ohne Zugriff von Drittanbietern begrenzt. |Mindestens 90 Tage bis maximal 3 Jahre. |
     |Können verwendet werden, um dem Kunden Vorschläge zu machen.  Zum Beispiel können Sie Folgendes vorschlagen: „Ziehen Sie das Feature *X* in Betracht, da es für Ihre Verwendung des Produkts besser geeignet ist.“ |Microsoft kann die Daten für den ursprünglichen Kunden, z.B. über Dashboards, verfügbar machen. |Sicherheitsprotokolle zu Kundendaten: Mindestens 3 Jahre bis maximal 6 Jahre. |
     |Können von Microsoft für die zukünftige Produktplanung verwendet werden. |Microsoft kann diese Informationen mit anderen Hardware- und Softwareanbietern teilen, um die Ausführung derer Produkte im Zusammenspiel mit Microsoft-Software zu verbessern. |Mindestens 90 Tage bis maximal 3 Jahre.|
     |Können von Microsoft verwendet werden, um cloudbasierte Dienste bereitzustellen, die auf den ausgegebenen Nutzungs- und Diagnosedaten basieren. Zum Beispiel ein Kundendashboard, das die Verwendung von Features in allen Installationen von SQL Server in einer Organisation anzeigt. |Microsoft kann die Daten für den ursprünglichen Kunden, z.B. über Dashboards, verfügbar machen. |Mindestens 90 Tage bis maximal 3 Jahre. |
     |Benutzerfeedback, das Kundendaten enthält, kann mit Zustimmung der Kunden an Microsoft gesendet werden. |Der Zugriff ist auf den internen Gebrauch von Microsoft ohne Zugriff von Drittanbietern begrenzt. Microsoft kann die Daten für den ursprünglichen Kunden verfügbar machen. |Benutzerfeedback: Maximal 1 Jahr. |
     |Datenbankname und Anwendungsname werden möglicherweise verwendet, um die Datenbanken und Anwendungen bekannten Kategorien zuzuordnen, z.B. Anwendungen, die Microsoft-Software oder Software von anderen Unternehmen ausführen.|Der Zugriff ist auf den internen Gebrauch von Microsoft ohne Zugriff von Drittanbietern begrenzt.|Mindestens 90 Tage bis maximal 3 Jahre. |

## <a name="system-generated-logs-controls"></a>Steuerelemente für systemgenerierte Protokolle

Anweisungen dazu, wie systemgenerierte Protokolle im Produkt aktiviert/deaktiviert werden können, finden Sie hier: [Konfigurieren der Sammlung von Nutzungs- und Diagnosedaten für SQL Server (CEIP)](usage-and-diagnostic-data-configuration-for-sql-server.md).

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
