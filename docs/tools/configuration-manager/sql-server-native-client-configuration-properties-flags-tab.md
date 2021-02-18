---
title: SQL Server Native Client-Konfigurationseigenschaften (Registerkarte Flags)
description: Hier erfahren Sie mehr zu den Optionen auf der Registerkarte „Flags“ im Dialogfeld „SQL Server Native Client Configuration Properties“ (SQL Server Native Client-Konfigurationseigenschaften).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 59af121d-c8b9-4faa-91a1-b664f2c9b441
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 582c05b7dad6d952aeec4a2f9464759069159b2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349601"
---
# <a name="sql-server-native-client-configuration-properties-flags-tab"></a>SQL Server Native Client-Konfigurationseigenschaften (Registerkarte Flags)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Clients auf diesem Computer kommunizieren mithilfe der Protokolle der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client-Bibliotheksdatei mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Servern. Mithilfe dieser Seite wird der Clientcomputer so konfiguriert, dass die Anforderung einer verschlüsselten Verbindung mit Transport Layer Security (TLS), früher als Secure Sockets Layer (SSL) bezeichnet, möglich ist. Wenn keine verschlüsselte Verbindung hergestellt werden kann, führt dies zu einem Verbindungsfehler.  
  
 Der Anmeldeprozess ist immer verschlüsselt. Die unten genannten Optionen gelten nur für verschlüsselte Daten. Weitere Informationen dazu, wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Kommunikation verschlüsselt, und eine Anleitung zum Konfigurieren des Clients, sodass er der Stammzertifizierungsstelle des Serverzertifikats vertraut, finden Sie unter „Verschlüsseln von Verbindungen mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]“ und „Vorgehensweise: Aktivieren von verschlüsselten Verbindungen zum [!INCLUDE[ssDE](../../includes/ssde-md.md)] ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Konfigurations-Manager)" in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Onlinedokumentation.  
  
## <a name="options"></a>Tastatur  
 **ForceEncryption**  
 Fordern Sie eine Verbindung über TLS an.  
  
 **TrustServerCertificate**  
 Wenn diese Option auf **Nein** festgelegt ist, versucht der Clientprozess, das Serverzertifikat zu überprüfen. Client und Server müssen jeweils über ein Zertifikat von einer öffentlichen Zertifizierungsstelle verfügen. Wenn dieses Zertifikat auf dem Clientcomputer nicht vorhanden ist oder bei der Überprüfung des Zertifikats ein Fehler erzeugt wird, wird die Verbindung beendet.  
  
 Wenn diese Option auf **Ja** festgelegt ist, überprüft der Client das Serverzertifikat nicht. Dadurch wird die Verwendung eines selbstsignierten Zertifikats ermöglicht.  
  
 Das **TrustServerCertificate** -Flag ist nur verfügbar, wenn das **ForceEncryption** -Flag auf **Ja** festgelegt ist.  
  
  
