---
description: Arbeiten mit Anweisungen und Resultsets
title: Verwenden von Anweisungen und Resultsets | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: cc917534-f5f8-4844-87c8-597c48b4e06d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5699e2cf4155da371aaffcf6478ac8d323743770
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88450028"
---
# <a name="working-with-statements-and-result-sets"></a>Arbeiten mit Anweisungen und Resultsets

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Wenn Sie die Statement- und ResultSet-Objekte von [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] verwenden, gibt es mehrere Möglichkeiten, um die Leistung und Zuverlässigkeit der Anwendungen zu verbessern.

## <a name="use-the-appropriate-statement-object"></a>Verwenden des geeigneten Statement-Objekts

Wenn Sie eines der Statement-Objekte des JDBC-Treibers verwenden, z.B. ein [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md)-, ein [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)- oder ein [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md)-Objekt, müssen Sie das für den Auftrag geeignete Objekt verwenden.

- Wenn Sie über keine OUT-Parameter verfügen, müssen Sie das SQLServerCallableStatement-Objekt nicht verwenden. Verwenden Sie stattdessen das SQLServerStatement- oder das SQLServerPreparedStatement-Objekt.

- Wenn Sie die Anweisung nicht mehrmals ausführen möchten oder keine IN- oder OUT-Parameter verwenden, müssen Sie das SQLServerCallableStatement-Objekt oder das SQLServerPreparedStatement-Objekt nicht verwenden. Verwenden Sie stattdessen das SQLServerStatement-Objekt.

## <a name="use-the-appropriate-concurrency-for-resultset-objects"></a>Verwenden der geeigneten Parallelität für ResultSet-Objekte

Verwenden Sie beim Erstellen von Anweisungen, die Resultsets erzeugen, keine aktualisierbare Parallelität, wenn die Ergebnisse nicht aktualisiert werden sollen. Das Standardmodell mit schreibgeschütztem Vorwärtscursor weist beim Lesen von kleinen Resultsets die höchste Leistung auf.

## <a name="limit-the-size-of-your-result-sets"></a>Einschränken der Größe von Resultsets

Sie sollten die [setMaxRows](../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md)-Methode (bzw. die SQL-Syntax SET ROWCOUNT oder SELECT TOP N) verwenden, um die Anzahl der von möglicherweise umfangreichen Resultsets zurückgegebenen Zeilen zu beschränken. Bei der Verarbeitung von umfangreichen Resultsets sollten Sie eine adaptive Antwortpufferung verwenden, indem Sie die Verbindungszeichenfolgeneigenschaft "responseBuffering" auf "adaptive" (den Standardmodus) festlegen. So kann die Anwendung große Resultsets verarbeiten, ohne serverseitige Cursor verwenden zu müssen, und die Anwendungsspeicherauslastung minimieren. Weitere Informationen finden Sie unter [Using adaptive buffering (Verwenden der adaptiven Pufferung)](../../connect/jdbc/using-adaptive-buffering.md).

## <a name="use-the-appropriate-fetch-size"></a>Verwenden der geeigneten Abrufgröße

Bei schreibgeschützten Servercursorn muss ein Kompromiss zwischen den Roundtrips zum Server und dem im Treiber verwendeten Speicher gefunden werden. Bei aktualisierbaren Servercursorn beeinflusst außerdem die Abrufgröße die Sensitivität des Resultsets gegenüber Änderungen und die Parallelität des Servers. Updates von Zeilen im Fetchpuffer werden erst dann sichtbar, wenn explizit eine [refreshRow](../../connect/jdbc/reference/refreshrow-method-sqlserverresultset.md)-Methode ausgegeben wird oder der Cursor den Fetchpuffer verlässt. Große Fetchpuffer weisen eine höhere Leistung (weniger Serverroundtrips) aber eine geringere Sensitivität gegenüber Änderungen auf und verringern die Parallelität des Servers, wenn CONCUR_SS_SCROLL_LOCKS (1009) verwendet wird. Verwenden Sie die Abrufgröße "1", um die Sensitivität gegenüber Änderungen zu maximieren. Beachten Sie jedoch, dass dabei für jede abgerufene Zeile ein Roundtrip zum Server erforderlich ist.

## <a name="use-streams-for-large-in-parameters"></a>Verwenden von Datenströmen für umfangreiche IN-Parameter

Verwenden Sie Datenströme oder BLOBs und CLOBs, die inkrementell gefüllt werden, um umfangreiche Spaltenwerte zu aktualisieren oder umfangreiche IN-Parameter zu senden. Der JDBC-Treiber sendet diese blockweise in mehreren Roundtrips an den Server, sodass Werte festgelegt und aktualisiert werden können, die größer sind als der verfügbare Speicher.

## <a name="see-also"></a>Weitere Informationen

[Verbessern von Leistung und Zuverlässigkeit mit dem JDBC-Treiber](../../connect/jdbc/improving-performance-and-reliability-with-the-jdbc-driver.md)
