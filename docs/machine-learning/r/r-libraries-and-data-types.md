---
title: Konvertieren von R- und SQL-Datentypen
description: Überprüfen Sie die implizite und explizite Konvertierung von Datentypen zwischen R und SQL Server in Data Science- und Machine Learning-Lösungen.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e024d65ed8a2dd9b55b7940fa19ed400a01af175
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348125"
---
# <a name="data-type-mappings-between-r-and-sql-server"></a>Zuordnungen von Datentypen zwischen R und SQL Server
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

In diesem Artikel werden die unterstützten Datentypen aufgelistet und die Datentypkonvertierungen ausgeführt, die für die Verwendung des Features zur R-Integration in SQL Server Machine Learning Services gelten.

## <a name="base-r-version"></a>R-Basisversion

SQL Server 2016 R Services und SQL Server Machine Learning Services mit R sind auf bestimmte Versionen von Microsoft R Open ausgerichtet. Die neueste Version, SQL Server 2019 Machine Learning Services, baut beispielsweise auf Microsoft R Open 3.5.2 auf.

Öffnen Sie **RGui** in der SQL-Instanz, um die einer bestimmten Instanz von SQL Server zugeordnete R-Version anzuzeigen. Beispielsweise lautet der Pfad für die Standardinstanz in SQL Server 2019: `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES\bin\x64\Rgui.exe`.

Das Tool lädt die R-Basisversion und andere Bibliotheken. Für jedes Paket, das beim Start der Sitzung geladen wird, werden in Form einer Benachrichtigung Informationen zur Version bereitgestellt.

## <a name="r-and-sql-data-types"></a>R- und SQL-Datentypen

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt mehrere Dutzend verschiedene Datentypen, hingegen weist R eine beschränkte Zahl von skalaren Datentypen (numerisch, ganzzahlig, komplex, logisch, Zeichen, Datum/Zeit und Rohdaten) auf. Daher ist es möglich, dass bei der Verwendung von Daten aus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in R-Skripts diese implizit in einen kompatiblen Datentyp konvertiert werden. Da eine exakte automatische Konvertierung jedoch häufig nicht möglich ist, wird ein Fehler zurückgegeben, z. B. "Unhandled SQL data type" (Unbehandelter SQL-Datentyp).

In diesem Abschnitt werden die bereitgestellten impliziten Konvertierungen sowie nicht unterstützte Datentypen aufgelistet. Außerdem werden einige Anleitungen für die Zuordnung von Datentypen zwischen R und SQL Server bereitgestellt.

## <a name="implicit-data-type-conversions"></a>Implizite Datentypkonvertierungen

Die folgende Tabelle zeigt die Änderungen der Datentypen und Werte an, wenn Daten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in einem R-Skript verwendet und anschließend an [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]zurückgegeben werden.

| SQL-Typ                         | R-Klasse     | Resultsettyp    | Kommentare            |
|----------------------------------|-------------|--------------------|---------------------|
| **bigint**                       | `numeric`   | **float**          | Bei der Ausführung eines R-Skripts mit `sp_execute_external_script` ist der bigint-Datentyp für Eingabedaten zulässig. Da diese jedoch in den numerischen Typ von R konvertiert werden, verlieren sehr hohe Werte oder Werte mit Dezimalstellen an Genauigkeit. R unterstützt nur ganze Zahlen bis zu 53 Bit. Höhere Werte verlieren an Genauigkeit.                                                                                         |
| **binary(n)**<br /> n <= 8000    | `raw`       | **varbinary(max)** | Nur als Eingabeparameter und als Ausgabe zulässig |
| **bit**                          | `logical`   | **bit**            |                     |
| **char(n)**<br /> n <= 8000      | `character` | **varchar(max)**   | Der Eingabedatenframe (input_data_1) wird ohne explizites Festlegen des Parameters *stringsAsFactors* erstellt, sodass der Spaltentyp von *default.stringsAsFactors()* in R abhängt.                                                                                                                                                                                                                                           |
| **datetime**                     | `POSIXct`   | **datetime**       | Dargestellt als GMT  |
| **date**                         | `POSIXct`   | **datetime**       | Dargestellt als GMT  |
| **decimal(p,s)**                 | `numeric`   | **float**          | Bei der Ausführung eines R-Skripts mit `sp_execute_external_script` ist der decimal-Datentyp für Eingabedaten zulässig. Da diese jedoch in den numerischen Typ von R konvertiert werden, verlieren sehr hohe Werte oder Werte mit Dezimalstellen an Genauigkeit. `sp_execute_external_script` mit einem R-Skript unterstützt nicht den vollständigen Bereich des Datentyps und würde die letzten paar Dezimalstellen ändern, insbesondere Bruchzahlen. |
| **float**                        | `numeric`   | **float**          |                     |
| **int**                          | `integer`   | **int**            |                     |
| **money**                        | `numeric`   | **float**          | Bei der Ausführung eines R-Skripts mit `sp_execute_external_script` ist der money-Datentyp für Eingabedaten zulässig. Da diese jedoch in den numerischen Typ von R konvertiert werden, verlieren sehr hohe Werte oder Werte mit Dezimalstellen an Genauigkeit. Manchmal könnten Cent-Werte ungenau sein, und es würde folgende Warnung ausgegeben: *Warning: unable to precisely represent cents values* (Warnung: Cent-Werte können nicht genau dargestellt werden).                                               |
| **numeric(p,s)**                 | `numeric`   | **float**          | Bei der Ausführung eines R-Skripts mit `sp_execute_external_script` ist der numeric-Datentyp für Eingabedaten zulässig. Da diese jedoch in den numerischen Typ von R konvertiert werden, verlieren sehr hohe Werte oder Werte mit Dezimalstellen an Genauigkeit. `sp_execute_external_script` mit einem R-Skript unterstützt nicht den vollständigen Bereich des Datentyps und würde die letzten paar Dezimalstellen ändern, insbesondere Bruchzahlen. |
| **real**                         | `numeric`   | **float**          |                     |
| **smalldatetime**                | `POSIXct`   | **datetime**       | Dargestellt als GMT  |
| **smallint**                     | `integer`   | **int**            |                     |
| **smallmoney**                   | `numeric`   | **float**          |                     |
| **tinyint**                      | `integer`   | **int**            |                     |
| **uniqueidentifier**             | `character` | **varchar(max)**   |                     |
| **varbinary(n)**<br /> n <= 8000 | `raw`       | **varbinary(max)** | Nur als Eingabeparameter und als Ausgabe zulässig |
| **varbinary(max)**               | `raw`       | **varbinary(max)** | Nur als Eingabeparameter und als Ausgabe zulässig |
| **varchar(n)**<br /> n <= 8000   | `character` | **varchar(max)**   | Der Eingabedatenframe (input_data_1) wird ohne explizites Festlegen des Parameters *stringsAsFactors* erstellt, sodass der Spaltentyp von *default.stringsAsFactors()* in R abhängt.                                                                                                                                                                                                                                           |

## <a name="data-types-not-supported-by-r"></a>Von R nicht unterstützte Datentypen

Folgende Typen von Datentypkategorien, die vom [Typsystem von SQL Server](../../t-sql/data-types/data-types-transact-sql.md) unterstützt werden, verursachen möglicherweise Probleme, wenn Sie an R-Code übergeben werden:

+ Im Abschnitt **Andere** des Artikels zum SQL-Typsystem aufgelistete Datentypen: **cursor**, **timestamp**, **hierarchyid**, **uniqueidentifier**, **sql_variant**, **xml** und **table**
+ Alle räumlichen Typen
+ **image**

## <a name="data-types-that-might-convert-poorly"></a>Datentypen, die möglicherweise schlecht konvertiert werden

+ Die meisten Datum-/Zeittypen sollten funktionieren; nur **dattimeoffset** funktioniert nicht.
+ Die meisten numerischen Datentypen werden unterstützt; allerdings schlagen Konvertierungen von **money** und **smallmoney** möglicherweise fehl.
+ **varchar** wird unterstützt; da jedoch SQL Server grundsätzlich Unicode verwendet, wird empfohlen, wenn möglich **nvarchar** und andere Unicode-Textdatentypen zu verwenden.
+ Funktionen aus der RevoScaleR-Bibliothek können mit dem Präfix „rx“ versehen werden, um die binären SQL-Datentypen (**binary** und **varbinary**) zu behandeln; in den meisten Szenarios ist für diese Typen jedoch eine spezielle Behandlung vonnöten. In den meisten Fällen funktioniert R-Code nicht mit binären Spalten.

  
 Weitere Informationen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datentypen finden Sie unter [Datentypen &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)

## <a name="changes-in-data-types-between-sql-server-versions"></a>Änderungen an den Datentypen zwischen den verschiedenen SQL Server-Versionen

Microsoft SQL Server 2016 und höher enthalten Verbesserungen bei der Datentypkonvertierung und verschiedenen anderen Vorgängen. Die meisten dieser Verbesserungen tragen zu einer verbesserten Genauigkeit beim Arbeiten mit Gleitkommatypen bei; außerdem gibt es kleine Änderungen bei Vorgängen mit herkömmlichen **datetime**-Typen.

Diese Verbesserungen stehen Ihnen standardmäßig zur Verfügung, wenn Sie einen Datenbankkompatibilitätsgrad von 130 oder höher verwenden. Wenn Sie allerdings einen anderen Kompatibilitätsgrad verwenden oder über eine ältere Version mit der Datenbank verbunden sind, sehen Sie möglicherweise Unterschiede in der Genauigkeit von Zahlen oder anderen Ergebnissen. 

Weitere Informationen finden Sie unter [SQL Server 2016 improvements in handling some data types and uncommon operations (Verbesserungen der Behandlung einiger Datentypen und seltener Vorgänge in SQL Server 2016)](https://support.microsoft.com/help/4010261/sql-server-2016-improvements-in-handling-some-data-types-and-uncommon-).
 

## <a name="verify-r-and-sql-data-schemas-in-advance"></a>Überprüfen R- und SQL-Datenschemas im Voraus

Im Allgemeinen ist es empfehlenswert, die  `str()` -Funktion zu verwenden, um die interne Struktur und den Typ des R-Objekts zu erhalten, wenn Sie nicht wissen, wie ein spezieller Datentyp oder eine Datenstruktur in R verwendet wird. Das Ergebnis der Funktion wird in die R-Konsole gedruckt, und ist auch in den Abfrageergebnissen verfügbar, in der Registerkarte **Meldungen** in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. 

Wenn Sie Daten aus einer Datenbank abrufen, um diese in R-Code zu verwenden, sollte Sie in jedem Fall die Spalten entfernen, die nicht in R verwendet werden können. Ebenso sollten Sie die Spalten entfernen, die keinen Nutzen für Ihre Analyse haben, z. B. GUIDs (uniqueidentifier), Zeitstempel und andere Spalten zum Überwachen, oder von ETL-Prozessen erstellte Informationen bezüglich der Datenherkunft. 

Beachten Sie, dass die Leistung von R-Code durch unnötige Spalten deutlich beeinträchtigt werden kann, besonders dann, wenn Spalten mit hoher Kardinalität als Faktoren verwendet werden. Aus diesem Grund wird empfohlen, im System gespeicherte Prozeduren von SQL Server und Informationsansichten zu verwenden, um die Datentypen für eine angegebene Tabelle im Voraus abzurufen und inkompatible Spalten entweder zu löschen oder zu konvertieren. Weitere Informationen finden Sie unter [Information Schema Views in Transact-SQL (Ansichten des Informationsschemas in Transact-SQL)](../../relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)

Falls ein bestimmter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datentyp in R nicht unterstützt wird, Sie aber die Datenspalten im R-Skript verwenden müssen, empfehlen wir Ihnen, die Funktionen [„Cast“ und „Convert“ &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md) zu verwenden, um sicherzustellen, dass die Datentypenkonvertierungen wie gewünscht ausgeführt werden, bevor Sie die Daten in Ihrem R-Skript verwenden.  

> [!WARNING]
> Wenn Sie **rxDataStep** verwenden, um inkompatible Spalten beim Verschieben von Daten fallen zu lassen, denken Sie daran, dass die Argumente _varsToKeep_ und _varsToDrop_ für den Datenquelltyp **RxSqlServerData** nicht unterstützt werden.


## <a name="examples"></a>Beispiele

### <a name="example-1-implicit-conversion"></a>Beispiel 1: Implizite Konvertierung

In folgendem Beispiel wird veranschaulicht, wie Daten auf dem Weg zwischen SQL Server und R umgewandelt werden.

Die Abfrage ruft eine Reihe von Werten aus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Tabelle ab und verwendet die gespeicherte Prozedur [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), um die Werte mithilfe der R-Runtime auszugeben.

```sql
CREATE TABLE MyTable (    
 c1 int,    
 c2 varchar(10),    
 c3 uniqueidentifier    
);    
go    
INSERT MyTable VALUES(1, 'Hello', newid());    
INSERT MyTable VALUES(-11, 'world', newid());    
SELECT * FROM MyTable;    
  
EXECUTE sp_execute_external_script    
 @language = N'R'    
 , @script = N'    
inputDataSet["cR"] <- c(4, 2)    
str(inputDataSet)    
outputDataSet <- inputDataSet'    
 , @input_data_1 = N'SELECT c1, c2, c3 FROM MyTable'    
 , @input_data_1_name = N'inputDataSet'    
 , @output_data_1_name = N'outputDataSet'    
 WITH RESULT SETS((C1 int, C2 varchar(max), C3 varchar(max), C4 float));  
```

**Ergebnisse**

|Zeile \#|C1|C2|C3|C4|
|-|-|-|-|-|
|1|1|Hallo|6e225611-4b58-4995-a0a5-554d19012ef1|4|
|2|-11|world|6732ea46-2d5d-430b-8ao1-86e7f3351c3e|2|

Beachten Sie die Verwendung der `str` -Funktion in R, um das Schema der Ausgabedaten zu erhalten. Diese Funktion gibt die folgenden Informationen zurück:

```output
'data.frame':2 obs. of  4 variables:
 $ c1: int  1 -11
 $ c2: Factor w/ 2 levels "Hello","world": 1 2
 $ c3: Factor w/ 2 levels "6732EA46-2D5D-430B-8A01-86E7F3351C3E",..: 2 1
 $ cR: num  4 2
```

Daraus können Sie erkennen, dass die folgenden Datentypenkonvertierungen implizit als Teil der Abfrage durchgeführt wurden:

+ **Spalte C1**. Die Spalte wird in **ssNoversion** als [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], in R als `integer` und im Ausgaberesultset als **ssNoversion** dargestellt.  
  
  Es wurde keine Typenkonvertierung durchgeführt.  
  
+ **Spalte C2**. Die Spalte wird in **ssNoversion** als [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], in R als `factor` und im Ausgaberesultset als **varchar(max)** dargestellt.  
  
  Beachten Sie, wie sich die Ausgabe ändert; jede Zeichenfolge von R (entweder ein Faktor oder eine reguläre Zeichenfolge) wird als **varchar(max)** dargestellt, unabhängig von der Länge der Zeichenfolge.  
  
+ **Spalte C3**.  Die Spalte wird in **ssNoversion** als [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], in R als `character` und im Ausgaberesultset als **varchar(max)** dargestellt.
  
  Beachten Sie die ausgeführte Datentypenkonvertierung. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt den **ssNoversion** , R jedoch nicht; daher werden die Bezeichner als Zeichenfolgen dargestellt.
  
+ **Spalte C4**. Die Spalte enthält Werte, die vom R-Skript generiert wurden und die in den ursprünglichen Daten nicht vorhanden waren.


## <a name="example-2-dynamic-column-selection-using-r"></a>Beispiel 2: Dynamische Spaltenauswahl mithilfe von R

Im folgenden Beispiel wird veranschaulicht, wie Sie R-Code zum Prüfen auf ungültige Spaltentypen verwenden können. Der R-Code ruft das Schema der angegebenen Tabelle mithilfe der Systemsichten von SQL Server ab und entfernt alle Spalten, die einen angegebenen ungültigen Typen aufweisen.

```R
connStr <- "Server=.;Database=TestDB;Trusted_Connection=Yes"
data <- RxSqlServerData(connectionString = connStr, sqlQuery = "SELECT COLUMN_NAME FROM TestDB.INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = N'testdata' AND DATA_TYPE <> 'image';")
columns <- rxImport(data)
columnList <- do.call(paste, c(as.list(columns$COLUMN_NAME), sep = ","))
sqlQuery <- paste("SELECT", columnList, "FROM testdata")
```

## <a name="see-also"></a>Weitere Informationen

+ [Zuordnungen von Datentypen zwischen Python und SQL Server](../python/python-libraries-and-data-types.md)
