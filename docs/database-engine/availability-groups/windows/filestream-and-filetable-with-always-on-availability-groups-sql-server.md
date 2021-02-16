---
title: Verwenden von FILESTREAM und Dateitabellen mit Verfügbarkeitsgruppen
description: In diesem Artikel wird die Verwendung von FILESTREAM oder Dateitabellen mit Datenbanken beschrieben, die Teil einer Always On-Verfügbarkeitsgruppe sind.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- FileTables [SQL Server], Availability Groups
- FILESTREAM [SQL Server], Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: fdceda9a-a9db-4d1d-8745-345992164a98
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 7019f87f5b6476c273a20008fb6d081f816a4e2e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353873"
---
# <a name="use-filestream-and-filetable-with-always-on-availability-groups"></a>Verwenden von FILESTREAM und Dateitabellen mit Always On-Verfügbarkeitsgruppen

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  Dieses Thema enthält Informationen zur Verwendung der FILESTREAM- und FileTable-Funktionen mit [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
 Alle FILESTREAM-Funktionen werden unterstützt. Nach einem Failover kann sowohl auf lesbaren sekundären Replikaten als auch auf dem neuen primären Replikat auf FILESTREAM-Daten zugegriffen werden.  
  
 Die FileTable-Funktionalität wird teilweise unterstützt. Nach einem Failover kann auf dem primären Replikat auf FileTable-Daten zugegriffen werden, nicht jedoch auf FileTable-Daten auf lesbaren sekundären Replikaten.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
  
-   Bevor Sie eine Datenbank, die FILESTREAM mit oder ohne FileTable verwendet, zu einer Verfügbarkeitsgruppe hinzufügen, stellen Sie sicher, dass FILESTREAM auf jeder Serverinstanz, die ein Verfügbarkeitsreplikat für die Verfügbarkeitsgruppe hostet, aktiviert worden ist. Weitere Informationen finden Sie unter [Enable and Configure FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md).  
  
##  <a name="using-virtual-network-names-vnns-for-filestream-and-filetable-access"></a><a name="vnn"></a> Verwenden von virtuellen Netzwerknamen (VNNs) für den Zugriff auf FILESTREAM- und FileTable-Daten  
 Wenn Sie FILESTREAM auf einer Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]aktivieren, wird eine Freigabe auf Instanzebene erstellt, um Zugriff auf die FILESTREAM-Daten zu gewähren. Sie greifen auf diese Freigabe zu, indem Sie den Computernamen im folgende Format angeben:  
  
 `\\<computer_name>\<filestream_share_name>`  
  
 In einer Always On-Verfügbarkeitsgruppe wird der Name des Computers jedoch mit einem virtuellen Netzwerknamen (kurz VNN) virtualisiert. Wenn der Computer das primäre Replikat in einer Verfügbarkeitsgruppe ist und Datenbanken in der Verfügbarkeitsgruppe FILESTREAM-Daten enthalten, dann wird auch eine Freigabe im Bereich des VNNs erstellt, um Zugriff auf die FILESTREAM-Daten zu bieten. Dies wirkt sich nicht auf den Transact-SQL-Zugriff auf FILESTREAM-Daten aus. Anwendungen, die Dateisystem-APIs verwenden, müssen jedoch die Freigabe im Bereich des VNNs verwenden. Diese Freigabe hat einen Pfad im folgenden Format:  
  
 `\\<VNN>\<filestream_share_name>`  
  
 Diese Freigabe im VNN-Bereich wird erstellt, wenn eines der folgenden Ereignisse auftritt.  
  
-   Sie fügen eine Datenbank hinzu, die FILESTREAM-Daten zu einer Always On-Verfügbarkeitsgruppe für das primäre Replikat enthält. In diesem Fall ist die Freigabe `\\<computer_name>\<filestream_share_name>` bereits vorhanden. Die Freigabe `\\<VNN>\<filestream_share_name>` wird erstellt.  
  
-   Sie aktivieren FILESTREAM für E/A-Streamingzugriff auf Datei für ein primäres Replikat, das Verfügbarkeitsgruppen enthält. Die folgenden Freigaben werden erstellt:  
  
    1.  `\\<computer_name>\<filestream_share_name>`  
  
    2.  `\\<VNN1>\<filestream_share_name>` für die Verfügbarkeitsgruppe 1.  
  
    3.  `\\<VNN2>\<filestream_share_name>` für die Verfügbarkeitsgruppe 2.  
  
 Diese Freigaben im VNN-Bereich werden auch an alle sekundären Replikate weitergegeben.  
  
 Wenn die Datenbank, die FILESTREAM- oder FileTable-Daten enthält, zu einer Always On-Verfügbarkeitsgruppe gehört:  
  
-   Die FILESTREAM-Funktion und die FileTable-Funktion akzeptieren oder geben virtuelle Netzwerknamen (VNNs) statt Computernamen zurück. Weitere Informationen zu diesen Funktionen finden Sie unter [Filestream- und FileTable-Funktionen &#40;Transact-SQL&#41;](../../../relational-databases/system-functions/filestream-and-filetable-functions-transact-sql.md).  
  
-   Bei allen Zugriffen auf FILESTREAM- oder FileTable-Daten über Dateisystem-APIs sollten VNNs statt der Computernamen verwendet werden.  
  
 Wenn die Datenbank Teil einer Verfügbarkeitsgruppe ist und die Anwendung versucht, mit einem Computernamen im Format `\\<computer_name>\<filestream_share_name>` auf die Freigabe zuzugreifen, dann wird ein Fehler ausgelöst.  
  
 Wenn die Datenbank kein Teil einer Verfügbarkeitsgruppe ist und die Anwendung versucht, auf die Freigabe mit einem Pfad im VNN-Bereich zuzugreifen, dann wird die Anforderung möglicherweise erfolgreich ausgeführt. In diesem Fall wird der virtuelle Netzwerkname in den Computernamen aufgelöst. Von dieser Vorgehensweise wird jedoch stark abgeraten, da der Pfad im VNN-Bereich nicht mehr gültig ist, wenn die Verfügbarkeitsgruppe gelöscht wird.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Aktivieren und Konfigurieren von FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md)  
  
-   [Aktivieren der erforderlichen Komponenten für FileTable](../../../relational-databases/blob/enable-the-prerequisites-for-filetable.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
 Keine.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht zu AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
