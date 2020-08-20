---
description: Treiberarchitektur
title: Treiberarchitektur | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- ODBC architecture [ODBC], drivers
- drivers [ODBC], architecture
ms.assetid: c5003413-0cc1-4f41-b877-a64e2f5ab118
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 530940cef233b8a6b6a3f5c0575dd154998126cd
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88494613"
---
# <a name="driver-architecture"></a>Treiberarchitektur
Die Treiberarchitektur lässt sich je nach Software, die SQL-Anweisungen verarbeitet, in zwei Kategorien unterteilen:  
  
-   **Dateibasierte Treiber** Der Treiber greift direkt auf die physischen Daten zu. In diesem Fall fungiert der Treiber sowohl als Treiber als auch als Datenquelle. Das heißt, dass ODBC-Aufrufe und SQL-Anweisungen verarbeitet werden. DBase-Treiber sind z. b. dateibasierte Treiber, da dBASE keine eigenständige Datenbank-Engine bereitstellt, die vom Treiber verwendet werden kann. Es ist wichtig zu beachten, dass Entwickler von dateibasierten Treibern eigene Datenbank-Engines schreiben müssen.  
  
-   **DBMS-basierte Treiber** Der Treiber greift über eine separate Datenbank-Engine auf die physischen Daten zu. In diesem Fall verarbeitet der Treiber nur ODBC-Aufrufe. SQL-Anweisungen werden zur Verarbeitung an die Datenbank-Engine weitergeleitet. Oracle-Treiber sind z. b. DBMS-basierte Treiber, da Oracle über eine eigenständige Datenbank-Engine verfügt, die der Treiber verwendet. Die Datenbank-Engine ist unerheblich. Sie kann sich auf demselben Computer wie der Treiber oder auf einem anderen Computer im Netzwerk befinden. Es kann sogar über ein Gateway darauf zugegriffen werden.  
  
 Treiberarchitektur ist in der Regel nur für treiberwriter interessant; Das heißt, die Treiberarchitektur unterscheidet sich in der Regel nicht von der Anwendung. Die Architektur kann jedoch beeinflussen, ob eine Anwendung DBMS-spezifisches SQL verwenden kann. Beispielsweise stellt Microsoft Access eine eigenständige Datenbank-Engine bereit. Wenn ein Microsoft Access-Treiber DBMS-basiert und auf die Daten über dieses Modul zugreift, kann die Anwendung Microsoft Access-SQL-Anweisungen zur Verarbeitung an die Engine übergeben.  
  
 Wenn der Treiber jedoch Datei basiert ist, d. h., er enthält ein proprietäres Modul, das direkt auf die Microsoft® Access. mdb-Datei zugreift. alle Versuche, Microsoft Access-spezifische SQL-Anweisungen an die Engine zu übergeben, führen wahrscheinlich zu Syntax Fehlern. Der Grund hierfür ist, dass die proprietäre Engine wahrscheinlich nur ODBC SQL implementiert.  
  
 In diesem Abschnitt werden die folgenden Themen behandelt:  
  
-   [Dateibasierte Treiber](../../odbc/reference/file-based-drivers.md)  
  
-   [DBMS-basierte Treiber](../../odbc/reference/dbms-based-drivers.md)  
  
-   [Netzwerkbeispiel](../../odbc/reference/network-example.md)  
  
-   [Andere Treiberarchitekturen](../../odbc/reference/other-driver-architectures.md)
