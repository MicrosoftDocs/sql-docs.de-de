---
title: SQL Injection | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Angriffe durch Einschleusung von SQL-Befehlen funktionieren. Schützen Sie sich vor solchen Angriffen, indem Sie Eingaben validieren und Code für die Einschleusung von SQL-Befehlen in SQL Server überprüfen.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: security, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- SQL Injection
ms.assetid: eb507065-ac58-4f18-8601-e5b7f44213ab
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 074033472f37787c0bcf1da6f261a5f835f0cbec
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838975"
---
# <a name="sql-injection"></a>Einschleusung von SQL-Befehlen
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Eine Einschleusung von SQL-Befehlen ist ein Angriff, bei dem Schadcode in Zeichenfolgen eingefügt wird, die später zur Analyse und Ausführung an eine Instanz von SQL Server übergeben werden. Sie sollten jede Prozedur, die SQL-Anweisungen erstellt, nach Injection-Anfälligkeiten überprüfen, denn SQL Server führt alle empfangenen gültigen Abfragen aus. Auch parametrisierte Daten können von einem technisch versierten Angreifer manipuliert werden.  
  
## <a name="how-sql-injection-works"></a>Funktionsweise von SQL Injection  
 Die Primärform von SQL-Injection besteht aus dem direkten Einfügen des Codes in Benutzereingabevariablen, die mit SQL-Befehlen verkettet und ausgeführt werden. Bei einem weniger direkten Angriff wird bösartiger Code in Zeichenfolgen eingefügt, die in einer Tabelle oder als Metadaten gespeichert werden sollen. Wenn die gespeicherten Zeichenfolgen später in einem dynamischen SQL-Befehl verkettet werden, wird der bösartige Code ausgeführt.  
  
 Die Funktionalität des Injection-Prozesses besteht in der vorzeitigen Beendigung einer Textzeichenfolge und dem Anhängen eines neuen Befehls. Da vor der Ausführung des eingefügten Befehls möglicherweise weitere Zeichenfolgen daran angehängt wurden, beendet der Missetäter die eingefügte Zeichenfolge durch eine Kommentarmarkierung "--". Nachfolgender Text wird zur Ausführungszeit ignoriert.  
  
 Das folgende Skript zeigt ein einfaches SQL-Injection-Beispiel. Das Skript erstellt eine SQL-Abfrage, indem hartcodierte Zeichenfolgen mit einer vom Benutzer eingegebenen Zeichenfolge verkettet werden:  
  
```csharp
var Shipcity;  
ShipCity = Request.form ("ShipCity");  
var sql = "select * from OrdersTable where ShipCity = '" + ShipCity + "'";  
```  
  
 Der Benutzer wird aufgefordert, den Namen einer Stadt einzugeben. Bei der Eingabe `Redmond`sieht die vom Skript erstellt Abfrage ungefähr wie die folgende aus:  
  
```sql
SELECT * FROM OrdersTable WHERE ShipCity = 'Redmond'  
```  
  
 Angenommen, der Benutzer gibt jedoch Folgendes ein:  
  
```sql
Redmond'; drop table OrdersTable--  
```  
  
 In diesem Fall wird die folgende Abfrage von dem Skript erstellt:  
  
```sql
SELECT * FROM OrdersTable WHERE ShipCity = 'Redmond';drop table OrdersTable--'  
```  
  
 Das Semikolon (;) bezeichnet das Ende der einen Abfrage und den Start einer anderen. Der doppelte Bindestrich (--) zeigt an, dass es sich beim restlichen Teil der aktuellen Zeile um einen Kommentar handelt, und die Zeile sollte ignoriert werden. Wenn der geänderte Code syntaktisch richtig ist, wird er vom Server ausgeführt. Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Anweisung verarbeitet, wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zuerst alle Datensätze in `OrdersTable` auswählen, wobei für `ShipCity` der Wert `Redmond`gilt. Anschließend wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] von `OrdersTable`gelöscht.  
  
 Solange der eingefügte SQL-Code syntaktisch richtig ist, kann eine Manipulation nicht programmgesteuert erkannt werden. Aus diesem Grund müssen Sie die Gültigkeit aller Benutzereingaben überprüfen und sorgfältig den Code überprüfen, der in dem von Ihnen verwendeten Server die erstellten SQL-Befehle ausführt. Die bewährten Methoden zum Schreiben von Code werden in den nachfolgenden Abschnitten in diesem Thema beschrieben.  
  
## <a name="validate-all-input"></a>Überprüfen aller Eingaben  
 Überprüfen Sie immer alle Benutzereingaben, indem Sie Typ, Länge, Format und Bereich testen. Bei der Implementierung von Vorsichtsmaßnahmen gegen böswillige Eingaben sollten Sie die Architektur und die Bereitstellungsszenarien Ihrer Anwendung berücksichtigen. Denken Sie daran, dass Programme, die zur Ausführung in einer sicheren Umgebung entworfen wurden, in eine nicht sichere Umgebung kopiert werden können. Die folgenden Vorschläge sollten als bewährte Methoden angesehen werden:  
  
-   Machen Sie keine Annahmen über Größe, Typ oder Inhalt der Daten, die von der Anwendung empfangen wurden. Beispielsweise sollten Sie die folgende Auswertung vornehmen:  
  
    -   Wie verhält sich Ihre Anwendung, wenn ein fehlgeleiteter oder böswilliger Benutzer eine 10 MB große MPEG-Datei an einer Stelle eingibt, an der die Anwendung eine Postleitzahl erwartet?  
  
    -   Wie verhält sich die Anwendung bei einer eingebetteten `DROP TABLE` -Anweisung in einem Textfeld?  
  
-   Testen Sie Größe und Datentyp der Eingaben, und erzwingen Sie entsprechende Grenzwerte. Dies kann bei der Vermeidung absichtlicher Pufferüberläufe hilfreich sein.  
  
-   Testen Sie den Inhalt der Zeichenfolgenvariablen, und akzeptieren Sie nur erwartete Werte. Lehnen Sie Einträge mit Binärdaten, Escapesequenzen und Befehlszeichen ab. Dies kann bei der Verhinderung von Injection-Skripts hilfreich sein und schützt möglicherweise vor einigen Pufferüberlauf-Exploits.  
  
-   Überprüfen Sie alle Daten bei der Eingabe gegen das Schema, wenn Sie mit XML-Dokumenten arbeiten.  
  
-   Erstellen Sie keine [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen direkt aus den Benutzereingaben.  
  
-   Verwenden Sie gespeicherte Prozeduren, um Benutzereingaben zu überprüfen.  
  
-   In Multi-Tier-Umgebungen sollten alle Daten vor dem Zugang zu einer vertrauensvollen Zone überprüft werden. Daten, die den Verifizierungsprozess nicht bestehen, sollten zurückgewiesen und eine Fehlermeldung sollte an die vorherige Stufe zurückgegeben werden.  
  
-   Implementieren Sie mehrere Überprüfungsebenen. Vorsichtsmaßnahmen, die Sie gegen gelegentliche böswillige Benutzer einsetzen, sind möglicherweise gegen technisch versierte Angreifer unwirksam. Die bessere Methode besteht aus dem Überprüfen der Eingaben in die Benutzeroberfläche und aller nachfolgenden Punkte, an denen eine Vertrauensgrenze gekreuzt wird.   
    Beispielsweise kann die Datenüberprüfung in einer clientseitigen Anwendung einfache Skript-Injection verhindern. Wenn jedoch auf der nächsten Ebene davon ausgegangen wird, dass die Eingabe bereits überprüft wurde, erhalten böswillige Benutzer mit der Fähigkeit, einen Client zu umgehen, uneingeschränkten Systemzugriff.  
  
-   Verketten Sie niemals Benutzereingaben, die nicht überprüft wurden. Die Zeichenfolgenverkettung ist der primäre Eingangspunkt für Script-Injection.  
  
-   Folgende Zeichenfolge dürfen in Feldern, aus denen Dateiname erstellt werden können, nicht zugelassen werden: AUX, CLOCK$, COM1 bis COM8, CON, CONFIG$, LPT1 bis LPT8, NUL und PRN.  
  
 Weisen Sie, wenn möglich, Eingaben mit den folgenden Zeichen zurück.  
  
|Eingabezeichen|Bedeutung in Transact-SQL|  
|---------------------|------------------------------|  
|**;**|Abfragetrennzeichen|  
|**'**|Trennzeichen für Datenzeichenfolgen.|  
|**--**|Einzeiliges Kommentartrennzeichen. Text hinter **--** bis zum Ende dieser Zeile wird vom Server nicht ausgewertet.|  
|**/\**_ ... _*\*/**|Kommentartrennzeichen. Der Text zwischen **/\* *_ und* \*/** _ wird vom Server nicht ausgewertet.|  
|**xp_**|Wird am Anfang des Namens von erweiterten gespeicherten Katalogprozeduren verwendet, wie z.B. `xp_cmdshell`.|  
  
### <a name="use-type-safe-sql-parameters"></a>Verwenden von typsicheren SQL-Parametern  
 Die **Parameters** -Sammlung in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt die Überprüfung von Typ und Länge bereit. Wenn Sie die **Parameters** -Sammlung verwenden, werden Eingaben als Literalwert und nicht als ausführbarer Code behandelt. Ein weiterer Vorteil beim Verwenden der **Parameters** -Sammlung besteht im Erzwingen von Typ- und Längenprüfungen. Werte außerhalb des Bereichs lösen eine Ausnahme aus. Das folgende Codefragment zeigt die Verwendung der **Parameters** -Sammlung:  
  
```csharp
SqlDataAdapter myCommand = new SqlDataAdapter("AuthorLogin", conn);  
myCommand.SelectCommand.CommandType = CommandType.StoredProcedure;  
SqlParameter parm = myCommand.SelectCommand.Parameters.Add("@au_id",  
     SqlDbType.VarChar, 11);  
parm.Value = Login.Text;  
```  
  
 In diesem Beispiel wird der `@au_id` -Parameter als Literalwert und nicht als ausführbarer Code behandelt. Bei diesem Wert werden Typ und Länge überprüft. Wenn der Wert `@au_id` keiner der angegebenen Typ- und Längeneinschränkungen entspricht, wird eine Ausnahme zurückgegeben.  
  
### <a name="use-parameterized-input-with-stored-procedures"></a>Verwenden von parametrisierten Eingaben mit gespeicherten Prozeduren  
 Wenn gespeicherte Prozeduren ungefilterte Eingaben verwenden, sind sie möglicherweise anfällig für SQL Injection. Der folgende Code ist z. B. nicht gegen Angriffe geschützt:  
  
```csharp
SqlDataAdapter myCommand =   
new SqlDataAdapter("LoginStoredProcedure '" +   
                               Login.Text + "'", conn);  
```  
  
 Wenn Sie gespeicherte Prozeduren verwenden, sollten Sie Parameter als Eingaben verwenden.  
  
### <a name="use-the-parameters-collection-with-dynamic-sql"></a>Verwenden der Parameterauflistung mit Dynamic SQL  
 Wenn die Verwendung von gespeicherten Prozeduren nicht möglich ist, können Sie weiterhin Parameter verwenden, wie in dem nachfolgenden Codebeispiel gezeigt wird.  
  
```csharp
SqlDataAdapter myCommand = new SqlDataAdapter(  
"SELECT au_lname, au_fname FROM Authors WHERE au_id = @au_id", conn);  
SQLParameter parm = myCommand.SelectCommand.Parameters.Add("@au_id",   
                        SqlDbType.VarChar, 11);  
Parm.Value = Login.Text;  
```  
  
### <a name="filtering-input"></a>Filtern von Eingaben  
 Auch das Filtern von Eingaben kann sich beim Schutz vor SQL-Injection als hilfreich erweisen. Bei dieser Methode werden Escapezeichen entfernt. Die große Zeichenmenge kann aber möglicherweise Probleme verursachen, die dazu führen, dass diese Verteidigungsmaßnahme nicht verlässlich ist. Das folgende Beispiel sucht nach Zeichenfolgentrennzeichen.  
  
```csharp
private string SafeSqlLiteral(string inputSQL)  
{  
  return inputSQL.Replace("'", "''");  
}  
```  
  
### <a name="like-clauses"></a>LIKE-Klauseln  
 Beachten Sie beim Verwenden einer `LIKE` -Klausel, dass Platzhalterzeichen weiterhin mit Escapezeichen versehen werden müssen:  
  
```csharp
s = s.Replace("[", "[[]");  
s = s.Replace("%", "[%]");  
s = s.Replace("_", "[_]");  
```  
  
## <a name="reviewing-code-for-sql-injection"></a>Überprüfen von Code gegen SQL-Injection  
 Sie sollten sämtlichen Code überprüfen, der `EXECUTE`, `EXEC`oder `sp_executesql`aufruft. Mithilfe von Abfragen, die den folgenden ähnlich sind, können Sie Prozeduren identifizieren, die diese Anweisungen enthalten. Diese Abfrage überprüft 1, 2, 3 oder 4 Leerzeichen nach den Wörtern `EXECUTE` und `EXEC`.  
  
```sql
SELECT object_Name(id) FROM syscomments  
WHERE UPPER(text) LIKE '%EXECUTE (%'  
OR UPPER(text) LIKE '%EXECUTE  (%'  
OR UPPER(text) LIKE '%EXECUTE   (%'  
OR UPPER(text) LIKE '%EXECUTE    (%'  
OR UPPER(text) LIKE '%EXEC (%'  
OR UPPER(text) LIKE '%EXEC  (%'  
OR UPPER(text) LIKE '%EXEC   (%'  
OR UPPER(text) LIKE '%EXEC    (%'  
OR UPPER(text) LIKE '%SP_EXECUTESQL%';  
```  
  
### <a name="wrapping-parameters-with-quotename-and-replace"></a>Einzuschließende Parameter mit QUOTENAME() und REPLACE()  
 Überprüfen Sie für jede ausgewählte gespeicherte Prozedur, ob alle Variablen richtig behandelt werden, die in dynamischem Transact-SQL verwendet werden. Daten aus den Eingabeparametern der gespeicherten Prozedur oder Daten, die aus der Tabelle gelesen werden, sollten in QUOTENAME() oder REPLACE() eingeschlossen werden. Beachten Sie, dass der @variable-Wert, der an QUOTENAME() übergeben wird, zu „sysname“ gehört. Er hat eine maximale Länge von 128 Zeichen.  
  
|\@variable|Empfohlener Wrapper|  
|---------------|-------------------------|  
|Der Name des sicherungsfähigen Elements|`QUOTENAME(@variable)`|  
|Zeichenfolge aus ≤ 128 Zeichen|`QUOTENAME(@variable, '''')`|  
|Zeichenfolge aus > 128 Zeichen|`REPLACE(@variable,'''', '''''')`|  
  
 Wenn Sie diese Technik verwenden, kann eine SET-Anweisung wie folgt überarbeitet werden:  
  
```sql
-- Before:  
SET @temp = N'SELECT * FROM authors WHERE au_lname ='''   
 + @au_lname + N'''';  
  
-- After:  
SET @temp = N'SELECT * FROM authors WHERE au_lname = '''   
 + REPLACE(@au_lname,'''','''''') + N'''';  
```  
  
### <a name="injection-enabled-by-data-truncation"></a>Aktivieren von SQL-Injection durch das Abschneiden von Daten  
 Jede dynamische [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung, die einer Variablen zugewiesen ist, wird abgeschnitten, falls sie größer als der für die Variable zugewiesene Puffer ist. Ein Angreifer, der in der Lage ist, das Abschneiden der Anweisungen durch das Übergeben von unerwartet langen Zeichenfolgen an eine gespeicherte Prozedur zu erzwingen, kann das Ergebnis verändern. Beispielsweise ist die gespeicherte Prozedur, die von dem folgenden Skript erstellt wurde, anfällig für SQL-Injection durch aktiviertes Abschneiden.  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
@loginname sysname,  
@old sysname,  
@new sysname  
AS  
-- Declare variable.  
-- Note that the buffer here is only 200 characters long.   
DECLARE @command varchar(200)  
-- Construct the dynamic Transact-SQL.  
-- In the following statement, we need a total of 154 characters   
-- to set the password of 'sa'.   
-- 26 for UPDATE statement, 16 for WHERE clause, 4 for 'sa', and 2 for  
-- quotation marks surrounded by QUOTENAME(@loginname):  
-- 200 - 26 - 16 - 4 - 2 = 154.  
-- But because @new is declared as a sysname, this variable can only hold  
-- 128 characters.   
-- We can overcome this by passing some single quotation marks in @new.  
SET @command= 'update Users set password='   
    + QUOTENAME(@new, '''') + ' where username='   
    + QUOTENAME(@loginname, '''') + ' AND password = '   
    + QUOTENAME(@old, '''')  
  
-- Execute the command.  
EXEC (@command)  
GO  
```  
  
 Durch das Übergeben von 154 Zeichen an einen Puffer aus 128 Zeichen kann ein Angreifer ein neues Kennwort für „sa“ festlegen, ohne das alte Kennwort zu kennen.  
  
```sql
EXEC sp_MySetPassword 'sa', 'dummy',   
'123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012'''''''''''''''''''''''''''''''''''''''''''''''''''   
```  
  
 Aus diesem Grund sollten Sie einen großen Puffer für eine Befehlsvariable verwenden oder dynamisches [!INCLUDE[tsql](../../includes/tsql-md.md)] direkt in der `EXECUTE` -Anweisung ausführen.  
  
### <a name="truncation-when-quotenamevariable--and-replace-are-used"></a>Abschneiden beim Verwenden von QUOTENAME(@variable, '''') und REPLACE()  
 Zeichenfolgen, die von QUOTENAME() und REPLACE() zurückgegeben werden, werden automatisch abgeschnitten, falls Sie über dem zugewiesenen Speicherplatz liegen. Die im folgenden Beispiel erstellte, gespeicherte Prozedur zeigt, was in den Fällen passieren kann.  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
    @loginname sysname,  
    @old sysname,  
    @new sysname  
AS  
  
-- Declare variables.  
    DECLARE @login sysname  
    DECLARE @newpassword sysname  
    DECLARE @oldpassword sysname  
    DECLARE @command varchar(2000)  
  
-- In the following statements, the data stored in temp variables  
-- will be truncated because the buffer size of @login, @oldpassword,  
-- and @newpassword is only 128 characters, but QUOTENAME() can return  
-- up to 258 characters.  
    SET @login = QUOTENAME(@loginname, '''')  
    SET @oldpassword = QUOTENAME(@old, '''')  
    SET @newpassword = QUOTENAME(@new, '''')  
  
-- Construct the dynamic Transact-SQL.  
-- If @new contains 128 characters, then @newpassword will be '123... n  
-- where n is the 127th character.   
-- Because the string returned by QUOTENAME() will be truncated,   
-- it can be made to look like the following statement:  
-- UPDATE Users SET password ='1234. . .[127] WHERE username=' -- other stuff here  
    SET @command = 'UPDATE Users set password = ' + @newpassword   
     + ' where username =' + @login + ' AND password = ' + @oldpassword;  
  
-- Execute the command.  
EXEC (@command);  
GO  
```  
  
 Daher legt die folgende Anweisung die Kennwörter von allen Benutzern auf den Wert fest, der im vorherigen Code übergeben wurde.  
  
```sql
EXEC sp_MyProc '--', 'dummy', '12345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678'  
```  
  
 Sie können das Abschneiden der Zeichenfolge durch das Überschreiten des zugewiesenen Pufferplatzes erzwingen, falls Sie REPLACE() verwenden. Die im folgenden Beispiel erstellte, gespeicherte Prozedur zeigt, was in den Fällen passieren kann.  
  
```sql
CREATE PROCEDURE sp_MySetPassword  
    @loginname sysname,  
    @old sysname,  
    @new sysname  
AS  
  
-- Declare variables.  
    DECLARE @login sysname  
    DECLARE @newpassword sysname  
    DECLARE @oldpassword sysname  
    DECLARE @command varchar(2000)  
  
-- In the following statements, data will be truncated because   
-- the buffers allocated for @login, @oldpassword and @newpassword   
-- can hold only 128 characters, but QUOTENAME() can return   
-- up to 258 characters.   
    SET @login = REPLACE(@loginname, '''', '''''')  
    SET @oldpassword = REPLACE(@old, '''', '''''')  
    SET @newpassword = REPLACE(@new, '''', '''''')  
  
-- Construct the dynamic Transact-SQL.  
-- If @new contains 128 characters, @newpassword will be '123...n   
-- where n is the 127th character.   
-- Because the string returned by QUOTENAME() will be truncated, it  
-- can be made to look like the following statement:  
-- UPDATE Users SET password='1234...[127] WHERE username=' -- other stuff here   
    SET @command= 'update Users set password = ''' + @newpassword + ''' where username='''   
     + @login + ''' AND password = ''' + @oldpassword + '''';  
  
-- Execute the command.  
EXEC (@command);  
GO  
```  
  
 Wie bei QUOTENAME() kann das Abschneiden der Zeichenfolge durch REPLACE() vermieden werden, indem temporäre Variablen deklariert werden, die für alle Fälle groß genug sind. Falls möglich, sollten Sie QUOTENAME() oder REPLACE() direkt innerhalb von dynamischem [!INCLUDE[tsql](../../includes/tsql-md.md)]aufrufen. Andernfalls können Sie den erforderlichen Puffer wie folgt berechnen. Für `@outbuffer = QUOTENAME(@input)`sollte die Größe von `@outbuffer` gleich dem Wert `2*(len(@input)+1)`sein. Wenn Sie wie in dem vorherigen Beispiel `REPLACE()` und doppelte Anführungszeichen verwenden, reicht ein Puffer von `2*len(@input)` aus.  
  
 Die folgende Berechnung deckt alle Fälle ab:  
  
```sql
WHILE LEN(@find_string) > 0, required buffer size =  
ROUND(LEN(@input)/LEN(@find_string),0) * LEN(@new_string)   
 + (LEN(@input) % LEN(@find_string))  
```  
  
### <a name="truncation-when-quotenamevariable--is-used"></a>Abschneiden beim Verwenden von QUOTENAME(@variable, ']')  
 Ein Abschneiden ist möglich, wenn der Name eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sicherungsfähigen Elements an Anweisungen übergeben wird, die das Format `QUOTENAME(@variable, ']')`verwenden. Im folgenden Beispiel wird dies veranschaulicht.  
  
```sql
CREATE PROCEDURE sp_MyProc  
    @schemaname sysname,  
    @tablename sysname,  
AS  
  
-- Declare a variable as sysname. The variable will be 128 characters.  
-- But @objectname actually must allow for 2*258+1 characters.   
DECLARE @objectname sysname  
SET @objectname = QUOTENAME(@schemaname)+'.'+ QUOTENAME(@tablename)   
-- Do some operations.  
GO  
```  
  
 Wenn Sie Werte vom Typ „sysname“ verketten, sollten Sie temporäre Variablen verwenden, die groß genug sind, um die maximale Anzahl von 128 Zeichen pro Wert aufzunehmen. Falls möglich, sollten Sie `QUOTENAME()` direkt innerhalb von dynamischem [!INCLUDE[tsql](../../includes/tsql-md.md)]aufrufen. Andernfalls können Sie die erforderliche Puffergröße so berechnen, wie es im vorherigen Abschnitt beschrieben wird.  
  
## <a name="see-also"></a>Weitere Informationen  
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [REPLACE &#40;Transact-SQL&#41;](../../t-sql/functions/replace-transact-sql.md)   
 [QUOTENAME &#40;Transact-SQL&#41;](../../t-sql/functions/quotename-transact-sql.md)   
 [sp_executesql &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md)   
 [Sichern von SQL Server](../../relational-databases/security/securing-sql-server.md)  
  
  
