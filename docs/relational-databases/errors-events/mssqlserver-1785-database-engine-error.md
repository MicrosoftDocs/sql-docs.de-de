---
description: MSSQLSERVER_1785
title: MSSQLSERVER_1785
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1785 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 6adb44b26ca0af5ab00476c680c924875cd50d2d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348827"
---
# <a name="mssqlserver_1785"></a>MSSQLSERVER_1785
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|1785|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|CRTFKINVTOPO|
|Meldungstext|Das Einführen der FOREIGN KEY-Einschränkung "%.ls" für die "%. ls"-Tabelle kann Schleifen oder mehrere Kaskadepfade verursachen. Geben Sie ON DELETE NO ACTION oder ON UPDATE NO ACTION an, oder ändern Sie andere FOREIGN KEY-Einschränkungen.|
||

## <a name="explanation"></a>Erklärung

Diese Fehlermeldung wird angezeigt, weil in SQL Server eine Tabelle nur einmal in einer Liste aller kaskadierenden referenziellen Aktionen vorkommen kann, die entweder durch eine `DELETE`- oder `UPDATE`-Anweisung gestartet werden. Die Struktur von kaskadierenden referenziellen Aktionen darf nur einen Pfad zu einer bestimmten Tabelle in der Struktur der kaskadierenden referenziellen Aktionen aufweisen.

Dem Benutzer wird eine Fehlermeldung wie die folgende angezeigt:

> Server:  Meldung 1785, Ebene 16, Status 1, Zeile 1 Das Einführen der FOREIGN KEY-Einschränkung „fk_two“ für die Tabelle „table2“ kann Schleifen oder mehrere Weitergabepfade verursachen. Geben Sie ON DELETE NO ACTION oder ON UPDATE NO ACTION an, oder ändern Sie andere FOREIGN KEY-Einschränkungen. Server:  Meldung 1750, Ebene 16, Status 1, Zeile 1 Einschränkung konnte nicht erstellt werden. Siehe vorherige Fehler

## <a name="user-action"></a>Benutzeraktion

Um dieses Problem zu lösen, erstellen Sie einen Fremdschlüssel, der einen einzigen Pfad zu einer Tabelle in einer Liste mit kaskadierenden referenziellen Aktionen erstellt.

Sie können die referenzielle Integrität auf verschiedene Weise erzwingen. Die deklarative referentielle Integrität (DRI) ist die einfachste, aber auch die am wenigsten flexible Methode. Wenn Sie mehr Flexibilität benötigen, aber dennoch ein hohes Maß an Integrität wünschen, können Sie stattdessen Trigger verwenden.

## <a name="more-information"></a>Weitere Informationen

Der folgende Beispielcode ist ein Beispiel für den FOREIGN KEY-Erstellungsversuch, der die Fehlermeldung generiert:

```sql
USE tempdb
GO

CREATE TABLE table1 (user_ID INTEGER NOT NULL PRIMARY KEY, user_name
CHAR(50) NOT NULL)
GO

CREATE TABLE table2 (author_ID INTEGER NOT NULL PRIMARY KEY, author_name
CHAR(50) NOT NULL, lastModifiedBy INTEGER NOT NULL, addedby INTEGER NOT NULL)
GO

ALTER TABLE table2 ADD CONSTRAINT fk_one FOREIGN KEY (lastModifiedby)
REFERENCES table1 (user_ID) ON DELETE CASCADE ON UPDATE cascade
GO

ALTER TABLE table2 ADD CONSTRAINT fk_two FOREIGN KEY (addedby)
REFERENCES table1(user_ID) ON DELETE NO ACTION ON UPDATE cascade
GO
--this fails with the error because it provides a second cascading path to table2.

ALTER TABLE table2 ADD CONSTRAINT fk_two FOREIGN KEY (addedby)
REFERENCES table1 (user_ID) ON DELETE NO ACTION ON UPDATE NO ACTION
GO
-- this works.
```

### <a name="see-also"></a>Weitere Informationen

[Kaskadierende referenzielle Integrität](../tables/primary-and-foreign-key-constraints.md#referential-integrity)