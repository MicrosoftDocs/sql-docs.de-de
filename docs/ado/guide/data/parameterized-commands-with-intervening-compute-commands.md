---
description: Parametrisierte Befehle mit dazwischen liegenden COMPUTE-Befehlen
title: Parametrisierte Befehle mit dazwischenliegenden computebefehlen | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], parameterized commands
- parameterized commands [ADO]
- APPEND clause [ADO]
- COMPUTE command [ADO]
ms.assetid: 732f624f-8900-4608-9815-194302d22e8b
author: rothja
ms.author: jroth
ms.openlocfilehash: ce78d5edf2ded2744c3633368d571d72912c1322
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037220"
---
# <a name="parameterized-commands-with-intervening-compute-commands"></a>Parametrisierte Befehle mit dazwischen liegenden COMPUTE-Befehlen
Ein typischer parametrisierter Shape-Anfüge Befehl verfügt über eine-Klausel, die ein übergeordnetes **Recordset** mit einem Abfragebefehl und eine andere Klausel erstellt, die ein untergeordnetes **Recordset** mit einem parametrisierten Abfragebefehl erstellt, d. h. einen Befehl, der einen Parameter Platzhalter (Fragezeichen, "?") enthält. Das resultierende geformte **Recordset** verfügt über zwei Ebenen, wobei das übergeordnete Element die obere Ebene einnimmt und das untergeordnete Element die untere Ebene einnimmt.  
  
 Die-Klausel, die das untergeordnete **Recordset** erstellt, kann jetzt eine beliebige Anzahl geschachtelter Form-Compute-Befehle sein, bei denen der untersten geschachtelte Befehl die parametrisierte Abfrage enthält. Das resultierende geformte **Recordset** verfügt über mehrere Ebenen, bei denen das übergeordnete Element die oberste Ebene einnimmt, das untergeordnete Element die unterste Ebene einnimmt und eine beliebige Anzahl von **Recordsets**, die durch die Form-Compute-Befehle generiert werden, die dazwischen liegenden Ebenen einnimmt.  
  
 Die typische Verwendung dieses Features besteht darin, die Aggregatfunktion und die Gruppierungs Möglichkeiten der shapecompute-Befehle aufzurufen, um zwischengeschaltete **Recordsetobjekte** mit analytischen Informationen über das untergeordnete **Recordset** zu erstellen. Da dies ein parametrisierter Shape-Befehl ist, kann jedes Mal, wenn auf eine Kapitel Spalte des übergeordneten Elements zugegriffen wird, ein neues untergeordnetes **Recordset** abgerufen werden. Da die dazwischen liegenden Ebenen vom untergeordneten Element abgeleitet sind, werden Sie ebenfalls neu berechnet.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenstrukturierung – Beispiel](../../../ado/guide/data/data-shaping-example.md)
