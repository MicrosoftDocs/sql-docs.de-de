---
title: Virtualisieren externer Daten
description: Auf dieser Seite wird die Verwendung des Assistenten zum Erstellen externer Tabellen für ODBC-Datenquellen detailliert beschrieben.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: polybase
monikerRange: '>= sql-server-ver15'
ms.metadata: seo-lt-2019
ms.openlocfilehash: 85365639565eaf42ca1bf23825016a08ef8f50bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081341"
---
# <a name="use-the-external-table-wizard-with-odbc-data-sources"></a>Verwenden des Assistenten für externe Tabellen mit ODBC-Datenquellen

Eine der wichtigsten Funktionen in SQL Server 2019 ist die Datenvirtualisierung. Dabei können die Daten ihren ursprünglichen Speicherort beibehalten. Diese Daten können Sie in einer SQL Server-Instanz *virtualisieren*, sodass sie wie in jeder anderen Tabelle in SQL Server abgefragt werden können. Dadurch werden weniger ETL-Vorgänge benötigt. Dieser Prozess wird durch die Verwendung von PolyBase-Connectors ermöglicht. Weitere Informationen finden Sie unter [Erste Schritte mit PolyBase](polybase-guide.md).

Dieses Video bietet eine Einführung in die Datenvirtualisierung:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Introducing-Data-Virtualization/player?WT.mc_id=dataexposed-c9-niner]


## <a name="start-the-external-table-wizard"></a>Starten des Assistenten für externe Tabellen

Stellen Sie eine Verbindung mit der Masterinstanz her, indem Sie die IP-Adresse/Portnummer des **sql-server-master**-Endpunkts verwenden, den Sie mithilfe des Befehls [**azdata cluster endpoints list**](../../big-data-cluster/deployment-guidance.md#endpoints) abgerufen haben. Erweitern Sie im Objekt-Explorer den Knoten **Datenbanken**. Wählen Sie dann eine der Datenbanken aus, in der Sie die Daten aus einer vorhandenen SQL Server-Instanz virtualisieren möchten. Klicken Sie erst mit der rechten Maustaste auf die Datenbank und anschließend mit der linken auf **Create External Table** (Externe Tabelle erstellen), um den Assistenten zum Virtualisieren von Daten zu starten. Sie können den Assistenten zum Virtualisieren von Daten auch über die Befehlspalette starten. Drücken Sie unter Windows STRG+UMSCHALT+P oder unter macOS CMD+UMSCHALT+P.

![Assistent zum Virtualisieren von Daten](media/data-virtualization/virtualize-data-wizard.png)
## <a name="select-a-data-source"></a>Auswählen einer Datenquelle

Wenn Sie den Assistenten über eine der Datenbanken gestartet haben, wird das Dropdownfeld unter „Ziel“ automatisch aufgefüllt. Auf dieser Seite haben Sie auch die Möglichkeit, die Zieldatenbank anzugeben oder zu ändern. SQL Server, Oracle, MongoDB und Teradata werden als Typen der externen Datenquelle vom Assistenten unterstützt.

> [!NOTE]
>SQL Server wird standardmäßig hervorgehoben.


![Auswählen einer Datenquelle](media/data-virtualization/select-data-source.png)

Klicken Sie auf **Weiter**, um fortzufahren.

## <a name="create-a-database-master-key"></a>Erstellen eines Datenbankhauptschlüssels

In diesem Schritt sollten Sie einen Datenbankhauptschlüssel erstellen. Sie müssen einen Hauptschlüssel erstellen. Dieser sichert die von einer externen Datenquelle verwendeten Anmeldeinformationen. Wählen Sie ein sicheres Kennwort für Ihren Hauptschlüssel aus. Sichern Sie den Hauptschlüssel außerdem über den Befehl **BACKUP MASTER KEY**. Verwahren Sie die Sicherungskopie an einem sicheren Ort außerhalb der Geschäftsräume.

![Erstellen eines Datenbankhauptschlüssels](media/data-virtualization/virtualize-data-master-key.png)

> [!IMPORTANT]
> Wenn Sie bereits über einen Datenbankhauptschlüssel verfügen, wird dieser Schritt automatisch umgangen.

## <a name="enter-external-data-source-credentials"></a>Eingeben von Anmeldeinformationen für die externe Datenquelle

In diesem Schritt erfahren Sie, wie Sie Ihre externe Datenquelle und die entsprechenden Anmeldeinformationen eingeben, um ein externes Datenquellenobjekt zu erstellen. Die Anmeldeinformationen werden verwendet, damit das Datenbankobjekt eine Verbindung mit der Datenquelle herstellen kann. Geben Sie einen Namen für die externe Datenquelle ein. Beispielsweise „Test“. Geben Sie die Details der Verbindung zwischen der externen Datenquelle und SQL Server an. Geben Sie den **Servernamen** und den **Datenbanknamen** an, für den die externe Datenquelle erstellt werden soll.

Konfigurieren Sie anschließend Anmeldeinformationen. Geben Sie einen Namen für die Anmeldeinformationen ein. Es handelt sich dabei um die für die gesamte Datenbank gültigen Anmeldeinformationen, die verwendet werden, um die Anmeldeinformationen für die externe Datenquelle zu speichern, die Sie erstellen. z. B. `TestCred`. Geben Sie einen Benutzernamen und ein Kennwort an, um eine Verbindung mit der Datenquelle herzustellen.

![Screenshot zu Schritt 3: Herstellen einer Verbindung mit der Datenquelle](media/data-virtualization/data-source-credentials.png)

## <a name="external-data-table-mapping"></a>Zuordnungstabelle für externe Daten

Wählen Sie auf der nächsten Seite die Tabellen aus, für die Sie externe Ansichten erstellen möchten. Wenn Sie übergeordnete Datenbanken auswählen, werden auch die untergeordneten Tabellen hinzugefügt. Wenn Sie Tabellen ausgewählt haben, wird auf der nächsten Seite rechts eine Zuordnungstabelle angezeigt. An dieser Stelle können Sie Typänderungen vornehmen. Außerdem können Sie den Namen der ausgewählten externen Tabelle ändern.

![Screenshot zu Schritt 4: Zuordnen der Datenquellenobjekte zur externen Tabelle](media/data-virtualization/data-table-map.png)

> [!NOTE]
>Wenn Sie auf eine andere ausgewählte Tabelle doppelklicken, ändert sich die Zuordnungsansicht.

> [!IMPORTANT]
>Der Fototyp wird vom Tool für externe Tabellen noch nicht unterstützt. Wenn Sie eine externe Ansicht mit einem Fototyp erstellen, wird ein Fehler angezeigt, nachdem die Tabelle erstellt wurde. Sie wird aber trotzdem erstellt.

## <a name="summary"></a>Zusammenfassung

In diesem Schritt erhalten Sie eine Zusammenfassung Ihrer Auswahl. Diese umfasst den Namen der für die gesamte Datenbank gültigen Anmeldeinformationen und der Objekte der externen Datenquelle, die in der Zieldatenbank erstellt werden. Klicken Sie auf **Skript generieren**, um in T-SQL ein Skript mit der Syntax zu erstellen, die zum Erstellen der externen Datenquelle verwendet wird. Klicken Sie auf **Erstellen**, um ein externes Datenquellenobjekt zu erstellen.

![Bildschirm „Zusammenfassung“](media/data-virtualization/virtualize-data-summary.png)

Wenn Sie auf **Erstellen** klicken, wird das in der Zieldatenbank erstellte externe Datenquellenobjekt angezeigt.

![Externe Datenquellen](media/data-virtualization/external-data-sources.png)

Wenn Sie auf **Skript generieren** klicken, wird die T-SQL-Abfrage angezeigt, die zum Erstellen des Objekts der externen Datenquelle generiert wird.

![Skript generieren](media/data-virtualization/generated-script.png)

> [!NOTE]
> Die Option **Skript generieren** sollte nur auf der letzten Seite im Assistenten angezeigt werden. Derzeit wird die Option auf allen Seiten angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum SQL Server-Cluster für Big Data und den entsprechenden Szenarios finden Sie unter [What are SQL Server big data clusters? (Was sind SQL Server-Cluster für Big Data?)](../../big-data-cluster/big-data-cluster-overview.md).
