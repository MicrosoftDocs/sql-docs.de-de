---
title: Azure Arc-fähige Version von SQL Server – Versionshinweise
description: Aktuelle Versionshinweise
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 12/08/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: f53731685f5ba1723ebdd8d20064342808205566
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070305"
---
# <a name="release-notes---azure-arc-enabled-sql-server-preview"></a>Versionshinweise – Azure Arc-fähige Version von SQL Server (Vorschauversion)

> [!NOTE]
> Als Previewfunktion unterliegt die in diesem Artikel vorgestellte Technologie den [zusätzlichen Nutzungsbedingungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="december-2020"></a>Dezember 2020

### <a name="breaking-change"></a>Unterbrechende Änderung

Mit diesem Release wird ein aktualisierter [Ressourcenanbieter](/azure/azure-resource-manager/management/azure-services-resource-providers) mit dem Namen `Microsoft.AzureArcData` eingeführt. Damit Sie Azure Arc-fähige SQL Server-Instanzen weiterhin verwenden können, müssen Sie diesen Ressourcenanbieter registrieren. Weitere Informationen finden Sie in den Anweisungen für die Ressourcenanbieterregistrierung im Abschnitt [Voraussetzungen](connect.md#prerequisites).

Wenn Sie über vorhandene Ressourcen vom Typ „SQL Server – Azure Arc“ verfügen, führen Sie die folgenden Schritte aus, um diese Ressourcen zum Microsoft.AzureArcData-Namespace zu migrieren.

1. Starten Sie [Cloud Shell](https://shell.azure.com/). Sollten Sie weitere Informationen hierzu benötigen, [informieren Sie sich über PowerShell in Cloud Shell](/azure/cloud-shell/quickstart-powershell).

2. Laden Sie das Skript über den folgenden Befehl in die Shell hoch:

    ```console
    curl https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/manage/azure-arc-enabled-sql-server/migrate-to-azure-arc-data.ps1 -o migrate-to-azure-arc-data.ps1
    ```
3. Führen Sie das Skript aus.  

    ```console
   ./migrate-to-azure-arc-data.ps1
    ```

> [!NOTE]
> - Verwenden Sie `Ctrl-Shift-V` unter Windows oder `Cmd-v` unter macOS, um die Befehle in die Shell zu kopieren.
> - Der `curl`-Befehl kopiert das Skript direkt in den Basisordner für Ihre Cloud Shell-Sitzung hochgeladen.
> - Das Skript fordert den Ressourcengruppennamen an und gibt eine Meldung zurück, wenn die Migration abgeschlossen ist.

### <a name="other-changes"></a>Weitere Änderungen

* Die *TCPPorts*-Eigenschaft für den Ressourcentyp **SQL Server – Azure Arc** wurde in *TCPStaticPorts* umbenannt.
* Es sind nun weniger Berechtigungen erforderlich als zuvor. Ausführliche Informationen finden Sie im Abschnitt [Erforderliche Berechtigungen](overview.md#required-permissions).

### <a name="known-issues"></a>Bekannte Probleme

* Die *CreateTime*-Eigenschaft wird nicht zu neu erstellten Ressourcen im AzureArcData-Namespace hinzugefügt. Dies betrifft auch die Ressourcen vom Typ **SQL Server – Azure Arc**.

## <a name="october-2020"></a>Oktober 2020

Das Oktoberupdate umfasst die folgenden Verbesserungen:

* Das Blatt „Azure Arc-fähigen SQL-Server registrieren“ enthält jetzt die Registerkarte **Tags**. Die Tags sind im Registrierungsskript enthalten und werden in den Ressourcen unter **SQL Server - Azure Arc** angezeigt. Weitere Informationen finden Sie unter [Herstellen einer Verbindung zwischen Ihrer SQL Server-Instanz und Azure Arc](connect.md).

* Der Eintrag **Environment Health** (Umgebungsintegrität) unterstützt jetzt die Aktivierung der **SQL-Bewertung** über das Portal durch Bereitstellen einer *benutzerdefinierten Skripterweiterung* (CustomScriptExtension). Ausführliche Informationen finden Sie unter [Konfigurieren der SQL-Bewertung](assess.md#run-on-demand-sql-assessment).

### <a name="known-issues"></a>Bekannte Probleme

Das Oktoberrelease weist die folgenden Probleme auf:

* Das Herstellen einer Verbindung zwischen SQL Server-Instanzen und Azure Arc erfordert ein Konto, das über verschiedene Berechtigungen verfügt. Weitere Informationen finden Sie unter [Erforderliche Berechtigungen](overview.md#required-permissions).

## <a name="september-2020"></a>September 2020

Die Azure Arc-fähige Version von SQL Server ist nun als Public Preview verfügbar. Dank der Azure Arc-fähigen Version von SQL Server sind die Azure-Dienste auch für Instanzen verfügbar, die außerhalb von Azure im Rechenzentrum des Kunden, am Edge oder in einer Multicloudumgebung gehostet werden.

Weitere Informationen finden Sie unter [Übersicht über die Azure Arc-fähige Version von SQL Server](overview.md).

### <a name="known-issues"></a>Bekannte Probleme

Das Septemberrelease weist die folgenden Probleme auf:

* Das Blatt **Register Azure Arc enabled SQL Server** (Azure Arc-fähigen SQL-Server registrieren) unterstützt nicht das Konfigurieren benutzerdefinierter Tags. Zum Hinzufügen von benutzerdefinierten Tags müssen Sie nach der Registrierung die Ressource **SQL Server - Azure Arc** öffnen und die Tags auf der Seite **Übersicht** ändern.

* Das Herstellen einer Verbindung zwischen SQL Server-Instanzen und Azure Arc erfordert ein Konto, das über verschiedene Berechtigungen verfügt. Weitere Informationen finden Sie unter [Erforderliche Berechtigungen](overview.md#required-permissions).

## <a name="next-steps"></a>Nächste Schritte

**Möchten Sie es selbst ausprobieren?**  Legen Sie los mit dem [Schnellstart für die Azure Arc-fähige Version von SQL Server](https://aka.ms/AzureArcSqlServerJumpstart).