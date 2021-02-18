---
title: Überwachen von Skripts mit benutzerdefinierten Berichten
description: Verwenden Sie benutzerdefinierte Berichte in SQL Server Management Studio (SSMS), um die externe Skriptausführung (Python und R) und Ressourcennutzung zu überwachen, Probleme zu diagnostizieren und die Leistung in SQL Server Machine Learning Services zu optimieren.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e0c3874177691832bd6fa66f81c3fe94c726ff76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337081"
---
# <a name="monitor-python-and-r-script-execution-using-custom-reports-in-sql-server-management-studio"></a>Überwachen der Python- und R-Skriptausführung mithilfe von benutzerdefinierten Berichten in SQL Server Management Studio
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Verwenden Sie benutzerdefinierte Berichte in [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md), um die externe Skriptausführung (Python und R) und Ressourcennutzung zu überwachen, Probleme zu diagnostizieren und die Leistung in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) zu optimieren.

In diesen Berichten können Sie beispielsweise die folgenden Informationen sehen:

- Aktive Python- oder R-Sitzungen
- Konfigurationseinstellungen der Instanz
- Ausführungsstatistiken für Machine Learning-Aufträge
- Erweiterte Ereignisse bei R Services
- Auf der aktuellen Instanz installierte Python- oder R-Pakete

In diesem Artikel wird die Installation und Verwendung der für SQL Server Machine Learning Services bereitgestellten benutzerdefinierten Berichte erläutert.

Weitere Informationen über Berichte in SQL Server Management Studio finden Sie unter [Benutzerdefinierte Berichte in Management Studio](../../ssms/object/custom-reports-in-management-studio.md).

## <a name="how-to-install-the-reports"></a>So führen Sie die Installation von Berichten durch

Die Berichte wurden mithilfe von SQL Server Reporting Services entworfen, können aber direkt über SQL Server Management Studio verwendet werden. Reporting Services muss nicht auf Ihrer SQL Server-Instanz installiert sein.

Führen Sie die folgenden Schritte aus, um diese Berichte zu verwenden:

1. Laden Sie [benutzerdefinierte SSMS-Berichte](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/machine-learning-services/ssms-custom-reports) für SQL Server Machine Learning Services von GitHub herunter.

   ::: moniker range="=azuresqldb-mi-current"
   >[!NOTE]
   > Der benutzerdefinierte Bericht **ML Services: Konfigurieren einer Instanz** wird in Azure SQL Managed Instance nicht unterstützt.
   ::: moniker-end

2. Kopieren der Berichte in Management Studio

    1. Suchen Sie den Ordner der benutzerdefinierten Berichte, der von SQL Server Management Studio verwendet wird. Benutzerdefinierte Berichte werden standardmäßig im folgenden Ordner gespeichert (**user_name** entspricht hier Ihrem Windows-Benutzernamen):

        `C:\Users\user_name\Documents\SQL Server Management Studio\Custom Reports`

       Sie können auch einen anderen Ordner festlegen oder Unterordner erstellen.

    2. Kopieren Sie die heruntergeladenen RDL-Dateien in den Ordner für benutzerdefinierte Berichte.

3. Führen Sie die Berichte in Management Studio aus.

    1. Klicken Sie in Management Studio mit der rechten Maustaste auf den Knoten **Datenbanken** der Instanz, auf der Sie die Berichte ausführen möchten.

    2. Klicken Sie auf **Berichte**, und klicken Sie dann auf **Benutzerdefinierte Berichte**.

    3. Suchen Sie im Dialogfeld **Datei öffnen** den Ordner mit den benutzerdefinierten Berichten.

    4. Wählen Sie eine der RDL-Dateien aus, die Sie heruntergeladen haben, und klicken Sie dann auf **Öffnen**.

## <a name="reports"></a>Berichte

Das [Repository für benutzerdefinierte SSMS-Berichte auf GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/machine-learning-services/ssms-custom-reports) enthält die folgenden Berichte:

| Bericht | BESCHREIBUNG |
|-|-|
| Aktive Sitzungen | In diesem Bericht werden die Benutzer aufgeführt, die derzeit mit der SQL Server-Instanz verbunden sind und ein Python- oder R-Skript ausführen. |
| Konfiguration | In diesem Bericht werden die Installationseinstellungen von Machine Learning Services und die Eigenschaften der Python- oder R-Runtime aufgeführt. |
| Konfigurieren einer Instanz | Dieser Bericht dient zur Konfiguration von Machine Learning Services. |
| Ausführungsstatistiken | Dieser Bericht enthält Ausführungsstatistiken für Machine Learning Services. Sie können beispielsweise die Gesamtzahl der externen Skriptausführungen und die Anzahl paralleler Ausführungen abrufen. |
| Erweiterte Ereignisse | In diesem Bericht werden die verfügbaren erweiterte Ereignisse aufgeführt, mit denen Sie mehr Erkenntnisse aus der externen Skriptausführung erlangen können. |
| Pakete | In diesem Bericht werden die auf der SQL Server-Instanz installierten R- oder Python-Pakete sowie deren Eigenschaften (z. B. Version und Name) aufgeführt. |
| Resource Usage | In diesem Bericht wird die CPU-, Arbeitsspeicher- und E/A-Auslastung von SQL Server und der externen Skriptausführung aufgeführt. Außerdem können Sie in diesem Bericht die Arbeitsspeichereinstellungen für externe Ressourcenpools einsehen. |

## <a name="next-steps"></a>Nächste Schritte

- [Überwachen von SQL Server Machine Learning Services mithilfe von dynamischen Verwaltungssichten](monitor-sql-server-machine-learning-services-using-dynamic-management-views.md)
- [Überwachen von Python- und R-Skripts mit erweiterten Ereignissen in SQL Server Machine Learning Services](extended-events.md)
