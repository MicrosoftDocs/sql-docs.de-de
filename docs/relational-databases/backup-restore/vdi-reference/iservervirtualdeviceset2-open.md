---
title: IServerVirtualDeviceSet2::Open
titlesuffix: SQL Server VDI reference
description: In diesem Artikel erhalten Sie Referenzinformationen zum Befehl „IServerVirtualDeviceSet2::Open“.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0e4d91e75ee2c030112a1e826366d5caee537ee3
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2020
ms.locfileid: "96128858"
---
# <a name="iservervirtualdeviceset2open-vdi"></a>IServerVirtualDeviceSet2::Open (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Die Funktion **Open** öffnet die Gruppe virtueller Geräte auf dem Server; dies muss der erste Aufruf des COM-bereitgestellten Schnittstellenhandles sein.

## <a name="syntax"></a>Syntax

```c
HRESULT IServerVirtualDeviceSet2::Open (
   LPCWSTR      lpInstanceName,
   LPCWSTR      lpName
);
```

## <a name="parameters"></a>Parameter

*lpInstanceName*: Diese Zeichenfolge gibt die SQL Server-Instanz an, an die der SQL-Befehl gesendet wird. NULL kann zur Identifizierung der Standardinstanz auf dem aktuellen Computer übergeben werden.

*lpName*: Wird von der ersten VIRTUAL_DEVICE=-Klausel des BACKUP- oder RESTORE-Befehls bereitgestellt. Dieser Name wird als Schlüssel für den Zugriff auf die Gruppe virtueller Geräte verwendet, die vom Client erstellt wurde.

## <a name="return-value"></a>Rückgabewert

|Rückgabewert | Erklärung |
|---|---|
| NOERROR | Die Funktion wurde erfolgreich ausgeführt. |
| VD_E_INVALID | Der angegebene Name hat keine Gruppe virtueller Geräte identifiziert, auf den der Server zugreifen kann. |

## <a name="remarks"></a>Bemerkungen

Nachdem diese Funktion erfolgreich aufgerufen wurde, kann der Server die Konfiguration der Gruppe virtueller Geräte mithilfe von "GetConfiguration" und "SetConfiguration" fortsetzen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Referenz zur Schnittstelle für virtuelle Geräte in SQL Server](reference-virtual-device-interface.md).