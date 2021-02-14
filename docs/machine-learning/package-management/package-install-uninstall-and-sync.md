---
title: Synchronisierung von R-Paketen über das Dateisystem
description: Informationen zum Aktualisieren von R-Bibliotheken in SQL Server mit neueren Versionen, die im Dateisystem installiert sind.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 06/13/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2017
ms.openlocfilehash: 72f10f291041f44c0598d40b21ca0bfe379a42d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073885"
---
# <a name="r-package-synchronization-for-sql-server"></a>Synchronisierung von R-Paketen für SQL Server
[!INCLUDE [SQL Server 2017 only](../../includes/applies-to-version/sqlserver2017-only.md)]

Die Version von RevoScaleR in SQL Server 2017 bietet die Möglichkeit, Sammlungen von R-Paketen zwischen dem Dateisystem und der Instanz und Datenbank, in der die Pakete verwendet werden, zu synchronisieren.

Diese Funktion wurde bereitgestellt, um das Sichern von R-Paketsammlungen, die SQL Server-Datenbanken zugeordnet sind, zu erleichtern. Mit dieser Funktion kann ein Administrator nicht nur die Datenbank, sondern auch alle R-Pakete wiederherstellen, die von den in dieser Datenbank arbeitenden Datenwissenschaftlern verwendet wurden.

Dieser Artikel beschreibt die Paketsynchronisierungsfunktion und die Verwendung der Funktion [rxSyncPackages](/machine-learning-server/r-reference/revoscaler/rxsyncpackages) zur Durchführung der folgenden Aufgaben:

+ Synchronisieren einer Liste von Paketen für eine gesamte SQL Server-Datenbank

+ Synchronisieren von Paketen, die von einem einzelnen Benutzer oder einer Benutzergruppe verwendet werden

+ Wenn ein Benutzer zu einer anderen SQL Server-Instanz wechselt, können Sie eine Sicherung der Arbeitsdatenbank des Benutzers erstellen und diese auf dem neuen Server wiederherstellen. Die Pakete für den Benutzer werden dann, wie von R gefordert, in das Dateisystem auf dem neuen Server installiert.

Beispielsweise können Sie in diesen Szenarien die Paketsynchronisierung verwenden:

+ Der Datenbankadministrator hat eine Instanz von SQL Server auf einem neuen Computer wiederhergestellt und fordert die Benutzer auf, von ihren R-Clients aus eine Verbindung herzustellen und `rxSyncPackages` auszuführen, um ihre Pakete zu aktualisieren und wiederherzustellen.

+ Sie glauben, dass ein R-Paket im Dateisystem beschädigt ist, weshalb Sie `rxSyncPackages` für die SQL Server-Instanz ausführen.

## <a name="requirements"></a>Requirements (Anforderungen)

Bevor Sie die Paketsynchronisierung verwenden können, müssen Sie über die entsprechende Version von Microsoft R oder Machine Learning Server verfügen. Dieses Feature wird ab Microsoft R 9.1.0 bereitgestellt. 

Sie müssen außerdem die [Paketverwaltungsfunktion](r-package-how-to-enable-or-disable.md) auf dem Server aktivieren.

### <a name="determine-whether-your-server-supports-package-management"></a>Bestimmen, ob der Server die Paketverwaltung unterstützt

Diese Funktion ist ab SQL Server 2017 CTP 2 verfügbar.

Sie können dieses Feature einer Instanz von SQL Server 2016 hinzufügen, indem Sie die Instanz so aktualisieren, dass die neueste Version von Microsoft R verwendet wird. Weitere Informationen finden Sie unter [Verwenden von SqlBindR.exe zum Aktualisieren von SQL Server R Services](../install/upgrade-r-and-python.md).

### <a name="enable-the-package-management-feature"></a>Aktivieren der Paketverwaltungsfunktion

Um die Paketsynchronisierung zu verwenden, muss die neue Paketverwaltungsfunktion in der SQL Server-Instanz und den einzelnen Datenbanken aktiviert werden. Weitere Informationen finden Sie unter [Aktivieren oder Deaktivieren der Paketverwaltung für SQL Server](r-package-how-to-enable-or-disable.md).

1. Der Serveradministrator aktiviert die Funktion für die SQL Server-Instanz.
2. Für jede Datenbank gewährt der Administrator einzelnen Benutzern die Berechtigung, R-Pakete unter Verwendung von Datenbankrollen zu installieren oder gemeinsam zu nutzen.

Wenn dies erfolgt ist, können Sie RevoScaleR-Funktionen wie [rxInstallPackages](/machine-learning-server/r-reference/revoscaler/rxinstallpackages) verwenden, um Pakete in eine Datenbank zu installieren.  Informationen zu Benutzern und den Paketen, die Sie verwenden können, werden in der SQL Server-Instanz gespeichert. 

Jedes Mal, wenn Sie ein neues Paket mithilfe der Paketverwaltungsfunktionen hinzufügen, werden sowohl die Datensätze in SQL Server als auch im Dateisystem aktualisiert. Diese Informationen können zur Wiederherstellung von Paketinformationen für die gesamte Datenbank verwendet werden.

### <a name="permissions"></a>Berechtigungen

+ Die Person, die die Paketsynchronisierungsfunktion ausführt, muss ein Sicherheitsprinzipal für die SQL Server-Instanz und die Datenbank sein, die die Pakete enthält.

+ Der Aufrufer der Funktion muss Mitglied einer dieser Paketverwaltungsrollen sein: **rpkgs-shared** oder **rpkgs-private**.

+ Um Pakete, die als **freigegeben** markiert sind, zu synchronisieren, muss die Person, die die Funktion ausführt, Mitglied der Rolle **rpkgs-shared** sein. Die Pakete, die verschoben werden, müssen in eine Bibliothek mit gemeinsamem Geltungsbereich installiert worden sein.

+ Um als **privat** markierte Pakete zu synchronisieren, muss entweder der Besitzer des Pakets oder der Administrator die Funktion ausführen, und die Pakete müssen privat sein.

+ Um Pakete im Auftrag anderer Benutzer zu synchronisieren, muss der Besitzer Mitglied der Datenbankrolle **db_owner** sein.

## <a name="how-package-synchronization-works"></a>Funktionsweise der Paketsynchronisierung

Um die Paketsynchronisierung zu verwenden, rufen Sie [rxSyncPackages](/r-server/r-reference/revoscaler/rxsyncpackages) auf, eine neue Funktion in [RevoScaleR](/machine-learning-server/r-reference/revoscaler/revoscaler). 

Für jeden Aufruf von `rxSyncPackages` müssen Sie eine SQL Server-Instanz und eine Datenbank angeben. Listen Sie dann entweder die zu synchronisierenden Pakete auf, oder geben Sie den Geltungsbereich der Pakete an.

1. Erstellen Sie den SQL Server-Computekontext, indem Sie die `RxInSqlServer`-Funktion verwenden. Wenn Sie keinen Computekontext angeben, wird der aktuelle Computekontext verwendet.

2. Geben Sie den Namen einer Datenbank in der Instanz im angegebenen Computekontext an. Pakete werden datenbankbezogen synchronisiert.

3. Geben Sie die zu synchronisierenden Pakete mithilfe des Geltungsbereichsarguments an.

    Wenn Sie den Geltungsbereich **privat** verwenden, werden nur Pakete synchronisiert, die dem angegebenen Besitzer gehören. Wenn Sie den Geltungsbereich **freigegeben** angeben, werden alle nicht privaten Pakete in der Datenbank synchronisiert. 
    
    Wenn Sie die Funktion ausführen, ohne entweder **privaten** oder **freigegebenen** Geltungsbereich anzugeben, werden alle Pakete synchronisiert.

4. Wenn der Befehl erfolgreich ist, werden vorhandene Pakete im Dateisystem mit dem angegebenen Geltungsbereich und Besitzer zur Datenbank hinzugefügt.

    Wenn das Dateisystem beschädigt ist, werden die Pakete auf Grundlage der in der Datenbank gepflegten Liste wiederhergestellt.

    Wenn die Paketverwaltungsfunktion in der Zieldatenbank nicht verfügbar ist, wird ein Fehler ausgelöst: „Die Paketverwaltungsfunktion ist entweder nicht in der SQL Server-Instanz aktiviert, oder die Version ist zu alt“

### <a name="example-1-synchronize-all-package-by-database"></a>Beispiel 1: Alle Pakete nach Datenbank synchronisieren

Dieses Beispiel ruft alle neuen Pakete aus dem lokalen Dateisystem ab und installiert die Pakete in der Datenbank [TestDB]. Da kein Besitzer spezifisch angegeben ist, enthält die Liste alle Pakete, die für private und freigegebene Geltungsbereiche installiert wurden.

```R
connectionString <- "Driver=SQL Server;Server=myServer;Database=TestDB;Trusted_Connection=True;"
computeContext <- RxInSqlServer(connectionString = connectionString )
rxSyncPackages(computeContext=computeContext, verbose=TRUE)
```

### <a name="example-2-restrict-synchronized-packages-by-scope"></a>Beispiel 2: Synchronisierte Pakete nach Geltungsbereich einschränken

In den folgenden Beispielen werden nur die Pakete im angegebenen Geltungsbereich synchronisiert.

```R
#Shared scope
rxSyncPackages(computeContext=computeContext, scope="shared", verbose=TRUE)

#Private scope
rxSyncPackages(computeContext=computeContext, scope="private", verbose=TRUE)
```

### <a name="example-3-restrict-synchronized-packages-by-owner"></a>Beispiel 3: Synchronisierte Pakete nach Besitzer einschränken

Das folgende Beispiel zeigt, wie nur die Pakete synchronisiert werden, die für einen bestimmten Benutzer installiert wurden. In diesem Beispiel wird der Benutzer anhand des SQL-Anmeldenamens *user1* identifiziert.

```R
rxSyncPackages(computeContext=computeContext, scope="private", owner = "user1", verbose=TRUE))
```

## <a name="related-resources"></a>Zugehörige Ressourcen

[R-Paketverwaltung für SQL Server](install-additional-r-packages-on-sql-server.md)