---
title: Wideworldimportersdw-ETL-Workflow | Microsoft-Dokumentation
description: Verwenden Sie das ETL-Paket mit SQL Server Integration Services (SSIS), um Daten in regelmäßigen Abständen aus der Datenbank wideworldimporters zu wideworldimportersdw zu migrieren.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ce9cd8ee5a4153cb1d744d505a26b400497b460b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354055"
---
# <a name="wideworldimportersdw-etl-workflow"></a>Wideworldimportersdw-ETL-Workflow
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
Verwenden Sie das *WWI_Integration* ETL-Paket, um Daten aus der wideworldimporters-Datenbank in die Datenbank "wideworldimportersdw" zu migrieren, wenn sich Daten ändern. Das Paket wird in regelmäßigen Abständen ausgeführt (normalerweise täglich).

Das Paket stellt eine hohe Leistung sicher, indem SQL Server Integration Services zum orchestrieren von T-SQL-Massen Vorgängen (anstelle von separaten Transformationen in Integration Services) verwendet wird.

Dimensionen werden zuerst geladen, und anschließend werden Fakten Tabellen geladen. Sie können das Paket nach einem Fehler jederzeit erneut ausführen.

Der Workflow sieht wie folgt aus:

 ![Wideworldimporters-ETL-Workflow](media/wide-world-importers/wideworldimporters-etl-workflow.png)

Der Workflow beginnt mit einem Ausdrucks Task, der die entsprechende Umstellungszeit bestimmt. Die Umstellungszeit ist die aktuelle Zeit abzüglich weniger Minuten. (Dieser Ansatz ist robuster als das Anfordern von Daten direkt zum aktuellen Zeitpunkt.) Alle Millisekunden werden von der Zeit abgeschnitten.

Die Haupt Verarbeitung beginnt mit dem Auffüllen der Date-Dimensions Tabelle. Die Verarbeitung stellt sicher, dass alle Datumsangaben für das aktuelle Jahr in der Tabelle aufgefüllt wurden.

Anschließend lädt eine Reihe von Datenfluss Tasks jede Dimension. Anschließend laden Sie jeden Fakt.

## <a name="prerequisites"></a>Voraussetzungen

- SQL Server 2016 (oder höher) mit den Datenbanken "wideworldimporters" und "wideworldimportersdw" (in der gleichen oder in verschiedenen Instanzen von SQL Server)
- SQL Server Management Studio
- SQL Server 2016 Integration Services
  - Stellen Sie sicher, dass Sie einen Integration Services Katalog erstellen. Klicken Sie zum Erstellen eines Integration Services Katalogs in SQL Server Management Studio Objekt-Explorer mit der rechten Maustaste auf **Integration Services**, und wählen Sie dann **Katalog hinzufügen** aus. Überlassen Sie die Standardoptionen. Sie werden aufgefordert, SQLCLR zu aktivieren und ein Kennwort anzugeben.


## <a name="download"></a>Herunterladen

Die neueste Version des Beispiels finden Sie unter [Wide-World-importierungsrelease](https://go.microsoft.com/fwlink/?LinkID=800630). Laden Sie die *tägliche ETL. ispac* -Integration Services Paketdatei herunter.

Informationen zum erneuten Erstellen der Beispieldatenbank durch den Quellcode finden Sie unter [Wide-World-Importierung.](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/wwi-ssis)

## <a name="install"></a>Installieren

1. Stellen Sie das Integration Services Paket bereit:
   1. Öffnen Sie in Windows-Explorer das *tägliche ETL. ispac* -Paket. Dadurch wird der Assistent für die SQL Server Integration Services Bereitstellung gestartet.
   2. Befolgen Sie unter **Quelle auswählen** die Standardeinstellungen für die Projekt Bereitstellung, wobei der Pfad auf das *tägliche ETL. ispac* -Paket verweist.
   3. Geben Sie unter **Ziel auswählen** den Namen des Servers ein, der den Integration Services Katalog hostet.
   4. Wählen Sie unter dem Integration Services Katalog einen Pfad aus, z. b. in einem neuen Ordner mit dem Namen *wideworldimporters*.
   5. Wählen **Sie** bereitstellen aus, um den Assistenten abzuschließen.

2. Erstellen Sie einen SQL Server-Agent Auftrag für den ETL-Prozess:
   1. Klicken Sie in Management Studio mit der rechten Maustaste auf **SQL Server-Agent**, und wählen Sie dann **neuer**  >  **Auftrag** aus.
   2. Geben Sie einen Namen ein, z. b. *wideworldimporters ETL*.
   3. Fügen Sie einen **Auftrags Schritt** des Typs **SQL Server Integration Services Pakets** hinzu.
   4. Wählen Sie den Server mit dem Integration Services Katalog aus, und wählen Sie dann das *tägliche ETL* -Paket aus.
   5. Stellen Sie unter **Konfigurations**  >  **Verbindungs-Manager** sicher, dass die Verbindungen mit der Quelle und dem Ziel ordnungsgemäß konfiguriert sind. Der Standardwert besteht darin, eine Verbindung mit der lokalen Instanz herzustellen.
   6. Wählen Sie **OK** aus, um den Auftrag zu erstellen.

3. Ausführen oder Planen des Auftrags.
