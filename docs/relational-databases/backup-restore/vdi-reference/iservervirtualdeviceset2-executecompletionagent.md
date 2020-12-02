---
title: IServerVirtualDeviceSet2::ExecuteCompletionAgent
titlesuffix: SQL Server VDI reference
description: In diesem Artikel erhalten Sie Referenzinformationen zum Befehl „IServerVirtualDeviceSet2::ExecuteCompletionAgent“.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: ce53793d624c0a56fda48805c5c2fc8fc178e42c
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2020
ms.locfileid: "96128878"
---
# <a name="iservervirtualdeviceset2executecompletionagent-vdi"></a>IServerVirtualDeviceSet2::ExecuteCompletionAgent (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Die Funktion **ExecuteCompletionAgent** wird verwendet, um die Hauptschleife des Vervollständigungs-Agents zu implementieren.

## <a name="syntax"></a>Syntax

```c
HRESULT IServerVirtualDeviceSet2::ExecuteCompletionAgent ();
```

## <a name="return-value"></a>Rückgabewert

Gibt *HRESULT* zurück, wodurch der Erfolg oder das Fehlschlagen des Methodenaufrufs angegeben wird. Der Wert „NOERROR“ gibt an, dass der Methodenaufruf erfolgreich war. Ein Wert ungleich 0 (null) gibt an, dass ein Fehler aufgetreten ist.

## <a name="remarks"></a>Bemerkungen

Der Vervollständigungs-Agent stellt einen Mechanismus bereit, über den sich die SQL Server-Instanz mit den Vervollständigungen der Befehle des virtuellen Geräts synchronisieren kann. Dieser muss aktiv sein, damit Befehle ausgegeben werden können, und er sollte aktiv bleiben, bis alle Geräte geschlossen sind.

Da die SQL Server-Instanz möglicherweise eine spezielle Threadinitialisierung ausführen muss, startet diese Schnittstelle keinen neuen Steuerungsthread. Stattdessen richtet die SQL Server-Instanz einen Thread ein und übergibt dann die Steuerung an diese Routine. Der Thread muss in Windows NT-Mechanismen für die prozessübergreifende Kommunikation (Inter-Process Communication, IPC) blockiert werden können und in der Lage sein, jede der Rückruffunktionen aufzurufen, die mit Befehlen bereitgestellt werden, die über „IServerVirtualDevice::SendCommand“ gesendet werden.

Diese Funktion wird erst zurückgegeben, wenn „IServerVirtualDeviceSet2::Close“ oder „SignalAbort“ aufgerufen wird.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Referenz zur Schnittstelle für virtuelle Geräte in SQL Server](reference-virtual-device-interface.md).