---
title: SQL Server Parallel Data Warehouse-Verbindungstyp | Microsoft-Dokumentation
description: Verwenden Sie die Informationen in diesem Artikel zum Verbindungstyp „SQL Server Parallel Data Warehouse“, um herauszufinden, wie Sie eine Datenquelle erstellen.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 3925fd3d-2aa1-4768-96ad-cfc2c0ba9283
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: dfa373b98dfda2af0c8d18b1cc58fc7f0df5a1cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030992"
---
# <a name="sql-server-parallel-data-warehouse-connection-type-ssrs"></a>SQL Server Parallel Data Warehouse-Verbindungstyp (SSRS)

  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDWCurrentFull](../../includes/ssdwcurrentfull-md.md)] ist eine skalierbare Data Warehouse-Anwendung, die durch massive Parallelverarbeitung Leistung und Skalierbarkeit bietet. [!INCLUDE[ssDW](../../includes/ssdw-md.md)] verwendet SQL Server-Datenbanken für die verteilte Verarbeitung und Datenspeicherung.  
  
 Die Anwendung partitioniert große Datenbanktabellen auf mehrere physische Knoten, wobei jeder Knoten eine eigene Instanz von SQL Server ausführt. Wenn ein Bericht eine Verbindung mit [!INCLUDE[ssDW](../../includes/ssdw-md.md)] herstellt, um Berichtsdaten abzurufen, wird eine Verbindung mit dem Steuerungsknoten hergestellt, der die Abfrageverarbeitung in der [!INCLUDE[ssDW](../../includes/ssdw-md.md)] -Anwendung verwaltet. Nachdem die Verbindung hergestellt wurde, arbeiten Sie mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz in einer [!INCLUDE[ssDW](../../includes/ssdw-md.md)] -Umgebung wie mit anderen Instanzen.  
  
 Sie müssen über ein Dataset verfügen, das auf einer Berichtsdatenquelle vom Typ „[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Parallel Data Warehouse“ basiert, um Daten aus [!INCLUDE[ssDW](../../includes/ssdw-md.md)] in Ihren Bericht einzufügen. Dieser integrierte Datenquellentyp basiert auf der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Parallel Data Warehouse-Datenerweiterung. Verwenden Sie diesen Datenquellentyp, um eine Verbindung mit [!INCLUDE[ssDW](../../includes/ssdw-md.md)]herzustellen und Daten abzurufen.  
  
 Diese Datenerweiterung unterstützt mehrwertige Parameter, Serveraggregate und getrennt von der Verbindungszeichenfolge verwaltete Anmeldeinformationen.  
   
  
 Verwenden Sie die Informationen in diesem Thema, um eine Datenquelle zu erstellen. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Hinzufügen und Prüfen einer Datenverbindung &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
##  <a name="connection-string"></a><a name="Connection"></a> Verbindungszeichenfolge  
 Wenn Sie eine Verbindung mit [!INCLUDE[ssDW](../../includes/ssdw-md.md)]herstellen, stellen Sie eine Verbindung mit einem Datenbankobjekt in einer [!INCLUDE[ssDW](../../includes/ssdw-md.md)] -Anwendung her. Sie geben das zu verwendende Datenbankobjekt im Abfrage-Designer an. Falls Sie in der Verbindungszeichenfolge keine Datenbank angeben, wird eine Verbindung mit der vom Administrator zugewiesenen Standarddatenbank hergestellt. Erfragen Sie bei Ihrem Datenbankadministrator die Verbindungsinformationen und die Anmeldeinformationen, die verwendet werden sollen, um eine Verbindung mit der Datenquelle herzustellen. In der Verbindungszeichenfolge im folgenden Beispiel wird die **CustomerSales**-Beispieldatenbank in der [!INCLUDE[ssDW](../../includes/ssdw-md.md)] -Anwendung angegeben.  
  
```  
HOST=<IP address>; database= CustomerSales; port=<port>  
```  
  
 Außerdem verwenden Sie das Dialogfeld **Datenquelleneigenschaften** , um Anmeldeinformationen wie Benutzername und Kennwort anzugeben. Die Optionen `User Id` und `Password` werden automatisch an die Verbindungszeichenfolge angefügt und müssen nicht als Teil der Verbindungszeichenfolge eingegeben werden. Die Benutzeroberfläche enthält auch Optionen zum Angeben der IP-Adresse des Steuerungsknotens in der [!INCLUDE[ssDW](../../includes/ssdw-md.md)] -Anwendung und der Portnummer. Die Standardeinstellung ist der Port 17000. Da der Port vom Administrator konfiguriert werden kann, wird in der Verbindungszeichenfolge möglicherweise eine andere Portnummer verwendet.  
  
 Weitere Informationen über Beispiele für Verbindungszeichenfolgen finden Sie unter [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
##  <a name="credentials"></a><a name="Credentials"></a> Anmeldeinformationen  
 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] verfügt über eine eigene Sicherheitstechnologie zum Implementieren und Speichern von Benutzernamen und Kennwörtern. Die Windows-Authentifizierung kann nicht verwendet werden. Wenn Sie versuchen, mithilfe der Windows-Authentifizierung eine Verbindung mit [!INCLUDE[ssDW](../../includes/ssdw-md.md)] herzustellen, tritt ein Fehler auf.  
  
 Die Anmeldeinformationen müssen über Zugriffsberechtigungen für die Datenbank verfügen. Abhängig von der Abfrage benötigen Sie u. U. andere Berechtigungen, z. B. Berechtigungen für den Zugriff auf Tabellen und Sichten. Der Besitzer der externen Datenquelle muss entsprechende Anmeldeinformationen konfigurieren, die Lesezugriff auf die benötigten Datenbankobjekte gewähren.  
  
 Auf einem Berichterstellungsclient sind die folgenden Optionen zum Angeben von Anmeldeinformationen verfügbar:  
  
-   Verwendung eines gespeicherten Benutzernamens und eines gespeicherten Kennworts. Aktivieren Sie Optionen zur Verwendung der Anmeldeinformationen als Windows-Anmeldeinformationen, um den doppelten Sprung auszuhandeln, der auftritt, wenn sich die Datenbank mit den Berichtsdaten vom Berichtsserver unterscheidet. Sie können auch die Identität des authentifizierten Benutzers annehmen, nachdem die Verbindung mit der Datenquelle hergestellt wurde.  
  
-   Anmeldeinformationen sind nicht erforderlich. Zur Verwendung dieser Option müssen Sie zuvor das Konto für die unbeaufsichtigte Ausführung auf dem Berichtsserver konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren des Kontos für die unbeaufsichtigte Ausführung &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md). 
  
 Weitere Informationen finden Sie unter [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) oder [Angeben der Anmeldeinformationen und Verbindungsinformationen für Berichtsdatenquellen](specify-credential-and-connection-information-for-report-data-sources.md).  
  
  
##  <a name="queries"></a><a name="Query"></a> Abfragen  
 Mit einer Abfrage wird angegeben, welche Daten für ein Berichtsdataset abgerufen werden sollen.  
  
 Die Feldauflistung für ein Dataset wird mit den Spalten aus dem Resultset einer Abfrage aufgefüllt. Wenn die Abfrage mehrere Resultsets zurückgibt, verarbeitet der Bericht nur das erste Resultset, das durch eine Abfrage abgerufen wird. Wenn Sie eine neue Abfrage erstellen oder eine vorhandene Abfrage öffnen, die im grafischen Abfrage-Designer dargestellt werden kann, ist der relationale Abfrage-Designer standardmäßig verfügbar. Zum Angeben einer Abfrage stehen Ihnen folgende Methoden zur Auswahl:  
  
-   Erstellen Sie eine Abfrage interaktiv. Verwenden Sie den relationalen Abfrage-Designer, in dem eine nach Datenbankschema geordnete hierarchische Ansicht der Tabellen, Sichten und anderen Datenbankelemente angezeigt wird. Wählen Sie Spalten aus Tabellen oder Sichten aus. Beschränken Sie die Anzahl abzurufender Datenzeilen mit Filterkriterien, einer Gruppierung und Aggregaten. Passen Sie den Filter an, wenn der Bericht ausgeführt wird, indem Sie die Parameteroption festlegen.  
  
-   Geben Sie eine Abfrage ein, oder fügen Sie sie ein. Verwenden Sie den textbasierten Abfrage-Designer, um [!INCLUDE[DWsql](../../includes/dwsql-md.md)] -Text direkt einzugeben, Abfragetext aus einer anderen Quelle einzufügen, komplexe Abfragen einzugeben, die mit dem relationalen Abfrage-Designer nicht erstellt werden können, oder um abfragebasierte Ausdrücke einzugeben.  
  
-   Importiert eine vorhandene Abfrage aus einer Datei oder einem Bericht. Verwenden Sie die Schaltfläche **Abfrage importieren** in einem Abfrage-Designer, um nach einer SQL- oder RDL-Datei zu suchen und eine Abfrage zu importieren.  
  
 Weitere Informationen finden Sie unter [Benutzeroberfläche des relationalen Abfrage-Designers (Berichts-Generator)](../../reporting-services/report-data/relational-query-designer-user-interface-report-builder.md) und [Benutzeroberfläche des textbasierten Abfrage-Designers (Berichts-Generator)](../../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md).  
  
 Der textbasierte Abfrage-Designer unterstützt den [Textmodus](#QueryText) , in dem Sie [!INCLUDE[DWsql](../../includes/dwsql-md.md)] -Befehle zur Auswahl von Daten aus der Datenquelle eingeben.  
  
-   [Text](#QueryText)  
  
 Sie verwenden [!INCLUDE[DWsql](../../includes/dwsql-md.md)] mit [!INCLUDE[ssDW](../../includes/ssdw-md.md)] und [!INCLUDE[tsql](../../includes/tsql-md.md)] mit SQL Server. Die zwei Dialekte der SQL-Programmiersprache sind sehr ähnlich. Für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenquellenverbindungstyp geschriebene Abfragen können in der Regel für den [!INCLUDE[ssDWCurrentFull](../../includes/ssdwcurrentfull-md.md)] -Datenquellenverbindungstyp verwendet werden.  
  
 Eine Abfrage, die Berichtsdaten aus einer großen Datenbank abruft (z. B. einem Data Warehouse wie [!INCLUDE[ssDW](../../includes/ssdw-md.md)]), kann ein Resultset mit sehr vielen Zeilen generieren, sofern Sie die Daten nicht aggregieren und zusammenfassen, um die Anzahl der zurückgegebenen Zeilen zu reduzieren. Abfragen, die Aggregate und eine Gruppierung enthalten, können mit dem grafischen oder dem textbasierten Abfrage-Designer geschrieben werden.  
  
 [!INCLUDE[DWsql](../../includes/dwsql-md.md)] unterstützt die Klausel, das Schlüsselwort und die Aggregate, die der Abfrage-Designer bereitstellt, um Daten zusammenzufassen.  
  
 Der von [!INCLUDE[ssDW](../../includes/ssdw-md.md)] verwendete grafische Abfrage-Designer bietet integrierte Unterstützung für das Gruppieren und Aggregate. Sie können daher Abfragen schreiben, durch die nur Zusammenfassungsdaten abgerufen werden. Die [!INCLUDE[DWsql](../../includes/dwsql-md.md)] -Sprachfunktionen sind die GROUP BY-Klausel, das DISTINCT-Schlüsselwort und Aggregate wie SUM und COUNT. Der textbasierte Abfrage-Designer bietet vollständige Unterstützung für die [!INCLUDE[DWsql](../../includes/dwsql-md.md)] -Sprache, einschließlich Gruppieren und Aggregate.  
  
 Weitere Informationen über [!INCLUDE[tsql](../../includes/tsql-md.md)] finden Sie unter [Transact-SQL-Referenz &#40;Datenbank-Engine&#41;](../../t-sql/language-reference.md).  
  
###  <a name="using-query-type-text"></a><a name="QueryText"></a> Verwenden des Abfragetyps "Text"  
 Im textbasierten Abfrage-Designer geben Sie [!INCLUDE[DWsql](../../includes/dwsql-md.md)] -Befehle ein, um die Daten in einem Dataset zu definieren. Die Abfragen, mit denen Sie Daten aus [!INCLUDE[ssDW](../../includes/ssdw-md.md)] abrufen, sind mit denen identisch, die Sie zum Abrufen von Daten aus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanzen verwenden, die nicht innerhalb einer [!INCLUDE[ssDW](../../includes/ssdw-md.md)] -Anwendung ausgeführt werden. Mit der folgenden [!INCLUDE[DWsql](../../includes/dwsql-md.md)] -Abfrage werden z. B. die Namen aller Mitarbeiter ausgewählt, die Marketingassistenten sind:  
  
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
  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parameter  
 Wenn Abfragetext Abfragevariablen oder gespeicherte Prozeduren mit Eingabeparametern enthält, werden die entsprechenden Abfrageparameter für das Dataset und Berichtsparameter für den Bericht automatisch generiert. Der Abfragetext darf keine DECLARE-Anweisung für jede Abfragevariable enthalten.  
  
 Durch die folgende SQL-Abfrage wird z. B. ein Berichtsparameter mit dem Namen **EmpID** erstellt:  
  
```  
SELECT FirstName, LastName FROM HumanResources.Employee E INNER JOIN  
       Person.Contact C ON  E.ContactID=C.ContactID   
WHERE EmployeeID = (@EmpID)  
```  
  
 Jeder Berichtsparameter ist standardmäßig vom Datentyp "Text" und verfügt über ein automatisch erstelltes Dataset, mit dem eine Dropdownliste verfügbarer Werte bereitgestellt wird. Die Standardwerte müssen möglicherweise nach dem Erstellen der Berichtsparameter geändert werden. Weitere Informationen finden Sie unter [Berichtsparameter &#40;Berichts-Generator und Berichts-Designer&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)" basiert.  
  
  
##  <a name="remarks"></a><a name="Remarks"></a> Hinweise  
  
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

## <a name="next-steps"></a>Nächste Schritte

[Berichtsparameter](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
[Filtern, Gruppieren und Sortieren von Daten](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
[Ausdrücke](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)