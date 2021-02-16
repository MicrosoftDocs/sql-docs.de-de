---
title: IServerVirtualDeviceSet2::EndConfiguration
titlesuffix: SQL Server VDI reference
description: In diesem Artikel erhalten Sie Referenzinformationen zum Befehl „IServerVirtualDeviceSet2::EndConfiguration“.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 522a2edd0a4b8ffb30cdffc9fc271f879f8d336c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347207"
---
# <a name="iservervirtualdeviceset2endconfiguration-vdi"></a>IServerVirtualDeviceSet2::EndConfiguration (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Mit der Funktion **EndConfiguration** wird die VDI darüber informiert, dass der Server die Konfiguration beendet hat.

## <a name="syntax"></a>Syntax

```c
HRESULT IServerVirtualDeviceSet2::EndConfiguration ();
```

## <a name="return-value"></a>Rückgabewert

|Rückgabewert | Erklärung |
|---|---|
| NOERROR | Die Funktion wurde erfolgreich ausgeführt. |
| VD_E_ABORT | Ein Abbruch wurde angefordert. |
| VD_E_PROTOCOL | Die Gruppe befindet sich nicht im Zustand „Konfigurierbar“. |
| VD_E_MEMORY | Der über die RequestBuffers-Aufrufe angeforderte Arbeitsspeicher konnte nicht abgerufen werden. Die Gruppe bleibt im Zustand „Konfigurierbar“ ohne verfügbaren Pufferspeicher. Der Server kann entweder die Pufferanforderungen reduzieren oder den Vorgang abbrechen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Referenz zur Schnittstelle für virtuelle Geräte in SQL Server](reference-virtual-device-interface.md).