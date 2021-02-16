---
title: IClientVirtualDeviceSet2::MapBufferHandle
titlesuffix: SQL Server VDI reference
description: In diesem Artikel erhalten Sie Referenzinformationen zum Befehl „IClientVirtualDeviceSet2::MapBufferHandle“.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 28ce82433b718bc98c6a27e66385b97996fe65a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347218"
---
# <a name="iclientvirtualdeviceset2mapbufferhandle-vdi"></a>IClientVirtualDeviceSet2::MapBufferHandle (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Mit der Funktion **MapBufferHandle** wird eine gültige Pufferadresse aus einem Pufferhandle abgerufen, das wiederum aus einem anderen Prozess abgerufen wurde.

## <a name="syntax"></a>Syntax

```c
HRESULT IClientVirtualDeviceSet2::MapBufferHandle (
   DWORD      dwBuffer,
   BYTE**      ppBuffer
);
```

## <a name="parameters"></a>Parameter

*dwBuffer*: Handle, das von „IClientVirtualDeviceSet2::GetBufferHandle“ zurückgegeben wird.

*ppBuffer*: Adresse des Puffers, der im aktuellen Prozess gültig ist.

## <a name="return-value"></a>Rückgabewert

|Rückgabewert | Erklärung |
|---|---|
| NOERROR | Die Funktion wurde erfolgreich ausgeführt. |
| VD_E_PROTOCOL | Die Gruppe virtueller Geräte ist aktuell nicht geöffnet. |
| VD_E_INVALID | „ppBuffer“ ist ein ungültiges Handle. |

## <a name="remarks"></a>Bemerkungen

Die Handles müssen unbedingt korrekt übermittelt werden. da sie für eine einzelne Gruppe virtueller Geräte nur lokal verfügbar sind. Die Partnerprozesse, die ein Handle gemeinsam nutzen, müssen sicherstellen, dass Pufferhandles nur innerhalb des Bereichs der Gruppe virtueller Geräte verwendet werden, aus dem der Puffer ursprünglich abgerufen wurde.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Referenz zur Schnittstelle für virtuelle Geräte in SQL Server](reference-virtual-device-interface.md).