---
title: Ermitteln der richtigen Azure SQL-Datenbank-SKU für Ihre lokale Datenbank (Datenmigrations-Assistent) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Datenmigrations-Assistent verwenden können, um die richtige Azure SQL-Datenbank oder Azure SQL verwaltete Instanz SKU für Ihre lokale Datenbank zu identifizieren.
ms.custom: ''
ms.date: 05/06/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.openlocfilehash: b29815876d804f387ce31754c960e80f9db99434
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247169"
---
# <a name="identify-the-right-azure-sql-database-or-sql-managed-instance-sku-for-your-on-premises-database"></a>Identifizieren der richtigen Azure SQL-Datenbank-oder SQL verwaltete Instanz-SKU für Ihre lokale Datenbank

Das Migrieren von Datenbanken in die Cloud kann kompliziert sein, insbesondere bei der Auswahl der besten Azure SQL-Datenbank oder des SQL verwaltete Instanz-Ziels und der SKU für Ihre Datenbank. Das Ziel des Daten Bank Migration Assistant (DMA) ist es, diese Fragen zu beantworten und die Daten Bank Migration zu vereinfachen, indem Sie diese SKU-Empfehlungen in einer benutzerfreundlichen Ausgabe bereitstellen.


Mit der SKU-Empfehlungs Funktion können Sie die empfohlene Mindestanzahl von Azure SQL-Datenbank oder Azure SQL verwaltete Instanz-SKU basierend auf Leistungsindikatoren ermitteln, die auf den Computern gesammelt werden, auf denen die Datenbanken gehostet werden. Die-Funktion bietet Empfehlungen im Zusammenhang mit Tarif, computeebene und maximaler Datengröße sowie geschätzten Kosten pro Monat. Außerdem bietet es die Möglichkeit, für alle empfohlenen Datenbanken eine Massen Bereitstellung einzelner Datenbanken und verwalteter Instanzen durchzusetzen. Diese Funktion ist zurzeit nur über die Befehlszeilenschnittstelle (CLI) verfügbar.

Im folgenden finden Sie Anweisungen zum Bestimmen der SKU-Empfehlungen und zum Bereitstellen der entsprechenden Einzel Datenbanken oder verwalteten Instanzen in Azure mithilfe von DMA.

[!INCLUDE [online-offline](../includes/azure-migrate-to-assess-sql-data-estate.md)]

## <a name="prerequisites"></a>Voraussetzungen

- Laden Sie die neueste Version von [DMA](https://aka.ms/get-dma)herunter, und installieren Sie Sie. Wenn Sie bereits über eine frühere Version des Tools verfügen, öffnen Sie Sie, und Sie werden aufgefordert, DMA zu aktualisieren.
- Stellen Sie sicher, dass auf Ihrem Computer [PowerShell Version 5,1](https://www.microsoft.com/download/details.aspx?id=54616) oder höher angegeben ist, die zum Ausführen aller Skripts erforderlich ist. Informationen dazu, wie Sie herausfinden, welche Version von PowerShell auf Ihrem Computer installiert ist, finden Sie im Artikel [herunterladen und Installieren von Windows PowerShell 5,1](/skypeforbusiness/set-up-your-computer-for-windows-powershell/download-and-install-windows-powershell-5-1).
  > [!NOTE]
  > Zum Sammeln von Computer Informationen verwendet das datensammlungsskript das Get-WmiObject-Cmdlet, das in PowerShell 6 als veraltet markiert wurde. Zum Ausführen dieses Skripts in PowerShell 6 oder 7 müssen Sie die WMI-Cmdlets durch die neueren CIM-Cmdlets ersetzen.
- Stellen Sie sicher, dass auf Ihrem Computer das Azure PowerShell-Modul installiert ist. Weitere Informationen finden Sie im Artikel [Installieren des Azure PowerShell Moduls](/powershell/azure/install-az-ps?view=azps-1.8.0&preserve-view=true).
- Vergewissern Sie sich, dass die PowerShell-Datei **SkuRecommendationDataCollectionScript.ps1**, die für die Erfassung der Leistungsindikatoren erforderlich ist, im DMA-Ordner installiert ist.
- Stellen Sie sicher, dass der Computer, auf dem Sie diesen Prozess ausführen, über Administrator Berechtigungen für den Computer verfügt, auf dem die Datenbanken gehostet werden.

## <a name="collect-performance-counters"></a>Leistungsindikatoren erfassen

Der erste Schritt im Prozess besteht darin, Leistungsindikatoren für Ihre Datenbanken zu erfassen. Sie können Leistungsindikatoren erfassen, indem Sie einen PowerShell-Befehl auf dem Computer ausführen, der die-Datenbanken hostet. DMA bietet Ihnen eine Kopie dieser PowerShell-Datei, aber Sie können auch Ihre eigene Methode verwenden, um Leistungsindikatoren von Ihrem Computer zu erfassen.

Diese Aufgabe muss nicht für jede Datenbank einzeln durchgeführt werden. Die von einem Computer gesammelten Leistungsindikatoren können verwendet werden, um die SKU für alle auf dem Computer gehosteten Datenbanken zu empfehlen.

1. Suchen Sie im DMA-Ordner die PowerShell-Datei SkuRecommendationDataCollectionScript.ps1. Diese Datei ist erforderlich, um die Leistungsindikatoren zu erfassen.

    ![Im DMA-Ordner angezeigte PowerShell-Datei](../dma/media/dma-sku-recommend-data-collection-file.png)

2. Führen Sie das PowerShell-Skript mit den folgenden Argumenten aus:
    - **Computername**: der Name des Computers, auf dem die Datenbanken gehostet werden.
    - **Outputfilepath**: der Ausgabe Dateipfad zum Speichern der gesammelten Leistungsindikatoren.
    - **Collectiontimeinseconds**: die Zeitspanne, in der Leistungsdaten gesammelt werden sollen. Erfassen Sie Leistungsindikatoren für mindestens 40 Minuten, um eine sinnvolle Empfehlung zu erhalten. Wenn die Dauer der Erfassung länger dauert, desto genauer ist die Empfehlung. Stellen Sie außerdem sicher, dass die Workloads für die gewünschten Datenbanken ausgeführt werden, um genauere Empfehlungen zu ermöglichen.
    - **DbConnectionString**: die Verbindungs Zeichenfolge, die auf die Master-Datenbank verweist, die auf dem Computer gehostet wird, von dem aus Sie Leistungs Leistungsdaten sammeln.

    Hier ist ein Beispiel Aufruf:

    ```
    .\SkuRecommendationDataCollectionScript.ps1
     -ComputerName Foobar1
     -OutputFilePath D:\counters2.csv
     -CollectionTimeInSeconds 2400
     -DbConnectionString Server=localhost;Initial Catalog=master;Integrated Security=SSPI;
    ```

    Nachdem der Befehl ausgeführt wurde, gibt der Prozess eine Datei mit Leistungsindikatoren an den von Ihnen angegebenen Speicherort aus. Sie können diese Datei als Eingabe für den nächsten Teil des Prozesses verwenden, der SKU-Empfehlungen für Optionen für Einzel Datenbanken und verwaltete Instanzen bereitstellt.

## <a name="use-the-dma-cli-to-get-sku-recommendations"></a>Verwenden der DMA-CLI, um SKU-Empfehlungen zu erhalten

Verwenden Sie die Leistungsindikator-Ausgabedatei, die Sie als Eingabe für diesen Prozess erstellt haben.

Für die Einzeldaten Bankoption bietet DMA Empfehlungen für den Tarif der einzelnen Datenbanken der Azure SQL-Datenbank, die computeebene und die maximale Datengröße für jede Datenbank auf dem Computer. Wenn Sie über mehrere Datenbanken auf dem Computer verfügen, können Sie auch die Datenbanken angeben, für die Sie Empfehlungen erhalten möchten. DMA bietet Ihnen außerdem die geschätzten monatlichen Kosten für die einzelnen Datenbanken.

Für verwaltete Instanzen unterstützen die Empfehlungen ein Lift-and-Shift-Szenario. DMA bietet Ihnen daher Empfehlungen für den Azure SQL-verwaltete Instanz Tarif, die computeebene und die maximale Datengröße für die Daten Bank Gruppe auf dem Computer. Wenn Sie auf Ihrem Computer über mehrere Datenbanken verfügen, können Sie auch die Datenbanken angeben, für die Sie Empfehlungen erhalten möchten. DMA bietet Ihnen außerdem die geschätzten monatlichen Kosten für die verwaltete Instanz.

Wenn Sie die DMA-CLI verwenden möchten, um SKU-Empfehlungen zu erhalten, führen Sie an der Eingabeaufforderung dmacmd.exe mit den folgenden Argumenten aus:

- **/Action = skuempfehlungs**: Geben Sie dieses Argument ein, um SKU-Bewertungen auszuführen.
- **/SkuRecommendationInputDataFilePath**: der Pfad zu der im vorherigen Abschnitt gesammelten Datei mit der gegen Datei.
- **/SkuRecommendationTsvOutputResultsFilePath**: der Pfad zum Schreiben der Ausgabe ergibt das TSV-Format.
- **/SkuRecommendationJsonOutputResultsFilePath**: der Pfad zum Schreiben der Ausgabe ergibt das JSON-Format.
- **/SkuRecommendationHtmlResultsFilePath**: der Pfad zum Schreiben der Ausgabe ergibt das HTML-Format.

Wählen Sie außerdem eines der folgenden Argumente aus:

- Preis Aktualisierung verhindern
  - **/SkuRecommendationPreventPriceRefresh**: Wenn der Wert auf true festgelegt ist, wird die Preis Aktualisierung verhindert, und es werden die Standardpreise angenommen. Verwenden Sie, wenn Sie im Offline Modus ausgeführt werden. Wenn Sie diesen Parameter nicht verwenden, müssen Sie die unten aufgeführten Parameter angeben, um die aktuellen Preise basierend auf einem bestimmten Bereich zu erhalten.
- Aktuelle Preise erhalten
  - **/SkuRecommendationCurrencyCode**: die Währung, in der die Preise angezeigt werden (z. b. "USD").
  - **/SkuRecommendationOfferName**: der Angebots Name (z. b. "MS-AZR-0003p"). Weitere Informationen finden Sie auf der Seite [Microsoft Azure Angebots Details](https://azure.microsoft.com/support/legal/offer-details/) .
    - **/SkuRecommendationRegionName**: der Name der Region (z. b. "westus").
    - **/SkuRecommendationSubscriptionId**: die Abonnement-ID.
    - **/AzureAuthenticationTenantId**: der Authentifizierungs Mandant.
    - **/AzureAuthenticationClientId**: die Client-ID der Aad-APP, die für die Authentifizierung verwendet wird.
    - Eine der folgenden Authentifizierungs Optionen:
      - Interactive
        - **Azureauthenticationinteractiveauthentication**: bei einem Popup Fenster für die Authentifizierung auf true festgelegt.
      - Zertifikat basiert
        - **Azureauthenticationcertifierestoreloation:** auf den Zertifikat Speicherort (z. b. "CurrentUser") festgelegt.
        - **Azureauthenticationcertifichanethumbprint**: Legen Sie den Fingerabdruck des Zertifikats fest.
      - Tokenbasiert
        - **Azureauthenticationtoken**: wird auf das Zertifikat Token festgelegt.

> [!NOTE]
> Um ClientID und tenantid für die interaktive Authentifizierung zu erhalten, müssen Sie eine neue Aad-Anwendung konfigurieren. Weitere Informationen zur Authentifizierung und zum erhalten dieser Anmelde Informationen finden Sie im Artikel [Microsoft Azure Abrechnungs-API-Code Beispiele: Ratecard-API](https://github.com/Azure-Samples/billing-dotnet-ratecard-api). Befolgen Sie die Anweisungen unter **Schritt 1: Konfigurieren einer nativen Client Anwendung in Ihrem Aad-Mandanten**.

Schließlich gibt es ein optionales Argument, das Sie verwenden können, um die Datenbanken anzugeben, für die Sie Empfehlungen wünschen: 

- **/SkuRecommendationDatabasesToRecommend**: eine Liste der Datenbanken, für die Empfehlungen zu erstellen sind. Bei den Datenbanknamen muss die Groß-/Kleinschreibung beachtet werden, und (1) muss (1) in der Eingabe. CSV enthalten sein, (2) jede in doppelte Anführungszeichen eingeschlossen ist und (3) jeweils durch ein einzelnes Leerzeichen zwischen den Namen (z. b./SkuRecommendationDatabasesToRecommend = "Database1" "Database2" "Database3") getrennt werden. Wenn Sie diesen Parameter weglassen, stellen Sie sicher, dass die Empfehlungen für alle Benutzer Datenbanken bereitgestellt werden, die in der CSV-Eingabedatei  

Im folgenden finden Sie einige Beispiel Aufrufe:

**Beispiel 1: erhalten von Empfehlungen mit Standardpreisen. Verwenden Sie, wenn Sie im Offline Modus ausgeführt werden oder wenn Sie nicht über Anmelde Informationen für die Authentifizierung verfügen.**

```
.\DmaCmd.exe /Action=SkuRecommendation
/SkuRecommendationInputDataFilePath="C:\TestOut\out.csv"
/SkuRecommendationTsvOutputResultsFilePath="C:\TestOut\prices.tsv"
/SkuRecommendationJsonOutputResultsFilePath="C:\TestOut\prices.json"
/SkuRecommendationOutputResultsFilePath="C:\TestOut\prices.html"
/SkuRecommendationPreventPriceRefresh=true
```

**Beispiel 2: erhalten von Empfehlungen mit den neuesten Preisen für die angegebene Region (z. b. "ukwest").**

```
.\DmaCmd.exe /Action=SkuRecommendation
/SkuRecommendationInputDataFilePath="C:\TestOut\out.csv"
/SkuRecommendationTsvOutputResultsFilePath="C:\TestOut\prices.tsv"
/SkuRecommendationJsonOutputResultsFilePath="C:\TestOut\prices.json"
/SkuRecommendationOutputResultsFilePath="C:\TestOut\prices.html"
/SkuRecommendationCurrencyCode=USD
/SkuRecommendationOfferName=MS-AZR-0044p
/SkuRecommendationRegionName=UKWest
/SkuRecommendationSubscriptionId=<Your Subscription Id>
/AzureAuthenticationInteractiveAuthentication=true
/AzureAuthenticationClientId=<Your AzureAuthenticationClientId>
/AzureAuthenticationTenantId=<Your AzureAuthenticationTenantId>
```

**Beispiel 3: erhalten von Empfehlungen für bestimmte Datenbanken (z. b. "TPCDS1G, EDW_3G, TPCDS10G").**

```
.\DmaCmd.exe /Action=SkuRecommendation 
/SkuRecommendationInputDataFilePath="C:\TestOut\out.csv" 
/SkuRecommendationTsvOutputResultsFilePath="C:\TestOut\prices.tsv" 
/SkuRecommendationJsonOutputResultsFilePath="C:\TestOut\prices.json" 
/SkuRecommendationOutputResultsFilePath="C:\TestOut\prices.html" 
/SkuRecommendationCurrencyCode=USD 
/SkuRecommendationOfferName=MS-AZR-0044p 
/SkuRecommendationRegionName=UKWest 
/SkuRecommendationSubscriptionId=<Your Subscription Id> 
/SkuRecommendationDatabasesToRecommend=“TPCDS1G” “EDW_3G” “TPCDS10G” 
/AzureAuthenticationInteractiveAuthentication=true 
/AzureAuthenticationClientId=<Your AzureAuthenticationClientId> 
/AzureAuthenticationTenantId=<Your AzureAuthenticationTenantId>
```

Für einzelne Daten Bank Empfehlungen sieht die TSV-Ausgabedatei wie folgt aus:

![Im DMA-Ordner angezeigte PowerShell-einzeldb-Datei](../dma/media/dma-sku-recommend-single-db-recommendations.png)

Bei Empfehlungen für verwaltete Instanzen sieht die TSV-Ausgabedatei wie folgt aus:

![Im DMA-Ordner angezeigte PowerShell-Datei für verwaltete Instanzen](../dma/media/dma-sku-recommend-mi-recommendations.png)

Eine Beschreibung der einzelnen Spalten in der Ausgabedatei folgt.

- **DatabaseName** : der Name Ihrer Datenbank.
- **Metrictype** : Empfohlene Leistungsstufe.
- **Metricvalue** -Empfohlene SKU.
- Preisgestaltung – der geschätzte Preis **pro Monat für** die entsprechende SKU.
- **RegionName** – der Name der Region für die entsprechende SKU. 
- **Istierrecommended** : Wir legen eine minimale SKU-Empfehlung für jede Ebene an. Anschließend wenden wir Heuristik an, um die richtige Ebene für Ihre Datenbank zu ermitteln. Dies gibt an, welche Ebene für die Datenbank empfohlen wird. 
- **Exclusionreasons** : dieser Wert ist leer, wenn eine Ebene empfohlen wird. Für jede Ebene, die nicht empfohlen wird, geben wir die Gründe dafür an, warum Sie nicht ausgewählt wurden.
- **Appliedrules** : eine kurze Schreibweise der angewendeten Regeln.

Der letzte Empfohlene Tarif (d. h. **metrictype**) und der Wert (d. h. **metricvalue**), in dem die Spalte " **istierrecommended** " den Wert "true" aufweist, entsprechen der minimalen SKU, die für die Ausführung Ihrer Abfragen in Azure erforderlich ist, und mit einer Erfolgsrate vergleichbar mit Ihren lokalen Datenbanken. Für Azure SQL-verwaltete Instanz unterstützt DMA derzeit Empfehlungen für die am häufigsten verwendeten SKUs von 8vcore zu 40vcore. Wenn z. b. die empfohlene minimale SKU S4 für den Standard--Wert ist, führt die Auswahl von S3 oder unten dazu, dass für Abfragen ein Timeout auftritt oder die Ausführung fehlschlägt.

Diese Informationen sind in der HTML-Datei in einem grafischen Format enthalten. Es bietet eine benutzerfreundliche Möglichkeit, die abschließende Empfehlung anzuzeigen und den nächsten Teil des Prozesses bereitzustellen. Weitere Informationen zur HTML-Ausgabe finden Sie im folgenden Abschnitt.

## <a name="provision-recommended-skus-to-azure"></a>Bereitstellen von empfohlenen SKUs in Azure

Mit nur wenigen Klicks können Sie die für die Bereitstellung von Ziel-SKUs identifizierten Empfehlungen in Azure verwenden, auf die Sie Ihre Datenbanken migrieren können. Sie können die HTML-Datei verwenden, um ein Azure-Abonnement einzugeben. Wählen Sie Tarif, COMPUTE-Ebene und maximale Datengröße für Ihre Datenbanken aus. und generieren Sie ein Skript, um Ihre Datenbanken bereitzustellen. Sie können dieses Skript mithilfe von PowerShell ausführen.

Sie können diesen Vorgang auf einem einzelnen Computer ausführen, oder Sie können ihn auf mehreren Computern ausführen, um SKU-Empfehlungen in der Skala zu ermitteln. DMA ist zurzeit eine einfache und skalierbare Benutzeroberfläche, da der gesamte Prozess über die Befehlszeilenschnittstelle unterstützt wird.

Um Bereitstellungs Informationen einzugeben und Änderungen an den Empfehlungen vorzunehmen, aktualisieren Sie die HTML-Datei wie folgt.

**Für Einzeldaten Bank Empfehlungen**

![Azure SQL-Datenbank-SKU-Empfehlungs Bildschirm](../dma/media/dma-sku-recommend-single-db-recommendations1.png)

1. Öffnen Sie die HTML-Datei, und geben Sie die folgenden Informationen ein:
    - **Abonnement-ID** : die Abonnement-ID des Azure-Abonnements, für das Sie die Datenbanken bereitstellen möchten.
    - **Ressourcengruppe** : die Ressourcengruppe, in der Sie die Datenbanken bereitstellen möchten. Geben Sie eine Ressourcengruppe ein, die vorhanden ist.
    - **Region** : die Region, in der Datenbanken bereitgestellt werden. Stellen Sie sicher, dass Ihr Abonnement die SELECT-Region unterstützt.
    - **Server Name** : der Azure SQL-Datenbankserver, auf dem die Datenbanken bereitgestellt werden sollen. Wenn Sie einen Servernamen eingeben, der nicht vorhanden ist, wird er erstellt.
    - **Administrator Benutzername** : der Benutzername des Server Administrators. Stellen Sie sicher, dass Ihr Anmelde Name die folgenden Anforderungen erfüllt:
      - Ihr Anmelde Name darf keinen SQL-Bezeichner oder einen typischen Systemnamen (z. b. admin, Administrator, Sa, root, DBManager, loginmanager usw.) oder einen integrierten Datenbankbenutzer oder eine integrierte Rolle (z. b. dbo, Gast, Public usw.) enthalten.
      - Der Anmelde Name darf keine nicht alphanumerischen Zeichen (einschließlich Leerzeichen, Unicode-Zeichen) enthalten.
      - Der Anmelde Name darf nicht mit Zahlen oder Symbolen beginnen.

    - **Administrator Kennwort** : das Server Administrator Kennwort. 
      - Ihr Kennwort muss mindestens 8 Zeichen lang sein und darf nicht mehr als 128 Zeichen lang sein.
      - Ihr Kennwort muss Zeichen aus drei der folgenden Kategorien enthalten: englische Großbuchstaben, englische Kleinbuchstaben, Zahlen (0-9) und nicht alphanumerische Zeichen (!, $, #, % usw.).
      - Ihr Kennwort darf den Anmelde Namen nicht ganz oder teilweise enthalten. (Ein Teil eines Anmelde namens ist als drei oder mehr aufeinander folgende alphanumerische Zeichen definiert.)

2. Überprüfen Sie die Empfehlungen für jede Datenbank, und ändern Sie den Tarif, die computeebene und die maximale Datengröße nach Bedarf. Stellen Sie sicher, dass Sie alle Datenbanken deaktivieren, die Sie zurzeit nicht bereitstellen möchten.

3. Wählen Sie **Bereitstellungs Skript generieren** aus, speichern Sie das Skript, und führen Sie es dann in PowerShell aus.

    Dieser Prozess sollte alle Datenbanken erstellen, die Sie auf der HTML-Seite ausgewählt haben.

**Empfehlungen für Azure SQL-verwaltete Instanz**

![Azure SQL-SKU-Empfehlungen (Bildschirm)](../dma/media/dma-sku-recommend-mi-recommendations1.png)

1. Öffnen Sie die HTML-Datei, und geben Sie die folgenden Informationen ein:
    - **Abonnement-ID** : die Abonnement-ID des Azure-Abonnements, für das Sie die Datenbanken bereitstellen möchten.
    - **Ressourcengruppe** : die Ressourcengruppe, in der Sie die Datenbanken bereitstellen möchten. Geben Sie eine Ressourcengruppe ein, die vorhanden ist.
    - **Region** : die Region, in der Datenbanken bereitgestellt werden. Stellen Sie sicher, dass Ihr Abonnement die SELECT-Region unterstützt.
    - **Instanzname** – die Instanz von Azure SQL verwaltete Instanz, zu der Sie die Datenbanken migrieren möchten. Der Instanzname darf nur Kleinbuchstaben, Ziffern und "-" enthalten. er darf jedoch nicht mit "-" beginnen oder enden oder mehr als 63 Zeichen enthalten.
    - **Instanzadministrator-Benutzername** – der Administrator Benutzername der Instanz. Stellen Sie sicher, dass Ihr Anmelde Name die folgenden Anforderungen erfüllt:
      - Ihr Anmelde Name darf keinen SQL-Bezeichner oder einen typischen Systemnamen (z. b. admin, Administrator, Sa, root, DBManager, loginmanager usw.) oder einen integrierten Datenbankbenutzer oder eine integrierte Rolle (z. b. dbo, Gast, Public usw.) enthalten.
      - Der Anmelde Name darf keine nicht alphanumerischen Zeichen (einschließlich Leerzeichen, Unicode-Zeichen) enthalten.
      - Der Anmelde Name darf nicht mit Zahlen oder Symbolen beginnen.

    - **Instanzadministratorkennwort** : das Instanz-Administrator Kennwort. 
      - Ihr Kennwort muss mindestens 16 Zeichen lang sein und darf nicht mehr als 128 Zeichen lang sein.
      - Ihr Kennwort muss Zeichen aus drei der folgenden Kategorien enthalten: englische Großbuchstaben, englische Kleinbuchstaben, Zahlen (0-9) und nicht alphanumerische Zeichen (!, $, #, % usw.).
      - Ihr Kennwort darf den Anmelde Namen nicht ganz oder teilweise enthalten. (Ein Teil eines Anmelde namens ist als drei oder mehr aufeinander folgende alphanumerische Zeichen definiert.)

    - **Vnet-Name** – der Name des vnets, unter dem die verwaltete Instanz bereitgestellt werden soll. Geben Sie einen vorhandenen vnet-Namen ein.
    - **Subnetzname** – der Name des Subnetzes, unter dem die verwaltete Instanz bereitgestellt werden soll. Geben Sie einen vorhandenen Subnetznamen ein.

2. Überprüfen Sie die Empfehlungen für jede Instanz, und ändern Sie den Tarif, die computeebene und die maximale Datengröße nach Bedarf. Obwohl die Empfehlungen derzeit auf die SKUs 8vcore und 40vcore beschränkt sind, besteht weiterhin die Möglichkeit, bei Bedarf 64vcore-und 80vcore-SKUs bereitzustellen. Stellen Sie sicher, dass Sie alle Instanzen deaktivieren, die Sie zurzeit nicht bereitstellen möchten.

    Dieser Prozess sollte alle Datenbanken erstellen, die Sie auf der HTML-Seite ausgewählt haben.

    > [!NOTE]
    > Das Erstellen verwalteter Instanzen in einem Subnetz (insbesondere zum ersten Mal) kann mehrere Stunden in Anspruch nehmen. Nachdem Sie das Bereitstellungs Skript über PowerShell ausgeführt haben, können Sie den Status Ihrer Bereitstellung im Azure-Portal überprüfen.

## <a name="next-step"></a>Nächster Schritt

- Eine komplette Liste der Befehle zum Ausführen von DMA über die CLI finden Sie im Artikel [Ausführen von Datenmigrations-Assistent von der Befehlszeile aus](./dma-commandline.md).
