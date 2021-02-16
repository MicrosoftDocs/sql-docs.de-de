---
description: MSSQLSERVER_8632
title: MSSQLSERVER_8632
ms.custom: ''
ms.date: 10/27/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8632 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: b7c59634423bd6df623725720bf8bd0d262d5be0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348689"
---
# <a name="mssqlserver_8632"></a>MSSQLSERVER_8632
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|8632|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|QUERY_EXPRESSION_TOO_COMPLEX|
|Meldungstext|Interner Fehler: Ein Ausdrucksdienstelimit wurde erreicht. Suchen Sie nach potenziell komplexen Ausdrücken in Ihrer Abfrage, und vereinfachen Sie diese.|
||

## <a name="explanation"></a>Erklärung

Der Fehler 8632 wird ausgelöst, wenn Sie eine Abfrage in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausführen, die eine große Anzahl von Bezeichnern und Konstanten in einem einzelnen Ausdruck enthält. Dem Benutzer wird eine Fehlermeldung wie die folgende angezeigt:

> Server:  Meldung 8632, Ebene 17, Status 2, Zeile 1  
Interner Fehler: Ein Ausdrucksdienstelimit wurde erreicht. Suchen Sie nach potenziell komplexen Ausdrücken in Ihrer Abfrage, und vereinfachen Sie diese.

## <a name="cause"></a>Ursache

Dieses Problem tritt auf, weil [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Anzahl von Bezeichnern und Konstanten beschränkt, die in einem einzelnen Ausdruck einer Abfrage enthalten sein können. Dieser Grenzwert ist 65.535. Die folgende Abfrage umfasst beispielsweise nur einen Ausdruck:

```sql
select a, b + c, d + e
```

Dieser Ausdruck ruft alle fünf Spalten ab, berechnet die Additionsoperatoren und sendet drei projizierte Ergebnisse an den Client.

Der Test für die Anzahl der Bezeichner und Konstanten wird ausgeführt, nachdem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alle referenzierten Bezeichner und Konstanten erweitert hat. Beispielsweise können die folgenden Elemente erweitert werden:

- das Sternchen (*) in der Auswahlliste
- eine Ansicht
- die Definition einer berechneten Spalte

Wenn die Zahl den Grenzwert nach der Erweiterung überschreitet, kann die Abfrage nicht ausgeführt werden.

## <a name="user-action"></a>Benutzeraktion

Sie müssen die Abfrage neu schreiben, um dieses Problem zu umgehen. Verweisen Sie im größten Ausdruck in der Abfrage auf weniger Bezeichner und Konstanten. Sie müssen sicherstellen, dass die Anzahl der Bezeichner und Konstanten in den einzelnen Ausdrücken der Abfrage den Grenzwert nicht überschreitet. Zu diesem Zweck müssen Sie eine Abfrage möglicherweise in mehr als eine einzelne Abfrage aufteilen. Erstellen Sie dann ein temporäres Zwischenergebnis.
