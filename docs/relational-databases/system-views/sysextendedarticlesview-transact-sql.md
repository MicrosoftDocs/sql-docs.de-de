---
description: sysextendedarticlesview (Transact-SQL)
title: sysextendedarticlesview (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysextendedarticlesview_TSQL
- sysextendedarticlesview
dev_langs:
- TSQL
helpviewer_keywords:
- sysextendedarticlesview view
ms.assetid: 8bdd22f7-c268-49b6-820c-3fe603feb128
author: stevestein
ms.author: sstein
ms.openlocfilehash: 4eb45e9ab8113b6a66f6f7b5650546c2d6d7f06e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160313"
---
# <a name="sysextendedarticlesview-transact-sql"></a>sysextendedarticlesview (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **sysextendedarticlesview** -Sicht enthält Informationen zu veröffentlichten Artikeln. Diese Sicht wird in der distribution-Datenbank gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Die Identitätsspalte, die eine eindeutige ID für den Artikel bereitstellt.|  
|**creation_script**|**nvarchar(255)**|Das Schemaerstellungsskript für den Artikel|  
|**del_cmd**|**nvarchar(255)**|Der bei einer DELETE-Anweisung auszuführende Befehl, wird sonst aus dem Protokoll konstruiert|  
|**description**|**nvarchar(255)**|Der Beschreibungseintrag für den Artikel.|  
|**dest_table**|**nvarchar(128)**|Der Name der Zieltabelle|  
|**filter**|**int**|Der Objektbezeichner der gespeicherten Prozedur, die für horizontale Partitionen verwendet wird|  
|**filter_clause**|**ntext**|Die WHERE-Klausel des Artikels, die für horizontales Filtern verwendet wird.|  
|**ins_cmd**|**nvarchar(255)**|Der bei einer INSERT-Anweisung auszuführende Befehl|  
|**name**|**nvarchar(128)**|Der mit dem Artikel verknüpfte Name, der innerhalb der Veröffentlichung eindeutig ist.|  
|**objid**|**int**|Die Objekt-ID der veröffentlichten Tabelle.|  
|**pubid**|**int**|Die ID der Veröffentlichung, zu der der Artikel gehört.|  
|**pre_creation_cmd**|**tinyint**|Der Voraberstellungsbefehl für DROP TABLE, DELETE TABLE oder TRUNCATE:<br /><br /> **0** = keine.<br /><br /> **1** = DROP.<br /><br /> **2** = DELETE.<br /><br /> **3** = TRUNCATE.|  
|**status**|**int**|Die Bitmaske der Artikeloptionen und der Status, die das Ergebnis des bitweisen logischen OR von mindestens einem der folgenden Werte sein können:<br /><br /> **1** = Artikel ist aktiv.<br /><br /> **8** = Den Spaltennamen in INSERT-Anweisungen einschließen.<br /><br /> **16** = Parametrisierte Anweisungen verwenden.<br /><br /> **24** = Sowohl den Spaltennamen in INSERT-Anweisungen einschließen als auch parametrisierte Anweisungen verwenden.<br /><br /> Ein aktiver Artikel, der parametrisierte Anweisungen verwendet, würde in dieser Spalte beispielsweise den Wert 17 anzeigen. Der Wert 0 gibt an, dass der Artikel inaktiv ist und keine zusätzlichen Eigenschaften definiert wurden.|  
|**sync_objid**|**int**|Die ID der Tabelle oder Sicht, die die Artikeldefinition darstellt.|  
|**type**|**tinyint**|Der Artikeltyp:<br /><br /> **1** = Protokollbasierter Artikel.<br /><br /> **3** = Protokollbasierter Artikel mit manuell erstelltem Filter.<br /><br /> **5** = Protokollbasierter Artikel mit manuell erstellter Sicht.<br /><br /> **7** = Protokollbasierter Artikel mit manuell erstelltem Filter und manuell erstellter Sicht.|  
|**upd_cmd**|**nvarchar(255)**|Der bei einer UPDATE-Anweisung auszuführende Befehl, wird sonst aus dem Protokoll konstruiert|  
|**schema_option**|**binary**|Gibt an, welche Eigenschaften des veröffentlichten Objekts in der Momentaufnahme ausgegeben wurden.  Eine Liste der unterstützten Schema Optionen finden Sie unter [sp_addarticle](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).|  
|**dest_owner**|**nvarchar(128)**|Der Besitzer der Tabelle in der Zieldatenbank|  
|**ins_scripting_proc**|**int**|Der Objektbezeichner der benutzerdefinierten gespeicherten Prozedur oder des Skripts, die bzw. das beim Replizieren einer INSERT-Anweisung ausgeführt wird|  
|**del_scripting_proc**|**int**|Der Objektbezeichner der benutzerdefinierten gespeicherten Prozedur oder des Skripts, die bzw. das beim Replizieren einer DELETE-Anweisung ausgeführt wird|  
|**upd_scripting_proc**|**int**|Der Objektbezeichner der benutzerdefinierten gespeicherten Prozedur oder des Skripts, die bzw. das beim Replizieren einer UPDATE-Anweisung ausgeführt wird|  
|**custom_script**|**int**|Der Objektbezeichner des benutzerdefinierten Skripts oder der Prozedur, die bzw. das nach Ausführung eines DDL-Triggers ausgeführt wird|  
|**fire_triggers_on_snapshot**|**int**|Gibt an, ob replizierte Trigger beim Anwenden der Momentaufnahme ausgeführt werden. Es werden folgende Werte unterstützt:<br /><br /> **0** = Trigger werden nicht ausgeführt.<br /><br /> **1** = Trigger werden ausgeführt.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikations Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addarticle &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sysarticles &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md)  
  
  
