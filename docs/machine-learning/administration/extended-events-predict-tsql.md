---
title: Überwachen von T-SQL mit erweiterten Ereignissen
description: Erfahren Sie, wie Sie erweiterte Ereignisse verwenden, um PREDICT-T-SQL-Anweisungen in SQL Server Machine Learning Services zu überwachen und damit einhergehende Probleme zu beheben.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 09/24/2019
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 099e01e8e626b7a812ebcf2135b7d5e126a49e4e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337138"
---
# <a name="monitor-predict-t-sql-statements-with-extended-events-in-sql-server-machine-learning-services"></a>Überwachen von PREDICT-T-SQL-Anweisungen mit erweiterten Ereignissen in SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Erfahren Sie, wie Sie erweiterte Ereignisse verwenden, um [PREDICT](../../t-sql/queries/predict-transact-sql.md)-T-SQL-Anweisungen in SQL Server Machine Learning Services zu überwachen und damit einhergehende Probleme zu beheben.

## <a name="table-of-extended-events"></a>Tabelle erweiterter Ereignisse

Die folgenden erweiterten Ereignisse sind in allen Versionen von SQL Server verfügbar, die die [PREDICT](../../t-sql/queries/predict-transact-sql.md)-T-SQL-Anweisung unterstützen. 

| name                       | object_type | description |
|----------------------------|-------------|-------------|
| predict_function_completed | event       | Integrierte Aufschlüsselung der Ausführungszeit|
| predict_model_cache_hit    | event       | Tritt auf, wenn ein Modell aus dem Modellcache der PREDICT-Funktion abgerufen wird. Verwenden Sie dieses Ereignis zusammen mit anderen predict_model_cache_*-Ereignissen zur Behandlung von Problemen, die vom Modellcache der PREDICT-Funktion verursacht werden.|
| predict_model_cache_insert | event       | Tritt auf, wenn ein Modell in den Modellcache der PREDICT-Funktion eingefügt wird. Verwenden Sie dieses Ereignis zusammen mit anderen predict_model_cache_*-Ereignissen zur Behandlung von Problemen, die vom Modellcache der PREDICT-Funktion verursacht werden.   |
| predict_model_cache_miss   | event       | Tritt auf, wenn ein Modell im Modellcache der PREDICT-Funktion nicht gefunden wird. Ein häufiges Vorkommen dieses Ereignisses kann darauf hindeuten, dass die SQL Server-Instanz mehr Arbeitsspeicher benötigt. Verwenden Sie dieses Ereignis zusammen mit anderen predict_model_cache_*-Ereignissen zur Behandlung von Problemen, die vom Modellcache der PREDICT-Funktion verursacht werden.|
| predict_model_cache_remove | event       | Tritt auf, wenn ein Modell aus dem Modellcache der PREDICT-Funktion entfernt wird. Verwenden Sie dieses Ereignis zusammen mit anderen predict_model_cache_*-Ereignissen zur Behandlung von Problemen, die vom Modellcache der PREDICT-Funktion verursacht werden.|

## <a name="query-for-related-events"></a>Abfragen für verwandte Ereignisse

Wenn Sie eine Liste aller für diese Ereignisse zurückgegebenen Spalten anzeigen möchten, führen Sie die folgende Abfrage in SQL Server Management Studio aus:

```sql
SELECT *
FROM sys.dm_xe_object_columns
WHERE object_name LIKE 'predict%'
```

## <a name="examples"></a>Beispiele

So erfassen Sie Informationen zur Leistung einer Bewertungssitzung mithilfe von PREDICT:

1. Erstellen Sie eine neue Sitzung für erweiterte Ereignisse, indem Sie Management Studio oder ein anderes unterstütztes [Tool](../../relational-databases/extended-events/extended-events-tools.md) verwenden.
2. Fügen Sie die Ereignisse `predict_function_completed` und `predict_model_cache_hit` der Sitzung hinzu.
3. Starten Sie die Sitzung für erweiterte Ereignisse.
4. Führen Sie die Abfrage mit der PREDICT-Anweisung aus.

Überprüfen Sie in den Ergebnissen die folgenden Spalten:

+ Der Wert für `predict_function_completed` zeigt an, wie lange die Abfrage benötigt hat, um das Modell und die Bewertung zu laden.
+ Der boolesche Wert für `predict_model_cache_hit` weist darauf hin, ob die Abfrage ein zwischengespeichertes Modell verwendet hat oder nicht. 

### <a name="native-scoring-model-cache"></a>Modellcache für native Bewertung

Zusätzlich zu den PREDICT-spezifischen Ereignissen können Sie die folgenden Abfragen verwenden, um weitere Informationen über das Modell im Cache und die Cachenutzung zu erhalten:

Zeigen Sie den Modellcache für native Bewertung an:

```sql
SELECT *
FROM sys.dm_os_memory_clerks
WHERE type = 'CACHESTORE_NATIVESCORING';
```

Zeigen Sie die Objekte im Modellcache an:

```sql
SELECT *
FROM sys.dm_os_memory_objects
WHERE TYPE = 'MEMOBJ_NATIVESCORING';
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu erweiterten Ereignissen (manchmal auch als XEvents bezeichnet) und zum Nachverfolgen von Ereignissen in einer Sitzung finden Sie in diesen Artikeln:

+ [Überwachen von Python- und R-Skripts mit erweiterten Ereignissen in SQL Server Machine Learning Services](extended-events.md)
+ [Übersicht über erweiterte Ereignisse](../../relational-databases/extended-events/extended-events.md)
+ [Schnellstart: Erweiterte Ereignisse in SQL Server](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md)
+ [Verwalten von Ereignissitzungen im Objekt-Explorer](../../relational-databases/extended-events/manage-event-sessions-in-the-object-explorer.md)