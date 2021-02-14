---
title: Aktivieren oder Deaktivieren der Remoteverwaltung von R-Paketen
description: Aktivieren der Remoteverwaltung von R-Paketen in SQL Server 2016 R Services oder SQL Server-Machine Learning Services (datenbankintern)
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 06/13/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: 728dc656d3fdb94abae0a5c2c0e2665af2c4807c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073919"
---
# <a name="enable-or-disable-remote-package-management-for-sql-server"></a>Aktivieren oder Deaktivieren der Remoteverwaltung von R-Paketen für SQL Server
[!INCLUDE [SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

In diesem Artikel wird beschrieben, wie Sie die Remoteverwaltung von R-Paketen auf einer Clientarbeitsstation oder in einer anderen Machine Learning Server-Instanz aktivieren. Nachdem die Paketverwaltungsfunktion in SQL Server aktiviert wurde, können Sie auf einem Client mit RevoScaleR-Befehlen Pakete in SQL Server installieren.

Standardmäßig ist die Funktion zur Verwaltung externer Pakete für SQL Server deaktiviert. Sie müssen ein gesondertes Skript ausführen, um die Funktion wie im nächsten Abschnitt beschrieben zu aktivieren.

## <a name="overview-of-process-and-tools"></a>Übersicht über Prozess und Tools

Führen Sie zum Aktivieren oder Deaktivieren der Paketverwaltung für SQL Server das Befehlszeilenprogramm **RegisterRExt.exe** aus, das im Paket **RevoScaleR** enthalten ist.

Das [Aktivieren](#bkmk_enable) dieser Funktion ist ein zweistufiger von einem Datenbankadministrator auszuführender Prozess. Sie aktivieren die Paketverwaltung für die SQL Server-Instanz (einmal pro SQL Server-Instanz) und aktivieren dann die Paketverwaltung für die SQL-Datenbank (einmal pro SQL Server-Datenbank).

Das [Deaktivieren](#bkmk_disable) der Paketverwaltungsfunktion erfordert ebenfalls mehrere Schritte. Sie entfernen Pakete und Berechtigungen auf Datenbankebene (einmal pro Datenbank) und entfernen dann die Rollen vom Server (einmal pro Instanz).

## <a name="enable-package-management"></a><a name="bkmk_enable"></a> Aktivieren der Paketverwaltung

1. Öffnen Sie in SQL Server eine Eingabeaufforderung mit erhöhten Rechten, und navigieren Sie zum Ordner mit dem Hilfsprogramm RegisterRExt.exe. Der Standardspeicherort ist `<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExe.exe`.

2. Führen Sie den folgenden Befehl aus, und geben Sie dabei geeignete Argumente für Ihre Umgebung an:

    `RegisterRExt.exe /install pkgmgmt [/instance:name] [/user:username] [/password:*|password]`

    Durch diesen Befehl werden auf dem Computer mit SQL Server Objekte auf Instanzebene erstellt, die für die Paketverwaltung erforderlich sind. Außerdem wird das Launchpad für die Instanz neu gestartet.

    Wenn Sie keine Instanz angeben, wird die Standardinstanz verwendet. Wenn Sie keinen Benutzer angeben, wird der aktuelle Sicherheitskontext verwendet. Der folgende Befehl ermöglicht beispielsweise die Paketverwaltung auf der Standardinstanz mithilfe der Anmeldeinformationen des Benutzers, der die Eingabeaufforderung geöffnet hat:

    `REgisterRExt.exe /install pkgmgmt`

3. Um einer bestimmten Datenbank die Paketverwaltung hinzuzufügen, führen Sie den folgenden Befehl an einer Eingabeaufforderung mit erhöhten Rechten aus:

    `RegisterRExt.exe /install pkgmgmt /database:databasename [/instance:name] [/user:username] [/password:*|password]`
   
    Dieser Befehl erstellt einige Datenbankartefakte, einschließlich der folgenden Datenbankrollen, die für das Steuern von Benutzerberechtigungen genutzt werden: `rpkgs-users`, `rpkgs-private` und `rpkgs-shared`.

    Beispielsweise aktiviert der folgende Befehl die Paketverwaltung in der Datenbank auf der Standardinstanz. Wenn Sie keinen Benutzer angeben, wird der aktuelle Sicherheitskontext verwendet.

    `RegisterRExt.exe /install pkgmgmt /database:TestDB`

4. Wiederholen Sie den Befehl für jede Datenbank, in der Pakete installiert werden müssen.

5. Um zu bestätigen, dass die neuen Rollen erfolgreich erstellt wurden, klicken Sie in SQL Server Management Studio auf die Datenbank, erweitern Sie **Sicherheit** und dann **Datenbankrollen**.

    Sie können auch eine Abfrage wie die folgende auf sys.database_principals anwenden:

    ```sql
    SELECT pr.principal_id, pr.name, pr.type_desc,   
        pr.authentication_type_desc, pe.state_desc,   
        pe.permission_name, s.name + '.' + o.name AS ObjectName  
    FROM sys.database_principals AS pr  
    JOIN sys.database_permissions AS pe  
        ON pe.grantee_principal_id = pr.principal_id  
    JOIN sys.objects AS o  
        ON pe.major_id = o.object_id  
    JOIN sys.schemas AS s  
        ON o.schema_id = s.schema_id;
    ```

Nachdem Sie diese Funktion aktiviert haben, können Sie die RevoScaleR-Funktion verwenden, um auf einem R-Remoteclient Pakete zu installieren oder zu deinstallieren.

## <a name="disable-package-management"></a><a name="bkmk_disable"></a> Deaktivieren der Paketverwaltung

1. Führen Sie an einer Eingabeaufforderung mit erhöhten Rechten erneut das Hilfsprogramm RegisterRExt aus, und deaktivieren Sie die Paketverwaltung auf Datenbankebene:

    `RegisterRExt.exe /uninstall pkgmgmt /database:databasename [/instance:name] [/user:username] [/password:*|password]`

    Dieser Befehl entfernt Datenbankobjekte, die mit der Paketverwaltung zusammenhängen, aus der angegebenen Datenbank. Er entfernt außerdem alle Pakete, die über den geschützten Speicherort im Dateisystem auf dem Computer mit SQL Server installiert wurden.

2. Wiederholen Sie diesen Befehl für jede Datenbank, in der die Paketverwaltung genutzt wurde.

3.  (Optional) Nachdem im vorhergehenden Schritt Pakete aus allen Datenbanken gelöscht wurden, führen Sie den folgenden Befehl an einer Eingabeaufforderung mit erhöhten Rechten aus:

    `RegisterRExt.exe /uninstall pkgmgmt [/instance:name] [/user:username] [/password:*|password]`

    Dieser Befehl entfernt die Paketverwaltungsfunktion aus der Instanz. Möglicherweise müssen Sie den Launchpad-Dienst noch einmal manuell neu starten, um die Änderungen zu sehen.

## <a name="next-steps"></a>Nächste Schritte

+ [Verwenden von RevoScaleR zum Installieren von R-Paketen](install-r-packages-with-revoscaler.md)
+ [Abrufen von R-Paketinformationen](r-package-information.md)
+ [Tipps für die Verwendung von R-Paketen](tips-for-using-r-packages.md)
