---
author: MikeRayMSFT
ms.prod: sql
ms.technology: big-data-cluster
ms.topic: include
ms.date: 06/22/2020
ms.author: mikeray
ms.openlocfilehash: 599b4072c5d03c8a4753c7c00bc7edc14e0f3b05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038935"
---
Beginnend mit SQL Server 2019 CU5 verwenden alle Endpunkte einschließlich Gateway `AZDATA_USERNAME` und `AZDATA_PASSWORD`, wenn Sie einen neuen Cluster mit Standardauthentifizierung bereitstellen. Endpunkte auf Clustern, die ein Upgrade auf CU5 erhalten, verwenden weiterhin `root` als Benutzername für die Verbindung mit dem Gatewayendpunkt. Diese Änderung gilt nicht für Bereitstellungen, die die Active Directory-Authentifizierung verwenden. Weitere Informationen finden Sie unter [Anmeldeinformationen für den Zugriff auf Dienste über den Gatewayendpunkt](../big-data-cluster/release-notes-big-data-cluster.md#credentials-for-accessing-services-through-gateway-endpoint) in den Versionshinweisen.