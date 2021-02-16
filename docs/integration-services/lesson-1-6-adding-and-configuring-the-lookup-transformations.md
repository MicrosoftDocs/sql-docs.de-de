---
description: 'Lektion 1.6: Hinzufügen und Konfigurieren von Suchtransformationen'
title: 'Schritt 6: Hinzufügen und Konfigurieren von Suchtransformationen | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 03/19/2019
ms.prod: sql
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 5c59f723-9707-4407-80ae-f05f483cf65f
author: chugugrace
ms.author: chugu
ms.reviewer: ''
ms.openlocfilehash: 7c88bd5e8abafdb15eba50fa7cdea22f29910bc4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354321"
---
# <a name="lesson-1-6-add-and-configure-the-lookup-transformations"></a>Lektion 1.6: Hinzufügen und Konfigurieren von Suchtransformationen

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Nach dem Konfigurieren der Flatfilequelle zum Extrahieren von Daten aus der Quelldatei definieren Sie die Suchtransformationen, die zum Abrufen der Werte für **CurrencyKey** und **DateKey** erforderlich sind. Von einer Transformation zum Suchen wird eine Suche durchgeführt, indem Daten in der angegebenen Eingabespalte mit einer Spalte in einem referenzierten Dataset verknüpft werden. Bei dem Verweisdataset kann es sich um eine vorhandene Tabelle oder Sicht, eine neue Tabelle oder das Ergebnis einer SQL-Anweisung handeln. In diesem Tutorial stellt die Suchtransformation mithilfe eines OLE DB-Verbindungs-Managers eine Verbindung mit der Datenbank her, die die Quelldaten des Verweis-DataSets enthält.  
  
> [!NOTE]  
> Sie können die Transformation für Suche auch so konfigurieren, dass sie eine Verbindung mit einem Cache herstellt, der das Verweisdataset enthält. Weitere Informationen finden Sie unter [Lookup transformation (Suchtransformation)](../integration-services/data-flow/transformations/lookup-transformation.md).  
  
In dieser Aufgabe fügen Sie dem Paket die folgenden beiden Komponenten für die Suchtransformation hinzu und konfigurieren sie:  
  
-   Eine Transformation wird zum Suchen nach Werten in der **CurrencyKey**-Spalte der **DimCurrency**-Dimensionstabelle verwendet, wobei **CurrencyID**-Spaltenwerte aus der Flatfile abgeglichen werden.  
  
-   Die andere Transformation wird zum Suchen nach Werten in der **DateKey**-Spalte der **DimDate**-Dimensionstabelle verwendet, wobei **CurrencyDate**-Spaltenwerte aus der Flatfile abgeglichen werden.  
  
In beiden Fällen wird von der Suchtransformation der OLE DB-Verbindungs-Manager genutzt, den Sie zuvor erstellt haben.  
  
## <a name="add-and-configure-the-lookup-currency-key-transformation"></a>Hinzufügen und Konfigurieren der Lookup Currency Key-Transformation  
  
1.  Erweitern Sie in der **SSIS-Toolbox** die Option **Allgemein**, und ziehen Sie anschließend **Suche** auf die Entwurfsoberfläche der Registerkarte **Datenfluss** . Legen Sie **Suche** direkt unterhalb der Quelle **Extract Sample Currency Data** ab.  
  
2.  Klicken Sie auf die Flatfilequelle **Extract Sample Currency Data**, und ziehen Sie den blauen Pfeil auf die neu hinzugefügte Transformation **Suche**, um die zwei Komponenten zu verbinden.  
  
3.  Klicken Sie auf der **Datenfluss**-Entwurfsoberfläche auf **Suche** in der Transformation **Suche**, und ändern Sie den Namen in **Lookup Currency Key**.  
  
4.  Doppelklicken Sie auf die **Lookup Currency Key**-Transformation, um den **Transformations-Editor für Suche** anzuzeigen.  
  
5.  Wählen Sie auf der Seite **Allgemein** die folgenden Optionen aus:  
  
    1.  Wählen Sie **Vollcache** aus.  
  
    2.  Wählen Sie im Bereich **Verbindungstyp****OLE DB-Verbindungs-Manager** aus.  
  
6.  Wählen Sie auf der Seite **Verbindung** die folgenden Optionen aus:  
  
    1.  Stellen Sie im **OLE DB-Verbindungs-Manager** sicher, dass **localhost.AdventureWorksDW2012** angezeigt wird.  
  
    2.  Klicken Sie auf **Ergebnisse einer SQL-Abfrage verwenden**, und geben Sie anschließend die folgende SQL-Anweisung ein, oder kopieren Sie diese:  
  
        ```sql
        SELECT * FROM [Sales].[Currency]
        WHERE [CurrencyCode]
        IN ('ARS', 'AUD', 'BRL', 'CAD', 'CNY',
            'DEM', 'EUR', 'FRF', 'GBP', 'JPY',
            'MXN', 'SAR', 'USD', 'VEB')
        ```  
    3.  Klicken Sie auf **Vorschau**, um die Ergebnisse der Abfrage zu überprüfen.
  
7.  Wählen Sie auf der Seite **Spalten** die folgenden Optionen aus:  
  
    1.  Ziehen Sie aus dem Bereich **Verfügbare Eingabespalten** den Spaltennamen **CurrencyID** in den Bereich **Verfügbare Suchspalten** auf **CurrencyAlternateKey**.  
  
    2.  Aktivieren Sie in der Liste **Verfügbare Suchspalten** das Kontrollkästchen links neben **CurrencyKey**.  
  
8.  Klicken Sie auf **OK**, um zur **Datenfluss**-Entwurfsoberfläche zurückzukehren.  
  
9. Klicken Sie mit der rechten Maustaste auf die Lookup Currency Key-Transformation und anschließend auf **Eigenschaften**.  
  
10. Überprüfen Sie im Fenster **Eigenschaften**, ob die **LocaleID**-Eigenschaft auf **Englisch (USA)** und die **DefaultCodePage**-Eigenschaft auf **1252** festgelegt ist.  
  
## <a name="add-and-configure-the-lookup-date-key-transformation"></a>Hinzufügen und Konfigurieren der Lookup Date Key-Transformation  
  
1.  Ziehen Sie in der **SSIS-Toolbox** die Option **Suche** auf die **Datenfluss** -Entwurfsoberfläche. Legen Sie **Suche** direkt unterhalb der **Lookup Currency Key**-Transformation ab.  
  
2.  Klicken Sie auf die **Lookup Currency Key**-Transformation, und ziehen Sie den blauen Pfeil auf die neue Transformation **Suche**, um die zwei Komponenten zu verbinden.  
  
3.  Klicken Sie im Dialogfeld **Eingabe-/Ausgabe-Auswahl** im Listenfeld **Ausgabe** auf **Ausgabe der Suchübereinstimmungen**, und klicken Sie anschließend auf **OK**.  
  
4.  Klicken Sie auf der **Datenfluss**-Entwurfsoberfläche in der neu hinzugefügten Transformation **Suche** auf **Suche**, und ändern Sie den Namen in **Lookup Date Key**.  
  
5.  Doppelklicken Sie auf die Transformation **Lookup Date Key** .  
  
6.  Wählen Sie auf der Seite **Allgemein** die Option **Teilcache** aus.  
  
7.  Wählen Sie auf der Seite **Verbindung** die folgenden Optionen aus:  
  
    1.  Stellen Sie im Dialogfeld **OLE DB-Verbindungs-Manager** sicher, dass **localhost.AdventureWorksDW2012** angezeigt wird.  
  
    2.  Geben Sie im Feld **Use a table or view** (Tabelle oder Sicht verwenden) den Eintrag **[dbo].[DimDate]** ein, oder wählen Sie diesen aus.  
  
8.  Wählen Sie auf der Seite **Spalten** die folgenden Optionen aus:  
  
    1.  Ziehen Sie aus dem Bereich **Verfügbare Eingabespalten** den Spaltennamen **CurrencyDate** in den Bereich **Verfügbare Suchspalten** auf **FullDateAlternateKey**.  Wenn Sie eine Meldung erhalten, die auf eine fehlende Übereinstimmung zwischen den Datentypen hinweist, ändern Sie den Datentyp von „CurrencyDate“ in [DT_DBDATE].
  
    2.  Aktivieren Sie in der Liste **Verfügbare Suchspalten** das Kontrollkästchen links neben **DateKey**.  
  
9. Überprüfen Sie auf der Seite **Erweitert** die Optionen für die Zwischenspeicherung.  
  
10. Klicken Sie auf **OK**, um zur **Datenfluss**-Entwurfsoberfläche zurückzukehren.  
  
11. Klicken Sie mit der rechten Maustaste auf die **Lookup Date Key**-Transformation und anschließend auf **Eigenschaften**.
  
12. Überprüfen Sie im Fenster **Eigenschaften**, ob die **LocaleID**-Eigenschaft auf **Englisch (USA)** und die **DefaultCodePage**-Eigenschaft auf **1252** festgelegt ist.  
  
## <a name="go-to-next-task"></a>Weiter zur nächsten Aufgabe
[Schritt 7: Hinzufügen und Konfigurieren des OLE DB-Ziels](../integration-services/lesson-1-7-adding-and-configuring-the-ole-db-destination.md)  
  
## <a name="see-also"></a>Weitere Informationen  
[Lookup transformation (Suchtransformation)](../integration-services/data-flow/transformations/lookup-transformation.md)  
