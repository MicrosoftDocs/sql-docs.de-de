---
title: Verwenden von Testbedingungen in SQL Server-Komponententests
description: Hier erfahren Sie mehr über Testbedingungen in SQL Server-Komponententests. Sie erhalten Informationen dazu, wie Sie vordefinierte Bedingungen und negative Tests verwenden und Informationen zu benutzerdefinierten Bedingungen anzeigen.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.unittesting.testconditions
ms.assetid: e3d1c86c-1e58-4d2c-b625-d1b591b221aa
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 18021d85c9b490664411b7bb98f1b55972a13022
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082411"
---
# <a name="using-test-conditions-in-sql-server-unit-tests"></a>Verwenden von Testbedingungen in SQL Server-Komponententests

In einem SQL Server-Komponententest wird mindestens ein Transact\-SQL-Testskript ausgeführt. Die Ergebnisse können innerhalb des Transact\-SQL-Skripts ausgewertet werden, wobei durch THROW oder RAISERROR zurückgegeben wird, dass ein Fehler aufgetreten bzw. der Test nicht erfolgreich war. Alternativ können Testbedingungen im Test definiert werden, um die Ergebnisse auszuwerten. Der Test gibt eine Instanz der Klasse [SqlExecutionResult](/previous-versions/sql/sql-server-data-tools/jj856590(v=vs.103)) zurück. Die Instanz dieser Klasse enthält mindestens ein DataSet, die Ausführungszeit sowie die vom Skript betroffenen Zeilen. Alle diese Informationen werden während der Skriptausführung gesammelt. Diese Ergebnisse können mithilfe von Testbedingungen ausgewertet werden. SQL Server Data Tools stellt eine Reihe von vordefinierten Testbedingungen bereit. Sie können auch benutzerdefinierte Bedingungen erstellen und verwenden. Lesen Sie hierzu [Benutzerdefinierte Testbedingungen für SQL Server-Komponententests](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md).  
  
## <a name="predefined-test-conditions"></a>Vordefinierte Testbedingungen  
In der folgenden Tabelle sind die vordefinierten Testbedingungen aufgeführt, die Sie im Bereich „Testbedingungen“ des SQL Server-Komponententest-Designers hinzufügen können.  
  
|**Testbedingung**|**Beschreibung der Testbedingung**|  
|----------------------|----------------------------------|  
|Datenprüfsumme|Verursacht einen Fehler, wenn die Prüfsumme des vom Transact\-SQL-Skript zurückgegebenen Resultsets nicht mit der erwarteten Prüfsumme übereinstimmt. Weitere Informationen finden Sie unter [Angeben einer Datenprüfsumme](#SpecifyDataChecksum).<br /><br />**HINWEIS:** Wenn Daten zurückgegeben werden, die zwischen den Testläufen variieren, wird diese Testbedingung nicht empfohlen. Beispiel: Wenn das Resultset generierte Datumsangaben oder Uhrzeiten bzw. Identitätsspalten enthält, schlagen die Tests fehl, weil die Prüfsummen der jeweiligen Ausführungen unterschiedlich sind.|  
|ResultSet ist leer|Verursacht einen Fehler, wenn das vom Transact\-SQL-Skript zurückgegebene Resultset nicht leer ist.|  
|Ausführungszeit|Verursacht einen Fehler, wenn die Ausführung des Transact\-SQL-Testskripts länger als erwartet dauert. Standardmäßig beträgt die Ausführungszeit 30 Sekunden.<br /><br />Die Ausführungszeit gilt nur für Tests mit dem Testskript und nicht mit dem Vortestskript oder Nachtestskript.|  
|Erwartetes Schema|Verursacht einen Fehler, wenn die Spalten und Datentypen des Resultsets nicht mit den für die Testbedingung angegebenen Spalten und Datentypen übereinstimmen. Sie müssen ein Schema in den Eigenschaften der Testbedingung angeben. Weitere Informationen finden Sie unter [Angeben eines erwarteten Schemas](#SpecifyExpectedSchema).|  
|Nicht eindeutig|Erzeugt immer einen Test mit dem Ergebnis „Nicht eindeutig“. Dies ist die Standardbedingung, die jedem Test hinzugefügt wird. Diese Testbedingung wird eingeschlossen, um anzuzeigen, dass die Testüberprüfung nicht implementiert wurde. Löschen Sie diese Testbedingung aus dem Test, nachdem Sie weitere Testbedingungen hinzugefügt haben.|  
|ResultSet ist nicht leer|Verursacht einen Fehler, wenn das Resultset leer ist. Sie können diese Testbedingung oder EmptyResultSet mit der Transact\-SQL-Funktion „@@RAISERROR“ im Testskript verwenden, um zu überprüfen, ob ein Update ordnungsgemäß ausgeführt wurde. Beispielsweise können Sie Werte vor dem Update speichern, das Update ausführen, die Werte mit den Ergebnissen nach dem Update vergleichen und einen Fehler auslösen, wenn Sie nicht die erwarteten Ergebnisse erhalten.|  
|Zeilenanzahl|Verursacht einen Fehler, wenn das Resultset nicht die erwartete Anzahl von Zeilen enthält.|  
|Skalarwert|Verursacht einen Fehler, wenn ein bestimmter Wert im Resultset nicht mit dem angegebenen Wert übereinstimmt. **Erwarteter Wert** ist standardmäßig NULL.|  
  
> [!NOTE]  
> Mit der Testbedingung „Ausführungszeit“ wird ein Zeitlimit angegeben, das bei der Ausführung des Transact\-SQL-Testskripts nicht überschritten werden darf. Wird das Zeitlimit überschritten, schlägt der Test fehl. Die Testergebnisse umfassen auch eine Statistik zur Dauer, die sich von der Testbedingung „Ausführungszeit“ unterscheidet. Die Statistik zur Zeitdauer umfasst neben der Ausführungszeit auch die Zeit für die zweimalige Verbindung mit der Datenbank, die Zeit zur Ausführung anderer Testskripts, z. B. des Vor- und Nachtestskripts, sowie die Zeit zur Ausführung der Testbedingungen. Daher kann ein Test selbst dann erfolgreich verlaufen, wenn die Dauer über der Ausführungszeit liegt.  
>   
> Die für die Datengenerierung und Schemabereitstellung benötigte Zeit ist nicht in der angegebenen Zeitdauer enthalten, da sie vor der Testausführung liegt. Um die Testdauer anzuzeigen, wählen Sie im Fenster **Testergebnisse** einen Testlauf aus, klicken mit der rechten Maustaste darauf und wählen **Details der Testergebnisse anzeigen** aus.  
  
Sie können den Bereich „Testbedingungen“ des SQL Server-Komponententest-Designers verwenden, um SQL Server-Komponentetests Testbedingungen hinzuzufügen. Weitere Informationen finden Sie unter [Vorgehensweise: Hinzufügen von Testbedingungen zu SQL Server-Komponententests](../ssdt/how-to-add-test-conditions-to-sql-server-unit-tests.md).  
  
Sie können den Code der Testmethode auch direkt bearbeiten, um weitere Funktionen hinzuzufügen. Weitere Informationen finden Sie unter [Gewusst wie: Öffnen eines SQL Server-Komponententests zur Bearbeitung](../ssdt/how-to-open-a-sql-server-unit-test-to-edit.md) und [Gewusst wie: Schreiben eines SQL Server-Komponententests, der im Gültigkeitsbereich einer einzelnen Transaktion ausgeführt wird](../ssdt/how-to-write-sql-server-unit-test-that-runs-in-single-transaction-scope.md). Beispielsweise können Sie eine Testmethode durch Funktionen erweitern, indem Sie Assert-Anweisungen hinzufügen. Weitere Informationen finden Sie unter [Verwenden von Transact-SQL-Assertionen in SQL Server-Komponententests](../ssdt/using-transact-sql-assertions-in-sql-server-unit-tests.md).  
  
## <a name="expected-failures"></a>Erwartete Fehler  
Sie können SQL Server-Komponententests erstellen, um ein Verhalten zu überprüfen, von dem erwartet wird, dass es nicht erfolgreich ist. Diese erwarteten Fehler werden auch als Negativnachweis bezeichnet. Im Folgenden einige Beispiele:  
  
-   Sicherstellen, dass eine gespeicherte Prozedur zum Löschen von Kundendaten bei Angabe einer ungültigen Kunden-ID fehlschlägt.  
  
-   Sicherstellen, dass eine gespeicherte Prozedur zum Ausführen einer Bestellung fehlschlägt, wenn die Bestellung nie aufgegeben bzw. bereits ausgeführt wurde.  
  
-   Sicherstellen, dass eine gespeicherte Prozedur zum Stornieren von Bestellungen keine abgeschlossenen oder bereits stornierten Bestellungen stornieren kann.  
  
Sie können SQL Server-Komponententests für gespeicherte Prozeduren definieren, die erwartete SQL-Ausnahmen auslösen. Um anzugeben, welche Ausnahmen erwartet werden, können Sie der Komponententestmethode ein Attribut hinzufügen. So verhindern Sie, dass der Test beim Auftreten der Ausnahme fehlschlägt.  
  
Um eine SQL Server-Komponententestmethode mit erwarteten Ausnahmen zu kennzeichnen, fügen Sie das folgende Attribut hinzu:  
  
```  
[ExpectedSqlException(MessageNumber = nnnnn, Severity = x, MatchFirstError = false, State = y)]  
```  
  
Hierbei gilt:  
  
-   *nnnnn* ist die Nummer der erwarteten Meldung, z. B. 14025.  
  
-   *x* ist der Schweregrad der erwarteten Ausnahme.  
  
-   *y* ist der Zustand der erwarteten Ausnahme.  
  
Nicht festgelegte Parameter werden ignoriert. Sie übergeben diese Parameter an die **THROW** -Anweisung im Datenbankcode. Wenn Sie „MatchFirstError = false“ angeben, werden alle „SqlErrors“ in der Ausnahme vom Attribut verglichen. Das Standardverhalten („MatchFirstError = true“) besteht darin, dass nur der erste aufgetretene Fehler vom Attribut verglichen wird.  
  
Ein Beispiel dafür, wie erwartete Ausnahmen und ein negativer SQL Server-Komponententest verwendet werden, finden Sie unter [Exemplarische Vorgehensweise: Erstellen und Ausführen eines SQL Server-Komponententests](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md).  
  
## <a name="specifying-a-data-checksum"></a><a name="SpecifyDataChecksum"></a>Angeben einer Datenprüfsumme  
Um den SQL Server-Komponententest-Designer anzuzeigen, doppelklicken Sie im **Projektmappen-Explorer** auf die Quellcodedatei eines Komponententests.  
  
Nachdem Sie dem Datenbankkomponententest die Testbedingung „Datenprüfsumme“ hinzugefügt haben, müssen Sie die erwartete Prüfsumme mithilfe des folgenden Verfahrens konfigurieren:  
  
#### <a name="to-specify-an-expected-checksum"></a>So geben Sie eine erwartete Prüfsumme an  
  
1.  Klicken Sie in der Liste mit Testbedingungen auf die Testbedingung „Datenprüfsumme“, für die Sie eine Prüfsumme angeben möchten.  
  
2.  Öffnen Sie das **Eigenschaftenfenster** durch Drücken von F4. Sie können auch im Menü **Ansicht** auf **Eigenschaftenfenster** klicken.  
  
3.  (Optional) Sie können die Eigenschaft **(Name)** der Testbedingung auch in einen aussagekräftigeren Namen ändern.  
  
4.  Klicken Sie in der Eigenschaft **Configuration** (Konfiguration) auf die Schaltfläche zum Durchsuchen ( **…** ).  
  
    Das Dialogfeld **Konfiguration für TestConditionName** wird angezeigt.  
  
5.  Geben Sie eine Verbindung mit der Datenbank an, die getestet werden soll. Weitere Informationen finden Sie unter [Vorgehensweise: Create a Database Connection (Vorgehensweise: Erstellen einer Datenbankverbindung)](/previous-versions/visualstudio/visual-studio-2010/aa833420(v=vs.100)).  
  
6.  Standardmäßig wird der Transact\-SQL-Text des Tests im Bearbeitungsbereich angezeigt. Sie können ggf. den Code ändern, um die erwarteten Ergebnisse zu erzeugen. Wenn im Vortest des Tests z. B. Code enthalten ist, müssen Sie diesen Code u. U. hinzufügen.  
  
    > [!IMPORTANT]  
    > Wenn Sie eine Prüfsummenbedingung ändern, für die Sie zuvor eine Prüfsumme angegeben hatten, werden die im Bearbeitungsbereich vorgenommenen Änderungen nicht gespeichert. Sie müssen diese Änderungen erneut vornehmen, bevor Sie auf **Abrufen** klicken.  
  
7.  Klicken Sie auf **Abrufen**.  
  
    Der Transact\-SQL-Code wird für die angegebene Datenbankverbindung ausgeführt, und die Ergebnisse werden im Dialogfeld angezeigt.  
  
8.  Wenn die Ergebnisse mit den erwarteten Testergebnissen übereinstimmen, klicken Sie auf **OK**. Ändern Sie andernfalls den Transact\-SQL-Text, und wiederholen Sie die Schritte 6, 7 und 8, bis die Ergebnisse den Erwartungen entsprechen.  
  
    In der Spalte **Wert** der Testbedingung wird der Wert der erwarteten Prüfsumme angezeigt.  
  
## <a name="specifying-an-expected-schema"></a><a name="SpecifyExpectedSchema"></a>Angeben eines erwarteten Schemas  
Nachdem Sie einem SQL Server-Komponententest die Testbedingung „Erwartetes Schema“ hinzugefügt haben, müssen Sie das erwartete Schema mithilfe des folgenden Verfahrens konfigurieren:  
  
#### <a name="to-specify-an-expected-schema"></a>So geben Sie ein erwartetes Schema an  
  
1.  Klicken Sie in der Liste mit Testbedingungen auf die Testbedingung „Erwartetes Schema“, für die Sie ein Schema angeben möchten.  
  
2.  Öffnen Sie das **Eigenschaftenfenster** durch Drücken von F4. Sie können auch im Menü **Ansicht** auf **Eigenschaftenfenster** klicken.  
  
3.  (Optional) Sie können die Eigenschaft **(Name)** der Testbedingung auch in einen aussagekräftigeren Namen ändern.  
  
4.  Klicken Sie in der Eigenschaft **Configuration** (Konfiguration) auf die Schaltfläche zum Durchsuchen ( **…** ).  
  
    Das Dialogfeld **Konfiguration für TestConditionName** wird angezeigt.  
  
5.  Geben Sie eine Verbindung mit der Datenbank an, die getestet werden soll. Weitere Informationen finden Sie unter [Vorgehensweise: Create a Database Connection (Vorgehensweise: Erstellen einer Datenbankverbindung)](/previous-versions/visualstudio/visual-studio-2010/aa833420(v=vs.100)).  
  
6.  Standardmäßig wird der Transact\-SQL-Text des Tests im Bearbeitungsbereich angezeigt. Sie können ggf. den Code ändern, um die erwarteten Ergebnisse zu erzeugen. Wenn im Vortest des Tests z. B. Code enthalten ist, müssen Sie diesen Code u. U. hinzufügen.  
  
    > [!IMPORTANT]  
    > Wenn Sie eine Bedingung für ein erwartetes Schema ändern, für die Sie zuvor ein Schema angegeben hatten, werden die im Bearbeitungsbereich vorgenommenen Änderungen nicht gespeichert. Sie müssen diese Änderungen erneut vornehmen, bevor Sie auf **Abrufen** klicken.  
  
7.  Klicken Sie auf **Abrufen**.  
  
    Der Transact\-SQL-Code wird für die angegebene Datenbankverbindung ausgeführt, und die Ergebnisse werden im Dialogfeld angezeigt. Da Sie das Schema bzw. die Form des Resultsets und nicht die Werte der Ergebnisse überprüfen, müssen keine Daten in den zurückgegebenen Ergebnissen angezeigt werden, solange die Spalten in der erwarteten Weise dargestellt werden.  
  
8.  Wenn die Ergebnisse mit den erwarteten Testergebnissen übereinstimmen, klicken Sie auf **OK**. Ändern Sie andernfalls den Transact\-SQL-Text, und wiederholen Sie die Schritte 6, 7 und 8, bis die Ergebnisse den Erwartungen entsprechen.  
  
    In der Spalte **Wert** der Testbedingung werden Informationen zum erwarteten Schema angezeigt. Beispielsweise könnte „Erwartet: 2 Tabellen“ angezeigt werden.  
  
## <a name="extensible-test-conditions"></a>Erweiterbare Testbedingungen  
Neben den sechs vordefinierten Testbedingungen können Sie eigene neue Testbedingungen erstellen. Diese Testbedingungen werden im Bereich „Testbedingungen“ des SQL Server-Komponententest-Designers angezeigt. Weitere Informationen finden Sie unter [Benutzerdefinierte Testbedingungen für SQL Server-Komponententests](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md).  
  
## <a name="see-also"></a>Weitere Informationen  
[Erstellen und Definieren von SQL Server-Komponententests](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Verwenden von Transact-SQL-Assertionen in SQL Server-Komponententests](../ssdt/using-transact-sql-assertions-in-sql-server-unit-tests.md)  
[Skripts in SQL Server-Komponententests](../ssdt/scripts-in-sql-server-unit-tests.md)  
