---
description: Abonnements überprüfen
title: Abonnements überprüfen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.validate.subscriptions.f1
helpviewer_keywords:
- Validate Subscriptions dialog box
ms.assetid: 0ca39a35-f22c-46c5-82a4-342e34bf5d1b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: bb8a56e2b165f5ccfa7db701a2d95792de47e19d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468681"
---
# <a name="validate-subscriptions"></a>Abonnements überprüfen
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
   Im Dialogfeld **Abonnements überprüfen** können Sie angeben, dass Abonnements für eine Transaktionsveröffentlichung überprüft werden sollen, sobald der Verteilungs-Agent für die einzelnen Abonnements das nächste Mal ausgeführt wird. Die Ergebnisse der Überprüfung werden im Replikationsmonitor angezeigt. Weitere Informationen finden Sie unter [Validate Data at the Subscriber](../../relational-databases/replication/validate-data-at-the-subscriber.md).  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
  
## <a name="options"></a>Optionen  
 **Alle SQL Server-Abonnements überprüfen**  
 Wählen Sie diese Option aus, um Daten für alle SQL Server-Abonnements für diese Veröffentlichung zu überprüfen.  
  
 **Folgende Abonnements überprüfen**  
 Wählen Sie diese Option aus, wenn Sie nicht alle Abonnements überprüfen möchten. Wählen Sie aus der Liste die Abonnements aus, die überprüft werden sollen.  
  
 **Überprüfungsoptionen**  
 Klicken Sie auf diese Option, um auf das Dialogfeld **Optionen für die Abonnementüberprüfung** zuzugreifen, in dem Sie angeben können, ob die Zeilenanzahlüberprüfung oder die binäre Prüfsummenüberprüfung verwendet werden soll.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Überprüfen von replizierten Daten](../../relational-databases/replication/validate-data-at-the-subscriber.md)  
  
  
