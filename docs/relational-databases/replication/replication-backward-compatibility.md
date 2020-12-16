---
title: Abwärtskompatibilität von Replikationen | Microsoft-Dokumentation
description: Überprüfen Sie die in diesem Artikel bereitgestellten Ressourcen zur Abwärtskompatibilität bei der Replikation, bevor Sie Upgrades durchführen, oder wenn Sie über mehrere Versionen von SQL Server in einer Replikationstopologie verfügen.
ms.custom: ''
ms.date: 03/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- transactional replication, backward compatibility
- backward compatibility [SQL Server replication]
- merge replication backward compatibility [SQL Server replication]
- replication [SQL Server], backward compatibility
- backward compatibility [SQL Server], replication
- snapshot replication [SQL Server], backward compatibility
- compatibility [SQL Server replication]
ms.assetid: 091c51dc-8b32-4b4f-847e-b317456c8394
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: a48ca05acd680cfd89f4438a64cac83c5d00dd0f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468821"
---
# <a name="replication-backward-compatibility"></a>Abwärtskompatibilität von Replikationen
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Es ist wichtig, die Abwärtskompatibilität zu verstehen, wenn Sie ein Upgrade ausführen, oder wenn Sie über mehrere Versionen von SQL Server in einer Replikationstopologie verfügen. 

Allgemeine Regeln 

-   Für den Verteiler ist jede Version zulässig, die der Verleger-Version entspricht oder höher als diese ist. (In vielen Fällen gehören Verteiler und Verleger derselben Instanz an.)    
-   Für den Verleger ist jede Version zulässig, die der Verteiler-Version entspricht oder niedriger als diese ist.    
-   Die Abonnenten-Version ist vom Veröffentlichungstyp abhängig:    
    - Ein Abonnent einer Transaktionsveröffentlichung kann einer der beiden Versionen im Rahmen der Verlegerversion angehören. Zum Beispiel: Ein SQL Server 2012-Verleger (11.x) kann SQL Server 2014-Abonnenten (12.x) und SQL Server 2016-Abonnenten (13.x) vorweisen. Ein SQL Server 2016-Verleger (13.x) kann SQL Server 2014-Abonnenten (12.x) und SQL Server 2012-Abonnenten (11.x) vorweisen.     
    - Ein Abonnent für eine Mergeveröffentlichung kann alle Versionen, die gleich oder niedriger sind als die Verlegerversion, benutzen. Diese werden gemäß dem Supportzeitraums für den Versionenlebenszyklus unterstützt.  


## <a name="replication-matrix"></a>Replikationsmatrix
[!INCLUDE[repl matrix](../../includes/replication-compat-matrix.md)]


## <a name="additional-resources"></a>Weitere Ressourcen
 [Als veraltet markierte Funktionen der SQL Server-Replikation](../../relational-databases/replication/deprecated-features-in-sql-server-replication.md)  
 Replikationsfeatures, die in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] aufgrund der Abwärtskompatibilität beibehalten wurden, die jedoch in einer zukünftigen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entfernt werden.  
  
 [Wichtige Änderungen in der SQL Server-Replikation](../../relational-databases/replication/breaking-changes-in-sql-server-replication.md)  
 Änderungen an Replikationsfunktionen, die möglicherweise Änderungen an Anwendungen erfordern. 

 [Aktualisieren von replizierten Datenbanken](../../database-engine/install-windows/upgrade-replicated-databases.md)  
 Schritte und Überlegungen zum Upgrade von SQL Server-Instanzen, die an einer Replikationstopologie beteiligt sind. 
  
  
