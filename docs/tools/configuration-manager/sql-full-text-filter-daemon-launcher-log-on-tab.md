---
title: Startprogramm für SQL-Volltextfilterdaemon (Registerkarte Anmelden)
description: Hier erfahren Sie mehr über das Startprogramm für SQL-Volltextfilterdaemon, das von der SQL Server-Volltextsuche verwendet wird. Außerdem erhalten Sie Informationen zur Registerkarte „Anmelden“ im Dialogfeld „Eigenschaften“ des Programms.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 13e260f9-a75f-430b-88a3-959ddcead8fe
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 08224bf7ed6ec8a6ba2c5b4ccd8cc88bea4bd22c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346042"
---
# <a name="sql-full-text-filter-daemon-launcher-log-on-tab"></a>Startprogramm für SQL-Volltextfilterdaemon (Registerkarte Anmelden)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Ab [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]wird der Dienst SQL-Volltextfilterdaemon (FDHOST-Startprogramm) von der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Volltextsuche verwendet. Dieser Dienst muss ausgeführt werden, wenn Sie die Volltextsuche verwenden. Informationen über die Prozesse des Filterdaemonhosts finden Sie unter "Architektur der Volltextsuche" in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Onlinedokumentation.  
  
 Verwenden Sie im Dialogfeld **Eigenschaften von Startprogramm für SQL-Volltextfilterdaemon** die Registerkarte **Anmelden** , um das vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Volltextdienst verwendete Konto anzugeben, das Kennwort des Kontos zu ändern sowie den Dienst zu starten und zu beenden. Das Ändern eines Kennworts für ein Konto wird nach einem Neustart des Diensts wirksam.  
  
> [!NOTE]  
>  Beim Ändern des Kontonamens, der von einem Dienst auf einer gruppierten Instanz verwendet wird, muss das neue Konto Mitglied der Domänengruppe sein, die während des Setups für den zu ändernden Dienst angegeben wird, oder Sie müssen die Berechtigung zum Hinzufügen von Mitgliedern zu dieser Gruppe besitzen. Wenden Sie sich an Ihren Domänenadministrator, falls Sie keine Berechtigung zum Ändern der Gruppenmitgliedschaft haben.  
>   
>  Weitere Informationen über das Auswählen eines Kontos zum Ausführen des Dienstes finden Sie unter "Einrichten von Windows-Dienstkonten" in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Onlinedokumentation.  
  
## <a name="options"></a>Tastatur  
 **Integriertes Konto**  
 **Lokales System**  
 Geben Sie das lokale Systemkonto an. Dieses Konto erfordert kein Kennwort. Das lokale Systemkonto kann die Zusammenarbeit mit anderen Servern verhindern. Dies hängt von den Privilegien ab, die dem Konto erteilt wurden.  
  
 **Lokaler Dienst**  
 Geben Sie das lokale Dienstkonto an. Dieses Konto erfordert kein Kennwort. Das lokale Dienstkonto kann die Zusammenarbeit mit anderen Servern verhindern. Dies hängt von den Privilegien ab, die dem Konto erteilt wurden.  
  
 **Netzwerkdienst**  
 Für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] - oder die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent-Dienste empfiehlt sich die Verwendung des Netzwerkdienstkontos. Lokale Benutzerkonten oder Domänenbenutzerkonten sind für die Dienste besser geeignet.  
  
 **Dieses Konto**  
 Geben Sie ein lokales oder ein Domänenbenutzerkonto an, das die Windows-Authentifizierung verwendet. Das Verwenden eines Domänenbenutzerkontos mit minimalen Berechtigungen für Dienste wird empfohlen.  
  
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
 Halten Sie den Dienst an. Für diesen Dienst nicht verfügbar.  
  
 **Fortsetzen**  
 Setzen Sie einen angehaltenen Dienst fort.  
  
  
