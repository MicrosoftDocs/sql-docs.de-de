---
description: Filtereinstellungen
title: Filtereinstellungen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.monitor.filtersettings.f1
ms.assetid: 1b401d7d-db8a-4ba1-acb1-b8dec14e3311
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 74ddf2a67970c290d427e777184eba74af28a3d1
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88486672"
---
# <a name="filter-settings"></a>Filtereinstellungen
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Im Dialogfeld **Filtereinstellungen** können Sie Filter für Raster im Replikationsmonitor definieren. Wenn Sie z. B. nur die aktiven Abonnements auf der Registerkarte **Alle Abonnements** anzeigen möchten, wählen Sie in der Spalte **Spaltenname** den Eintrag **Status** , in der Spalte **Operator** den Eintrag **Ist gleich** und in der Spalte **Wert1** den Eintrag **Aktiv** aus. Wenn Sie einen auf einer oder mehreren Spalten basierenden Filter definiert haben, wird dieser Filter angewendet, sodass im Raster nur die Teilmenge von Zeilen angezeigt wird, die die Filterkriterien erfüllen.  
  
## <a name="options"></a>Optionen  
 **Spaltenname**  
 Wählen Sie den Namen der Spalte aus, für die Sie einen Filter definieren möchten. Sie können einen Filter für eine oder mehrere Spalten definieren.  
  
 **Operator**  
 Wählen Sie für den Filter einen Operator wie **Kleiner als oder gleich**aus.  
  
 **Wert1** und **Wert2**  
 Geben Sie einen Wert für den Filter ein, oder wählen Sie einen Wert aus. Für die meisten Operatoren muss nur in der Spalte **Wert1** ein Wert eingegeben werden. Für die Operatoren **Zwischen** und **Nicht zwischen** muss jedoch auch in der Spalte **Wert2** ein Wert eingegeben werden.  
  
 **Filter löschen**  
 Klicken Sie auf diese Schaltfläche, um alle definierten Filter zu löschen. Wenn Sie einen einzelnen Filter entfernen möchten, wählen Sie die Filterzeile aus, und drücken Sie die ENTF-TASTE.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Überwachen der Replikation](../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  
