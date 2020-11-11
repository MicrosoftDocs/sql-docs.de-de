---
description: 'Problemumgehung zum Aktivieren des Kopierens aus dem Fenster „Suchen und Ersetzen“ '
title: Aktivieren des Kopierens aus dem Fenster „Suchen und Ersetzen“
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
ms.openlocfilehash: ea5ae3f9fa941e723d3bdf10de0ec900310cc28d
ms.sourcegitcommit: 985e2e8e494badeac6d6b652cd35765fd9c12d80
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2020
ms.locfileid: "93328780"
---
# <a name="workaround-to-enable-copying-from-find-and-replace-window"></a>Problemumgehung zum Aktivieren des Kopierens aus dem Fenster „Suchen und Ersetzen“

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

Für das Aktivieren des Kopierens aus dem Fenster „Suchen und Ersetzen“ ist eine Problemumgehung erforderlich.  Wenn Sie keinen Text aus dem Fenster „Suchen und Ersetzen“ in SQL Server Management Studio kopieren können, befolgen Sie einfach die folgende [Problemumgehung](#workaround).

## <a name="error-message"></a>Fehlermeldung

Wenn Sie versuchen, Text aus dem Fenster „Suchen und Ersetzen“ in SQL Server Management Studio zu kopieren, erhalten Sie eine Fehlermeldung.

> Nicht gespeicherte Dokumente können nicht aus dem Projekt "Sonstige Dateien" ausgeschnitten oder in die Zwischenablage kopiert werden. Sie müssen diese Dokumente vor dem Ausschneiden oder Kopieren speichern.

![Fehlerdialogfeld für: Nicht gespeicherte Dokumente können nicht aus dem Projekt "Sonstige Dateien" ausgeschnitten oder in die Zwischenablage kopiert werden. Sie müssen diese Dokumente vor dem Ausschneiden oder Kopieren speichern.](../media/troubleshoot/unable-copy-find-replace-window.png)

## <a name="workaround"></a>Problemumgehung

Führen Sie die folgenden Schritte aus, um Text aus dem Fenster „Suchen und Ersetzen“ kopieren zu können:

1. Klicken Sie im Menü **Extras** auf **Optionen**.

2. Deaktivieren Sie unter **Umgebung**>**Dokumente** das Kontrollkästchen für „Sonstige Dateien im Projektmappen-Explorer anzeigen“.

3. Schließen Sie SQL Server Management Studio, und öffnen Sie die Anwendung neu.

![Optionsfenster mit deaktiviertem Kontrollkästchen „Sonstige Dateien im Projektmappen-Explorer anzeigen“](../media/troubleshoot/fix-copy-find-replace-window.png)

