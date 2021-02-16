---
title: IServerVirtualDeviceSet2::FreeBuffer
titlesuffix: SQL Server VDI reference
description: In diesem Artikel erhalten Sie Referenzinformationen zum Befehl „IServerVirtualDeviceSet2::FreeBuffer“.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: d5cdd5fade247458526795279302a20ffd7919da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347135"
---
# <a name="iservervirtualdeviceset2freebuffer-vdi"></a>IServerVirtualDeviceSet2::FreeBuffer (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Mit der Funktion **FreeBuffer** wird ein freigegebener Speicherpuffer aus der Gruppe virtueller Geräte abgerufen.

## <a name="syntax"></a>Syntax

```c
HRESULT IServerVirtualDeviceSet2::FreeBuffer (
   LPVOID   pBuffer,
   UINT32   dwSize
);
```

## <a name="parameters"></a>Parameter

*pBuffer*: Es wird ein von „IServerVirtualDeviceSet2::AllocateBuffer“ zurückgegebener Puffer zurückgegeben.

*dwSize*: Größe des Puffers in Bytes. Dies gilt nicht für Präfixzonen, die vom Client angefordert werden. Diese werden auf dem Server ausgeblendet, und vor dem Zurückgeben der Adresse des Puffers steht Speicherplatz zur Verfügung.

## <a name="return-value"></a>Rückgabewert

|Rückgabewert | Erklärung |
|---|---|
| NOERROR | Der Puffer wird zurückgegeben. |
| VD_E_INVALID | Ein Parameter war ungültig. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Referenz zur Schnittstelle für virtuelle Geräte in SQL Server](reference-virtual-device-interface.md).