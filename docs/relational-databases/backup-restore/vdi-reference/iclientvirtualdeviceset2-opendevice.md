---
title: IClientVirtualDeviceSet2::OpenDevice
titlesuffix: SQL Server VDI reference
description: In diesem Artikel erhalten Sie Referenzinformationen zum Befehl „IClientVirtualDeviceSet2::OpenDevice“.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: f6abf203001af56c11bb8d1cee3f233877fb8d51
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347265"
---
# <a name="iclientvirtualdeviceset2opendevice-vdi"></a>IClientVirtualDeviceSet2::OpenDevice (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Die Funktion **OpenDevice** öffnet eines der Geräte in der Gruppe virtueller Geräte.

## <a name="syntax"></a>Syntax

```c
HRESULT IClientVirtualDeviceSet2::OpenDevice (
   LPCWSTR                  lpName,
   IClientVirtualDevice**   ppVirtualDevice

);
```

## <a name="parameters"></a>Parameter

*IpName*: Gibt das virtuelle Gerät an.

*ppVirtualDevice*: Wenn die Funktion erfolgreich ausgeführt wird, wird ein Schnittstellenzeiger auf das virtuelle Gerät zurückgegeben. Diese Schnittstelle wird für „GetCommand“ und „CompleteCommand“ verwendet.

## <a name="return-value"></a>Rückgabewert

|Rückgabewert | Erklärung |
|---|---|
| NOERROR | Die Funktion wurde erfolgreich ausgeführt. |
| VD_E_ABORT | Ein Abbruch wurde angefordert. |
| VD_E_OPEN |Alle Geräte sind geöffnet. |
| VD_E_PROTOCOL | Die Gruppe befindet sich nicht im Initialisierungszustand, oder das Gerät ist bereits geöffnet. |
| VD_E_INVALID | Der Gerätename ist ungültig. Er lässt sich keinem Gerät der Gruppe zuordnen. |

## <a name="remarks"></a>Bemerkungen

VD_E_OPEN kann möglicherweise ohne Probleme zurückgegeben werden. Der Client kann „OpenDevice“ mithilfe einer Schleife aufrufen, bis dieser Code zurückgegeben wird.
Wenn mehr als ein Gerät konfiguriert ist (z. B. n Geräte), werden von der Gruppe virtueller Geräte n eindeutige Geräteschnittstellen zurückgegeben. Das erste Gerät hat den gleichen Namen wie die Gruppe der virtuellen Geräte. Andere Geräte werden wie in den VIRTUAL_DEVICE-Klauseln der BACKUP/RESTORE-Anweisung angegeben benannt.

Die Funktion „GetConfiguration“ kann verwendet werden, um solange zu warten, bis sich die Geräte öffnen lassen.

Wenn diese Funktion nicht erfolgreich ausgeführt wird, wird ein NULL-Wert über „ppVirtualDevice“ zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Referenz zur Schnittstelle für virtuelle Geräte in SQL Server](reference-virtual-device-interface.md).