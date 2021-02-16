---
title: IClientVirtualDeviceSet2::OpenInSecondaryEx
titlesuffix: SQL Server VDI reference
description: In diesem Artikel erhalten Sie Referenzinformationen zum Befehl „IClientVirtualDeviceSet2::OpenInSecondaryEx“.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 18e53141365aa4581cef05c14d35cf53c8f24387
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347215"
---
# <a name="iclientvirtualdeviceset2openinsecondaryex-vdi"></a>IClientVirtualDeviceSet2::OpenInSecondaryEx (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Mit der Funktion **OpenInSecondaryEx** wird die Gruppe virtueller Geräte auf einem sekundären Client geöffnet. Der primäre Client muss vorher bereits „CreateEx“ und „GetConfiguration“ verwendet haben, um die Gruppe virtueller Geräte einzurichten.

## <a name="syntax"></a>Syntax

```c
HRESULT IClientVirtualDeviceSet2::OpenInSecondaryEx (
   LPCWSTR      lpInstanceName,
   LPCWSTR      lpSetName
);
```

## <a name="parameters"></a>Parameter

*lpInstanceName*: Diese Zeichenfolge gibt die SQL Server-Instanz an, an die der SQL-Befehl gesendet wird.

*lpSetName*: Mit diesem Argument wird die Gruppe identifiziert. Die Groß-/Kleinschreibung dieses Namens wird beachtet. Der Name muss außerdem mit dem Namen übereinstimmen, der vom primären Client beim Aufruf von „IClientVirtualDeviceSet2::Create“ verwendet wurde.

## <a name="return-value"></a>Rückgabewert

|Rückgabewert | Erklärung |
|---|---|
| NOERROR | Die Funktion wurde erfolgreich ausgeführt. |
| VD_E_PROTOCOL | Die Gruppe virtueller Geräte wurde geöffnet oder akzeptiert keine Öffnungsanforderungen von sekundären Clients. |
| VD_E_ABORT | Der Vorgang wird abgebrochen. |

## <a name="remarks"></a>Bemerkungen

Wenn ein Modell mit mehreren Prozessen genutzt wird, ist der primäre Client dafür verantwortlich, eine planmäßige und unplanmäßige Beendigung der sekundären Clients zu erkennen.

Der Instanzname muss die Instanz identifizieren, an die die T-SQL-Sprache ausgegeben wird. NULL gibt die Standardinstanz an. Das Präfix „machineName“ wird nicht akzeptiert.

„OpenInSecondaryEx“ setzt das ursprüngliche „IClientVirtualDeviceSet::OpenInSecondary“-Objekt, das in der ursprünglichen SQL Server-Schnittstelle der Version 7.0 definiert wurde, außer Kraft. Bei neuen Entwicklungen sollte „OpenInSecondaryEx“ verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Referenz zur Schnittstelle für virtuelle Geräte in SQL Server](reference-virtual-device-interface.md).