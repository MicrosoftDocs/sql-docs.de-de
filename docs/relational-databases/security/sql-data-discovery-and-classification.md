---
title: SQL-Datenermittlung und -klassifizierung | Microsoft-Dokumentation
description: SQL-Datenermittlung und -klassifizierung
documentationcenter: ''
ms.reviewer: vanto
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.prod_service: sql-database,sql
ms.custom: security
ms.topic: conceptual
ms.date: 02/05/2021
ms.author: datrigan
author: DavidTrigano
ms.openlocfilehash: 74478f363ac4c6fac4ab684fde1bafd0a2bb8551
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340174"
---
# <a name="sql-data-discovery-and-classification"></a>SQL-Datenermittlung und -klassifizierung
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Die Datenermittlung und -klassifizierung führt ein neues Tool in [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) für das **Ermitteln**, **Klassifizieren**, **Bezeichnen**  &  **Melden** von vertraulichen Daten in Ihren Datenbanken ein.
Die Ermittlung und Klassifizierung Ihrer sensibelsten Daten (geschäftliche, finanzielle, gesundheitliche usw.) kann im Informationsschutzformat Ihres Unternehmens eine entscheidende Rolle spielen. Sie kann für Folgendes als Infrastruktur gelten:
* Maßnahmen zum Einhalten von Datenschutzstandards.
* Überwachen des Zugriffs auf Datenbanken/Spalten, die hochsensible Daten enthalten

> [!NOTE]
> Die Datenermittlung und -klassifizierung wird **in SQL Server 2012 und höher unterstützt und kann mit [SSMS 17.5](../../ssms/download-sql-server-management-studio-ssms.md) oder höher verwendet werden**. Weitere Informationen zur Datenermittlung und -klassifizierung in Azure SQL-Datenbanken finden Sie unter [Azure SQL-Datenbank – Datenermittlung und -klassifizierung](/azure/sql-database/sql-database-data-discovery-and-classification/).

## <a name="overview"></a><a id="Overview"></a>Übersicht
Die Datenermittlung und -klassifizierung führen eine Reihe von Diensten ein, und sie bilden ein neues SQL Information Protection-Paradigma für den Schutz von Daten, nicht nur von der Datenbank:

* **Ermittlung und Empfehlungen:** Die Klassifizierungsengine überprüft Ihre Datenbank und identifiziert Spalten, die möglicherweise sensible Daten enthalten. Dann besteht die einfache Möglichkeit zum Überprüfen und Anwenden der geeigneten Empfehlungen zur Klassifizierung und zum manuellen Klassifizieren von Spalten.
* **Bezeichnung:** Sensible Daten können dauerhaft mit Klassifizierungsbezeichnungen gekennzeichnet werden.
* **Sichtbarkeit**: Der Klassifizierungsstatus der Datenbank kann in einem detaillierten Bericht im Portal angezeigt werden, der zwecks Konformität und Überwachung sowie anderer Gründe ausgedruckt oder exportiert werden kann.

## <a name="discovering-classifying--labeling-sensitive-columns"></a><a id="Discovering-classifying-labeling-sensitive-columns"></a>Ermitteln, Klassifizieren und Bezeichnen von sensiblen Spalten
Im folgenden Abschnitt werden die Schritte zum Ermitteln, Klassifizieren und Bezeichnen von Spalten mit sensiblen Daten in Ihrer Datenbank sowie das Anzeigen des aktuellen Klassifizierungsstatus Ihrer Datenbank und das Exportieren von Berichten beschrieben.

Die Klassifizierung umfasst zwei Metadatenattribute:
* Bezeichnungen: Die wichtigsten Klassifizierungsattribute zum Definieren der Vertraulichkeitsstufe der in der Spalte gespeicherten Daten.  
* Informationstypen: Sie bieten zusätzliche Granularität für den Typ der in der Spalte gespeicherten Daten.

**Klassifizieren Ihrer SQL Server-Datenbank:**

1. Stellen Sie in SSMS (SQL Server Management Studio) eine Verbindung zu SQL Server her.

2. Klicken Sie im Objekt-Explorer von SSMS mit der rechten Maustaste auf die Datenbank, die Sie klassifizieren möchten, und wählen Sie dann **Aufgaben** > **Datenermittlung und -klassifizierung** > **Daten klassifizieren…** aus.

   ![Screenshot: SSMS-Objekt-Explorer mit ausgewählter Option „Aufgaben“ > „Datenermittlung und -klassifizierung“ > „Daten klassifizieren...“][0]

3. Die Klassifizierungsengine prüft Ihre Datenbank auf Spalten, die potenziell sensible Daten enthalten, und erstellt eine Liste der **empfohlenen Spaltenklassifizierungen**:

    * Klicken Sie auf das Benachrichtigungsfeld für die Empfehlungen im oberen Rand des Fensters, oder klicken Sie auf den Bereich für Empfehlungen im unteren Rand des Fensters, um eine Liste der empfohlenen Spaltenklassifizierungen anzuzeigen:

        ![Screenshot: Benachrichtigung „Wir haben 39 Spalten mit Klassifizierungsempfehlungen gefunden. Klicken Sie hier, um sie anzuzeigen.“][2]

        ![Screenshot: Benachrichtigung „Wir haben 39 Spalten mit Klassifizierungsempfehlungen gefunden. Klicken Sie hier, um sie anzuzeigen.“][3]

    * Überprüfen Sie die Liste der Empfehlungen:
        * Zum Akzeptieren einer Empfehlung für eine bestimmte Spalte aktivieren Sie das Kontrollkästchen in der linken Spalte der entsprechenden Zeile. Sie können auch *alle Empfehlungen* als akzeptiert markieren, indem Sie das Kontrollkästchen im Tabellenkopf der Empfehlungen aktivieren.

        * Sie können auch den empfohlenen Informationstyp und die Vertraulichkeitsbezeichnung mithilfe der Dropdownfelder ändern.        

        ![Screenshot: Liste mit Empfehlungen][4]

    * Klicken Sie zum Anwenden Ihrer ausgewählten Empfehlungen auf die blaue Schaltfläche **Accept selected recommendations** (Ausgewählte Änderungen akzeptieren).

        ![Screenshot: Schaltfläche „Ausgewählte Empfehlungen annehmen“][5]

4. Alternativ oder zusätzlich zur empfehlungsbasierten Klassifizierung können Sie Spalten auch **manuell klassifizieren**:

    * Klicken Sie im oberen Menü des Fensters auf **Klassifizierung hinzufügen**.

        ![Screenshot: Menü oben mit umrandeter Option „Klassifizierung hinzufügen“][6]

    * Wählen Sie im daraufhin geöffneten Kontextfenster das Schema, die Tabelle und dann die Spalte, die Sie klassifizieren möchten, sowie den Informationstyp und die Vertraulichkeitsbezeichnung aus. Klicken Sie dann auf die Schaltfläche **Klassifizierung hinzufügen** am unteren Rand des Kontextfensters.

        ![Screenshot: Kontextfenster „Klassifizierung hinzufügen“][7]

5. Klicken Sie im oberen Menü des Fensters auf **Speichern**, um Ihre Klassifizierung zu vervollständigen und die Datenbankspalten dauerhaft mit den neuen Klassifizierungsmetadaten zu bezeichnen (Tag).

    ![Screenshot: Menü oben mit umrandeter Option „Speichern“][8]


6. Klicken Sie im oberen Menü des Fensters auf **Bericht anzeigen**, um einen Bericht mit einer vollständigen Zusammenfassung des Klassifizierungsstatus der Datenbank zu generieren. Sie können auch über SSMS einen Bericht erstellen. Klicken Sie mit der rechten Maustaste auf die Datenbank, in der der Bericht erstellt werden soll, und wählen Sie **Aufgaben** > **Datenermittlung und -klassifizierung** > **Bericht generieren…** aus.

    ![Screenshot: Menü oben mit umrandeter Option „Bericht anzeigen“][9]

    ![Screenshot: Bericht zur SQL-Datenklassifizierung][10]

## <a name="manage-information-protection-policy-with-ssms"></a><a id="Manage-information-protection-policy-with-SSMS"></a>Verwalten der Information Protection-Richtlinie mit SSMS

Sie können die Information Protection-Richtlinie mit [SSMS 18.4](../../ssms/download-sql-server-management-studio-ssms.md) oder höher verwalten:

1. Stellen Sie in SSMS (SQL Server Management Studio) eine Verbindung zu SQL Server her.

2. Klicken Sie im Objekt-Explorer in SSMS mit der rechten Maustaste auf eine Ihrer Datenbanken, und wählen Sie **Aufgaben** > **Datenermittlung- und -klassifizierung** aus.

   Mithilfe der folgenden Menüoptionen können Sie die Information Protection-Richtlinie verwalten:

* **Information Protection-Richtliniendatei festlegen:** Hier wird die Information Protection-Richtlinie wie in der ausgewählten JSON-Datei definiert verwendet.

* **Information Protection-Richtlinie exportieren:** Hier wird die Information Protection-Richtlinie in eine JSON-Datei exportiert.

* **Information Protection-Richtlinie zurücksetzen:** Hier wird die Information Protection-Richtlinie auf die Information Protection-Standardrichtlinie zurückgesetzt.

> [!IMPORTANT]
> Die Information Protection-Richtliniendatei wird nicht in SQL Server gespeichert.
> Für SSMS wird eine Information Protection-Standardrichtlinie verwendet. Wenn eine benutzerdefinierte Information Protection-Richtlinie nicht angewendet werden kann, kann in SSMS nicht die Standardrichtlinie verwendet werden. Die Datenklassifizierung schlägt fehl. Klicken Sie auf **Information Protection-Richtlinie zurücksetzen**, damit die Standardrichtlinie verwendet, das Problem behoben und die Datenklassifizierung ausgeführt wird.

## <a name="accessing-the-classification-metadata"></a><a id="sAccessing-the-classification-metadata"></a>Zugriff auf Klassifizierungsmetadaten

In SQL Server 2019 wird die Systemkatalogsicht [`sys.sensitivity_classifications`](../system-catalog-views/sys-sensitivity-classifications-transact-sql.md) eingeführt. Diese Sicht gibt Informationstypen und Vertraulichkeitsbezeichnungen zurück.

Fragen Sie für SQL Server 2019-Instanzen `sys.sensitivity_classifications` ab, um alle klassifizierten Spalten mit dazugehörigen Klassifizierungen zu überprüfen. Beispiel: 

```sql
SELECT 
    schema_name(O.schema_id) AS schema_name,
    O.NAME AS table_name,
    C.NAME AS column_name,
    information_type,
    label,
    rank,
    rank_desc
FROM sys.sensitivity_classifications sc
    JOIN sys.objects O
    ON  sc.major_id = O.object_id
    JOIN sys.columns C 
    ON  sc.major_id = C.object_id  AND sc.minor_id = C.column_id
```

In Versionen vor SQL Server 2019 werden die Klassifizierungsmetadaten für Informationstypen und Vertraulichkeitsbezeichnungen in den folgenden erweiterten Eigenschaften gespeichert: 

* `sys_information_type_name`
* `sys_sensitivity_label_name`

Bei Instanzen von SQL Server 2017 und älteren Versionen gibt das folgende Codebeispiel alle klassifizierten Spalten mit der jeweiligen Klassifizierung zurück:

```sql
SELECT
    schema_name(O.schema_id) AS schema_name,
    O.NAME AS table_name,
    C.NAME AS column_name,
    information_type,
    sensitivity_label 
FROM
    (
        SELECT
            IT.major_id,
            IT.minor_id,
            IT.information_type,
            L.sensitivity_label 
        FROM
        (
            SELECT
                major_id,
                minor_id,
                value AS information_type 
            FROM sys.extended_properties 
            WHERE NAME = 'sys_information_type_name'
        ) IT 
        FULL OUTER JOIN
        (
            SELECT
                major_id,
                minor_id,
                value AS sensitivity_label 
            FROM sys.extended_properties 
            WHERE NAME = 'sys_sensitivity_label_name'
        ) L 
        ON IT.major_id = L.major_id AND IT.minor_id = L.minor_id
    ) EP
    JOIN sys.objects O
    ON  EP.major_id = O.object_id 
    JOIN sys.columns C 
    ON  EP.major_id = C.object_id AND EP.minor_id = C.column_id
```

## <a name="permissions"></a><a id="Permissions"></a>Berechtigungen

Bei SQL Server 2019-Instanzen erfordert die Anzeige der Klassifizierung die Berechtigung **BELIEBIGE VERTRAULICHKEITSKLASSIFIZIERUNG ANZEIGEN**. Weitere Informationen finden Sie unter [Metadata Visibility Configuration](./metadata-visibility-configuration.md).

In Instanzen vor SQL Server 2019 können Sie mithilfe der Katalogsicht für erweiterte Eigenschaften ([`sys.extended_properties`](../system-catalog-views/extended-properties-catalog-views-sys-extended-properties.md)) auf die Metadaten zugreifen.

Zum Verwalten von Klassifizierungen ist die Berechtigung BELIEBIGE VERTRAULICHKEITSKLASSIFIZIERUNG ÄNDERN erforderlich. Die Berechtigung ALTER ANY SENSITIVITY CLASSIFICATION wird von der Datenbankberechtigung ALTER oder der Serverberechtigung CONTROL SERVER impliziert.

## <a name="manage-classifications"></a><a id="subheading-5"></a>Verwalten von Klassifizierungen

# <a name="t-sql"></a>[T-SQL](#tab/t-sql)
Mit T-SQL können Sie Spaltenklassifizierungen hinzufügen/entfernen sowie alle Klassifizierungen für die gesamte Datenbank abrufen.

- Hinzufügen/Aktualisieren der Klassifizierung einer oder mehrerer Spalten: [VERTRAULICHKEITSKLASSIFIZIERUNG HINZUFÜGEN](../../t-sql/statements/add-sensitivity-classification-transact-sql.md)
- Entfernen der Klassifizierung aus einer oder mehreren Spalten: [VERTRAULICHKEITSKLASSIFIZIERUNG LÖSCHEN](../../t-sql/statements/drop-sensitivity-classification-transact-sql.md)

# <a name="powershell-cmdlet"></a>[PowerShell-Cmdlet](#tab/sql-powelshell)
Sie können das Power Shell-Cmdlet verwenden, um Spaltenklassifizierungen hinzuzufügen/zu entfernen sowie alle Klassifizierungen abzurufen und Empfehlungen für die gesamte Datenbank zu erhalten.

- [Get-SqlSensitivityClassification](/powershell/module/sqlserver/Get-SqlSensitivityClassification)
- [Get-SqlSensitivityRecommendations](/powershell/module/sqlserver/Get-SqlSensitivityRecommendations)
- [Set-SqlSensitivityClassification](/powershell/module/sqlserver/Set-SqlSensitivityClassification)
- [Remove-SqlSensitivityClassification](/powershell/module/sqlserver/Remove-SqlSensitivityClassification)

## <a name="next-steps"></a><a id="Next-steps"></a>Nächste Schritte

Weitere Informationen zur Datenermittlung und -klassifizierung in Azure SQL-Datenbanken finden Sie unter [Azure SQL-Datenbank – Datenermittlung und -klassifizierung](/azure/azure-sql/database/data-discovery-and-classification-overview).

Ziehen Sie in Betracht, Ihre sensiblen Spalten durch Anwenden von Sicherheitsmechanismen auf der Spaltenebene zu schützen:

* [Dynamische Datenmaskierung](./dynamic-data-masking.md) zum Verbergen der verwendeten sensiblen Spalten.
* [Immer verschlüsselt](./encryption/always-encrypted-database-engine.md) zum Verschlüsseln der sensiblen Spalten im Ruhezustand.

<!--Anchors-->
[SQL Data Discovery & Classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Manage information protection policy with SSMS]: #subheading-3
[Accessing the classification metadata]: #subheading-4
[Manage classifications]: #subheading-5
[Next Steps]: #subheading-6

<!--Image references-->

[0]: ./media/sql-data-discovery-and-classification/0-data-classification-explorer.png
[2]: ./media/sql-data-discovery-and-classification/2-recommendations-notification-box.png
[3]: ./media/sql-data-discovery-and-classification/3-recommendations-panel.png
[4]: ./media/sql-data-discovery-and-classification/4-recommendations.png
[5]: ./media/sql-data-discovery-and-classification/5-accept-recommendations-button.png
[6]: ./media/sql-data-discovery-and-classification/6-add-classification-button.png
[7]: ./media/sql-data-discovery-and-classification/7-manual-classification.png
[8]: ./media/sql-data-discovery-and-classification/8-save.png
[9]: ./media/sql-data-discovery-and-classification/9-view-report.png
[10]: ./media/sql-data-discovery-and-classification/10-report.png
