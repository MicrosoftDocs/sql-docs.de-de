---
title: IClientVirtualDeviceSet2::CreateEx
titlesuffix: SQL Server VDI reference
description: Dieser Artikel enthält Referenzinformationen zum IClientVirtualDeviceSet2::CreateEx-Befehl.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: b65dfc635b69519aae4198844a166373234559b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347246"
---
# <a name="iclientvirtualdeviceset2createex-vdi"></a>IClientVirtualDeviceSet2::CreateEx (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Die **CreateEx**-Funktion erstellt die Gruppe virtueller Geräte.

## <a name="syntax"></a>Syntax

```c
HRESULT IClientVirtualDeviceSet2::CreateEx (
   LPCWSTR         lpInstanceName,
   LPCWSTR         lpName,
   VDConfig*      pCfg
);
```

## <a name="parameters"></a>Parameter

*lpInstanceName*: Diese Zeichenfolge gibt die SQL Server-Instanz an, an die der SQL-Befehl gesendet wird.

*IpName*: Gibt die Gruppe virtueller Geräte an. Die Benennungsregeln, die von „CreateFileMapping()“ verwendet werden, müssen beachtet werden. Jedes Zeichen außer dem umgekehrten Schrägstrich (\) kann verwendet werden. Dies ist eine Unicode-Zeichenfolge für breite Zeichen. Es wird empfohlen, der Zeichenfolge den Produkt- oder Firmennamen des Benutzers und den Datenbanknamen voranzustellen.

*pCfg*: Dieses Argument ist die Konfiguration für die Gruppe virtueller Geräte. Weitere Informationen finden Sie unter „Konfiguration“.

## <a name="return-value"></a>Rückgabewert

|Rückgabewert | Erklärung |
|---|---|
| NOERROR | Die Funktion wurde erfolgreich ausgeführt. |
| VD_E_NOTSUPPORTED | Mindestens ein Feld in der Konfiguration war ungültig oder wurde nicht unterstützt. |
| VD_E_PROTOCOL | Die Gruppe virtueller Geräte wurde erstellt. |

## <a name="remarks"></a>Bemerkungen

Die „CreateEx“-Methode sollte nur einmal pro BACKUP- oder RESTORE-Vorgang aufgerufen werden. Nachdem die „Close“-Methode aufgerufen wurde, kann der Client die Schnittstelle wiederverwenden, um eine weitere Gruppe virtueller Geräte zu erstellen.

Der Instanzname muss die Instanz identifizieren, an die die Transact-SQL-Sprache ausgegeben wird. NULL gibt die Standardinstanz an. Das Präfix „machineName“ wird nicht akzeptiert.

Die „CreateEx“- (und „Create“)-Aufrufe ändern die Sicherheits-DACL für das Prozesshandle im Clientprozess. Aus diesem Grund muss jede andere Änderung des Prozesshandles mit dem Aufruf von „CreateEx“ serialisiert werden. Dabei wird „CreateEx“ mit anderen Aufrufen von „CreateEx“ serialisiert, kann aber nicht mit der externen Verarbeitung serialisiert werden. Der Zugriff wird dem Konto gewährt, das den SQL Server-Dienst ausführt.

Die „CreateEx“-Methode ersetzt die im ursprünglichen IClientVirtualDeviceSet-Befehl definierte „Create“-Methode. Die ursprüngliche „Create“-Methode ist veraltet und sollte bei künftigen Entwicklungen nicht mehr verwendet werden. Mit dieser ursprünglichen „Create“-Methode wird mit der Umgebungsvariablen _VIRTUAL_SERVER_NAME_ eine Art Unterstützung für Instanznamen implementiert. Wenn diese Variable in der Umgebung festgelegt ist, ruft die „Create“-Methode intern die „CreateEx“-Methode auf und übergibt den Wert von _VIRTUAL_SERVER_NAME_ als Instanzname.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der [Referenz zur Schnittstelle für virtuelle Geräte in SQL Server](reference-virtual-device-interface.md).