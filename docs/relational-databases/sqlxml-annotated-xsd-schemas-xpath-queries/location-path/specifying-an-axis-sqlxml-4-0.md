---
title: Angeben einer Achse (SQLXML)
description: Erfahren Sie, wie Sie in einer SQLXML 4,0 XPath-Abfrage eine Achse angeben, die Struktur Beziehung zwischen den Knoten angibt, die vom Location-Step und dem context-Knoten ausgewählt werden.
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- XPath queries [SQLXML], axes
- XPath queries [SQLXML], location paths
- self axis
- attribute axis [SQLXML]
- axis [SQLXML]
- child axis
- parent axis
- location path for XPath query
- axes [SQLXML]
ms.assetid: 65631795-3389-40cf-90ea-85e9438956c5
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 03c0fff271ef3774116eb9c97025b1f602f7852c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97431158"
---
# <a name="specifying-an-axis-sqlxml-40"></a>Angeben einer Achse (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
    
-   Die Achse gibt die Strukturbeziehung zwischen den vom Positionsschritt ausgewählten Knoten und dem Kontextknoten an. Die folgenden Achsen werden unterstützt:  **Child**  
  
     Enthält das untergeordnete Element des Kontextknotens.  
  
     Der folgende XPath-Ausdruck (Speicherort Pfad) wählt alle untergeordneten Elemente aus dem aktuellen Kontext Knoten aus **\<Customer>** :  
  
    ```  
    child::Customer  
    ```  
  
     In der folgenden XPath-Abfrage ist `child` die Achse. `Customer` ist der Knotentest.  
  
-   **parent**  
  
     Enthält das übergeordnete Element des Kontextknotens.  
  
     Der folgende XPath-Ausdruck wählt alle **\<Customer>** übergeordneten Elemente der untergeordneten Elemente aus **\<Order>** :  
  
    ```  
    child::Customer/child::Order[parent::Customer/@customerID="ALFKI"]  
    ```  
  
     Dies entspricht exakt der Angabe `child::Customer`. In dieser XPath-Abfrage sind `child` und `parent` die Achsen. `Customer` und `Order` sind die Knotentests.  
  
-   **attribute**  
  
     Enthält das Attribut des Kontextknotens.  
  
     Der folgende XPath-Ausdruck wählt das **CustomerID-** Attribut des Kontext Knotens aus:  
  
    ```  
    attribute::CustomerID  
    ```  
  
-   **Selbstbedienungs**  
  
     Enthält den Kontextknoten selbst.  
  
     Der folgende XPath-Ausdruck wählt den aktuellen Knoten aus, wenn er der **\<Order>** Knoten ist:  
  
    ```  
    self::Order  
    ```  
  
     In dieser XPath-Abfrage ist `self` die Achse und `Order` der Knotentest.  
  
  
