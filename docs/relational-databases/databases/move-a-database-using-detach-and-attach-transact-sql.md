---
description: Verschieben einer Datenbank durch Trennen und Anfügen (Transact-SQL)
title: Verschieben einer Datenbank durch Trennen und Anfügen (Transact-SQL)
ms.date: 06/03/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- database attaching [SQL Server]
- moving databases [SQL Server]
- database detaching [SQL Server]
- relocating databases [SQL Server]
- detaching databases [SQL Server]
- attaching databases [SQL Server]
ms.assetid: 6732a431-cdef-4f1e-9262-4ac3b77c275e
author: stevestein
ms.author: sstein
ms.custom: seo-dt-2019
ms.openlocfilehash: 99abbddfa51a62ef1075755126712515b3b5c9ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348008"
---
# <a name="move-a-database-using-detach-and-attach-transact-sql"></a>Verschieben einer Datenbank durch Trennen und Anfügen (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie eine getrennte Datenbank an einen anderen Speicherort verschoben und in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]an die gleiche oder eine andere Serverinstanz angefügt wird. Es wird jedoch empfohlen, Datenbanken mit der ALTER DATABASE-Prozedur für geplante Verschiebungen zu verschieben, anstatt die Optionen zum Trennen und Anfügen zu verwenden. Weitere Informationen finden Sie unter [Move User Databases](../../relational-databases/databases/move-user-databases.md).  
  
> [!IMPORTANT]  
>  Das Anfügen oder Wiederherstellen von Datenbanken aus unbekannten oder nicht vertrauenswürdigen Quellen wird nicht empfohlen. Solche Datenbanken können bösartigen Code enthalten, der möglicherweise unbeabsichtigten [!INCLUDE[tsql](../../includes/tsql-md.md)] -Code ausführt oder Fehler verursacht, indem er das Schema oder die physische Datenbankstruktur ändert. Bevor Sie eine Datenbank aus einer unbekannten oder nicht vertrauenswürdigen Quelle verwenden, führen Sie auf einem Nichtproduktionsserver [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) für die Datenbank aus. Überprüfen Sie außerdem den Code in der Datenbank, z.B. gespeicherte Prozeduren oder anderen benutzerdefinierten Code.  
  
## <a name="procedure"></a>Verfahren  
  
#### <a name="to-move-a-database-by-using-detach-and-attach"></a>So verschieben Sie eine Datenbank durch Trennen und Anfügen  
  
1.  Trennen Sie die Datenbank. Weitere Informationen finden Sie unter [Trennen einer Datenbank](../../relational-databases/databases/detach-a-database.md).  
  
2.  Verschieben Sie die getrennten Datenbankdateien und Protokolldateien im Windows-Explorer oder an der Windows-Eingabeaufforderung an den neuen Speicherort.  
  
     Sie sollten die Protokolldateien verschieben, selbst wenn Sie neue Protokolldateien erstellen möchten. In manchen Fällen sind zum erneuten Anfügen der Datenbank die vorhandenen Protokolldateien erforderlich. Deshalb sollten Sie immer alle getrennten Protokolldateien behalten, bis die Datenbank ohne sie erfolgreich angefügt wurde.  
  
    > [!NOTE]  
    >  Wenn Sie versuchen, die Datenbank ohne Angabe der Protokolldatei anzufügen, wird die Protokolldatei an ihrem ursprünglichen Speicherort gesucht. Falls noch eine Kopie der Protokolldatei im ursprünglichen Speicherort vorhanden ist, wird diese Kopie angefügt. Wenn Sie die Verwendung der ursprünglichen Protokolldatei verhindern möchten, geben Sie entweder den Pfad der neuen Protokolldatei an, oder entfernen Sie die ursprüngliche Kopie der Protokolldatei (nachdem Sie sie an einen neuen Speicherort kopiert haben).  
  
3.  Fügen Sie die kopierten Dateien an. Weitere Informationen finden Sie unter [Attach a Database](../../relational-databases/databases/attach-a-database.md).  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird eine Kopie der [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] -Datenbank mit dem Namen `MyAdventureWorks`erstellt. Die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen werden in einem Abfrage-Editorfenster ausgeführt, das mit der Serverinstanz verbunden ist, an die es angefügt ist.  
  
1.  Führen Sie die folgenden [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] -Anweisungen aus, um die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Datenbank zu trennen:  
  
    ```sql
    USE master;  
    GO  
    EXEC sp_detach_db @dbname = N'AdventureWorks2012';  
    GO  
    ```  
  
2.  Verwenden Sie die gewünschte Methode, und kopieren Sie die Datenbankdateien (AdventureWorks208R2_Data.mdf und AdventureWorks208R2_log) nach C:\MySQLServer\AdventureWorks208R2_Data.mdf bzw. C:\MySQLServer\AdventureWorks208R2_Log.ldf.  
  
    > [!IMPORTANT]  
    >  Platzieren Sie Datenbank und Transaktionsprotokoll bei einer Produktionsdatenbank auf separaten Datenträgern.  
  
     Um Dateien im Netzwerk auf einen Datenträger auf einem Remotecomputer zu kopieren, verwenden Sie den UNC-Namen (Universal Naming Convention) des Remotespeicherorts. Ein UNC-Name hat das Format **\\\\** _Servername_ **\\** _Sharename_ **\\** _Path_ **\\** _Filename_. Wie beim Schreiben von Dateien auf die lokale Festplatte müssen die entsprechenden Berechtigungen für das Lesen oder Schreiben einer Datei auf dem Remotedatenträger dem von der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz verwendeten Benutzerkonto erteilt werden.  
  
3.  Führen Sie die folgenden [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen aus, um die verschobene Datenbank und optional das zugehörige Protokoll anzufügen:  
  
    ```sql
    USE master;  
    GO  
    CREATE DATABASE MyAdventureWorks   
        ON (FILENAME = 'C:\MySQLServer\AdventureWorks2012_Data.mdf'),  
        (FILENAME = 'C:\MySQLServer\AdventureWorks2012_Log.ldf')  
        FOR ATTACH;  
    GO  
    ```  
  
     In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]ist eine neu angefügte Datenbank nicht sofort im Objekt-Explorer sichtbar. Um die Datenbank anzuzeigen, klicken Sie im Objekt-Explorer im Menü **Ansicht** auf **Aktualisieren**. Wenn der **Datenbanken** -Knoten im Objekt-Explorer erweitert wird, wird nun die neu angefügte Datenbank in der Liste der Datenbanken angezeigt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Anfügen und Trennen von Datenbanken &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)  
  
  
