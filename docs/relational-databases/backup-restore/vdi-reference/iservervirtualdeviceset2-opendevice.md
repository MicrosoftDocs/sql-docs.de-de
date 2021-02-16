---
title: IServerVirtualDeviceSet2::OpenDevice
titlesuffix: SQL Server VDI reference
description: In diesem Artikel erhalten Sie Referenzinformationen zum Befehl „IServerVirtualDeviceSet2::OpenDevice“.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: d0da9263463fbe6c875bc411f88ec7f8ab440f46
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347072"
---
# <a name="iservervirtualdeviceset2opendevice-vdi"></a>IServerVirtualDeviceSet2::OpenDevice (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Mit der Funktion **OpenDevice** werden Schnittstellen virtueller Geräte aus der Gruppe virtueller Geräte abgerufen.

## <a name="syntax"></a>Syntax

```c
HRESULT IServerVirtualDeviceSet2::OpenDevice (
   LPCWSTR                     lpName,
   IServerVirtualDevice**      ppVirtualDevice
);
```

## <a name="parameters"></a>Parameter

*lpName*: Wird von der ersten VIRTUAL_DEVICE=-Klausel des BACKUP- oder RESTORE-Befehls bereitgestellt. Dieser Name wird als Schlüssel für den Zugriff auf die Gruppe virtueller Geräte verwendet, die vom Client erstellt wurde.

*ppVirtualDevice*: Wird für die Rückgabe der Schnittstelle eines virtuellen Geräts verwendet.

## <a name="return-value"></a>Rückgabewert

|Rückgabewert | Erklärung |
|---|---|
| NOERROR | Die Funktion wurde erfolgreich ausgeführt. |
| VD_E_OPEN |Alle Geräte wurden geöffnet. |

## <a name="remarks"></a>Bemerkungen

Mit jedem Aufruf wird das nächste nicht geöffnete Gerät zurückgegeben. Die Anzahl der möglichen Aufrufe der Funktion entspricht der Anzahl der Geräte, die in der Konfiguration der Gruppe virtueller Geräte angegeben wurde.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Referenz zur Schnittstelle für virtuelle Geräte in SQL Server](reference-virtual-device-interface.md).