---
description: sysssispackages (Transact-SQL)
title: sysssispackages (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysdtspackages90_TSQL
- sysdtspackages90
dev_langs:
- TSQL
helpviewer_keywords:
- sysssispackages system table
ms.assetid: 66155dcd-dcdb-4e33-a242-1625828ad8d2
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 31f1833808260508f29a858381d258f21ebd744a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186661"
---
# <a name="sysssispackages-transact-sql"></a>sysssispackages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Enthält eine Zeile für jedes Paket, das in gespeichert wird [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Diese Tabelle wird in der **msdb** -Datenbank gespeichert.  
  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Der eindeutige Bezeichner des Pakets.|  
|**id**|**uniqueidentifier**|GUID des Pakets|  
|**description**|**nvarchar**|Eine optionale Beschreibung des Pakets.|  
|**mit "up"**|**datetime**|Erstellungsdatum des Pakets|  
|**folderid**|**uniqueidentifier**|Die GUID des logischen Ordners, in dem das Paket von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] aufgelistet ist.|  
|**ownersid**|**varbinary**|Die eindeutige Sicherheits-ID des Benutzers, der das Paket erstellt hat|  
|**packagedata**|**image**|Das Paket.|  
|**packageformat**|**int**|Das Format, in dem das Paket gespeichert wird:<br /><br /> Der Wert 2 gibt an, dass das Paket im-Format gespeichert wird [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .<br /><br /> Der Wert 3 gibt an, dass das Paket im Format [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] oder später gespeichert wird.|  
|**PackageType**|**int**|Der Client, der das Paket erstellt hat. Die folgenden Werte sind möglich:<br /><br /> 0 (Standardwert)<br /><br /> 1 ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Import/Export-Assistent)<br /><br /> 3 ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Replikation)<br /><br /> 5 ( [!INCLUDE[ssIS](../../includes/ssis-md.md)] Designer)<br /><br /> 6 (Wartungsplan-Designer oder -Assistent).<br /><br /> <br /><br /> Beachten Sie, dass die Werte in dieser Spalte der- <xref:Microsoft.SqlServer.Dts.Runtime.DTSPackageType> Enumeration entsprechen.|  
|**vermajor**|**int**|Die aktuelle Hauptversion des Pakets.|  
|**verminor**|**int**|Die aktuelle Nebenversion des Pakets.|  
|**verbuild**|**int**|Das letzte Build des Pakets.|  
|**vercomments**|**nvarchar**|Kommentare zur Paketversion|  
|**verid**|**uniqueidentifier**|GUID der Paketversion|  
|**IsEncrypted**|**bit**|Ein boolescher Wert, der angibt, ob das Paket verschlüsselt ist|  
|**readrolesid**|**varbinary**|Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rolle, die Pakete laden kann|  
|**writerolesid**|**varbinary**|Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Rolle, die Pakete speichern kann|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Integration Services &#40;SSIS&#41; Packages](../../integration-services/integration-services-ssis-packages.md) (Integration Services-Pakete [SSIS])  
  
  
