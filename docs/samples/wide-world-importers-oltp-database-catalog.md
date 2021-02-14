---
title: Wideworldimporters OLTP-Daten Bank Katalog-SQL | Microsoft-Dokumentation
description: Informationen zu Schemas, Tabellen, gespeicherten Prozeduren und Entwurfs Überlegungen für den Daten Bank Katalog "wideworldimporters".
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 991f001ea8fd43298cefe1f9ba542472de6935ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354089"
---
# <a name="wideworldimporters-database-catalog"></a>Wideworldimporters-Daten Bank Katalog
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
Die Datenbank "wideworldimporters" enthält alle Transaktionsinformationen und täglichen Daten für Verkäufe und Einkäufe sowie Sensordaten für Fahrzeuge und kalte Räume.

## <a name="schemas"></a>Schemas

Wideworldimporters verwendet Schemas für verschiedene Zwecke, z. b. das Speichern von Daten, das definieren, wie Benutzer auf die Daten zugreifen können, und das Bereitstellen von Objekten für die Data Warehouse Entwicklung und Integration.

### <a name="data-schemas"></a>Datenschemas

Diese Schemas enthalten die Daten. Eine Reihe von Tabellen werden von allen anderen Schemas benötigt und befinden sich im Anwendungsschema.

|Schema|BESCHREIBUNG|
|-----------------------------|---------------------|
|Application|Anwendungs weite Benutzer, Kontakte und Parameter. Dies enthält auch Verweis Tabellen mit Daten, die von mehreren Schemas verwendet werden.|
|Erwerb|Bestands Artikel Käufe von Lieferanten und Details zu Zulieferern.|  
|Sales|Bestands Element Verkäufe bei Einzelhandelskunden und Details zu Kunden und Vertriebsmitarbeitern. |  
|Warehouse|Inventur und Transaktionen von Aktien Elementen.|  

### <a name="secure-access-schemas"></a>Sichere Zugriffs Schemas

Diese Schemas werden für externe Anwendungen verwendet, die nicht direkt auf die Datentabellen zugreifen dürfen. Sie enthalten Sichten und gespeicherte Prozeduren, die von externen Anwendungen verwendet werden.

|Schema|BESCHREIBUNG|
|-----------------------------|---------------------|
|Website|Der gesamte Zugriff auf die Datenbank von der Unternehmenswebsite erfolgt über dieses Schema.|
|Berichte|Der gesamte Zugriff auf die Datenbank aus Reporting Services Berichten erfolgt über dieses Schema.|
|PowerBI|Der gesamte Zugriff auf die Datenbank aus den Power BI Dashboards über das Enterprise-Gateway erfolgt über dieses Schema.|

Beachten Sie, dass die Berichte und die powerbi-Schemas in der ersten Version der-Beispieldatenbank nicht verwendet werden. Alle Reporting Services-und Power BI Beispiele, die auf dieser Datenbank basieren, werden jedoch empfohlen, diese Schemas zu verwenden.

### <a name="development-schemas"></a>Entwicklungs Schemas

Sonderschemas

|Schema|BESCHREIBUNG|
|-----------------------------|---------------------|
|Integration|Für Data Warehouse Integration erforderliche Objekte und Prozeduren (d. h. die Daten werden in die wideworldimportersdw-Datenbank migriert).|
|Sequenzen|Enthält Sequenzen, die von allen Tabellen in der Anwendung verwendet werden.|

## <a name="tables"></a>Tabellen

Alle Tabellen in der Datenbank befinden sich in den Daten Schemas.

### <a name="application-schema"></a>Anwendungs Schema

Details zu Parametern und Personen (Benutzer und Kontakte) zusammen mit allgemeinen Verweis Tabellen (gemeinsame Verwendung mehrerer anderer Schemas).

|Tabelle|BESCHREIBUNG|
|-----------------------------|---------------------|
|SystemParameters|Enthält systemweite konfigurierbare Parameter.|
|Personen|Enthält Benutzernamen, Kontaktinformationen, für alle Benutzer, die die Anwendung verwenden, und für die Personen, mit denen sich die weltweiten Importierer bei Kundenorganisationen beschäftigt. Dazu zählen Mitarbeiter, Kunden, Lieferanten und andere Kontakte. Für Personen, denen die Berechtigung erteilt wurde, das System oder die Website zu verwenden, enthält die Informationen Anmelde Informationen.|
|Cities (Städte)|Im System werden viele Adressen gespeichert, für Personen, Lieferadressen der Kundenorganisation, Abhol Adressen bei Lieferanten usw. Wenn eine Adresse gespeichert wird, gibt es einen Verweis auf eine Stadt in dieser Tabelle. Es gibt auch einen räumlichen Standort für jede Stadt.|
|Stateprovinzen|Städte sind Teil der Bundesstaaten oder Provinzen. Diese Tabelle enthält Details zu diesen, einschließlich räumlicher Daten, die die Grenzen der einzelnen Bundesstaaten oder Provinzen beschreiben.|
|Länder|Staaten oder Provinzen sind Teil der Länder. Diese Tabelle enthält Details zu diesen, einschließlich räumlicher Daten, die die Grenzen der einzelnen Länder beschreiben.|
|DeliveryMethods|Auswahlmöglichkeiten für die Bereitstellung von Aktien Elementen (z. b. LKW/van, Post, Pickup, Courier usw.)|
|Paymentmethods|Auswahlmöglichkeiten für Zahlungen (z. b. Bargeld, Überprüfung, EFT usw.)|
|Transaktionstypen|Typen von Kunden-, Lieferanten-oder Aktientransaktionen (z. b. Rechnung, Gutschrift usw.)|

### <a name="purchasing-schema"></a>Kauf Schema

Details zu Lieferanten und Bestands Element Käufen.

|Tabelle|BESCHREIBUNG|
|-----------------------------|---------------------|
|Suppliers|Haupt Entitäts Tabelle für Zulieferer (Organisationen)|
|Suppliercategories|Kategorien für Lieferanten (z. b. Neuheiten, Toys, Bekleidung, Verpacken usw.)|
|Suppliertransactions|Alle im Lieferanten zusammenhängenden Finanztransaktionen (Rechnungen, Zahlungen)|
|"PurchaseOrders|Details zu Lieferanten Bestellungen|
|Purchaseorderlines|Detail Zeilen von Lieferanten Bestellungen|

 
### <a name="sales-schema"></a>Sales-Schema

Details zu Kunden, Vertriebsmitarbeitern und Bestands Element Verkäufen.

|Tabelle|BESCHREIBUNG|
|-----------------------------|---------------------|
|Kunden|Haupt Entitäts Tabellen für Kunden (Organisationen oder Einzelpersonen)|
|Customercategories|Kategorien für Kunden (z. b. Neuheits Geschäfte, Supermärkte usw.)|
|Buyinggroups|Kundenorganisationen können Teil von Gruppen sein, die eine größere Kauf Leistung haben.|
|Customertransactions|Alle Finanztransaktionen, die Kunden bezogen sind (Rechnungen, Zahlungen)|
|Specialdeals|Sonderpreise. Dies kann festes Preis, Rabatt in Dollar oder Rabatt in Prozent enthalten.|
|Orders|Details der Kunden Bestellungen|
|OrderLines|Detail Zeilen von Kunden Bestellungen|
|Invoices|Details zu Kunden Rechnungen|
|Invoicelines|Detail Zeilen von Kunden Rechnungen|

### <a name="warehouse-schema"></a>Warehouse-Schema

Details zu Aktien Elementen, deren Bestand und Transaktionen.

|Tabelle|BESCHREIBUNG|
|-----------------------------|---------------------|
|Stockitems|Haupt Entitäts Tabelle für Aktien Elemente|
|Stockitemholdings|Nicht Temporale Spalten für Aktien Elemente. Dabei handelt es sich um häufig aktualisierte Spalten.|
|Stockgroups|Gruppen zum kategorialisieren von Aktien Elementen (z. b. Neuheiten, Spielsachen, essbare Neuheiten usw.)|
|Stockitemstockgroups|Welche Aktien Elemente sind in welchen Aktien Gruppen (n zu viele)|
|Farben|Aktien Elemente können (optional) über Farben verfügen.|
|PackageTypes|Möglichkeiten zum Packen von Bestands Elementen (z. b. Box, Karton, Palette, kg usw.)|
|Stockitemtransactions|Transaktionen, die alle Bewegungen aller Aktien Elemente abdecken (Receipt, Sale, Write-off)|
|Vehicletemperaturen|Regelmäßig aufgezeichnete Temperatur von fahrzeugchillern|
|Coldroomtemperaturen|Regelmäßig aufgezeichnete Temperaturen von kalt Raum chillern|


## <a name="design-considerations"></a>Überlegungen zum Entwurf

Der Daten bankentwurf ist subjektiv, und es gibt keine Rechte oder falsche Methode zum Entwerfen einer Datenbank. In den Schemas und Tabellen in dieser Datenbank werden Ideen zum Entwerfen Ihrer eigenen Datenbank angezeigt.

### <a name="schema-design"></a>Schema Entwurf

Wideworldimporters verwendet eine kleine Anzahl von Schemas, sodass es einfach ist, das Datenbanksystem zu verstehen und Daten bankprinzipien zu veranschaulichen.  

Wenn möglich, werden Tabellen, die häufig zusammen im selben Schema abgefragt werden, von der Datenbank zusammengefasst, um die Komplexität des Joins zu minimieren.

Das Datenbankschema wurde basierend auf einer Reihe von Metadatentabellen in einem anderen Daten Bank WWI_Preparation Code generiert. Dadurch erhält wideworldimporters einen sehr hohen Grad an Entwurfs Konsistenz, namens Konsistenz und Vollständigkeit. Ausführliche Informationen zur Generierung des Schemas finden Sie im Quellcode: [Wide-World-importierungs-/WWI-Database-Scripts](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/sample-scripts) .

### <a name="table-design"></a>Tabellenentwurf

- Alle Tabellen haben Primärschlüssel für einzelne Spalten für die Einfachheit der Verknüpfung.
- Alle Schemas, Tabellen, Spalten, Indizes und Check-Einschränkungen verfügen über eine erweiterte Beschreibungs Eigenschaft, die zum Identifizieren des Zwecks des Objekts oder der Spalte verwendet werden kann. Speicher optimierte Tabellen stellen eine Ausnahme dar, da Sie zurzeit keine erweiterten Eigenschaften unterstützen.
- Alle Fremdschlüssel werden automatisch indiziert, es sei denn, es gibt einen anderen nicht gruppierten Index, der über dieselbe Linke Komponente verfügt.
- Die automatische Nummerierung in Tabellen basiert auf Sequenzen. Diese Sequenzen sind für verknüpfte Server und ähnliche Umgebungen einfacher als Identitäts Spalten. Für Speicher optimierte Tabellen werden Identitäts Spalten verwendet, da Sie in SQL Server 2016 nicht unterstützt werden.
- Eine einzelne Sequenz (transaktionid) wird für diese Tabellen verwendet: customertransactions, suppliertransactions und stockitemtransactions. Dadurch wird veranschaulicht, wie eine Gruppe von Tabellen eine einzelne Sequenz aufweisen kann.
- Einige Spalten verfügen über geeignete Standardwerte.

### <a name="security-schemas"></a>Sicherheits Schemas

Aus Sicherheitsgründen lässt wideworldimporters nicht zu, dass externe Anwendungen direkt auf Daten Schemas zugreifen. Um den Zugriff zu isolieren, verwendet wideworldimporters Sicherheits Zugriffs Schemas, die keine Daten enthalten, aber Sichten und gespeicherte Prozeduren enthalten. Externe Anwendungen verwenden die Sicherheits Schemas, um die Daten abzurufen, die Sie anzeigen dürfen.  Auf diese Weise können Benutzer nur die Sichten und gespeicherten Prozeduren in den Schemas für den sicheren Zugriff ausführen.

Dieses Beispiel enthält z. b. Power BI Dashboards. Eine externe Anwendung greift auf diese Power BI Dashboards vom Power BI Gateway aus als Benutzer, der über die Berechtigung schreibgeschützt für das Power BI-Schema verfügt.  Für die schreibgeschützte Berechtigung benötigt der Benutzer nur die SELECT-und EXECUTE-Berechtigung für das Power BI-Schema. Ein Datenbankadministrator bei WWI weist diese Berechtigungen nach Bedarf zu.

## <a name="stored-procedures"></a>Gespeicherte Prozeduren

Gespeicherte Prozeduren werden in Schemas organisiert. Die meisten Schemas werden für Konfigurations-oder Beispiel Zwecke verwendet.

Das `Website` Schema enthält die gespeicherten Prozeduren, die von einem Web-Front-End verwendet werden können.

Die `Reports` `PowerBI` Schemas und sind für Reporting Services und Power BI vorgesehen. Es wird empfohlen, diese Schemas für Berichts Zwecke zu verwenden.

### <a name="website-schema"></a>Website Schema

Dies sind die Verfahren, die von einer Client Anwendung verwendet werden, z. b. ein Web-Front-End.

|Prozedur|Zweck|
|-----------------------------|---------------------|
|Activatewebsitelogon|Ermöglicht es einer Person (von `Application.People` ), Zugriff auf die Website zu haben.|
|ChangePassword|Ändert das Kennwort eines Benutzers (für Benutzer, die keine externen Authentifizierungsmechanismen verwenden).|
|Insertcustomerorders|Ermöglicht das Einfügen von einem oder mehreren Kunden Bestellungen (einschließlich der Bestell Zeilen).|
|Invoicecustomerorders|Führt eine Liste der Bestellungen aus, die in Rechnung gestellt werden, und verarbeitet die Rechnungen.|
|Recordcoldroomtemperaturen|Nimmt eine Sensordaten Liste als Tabellenwert Parameter (TVP) an und wendet die Daten auf die `Warehouse.ColdRoomTemperatures` Temporale Tabelle an.|
|Recordvehicletemperatur|Nimmt ein JSON-Array an und verwendet es, um zu aktualisieren `Warehouse.VehicleTemperatures` .|
|Searchforcustomers|Sucht nach Kunden anhand des Namens oder eines Teils des Namens (entweder Firmenname oder Personen Name).|
|Searchforpeople|Sucht nach Personen anhand des Namens oder eines Teils des Namens.|
|Searchforstockitems|Sucht nach Aktien Elementen anhand des Namens oder eines Teils der Namens-oder Marketing Kommentare.|
|Searchforstockitemsbytags|Sucht nach Aktien Elementen nach Tags.|
|Searchforsuppliers|Sucht nach Lieferanten anhand des Namens oder eines Teils des Namens (entweder Firmenname oder Personen Name).|

### <a name="integration-schema"></a>Integrations Schema

Die gespeicherten Prozeduren in diesem Schema werden vom ETL-Prozess verwendet. Sie erhalten die benötigten Daten aus verschiedenen Tabellen für den Zeitraum, der für das [ETL-Paket](wide-world-importers-perform-etl.md)erforderlich ist.

### <a name="dataloadsimulation-schema"></a>Dataloadsimulation-Schema

Simuliert eine Arbeitsauslastung, die Verkäufe und Einkäufe einfügt. Die Haupt gespeicherte Prozedur ist `PopulateDataToCurrentDate` , mit der Beispiel Daten bis zum aktuellen Datum eingefügt werden.

|Prozedur|Zweck|
|-----------------------------|---------------------|
|Configuration_ApplyDataLoadSimulationProcedures|Erstellt die für die Daten Lade Simulation erforderlichen Prozeduren neu. Dies ist erforderlich, um Daten bis zum aktuellen Datum zu verschieben.|
|Configuration_RemoveDataLoadSimulationProcedures|Dadurch werden die Prozeduren nach Abschluss der Daten Simulation wieder entfernt.|
|"De activatetemporaltablesbeforedataload"|Entfernt die zeitliche Natur aller temporalen Tabellen und wendet ggf. einen---------------------Typ an|
|Populatedatatocurrentdate|Wird verwendet, um die Daten bis zum aktuellen Datum zu verschieben. Sollte vor allen anderen Konfigurationsoptionen ausgeführt werden, nachdem die Datenbank aus einer ersten Sicherung wieder hergestellt wurde.|
|Reactivatetemporaltablesafterdataload|Stellt die temporalen Tabellen wieder her, einschließlich der Überprüfung der Datenkonsistenz. (Entfernt die zugeordneten Trigger).|


### <a name="application-schema"></a>Anwendungs Schema

Diese Prozeduren werden verwendet, um das Beispiel zu konfigurieren. Sie dienen zum Anwenden von Enterprise Edition-Features auf die Standard Edition-Version des Beispiels sowie zum Hinzufügen von Überwachungen und Volltextindizierung.

|Prozedur|Zweck|
|-----------------------------|---------------------|
|Addrolememberibernonstant|Fügt einer Rolle ein Mitglied hinzu, wenn der Member nicht bereits in der Rolle vorhanden ist.|
|Configuration_ApplyAuditing|Fügt eine Überwachung hinzu. Die Server Überwachung wird für Standard Edition-Datenbanken angewendet. für Enterprise Edition wird eine zusätzliche Daten Bank Überwachung hinzugefügt.|
|Configuration_ApplyColumnstoreIndexing|Wendet die columnstore--Indizierung `Sales.OrderLines` entsprechend auf und und neu auf `Sales.InvoiceLines` .|
|Configuration_ApplyFullTextIndexing|Wendet Volltextindizes auf `Application.People` , `Sales.Customers` , `Purchasing.Suppliers` und an `Warehouse.StockItems` . Ersetzt `Website.SearchForPeople` , `Website.SearchForSuppliers` , `Website.SearchForCustomers` , `Website.SearchForStockItems` `Website.SearchForStockItemsByTags` durch Ersatz Prozeduren, die die Volltextindizierung verwenden.|
|Configuration_ApplyPartitioning|Wendet die Tabellenpartitionierung auf `Sales.CustomerTransactions` und an `Purchasing.SupplierTransactions` und ordnet die Indizes entsprechend an.|
|Configuration_ApplyRowLevelSecurity|Wendet die Sicherheit auf Zeilenebene an, um Kunden nach Vertriebs Gebiets bezogenen Rollen zu filtern.|
|Configuration_ConfigureForEnterpriseEdition|Wendet die columnstore--Indizierung, den vollständigen Text, den in-Memory, polybase und die Partitionierung an.|
|Configuration_EnableInMemory|Fügt eine Speicher optimierte Datei Gruppe hinzu (wenn Sie nicht in Azure verwendet wird), ersetzt `Warehouse.ColdRoomTemperatures` `Warehouse.VehicleTemperatures` durch in-Memory-Entsprechungen und migriert die Daten, erstellt `Website.OrderIDList` die `Website.OrderList` `Website.OrderLineList` Tabellentypen,,, `Website.SensorDataList` mit Speicher optimierten Entsprechungen, löscht und erstellt sie neu und erstellt die Prozeduren, und, die `Website.InvoiceCustomerOrders` `Website.InsertCustomerOrders` `Website.RecordColdRoomTemperatures` diese Tabellentypen verwenden.|
|Configuration_RemoveAuditing|Entfernt die Überwachungskonfiguration.|
|Configuration_RemoveRowLevelSecurity|Entfernt die Sicherheitskonfiguration auf Zeilenebene (Dies wird für Änderungen an den zugeordneten Tabellen benötigt).|
|"Kreateroleinicht verlassen"|Erstellt eine Daten Bank Rolle, wenn Sie nicht bereits vorhanden ist.|


### <a name="sequences-schema"></a>Sequenz Schema

Prozeduren zum Konfigurieren der Sequenzen in der Datenbank.

|Prozedur|Zweck|
|-----------------------------|---------------------|
|Reseedallsequenzen|Ruft die Prozedur reseedsequencebeyondtablevalue für alle Sequenzen auf.|
|Reseedsequencebeyondtablevalue|Wird verwendet, um den nächsten Sequenzwert hinter dem Wert in einer Tabelle, die dieselbe Sequenz verwendet, neu zu positionieren. (Z. b. eine DBCC CHECKIDENT für Identitäts Spalten, die für Sequenzen gleichwertig sind, aber über potenziell mehrere Tabellen hinweg|
