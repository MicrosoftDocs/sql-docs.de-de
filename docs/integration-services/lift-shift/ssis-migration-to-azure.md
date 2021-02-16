---
title: Übersicht über die Migration von SQL Server Integration Services zu Azure | Microsoft-Dokumentation
description: In diesem Artikel werden der Migrationsprozess von SQL Server Integration Services zu Azure sowie die dafür erforderlichen Tools beschrieben.
ms.date: 04/10/2020
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f443bc4209365253cf6e4e4005d19afe9ee11268
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061465"
---
# <a name="migrate-on-premises-ssis-workloads-to-ssis-in-adf"></a>Migrieren von lokalen SSIS-Workloads zu SSIS in ADF

Dieser Artikel enthält Informationen zum Migrationsprozess von SQL Server Integration Services-Workloads (SSIS) zu SSIS in ADF sowie die erforderlichen Tools.

In der [Übersicht zur Migration](/azure/data-factory/scenario-ssis-migration-overview) wird der gesamte Migrationsprozess Ihrer ETL-Workloads von lokalen SSIS zu SSIS in ADF beschrieben.

Der Migrationsprozess besteht aus zwei Phasen: [Bewertung](/azure/data-factory/scenario-ssis-migration-overview#assessment) und [Migration](/azure/data-factory/scenario-ssis-migration-overview#migration).

## <a name="assessment"></a>Bewertung

Der Datenmigrations-Assistent (DMA) ist ein zu diesem Zweck frei herunterladbares Tool, das lokal installiert und ausgeführt werden kann. Das DMA-Bewertungsprojekt vom Typ „Integration Services“ kann erstellt werden, um SSIS-Pakete in Batches zu bewerten und Kompatibilitätsprobleme zu identifizieren.

Hier können Sie den [Datenbankmigrations-Assistenten](../../dma/dma-overview.md) abrufen und eine [Paketbewertung](../../dma/dma-assess-ssis.md) durchführen.

## <a name="migration"></a>Migration

In Abhängigkeit von den Speichertypen der SSIS-Quellpakete und dem Migrationsziel der Datenbankworkloads können die Schritte für die Migration von SSIS-Paketen und SQL Server-Agent-Aufträgen variieren, die die Ausführung von SSIS-Paketen planen. Weitere Informationen finden Sie auf [dieser Seite](/azure/data-factory/scenario-ssis-migration-overview#migration).

## <a name="next-steps"></a>Nächste Schritte

- [Migrieren von SSIS-Paketen zu Azure SQL Managed Instance](/azure/dms/how-to-migrate-ssis-packages-managed-instance)
- [Migrieren von SSIS-Aufträgen zu Azure Data Factory (ADF) mit SQL Server Management Studio (SSMS)](/azure/data-factory/how-to-migrate-ssis-job-ssms)