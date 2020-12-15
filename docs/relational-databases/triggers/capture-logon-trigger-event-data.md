---
description: Erfassen von Ereignisdaten für Logon-Trigger
title: Erfassen von Ereignisdaten für Logon-Trigger | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: e05b1ab4-c10b-402a-9591-f6ec1e3db8c0
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 429afe57e13c6eb9285256f54f2e83e2609ff7c0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97403263"
---
# <a name="capture-logon-trigger-event-data"></a>Erfassen von Ereignisdaten für Logon-Trigger
[!INCLUDE[tsql-appliesto-ss2008-appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md)]
  Wenn Sie XML-Daten zu LOGON-Ereignissen für die Verwendung in Logon-Triggern erfassen möchten, verwenden Sie die [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md) -Funktion. Mit dem LOGON-Ereignis wird das folgende Ereignisdatenschema zurückgegeben:  
  
 `<EVENT_INSTANCE>`  
  
 `<EventType>event_type</EventType>`  
  
 `<PostTime>post_time</PostTime>`  
  
 `<SPID>spid</SPID>`  
  
 `<ServerName>server_name</ServerName>`  
  
 `<LoginName>login_name</LoginName>`  
  
 `<LoginType>login_type</LoginType>`  
  
 `<SID>sid</SID>`  
  
 `<ClientHost>client_host</ClientHost>`  
  
 `<IsPooled>is_pooled</IsPooled>`  
  
 `</EVENT_INSTANCE>`  
  
 `<EventType>`  
 Enthält `LOGON`.  
  
 `<PostTime>`  
 Enthält die Uhrzeit, zu der eine Anforderung zum Erstellen einer Sitzung ausgegeben wird.  
  
 `<SID>`  
 Enthält den base64-verschlüsselten binären Datenstrom der Sicherheits-ID (SID) für den angegebenen Anmeldenamen.  
  
 `<ClientHost>`  
 Enthält den Hostnamen des Clients, von dem aus die Verbindung hergestellt wird. Der Wert lautet '`<local_machine>`', wenn der Name des Clients und des Servers identisch sind. Andernfalls entspricht der Wert der IP-Adresse des Clients.  
  
 `<IsPooled>`  
 Lautet `1` , wenn die Verbindung durch Verbindungspooling wiederverwendet wird. Andernfalls lautet der Wert `0`.  
  
  
