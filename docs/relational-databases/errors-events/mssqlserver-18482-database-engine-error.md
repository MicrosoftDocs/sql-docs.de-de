---
description: MSSQLSERVER_18482
title: MSSQLSERVER_18482
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 18482 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: ff7d1cf9be26bd9301d16ae21dcbd6af96f57bc3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345548"
---
# <a name="mssqlserver_18482"></a>MSSQLSERVER_18482
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|18482|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|REMLOGIN_INVALID_SITE|
|Meldungstext|Zum Server "%.ls" konnte keine Verbindung hergestellt werden, weil "%. ls" nicht als Remoteserver definiert ist. Überprüfen Sie, ob Sie den richtigen Servernamen angegeben haben. %.*ls|
||

## <a name="explanation"></a>Erklärung

Dieser Fehler tritt auf, wenn Sie versuchen, einen Remoteprozeduraufruf (RPC) von einem Server zu einem anderen auszuführen (z. B. durch Ausführen einer gespeicherten Prozedur auf einem Remotecomputer mit einer Anweisung wie `EXEC SERV_REMOTE.pubs..byroyalty`). Dem Benutzer wird eine Fehlermeldung angezeigt, die der folgenden ähnelt:

> Fehler 18482: Zum Server \<SERV_REMOTE> konnte keine Verbindung hergestellt werden, weil \<SERV_REMOTE> nicht als Remoteserver definiert ist. Überprüfen Sie, ob Sie den richtigen Servernamen angegeben haben.

## <a name="cause"></a>Ursache

Dieser Fehler tritt auf, wenn SQL Server einen Remoteprozeduraufruf nicht ausführen kann. Dies kann durch einen nicht ordnungsgemäß konfigurierten lokalen Server verursacht werden. Um eine Remoteprozedur aufzurufen, wird von SQL Server zuerst mit „srvid = 0“ in „sysservers“ nach dem Servernamen gesucht, um den lokalen Server zu ermitteln. Wenn kein Eintrag mit „srvid = 0“ in „sysservers“ gefunden wird oder wenn der Servername mit „srvid = 0“ zu einem Servernamen gehört, der sich vom lokalen Windows-Computernamen unterscheidet, erhalten Sie die Fehlermeldung.

## <a name="user-action"></a>Benutzeraktion

Um zu ermitteln, ob der lokale Server ordnungsgemäß konfiguriert ist, überprüfen Sie die Spalte `srvstatus` in „master..sysservers“. Der Wert sollte für den lokalen Server 0 sein.

Angenommen, der lokale Server heißt SERV_LOCAL, der Remoteserver heißt *SERV_REMOTE*, und „sysservers“ enthält die folgenden Informationen:

|srvid|srvstatus|srvname|srvname|
|---|---|---|---|
|1|2|SERV_LOCAL|SERV_LOCAL|
|2|1|SERV_REMOTE|SERV_REMOTE|
||||

In der vorangehenden Ausgabe ist SERV_LOCAL der lokale Server, weist aber eine „srvid“ von 1 auf; diese sollte 0 sein. Gehen Sie folgendermaßen vor, um dies zu korrigieren:

1. Führen Sie „`sp_dropserver` local_server_name, droplogins“ aus (in diesem Beispiel würden Sie `sp_dropserver SERV_LOCAL, droplogins` ausführen).
1. Führen Sie „`sp_addserver` local_server_name, LOCAL“ aus (in diesem Beispiel würden Sie `sp_addserver SERV_LOCAL, LOCAL` ausführen).
1. Beenden Sie SQL Server, und starten Sie es neu.

Nachdem Sie diese Schritte ausgeführt haben, sollte die sysservers-Tabelle wie folgt aussehen:

|srvid|srvstatus|srvname|srvname|
|---|---|---|---|
|0|0|SERV_LOCAL|SERV_LOCAL|
|2|1|SERV_REMOTE|SERV_REMOTE|
||||

> [!NOTE]
> Die Server-ID (srvid) sollte für den lokalen Server 0 sein.

In einigen Fällen können die Einträge in der sysservers-Tabelle korrekt aussehen, aber wenn Sie `select @@servername` ausführen, wird NULL zurückgegeben. In diesen Szenarien sollten Sie trotzdem die oben aufgeführten Schritte 1 bis 3 ausführen, um das Problem zu beheben.

## <a name="more-information"></a>Weitere Informationen

Diese Fehlermeldung wird möglicherweise beim Installieren der Replikation angezeigt, da der Installationsvorgang Remoteprozeduraufrufe zwischen den an der Replikation beteiligten Servern ausführt.
