---
title: IServerVirtualDevice::CloseDevice
titlesuffix: SQL Server VDI reference
description: In diesem Artikel erhalten Sie Referenzinformationen zum Befehl „IServerVirtualDevice::CloseDevice“.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7034c517483a114d9b1a0be4fbf15f80fd8801dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347171"
---
# <a name="iservervirtualdeviceclosedevice-vdi"></a>IServerVirtualDevice::CloseDevice (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Mit der Funktion **CloseDevice** wird ein virtuelles Gerät geschlossen, das mit „IServerVirtualDeviceSet2::OpenDevice“ geöffnet wurde.

## <a name="syntax"></a>Syntax

```c
HRESULT IServerVirtualDevice::CloseDevice ();
```

## <a name="return-value"></a>Rückgabewert

|Rückgabewert | Erklärung |
|---|---|
| VD_E_CLOSE | Das Gerät ist bereits geschlossen. |
| VD_E_ABORT | Die Schnittstelle befindet sich im Zustand „Abbruch“. |

## <a name="remarks"></a>Bemerkungen

Wurde „SignalAbort“ zum Erzwingen einer unplanmäßigen Beendigung verwendet, ist die Verwendung von „CloseDevice“ nicht mehr erforderlich. Wenn „CloseDevice“ nach der Verwendung von „SignalAbort“ aufgerufen wird, wird keine Aktion ausgeführt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Referenz zur Schnittstelle für virtuelle Geräte in SQL Server](reference-virtual-device-interface.md).