---
description: Abonnententypen
title: Abonnententypen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.subscribertypes.f1
ms.assetid: a70656cb-21c9-4489-be77-ccd396747e3b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 5e557028d035ae9e04f52eedfdef10d66d3ba2d7
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88493851"
---
# <a name="subscriber-types"></a>Abonnententypen
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  Mithilfe der Mergereplikation können Sie die Typen der Abonnenten angeben, die eine Veröffentlichung unterstützen muss. Die Auswahl der Abonnententypen legt den *Veröffentlichungskompatibilitätsgrad*fest, der bestimmt, welche Funktionen von einer Veröffentlichung verwendet werden können.  
  
 Nachdem eine Veröffentlichungsmomentaufnahme erstellt wurde, kann der Veröffentlichungskompatibilitätsgrad im Dialogfeld **Veröffentlichungseigenschaften** auf der Seite **Allgemein** gesteigert (restriktiver gemacht) werden. Der Veröffentlichungskompatibilitätsgrad kann nicht gesenkt werden.  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
  
## <a name="options"></a>Optionen  
 Wählen Sie jeden Abonnententypen aus, der von der Veröffentlichung unterstützt werden muss.  
  
 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]  
 Die Veröffentlichung kann alle Funktionen verwenden.  
  
 [!INCLUDE[ssEW](../../includes/ssew-md.md)]  
 Momentaufnahmedateien für die Veröffentlichung müssen ein Zeichenformat aufweisen (dies wird automatisch vom Momentaufnahme-Agent gehandhabt). Für[!INCLUDE[ssEW](../../includes/ssew-md.md)] gilt auch eine Reihe von Einschränkungen ohne Bezug zum Kompatibilitätsgrad.  
  
 Wenn diese Option ausgewählt ist, dann ist die Option zur Websynchronisierung für die Veröffentlichung aktiviert. Weitere Informationen zur Websynchronisierung finden Sie unter [Web Synchronization for Merge Replication](../../relational-databases/replication/web-synchronization-for-merge-replication.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Veröffentlichen von Daten und Datenbankobjekten](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Anzeigen und Ändern der Verteiler- und Verlegereigenschaften](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
  
  
