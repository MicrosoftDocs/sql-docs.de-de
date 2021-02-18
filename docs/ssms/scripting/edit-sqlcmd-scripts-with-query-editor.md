---
title: Bearbeiten von SQLCMD-Skripts mit dem Abfrage-Editor
description: Sie verwenden SQLCMD-Skripts, wenn Sie Windows-Systembefehle und Transact-SQL -Anweisungen in demselben Skript verarbeiten müssen. In diesem Artikel erfahren Sie, wie Sie SQLCMD-Skripts mit dem Abfrage-Editor der Datenbank-Engine schreiben und bearbeiten.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- scripts [SQL Server], SQLCMD scripts
- SQLCMD scripts
- modifying scripts
- Query Editor [Database Engine], SQLCMD scripts
- scripts [SQL Server], SQL Server Management Studio
ms.assetid: f77b866d-c330-47c9-9e74-0b8d8dff4b31
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 60c88bce82f6eb13d1441ded4cb815cecd0842ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354668"
---
# <a name="edit-sqlcmd-scripts-with-query-editor"></a>Bearbeiten von SQLCMD-Skripts mit dem Abfrage-Editor

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Mit dem [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] können Sie Abfragen als SQLCMD-Skripts schreiben und bearbeiten. Sie verwenden SQLCMD-Skripts, wenn Sie Windows-Systembefehle und [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen in einem Skript verarbeiten müssen.  
  
## <a name="sqlcmd-mode"></a>SQLCMD-Modus  
 Wenn Sie den [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor zum Schreiben oder Bearbeiten von SQLCMD-Skripts verwenden möchten, müssen Sie den SQLCMD-Skriptmodus aktivieren. Standardmäßig ist der SQLCMD-Skriptmodus im Abfrage-Editor nicht aktiviert. Zum Aktivieren des Skriptmodus klicken Sie auf das Symbol **SQLCMD-Modus** auf der Symbolleiste, oder wählen Sie im Menü **Abfrage** die Option **SQLCMD-Modus** aus.  
  
> [!NOTE]  
>  Durch Aktivieren des SQLCMD-Modus werden IntelliSense und der [!INCLUDE[tsql](../../includes/tsql-md.md)] -Debugger im [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor deaktiviert.  
  
 SQLCMD-Skripts im Abfrage-Editor können dieselben Funktionen wie alle [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skripts verwenden. Zu diesen Funktionen gehören folgende:  
  
-   Farbcodierung  
  
-   Ausführen von Skripts  
  
-   Quellcodeverwaltung  
  
-   Analysieren von Skripts  
  
-   Showplan  
  
## <a name="enable-sqlcmd-scripting-in-query-editor"></a>Aktivieren von SQLCMD-Skripts im Abfrage-Editor  
 Verwenden Sie das folgende Verfahren, um die SQLCMD-Skripterstellung für ein aktives Fenster des [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editors zu aktivieren.  
  
#### <a name="to-switch-a-database-engine-query-editor-window-to-sqlcmd-mode"></a>So wechseln Sie in einem Abfrage-Editorfenster der Datenbank-Engine in den SQLCMD-Modus  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf den Server, und klicken Sie dann auf **Neue Abfrage**, um ein neues [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editorfenster zu öffnen.  
  
2.  Klicken Sie im Menü **Abfrage** auf **SQLCMD-Modus**.  
  
     Der Abfrage-Editor führt **sqlcmd** -Anweisungen im Kontext des Abfrage-Editors aus.  
  
3.  Wählen Sie auf der **SQL-Editor** -Symbolleiste in der Liste **Verfügbare Datenbanken** die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]-Datenbank aus.  
  
4.  Geben Sie im Fenster des Abfrage-Editors die folgenden beiden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen und die `!!DIR`-Anweisung **sqlcmd** ein:  
  
    ```  
    SELECT DISTINCT Type FROM Sales.SpecialOffer;  
    GO  
    !!DIR  
    GO  
    SELECT ProductCategoryID, Name FROM Production.ProductCategory;  
    GO  
    ```  
  
5.  Drücken Sie F5, um den gesamten Abschnitt gemischter [!INCLUDE[tsql](../../includes/tsql-md.md)] - und MS-DOS-Anweisungen auszuführen.  
  
     Beachten Sie die beiden SQL-Ergebnisbereiche aus der ersten und dritten Anweisung.  
  
6.  Klicken Sie im Bereich **Ergebnisse** auf die Registerkarte **Meldungen** , um die Meldungen für alle drei Anweisungen anzuzeigen:  
  
    -   (6 Zeile(n) betroffen)  
  
    -   \<The directory information>  
  
    -   (4 Zeile(n) betroffen)  
  
> [!IMPORTANT]  
>  Wenn das Hilfsprogramm **sqlcmd** an der Befehlszeile ausgeführt wird, ermöglicht es die vollständige Interaktion mit dem Betriebssystem. Wird der Abfrage-Editor im **SQLCMD-Modus** ausgeführt, müssen Sie darauf achten, dass Sie keine interaktiven Anweisungen ausführen. Der Abfrage-Editor kann auf keine Aufforderungen des Betriebssystems reagieren.  
  
 Weitere Informationen zum Ausführen von SQLCMD finden Sie unter [sqlcmd Utility](../../tools/sqlcmd-utility.md). Sie können auch das SQLCMD-Lernprogramm ausführen.  
  
## <a name="enable-sqlcmd-scripting-by-default"></a>Aktivieren der SQLCMD-Skripterstellung als Standard  
 Zum Aktivieren der SQLCMD-Skripterstellung als Standard wählen Sie im Menü **Extras** die Option **Optionen** aus, erweitern Sie **Abfrageausführung** und **SQL Server**, klicken Sie auf die Seite **Allgemein** , und aktivieren Sie das Kontrollkästchen **Standardmäßig neue Abfragen im SQLCMD-Modus öffnen** .  
  
## <a name="writing-and-editing-sqlcmd-scripts"></a>Schreiben und Bearbeiten von SQLCMD-Skripts  
 Wenn Sie den Skriptmodus aktiviert haben, können Sie SQLCMD-Befehle und [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen schreiben. Es gelten die folgenden Regeln:  
  
-   SQLCMD-Befehle müssen die erste Anweisung in einer Zeile sein.  
  
-   Pro Zeile ist nur ein SQLCMD-Befehl zulässig.  
  
-   Vor SQLCMD-Befehlen können Kommentare oder Leerzeichen stehen.  
  
-   SQLCMD-Befehle in Kommentarzeichen werden nicht ausgeführt.  
  
-   Bei Kommentaren, die aus einer einzelnen Zeile bestehen, bestehen die Kommentarzeichen aus zwei Bindestrichen (`--)` . Sie müssen am Anfang einer Zeile stehen.  
  
-   Vor Betriebssystembefehlen müssen zwei Ausrufezeichen (`!!`) stehen. Der Befehl mit doppelten Ausrufezeichen sorgt dafür, dass die Anweisung nach den Ausrufezeichen mit dem Befehlsprozessor `cmd.exe` ausgeführt wird. Der Text nach `!!` wird als Parameter an `cmd.exe`übergeben. Die letzte Befehlszeile wird somit wie folgt ausgeführt: `"%SystemRoot%\system32\cmd.exe /c <text after !!>"`.  
  
-   Um eine klare Unterscheidung zwischen SQLCMD-Befehlen und [!INCLUDE[tsql](../../includes/tsql-md.md)]zu treffen, müssen alle SQLCMD-Befehle als Präfix einen Doppelpunkt (`:`) haben.  
  
-   Der `GO` -Befehl kann möglicherweise verwendet werden, ohne ihm etwas voranzustellen oder mit vorangestelltem `!!:`  
  
-   Der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor unterstützt Umgebungsvariablen und Variablen, die als Teil eines SQLCMD-Skripts definiert sind. Integrierte SQLCMD- oder **osql** -Variablen werden jedoch nicht unterstützt. Bei der SQLCMD-Verarbeitung von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] wird für Variablen die Groß- und Kleinschreibung beachtet. PRINT '$(COMPUTERNAME)' erzeugt beispielsweise das korrekte Ergebnis, bei PRINT '$(ComputerName)' wird jedoch ein Fehler zurückgegeben.  
  
> [!CAUTION]
>  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] verwendet [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]SqlClient zur Ausführung im regulären und im SQLCMD-Modus. Beim Ausführen an der Befehlszeile verwendet SQLCMD den OLE DB-Anbieter. Da unterschiedliche Standardoptionen gelten können, wird beim Ausführen derselben Abfrage im SQLCMD-Modus in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] und im Hilfsprogramm SQLCMD möglicherweise ein anderes Verhalten erzielt.  
  
## <a name="supported-sqlcmd-syntax"></a>Unterstützte SQLCMD-Syntax  
 Der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor unterstützt die folgenden Schlüsselwörter für SQLCMD-Skripts:  
  
 `[!!:]GO[count]`  
  
 `!! <command>`  
  
 `:exit(statement)`  
  
 `:Quit`  
  
 `:r <filename>`  
  
 `:setvar <var> <value>`  
  
 `:connect server[\instance] [-l login_timeout] [-U user [-P password]]`  
  
 `:on error [ignore|exit]`  
  
 `:error <filename>|stderr|stdout`  
  
 `:out <filename>|stderr|stdout`  
  
> [!NOTE]  
>  `:error` und `:out`senden sowohl für `stderr` als auch für `stdout` die Ausgabe an die Registerkarte Meldungen.  
  
 SQLCMD-Befehle, die oben nicht aufgeführt sind, werden im Abfrage-Editor nicht unterstützt. Wenn ein Skript ausgeführt wird, das nicht unterstützte SQLCMD-Schlüsselwörter enthält, sendet der Abfrage-Editor für jedes nicht unterstützte Schlüsselwort die Meldung „Ignoring command \<ignored command*>“ (Befehl <ignorierter Befehl> wird ignoriert) an das Ziel. Das Skript wird erfolgreich ausgeführt. Die nicht unterstützten Befehle werden allerdings ignoriert.  
  
> [!CAUTION]  
>  Da Sie SQLCMD nicht über die Befehlszeile starten, bestehen beim Ausführen des Abfrage-Editors im SQLCMD-Modus einige Einschränkungen. So können Sie keine Befehlszeilenparameter wie Variablen übergeben. Außerdem müssen Sie darauf achten, keine interaktiven Anweisungen auszuführen, da der Abfrage-Editor nicht auf Eingabeaufforderungen des Betriebssystems antworten kann.  
  
## <a name="color-coding-in-sqlcmd-scripts"></a>Farbcodierung in SQLCMD-Skripts  
 Wenn die SQLCMD-Skripterstellung aktiviert sind, werden die Skripts farblich codiert. Die Farbcodierung für [!INCLUDE[tsql](../../includes/tsql-md.md)] -Schlüsselwörter bleibt gleich. SQLCMD-Befehle werden mit einem schattierten Hintergrund angezeigt.  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird eine **sqlcmd** -Anweisung zum Erstellen einer Ausgabedatei mit dem Namen „testoutput.txt“ verwendet, und es werden zwei [!INCLUDE[tsql](../../includes/tsql-md.md)] -SELECT-Anweisungen zusammen mit einem Betriebssystembefehl ausgeführt (zum Drucken des aktuellen Verzeichnisses). Die ergebene Datei enthält die Meldungsausgabe von der `DIR` -Anweisung gefolgt von der ergebenen Ausgabe der [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen.  
  
```  
:out C:\testoutput.txt  
SELECT @@VERSION As 'Server Version'  
!!DIR  
!!:GO  
SELECT @@SERVERNAME AS 'Server Name'  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQLCMD-Hilfsprogramm](../../tools/sqlcmd-utility.md)  
  
  
