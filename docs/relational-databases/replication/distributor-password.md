---
description: Verteilerkennwort
title: Verteilerkennwort | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.configuredistributionwizard.distributorpassword.f1
ms.assetid: 52787c5e-c9ef-440e-a000-0787111b7dbb
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 6593e4065e4ea40f0befb7bb236db5c99a54461e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484872"
---
# <a name="distributor-password"></a>Verteilerkennwort
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Wenn Sie auf der Seite **Verleger** des Assistenten mindestens einen Verleger für die Verwendung dieses Servers als Remoteverteiler aktivieren, müssen Sie für die von der Replikation hergestellte Verbindung zwischen dem Verleger und dem Remoteverteiler mithilfe des Anmeldenamens **distributor_admin** ein Kennwort angeben. Dieses Kennwort muss auch für jeden Verleger eingegeben werden, von dem der Verteiler über die Seite **Administratorkennwort** des Assistenten für neue Veröffentlichung oder des Verteilungskonfigurations-Assistenten verwendet wird. Weitere Informationen zur Sicherheit für Verteiler finden Sie unter [Schützen des Verteilers](../../relational-databases/replication/security/secure-the-distributor.md).  
  
## <a name="options"></a>Optionen  
 **Kennwort**  
 Geben Sie für die Verbindung zwischen dem Verleger und dem Remoteverteiler ein sicheres Kennwort ein.  
  
 **Kennwort bestätigen**  
 Geben Sie das Kennwort zur Bestätigung der fehlerfreien Eingabe erneut ein.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verteilung konfigurieren](../../relational-databases/replication/configure-distribution.md)   
 [Konfigurieren der Veröffentlichung und der Verteilung](../../relational-databases/replication/configure-publishing-and-distribution.md)  
  
  
