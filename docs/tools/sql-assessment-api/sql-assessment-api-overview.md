---
title: API für die SQL Server-Bewertung
description: Erfahren Sie mehr über die API für die SQL-Bewertung, die einen Mechanismus zur Bewertung der Konfiguration Ihrer SQL Server-Instanz im Hinblick auf bewährte Methoden bietet.
ms.prod: sql
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: ''
ms.date: 3/10/2021
ms.openlocfilehash: 34a7ff8c9380a88bb285a6d2e51e57e652f0310d
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622825"
---
# <a name="sql-assessment-api"></a>SQL-Bewertungs-API

[!INCLUDE [SQL Server 2012, ASMI, SQL Server on Azure VM, SQL on Linux](../../includes/applies-to-version/sql-asmi-sqlavm-sql-linux.md)]

Die API für die SQL-Bewertung bietet einen Mechanismus, um die Konfiguration Ihrer SQL Server-Instanz nach Best Practices zu bewerten. Die API umfasst einen vordefinierten Regelsatz, der vom SQL Server Team vorgeschlagene Best Practice-Regeln enthält. Dieser Regelsatz wird mit der Veröffentlichung neuer Versionen erweitert, aber gleichzeitig wurde die API mit der Absicht konzipiert, eine hochgradig anpassbare und erweiterbare Lösung bereitzustellen. Benutzer können deshalb die Standardregeln optimieren und eigene Regeln erstellen.

Die API für die SQL-Bewertung ist nützlich, wenn Sie sicherstellen möchten, dass Ihre SQL Server-Konfiguration den empfohlenen Best Practices entspricht. Nach einer anfänglichen Bewertung kann die Konfigurationsstabilität durch regelmäßig durchgeführte Bewertungen nachverfolgt werden.

Die API kann verwendet werden, um folgende Systeme zu bewerten:
 
* SQL Server auf Azure-VMs

* Verwaltete Azure SQL-Datenbank-Instanz

* SQL Server 2012 und höher

* SQL auf Linux-basierten Systemen

Die API wird auch von der SQL Server-Bewertungserweiterung für Azure Data Studio (ADS) verwendet.

>[!NOTE]
>Die API für die SQL-Bewertung bietet eine Bewertungsgrundlage für eine Vielzahl von Bereichen, aber sie wird nicht tief in die Sicherheit integriert. Es wird empfohlen, die [SQL-Sicherheitsrisikobewertung](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment) zu verwenden, um Ihre Datenbanksicherheit proaktiv zu verwenden.

## <a name="rules"></a>Regeln

Regeln, die gelegentlich auch als Überprüfungen bezeichnet werden, werden in JSON-formatierten Dateien definiert. Das Regelsatzformat erfordert, dass ein Regelsatzname und eine Version angegeben werden. Wenn Sie benutzerdefinierte Regelsätze verwenden, können Sie so leicht erkennen, welche Empfehlungen aus welchem Regelsatz stammen.

Der von Microsoft bereitgestellte Regelsatz ist auf GitHub verfügbar. Sie können den [gesamten Regelsatz](https://github.com/microsoft/sql-server-samples/blob/567d49a42d4cf10e4942b19290ab80828b451b77/samples/manage/sql-assessment-api/DefaultRuleset.csv) im [Beispielrepository](https://aka.ms/sql-assessment-api) anzeigen.

## <a name="sql-assessment-cmdlets-and-associated-extensions"></a>Cmdlets für SQL-Bewertung und zugehörige Erweiterungen

Die API der SQL-Bewertung ist Teil von:

* [Azure Data Studio (ADS)](../../azure-data-studio/what-is-azure-data-studio.md)

    Releaseversion von Juni 2020 und später

* [SQL Server Management Objects (SMO)](../../relational-databases/server-management-objects-smo/installing-smo.md)

    Releaseversion von Juli 2019 und später

* [Dem SQL Server PowerShell-Modul](../../powershell/download-sql-server-ps-module.md)

    Releaseversion von Juli 2019 und später

Bevor Sie mit der Verwendung der API der SQL-Bewertung beginnen, führen Sie folgende Schritte aus:

* [Installieren von ADS](https://techcommunity.microsoft.com/t5/sql-server/released-sql-server-assessment-extension-for-azure-data-studio/ba-p/1470603)

* [Installieren von SMO](../../relational-databases/server-management-objects-smo/installing-smo.md)

* [Installieren des SQL Server PowerShell-Moduls](../../powershell/download-sql-server-ps-module.md)

Das SqlServer-Modul enthält zwei neue Cmdlets für die Arbeit mit der API für die SQL-Bewertung:

* **Get-SqlAssessmentItem**: Stellt eine Liste der verfügbaren Bewertungsüberprüfungen für ein SQL Server-Objekt bereit.

* **Invoke-SqlAssessment**: Stellt die Ergebnisse einer Bewertung bereit.

Das SMO-Framework wird um die API-Erweiterung für die SQL-Bewertung ergänzt, die folgende Methoden bereitstellt:

* **GetAssessmentItems**: Gibt verfügbare Überprüfungen für ein bestimmtes SQL-Objekt zurück (IEnumerable<…>)

* **GetAssessmentResults**: Führt eine synchrone Auswertung der Bewertung durch und gibt Fehler zurück, sofern vorhanden (IEnumerable<…>)

* **GetAssessmentResultsList**: Führt eine asynchrone Auswertung der Bewertung durch und gibt Fehler zurück, sofern vorhanden (Task<…>)

## <a name="get-started-using-sql-assessment-cmdlets"></a>Erste Schritte mit den Cmdlets für die SQL-Bewertung

Eine Bewertung wird für ein ausgewähltes SQL Server-Objekt durchgeführt. Im Standardregelsatz sind nur Überprüfungen für zwei Arten von Objekten enthalten: Server und Datenbank (zusätzlich dazu unterstützt die API zwei weitere Typen: Dateigruppe und Verfügbarkeitsgruppe). Wenn Sie eine SQL-Instanz und alle zugehörigen Datenbanken bewerten möchten, müssen Sie die Cmdlets für die SQL-Bewertung für jedes Objekt separat ausführen. Alternativ können Sie die zu bewertenden Objekte in einer Variablen oder der Pipeline an die Cmdlets für die SQL-Bewertung übergeben.

SqlServer- und RegisteredServer-Objekte sind austauschbar, deshalb können Sie ein beliebiges dieser Objekte an die Cmdlets für die SQL-Bewertung übergeben.

Gehen Sie zum Einstieg die folgenden Beispiele durch.

1. Rufen Sie eine Liste der verfügbaren Überprüfungen für eine lokale Standardinstanz ab, um sich mit den Überprüfungen vertraut zu machen. In diesem Beispiel wird die Ausgabe des Get-SqlInstance-Cmdlets per Pipe an das Get-SqlAssessmentItem-Cmdlet übergeben, um das Instanzobjekt zu übergeben.

    ```powershell
    Get-SqlInstance -ServerInstance 'localhost' | Get-SqlAssessmentItem
    ```

2. Rufen Sie eine Liste der verfügbaren Überprüfungen für alle Datenbanken der Instanz ab. Hier verwenden wir das Get-Item-Cmdlet und einen mit dem Windows PowerShell-SQL Server-Anbieter implementierten Pfad, um eine Liste der Datenbanken abzurufen. Anschließend wird die Liste per Pipe an das Get-SqlDatabase-Cmdlet übergeben.

    ```powershell
    Get-Item SQLSERVER:\SQL\localhost\default | Get-SqlAssessmentItem
    ```

    Sie können hierzu auch das Get-SqlDatabase-Cmdlet verwenden.

    ```powershell
    Get-SqlDatabase -ServerInstance 'localhost' | Get-SqlAssessmentItem
    ```

3. Rufen Sie eine Liste der verfügbaren Überprüfungen für alle Datenbanken der Instanz ab. Hier verwenden wir das Get-Item-Cmdlet und einen mit dem Windows PowerShell-SQL Server-Anbieter implementierten Pfad, um eine Liste der Datenbanken abzurufen. Anschließend wird die Liste per Pipe an das Get-SqlDatabase-Cmdlet übergeben.

    ```powershell
    Get-Item SQLSERVER:\SQL\localhost\default | Get-SqlAssessmentItem
    ```

    Sie können hierzu auch das Get-SqlDatabase-Cmdlet verwenden.

    ```powershell
    Get-SqlDatabase -ServerInstance 'localhost' | Get-SqlAssessmentItem
    ```

4. Rufen Sie die Bewertung für die Instanz auf, und speichern Sie die Ergebnisse in einer SQL-Tabelle. In diesem Beispiel wird die Ausgabe des Get-SqlInstance-Cmdlets per Pipe an das Invoke-SqlAssessment-Cmdlet übergeben, dessen Ergebnisse per Pipe an das Write-SqlTableData-Cmdlet übergeben werden. Das Invoke-Assessment-Cmdlet wird in diesem Beispiel mit dem `-FlattenOutput`-Parameter ausgeführt. Durch diesen Parameter ist die Ausgabe für das Write-SqlTableData-Cmdlet geeignet. Fehlt der Parameter, löst das Cmdlet einen Fehler aus.

    ```powershell
    Get-SqlInstance -ServerInstance 'localhost' |
    Invoke-SqlAssessment -FlattenOutput |
    Write-SqlTableData -ServerInstance 'localhost' -DatabaseName SQLAssessmentDemo -SchemaName Assessment -TableName Results -Force
    ```

    Rufen wir jetzt eine Bewertung für alle Datenbanken der Instanz auf und fügen die Ergebnisse in dieselbe Tabelle ein.

    ```powershell
    Get-SqlDatabase -ServerInstance 'localhost' |
    Invoke-SqlAssessment -FlattenOutput |
    Write-SqlTableData -ServerInstance 'localhost' -DatabaseName SQLAssessmentDemo -SchemaName Assessment -TableName Results -Force
    ```

5. Folgen Sie den Beschreibungen und Links in der Tabelle, um die Empfehlungen besser zu verstehen.

6. Passen Sie die Regeln basierend auf Ihrer Umgebung und den Organisationsanforderungen an (siehe unten).

7. Planen Sie eine Aufgabe oder einen Auftrag, um die Bewertung regelmäßig oder bei Bedarf auszuführen, um den Fortschritt zu messen.

## <a name="customizing-rules"></a>Anpassen von Regeln

Regeln sind so entworfen, dass sie angepasst und erweitert werden können. Der Regelsatz von Microsoft ist so konzipiert, dass er für die meisten Umgebungen geeignet ist. Es gibt jedoch keinen Regelsatz, der für jede einzelne Umgebung funktioniert. Benutzer können eigene JSON-Dateien schreiben und vorhandene Regeln anpassen oder neue Regeln erstellen. Im [Repository mit Beispielen](https://aka.ms/sql-assessment-api) finden Sie Beispiele für Anpassungen und den vollständigen von Microsoft veröffentlichten Regelsatz. Weitere Informationen zum Ausführen der Cmdlets für die SQL-Bewertung mit benutzerdefinierten JSON-Dateien finden Sie im Get-Help-Cmdlet.

### <a name="options-available-with-rule-customization-feature"></a>Verfügbare Optionen für die Regelanpassung

#### <a name="enablingdisabling-certain-rules-or-groups-of-rules-using-tags"></a>Aktivieren/Deaktivieren bestimmter Regeln oder Regelgruppen (mithilfe von Tags)

Sie können bestimmte Regeln unterdrücken, wenn sie nicht auf Ihre Umgebung anwendbar sind oder bis geplante Arbeiten zur Behebung des Problems abgeschlossen sind.

#### <a name="changing-threshold-parameters"></a>Ändern von Schwellenwertparametern

Bestimmte Regeln verfügen über Schwellenwerte, die mit dem aktuellen Wert einer Metrik verglichen werden, um Probleme zu ermitteln. Wenn die Standardschwellenwerte nicht geeignet sind, können Sie sie ändern.

#### <a name="adding-more-rules-written-by-you-or-third-parties"></a>Hinzufügen weiterer Regeln, die von Ihnen oder Drittanbietern geschrieben wurden

Sie können Regelsätze bündeln, indem Sie dem Aufruf der API für die SQL-Bewertung mindestens eine JSON-Datei als Parameter hinzufügen. Ihre Organisation kann solche Dateien schreiben oder von einem Drittanbieter erwerben. So können Sie beispielsweise Ihre JSON-Datei, die bestimmte Regeln aus dem Microsoft-Regelsatz deaktiviert, eine weitere JSON-Datei mit nützlichen Regeln für Ihre Umgebung von einem Branchenexperten und danach eine weitere JSON-Datei verwenden, die einige Schwellenwerte in dieser JSON-Datei ändert.

>[!IMPORTANT]
>Es wird dringend davon abgeraten, Regelsätze aus nicht vertrauenswürdigen Quellen zu verwenden. Setzen Sie diese erst ein, wenn Sie sie gründlich untersucht und sich davon überzeugt haben, dass ihre Verwendung sicher ist.

## <a name="next-steps"></a>Nächste Schritte

* [SQL Server Management Objects (SMO)](../../relational-databases/server-management-objects-smo/overview-smo.md)
* [PowerShell](../../powershell/download-sql-server-ps-module.md)
* [Sicherheitsrisikobewertung mit der SQL](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment)
