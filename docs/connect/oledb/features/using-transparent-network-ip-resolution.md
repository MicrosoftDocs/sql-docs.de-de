---
description: Verwenden der transparenten Netzwerk-IP-Adressauflösung
title: Verwenden der transparenten Netzwerk-IP-Adressauflösung | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/02/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: chris-rossi
ms.author: v-chross
ms.openlocfilehash: 944c51fd392ffb684cf2c2fc02967b040163a9d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351120"
---
# <a name="using-transparent-network-ip-resolution"></a>Verwenden der transparenten Netzwerk-IP-Adressauflösung
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

## <a name="purpose"></a>Zweck
Die transparente Netzwerk-IP-Adressauflösung (Transparent Network IP Resolution, TNIR) ist eine Überarbeitung des vorhandenen MultiSubnetFailover-Features. TNIR wirkt sich auf die Verbindungssequenz des Treibers aus, wenn die erste aufgelöste IP-Adresse des Hostnamens nicht reagiert und dem Hostnamen mehrere IP-Adressen zugeordnet sind. TNIR interagiert mit MultiSubnetFailover, um die folgenden drei Verbindungssequenzen bereitzustellen:<br />
* 0: Erst wird eine IP-Adresse ausprobiert, dann alle IP-Adressen gleichzeitig.
* 1: Die IP-Adressen werden alle gleichzeitig ausprobiert.
* 2: Die IP-Adressen werden alle nacheinander ausprobiert.

|TransparentNetworkIPResolution|MultiSubnetFailover|Verhalten|
|--------|--------|--------|
|True|True|1|
|Richtig|False|0|
|False|True|1|
|False|False|2|

## <a name="setting-transparent-network-ip-resolution"></a>Festlegen der transparenten Netzwerk-IP-Adressauflösung
TransparentNetworkIPResolution ist in der Standardeinstellung aktiviert. MultiSubnetFailover ist in der Standardeinstellung deaktiviert. Auf den folgenden Seiten finden Sie weitere Informationen zum Festlegen dieser Eigenschaften: 
- [Verwenden von Verbindungszeichenfolgen-Schlüsselwörtern mit dem OLE DB-Treiber für SQL Server](..\applications\using-connection-string-keywords-with-oledb-driver-for-sql-server.md)
- [Initialisierungs- und Autorisierungseigenschaften](..\ole-db-data-source-objects\initialization-and-authorization-properties.md)

## <a name="see-also"></a>Weitere Informationen 
[OLE DB-Treiber für SQL Server-Unterstützung für Hochverfügbarkeit, Notfallwiederherstellung](./oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)
