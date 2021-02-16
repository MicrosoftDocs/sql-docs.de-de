---
title: IServerVirtualDeviceSet2::GetConfiguration
titlesuffix: SQL Server VDI reference
description: In diesem Artikel erhalten Sie Referenzinformationen zum Befehl „IServerVirtualDeviceSet2::GetConfiguration“.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: d0b2b89a9ac7415c80927e4cedd28a2530c7c5bc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347109"
---
# <a name="iservervirtualdeviceset2getconfiguration-vdi"></a>IServerVirtualDeviceSet2::GetConfiguration (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Die Funktion **GetConfiguration** ruft die vom Client angeforderte Konfiguration ab.

## <a name="syntax"></a>Syntax

```c
HRESULT IServerVirtualDeviceSet2::GetConfiguration (
   VDConfig*   pCfg
);
```

## <a name="parameters"></a>Parameter

*pCfg*: Konfiguration, die vom Client mithilfe von „IClientVirtualDeviceSet2::Create“ angegeben wird.

## <a name="return-value"></a>Rückgabewert

Gibt *HRESULT* zurück, wodurch der Erfolg oder das Fehlschlagen des Methodenaufrufs angegeben wird. Der Wert „NOERROR“ gibt an, dass der Methodenaufruf erfolgreich war. Ein Wert ungleich 0 (null) gibt an, dass ein Fehler aufgetreten ist.

## <a name="remarks"></a>Bemerkungen

Der Server soll die vom Client bereitgestellten Einstellungen überprüfen und darauf reagieren. Weitere Informationen finden Sie unter „Konfiguration“. Der Server kann „SignalAbort“ verwenden, wenn er feststellt, dass ein ordnungsgemäßes Arbeiten mit der bereitgestellten Konfiguration nicht möglich ist.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Referenz zur Schnittstelle für virtuelle Geräte in SQL Server](reference-virtual-device-interface.md).