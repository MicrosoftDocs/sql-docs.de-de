---
title: Enthaltene Datenbanksortierungen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie die Sortierung in eigenständigen und nicht eigenständigen Datenbanken funktioniert. Außerdem werden Probleme vorgestellt, die auftreten können, wenn Sitzungen zwischen eigenständigen und nicht eigenständigen Kontexten wechseln.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- contained database, collations
ms.assetid: 4b44f6b9-2359-452f-8bb1-5520f2528483
author: stevestein
ms.author: sstein
ms.openlocfilehash: 260c4afa90b28aa8efc22a15fbda13cfa7f35417
ms.sourcegitcommit: 98acedd435aecfda1b3c4c23d3f0c3c1a12682a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102532322"
---
# <a name="contained-database-collations"></a>Enthaltene Datenbanksortierungen
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Auf die Sortierreihenfolge und die Gleichheitssemantik von Textdaten wirken sich verschiedene Eigenschaften aus, u. a. die Berücksichtigung der Groß- und Kleinschreibung, die Berücksichtigung von Akzenten sowie die verwendete Basissprache. Diese Eigenschaften werden für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] durch die ausgewählte Sortierung der Daten ausgedrückt. Eine ausführliche Erläuterung zu Sortierungen finden Sie unter [Sortierung und Unicode-Unterstützung](../../relational-databases/collations/collation-and-unicode-support.md).  
  
 Sortierungen gelten nicht nur für in Benutzertabellen gespeicherte Daten, sondern für jeden von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] behandelten Text, einschließlich Metadaten, temporäre Objekte, Variablennamen usw. Deren Behandlung variiert in eigenständigen und abhängigen Datenbanken Diese Änderung wirkt sich nicht auf viele Benutzer aus. Stattdessen trägt sie zu Unabhängigkeit von Instanzen und Einheitlichkeit bei. Dies verursacht jedoch möglicherweise auch etwas Verwirrung sowie Probleme bei Sitzungen, in denen sowohl auf enthaltene als auch auf nicht enthaltene Datenbanken zugegriffen wird.  
  
 In diesem Thema wird das Wesen der Änderung erläutert. Zudem werden einige Bereiche beschrieben, in denen sie möglicherweise Probleme verursacht.  
  
## <a name="non-contained-databases"></a>Abhängige Datenbanken  
 Alle Datenbanken weisen eine Standardsortierung auf, die beim Erstellen oder Ändern einer Datenbank festgelegt werden kann. Diese Sortierung wird für sämtliche Metadaten in der Datenbank sowie als Standard für alle Zeichenfolgenspalten in der Datenbank verwendet. Benutzer können mit der **COLLATE** -Klausel für jede einzelne Spalte eine andere Sortierung auswählen.  
  
### <a name="example-1"></a>Beispiel 1  
 Wenn Sie z. B. in Peking arbeiten, kann eine chinesische Sortierung verwendet werden:  
  
```sql  
ALTER DATABASE MyDB COLLATE Chinese_Simplified_Pinyin_100_CI_AS;  
```  
  
 Wenn nun eine Spalte erstellt wird, ist deren Standardsortierung diese chinesische Sortierung. Gegebenenfalls kann jedoch eine andere Sortierung ausgewählt werden:  
  
```sql  
CREATE TABLE MyTable  
      (mycolumn1 nvarchar,  
      mycolumn2 nvarchar COLLATE Frisian_100_CS_AS);  
GO  
SELECT name, collation_name  
FROM sys.columns  
WHERE name LIKE 'mycolumn%' ;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```sql  
name            collation_name  
--------------- ----------------------------------  
mycolumn1       Chinese_Simplified_Pinyin_100_CI_AS  
mycolumn2       Frisian_100_CS_AS  
```  
  
 Dies erscheint relativ einfach, es treten jedoch mehrere Probleme auf. Da die Sortierung für eine Spalte von der Datenbank abhängt, in der die Tabelle erstellt wurde, treten Probleme mit der Verwendung von temporären Tabellen auf, die in **tempdb** gespeichert werden. Die Sortierung von **tempdb** entspricht normalerweise der Sortierung für die Instanz. Diese muss nicht unbedingt der Datenbanksortierung entsprechen.  
  
### <a name="example-2"></a>Beispiel 2  
 Betrachten Sie beispielsweise die obige (chinesische) Datenbank, wenn diese in einer Instanz mit der Sortierung **Latin1_General** verwendet wird:  
  
```sql  
CREATE TABLE T1 (T1_txt nvarchar(max)) ;  
GO  
CREATE TABLE #T2 (T2_txt nvarchar(max)) ;  
GO  
```  
  
 Auf den ersten Blick weisen diese beiden Tabellen dasselbe Schema auf. Da sich aber die Sortierungen der Datenbanken unterscheiden, sind die Werte tatsächlich nicht kompatibel:  
  
```  
SELECT T1_txt, T2_txt  
FROM T1   
JOIN #T2   
    ON T1.T1_txt = #T2.T2_txt  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 Meldung 468, Ebene 16, Status 9, Zeile 2  
  
 Ein Sortierungskonflikt zwischen "Latin1_General_100_CI_AS_KS_WS_SC" und "Chinese_Simplified_Pinyin_100_CI_AS" im Equal To-Vorgang kann nicht aufgelöst werden.  
  
 Dies kann durch das explizite Sortieren der temporären Tabelle korrigiert werden. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erleichtert diesen Vorgang etwas, indem das **DATABASE_DEFAULT** -Schlüsselwort für die **COLLATE** -Klausel bereitgestellt wird.  
  
```sql  
CREATE TABLE T1 (T1_txt nvarchar(max)) ;  
GO  
CREATE TABLE #T2 (T2_txt nvarchar(max) COLLATE DATABASE_DEFAULT);  
GO  
SELECT T1_txt, T2_txt  
FROM T1   
JOIN #T2   
    ON T1.T1_txt = #T2.T2_txt ;  
```  
  
 Die Ausführung erfolgt nun ohne Fehler.  
  
 Das sortierungsabhängige Verhalten ist auch bei Variablen zu beobachten. Betrachten Sie die folgende Funktion:  
  
```  
CREATE FUNCTION f(@x INT) RETURNS INT  
AS BEGIN   
      DECLARE @I INT = 1  
      DECLARE @İ INT = 2  
      RETURN @x * @i  
END;  
```  
  
 Dies ist eine relativ spezielle Funktion. In einer Sortierung mit Berücksichtigung der Groß-/Kleinschreibung kann für das @i in der Rückgabeklausel keine Bindung an @I oder @İ hergestellt werden. Bei der Sortierung „Latin1_General“ ohne Berücksichtigung der Groß-/Kleinschreibung wird @i an @I gebunden, und die Funktion gibt 1 zurück. Bei der Sortierung „Turkish“ ohne Berücksichtigung der Groß-/Kleinschreibung wird jedoch @i an @İ gebunden, und die Funktion gibt 2 zurück. Dies kann erhebliche Beschädigungen in einer Datenbank verursachen, bei der zwischen Instanzen mit unterschiedlichen Sortierungen gewechselt wird.  
  
## <a name="contained-databases"></a>Eigenständige Datenbanken  
 Da eines der Entwurfsziele bei eigenständigen Datenbanken darin besteht, diese in sich abgeschlossen einzurichten, muss die Abhängigkeit von Instanzen und **tempdb** -Sortierungen abgetrennt werden. Hierzu wurde für eigenständige Datenbanken das Konzept der Katalogsortierung eingeführt. Die Katalogsortierung wird für Systemmetadaten und vorübergehende Objekte verwendet. Einzelheiten hierzu finden Sie weiter unten.  
  
 In einer eigenständigen Datenbank ist die Katalogsortierung **Latin1_General_100_CI_AS_WS_KS_SC**. Diese Sortierung ist für alle eigenständigen Datenbanken in allen Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identisch und kann nicht geändert werden.  
  
 Die Datenbanksortierung wird beibehalten, sie wird jedoch nur für Benutzerdaten als Standardsortierung verwendet. Standardmäßig entspricht die Datenbanksortierung der model-Datenbanksortierung, sie kann jedoch vom Benutzer wie bei abhängigen Datenbanken durch einen **CREATE** - oder **ALTER DATABASE** -Befehl geändert werden.  
  
 Ein neues Schlüsselwort, **CATALOG_DEFAULT**, steht in den **COLLATE** -Klausel zur Verfügung. Diese wird als Verknüpfung zur aktuellen Sortierung der Metadaten in enthaltenen und nicht enthaltenen Datenbanken verwendet. Das heißt, in einer nicht enthaltenen Datenbank gibt **CATALOG_DEFAULT** die aktuelle Datenbanksortierung zurück, da Metadaten in der Datenbanksortierung sortiert werden. In einer enthaltenen Datenbank können sich diese zwei Werte unterscheiden, da der Benutzer die Datenbanksortierung ändern kann, sodass sie von der Katalogsortierung abweicht.  
  
 Das Verhalten verschiedener Objekte in nicht enthaltenen und enthaltenen Datenbanken wird in dieser Tabelle zusammengefasst:  
  
|Element|Nicht enthaltene Datenbank|Enthaltene Datenbank|  
|-|-|-|  
|Benutzerdaten (Standard)|DATABASE_DEFAULT|DATABASE_DEFAULT|  
|Temp-Daten (Standard)|TempDB-Sortierung|DATABASE_DEFAULT|  
|Metadaten|DATABASE_DEFAULT/CATALOG_DEFAULT|CATALOG_DEFAULT|  
|Temporäre Metadaten|TempDB-Sortierung|CATALOG_DEFAULT|  
|Variables|Instanzsortierung|CATALOG_DEFAULT|  
|Goto-Bezeichnungen|Instanzsortierung|CATALOG_DEFAULT|  
|Cursornamen|Instanzsortierung|CATALOG_DEFAULT|  
  
 Im zuvor beschriebenen Beispiel für eine temporäre Tabelle ist ersichtlich, dass dieses Sortierverhalten bei den meisten Verwendungen temporärer Tabellen eine explizite **COLLATE**-Klausel überflüssig macht. In einer enthaltenen Datenbank wird dieser Code nun ohne Fehler ausgeführt, selbst wenn sich die Datenbanksortierung und die Instanzsortierung unterscheiden:  
  
```sql  
CREATE TABLE T1 (T1_txt nvarchar(max)) ;  
GO  
CREATE TABLE #T2 (T2_txt nvarchar(max));  
GO  
SELECT T1_txt, T2_txt  
FROM T1   
JOIN #T2   
    ON T1.T1_txt = #T2.T2_txt ;  
```  
  
 Dies funktioniert, da sowohl `T1_txt` als auch `T2_txt` in der Datenbanksortierung der enthaltenen Datenbank sortiert werden.  
  
## <a name="crossing-between-contained-and-uncontained-contexts"></a>Wechseln zwischen einem enthaltenen und einem nicht enthaltenen Kontext  
 Solange sich eine Sitzung auf eine enthaltene Datenbank beschränkt, darf die Datenbank nicht verlassen werden, mit der eine Verbindung besteht. In diesem Fall ist das Verhalten sehr einfach. Wenn jedoch in einer Sitzung zwischen einem enthaltenen und einem nicht enthaltenen Kontext gewechselt wird, ist das Verhalten komplexer, da die beiden Regelsätze überbrückt werden müssen. Dies kann in einer teilweise enthaltenen Datenbank der Fall sein, da ein Benutzer mit **USE** auf eine andere Datenbank zugreifen kann. In diesem Fall werden die Unterschiede zwischen den Sortierungsregeln gemäß dem folgenden Prinzip behandelt.  
  
-   Das Sortierungsverhalten für einen Batch wird von der Datenbank bestimmt, in der der Batch beginnt.  
  
 Beachten Sie, dass diese Entscheidung getroffen wird, bevor Befehle ausgegeben werden (auch der anfängliche **USE**-Befehl). Das heißt, wenn ein Batch in einer eigenständigen Datenbank beginnt, der erste Befehl jedoch ein **USE** -Befehl für eine abhängige Datenbank ist, wird dennoch das Sortierungsverhalten der eigenständigen Datenbank für den Batch verwendet. Angesichts dessen kann beispielsweise ein Verweis auf eine Variable mehrere mögliche Ergebnisse haben:  
  
-   Durch den Verweis kann genau eine Übereinstimmung gefunden werden. In diesem Fall funktioniert der Verweis ohne Fehler.  
  
-   Durch den Verweis wird möglicherweise keine Übereinstimmung in der aktuellen Sortierung gefunden, obwohl zuvor eine vorhanden war. Dadurch wird ein Fehler ausgelöst, der angibt, dass die Variable nicht vorhanden ist, auch wenn diese offensichtlich erstellt wurde.  
  
-   Durch den Verweis können mehrere Übereinstimmungen gefunden werden, die sich ursprünglich voneinander unterschieden haben. Auch hierdurch wird ein Fehler ausgelöst.  
  
 Dies wird im Folgenden anhand einiger Beispiele veranschaulicht. Dabei wird angenommen, dass eine teilweise eigenständige Datenbank mit dem Namen `MyCDB` vorhanden ist, deren Datenbanksortierung auf die Standardsortierung **Latin1_General_100_CI_AS_WS_KS_SC** festgelegt ist. Wir nehmen an, dass die instanzsortierung **Latin1_General_100_CS_AS_WS_KS_SC** ist. Die beiden Sortierungen unterscheiden sich nur hinsichtlich der Berücksichtigung der Groß- und Kleinschreibung.  
  
### <a name="example-1"></a>Beispiel 1  
 Im folgenden Beispiel wird der Fall veranschaulicht, bei dem durch den Verweis genau eine Übereinstimmung gefunden wird.  
  
```  
USE MyCDB;  
GO  
  
CREATE TABLE #a(x int);  
INSERT INTO #a VALUES(1);  
GO  
  
USE master;  
GO  
  
SELECT * FROM #a;  
GO  
  
Results:  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
x  
-----------  
1  
```  
  
 In diesem Fall wird vom erkannten #a eine Bindung sowohl mit der Katalogsortierung ohne Berücksichtigung der Groß- und Kleinschreibung als auch mit der Instanzsortierung mit Berücksichtigung der Groß- und Kleinschreibung hergestellt, und der Code wird fehlerfrei ausgeführt.  
  
### <a name="example-2"></a>Beispiel 2  
 Im folgenden Beispiel wird der Fall veranschaulicht, in dem durch den Verweis keine Übereinstimmung in der aktuellen Sortierung gefunden wird, wo zuvor eine Übereinstimmung vorhanden war.  
  
```  
USE MyCDB;  
GO  
  
CREATE TABLE #a(x int);  
INSERT INTO #A VALUES(1);  
GO  
```  
  
 Hier wird eine Bindung von "#A" an "#a" in der Standardsortierung ohne Berücksichtigung der Groß- und Kleinschreibung hergestellt, und die Einfügung funktioniert.  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
(1 row(s) affected)  
```  
  
 Wenn jedoch das Skript fortgesetzt wird...  
  
```  
USE master;  
GO  
  
SELECT * FROM #A;  
GO  
```  
  
 Beim Versuch, eine Bindung an #A in der Instanzsortierung mit Berücksichtigung der Groß- und Kleinschreibung herzustellen, wird ein Fehler ausgegeben:  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 Meldung 208, Ebene 16, Status 0, Zeile 2  
  
 Ungültiger Objektname '#A'.  
  
### <a name="example-3"></a>Beispiel 3  
 Im folgenden Beispiel wird der Fall veranschaulicht, wo durch den Verweis mehrere Übereinstimmungen gefunden werden, die sich ursprünglich voneinander unterschieden haben. Zunächst wird in **tempdb** begonnen (die dieselbe Sortierung mit Berücksichtigung von Groß-/Kleinschreibung aufweist wie die gegebene Instanz), und folgende Anweisungen werden ausgeführt.  
  
```  
USE tempdb;  
GO  
  
CREATE TABLE #a(x int);  
GO  
CREATE TABLE #A(x int);  
GO  
INSERT INTO #a VALUES(1);  
GO  
INSERT INTO #A VALUES(2);  
GO  
```  
  
 Diese Ausführung ist erfolgreich, da die Tabellen in dieser Sortierung eindeutig sind:  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
(1 row(s) affected)  
(1 row(s) affected)  
```  
  
 Beim Wechseln in die enthaltene Datenbank wird jedoch festgestellt, dass keine Bindungen an diese Tabellen hergestellt werden können.  
  
```  
USE MyCDB;  
GO  
SELECT * FROM #a;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 Meldung 12800, Ebene 16, Status 1, Zeile 2  
  
 Der Verweis auf den Namen '#a' der temporären Tabelle ist mehrdeutig und kann nicht aufgelöst werden. Verwenden Sie entweder '#a' oder '#A'.  
  
## <a name="conclusion"></a>Zusammenfassung  
 Das Sortierungsverhalten enthaltener Datenbanken unterscheidet sich leicht von dem nicht enthaltener Datenbanken. Dieses Verhalten ist im Allgemeinen vorteilhaft und trägt zu Unabhängigkeit von Instanzen sowie Einfachheit bei. Für einige Benutzer können Probleme auftreten, insbesondere dann, wenn in einer Sitzung sowohl auf enthaltene als auch auf nicht enthaltene Datenbanken zugegriffen wird.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Eigenständige Datenbanken](../../relational-databases/databases/contained-databases.md)  
  
  
