---
description: QN:Parameter Table (Ereignisklasse)
title: 'QN: Parameter Table (Ereignisklasse) | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- event classes [SQL Server], QN:Parameter Table
ms.assetid: 292da1ed-4c7e-4bd2-9b84-b9ee09917724
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: daec049469ed46d92267f7f9fe1abd1cde336b8b
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88494339"
---
# <a name="qnparameter-table-event-class"></a>QN:Parameter Table (Ereignisklasse)
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  Das QN:Parameter Table-Ereignis übermittelt Informationen zu Vorgängen, die notwendig sind, um interne Tabellen, in denen Parameterinformationen gespeichert sind, zu erstellen und abzulegen und den Verweiszähler zu erhalten. Dieses Ereignis übermittelt außerdem die interne Aktivität zum Zurücksetzen des Verwendungszählers für eine Parametertabelle.  
  
## <a name="qnparameter-table-event-class-data-columns"></a>Datenspalten der QN:Parameter Table-Ereignisklasse  
  
|Datenspalte|type|BESCHREIBUNG|Spaltennummer|Filterbar|  
|-----------------|----------|-----------------|-------------------|----------------|  
|ApplicationName|**nvarchar**|Der Name der Clientanwendung, die die Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]hergestellt hat. Diese Spalte wird mit den Werten aufgefüllt, die von der Anwendung übergeben werden, und nicht mit dem angezeigten Namen des Programms.|10|Ja|  
|ClientProcessID|**int**|Die ID, die der Hostcomputer dem Prozess zuweist, in dem die Clientanwendung ausgeführt wird. Diese Datenspalte wird aufgefüllt, wenn die Clientprozess-ID durch den Client bereitgestellt wird.|9|Ja|  
|DatabaseID|**int**|Die ID der Datenbank, die durch die USE *database* -Anweisung angegeben wurde, bzw. die ID der Standarddatenbank, wenn für eine bestimmte Instanz keine USE *database*-Anweisung ausgegeben wurde. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] zeigt den Namen der Datenbank an, wenn die Server Name-Datenspalte in der Ablaufverfolgung aufgezeichnet wird und der Server verfügbar ist. Der Wert für eine Datenbank kann mithilfe der DB_ID-Funktion ermittelt werden.|3|Ja|  
|DatabaseName|**nvarchar**|Der Name der Datenbank, in der die Benutzeranweisung ausgeführt wird.|35|Ja|  
|EventClass|**Int**|Ereignistyp = 200.|27|Nein|  
|EventSequence|**int**|Die Sequenznummer für dieses Ereignis.|51|Nein|  
|EventSubClass|**nvarchar**|Der Typ der Ereignisunterklasse, der weitere Informationen zu jeder Ereignisklasse liefert. Diese Spalte kann die folgenden Werte enthalten:<br /><br /> **Table created**: Gibt an, dass in der Datenbank eine Parametertabelle erstellt wurde.<br /><br /> **Table drop attempt**: Gibt an, dass die Datenbank versucht hat, automatisch eine nicht verwendete Parametertabelle zu löschen, um Ressourcen frei zu geben.<br /><br /> **Table drop attempt failed**: Gibt an, dass die Datenbank versucht hat, eine nicht verwendete Parametertabelle zu löschen, der Löschvorgang aber nicht durchgeführt werden konnte. [!INCLUDE[ssDE](../../includes/ssde-md.md)] plant die Löschung der Parametertabelle automatisch neu, um Ressourcen frei zu geben.<br /><br /> **Table dropped**: Gibt an, dass die Datenbank erfolgreich eine Parametertabelle gelöscht hat.<br /><br /> **Table pinned**: Gibt an, dass die Parametertabelle für die aktuelle Verwendung durch die interne Verarbeitung markiert ist.<br /><br /> **Table unpinned**: Gibt an, dass die Fixierung der Tabelle aufgehoben wurde. Die Tabelle wird von der internen Verarbeitung nicht mehr verwendet.<br /><br /> **Number of users incremented**: Gibt an, dass sich die Anzahl von Abfragebenachrichtigungsabonnements erhöht hat, die auf eine Parametertabelle verweisen.<br /><br /> **Number of users decremented**: Gibt an, dass sich die Anzahl von Abfragebenachrichtigungsabonnements verringert hat, die auf eine Parametertabelle verweisen.<br /><br /> **LRU counter reset**: Gibt an, dass der Verwendungszähler für die Parametertabelle zurückgesetzt wurde.<br /><br /> **Cleanup task started**: Gibt an, wann mit dem Cleanup aller Abonnements in dieser Parametertabelle begonnen wurde. Dieser Vorgang findet statt, wenn die Datenbank gestartet oder eine den Abonnements dieser Parametertabelle zugrunde liegende Tabelle gelöscht wird.<br /><br /> **Cleanup task finished**: Gibt an, wann der Cleanupvorgang für alle Abonnements in dieser Parametertabelle beendet wurde.|21|Ja|  
|GroupID|**int**|ID der Arbeitsauslastungsgruppe, in der das SQL-Ablaufverfolgungsereignis ausgelöst wird.|66|Ja|  
|HostName|**nvarchar**|Der Name des Computers, auf dem der Client ausgeführt wird. Diese Datenspalte wird aufgefüllt, wenn der Hostname durch den Client bereitgestellt wird. Der Hostname kann mithilfe der HOST_NAME-Funktion bestimmt werden.|8|Ja|  
|IsSystem|**int**|Gibt an, ob das Ereignis bei einem Systemprozess oder einem Benutzerprozess aufgetreten ist.<br /><br /> 0 = Benutzer<br /><br /> 1 = System|60|Nein|  
|LoginName|**nvarchar**|Anmeldename des Benutzers ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherheitsanmeldung oder Windows-Anmeldeinformationen im Format *DOMAIN*\\*username*).|11|Nein|  
|LoginSID|**image**|Die Sicherheits-ID (SID, Security Identification Number) des angemeldeten Benutzers. Diese Informationen finden Sie in der sys.server_principals-Katalogsicht. Die SID ist für jede Anmeldung beim Server eindeutig.|41|Ja|  
|NTDomainName|**nvarchar**|Die Windows-Domäne, der der Benutzer angehört.|7|Ja|  
|NTUserName|**nvarchar**|Der Name des Benutzers, der Besitzer der Verbindung ist, die dieses Ereignis generiert hat.|6|Ja|  
|RequestID|**int**|Der Bezeichner der Anforderung, die die Anweisung enthält.|49|Ja|  
|ServerName|**nvarchar**|Der Name der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , für die eine Ablaufverfolgung ausgeführt wird.|26|Nein|  
|SessionLoginName|**nvarchar**|Der Anmeldename des Benutzers, der die Sitzung geöffnet hat. Wenn beispielsweise eine Anwendung mithilfe von Login1 eine Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellt und eine Anweisung als Login2 ausführt, zeigt SessionLoginName "Login1" an, und LoginName zeigt "Login2" an. Diese Spalte zeigt sowohl den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] - als auch den Windows-Anmeldenamen an.|64|Ja|  
|SPID|**int**|Die ID der Sitzung, in der das Ereignis aufgetreten ist.|12|Ja|  
|StartTime|**datetime**|Zeitpunkt, zu dem das Ereignis begonnen hat (falls vorhanden).|14|Ja|  
|TextData|**ntext**|Gibt ein XML-Dokument mit spezifischen Informationen zu diesem Ereignis zurück. Dieses Dokument stimmt mit dem XML-Schema überein, das auf der Seite [SQL Server Query Notification Profiler Event Schema](https://go.microsoft.com/fwlink/?LinkId=63331) (in englischer Sprache) zur Verfügung gestellt wird.|1|Ja|  
  
  
