---
description: MSSQLSERVER_2577
title: MSSQLSERVER_2577 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 2577 (Database Engine error)
ms.assetid: f53256a2-2fb0-47fd-9ed9-c45389104145
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f19f46f5876b413987598301f727250a6129f24c
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88482760"
---
# <a name="mssqlserver_2577"></a>MSSQLSERVER_2577
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Details  
  
| attribute | Wert |  
| :-------- | :---- |  
|Produktname|SQL Server|  
|Ereignis-ID|2577|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|SQLEngine|  
|Symbolischer Name|DBCC_IAM_CHAIN_SEQUENCE_OUT_OF_ORDER|  
|Meldungstext|Die Kettensequenznummern in der IAM-Kette (Index Allocation Map) für Objekt-ID O_ID, Index-ID I_ID, Partitions-ID PN_ID, Zuordnungseinheits-ID A_ID (TYPE-Typ) weisen nicht die richtige Reihenfolge auf. Die Seite P_ID1 mit der Sequenznummer SEQUENCE1 zeigt auf die Seite P_ID2 mit der Sequenznummer SEQUENCE2.|  
  
## <a name="explanation"></a>Erklärung  
Jede IAM-Seite (Index Allocation Map) hat eine Sequenznummer. Die Sequenznummer gibt die Position der IAM-Seite in der IAM-Kette an. Die Regel besagt, dass Sequenznummern für jede IAM-Seite um eins erhöht werden. IAM-Seite *P_ID2* hat eine Sequenznummer, die dieser Regel nicht entspricht.  
  
## <a name="user-action"></a>Benutzeraktion  
  
### <a name="look-for-hardware-failure"></a>Hardwarefehlersuche  
Führen Sie eine Hardwarediagnose aus, und beheben Sie alle Probleme. Überprüfen Sie auch das Systemprotokoll und das Anwendungsprotokoll von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows sowie das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll, um festzustellen, ob der Fehler aufgrund eines Hardwarefehlers aufgetreten ist. Beheben Sie alle hardwarebedingten Probleme, die in den Protokollen enthalten sind.  
  
Lagern Sie verschiedene Hardwarekomponenten aus, um das Problem zu isolieren, falls Beschädigungsprobleme bei permanenten Daten auftreten. Stellen Sie sicher, dass beim System der Schreibcache auf dem Datenträgercontroller nicht aktiviert ist. Wenden Sie sich an Ihren Hardwarehersteller, falls Sie beim Schreibcache das Problem vermuten.  
  
Letztendlich kann es vorteilhaft sein, wenn Sie zu einem neuen Hardwaresystem wechseln. Der Wechsel kann die Neuformatierung der Laufwerke und eine Neuinstallation des Betriebssystems beinhalten.  
  
### <a name="restore-from-backup"></a>Sicherungswiederherstellung  
Stellen Sie die Datenbank aus der Sicherung wieder her, wenn das Problem nicht hardwarebezogen ist und eine bekannte intakte Sicherungskopie vorhanden ist.  
  
### <a name="run-dbcc-checkdb"></a>Ausführen von DBCC CHECKDB  
Führen Sie DBCC CHECKDB ohne eine REPAIR-Klausel aus, um das Ausmaß der Beschädigung festzustellen, falls keine fehlerfreie Sicherung verfügbar ist. DBCC CHECKDB empfiehlt die Verwendung einer REPAIR-Klausel. Führen Sie dann DBCC CHECKDB mit der entsprechenden REPAIR-Klausel aus, um die Beschädigung zu reparieren.  
  
> [!CAUTION]  
> Wenden Sie sich an Ihren primären Unterstützungsanbieter, bevor Sie diese Anweisung ausführen, wenn Sie nicht sicher sind, inwiefern sich die Verwendung von DBCC CHECKDB mit einer REPAIR-Klausel auf Ihre Daten auswirkt.  
  
Wenn DBCC CHECKDB mit einer der REPAIR-Klauseln ausgeführt wird und das Problem nicht behebt, wenden Sie sich an Ihren primären Unterstützungsanbieter.  
  
### <a name="results-of-running-repair-options"></a>Ergebnis der Ausführung von REPAIR-Optionen  
Durch das Ausführen von REPAIR wird die IAM-Kette neu erstellt. REPAIR teilt die vorhandene IAM-Kette zunächst in zwei Hälften. Die erste Hälfte der Kette endet mit IAM-Seite *P_ID1*. Der Zeiger für die nächste Seite der Seite *P_ID1* wird auf (0:0) festgelegt. Die zweite Hälfte der Kette beginnt mit IAM-Seite *P_ID2*. Der Zeiger für die vorherige Seite der Seite *P_ID2* wird auf (0:0) festgelegt.  
  
REPAIR verbindet dann die beiden Hälften der Kette und generiert die Sequenznummern für die IAM-Kette neu. Zuordnungen von IAM-Seiten, die nicht repariert werden können, werden aufgehoben.  
  
> [!CAUTION]  
> Diese Reparatur führt möglicherweise zum Datenverlust.  
  
