---
description: MSSQL_ENG014117
title: MSSQL_ENG014117 | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG014117 error
ms.assetid: e5906a76-9511-4c47-8826-8c765b58a39d
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: b8eab851e7bde95fa0ab6aaf2ee75ae0ffc6d0a1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481011"
---
# <a name="mssql_eng014117"></a>MSSQL_ENG014117
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Meldungsdetails  
  
|attribute|Wert|  
|-|-|  
|Produktname|SQL Server|  
|Ereignis-ID|14117|  
|Ereignisquelle|MSSQLSERVER|  
|Komponente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Symbolischer Name||  
|Meldungstext|'%1!s!' ist nicht als Verteilungsdatenbank konfiguriert.|  
  
## <a name="explanation"></a>Erklärung  
 Dieser Fehler kann auftreten, wenn eine oder beide der folgenden Aussagen wahr sind:  
  
-   In **msdb..MSdistributiondbs** fehlt der Eintrag für die angegebene Verteilungsdatenbank.  
  
-   In der **master** -Datenbank gibt es keinen Eintrag für den lokalen Server, oder es ist zwar ein Eintrag vorhanden, dieser ist aber falsch.  
  
     Die Replikation erwartet, dass alle Server in einer Topologie mithilfe des Computernamens mit einem optionalen Instanznamen (bei Clusterinstanzen mithilfe des virtuellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Servernamens mit dem optionalen Instanznamen) registriert werden. Damit die Replikation ordnungsgemäß funktioniert, muss der von `SELECT @@SERVERNAME` für jeden Server in der Topologie zurückgegebene Wert mit dem Computernamen bzw. dem virtuellen Servernamen mit dem optionalen Instanznamen übereinstimmen.  
  
     Die Replikation wird nicht unterstützt, wenn Sie eine der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanzen über die IP-Adresse oder den vollqualifizierten Domänennamen registriert haben. Dieser Fehler kann ausgelöst werden, wenn beim Konfigurieren der Replikation eine der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanzen über die IP-Adresse oder den vollqualifizierten Domänennamen in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] registriert war.  
  
## <a name="user-action"></a>Benutzeraktion  
 Überprüfen Sie, ob die Verteilerinstanz ordnungsgemäß registriert ist. Wenn der Netzwerkname des Computers und der Name der SQL Server-Instanz nicht identisch sind, gehen Sie wie folgt vor:  
  
-   Fügen Sie den SQL Server-Instanznamen als gültigen Netzwerknamen hinzu. Eine Möglichkeit, einen alternativen Netzwerknamen festzulegen, besteht darin, diesen Namen der lokalen Hostdatei hinzuzufügen. Die lokale Hostdatei befindet sich im Verzeichnis WINDOWS\system32\drivers\usw. oder WINNT\system32\drivers\usw. Weitere Informationen finden Sie in der Windows-Dokumentation.  
  
     Wenn der Computername z. B. comp1 ist und die IP-Adresse des Computers 10.193.17.129 lautet und wenn der Instanzname inst1/instname ist, ist der Hostdatei der folgende Eintrag hinzuzufügen:  
  
     10.193.17.129 inst1  
  
-   Deaktivieren Sie die Verteilung, registrieren Sie die Instanz, und stellen Sie dann die Verteilung wieder her. Wenn der @@SERVERNAME-Wert für eine nicht in einem Cluster befindliche Instanz falsch ist, führen Sie die folgenden Schritte aus:  
  
    ```  
    sp_dropserver '<old_name>', 'droplogins'  
    go  
    sp_addserver '<new_name>', 'local'  
    go  
    ```  
  
     Nachdem Sie die gespeicherte Prozedur [sp_addserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)ausgeführt haben, müssen Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienst neu starten, damit die Änderung an @@SERVERNAME wirksam wird.  
  
     Wenn der @@SERVERNAME-Wert für eine in einem Cluster befindliche Instanz falsch ist, müssen Sie mithilfe der Clusterverwaltung den Namen ändern. Weitere Informationen finden Sie unter [ Always On-Failoverclusterinstanzen (SQL Server)](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md).  
  
 Nachdem Sie die ordnungsgemäße Registrierung der Verteilerinstanz geprüft haben, kontrollieren Sie, dass die Verteilungsdatenbank in **msdb..MSdistributiondbs** aufgeführt ist. Falls nicht, gehen Sie wie folgt vor:  
  
1.  Erstellen Sie ein Skript für die Verteilungskonfiguration. Weitere Informationen finden Sie unter [Scripting Replication](../../relational-databases/replication/scripting-replication.md).  
  
2.  Deaktivieren Sie die Verteilung, und aktivieren Sie sie dann erneut. Weitere Informationen finden Sie unter [Configure Distribution](../../relational-databases/replication/configure-distribution.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Fehler- und Ereignisreferenz &#40;Replikation&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
