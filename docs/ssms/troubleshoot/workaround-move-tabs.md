---
description: Problemumgehung zum Verschieben von Registerkarten
title: Verschieben von Registerkarten ohne Absturz von SSMS ermöglichen
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan, sstein
ms.custom: seo-lt-2019
ms.date: 11/03/2020
ms.openlocfilehash: ae7c79792d962ce578059e672de7165f477f1c4a
ms.sourcegitcommit: 6c93282cce1216dac327cb28848a3ab4d51b776e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/18/2021
ms.locfileid: "100654631"
---
# <a name="workaround-to-move-tabs"></a>Problemumgehung zum Verschieben von Registerkarten

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

Zum Verschieben von Registerkarten im Fenster des Abfrage-Editors oder zum Andocken einer zuvor entfernten Registerkarte ist möglicherweise eine Problemumgehung erforderlich.  Wenn Sie alle verfügbaren Windows-Updates angewendet haben und feststellen, dass Sie die Registerkarten im Abfrage-Editor nicht ohne Absturz verschieben können, befolgen Sie die Anweisungen in der [Problemumgehung](#workaround) weiter unten.

## <a name="applicable-environments"></a>Geeignete Umgebungen
Mit den Windows-Updates für .NET Framework wurde ein bekanntes Problem eingeführt, das dazu führt, dass eine Anwendung in SQL Server Management Studio (SSMS) abstürzt, wenn Registerkarten angedockt oder das Fenster geteilt wird.  Aktuelle Informationen hierzu finden Sie unter [SQL Server-Benutzerfeedback](https://feedback.azure.com/forums/908035/suggestions/42651556).

## <a name="workaround"></a>Problemumgehung

Wenn die Anwendung nach dem Anwenden aller verfügbaren Windows-Updates weiterhin abstürzen, gehen Sie wie folgt vor, um das Problem zu beheben:

1. Schließen Sie alle Instanzen von SQL Server Management Studio (SSMS).

2. Suchen Sie die Anwendungsdatei (EXE) von SSMS.  Diese befindet sich üblicherweise im Verzeichnis `C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE`.

3. Öffnen Sie die Datei `Ssms.exe.config` in Editor als Administrator.

4. Suchen Sie den Knoten `AppContextSwitchOverrides`, und fügen Sie diese beiden Eigenschaften an den Wert an.
    ```
    ;Switch.System.Windows.Interop.MouseInput.OptOutOfMoveToChromedWindowFix=true; Switch.System.Windows.Interop.MouseInput.DoNotOptOutOfMoveToChromedWindowFix=true
    ```

    ![Bearbeiten Sie „ssms.exe.config“.](../media/troubleshoot/execonfig-edit.png)

5. Speichern Sie die Konfigurationsdatei, und öffnen Sie SSMS erneut.
