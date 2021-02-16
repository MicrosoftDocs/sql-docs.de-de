---
description: Bestimmen, ob die Änderungsdaten bereit sind
title: Bestimmen, ob die Änderungsdaten bereit sind | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- incremental load [Integration Services],determining readiness
ms.assetid: 04935f35-96cc-4d70-a250-0fd326f8daff
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9fd82f56cb1edb459e662e31778fcb8341a6887c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353029"
---
# <a name="determine-whether-the-change-data-is-ready"></a>Bestimmen, ob die Änderungsdaten bereit sind

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  In der Ablaufsteuerung eines [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Pakets, das ein inkrementelles Laden von Änderungsdaten ausführt, besteht der zweite Task darin, sicherzustellen, dass die Änderungsdaten für das ausgewählte Intervall bereit sind. Dieser Schritt ist notwendig, da der asynchrone Aufzeichnungsprozess möglicherweise noch nicht alle Änderungen bis zum ausgewählten Endpunkt verarbeitet hat.  
  
> [!NOTE]  
>  Beim ersten Task für die Ablaufsteuerung müssen die Endpunkte des Änderungsintervalls berechnet werden. Weitere Informationen zu diesem Task finden Sie unter [Angeben eines Intervalls von Änderungsdaten](../../integration-services/change-data-capture/specify-an-interval-of-change-data.md). Eine Beschreibung des Gesamtprozesses zum Entwurf der Ablaufsteuerung finden Sie unter [Change Data Capture &#40;SSIS&#41;](../../integration-services/change-data-capture/change-data-capture-ssis.md).  
  
## <a name="understanding-the-components-of-the-solution"></a>Grundlegendes zu den Komponenten der Lösung  
 Die in diesem Thema beschriebene Lösung verwendet 4 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Komponenten:  
  
-   Ein For-Schleifencontainer, der wiederholt die Ausgabe eines Tasks "SQL ausführen" auswertet.  
  
-   Ein Task "SQL ausführen", der spezielle Tabellen abfragt, die der Change Data Capture-Prozess verwaltet, und dann diese Informationen verwendet, um zu bestimmen, ob Daten bereit sind.  
  
-   Eine Komponente, die eine Verzögerung in die Verarbeitung implementiert, wenn die Daten nicht bereit sind. Dies kann entweder ein Skripttask oder ein Task "SQL ausführen" sein.  
  
-   Optional eine Komponente, die einen Fehler oder ein Timeout meldet, wenn der Tast "SQL ausführen" einen Wert zurückgibt, der eine Fehler- oder eine Timeoutbedingung angibt.  
  
 Diese Komponenten legen die Werte von mehreren Paketvariablen fest oder lesen sie, um den Ausführungsablauf innerhalb der Schleife und später im Paket zu steuern.  
  
#### <a name="to-set-up-package-variables"></a>So richten Sie Paketvariablen ein  
  
1.  Erstellen Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]im Fenster **Variablen** die folgenden Variablen:  
  
    1.  Erstellen Sie eine Variable mit einem integer-Datentyp, damit der Statuswert verzögert wird, der vom Task "SQL ausführen" zurückgegeben wird.  
  
         In diesem Beispiel wird der Variablenname "DataReady" mit einem Anfangswert von 0 verwendet.  
  
    2.  Erstellen Sie eine Variable, um den Zeitraum für die Verzögerung festzulegen, wenn Daten nicht bereit sind. Wenn Sie einen Skripttask verwenden möchten, um die Verzögerung zu implementieren, sollte die Variable einen integer-Datentyp besitzen. Wenn Sie einen Task "SQL ausführen" mit einer WAITFOR-Anweisung verwenden möchten, sollte die Variable einen Zeichenfolge-Datentyp besitzen, um Werte wie "00:00:10" zu akzeptieren.  
  
         In diesem Beispiel wird der Variablenname "DelaySeconds" mit einem Anfangswert von 10 verwendet.  
  
    3.  Erstellen Sie eine Variable mit einem integer-Datentyp, um die aktuelle Iteration der Schleife zu verzögern.  
  
         In diesem Beispiel wird der Variablenname "TimeoutCount" mit einem Anfangswert von 0 verwendet.  
  
    4.  Erstellen Sie eine Variable mit einem integer-Datentyp, um die Anzahl festzulegen, die die Schleife nach Daten prüfen soll, bevor ein Timeout gemeldet wird.  
  
         In diesem Beispiel wird der Variablenname "TimeoutCeiling" mit einem Anfangswert von 20 verwendet.  
  
    5.  (Optional) Erstellen Sie eine Variable mit einem integer-Datentyp, die Sie verwenden können, um das erste Laden von Änderungsdaten anzuzeigen.  
  
         In diesem Beispiel wird der Variablenname "IntervalID" verwendet, und es wird nur auf einen Wert von 0 geprüft, um das erste Laden anzuzeigen.  
  
## <a name="configuring-a-for-loop-container"></a>Konfigurieren eines For-Schleifencontainers  
 Mit dem Variablensatz ist der For-Schleifencontainer die erste Komponente, die hinzugefügt werden muss.  
  
#### <a name="to-configure-a-for-loop-container-to-wait-until-change-data-is-ready"></a>So konfigurieren Sie einen For-Schleifencontainer für das Warten auf die Bereitschaft von Änderungsdaten  
  
1.  Fügen Sie auf der Registerkarte **Ablaufsteuerung** des [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designers der Ablaufsteuerung einen For-Schleifencontainer hinzu.  
  
2.  Verbinden Sie den Task "SQL ausführen", der die Endpunkte des Intervalls berechnet, mit dem For-Schleifencontainer.  
  
3.  Aktivieren Sie im **For-Schleifen-Editor** die folgenden Optionen:  
  
    1.  Geben Sie für **InitExpression**`@DataReady = 0`ein.  
  
         Dieser Ausdruck legt den Anfangswert der Schleifenvariablen fest.  
  
    2.  Geben Sie für **EvalExpression**`@DataReady == 0`ein.  
  
         Wenn dieser Ausdruck **False** ergibt, wird die Ausführung aus der Schleife weitergegeben, und das inkrementelle Laden beginnt.  
  
## <a name="configuring-the-execute-sql-task-that-queries-for-change-data"></a>Konfigurieren des Tasks "SQL ausführen", der Änderungsdaten abfragt  
 Innerhalb des For-Schleifencontainers fügen Sie einen Task "SQL ausführen" hinzu. Dieser Task fragt die Tabellen ab, die der Change Data Capture-Prozess in der Datenbank verwaltet. Das Ergebnis dieser Abfrage ist ein Statuswert, der angibt, ob die Änderungsdaten bereit sind.  
  
 In der folgenden Tabelle zeigt die erste Spalte die Werte an, die von der Task "SQL ausführen" durch die Beispiel-Transact-SQL-Abfrage zurückgegeben werden. Die zweite Spalte zeigt an, wie die anderen Komponenten auf diese Werte reagieren.  
  
|Rückgabewert|Bedeutung|Antwort|  
|------------------|-------------|--------------|  
|0|Gibt an, dass die Änderungsdaten nicht bereit sind.<br /><br /> Es gibt nach dem Endpunkt des ausgewählten Intervalls keine Change Data Capture-Datensätze.|Die Ausführung setzt mit der Komponente fort, die eine Verzögerung implementiert. Dann kehrt die Steuerung zum For-Schleifencontainer zurück, der weiterhin den Task "SQL ausführen" überprüft, solange der zurückgegebene Wert 0 ist.|  
|1|Könnte darauf hinweisen, dass die Änderungsdaten nicht für das vollständige Intervall erfasst wurden oder dass sie gelöscht wurden. Dies wird als Fehlerbedingung behandelt.<br /><br /> Es gibt vor dem Anfangspunkt des ausgewählten Intervalls keine Change Data Capture-Datensätze.|Die Ausführung setzt mit der optionalen Komponente fort, die den Fehler protokolliert.|  
|2|Gibt an, dass Daten bereit sind.<br /><br /> Es gibt Change Data Capture-Datensätze, die vor dem Anfangspunkt und nach dem Endpunkt des ausgewählten Intervalls liegen.|Die Ausführung wird aus dem For-Schleifencontainer weitergegeben, und das inkrementelle Laden beginnt.|  
|3|Gibt das erstmalige Laden aller verfügbaren Änderungsdaten an.<br /><br /> Die Bedingungslogik erhält diesen Wert von einer speziellen Paketvariablen, die nur für diesen Zweck verwendet wird.|Die Ausführung wird aus dem For-Schleifencontainer weitergegeben, und das inkrementelle Laden beginnt.|  
|5|Gibt an, dass der TimeoutCeiling erreicht wurde.<br /><br /> Die Schleife hat für die festgelegte Anzahl auf Daten getestet, und Daten sind immer noch nicht verfügbar. Ohne diesen Test oder einen ähnlichen Test könnte das Paket unbegrenzt ausgeführt werden.|Die Ausführung setzt mit der optionalen Komponente fort, die den Timeout protokolliert.|  
  
#### <a name="to-configure-an-execute-sql-task-to-query-whether-change-data-is-ready"></a>So konfigurieren Sie einen Task "SQL ausführen", um abzufragen, ob Änderungsdaten bereit sind  
  
1.  Fügen Sie innerhalb des For-Schleifencontainers einen Task "SQL ausführen" hinzu.  
  
2.  Aktivieren Sie im **Skripttask-Editor** auf der Seite **Skript** die folgenden Optionen:  
  
    1.  Wählen Sie für **ResultSet** die Option **Einzelne Zeile** aus.  
  
    2.  Konfigurieren Sie zur Quelldatenbank eine gültige Verbindung.  
  
    3.  Wählen Sie für **SQLSourceType** die Option **Direkteingabe** aus.  
  
    4.  Geben Sie für **SQLStatement** die folgende SQL-Anweisung ein:  
  
        ```  
        declare @DataReady int, @TimeoutCount int  
  
        if not exists (select tran_end_time from cdc.lsn_time_mapping  
                where tran_end_time > ?  )  
            select @DataReady = 0  
        else  
            if ? = 0  
                select @DataReady = 3   
        else  
            if not exists (select tran_end_time from cdc.lsn_time_mapping  
                    where tran_end_time <= ? )  
                select @DataReady = 1   
        else  
            select @DataReady = 2  
  
        select @TimeoutCount = ?  
        if (@DataReady = 0)  
            select @TimeoutCount = @TimeoutCount + 1  
        else  
            select @TimeoutCount = 0  
  
        if (@TimeoutCount > ?)  
            select @DataReady = 5  
  
        select @DataReady as DataReady, @TimeoutCount as TimeoutCount  
  
        ```  
  
3.  Nehmen Sie auf der Seite **Parameterzuordnung** vom **Editor für den Task 'SQL ausführen'** die folgenden Zuordnungen vor:  
  
    1.  Ordnen Sie dem Parameter 0 die ExtractEndTime-Variable zu.  
  
    2.  Ordnen Sie dem Parameter 1 die IntervalID-Variable zu.  
  
    3.  Ordnen Sie dem Parameter 2 die ExtractStartTime-Variable zu.  
  
    4.  Ordnen Sie dem Parameter 3 die TimeoutCount-Variable zu.  
  
    5.  Ordnen Sie dem Parameter 4 die TimeoutCeiling-Variable zu.  
  
4.  Ordnen Sie auf der Seite **Resultset** vom **Editor für den Task 'SQL ausführen'** das DataReady-Ergebnis der DataReady-Variablen und das TimeoutCount-Ergebnis der TimeoutCount-Variablen zu.  
  
## <a name="waiting-until-the-change-data-is-ready"></a>Warten, bis die Änderungsdaten bereit sind  
 Sie können unterschiedliche Methoden verwenden, um eine Verzögerung zu implementieren, wenn die Änderungsdaten nicht bereit sind. Die folgenden zwei Prozeduren veranschaulichen, wie mit einem Skripttask oder einem Task "SQL ausführen" eine Verzögerung implementiert wird.  
  
> [!NOTE]  
>  Ein vorkompiliertes Skript verursacht weniger Aufwand als ein Task "SQL ausführen".  
  
#### <a name="to-implement-a-delay-by-using-a-script-task"></a>So implementieren Sie eine Verzögerung mit einem Skripttask  
  
1.  Fügen Sie innerhalb des For-Schleifencontainers einen Skripttask hinzu.  
  
2.  Verbinden Sie den abfragenden Task "SQL ausführen", um zu bestimmen, ob die Änderungsdaten für den neuen Skripttask bereit sind.  
  
3.  Für die Rangfolgeneinschränkung, die den Task "SQL ausführen" mit dem Skripttask verbindet, öffnen Sie den **Rangfolgeneinschränkungs-Editor** , und wählen Sie die folgenden Optionen aus:  
  
    1.  Wählen Sie für **Auswertungsvorgang****Ausdruck und Einschränkung** aus.  
  
    2.  Wählen Sie für **Wert****Erfolg** aus.  
  
         Der Einschränkungswert von **Erfolg** verweist auf den Erfolg des vorherigen Tasks. In diesem Fall auf den Erfolg des Tasks "SQL ausführen".  
  
    3.  Geben Sie für **Ausdruck**`@DataReady == 0 && @TimeoutCount <= @TimeoutCeiling`ein.  
  
    4.  Wählen Sie **Logischer AND-Operator. Alle Einschränkungen müssen zu TRUE** ausgewertet werden, sofern die Option nicht bereits ausgewählt ist.  
  
4.  Wählen Sie im **Skripttask-Editor** auf der Seite **Skript** für **ReadOnlyVariables** die ganzzahlige Variable **User::DelaySeconds** aus der Liste aus.  
  
5.  Klicken Sie im **Skripttask-Editor** auf der Seite **Skript** auf **Skript bearbeiten** , um die Skriptentwicklungsumgebung zu öffnen.  
  
6.  Geben Sie in der Main-Prozedur eine der folgenden Codezeilen ein:  
  
    -   Wenn Sie in C# programmieren, geben Sie die folgende Codezeile ein:  
  
        ```  
        System.Threading.Thread.Sleep((int)Dts.Variables["DelaySeconds"].Value * 1000);  
        ```  
  
         \- oder -  
  
    -   Wenn Sie in [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]programmieren, geben Sie die folgende Codezeile ein:  
  
        ```  
        System.Threading.Thread.Sleep(Ctype(Dts.Variables("DelaySeconds").Value, Integer) * 1000)  
  
        ```  
  
        > [!NOTE]  
        >   Die **Thread.Sleep** -Methode erwartet ein Argument, das in Millisekunden angegeben wird.  
  
7.  Verlassen Sie die Standardcodezeile, die **DtsExecResult.Success** aus der Ausführung des Skripts zurückgibt.  
  
8.  Schließen Sie die Skriptentwicklungsumgebung und den **Skripttask-Editor**.  
  
#### <a name="to-implement-a-delay-by-using-an-execute-sql-task"></a>So implementieren Sie eine Verzögerung mit einem Task "SQL ausführen"  
  
1.  Fügen Sie innerhalb des For-Schleifencontainers einen Task "SQL ausführen" hinzu.  
  
2.  Verbinden Sie den abfragenden Task "SQL ausführen", um zu bestimmen, ob die Änderungsdaten für den neuen Task "SQL ausführen" bereit sind.  
  
3.  Für die Rangfolgeneinschränkung, die die zwei Tasks "SQL ausführen" verbindet, öffnen Sie den **Rangfolgeneinschränkungs-Editor** , und wählen Sie die folgenden Optionen aus:  
  
    1.  Wählen Sie für **Auswertungsvorgang****Ausdruck und Einschränkung** aus.  
  
    2.  Wählen Sie für **Wert****Erfolg** aus.  
  
         Der Einschränkungswert von **Erfolg** verweist auf den Erfolg des vorherigen Tasks "SQL ausführen".  
  
    3.  Geben Sie für **Ausdruck**`@DataReady == 0`ein.  
  
    4.  Wählen Sie **Logischer AND-Operator. Alle Einschränkungen müssen zu TRUE** ausgewertet werden, sofern die Option nicht bereits ausgewählt ist.  
  
         Diese Auswahl erfordert, dass beide Bedingungen, die Einschränkung und der Ausdruck, den Wert true haben müssen.  
  
4.  Aktivieren Sie im **Skripttask-Editor** auf der Seite **Skript** die folgenden Optionen:  
  
    1.  Wählen Sie für **ResultSet** die Option **Einzelne Zeile** aus.  
  
    2.  Konfigurieren Sie zur Quelldatenbank eine gültige Verbindung.  
  
    3.  Wählen Sie für **SQLSourceType** die Option **Direkteingabe** aus.  
  
    4.  Geben Sie für **SQLStatement** die folgende SQL-Anweisung ein:  
  
        ```  
        WAITFOR DELAY ?  
  
        ```  
  
5.  Ordnen Sie auf der Seite **Parameterzuordnung** des Editors dem Parameter 0 die DelaySeconds-Zeichenfolgenvariable zu.  
  
## <a name="handling-an-error-condition"></a>Behandeln einer Fehlerbedingung  
 Sie können optional eine zusätzliche Komponente innerhalb der Schleife konfigurieren, um eine Fehler- oder Timeoutbedingung zu protokollieren:  
  
-   Diese Komponente kann eine Fehlerbedingung protokollieren, wenn der Wert der DataReady-Variablen = 1. Dieser Wert gibt an, dass es keine verfügbaren Änderungsdaten vor dem Start des ausgewählten Intervalls gibt.  
  
-   Diese Komponente kann auch eine Timeoutbedingung protokollieren, wenn der Wert der TimeoutCeiling-Variablen erreicht wird. Dieser Wert gibt an, dass die Schleife für die festgelegte Anzahl auf Daten getestet hat und Daten immer noch nicht verfügbar sind. Ohne diesen Test oder einen ähnlichen Test könnte das Paket unbegrenzt ausgeführt werden.  
  
#### <a name="to-configure-an-optional-script-task-to-log-an-error-condition"></a>So konfigurieren Sie einen optionalen Skripttask zur Protokollierung einer Fehlerbedingung  
  
1.  Wenn Sie den Fehler oder das Timeout berichten möchten, indem Sie eine Meldung ins Protokoll schreiben, konfigurieren Sie die Protokollierung für das Paket. Weitere Informationen finden Sie unter [Aktivieren der Paketprotokollierung in SQL Server Data Tools](../../integration-services/performance/integration-services-ssis-logging.md#ssdt).  
  
2.  Fügen Sie innerhalb des For-Schleifencontainers einen Skripttask hinzu.  
  
3.  Verbinden Sie den abfragenden Task "SQL ausführen", um zu bestimmen, ob die Änderungsdaten für den neuen Skripttask bereit sind.  
  
4.  Für die Rangfolgeneinschränkung, die den Task "SQL ausführen" mit dem Skripttask verbindet, öffnen Sie den **Rangfolgeneinschränkungs-Editor** , und wählen Sie die folgenden Optionen aus:  
  
    1.  Wählen Sie für **Auswertungsvorgang****Ausdruck und Einschränkung** aus.  
  
    2.  Wählen Sie für **Wert****Erfolg** aus.  
  
         Der Einschränkungswert von **Erfolg** verweist auf den Erfolg des vorherigen Tasks. In diesem Fall auf den Erfolg des Tasks "SQL ausführen".  
  
    3.  Geben Sie für **Ausdruck**`@DataReady == 1 || @DataReady == 5`ein.  
  
    4.  Wählen Sie **Logischer AND-Operator. Alle Einschränkungen müssen zu TRUE** ausgewertet werden, sofern die Option nicht bereits ausgewählt ist.  
  
         Diese Auswahl erfordert, dass beide Bedingungen, die Einschränkung und der Ausdruck, den Wert true haben müssen.  
  
5.  Wählen Sie im **Skripttask-Editor** auf der Seite **Skript** des Editors für **ReadOnlyVariables****User::DataReady** und **User::ExtractStartTime** aus der Liste aus, um deren Werte für das Skript verfügbar zu machen.  
  
     Wenn Sie Informationen von bestimmten Systemvariablen (z. B. System::PackageName) in die Informationen, die in das Protokoll geschrieben werden, einschließen möchten, wählen Sie auch diese Variablen aus.  
  
6.  Klicken Sie im **Skripttask-Editor** auf der Seite **Skript** auf **Skript bearbeiten** , um die Skriptentwicklungsumgebung zu öffnen.  
  
7.  Geben Sie in der Main-Prozedur Code ein, um einen Fehler zu protokollieren, indem Sie die **Dts.Log** -Methode aufrufen, oder um ein Ereignis auszulösen, indem Sie eine der Methoden der **Dts.Events** -Schnittstelle aufrufen. Informieren Sie das Paket über den Fehler, indem Sie `Dts.TaskResult = Dts.Results.Failure`zurückgeben.  
  
     Im folgenden Beispiel wird gezeigt, wie eine Meldung ins Protokoll geschrieben wird. Weitere Informationen finden Sie unter [Logging in the Script Task](../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md), [Raising Events in the Script Task](../../integration-services/extending-packages-scripting/task/raising-events-in-the-script-task.md)und [Returning Results from the Script Task](../../integration-services/extending-packages-scripting/task/returning-results-from-the-script-task.md).  
  
    ```  
    ' User variables.  
    Dim dataReady As Integer = _  
      CType(Dts.Variables("DataReady").Value, Integer)  
    Dim extractStartTime As Date = _  
      CType(Dts.Variables("ExtractStartTime").Value, DateTime)  
  
    ' System variables.  
    Dim packageName As String = _  
      Dts.Variables("PackageName").Value.ToString()  
    Dim executionStartTime As Date = _  
      CType(Dts.Variables("StartTime").Value, DateTime)  
  
    Dim eventMessage As New System.Text.StringBuilder()  
  
    If dataReady = 1 OrElse dataReady = 5 Then  
  
      If dataReady = 1 Then  
        eventMessage.AppendLine("Start Time Error")  
      Else  
        eventMessage.AppendLine("Timeout Error")  
      End If  
  
      With eventMessage  
        .Append("The package ")  
        .Append(packageName)  
        .Append(" started at ")  
        .Append(executionStartTime.ToString())  
        .Append(" and ended at ")  
        .AppendLine(DateTime.Now().ToString())  
        If dataReady = 1 Then  
          .Append("The specified ExtractStartTime was ")  
          .AppendLine(extractStartTime.ToString())  
        End If  
      End With  
  
      System.Windows.Forms.MessageBox.Show(eventMessage.ToString())  
  
      Dts.Log(eventMessage.ToString(), 0, Nothing)  
  
      Dts.TaskResult = Dts.Results.Failure  
  
    Else  
  
      Dts.TaskResult = Dts.Results.Success  
  
    End If  
  
    ```  
  
8.  Schließen Sie die Skriptentwicklungsumgebung und den **Skripttask-Editor**.  
  
## <a name="next-step"></a>Nächster Schritt  
 Nachdem Sie ermittelt haben, dass die Änderungsdaten bereit sind, müssen Sie im nächsten Schritt die Abfrage der Änderungsdaten vorbereiten.  
  
 **Nächstes Thema:** [Vorbereiten zur Abfrage der Änderungsdaten](../../integration-services/change-data-capture/prepare-to-query-for-the-change-data.md)  
  
  
