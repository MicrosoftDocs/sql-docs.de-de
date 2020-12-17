---
description: Airline-Demodaten für Python- und R-Tutorials in SQL Server
title: Airline-Demodaten für Tutorials
Description: Erstellen Sie eine Datenbank, die das Airline-Dataset von R und Python enthält. Dieses Dataset wird in R- und Python-Tutorials für SQL Server Machine Learning Services verwendet.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/22/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 3b19e905d68d53764a496ab5ee7745e3529484cf
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489900"
---
#  <a name="airline-flight-arrival-demo-data-for-sql-server-python-and-r-tutorials"></a>Airline-Demodaten für Python- und R-Tutorials in SQL Server
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

In dieser Übung erstellen Sie eine SQL Server-Datenbank, um importierte Daten aus den integrierten Airline-Demodatasets von R oder Python zu speichern. Die R- und Python-Verteilungen stellen identische Daten bereit, die Sie mithilfe von Management Studio in eine SQL Server-Datenbank importieren können.

Für diese Übung benötigen Sie [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) oder ein anderes Tool zum Ausführen von T-SQL-Abfragen.

Dieses Dataset wird in folgenden Tutorials und Schnellstarts verwendet:

+  [Erstellen eines Python-Modells mithilfe von revoscalepy](use-python-revoscalepy-to-create-model.md)

## <a name="create-the-database"></a>Erstellen der Datenbank

1. Starten Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit einer Datenbank-Engine-Instanz her, die über eine R- oder Python-Integration verfügt.  

2. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf **Datenbanken**, und erstellen Sie eine neue Datenbank namens **flightdata**.

3. Klicken Sie mit der rechten Maustaste auf **flightdata**, und klicken Sie dann auf **Aufgaben** gefolgt von **Flatfile importieren**.

4. Öffnen Sie die Datei „AirlineDemoData.csv“, die in der R- oder Python-Verteilung bereitgestellt wird, je nachdem, welche Sprache Sie installiert haben.

   Wenn Sie R verwenden, suchen Sie unter C:\Programme\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR\SampleData nach **AirlineDemoSmall.csv**.
   
   Wenn Sie Python verwenden, suchen Sie unter C:\Programme\Microsoft SQL Server\MSSQL14.MSSQLSERVER\PYTHON_SERVICES\Lib\site-packages\revoscalepy\data\sample_data nach **AirlineDemoSmall.csv**.
  
Sobald Sie die Datei ausgewählt haben, werden Standardwerte für den Tabellennamen und das Schema eingefügt.

  ![Assistent zum Importieren von Flatfiles mit Standardwerten der Airline-Demodaten](media/import-airlinedemosmall.png)

Klicken Sie durch die restlichen Seiten, und akzeptieren Sie dabei die Standardwerte, um die Daten zu importieren.


## <a name="query-the-data"></a>Abfragen der Daten

Führen Sie eine Abfrage aus, um zu prüfen, ob die Daten hochgeladen wurden.

1. Klicken Sie in Objekt-Explorer unter „Datenbanken“ mit der rechten Maustaste auf die Datenbank **flightdata**, und starten Sie eine neue Abfrage.

2. Führen Sie einige einfache Abfragen aus:

    ```sql
    SELECT TOP(10) * FROM AirlineDemoSmall;
    SELECT COUNT(*) FROM AirlineDemoSmall;
    ```

## <a name="next-steps"></a>Nächste Schritte

In der nächsten Lerneinheit erstellen Sie ein lineares Regressionsmodell basierend auf diesen Daten.

+ [Erstellen eines Python-Modells mithilfe von revoscalepy](use-python-revoscalepy-to-create-model.md)
