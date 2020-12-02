---
title: IServerVirtualDeviceSet2::RequestBuffers
titlesuffix: SQL Server VDI reference
description: Dieser Artikel enthält Referenzinformationen zum IServerVirtualDeviceSet2::RequestBuffers-Befehl.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 50a99b29c5bc9e814f669f925115da0c8619c632
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2020
ms.locfileid: "96128854"
---
# <a name="iservervirtualdeviceset2requestbuffers-vdi"></a>IServerVirtualDeviceSet2::RequestBuffers (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Mit der **RequestBuffers**-Funktion wird die VDI darüber informiert, dass der Server eine bestimmte Anzahl von Puffern mit einer bestimmten Größen- und Ausrichtungsanforderung benötigt.

## <a name="syntax"></a>Syntax

```c
HRESULT IServerVirtualDeviceSet2::RequestBuffers (
   DWORD   dwSize,
   DWORD   dwAlignment,
   DWORD   dwCount
);
```

## <a name="parameters"></a>Parameter

*dwSize*: Gibt die Größe der einzelnen Puffer an. Diese Größe sollte nur den für die Daten benötigten Bereich umfassen. Die VDI übernimmt alle Ausrichtungs- und Präfixanforderungen.

*dwAlignment*: Die für diese Puffer erforderliche Ausrichtung. Dies kann ein restriktiverer Wert als der mit „BeginConfiguration“ angegebene Basisausrichtungswert sein. Wenn der Wert 0 ist, wird er standardmäßig auf den Basisausrichtungswert gesetzt.

*dwCount*: Die Anzahl der Puffer, die von „AllocateBuffer“ mit der festgelegten Größe und Ausrichtung angefordert werden.

## <a name="return-value"></a>Rückgabewert

|Rückgabewert | Erklärung |
|---|---|
| NOERROR | Die Funktion wurde erfolgreich ausgeführt. |
| VD_E_ABORT | Ein Abbruch wurde angefordert. |
| VD_E_PROTOCOL | Der Satz weist keinen Status auf, in dem Pufferzuweisungen deklariert werden können (siehe Statusübergangsmatrix). |
| VD_E_MEMORY | Der angeforderte Arbeitsspeicher konnte nicht abgerufen werden. |

## <a name="remarks"></a>Bemerkungen

Diese Methode sollte verwendet werden, bevor Puffer mit „AllocateBuffer“ zugewiesen werden. Mehrere Puffer mit einer bestimmten Größe und Ausrichtung werden mit der „RequestBuffers“-Funktion angefordert, und anschließend werden einzelne Puffer mit „AllocateBuffer“ zugewiesen.

Während der Konfigurationsphase werden die „RequestBuffers“-Aufrufe „zusammengefasst“, sodass beim „EndConfiguration“-Aufruf ein einzelner Pufferbereich verwendet werden kann (er wird dann zugewiesen). Nach Abschluss der Konfiguration führen alle „RequestBuffer“-Aufrufe zu einer sofortigen Zuweisung von mehr Pufferspeicher.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Referenz zur Schnittstelle für virtuelle Geräte in SQL Server](reference-virtual-device-interface.md).