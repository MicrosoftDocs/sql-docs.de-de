---
title: Installieren & Konfigurieren der DW wideworldimporters-Beispieldatenbank
description: Befolgen Sie diese Anweisungen, um die Beispieldatenbank "wideworldimportersdw" mit SQL Server Management Studio herunterzuladen, zu installieren und zu konfigurieren.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 08/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||>=aps-pdw-2016||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 70128430c6ea0943605bd29f00597d51dee71d86
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077751"
---
# <a name="wideworldimportersdw-installation-and-configuration"></a>Installation und Konfiguration von wideworldimportersdw
[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]
Installations-und Konfigurations Anweisungen für die wideworldimportersdw-Datenbank.

- [SQL Server 2016](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016) (oder höher) oder [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/). Um die vollständige Version des Beispiels zu verwenden, verwenden Sie SQL Server Evaluation/Developer/Enterprise Edition.
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md): Um die besten Ergebnisse zu erzielen, verwenden Sie die Version vom Juni 2016 oder höher.

## <a name="download"></a>Download

Die neueste Version des Beispiels:

[Wide-World-importierungsrelease](https://go.microsoft.com/fwlink/?LinkID=800630)

Laden Sie die Beispieldatenbank "wideworldimportersdw Database Backup/BacPac" herunter, die Ihrer Edition von SQL Server oder Azure SQL-Datenbank entspricht.

Der Quellcode zum erneuten Erstellen der Beispieldatenbank ist unter folgendem Speicherort verfügbar. Beachten Sie, dass die Daten Population auf ETL aus der OLTP-Datenbank (wideworldimporters) basiert:

[Wide-World-importierungsource](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/sample-scripts)

## <a name="install"></a>Installieren


### <a name="sql-server"></a>SQL Server

Zum Wiederherstellen einer Sicherung in einer SQL Server Instanz können Sie Management Studio verwenden.

1. Öffnen Sie SQL Server Management Studio und stellen Sie eine Verbindung mit der Ziel SQL Server Instanz her.
2. Klicken Sie mit der rechten Maustaste auf den Knoten **Datenbanken** , und wählen Sie **Datenbank wiederherstellen**.
3. Wählen Sie **Gerät** aus, und klicken Sie auf die Schaltfläche **...**
4. Wählen Sie im Dialogfeld **Sicherungs** Medien aus, klicken Sie auf **Hinzufügen**, navigieren Sie im Dateisystem des Servers zu der Datenbanksicherung, und wählen Sie die Sicherung aus. Klicken Sie auf **OK**.
5. Ändern Sie ggf. den Zielort für die Daten-und Protokolldateien im Bereich " **Dateien** ". Beachten Sie, dass es eine bewährte Vorgehensweise ist, Daten-und Protokolldateien auf verschiedenen Laufwerken zu platzieren.
6. Klicken Sie auf **OK**. Dadurch wird die Daten Bank Wiederherstellung initiiert. Nachdem der Vorgang abgeschlossen ist, wird die Datenbank "wideworldimporters" auf der SQL Server-Instanz installiert.

### <a name="azure-sql-database"></a>Azure SQL-Datenbank

Zum Importieren einer BacPac-Datenbank in eine neue SQL-Datenbank können Sie Management Studio verwenden.

1. optionale Wenn Sie noch keine SQL Server in Azure haben, navigieren Sie zum [Azure-Portal](https://portal.azure.com/) , und erstellen Sie eine neue SQL-Datenbank. Beim Erstellen einer Datenbank erstellen Sie einen Server. Notieren Sie sich den Server.
   - In [diesem Tutorial](/azure/azure-sql/database/single-database-create-quickstart) finden Sie Informationen zum Erstellen einer Datenbank in wenigen Minuten.
2. Öffnen Sie SQL Server Management Studio, und stellen Sie in Azure eine Verbindung mit Ihrem Server her
3. Klicken Sie mit der rechten Maustaste auf den Knoten **Datenbanken** , und wählen Sie **Data-Tier Anwendung importieren** aus.
4. Wählen Sie unter **Import Einstellungen** die Option **aus lokalem Datenträger importieren aus** , und wählen Sie die BacPac-Datei der Beispieldatenbank aus dem Dateisystem aus.
5. Ändern Sie unter **Datenbankeinstellungen** den Datenbanknamen in *wideworldimportersdw* , und wählen Sie die zu verwendende Ziel Edition und das Dienst Ziel aus.
6. Klicken Sie auf **weiter** und **Beenden** , um die Bereitstellung zu starten. Dies nimmt einige Minuten in Anspruch. Wenn Sie ein Dienst Ziel angeben, das kleiner als S2 ist, kann es länger dauern.

## <a name="configuration"></a>Konfiguration

[Gilt für SQL Server 2016 (und höher) Developer/Evaluation/Enterprise Edition]

Die Beispieldatenbank kann polybase verwenden, um Dateien in Hadoop oder Azure BLOB Storage abzufragen. Diese Funktion wird jedoch nicht standardmäßig mit SQL Server installiert. Sie müssen Sie während SQL Server Setup auswählen. Daher ist ein Schritt nach der Installation erforderlich.

1. Stellen Sie in SQL Server Management Studio eine Verbindung mit der Datenbank wideworldimportersdw her, und öffnen Sie ein neues Abfragefenster.
2. Führen Sie den folgenden T-SQL-Befehl aus, um die Verwendung von polybase in der Datenbank zu aktivieren:

   Führen Sie [Anwendung] aus. [Configuration_ApplyPolyBase]