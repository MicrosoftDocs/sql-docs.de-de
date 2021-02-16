---
description: MSSQLSERVER_855
title: MSSQLSERVER_855
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 855 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: f4679f3018ef6821ba9b4894c5ef1a7787ca6d81
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348661"
---
# <a name="mssqlserver_855"></a>MSSQLSERVER_855
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|855|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|BAD_MEMORY_OUTSIDE_BPOOL|
|Meldungstext|Uncorrectable hardware memory corruption detected. Your system may become unstable. Check the Windows event log for more details (Es wurde eine nicht behebbare Beschädigung des Hardwarespeichers festgestellt. Das System kann instabil werden. Weitere Informationen finden Sie im Windows-Ereignisprotokoll.)|
||

## <a name="explanation"></a>Erklärung

Diese Meldung deutet darauf hin, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in einem zwischengespeicherten Objekt außerhalb des Pufferpools eine fehlerhafte Arbeitsspeicherseite gefunden hat. Diese Meldung wird für Systeme ausgelöst, die die Wiederherstellung nach Arbeitsspeicherfehlern unterstützen. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann in diesen Szenarios nicht wieder hergestellt werden und protokolliert diese Meldung.

## <a name="user-action"></a>Benutzeraktion

Sie sollten Hardware- oder Systemprüfungen durchführen, um zu ermitteln, ob ein Problem mit dem Arbeitsspeicher oder der CPU besteht. Stellen Sie sicher, dass Sie über alle Systemtreiber verfügen und Ihr Betriebssystem und Ihre Hardware auf dem neusten Stand sind. Führen Sie ggf. eine beliebige Diagnose eines Hardwareherstellers einschließlich speicherbezogener Tests durch. Wenn dieser Fehler angezeigt wird, sollten Sie `DBCC CHECKDB` für alle Datenbanken in dieser Instanz ausführen.

## <a name="more-information"></a>Weitere Informationen

Auf Computern mit neuerer Hardware, auf denen Windows Server 2012 oder eine höhere Version ausgeführt wird, kann die Hardware das Betriebssystem und die Anwendungen darüber benachrichtigen, dass Arbeitsspeicherseiten (Betriebssystemseiten) als „fehlerhaft“ oder „beschädigt“ gekennzeichnet sind. Anwendungen wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] können diese Benachrichtigungen zu fehlerhaften Arbeitsspeicherseiten mithilfe der folgenden APIs registrieren:

- `GetMemoryErrorHandlingCapabilities`
- `RegisterBadMemoryNotification`
- `BadMemoryCallbackRoutine`

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fügt Unterstützung für diese Benachrichtigungen in Microsoft [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höheren Versionen hinzu. Während des Starts von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] überprüft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ob die Hardware dieses neue Feature unterstützt. Außerdem wird Ihnen im Fehlerprotokoll die folgende Meldung angezeigt:

> \<Datetime> Der Server-Computer unterstützt die Wiederherstellung nach einem Arbeitsspeicherfehler. SQL memory protection is enabled to recover from memory corruption. (Der Server-Computer unterstützt die Wiederherstellung nach einem Arbeitsspeicherfehler. Der SQL-Speicherschutz ist aktiviert, um die Wiederherstellung nach einer Arbeitsspeicherbeschädigung zu ermöglichen.)

Derzeit führt nur der Pufferpool Aktionen aus, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] diese Benachrichtigungen empfängt. Wenn eine Benachrichtigung empfangen wird, muss [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] den gesamten Pufferpool durchlaufen und die Adressen der zugeordneten Puffer ermitteln. Dann verwendet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die `QueryWorkingSetEX`-API, um zu überprüfen, ob eine der Arbeitsspeicherseiten, die die Datenseite stützen, als „fehlerhaft“ gekennzeichnet ist. Die fehlerhaften Member der Ausgabestruktur `PSAPI_WORKING_SET_EX_BLOCK`, die dieser Arbeitsspeicherseite zugeordnet werden kann, werden auf 1 festgelegt, wenn ein Schaden gemeldet wird.

Wenn der Pufferpool oder die Datenseite zum jeweiligen Zeitpunkt nicht geändert wird oder keine E/A-Prozesse verarbeitet, kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Datenseite verwerfen und den Commit rückgängig machen. Dann protokolliert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die folgende Meldung:

> Von SQL Server wurde eine Beschädigung des Hardwarespeichers in der %ls-Datenbank mit Datei-ID %u, Seiten-ID %u, Speicheradresse 0x%I64x festgestellt. Die Seite wurde erfolgreich wiederhergestellt.

Wenn diese Datenseite nochmals für Abfragen benötigt wird, kann der Pufferpool die Datenseite wieder vom Datenträger lesen und die Inhalte wieder an den Pufferpool übertragen. Außerdem kann sich die auf dem Datenträger gespeicherte Version der Seite in einem beschädigten Zustand befinden. In diesem Fall kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zusätzliche Fehler protokollieren, z. B. Fehler 824.

Wenn die fehlerhafte Seite nicht vom Pufferpool, sondern von einem anderen zwischengespeicherten Objekt oder einer zwischengespeicherten Struktur verwendet wird, protokolliert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die folgende Meldung:

> Es wurde eine nicht behebbare Beschädigung des Hardwarespeichers festgestellt. Das System kann instabil werden. Please check the Windows event log for more details (Es wurde eine nicht behebbare Beschädigung des Hardwarespeichers festgestellt. Das System kann instabil werden. Weitere Informationen finden Sie im Windows-Ereignisprotokoll.)

Wenn der Server Arbeitsspeicherfehler meldet, wenden Sie sich an den Hersteller der Computerhardware, und ergreifen Sie passende Maßnahmen. Sie können beispielsweise eine Arbeitsspeicherdiagnose durchführen, das BIOS und die Firmware aktualisieren sowie fehlerhafte Arbeitsspeichermodule ersetzen.

Sie können das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ablaufverfolgungsflag 849 verwenden, um zu verhindern, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] beim Betriebssystem für Benachrichtigungen zu Arbeitsspeicherfehlern registriert wird. Beachten Sie jedoch, dass das Aktivieren des Ablaufverfolgungsflags 849 verhindert, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vom Betriebssystem Benachrichtigungen zu Fehlern im Arbeitsspeicher erhält. Daher wird empfohlen, dieses Ablaufverfolgungsflag im Normalfall nicht zu verwenden.

Beachten Sie außerdem, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] diese Benachrichtigungen standardmäßig auf unterstützter Hardware empfängt.

Außerdem sollte Ihnen bewusst sein, dass der Lazy Writer-Systemprozess konstante Seiten nicht überprüft, wenn sich [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für diese Benachrichtigungen über Arbeitsspeicherfehler registriert. Weitere Informationen zu Überprüfungen von konstanten Seiten finden Sie unter [Behandeln von Meldung 832 (konstante Seite hat sich geändert) in SQL Server](https://support.microsoft.com/help/2015759).
