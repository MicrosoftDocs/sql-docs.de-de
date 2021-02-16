---
title: IClientVirtualDevice::GetCommand
titlesuffix: SQL Server VDI reference
description: In diesem Artikel erhalten Sie Referenzinformationen zum Befehl „IClientVirtualDevice::GetCommand“.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: f57e2b71e99f0d4fcc1370aa6da0bcd5310c6a13
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347248"
---
# <a name="iclientvirtualdevicegetcommand-vdi"></a>IClientVirtualDevice::GetCommand (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Mit der Funktion **GetCommand** wird der nächste Befehl abgerufen, der sich in der Warteschlange eines Geräts befindet. Wenn diese Funktion aufgerufen wird, wartet sie auf den nächsten Befehl.

## <a name="syntax"></a>Syntax

```c
HRESULT IClientVirtualDevice::GetCommand (
   DWORD               dwTimeOut,
   VDC_Command**      const ppCmd
);
```

## <a name="parameters"></a>Parameter

*ppCmd*: Wird ein Befehl erfolgreich zurückgegeben, gibt der Parameter die Adresse eines auszuführenden Befehls zurück. Der zurückgegebene Speicher ist schreibgeschützt. Nachdem der Befehl abgeschlossen wurde, wird dieser Zeiger der „CompleteCommand“-Routine übergeben. Weitere Informationen zu den einzelnen Befehlen finden Sie unter „Befehle“.

*dwTimeOut*: Wartezeit in Millisekunden. Mit INFINTE legen Sie eine unbegrenzte Wartezeit fest. Mit 0 rufen Sie einen Befehl ab. „VD_E_TIMEOUT“ wird zurückgegeben, wenn aktuell kein Befehl verfügbar ist. Wenn der Timeout auftritt, entscheidet der Client, welche Aktion als Nächstes ausgeführt wird.

## <a name="return-value"></a>Rückgabewert

|Rückgabewert | Erklärung |
|---|---|
| NOERROR | Ein Befehl wurde abgerufen. |
| VD_E_CLOSE | Das Gerät wurde vom Server geschlossen. |
| VD_E_TIMEOUT | Es war kein Befehl verfügbar, und der Timeout ist abgelaufen. |
| VD_E_ABORT | Der Client oder Server hat mit „SignalAbort“ das Herunterfahren erzwungen. |

## <a name="remarks"></a>Bemerkungen

„VD_E_CLOSE“ wird zurückgegeben, wenn SQL Server das Gerät geschlossen hat. Dieser Vorgang findet während des regulären Herunterfahrens statt. Nachdem alle Geräte geschlossen wurden, ruft der Client „IClientVirtualDeviceSet2::Close“ auf, um die Gruppe virtueller Geräte zu schließen.

Wenn diese Routine blockiert werden muss, um auf einen Befehl zu warten, wird der Thread in den Warnzustand versetzt.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Referenz zur Schnittstelle für virtuelle Geräte in SQL Server](reference-virtual-device-interface.md).