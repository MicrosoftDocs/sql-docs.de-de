---
title: Abschließen des Datenbank-Engine-Upgrades | Microsoft-Dokumentation
description: In diesem Artikel werden einige zusätzliche Schritte beschrieben, die Sie möglicherweise ausführen müssen, nachdem Sie ein Upgrade für die Datenbank-Engine von SQL Server durchgeführt haben.
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.technology: install
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 3f08087e-e532-416c-8caa-e0ec88c57596
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 5e4d727666ad2e3c013aacdabee93517389059bd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347831"
---
# <a name="complete-the-database-engine-upgrade"></a>Abschließen des Datenbank-Engine-Upgrades

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Nach Abschluss des Upgrades auf SQL Server gibt es eine Reihe zusätzlicher Schritte, die Sie möglicherweise durchführen müssen. Dabei handelt es sich z. B. um:  
  
Führen Sie nach dem Aktualisieren von [!INCLUDE[ssDE](../../includes/ssde-md.md)]die folgenden Aufgaben aus:  
  
- **Sichern Ihrer Datenbanken:** Führen Sie für jede Datenbank eine vollständige Sicherung durch.  

- **Neue Funktionen aktivieren:** In SQL Server 2016, 2017 und SQL Server 2019 treten einige Änderungen erst in Kraft, nachdem der DATABASE_COMPATIBILITY-Grad für eine Datenbank in 130 oder einen höheren Wert geändert wurde.  Weitere Informationen und den empfohlenen Workflow finden Sie unter [Ändern des Datenbank-Kompatibilitätsmodus und Verwenden des Abfragespeichers](../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md). Wenn Ihre Datenbank über speicheroptimierte Tabellen verfügt, die in SQL Server 2014 erstellt wurden, informieren Sie sich unter [Statistiken für speicheroptimierte Tabellen](../../relational-databases/in-memory-oltp/statistics-for-memory-optimized-tables.md).
  
- **Integration Services:**  
  
     Migrieren Sie die Integration Services-Pakete zum [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] -Format. Weitere Informationen finden Sie unter [Upgrade Integration Services Packages](../../integration-services/install-windows/upgrade-integration-services-packages.md).  
  
- **Reporting Services:** Für ein neues Installationsupgrade stellen Sie die Reporting Services-Verschlüsselungsschlüssel wieder her. Weitere Informationen finden Sie unter [Back Up and Restore Reporting Services Encryption Keys](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).  
  
- **Master Data Services:**  Führen Sie ein Upgrade des MDS-Datenbankschemas durch, und erstellen Sie die SQL Server 2019-Webanwendung. Weitere Informationen finden Sie unter [Upgrade Master Data Services](../../database-engine/install-windows/upgrade-master-data-services.md).  
  
- **Data Quality Services:** Führen Sie ein Upgrade des DQS-Datenbankschemas durch, und überprüfen Sie das Upgrade des DQS-Datenbankschemas. Weitere Informationen finden Sie unter [Upgrade Data Quality Services](../../database-engine/install-windows/upgrade-data-quality-services.md).  
  
- **Volltextsuche:** Füllen Sie die Volltextkataloge wieder auf, um eine konsistente Semantik in Abfrageergebnissen zu gewährleisten. Weitere Informationen finden Sie unter [Auffüllen von Volltextindizes](../../relational-databases/search/populate-full-text-indexes.md).  
  
