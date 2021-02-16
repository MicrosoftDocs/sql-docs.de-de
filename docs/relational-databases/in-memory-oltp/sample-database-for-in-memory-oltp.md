---
title: Beispieldatenbank für In-Memory OLTP | Microsoft-Dokumentation
description: Informieren Sie sich über das In-Memory-OLTP-Feature und die zugehörigen Leistungsvorteile. Dieses Beispiel zeigt speicheroptimierte Tabellen und nativ kompilierte gespeicherte Prozeduren.
ms.custom: ''
ms.date: 12/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: df347f9b-b950-4e3a-85f4-b9f21735eae3
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 67f29ef6d47de5cc14cb248d20a243053857c9e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353640"
---
# <a name="sample-database-for-in-memory-oltp"></a>Beispieldatenbank für In-Memory OLTP
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
    
## <a name="overview"></a>Übersicht  
 In diesem Beispiel wird das In-Memory OLTP-Feature vorgestellt. Es zeigt speicheroptimierte Tabellen sowie nativ kompilierte gespeicherte Prozeduren und kann verwendet werden, um die Leistungsvorteile von In-Memory OLTP zu veranschaulichen.  
  
> [!NOTE]  
>  Informationen zum Anzeigen dieses Themas für [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]finden Sie unter [Erweiterungen von AdventureWorks zur Veranschaulichung von In-Memory OLTP](./overview-and-usage-scenarios.md#in-memory-oltp-overview).  
  
 Im Beispiel werden fünf Tabellen aus der AdventureWorks-Datenbank zu speicheroptimierten Tabellen migriert. Zusätzlich enthält es eine exemplarische Arbeitsauslastung zur Abwicklung von Verkaufsaufträgen. Die exemplarische Arbeitsauslastung veranschaulicht die Leistungsvorteile von In-Memory OLTP auf dem Server.  
  
 In der Beschreibung des Beispiels wird erläutert, welche Features bei der Migration der Tabellen zu In-Memory OLTP nicht ausgeschöpft werden konnten, weil sie für speicheroptimierte Tabellen derzeit (noch) nicht unterstützt werden.  
  
 Die Dokumentation dieses Beispiels ist wie folgt gegliedert:  
  
-   [Erforderliche Komponenten](#Prerequisites) für die Installation des Beispiels und die Ausführung der exemplarischen Arbeitsauslastung  
  
-   Anweisungen für [Installieren des auf AdventureWorks basierenden InMemory OLTP-Beispiels](#InstallingtheIn-MemoryOLTPsamplebasedonAdventureWorks)  
  
-   [Beschreibung der Beispieltabellen und -prozeduren](#Descriptionofthesampletablesandprocedures) einschließlich einer Beschreibung der Tabellen und Verfahren, die AdventureWorks durch das Beispiel für In-Memory OLTP hinzugefügt werden, sowie Überlegungen zur Migration einiger ursprünglicher AdventureWorks-Tabellen zu speicheroptimierten Tabellen  
  
-   Anweisungen zur Ausführung von [Leistungsmessungen anhand der exemplarischen Arbeitsauslastung](#PerformanceMeasurementsusingtheDemoWorkload), einschließlich Anweisungen zur Installation und Ausführung von OSTRESS (einem Tool zum Steuern der Arbeitsauslastung) sowie zur Ausführung der exemplarischen Arbeitsauslastung selbst  
  
-   [Arbeitsspeicher- und Datenträgernutzung im Beispiel](#MemoryandDiskSpaceUtilizationintheSample)  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]  
  
-   Für Leistungstests benötigen Sie einen Server, dessen Kapazität ungefähr der eines Servers in Ihrer Produktionsumgebung entspricht. Für dieses spezielle Beispiel sollten SQL Server mindestens 16 GB Arbeitsspeicher zur Verfügung stehen. Allgemeine Richtlinien zur Hardware für In-Memory-OLTP finden Sie in folgendem Blogbeitrag: [Aspekte zur Hardware für In-Memory-OLTP in SQL Server 2014](blog-hardware-in-memory-oltp.md).

##  <a name="installing-the-in-memory-oltp-sample-based-on-adventureworks"></a><a name="InstallingtheIn-MemoryOLTPsamplebasedonAdventureWorks"></a> Installieren des auf AdventureWorks basierenden InMemory OLTP-Beispiels  
 Führen Sie die folgenden Schritte aus, um das Beispiel zu installieren:  
  
1.  Laden Sie „AdventureWorks2016CTP3.bak“ und „SQLServer2016CTP3Samples.zip“ von [https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks](https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks) in einen lokalen Ordner herunter, z.B. C:\temp.  
  
2.  Stellen Sie die Datenbanksicherung mithilfe von [!INCLUDE[tsql](../../includes/tsql-md.md)] oder [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]wieder her:  
  
    1.  Geben Sie den Zielordner und Dateinamen für die Datendatei an, z. B.  
  
         „h:\DATA\AdventureWorks2016CTP3_Data.mdf“  
  
    2.  Geben Sie den Zielordner und Dateinamen für die Protokolldatei an, z. B.  
  
         „i:\DATA\AdventureWorks2016CTP3_log.ldf“  
  
        1.  Um optimale Leistung zu gewährleisten, sollte die Protokolldatei auf einem anderen Laufwerk als die Datendatei gespeichert werden, idealerweise auf einem Laufwerk mit niedriger Latenz wie einem SSD- oder PCIe-Speichermedium.  
  
     T-SQL-Beispielskript:  
  
    ```sql
    RESTORE DATABASE [AdventureWorks2016CTP3]   
      FROM DISK = N'C:\temp\AdventureWorks2016CTP3.bak'   
        WITH FILE = 1,    
      MOVE N'AdventureWorks2016_Data' TO N'h:\DATA\AdventureWorks2016CTP3_Data.mdf',    
      MOVE N'AdventureWorks2016_Log' TO N'i:\DATA\AdventureWorks2016CTP3_log.ldf',  
      MOVE N'AdventureWorks2016CTP3_mod' TO N'h:\data\AdventureWorks2016CTP3_mod'  
     GO  
    ```  
  
3.  Entpacken Sie die Datei „SQLServer2016CTP3Samples.zip“ in einen lokalen Ordner, um die Beispielskripts und die Arbeitsauslastung anzuzeigen. Informationen zum Ausführen der Arbeitsauslastung finden Sie in der Datei „In-Memory OLTP\readme.txt“.  
  
##  <a name="description-of-the-sample-tables-and-procedures"></a><a name="Descriptionofthesampletablesandprocedures"></a> Beschreibung der Beispieltabellen und -prozeduren  
 Im Beispiel werden neue Tabellen für Produkte und Verkaufsaufträge auf Grundlage vorhandener AdventureWorks-Tabellen erstellt. Das Schema der neuen Tabellen entspricht bis auf die nachfolgend beschriebenen Unterschiede dem der vorhandenen Tabellen.  
  
 Die neuen speicheroptimierten Tabellen haben das Suffix „_inmem“. Zusätzlich umfasst das Beispiel entsprechende Tabellen mit dem Suffix „_ondisk“. Mithilfe dieser Tabellen können 1:1-Vergleiche zwischen der Leistung speicheroptimierter und datenträgerbasierter Tabellen im System angestellt werden.  
  
 Die in der Arbeitsauslastung für Leistungsvergleiche verwendeten speicheroptimierten Tabellen sind vollständig dauerhaft und vollständig protokolliert, d. h., dass Leistungsvorteile nicht auf Kosten der Dauerhaftigkeit oder Zuverlässigkeit erzielt werden.  
  
 Die Zielarbeitsauslastung in diesem Beispiel dient der Abwicklung von Verkaufsaufträgen. Dabei fließen auch Produkt- und Rabattinformationen aus den Tabellen SalesOrderHeader, SalesOrderDetail, Product, SpecialOffer und SpecialOfferProduct ein.  
  
 Mithilfe der beiden neuen gespeicherten Prozeduren Sales.usp_InsertSalesOrder_inmem und Sales.usp_UpdateSalesOrderShipInfo_inmem werden Verkaufsaufträge eingefügt bzw. Versandinformationen zu bestimmten Verkaufsaufträgen aktualisiert.  
  
 Das neue Schema "Demo" enthält Hilfstabellen und gespeicherte Prozeduren zum Ausführen einer exemplarischen Arbeitsauslastung.  
  
 Durch das In-Memory OLTP-Beispiel werden AdventureWorks im Einzelnen die folgenden Objekte hinzugefügt:  
  
### <a name="tables-added-by-the-sample"></a>Tabellen, die durch das Beispiel hinzugefügt werden  
  
#### <a name="the-new-tables"></a>Neue Tabellen  
 Sales.SalesOrderHeader_inmem  
  
-   Kopfzeileninformationen zu Verkaufsaufträgen. Diese Tabelle enthält eine Zeile für jeden Verkaufsauftrag.  
  
 Sales.SalesOrderDetail_inmem  
  
-   Detailinformationen zu Verkaufsaufträgen. Diese Tabelle enthält eine Zeile für jeden Einzelposten eines Verkaufsauftrags.  
  
 Sales.SpecialOffer_inmem  
  
-   Informationen zu Sonderangeboten, einschließlich des prozentualen Rabatts, der den einzelnen Sonderangeboten zugeordnet ist.  
  
 Sales.SpecialOfferProduct_inmem  
  
-   Verweistabelle zwischen Sonderangeboten und Produkten. Jedes Sonderangebot kann für 0 (null) oder mehrere Produkte gelten, und jedes Produkt kann in 0 oder mehreren Sonderangeboten vertreten sein.  
  
 Production.Product_inmem  
  
-   Informationen zu Produkten, einschließlich der Listenpreise.  
  
 Demo.DemoSalesOrderDetailSeed  
  
-   Wird in der exemplarischen Arbeitsauslastung zum Erstellen von Beispielverkaufsaufträgen verwendet.  
  
 Datenträgerbasierte Tabellenvarianten:  
  
-   Sales.SalesOrderHeader_ondisk  
  
-   Sales.SalesOrderDetail_ondisk  
  
-   Sales.SpecialOffer_ondisk  
  
-   Sales.SpecialOfferProduct_ondisk  
  
-   Production.Product_ondisk  
  
#### <a name="differences-between-original-disk-based-and-the-and-new-memory-optimized-tables"></a>Unterschiede zwischen ursprünglichen datenträgerbasierten und neuen speicheroptimierten Tabellen  
 Meistens verwenden die neuen, in diesem Beispiel eingeführten Tabellen bis auf wenige Ausnahmen dieselben Spalten und Datentypen wie die ursprünglichen Tabellen. Die Unterschiede und der Grund für die Abweichung sind im Folgenden aufgeführt.  
  
 Sales.SalesOrderHeader_inmem  
  
-   Da *Standardeinschränkungen* bei speicheroptimierten Tabellen unterstützt werden, wurden die meisten Standardeinschränkungen unverändert migriert. Die ursprüngliche Tabelle Sales.SalesOrderHeader enthält jedoch zwei Standardeinschränkungen, durch die für die Spalten OrderDate und ModifiedDate das aktuelle Datum abgerufen wird. In einer Arbeitsauslastung für die Auftragsverarbeitung mit hohem Durchsatz und einem hohen Maß an Parallelität können globale Ressourcen zu Konflikten führen. Die Systemzeit ist beispielsweise eine solche globale Ressource und kann bei einer In-Memory OLTP-Arbeitsauslastung, durch die Verkaufsaufträge eingefügt werden, erfahrungsgemäß einen Engpass verursachen. Dies gilt insbesondere, wenn die Systemzeit für mehrere Spalten sowohl in der Auftragskopfzeile als auch in den Auftragsdetails abgerufen werden muss. In diesem Beispiel wird das Problem umgangen, indem die Systemzeit für jeden eingefügten Verkaufsauftrag nur einmal abgerufen und dieser Wert in der gespeicherten Prozedur Sales.usp_InsertSalesOrder_inmem für die datetime-Spalten in SalesOrderHeader_inmem und SalesOrderDetail_inmem verwendet wird.  
  
-   *Alias-UDTs*: In der ursprünglichen Tabelle werden die beiden Alias-UDTs (User-defined Data Types, benutzerdefinierte Datentypen) „dbo.OrderNumber“ und „dbo.AccountNumber“ für die Spalten „PurchaseOrderNumber“ bzw. „AccountNumber“ verwendet. [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] unterstützt keine Alias-UDTs für speicheroptimierte Tabellen, daher verwenden die neuen Tabellen die Systemdatentypen nvarchar(25) bzw. nvarchar(15).  
  
-   *Spalten mit NULL-Werten in Indexschlüsseln* : In der ursprünglichen Tabelle sind für die Spalte „SalesPersonID“ NULL-Werte zulässig, während die Spalte in den neuen Tabellen keine NULL-Werte zulässt und über eine Standardeinschränkung mit dem Wert (-1) verfügt. Dieser Umstand ist darin begründet, dass Indizes für speicheroptimierte Tabellen im Indexschlüssel keine Spalten aufweisen dürfen, die Nullwerte zulassen. Daher dient -1 in diesem Fall als Ersatz für NULL.  
  
-   *Berechnete Spalten* : Auf die berechneten Spalten SalesOrderNumber und TotalDue wurde verzichtet, da berechnete Spalten in speicheroptimierten Tabellen von [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] nicht unterstützt werden. In der neuen Sicht Sales.vSalesOrderHeader_extended_inmem sind die Spalten SalesOrderNumber und TotalDue enthalten. Falls diese Spalten benötigt werden, können Sie diese Sicht verwenden.  

    - **Anwendungsbereich:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.  
Ab [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 werden in speicheroptimierten Tabellen und Indizes berechnete Spalten unterstützt.

  
-   *Fremdschlüsseleinschränkungen* werden für speicheroptimierte Tabellen in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]nur dann unterstützt, wenn die referenzierten Tabellen ebenfalls speicheroptimiert sind. Fremdschlüssel mit Verweis auf Tabellen, die ebenfalls zu speicheroptimierten Tabellen migriert werden, werden in den migrierten Tabellen beibehalten, während andere Fremdschlüssel ausgelassen werden.  SalesOrderHeader_inmem stellt in der exemplarischen Arbeitsauslastung eine aktive Tabelle dar. Darüber hinaus verursachen FOREIGN KEY-Einschränkungen für DML-Vorgänge zusätzlichen Verarbeitungsaufwand, da alle anderen Tabellen, auf die in diesen Einschränkungen verwiesen wird, durchsucht werden müssen. Daher wird davon ausgegangen, dass die App referenzielle Integrität für die Tabelle „Sales.SalesOrderHeader_inmem“ gewährleistet, und eingefügte Zeilen werden nicht auf referenzielle Integrität überprüft.  
  
-   *Rowguid* : Die rowguid-Spalte wird nicht verwendet. Im Gegensatz zu uniqueidentifier wird die ROWGUIDCOL-Option für speicheroptimierte Tabellen in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]nicht unterstützt. Spalten dieses Typs werden normalerweise für Mergereplikationen oder für Tabellen mit FILESTREAM-Spalten verwendet, die in diesem Beispiel beide nicht zum Einsatz kommen.  
  
 Sales.SalesOrderDetail  
  
-   *Standardeinschränkungen*: Ähnlich wie SalesOrderHeader wird die Standardeinschränkung, die das Systemdatum bzw. die Systemzeit erfordert, nicht migriert. Stattdessen wird das aktuelle Systemdatum bzw. die aktuelle Systemzeit von der gespeicherten Prozedur, die Verkaufsaufträge einfügt, beim ersten Einfügevorgang hinzugefügt.  
  
-   *Berechnete Spalten*: Die berechnete Spalte „LineTotal“ wurde nicht migriert, weil berechnete Spalten bei speicheroptimierten Tabellen in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]nicht unterstützt werden. Um auf diese Spalte zuzugreifen, verwenden Sie die Sicht „Sales.vSalesOrderDetail_extended_inmem“.  
  
-   *Rowguid* : Die rowguid-Spalte wird nicht verwendet. Ausführliche Informationen finden Sie in der Beschreibung zur Tabelle SalesOrderHeader.  
  
 Production.Product  
  
-   *Alias-UDTs*: Die ursprüngliche Tabelle verwendet den benutzerdefinierten Datentyp „dbo.Flag“, der dem Systemdatentyp „bit“ entspricht. Die migrierte Tabelle verwendet stattdessen den Datentyp bit.  
  
-   *Rowguid* : Die rowguid-Spalte wird nicht verwendet. Ausführliche Informationen finden Sie in der Beschreibung zur Tabelle SalesOrderHeader.  
  
 Sales.SpecialOffer  
  
-   *Rowguid* : Die rowguid-Spalte wird nicht verwendet. Ausführliche Informationen finden Sie in der Beschreibung zur Tabelle SalesOrderHeader.  
  
 Sales.SpecialOfferProduct  
  
-   *Rowguid* : Die rowguid-Spalte wird nicht verwendet. Ausführliche Informationen finden Sie in der Beschreibung zur Tabelle SalesOrderHeader.  
  
#### <a name="considerations-for-indexes-on-memory-optimized-tables"></a>Überlegungen zu Indizes bei speicheroptimierten Tabellen  
 Der Basisindex für speicheroptimierte Tabellen ist der NONCLUSTERED-Index, der Punktsuchen (Indexsuche in Gleichheitsprädikaten), Bereichsscans (Indexsuche in Ungleichheitsprädikaten), vollständige Indexscans und sortierte Scans unterstützt. Zusätzlich unterstützen NONCLUSTERED-Indizes Suchen in führenden Indexschlüsselspalten. Bis auf Rückwärtsscans unterstützen speicheroptimierte NONCLUSTERED-Indizes alle Vorgänge, die auch von datenträgerbasierten NONCLUSTERED-Indizes unterstützt werden. Daher sind NONCLUSTERED-Indizes eine sichere Wahl für Ihre Indizes.  
  
 Mit HASH-Indizes kann die Arbeitsauslastung weiter optimiert werden, da sie speziell für Punktsuchen und Zeileneinfügungen optimiert sind. Allerdings unterstützen sie keine Bereichsscans, sortierte Scans oder Suchen in führenden Indexschlüsselspalten. Daher sollten diese Indizes mit Vorsicht verwendet werden. Außerdem muss während der Erstellung der bucket_count-Wert angegeben werden. Die Bucketanzahl sollte normalerweise auf einen Wert zwischen der einfachen und doppelten Anzahl von Indexschlüsselwerten festgelegt werden. Ein zu hoher Wert stellt in der Regel aber auch kein Problem dar.  
  
Weitere Informationen zu [Indexrichtlinien](../indexes/guidelines-for-online-index-operations.md) und Richtlinien zum [Auswählen des geeigneten bucket_count-Werts](https://sqlserver-help.com/tag/bucket_count/) finden Sie in der Onlinedokumentation.  

Die Onlinedokumentation bietet weitere Informationen zu folgenden Themen:
- [Indexrichtlinien](./indexes-for-memory-optimized-tables.md) <!-- On MSDN-TechNet was version sql.120 (2014), library/dn133166 -->

 Die Indizes der migrierten Tabellen wurden für die exemplarische Arbeitsauslastung, die zur Abwicklung von Verkaufsaufträgen eingesetzt wird, optimiert. Die Arbeitsauslastung basiert auf Einfügungen und Punktsuchen in den Tabellen Sales.SalesOrderHeader_inmem und Sales.SalesOrderDetail_inmem sowie auf Punktsuchen in den Primärschlüsselspalten der Tabellen Production.Product_inmem und Sales.SpecialOffer_inmem.  
  
 Sales.SalesOrderHeader_inmem verfügt über drei Indizes, die aus Leistungsgründen und weil für die Arbeitsauslastung keine sortierten oder Bereichsscans erforderlich sind, alle HASH-Indizes sind.  
  
-   HASH-Index für (SalesOrderID): bucket_count hat einen Wert von 10 Millionen (und wird auf 16 Millionen aufgerundet), da die erwartete Anzahl von Verkaufsaufträgen 10 Millionen beträgt.  
  
-   HASH-Index für (SalesPersonID): bucket_count beträgt 1 Million. Das bereitgestellte Dataset enthält nicht viele Vertriebsmitarbeiter. Dieser große bucket_count-Wert bietet jedoch genügend Platz für zukünftiges Wachstum. Und Sie müssen keine Leistungseinbußen für Punktsuchen in Kauf nehmen, wenn bucket_count überdimensioniert ist.  
  
-   HASH-Index für (CustomerID): bucket_count beträgt 1 Million. Dem bereitgestellten Dataset ist nur eine geringe Anzahl von Kunden zugeordnet, sodass es zukünftig anwachsen kann.  
  
 Sales.SalesOrderDetail_inmem verfügt über drei Indizes, die aus Leistungsgründen und weil für die Arbeitsauslastung keine sortierten oder Bereichsscans erforderlich sind, alle HASH-Indizes sind.  
  
-   HASH-Index für (SalesOrderID, SalesOrderDetailID): Dies ist der Primärschlüsselindex. Obwohl für (SalesOrderID, SalesOrderDetailID) nur selten Suchen ausgeführt werden, lassen sich Zeileneinfügungen beschleunigen, indem ein HASH-Index für den Schlüssel verwendet wird. Der bucket_count-Wert ist auf 50 Millionen festgelegt (und wird auf 67 Millionen aufgerundet): Die erwartete Anzahl von Verkaufsaufträgen beträgt 10 Millionen mit durchschnittlich fünf Einzelposten pro Auftrag.  
  
-   HASH-Index für (SalesOrderID): Es werden häufig Suchen nach Verkaufsaufträgen ausgeführt, und Sie möchten alle Einzelposten ermitteln, die sich auf einen einzelnen Auftrag beziehen.  bucket_count hat einen Wert von 10 Millionen (und wird auf 16 Millionen aufgerundet), da die erwartete Anzahl von Verkaufsaufträgen 10 Millionen beträgt.  
  
-   HASH-Index für (ProductID): bucket_count beträgt 1 Million. Dem bereitgestellten Dataset ist nur eine geringe Anzahl von Produkten zugeordnet, sodass es zukünftig anwachsen kann.  
  
 Production.Product_inmem verfügt über drei Indizes.  
  
-   HASH-Index für (ProductID): Da Suchen nach ProductID ein wesentlicher Bestandteil der exemplarischen Arbeitsauslastung sind, wird hier ein HASH-Index verwendet.  
  
-   NONCLUSTERED-Index für (Name): ermöglicht sortierte Scans für Produktnamen.  
  
-   NONCLUSTERED-Index für (ProductNumber): ermöglicht sortierte Scans für Produktnummern.  
  
 Sales.SpecialOffer_inmem verfügt über einen HASH-Index für (SpecialOfferID): Punktsuchen nach Sonderangeboten sind ein wesentlicher Bestandteil der exemplarischen Arbeitsauslastung. bucket_count beträgt 1 Million und ist auf zukünftiges Wachstum ausgelegt.  
  
 Da in der exemplarischen Arbeitsauslastung nicht auf Sales.SpecialOfferProduct_inmem verwiesen wird, ist es nicht erforderlich, HASH-Indizes für die Tabelle zu verwenden, um die Arbeitsauslastung zu optimieren. Für (SpecialOfferID, ProductID) und (ProductID) werden NONCLUSTERED-Indizes verwendet.  
  
 Beachten Sie, dass einige der oben genannten Bucketanzahlen zu hoch angesetzt sind. Auf die Bucketanzahlen für die Indizes von SalesOrderHeader_inmem und SalesOrderDetail_inmem trifft dies jedoch nicht zu, da sie auf eine Anzahl von 10 Millionen Verkaufsaufträgen beschränkt sind. Auf diese Weise kann das Beispiel auch auf Systemen mit geringerer Arbeitsspeicherkapazität installiert werden. In diesen Fällen verursacht die exemplarische Arbeitsauslastung jedoch einen Fehler vom Typ "Nicht genügend Arbeitsspeicher". Wenn Sie einen Wert festlegen möchten, der 10 Millionen Verkaufsaufträge erheblich überschreitet, können Sie die Bucketanzahlen einfach entsprechend erhöhen.  
  
#### <a name="considerations-for-memory-utilization"></a>Überlegungen zur Arbeitsspeichernutzung  
 Die Arbeitsspeichernutzung der Beispieldatenbank vor und nach der Ausführung der exemplarischen Arbeitsauslastung wird im Abschnitt [Arbeitsspeichernutzung für speicheroptimierte Tabellen](#Memoryutilizationforthememory-optimizedtables)erörtert.  
  
### <a name="stored-procedures-added-by-the-sample"></a>Gespeicherte Prozeduren, die durch das Beispiel hinzugefügt wurden  
 Die beiden wichtigsten gespeicherten Prozeduren zum Einfügen von Verkaufsaufträgen und Aktualisieren von Versanddetails lauten:  
  
-   Sales.usp_InsertSalesOrder_inmem  
  
    -   Fügt einen neuen Verkaufsauftrag in die Datenbank ein und gibt SalesOrderID für den Verkaufsauftrag aus. Als Eingabeparameter werden Details zur Auftragskopfzeile sowie die Einzelposten im Auftrag akzeptiert.  
  
    -   Ausgabeparameter:  
  
        -   @SalesOrderID int: SalesOrderID für den gerade eingefügten Verkaufsauftrag  
  
    -   Eingabeparameter (erforderlich):  
  
        -   @DueDate datetime2  
  
        -   @CustomerID int  
  
        -   @BillToAddressID [int]  
  
        -   @ShipToAddressID [int]  
  
        -   @ShipMethodID [int]  
  
        -   @SalesOrderDetails Sales.SalesOrderDetailType_inmem: Tabellenwertparameter, der die Einzelposten des Auftrags enthält  
  
    -   Eingabeparameter (optional):  
  
        -   @Status [tinyint]  
  
        -   @OnlineOrderFlag [bit]  
  
        -   @PurchaseOrderNumber [nvarchar](25\)  
  
        -   @AccountNumber [nvarchar](15\)  
  
        -   @SalesPersonID [int]  
  
        -   @TerritoryID [int]  
  
        -   @CreditCardID [int]  
  
        -   @CreditCardApprovalCode [varchar](15\)  
  
        -   @CurrencyRateID [int]  
  
        -   @Comment nvarchar(128)  
  
-   Sales.usp_UpdateSalesOrderShipInfo_inmem  
  
    -   Aktualisiert die Versandinformationen für einen bestimmten Verkaufsauftrag. Gleichzeitig werden auch die Versandinformationen für alle Einzelposten des Verkaufsauftrags aktualisiert.  
  
    -   Dies ist eine Wrapperprozedur für die systemintern kompilierte gespeicherte Prozedur Sales.usp_UpdateSalesOrderShipInfo_native. Sie verfügt über eine Wiederholungslogik zur Behandlung (unerwarteter) potenzieller Konflikte mit Transaktionen, die gleichzeitig ausgeführt werden und denselben Auftrag aktualisieren. Weitere Informationen zur Wiederholungslogik finden Sie in [diesem Thema](./transactions-with-memory-optimized-tables.md)in der Onlinedokumentation.  
  
-   Sales.usp_UpdateSalesOrderShipInfo_native  
  
    -   Dies ist die systemintern kompilierte gespeicherte Prozedur, durch die das Update der Versandinformationen tatsächlich verarbeitet wird. Sie sollte normalerweise von der gespeicherten Wrapperprozedur Sales.usp_UpdateSalesOrderShipInfo_inmem aufgerufen werden. Wenn der Client Fehler behandeln kann und eine Wiederholungslogik implementiert wurde, können Sie diese Prozedur direkt aufrufen, anstatt die gespeicherte Wrapperprozedur zu verwenden.  
  
 Die folgende gespeicherte Prozedur wird für die exemplarische Arbeitsauslastung verwendet.  
  
-   Demo.usp_DemoReset  
  
    -   Setzt die exemplarische Arbeitsauslastung zurück, indem für die Tabellen SalesOrderHeader und SalesOrderDetail ein erneutes Seeding ausgeführt wird, nachdem sie geleert wurden.  
  
 Mit den folgenden gespeicherten Prozeduren werden Daten in speicheroptimierten Tabellen eingefügt und daraus gelöscht, ohne die Domänenintegrität und referenzielle Integrität zu gefährden.  
  
-   Production.usp_InsertProduct_inmem  
  
-   Production.usp_DeleteProduct_inmem  
  
-   Sales.usp_InsertSpecialOffer_inmem  
  
-   Sales.usp_DeleteSpecialOffer_inmem  
  
-   Sales.usp_InsertSpecialOfferProduct_inmem  
  
 Zum Schluss werden Domänenintegrität und referenzielle Integrität mit der folgenden gespeicherten Prozedur überprüft.  
  
1.  dbo.usp_ValidateIntegrity  
  
    -   Optionaler Parameter: @object_id: ID des Objekts, dessen Integrität überprüft werden soll  
  
    -   Diese Prozedur ermittelt anhand der Tabellen dbo.DomainIntegrity, dbo.ReferentialIntegrity und dbo.UniqueIntegrity, welche Integritätsregeln überprüft werden müssen. Im Beispiel werden diese Tabellen auf der Grundlage der CHECK-, FOREIGN KEY- und UNIQUE-Einschränkungen der ursprünglichen Tabellen in der AdventureWorks-Datenbank aufgefüllt.  
  
    -   Die zum Ausführen der Integritätsprüfungen erforderliche T-SQL-Anweisung wird mithilfe der Hilfsprozeduren dbo.usp_GenerateCKCheck, dbo.usp_GenerateFKCheck und dbo.GenerateUQCheck generiert.  
  
##  <a name="performance-measurements-using-the-demo-workload"></a><a name="PerformanceMeasurementsusingtheDemoWorkload"></a> Leistungsmessungen anhand der exemplarischen Arbeitsauslastung  
 OSTRESS ist ein Befehlszeilentool, das vom Microsoft CSS SQL Server-Supportteam entwickelt wurde. Mit diesem Tool können Abfragen ausgeführt oder gespeicherte Prozeduren parallel aufgerufen werden. Sie können die Anzahl der Threads zur parallelen Ausführung einer bestimmten T-SQL-Anweisung konfigurieren und angeben, wie oft die Anweisung in diesem Thread ausgeführt werden soll. OSTRESS bündelt die Threads und führt die Anweisung in allen Threads gleichzeitig aus. Nachdem die Ausführung aller Threads beendet wurde, meldet OSTRESS die zur Beendigung sämtlicher Threads benötigte Dauer.  
  
### <a name="installing-ostress"></a>Installieren von OSTRESS  
 OSTRESS wird nicht eigenständig, sondern als Teil der RML-Hilfsprogramme (Remote Markup Language) installiert.  
  
 Installationsschritte:  
  
1.  Laden Sie die x64-Version des Installationspakets für die RML-Hilfsprogramme von folgender Seite herunter, und führen Sie das Paket aus: [Herunterladen von RML für SQL Server](https://www.microsoft.com/download/details.aspx?id=4511)

2.  Falls Sie in einem Dialogfeld darauf hingewiesen werden, dass bestimmte Dateien gerade verwendet werden, klicken Sie auf „Weiter“.  
  
### <a name="running-ostress"></a>Ausführen von OSTRESS  
 OSTRESS wird an der Eingabeaufforderung ausgeführt. Am einfachsten lässt sich das Tool über die RML-Eingabeaufforderung ausführen, die mit den RML-Hilfsprogrammen installiert wird.  
  
 Führen Sie die folgenden Schritte aus, um die RML-Eingabeaufforderung zu öffnen:  
  
 Öffnen Sie in Windows Server 2012 [R2] sowie in Windows 8 und 8.1 das Startmenü, indem Sie die Windows-Taste drücken, und geben Sie „rml“ ein. Klicken Sie auf die in der Liste der Suchergebnisse angezeigte RML-Eingabeaufforderung (RML Cmd Prompt).  
  
 Vergewissern Sie sich, dass sich die Eingabeaufforderung im Installationsordner für die RML-Hilfsprogramme befindet.  
  
 Um die Befehlszeilenoptionen für OSTRESS anzuzeigen, führen Sie ostress.exe einfach ohne Angabe von Befehlszeilenoptionen aus. Im Folgenden die wichtigsten Optionen, die beim Ausführen von OSTRESS für dieses Beispiel angegeben werden:  
  
-   -S: Der Name der Microsoft SQL Server-Instanz, mit der eine Verbindung hergestellt werden soll.  
  
-   -E: Verwendet die Windows-Authentifizierung für Verbindungen (Standard). Bei Verwendung der SQL Server-Authentifizierung können Sie mit den Optionen -U und -P den Benutzernamen bzw. das Kennwort angeben.  
  
-   -d: Der Name der Datenbank, in diesem Beispiel "AdventureWorks2014".  
  
-   -Q: Die auszuführende T-SQL-Anweisung.  
  
-   -n: Die Anzahl der Verbindungen, über die die einzelnen Eingabedateien/Abfragen verarbeitet werden.  
  
-   -r: Die Anzahl der Iterationen für jede Verbindung, über die die einzelnen Eingabedateien/Abfragen verarbeitet werden.  
  
### <a name="demo-workload"></a>Exemplarische Arbeitsauslastung  
 Die wichtigste in der exemplarischen Arbeitsauslastung verwendete gespeicherte Prozedur ist Sales.usp_InsertSalesOrder_inmem/ondisk. Das folgende Skript erstellt einen Tabellenwertparameter (Table-valued Parameter, TVP) mit Beispieldaten und ruft anschließend die Prozedur auf, um einen Verkaufsauftrag mit fünf Einzelpositionen einzufügen.  
  
 Das OSTRESS-Tool wird verwendet, um die Aufrufe der gespeicherten Prozedur parallel auszuführen und Clients zu simulieren, die zeitgleich Verkaufsaufträge einfügen.  
  
 Setzen Sie die exemplarische Arbeitsauslastung nach jedem Belastungstest zurück, indem Sie Demo.usp_DemoReset ausführen. Durch diese Prozedur werden die Zeilen in den speicheroptimierten Tabellen gelöscht, die datenträgerbasierten Tabellen abgeschnitten und ein Datenbankprüfpunkt ausgeführt.  
  
 Das folgende Skript wird gleichzeitig ausgeführt, um eine Arbeitsauslastung zur Auftragsabwicklung zu simulieren:  
  
```sql
DECLARE   
      @i int = 0,   
      @od Sales.SalesOrderDetailType_inmem,   
      @SalesOrderID int,   
      @DueDate datetime2 = sysdatetime(),   
      @CustomerID int = rand() * 8000,   
      @BillToAddressID int = rand() * 10000,   
      @ShipToAddressID int = rand() * 10000,   
      @ShipMethodID int = (rand() * 5) + 1;   
  
INSERT INTO @od   
SELECT OrderQty, ProductID, SpecialOfferID   
FROM Demo.DemoSalesOrderDetailSeed   
WHERE OrderID= cast((rand()*106) + 1 as int);   
  
WHILE (@i < 20)   
BEGIN;   
      EXEC Sales.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @ShipMethodID, @od;   
      SET @i += 1   
END
```  
  
 Mit diesem Skript wird jeder erstellte Beispielauftrag durch 20 in einer WHILE-Schleife ausgeführte gespeicherte Prozeduren 20 Mal eingefügt. Die Schleife wird verwendet, weil die Datenbank zum Erstellen des Beispielauftrags verwendet wird. In einer typischen Produktionsumgebung wird der einzufügende Verkaufsauftrag durch die Mid-Tier-Anwendung erstellt.  
  
 Durch das oben angegebene Skript werden Verkaufsaufträge in speicheroptimierte Tabellen eingefügt. Sie erhalten das Skript zum Einfügen von Verkaufsaufträgen in datenträgerbasierte Tabellen, indem Sie die beiden Suffixe „_inmem in _ondisk“ ändern.  
  
 Wir verwenden das OSTRESS-Tool, um die Skripts unter Verwendung mehrerer gleichzeitiger Verbindungen auszuführen. Dabei wird mit dem Parameter „-n“ gesteuert, wie viele Verbindungen verwendet werden, und mit dem Parameter „-r“, wie oft das Skript für jede Verbindung ausgeführt wird.  
  
#### <a name="running-the-workload"></a>Ausführen der Arbeitsauslastung  
 Um das Verhalten in einem größeren Szenario zu testen, fügen wir unter Verwendung von 100 Verbindungen 10 Millionen Verkaufsaufträge ein. Bei einem einfach ausgestatteten Server (z. B. mit 8 physischen und 16 logischen Kernen) und SSD-Basisspeicher für das Protokoll liefert der Test zufriedenstellende Ergebnisse. Falls der Test mit Ihrer Hardware nicht gut abschneidet, sollten Sie sich im Abschnitt [Problembehandlung bei langsamer Testausführung](#Troubleshootingslow-runningtests) informieren. Wenn Sie das Belastungsniveau für diesen Test verringern möchten, reduzieren Sie die Anzahl der Verbindungen, indem Sie den Parameter „-n“ ändern. Um die Anzahl der Verbindungen z. B. auf 40 zu verringern, ändern Sie den Parameter „-n100“ in „-n40“.  
  
 Als Leistungskennzahl für die Arbeitsauslastung wird die Zeitspanne verwendet, die von ostress.exe nach Ausführung der Arbeitsauslastung gemeldet wird.  
  
 Den unten angegebenen Anweisungen und Messungen liegt eine Arbeitsauslastung zugrunde, bei der 10 Millionen Verkaufsaufträge eingefügt werden. Eine Anleitung zum Ausführen einer herunterskalierten Arbeitsauslastung zum Einfügen von 1 Million Verkaufsaufträgen finden in der Datei „In-Memory OLTP\readme.txt“, die im Archiv „SQLServer2016CTP3Samples.zip“ enthalten ist.  
  
##### <a name="memory-optimized-tables"></a>Speicheroptimierte Tabellen  
 Zuerst führen wir die Arbeitsauslastung für speicheroptimierte Tabellen aus. Mit dem folgenden Befehl werden 100 Threads geöffnet, die jeweils für 5.000 Iterationen ausgeführt werden.  Pro Iteration werden 20 Verkaufsaufträge in getrennten Transaktionen eingefügt. Die 20 Einfügungen pro Iteration sind darauf zurückzuführen, dass die einzufügenden Daten unter Verwendung der Datenbank generiert werden. Daraus ergeben sich insgesamt 20 \* 5.000 \* 100 = 10.000.000 eingefügte Verkaufsaufträge.  
  
 Öffnen Sie die RML-Eingabeaufforderung, und führen Sie folgenden Befehl aus:  
  
 Klicken Sie auf die Schaltfläche zum Kopieren, um den Befehl zu kopieren, und fügen Sie ihn in die Eingabeaufforderung der RML-Hilfsprogramme ein.  
  
```console
ostress.exe -n100 -r5000 -S. -E -dAdventureWorks2016CTP3 -q -Q"DECLARE @i int = 0, @od Sales.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand() * 10000, @ShipMethodID int = (rand() * 5) + 1; INSERT INTO @od SELECT OrderQty, ProductID, SpecialOfferID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*106) + 1 as int); while (@i < 20) begin; EXEC Sales.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @ShipMethodID, @od; set @i += 1 end"  
```  
  
 Auf einem Testserver mit insgesamt 8 physischen (16 logischen) Kernen betrug die Dauer zwei Minuten und fünf Sekunden. Auf einem zweiten Testserver mit 24 physischen (48 logischen) Kernen dauerte der Vorgang eine Minute und 0 (null) Sekunden.  
  
 Beobachten Sie bei der Ausführung der Arbeitsauslastung die CPU-Auslastung, beispielsweise mit dem Task-Manager. Sie werden feststellen, dass die CPU-Auslastung bei fast 100 % liegt. Andernfalls verursachen E/A-Protokollvorgänge einen Engpass. Weitere Informationen finden Sie auch unter [Problembehandlung bei langsamer Testausführung](#Troubleshootingslow-runningtests).  
  
##### <a name="disk-based-tables"></a>Datenträgerbasierte Tabellen  
 Mit dem folgenden Befehl wird die Arbeitsauslastung für datenträgerbasierte Tabellen ausgeführt. Die Ausführung dieser Arbeitsauslastung kann einige Zeit dauern, was hauptsächlich auf Latchkonflikte im System zurückzuführen ist. Dieses Problem tritt bei speicheroptimierten Tabellen nicht auf, da sie ohne Latches auskommen.  
  
 Öffnen Sie die RML-Eingabeaufforderung, und führen Sie folgenden Befehl aus:  
  
 Klicken Sie auf die Schaltfläche zum Kopieren, um den Befehl zu kopieren, und fügen Sie ihn in die Eingabeaufforderung der RML-Hilfsprogramme ein.  
  
```console
ostress.exe -n100 -r5000 -S. -E -dAdventureWorks2016CTP3 -q -Q"DECLARE @i int = 0, @od Sales.SalesOrderDetailType_ondisk, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand() * 10000, @ShipMethodID int = (rand() * 5) + 1; INSERT INTO @od SELECT OrderQty, ProductID, SpecialOfferID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*106) + 1 as int); while (@i < 20) begin; EXEC Sales.usp_InsertSalesOrder_ondisk @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @ShipMethodID, @od; set @i += 1 end"  
```  
  
 Auf einem Testserver mit insgesamt 8 physischen (16 logischen) Kernen betrugt die Dauer 41 Minuten und 25 Sekunden. Auf einem zweiten Testserver mit 24 physischen (48 logischen) Kernen dauerte der Vorgang 52 Minuten und 16 Sekunden.  
  
 Der Hauptunterschied zwischen der Leistung speicheroptimierter und datenträgerbasierter Tabellen in diesem Test besteht darin, dass die CPU bei Verwendung datenträgerbasierter Tabellen von SQL Server nicht voll ausgenutzt werden kann. Die Ursache sind Latchkonflikte: Wenn gleichzeitige Transaktionen versuchen, Daten in dieselbe Datenseite zu schreiben, wird mithilfe von Latches sichergestellt, dass jeweils nur eine Transaktion Schreibzugriff auf eine Seite hat. Die In-Memory-OLTP-Engine verwendet keine Latches, und Datenzeilen sind nicht seitenweise angeordnet. Da sich Einfügungen gleichzeitiger Transaktionen nicht gegenseitig blockieren, kann die CPU-Leistung von SQL Server voll ausgeschöpft werden.  
  
 Sie können die CPU-Auslastung bei der Ausführung der Arbeitsauslastung beispielsweise mit dem Task-Manager beobachten. Sie werden feststellen, dass die CPU-Auslastung bei Verwendung datenträgerbasierter Tabellen weit von 100 % entfernt ist. In einer Testkonfiguration mit 16 logischen Prozessoren würde sich die Auslastung um 24 % bewegen.  
  
 Optional können Sie den Leistungsindikator „\SQL Server:Latches\Latchwartevorgänge/Sekunde“ im Systemmonitor verwenden, um die Anzahl der Latchwartevorgänge pro Sekunde anzuzeigen.  
  
#### <a name="resetting-the-demo"></a>Zurücksetzen der exemplarischen Arbeitsauslastung  
 Um die exemplarische Arbeitsauslastung zurückzusetzen, öffnen Sie die RML-Eingabeaufforderung und führen folgenden Befehl aus:  
  
```console
ostress.exe -S. -E -dAdventureWorks2016CTP3 -Q"EXEC Demo.usp_DemoReset"  
```  
  
 Je nach Hardware kann die Ausführung einige Minuten dauern.  
  
 Es wird empfohlen, die Arbeitsauslastung nach jedem Durchgang zurückzusetzen. Da bei dieser Arbeitsauslastung nur Einfügungen stattfinden, wird bei jedem Durchgang mehr Arbeitsspeicher belegt. Durch das Zurücksetzen wird verhindert, dass der Arbeitsspeicher knapp wird. Der Abschnitt [Arbeitsspeichernutzung nach dem Ausführen der Arbeitsauslastung](#Memoryutilizationafterrunningtheworkload)enthält Informationen darüber, wie viel Arbeitsspeicher nach einer Ausführung belegt ist.  
  
###  <a name="troubleshooting-slow-running-tests"></a><a name="Troubleshootingslow-runningtests"></a> Problembehandlung bei langsamer Testausführung  
 Die Testergebnisse variieren normalerweise je nach Hardware und dem im Testlauf verwendeten Parallelitätsgrad. Wenn die Ergebnisse nicht wie erwartet ausfallen, sollten Sie folgende Punkte überprüfen:  
  
-   Anzahl gleichzeitiger Transaktionen: Wenn die Arbeitsauslastung in einem einzelnen Thread ausgeführt wird, liegt der Leistungsgewinn bei In-Memory OLTP wahrscheinlich unter dem zweifachen Wert. Latchkonflikte stellen nur bei einem hohen Parallelitätsgrad ein wirkliches Problem dar.  
  
-   SQL Server arbeitet mit einer geringen Anzahl von Kernen: Dies bedeutet, dass das System einen geringen Parallelitätsgrad aufweist, da nur so viele Transaktionen gleichzeitig ausgeführt werden können, wie Kerne für SQL verfügbar sind.  
  
    -   Symptom: Wenn die CPU-Auslastung beim Ausführen der Arbeitsauslastung für datenträgerbasierte Tabellen hoch ist, liegen normalerweise wenig Konflikte vor, was auf eine fehlende Parallelität hinweist.  
  
-   Geschwindigkeit des Protokolllaufwerks: Wenn das Protokolllaufwerk für den Transaktionsdurchsatz im System zu langsam ist, verursacht die Arbeitsauslastung bei E/A-Protokollvorgängen einen Engpass. Obwohl die Protokollierung mit In-Memory OLTP effizienter ist, wenn E/A-Protokollvorgänge einen Engpass verursachen, ist der potenzielle Leistungsgewinn begrenzt.  
  
    -   Symptom: Wenn die CPU-Auslastung beim Ausführen der Arbeitsauslastung für speicheroptimierte Tabellen nicht nahe 100 % liegt oder unregelmäßige Spitzen aufweist, kann ein Engpass bei E/A-Protokollvorgängen vorliegen. Sie können die Ursache im Ressourcenmonitor anhand der Warteschlangenlänge für das Protokolllaufwerk ermitteln.  
  
##  <a name="memory-and-disk-space-utilization-in-the-sample"></a><a name="MemoryandDiskSpaceUtilizationintheSample"></a> Arbeitsspeicher- und Datenträgernutzung im Beispiel  
 Im Folgenden wird beschrieben, wie viel Arbeitsspeicher und Datenträgerspeicher für die Beispieldatenbank benötigt wird. Außerdem sind die Ergebnisse aufgeführt, die auf einem Testserver mit 16 logischen Kernen ermittelt wurden.  
  
###  <a name="memory-utilization-for-the-memory-optimized-tables"></a><a name="Memoryutilizationforthememory-optimizedtables"></a> Arbeitsspeichernutzung für speicheroptimierte Tabellen  
  
#### <a name="overall-utilization-of-the-database"></a>Gesamtnutzung der Datenbank  
 Mithilfe der folgenden Abfrage kann die gesamte Arbeitsspeichernutzung für In-Memory OLTP im System ermittelt werden.  
  
```sql
SELECT type  
   , name  
, pages_kb/1024 AS pages_MB   
FROM sys.dm_os_memory_clerks WHERE type LIKE '%xtp%'  
```  
  
 Momentaufnahme direkt nach der Erstellung der Datenbank:  
  
| type | name | pages_MB |
| ---- | ---- | -------- |
|MEMORYCLERK_XTP|Standard|94|  
|MEMORYCLERK_XTP|DB_ID_5|877|  
|MEMORYCLERK_XTP|Standard|0|  
|MEMORYCLERK_XTP|Standard|0|  
  
 Die standardmäßigen Arbeitsspeicherclerks basieren auf systemweiten Strukturen und sind relativ klein. Der Arbeitsspeicherclerk für die Benutzerdatenbank, d. h. die Datenbank mit der ID 5, umfasst etwa 900 MB.  
  
#### <a name="memory-utilization-per-table"></a>Arbeitsspeichernutzung pro Tabelle  
 Mithilfe der folgenden Abfrage kann ein Drilldown ausgeführt werden, um die Arbeitsspeichernutzung der einzelnen Tabellen und ihrer Indizes zu ermitteln:  
  
```sql
SELECT object_name(t.object_id) AS [Table Name]  
     , memory_allocated_for_table_kb  
 , memory_allocated_for_indexes_kb  
FROM sys.dm_db_xtp_table_memory_stats dms JOIN sys.tables t   
ON dms.object_id=t.object_id  
WHERE t.type='U'  
```  
  
 Die folgende Tabelle zeigt die Ergebnisse, die diese Abfrage bei einer Neuinstallation des Beispiels zurückgibt:  
  
| Tabellenname | memory_allocated_for_table_kb | memory_allocated_for_indexes_kb |
| ---------- | ----------------------------- | ------------------------------- |
|SpecialOfferProduct_inmem|64|3840|  
|DemoSalesOrderHeaderSeed|1984|5504|  
|SalesOrderDetail_inmem|15316|663552|  
|DemoSalesOrderDetailSeed|64|10432|  
|SpecialOffer_inmem|3|8192|  
|SalesOrderHeader_inmem|7168|147456|  
|Product_inmem|124|12352|  

 Sie sehen, dass die Tabellen relativ klein sind: „SalesOrderHeader_inmem“ umfasst ca. 7 MB und „SalesOrderDetail_inmem“ ca. 15 MB.  
  
 Hier fällt auf, dass die den Indizes zugeordnete Arbeitsspeicherkapazität deutlich über der Kapazität der Tabellendaten liegt. Dies liegt daran, dass die Datengröße für die Hashindizes im Beispiel vorab auf einen höheren Wert festgelegt wurde. Da Hashindizes über eine feste Größe verfügen, wachsen sie nicht mit der Größe der Daten in der Tabelle mit.  
  
####  <a name="memory-utilization-after-running-the-workload"></a><a name="Memoryutilizationafterrunningtheworkload"></a> Arbeitsspeichernutzung nach dem Ausführen der Arbeitsauslastung  
 Nach 10 Millionen eingefügten Verkaufsaufträgen stellt sich die Arbeitsspeichernutzung insgesamt wie folgt dar:  
  
```sql
SELECT type  
, name  
, pages_kb/1024 AS pages_MB   
FROM sys.dm_os_memory_clerks WHERE type LIKE '%xtp%'  
```  
  
| type | name | pages_MB |
| ---- | ---- | -------- |
|MEMORYCLERK_XTP|Standard|146|  
|MEMORYCLERK_XTP|DB_ID_5|7374|  
|MEMORYCLERK_XTP|Standard|0|  
|MEMORYCLERK_XTP|Standard|0|  

 Wie Sie sehen, belegt SQL Server etwas weniger als 8 GB für die speicheroptimierten Tabellen und Indizes in der Beispieldatenbank.  
  
 Nach einem Testlauf ergibt sich die folgende Arbeitsspeichernutzung nach Tabellen:  
  
```sql
SELECT object_name(t.object_id) AS [Table Name]  
     , memory_allocated_for_table_kb  
 , memory_allocated_for_indexes_kb  
FROM sys.dm_db_xtp_table_memory_stats dms JOIN sys.tables t   
ON dms.object_id=t.object_id  
WHERE t.type='U'  
```  
  
| Tabellenname | memory_allocated_for_table_kb | memory_allocated_for_indexes_kb |
| ---------- | ----------------------------- | ------------------------------- |
|SalesOrderDetail_inmem|5113761|663552|  
|DemoSalesOrderDetailSeed|64|10368|  
|SpecialOffer_inmem|2|8192|  
|SalesOrderHeader_inmem|1575679|147456|  
|Product_inmem|111|12032|  
|SpecialOfferProduct_inmem|64|3712|  
|DemoSalesOrderHeaderSeed|1984|5504|  

 Die Gesamtmenge an Daten beträgt etwa 6,5 GB. Beachten Sie, dass die Größe der Indizes für die Tabellen SalesOrderHeader_inmem und SalesOrderDetail_inmem der Größe der Indizes vor dem Einfügen der Verkaufsaufträge entspricht. Die Indexgröße hat sich nicht geändert, weil beide Tabellen Hashindizes verwenden, die wiederum statisch sind.  
  
#### <a name="after-demo-reset"></a>Nach dem Zurücksetzen der exemplarischen Arbeitauslastung  
 Die gespeicherte Prozedur Demo.usp_DemoReset kann verwendet werden, um die exemplarische Arbeitsauslastung zurückzusetzen. Durch sie werden die Daten in den Tabellen SalesOrderHeader_inmem und SalesOrderDetail_inmem gelöscht und mit neuen Ausgangsdaten aus den urspünglichen Tabellen SalesOrderHeader und SalesOrderDetail aufgefüllt.  
  
 Obwohl die Zeilen in den Tabellen gelöscht wurden, bedeutet dies nicht, dass der Arbeitsspeicher sofort freigegeben wird. SQL Server gibt den Arbeitsspeicher, der von den aus speicheroptimierten Tabellen gelöschten Zeilen belegt wurde, nach Bedarf im Hintergrund frei. Wenn im System keine Transaktionen ausgeführt werden, werden Sie feststellen, dass der von den gelöschten Zeilen belegte Arbeitsspeicher unmittelbar nach dem Zurücksetzen der exemplarischen Arbeitsauslastung noch nicht freigegeben wurde:  
  
```sql
SELECT type  
, name  
, pages_kb/1024 AS pages_MB   
FROM sys.dm_os_memory_clerks WHERE type LIKE '%xtp%'  
```  
  
| type | name | pages_MB |
| ---- | ---- | -------- |
|MEMORYCLERK_XTP|Standard|2261|  
|MEMORYCLERK_XTP|DB_ID_5|7396|  
|MEMORYCLERK_XTP|Standard|0|  
|MEMORYCLERK_XTP|Standard|0|  

 Erwartetes Verhalten: Der Arbeitsspeicher wird beim Ausführen der Transaktionsarbeitsauslastung freigegeben.  
  
 Wenn Sie die exemplarische Arbeitsauslastung ein zweites Mal ausführen, werden Sie anfänglich einen Rückgang der Arbeitsspeichernutzung feststellen, da die zuvor gelöschten Zeilen bereinigt werden. Gleichzeitig nimmt die Arbeitsspeichergröße wieder zu, bis die Arbeitsauslastung abgeschlossen wurde. Nachdem die exemplarische Arbeitsauslastung zurückgesetzt und 10 Millionen Zeilen eingefügt wurden, entspricht die Arbeitsspeichernutzung weitestgehend der Nutzung nach der ersten Ausführung. Beispiel:  
  
```sql
SELECT type  
, name  
, pages_kb/1024 AS pages_MB   
FROM sys.dm_os_memory_clerks WHERE type LIKE '%xtp%'  
```  
  
| type | name | pages_MB |
| ---- | ---- | -------- |
|MEMORYCLERK_XTP|Standard|1863|  
|MEMORYCLERK_XTP|DB_ID_5|7390|  
|MEMORYCLERK_XTP|Standard|0|  
|MEMORYCLERK_XTP|Standard|0|  

### <a name="disk-utilization-for-memory-optimized-tables"></a>Datenträgernutzung für speicheroptimierte Tabellen  
 Mithilfe der folgenden Abfrage können Sie ermitteln, wie viel Gesamtspeicherplatz die Prüfpunktdateien einer Datenbank zu einem bestimmten Zeitpunkt auf dem Datenträger belegen:  
  
```sql
SELECT SUM(df.size) * 8 / 1024 AS [On-disk size in MB]  
FROM sys.filegroups f JOIN sys.database_files df   
   ON f.data_space_id=df.data_space_id  
WHERE f.type=N'FX'  
  
```  
  
#### <a name="initial-state"></a>Anfangszustand  
 Bei der Erstellung der Beispieldateigruppe und der speicheroptimierten Beispieltabellen werden eine Reihe von Prüfpunktdateien vorab erstellt, und das System beginnt, die Dateien mit Daten aufzufüllen. Wie viele Prüfpunktdateien vorab erstellt werden, hängt von der Anzahl der logischen Prozessoren im System ab. Da das Beispiel zunächst noch sehr klein ist, sind die vorab erstellten Dateien anfänglich überwiegend leer.  
  
 Der folgende Code zeigt die anfängliche Größe des Beispiels auf dem Datenträger eines Computers mit 16 logischen Prozessoren:  
  
```sql
SELECT SUM(df.size) * 8 / 1024 AS [On-disk size in MB]  
FROM sys.filegroups f JOIN sys.database_files df   
   ON f.data_space_id=df.data_space_id  
WHERE f.type=N'FX'  
```  
  
| Größe auf dem Datenträger (MB) |
| ------------------ |
|2312|  

 Wie Sie sehen, besteht eine große Diskrepanz zwischen der Größe der Prüfpunktdateien auf dem Datenträger, die 2,3 GB beträgt, und der tatsächlichen Datengröße, die eher bei 30 MB liegt.  
  
 Mithilfe der folgenden Abfrage können Sie untersuchen, worauf die Datenträgerbelegung zurückzuführen ist. Die Größe auf dem Datenträger, die von dieser Abfrage zurückgegeben wird, stellt bei Dateien mit Status 5 (REQUIRED FOR BACKUP/HA), 6 (IN TRANSITION TO TOMBSTONE) oder 7 (TOMBSTONE) einen Schätzwert dar.  
  
```sql
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(CASE  
   WHEN state = 5 AND file_type=0 THEN 128*1024*1024  
   WHEN state = 5 AND file_type=1 THEN 8*1024*1024  
   WHEN state IN (6,7) THEN 68*1024*1024  
   ELSE file_size_in_bytes  
    END) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  
 Im Anfangszustand erzielt das Beispiel Ergebnisse, die in etwa denen eines Servers mit 16 logischen Prozessoren entsprechen:  
  
| state_desc | file_type_desc | count | Größe auf dem Datenträger (MB) |
| ---------- | -------------- | ----- | --------------- |
|PRECREATED|DATA|16|2048|  
|PRECREATED|DELTA|16|128|  
|UNDER CONSTRUCTION|DATA|1|128|  
|UNDER CONSTRUCTION|DELTA|1|8|  

 Wie Sie sehen, wird der meiste Speicherplatz durch vorab erstellte Daten- und Änderungsdateien belegt. SQL Server hat vorab ein Dateipaar (bestehend aus Daten- und Änderungsdatei) pro logischem Prozessor erstellt. Darüber hinaus wird für Datendateien vorab eine Größe von 128 MB und für Änderungsdateien eine Größe von 8 MB festgelegt. So können Daten effizienter in diese Dateien eingefügt werden.  
  
 Die tatsächlichen Daten der speicheroptimierten Tabellen sind in einer einzelnen Datendatei gespeichert.  
  
#### <a name="after-running-the-workload"></a>Nach dem Ausführen der Arbeitsauslastung  
 Nach einem einzelnen Testlauf, bei dem 10 Millionen Verkaufsaufträge eingefügt wurden, wird in etwa folgender Gesamtspeicherplatz auf dem Datenträger belegt (Testserver mit 16 Kernen):  
  
```sql
SELECT SUM(df.size) * 8 / 1024 AS [On-disk size in MB]  
FROM sys.filegroups f JOIN sys.database_files df   
   ON f.data_space_id=df.data_space_id  
WHERE f.type=N'FX'  
```  
  
| Größe auf dem Datenträger (MB) |
| ------------------ |
|8828|
  
 Die Größe auf dem Datenträger liegt bei etwa 9 GB. Dies entspricht weitestgehend der Größe der Daten im Arbeitsspeicher.  
  
 Im Folgenden sind die Größen der Prüfpunktdateien in den einzelnen Phasen aufgeschlüsselt:  
  
```sql
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(CASE  
   WHEN state = 5 AND file_type=0 THEN 128*1024*1024  
   WHEN state = 5 AND file_type=1 THEN 8*1024*1024  
   WHEN state IN (6,7) THEN 68*1024*1024  
   ELSE file_size_in_bytes  
    END) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  
| state_desc | file_type_desc | count | Größe auf dem Datenträger (MB) |
| ---------- | -------------- | ----- | --------------- |
|PRECREATED|DATA|16|2048|  
|PRECREATED|DELTA|16|128|  
|UNDER CONSTRUCTION|DATA|1|128|  
|UNDER CONSTRUCTION|DELTA|1|8|  

 Es sind weiterhin 16 vorab erstellte Dateipaare verfügbar, die nach dem Schließen der Prüfpunkte sofort einsatzbereit sind.  
  
 Ein Paar wird gerade erstellt, das bis zum Schließen des aktuellen Prüfpunkts verwendet wird. Zusammen mit den aktiven Prüfpunktdateien ergeben 6,5 GB Daten im Arbeitsspeicher eine Datenträgernutzung von ca. 6,5 GB. Wie Sie wissen, werden Indizes nicht dauerhaft auf dem Datenträger gespeichert. Folglich ist die Gesamtgröße auf dem Datenträger in diesem Fall kleiner als die Datengröße im Arbeitsspeicher.  
  
#### <a name="after-demo-reset"></a>Nach dem Zurücksetzen der exemplarischen Arbeitauslastung  
 Nach dem Zurücksetzen der exemplarischen Arbeitsauslastung wird der Speicherplatz auf dem Datenträger nicht sofort freigegeben, wenn das System keine Transaktionen ausführt und keine Datenbank-Prüfpunkte vorhanden sind. Damit Prüfpunktdateien die verschiedenen Phasen durchlaufen und schließlich entfernt werden können, müssen eine Reihe von Prüfpunkten sowie Protokollkürzungen vorangegangen sein. Das ist erforderlich, um die Zusammenführung von Prüfpunktdateien sowie die Garbage Collection zu initiieren. Wenn das System Transaktionen ausführt (und Sie bei Verwendung des vollständigen Wiederherstellungsmodells regelmäßige Protokollsicherungen vornehmen), erfolgt dieser Schritt automatisch. Befindet sich das System wie im Beispielszenario jedoch im Leerlauf, wird dieser Schritt nicht ausgeführt.  
  
 Nach dem Zurücksetzen der exemplarischen Arbeitauslastung sehen Sie in etwa folgende Ergebnisse  
  
```sql
SELECT SUM(df.size) * 8 / 1024 AS [On-disk size in MB]  
FROM sys.filegroups f JOIN sys.database_files df   
   ON f.data_space_id=df.data_space_id  
WHERE f.type=N'FX'  
```  
  
| Größe auf dem Datenträger (MB) |
| ------------------ |
|11839|
  
 Mit fast 12 GB liegen die Ergebnisse deutlich über den 9 GB vor dem Zurücksetzen der exemplarischen Arbeitsauslastung. Dies ist darauf zurückzuführen, dass die Zusammenführung einiger Prüfpunktdateien zwar bereits gestartet, einige der Zusammenführungsziele jedoch noch nicht installiert wurden. Darüber hinaus wurden einige der zusammengeführten Quelldateien noch nicht bereinigt, wie im Folgenden ersichtlich:  
  
```sql
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(CASE  
   WHEN state = 5 AND file_type=0 THEN 128*1024*1024  
   WHEN state = 5 AND file_type=1 THEN 8*1024*1024  
   WHEN state IN (6,7) THEN 68*1024*1024  
   ELSE file_size_in_bytes  
    END) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  
| state_desc | file_type_desc | count | Größe auf dem Datenträger (MB) |
| ---------- | -------------- | ----- | --------------- |
|PRECREATED|DATA|16|2048|  
|PRECREATED|DELTA|16|128|  
|ACTIVE|DATA|38|5152|  
|ACTIVE|DELTA|38|1331|  
|MERGE TARGET|DATA|7|896|  
|MERGE TARGET|DELTA|7|56|  
|MERGED SOURCE|DATA|13|1772|  
|MERGED SOURCE|DELTA|13|455|  

 Sobald Transaktionsaktivitäten im System auftreten, werden Zusammenführungsziele installiert und zusammengeführte Quelldateien bereinigt.  
  
 Nachdem die exemplarische Arbeitsauslastung ein zweites Mal ausgeführt, zurückgesetzt und 10 Millionen Verkaufsaufträge eingefügt wurden, werden Sie feststellen, dass die während der ersten Ausführung der Arbeitsauslastung erstellten Dateien bereinigt wurden. Wenn Sie die vorangehende Abfrage bei aktiver Arbeitsauslastung mehrere Male ausführen, können Sie beobachten, wie die Prüfpunktdateien die verschiedenen Phasen durchlaufen.  
  
 Nachdem die Arbeitsauslastung zum zweiten Mal ausgeführt und 10 Millionen Verkaufsaufträge eingefügt wurden, ist die Datenträgernutzung ähnlich (wenn auch nicht identisch) mit der Nutzung nach der ersten Ausführung. Dies liegt an der Dynamik des Systems. Beispiel:  
  
```sql
SELECT state_desc  
 , file_type_desc  
 , COUNT(*) AS [count]  
 , SUM(CASE  
   WHEN state = 5 AND file_type=0 THEN 128*1024*1024  
   WHEN state = 5 AND file_type=1 THEN 8*1024*1024  
   WHEN state IN (6,7) THEN 68*1024*1024  
   ELSE file_size_in_bytes  
    END) / 1024 / 1024 AS [on-disk size MB]   
FROM sys.dm_db_xtp_checkpoint_files  
GROUP BY state, state_desc, file_type, file_type_desc  
ORDER BY state, file_type  
```  
  
| state_desc | file_type_desc | count | Größe auf dem Datenträger (MB) |
| ---------- | -------------- | ----- | --------------- |
|PRECREATED|DATA|16|2048|  
|PRECREATED|DELTA|16|128|  
|UNDER CONSTRUCTION|DATA|2|268|  
|UNDER CONSTRUCTION|DELTA|2|16|  
|ACTIVE|DATA|41|5608|  
|ACTIVE|DELTA|41|328|  

 In diesem Fall gibt es zwei Prüfpunktdateipaare mit dem Status „under construction“. Das legt die Vermutung nahe, dass aufgrund des hohen Parallelitätsgrads der Arbeitsauslastung mehrere Dateipaare in den Status „under construction“ versetzt wurden. Mehrere gleichzeitige Threads erforderten also zur selben Zeit ein neues Dateipaar, wodurch sich der Status eines Paares von „precreated“ in „under construction“ geändert hat.  
  
## <a name="see-also"></a>Weitere Informationen

[In-Memory-OLTP &#40;Arbeitsspeicheroptimierung&#41;](in-memory-oltp-in-memory-optimization.md)