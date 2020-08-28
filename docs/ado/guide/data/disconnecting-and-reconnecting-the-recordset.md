---
description: Trennen und erneutes Herstellen einer Verbindung mit dem Recordset
title: Trennen und erneutes Verbinden des Recordsets | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset object [ADO], disconnecting and reconnecting
ms.assetid: c5134af7-81d6-4de4-9fd1-cfe29973545e
author: rothja
ms.author: jroth
ms.openlocfilehash: 6f8a759ed099f51e1256a8e4701c0aed34a3ff03
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88991371"
---
# <a name="disconnecting-and-reconnecting-the-recordset"></a>Trennen und erneutes Herstellen einer Verbindung mit dem Recordset
Eines der leistungsstärksten Funktionen in ADO ist die Möglichkeit, ein Client seitiges Recordset aus einer Datenquelle zu öffnen und dann das Recordset von der Datenquelle zu trennen. Nachdem die Verbindung mit der recordmenge getrennt wurde, kann die Verbindung mit der Datenquelle geschlossen werden. Dadurch werden die Ressourcen auf dem Server freigegeben, der zur Wartung verwendet wird. Sie können die Daten im Recordset weiterhin anzeigen und bearbeiten, während Sie getrennt sind, und später erneut eine Verbindung mit der Datenquelle herstellen und Ihre Updates im Batch Modus senden.  
  
 Um die Verbindung mit einem Recordset zu trennen, öffnen Sie es mit der Cursorposition adUseClient, und legen Sie dann die ActiveConnection-Eigenschaft auf "Nothing" fest. (C++ Benutzer sollten die Aktivierung von ActiveConnection auf NULL festlegen.)  
  
 Wir werden später in diesem Abschnitt ein nicht verbundenes Recordset verwenden, wenn wir die recordsetpersistenz zum Behandeln eines Szenarios erörtern, in dem die Daten in einem Recordset für eine Anwendung verfügbar sein müssen, während der Client Computer nicht mit einem Netzwerk verbunden ist.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Batchmodus](./batch-mode.md)