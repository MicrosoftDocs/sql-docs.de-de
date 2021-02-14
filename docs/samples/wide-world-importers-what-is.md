---
title: Wide World-Import Programme-Beispieldatenbank für SQL | Microsoft-Dokumentation
description: Erfahren Sie, wie die Beispiel Datenbanken "wideworldimporters" die Workflows des fiktiven Unternehmens "wideworldimporters" unterstützen.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f844299a392f9521afe3af7d72bc72272304d3a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354047"
---
# <a name="wide-world-importers-sample-databases-for-microsoft-sql"></a>Beispiel Datenbanken für Wide World-Import Programme für Microsoft SQL
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
Dies ist eine Übersicht über die fiktiven unternehmensweiten Unternehmen und die Workflows, die in den wideworldimporters-Beispiel Datenbanken für SQL Server und Azure SQL-Datenbank adressiert sind.  

Wide World Importeurs (WWI) ist ein Großhändler und Verteiler der Groß-und herfach Produkte aus dem San Francisco-Bay-Bereich.

Als Großhändler sind die Kunden von WWLAN größtenteils Unternehmen, die an Einzelpersonen weiterverkaufen. WWI verkauft sich für Einzelhandelskunden in der USA, einschließlich der speziellen Geschäfte, der Unternehmen, der computergeschäfte, der touristischen Attraktionen und einiger Personen. WWI wird auch über ein Netzwerk von Agents an andere Händler übertragen, die die Produkte im Auftrag von WWI herauf Stufen. Während alle WWI-Kunden zurzeit auf dem USA basieren, ist das Unternehmen in der Absicht, eine Erweiterung in andere Länder zu übernehmen.

WWI kauft waren von Lieferanten, einschließlich der Neuheiten und Hersteller von Herstellern und anderer Großhändler. Sie stellen die waren in Ihrem WWI-Warehouse zusammen und ordnen Sie nach Bedarf von Lieferanten neu an, um Kunden Bestellungen zu erfüllen. Außerdem erwerben Sie große Mengen von Verpackungsmaterialien und verkaufen diese in kleineren Mengen, um Sie an die Kunden zu senden.

Vor kurzem begann WWI, eine Vielzahl von genießbaren Neuerungen wie Chilli-Pralinen zu verkaufen.  Das Unternehmen musste zuvor keine gekühlten Elemente verarbeiten. Um die Anforderungen an die Lebensmittel Behandlung zu erfüllen, müssen Sie nun die Temperatur in Ihrem Kühlraum und alle LKW überwachen, die über Chiller-Abschnitte verfügen.

## <a name="workflow-for-warehouse-stock-items"></a>Workflow für Lagerbestand Teile

Der typische Ablauf bei der Art und Weise, wie Elemente aufgefüllt und verteilt werden, lautet wie folgt:
- WWI erstellt Bestellungen und übermittelt die Aufträge an die Lieferanten.
- Lieferanten senden die Elemente, die von WWI empfangen und in Ihrem Lager gespeichert werden.
- Kunden bestellen Elemente aus WWI
- WWI füllt die Kundenbestellung mit Bestands Artikeln im Warehouse aus, und wenn Sie nicht über genügend Aktien verfügen, ordnen Sie die zusätzlichen Aktien von den Lieferanten an.
- Einige Kunden möchten nicht auf Elemente warten, die nicht im Lager sind. Wenn Sie beispielsweise fünf unterschiedliche Bestandteile anordnen und vier verfügbar sind, möchten Sie die vier Elemente erhalten und das restliche Element neu anordnen. Das Element wird dann später in einer separaten Lieferung gesendet.
- WWI-Rechnungen Kunden für die Aktien Elemente, in der Regel durch die Umstellung der Bestellung in eine Rechnung.
- Kunden können Elemente anordnen, die nicht im Lager sind. Diese Elemente sind Rück stellbar.
- WWI liefert Bestands Elemente entweder über ihre eigenen Liefer Lieferwagen oder über andere Schulungs-oder Fracht Methoden an Kunden.
- Kunden bezahlen Rechnungen an WWI.
- In regelmäßigen Abständen bezahlt WWI Lieferanten für Elemente, die in Bestellungen waren. Dies ist häufig der Fall, wenn Sie die waren empfangen haben.

## <a name="data-warehouse-and-analysis-workflow"></a>Workflow für Data Warehouse und Analyse

Obwohl das Team bei WWI SQL Server Reporting Services verwendet, um operative Berichte aus der Datenbank "wideworldimporters" zu generieren, müssen Sie auch Analysen der Daten durchführen und strategische Berichte generieren. Das Team hat ein dimensionales Datenmodell in einer Datenbank wideworldimportersdw erstellt. Diese Datenbank wird von einem Integration Services Paket aufgefüllt.

SQL Server Analysis Services wird verwendet, um analytische Datenmodelle aus den Daten im dimensionalen Datenmodell zu erstellen. SQL Server Reporting Services wird zum Generieren von strategischen Berichten direkt aus dem dimensionalen Datenmodell und auch aus dem analytischen Modell verwendet. Power BI wird verwendet, um Dashboards aus denselben Daten zu erstellen. Die Dashboards werden auf Websites und Smartphones und Tablets verwendet. *Hinweis: Diese Datenmodelle und Berichte sind noch nicht verfügbar.*

## <a name="additional-workflows"></a>Weitere Workflows

Dabei handelt es sich um zusätzliche Workflows.
- WWI gibt Gutschriften aus, wenn ein Kunde den guten aus irgendeinem Grund nicht erhält oder wenn die waren fehlerhaft sind. Diese werden als negative Rechnungen behandelt.
- WWI zählt in regelmäßigen Abständen die Hand Mengen von Aktien Elementen, um sicherzustellen, dass die im System verfügbaren Aktien Mengen korrekt sind. (Dieser Vorgang wird als stocktake bezeichnet.)
- Kalt Raumtemperatur. Nicht unter Scheid Bare waren werden in Kühlräumen gespeichert. Sensor Daten aus diesen Räumen werden zu Überwachungs-und Analysezwecken in die Datenbank aufgenommen.
- Fahrzeugspeicherort Nachverfolgung. Fahrzeuge, die waren für WWI transportieren, beinhalten Sensoren, die den Standort nachverfolgen. Dieser Speicherort wird für die Überwachung und weitere Analyse in der Datenbank wieder aufgenommen.

## <a name="fiscal-year"></a>Geschäftsjahr

Das Unternehmen arbeitet mit einem Finanz Jahr, das am 1. November beginnt.

## <a name="terms-of-use"></a>Nutzungsbedingungen

Die Lizenz für die-Beispieldatenbank und den Beispielcode finden Sie hier: [license.txt](https://github.com/Microsoft/sql-server-samples/blob/master/license.txt)

Die Beispieldatenbank enthält öffentliche Daten, die aus Data.gov und natürlichen erddaten geladen wurden. Die Nutzungsbedingungen lauten wie folgt: [https://www.naturalearthdata.com/about/terms-of-use/](https://www.naturalearthdata.com/about/terms-of-use/)
