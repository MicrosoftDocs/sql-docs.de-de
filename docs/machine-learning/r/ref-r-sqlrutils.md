---
title: sqlrutils-Hilfsfunktionen
description: sqlrutils ist ein R-Paket von Microsoft, das einen Mechanismus bietet, mit dem R-Benutzer ihre R-Skripts in eine gespeicherte T-SQL-Prozedur einbinden, diese gespeicherte Prozedur bei einer Datenbank registrieren und die gespeicherte Prozedur in einer R-Entwicklungsumgebung ausführen können. Das Paket ist in SQL Server Machine Learning Services und SQL Server 2016 R Services enthalten.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 874c3175b830cbcdb5805c5bdb06a747015ff889
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273011"
---
# <a name="sqlrutils-r-package-in-sql-server-machine-learning-services"></a>sqlrutils (R-Paket in SQL Server Machine Learning Services)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

**sqlrutils** ist ein R-Paket von Microsoft, das einen Mechanismus bietet, mit dem R-Benutzer ihre R-Skripts in eine gespeicherte T-SQL-Prozedur einbinden, diese gespeicherte Prozedur bei einer Datenbank registrieren und die gespeicherte Prozedur in einer R-Entwicklungsumgebung ausführen können. Das Paket ist in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) und [SQL Server 2016 R Services](sql-server-r-services.md) enthalten.

Durch Konvertieren Ihres R-Codes, sodass er in einer einzelnen gespeicherten Prozedur ausgeführt wird, können Sie SQL Server R Services effektiver nutzen, wozu es erforderlich ist, dass das R-Skript als Parameter für [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)eingebettet wird. Das **sqlrutils** -Paket unterstützt Sie dabei, dieses eingebettete R-Skript zu erstellen und zugehörige Parameter entsprechend festzulegen.

Das **sqlrutils** -Paket führt die folgenden Aufgaben aus:

- Es speichert das generierte T-SQL-Skript als Zeichenfolge in einer R-Datenstruktur.
- Es generiert optional eine SQL-Datei für das T-SQL-Skript, die Sie bearbeiten oder ausführen können, um eine gespeicherte Prozedur zu erstellen.
- Es registriert die neu erstellte gespeicherte Prozedur für die SQL Server-Instanz aus Ihrer R-Entwicklungsumgebung.

Sie können die gespeicherte Prozedur auch aus einer R-Umgebung ausführen, indem Sie wohlgeformte Parameter übergeben und die Ergebnisse verarbeiten. Oder Sie können die gespeicherte Prozedur aus SQL Server verwenden, um allgemeine Datenbankintegrationsszenarios wie ETL, Modelltraining und Massenbewertung zu unterstützen.

> [!NOTE]
> Wenn Sie beabsichtigen, die gespeicherte Prozedur aus einer R-Umgebung auszuführen, indem Sie die *executeStoredProcedure* -Funktion aufrufen, müssen Sie einen ODBC 3.8-Anbieter verwenden, z.B. ODBC Driver 13 for SQL Server.  
  
## <a name="full-reference-documentation"></a>Vollständige Referenzdokumentation

Das **sqlrutils**-Paket wird in mehreren Microsoft-Produkten bereitgestellt. Die Verwendung ist jedoch immer identisch, unabhängig davon, ob Sie das Paket in SQL Server oder einem anderen Produkt abrufen. Aus diesem Grund wird die [Dokumentation für einzelne sqlrutils-Funktionen](/machine-learning-server/r-reference/revoscaler/revoscaler) nur an einer Stelle in der [R-Referenz](/machine-learning-server/r-reference/introducing-r-server-r-package-reference) für Microsoft Machine Learning Server veröffentlicht. Abweichungen durch produktspezifisches Verhalten finden Sie ggf. auf der Hilfeseite der Funktion.

## <a name="functions-list"></a>Funktionsliste

Der folgende Abschnitt bietet eine Übersicht über die Funktionen, die Sie aus dem **sqlrutils**-Paket aufrufen können, um eine gespeicherte Prozedur mit eingebettetem R-Code zu entwickeln. Weitere Informationen über die Parameter für jede Methode oder Funktion finden Sie in der R-Hilfe für das Paket: `help(package="sqlrutils")`

|Funktion | BESCHREIBUNG |
|------|-------------|
|[executeStoredProcedure](/machine-learning-server/r-reference/sqlrutils/executestoredprocedure)| Führt eine gespeicherte Prozedur aus.|
|[getInputParameters](/machine-learning-server/r-reference/sqlrutils/getinputparameters)| Ruft eine Liste der Eingabeparameter in der gespeicherten Prozedur ab.| 
|[InputData](/machine-learning-server/r-reference/sqlrutils/inputdata)| Definiert die Datenquelle in SQL Server, die in dem R-Datenrahmen verwendet wird. Sie geben den Namen des Datenrahmens (data.frame), in dem die Eingabedaten gespeichert werden sollen, und eine Abfrage, mit der die Daten abgerufen werden, oder einen Standardwert an. Es werden nur einfache SELECT-Abfragen unterstützt. | 
|[InputParameter](/machine-learning-server/r-reference/sqlrutils/inputparameter)| Definiert einen einzelnen Eingabeparameter, der in das T-SQL-Skript eingebettet wird. Sie müssen den Namen des Parameters und dessen R-Datentyp angeben.| 
|[OutputData](/machine-learning-server/r-reference/sqlrutils/outputdata)| Generiert ein Zwischendatenobjekt, das erforderlich ist, wenn Ihre R-Funktion eine Liste zurückgibt, die einen Datenrahmen (data.frame) enthält. Das *OutputData* -Objekt wird verwendet, um den Namen eines einzelnen „data.frame“ zu speichern, das aus der Liste abgerufen wurde.| 
|[OutputParameter](/machine-learning-server/r-reference/sqlrutils/outputparameter) | Generiert ein Zwischendatenobjekt, das erforderlich ist, wenn Ihre R-Funktion eine Liste zurückgibt. Im *OutputParameter* -Objekt werden der Name und der Datentyp eines einzelnen Elements der Liste gespeichert, wobei vorausgesetzt wird, dass das Element **kein** Datenrahmen ist. |
|[registerStoredProcedure](/machine-learning-server/r-reference/sqlrutils/registerstoredprocedure) | Registriert die gespeicherte Prozedur bei einer Datenbank.|
|[setInputDataQuery](/machine-learning-server/r-reference/sqlrutils/setinputdataquery)| Weist einem Eingabedatenparameter der gespeicherten Prozedur eine Abfrage zu.| 
|[setInputParameterValue](/machine-learning-server/r-reference/sqlrutils/setinputparametervalue)| Weist einem Eingabeparameter der gespeicherten Prozedur einen Wert zu.| 
|[StoredProcedure](/machine-learning-server/r-reference/sqlrutils/storedprocedure)| Ein gespeichertes Prozedurobjekt.|


## <a name="how-to-use-sqlrutils"></a>Verwenden von sqlrutils

Die Funktionen im **sqlrutils**-Paket müssen auf einem Computer mit SQL Server Machine Learning mit R ausgeführt werden. Wenn Sie auf einer Clientarbeitsstation arbeiten, legen Sie einen Remotecomputekontext fest, um die Ausführung auf die SQL Server-Instanz zu verschieben. Der Workflow für die Verwendung dieses Pakets umfasst die folgenden Schritte:

+ Definieren der Parameter (Eingaben, Ausgaben oder beides) einer gespeicherten Prozedur 
+ Generieren und Registrieren der gespeicherten Prozedur    
+ Ausführen der gespeicherten Prozedur  

Laden Sie in einer R-Sitzung **sqlrutils** über die Befehlszeile, indem Sie `library(sqlrutils)` eingeben.

> [!Note]
> Sie können dieses Paket auf einen Computer ohne SQL Server laden (z. B. auf eine R-Clientinstanz), wenn Sie den Computekontext in SQL Server ändern und den Code in diesem Computekontext ausführen.


### <a name="define-stored-procedure-parameters-and-inputs"></a>Definieren der Parameter und Eingaben einer gespeicherten Prozedur

`StoredProcedure` ist der Hauptkonstruktor, der zum Erstellen der gespeicherten Prozedur verwendet wird. Dieser Konstruktor generiert eine gespeicherte Prozedur als *SQL Server-Objekt* und erstellt optional eine Textdatei, die eine Abfrage enthält, die dazu verwendet werden kann, die gespeicherte Prozedur mit einem T-SQL-Befehl zu generieren. 

Außerdem kann die *StoredProcedure* -Funktion dazu verwendet werden, die gespeicherte Prozedur für die angegebene Instanz und Datenbank zu registrieren.

+ Verwenden Sie das `func` Argument, um eine gültige R-Funktion anzugeben. Alle Variablen, die in der Funktion verwendet werden, müssen entweder innerhalb der Funktion definiert sein oder als Eingabeparameter bereitgestellt werden. Diese Parameter können maximal einen Datenrahmen enthalten.

+ Die R-Funktion muss entweder einen Datenrahmen, eine benannte Liste oder NULL zurückgeben. Gibt die Funktion eine Liste zurück, darf die Liste maximal einen Datenrahmen (data.frame) enthalten.

+ Verwenden Sie das Argument `spName` , um den Namen der zu erstellenden gespeicherten Prozedur anzugeben.

+ Sie können optionale Eingabe- und Ausgabeparameter mithilfe der Objekte übergeben, die mit diesen Hilfsfunktionen erstellt wurden: `setInputData`, `setInputParameter`und `setOutputParameter`.

+  Verwenden Sie optional `filePath` , um den Pfad und den Namen der zu erstellenden SQL-Datei bereitzustellen. Sie können diese Datei für die SQL Server-Instanz ausführen, um die gespeicherte Prozedur mit T-SQL zu generieren.

+ Um den Server und die Datenbank festzulegen, in der die gespeicherte Prozedur gespeichert werden soll, verwenden Sie die Argumente `dbName` und  `connectionString`.

+ Wenn Sie eine Liste des *InputData* - und des *InputParameter* -Objekts abrufen möchten, die zum Erstellen eines bestimmten *StoredProcedure* -Objekts verwendet wurden, rufen Sie `getInputParameters`auf. 

+ Um die gespeicherte Prozedur für die angegebene Datenbank zu registrieren, verwenden Sie `registerStoredProcedure`.

Dem Objekt für die gespeicherte Prozedur sind üblicherweise weder Daten noch Werte zugeordnet, es sei denn, es wurde ein Standardwert angegeben. Daten werden erst abgerufen, wenn die gespeicherte Prozedur ausgeführt wird. 

### <a name="specify-inputs-and-execute"></a>Angeben von Eingaben und Ausführen

+ Verwenden Sie `setInputDataQuery` , um einem *InputParameter* -Objekt eine Abfrage zuzuweisen. Wenn Sie beispielsweise ein Objekt für die gespeicherte Prozedur in R erstellt haben, können Sie `setInputDataQuery` verwenden, um Argumente an die *StoredProcedure* -Funktion zu übergeben, damit die gespeicherte Prozedur mit den gewünschten Eingaben ausgeführt wird.

+ Verwenden Sie `setInputValue` , wenn Sie einem Parameter, der als ein *InputParameter* -Objekt gespeichert ist, bestimmte Werte zuweisen möchten. Sie können dann das Parameterobjekt und dessen Wertzuweisung an die *StoredProcedure* -Funktion übergeben, um die gespeicherte Prozedur mit den festgelegten Werten auszuführen.

+ Verwenden Sie `executeStoredProcedure` , wenn Sie eine gespeicherte Prozedur ausführen möchten, die als ein *StoredProcedure* -Objekt definiert ist. Rufen Sie diese Funktion nur auf, wenn Sie eine gespeicherte Prozedur aus R-Code ausführen. Verwenden Sie diese Funktion nicht, wenn Sie die gespeicherte Prozedur aus SQL Server mithilfe von T-SQL ausführen.

> [!NOTE]
> Die *executeStoredProcedure* -Funktion erfordert einen ODBC 3.8-Anbieter, z.B. ODBC Driver 13 for SQL Server.  

## <a name="see-also"></a>Weitere Informationen

[Erstellen einer gespeicherten Prozedur mithilfe von sqlrutils](how-to-create-a-stored-procedure-using-sqlrutils.md)