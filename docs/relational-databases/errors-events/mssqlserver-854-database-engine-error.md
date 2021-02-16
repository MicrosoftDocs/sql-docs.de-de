---
description: MSSQLSERVER_854
title: MSSQLSERVER_854
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 854 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: e854963d9b14833bcadbdbe046cccfb16e08e03f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348672"
---
# <a name="mssqlserver_854"></a>MSSQLSERVER_854
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|854|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|HARDWARE_MEMORY_SCRUBBER|
|Meldungstext|Der Computer unterstützt die Wiederherstellung nach einem Arbeitsspeicherfehler. Der SQL-Arbeitsspeicherschutz ist aktiviert, um die Wiederherstellung nach einer Arbeitsspeicherbeschädigung zu ermöglichen.|
||

## <a name="explanation"></a>Erklärung

Diese Meldung gibt an, dass die Hardware im Betriebssystem die Möglichkeit zur Wiederherstellung nach Arbeitsspeicherfehlern unterstützt. Auf Computern mit neuerer Hardware, auf denen Windows Server 2012 oder eine höhere Version ausgeführt wird, kann die Hardware das Betriebssystem und die Anwendungen darüber benachrichtigen, dass Arbeitsspeicherseiten (Betriebssystemseiten) als „fehlerhaft“ oder „beschädigt“ gekennzeichnet sind. Anwendungen wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] können diese Benachrichtigungen zu fehlerhaften Arbeitsspeicherseiten mithilfe der folgenden APIs registrieren:

- `GetMemoryErrorHandlingCapabilities`
- `RegisterBadMemoryNotification`
- `BadMemoryCallbackRoutine`

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fügt Unterstützung für diese Benachrichtigungen in Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012 und höheren Versionen hinzu. Während des Starts von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] überprüft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ob die Hardware dieses neue Feature unterstützt. Außerdem wird Ihnen im Fehlerprotokoll die folgende Meldung angezeigt:

> \<Datetime> Der Server-Computer unterstützt die Wiederherstellung nach einem Arbeitsspeicherfehler. Der SQL-Arbeitsspeicherschutz ist aktiviert, um die Wiederherstellung nach einer Arbeitsspeicherbeschädigung zu ermöglichen.

## <a name="user-action"></a>Benutzeraktion

Überprüfen Sie, ob andere Fehler wie 855 und 856 auftreten, und ergreifen Sie entsprechende Korrekturmaßnahmen.

## <a name="more-information"></a>Weitere Informationen

Sie können das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ablaufverfolgungsflag 849 verwenden, um zu verhindern, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] beim Betriebssystem für Benachrichtigungen zu Arbeitsspeicherfehlern registriert wird. Beachten Sie jedoch, dass das Aktivieren des Ablaufverfolgungsflags 849 verhindert, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vom Betriebssystem Benachrichtigungen zu Fehlern im Arbeitsspeicher erhält. Daher wird empfohlen, dieses Ablaufverfolgungsflag unter üblichen Umständen nicht zu verwenden.
