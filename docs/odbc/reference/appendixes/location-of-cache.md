---
description: Speicherort des Caches
title: Speicherort des Caches | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], cache
- cursor library [ODBC], cache
- cache [ODBC]
ms.assetid: 240d6162-4da6-4b1f-96c7-f379f4ecb16f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b500aa9b7955c91c6ca39b3f7f830b2022dc4a57
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184361"
---
# <a name="location-of-cache"></a>Speicherort des Caches
> [!IMPORTANT]  
>  Diese Funktion wird in einer zukünftigen Version von Windows entfernt. Vermeiden Sie die Verwendung dieses Features bei der Entwicklung neuer Anwendungen, und planen Sie das Ändern von Anwendungen, in denen diese Funktion derzeit verwendet wird Microsoft empfiehlt die Verwendung der Cursor-Funktionalität des Treibers.  
  
 Die Cursor Bibliothek speichert Daten im Arbeitsspeicher und in Windows® temporären Dateien. Dadurch wird die Größe des Resultsets beschränkt, das von der Cursor Bibliothek nur durch den verfügbaren Speicherplatz behandelt werden kann. Eine temporäre Datei wird verwendet, wenn die Daten, die zwischengespeichert werden sollen, die Segment Grenze überschreiten würden, wenn Sie am Ende des Cursor-Bibliotheks Caches eingefügt werden. Stattdessen werden die zwischengespeicherten Daten anstelle des zuletzt gespeicherten Datenblocks im Cache hinzugefügt. Der zuletzt gespeicherte Datenblock wird in einer temporären Datei gespeichert. Wenn die Cursor Bibliothek nicht ordnungsgemäß beendet wird, z. b. wenn der Strom ausfällt, können temporäre Windows-Dateien auf dem Datenträger belassen werden. Diese werden mit dem Namen ~ CTT *nnnn*. tmp benannt und im aktuellen Verzeichnis erstellt.  
  
> [!NOTE]  
>  Wenn die Cursor Bibliothek in Microsoft® Windows NT®/Windows2000 versucht, Daten in einer temporären Datei im aktuellen Verzeichnis zwischenzuspeichern, während die Anwendung von einer schreibgeschützten Freigabe oder einem Compact Disk (z. b. einem Microsoft Foundation Class-Bibliothek Beispiel) ausgeführt wird, wird SQLSTATE HY000 (allgemeine Error-Unable zum Erstellen eines Datei Puffers) zurückgegeben.
