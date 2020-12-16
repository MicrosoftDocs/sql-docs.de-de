---
title: Prüfpunktvorgang für speicheroptimierte Tabellen
description: Hier erfahren Sie mehr über Prüfpunkte in speicheroptimierten Tabellen in SQL Server. Der Prüfpunktvorgang für speicheroptimierte Tabellen unterscheidet sich von dem Vorgang für datenträgerbasierte Tabellen.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 47975bd5-373f-43cd-946a-da8e8088b610
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8ea64147c8a7bf9f6142b36cb048d7ad39f43b66
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465391"
---
# <a name="checkpoint-operation-for-memory-optimized-tables"></a>Prüfpunktvorgang für speicheroptimierte Tabellen
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Für speicheroptimierte Daten in Daten- und Änderungsdateien muss in regelmäßigen Abständen ein Prüfpunkt erstellt werden, um den aktiven Teil des Transaktionsprotokolls fortzuführen. Der Prüfpunkt ermöglicht die Wiederherstellung speicheroptimierter Tabellen am letzten erfolgreichen Prüfpunkt. Anschließend wird der aktive Teil des Transaktionsprotokolls angewendet, um die speicheroptimierten Tabellen zu aktualisieren und die Wiederherstellung abzuschließen. Bei den Prüfpunktvorgängen für datenträgerbasierte und speicheroptimierte Tabellen handelt es sich um unterschiedliche Vorgänge. Im Folgenden werden verschiedene Szenarien und das Prüfpunktverhalten für datenträgerbasierte und speicheroptimierte Tabellen beschrieben:  
  
## <a name="manual-checkpoint"></a>Manueller Prüfpunkt  
 Bei Ausgabe eines manuellen Prüfpunkts wird der Prüfpunkt sowohl für datenträgerbasierte als auch für speicheroptimierte Tabellen geschlossen. Die aktive Datendatei wird geschlossen, obwohl sie möglicherweise teilweise gefüllt ist.  
  
## <a name="automatic-checkpoint"></a>Automatischer Prüfpunkt  
 Automatische Prüfpunkte werden für datenträgerbasierte und speicheroptimierte Tabellen unterschiedlich implementiert, da die Beibehaltung der Daten sich bei diesen beiden Tabellentypen unterscheidet.  
  
 Für datenträgerbasierte Tabellen wird ein automatischer Prüfpunkt basierend auf der Konfigurationsoption Wiederherstellungsintervall erstellt. (Weitere Informationen finden Sie unter [Ändern der Zielwiederherstellungszeit einer Datenbank &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md).)  
  
 Für speicheroptimierte Tabellen wird ein automatischer Prüfpunkt erstellt, wenn die Größe der Transaktionsprotokolldatei seit dem letzten Prüfpunkt um 1,5 GB gestiegen ist. In dieser Größe von 1,5 GB sind Transaktionsprotokolldatensätze sowohl für datenträgerbasierte als auch für speicheroptimierte Tabellen enthalten.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen und Verwalten von Speicher für speicheroptimierte Objekte](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)  
  
  
