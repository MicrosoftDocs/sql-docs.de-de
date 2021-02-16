---
title: IClientVirtualDeviceSet2::SignalAbort
titlesuffix: SQL Server VDI reference
description: In diesem Artikel erhalten Sie Referenzinformationen zum Befehl „IClientVirtualDeviceSet2::SignalAbort“.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: b03452441b99cd477f0901e3868109867c66e2b7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347195"
---
# <a name="iclientvirtualdeviceset2signalabort-vdi"></a>IClientVirtualDeviceSet2::SignalAbort (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Mit der Funktion **SignalAbort** wird angegeben, dass ein Vorgang unplanmäßig beendet werden soll.

## <a name="syntax"></a>Syntax

```c
HRESULT IClientVirtualDeviceSet2::SignalAbort ();
```

## <a name="return-value"></a>Rückgabewert

Gibt *HRESULT* zurück, wodurch der Erfolg oder das Fehlschlagen des Methodenaufrufs angegeben wird. Der Wert „NOERROR“ gibt an, dass der Methodenaufruf erfolgreich war. Ein Wert ungleich 0 (null) gibt an, dass ein Fehler aufgetreten ist.

## <a name="remarks"></a>Bemerkungen

Der Client kann jederzeit den Vorgang „BACKUP“ oder „RESTORE“ abbrechen. Mit dieser Routine wird angegeben, dass alle Vorgänge beendet werden sollen. Die gesamte Gruppe virtueller Geräte wird in den Zustand „Abbruch“ versetzt. Von Geräten werden keine weiteren Befehle zurückgegeben. Alle nicht abgeschlossenen Befehle werden automatisch abgeschlossen. Als Abschlusscode wird ERROR_OPERATION_ABORTED zurückgegeben. Der Client sollte „IClientVirtualDeviceSet2::Close“ aufrufen, sobald ausstehende Vorgänge zur Verwendung von bereitgestellten Puffern sicher beendet wurden. Weitere Informationen finden Sie unter „Unplanmäßige Beendigung“.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Referenz zur Schnittstelle für virtuelle Geräte in SQL Server](reference-virtual-device-interface.md).