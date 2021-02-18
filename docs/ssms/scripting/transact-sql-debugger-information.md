---
title: Transact-SQL-Debuggerinformationen
description: Hier erfahren Sie, wie Sie Transact-SQL-Debuggerausgaben abrufen, die Informationen wie Aufruflisten, Threads, Breakpoints, Code, Variablen und Befehle enthalten.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, Locals Window
- Transact-SQL debugger, Watch Window
- Transact-SQL debugger, Threads Window
- Transact-SQL debugger, Call Stack Window
- Transact-SQL debugger, QuickWatch
- Transact-SQL debugger, viewing information
ms.assetid: b99819cc-f388-41a1-b304-36e78ce24147
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 412ae1788542d4fa00df6a17df1b6450f7605851
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341000"
---
# <a name="transact-sql-debugger---information"></a>Transact-SQL-Debuggerinformationen

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Jedes Mal, wenn der Debugger bei einer bestimmten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung die Ausführung unterbricht, können Sie den aktuellen Ausführungsstatus in den verschiedenen Debuggerfenstern anzeigen. 

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="debugger-windows"></a>Debuggerfenster  

Im Debuggermodus öffnet der Debugger zwei Fenster am unteren Rand des [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] -Fensters. Der Debugger zeigt alle seine Informationen in diesen zwei Fenstern an. Jedes Debuggerfenster enthält Registerkarten, durch deren Auswahl Sie bestimmen können, welcher Satz von Informationen im Fenster angezeigt wird. Das linke Debuggerfenster enthält die Registerkarten **Lokal**, **Überwachen 1**, **Überwachen 2**, **Überwachen 3** und **Überwachen 4** . Das rechte Debuggerfenster enthält die Registerkarten **Aufrufliste**, **Threads**, **Breakpoints**, **Befehlsfenster** und **Ausgabe** .  
  
> [!NOTE]  
>  Die vorherigen Beschreibungen gelten für die Standardpositionen der Debuggerfenster. Sie können eine Registerkarte ziehen, um sie von einem Fenster in ein anderes zu verschieben, oder Sie können die Verankerung einer Registerkarte aufheben, um ein neues Fenster zu erstellen, das Sie beliebig platzieren können.  
  
 Standardmäßig sind nicht alle dieser Registerkarten oder Fenster aktiv. Sie haben die folgenden Möglichkeiten, um ein bestimmtes Fenster zu öffnen:  
  
-   Klicken Sie im Menü **Debuggen** auf **Fenster**, und wählen Sie dann das gewünschte Fenster aus.  
  
-   Klicken Sie auf der Symbolleiste **Debuggen** auf **Breakpoints**, und wählen Sie dann das gewünschte Fenster aus.  
  
## <a name="transact-sql-expressions"></a>Transact-SQL-Ausdrücke  
 Ausdrücke sind [!INCLUDE[tsql](../../includes/tsql-md.md)] -Klauseln, die einen einzelnen Skalarwert ergeben, z. B. Variablen oder Parameter. Im linken Debuggerfenster können die Datenwerte anzeigt werden, die derzeit Ausdrücken auf bis zu fünf dieser Registerkarten oder Fenster zugeordnet sind: **Lokal, Überwachen 1**, **Überwachen 2**, **Überwachen 3** und **Überwachen 4**.  
  
 Das Fenster **Lokal** zeigt Informationen über die lokalen Variablen im aktuellen Bereich des [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debuggers an. Der Satz von Ausdrücken, die im Fenster **Lokal** aufgeführt sind, ändert sich, wenn der Debugger die verschiedenen Teile des Codes durchläuft.  
  
 Die Ausdrücke in der **Schnellüberwachung** und den vier **Überwachungsfenster** sind nicht darauf beschränkt, die Bezeichner einer Variablen aufzulisten. Sie können einen [!INCLUDE[tsql](../../includes/tsql-md.md)] -Ausdruck angeben, dessen Auswertung einen einzelnen Wert ergibt. Beispiele:  
  
-   Der Name einer Variablen, z.B. @IntegerCounter.  
  
-   Eine arithmetische Operation an einer Variablen, z.B. @IntegerCounter + 1.  
  
-   Eine Zeichenfolgenoperation an zwei Zeichenvariablen, zum Beispiel @FirstName + @LastName.  
  
-   Eine SELECT-Anweisung, die einen einzelnen Wert zurückgibt, zum Beispiel SELECT CharCol FROM MyTable WHERE PrimaryKey = 1.  
  
 Mithilfe des Fensters **Schnellüberwachung** können Sie den Wert eines [!INCLUDE[tsql](../../includes/tsql-md.md)] -Ausdrucks anzeigen und anschließend diesen Ausdruck in einem **Überwachungsfenster** speichern. Zum Auswählen eines Ausdrucks in **Schnellüberwachung** wählen Sie den Namen des Ausdrucks entweder im Feld **Ausdruck** aus oder geben ihn dort ein.  
  
 Die vier **Überwachungsfenster** zeigen Informationen über Variablen und Ausdrücke an, die Sie ausgewählt haben. Der Satz von Ausdrücken, die in den **Überwachungsfenstern** aufgeführt sind, ändert sich erst dann, wenn Sie Ausdrücke entweder der Liste hinzufügen oder aus dieser entfernen.  
  
 Zum Hinzufügen eines Ausdrucks zu einem **Überwachungsfenster** können Sie entweder im Dialogfeld **Schnellüberwachung** die Option **Überwachung hinzufügen** auswählen oder den Namen des Ausdrucks in die **Namensspalte** einer leeren Zeile in einem **Überwachungsfenster** eingeben.  
  
 Sie können die Datenwerte für Variablen in den Fenstern **Lokal**, **Überwachung** oder **Schnellüberwachung** festlegen, indem Sie mit der rechten Maustaste auf die Zeile klicken und dann **Wert bearbeiten** auswählen. Die **Wert** -Spalten im Fenster **Lokal** , im Fenster **Überwachung** und im Dialogfeld **Schnellüberwachung** unterstützen alle Text-, XML- und HTML-Datenschnellansichten. Die Schnellansichten werden durch einen Vergrößerungsglas-Datentipp ganz rechts neben der Spalte **Werte** dargestellt. Mithilfe der Schnellansichten können Sie Text-, XML- oder HTML-Datenwerte in Anzeigen für die entsprechenden Datentypen, z. B. XML-Dateien in einem Browserfenster, betrachten.  
  
 Wenn Sie im Debugmodus mit der Maus auf einen Bezeichner zeigen, werden in einem **QuickInfo** -Popupfenster der Name des Ausdrucks und sein aktueller Wert angezeigt. Weitere Informationen finden Sie unter [QuickInfo &#40;IntelliSense&#41;](./quick-info-intellisense.md).  
  
## <a name="breakpoints"></a>Breakpoints  
 Im Fenster **Breakpoints** können Sie die aktuell festgelegten Breakpoints anzeigen und verwalten. Weitere Informationen finden Sie unter [Schrittweises Durchlaufen von Transact-SQL-Code](./step-through-transact-sql-code.md).  
  
## <a name="call-stacks"></a>Aufruflisten  
 Das Fenster **Aufrufliste** zeigt den aktuellen Ausführungsort sowie Informationen über die Art und Weise an, in der die Ausführung vom ursprünglichen Editor-Fenster über [!INCLUDE[tsql](../../includes/tsql-md.md)] -Module (Funktionen, gespeicherte Prozeduren oder Trigger) übergeben wurde, um den aktuellen Ausführungsort zu erreichen. Jede Zeile im Fenster **Aufrufliste** wird als Stapelrahmen bezeichnet und stellt eines der folgenden Elemente dar:  
  
-   Den aktuellen Ausführungsort  
  
-   Einen Aufruf von einem Modul zu einem anderen  
  
-   Einen Aufruf von einem Editorfenster zu einem [!INCLUDE[tsql](../../includes/tsql-md.md)] -Modul  
  
 Der Stapel ist in umgekehrter Reihenfolge, in der die Module aufgerufen wurden, angeordnet. Der aktuelle Ausführungsort befindet sich oben und der ursprüngliche Aufruf unten im Stapel. Ein gelber Pfeil am linken Rand des Stapelrahmens bezeichnet den Rahmen, in dem der Debugger die Ausführung unterbrochen hat.  
  
 Die Spalte **Name** zeichnet die folgenden Informationen auf:  
  
-   Das Quellmodul, das die Codezeile enthält, die hinunter zur nächsten Ebene aufgerufen hat  
  
-   Die Codezeile, die das nächste Modul auf dem Stapel aufgerufen hat  
  
-   Wenn der Aufruf an eine gespeicherte Prozedur oder eine Funktion mit Parametern gegangen ist, werden auch die Namen, Datentypen und Werte aller Parameter aufgelistet.  
  
 Die Ausdrücke in den Fenstern **Lokal**, **Überwachung** und **Schnellüberwachung** werden für den aktuellen Stapelrahmen ausgewertet. Standardmäßig ist der aktuelle Stapelrahmen der oberste Rahmen im Stapel, bei dem der Debugger die Ausführung unterbrochen hat. Wenn Sie einen anderen Stapelrahmen als aktuellen Stapelrahmen angeben, werden die Ausdrücke in den Fenstern **Lokal**, **Überwachung** und **Schnellüberwachung** für den neuen Stapelrahmen neu ausgewertet. Sie können den aktuellen Stapelrahmen wechseln, indem Sie entweder auf einen Rahmen doppelklicken oder auf einen Rahmen klicken und **Zu Rahmen wechseln** auswählen. Daraufhin werden die Ausdrücke in den Fenstern **Lokal**, **Überwachung** und **Schnellüberwachung** für den neuen Rahmen neu ausgewertet. Wenn der aktuelle Stapelrahmen nicht der oberste Rahmen im Stapel ist, kennzeichnet ein grüner Pfeil am linken Rand des Stapels den aktuellen Stapelrahmen.  
  
 Wenn Sie mit der rechten Maustaste auf einen Stapelrahmen klicken und **Gehe zu Quellcode** auswählen, wird der Code für diesen Rahmen in einem Abfrage-Editor-Fenster angezeigt. Dieser Rahmen wird jedoch nicht zum aktuellen Rahmen gemacht, und die Inhalte der Fenster **Lokal**, **Überwachung** und **Schnellüberwachung** werden nicht geändert.  
  
## <a name="system-information-and-transact-sql-results"></a>Systeminformationen und Transact-SQL-Ergebnisse  
 Der Debugger listet seine Status- und Ereignismeldungen im Fenster **Ausgabe** auf. Hierzu gehören Informationen wie z. B. zum Zeitpunkt, zu dem andere Prozessen debuggt werden oder zu dem Debuggerthreads enden.  
  
 Im Debugmodus sind die Registerkarten **Ergebnisse** und **Meldungen** nach wie vor im Abfrage-Editor aktiv. Auf der Registerkarte **Ergebnisse** werden weiterhin die Resultsets aus den [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen angezeigt, die während einer Debuggingsitzung ausgeführt werden. Auf der Registerkarte **Meldungen** werden weiterhin Systemmeldungen angezeigt, wie z. B. „ *xx* Zeilen betroffen“, und die Ausgabe von PRINT- und RAISERROR-Anweisungen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Lokal (Fenster)](./transact-sql-debugger-locals-window.md)   
 [Überwachung (Fenster)](./transact-sql-debugger-watch-window.md)   
 [Dialogfeld 'Schnellüberwachung'](./transact-sql-debugger-quickwatch-dialog-box.md)   
 [Fenster 'Breakpoints'](./transact-sql-debugger-breakpoints-window.md)   
 [Fenster 'Aufrufliste'](./transact-sql-debugger-call-stack-window.md)   
 [Fenster 'Threads'](./transact-sql-debugger-threads-window.md)   
 [Ausgabefenster](./transact-sql-debugger-output-window.md)   
 [Transact-SQL-Debugger](./transact-sql-debugger.md)  
  
