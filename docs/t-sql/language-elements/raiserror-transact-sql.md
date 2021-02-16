---
description: RAISERROR (Transact-SQL)
title: RAISERROR (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 02/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RAISERROR
- RAISERROR_TSQL
- RAISEERROR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmessages system table
- errors [SQL Server], RAISERROR statement
- user-defined error messages [SQL Server]
- system flags
- generating errors [SQL Server]
- TRY block [SQL Server]
- recording errors
- ad hoc messages
- RAISERROR statement
- CATCH block
- messages [SQL Server], RAISERROR statement
ms.assetid: 483588bd-021b-4eae-b4ee-216268003e79
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f56638fef4166b07014314e5a79defd0d093b3cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339832"
---
# <a name="raiserror-transact-sql"></a>RAISERROR (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> [!NOTE]
> Die **RAISERROR**-Anweisung berücksichtigt **SET XACT_ABORT** nicht. In neuen Anwendungen sollte **THROW** statt **RAISERROR** verwendet werden.

  Generiert eine Fehlermeldung und initiiert die Verarbeitung von Fehlern für die Sitzung. RAISERROR kann entweder auf eine benutzerdefinierte, in der Katalogsicht „sys.messages“ gespeicherte Meldung verweisen oder eine Meldung dynamisch erstellen. Die Meldung wird als Serverfehlermeldung an die aufrufende Anwendung oder an einen zugeordneten CATCH-Block eines TRY...CATCH-Konstrukts zurückgegeben. In neuen Anwendungen sollte stattdessen [THROW](../../t-sql/language-elements/throw-transact-sql.md) verwendet werden.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
RAISERROR ( { msg_id | msg_str | @local_variable }  
    { ,severity ,state }  
    [ ,argument [ ,...n ] ] )  
    [ WITH option [ ,...n ] ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
RAISERROR ( { msg_str | @local_variable }  
    { ,severity ,state }  
    [ ,argument [ ,...n ] ] )  
    [ WITH option [ ,...n ] ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *msg_id*  
 Ist die Nummer einer in der Katalogsicht „sys.message“ mithilfe von „sp_addmessage“ gespeicherten benutzerdefinierten Fehlermeldung. Fehlernummern für benutzerdefinierte Fehlermeldungen müssen größer als 50000 sein. Wenn *msg_id* nicht angegeben ist, löst RAISERROR eine Fehlermeldung mit der Fehlernummer 50000 aus.  
  
 *msg_str*  
 Ist eine benutzerdefinierte Meldung mit einer Formatierung, die der **printf**-Funktion in der Standardbibliothek für C ähnelt. Die Fehlermeldung kann maximal 2.047 Zeichen enthalten. Wenn die Meldung mehr als 2.048 Zeichen enthält, werden nur die ersten 2.044 angezeigt und Auslassungspunkte angefügt, die anzeigen, dass die Meldung abgeschnitten wurde. Aufgrund des internen Speicherverhaltens beanspruchen Ersetzungsparameter mehr Zeichen als in der Ausgabe angezeigt werden. So ergibt beispielsweise der Ersetzungsparameter *%d* mit dem zugewiesenen Wert 2 tatsächlich ein einzelnes Zeichen in der Meldungszeichenfolge, beansprucht intern beim Speichern jedoch drei zusätzliche Zeichen. Durch diese Speicheranforderung wird die Anzahl von verfügbaren Zeichen für die Meldungsausgabe gesenkt.  
  
 Wenn *msg_str* angegeben ist, löst RAISERROR eine Fehlermeldung mit der Fehlernummer 50000 aus.  
  
 *msg_str* ist eine Zeichenfolge mit optionalen eingebetteten Konvertierungsspezifikationen. Jede Konvertierungsspezifikation definiert, wie ein Wert in der Argumentliste formatiert und in einem Feld an der Position der Konvertierungsspezifikation in *msg_str* platziert wird. Konvertierungsspezifikationen weisen das folgende Format auf:  
  
 % [[*Flag*] [*Breite*] [. *Genauigkeit*] [{h | l}]] *Typ*  
  
 Die folgenden Parameter können in *msg_str* verwendet werden:  
  
 *flag*  
  
 Ist ein Code, der den Abstand und die Ausrichtung des ersetzten Werts bestimmt.  
  
|Code|Präfix oder Ausrichtung|BESCHREIBUNG|  
|----------|-----------------------------|-----------------|  
|- (Minus)|Linksbündig|Der Argumentwert wird innerhalb der angegebenen Feldbreite linksbündig ausgegeben.|  
|+ (Plus)|Zeichenpräfix|Dem Argumentwert wird ein Plus- (+) oder Minuszeichen (-) vorangestellt, wenn der Wert einen Datentyp mit Vorzeichen aufweist.|  
|0 (Null)|Auffüllung mit Nullen|Der Ausgabe wird 0 vorangestellt, bis die Mindestbreite erreicht ist. Wenn 0 und das Minuszeichen (-) auftreten, wird 0 ignoriert.|  
|# (Nummer)|0x-Präfix für Hexadezimaltyp von x oder X|Das Nummernzeichenflag (#) wird jedem Wert ungleich 0 mit 0, 0x bzw. 0X vorangestellt, wenn es mit dem Format o, x oder X verwendet wird. Wenn das Nummernzeichenflag (#) vor d, i oder u steht, wird das Flag ignoriert.|  
|" " (Leerzeichen)|Auffüllung mit Leerstellen|Dem Ausgabewert werden Leerzeichen vorangestellt, wenn der Wert ein Vorzeichen aufweist und positiv ist. Dies wird ignoriert, wenn er mit dem Pluszeichenflag (+) versehen ist.|  
  
 *width*  
  
 Ist eine ganze Zahl, die die Mindestbreite des Felds definiert, in das der Argumentwert platziert werden soll. Wenn die Länge des Argumentwerts gleich oder länger als der Parameter *width* ist, wird der Wert ohne Auffüllung ausgegeben. Wenn der Wert kürzer als der Parameter *width* ist, wird der Wert bis zu der unter *width* angegebenen Länge aufgefüllt.  
  
 Ein Sternchen (*) bedeutet, dass die Breite durch das zugeordnete Argument in der Argumentliste angegeben wird, welches ein Wert für eine ganze Zahl sein muss.  
  
 *precision*  
  
 Ist die maximale Anzahl von Zeichen aus dem Argumentwert für Zeichenfolgenwerte. Wenn beispielsweise eine Zeichenfolge über fünf Zeichen verfügt und die Genauigkeit 3 beträgt, werden nur die ersten drei Zeichen des Zeichenfolgenwerts verwendet.  
  
 Bei ganzzahligen Werten gibt der Parameter *precision* die Mindestanzahl der ausgegebenen Stellen an.  
  
 Ein Sternchen (*) bedeutet, dass die Genauigkeit durch das zugeordnete Argument in der Argumentliste angegeben wird, welches ein Wert für eine ganze Zahl sein muss.  
  
 {h | l} *type*  
  
 Wird zusammen mit den Zeichentypen d, i, o, s, x, X oder u verwendet und erstellt **shortint** (h)- oder **longint** (l)-Werte.  
  
|Typspezifikation|repräsentiert|  
|------------------------|----------------|  
|d oder i|Ganze Zahl mit Vorzeichen|  
|o|Oktal ohne Vorzeichen|  
|s|String|  
|u|Ganze Zahl ohne Vorzeichen|  
|x oder X|Hexadezimal ohne Vorzeichen|  
  
> [!NOTE]  
>  Diese Typspezifikationen basieren auf den ursprünglich für die **printf**-Funktion in der Standardbibliothek für C definierten Typspezifikationen. Die in RAISERROR-Meldungszeichenfolgen verwendeten Typspezifikationen werden [!INCLUDE[tsql](../../includes/tsql-md.md)]-Datentypen zugeordnet, während die in **printf** verwendeten Spezifikationen Datentypen der Programmiersprache C zugeordnet werden. In **printf** verwendete Typspezifikationen werden nicht von RAISERROR unterstützt, wenn [!INCLUDE[tsql](../../includes/tsql-md.md)] nicht über einen mit dem zugeordneten C-Datentyp vergleichbaren Datentyp verfügt. So wird die *%p*-Spezifikation für Zeiger beispielsweise nicht in RAISERROR unterstützt, da [!INCLUDE[tsql](../../includes/tsql-md.md)] über keinen Zeigerdatentyp verfügt.  
  
> [!NOTE]  
>  Zum Konvertieren eines Werts in den [!INCLUDE[tsql](../../includes/tsql-md.md)]-Datentyp **bigint** müssen Sie **%I64d** angeben.  
  
 *\@local_variable*  
 Eine Variable eines beliebigen Zeichendatentyps, die eine Zeichenfolge enthält, die auf die gleiche Weise wie *msg_str* formatiert ist. *\@local_variable* muss auf **char** oder **varchar** festgelegt oder implizit in diese Datentypen konvertierbar sein.  
  
 *severity*  
 Der benutzerdefinierte [Schweregrad](../../relational-databases/errors-events/database-engine-error-severities.md), der dieser Meldung zugeordnet ist. Wird *msg_id* zum Auslösen einer mithilfe von „sp_addmessage“ erstellten benutzerdefinierten Meldung verwendet, überschreibt der in RAISERROR angegebene Schweregrad den Schweregrad in „sp_addmessage“.  
  
 Schweregrade von 0 bis 18 können von jedem Benutzer angegeben werden. Schweregrade von 19 bis 25 können nur von Mitgliedern der festen Serverrolle „sysadmin“ oder von Benutzern mit ALTER TRACE-Berechtigungen angegeben werden. Für Schweregrade von 19 bis 25 ist die Option WITH LOG erforderlich. Geringere Schweregrade als 0 werden als 0 interpretiert. Höhere Schweregrade als 25 werden als 25 interpretiert.  
  
> [!CAUTION]  
>  Die Schweregrade von 20 bis 25 werden als schwerwiegend angesehen. Wird ein schwerwiegender Schweregrad ermittelt, so wird die Clientverbindung nach Empfang der Meldung beendet und der Fehler in den Fehler- und Anwendungsprotokollen protokolliert.  
  
 Sie können -1 angeben, um den Wert für den Schweregrad des Fehlers zurückzugeben, wie im folgenden Beispiel veranschaulicht.  
  
```sql  
RAISERROR (15600,-1,-1, 'mysp_CreateCustomer');  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Msg 15600, Level 15, State 1, Line 1   
 An invalid parameter or option was specified for procedure 'mysp_CreateCustomer'.
 ```  
  
 *state*  
 Eine ganze Zahl zwischen 0 und 255. Negative Werte werden standardmäßig auf 1 festgelegt. Werte größer als 255 sollten nicht verwendet werden. 
  
 Wird derselbe benutzerdefinierte Fehler an mehreren Stellen ausgelöst, kann durch Verwenden einer eindeutigen Statusnummer für die einzelnen Positionen der Codeabschnitt ermittelt werden, der die Fehler auslöst.  
  
 *argument*  
 Entspricht den Parametern, die beim Ersetzen der in *msg_str* oder der *msg_id* entsprechenden Meldung definierten Variablen verwendet werden. Es können 0 oder mehr Ersetzungsparameter vorhanden sein, jedoch nicht mehr als 20. Die einzelnen Ersetzungsparameter können lokale Variablen sein oder einen der folgenden Datentypen aufweisen: **tinyint**, **smallint**, **int**, **char**, **varchar**, **nchar**, **nvarchar**, **binary** oder **varbinary**. Es werden keine weiteren Datentypen unterstützt.  
  
 *Option*  
 Ist eine benutzerdefinierte Option für den Fehler und kann einer der Werte in der folgenden Tabelle sein.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|PROTOKOLL|Protokolliert den Fehler im Fehler- und im Anwendungsprotokoll für die [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz. Im Fehlerprotokoll protokollierte Fehler sind derzeit auf maximal 440 Bytes beschränkt. Nur Mitglieder der festen Serverrolle „sysadmin“ oder Benutzer mit ALTER TRACE-Berechtigungen können WITH LOG angeben.<br /><br /> [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)]|  
|NOWAIT|Sendet Meldungen sofort an den Client.<br /><br /> [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)]|  
|SETERROR|Legt die Werte @@ERROR und ERROR_NUMBER unabhängig vom Schweregrad auf *msg_id* oder 50000 fest.<br /><br /> [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)]|  
  
## <a name="remarks"></a>Bemerkungen  
 Die von RAISERROR generierten Fehler sind mit Fehlern vergleichbar, die durch den [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Code generiert werden. Die von RAISERROR angegebenen Werte werden von den Systemfunktionen ERROR_LINE, ERROR_MESSAGE, ERROR_NUMBER, ERROR_PROCEDURE, ERROR_SEVERITY, ERROR_STATE und @@ERROR gemeldet. Wird RAISERROR mit einem Schweregrad von 11 oder höher in einem TRY-Block ausgeführt, wird die Steuerung an den zugeordneten CATCH-Block übertragen. Der Fehler wird an den Aufrufer zurückgegeben, wenn RAISERROR ausgeführt wird:  
  
-   Außerhalb des Bereichs eines beliebigen TRY-Blocks.  
  
-   Mit einem Schweregrad von 10 oder tiefer in einem TRY-Block.  
  
-   Mit einem Schweregrad von 20 oder höher, der die Datenbankverbindung beendet.  
  
 CATCH-Blöcke können mithilfe von RAISERROR den Fehler erneut auslösen, der den CATCH-Block mithilfe von Systemfunktionen wie ERROR_NUMBER und ERROR_MESSAGE aufgerufen hat, um die ursprünglichen Fehlerinformationen abzurufen. @@ERROR wird bei Meldungen mit einem Schweregrad von 1 bis 10 standardmäßig auf 0 festgelegt.  
  
 Gibt *msg_id* eine benutzerdefinierte Meldung aus der Katalogsicht „sys.messages“ an, verarbeitet RAISERROR die Meldung aus der Textspalte mithilfe derselben Regeln, die auch auf den Text einer mithilfe von *msg_str* angegebenen benutzerdefinierten Meldung angewendet werden. Der Text der benutzerdefinierten Meldung kann Konvertierungsspezifikationen enthalten, und RAISERROR ordnet den Konvertierungsspezifikationen Argumentwerte zu. Mithilfe von „sp_addmessage“ können Sie benutzerdefinierte Fehlermeldungen hinzufügen, während mit „sp_dropmessage“ benutzerdefinierte Fehlermeldungen gelöscht werden.  
  
 RAISERROR kann alternativ zu PRINT für die Rückgabe von Meldungen an aufrufende Anwendungen verwendet werden. RAISERROR unterstützt das Ersetzen von Zeichen ähnlich wie die **printf**-Funktion in der Standardbibliothek für C, während die [!INCLUDE[tsql](../../includes/tsql-md.md)] PRINT-Anweisung diese Funktionalität nicht besitzt. Die PRINT-Anweisung ist von TRY-Blöcken nicht betroffen. Wird jedoch RAISERROR mit einem Schweregrad von 11 bis 19 in einem TRY-Block ausgeführt, wird die Steuerung an den zugeordneten CATCH-Block übertragen. Geben Sie einen Schweregrad von 10 oder niedriger an, um RAISERROR für die Rückgabe einer Meldung aus einem TRY-Block ohne Aufrufen des CATCH-Blocks zu verwenden.  
  
 In der Regel ersetzen aufeinander folgende Argumente aufeinander folgende Konvertierungsspezifikationen. Das erste Argument ersetzt die erste Konvertierungsspezifikation, das zweite Argument ersetzt die zweite Konvertierungsspezifikation usw. In der folgenden `RAISERROR`-Anweisung ersetzt beispielsweise das erste Argument von `N'number'` die erste Konvertierungsspezifikation von `%s`, und das zweite Argument von `5` ersetzt die zweite Konvertierungsspezifikation von `%d.`.  
  
```sql  
RAISERROR (N'This is message %s %d.', -- Message text.  
           10, -- Severity,  
           1, -- State,  
           N'number', -- First argument.  
           5); -- Second argument.  
-- The message text returned is: This is message number 5.  
GO  
```  
  
 Wird ein Sternchen (*) entweder für die Breite oder die Genauigkeit einer Konvertierungsspezifikation angegeben, wird der für die Breite oder Genauigkeit zu verwendende Wert als ein ganzzahliger Argumentwert angegeben. In diesem Fall kann eine Konvertierungsspezifikation bis zu drei Argumente verwenden - jeweils eines für den Breiten-, Genauigkeits- und den Ersetzungswert.  
  
 So geben beispielsweise beide folgenden `RAISERROR`-Anweisungen dieselbe Zeichenfolge zurück. Eine gibt die Breiten- und Genauigkeitswerte in der Argumentliste an, während die andere die Werte in der Konvertierungsspezifikation angibt.  
  
```sql  
RAISERROR (N'<\<%*.*s>>', -- Message text.  
           10, -- Severity,  
           1, -- State,  
           7, -- First argument used for width.  
           3, -- Second argument used for precision.  
           N'abcde'); -- Third argument supplies the string.  
-- The message text returned is: <<    abc>>.  
GO  
RAISERROR (N'<\<%7.3s>>', -- Message text.  
           10, -- Severity,  
           1, -- State,  
           N'abcde'); -- First argument supplies the string.  
-- The message text returned is: <<    abc>>.  
GO  
```  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-returning-error-information-from-a-catch-block"></a>A. Zurückgeben von Fehlerinformationen aus einem CATCH-Block  
 Im folgenden Codebeispiel wird dargestellt, wie `RAISERROR` in einem `TRY`-Block verwendet wird, sodass die Ausführung zum zugeordneten `CATCH`-Block springt. Darüber hinaus wird dargestellt, wie `RAISERROR` verwendet wird, um Informationen zu dem Fehler zurückzugeben, der den `CATCH`-Block aufgerufen hat.  
  
> [!NOTE]  
>  RAISERROR generiert nur Fehler mit dem Status 1 bis 127. Da [!INCLUDE[ssDE](../../includes/ssde-md.md)] u. U. Fehler mit dem Status 0 ausgibt, wird empfohlen, den von ERROR_STATE zurückgegebenen Fehlerzustand zu überprüfen, bevor Sie ihn als Wert an den Statusparameter von RAISERROR weitergeben.  
  
```sql  
BEGIN TRY  
    -- RAISERROR with severity 11-19 will cause execution to   
    -- jump to the CATCH block.  
    RAISERROR ('Error raised in TRY block.', -- Message text.  
               16, -- Severity.  
               1 -- State.  
               );  
END TRY  
BEGIN CATCH  
    DECLARE @ErrorMessage NVARCHAR(4000);  
    DECLARE @ErrorSeverity INT;  
    DECLARE @ErrorState INT;  
  
    SELECT   
        @ErrorMessage = ERROR_MESSAGE(),  
        @ErrorSeverity = ERROR_SEVERITY(),  
        @ErrorState = ERROR_STATE();  
  
    -- Use RAISERROR inside the CATCH block to return error  
    -- information about the original error that caused  
    -- execution to jump to the CATCH block.  
    RAISERROR (@ErrorMessage, -- Message text.  
               @ErrorSeverity, -- Severity.  
               @ErrorState -- State.  
               );  
END CATCH;  
```  
  
### <a name="b-creating-an-ad-hoc-message-in-sysmessages"></a>B. Erstellen einer Ad-hoc-Meldung in sys.messages  
 Im folgenden Beispiel wird dargestellt, wie eine in der Katalogsicht „sys.messages“ gespeicherte Meldung ausgelöst wird. Die Meldung wurde mithilfe der gespeicherten Systemprozedur `sp_addmessage` zur Katalogsicht „sys.messages“ als Meldung mit der Meldungsnummer `50005` hinzugefügt.  
  
```sql  
sp_addmessage @msgnum = 50005,  
              @severity = 10,  
              @msgtext = N'<\<%7.3s>>';  
GO  
RAISERROR (50005, -- Message id.  
           10, -- Severity,  
           1, -- State,  
           N'abcde'); -- First argument supplies the string.  
-- The message text returned is: <<    abc>>.  
GO  
sp_dropmessage @msgnum = 50005;  
GO  
```  
  
### <a name="c-using-a-local-variable-to-supply-the-message-text"></a>C. Verwenden einer lokalen Variable zum Angeben des Meldungstexts  
 Im folgenden Codebeispiel wird die Verwendung einer lokalen Variablen zum Bereitstellen des Meldungstexts für eine `RAISERROR`-Anweisung dargestellt.  
  
```sql  
DECLARE @StringVariable NVARCHAR(50);  
SET @StringVariable = N'<\<%7.3s>>';  
  
RAISERROR (@StringVariable, -- Message text.  
           10, -- Severity,  
           1, -- State,  
           N'abcde'); -- First argument supplies the string.  
-- The message text returned is: <<    abc>>.  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integrierte Funktionen &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [PRINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/print-transact-sql.md)   
 [sp_addmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [sp_dropmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmessage-transact-sql.md)   
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [xp_logevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/xp-logevent-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)  
  
  

