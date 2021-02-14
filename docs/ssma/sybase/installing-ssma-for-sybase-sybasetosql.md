---
title: Installieren von SSMA für SAP ASE (sybasedesql) | Microsoft-Dokumentation
description: Verwenden Sie diese Artikel zum Installieren, aktualisieren und Deinstallieren von SQL Server Migration Assistant für SAP ASE, darunter eine Client Anwendung und ein Erweiterungspaket.
ms.custom: ''
ms.date: 11/29/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 8d5a4ce6-b747-46e3-9184-645d56e8b35c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 8fca8219dfe89c8229b8d296fe355b97417d9daf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073364"
---
# <a name="installing-ssma-for-sap-ase-sybasetosql"></a>Installieren von SSMA für SAP ASE (sybasedesql)
[!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) für SAP Adaptive Server Enterprise (ASE) besteht aus einer Client Anwendung, mit der Sie eine Migration von SAP ASE zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Azure SQL-Datenbank durchführen. Außerdem enthält es ein Erweiterungspaket, das die Datenmigration und die Verwendung von ASE-Systemfunktionen in den migrierten Datenbanken unterstützt.  
  
Installieren Sie die Client Anwendung auf dem Computer, von dem aus Sie die Migrations Schritte ausführen möchten. Installieren Sie die Erweiterungspaket Dateien auf dem Computer, auf dem ausgeführt wird, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf dem die migrierten Datenbanken gehostet werden sollen.  
  
## <a name="upgrading-ssma-for-sap-ase"></a>Aktualisieren von SSMA für SAP ASE  
Wenn Sie ein Upgrade auf eine höhere Version von SSMA für SAP ASE durchführen möchten, müssen Sie zunächst den-Client und das Server Erweiterungspaket deinstallieren. Installieren Sie dann die neuere Version.  
  
Wenn Sie ein Projekt aus einer früheren Version von SSMA für SAP ASE öffnen, fragt SSMA, ob Sie das Projekt in die neuere Version konvertieren möchten. Klicken Sie auf **Ja** , um mit dem Projekt in der neueren Version von SSMA zu arbeiten.  
  
## <a name="contents"></a>Inhalte  
  
|Artikel|BESCHREIBUNG|  
|---------|---------------|  
|[Installieren von SSMA für SAP ASE Client &#40;sybasedesql&#41;](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md)|Enthält Informationen und Anweisungen zum Installieren von SSMA für den SAP ASE-Client.|  
|[Installieren von SSMA-Komponenten auf SQL Server &#40;sybasedesql-&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)|Enthält Informationen und Anweisungen zum Installieren des Erweiterungspakets auf Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|[Entfernen von SSMA für SAP ASE-Komponenten &#40;sybasedesql&#41;](../../ssma/sybase/removing-ssma-for-sybase-components-sybasetosql.md)|Enthält Anweisungen zum Deinstallieren des Client Programms und des Erweiterungspakets.|  
  
## <a name="see-also"></a>Weitere Informationen  
[Migrieren von SAP ASE-Datenbanken zu SQL Server Azure SQL-Datenbank &#40;sybaseto SQL-&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
