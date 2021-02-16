---
description: MSSQLSERVER_3988
title: MSSQLSERVER_3988
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3988 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 42e07f7731696e23579650ccbfe8ca3930c69cb9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348788"
---
# <a name="mssqlserver_3988"></a>MSSQLSERVER_3988
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|3988|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|XACT_UNSUPPORT_PARALLEL_TRAN2|
|Meldungstext|Eine neue Transaktion ist unzulässig, weil in dieser Sitzung andere Threads ausgeführt werden.|
||

## <a name="explanation"></a>Erklärung

Dieser Fehler tritt auf, wenn Sie eine verteilte Abfrage zum Verknüpfen mehrerer Tabellen ausführen, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Remoteinstanzen gehostet werden, während die Einstellung der `XACT_ABORT`-Sitzung auf **ON** festgelegt ist. Dem Benutzer wird eine Fehlermeldung angezeigt, die der folgenden ähnelt:

> Meldung 3988, Ebene 16, Status 1, Zeile #  
Eine neue Transaktion ist unzulässig, weil in dieser Sitzung andere Threads ausgeführt werden.

## <a name="cause"></a>Ursache

Unter den folgenden Bedingungen gelten einige Einschränkungen bei der Art und Weise, wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verteilte Abfragen behandelt:

- SQL Server verknüpft mehrere Tabellen aus einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Remotedatenquelle.
- Die Sitzung, in der die Abfrage ausgeführt wird, ist nicht in einer verteilten Transaktion eingetragen.

In dieser Situation wird beim Versuch, die Abfrage auszuführen, möglicherweise einer der beiden Fehler ausgelöst, die im Abschnitt **Erklärung** erwähnt werden.

## <a name="user-action"></a>Benutzeraktion

Um dieses Problem zu umgehen, schließen Sie die verteilte Abfrage in einer „BEGIN DISTRIBUTED TRANSACTION“-Anweisung ein:

```sql
BEGIN DISTRIBUTED TRANSACTION <Distributed Query> COMMIT TRANSACTION
```
