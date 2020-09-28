---
title: Verbessern von Leistung und Zuverlässigkeit mit dem JDBC-Treiber
description: Lernen Sie verschiedene Verfahren kennen, um die Leistung und Zuverlässigkeit von Anwendungen zu verbessern, wenn der Microsoft JDBC-Treiber für SQL Server verwendet wird.
ms.custom: ''
ms.date: 07/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: e1592499-b87b-45ee-bab8-beaba8fde841
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bc04a90569974acbc99dcb66680d66289db1c0d1
ms.sourcegitcommit: b80364e31739d7b08cc388c1f83bb01de5dd45c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/04/2020
ms.locfileid: "87565378"
---
# <a name="improving-performance-and-reliability-with-the-jdbc-driver"></a>Verbessern von Leistung und Zuverlässigkeit mit dem JDBC-Treiber

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Ein allen Anwendungen gemeinsamer Aspekt der Anwendungsentwicklung ist die ständige Notwendigkeit, Leistung und Zuverlässigkeit zu verbessern. Nachstehend sind einige Verfahren aufgeführt, um dieses Ziel mit [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] zu erreichen.  
  
Die Themen in diesem Abschnitt beschreiben verschiedene Verfahren, um die Leistung und Zuverlässigkeit von Anwendungen zu verbessern, wenn der JDBC-Treiber verwendet wird.  

## <a name="in-this-section"></a>In diesem Abschnitt

|Thema|BESCHREIBUNG|  
|-----------|-----------------|  
|[Schließen von nicht verwendeten Objekten](../../connect/jdbc/closing-objects-when-not-in-use.md)|Beschreibt die Bedeutung des Schließens von JDBC-Treiberobjekten, wenn sie nicht mehr benötigt werden.|  
|[Verwalten des Transaktionsumfangs](../../connect/jdbc/managing-transaction-size.md)|Beschreibt Verfahren zur Steigerung der Transaktionsleistung.|  
|[Arbeiten mit Anweisungen und Resultsets](../../connect/jdbc/working-with-statements-and-result-sets.md)|In diesem Artikel werden Vorgehensweisen zum Verbessern der Leistung bei Verwendung der Statement- oder ResultSet-Objekte beschrieben.|  
|[Verwenden der adaptiven Pufferung](../../connect/jdbc/using-adaptive-buffering.md)|Beschreibt die Funktion für die adaptive Pufferung, durch die alle Daten mit großen Werten ohne den Aufwand für Servercursor abgerufen werden können.|  
|[Sparsespalten](../../connect/jdbc/sparse-columns.md)|Erläutert, auf welche Weise vom JDBC-Treiber Sparsespalten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt werden.|  
|[Vorbereitetes Caching von Anweisungsmetadaten für den JDBC-Treiber](../../connect/jdbc/prepared-statement-metadata-caching-for-the-jdbc-driver.md)|In diesem Artikel werden Vorgehensweisen zum Verbessern der Leistung von Abfragen von Prepared Statements beschrieben.|
|[Verwenden der Massenkopierungs-API für den Batcheinfügungsvorgang](../../connect/jdbc/use-bulk-copy-api-batch-insert-operation.md)|In diesem Artikel wird beschrieben, wie Sie die API für Massenkopiervorgänge für Batcheinfügevorgänge und deren Vorteile aktivieren.|
|[Zeichenfolgenparameter werden nicht als Unicode gesendet.](../../connect/jdbc/setting-the-connection-properties.md)|Bei Verwendung von **CHAR**-, **VARCHAR**- und **LONGVARCHAR**-Daten können Benutzer die Verbindungseigenschaft **sendStringParametersAsUnicode** auf `false` festlegen, um eine optimale Leistungssteigerung zu erzielen.|

## <a name="see-also"></a>Weitere Informationen

[Übersicht über den JDBC-Treiber](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
  
