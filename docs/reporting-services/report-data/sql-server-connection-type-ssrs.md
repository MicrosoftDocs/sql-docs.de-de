---
title: SQL Server-Verbindungstyp | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über den SQL Server-Verbindungstyp und das Hinzufügen von Daten aus einer SQL Server-Datenbank in Ihren Bericht.
ms.date: 08/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 957e7091-e08f-48d2-9506-872227ae8b20
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 347ad41fb6165b3ab9364ee799aac3d3629ee78a
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91935469"
---
# <a name="sql-server-connection-type-ssrs"></a>SQL Server-Verbindungstyp (SSRS)
  Wenn Sie Daten aus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenquelle zu Ihrem Bericht hinzufügen möchten, benötigen Sie ein Dataset, das auf einer Berichtsdatenquelle vom Typ „[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]“ basiert. Dieser integrierte Datenquellentyp basiert auf der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenerweiterung. Verwenden Sie diesen Datenquellentyp, um eine Verbindung mit der aktuellen Version und früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbanken herzustellen und Daten abzurufen.  
  
 Diese Datenerweiterung unterstützt mehrwertige Parameter, Serveraggregate und getrennt von der Verbindungszeichenfolge verwaltete Anmeldeinformationen.  
  
 Verwenden Sie die Informationen in diesem Thema, um eine Datenquelle zu erstellen. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Hinzufügen und Prüfen einer Datenverbindung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
##  <a name="connection-string"></a><a name="Connection"></a> Verbindungszeichenfolge  
 Wenn Sie eine Verbindung mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank herstellen, stellen Sie eine Verbindung mit dem Datenbankobjekt in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz auf einem Server her. Für die Datenbank können mehrere Schemas mit mehreren Tabellen, Sichten und gespeicherten Prozeduren vorhanden sein. Sie geben das zu verwendende Datenbankobjekt im Abfrage-Designer an. Falls Sie in der Verbindungszeichenfolge keine Datenbank angeben, wird eine Verbindung mit der vom Datenbankadministrator zugewiesenen Standarddatenbank hergestellt.  
  
 Erfragen Sie bei Ihrem Datenbankadministrator die Verbindungsinformationen und die Anmeldeinformationen, die verwendet werden sollen, um eine Verbindung mit der Datenquelle herzustellen. In der Verbindungszeichenfolge im folgenden Beispiel wird eine Beispieldatenbank auf dem lokalen Client angegeben:  
  
```  
Data Source=<server>;Initial Catalog=AdventureWorks  
```  
  
 Weitere Informationen über Beispiele für Verbindungszeichenfolgen finden Sie unter [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
##  <a name="credentials"></a><a name="Credentials"></a> Anmeldeinformationen  
 Anmeldeinformationen sind erforderlich, um Abfragen auszuführen und den Bericht lokal oder vom Berichtsserver aus in der Vorschau anzuzeigen.  
  
 Nachdem Sie den Bericht veröffentlicht haben, müssen Sie eventuell die Anmeldeinformationen für die Datenquelle ändern, sodass die Berechtigungen zum Abrufen der Daten beim Ausführen des Berichts auf dem Berichtsserver gültig sind.  
  
 Auf einem Berichterstellungsclient sind die folgenden Optionen zum Angeben von Anmeldeinformationen verfügbar:  
  
-   Aktueller Windows-Benutzer (auch bekannt als integrierte Sicherheit).  
  
-   Verwendung eines gespeicherten Benutzernamens und eines gespeicherten Kennworts.  
  
-   Aufforderung zur Eingabe der Anmeldeinformationen. Diese Option unterstützt nur die integrierte Windows-Sicherheit.  
  
-   Anmeldeinformationen sind nicht erforderlich. Zur Verwendung dieser Option müssen Sie zuvor das Konto für die unbeaufsichtigte Ausführung auf dem Berichtsserver konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren des Kontos für die unbeaufsichtigte Ausführung &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md). 
  
 Weitere Informationen finden Sie unter [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) oder [Angeben der Anmeldeinformationen und Verbindungsinformationen für Berichtsdatenquellen](specify-credential-and-connection-information-for-report-data-sources.md).  
  
  
##  <a name="queries"></a><a name="Query"></a> Abfragen  
 Mit einer Abfrage wird angegeben, welche Daten für ein Berichtsdataset abgerufen werden sollen. Die Feldauflistung für ein Dataset wird mit den Spalten aus dem Resultset einer Abfrage aufgefüllt. In Berichten wird nur das erste Resultset verarbeitet, das von einer Abfrage abgerufen wird.  
  
 Wenn Sie eine neue Abfrage erstellen oder eine vorhandene Abfrage öffnen, die im grafischen Abfrage-Designer dargestellt werden kann, ist der relationale Abfrage-Designer standardmäßig verfügbar. Zum Angeben einer Abfrage stehen Ihnen folgende Methoden zur Auswahl:  
  
-   Erstellen Sie eine Abfrage interaktiv. Verwenden Sie den relationalen Abfrage-Designer, der eine hierarchische Ansicht der Tabellen, Sichten, gespeicherten Prozeduren und anderen Datenbankelemente nach Datenbankschema angeordnet anzeigt. Wählen Sie Spalten aus Tabellen oder Sichten aus, oder geben Sie gespeicherte Prozeduren oder Tabellenwertfunktionen an. Begrenzen Sie die Anzahl abzurufender Datenzeilen durch Angabe von Filterkriterien. Passen Sie den Filter an, wenn der Bericht ausgeführt wird, indem Sie die Parameteroption festlegen.  
  
-   Geben Sie eine Abfrage ein, oder fügen Sie sie ein. Verwenden Sie den textbasierten Abfrage-Designer, um [!INCLUDE[tsql](../../includes/tsql-md.md)] -Text direkt einzugeben, Abfragetext aus einer anderen Quelle einzufügen, komplexe Abfragen einzugeben, die mit dem relationalen Abfrage-Designer nicht erstellt werden können, oder um abfragebasierte Ausdrücke einzugeben.  
  
-   Importiert eine vorhandene Abfrage aus einer Datei oder einem Bericht. Verwenden Sie die Schaltfläche **Abfrage importieren** in einem Abfrage-Designer, um nach einer SQL- oder RDL-Datei zu suchen und eine Abfrage zu importieren.  
  
 Weitere Informationen finden Sie unter [Benutzeroberfläche des relationalen Abfrage-Designers (Berichts-Generator)](../../reporting-services/report-data/relational-query-designer-user-interface-report-builder.md) und [Benutzeroberfläche des textbasierten Abfrage-Designers (Berichts-Generator)](../../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md).  
  
 Die folgenden Abfragemodi werden unterstützt:  
  
-   [Text](#QueryText) Geben Sie [!INCLUDE[tsql](../../includes/tsql-md.md)] -Befehle ein.  
  
-   [Gespeicherte Prozedur](#QueryStoredProcedure) Treffen Sie in einer Liste von gespeicherten Prozeduren eine Auswahl.  
  
###  <a name="using-query-type-text"></a><a name="QueryText"></a> Verwenden des Abfragetyps "Text"  
 Im textbasierten Abfrage-Designer können Sie [!INCLUDE[tsql](../../includes/tsql-md.md)] -Befehle eingeben, um die Daten in einem Dataset zu definieren. Mit der folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)] -Abfrage werden z. B. die Namen aller Mitarbeiter ausgewählt, die Marketingassistenten sind:  
  
```  
SELECT  
  HumanResources.Employee.BusinessEntityID  
  ,HumanResources.Employee.JobTitle  
  ,Person.Person.FirstName  
  ,Person.Person.LastName  
FROM  
  Person.Person  
  INNER JOIN HumanResources.Employee  
    ON Person.Person.BusinessEntityID = HumanResources.Employee.BusinessEntityID  
WHERE HumanResources.Employee.JobTitle = 'Marketing Assistant'   
```  
  
 Klicken Sie auf der Symbolleiste auf die Schaltfläche **Ausführen** ( **!** ), um die Abfrage auszuführen und ein Resultset anzuzeigen.  
  
 Um diese Abfrage zu parametrisieren, fügen Sie einen Abfrageparameter hinzu. Beispiel: Ändern Sie die WHERE-Klausel wie folgt:  
  
 `WHERE HumanResources.Employee.JobTitle = (@JobTitle)`  
  
 Bei der Ausführung der Abfrage werden automatisch Berichtsparameter erstellt, die den Abfrageparametern entsprechen. Weitere Informationen finden Sie unter [Abfrageparameter](#Parameters) weiter unten in diesem Thema.  
  
  
###  <a name="using-query-type-storedprocedure"></a><a name="QueryStoredProcedure"></a> Verwenden des Abfragetyps "StoredProcedure"  
 Zum Angeben einer gespeicherte Prozedur für eine Datasetabfrage stehen Ihnen folgende Methoden zur Verfügung:  
  
-   Legen Sie im Dialogfeld **Dataseteigenschaften** die Option **Gespeicherte Prozedur** fest. Wählen Sie aus der Dropdownliste mit gespeicherten Prozeduren und Tabellenwertfunktionen aus.  
  
-   Wählen Sie im relationalen Abfrage-Designer im Bereich Datenbanksicht eine gespeicherte Prozedur oder Tabellenwertfunktion aus.  
  
-   Wählen Sie im textbasierten Abfrage-Designer **StoredProcedure** auf der Symbolleiste aus.  
  
 Nachdem Sie eine gespeicherte Prozedur oder Tabellenwertfunktion gewählt haben, können Sie die Abfrage ausführen. Sie werden zur Eingabe von Eingabeparameterwerten aufgefordert. Bei der Ausführung der Abfrage werden automatisch Berichtsparameter erstellt, die den Eingabeparametern entsprechen. Weitere Informationen finden Sie unter [Abfrageparameter](#Parameters) weiter unten in diesem Thema.  
  
 Nur das erste Resultset wird unterstützt, das für eine gespeicherte Prozedur abgerufen wird. Wenn eine gespeicherte Prozedur mehrere Resultsets zurückgibt, wird nur das erste Resultset verwendet.  
  
 Falls eine gespeicherte Prozedur einen Parameter mit einem Standardwert enthält, können Sie auf diesen Wert zugreifen, indem Sie das DEFAULT-Schlüsselwort als Wert für den Parameter verwenden. Wenn der Abfrageparameter mit einem Berichtsparameter verknüpft ist, kann der Benutzer das Wort DEFAULT im Eingabefeld für den Berichtsparameter eingeben oder auswählen.  
  
 Weitere Informationen finden Sie unter [Gespeicherte Prozeduren (Datenbank-Engine)](../../relational-databases/stored-procedures/stored-procedures-database-engine.md).  
  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parameter  
 Wenn Abfragetext Abfragevariablen oder gespeicherte Prozeduren mit Eingabeparametern enthält, werden die entsprechenden Abfrageparameter für das Dataset und Berichtsparameter für den Bericht automatisch generiert. Der Abfragetext darf keine DECLARE-Anweisung für jede Abfragevariable enthalten.  
  
 Durch die folgende SQL-Abfrage wird z. B. ein Berichtsparameter mit dem Namen **EmpID**erstellt:  
  
```  
SELECT FirstName, LastName FROM HumanResources.Employee E INNER JOIN  
       Person.Contact C ON  E.ContactID=C.ContactID   
WHERE EmployeeID = (@EmpID)  
```  
  
 Berichtsparameter werden mit Standardeigenschaftswerten erstellt, die Sie ggf. ändern müssen. Beispiel:  
  
-   Standardmäßig ist jeder Berichtsparameter vom Datentyp **Text**. Wenn die zugrunde liegenden Daten von einem anderen Datentyp sind, müssen Sie den Parameterdatentyp ändern.  
  
-   Wenn Sie die Option für mehrwertige Parameter aktivieren, müssen Sie die Abfrage manuell ändern, um mit dem **IN** -Operator zu überprüfen, ob Werte Teil eines Satzes sind, z.B. `WHERE EmployeeID IN (@EmpID)`.  
  
 Weitere Informationen finden Sie unter [Berichtsparameter &#40;Berichts-Generator und Berichts-Designer&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)" basiert.  
  
  
##  <a name="remarks"></a><a name="Remarks"></a> Hinweise  
 Mit einem OLE DB- oder einem ODBC-Datenquellentyp können Sie Daten auch aus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank abrufen. Weitere Informationen finden Sie unter [OLE DB-Verbindungstyp (SSRS)](../../reporting-services/report-data/ole-db-connection-type-ssrs.md) oder unter [ODBC-Verbindungstyp (SSRS)](../../reporting-services/report-data/odbc-connection-type-ssrs.md).  
  
###### <a name="platform-and-version-information"></a>Plattform- und Versionsinformationen  
 Weitere Informationen zur Plattform- und Versionsunterstützung finden Sie unter [Von Reporting Services unterstützte Datenquellen &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
  
##  <a name="how-to-topics"></a><a name="HowTo"></a> Themen zur Vorgehensweise  
 Dieser Abschnitt enthält schrittweise Anweisungen zum Arbeiten mit Datenverbindungen, Datenquellen und Datasets.  
  
 [Hinzufügen und Prüfen einer Datenverbindung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)  
  
 [Erstellen eines freigegebenen Datasets oder eingebetteten Datasets &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)  
  
 [Hinzufügen eines Filters zu einem Dataset &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md)  
  
  
##  <a name="related-sections"></a><a name="Related"></a> Verwandte Abschnitte  
 Diese Abschnitte der Dokumentation enthalten umfassende grundlegende Informationen zu Berichtsdaten und Informationen zum Definieren, Entwerfen, Anpassen und Verwenden der mit Daten zusammenhängenden Teile eines Berichts.  
  
 [Berichtsdatasets &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)  
 Bietet eine Übersicht über den Zugriff auf Daten für den Bericht.  
  
 [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)  
 Enthält Informationen zu Datenverbindungen und Datenquellen.  
  
 [Erstellen von Berichten zu eingebetteten und freigegebenen Datasets &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
 Enthält Informationen zu eingebetteten und freigegebenen Datasets.  
  
 [Datasetfelder-Sammlung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)  
 Enthält Informationen zur von der Abfrage generierten Datasetfeldauflistung.  
  
 [Von Reporting Services unterstützte Datenquellen &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)  
 Enthält ausführliche Informationen zur Plattform- und Versionsunterstützung für die einzelnen Datenerweiterungen.  
  
  
## <a name="see-also"></a>Weitere Informationen  
 [Berichtsparameter &#40;Berichts-Generator und Berichts-Designer&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Filtern, Gruppieren und Sortieren von Daten &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Ausdrücke &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
  
