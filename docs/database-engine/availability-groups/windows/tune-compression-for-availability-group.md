---
title: Optimieren der Komprimierung für die Verfügbarkeitsgruppe | Microsoft-Dokumentation
description: Hier erfahren Sie, wie SQL Server Datenströme für Verfügbarkeitsgruppen komprimiert, wodurch der Netzwerkdatenverkehr reduziert, die CPU-Auslastung erhöht und möglicherweise Wartezeiten ausgelöst werden.
ms.custom: ''
ms.date: 06/22/2016
ms.prod: sql
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: 7632769c-b246-4766-886f-7c60ec540be8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 86e85a392a57eb4782be21becb8c06fa405516b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337914"
---
# <a name="tune-compression-for-availability-group"></a>Optimieren der Komprimierung für die Verfügbarkeitsgruppe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
Standardmäßig werden Datenströme für Verfügbarkeitsgruppen gegebenenfalls von SQL Server komprimiert. Die Komprimierung reduziert den Netzwerkverkehr, steigert die CPU-Auslastung und kann die Latenz erhöhen. Sie müssen Mitglied der festen Serverrolle „sysadmin“ sein, um die Komprimierung zu aktivieren. Die folgende Tabelle zeigt, wann SQL Server Protokolldatenströme von Verfügbarkeitsgruppen komprimiert:

| Szenario | Komprimierungseinstellung
| ---- | ----
| Replikat für synchrone Commits | Nicht komprimiert
| Replikat für asynchrone Commits | Compressed
| Während des automatischen Seedings | Nicht komprimiert
| Datenbank mit TDE aktiviert  | Nicht komprimiert

## <a name="trace-flags-for-availability-group-compression"></a>Ablaufverfolgungsflags für Verfügbarkeitsgruppenkomprimierung 

In den meisten Szenarien empfiehlt Microsoft, diese Einstellungen nicht zu ändern. Sie können globale Ablaufverfolgungsflags verwenden, um Änderungen an diesen Einstellungen zu testen. SQL Server wendet globale Ablaufverfolgungsflags auf die gesamte Instanz an. Alle Verfügbarkeitsgruppen in der Instanz sind von diesen Einstellungen betroffen.  

Die folgende Tabelle zeigt Ablaufverfolgungsflags, die das Standardkomprimierungsverhalten für SQL Server ändern. 

Ablaufverfolgungsflag | BESCHREIBUNG
------------- | -------------
1462          | Deaktiviert die Protokolldatenstrom-Komprimierung für Verfügbarkeitsgruppen mit asynchronen Replikaten. Dieses Feature ist für asynchrone Replikate standardmäßig aktiviert, um die Netzwerkbandbreite zu optimieren.
9567          | Aktiviert die Komprimierung des Datenstroms für Verfügbarkeitsgruppen während des automatischen Seedings. Während des automatischen Seedings kann die Übertragungszeit durch die Komprimierung erheblich reduziert und die Arbeitslast für den Prozessor erhöht werden.
9592          | Aktiviert die Protokolldatenstrom-Komprimierung für Verfügbarkeitsgruppen mit synchronen Replikaten. Dieses Feature ist für synchrone Replikate standardmäßig deaktiviert, da die Komprimierung die Latenz erhöht. Für asynchrone Replikate ist die Protokolldatenstrom-Komprimierung standardmäßig aktiviert.


## <a name="resources"></a>Ressourcen


[Startoptionen für die Datenbank-Engine](../../../database-engine/configure-windows/database-engine-service-startup-options.md)

[Automatisches Seeding](./automatically-initialize-always-on-availability-group.md)

[Voraussetzungen für Always On](prereqs-restrictions-recommendations-always-on-availability.md)