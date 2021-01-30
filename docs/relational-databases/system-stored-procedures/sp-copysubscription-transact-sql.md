---
description: sp_copysubscription (Transact-SQL)
title: sp_copysubscription (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_copysubscription
- sp_copysubscription_TSQL
helpviewer_keywords:
- sp_copysubscription
ms.assetid: 3c56cd62-2966-4e87-a986-44cb3fd0b760
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b7377048df6d32715b8f91e26b5b21617c22a0d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192013"
---
# <a name="sp_copysubscription-transact-sql"></a>sp_copysubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

    
> [!IMPORTANT]  
>  Die Funktion für anfügbare Abonnements ist als veraltet markiert und wird in einer zukünftigen Version entfernt. Diese Funktion sollte bei neuen Entwicklungen nicht verwendet werden. Für Mergeveröffentlichungen, die mithilfe von parametrisierten Filtern partitioniert werden, ist es empfehlenswert, die neuen Funktionen von partitionierten Momentaufnahmen zu verwenden. Diese vereinfachen die Initialisierung zahlreicher Abonnements. Weitere Informationen finden Sie unter [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md). Für nicht partitionierte Veröffentlichungen können Sie ein Abonnement mit einer Sicherung initialisieren. Weitere Informationen finden Sie unter [Initialize a Transactional Subscription Without a Snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md)initialisiert wird.  
  
 Kopiert eine Abonnementdatenbank, die über Pullabonnements, nicht jedoch über Pushabonnements verfügt. Es können nur einzelne Dateidatenbanken kopiert werden. Diese gespeicherte Prozedur wird auf dem Abonnenten für die Abonnement Datenbank ausgeführt.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```  
  
sp_copysubscription [ @filename = ] 'file_name'  
    [ , [ @temp_dir = ] 'temp_dir' ]  
    [ , [ @overwrite_existing_file = ] overwrite_existing_file]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @filename = ] 'file_name'` Die Zeichenfolge, die den kompletten Pfad, einschließlich des Datei namens, angibt, in dem eine Kopie der Datendatei (. mdf) gespeichert wird. der *Dateiname ist vom Datentyp* **nvarchar (260)** und hat keinen Standardwert.  
  
`[ @temp_dir = ] 'temp_dir'` Der Name des Verzeichnisses, das die temporären Dateien enthält. *temp_dir* ist vom Datentyp **nvarchar (260)** und hat den Standardwert NULL. Wenn der Wert NULL ist, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird das Standarddaten Verzeichnis verwendet. Das Verzeichnis sollte über ausreichenden Speicherplatz verfügen, um eine Datei aufzunehmen, die der Größe aller Datenbankdateien auf dem Abonnenten zusammen entspricht.  
  
`[ @overwrite_existing_file = ] 'overwrite_existing_file'`Ist ein optionales boolesches Flag, das angibt, ob eine vorhandene Datei mit demselben Namen überschrieben werden soll, die in **\@ filename** angegeben ist. *overwrite_existing_file* ist vom Typ **Bit**. der Standardwert ist **0**. Wenn der Wert **1** ist, wird die durch **\@ filename** angegebene Datei überschrieben, sofern vorhanden. Wenn der Wert **0** ist, schlägt die gespeicherte Prozedur fehl, wenn die Datei vorhanden ist, und die Datei wird nicht überschrieben.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **0** (Erfolg) oder **1** (Fehler)  
  
## <a name="remarks"></a>Bemerkungen  
 **sp_copysubscription** wird bei allen Replikations Typen verwendet, um eine Abonnement Datenbank als Alternative zum Anwenden einer Momentaufnahme auf dem Abonnenten in eine Datei zu kopieren. Die Datenbank muss so konfiguriert sein, dass ausschließlich Pullabonnements unterstützt werden. Benutzer mit entsprechenden Berechtigungen können Kopien der Abonnementdatenbank erstellen und die Abonnementdatei (MSF) dann per E-Mail, durch Kopieren oder Übertragen an einen anderen Abonnenten senden. Dort kann die Datei dann als Abonnement angefügt werden.  
  
 Die Größe der kopierten Abonnementdatenbank muss weniger als 2 Gigabyte (GB) betragen.  
  
 **sp_copysubscription** wird nur für Datenbanken mit Client Abonnements unterstützt und kann nicht ausgeführt werden, wenn die Datenbank über Server Abonnements verfügt.  
  
## <a name="permissions"></a>Berechtigungen  
 Nur Mitglieder der festen Server Rolle **sysadmin** können **sp_copysubscription** ausführen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Alternative Speicherorte für Momentaufnahme Ordner](../../relational-databases/replication/snapshot-options.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
