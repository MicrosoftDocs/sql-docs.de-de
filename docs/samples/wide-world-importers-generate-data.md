---
title: Generieren von Daten in SQL-Beispielen wideworldimporters
description: Verwenden Sie diese SQL-Anweisungen zum Generieren und Importieren von Beispiel Daten bis zum aktuellen Datum der wideworldimporters-Beispiel Datenbanken.
ms.date: 10/23/2020
ms.reviewer: ''
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: f54370d9aa54602964817fc17704a324134c0757
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354102"
---
# <a name="wideworldimporters-data-generation"></a>Datengenerierung von wideworldimporters
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
Die veröffentlichten Versionen der Datenbanken wideworldimporters und wideworldimportersdw verfügen über Daten vom 1. Januar 2013 bis zum Tag, an dem die Datenbanken generiert wurden.

Wenn Sie diese Beispiel Datenbanken verwenden, möchten Sie möglicherweise aktuellere Beispiel Daten einschließen.

## <a name="data-generation-in-wideworldimporters"></a>Datengenerierung in wideworldimporters

So generieren Sie Beispiel Daten bis zum aktuellen Datum:

1. Wenn Sie dies noch nicht getan haben, installieren Sie eine saubere Version der wideworldimporters-Datenbank. Installationsanweisungen finden Sie unter [Installation und Konfiguration](wide-world-importers-oltp-install-configure.md).
2. Führen Sie die folgende Anweisung in der Datenbank aus:

    ```
        EXECUTE DataLoadSimulation.PopulateDataToCurrentDate
            @AverageNumberOfCustomerOrdersPerDay = 60,
            @SaturdayPercentageOfNormalWorkDay = 50,
            @SundayPercentageOfNormalWorkDay = 0,
            @IsSilentMode = 1,
            @AreDatesPrinted = 1;
    ```

    Diese Anweisung fügt der Datenbank bis zum aktuellen Datum Beispiel Verkäufe und Kauf Daten hinzu. Der Fortschritt der Datengenerierung nach Tag wird angezeigt. Aufgrund eines zufälligen Faktors bei der Datengenerierung gibt es einige Unterschiede in den Daten, die zwischen den Ausführungen generiert werden.

    Ändern Sie den Wert für den-Parameter, um die Datenmenge zu erhöhen oder zu verringern, die für Bestellungen pro Tag generiert wurde `@AverageNumberOfCustomerOrdersPerDay` . Verwenden Sie die Parameter `@SaturdayPercentageOfNormalWorkDay` und `@SundayPercentageOfNormalWorkDay` , um das Bestell Volume für Wochentage festzulegen.

> [!TIP]
> Das Erzwingen der [verzögerten Dauerhaftigkeit](../relational-databases/logs/control-transaction-durability.md) in der Datenbank kann die Daten Generierungs Geschwindigkeit verbessern, insbesondere wenn sich das Daten Bank Transaktionsprotokoll auf einem Speichersubsystem mit hoher Latenz befindet Beachten Sie bei der Verwendung von verzögerter Dauerhaftigkeit die Auswirkungen auf potenzielle [Datenverluste](../relational-databases/logs/control-transaction-durability.md#bkmk_DataLoss) , und ziehen Sie in Erwägung, die verzögerte Dauerhaftigkeit der Datengenerierung zu aktivieren.

## <a name="import-generated-data-in-wideworldimportersdw"></a>Importieren generierter Daten in wideworldimportersdw

So importieren Sie Beispiel Daten bis zum aktuellen Datum in der OLAP-Datenbank "wideworldimportersdw":

1. Führen Sie die Daten Generierungs Logik in der wideworldimporters-OLTP-Datenbank aus, indem Sie die Schritte im vorherigen Abschnitt verwenden.
2. Wenn Sie dies noch nicht getan haben, installieren Sie eine saubere Version der wideworldimportersdw-Datenbank. Installationsanweisungen finden Sie unter [Installation und Konfiguration](wide-world-importers-oltp-install-configure.md).
3. Führen Sie die folgende Anweisung in der Datenbank aus, um die OLAP-Datenbank neu zu starten:

    ```sql
    EXECUTE [Application].Configuration_ReseedETL
    ```

4. Führen Sie das *tägliche ETL. ispac* -SQL Server Integration Services Paket aus, um die Daten in die OLAP-Datenbank zu importieren. Informationen zum Ausführen des ETL-Auftrags finden Sie unter [wideworldimporters ETL-Workflow](wide-world-importers-perform-etl.md).

## <a name="generate-data-in-wideworldimportersdw-for-performance-testing"></a>Generieren von Daten in wideworldimportersdw für Leistungstests

Wideworldimportersdw kann die Datengröße für Leistungstests beliebig erhöhen. Beispielsweise kann die Datengröße für die Verwendung mit der gruppierten columnstore--Indizierung erhöht werden.

Eine der Herausforderungen besteht darin, die Größe des Downloads klein genug zu halten, um problemlos herunterzuladen, aber groß genug, um SQL Server Leistungs Features zu veranschaulichen. Beispielsweise werden bedeutende Vorteile für columnstore--Indizes nur erreicht, wenn Sie mit einer größeren Anzahl von Zeilen arbeiten. 

Mit dem Verfahren können Sie `Application.Configuration_PopulateLargeSaleTable` die Anzahl der Zeilen in der Tabelle erhöhen `Fact.Sale` . Die Zeilen werden in das 2012-Kalenderjahr eingefügt, um zu vermeiden, dass mit den vorhandenen weltweiten importierungsdaten, die am 1. Januar 2013 beginnen, kollidieren

### <a name="procedure-details"></a>Prozedur Details

#### <a name="name"></a>Name

`Application.Configuration_PopulateLargeSaleTable`

#### <a name="parameters"></a>Parameter

`@EstimatedRowsFor2012`**bigint** (mit dem Standardwert 12 Millionen)

#### <a name="result"></a>Ergebnis

Ungefähr die erforderliche Anzahl von Zeilen wird `Fact.Sale` in die Tabelle im Jahr 2012 eingefügt. Die Prozedur schränkt die Anzahl der Zeilen künstlich auf 50.000 pro Tag ein. Sie können diese Einschränkung ändern, aber die Einschränkung hilft Ihnen, versehentliche overinflations der Tabelle zu vermeiden.

Die Prozedur wendet auch eine gruppierte columnstore--Indizierung an, wenn Sie noch nicht angewendet wurde.
