---
title: SQL Server-Agent-Eigenschaften (Registerkarte Anmelden)
description: In diesem Artikel erhalten Sie Informationen zur Registerkarte „Anmelden“ des Dialogfelds für Eigenschaften des SQL Server-Agents. Hier erfahren Sie, wie Sie diese verwenden, um ein Konto anzugeben und den Dienst zu starten oder anzuhalten.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 01fc6329-5d6b-4186-9565-395f375477bb
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: badbbdc6266e3e33e0e019a109ebb9f4af2a12d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345232"
---
# <a name="sql-server-agent-properties-log-on-tab"></a>SQL Server-Agent-Eigenschaften (Registerkarte Anmelden)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Verwenden Sie im Dialogfeld **SQL Server-Agent-Eigenschaften** die Registerkarte **Anmelden** , um das vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agentdienst verwendete Konto anzugeben und den Dienst zu starten und zu beenden. Das Ändern eines Kennworts für ein Konto wird sofort wirksam. Es ist kein Neustart des Dienstes erforderlich.  
  
> [!NOTE]  
>  Beim Ändern des Kontonamens, der von einem Dienst auf einer gruppierten Instanz verwendet wird, muss das neue Konto Mitglied der Domänengruppe sein, die während des Setups für den zu ändernden Dienst angegeben wird. Andernfalls müssen Sie die Berechtigung zum Hinzufügen von Mitgliedern zu dieser Gruppe besitzen. Wenden Sie sich an Ihren Domänenadministrator, falls Sie keine Berechtigung zum Ändern der Gruppenmitgliedschaft haben.  
  
## <a name="options"></a>Tastatur  
 **Lokales System**  
 Geben Sie ein lokales Systemkonto an, das kein Kennwort erfordert. Das lokale Systemkonto kann sich allerdings einschränkend auf die Zusammenarbeit mit anderen Servern auswirken. Dies hängt von den Privilegien ab, die dem Konto erteilt wurden.  
  
 **Dieses Konto**  
 Geben Sie ein lokales oder ein Domänenbenutzerkonto an, das die Windows-Authentifizierung verwendet. [!INCLUDE[msCoName](../../includes/msconame-md.md)] empfiehlt, ein Domänenbenutzerkonto mit minimalen Berechtigungen für Dienste zu verwenden. Informationen zum Auswählen eines Kontos finden Sie in der Onlinedokumentation unter "Einrichten von Windows-Dienstkonten".  
  
 **Kontoname**  
 Geben Sie den Kontonamen des lokalen oder Domänenbenutzers an.  
  
 **Kennwort**  
 Geben Sie das Kennwort des Kontos ein.  
  
 **Kennwort bestätigen**  
 Geben Sie das Kennwort des Kontos erneut ein.  
  
 **Starten**  
 Starten Sie den Dienst.  
  
 **Beenden**  
 Beenden Sie den Dienst.  
  
 **Anhalten**  
 Halten Sie den Dienst an.  
  
 **Fortsetzen**  
 Setzen Sie einen angehaltenen Dienst fort.  
  
  
