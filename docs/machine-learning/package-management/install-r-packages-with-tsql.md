---
title: Verwenden von T-SQL (CREATE EXTERNAL LIBRARY) zum Installieren von R-Paketen
description: Informationen zum Hinzufügen neuer R-Pakete zu SQL Server 2016 R Services oder SQL Server Machine Learning Services (datenbankintern).
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2017
ms.openlocfilehash: 058cd3a573abfa427b1e71ac08ac66c4b0e17932
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074351"
---
# <a name="use-t-sql-create-external-library-to-install-r-packages-on-sql-server"></a>Verwenden von T-SQL (CREATE EXTERNAL LIBRARY) zum Installieren von R-Paketen in SQL Server
[!INCLUDE [SQL Server 2017 only](../../includes/applies-to-version/sqlserver2017-only.md)]

In diesem Artikel wird erläutert, wie neue R-Pakete in einer Instanz von SQL Server installiert werden können, in der maschinelles Lernen aktiviert ist. Es stehen mehrere Ansätze zur Wahl. T-SQL eignet sich am besten für Serveradministratoren, die mit R nicht vertraut sind.

Die Anweisung [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) ermöglicht es, ein Paket oder eine Gruppe von Paketen zu einer Instanz oder einer bestimmten Datenbank hinzuzufügen, ohne R- oder Python-Code direkt auszuführen. Diese Methode erfordert jedoch eine Paketvorbereitung und zusätzliche Datenbankberechtigungen.

+ Alle Pakete müssen als lokale ZIP-Datei zur Verfügung stehen und dürfen nicht bedarfsweise aus dem Internet heruntergeladen werden.

+ Alle Abhängigkeiten müssen anhand des Namens und der Version identifiziert und in die ZIP-Datei eingebunden werden. Die Anweisung schlägt fehl, wenn erforderliche Pakete nicht verfügbar sind, einschließlich nachgelagerter Paketabhängigkeiten. 

+ Sie müssen die Rolle **db_owner** oder die Berechtigung CREATE EXTERNAL LIBRARY in einer Datenbankrolle haben. Einzelheiten finden Sie unter [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md).

## <a name="download-packages-in-archive-format"></a>Herunterladen von Paketen im Archivformat

Wenn Sie ein einzelnes Paket installieren, laden Sie das Paket im ZIP-Format herunter.

Aufgrund von Paketabhängigkeiten ist es üblich, mehrere Pakete zu installieren. Wenn ein Paket andere Pakete erfordert, müssen Sie sicherstellen, dass alle anderen Pakete während der Installation aufeinander zugreifen können. Es wird empfohlen, die Tools [miniCRAN](https://andrie.github.io/miniCRAN/) zum Erstellen [eines lokalen Repositorys](create-a-local-package-repository-using-minicran.md) für das Zusammenstellen einer vollständigen Paketsammlung und [igraph](https://igraph.org/r/) zum Analysieren von Paketabhängigkeiten zu verwenden. Das Installieren der falschen Version eines Pakets oder Weglassen einer Paketabhängigkeit kann dazu führen, dass eine CREATE EXTERNAL LIBRARY-Anweisung fehlschlägt. 

## <a name="copy-the-file-to-a-local-folder"></a>Kopieren der Datei in einen lokalen Ordner

Kopieren Sie die ZIP-Datei mit allen Paketen in einen lokalen Ordner auf dem Server. Wenn Sie keinen Zugriff auf das Dateisystem auf dem Server haben, können Sie auch ein komplettes Paket in einem Binärformat als Variable übergeben. Weitere Informationen finden Sie unter [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-library-transact-sql.md).

## <a name="run-the-statement-to-upload-packages"></a>Ausführen der Anweisung zum Hochladen von Paketen

Öffnen Sie mit einem Konto mit Administratorrechten das Fenster **Abfrage**.

Führen Sie die T-SQL-Anweisung `CREATE EXTERNAL LIBRARY` aus, um die gezippte Paketsammlung in die Datenbank hochzuladen.

Die folgende Anweisung benennt beispielsweise als Paketquelle ein miniCRAN-Repository, welches das Paket **randomForest** und seine Abhängigkeiten enthält. 

```sql
CREATE EXTERNAL LIBRARY [randomForest]
FROM (CONTENT = 'C:\Temp\Rpackages\randomForest_4.6-12.zip')
WITH (LANGUAGE = 'R');
```

Sie können keinen willkürlichen Namen verwenden. Der Name der externen Bibliothek muss denselben Namen haben, den Sie beim Laden oder Aufrufen des Pakets erwarten.

## <a name="verify-package-installation"></a>Überprüfen der Paketinstallation

Wenn die Bibliothek erfolgreich erstellt wurde, können Sie das Paket in SQL Server ausführen, indem Sie es innerhalb einer gespeicherten Prozedur aufrufen.
    
```sql
EXEC sp_execute_external_script
@language =N'R',
@script=N'library(randomForest)'
```

## <a name="see-also"></a>Weitere Informationen

+ [Abrufen von R-Paketinformationen](r-package-information.md)
+ [R-Tutorials](../tutorials/r-tutorials.md)