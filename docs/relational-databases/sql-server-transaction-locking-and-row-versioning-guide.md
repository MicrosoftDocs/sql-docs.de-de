---
description: Handbuch zu Transaktionssperren und Zeilenversionsverwaltung
title: Handbuch zu Transaktionssperren und Zeilenversionsverwaltung
ms.custom: seo-dt-2019
ms.date: 03/10/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- guide, transaction locking and row versioning
- transaction locking and row versioning guide
- lock compatibility matrix, [SQL Server]
- lock granularity and hierarchies, [SQL Server]
- deadlocks, [SQL Server]
- lock escalation, [SQL Server]
- lock partitioning, [SQL Server]
ms.assetid: 44fadbee-b5fe-40c0-af8a-11a1eecf6cb7
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 47545adc6e3e620cd74585d477bdd028f1e8625f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352497"
---
# <a name="transaction-locking-and-row-versioning-guide"></a>Handbuch zu Transaktionssperren und Zeilenversionsverwaltung
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

In jeder Datenbank führt die fehlerhafte Verwaltung von Transaktionen bei Systemen mit zahlreichen Benutzern häufig zu Konflikten und Leistungsproblemen. Mit steigender Anzahl von Benutzern, die auf die Daten zugreifen, wird der Einsatz von Anwendungen, die Transaktionen effizient verwenden, immer wichtiger. In diesem Handbuch werden Mechanismen für Sperren und die Zeilenversionsverwaltung beschrieben, durch die das [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] die physische Integrität jeder Transaktion sicherstellt. Darüber hinaus erfahren Sie, wie Transaktionen von Anwendungen effizient gesteuert werden.  
  
**Gilt für:** [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ([!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] bis [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)], sofern nicht anders angegeben) und [!INCLUDE[ssSDSfull](../includes/sssdsfull-md.md)]. 
  
##  <a name="transaction-basics"></a><a name="Basics"></a> Grundlagen zu Transaktionen  
 Eine Transaktion ist eine Folge von Operationen, die als einzelne logische Arbeitseinheit ausgeführt wird. Eine logische Arbeitseinheit muss vier Eigenschaften aufweisen, die als ACID-Eigenschaften (Atomicity, Consistency, Isolation und Durability; Unteilbarkeit, Konsistenz, Isolation und Beständigkeit) bezeichnet werden, um als Transaktion zu gelten.  
  
 **Unteilbarkeit**  
 Eine Transaktion muss eine unteilbare Arbeitseinheit sein; entweder werden alle durch sie vorgesehenen Datenänderungen oder keine der Änderungen ausgeführt.  
  
 **Konsistenz**  
 Am Ende einer Transaktion müssen sich alle Daten in einem konsistenten Status befinden. In einer relationalen Datenbank müssen alle Regeln auf die Änderungen der Transaktion angewendet werden, um die Integrität aller Daten zu erhalten. Alle internen Datenstrukturen, wie B-Struktur-Indizes oder doppelt verknüpfte Listen, müssen am Ende der Transaktion richtig sein.  
  
 **Isolation**  
 Änderungen, die von gleichzeitigen Transaktionen ausgeführt werden, müssen von allen Änderungen, die von anderen gleichzeitigen Transaktionen ausgeführt werden, isoliert sein. Einer Transaktion stehen Daten entweder in dem Status zur Verfügung, in dem sie sich vor der Änderung durch eine andere gleichzeitige Transaktion befanden, oder in dem Status nach Beenden der zweiten Transaktion, jedoch nicht in einem Zwischenstatus. Dies wird als Serialisierbarkeit bezeichnet, da sich daraus die Fähigkeit ableitet, die Ausgangsdaten erneut zu laden und eine Reihe von Transaktionen erneut durchzuführen, um schließlich die Daten in dem Status zu erhalten, der vorlag, nachdem die ursprünglichen Transaktionen ausgeführt wurden.  
  
 **Dauerhaftigkeit**  
 Nach Abschluss einer voll beständigen Transaktion sind ihre Auswirkungen im System dauerhaft. Die Änderungen bleiben auch bei einem Systemfehler persistent. [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] und höher ermöglichen verzögerte dauerhafte Transaktionen. Für verzögerte dauerhafte Transaktionen wird ein Commit ausgeführt, bevor der Transaktionsprotokolldatensatz auf dem Datenträger beibehalten wird. Weitere Informationen zur Dauerhaftigkeit verzögerter Transaktionen finden Sie im Thema [Transaktionsdauerhaftigkeit](../relational-databases/logs/control-transaction-durability.md).  
  
 SQL-Programmierer sind dafür verantwortlich, Transaktionen an Punkten zu starten und zu beenden, die die logische Konsistenz der Daten erzwingen. Der Programmierer muss die Sequenz der Datenänderungen so definieren, dass die Daten hinsichtlich der Geschäftsregeln der Organisation in konsistentem Status bleiben. Daraufhin schließt der Programmierer diese Änderungsanweisungen in eine einzelne Transaktion ein, sodass [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] die physische Integrität der Transaktion erzwingen kann.  
  
 Es ist die Aufgabe eines Unternehmensdatenbank-Systems, wie z. B. einer Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], Mechanismen bereitzustellen, durch die die physische Integrität aller Transaktionen sichergestellt wird. [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] stellt Folgendes bereit:  
  
-   Sperrvorrichtungen, durch die die Isolation jeder Transaktion erhalten bleibt.  
  
-   Protokolliervorrichtungen stellen die Beständigkeit von Transaktionen sicher. Bei vollständig dauerhaften Transaktionen wird der Protokolldatensatz vor dem Transaktionscommit auf den Datenträger geschrieben. Bei einem Fehler der Serverhardware, des Betriebssystems oder der Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] selbst verwendet die Instanz nach dem Neustart die Transaktionsprotokolle, um automatisch einen Rollback für alle nicht beendeten Transaktionen auszuführen, der sie auf ihren Status vor dem Systemfehler zurücksetzt. Für verzögerte dauerhafte Transaktionen wird ein Commit ausgeführt, bevor der Transaktionsprotokolldatensatz auf dem Datenträger gespeichert wird. Solche Transaktionen gehen möglicherweise verloren, wenn ein Systemfehler auftritt, bevor die Protokolldatensätze auf dem Datenträger gespeichert werden. Weitere Informationen zur Dauerhaftigkeit verzögerter Transaktionen finden Sie im Thema [Transaktionsdauerhaftigkeit](../relational-databases/logs/control-transaction-durability.md).  
  
-   Funktionen der Transaktionsverwaltung, die die Unteilbarkeit und Konsistenz der Transaktionen erzwingen. Nach Beginn einer Transaktion muss die Transaktion erfolgreich (mit einem Commit) beendet werden, da die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Instanz sonst alle Datenänderungen rückgängig macht, die seit Beginn der Transaktion ausgeführt wurden. Dieser Vorgang wird als Rollback einer Transaktion bezeichnet, da die Daten in den Zustand zurückversetzt werden, der vor den Änderungen gültig war.  
  
### <a name="controlling-transactions"></a>Steuern von Transaktionen  
 Transaktionen werden von Anwendungen hauptsächlich durch Angeben der Zeitpunkte für Transaktionsbeginn und -ende gesteuert. Die Steuerung kann über [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisungen oder Datenbank-API-Funktionen erfolgen. Das System muss auch in der Lage sein, Fehler richtig zu behandeln, die eine Transaktion vor deren Abschluss beenden. Weitere Informationen finden Sie unter [Transaktionen](../t-sql/language-elements/transactions-transact-sql.md), [Transaktionen in ODBC](../relational-databases/native-client/odbc/performing-transactions-in-odbc.md) und [Transaktionen in SQL Server Native Client (OLEDB)](../relational-databases/native-client-ole-db-transactions/transactions.md).  
  
 Standardmäßig werden Transaktionen auf der Verbindungsebene verwaltet. Wenn eine Transaktion über eine Verbindung gestartet wird, sind alle [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisungen, die über diese Verbindung ausgeführt werden, Teil der Transaktion, bis diese endet. In einer MARS-Sitzung (Multiple Active Result Set) wird eine explizite oder implizite [!INCLUDE[tsql](../includes/tsql-md.md)]-Transaktion jedoch zu einer Transaktion mit Batchbereich, die auf der Batchebene verwaltet wird. Wenn der Batch abgeschlossen wird, führt [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] automatisch einen Rollback aus, wenn für die Transaktion mit Batchbereich kein Commit oder Rollback ausgeführt wurde. Weitere Informationen finden Sie unter [Verwenden von Multiple Active Result Sets (MARS)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).  
  
#### <a name="starting-transactions"></a>Starten von Transaktionen  
 Mithilfe von API-Funktionen und [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisungen können Sie Transaktionen in einer Instanz des [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] als explizite, implizite oder Autocommit-Transaktionen starten.  
  
 **Explizite Transaktionen**  
 Eine explizite Transaktion ist eine Transaktion, in der Sie sowohl den Beginn als auch das Ende der Transaktion über eine API-Funktion oder durch Ausgabe einer der [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisungen BEGIN TRANSACTION, COMMIT TRANSACTION, COMMIT WORK oder ROLLBACK TRANSACTION bzw. der ROLLBACK WORK-Anweisungen ([!INCLUDE[tsql](../includes/tsql-md.md)]) explizit festlegen. Am Ende der Transaktion kehrt die Verbindung zu dem Transaktionsmodus zurück, in dem sie sich vor Beginn der expliziten Transaktion befand, also entweder zum impliziten oder zum Autocommitmodus.  
  
 Sie können alle [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisungen in einer expliziten Transaktion verwenden; ausgenommen davon sind die folgenden Anweisungen:  
  
:::row:::
    :::column:::
        ALTER DATABASE
    :::column-end:::
    :::column:::
        CREATE DATABASE
    :::column-end:::
    :::column:::
        DROP FULLTEXT INDEX
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ALTER FULLTEXT CATALOG
    :::column-end:::
    :::column:::
        CREATE FULLTEXT CATALOG
    :::column-end:::
    :::column:::
        RECONFIGURE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        ALTER FULLTEXT INDEX
    :::column-end:::
    :::column:::
        CREATE FULLTEXT INDEX
    :::column-end:::
    :::column:::
        RESTORE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        BACKUP
    :::column-end:::
    :::column:::
        DROP DATABASE
    :::column-end:::
    :::column:::
        Gespeicherte Volltext-Systemprozeduren
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE DATABASE
    :::column-end:::
    :::column:::
        DROP FULLTEXT CATALOG
    :::column-end:::
    :::column:::
        sp_dboption zum Festlegen von Datenbankoptionen oder einer beliebigen Systemprozedur, durch die die Masterdatenbank in expliziten bzw. impliziten Transaktionen geändert wird.
    :::column-end:::
:::row-end:::

> [!NOTE]  
> UPDATE STATISTICS kann in einer expliziten Transaktion verwendet werden. UPDATE STATISTICS führt jedoch unabhängig von der einschließenden Transaktion einen Commit aus, und es kann kein Rollback ausgeführt werden.  
  
 **Autocommit-Transaktionen**  
 Der Autocommit-Modus ist der Standardmodus zur Transaktionsverwaltung von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. Für jede [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisung wird beim Beenden ein Commit oder Rollback ausgeführt. Wenn eine Anweisung erfolgreich beendet wird, wird ein Commit ausgeführt; wenn hingegen Fehler auftreten, wird ein Rollback ausgeführt. Eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] befindet sich immer dann im Autocommit-Modus, wenn dieser Standardmodus nicht durch explizite oder implizite Transaktionen überschrieben wird. Der Autocommit-Modus ist ebenfalls der Standardmodus für ADO, OLE DB, ODBC und DB-Library.  
  
 **Implizite Transaktionen**  
 Wenn eine Verbindung sich im impliziten Transaktionsmodus befindet, startet [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] automatisch eine neue Transaktion, nachdem für die aktuelle Transaktion ein Commit oder Rollback ausgeführt wurde. Die Kennzeichnung des Starts einer Transaktion entfällt; Sie führen nur einen Commit oder Rollback für die einzelnen Transaktionen aus. Im impliziten Transaktionsmodus wird eine fortlaufende Kette von Transaktionen generiert. Sie legen den impliziten Transaktionsmodus entweder durch eine API-Funktion oder durch die [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisung SET IMPLICIT_TRANSACTIONS ON fest.  Dieser Modus wird auch Autocommit OFF genannt. Weitere Informationen finden Sie unter [setAutoCommit-Methode](../connect/jdbc/reference/setautocommit-method-sqlserverconnection.md). 
  
 Nachdem der implizite Transaktionsmodus für eine Verbindung aktiviert wurde, startet [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] automatisch eine Transaktion, wenn eine dieser Anweisungen zum ersten Mal ausgeführt wird:  
  
:::row:::
    :::column:::
        ALTER TABLE
    :::column-end:::
    :::column:::
        FETCH
    :::column-end:::
    :::column:::
        REVOKE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        CREATE
    :::column-end:::
    :::column:::
        GRANT
    :::column-end:::
    :::column:::
        SELECT
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        Delete
    :::column-end:::
    :::column:::
        INSERT
    :::column-end:::
    :::column:::
        TRUNCATE TABLE
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        DROP
    :::column-end:::
    :::column:::
        OPEN
    :::column-end:::
    :::column:::
        UPDATE
    :::column-end:::
:::row-end:::

-  **Transaktionen mit Batchbereich**  
   Trifft nur auf MARS (Multiple Active Result Sets) zu; eine explizite oder implizite [!INCLUDE[tsql](../includes/tsql-md.md)]-Transaktion, die unter einer MARS-Sitzung gestartet wird, wird zu einer Transaktion im Batchbereich. Für eine Transaktionen mit Batchbereich, für die nach Abschluss des Batches kein Commit oder Rollback ausgeführt wird, wird das Rollback automatisch durch [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] vorgenommen.  
  
-  **Distributed Transactions** (Verteilte Transaktionen)  
   Verteilte Transaktionen erstrecken sich auf mindestens zwei Server, die als Ressourcen-Manager bekannt sind. Die Verwaltung der Transaktionen muss zwischen den Ressourcen-Managern von einer Serverkomponente, dem Transaktions-Manager, koordiniert werden. Jede [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Instanz kann als Ressourcen-Manager in verteilten Transaktionen eingesetzt werden, die von Transaktions-Managern, wie [!INCLUDE[msCoName](../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC) oder anderen Transaktions-Managern, die die Open Group XA-Spezifikation für die verteilte Transaktionsverarbeitung unterstützen, koordiniert werden. Weitere Informationen finden Sie in der MS DTC-Dokumentation.  
  
   Eine Transaktion auf einem einzelnen Computer mit [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] die sich auf mindestens zwei Datenbanken erstreckt, ist tatsächlich eine verteilte Transaktion. Die Instanz verwaltet die verteilte Transaktion jedoch intern; für den Benutzer entsteht der Eindruck, es handele sich um eine lokale Transaktion.  
  
   Auf der Anwendungsebene wird eine verteilte Transaktion beinahe so wie eine lokale Transaktion verwaltet. Am Ende der Transaktion fordert die Anwendung die Transaktion auf, entweder einen Commit oder Rollback auszuführen. Ein verteilter Commit darf vom Transaktions-Manager nicht auf dieselbe Art verwaltet werden, um das Risiko zu minimieren, dass einige Ressourcen-Manager bei einem Netzwerkfehler den Commit erfolgreich ausführen, während andere für die Transaktion einen Rollback ausführen. Dies wird dadurch erreicht, dass der Commitvorgang in zwei Phasen verwaltet wird (die Vorbereitungsphase und die Commitphase), bekannt als Zweiphasencommit (2PC).  
  
   -  **Vorbereitungsphase**  
      Wenn der Transaktions-Manager eine Anforderung für ein Commit erhält, sendet er einen Vorbereitungsbefehl an alle Ressourcen-Manager, die an der Transaktion beteiligt sind. Jeder Ressourcen-Manager trifft dann die notwendigen Vorbereitungen, um die Transaktion beständig zu machen, und alle Puffer, die Images von Protokollen für die Transaktion enthalten, werden auf den Datenträger geleert. Wenn die Ressourcen-Manager die Vorbereitungsphase beenden, geben sie jeweils eine Information über den Erfolg oder das Fehlschlagen der Vorbereitung an den Transaktions-Manager zurück. Mit [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] wurde die verzögerte Transaktionsdauerhaftigkeit eingeführt. Für verzögerte dauerhafte Transaktionen wird ein Commit ausgeführt, bevor das Protokollimage auf den Datenträger geleert wird. Weitere Informationen zur Dauerhaftigkeit verzögerter Transaktionen finden Sie im Thema [Transaktionsdauerhaftigkeit](../relational-databases/logs/control-transaction-durability.md).  
  
   -  **Commitphase**  
      Wenn der Transaktions-Manager von der erfolgreichen Vorbereitung aller Ressourcen-Manager in Kenntnis gesetzt wird, sendet er Commitbefehle an alle Ressourcen-Manager. Die Ressourcen-Manager können dann den Commit beenden. Wenn alle Ressourcen-Manager eine erfolgreiche Ausführung des Commits melden, sendet der Transaktions-Manager eine Benachrichtigung über die erfolgreiche Ausführung an die Anwendung. Wenn einer der Ressourcen-Manager einen Fehler bei der Vorbereitung ausgibt, sendet der Transaktions-Manager einen Rollbackbefehl an alle Ressourcen-Manager und benachrichtigt die Anwendung über die fehlgeschlagene Ausführung des Commits.  
  
      [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Anwendungen können verteilte Transaktionen entweder über [!INCLUDE[tsql](../includes/tsql-md.md)] oder die Datenbank-API verwalten. Weitere Informationen finden Sie unter [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../t-sql/language-elements/begin-distributed-transaction-transact-sql.md).  
  
#### <a name="ending-transactions"></a>Beenden von Transaktionen  
 Sie können Transaktionen entweder mit einer COMMIT-Anweisung oder ROLLBACK-Anweisung oder durch eine entsprechende API-Funktion beenden.  
  
-  **COMMIT**  
   Wenn eine Transaktion erfolgreich ist, führen Sie einen Commit aus. Durch eine COMMIT-Anweisung wird sichergestellt, dass alle Änderungen der Transaktion zum dauerhaften Bestandteil der Datenbank werden. Durch eine COMMIT-Anweisung werden auch von der Transaktion verwendete Ressourcen, wie etwa Sperren, freigegeben.  
  
-  **ROLLBACK**  
   Wenn ein Fehler in einer Transaktion auftritt, oder wenn ein Benutzer beschließt, die Transaktion abzubrechen, führen Sie ein Rollback aus. Durch eine ROLLBACK-Anweisung werden alle Änderungen, die während der Transaktion vorgenommen wurden, rückgängig gemacht, sodass die Daten in ihren Ausgangsstatus zurückversetzt werden. Durch das ROLLBACK werden auch Ressourcen freigegeben, die von der Transaktion beansprucht wurden.  
  
> [!NOTE]  
> Bei Verbindungen, die für die Unterstützung von MARS (Multiple Active Result Sets) aktiviert sind, kann für eine durch eine API-Funktion gestartete explizite Transaktion kein Commit ausgeführt werden, solange Ausführungsanforderungen anstehen. Jeder Versuch, ein Commit für eine derartige Transaktion auszuführen, für die noch ausstehende Vorgänge vorhanden sind, führt zu einem Fehler.  
  
#### <a name="errors-during-transaction-processing"></a>Fehler während der Transaktionsverarbeitung  
 Wenn eine Transaktion aufgrund eines Fehlers nicht erfolgreich beendet werden kann, führt [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] automatisch ein Rollback für die Transaktion aus und gibt alle Ressourcen frei, die von der Transaktion beansprucht wurden. Wenn die Netzwerkverbindung des Clients mit einer Instanz des [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] unterbrochen ist, wird für alle ausstehenden Transaktionen dieser Verbindung ein Rollback ausgeführt, sobald das Netzwerk die Instanz über die Unterbrechung benachrichtigt. Wenn die Clientanwendung einen Fehler erzeugt oder wenn der Clientcomputer heruntergefahren oder neu gestartet wird, kommt es ebenfalls zu einer Unterbrechung der Verbindung, und die Instanz des [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] führt ein Rollback für alle ausstehenden Verbindungen aus, sobald das Netzwerk es über die Unterbrechung benachrichtigt. Wenn sich der Client von der Anwendung abmeldet, wird für alle ausstehenden Transaktionen ein Rollback ausgeführt.  
  
 Wenn ein Anweisungsfehler zur Laufzeit (wie etwa eine Einschränkungsverletzung) in einem Batch auftritt, führt das [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] standardmäßig nur für die Anweisung ein Rollback aus, die den Fehler generiert hat. Sie können dieses Verhalten mithilfe der `SET XACT_ABORT`-Anweisung ändern. Nach dem Ausführen von `SET XACT_ABORT` ON führt jeder Anweisungsfehler zur Laufzeit dazu, dass automatisch ein Rollback für die aktuelle Transaktion ausgeführt wird. Kompilierungsfehler, wie z.B. Syntaxfehler, sind von `SET XACT_ABORT` nicht betroffen. Weitere Informationen finden Sie unter [SET XACT_ABORT &#40;Transact-SQL&#41;](../t-sql/statements/set-xact-abort-transact-sql.md).  
  
 Für den Fall, dass Fehler auftreten, sollte in den Anwendungscode eine korrigierende Aktion (`COMMIT` oder `ROLLBACK`) aufgenommen werden. Das [!INCLUDE[tsql](../includes/tsql-md.md)]-Konstrukt `TRY...CATCH` ist ein effektives Tool für die Fehlerbehandlung, einschließlich der Behandlung von Fehlern in Transaktionen. Weitere Informationen mit Beispielen zu Transaktionen finden Sie unter [TRY...CATCH &#40;Transact-SQL&#41;](../t-sql/language-elements/try-catch-transact-sql.md). Ab [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] kann die `THROW`-Anweisung verwendet werden, um eine Ausnahme auszulösen und die Ausführung an einen `CATCH`-Block eines `TRY...CATCH`-Konstrukts zu übergeben. Weitere Informationen finden Sie unter [THROW &#40;Transact-SQL&#41;](../t-sql/language-elements/throw-transact-sql.md).  
  
##### <a name="compile-and-run-time-errors-in-autocommit-mode"></a>Kompilierungs- und Laufzeitfehler im Autocommit-Modus  
 Im Autocommit-Modus entsteht hin und wieder der Eindruck, dass eine [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Instanz ein Rollback für einen gesamten Batch und nicht nur für eine einzelne SQL-Anweisung ausgeführt hat. Dies passiert, wenn es sich beim aufgetretenen Fehler um einen Kompilierungsfehler und nicht um einen Laufzeitfehler handelt. Bei einem Kompilierungsfehler wird verhindert, dass [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] einen Ausführungsplan erstellt; somit wird keine Anweisung im Batch ausgeführt. Obwohl der Eindruck entsteht, dass für alle Anweisungen vor derjenigen, die den Fehler generiert hat, ein Rollback ausgeführt wurde, hat der Fehler bereits verhindert, dass überhaupt eine Anweisung im Batch ausgeführt wurde. Im folgenden Beispiel wird aufgrund eines Kompilierungsfehlers keine der `INSERT`-Anweisungen im dritten Batch ausgeführt. Es entsteht der Eindruck, dass für die ersten zwei `INSERT`-Anweisungen ein Rollback ausgeführt wird, obwohl sie nie ausgeführt wurden.  
  
```sql  
CREATE TABLE TestBatch (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
INSERT INTO TestBatch VALUES (1, 'aaa');  
INSERT INTO TestBatch VALUES (2, 'bbb');  
INSERT INTO TestBatch VALUSE (3, 'ccc');  -- Syntax error.  
GO  
SELECT * FROM TestBatch;  -- Returns no rows.  
GO  
```  
  
 Im folgenden Beispiel generiert die dritte `INSERT`-Anweisung einen Laufzeitfehler aufgrund eines doppelten Primärschlüssels. Die ersten zwei `INSERT`-Anweisungen sind erfolgreich, sodass für sie ein Commit ausgeführt wird; sie bleiben somit nach dem Laufzeitfehler erhalten.  
  
```sql  
CREATE TABLE TestBatch (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
INSERT INTO TestBatch VALUES (1, 'aaa');  
INSERT INTO TestBatch VALUES (2, 'bbb');  
INSERT INTO TestBatch VALUES (1, 'ccc');  -- Duplicate key error.  
GO  
SELECT * FROM TestBatch;  -- Returns rows 1 and 2.  
GO  
```  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] verwendet die verzögerte Namensauflösung, bei der Objektnamen erst zur Ausführungszeit aufgelöst werden. Im folgenden Beispiel werden die ersten zwei `INSERT`-Anweisungen ausgeführt und mit einem Commit abgeschlossen; die entsprechenden beiden Zeilen bleiben in der `TestBatch`-Tabelle, nachdem die dritte `INSERT`-Anweisung einen Laufzeitfehler durch Verweisen auf eine nicht vorhandene Tabelle generiert.  
  
```sql  
CREATE TABLE TestBatch (Cola INT PRIMARY KEY, Colb CHAR(3));  
GO  
INSERT INTO TestBatch VALUES (1, 'aaa');  
INSERT INTO TestBatch VALUES (2, 'bbb');  
INSERT INTO TestBch VALUES (3, 'ccc');  -- Table name error.  
GO  
SELECT * FROM TestBatch;  -- Returns rows 1 and 2.  
GO  
```   
  
##  <a name="locking-and-row-versioning-basics"></a><a name="Lock_Basics"></a> Grundlagen zu Sperren und Zeilenversionsverwaltung  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] verwendet die folgenden Mechanismen, um die Integrität von Transaktionen sicherzustellen und die Konsistenz der Datenbanken beizubehalten, wenn mehrere Benutzer gleichzeitig auf Daten zugreifen:  
  
-  **Sperren**    

   Jede Transaktion fordert Sperren verschiedener Typen für die Ressourcen (wie z. B. Zeilen, Seiten oder Tabellen) an, von denen die Transaktion abhängt. Diese Sperren verhindern, dass die Ressourcen durch andere Transaktionen in einer Weise geändert werden, die zu Problemen für die Transaktion führen würde, die die Sperre angefordert hat. Jede Transaktion hebt ihre Sperren wieder auf, sobald sie nicht mehr von den gesperrten Ressourcen abhängig ist.  
  
-  **Zeilenversionsverwaltung**    

   Wenn eine auf Zeilenversionsverwaltung basierende Isolationsstufe aktiviert ist, bewahrt [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] Versionen jeder Zeile auf, an der Änderungen vorgenommen werden. Anwendungen können angeben, dass eine Transaktion die Zeilenversionen verwendet, um die Daten so anzuzeigen, wie sie zum Zeitpunkt des Transaktions- oder Abfragestarts vorgelegen haben, statt alle Lesevorgänge durch Sperren zu schützen. Durch Verwendung der Zeilenversionsverwaltung wird die Wahrscheinlichkeit, dass ein Lesevorgang zur Blockierung anderer Transaktionen führt, weitgehend reduziert.  
  
 Sperren und Zeilenversionsverwaltung verhindern, dass Benutzer Daten lesen, für die noch kein Commit ausgeführt wurde, und verhindern, dass viele Benutzer gleichzeitig versuchen, dieselben Daten zu ändern. Ohne Sperren oder Zeilenversionsverwaltung könnten Abfragen, die für Daten ausgeführt werden, zu unerwarteten Ergebnissen führen, weil Daten zurückgegeben werden, für die in der Datenbank noch kein Commit ausgeführt wurde.  
  
 Anwendungen können Transaktionsisolationsstufen auswählen. Diese Stufen definieren, inwieweit die jeweilige Transaktion vor Datenänderungen durch andere Transaktionen geschützt ist. Für einzelne [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisungen können Hinweise auf Tabellenebene angegeben werden, um das Verhalten noch weiter an die Anforderungen der Anwendung anzupassen.  
  
### <a name="managing-concurrent-data-access"></a>Verwalten des parallelen Datenzugriffs  
 Wenn Benutzer zum selben Zeitpunkt auf eine Ressource zugreifen, wird das als paralleler Zugriff auf die Ressource bezeichnet. Der parallele Datenzugriff erfordert Mechanismen, mit denen negative Auswirkungen vermieden werden, wenn mehrere Benutzer versuchen, Ressourcen zu ändern, die von anderen Benutzern aktiv verwendet werden.  
  
#### <a name="concurrency-effects"></a>Parallelitätseffekte  
 Benutzer, die Daten ändern, können einen Konflikt mit anderen Benutzern verursachen, die die gleichen Daten zur gleichen Zeit lesen oder ändern. Durch diese Benutzer erfolgt ein gleichzeitiger Zugriff auf die Daten. Wenn ein Datenspeichersystem keine Steuerung für die Parallelität besitzt, können Benutzer die folgenden Auswirkungen feststellen:  
  
-   **Verlorene Updates** 
  
     Verlorene Updates treten auf, wenn mindestens zwei Transaktionen dieselbe Zeile auswählen und diese Zeile dann auf der Grundlage des ursprünglich ausgewählten Werts aktualisieren. Eine einzelne Transaktion ist nicht über die anderen Transaktionen informiert. Das letzte Update überschreibt Updates von anderen Transaktionen; dies führt zu Datenverlusten.  
  
     Nehmen Sie beispielsweise an, dass zwei Redakteure eine elektronische Kopie desselben Dokuments erstellen. Jeder Redakteur ändert die eigene Kopie und speichert die geänderte Kopie anschließend, wodurch das Originaldokument überschrieben wird. Der Redakteur, der die Kopie zuletzt speichert, überschreibt die Änderungen des anderen Redakteurs. Das Problem könnte vermieden werden, wenn einer der Redakteure erst auf die Datei zugreifen kann, nachdem der andere Redakteur seine Arbeit beendet und ein Commit der Transaktion ausgeführt hat.  
  
-   **Abhängigkeit von Daten, für die kein Commit ausgeführt wurde (Dirty Read)**  
  
     Die Abhängigkeit von Daten, für die kein Commit ausgeführt wurde, tritt dann ein, wenn eine zweite Transaktion eine Zeile auswählt, die von einer anderen Transaktion aktualisiert wird. Die zweite Transaktion liest Daten, für die noch kein Commit ausgeführt wurde und die von der Transaktion, die die Zeile aktualisiert, noch geändert werden können.  
  
     Angenommen, ein Redakteur nimmt Änderungen an einem elektronischen Dokument vor. Während die Änderungen vorgenommen werden, verteilt ein zweiter Redakteur eine Kopie des Dokuments mit allen bisherigen Änderungen an die Zielgruppe. Der erste Redakteur entscheidet dann, dass die bisherigen Änderungen falsch sind, löscht sie und speichert das Dokument. Das verteilte Dokument enthält nun Änderungen, die nicht mehr vorhanden sind und so behandelt werden müssten, als ob sie nie vorhanden gewesen wären. Dieses Problem könnte vermieden werden, wenn das geänderte Dokument erst dann gelesen werden könnte, wenn der erste Redakteur die endgültige Speicherung der Änderungen vorgenommen und ein Commit der Transaktion ausgeführt hat.  
  
-   **Inkonsistente Analyse (nicht wiederholbarer Lesevorgang)**  
  
     Die inkonsistente Analyse tritt dann ein, wenn eine zweite Transaktion mehrmals auf dieselbe Zeile zugreift und jedes Mal verschiedene Daten liest. Die inkonsistente Analyse ist vergleichbar mit der Abhängigkeit von Daten, für die kein Commit ausgeführt wurde, da auch in diesem Fall eine andere Transaktion die Daten ändert, die eine zweite Transaktion liest. Bei der inkonsistenten Analyse wurde jedoch für die von der zweiten Transaktion gelesenen Daten ein Commit von der Transaktion, die die Änderung vornahm, ausgeführt. Darüber hinaus umfasst die inkonsistente Analyse mehrere Lesevorgänge (mindestens zwei) derselben Zeile, wobei jedes Mal die Informationen von einer anderen Transaktion geändert wurden; der Begriff "Nicht wiederholbarer Lesevorgang" bezieht sich auf diesen Vorgang.  
  
     Angenommen, ein Redakteur liest dasselbe Dokument zweimal, doch zwischen den einzelnen Lesedurchgängen schreibt der Verfasser das Dokument um. Wenn der Redakteur das Dokument zum zweiten Mal liest, unterscheidet es sich von der ersten Version. Der ursprüngliche Lesevorgang lässt sich nicht wiederholen. Dieses Problem könnte vermieden werden, wenn der Verfasser das Dokument erst ändern könnte, nachdem der Redakteur den letzten Lesevorgang beendet hat.  
  
-   **Phantomlesezugriffe**  
  
     Ein Phantomlesezugriff ist eine Situation, bei der zwei identische Abfragen ausgeführt werden, wobei die von der zweiten Abfrage zurückgegebene Zeilensammlung abweicht. Im unten stehenden Beispiel wird veranschaulicht, wie eine solche Situation auftreten kann. Angenommen, die beiden unten stehenden Transaktionen werden gleichzeitig ausgeführt. Die zwei `SELECT`-Anweisungen in der ersten Transaktion können ggf. andere Ergebnisse zurückgeben, da die `INSERT`-Anweisung in der zweiten Transaktion die von beiden verwendeten Daten ändert.  
  
    ```sql  
    --Transaction 1  
    BEGIN TRAN;  
    SELECT ID FROM dbo.employee  
    WHERE ID > 5 and ID < 10;  
    --The INSERT statement from the second transaction occurs here.  
    SELECT ID FROM dbo.employee  
    WHERE ID > 5 and ID < 10;  
    COMMIT;  
    ```  
  
    ```sql  
    --Transaction 2  
    BEGIN TRAN;  
    INSERT INTO dbo.employee  
      (Id, Name) VALUES(6 ,'New');  
    COMMIT;   
    ```  
  
-   **Durch Zeilenupdates verursachte fehlende und doppelte Lesezugriffe**  
  
    -   Übergehen einer aktualisierten Zeile oder mehrfaches Erkennen einer aktualisierten Zeile  
  
         Transaktionen, die auf der `READ UNCOMMITTED`-Ebene ausgeführt werden, geben keine freigegebenen Sperren aus, die verhindern würden, dass andere Transaktionen Daten ändern, die von der aktuellen Transaktion gelesen werden. Transaktionen, die auf der READ COMMITTED-Ebene ausgeführt werden, geben freigegebene Sperren aus. Diese Zeilen- oder Seitensperren werden jedoch aufgehoben, nachdem die Zeile gelesen wurde. In beiden Fällen kann beim Scannen eines Index eine Zeile zwei Mal erscheinen, wenn ein anderer Benutzer die Indexschlüsselspalte ändert, während Sie sie lesen, und die Spalte durch die Schlüsseländerung an eine Position hinter der aktuellen Leseposition verschoben wird. Ebenso ist es möglich, dass die Zeile nicht erscheint, wenn die Spalte durch die Schlüsseländerung an eine Indexposition verschoben wird, die bereits gelesen wurde. Um dies zu vermeiden, verwenden Sie den `SERIALIZABLE`- oder `HOLDLOCK`-Hinweis oder die Zeilenversionsverwaltung. Weitere Informationen finden Sie unter [Tabellenhinweise &#40;Transact-SQL&#41;](../t-sql/queries/hints-transact-sql-table.md).  
  
    -   Übergehen von Zeilen, die nicht Ziel von Updates waren  
  
         Wenn `READ UNCOMMITTED` angegeben wird und eine Abfrage Zeilen in der Speicherreservierungsreihenfolge (unter Verwendung der IAM-Seiten) liest, werden möglicherweise Zeilen übergangen, falls eine Seite durch eine andere Transaktion geteilt wird. Dieser Fall kann beim Einsatz von READ UNCOMMITTED nicht eintreten, weil während der Teilung einer Seite eine Tabellensperre aufrechterhalten wird. Es werden auch keine Zeilen übergangen, wenn die Tabelle nicht über einen gruppierten Index verfügt, da Updates keine Seitenteilungen verursachen.  
  
#### <a name="types-of-concurrency"></a>Parallelitätstypen  
 Wenn viele Benutzer gleichzeitig versuchen, Daten in einer Datenbank zu ändern, muss ein Steuerungssystem implementiert werden, durch das sichergestellt wird, dass sich die von einem Benutzer vorgenommenen Änderungen nicht auf die Änderungen eines anderen Benutzers auswirken. Dies wird als Parallelitätssteuerung bezeichnet.  
  
 In der Theorie der Parallelitätssteuerung werden die Methoden zum Implementieren der Parallelitätssteuerung in zwei Gruppen klassifiziert:  
  
-   Steuerung durch **eingeschränkte** Parallelität  
  
     Durch ein System aus Sperren werden Benutzer daran gehindert, Daten so zu verändern, dass sich dies nachteilig auf die Arbeit anderer Benutzer auswirkt. Sobald ein Benutzer eine Aktion ausführt, die zum Anwenden einer Sperre führt, können andere Benutzer so lange keine Aktionen ausführen, die mit dieser Sperre in Konflikt stehen, bis die Sperre durch den Besitzer aufgehoben wird. Diese Vorgehensweise wird als Steuerung durch eingeschränkte Parallelität bezeichnet und wird vorwiegend in Umgebungen verwendet, in denen die Wahrscheinlichkeit von Konflikten beim Zugriff auf Daten sehr hoch ist. In diesen Umgebungen verursacht das Schützen der Daten mithilfe von Sperren weniger Kosten als das Ausführen von Rollbacks für Transaktionen, wenn Parallelitätskonflikte aufgetreten sind.  
  
-   Steuerung durch **vollständige** Parallelität  
  
     Bei der Steuerung durch vollständige Parallelität werden keine Sperren für Daten eingerichtet, wenn diese von den Benutzern gelesen werden. Wenn ein Benutzer Daten aktualisiert, überprüft das System, ob ein anderer Benutzer die Daten geändert hat, nachdem sie gelesen wurden. Wenn die Daten bereits durch einen anderen Benutzer aktualisiert worden sind, wird ein Fehler ausgelöst. In der Regel führt der Benutzer, der die Fehlermeldung empfangen hat, einen Rollback für die Transaktion aus und beginnt mit der Bearbeitung von vorn. Diese Vorgehensweise wird als Steuerung durch vollständige Parallelität bezeichnet und wird vorwiegend in Umgebungen verwendet, in denen nur wenige Konflikte beim Zugriff auf Daten entstehen und die Kosten für das gelegentliche Ausführen von Rollbacks für eine Transaktion geringer sind als die Kosten für das Sperren der Daten, wenn sie gelesen werden.  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unterstützt verschiedene Typen der Parallelitätssteuerung. Benutzer geben den Typ der Parallelitätssteuerung an, indem sie Transaktionsisolationsstufen für Verbindungen oder Parallelitätsoptionen für Cursor angeben. Diese Attribute können mithilfe von [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisungen definiert werden oder über die Eigenschaften und Attribute der Schnittstellen zur Anwendungsprogrammierung (APIs, Application Programming Interfaces) der Datenbank, wie z. B. ADO, ADO.NET, OLE DB und ODBC.  
  
#### <a name="isolation-levels-in-the-ssdenoversion"></a>Isolationsstufen im [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]  
 Transaktionen geben eine Isolationsstufe an, mit der definiert wird, bis zu welchem Ausmaß eine Transaktion von Ressourcen- oder Datenänderungen isoliert sein muss, die von anderen Transaktionen durchgeführt werden. Die einzelnen Isolationsstufen werden dahingehend beschrieben, welche Parallelitätsnebeneffekte (wie z. B. Dirty Reads oder Phantomlesezugriffe) zulässig sind.  
  
 Durch die Transaktionsisolationsstufen wird Folgendes gesteuert:  
  
-   Ob beim Lesen von Daten Sperren aktiviert werden können, und welcher Sperrentyp angefordert wird.  
-   Wie lange die Lesesperren aufrechterhalten werden.  
-   Ob ein Lesevorgang, der auf Zeilen verweist, die durch eine andere Transaktion geändert wurden:  
    -   Blockiert wird, bis die exklusive Sperre für die Zeile aufgehoben wird.  
    -   Die durch einen Commit bestätigte Version der Zeile abruft, die zum Zeitpunkt des Anweisungs- oder Transaktionsstarts vorhanden war.  
    -   Die Datenänderung liest, für die noch kein Commit ausgeführt wurde.  
  
> [!IMPORTANT]  
> Das Auswählen einer Transaktionsisolationsstufe hat keine Auswirkungen auf die Sperren, die zum Schutz der Datenänderung eingerichtet werden. Eine Transaktion erhält immer eine exklusive Sperre für alle von ihr geänderten Daten und hält diese Sperre bis zum Abschluss der Transaktion aufrecht, und zwar unabhängig davon, welche Isolationsstufe für diese Transaktion festgelegt wurde. Für Lesevorgänge wird durch die Transaktionsisolationsstufen in erster Linie der Grad des Schutzes vor den Auswirkungen der Änderungen definiert, die durch andere Transaktionen vorgenommen werden.  
  
 Eine niedrigere Isolationsstufe erhöht einerseits die Möglichkeit, dass viele Benutzer gleichzeitig auf Daten zugreifen können, führt aber gleichzeitig zum Anstieg der negativen Parallelitätseffekte (Dirty Reads oder verlorene Updates), mit denen die Benutzer rechnen müssten. Und umgekehrt schränkt eine höhere Isolationsstufe zwar die Typen der Parallelitätseffekte ein, mit denen Benutzer rechnen müssen, gleichzeitig werden dafür aber mehr Systemressourcen beansprucht, und die Wahrscheinlichkeit steigt, dass sich die Transaktionen untereinander blockieren. So muss bei jeder Auswahl der geeigneten Isolationsstufe eine Abwägung zwischen den Datenintegritätsanforderungen der Anwendungen einerseits und dem mit jeder Isolationsstufe verbundenen Aufwand andererseits erfolgen. Die höchste Isolationsstufe (Serializable) garantiert, dass eine Transaktion jedes Mal, wenn sie einen Lesevorgang wiederholt, genau dieselben Daten liest. Dies wird jedoch durch ein Ausmaß an Sperren erreicht, das in Systemen mit mehreren Benutzern wahrscheinlich zu negativen Auswirkungen für andere Benutzer führt. Mit der niedrigsten Isolationsstufe (Read Uncommitted) können Daten abgerufen werden, die von anderen Transaktionen geändert wurden, für die jedoch noch kein Commit ausgeführt wurde. In der Isolationsstufe Read Uncommitted können sämtliche denkbaren Parallelitätsnebeneffekte auftreten, dagegen werden keine Lesesperren und keine Versionsverwaltung verwendet, wodurch der Aufwand minimiert wird.  
  
##### <a name="database-engine-isolation-levels"></a>Isolationsstufen der Datenbank-Engine  
 Der ISO-Standard definiert die folgenden Isolationsstufen, die alle von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] unterstützt werden:  
  
|Isolationsstufe|Definition|  
|---------------------|----------------|  
|**Read uncommitted**|Die niedrigste Isolationsstufe, bei der Transaktionen nur soweit isoliert werden, dass sichergestellt ist, dass keine physisch beschädigten Daten gelesen werden. Auf dieser Stufe sind Dirty Reads zulässig, d. h., eine Transaktion kann Änderungen verfolgen, die von anderen Transaktionen vorgenommen wurden und für die noch kein Commit ausgeführt wurde.|  
|**Read committed**|Ermöglicht einer Transaktion das Lesen von Daten, die zuvor von einer anderen Transaktion gelesen (nicht geändert) wurden, ohne warten zu müssen, bis die erste Transaktion abgeschlossen ist. Das [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] behält Schreibsperren (die für ausgewählte Daten angefordert wurden) bis zum Ende der Transaktion bei, Lesesperren werden jedoch bei Ausführung des SELECT-Vorgangs freigegeben. Dies ist die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Standardstufe.|  
|**Repeatable read**|Das [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] behält Lese- und Schreibsperren, die für ausgewählte Daten angefordert wurden, bis zum Ende der Transaktion bei. Da Bereichssperren jedoch nicht verwaltet werden, können Phantomlesevorgänge auftreten.|  
|**Serializable**|Die höchste Stufe, auf der Transaktionen vollständig voneinander isoliert sind. Das [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] behält Lese- und Schreibsperren, die für ausgewählte Daten angefordert wurden, bis zur Freigabe am Ende der Transaktion bei. Bereichssperren werden angefordert, wenn ein SELECT-Vorgang eine WHERE-Bereichsklausel verwendet. Dies dient vor allem der Vermeidung von Phantomlesevorgängen.<br /><br /> **Hinweis:** DDL-Vorgänge und -Transaktionen in replizierten Tabellen schlagen möglicherweise fehl, wenn die Isolationsstufe „Serializable“ angefordert wird. Das liegt daran, dass Replikationsabfragen Hinweise verwenden, die möglicherweise mit der serialisierbaren Isolationsstufe nicht kompatibel sind.|  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unterstützt außerdem zwei zusätzliche Transaktionsisolationsstufen, bei denen die Zeilenversionsverwaltung verwendet wird. Eine davon ist eine Implementierung der Read Committed-Isolation, die andere – Snapshot – ist eine Transaktionsisolationsstufe.  
  
|Isolationsstufe der Zeilenversionsverwaltung|Definition|  
|------------------------------------|----------------|  
|**READ COMMITTED-Momentaufnahme (RCSI)**|Wenn die READ_COMMITTED_SNAPSHOT-Datenbankoption auf ON festgelegt ist, verwendet die READ COMMITTED-Isolation die Zeilenversionsverwaltung, um eine Lesekonsistenz auf der Anweisungsebene zu gewährleisten. Lesevorgänge erfordern dabei lediglich SCH-S-Sperren auf der Tabellenebene und keine Seiten- oder Zeilensperren. Das heißt, das [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] verwendet die Zeilenversionsverwaltung, um jede Anweisung mit einer transaktionskonsistenten Momentaufnahme der Daten so darzustellen, wie sie zu Beginn der Anweisung vorhanden waren. Es werden keine Sperren verwendet, um die Daten vor Updates durch andere Transaktionen zu schützen. Eine benutzerdefinierte Funktion kann Daten zurückgeben, für die ein Commit ausgeführt wurde, nachdem die Anweisung mit dem UDF begonnen hat.<br /><br /> Wenn die Datenbankoption `READ_COMMITTED_SNAPSHOT` die Standardeinstellung OFF aufweist, verwendet die Read Committed-Isolation freigegebene Sperren, um zu verhindern, dass andere Transaktionen Zeilen ändern, während die aktuelle Transaktion einen Lesevorgang ausführt. Durch freigegebene Sperren wird außerdem verhindert, dass die Anweisung Zeilen, die von anderen Transaktionen geändert werden, erst nach Abschluss der anderen Transaktion lesen kann. Beide Implementierungen entsprechen der ISO-Definition der Read Committed-Isolation.|  
|**Momentaufnahme**|Die Momentaufnahmeisolationsstufe verwendet die Zeilenversionsverwaltung, um die Lesekonsistenz auf der Transaktionsebene zu gewährleisten. Dabei werden durch Lesevorgänge keine Seiten- oder Zeilensperren eingerichtet, sondern lediglich SCH-S-Tabellensperren. Beim Lesen von Zeilen, die durch eine andere Transaktion geändert wurden, wird die Version der Zeile abgerufen, die zum Startzeitpunkt der Transaktion vorhanden war. Sie können die Momentaufnahmeisolation für eine Datenbank nur verwenden, wenn die `ALLOW_SNAPSHOT_ISOLATION`-Datenbankoption auf ON festgelegt wurde. Standardmäßig ist diese Option für Benutzerdatenbanken auf OFF gesetzt.<br /><br /> **Hinweis:** [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] unterstützt die Versionsverwaltung von Metadaten nicht. Aus diesem Grund gibt es bezüglich der DDL-Vorgänge, die in einer unter Momentaufnahmeisolation ausgeführten expliziten Transaktion ausgeführt werden, Einschränkungen. Die folgenden DDL-Anweisungen sind bei der Momentaufnahme-Isolationsstufe nach einer BEGIN TRANSACTION-Anweisung nicht zulässig: ALTER TABLE, CREATE INDEX, CREATE XML INDEX, ALTER INDEX, DROP INDEX, DBCC REINDEX, ALTER PARTITION FUNCTION, ALTER PARTITION SCHEME oder eine beliebige CLR-DDL-Anweisung (Common Language Runtime). Diese Anweisungen sind zulässig, wenn Sie die Momentaufnahmeisolation in impliziten Transaktionen verwenden. Eine implizite Transaktion ist definitionsgemäß eine einzelne Anweisung, mit der die Semantik der Momentaufnahmeisolation auch in DDL-Anweisungen erzwungen werden kann. Verstöße gegen dieses Prinzip können zu Fehler 3961 führen: `Snapshot isolation transaction failed in database '%.*ls' because the object accessed by the statement has been modified by a DDL statement in another concurrent transaction since the start of this transaction. It is not allowed because the metadata is not versioned. A concurrent update to metadata could lead to inconsistency if mixed with snapshot isolation.`.|  
  
 Die folgende Tabelle veranschaulicht, welche Parallelitätsnebeneffekte in den einzelnen Isolationsstufen möglich sind.  
  
|Isolationsstufe|Dirty Read|Nonrepeatable Read|Phantom|  
|---------------------|----------------|------------------------|-------------|  
|**Read uncommitted**|Ja|Ja|Ja|  
|**Read committed**|Nein|Ja|Ja|  
|**Repeatable read**|Nein|Nein|Ja|  
|**Momentaufnahme**|Nein|Nein|Nein|  
|**Serializable**|Nein|Nein|Nein|  
  
 Weitere Informationen zu den speziellen Sperrentypen sowie zur Unterstützung der Zeilenversionsverwaltung durch die einzelnen Transaktionsisolationsstufen finden Sie unter [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
 Die Isolationsstufen von Transaktionen können mithilfe von [!INCLUDE[tsql](../includes/tsql-md.md)] oder durch eine Datenbank-API festgelegt werden.  
  
 **[!INCLUDE[tsql](../includes/tsql-md.md)]**                                      
 [!INCLUDE[tsql](../includes/tsql-md.md)]-Skripts verwenden die `SET TRANSACTION ISOLATION LEVEL`-Anweisung.  
  
 **ADO**  
 ADO-Anwendungen legen die `IsolationLevel`-Eigenschaft des **Connection**-Objekts auf „adXactReadUncommitted“, „adXactReadCommitted“, „adXactRepeatableRead“ oder „adXactReadSerializable“ fest.  
  
 **ADO.NET**  
 ADO.NET-Anwendungen, die den verwalteten Namespace `System.Data.SqlClient` verwenden, können die `SqlConnection.BeginTransaction`-Methode aufrufen und die Option *IsolationLevel* auf Unspecified, Chaos, ReadUncommitted, ReadCommitted, RepeatableRead, Serializable oder Momentaufnahme festlegen.  
  
 **OLE DB**  
 Beim Start einer Transaktion rufen Anwendungen, die OLE DB verwenden, `ITransactionLocal::StartTransaction` auf, wobei *isoLevel* auf ISOLATIONLEVEL_READUNCOMMITTED, ISOLATIONLEVEL_READCOMMITTED, ISOLATIONLEVEL_REPEATABLEREAD, ISOLATIONLEVEL_SNAPSHOT oder ISOLATIONLEVEL_SERIALIZABLE festgelegt ist.  
  
 Wenn die Isolationsstufe von Transaktionen im Autocommitmodus angegeben wird, können OLE DB-Anwendungen die DBPROPSET_SESSION-Eigenschaft DBPROP_SESS_AUTOCOMMITISOLEVELS auf DBPROPVAL_TI_CHAOS, DBPROPVAL_TI_READUNCOMMITTED, DBPROPVAL_TI_BROWSE, DBPROPVAL_TI_CURSORSTABILITY, DBPROPVAL_TI_READCOMMITTED, DBPROPVAL_TI_REPEATABLEREAD, DBPROPVAL_TI_SERIALIZABLE, DBPROPVAL_TI_ISOLATED oder DBPROPVAL_TI_SNAPSHOT festlegen.  
  
 **ODBC**  
 ODBC-Anwendungen rufen `SQLSetConnectAttr` auf, wobei *Attribute* auf SQL_ATTR_TXN_ISOLATION und *ValuePtr* auf SQL_TXN_READ_UNCOMMITTED, SQL_TXN_READ_COMMITTED, SQL_TXN_REPEATABLE_READ oder SQL_TXN_SERIALIZABLE festgelegt sind.  
  
 Für Momentaufnahmetransaktionen rufen Anwendungen `SQLSetConnectAttr` auf, wobei „Attribute“ auf SQL_COPT_SS_TXN_ISOLATION und „ValuePtr“ auf SQL_TXN_SS_SNAPSHOT festgelegt sind. Eine Momentaufnahmetransaktion kann mit SQL_COPT_SS_TXN_ISOLATION oder SQL_ATTR_TXN_ISOLATION abgerufen werden.  
  
##  <a name="locking-in-the-database-engine"></a><a name="Lock_Engine"></a> Sperren in der Datenbank-Engine  
 Sperren beschreiben einen Mechanismus, der von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] zum Synchronisieren des gleichzeitigen Benutzerzugriffs auf die gleichen Daten verwendet wird.  
  
 Bevor eine Transaktion eine Abhängigkeit für den aktuellen Status von Daten abruft, z. B. durch Lesen oder Ändern der Daten, muss sie sich selbst vor den Auswirkungen schützen, die sich ergeben können, wenn eine andere Transaktion die gleichen Daten ändert. Die Transaktion fordert zu diesem Zweck eine Sperre für die betreffenden Daten an. Sperren besitzen verschiedene Modi, z. B. freigegeben oder exklusiv. Der Sperrmodus definiert den Grad der Abhängigkeit, den die Transaktion für die Daten eingerichtet hat. Keiner Transaktion wird eine Sperre gewährt, die einen Konflikt mit dem Modus einer Sperre verursachen würde, die einer anderen Transaktion bereits für die betreffenden Daten erteilt wurde. Wenn eine Transaktion einen Sperrmodus anfordert, der einen Konflikt mit einer Sperre verursacht, die bereits für die gleichen Daten erteilt wurde, hält die Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] die anfordernde Transaktion an, bis die erste Sperre freigegeben wird.  
  
 Wenn eine Transaktion Daten ändert, wird die Sperre, die die Änderung schützt, aufrechterhalten, bis die Transaktion abgeschlossen ist. Die Zeitdauer der Aufrechterhaltung einer Sperre zum Schutz von Lesevorgängen durch eine Transaktion hängt von der Einstellung für die Isolationsstufe der Transaktion ab. Alle Sperren, die von einer Transaktion aufrechterhalten werden, werden freigegeben, wenn die Transaktion abgeschlossen ist (d. h. ein Commit oder ein Rollback ausgeführt wurde).  
  
 Anwendungen fordern in der Regel Sperren nicht direkt an. Sperren werden intern durch eine Komponente von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] verwaltet, die als Sperrenmanager bezeichnet wird. Wenn eine Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] eine [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisung verarbeitet, ermittelt der Abfrageprozessor von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], auf welche Ressourcen zugegriffen werden muss. Der Abfrageprozessor ermittelt, welche Arten von Sperren zum Schützen der einzelnen Ressourcen basierend auf dem Zugriffstyp und der Einstellung für den Isolationsgrad der Transaktion erforderlich sind. Der Abfrageprozessor fordert dann die entsprechenden Sperren vom Sperrenmanager an. Der Sperrenmanager erteilt die Sperren, wenn keine Sperren von anderen Transaktionen aufrechterhalten werden, die einen Konflikt verursachen.  
  
## <a name="lock-granularity-and-hierarchies"></a>Sperrengranularität und -hierarchien  
 Das [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] verwendet multigranulare Sperren, die das Sperren unterschiedlicher Ressourcentypen durch eine Transaktion ermöglichen. Um die Kosten für das Sperren zu minimieren, sperrt [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] automatisch Ressourcen auf einer für die Aufgabe geeigneten Stufe. Bei Verwendung von Sperren mit differenzierterer Granularität, z. B. Sperren für Zeilen, steigt die Parallelität, aber der Verwaltungsaufwand ist größer, da mehr Sperren aufrechterhalten werden müssen, wenn viele Zeilen gesperrt werden. Die Verwendung von Sperren mit gröberer Granularität, z. B. Sperren für Tabellen, wirkt sich nachteilig auf die Parallelität aus, da durch das Sperren einer gesamten Tabelle der Zugriff auf alle Teile der Tabelle für andere Transaktionen eingeschränkt wird. Der Verwaltungsaufwand nimmt jedoch ab, da weniger Sperren aufrechterhalten werden müssen.  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] muss häufig Sperren auf einer höheren Granularitätsebene einrichten, um eine Ressource vollständig zu schützen. Diese Gruppe von Sperren auf mehreren Granularitätsebenen wird als Sperrenhierarchie bezeichnet. Um z. B. das Lesen eines Indexes vollständig zu schützen, muss eine Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] gegebenenfalls freigegebene Sperren für Spalten und beabsichtigt-freigegebene Sperren für die Seiten und Tabellen einrichten.  
  
 Die folgende Tabelle zeigt die Ressourcen, die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] sperren kann.  
  
|Resource|BESCHREIBUNG|  
|--------------|-----------------|  
|RID|Ein Zeilenbezeichner, der verwendet wird, um eine einzelne Zeile in einem Heap zu sperren.|  
|KEY|Eine Zeilensperre in einem Index, die verwendet wird, um Schlüsselbereiche in serialisierbaren Transaktionen zu schützen.|  
|PAGE|Eine 8-KB-Seite in einer Datenbank, z. B. Daten- oder Indexseiten.|  
|EXTENT|Eine zusammenhängende Gruppe von acht Seiten, z. B. Datenseiten oder Indexseiten.|  
|HoBT|Ein Heap oder eine B-Struktur. Eine Sperre, die eine B-Struktur (Index) oder den Heap von Datenseiten in einer Tabelle schützt, die keinen gruppierten Index besitzt.|  
|TABLE|Die vollständige Tabelle mit sämtlichen Daten und Indizes.|  
|FILE|Eine Datenbankdatei.|  
|APPLICATION|Eine von der Anwendung angegebene Ressource.|  
|METADATA|Metadatensperren.|  
|ALLOCATION_UNIT|Eine Zuordnungseinheit.|  
|DATABASE|Die gesamte Datenbank.|  
  
> [!NOTE]  
> HoBT- und TABLE-Sperren können durch die LOCK_ESCALATION-Option von [ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md) beeinflusst werden.  
  
## <a name="lock-modes"></a><a name="lock_modes"></a> Sperrmodi  
 Das [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] sperrt Ressourcen mithilfe unterschiedlicher Sperrmodi, die bestimmen, wie gleichzeitige Transaktionen auf Ressourcen zugreifen können.  
  
 Die folgende Tabelle zeigt die Ressourcen-Sperrmodi, die das [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] verwendet.  
  
|Sperrmodus|BESCHREIBUNG|  
|---------------|-----------------|  
|**Freigegebene Sperre (Shared, S)**|Wird für Lesevorgänge verwendet, die Daten nicht ändern oder aktualisieren, wie z. B. SELECT-Anweisungen.|  
|**Updatesperre (U)**|Wird für Ressourcen verwendet, die aktualisiert werden können. Verhindert eine gängige Form des Deadlocks, die auftritt, wenn mehrere Sitzungen Ressourcen lesen, sperren und anschließend möglicherweise aktualisieren.|  
|**Exklusive Sperre (X)**|Wird bei Datenänderungen wie INSERT-, UPDATE- oder DELETE-Vorgängen verwendet. Stellt sicher, dass nicht mehrere Updates an derselben Ressource gleichzeitig vorgenommen werden können.|  
|**Absicht**|Wird verwendet, um eine Sperrhierarchie zu erstellen. Es gibt folgende Typen von beabsichtigten Sperren: beabsichtigte freigegebene Sperre (Intent Shared, IS), beabsichtigte exklusive Sperre (Intent Exclusive, IX) und freigegebene Sperre mit beabsichtigter exklusiver Sperre (Shared With Intent Exclusive, SIX).|  
|**Schema**|Wird beim Ausführen von Vorgängen verwendet, die vom Schema einer Tabelle abhängen. Es gibt folgende Typen von Schemasperren: Schemaänderungssperre (Sch-M) und Schemastabilitätssperre (Sch-S).|  
|**Massenaktualisierung (Bulk Update, BU)**|Wird beim Massenkopieren von Daten in eine Tabelle verwendet, wenn der **TABLOCK**-Hinweis angegeben ist.|  
|**Schlüsselbereich**|Schützt den von einer Abfrage gelesenen Zeilenbereich, wenn die serialisierbare Transaktionsisolationsstufe verwendet wird. Stellt sicher, dass keine anderen Transaktionen Zeilen einfügen können, die von den Abfragen der serialisierbaren Transaktion berücksichtigt werden können, falls diese erneut ausgeführt würden.|  
  
### <a name="shared-locks"></a><a name="shared"></a> Gemeinsame Sperren  
 Freigegebene Sperren (S) ermöglichen, dass Transaktionen eine Ressource gleichzeitig lesen können (SELECT), wenn die Steuerung durch eingeschränkte Parallelität aktiviert ist. Andere Transaktionen können die Daten nicht ändern, während freigegebene Sperren (S) für die Ressource eingerichtet sind. Freigegebene Sperren (S) einer Ressource werden aufgehoben, sobald der Lesevorgang abgeschlossen ist, es sei denn, die Isolationsstufe der Transaktion wird auf REPEATABLE READ oder höher festgelegt oder ein Sperrhinweis wird verwendet, um freigegebene Sperren (S) für die Dauer der Transaktion beizubehalten.  
  
### <a name="update-locks"></a><a name="update"></a> Aktualisierungssperren  
 Updatesperren (Update, U) verhindern eine häufige Form von Deadlocks. Bei REPEATABLE READ- oder SERIALIZABLE-Transaktionen liest die Transaktion Daten, wozu sie eine freigegebene Sperre (S) für die Ressource (Seite oder Zeile) einrichtet, und ändert anschließend die Daten, was eine Konvertierung der Sperre in eine exklusive Sperre (X) erfordert. Wenn zwei Transaktionen eine freigegebene Sperre für eine Ressource einrichten und anschließend versuchen, Daten gleichzeitig zu aktualisieren, versucht die erste Transaktion, die Sperre zu einer exklusiven Sperre (X) zu konvertieren. Diese Konvertierung muss aufgeschoben werden, da der Modus der exklusiven Sperre der einen Transaktion nicht kompatibel mit dem Modus der freigegebenen Sperre der anderen Transaktion ist. Es ergibt sich ein Sperrenwartevorgang. Die zweite Transaktion versucht nun ebenfalls, eine exklusive Sperre (X) für das Update einzurichten. Da beide Transaktionen das Konvertieren in eine exklusive Sperre (X) versuchen und darauf warten, dass die andere Transaktion die freigegebene Sperre aufhebt, kommt es zu einem Deadlock.  
  
 Um dieses potenzielle Deadlockproblem zu vermeiden, werden Updatesperren (U) verwendet. Es kann jeweils nur eine Transaktion eine Updatesperre (U) für eine Ressource einrichten. Wenn eine Transaktion eine Ressource ändert, wird die Updatesperre (U) in eine exklusive Sperre (X) konvertiert.  
  
### <a name="exclusive-locks"></a><a name="exclusive"></a> Exklusive Sperren  
 Exklusive Sperren (X) verhindern, dass Transaktionen gleichzeitig auf eine Ressource zugreifen. Eine exklusive Sperre (X) bewirkt, dass keine andere Transaktion Daten ändern kann. Lesevorgänge können nur mithilfe des NOLOCK-Hinweises oder der READ UNCOMMITTED-Isolationsstufe ausgeführt werden.  
  
 Datenänderungsanweisungen wie INSERT, UPDATE und DELETE setzen sowohl Änderungs- als auch Lesevorgänge voraus. Die Anweisung führt zunächst Lesevorgänge aus, um die Daten einzulesen, und anschließend die erforderlichen Änderungsvorgänge. Daher machen Datenänderungsanweisungen normalerweise sowohl freigegebene als auch exklusive Sperren erforderlich. Eine UPDATE-Anweisung kann beispielsweise Zeilen einer Tabelle ändern, die auf einem Join mit einer anderen Tabelle basieren. In diesem Fall fordert die UPDATE-Anweisung freigegebene Sperren für die Zeilen in der verknüpften Tabelle an, sowie exklusive Sperren für die zu aktualisierenden Zeilen.  
  
### <a name="intent-locks"></a><a name="intent"></a> Beabsichtigte Sperren  
 Das [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] verwendet beabsichtigte Sperren, um das Platzieren einer freigegebenen (S) oder exklusiven Sperre (X) auf eine Ressource zu schützen, die sich weiter unten in der Sperrhierarchie befinden. Die Bezeichnung 'beabsichtige Sperre' bedeutet, dass beabsichtigte Sperren vor Sperren auf untergeordneten Ebenen eingerichtet werden, und damit die Absicht ausdrücken, Sperren auf untergeordneten Ebenen zu platzieren.  
  
 Beabsichtigte Sperren werden aus zwei Gründen verwendet:  
  
-   Um zu verhindern, dass andere Transaktionen Ressourcen übergeordneter Ebenen ändern und damit die Sperren untergeordneter Ebenen ungültig werden. 
-   Um die Effizienz des [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] beim Erkennen von Sperrkonflikten auf einer höheren Granularitätsebene zu steigern.  
  
 Eine beabsichtigte freigegebene Sperre auf Tabellenebene wird also beispielsweise angefordert, bevor freigegebene Sperren (S) für Seiten oder Zeilen in dieser Tabelle angefordert werden. Durch Festlegen einer beabsichtigten Sperre auf Tabellenebene wird verhindert, dass andere Transaktionen anschließend eine exklusive Sperre (X) für die Tabelle einrichten können, die diese Seite enthält. Beabsichtigte Sperren tragen zur Leistungsverbesserung bei, da das [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] beabsichtige Sperren nur auf Tabellenebene überprüft, um zu bestimmen, ob eine Transaktion für diese Tabelle problemlos eine Sperre einrichten kann. Dadurch ist es nicht mehr erforderlich, jede Zeilen- oder Seitensperre in der Tabelle zu überprüfen, um zu ermitteln, ob eine Transaktion die gesamte Tabelle sperren kann.  
  
<a name="lock_intent_table"></a> Beabsichtigte Sperren umfassen beabsichtigte freigegebene (Intent Shared, IS), beabsichtigte exklusive (Intent Exclusive, IX) und freigegebene mit beabsichtigten exklusiven (Shared With Intent Exclusive, SIX) Sperren.  
  
|Sperrmodus|BESCHREIBUNG|  
|---------------|-----------------|  
|**Beabsichtigte freigegebene Sperre (Intent Shared, IS)**|Schützt angeforderte oder eingerichtete freigegebene Sperren bestimmter (aber nicht aller) Ressourcen untergeordneter Ebenen in der Hierarchie.|  
|**Beabsichtigte exklusive Sperre (Intent Exclusive, IX)**|Schützt angeforderte oder eingerichtete exklusive Sperren bestimmter (aber nicht aller) Ressourcen untergeordneter Ebenen in der Hierarchie. IX ist eine Obermenge der beabsichtigten freigegebenen Sperre und schützt auch vor Anforderung freigegebener Sperren auf Ressourcen untergeordneter Ebenen in der Hierarchie.|  
|**Freigegebene Sperre mit beabsichtigter exklusiver Sperre (Shared With Intent Exclusive, SIX)**|Schützt angeforderte oder eingerichtete freigegebene Sperren aller Ressourcen untergeordneter Ebenen in der Hierarchie sowie beabsichtigte exklusive Sperren bestimmter (aber nicht aller) Ressourcen untergeordneter Ebenen in der Hierarchie. Gleichzeitige beabsichtigte freigegebene Sperren auf der Ressource der obersten Ebene sind zugelassen. So werden beispielsweise bei einer Sperre des Typs SIX für eine Tabelle auch beabsichtigte exklusive Sperren für die zu ändernden Seiten sowie exklusive Sperren für die zu ändernden Zeilen eingerichtet. Es kann jeweils nur eine Sperre des Typs SIX pro Ressource eingerichtet werden, durch die Updates an der Ressource durch andere Transaktionen verhindert werden. Dennoch können andere Transaktionen Ressourcen, die sich weiter unten in der Hierarchie befinden, lesen, indem sie beabsichtigte freigegebene Sperren auf Tabellenebene einrichten.|  
|**Beabsichtigte Aktualisierungssperre (IU)**|Schützt angeforderte oder eingerichtete Updatesperren aller Ressourcen untergeordneter Hierarchieebenen. IU-Sperren werden nur mit Seitenressourcen verwendet. IU-Sperren werden zu IX-Sperren konvertiert, wenn ein Updatevorgang ausgeführt wird.|  
|**Gemeinsame Sperre mit beabsichtigter Aktualisierungssperre (SIU)**|Eine Kombination der Sperren vom Typ S und IU, die sich aus der separaten Einrichtung dieser Sperren und dem gleichzeitigen Beibehalten beider Sperren ergibt. Nehmen Sie beispielsweise an, eine Transaktion führt eine Abfrage mit dem PAGLOCK-Hinweis und anschließend einen Updatevorgang aus. Die Abfrage mit dem PAGLOCK-Hinweis richtet also die S-Sperre ein, wohingegen der Updatevorgang die IU-Sperre einrichtet.|  
|**Aktualisierungssperre mit beabsichtigter exklusiver Sperre (UIX)**|Eine Kombination der Sperren vom Typ U und IX, die sich aus dem separaten Einrichten dieser Sperren und dem gleichzeitigen Beibehalten beider Sperren ergibt.|  
  
### <a name="schema-locks"></a><a name="schema"></a> Schemasperren  
 Sperren des Typs Sch-M (Schema Modification, Schemaänderung) werden von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] verwendet, wenn für eine Tabelle ein DDL-Vorgang (Data Definition Language, Datendefinitionssprache) ausgeführt wird, wie etwa das Hinzufügen einer Spalte oder Löschen einer Tabelle. Während die Sch-M-Sperre besteht, werden gleichzeitige Zugriffe auf die Tabelle verhindert. Dies bedeutet, dass die Sch-M-Sperre alle externen Vorgänge blockiert, bis die Sperre aufgehoben wird.  
  
 Einige DML-Vorgänge (Data Manipulation Language), z. B. das Abschneiden von Tabellen, verhindern mithilfe von Sch-M-Sperren, dass gleichzeitige Vorgänge auf die betroffenen Tabellen zugreifen.  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] verwendet Sperren des Typs Sch-S (Schemastabilität) beim Kompilieren und Ausführen von Abfragen. Sch-S-Sperren blockieren keine Transaktionssperren, auch keine exklusive Sperren (X). Daher können während der Kompilierung einer Abfrage andere Transaktionen, einschließlich Transaktionen mit exklusiven Sperren (X) auf Tabellenebene, weiterhin ausgeführt werden. Gleichzeitige DDL-Vorgänge und gleichzeitige DML-Vorgänge, die Sch-M-Sperren abrufen, können für die Tabelle jedoch nicht ausgeführt werden.  
  
### <a name="bulk-update-locks"></a><a name="bulk_update"></a> Massenaktualisierungssperren  
 Massenupdatesperren (BU) werden verwendet, damit mehrere Threads gleichzeitig Daten in dieselbe Tabelle laden können, während sie zugleich anderen Prozessen, die keine Daten massenkopieren, keinen Zugriff auf die Tabelle gewähren. Das [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] verwendet Massenupdatesperren (Bulk Update, BU), wenn die folgenden Bedingungen zutreffen.  
  
-   Zum Massenkopieren von Daten in eine Tabelle verwenden Sie die [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisung BULK INSERT oder die OPENROWSET(BULK)-Funktion. Sie können auch einen der Masseneinfügungs-API-Befehle wie „.NET SqlBulkCopy“, OLEDB-FastLoad-APIs oder die ODBC-APIs für das Massenkopieren verwenden.  
-   Es wird entweder der **TABLOCK**-Hinweis angegeben oder die Tabellenoption **table lock on bulk load** mithilfe von **sp_tableoption** festgelegt.  
  
> [!TIP]  
> Im Gegensatz zur BULK INSERT-Anweisung, die eine weniger restriktive Massenupdatesperre enthält, weist INSERT INTO…SELECT mit dem TABLOCK-Hinweis eine exklusive Sperre (X) für die Tabelle auf. Das bedeutet, dass Sie keine Zeilen mit parallelen Einfügevorgängen einfügen können.  
  
### <a name="key-range-locks"></a><a name="key_range"></a> Schlüsselbereichssperren  
 Schlüsselbereichssperren schützen einen Bereich von Zeilen, die implizit in ein Recordset eingeschlossen wurden, das von einer [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisung gelesen wird; dies geschieht bei Verwendung der Transaktionsisolationsstufe SERIALIZABLE. Durch Schlüsselbereichssperren werden Phantomlesezugriffe verhindert. Indem die Schlüsselbereiche zwischen Zeilen geschützt werden, wird auch verhindert, dass beim Zugreifen von Transaktionen auf Recordsets Phantomeinfügungen oder -löschungen erfolgen.  
  
## <a name="lock-compatibility"></a><a name="lock_compatibility"></a> Kompatibilität von Sperren  
 Durch die Kompatibilität von Sperren wird gesteuert, ob mehrere Transaktionen gleichzeitig Sperren für dieselbe Ressource einrichten können. Wenn eine Ressource bereits durch eine andere Transaktion gesperrt wurde, kann eine erneute Sperranforderung nur gewährt werden, wenn der Modus der angeforderten Sperre mit dem Modus der vorhandenen Sperre kompatibel ist. Wenn der Modus der angeforderten Sperre nicht mit dem Modus der vorhandenen Sperre kompatibel ist, wartet die Transaktion, von der die neue Sperre angefordert wird, bis die vorhandene Sperre aufgehoben wird oder bis das Timeoutintervall der Sperre abgelaufen ist. So sind z. B. keine anderen Sperrmodi mit exklusiven Sperren kompatibel. Wenn eine exklusive Sperre (X) eingerichtet ist, kann eine andere Transaktion eine Sperre jeglicher Art (freigegeben, Update oder exklusiv) für die Ressource erst dann einrichten, wenn die exklusive Sperre (X) am Ende der ersten Transaktion aufgehoben wird. Falls hingegen eine freigegebene Sperre (Shared, S) auf eine Ressource angewendet wurde, können andere Transaktionen ebenfalls eine freigegebene Sperre oder eine Updatesperre (Update, U) auf dieses Element anwenden, selbst wenn die erste Transaktion noch nicht beendet ist. Andere Transaktionen können jedoch eine exklusive Sperre erst dann einrichten, wenn die freigegebene Sperre aufgehoben wurde.  
  
<a name="lock_compat_table"></a> Die folgende Tabelle stellt die Kompatibilität der am häufigsten auftretenden Sperrmodi dar.  
  
||Vorhandener erteilter Modus||||||  
|------|---------------------------|------|------|------|------|------|  
|**Angeforderter Modus**|**IS**|**S**|**U**|**IX**|**SIX**|**X**|  
|**Beabsichtigte freigegebene Sperre (Intent Shared, IS)**|Ja|Ja|Ja|Ja|Ja|Nein|  
|**Freigegebene Sperre (Shared, S)**|Ja|Ja|Ja|Nein|Nein|Nein|  
|**Updatesperre (U)**|Ja|Ja|Nein|Nein|Nein|Nein|  
|**Beabsichtigte exklusive Sperre (Intent Exclusive, IX)**|Ja|Nein|Nein|Ja|Nein|Nein|  
|**Freigegebene Sperre mit beabsichtigter exklusiver Sperre (Shared With Intent Exclusive, SIX)**|Ja|Nein|Nein|Nein|Nein|Nein|  
|**Exklusive Sperre (X)**|Nein|Nein|Nein|Nein|Nein|Nein|  
  
> [!NOTE]  
> Eine beabsichtigte exklusive Sperre (IX) ist mit einem Sperrmodus des Typs IX kompatibel, da IX nur die Absicht zum Aktualisieren einiger statt aller Zeilen anzeigt. Andere Transaktionen, die versuchen, einige der Zeilen zu lesen oder zu aktualisieren, werden ebenfalls zugelassen, sofern es sich nicht um dieselben Zeilen handelt, die von anderen Transaktionen aktualisiert werden. Wenn zwei Transaktionen versuchen, dieselbe Zeile zu aktualisieren, wird beiden Transaktionen eine IX-Sperre auf Tabellen- und Seitenebene erteilt. Bei nur einer Transaktion wird jedoch eine X-Sperre auf Zeilenebene erteilt. Die andere Transaktion muss warten, bis die Sperre auf Zeilenebene aufgehoben wird.  
  
<a name="lock_matrix"></a> Verwenden Sie die folgende Tabelle, um die Kompatibilität aller in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] verfügbaren Sperrmodi zu ermitteln.  
  
 ![lock_conflicts](../relational-databases/media/LockConflictTable.png)  
  
## <a name="key-range-locking"></a>Schlüsselbereichssperren  
 Schlüsselbereichssperren schützen einen Bereich von Zeilen, die implizit in ein Recordset eingeschlossen wurden, das von einer [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisung gelesen wird; dies geschieht bei Verwendung der Transaktionsisolationsstufe SERIALIZABLE. Für die Isolationsstufe SERIALIZABLE muss jede Abfrage, die während einer Transaktion ausgeführt wird, dieselben Zeilen erhalten, wenn sie im Rahmen der Transaktion ausgeführt wird. Durch eine Schlüsselbereichssperre wird diese Anforderung geschützt, indem verhindert wird, dass von anderen Transaktionen neue Zeilen eingefügt werden, deren Schlüssel dem Schlüsselbereich zugehörig sind, die von der serialisierbaren Transaktion gelesen werden.  
  
 Durch Schlüsselbereichssperren werden Phantomlesezugriffe verhindert. Indem die Schlüsselbereiche zwischen Zeilen geschützt werden, wird außerdem verhindert, dass es zu Phantomeinfügungsvorgängen in Datensätzen kommt, auf die eine Transaktion zugreift.  
  
 Eine Schlüsselbereichssperre wird für einen Index platziert; auf diese Weise wird ein Start- und Endschlüsselwert angegeben. Durch diese Sperre wird jeglicher Versuch blockiert, eine Zeile mit einem Schlüsselwert einzufügen, zu aktualisieren oder zu löschen, der dem Bereich zugehörig ist, da von diesen Vorgängen zunächst eine Sperre für den Index eingerichtet werden müsste. Eine serialisierbare Transaktion könnte beispielsweise eine `SELECT`-Anweisung ausgeben, die alle Zeilen liest, deren Schlüsselwerte mit der Bedingung `BETWEEN 'AAA' AND 'CZZ'` übereinstimmen. Eine Schlüsselbereichssperre für die Schlüsselwerte im Bereich von **'** AAA **'** bis **'** CZZ **'** verhindert, dass andere Transaktionen Zeilen mit Schlüsselwerten in diesem Bereich einfügen, beispielsweise **'** ADG **'** , **'** BBD **'** oder **'** CAL **'** .  
  
### <a name="key-range-lock-modes"></a><a name="key_range_modes"></a> Sperrmodi für Schlüsselbereiche  
 Zu Schlüsselbereichssperren gehören eine Bereichs- und eine Zeilenkomponente, die im Bereichszeilenformat angegeben werden.  
  
-   Bereich stellt den Sperrmodus dar, der den Bereich zwischen zwei aufeinander folgenden Indexeinträgen schützt.  
-   Zeile stellt den Sperrmodus dar, der den Indexeintrag schützt.  
-   Modus stellt den kombinierten Sperrmodus dar, der verwendet wird. Schlüsselbereichssperrmodi setzen sich aus zwei Teilen zusammen. Der erste gibt den Sperrtyp wieder, der zum Sperren des Indexbereichs (Range *T*) verwendet wird, und der zweite gibt den Sperrtyp wieder, der zum Sperren eines bestimmten Schlüssels (*K*) verwendet wird. Die beiden Teile sind durch einen Bindestrich (-) miteinander verbunden, beispielsweise Range *T*-*K*.  
  
    |Range|Zeile|Mode|BESCHREIBUNG|  
    |-----------|---------|----------|-----------------|  
    |RangeS|E|RangeS-S|Freigegebene Bereichssperre, freigegebene Ressourcensperre; serialisierbarer Bereichsscan.|  
    |RangeS|U|RangeS-U|Freigegebene Sperre für Bereich und Updatesperre für Ressource; serialisierbarer Updatescan.|  
    |RangeI|Null|RangeI-N|Einfügungssperre für Bereich und NULL-Sperre für Ressource; wird verwendet, um Bereiche vor dem Einfügen eines neuen Schlüssels in einen Index zu testen.|  
    |RangeX|X|RangeX-X|Exklusive Sperren für Bereich und Ressource; wird beim Aktualisieren eines Schlüssels in einem Bereich verwendet.|  
  
> [!NOTE]  
> Der interne NULL-Sperrmodus ist mit allen anderen Sperrmodi kompatibel.  
  
 Schlüsselbereichssperrmodi haben eine Kompatibilitätsmatrix, die zeigt, welche Sperren mit anderen Sperren, die für überlappende Schlüssel und Bereiche eingerichtet wurden, kompatibel sind.  
  
||Vorhandener erteilter Modus|||||||  
|------|---------------------------|------|------|------|------|------|------|  
|**Angeforderter Modus**|**S**|**U**|**X**|**RangeS-S**|**RangeS-U**|**RangeI-N**|**RangeX-X**|  
|**Freigegebene Sperre (Shared, S)**|Ja|Ja|Nein|Ja|Ja|Ja|Nein|  
|**Updatesperre (U)**|Ja|Nein|Nein|Ja|Nein|Ja|Nein|  
|**Exklusive Sperre (X)**|Nein|Nein|Nein|Nein|Nein|Ja|Nein|  
|**RangeS-S**|Ja|Ja|Nein|Ja|Ja|Nein|Nein|  
|**RangeS-U**|Ja|Nein|Nein|Ja|Nein|Nein|Nein|  
|**RangeI-N**|Ja|Ja|Ja|Nein|Nein|Ja|Nein|  
|**RangeX-X**|Nein|Nein|Nein|Nein|Nein|Nein|Nein|  
  
### <a name="conversion-locks"></a><a name="lock_conversion"></a> Konvertierungssperren  
 Konvertierungssperren werden erstellt, wenn eine Schlüsselbereichssperre eine andere Sperre überlappt.  
  
|Sperre 1|Sperre 2|Konvertierungssperre|  
|------------|------------|---------------------|  
|E|RangeI-N|RangeI-S|  
|U|RangeI-N|RangeI-U|  
|X|RangeI-N|RangeI-X|  
|RangeI-N|RangeS-S|RangeX-S|  
|RangeI-N|RangeS-U|RangeX-U|  
  
 Konvertierungssperren lassen sich für eine kurze Zeitdauer unter verschiedenen komplexen Bedingungen beobachten, so gelegentlich bei der Ausführung gleichzeitiger Prozesse.  
  
### <a name="serializable-range-scan-singleton-fetch-delete-and-insert"></a>Serialisierbarer Bereichsscan, Singleton-Abruf, Löschen und Einfügen  
 Durch Schlüsselbereichssperren wird sichergestellt, dass folgende Vorgänge serialisierbar sind:  
  
-   Bereichsscanabfrage  
-   Singleton-Abruf einer nicht vorhandenen Zeile  
-   Löschvorgang  
-   Einfügungsvorgang  
  
 Folgende Bedingungen müssen erfüllt werden, ehe Schlüsselbereichssperren verwendet werden können:  
  
-   Die Isolationsstufe der Transaktion muss auf SERIALIZABLE festgelegt sein.  
-   Der Abfrageprozessor muss zum Implementieren des Bereichsfilterprädikäts verwendet werden. Beispiel: Die WHERE-Klausel in einer SELECT-Anweisung könnte eine Bereichsbedingung mit diesem Prädikat erstellen: ColumnX BETWEEN N **'** AAA **'** AND N **'** CZZ **'** . Eine Schlüsselbereichssperre kann nur eingerichtet werden, wenn **ColumnX** durch einen Indexschlüssel abgedeckt ist.  
  
### <a name="examples"></a>Beispiele  
 Die nachfolgende Tabelle und der nachfolgende Index dienen als Grundlage für die Beispiele für Schlüsselbereichssperren, die nachfolgend aufgeführt sind.  
  
 ![B-Struktur](../relational-databases/media/btree4.png)  
  
#### <a name="range-scan-query"></a>Bereichsscanabfrage  
 Um sicherzustellen, dass eine Bereichsscanabfrage serialisierbar ist, sollte dieselbe Abfrage immer dieselben Ergebnisse zurückgeben, wenn sie innerhalb derselben Transaktion ausgeführt wird. Neue Zeilen dürfen innerhalb der Bereichsscanabfrage nicht von anderen Transaktionen eingefügt werden, da diese sonst zu Phantomeinfügungen werden. In der nachfolgenden Abfrage werden beispielsweise die Tabelle und der Index in der obigen Abbildung verwendet:  
  
```sql  
SELECT name  
FROM mytable  
WHERE name BETWEEN 'A' AND 'C';  
```  
  
 Es werden Schlüsselbereichssperren auf die Indexeinträge angewendet, die dem Datenzeilenbereich entsprechen, in dem der Name zwischen den Werten `Adam` und `Dale` liegt. Dadurch wird verhindert, dass neue Zeilen, die der vorhergehenden Abfrage entsprechen, hinzugefügt oder gelöscht werden. Obwohl `Adam` der erste Name in diesem Bereich ist, wird durch die Schlüsselbereichssperre mit dem Modus RangeS-S für diesen Indexeintrag sichergestellt, dass keine neuen Namen mit dem Anfangsbuchstaben A vor dem Namen `Adam` eingefügt werden können, beispielsweise `Abigail`. Entsprechend wird durch die Schlüsselbereichssperre mit dem Modus RangeS-S für den Indexeintrag für `Dale` sichergestellt, dass keine neuen Namen mit dem Anfangsbuchstaben C nach dem Namen `Carlos` eingefügt werden können, beispielsweise `Clive`.  
  
> [!NOTE]  
> Die Anzahl der aufrechterhaltenen Sperren vom Typ „RangeS-S“ entspricht *n*+1. Hierbei ist *n* die Anzahl der Zeilen, die der Abfrage entsprechen.  
  
#### <a name="singleton-fetch-of-nonexistent-data"></a>Singleton-Abruf nicht vorhandener Daten  
 Wenn eine Abfrage in einer Transaktion versucht, eine nicht vorhandene Zeile auszuwählen, muss die Abfrage, wenn sie zu einem späteren Zeitpunkt innerhalb derselben Transaktion erneut ausgegeben wird, zu demselben Ergebnis führen. Es darf für keine andere Transaktion zulässig sein, diese nicht vorhandene Zeile einzufügen. Angenommen, die folgende Abfrage wird ausgeführt:  
  
```sql  
SELECT name  
FROM mytable  
WHERE name = 'Bill';  
```  
  
 Es wird eine Schlüsselbereichssperre für den Indexeintrag platziert, der dem Namensbereich von `Ben` bis `Bing` entspricht, da der Name `Bill` zwischen den beiden aufeinander folgenden Indexeinträgen eingefügt würde. Die Schlüsselbereichssperre mit dem Modus RangeS-S wird für den Indexeintrag `Bing` platziert. Dadurch wird verhindert, dass andere Transaktionen Werte, wie etwa `Bill`, zwischen die Indexeinträge `Ben` und `Bing` einfügen.  
  
#### <a name="delete-operation"></a>Löschvorgang  
 Wenn ein Wert in einer Transaktion gelöscht wird, muss der Bereich, in dem der Wert liegt, nicht für die gesamte Dauer der Transaktion, die den Löschvorgang ausführt, gesperrt werden. Die Serialisierbarkeit wird bereits dann aufrechterhalten, wenn der gelöschte Schlüsselwert bis zum Ende der Transaktion gesperrt wird. Angenommen, folgende DELETE-Anweisung wird ausgeführt:  
  
```sql  
DELETE mytable  
WHERE name = 'Bob';  
```  
  
 Eine exklusive Sperre (X) wird für den Indexeintrag platziert, der dem Namen `Bob` entspricht. Andere Transaktionen können Werte vor oder nach dem gelöschten Wert `Bob` einfügen oder löschen. Eine Transaktion, die versucht, den Wert `Bob` zu lesen, einzufügen oder zu löschen, wird jedoch so lange blockiert, bis für die löschende Transaktion entweder ein Commit oder ein Rollback ausgeführt wird.  
  
 Das Löschen des Bereichs kann mithilfe von drei grundlegenden Sperrmodi ausgeführt werden: Zeilen-, Seiten- oder Tabellensperre. Die Verwendung der Zeilen-, Seiten- oder Tabellensperren wird vom Abfrageoptimierer festgelegt oder kann vom Benutzer über Abfrageoptimierungshinweise, wie ROWLOCK, PAGLOCK oder TABLOCK, angegeben werden. Wenn PAGLOCK oder TABLOCK verwendet wird, hebt [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] umgehend die Zuordnung einer Indexseite auf, wenn sämtliche Zeilen dieser Seite gelöscht werden. Wenn hingegen ROWLOCK verwendet wird, werden sämtliche Zeilen lediglich als gelöscht markiert und zu einem späteren Zeitpunkt mithilfe eines Hintergrundtasks von der Indexseite entfernt.  
  
#### <a name="insert-operation"></a>Einfügungsvorgang  
 Wenn ein Wert in einer Transaktion eingefügt wird, muss der Bereich, in dem der Wert liegt, nicht für die gesamte Dauer der Transaktion, die den Einfügungsvorgang ausführt, gesperrt werden. Die Serialisierbarkeit wird bereits dann aufrechterhalten, wenn der eingefügte Schlüsselwert bis zum Ende der Transaktion gesperrt wird. Angenommen, folgende INSERT-Anweisung wird ausgeführt:  
  
```sql  
INSERT mytable VALUES ('Dan');  
```  
  
 Für den Indexeintrag, der dem Namen David entspricht, wird die Schlüsselbereichssperre mit dem Modus RangeI-N platziert, um den Bereich zu testen. Wenn die Sperre erteilt wird, wird `Dan` eingefügt, und für den Wert `Dan` wird eine exklusive Sperre (X) platziert. Die Schlüsselbereichssperre mit dem Modus RangeI-N ist nur notwendig, um den Bereich zu testen, und wird nicht für die Dauer der Transaktion aufrechterhalten, die den Einfügungsvorgang ausführt. Andere Transaktionen können Werte vor oder nach dem eingefügten Wert `Dan` einfügen oder löschen. Eine Transaktion, die versucht, den Wert `Dan` zu lesen, einzufügen oder zu löschen, wird jedoch so lange gesperrt, bis für die einfügende Transaktion entweder ein Commit oder ein Rollback ausgeführt wird.  
  
## <a name="lock-escalation"></a>Sperrenausweitung
Die Sperrenausweitung ist der Vorgang, bei dem viele differenzierte Sperren zu einer kleineren Anzahl von groben Sperren konvertiert werden, wodurch zwar der Aufwand des Systems reduziert, aber gleichzeitig die Wahrscheinlichkeit von Parallelitätskonflikten erhöht wird.

Wenn [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Sperren auf niedriger Ebene eingerichtet werden, werden auch beabsichtigte Sperren für die Objekte eingerichtet, die diese Objekte der niedrigen Ebene enthalten:

-   Beim Sperren von Zeilen oder Indexschlüsselbereichen richtet [!INCLUDE[ssde_md](../includes/ssde_md.md)] eine beabsichtigte Sperre für die Seiten ein, die diese Zeilen oder Schlüssel enthalten.
-   Beim Sperren von Seiten richtet [!INCLUDE[ssde_md](../includes/ssde_md.md)] eine beabsichtigte Sperre für die Objekte der höheren Ebene ein, die diese Seiten enthalten. Zusätzlich zur beabsichtigten Sperre für das Objekt werden beabsichtigte Sperren für die folgenden Objekte angefordert:
    -  Seiten auf Blattebene von nicht gruppierten Indizes
    -  Datenseiten von gruppierten Indizes
    -  Heap mit Datenseiten

[!INCLUDE[ssde_md](../includes/ssde_md.md)] kann im Rahmen derselben Anweisung sowohl Zeilen- als auch Seitensperren bewirken, um die Anzahl der Sperren zu minimieren und um die Wahrscheinlichkeit zu verringern, dass eine Sperrenausweitung erforderlich wird. So könnte die Datenbank-Engine z. B. Seitensperren für einen nicht gruppierten Index (sofern ausreichend viele zusammenhängende Schlüssel im Indexknoten ausgewählt sind, um der Abfrage zu entsprechen) und Zeilensperren für die Daten einrichten.

Zum Ausweiten von Sperren versucht [!INCLUDE[ssde_md](../includes/ssde_md.md)], die beabsichtigte Sperre für die Tabelle in eine entsprechende vollständige Sperre zu ändern, wodurch eine beabsichtigte exklusive Sperre (IX) zu einer exklusiven Sperre (X) bzw. eine beabsichtigte freigegebene Sperre (IS) zu einer freigegebenen Sperre (S) wird. Wenn der Versuch der Sperrenausweitung erfolgreich ist und die vollständige Tabellensperre eingerichtet wird, werden alle durch die Transaktion für den Heap bzw. den Index gehaltenen Sperren des Typs Heap oder B-Baum, Seite (PAGE) oder Zeilenebene (RID) aufgehoben. Wenn die vollständige Sperre nicht erreicht wird, erfolgt keine Sperrenausweitung, und die Datenbank-Engine richtet weiterhin Zeilen-, Schlüssel- oder Seitensperren ein.

[!INCLUDE[ssde_md](../includes/ssde_md.md)] weitet keine Zeilen- oder Schlüsselbereichssperren zu Seitensperren aus, sondern weitet diese direkt zu Tabellensperren aus. In gleicher Weise werden Seitensperren immer zu Tabellensperren ausgeweitet. Das Sperren von partitionierten Tabellen kann auf die HoBT-Ebene für die zugehörige Partition statt auf die Tabellensperre ausgeweitet werden. Eine Sperre auf HoBT-Ebene führt nicht notwendigerweise dazu, dass die ausgerichteten HoBTs für die Partition gesperrt werden.

> [!NOTE]
> Sperren auf HoBT-Ebene führen in der Regel zur Erhöhung der Parallelität, erhöhen jedoch auch die Möglichkeit von Deadlocks, wenn Transaktionen, die verschiedene Partitionen sperren, jeweils ihre exklusiven Sperren auf andere Partitionen ausweiten möchten. In seltenen Fällen wird mit der TABLE-Sperrgranularität eine bessere Leistung erzielt.

Wenn beim Versuch zur Sperrenausweitung ein Fehler erzeugt wird, weil von gleichzeitigen Transaktionen miteinander im Konflikt stehende Sperren gehalten werden, versucht [!INCLUDE[ssde_md](../includes/ssde_md.md)] die Sperrenausweitung erneut für jeweils weitere 1.250 Sperren, die von der Transaktion eingerichtet werden.

Jedes Ausweitungsereignis wird primär auf der Ebene einer einzelnen [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisung ausgeführt. Wenn das Ereignis startet, versucht [!INCLUDE[ssde_md](../includes/ssde_md.md)] die Ausweitung aller Sperren, die von der aktuellen Transaktion in einer der Tabellen gehalten werden, auf die durch die aktive Anweisung verwiesen wird, vorausgesetzt, dass diese die Schwellenwertanforderungen für die Ausweitung erfüllt. Wenn das Ausweitungsereignis startet, bevor die Anweisung auf eine Tabelle zugegriffen hat, wird nicht versucht, die Sperren für diese Tabelle auszuweiten. Wenn die Sperrenausweitung erfolgreich ist, werden alle Sperren, die durch die Transaktion in einer früheren Anweisung eingerichtet wurden und zum Zeitpunkt des Ereignisstarts noch immer gehalten werden, ausgeweitet, wenn durch die aktuelle Anweisung auf die Tabelle verwiesen wird und die Tabelle in das Ausweitungsereignis eingeschlossen ist.

Angenommen, eine Sitzung führt z. B. die folgenden Operationen durch:

-  beginnt eine Transaktion,
-  Aktualisiert `TableA`. Damit werden exklusive Zeilensperren in TableA eingerichtet, die so lange gehalten werden, bis die Transaktion abgeschlossen ist.
-  Aktualisiert `TableB`. Damit werden exklusive Zeilensperren in TableB eingerichtet, die so lange gehalten werden, bis die Transaktion abgeschlossen ist.
-  Führt eine SELECT-Anweisung aus, die `TableA` mit `TableC` verknüpft. Der Abfrageausführungsplan ruft die aus `TableA` abzurufenden Zeilen auf, bevor die Zeilen aus `TableC` abgerufen werden.
-  Die SELECT-Anweisung löst die Sperrenausweitung aus, während sie die Zeilen aus `TableA` abruft und bevor sie auf `TableC` zugegriffen hat.

Bei erfolgreicher Sperrenausweitung werden nur die von der Sitzung für `TableA` gehaltenen Sperren ausgeweitet. Das schließt sowohl die freigegebenen Sperren aus der SELECT-Anweisung als auch die exklusiven Sperren aus der vorherigen UPDATE-Anweisung ein. Während bei der Beurteilung, ob die Sperrenausweitung erfolgen soll, nur die Sperren berücksichtigt werden, die die Sitzung in `TableA` für die SELECT-Anweisung eingerichtet hat, werden bei erfolgreicher Ausweitung alle von der Sitzung in `TableA` gehaltenen Sperren zu einer exklusiven Sperre für die Tabelle ausgeweitet, und alle anderen Sperren mit geringerer Granularität, einschließlich beabsichtigter Sperren, für `TableA` werden aufgehoben.

Es wird nicht versucht, die Sperren für `TableB` auszuweiten, weil es in der SELECT-Anweisung keinen aktiven Verweis auf `TableB` gibt. Desgleichen wird nicht versucht, die Sperren für `TableC` auszuweiten, weil zum Zeitpunkt der Ausweitung noch kein Zugriff auf die Tabelle erfolgt war.

### <a name="lock-escalation-thresholds"></a>Schwellenwerte für die Sperrenausweitung

Die Sperrenausweitung wird, wenn sie für die Tabelle nicht deaktiviert ist, mit der `ALTER TABLE SET LOCK_ESCALATION`-Option ausgelöst, und wenn eine der folgenden Bedingungen zutrifft:

-  Eine einzelne [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisung ruft mindestens 5.000 Sperren für eine einzelne nicht partitionierte Tabelle oder einen Index ab.
-  Eine einzelne [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisung ruft mindestens 5.000 Sperren für eine einzelne Partition einer partitionierten Tabelle ab, und die `ALTER TABLE SET LOCK_ESCALATION`-Option ist auf AUTO festgelegt.
-  Die Anzahl von Sperren in einer Instanz von [!INCLUDE[ssde_md](../includes/ssde_md.md)] überschreitet den Arbeitsspeicher oder die Konfigurationsschwellenwerte.

Wenn die Sperrenausweitung aufgrund von Sperrkonflikten nicht möglich ist, löst [!INCLUDE[ssde_md](../includes/ssde_md.md)] die Sperrenausweitung in regelmäßigen Abständen aus, sobald jeweils 1.250 neue Sperren eingerichtet werden.

### <a name="escalation-threshold-for-a-transact-sql-statement"></a>Ausweitungsschwellenwert für eine Transact-SQL-Anweisung
Wenn [!INCLUDE[ssde_md](../includes/ssde_md.md)] alle 1.250 neu abgerufenen Sperren auf mögliche Ausweitungen prüft, erfolgt eine Sperrenausweitung nur dann, wenn eine [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisung mindestens 5.000 Sperren für einen einzelnen Verweis einer Tabelle abgerufen hat. Die Sperrenausweitung wird ausgelöst, wenn eine [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisung mindestens 5.000 Sperren für einen einzelnen Verweis einer Tabelle abruft. So wird z. B. keine Sperrenausweitung ausgelöst, wenn eine Anweisung 3.000 Sperren in einem Index und 3.000 Sperren in einem anderen Index derselben Tabelle einrichtet. So wird auch dann keine Sperrenausweitung ausgelöst, wenn eine Anweisung einen Selbstjoin zu einer Tabelle enthält und jeder Verweis auf die Tabelle lediglich 3.000 Sperren in der Tabelle einrichtet.

Die Sperrenausweitung tritt nur für Tabellen auf, auf die zum Zeitpunkt der Ausweitungsauslösung bereits zugegriffen wurde. Angenommen, eine einzelne SELECT-Anweisung ist ein Join, der auf drei Tabellen in genau dieser Reihenfolge zugreift: `TableA`, `TableB` und `TableC`. Die Anweisung richtet 3.000 Zeilensperren im gruppierten Index für `TableA` ein und mindestens 5.000 Zeilensperren im gruppierten Index für `TableB`. Auf `TableC` wurde jedoch noch nicht zugegriffen. Wenn [!INCLUDE[ssde_md](../includes/ssde_md.md)] erkennt, dass die Anweisung mindestens 5.000 Zeilensperren in `TableB` eingerichtet hat, wird versucht, sämtliche von der aktuellen Transaktion in `TableB` gehaltenen Sperren auszuweiten. Es wird auch versucht, sämtliche von der aktuellen Transaktion in `TableA` gehaltenen Sperren auszuweiten, da aber die Anzahl der Sperren für `TableA` kleiner als 5.000 ist, ist die Ausweitung nicht erfolgreich. Es wird keine Sperrenausweitung für `TableC` versucht, da zum Zeitpunkt der Ausweitung noch kein Zugriff auf die Tabelle erfolgt war.

### <a name="escalation-threshold-for-an-instance-of-the-database-engine"></a>Ausweitungsschwellenwert für eine Instanz der Datenbank-Engine
Immer wenn die Anzahl der Sperren den Speicherschwellenwert für die Sperrenausweitung überschreitet, löst [!INCLUDE[ssde_md](../includes/ssde_md.md)] die Sperrenausweitung aus. Der Speicherschwellenwert richtet sich nach der Einstellung der [Konfigurationsoption „locks“](../database-engine/configure-windows/configure-the-locks-server-configuration-option.md):

-   Wenn die **locks**-Option auf die Standardeinstellung 0 festgelegt ist, wird der Schwellenwert der Sperrenausweitung erreicht, wenn der von Sperrobjekten belegte Speicheranteil 24 % des von der Datenbank-Engine verwendeten Speichers (ausschließlich AWE-Speicher) beträgt. Die Datenstruktur, mit der eine Sperre dargestellt wird, ist ungefähr 100 Bytes lang. Dieser Schwellenwert ist dynamisch, da die Datenbank-Engine je nach wechselnder Arbeitsauslastung dynamisch Speicher reserviert und freigibt.

-   Wenn die **locks**-Option einen von 0 abweichenden Wert hat, beträgt der Schwellenwert für die Sperrenauswertung 40 % des Werts der locks-Option (oder weniger, wenn nicht genügend Arbeitsspeicher verfügbar ist).

[!INCLUDE[ssde_md](../includes/ssde_md.md)] kann jede aktive Anweisung aus jeder Sitzung zur Ausweitung auswählen, und für jeweils 1.250 neue Sperren wählt es Anweisungen zur Ausweitung aus, so lange der in der Instanz für Sperren beanspruchte Arbeitsspeicher oberhalb des Schwellenwerts bleibt.

### <a name="escalating-mixed-lock-types"></a>Ausweiten von gemischten Sperrentypen
Wenn die Sperrenausweitung stattfindet, ist die für den Heap oder Index ausgewählte Sperre stark genug, um die Anforderungen der am restriktivsten Sperre auf unterer Ebene zu erfüllen.

Angenommen, eine Sitzung

-  beginnt eine Transaktion,
-  aktualisiert eine Tabelle, die einen gruppierten Index enthält,
-  gibt eine SELECT-Anweisung aus, die auf dieselbe Tabelle verweist.

Die UPDATE-Anweisung richtet die folgenden Sperren ein:

-  Exklusive Sperren (X) für die aktualisierten Datenzeilen.
-  Beabsichtigte exklusive Sperren (IX) für die gruppierten Indexseiten, die diese Zeilen enthalten.
-  Eine IX-Sperre für den gruppierten Index und eine weitere für die Tabelle.

Die SELECT-Anweisung richtet die folgenden Sperren ein:

-  Freigegebene Sperren (S) für alle gelesenen Datenzeilen, außer wenn die Zeile bereits durch eine X-Sperre aus der UPDATE-Anweisung geschützt wurde.
-  Beabsichtigte freigegebene Sperren (IS) für alle gruppierten Indexseiten, die diese Zeilen enthalten, außer wenn die Seite bereits durch eine IX-Sperre geschützt wurde.
-  Keine Sperre für den gruppierten Index oder die Tabelle, da diese bereits durch IX-Sperren geschützt sind.

Wenn die SELECT-Anweisung ausreichend Sperren eingerichtet hat, um die Sperrenausweitung auszulösen, und die Ausweitung erfolgreich ist, wird die IX-Sperre für die Tabelle in eine X-Sperre umgewandelt, und sämtliche Zeilen-, Seiten- und Indexsperren werden aufgehoben. Sowohl die Updates als auch Lesevorgänge sind durch die X-Sperre für die Tabelle geschützt.

### <a name="reducing-locking-and-escalation"></a>Verringern von Sperrung und Ausweitung
In den meisten Fällen erzielt [!INCLUDE[ssde_md](../includes/ssde_md.md)] die beste Leistung, wenn es mit seinen Standardeinstellungen zur Sperrung und zur Sperrenausweitung arbeitet. Wenn eine Instanz von [!INCLUDE[ssde_md](../includes/ssde_md.md)] jedoch viele Sperren generiert und häufige Sperrenausweitungen durchführt, sollten Sie durch folgende Maßnahmen versuchen, das Ausmaß der Sperrung zu verringern:

-   Verwenden einer Isolationsstufe, die keine freigegebenen Sperren für Lesevorgänge erzeugt:
    -  READ COMMITTED-Isolationsstufe, wenn die READ_COMMITTED_SNAPSHOT-Datenbankoption auf ON gesetzt ist.
    -  MOMENTAUFNAHME-Isolationsstufe.
    -  READ UNCOMMITTED-Isolationsstufe. Diese kann nur für Systeme verwendet werden, die mit Dirty Reads arbeiten können.    
  
    > [!NOTE]
    > Die Änderung der Isolationsstufe wirkt sich auf alle Tabellen der Instanz von [!INCLUDE[ssde_md](../includes/ssde_md.md)] aus.

-   Verwenden der Tabellenhinweise PAGLOCK oder TABLOCK, damit die Datenbank-Engine Seiten-, Heap- oder Indexsperren anstelle von Zeilensperren verwendet. Die Verwendung dieser Option führt jedoch verstärkt zu Problemen, weil Benutzer den Zugriff anderer Benutzer auf dieselben Daten blockieren, und darf daher nicht in Systemen mit mehr als einigen wenigen gleichzeitigen Benutzern verwendet werden.

-   Verwenden Sie für partitionierte Tabellen die LOCK_ESCALATION-Option von [ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md), um Sperren auf die HoBT-Ebene auszuweiten statt auf die Tabelle, oder um die Sperrenausweitung zu deaktivieren.

-   Teilen Sie eine große Anzahl von Vorgängen in mehrere kleinere Vorgänge auf. Angenommen, Sie führen die folgende Abfrage aus, um mehrere hunderttausend alte Datensätze aus einer Überwachungstabelle zu entfernen, und stellen dann fest, dass sie eine Sperrenausweitung verursacht, die andere Benutzer blockiert:
   
    ```sql
    DELETE FROM LogMessages WHERE LogDate < '2/1/2002'
    ```

    Wenn Sie einige Hundert dieser Datensätze auf einmal entfernen, können Sie die Anzahl der Sperren, die pro Transaktion anfallen, drastisch reduzieren und eine Sperrenausweitung verhindern. Beispiel:

    ```sql
    SET ROWCOUNT 500
    delete_more:
      DELETE FROM LogMessages WHERE LogDate < '2/1/2002'
    IF @@ROWCOUNT > 0 GOTO delete_more
    SET ROWCOUNT 0
    ```

-   Verringern Sie den Umfang von Abfragesperren, indem Sie die Abfrage so effizient wie möglich gestalten. Große Scans oder eine große Anzahl von Lesezeichenlookups erhöhen möglicherweise die Wahrscheinlichkeit einer Sperrenausweitung. Außerdem erhöht sich dadurch die Wahrscheinlichkeit von Deadlocks, und in der Regel gibt es negative Auswirkungen auf Parallelität und Leistung. Nachdem Sie die Abfrage ermittelt haben, die die Sperrenausweitung verursacht, suchen Sie nach Möglichkeiten, neue Indizes zu erstellen oder Spalten zu einem vorhandenen Index hinzuzufügen, um Index- oder Tabellenscans zu entfernen und die Effizienz der Indexsuchvorgänge zu maximieren. Verwenden Sie ggf. den [Datenbankoptimierungsratgeber](../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md), um eine automatische Indexanalyse für die Abfrage auszuführen. Weitere Informationen finden Sie im [Tutorial: Datenbankoptimierungsratgeber](../tools/dta/tutorial-database-engine-tuning-advisor.md).
    Ein Ziel dieser Optimierung besteht darin, dass Indexsuchvorgänge so wenige Zeilen wie möglich zurückgeben, um die Kosten von Lesezeichenlookups zu minimieren (Maximieren der Selektivität des Indexes für eine bestimmte Abfrage). Wenn [!INCLUDE[ssde_md](../includes/ssde_md.md)] schätzt, dass ein logischer Operator für den Lesezeichen-Lookupvorgang möglicherweise viele Zeilen zurückgibt, kann ein PREFETCH-Vorgang verwendet werden, um den Lesezeichen-Nachschlagevorgang auszuführen. Wenn [!INCLUDE[ssde_md](../includes/ssde_md.md)] PREFETCH für einen Lesezeichen-Lookupvorgang verwendet, muss Sie die Transaktionsisolationsstufe eines Teils der Abfrage für einen Teil der Abfrage auf wiederholbare Lesevorgänge erhöhen. Das bedeutet, dass das, was auf einer Isolationsebene mit Lesecommit ähnlich wie eine SELECT-Anweisung aussehen kann, viele Tausende von Schlüsselsperren (sowohl für den gruppierten Index als auch für einen nicht gruppierten Index) abrufen kann, was möglicherweise dazu führt, dass eine solche Abfrage die Sperrenausweitungs-Schwellenwerte überschreitet. Dies ist insbesondere dann wichtig, wenn Sie feststellen, dass es sich bei der ausgeweiteten Sperre um eine freigegebene Tabellensperre handelt, die jedoch bei der standardmäßigen Isolationsebene mit Lesecommit nicht häufig auftritt. Wenn ein Lesezeichen-Nachschlagevorgang MIT PREFETCH-Klausel die Ausweitung verursacht, sollten Sie in Erwägung ziehen, zusätzliche Spalten zum nicht gruppierten Index hinzuzufügen, der im logischen Operator „Index Seek“ oder „Index Scan“ unter dem logischen Operator „Bookmark Lookup“ im Abfrageplan enthalten ist. Unter Umständen ist es möglich, einen abdeckenden Index zu erstellen (einen Index, der alle Spalten in einer Tabelle enthält, die in der Abfrage verwendet wurden) oder zumindest einen Index, der die Spalten abdeckt, die für Verknüpfungskriterien oder in der WHERE-Klausel verwendet wurden, wenn es nicht praktikabel ist, alles in die Liste zum Auswählen von Spalten aufzunehmen.
    Bei einem Nested Loop-Join kann ebenfalls PREFETCH verwendet werden. Dies führt zu demselben Sperrverhalten.
   
-   Die Sperrenausweitung kann nicht auftreten, wenn eine andere SPID derzeit eine nicht kompatible Tabellensperre aufrecht erhält. Die Sperrenausweitung wird immer zu einer Tabellensperre ausgeweitet und niemals zu Seitensperren. Wenn der Versuch einer Sperrenausweitung fehlschlägt, weil eine andere SPID eine nicht kompatible TAB-Sperre enthält, wird außerdem die Abfrage, die die Ausweitung versucht hat, beim Warten auf eine TAB-Sperre nicht blockiert. Stattdessen werden Sperren weiterhin auf der ursprünglichen, präziseren Ebene (Zeile, Schlüssel oder Seite) abgerufen, und es erfolgen zusätzliche Ausweitungsversuche in regelmäßigen Abständen. Daher besteht eine Methode zum Verhindern der Sperrenausweitung für eine bestimmte Tabelle im Abrufen und Halten einer Sperre für eine andere Verbindung, die mit dem ausgweiteten Sperrentyp nicht kompatibel ist. Eine IX-Sperre (Intent Exclusive) auf Tabellenebene sperrt keine Zeilen oder Seiten, ist aber immer noch nicht mit einer ausgeweiteten S- (freigegeben) oder X-TAB-Sperre (exklusiv) kompatibel. Nehmen Sie beispielsweise an, dass Sie einen Batchauftrag ausführen müssen, der eine große Anzahl von Zeilen in der mytable-Tabelle ändert und eine Blockierung verursacht hat, die aufgrund von Sperrenausweitung auftritt. Wenn dieser Auftrag immer in weniger als einer Stunde abgeschlossen ist, können Sie einen [!INCLUDE[tsql](../includes/tsql-md.md)]-Auftrag erstellen, der den folgenden Code enthält, und den neuen Auftrag so planen, dass er mehrere Minuten vor der Startzeit des Batchauftrags gestartet wird:
  
    ```sql
    BEGIN TRAN
    SELECT * FROM mytable (UPDLOCK, HOLDLOCK) WHERE 1=0
    WAITFOR DELAY '1:00:00'
    COMMIT TRAN
    ```
   
    Mit dieser Abfrage wird eine IX-Sperre für mytable für eine Stunde abgerufen und aufrecht erhalten, wodurch die Sperrenausweitung für die Tabelle während dieser Zeit verhindert wird. Mit diesem Batch werden keine Daten geändert oder andere Abfragen blockiert (es sei denn, die andere Abfrage erzwingt eine Tabellensperre mit dem TABLOCK-Hinweis, oder ein Administrator hat Seiten- oder Zeilensperren mithilfe einer gespeicherten sp_indexoption-Prozedur deaktiviert).

Sie können auch die Ablaufverfolgungsflags 1211 und 1224 verwenden, um alle oder einige Sperrenausweitungen zu deaktivieren. Diese [Ablaufverfolgungsflags](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) deaktivieren jedoch die gesamte Sperrenausweitung für die gesamte [!INCLUDE[ssde_md](../includes/ssde_md.md)] global. Sperrenausweitung ist in [!INCLUDE[ssde_md](../includes/ssde_md.md)] sehr nützlich, indem die Effizienz von Abfragen maximiert wird, die andernfalls durch den Mehraufwand für das Abrufen und Freigeben mehrerer Tausend Sperren verlangsamt würden. Sperrenausweitung trägt auch dazu bei, den erforderlichen Arbeitsspeicher zu minimieren, um Sperren nachzuverfolgen. Der Arbeitsspeicher, den [!INCLUDE[ssde_md](../includes/ssde_md.md)] dynamisch für Sperrstrukturen zuordnen kann, ist begrenzt. Wenn Sie also Sperrenausweitung deaktivieren und der Sperrenarbeitsspeicher groß genug wird, können Versuche fehlschlagen, zusätzliche Sperren für eine beliebige Abfrage zuzuweisen, und der folgende Fehler tritt auf:

```Error: 1204, Severity: 19, State: 1
The SQL Server cannot obtain a LOCK resource at this time. Rerun your statement when there are fewer active users or ask the system administrator to check the SQL Server lock and memory configuration.
```

> [!NOTE]
> Wenn [Fehler 1204](../relational-databases/errors-events/mssqlserver-1204-database-engine-error.md) auftritt, wird die Verarbeitung der aktuellen Anweisung beendet und ein Rollback der aktiven Transaktion ausgelöst. Wenn Sie den Datenbankdienst neu starten, können Benutzer durch das Rollback selbst blockiert werden, oder es tritt möglicherweise eine lange Datenbankwiederherstellungszeit auf.

> [!NOTE]
> Wenn Sie einen Sperrhinweis wie ROWLOCK verwenden, wird nur der anfängliche Sperrplan geändert. Sperrhinweise verhindern keine Sperrenausweitung. 

Überwachen Sie außerdem die Sperrenausweitung mithilfe des erweiterten Ereignisses `lock_escalation` (XEvent). Das folgende Beispiel zeigt dies:

```sql
-- Session creates a histogram of the number of lock escalations per database 
CREATE EVENT SESSION [Track_lock_escalation] ON SERVER 
ADD EVENT sqlserver.lock_escalation(SET collect_database_name=(1),collect_statement=(1)
    ACTION(sqlserver.database_id,sqlserver.database_name,sqlserver.query_hash_signed,sqlserver.query_plan_hash_signed,sqlserver.sql_text,sqlserver.username))
ADD TARGET package0.histogram(SET source=N'sqlserver.database_id')
GO
```

> [!IMPORTANT]
> Das erweiterte `lock_escalation`-Ereignis (xEvent) sollte anstelle der Ereignisklasse Lock:Escalation in SQL Trace oder SQL Profiler verwendet werden.

## <a name="dynamic-locking"></a><a name="dynamic_locks"></a> Dynamische Sperre
 Wenn Sie Sperren auf niedriger Ebene verwenden, z. B. Zeilensperren, wird die Parallelität erhöht, da die Wahrscheinlichkeit geringer ist, dass zwei Transaktionen gleichzeitig Sperren für die gleichen Daten anfordern. Das Verwenden von Sperren auf niedriger Ebene erhöht außerdem die Anzahl der Sperren sowie der Ressourcen, die für deren Verwaltung erforderlich sind. Wenn Sie Tabellen- oder Seitensperren auf hoher Ebene verwenden, wird der Aufwand zwar gesenkt, jedoch auf Kosten der Parallelität.  
  
 ![Sperrkosten vs. Parallelitätskosten](../relational-databases/media/lockcht.png) 
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] legt Sperren dynamisch fest, um die kosteneffektivsten Sperren zu bestimmen. [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] bestimmt beim Ausführen einer Abfrage automatisch, welche Sperren, basierend auf den Merkmalen des Schemas und der Abfrage, am sinnvollsten sind. Um beispielsweise den Aufwand für die Sperren zu senken, kann der Abfrageoptimierer festlegen, dass beim Ausführen eines Indexscans Sperren auf Seitenebene für einen Index eingerichtet werden.  
  
 Dynamische Sperren bieten die folgenden Vorteile:  
  
-   Vereinfachte Datenbankverwaltung. Datenbankadministratoren müssen die Sperreneskalationsschwellen nicht anpassen.  
-   Gesteigerte Leistung. [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] minimiert den Aufwand des Systems mithilfe von Sperren, die speziell auf die Aufgabe zugeschnitten sind.  
-   Anwendungsentwickler können sich auf die Entwicklung konzentrieren. [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] passt Sperren automatisch an.  
  
 In [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] und höheren Versionen hat sich das Verhalten der Sperrenausweitung mit der Einführung der `LOCK_ESCALATION`-Option geändert. Weitere Informationen finden Sie unter der `LOCK_ESCALATION`-Option von [ALTER TABLE](../t-sql/statements/alter-table-transact-sql.md). 
   
## <a name="deadlocks"></a><a name="deadlocks"></a> Deadlocks  
 Ein Deadlock tritt auf, wenn zwei Tasks einander dauerhaft gegenseitig blockieren, weil jeder der Tasks eine Sperre für eine Ressource aufrecht erhält, die die anderen Tasks zu sperren versuchen. Beispiel:  
  
-   Die Transaktion A richtet eine freigegebene Sperre für Zeile 1 ein.  
-   Die Transaktion B richtet eine freigegebene Sperre für Zeile 2 ein.  
-   Transaktion A fordert nun eine exklusive Sperre für Zeile 2 an und ist blockiert, bis Transaktion B beendet ist und die freigegebene Sperre für Zeile 2 aufhebt.  
-   Transaktion B fordert nun eine exklusive Sperre für Zeile 1 an und ist blockiert, bis die Transaktion A beendet ist und die freigegebene Sperre für Zeile 1 aufhebt.  
  
 Transaktion A kann erst nach Ende von Transaktion B ausgeführt werden, aber Transaktion B ist durch Transaktion A blockiert. Diese Bedingung wird auch als zyklische Abhängigkeit bezeichnet. Transaktion A hängt von Transaktion B ab, und Transaktion B schließt den Kreis, da sie von Transaktion A abhängt.  
  
 Die beiden Transaktionen, die sich im Deadlock befinden, werden auf unbegrenzte Zeit aufeinander warten, es sei denn, der Deadlock wird von einem externen Prozess unterbrochen. Der [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Deadlockmonitor überprüft regelmäßig, ob sich Tasks in einem Deadlock befinden. Wenn der Monitor eine solche zyklische Abhängigkeit erkennt, wählt er einen der Tasks als Opfer aus und beendet dessen Transaktion mit einem Fehler. Dies ermöglicht dem anderen Task, seine Transaktion abzuschließen. Die Anwendung mit der Transaktion, die mit einem Fehler beendet wurde, kann nun erneut versuchen, die Transaktion auszuführen. Dies gelingt nun normalerweise, nachdem die andere, an dem Deadlock beteiligte Transaktion bereits abgeschlossen ist.  
  
 Deadlocks werden oft mit normalen Blockierungen verwechselt. Wenn eine Transaktion eine Sperre für eine Ressource anfordert, die bereits von einer anderen Transaktion gesperrt ist, wartet die anfordernde Transaktion, bis die Sperre aufgehoben wird. Standardmäßig treten bei [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Transaktionen keine Timeouts auf, es sei denn, LOCK_TIMEOUT wurde festgelegt. Die anfordernde Transaktion ist also blockiert, befindet sich aber nicht in einem Deadlock, da sie ihrerseits die andere Transaktion, die im Besitz der Sperre ist, nicht blockiert. Die Transaktion, die die Sperre besitzt, wird zu gegebener Zeit abgeschlossen und die Sperre aufgehoben, woraufhin die anfordernde Transaktion die Sperre erhält und den Transaktionsvorgang ausführt.  
  
> [!NOTE]
> Deadlocks werden manchmal auch "deadly embrace" (tödliche Umarmung) genannt.  
  
 Ein Deadlock ist eine Bedingung, die in jedem System mit mehreren Threads auftreten kann, nicht nur bei Managementsystemen für relationale Datenbanken, sowie in anderen Ressourcen als Sperren für Datenbankobjekte. Ein Thread in einem Multithread-Betriebssystem kann beispielsweise eine Ressource oder mehrere Ressourcen, wie z. B. Speicherblöcke, reservieren. Wenn sich die zu reservierende Ressource derzeit im Besitz eines anderen Threads befindet, muss der erste Thread eventuell warten, bis der Besitzerthread die Zielressource freigegeben hat. Der wartende Thread ist für diese bestimmte Ressource abhängig vom Besitzerthread. In einer Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] können Sitzungen beim Reservieren von anderen als Datenbankressourcen, wie z. B. Speicher oder Threads, in eine Deadlocksituation geraten.  
  
 ![Diagramm mit Transaktionsdeadlock](../relational-databases/media/deadlock.png)  
  
 In der Abbildung weist Transaktion T1 eine Abhängigkeit von Transaktion T2 für die Sperrressource der `Part`-Tabelle auf. Entsprechend weist Transaktion T2 eine Abhängigkeit von Transaktion T1 für die Sperrressource der `Supplier`-Tabelle auf. Da diese Abhängigkeiten einen Kreis bilden, besteht ein Deadlock zwischen den Transaktionen T1 und T2.  
  
 Deadlocks können auch auftreten, wenn eine Tabelle partitioniert wird und für die `LOCK_ESCALATION`-Einstellung von `ALTER TABLE` die Option AUTO festgelegt ist. Ist für `LOCK_ESCALATION` die Option AUTO festgelegt, nimmt die Parallelität durch Unterstützung der Sperre von Tabellenpartitionen auf HoBT-Ebene anstatt auf TABLE-Ebene durch [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] zu. Wenn jedoch separate Transaktionen Partitionssperren in eine Tabelle aufnehmen und in der anderen Partitionstransaktion eine Sperre hinzugefügt werden soll, wird hiermit ein Deadlock verursacht. Diese Art Deadlock kann durch Festlegen von `TABLE` für `LOCK_ESCALATION` vermieden werden, auch wenn mit dieser Einstellung die Parallelität verringert wird, indem große Updates gezwungen werden, auf eine Tabellensperre zu warten.  
  
### <a name="detecting-and-ending-deadlocks"></a>Erkennen und Beenden von Deadlocks  
 Ein Deadlock tritt auf, wenn zwei Tasks einander dauerhaft gegenseitig blockieren, weil jeder der Tasks eine Sperre für eine Ressource aufrecht erhält, die die anderen Tasks zu sperren versuchen. Die folgende Abbildung zeigt den Deadlockstatus auf hoher Ebene, wobei Folgendes gilt.  
  
-   Task T1 erhält eine Sperre für Ressource R1 aufrecht (wird durch den Pfeil von R1 zu T1 angezeigt) und hat eine Sperre für Ressource R2 angefordert (wird durch den Pfeil von T1 zu R2 angezeigt).  
-   Task T2 erhält eine Sperre für Ressource R2 aufrecht (wird durch den Pfeil von R2 zu T1 angezeigt) und hat eine Sperre für Ressource R1 angefordert (wird durch den Pfeil von T2 zu R1 angezeigt).  
-   Da keiner der Tasks fortgesetzt werden kann, bevor eine Ressource verfügbar ist, und keine der Ressourcen freigegeben werden kann, bevor ein Task fortgesetzt wird, ist ein Deadlock vorhanden.  
  
 ![Diagramm mit Tasks im Deadlockstatus](../relational-databases/media/Task_Deadlock_State.png)  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] erkennt Deadlockzyklen in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] automatisch. [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] wählt eine der Sitzungen als Deadlockopfer aus, und die aktuelle Transaktion wird mit einem Fehler beendet, um den Deadlock zu durchbrechen.  
  
#### <a name="resources-that-can-deadlock"></a><a name="deadlock_resources"></a> Ressourcen, die an einem Deadlock beteiligt sein können  
 Für jede Benutzersitzung werden möglicherweise ein oder mehrere Tasks ausgeführt, von denen jeder Task eine Vielzahl von Ressourcen abruft oder auf den Abruf wartet. Die folgenden Typen von Ressourcen können eine Blockierung bewirken, die zu einem Deadlock führt.  
  
-   **Sperren:** Das Warten auf den Abruf von Sperren für Ressourcen, z. B. Objekte, Seiten, Zeilen, Metadaten und Anwendungen, kann einen Deadlock verursachen. Transaktion T1 besitzt z. B. eine freigegebene (S) Sperre für Zeile r1 und wartet darauf, eine exklusive (X) Sperre für r2 zu erhalten. Transaktion T2 besitzt eine freigegebene (S) Sperre für Zeile r2 und wartet darauf, eine exklusive (X) Sperre für Zeile r1 zu erhalten. Dies führt zu einem Sperrenzyklus, in dem T1 und T2 darauf warten, dass die jeweils andere Transaktion die gesperrten Ressourcen freigibt.  
  
-   **Arbeitsthreads:** Ein Task in der Warteschlange, der auf einen verfügbaren Arbeitsthread wartet, kann einen Deadlock verursachen. Wenn der Task in der Warteschlange Ressourcen besitzt, die alle Arbeitsthreads blockieren, führt dies zu einem Deadlock. Sitzung S1 startet z. B. eine Transaktion, ruft eine freigegebene (S) Sperre für Zeile r1 ab und wird dann in den Ruhezustand versetzt. Aktive Sitzungen, die für alle verfügbaren Arbeitsthreads ausgeführt werden, versuchen, exklusive (X) Sperren für Zeile r1 abzurufen. Da Sitzung S1 keinen Arbeitsthread abrufen kann, kann kein Commit für die Transaktion ausgeführt und die Sperre für Zeile r1 nicht freigegeben werden. Das Ergebnis ist ein Deadlock.  
  
-   **Speicher** Wenn gleichzeitige Anforderungen auf Arbeitsspeicherzuweisungen warten, die mit dem verfügbaren Arbeitsspeicher nicht befriedigt werden können, kann ein Deadlock auftreten. Zwei gleichzeitige Abfragen, Q1 und Q2, werden z. B. als benutzerdefinierte Funktionen ausgeführt, die 10 MB bzw. 20 MB Arbeitsspeicher abrufen. Wenn jede der Abfragen 30 MB benötigt und der gesamte verfügbare Arbeitsspeicher 20 MB beträgt, müssen Q1 und Q2 warten, bis die jeweils andere Transaktion Arbeitsspeicher freigibt; dies führt zu einem Deadlock.  
  
-   **Ressourcen in Verbindung mit einer parallelen Abfrageausführung:** Coordinator-, Producer- oder Consumer-Threads, die mit einem Austauschport verknüpft sind, können einander blockieren und einen Deadlock verursachen, wenn mindestens ein weiterer Prozess eingeschlossen ist, der nicht Teil der parallelen Abfrage ist. Wenn also eine parallele Abfrageausführung gestartet wird, bestimmt [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] den Grad des Parallelismus oder die Anzahl der Arbeitsthreads auf Basis der aktuellen Arbeitsauslastung. Ein Deadlock kann auftreten, wenn sich die Arbeitsauslastung des Systems unerwartet ändert. Das ist beispielsweise der Fall, wenn neue Abfragen auf dem Server gestartet werden oder im System nicht mehr genügend Arbeitsthreads vorhanden sind.  
  
-   **MARS-Ressourcen (Multiple Active Result Sets):** Diese Ressourcen werden zum Steuern des Interleavings mehrerer aktiver Anforderungen unter MARS verwendet. Weitere Informationen finden Sie unter [Verwenden von Multiple Active Result Sets (MARS)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).  
  
    -   **Benutzerressource:** Wenn ein Thread auf eine Ressource wartet, die potenziell von einer Benutzeranwendung gesteuert wird, wird die Ressource als externe oder Benutzerressource betrachtet und wie eine Sperre behandelt.  
  
    -   **Sitzungsmutex:** Die Tasks, die in einer Sitzung ausgeführt werden, sind verzahnt. Dies bedeutet, dass nur jeweils ein Task unter der Sitzung zu einem bestimmten Zeitpunkt ausgeführt werden kann. Bevor der Task ausgeführt werden kann, muss er exklusiven Zugriff auf den Sitzungsmutex besitzen.  
  
    -   **Transaktionsmutex:** Alle Tasks, die in einer Transaktion ausgeführt werden, sind verzahnt. Dies bedeutet, dass nur jeweils ein Task unter der Transaktion zu einem bestimmten Zeitpunkt ausgeführt werden kann. Bevor der Task ausgeführt werden kann, muss er exklusiven Zugriff auf den Transaktionsmutex besitzen.  
  
     Damit ein Task unter MARS ausgeführt werden kann, muss er den Sitzungsmutex abrufen. Wenn der Task unter einer Transaktion ausgeführt wird, muss er den Transaktionsmutex abrufen. Auf diese Weise wird garantiert, dass nur jeweils ein Task gleichzeitig in einer bestimmten Sitzung und einer bestimmten Transaktion aktiviert ist. Nachdem die erforderlichen Mutexe abgerufen wurden, kann der Task ausgeführt werden. Nachdem der Task beendet ist oder in der Mitte der Anforderung ein Ergebnis liefert, gibt er zuerst den Transaktionsmutex frei und dann den Sitzungsmutex (in umgekehrter Reihenfolge des Abrufs). Mit diesen Ressourcen können jedoch Deadlocks auftreten. Im folgenden Codebeispiel werden zwei Tasks, Benutzeranforderung U1 und Benutzeranforderung U2, in der gleichen Sitzung ausgeführt.  
  
    ```  
    U1:    Rs1=Command1.Execute("insert sometable EXEC usp_someproc");  
    U2:    Rs2=Command2.Execute("select colA from sometable");  
    ```  
  
     Die gespeicherte Prozedur, die durch Benutzeranforderung U1 ausgeführt wird, hat den Sitzungsmutex abgerufen. Wenn die gespeicherte Prozedur viel Zeit für die Ausführung benötigt, geht [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] davon aus, dass die gespeicherte Prozedur auf Eingaben vom Benutzer wartet. Benutzeranforderung U2 wartet auf den Sitzungsmutex, während der Benutzer auf das Resultset aus U2 wartet, und U1 wartet auf eine Benutzerressource. Logisch stellt sich der Deadlockstatus wie folgt dar:  
  
 ![LogicFlowExamplec](../relational-databases/media/udb9_LogicFlowExamplec.png)  
  
### <a name="deadlock-detection"></a><a name="deadlock_detection"></a> Deadlockerkennung  
 Alle in diesem Abschnitt aufgeführten Ressourcen nehmen am [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Deadlockerkennungsschema teil. Die Deadlockerkennung wird von einem Sperrenüberwachungsthread ausgeführt, der periodisch alle Tasks in einer Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] durchsucht. Die folgenden Schritte beschreiben den Suchvorgang:  
  
-   Das Standardintervall beträgt 5 Sekunden.  
-   Wenn der Sperrenüberwachungsthread Deadlocks findet, sinkt das Deadlockerkennungsintervall abhängig von der Häufigkeit von Deadlocks von 5 Sekunden auf bis zu 100 Millisekunden.  
-   Wenn der Sperrenüberwachungsthread keine weiteren Deadlocks mehr findet, verlängert [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] die Intervalle zwischen den Suchvorgängen auf 5 Sekunden.  
-   Wenn ein Deadlock gerade erkannt wurde, wird davon ausgegangen, dass die nächsten Threads, die auf eine Sperre warten müssen, in den Deadlockzyklus eingehen. Die ersten Wartevorgänge auf Sperren nach der Erkennung eines Deadlocks lösen sofort eine Deadlocksuche aus; es wird nicht auf das nächste Deadlockerkennungsintervall gewartet. Wenn das aktuelle Intervall z. B. 5 Sekunden beträgt und soeben ein Deadlock erkannt wurde, löst der nächste Wartevorgang auf eine Sperre die Deadlockerkennung sofort aus. Wenn dieser Wartevorgang auf eine Sperre Teil eines Deadlocks ist, wird er sofort und nicht erst während der nächsten Deadlocksuche erkannt.  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] führt normalerweise nur regelmäßige Deadlockerkennung aus. Da die Anzahl der vorgefundenen Deadlocks im System in der Regel gering ist, kann mithilfe der regelmäßigen Erkennung von Deadlocks der Aufwand der Deadlockerkennung im System gesenkt werden.  
  
 Wenn die Sperrenüberwachung die Suche nach Deadlocks für einen bestimmten Thread initiiert, wird die Ressource identifiziert, auf die der Thread wartet. Die Sperrenüberwachung findet dann den (die) Besitzer dieser Ressource und führt rekursiv die Deadlocksuche für diese Threads fort, bis ein Zyklus gefunden wird. Ein auf diese Art identifizierter Zyklus bildet einen Deadlock.  
  
 Nachdem ein Deadlock erkannt wurde, beendet [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] den Deadlock, indem einer der Threads als Deadlockopfer ausgewählt wird. [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] beendet den aktuellen Batch, der für den Thread ausgeführt wird, führt ein Rollback der Transaktion des Deadlockopfers aus und gibt den Fehler 1205 an die Anwendung zurück. Durch den Rollback der Transaktion für das Deadlockopfer werden alle von der Transaktion aufrecht erhaltenen Sperren freigegeben. Auf diese Weise kann die Sperre der Transaktionen der anderen Threads aufgehoben werden, und diese können fortgesetzt werden. Der Fehler 1205 (Deadlockopfer) zeichnet Informationen zu den an einem Deadlock beteiligten Threads und Ressourcen im Fehlerprotokoll auf.  
  
 Standardmäßig wählt [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] die Sitzung als Deadlockopfer aus, die die Transaktion ausführt, für die mit dem geringsten Aufwand ein Rollback ausgeführt werden kann. Alternativ kann ein Benutzer mithilfe der `SET DEADLOCK_PRIORITY`-Anweisung die Priorität der Sitzungen im Falle eines Deadlocks angeben. DEADLOCK_PRIORITY kann auf LOW, NORMAL oder HIGH oder alternativ auf einen beliebigen ganzzahligen Wert im Bereich zwischen -10 und 10 festgelegt werden. Die Deadlockpriorität ist standardmäßig NORMAL. Wenn die Sitzungen verschiedene Deadlockprioritäten besitzen, wird die Sitzung mit der niedrigeren Deadlockpriorität als Deadlockopfer ausgewählt. Wurde für beide Sitzungen die gleiche Deadlockprioriät festgelegt, wird diejenige Sitzung als Deadlockopfer ausgewählt, für die der Rollback weniger aufwändig ist. Wenn die am Deadlockzyklus beteiligten Sitzungen die gleiche Deadlockpriorität und die gleichen Kosten besitzen, wird das Opfer zufällig ausgewählt.  
  
 Wenn CLR verwendet wird, erkennt der Deadlockmonitor automatisch Deadlocks für Synchronisierungsressourcen (Überwachungsprogramme, Leser/Schreibersperre und Threadjoin), auf die in verwalteten Prozeduren zugegriffen wird. Der Deadlock wird jedoch behoben, indem eine Ausnahme in der Prozedur ausgelöst wird, die als Deadlockopfer ausgewählt wurde. Beachten Sie unbedingt, dass die Ausnahme nicht automatisch Ressourcen freigibt, die sich zurzeit im Besitz des Opfers befinden; die Ressourcen müssen explizit freigegeben werden. Die zum Identifizieren eines Deadlockopfers verwendete Ausnahme kann konsistent mit dem Verhalten der Ausnahme abgefangen und behandelt werden.  
  
### <a name="deadlock-information-tools"></a><a name="deadlock_tools"></a> Tools zum Anzeigen von Deadlockinformationen  
 Zum Anzeigen von Deadlockinformationen werden in [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] Überwachungstools in Form einer system\_health xEvent-Sitzung, zwei Ablaufverfolgungsflags sowie das Deadlock Graph-Ereignis in SQL Profiler bereitgestellt.  

#### <a name="deadlock-extended-event"></a><a name="deadlock_xevent"></a> Erweitertes Deadlock-Ereignis
Ab [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] sollte das erweiterte Ereignis `xml_deadlock_report` (xEvent) anstelle der Ereignisklasse des Deadlock-Graphen in der SQL-Ablaufverfolgung oder in SQL Server Profiler verwendet werden.

Ebenfalls ab [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] erfasst die ***system\_health** _-Sitzung bei Auftreten eines Deadlock bereits alle `xml_deadlock_report`-xEvents, die den Deadlockgraphen enthalten. Da die _system\_health*-Sitzung standardmäßig aktiviert ist, ist es nicht erforderlich, eine eigene xEvent-Sitzung zu konfigurieren, um Deadlockinformationen erfassen. 

Der in der Regel erfasste Deadlock Graph verfügt über drei unterschiedliche Knoten:
-   **victim-list:** Prozessbezeichner des Deadlockopfers.
-   **process-list:** Informationen zu allen am Deadlock beteiligten Prozessen.
-   **resource-list:** Informationen zu den am Deadlock beteiligten Ressourcen.

Das folgende Beispiel zeigt, wie [!INCLUDE[ssManStudio](../includes/ssManStudio-md.md)] beim Öffnen der system\_health-Sitzungsdatei oder des -Ringpuffers (sofern der `xml_deadlock_report`-xEvent aufgezeichnet wird) eine grafische Darstellung der an einem Deadlock beteiligten Tasks und Ressourcen bereitstellt: 

![xEvent-Deadlock-Graph](../relational-databases/media/udb9_xEventDeadlockGraphc.png)

Die folgende Abfrage kann alle Deadlock-Ereignisse anzeigen, die vom Ringpuffer der *system\_health*-Sitzung erfasst wurden:

```sql
SELECT xdr.value('@timestamp', 'datetime') AS [Date],
    xdr.query('.') AS [Event_Data]
FROM (SELECT CAST([target_data] AS XML) AS Target_Data
            FROM sys.dm_xe_session_targets AS xt
            INNER JOIN sys.dm_xe_sessions AS xs ON xs.address = xt.event_session_address
            WHERE xs.name = N'system_health'
              AND xt.target_name = N'ring_buffer'
    ) AS XML_Data
CROSS APPLY Target_Data.nodes('RingBufferTarget/event[@name="xml_deadlock_report"]') AS XEventData(xdr)
ORDER BY [Date] DESC
```

[!INCLUDE[ssResult](../includes/ssresult-md.md)]

![system_health_xevent_query_result](../relational-databases/media/system_health_qry.png)

Das folgende Beispiel zeigt die Ausgabe nach dem Klicken auf den ersten Link der vorhergehenden Ergebnisse:

```xml
<event name="xml_deadlock_report" package="sqlserver" timestamp="2018-02-18T08:26:24.698Z">
  <data name="xml_report">
    <type name="xml" package="package0" />
    <value>
      <deadlock>
        <victim-list>
          <victimProcess id="process27b9b0b9848" />
        </victim-list>
        <process-list>
          <process id="process27b9b0b9848" taskpriority="0" logused="0" waitresource="KEY: 5:72057594214350848 (1a39e6095155)" waittime="1631" ownerId="11088595" transactionname="SELECT" lasttranstarted="2018-02-18T00:26:23.073" XDES="0x27b9f79fac0" lockMode="S" schedulerid="9" kpid="15336" status="suspended" spid="62" sbid="0" ecid="0" priority="0" trancount="0" lastbatchstarted="2018-02-18T00:26:22.893" lastbatchcompleted="2018-02-18T00:26:22.890" lastattention="1900-01-01T00:00:00.890" clientapp="SQLCMD" hostname="ContosoServer" hostpid="7908" loginname="CONTOSO\user" isolationlevel="read committed (2)" xactid="11088595" currentdb="5" lockTimeout="4294967295" clientoption1="538968096" clientoption2="128056">
            <executionStack>
              <frame procname="AdventureWorks2016CTP3.dbo.p1" line="3" stmtstart="78" stmtend="180" sqlhandle="0x0300050020766505ca3e07008ba8000001000000000000000000000000000000000000000000000000000000">
SELECT c2, c3 FROM t1 WHERE c2 BETWEEN @p1 AND @p1+    </frame>
              <frame procname="adhoc" line="4" stmtstart="82" stmtend="98" sqlhandle="0x020000006263ec01ebb919c335024a072a2699958d3fcce60000000000000000000000000000000000000000">
unknown    </frame>
            </executionStack>
            <inputbuf>
SET NOCOUNT ON
WHILE (1=1) 
BEGIN
    EXEC p1 4
END
   </inputbuf>
          </process>
          <process id="process27b9ee33c28" taskpriority="0" logused="252" waitresource="KEY: 5:72057594214416384 (e5b3d7e750dd)" waittime="1631" ownerId="11088593" transactionname="UPDATE" lasttranstarted="2018-02-18T00:26:23.073" XDES="0x27ba15a4490" lockMode="X" schedulerid="6" kpid="5584" status="suspended" spid="58" sbid="0" ecid="0" priority="0" trancount="2" lastbatchstarted="2018-02-18T00:26:22.890" lastbatchcompleted="2018-02-18T00:26:22.890" lastattention="1900-01-01T00:00:00.890" clientapp="SQLCMD" hostname="ContosoServer" hostpid="15316" loginname="CONTOSO\user" isolationlevel="read committed (2)" xactid="11088593" currentdb="5" lockTimeout="4294967295" clientoption1="538968096" clientoption2="128056">
            <executionStack>
              <frame procname="AdventureWorks2016CTP3.dbo.p2" line="3" stmtstart="76" stmtend="150" sqlhandle="0x03000500599a5906ce3e07008ba8000001000000000000000000000000000000000000000000000000000000">
UPDATE t1 SET c2 = c2+1 WHERE c1 = @p    </frame>
              <frame procname="adhoc" line="4" stmtstart="82" stmtend="98" sqlhandle="0x02000000008fe521e5fb1099410048c5743ff7da04b2047b0000000000000000000000000000000000000000">
unknown    </frame>
            </executionStack>
            <inputbuf>
SET NOCOUNT ON
WHILE (1=1) 
BEGIN
    EXEC p2 4
END
   </inputbuf>
          </process>
        </process-list>
        <resource-list>
          <keylock hobtid="72057594214350848" dbid="5" objectname="AdventureWorks2016CTP3.dbo.t1" indexname="cidx" id="lock27b9dd26a00" mode="X" associatedObjectId="72057594214350848">
            <owner-list>
              <owner id="process27b9ee33c28" mode="X" />
            </owner-list>
            <waiter-list>
              <waiter id="process27b9b0b9848" mode="S" requestType="wait" />
            </waiter-list>
          </keylock>
          <keylock hobtid="72057594214416384" dbid="5" objectname="AdventureWorks2016CTP3.dbo.t1" indexname="idx1" id="lock27afa392600" mode="S" associatedObjectId="72057594214416384">
            <owner-list>
              <owner id="process27b9b0b9848" mode="S" />
            </owner-list>
            <waiter-list>
              <waiter id="process27b9ee33c28" mode="X" requestType="wait" />
            </waiter-list>
          </keylock>
        </resource-list>
      </deadlock>
    </value>
  </data>
</event>
```

Weitere Informationen finden Sie unter [Verwenden der system_health-Sitzung](../relational-databases/extended-events/use-the-system-health-session.md).

#### <a name="trace-flag-1204-and-trace-flag-1222"></a><a name="deadlock_traceflags"></a> Ablaufverfolgungsflag 1204 und Ablaufverfolgungsflag 1222  
 Wenn Deadlocks auftreten, geben die Ablaufverfolgungsflags 1204 und 1222 Informationen zurück, die im [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Fehlerprotokoll erfasst werden. Ablaufverfolgungsflag 1204 meldet von jedem im Deadlock beteiligten Knoten formatierte Deadlockinformationen. Ablaufverfolgungsflag 1222 formatiert Deadlockinformationen, zunächst prozessweise, anschließend Ressource für Ressource. Es ist möglich, beide Ablaufverfolgungsflags zu aktivieren, um zwei Darstellungen desselben Deadlockereignisses zu erhalten.  

> [!IMPORTANT]
> Vermeiden Sie die Verwendung der Ablaufverfolgungsflags 1204 und 1222 für Systeme mit hoher Arbeitsauslastung, die Deadlocks verursachen. Die Verwendung dieser Ablaufverfolgungsflags kann zu Leistungsproblemen führen. Verwenden Sie stattdessen das erweiterte Deadlock-Ereignis (#deadlock_xevent).
  
 Zur weiteren Definition der Eigenschaften der Ablaufverfolgungsflags 1204 und 1222 werden in der folgenden Tabelle die Ähnlichkeiten und Unterschiede aufgeführt.  
  
|Eigenschaft|Ablaufverfolgungsflag 1204 und Ablaufverfolgungsflag 1222|Nur Ablaufverfolgungsflag 1204|Nur Ablaufverfolgungsflag 1222|  
|--------------|-----------------------------------------|--------------------------|--------------------------|  
|Ausgabeformat|Die Ausgabe wird im [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Fehlerprotokoll erfasst.|Ist auf die im Deadlock beteiligten Knoten ausgerichtet. Jeder Knoten verfügt über einen dedizierten Abschnitt, wobei der letzte Abschnitt das Deadlockopfer beschreibt.|Gibt Informationen im XML-ähnlichen Format zurück, das einer XSD-Sprache (XML Schema Definition) nicht entspricht. Das Format verfügt über drei große Abschnitte. Der erste Abschnitt deklariert das Deadlockopfer. Der zweite Abschnitt beschreibt die jeweiligen im Deadlock beteiligten Prozesse. Der dritte Abschnitt beschreibt die Ressourcen, die den Knoten des Ablaufverfolgungsflags 1204 entsprechen.|  
|Identifizieren von Attributen|**SPID:<x\> ECID:<x\>.** : Identifiziert den Thread der Systemprozess-ID bei parallelen Prozessen. Der Eintrag `SPID:<x> ECID:0`, in dem <x\> durch den SPID-Wert ersetzt wird, stellt den Hauptthread dar. Der Eintrag `SPID:<x> ECID:<y>`, in dem <x\> durch den SPID-Wert ersetzt wird und <y\> größer als 0 ist, stellt die Unterthreads desselben SPID-Werts dar.<br /><br /> **BatchID** (**sbid** für Ablaufverfolgungsflag 1222): Identifiziert den Batch, von dem die Codeausführung angefragt oder eine Sperre aufrechterhalten wird. Wenn MARS (Multiple Active Result Sets) deaktiviert sind, ist der BatchID-Wert gleich 0. Wenn MARS aktiviert sind, kann der Wert für aktive Batches von 1 bis *n* reichen. Sind in der Sitzung keine aktiven Batches vorhanden, ist der BatchID-Wert gleich 0.<br /><br /> **Mode**: Gibt den Typ der Sperre für eine bestimmte Ressource an, die angefragt, erteilt oder von einem Thread erwartet wird. Dies kann eine beabsichtigte freigegebene Sperre (Intent Shared, IS), eine freigegebene Sperre (Shared), eine Updatesperre (Update, U), eine beabsichtigte exklusive Sperre (Intent Exclusive, IX), eine freigegebene mit beabsichtigten exklusiven Sperren (Shared with Intent Exclusive, SIX) und eine exklusive Sperre (Exclusive, X) sein.<br /><br /> **Line #** (**line** für Ablaufverfolgungsflag 1222): Listet die Zeilennummer des aktuellen Batches von Anweisungen auf, die beim Auftreten des Deadlocks ausgeführt wurden.<br /><br /> **Input Buf** (**inputbuf** für Ablaufverfolgungsflag 1222): Listet alle Anweisungen im aktuellen Batch auf.|**Node**: Stellt die Eintragsnummer in der Deadlockkette dar.<br /><br /> **Lists**: Der Sperrenbesitzer kann Bestandteil dieser Listen sein:<br /><br /> **Grant List**: Zählt die aktuellen Besitzer der Ressource auf.<br /><br /> **Convert List**: Zählt die aktuellen Besitzer auf, die versuchen, ihre Sperren in eine höhere Ebene zu konvertieren.<br /><br /> **Wait List**: Zählt die neuesten Sperrenanforderungen für die Ressource auf.<br /><br /> **Statement Type**: Beschreibt den Typ der DML-Anweisung (SELECT, INSERT, UPDATE oder DELETE), für die Threads über Berechtigungen verfügen.<br /><br /> **Victim Resource Owner**: Gibt den teilnehmenden Thread an, den [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] zum Durchbrechen des Deadlockzyklus als Opfer auswählt. Der ausgewählte Thread und alle vorhandenen Unterthreads werden beendet.<br /><br /> **Next Branch**: Stellt die beiden oder mehreren im Deadlockzyklus beteiligten Unterthreads desselben SPID-Werts dar.|**deadlock victim**: Stellt die physische Speicheradresse des Tasks dar (siehe [sys.dm_os_tasks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)), der als Deadlockopfer ausgewählt wurde. Im Fall eines nicht aufgelösten Deadlocks kann diese Angabe 0 (Null) sein. Ein Task, für den ein Rollback ausgeführt wird, kann nicht als Deadlockopfer ausgewählt werden.<br /><br /> **executionstack**: Stellt den [!INCLUDE[tsql](../includes/tsql-md.md)]-Code dar, der zum Zeitpunkt des Auftretens des Deadlocks ausgeführt wird.<br /><br /> **priority**: Stellt die Deadlockpriorität dar. Unter bestimmten Umständen kann [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] die Deadlockpriorität für eine kurze Zeitspanne ändern, um eine bessere Parallelität zu erzielen.<br /><br /> **logused**: Vom Task verwendeter Protokollspeicherplatz.<br /><br /> **owner id**: Die ID der Transaktion, die die Steuerung der Anforderung durchführt.<br /><br /> **status**: Der Status des Tasks. Ist einer der folgenden Werte:<br /><br /> >> **pending**: Warten auf einen Arbeitsthread.<br /><br /> >> **runnable**: Bereit zum Ausführen, jedoch wird auf das Eintreffen eines Quantums gewartet.<br /><br /> >> **running**: Wird derzeit auf dem Zeitplanungsmodul ausgeführt.<br /><br /> >> **suspended**: Die Ausführung wird angehalten.<br /><br /> >> **done**: Der Task ist abgeschlossen.<br /><br /> >> **spinloop**: Es wird auf die Verfügbarkeit eines Spinlocks gewartet.<br /><br /> **waitresource**: Die vom Task benötigte Ressource.<br /><br /> **waittime**: Zeitspanne in Millisekunden, die auf die Ressource gewartet wurde.<br /><br /> **schedulerid**: Diesem Task zugeordnetes Zeitplanungsmodul. Siehe [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).<br /><br /> **hostname**: Der Name der Arbeitsstation.<br /><br /> **isolationlevel**: Die aktuelle Isolationsstufe für Transaktionen.<br /><br /> **Xactid**: Die ID der Transaktion, die die Steuerung der Anforderung durchführt.<br /><br /> **currentdb**: Die ID der Datenbank.<br /><br /> **lastbatchstarted**: Uhrzeit des letzten Starts der Batchausführung durch einen Clientprozess.<br /><br /> **lastbatchcompleted**: Uhrzeit des letzten Abschlusses der Batchausführung durch einen Clientprozess.<br /><br /> **clientoption1 und clientoption2**: SET-Optionen für diese Clientverbindung. Es handelt sich um ein Bitmuster, das Informationen zu Optionen enthält, die normalerweise durch SET-Anweisungen, z. B. SET NOCOUNT und SET XACTABORT, gesteuert werden.<br /><br /> **associatedObjectId**: Stellt die HoBT-ID (Heap- oder B-Struktur) dar.|  
|Ressourcenattribute|**RID**: Identifiziert die einzelne Zeile innerhalb einer Tabelle, in der eine Sperre aufrechterhalten oder angefragt wird. RID wird als RID dargestellt: *db_id:file_id:page_no:row_no*. Beispiel: `RID: 6:1:20789:0`.<br /><br /> **OBJECT**: Identifiziert die Tabelle, in der eine Sperre aufrechterhalten oder angefragt wird. OBJECT wird als OBJECT dargestellt: *db_id:object_id*. Beispiel: `TAB: 6:2009058193`.<br /><br /> **KEY**: Identifiziert den Schlüsselbereich innerhalb eines Indexes, in dem eine Sperre aufrechterhalten oder angefragt wird. KEY wird als KEY dargestellt: *db_id:hobt_id* (*Indexschlüssel-Hashwert*). Beispiel: `KEY: 6:72057594057457664 (350007a4d329)`.<br /><br /> **PAG**: Identifiziert die Seitenressource, in der eine Sperre aufrechterhalten oder angefragt wird. PAG wird als PAG dargestellt: *db_id:file_id:page_no*. Beispiel: `PAG: 6:1:20789`.<br /><br /> **EXT**: Identifiziert die Blockstruktur. EXT wird als EXT dargestellt: *db_id:file_id:extent_no*. Beispiel: `EXT: 6:1:9`.<br /><br /> **DB**: Identifiziert die Datenbanksperre. **DB wird auf eine der folgenden Arten dargestellt:**<br /><br /> DB: *db_id*<br /><br /> DB: *db_id*[BULK-OP-DB]: Identifiziert die von der Sicherungsdatenbank erstellte Datenbanksperre.<br /><br /> DB: *db_id*[BULK-OP-LOG]: Identifiziert die vom Sicherungsprotokoll für diese bestimmte Datenbank erstellte Sperre.<br /><br /> **APP**: Identifiziert die von einer Anwendungsressource erstellte Sperre. APP wird als APP dargestellt: *lock_resource*. Beispiel: `APP: Formf370f478`.<br /><br /> **METADATA**: Stellt die in einem Deadlock beteiligten Metadatenressourcen dar. Da METADATA über viele Unterressourcen verfügt, hängt der zurückgegebene Wert von der Unterressource ab, für die ein Deadlock vorliegt. METADATA.USER_TYPE gibt beispielsweise `user_type_id =` <*integer_value*> zurück. Weitere Informationen zu METADATA-Ressourcen und -Unterressourcen finden Sie unter [sys.dm_tran_locks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).<br /><br /> **HOBT**: Stellt eine in einem Deadlock beteiligte Heap- oder B-Struktur dar.|Gilt nicht ausschließlich für dieses Ablaufverfolgungsflag.|Gilt nicht ausschließlich für dieses Ablaufverfolgungsflag.|  
  
##### <a name="trace-flag-1204-example"></a>Beispiel für Ablaufverfolgungsflag 1204  
 Im folgenden Beispiel wird die Ausgabe beim Aktivieren des Ablaufverfolgungsflags 1204 gezeigt. Hierbei wird die Tabelle auf Knoten 1 als Heap ohne Indizes und die Tabelle auf Knoten 2 als Heap mit einem nicht gruppierten Index verwendet. Der Indexschlüssel auf Knoten 2 wird beim Auftreten des Deadlocks aktualisiert.  
  
```  
Deadlock encountered .... Printing deadlock information  
Wait-for graph  
  
Node:1  
  
RID: 6:1:20789:0               CleanCnt:3 Mode:X Flags: 0x2  
 Grant List 0:  
   Owner:0x0315D6A0 Mode: X          
     Flg:0x0 Ref:0 Life:02000000 SPID:55 ECID:0 XactLockInfo: 0x04D9E27C  
   SPID: 55 ECID: 0 Statement Type: UPDATE Line #: 6  
   Input Buf: Language Event:   
BEGIN TRANSACTION  
   EXEC usp_p2  
 Requested By:   
   ResType:LockOwner Stype:'OR'Xdes:0x03A3DAD0   
     Mode: U SPID:54 BatchID:0 ECID:0 TaskProxy:(0x04976374) Value:0x315d200 Cost:(0/868)  
  
Node:2  
  
KEY: 6:72057594057457664 (350007a4d329) CleanCnt:2 Mode:X Flags: 0x0  
 Grant List 0:  
   Owner:0x0315D140 Mode: X          
     Flg:0x0 Ref:0 Life:02000000 SPID:54 ECID:0 XactLockInfo: 0x03A3DAF4  
   SPID: 54 ECID: 0 Statement Type: UPDATE Line #: 6  
   Input Buf: Language Event:   
     BEGIN TRANSACTION  
       EXEC usp_p1  
 Requested By:   
   ResType:LockOwner Stype:'OR'Xdes:0x04D9E258   
     Mode: U SPID:55 BatchID:0 ECID:0 TaskProxy:(0x0475E374) Value:0x315d4a0 Cost:(0/380)  
  
Victim Resource Owner:  
 ResType:LockOwner Stype:'OR'Xdes:0x04D9E258   
     Mode: U SPID:55 BatchID:0 ECID:0 TaskProxy:(0x0475E374) Value:0x315d4a0 Cost:(0/380)  
```  
  
##### <a name="trace-flag-1222-example"></a>Beispiel für Ablaufverfolgungsflag 1222  
 Im folgenden Beispiel wird die Ausgabe beim Aktivieren des Ablaufverfolgungsflags 1222 gezeigt. Hierbei wird eine Tabelle als Heap ohne Indizes und die andere Tabelle als Heap mit einem nicht gruppierten Index verwendet. In der zweiten Tabelle wird der Indexschlüssel beim Auftreten des Deadlocks aktualisiert.  
  
```  
deadlock-list  
 deadlock victim=process689978  
  process-list  
   process id=process6891f8 taskpriority=0 logused=868   
   waitresource=RID: 6:1:20789:0 waittime=1359 ownerId=310444   
   transactionname=user_transaction   
   lasttranstarted=2005-09-05T11:22:42.733 XDES=0x3a3dad0   
   lockMode=U schedulerid=1 kpid=1952 status=suspended spid=54   
   sbid=0 ecid=0 priority=0 transcount=2   
   lastbatchstarted=2005-09-05T11:22:42.733   
   lastbatchcompleted=2005-09-05T11:22:42.733   
   clientapp=Microsoft SQL Server Management Studio - Query   
   hostname=TEST_SERVER hostpid=2216 loginname=DOMAIN\user   
   isolationlevel=read committed (2) xactid=310444 currentdb=6   
   lockTimeout=4294967295 clientoption1=671090784 clientoption2=390200  
    executionStack  
     frame procname=AdventureWorks2016.dbo.usp_p1 line=6 stmtstart=202   
     sqlhandle=0x0300060013e6446b027cbb00c69600000100000000000000  
     UPDATE T2 SET COL1 = 3 WHERE COL1 = 1;       
     frame procname=adhoc line=3 stmtstart=44   
     sqlhandle=0x01000600856aa70f503b8104000000000000000000000000  
     EXEC usp_p1       
    inputbuf  
      BEGIN TRANSACTION  
       EXEC usp_p1  
   process id=process689978 taskpriority=0 logused=380   
   waitresource=KEY: 6:72057594057457664 (350007a4d329)     
   waittime=5015 ownerId=310462 transactionname=user_transaction   
   lasttranstarted=2005-09-05T11:22:44.077 XDES=0x4d9e258 lockMode=U   
   schedulerid=1 kpid=3024 status=suspended spid=55 sbid=0 ecid=0   
   priority=0 transcount=2 lastbatchstarted=2005-09-05T11:22:44.077   
   lastbatchcompleted=2005-09-05T11:22:44.077   
   clientapp=Microsoft SQL Server Management Studio - Query   
   hostname=TEST_SERVER hostpid=2216 loginname=DOMAIN\user   
   isolationlevel=read committed (2) xactid=310462 currentdb=6   
   lockTimeout=4294967295 clientoption1=671090784 clientoption2=390200  
    executionStack  
     frame procname=AdventureWorks2016.dbo.usp_p2 line=6 stmtstart=200   
     sqlhandle=0x030006004c0a396c027cbb00c69600000100000000000000  
     UPDATE T1 SET COL1 = 4 WHERE COL1 = 1;       
     frame procname=adhoc line=3 stmtstart=44   
     sqlhandle=0x01000600d688e709b85f8904000000000000000000000000  
     EXEC usp_p2       
    inputbuf  
      BEGIN TRANSACTION  
        EXEC usp_p2      
  resource-list  
   ridlock fileid=1 pageid=20789 dbid=6 objectname=AdventureWorks2016.dbo.T2   
   id=lock3136940 mode=X associatedObjectId=72057594057392128  
    owner-list  
     owner id=process689978 mode=X  
    waiter-list  
     waiter id=process6891f8 mode=U requestType=wait  
   keylock hobtid=72057594057457664 dbid=6 objectname=AdventureWorks2016.dbo.T1   
   indexname=nci_T1_COL1 id=lock3136fc0 mode=X   
   associatedObjectId=72057594057457664  
    owner-list  
     owner id=process6891f8 mode=X  
    waiter-list  
     waiter id=process689978 mode=U requestType=wait  
```  
  
#### <a name="profiler-deadlock-graph-event"></a>Profiler Deadlock Graph-Ereignis  
Dies ist ein Ereignis in SQL Profiler, das eine grafische Darstellung der an einem Deadlock beteiligten Tasks und Ressourcen bereitstellt. Im folgenden Beispiel wird die Ausgabe von SQL Profiler gezeigt, wenn das Deadlock Graph-Ereignis aktiviert ist.  
  
 ![ProfilerDeadlockGraphc](../relational-databases/media/udb9_ProfilerDeadlockGraphc.png)  
  
Weitere Informationen zum Deadlockereignis finden Sie unter [Lock:Deadlock (Ereignisklasse)](../relational-databases/event-classes/lock-deadlock-event-class.md).

Weitere Informationen zum Ausführen von SQL Profiler Deadlock Graph finden Sie unter [Speichern von Deadlock Graphs &#40;SQL Server Profiler&#41;](../relational-databases/performance/save-deadlock-graphs-sql-server-profiler.md).  
  
### <a name="handling-deadlocks"></a>Behandeln von Deadlocks  
 Wenn eine [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Instanz eine Transaktion als Deadlockopfer auswählt, beendet sie den aktuellen Batch, führt einen Rollback der Transaktion durch und gibt die Fehlermeldung 1205 an die Anwendung zurück.  
  
 `Your transaction (process ID #52) was deadlocked on {lock | communication buffer | thread} resources with another process and has been chosen as the deadlock victim. Rerun your transaction.`  
  
 Da jede Anwendung, die [!INCLUDE[tsql](../includes/tsql-md.md)]-Abfragen sendet, als Deadlockopfer ausgewählt werden kann, sollten die Anwendungen über einen Fehlerhandler verfügen, der Fehlermeldung 1205 auffangen kann. Wenn eine Anwendung den Fehler nicht auffängt, wird sie möglicherweise weiter ausgeführt, da nicht erkannt wird, dass ein Rollback für die zugehörige Transaktion ausgeführt wurde, und es können Fehler auftreten.  
  
 Durch Implementieren eines Fehlerhandlers, der die Fehlermeldung 1205 abfängt, kann eine Anwendung den Deadlock verarbeiten und Abhilfemaßnahmen ergreifen, wie etwa die Abfrage, die am Deadlock beteiligt war, automatisch erneut abzusenden. Durch die automatische erneute Absendung der Abfrage ist es nicht notwendig, dass der Benutzer von dem Deadlock erfährt.  
  
 Die Anwendung sollte kurzzeitig angehalten werden, bevor die Abfrage erneut abgesendet wird. Auf diese Weise kann die andere Transaktion, die an einem Deadlock beteiligt ist, abgeschlossen werden und die Sperren freigeben, die einen Anteil am Deadlockzyklus hatten. Die Wahrscheinlichkeit, dass ein Deadlock erneut auftritt, wenn die erneut abgesendete Abfrage ihre Sperren anfordert, wird so verringert.  
  
### <a name="minimizing-deadlocks"></a><a name="deadlock_minimizing"></a> Minimieren von Deadlocks  
 Obwohl Deadlocks nicht vollständig vermieden werden können, kann das Risiko eines Deadlocks durch das Befolgen bestimmter Codierungskonventionen minimiert werden. Wenn die Anzahl der Deadlocks minimiert wird, können der Transaktionsdurchsatz erhöht und der Aufwand des Systems reduziert werden, und zwar aus folgenden Gründen:  
  
-   Die Anzahl der Transaktionen, für die ein Rollback ausgeführt wird, durch den die von einer Transaktion ausgeführte Arbeit rückgängig gemacht wird, ist geringer.  
-   Die Anzahl der Transaktionen, die von den Anwendungen erneut abgesendet werden, da für sie aufgrund des Deadlocks ein Rollback ausgeführt wurde, ist geringer.  
  
 So kann das Risiko von Deadlocks minimiert werden:  
  
-   Greifen Sie in derselben Reihenfolge auf Objekte zu.  
-   Vermeiden Sie Benutzerinteraktionen in Transaktionen.  
-   Verwenden Sie kurze Transaktionen in einem einzigen Batch.  
-   Verwenden Sie eine niedrigere Isolationsstufe.  
-   Verwenden Sie eine auf der Zeilenversionsverwaltung basierende Isolationsstufe.  
    -   Legen Sie die Datenbankoption `READ_COMMITTED_SNAPSHOT` auf ON fest, um die Verwendung der Zeilenversionsverwaltung für READ COMMITTED-Transaktionen zu aktivieren.  
    -   Verwenden Sie die Momentaufnahmeisolation.  
-   Verwenden Sie gebundene Verbindungen.  
  
#### <a name="access-objects-in-the-same-order"></a>Zugreifen auf Objekte in derselben Reihenfolge  
 Wenn alle gleichzeitigen Transaktionen in derselben Reihenfolge auf Objekte zugreifen, treten Deadlocks seltener auf. Wenn beispielsweise zwei gleichzeitige Transaktionen jeweils zuerst die **Supplier**-Tabelle und anschließend die **Part**-Tabelle mit einer Sperre belegen, wird eine Transaktion für die **Supplier**-Tabelle blockiert, bis die andere abgeschlossen ist. Nachdem für die erste Transaktion ein Commit- oder Rollback-Vorgang ausgeführt wurde, wird die Ausführung der zweiten Transaktion fortgesetzt, und es tritt kein Deadlock auf. Durch das Verwenden von gespeicherten Prozeduren für alle Datenänderungen kann die Reihenfolge, in der auf Objekte zugegriffen wird, standardisiert werden.  
  
 ![deadlock2](../relational-databases/media/dedlck2.png)  
  
#### <a name="avoid-user-interaction-in-transactions"></a>Vermeiden von Benutzerinteraktion in Transaktionen  
 Vermeiden Sie es, Transaktionen zu schreiben, die Benutzerinteraktionen enthalten, da die Geschwindigkeit von Batches, die ohne Benutzereingriffe ausgeführt werden, bedeutend höher ist als die Geschwindigkeit, mit der ein Benutzer manuell auf Abfragen reagieren muss (z. B. beim Antworten auf eine Eingabeaufforderung, wenn eine Anwendung einen Parameter anfordert). Wenn eine Transaktion z. B. auf eine Benutzereingabe wartet, der jeweilige Benutzer jedoch zum Essen oder sogar für das Wochenende nach Hause geht, verzögert der Benutzer die Fertigstellung der Transaktion. Dadurch wird der Durchsatz des Systems beeinträchtigt, da Sperren, die von der Transaktion aufrechterhalten werden, erst dann aufgehoben werden, wenn ein Commit oder Rollback für die Transaktion ausgeführt wird. Selbst wenn es nicht zu einem Deadlock kommt, werden andere Transaktionen blockiert, die auf dieselben Ressourcen zugreifen, da sie darauf warten, dass die Transaktion beendet wird.  
  
#### <a name="keep-transactions-short-and-in-one-batch"></a>Verwenden kurzer Transaktionen in einem einzigen Batch  
 Ein Deadlock tritt in der Regel dann auf, wenn mehrere Transaktionen mit langer Ausführungszeit gleichzeitig in derselben Datenbank ausgeführt werden. Je länger die Transaktion dauert, desto länger werden die exklusiven Sperren oder Updatesperren aufrechterhalten, wodurch andere Aktivitäten blockiert werden und es möglicherweise zu Deadlocks kommt.  
  
 Wenn die Transaktionen in einem einzigen Batch enthalten sind, wird die Anzahl der Netzwerkroundtrips während einer Transaktion minimiert, wodurch mögliche Verzögerungen beim Beenden der Transaktion und Aufheben der Sperren reduziert werden.  
  
#### <a name="use-a-lower-isolation-level"></a>Verwenden einer niedrigeren Isolationsstufe  
 Ermitteln Sie, ob eine Transaktion auf einer niedrigeren Isolationsstufe ausgeführt werden kann. Durch die READ COMMITTED-Implementierung kann eine Transaktion Daten, die zuvor von einer anderen Transaktion gelesen (nicht geändert) wurden, lesen, ohne warten zu müssen, bis die erste Transaktion abgeschlossen ist. Wenn eine niedrigere Isolationsstufe verwendet wird, beispielsweise READ COMMITTED, werden freigegebene Sperren kürzer aufrechterhalten als bei einer höheren Isolationsstufe, beispielsweise der serialisierbaren. Hierdurch werden Sperrkonflikte reduziert.  
  
#### <a name="use-a-row-versioning-based-isolation-level"></a>Verwenden einer auf der Zeilenversionsverwaltung basierenden Isolationsstufe  
 Wenn die Datenbankoption `READ_COMMITTED_SNAPSHOT` auf ON festgelegt ist, verwendet eine Transaktion, die gemäß der READ COMMITTED-Isolationsstufe ausgeführt wird, bei Lesevorgängen die Zeilenversionsverwaltung anstelle freigegebener Sperren.  
  
> [!NOTE]  
> Einige Anwendungen sind auf das Sperr- und Blockierverhalten der READ COMMITTED-Isolation angewiesen. Für diese Anwendungen sind Änderungen erforderlich, bevor diese Option aktiviert werden kann.  
  
 Die Momentaufnahmeisolation verwendet auch die Zeilenversionsverwaltung, die bei Lesevorgängen keine freigegebenen Sperren verwenden. Bevor eine Transaktion gemäß der Momentaufnahmeisolation ausgeführt werden kann, muss die Datenbankoption `ALLOW_SNAPSHOT_ISOLATION` auf ON festgelegt werden.  
  
 Implementieren Sie diese Isolationsstufen, um die Wahrscheinlichkeit von Deadlocks zu minimieren, die zwischen Lese- und Schreibvorgängen auftreten können.  
  
#### <a name="use-bound-connections"></a>Verwenden gebundener Verbindungen  
 Beim Verwenden gebundener Verbindungen können zwei oder mehr Verbindungen, die von derselben Anwendung geöffnet wurden, zusammenarbeiten. Sperren, die von den sekundären Verbindungen eingerichtet wurden, werden so aufrechterhalten, als ob sie von der primären Verbindung eingerichtet wurden, und umgekehrt. Folglich blockieren sie sich nicht gegenseitig.  
  
## <a name="lock-partitioning"></a><a name="lock_partitioning"></a> Sperrenpartitionierung  
 In großen Computersystemen können Sperren für häufig referenzierte Objekte einen Leistungsengpass darstellen, weil die Anforderung und Freigabe von Sperren zu Konflikten bei den internen Sperrenressourcen führt. Die Sperrenpartitionierung verbessert die Sperrenleistung, indem eine einzelne Sperrenressource in mehrere Sperrenressourcen aufgeteilt wird. Diese Funktion ist nur für Systeme mit 16 oder mehr CPUs verfügbar, wird automatisch aktiviert und kann nicht deaktiviert werden. Es können nur Objektsperren partitioniert werden. Objektsperren mit einem Untertyp werden nicht partitioniert. Weitere Informationen finden Sie unter [sys.dm_tran_locks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).  
  
### <a name="understanding-lock-partitioning"></a>Grundlegendes zur Sperrenpartitionierung  
 Sperrtasks greifen auf verschiedene freigegebene Ressourcen zu, von denen zwei durch die Sperrenpartitionierung optimiert werden:  
  
-   **Spinlock**: Diese Ressource steuert den Zugriff auf eine Sperrenressource wie z. B. eine Zeile oder Tabelle.  
  
     Ohne die Sperrenpartitionierung verwaltet ein Spinlock alle Sperrenanforderungen für eine einzelne Sperrenressource. Bei Systemen mit umfangreicher Aktivität kann es zu Konflikten kommen, wenn Sperrenanforderungen darauf warten, dass das Spinlock verfügbar wird. Unter diesen Umständen kann die Anforderung von Sperren zu einem Engpass werden und sich negativ auf die Leistung auswirken.  
  
     Um Konflikte bei einer einzelnen Sperrenressource zu verringern, teilt die Sperrenpartitionierung eine einzelne Sperrenressource in mehrere Sperrenressourcen auf, um die Auslastung auf mehrere Spinlocks zu verteilen.  
  
-   **Speicher** Wird zum Speichern der Strukturen von Sperrenressourcen verwendet.  
  
     Sobald das Spinlock aktiviert wurde, werden die Sperrenstrukturen im Arbeitsspeicher gespeichert, und anschließend erfolgt der Zugriff auf diese Strukturen, und sie werden möglicherweise geändert. Die Verteilung des Sperrenzugriffs auf mehrere Ressourcen senkt die Notwendigkeit zur Übertragung von Arbeitsspeicherblöcken zwischen CPUs, was zu einer verbesserten Leistung führt.  
  
### <a name="implementing-and-monitoring-lock-partitioning"></a>Implementieren und Überwachen der Sperrenpartitionierung  
 Die Sperrenpartitionierung wird bei Systemen mit mindestens 16 CPUs standardmäßig aktiviert. Wenn die Sperrenpartitionierung aktiviert ist, wird eine Informationsmeldung im [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Fehlerprotokoll gespeichert.  
  
 Beim Aktivieren von Sperren für eine partitionierte Ressource gelten folgende Grundsätze:  
  
-   Für eine einzelne Partition werden nur die Sperrmodi NL, SCH-S, IS, IU und IX aktiviert.  
  
-   Freigegebene Sperren (S), exklusive Sperren (X) und andere Sperren in anderen Modi als NL, SCH-S, IS, IU und IX müssen für alle Partitionen aktiviert werden, beginnend mit der Partitions-ID 0 und nachfolgend in der Partitions-ID-Reihenfolge. Diese Sperren für eine partitionierte Ressource beanspruchen mehr Arbeitsspeicher als Sperren im selben Modus für eine nicht partitionierte Ressource, weil jede Partition effektiv eine separate Sperre ist. Der erhöhte Arbeitsspeicherbedarf richtet sich nach der Anzahl der Partitionen. Die Sperren-Leistungsindikatoren von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] im Windows-Systemmonitor zeigen Informationen zum Arbeitsspeicher an, der von partitionierten und nicht partitionierten Sperren verwendet wird.  
  
 Beim Start einer Transaktion wird der Transaktion eine Partition zugewiesen. Bei der Transaktion verwenden alle Sperranforderungen, die partitioniert werden können, die der Transaktion zugewiesene Partition. Durch diese Methode wird der Zugriff auf Sperrenressourcen desselben Objekts durch unterschiedliche Transaktionen auf verschiedene Partitionen verteilt.  
  
 Die `resource_lock_partition`-Spalte in der dynamischen Verwaltungssicht (DMV, Dynamic Management View) von `sys.dm_tran_locks` stellt die Sperrenpartitions-ID für eine sperrenpartitionierte Ressource bereit. Weitere Informationen finden Sie unter [sys.dm_tran_locks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).  
  
### <a name="working-with-lock-partitioning"></a>Arbeiten mit der Sperrenpartitionierung  
 Die folgenden Codebeispiele veranschaulichen die Verwendung der Sperrenpartitionierung. In den Beispielen werden zwei Transaktionen in zwei verschiedenen Sitzungen ausgeführt, um das Verhalten der Sperrenpartitionierung in einem Computersystem mit 16 CPUs zu zeigen.  
  
 Mit diesen [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisungen werden Testobjekte erstellt, die in den folgenden Beispielen verwendet werden.  
  
```sql  
-- Create a test table.  
CREATE TABLE TestTable  (col1 int);  
GO  
  
-- Create a clustered index on the table.  
CREATE CLUSTERED INDEX ci_TestTable   
    ON TestTable (col1);  
GO  
  
-- Populate the table.  
INSERT INTO TestTable VALUES (1);  
GO  
```  
  
#### <a name="example-a"></a>Beispiel A  
 Sitzung 1  
  
 Im Rahmen einer Transaktion wird eine `SELECT`-Anweisung ausgeführt. Aufgrund des `HOLDLOCK`-Sperrhinweises aktiviert und hält diese Anweisung eine beabsichtigte freigegebene Sperre für die Tabelle (in dieser Veranschaulichung werden Zeilen- und Seitensperren ignoriert). Die beabsichtigte freigegebene Sperre wird nur für die Partition aktiviert, die der Transaktion zugewiesen ist. In diesem Beispiel wird vorausgesetzt, dass die beabsichtigte freigegebene Sperre für die Partitions-ID 7 aktiviert wird.  
  
```sql  
-- Start a transaction.  
BEGIN TRANSACTION  
    -- This SELECT statement will acquire an IS lock on the table.  
    SELECT col1  
    FROM TestTable  
    WITH (HOLDLOCK);  
```  
  
 Sitzung 2:  
  
 Eine Transaktion wird gestartet, und die im Rahmen dieser Transaktion ausgeführte `SELECT`-Anweisung aktiviert und hält eine freigegebene Sperre (S) für die Tabelle. Die S-Sperre wird für alle Partitionen aktiviert, was mehrere Tabellensperren ergibt, und zwar eine für jede Partition. Auf einem System mit 16 CPUs werden z. B. 16 S-Sperren für die Sperrpartitions-IDs 0 bis 15 aktiviert. Da die S-Sperre mit der beabsichtigten freigegebenen Sperre kompatibel ist, die von der Transaktion in Sitzung 1 für die Partitions-ID 7 gehalten wird, kommt es zu keiner Blockierung zwischen den Transaktionen.  
  
```sql  
BEGIN TRANSACTION  
    SELECT col1  
    FROM TestTable  
    WITH (TABLOCK, HOLDLOCK);  
```  
  
 Sitzung 1  
  
 Die folgende `SELECT`-Anweisung wird unter der Transaktion ausgeführt, die unter Sitzung 1 immer noch aktiv ist. Aufgrund des exklusiven (X) Tabellenblockhinweises versucht die Transaktion, eine X-Sperre für die Tabelle zu aktivieren. Allerdings blockiert die S-Sperre, die durch die Transaktion in Sitzung 2 gehalten wird, die X-Sperre für die Partitions-ID 0.  
  
```sql  
SELECT col1  
FROM TestTable  
WITH (TABLOCKX);  
```  
  
#### <a name="example-b"></a>Beispiel B  
 Sitzung 1  
  
 Im Rahmen einer Transaktion wird eine `SELECT`-Anweisung ausgeführt. Aufgrund des `HOLDLOCK`-Sperrhinweises aktiviert und hält diese Anweisung eine beabsichtigte freigegebene Sperre für die Tabelle (in dieser Veranschaulichung werden Zeilen- und Seitensperren ignoriert). Die beabsichtigte freigegebene Sperre wird nur für die Partition aktiviert, die der Transaktion zugewiesen ist. In diesem Beispiel wird vorausgesetzt, dass die beabsichtigte freigegebene Sperre für die Partitions-ID 6 aktiviert wird.  
  
```sql  
-- Start a transaction.  
BEGIN TRANSACTION  
    -- This SELECT statement will acquire an IS lock on the table.  
    SELECT col1  
    FROM TestTable  
    WITH (HOLDLOCK);  
```  
  
 Sitzung 2:  
  
 Im Rahmen einer Transaktion wird eine `SELECT`-Anweisung ausgeführt. Aufgrund des `TABLOCKX`-Sperrhinweises versucht die Transaktion, eine exklusive Sperre (X) für die Tabelle zu aktivieren. Denken Sie daran, dass die X-Sperre für alle Partitionen beginnend mit der Partitions-ID 0 aktiviert werden muss. Die X-Sperre wird für alle Partitions-IDs von 0 bis 5 aktiviert, sie wird jedoch von der für Partitions-ID 6 aktivierten Sperre blockiert.  
  
 Für die Partitions-IDs 7 bis 15, die die X-Sperre noch nicht erreicht hat, können andere Transaktionen weiterhin Sperren aktivieren.  
  
```sql  
BEGIN TRANSACTION  
    SELECT col1  
    FROM TestTable  
    WITH (TABLOCKX, HOLDLOCK);  
```   
  
##  <a name="row-versioning-based-isolation-levels-in-the-ssdenoversion"></a><a name="Row_versioning"></a> Auf Zeilenversionsverwaltung basierende Isolationsstufen im [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]  
 Ab [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] führt das [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] eine Implementierung der vorhandenen Transaktionsisolationsstufe READ COMMITTED ein, die mithilfe der Zeilenversionsverwaltung eine Momentaufnahme auf Anweisungsebene bereitstellt. Das [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] bietet außerdem die Transaktionsisolationsstufe SNAPSHOT, die ebenfalls die Zeilenversionsverwaltung verwendet, um Momentaufnahmen auf Transaktionsebene bereitzustellen.  
  
 Die Zeilenversionsverwaltung ist ein allgemeines Framework in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], das beim Ändern oder Löschen einer Zeile einen "Kopie-bei-Schreibvorgang"-Mechanismus aufruft. Das setzt bei einer ausgeführten Transaktion voraus, dass die alte Zeilenversion für Transaktionen verfügbar sein muss, die einen früheren transaktionskonsistenten Zustand erfordern. Die Zeilenversionsverwaltung wird zur Unterstützung folgender Funktionen verwendet:  
  
-   Erstellen der **inserted**- und **deleted**-Tabellen in Triggern. Für alle durch den Trigger geänderten Zeilen wird die Versionsverwaltung verwendet. Das schließt die Zeilen ein, die durch die Anweisung geändert wurden, mit der der Start des Triggers erfolgte, sowie alle vom Trigger bewirkten Datenänderungen.  
-   Unterstützen von Multiple Active Result Sets (MARS). Wenn eine MARS-Sitzung eine Datenänderungsanweisung (z.B. `INSERT`, `UPDATE` oder `DELETE`) ausgibt, während es ein aktives Resultset gibt, wird für die von der Änderungsanweisung betroffenen Zeilen die Versionsverwaltung verwendet.  
-   Unterstützen von Indexvorgängen, die die ONLINE-Option angeben.  
-   Unterstützen von auf der Zeilenversionsverwaltung basierenden Transaktionsisolationsstufen:  
    -   Eine neue Implementierung der Read Committed-Isolationsstufe, die die Zeilenversionsverwaltung verwendet, um die Lesekonsistenz auf Anweisungsebene zu gewährleisten.  
    -   Eine neue Isolationsstufe – Momentaufnahme, um die Lesekonsistenz auf der Transaktionsebene zu gewährleisten.  
  
 Die `tempdb`-Datenbank muss über ausreichend Speicherplatz verfügen, um die Versionen speichern zu können. Wenn `tempdb` voll ist, brechen Updatevorgänge die Versionsverwaltung ab und können fortgesetzt werden. Lesevorgänge können hingegen einen Fehler erzeugen, weil eine bestimmte Zeilenversion, die benötigt wird, nicht mehr vorhanden ist. Das wirkt sich auf Vorgänge wie Trigger, MARS und Onlineindizierung aus.  
  
 Das Verwenden der Zeilenversionsverwaltung für Read Committed- und Momentaufnahme-Transaktionen umfasst zwei Schritte:  
  
1.  Festlegen von einer oder beider Datenbankoptionen `READ_COMMITTED_SNAPSHOT` und `ALLOW_SNAPSHOT_ISOLATION` auf ON.  
2.  Festlegen der entsprechenden Transaktionsisolationsstufe in einer Anwendung:  

    -   Wenn die `READ_COMMITTED_SNAPSHOT`-Datenbankoption auf ON gesetzt ist, verwenden Transaktionen, die die Read Committed-Isolationsstufe festlegen, die Zeilenversionsverwaltung.  
    -   Wenn die `ALLOW_SNAPSHOT_ISOLATION`-Datenbankoption auf ON gesetzt ist, können Transaktionen die Momentaufnahme-Isolationsstufe festlegen.  
  
 Wenn eine der beiden Datenbankoptionen `READ_COMMITTED_SNAPSHOT` oder `ALLOW_SNAPSHOT_ISOLATION` auf ON gesetzt ist, weist das [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] jeder Transaktion, die Daten bearbeitet, mithilfe der Zeilenversionsverwaltung eine Transaktionssequenznummer (XSN, Transaction Sequence Number) zu. Die Transaktionen starten zu dem Zeitpunkt, wenn eine `BEGIN TRANSACTION`-Anweisung ausgeführt wird. Allerdings startet die Transaktionssequenznummer mit dem ersten Lese- oder Schreibvorgang nach der BEGIN TRANSACTION-Anweisung. Die Transaktionssequenznummer wird bei jeder Zuweisung um eins erhöht.  
  
 Wenn entweder die Datenbankoption `READ_COMMITTED_SNAPSHOT` oder `ALLOW_SNAPSHOT_ISOLATION` auf ON gesetzt ist, werden logische Kopien (Versionen) für alle in der Datenbank erfolgten Datenänderungen aufbewahrt. Jedes Mal, wenn eine Zeile durch eine bestimmte Transaktion geändert wird, speichert die Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] eine Version des zuvor durch ein Commit bestätigten Images der Zeile in `tempdb`. Jede Version wird mit der Transaktionssequenznummer der Transaktion markiert, von der die Änderung vorgenommen wurde. Die Versionen der geänderten Zeilen werden mithilfe einer Linkliste verkettet. Der neueste Zeilenwert wird immer in der aktuellen Datenbank gespeichert und mit den im Versionsspeicher von `tempdb` gespeicherten Zeilenversionen verkettet.  
  
> [!NOTE]  
> Beim Ändern großer Objekte (LOBs, Large Objects) wird nur das geänderte Fragment in den Versionsspeicher in `tempdb` kopiert.  
  
 Die Zeilenversionen werden lang genug aufbewahrt, um den Anforderungen von Transaktionen gerecht zu werden, die unter auf der Zeilenversionsverwaltung basierenden Isolationsstufen ausgeführt werden. [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] verfolgt die früheste nützliche Transaktionssequenznummer und löscht in regelmäßigen Abständen alle Zeilenversionen, die mit Transaktionssequenznummern versehen sind, die unterhalb der frühesten nützlichen Sequenznummer liegen.  
  
 Wenn beide Datenbankoptionen auf OFF gesetzt sind, werden nur die durch Trigger oder MARS-Sitzungen geänderten Zeilen oder die durch ONLINE-Indizierungsvorgänge gelesenen Zeilen in die Versionsverwaltung einbezogen. Diese Zeilenversionen werden jedoch freigegeben, sobald sie nicht mehr benötigt werden. Ein im Hintergrund ausgeführter Thread entfernt in regelmäßigen Abständen alle veralteten Zeilenversionen.  
  
> [!NOTE]  
> Für Transaktionen von kurzer Dauer kann eine Version einer geänderten Zeile im Pufferpool zwischengespeichert werden, ohne dass sie in die Datenträgerdateien der `tempdb`-Datenbank geschrieben wird. Wenn nur ein kurzfristiger Bedarf für die versionsverwaltete Zeile besteht, wird sie einfach aus dem Pufferpool gelöscht und verursacht dadurch nicht unbedingt E/A-Aufwand.  
  
### <a name="behavior-when-reading-data"></a>Verhalten beim Lesen von Daten  
 Wenn unter auf der Zeilenversionsverwaltung basierenden Isolationsstufen ausgeführte Transaktionen Daten lesen, fordern sie keine freigegebenen Sperren (S) für die gelesenen Daten an und blockieren deshalb keine Transaktionen, bei denen Daten geändert werden. Außerdem wird der Aufwand für das Sperren von Ressourcen minimiert, weil nur eine reduzierte Anzahl von Sperren angefordert wird. Die Read Committed-Isolation mit Zeilenversionsverwaltung und die Momentaufnahmeisolation wurden entwickelt, um die Lesekonsistenz der versionsbasierten Daten auf Anweisungsebene bzw. auf Transaktionsebene zu gewährleisten.  
  
 Alle Abfragen, einschließlich Transaktionen, die in auf der Zeilenversionsverwaltung basierenden Isolationsstufen ausgeführt werden, richten Sperren vom Typ Sch-S (Schemastabilität) während der Kompilierung und der Ausführung ein. Daher werden Abfragen gesperrt, wenn eine gleichzeitige Transaktion eine Schemaänderungssperre (Sch-M) für die Tabelle aufrechterhält. Beispielsweise aktiviert ein DDL-Vorgang (Data Definition Language, Datendefinitionssprache) eine Sch-S-Sperre, bevor die Schemainformationen für die Tabelle geändert werden. Abfragetransaktionen, einschließlich der Transaktionen, die eine auf der Zeilenversionsverwaltung basierende Isolationsstufe verwenden, werden beim Anfordern einer Sperre vom Typ Sch-S blockiert. Umgekehrt blockiert eine Abfrage, die eine Sch-S-Sperre aufrechterhält, eine gleichzeitige Transaktion, die versucht, eine Sch-M-Sperre zu errichten.  
  
 Wenn eine Transaktion mithilfe der Momentaufnahmeisolationsstufe gestartet wird, zeichnet die Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] alle aktuell aktiven Transaktionen auf. Wenn die Momentaufnahmetransaktion eine Zeile liest, die über eine Versionskette verfügt, verfolgt [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] diese Kette und ruft die Zeile dort ab, wo sich die Transaktionssequenznummer befindet:  
  
-   Am nächsten zur Sequenznummer der Momentaufnahmetransaktion, die die Zeile liest, jedoch unterhalb dieser Sequenznummer.  
  
-   Nicht in der Liste der beim Start der Momentaufnahmetransaktion aktiven Transaktionen.  
  
 Die von einer Momentaufnahmetransaktion ausgeführten Lesevorgänge rufen die letzte Version jeder Zeile ab, für die zum Startzeitpunkt der Momentaufnahmetransaktion ein Commit erfolgt war. Damit wird ein transaktionskonsistente Momentaufnahme der Daten bereitgestellt, wie sie beim Start der Transaktion vorlagen.  
  
 Read Committed-Transaktionen mit Zeilenversionsverwaltung funktionieren auf sehr ähnliche Weise. Der Unterschied besteht darin, dass die Read Committed-Transaktion beim Auswählen der Zeilenversionen nicht ihre eigene Transaktionssequenznummer verwendet. Jedes Mal, wenn eine Anweisung gestartet wird, liest die Read Committed-Transaktion die letzte Transaktionssequenznummer, die für diese Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] ausgegeben wurde. Das ist die Transaktionssequenznummer, die zum Auswählen der richtigen Zeilenversionen für diese Anweisung verwendet wird. Dadurch können Read Committed-Transaktionen eine Momentaufnahme der Daten sehen, wie sie beim Start jeder Anweisung vorgelegen haben.  
  
> [!NOTE]  
> Obwohl Read Committed-Transaktionen mit Zeilenversionsverwaltung eine im Hinblick auf Transaktionen konsistente Sicht der Daten auf Anweisungsebene bereitstellen, bleiben die von diesem Transaktionstyp generierten Zeilenversionen bzw. die Zeilenversionen, auf die dieser Transaktionstyp zugreift, bis zum Ende der Transaktion erhalten.  
  
### <a name="behavior-when-modifying-data"></a>Verhalten beim Ändern von Daten  
 In einer Read Committed-Transaktion mit Zeilenversionsverwaltung erfolgt das Auswählen der zu aktualisierenden Zeilen durch Verwenden eines Blockierungsscans, bei dem eine Updatesperre (U) für die Daten beim Lesen der Datenwerte bewirkt wird. Das ist dasselbe Verhalten wie bei Read Committed-Transaktionen ohne Zeilenversionsverwaltung. Wenn die Datenzeile nicht dem Updatekriterium entspricht, wird die Updatesperre für diese Zeile aufgehoben, und die nächste Zeile wird gesperrt und gescannt.  
  
 Transaktionen, die mit der Momentaufnahmeisolationsstufe ausgeführt werden, verwenden eine optimistische Vorgehensweise bei der Datenänderung, indem Sperren für Daten aktiviert werden, bevor die Änderung vorgenommen wird, damit Einschränkungen erzwungen werden. Andernfalls werden erst dann Sperren für Daten aktiviert, wenn die Daten geändert werden sollen. Wenn eine Datenzeile dem Updatekriterium entspricht, überprüft die Momentaufnahmetransaktion, dass die Datenzeile nicht durch eine parallele Transaktion geändert wurde, für die nach dem Start der Momentaufnahmetransaktion ein Commit erfolgte. Wenn die Datenzeile außerhalb der Momentaufnahmetransaktion geändert wurde, tritt ein Updatekonflikt auf, und die Momentaufnahmetransaktion wird beendet. Der Updatekonflikt wird von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] behandelt, und es gibt keinerlei Möglichkeit, die Erkennung von Updatekonflikten zu deaktivieren.  
  
> [!NOTE]  
> Updatevorgänge, die mit der Momentaufnahmeisolationsstufe gestartet werden, werden unter der Read Committed-Isolation ausgeführt, wenn die Momentaufnahmetransaktion auf eines der folgenden Elemente zugreift:  
>  
> Eine Tabelle mit einer FOREIGN KEY-Einschränkung.  
>  
> Eine Tabelle, auf die in der FOREIGN KEY-Einschränkung einer anderen Tabelle verwiesen wird.  
>  
> Eine indizierte Sicht, die auf mehrere Tabellen verweist.  
>  
> Allerdings wird der Updatevorgang selbst unter diesen Bedingungen fortgesetzt, um zu überprüfen, dass die Daten nicht durch eine andere Transaktion geändert wurden. Wenn die Daten durch eine andere Transaktion geändert wurden, erkennt die Momentaufnahmetransaktion einen Updatekonflikt und wird beendet.  
  
### <a name="behavior-in-summary"></a>Gesamtverhalten  
 In der folgenden Tabelle werden die Unterschiede zwischen der Momentaufnahmeisolation und der Read Committed-Isolation mit Zeilenversionsverwaltung zusammengefasst.  
  
|Eigenschaft|Read Committed-Isolationsstufe mit Zeilenversionsverwaltung|Momentaufnahmeisolationsstufe|  
|--------------|----------------------------------------------------------|------------------------------|  
|Die Datenbankoption, die auf ON gesetzt sein muss, um die erforderliche Unterstützung zu aktivieren.|READ_COMMITTED_SNAPSHOT|ALLOW_SNAPSHOT_ISOLATION|  
|Wie eine Sitzung den speziellen Typ der Zeilenversionsverwaltung anfordert.|Verwenden Sie die standardmäßige Read Committed-Isolationsstufe, oder führen Sie die SET TRANSACTION ISOLATION LEVEL-Anweisung aus, um die READ COMMITTED-Isolationsstufe anzugeben. Das kann nach dem Start der Transaktion durchgeführt werden.|Erfordert, dass SET TRANSACTION ISOLATION LEVEL zum Angeben der MOMENTAUFNAHMEN-Isolationsstufe vor dem Start der Transaktion ausgeführt wird.|  
|Die von den Anweisungen gelesene Datenversion.|Alle Daten, für die vor dem Start jeder Anweisung ein Commit erfolgte.|Alle Daten, für die vor dem Start jeder Transaktion ein Commit erfolgte.|  
|Wie Updates behandelt werden.|Kehrt von den Zeilenversionen zu den tatsächlichen Daten zurück, um die zu aktualisierenden Zeilen auszuwählen, und verwendet Updatesperren für die ausgewählten Datenzeilen. Aktiviert exklusive Sperren für die tatsächlichen Datenzeilen, die geändert werden sollen. Keine Erkennung von Updatekonflikten.|Verwendet die Zeilenversionen zum Auswählen der zu aktualisierenden Zeilen. Versucht, eine exklusive Sperre für die tatsächliche Datenzeile zu aktivieren, die geändert werden soll. Wenn die Daten durch eine andere Transaktion geändert wurden, tritt ein Updatekonflikt auf, und die Momentaufnahmetransaktion wird beendet.|  
|Erkennung von Updatekonflikten.|Keine.|Integrierte Unterstützung. Kann nicht deaktiviert werden.|  
  
### <a name="row-versioning-resource-usage"></a>Ressourcenverwendung bei der Zeilenversionsverwaltung  
 Das Framework für die Zeilenversionsverwaltung unterstützt die folgenden in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] verfügbaren Funktionen:  
  
-   Trigger  
-   Multiple Active Results Sets (MARS)  
-   Online-Indizierung  
  
 Das Framework für die Zeilenversionsverwaltung unterstützt zudem die folgenden auf der Zeilenversionsverwaltung basierenden Transaktionsisolationsstufen, die standardmäßig nicht aktiviert sind:  
  
-   Wenn für die Datenbankoption `READ_COMMITTED_SNAPSHOT` der Wert ON festgelegt ist, stellen `READ_COMMITTED`-Transaktionen bei Verwendung der Zeilenversionsverwaltung eine Lesekonsistenz auf Anweisungsebene bereit.  
-   Wenn für die Datenbankoption `ALLOW_SNAPSHOT_ISOLATION` der Wert ON festgelegt ist, stellen `SNAPSHOT`-Transaktionen bei Verwendung der Zeilenversionsverwaltung eine Lesekonsistenz auf Transaktionsebene bereit.  
  
 Durch die auf der Zeilenversionsverwaltung basierenden Isolationsstufen wird die Anzahl der von der Transaktion abgerufenen Sperren dadurch reduziert, dass keine freigegebenen Sperren für Lesevorgänge verwendet werden. Auf diese Weise wird die Systemleistung erhöht, da die Anzahl der für die Verwaltung der Sperren verwendeten Ressourcen reduziert wird. Die Leistung wird zudem dadurch erhöht, dass die Anzahl von Sperrungen einer Transaktion durch von anderen Transaktionen angeforderte Sperren verringert wird.  
  
 Auf der Zeilenversionsverwaltung basierende Isolationsstufen erhöhen die von Datenänderungen benötigten Ressourcen. Bei Aktivierung dieser Optionen werden für alle Datenänderungen für die Datenbank Versionen angegeben. Eine Kopie der Daten in dem Zustand vor der Änderung wird in tempdb gespeichert. Dies ist auch dann der Fall, wenn keine aktiven Transaktionen die auf der Zeilenversionsverwaltung basierende Isolation verwenden. Die Daten nach der Änderung enthalten einen Verweis auf die in tempdb gespeicherten Daten, die über eine Versionsangabe verfügen. Im Fall von großen Objekten wird nur ein Teil des geänderten Objekts in tempdb gespeichert.  
  
#### <a name="space-used-in-tempdb"></a>In tempdb verwendeter Speicherplatz  
 tempdb muss für jede Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] über genügend Speicherplatz für die Zeilenversionen verfügen, die für sämtliche Datenbanken in der Instanz generiert wurden. Der Datenbankadministrator muss sicherstellen, dass tempdb über genügend Speicherplatz verfügt, um den Versionsspeicher zu unterstützen. In tempdb befinden sich zwei Versionsspeicher:  
  
-   Der Onlineindexerstellungs-Versionsspeicher wird für Onlineindexerstellungen in allen Datenbanken verwendet.  
-   Der allgemeine Versionsspeicher wird für alle anderen Datenänderungsvorgänge in sämtlichen Datenbanken verwendet.  
  
 Zeilenversionen müssen so lange gespeichert werden, wie eine aktive Transaktion darauf zugreifen muss. Einmal pro Minute entfernt ein Hintergrundthread nicht mehr benötigte Zeilenversionen und gibt so Versionsspeicherplatz in tempdb frei. Eine Transaktion mit langer Ausführungszeit verhindert, dass der Speicherplatz im Versionsspeicher freigegeben werden kann, wenn sie eine der folgenden Bedingungen erfüllt:  
  
-   Sie verwendet die auf der Zeilenversionsverwaltung basierende Isolation.  
-   Sie verwendet Trigger, MARS oder Onlineindexerstellungs-Vorgänge.  
-   Sie generiert Zeilenversionen.  
  
> [!NOTE]  
> Wenn innerhalb einer Transaktion ein Trigger aufgerufen wird, werden die vom Trigger generierten Zeilenversionen bis zum Ende der Transaktion beibehalten, auch wenn die Zeilenversionen nach Abschluss des Triggers nicht mehr benötigt werden. Dies gilt auch für Read Committed-Transaktionen, die Zeilenversionsverwaltung verwenden. Bei diesem Transaktionstyp wird nur für die einzelnen Anweisungen in der Transaktion eine im Hinblick auf Transaktionen konsistente Sicht der Datenbank benötigt. Dies bedeutet, dass die für eine Anweisung in der Transaktion erstellten Zeilenversionen nach Abschluss der Anweisung nicht mehr benötigt werden. Die von den einzelnen Anweisungen in der Transaktion erstellten Zeilenversionen werden jedoch bis zum Abschluss der Transaktion beibehalten.  
  
 Wenn tempdb nicht mehr über genügend Speicherplatz verfügt, erzwingt [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] eine Verkleinerung der Versionsspeicher. Während des Verkleinerungsprozesses werden die Transaktionen mit der längsten Ausführungszeit, die noch keine Zeilenversionen generiert haben, als Opfer gekennzeichnet. Die Meldung 3967 wird im Fehlerprotokoll für jede Opfertransaktion generiert. Wenn eine Transaktion als Opfer gekennzeichnet ist, kann sie die Zeilenversionen im Versionsspeicher nicht mehr lesen. Wenn die Transaktion versucht, Zeilenversionen zu lesen, wird die Meldung 3966 generiert, und es wird ein Rollback für die Transaktion ausgeführt. Ist die Verkleinerung des Prozesses erfolgreich, wird Speicherplatz in tempdb verfügbar. Anderenfalls ist in tempdb nicht mehr genügend Speicherplatz vorhanden, und folgender Fehler tritt auf:  
  
-   Schreibvorgänge werden weiterhin ausgeführt, generieren jedoch keine Versionen. Eine Informationsmeldung (3959) wird im Fehlerprotokoll angezeigt. Die Transaktion, die Daten schreibt, ist jedoch nicht betroffen.  
  
-   Transaktionen, die versuchen, auf Zeilenversionen zuzugreifen, die aufgrund eines vollständigen tempdb-Rollbacks nicht generiert wurden, werden beendet, und der Fehler 3958 wird ausgegeben.  
  
#### <a name="space-used-in-data-rows"></a>In Datenzeilen verwendeter Speicherplatz  
 Jede Datenbankzeile kann am Ende der Zeile bis zu 14 Byte für Zeilenversionsverwaltungs-Informationen nutzen. Zu den Zeilenversionsverwaltungs-Informationen zählen die Transaktionssequenznummer der Transaktion, die den Commit für die Version ausgeführt hat, sowie der Zeiger auf die Zeile mit Versionsangabe. Diese 14 Byte werden hinzugefügt, wenn die Zeile zum ersten Mal geändert wird oder wenn unter einer der folgenden Bedingungen eine neue Zeile eingefügt wird:  
  
-   Die Option `READ_COMMITTED_SNAPSHOT` oder `ALLOW_SNAPSHOT_ISOLATION` ist auf ON festgelegt.  
-   Die Tabelle verfügt über einen Trigger.  
-   Multiple Active Results Sets (MARS) wird verwendet.  
-   Onlineindexerstellungs-Vorgänge werden derzeit für die Tabelle ausgeführt.  
  
 Diese 14 Byte werden aus der Datenbankzeile entfernt, wenn die Zeile zum ersten Mal unter allen der folgenden Bedingungen geändert wird:  
  
-   Die Optionen `READ_COMMITTED_SNAPSHOT` und `ALLOW_SNAPSHOT_ISOLATION` sind auf OFF festgelegt.  
-   Der Trigger ist nicht mehr für die Tabelle vorhanden.  
-   MARS wird nicht verwendet.  
-   Es werden derzeit keine Onlineindexerstellungs-Vorgänge ausgeführt.  
  
 Der Datenbank sollte so viel Speicherplatz zugeordnet werden, dass sie 14 Bytes pro Datenbankzeile aufnehmen kann, falls eine der Funktionen zur Zeilenversionsverwaltung verwendet wird. Das Hinzufügen von Zeilenversionsverwaltungs-Informationen kann Indexseitenteilungen oder die Zuordnung einer neuen Datenseite zur Folge haben, falls auf der aktuellen Seite nicht genügend Speicherplatz verfügbar ist. Beispiel: Wenn die durchschnittliche Zeilenlänge 100 Bytes beträgt, wächst eine vorhandene Tabelle durch die zusätzlichen 14 Bytes um 14 Prozent.  
  
 Durch Verringern des [Füllfaktors](../relational-databases/indexes/specify-fill-factor-for-an-index.md) kann die Fragmentierung der Indexseiten reduziert oder verhindert werden. Zum Anzeigen der Fragmentierungsinformationen für die Daten und Indizes einer Tabelle oder Sicht können Sie [sys.dm_db_index_physical_stats](../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md) verwenden.  
  
#### <a name="space-used-in-large-objects"></a>In großen Objekten verwendeter Speicherplatz  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] unterstützt sechs Datentypen, die lange Zeichenfolgen von bis zu 2 Gigabyte (GB) Länge aufnehmen können: `nvarchar(max)`, `varchar(max)`, `varbinary(max)`, `ntext`, `text` und `image`. Lange Zeichenfolgen, die mithilfe dieser Datentypen gespeichert werden, werden in einer Reihe von Datenfragmenten gespeichert, die mit der Datenzeile verknüpft sind. Zeilenversionsverwaltungs-Informationen werden in sämtlichen Fragmenten gespeichert, die zum Speichern dieser langen Zeichenfolgen verwendet werden. Datenfragmente stellen eine Sammlung von Seiten dar, die für große Objekte in einer Tabelle dediziert sind.  
  
 Wenn einer Datenbank neue große Werte hinzugefügt werden, werden diese mithilfe von maximal 8.040 Byte an Daten pro Fragment zugeordnet. In früheren Versionen von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] wurden bis zu 8.080 Byte an `ntext`-, `text`- oder `image`-Daten pro Fragment gespeichert.  
  
 Vorhandene `ntext`-, `text`- und `image`-Daten großer Objekte (LOB, Large Objects) werden nicht aktualisiert, um Speicherplatz für die Zeilenversionsverwaltungs-Informationen freizugeben, wenn ein Upgrade einer Datenbank von einer früheren Version von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auf [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] durchgeführt wird. Wenn die LOB-Daten jedoch zum ersten Mal geändert werden, wird mit ihnen ein dynamisches Upgrade durchgeführt, um das Speichern von Versionsinformationen zu ermöglichen. Dies ist auch dann der Fall, wenn keine Zeilenversionen generiert werden. Nachdem ein Upgrade mit den LOB-Daten durchgeführt wurde, wird die maximale Byteanzahl, die pro Fragment gespeichert wird von 8.080 auf 8.040 reduziert. Der Upgradeprozess ist dem Löschen des LOB-Werts und dem erneuten Einsetzen desselben Werts gleichwertig. Ein Upgrade der LOB-Daten wird auch dann durchgeführt, wenn nur ein Byte geändert wird. Es handelt sich hierbei um einen einmaligen Vorgang für jede `ntext`-, `text`-, oder `image`-Spalte. Durch jeden Vorgang wird jedoch je nach dem Umfang der LOB-Daten eine hohe Menge an Seitenzuordnungen und E/A-Aktivitäten generiert. Es können zudem viele Protokollierungsaktivitäten generiert werden, sofern die Änderung vollständig protokolliert wird. WRITETEXT- und UPDATETEXT-Vorgänge werden minimal protokolliert, wenn der Datenbankwiederherstellungsmodus auf den Wert FULL festgelegt ist.  
  
 Die `nvarchar(max)`-, `varchar(max)`- und `varbinary(max)`-Datentypen sind in früheren Versionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nicht verfügbar. Aus diesem Grund weisen sie keine Upgradeprobleme auf.  
  
 Es sollte genügend Speicherplatz zugeordnet werden, um dieser Anforderung gerecht zu werden.  
  
#### <a name="monitoring-row-versioning-and-the-version-store"></a>Überwachen der Zeilenversionsverwaltung und des Versionsspeichers  
 Für die Überwachung von Zeilenversionsverwaltungs-, Versionsspeicher- und Momentaufnahmeisolationsprozessen in Bezug auf die Leistung und Probleme stellt [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Tools in Form von dynamische Verwaltungssichten (DMVs, Dynamic Management Views) und Leistungsindikatoren im Systemmonitor von Windows zur Verfügung.  
  
##### <a name="dmvs"></a>DMVs  
 Die folgenden DMVs stellen Informationen zu den aktuellen Systemstatus von tempdb und den Versionsspeicher sowie die Transaktionen bereit, die die Zeilenversionsverwaltung verwenden.  
  
 sys.dm_db_file_space_usage. Gibt Informationen zur Speicherverwendung aller Dateien in der Datenbank zurück. Weitere Informationen finden Sie unter [sys.dm_db_file_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-file-space-usage-transact-sql.md).  
  
 sys.dm_db_session_space_usage. Gibt Aktivität für die Seitenzuordnung und die Zuordnungsaufhebung nach Sitzung für die Datenbank zurück. Weitere Informationen finden Sie unter [sys.dm_db_session_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md).  
  
 sys.dm_db_task_space_usage. Gibt für die Datenbank Aktivitäten zu Seitenzuordnungen und aufgehobenen Seitenzuordnungen nach Tasks zurück. Weitere Informationen finden Sie unter [sys.dm_db_task_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md).  
  
 sys.dm_tran_top_version_generators. Gibt eine virtuelle Tabelle für die Objekte zurück, die die meisten Versionen im Versionsspeicher erzeugen. Hierbei werden die ersten 256 aggregierten Datensatzlängen nach database_id und rowset_id gruppiert. Mithilfe dieser Funktion können Sie die größten Consumer des Versionsspeichers finden. Weitere Informationen finden Sie unter [sys.dm_tran_top_version_generators &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-top-version-generators-transact-sql.md).  
  
 sys.dm_tran_version_store. Gibt eine virtuelle Tabelle zurück, die alle Versionsdatensätze im allgemeinen Versionsspeicher anzeigt. Weitere Informationen finden Sie unter [sys.dm_tran_version_store &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-transact-sql.md).  

 sys.dm_tran_version_store_space_usage. Gibt eine virtuelle Tabelle zurück, die den gesamten in „tempdb“ verwendeten Speicherplatz der Versionsspeicherdatensätze für jede Datenbank anzeigt. Weitere Informationen finden Sie unter [sys.dm_tran_version_store_space_usage &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-version-store-space-usage.md).  

> [!NOTE]  
> Die Ausführung von sys.dm_tran_top_version_generators und sys.dm_tran_version_store kann sehr teuer sein, da beide Funktionen den gesamten Versionsspeicher abfragen, der möglicherweise sehr groß ist.  
> Die Ausführung von sys.dm_tran_version_store_space_usage ist effizient und nicht teuer, da es nicht durch die einzelnen Versionsspeicherdatensätze navigiert und den in „tempdb“ pro Datenbank verwendeten aggregierten Versionsspeicherplatz zurückgibt.
  
 sys.dm_tran_active_snapshot_database_transactions. Gibt eine virtuelle Tabelle für alle aktiven Transaktionen in sämtlichen Datenbanken in der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Instanz zurück, die die Zeilenversionsverwaltung verwenden. Systemtransaktionen werden in dieser DMV nicht angezeigt. Weitere Informationen finden Sie unter [sys.dm_tran_active_snapshot_database_transactions &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md).  
  
 sys.dm_tran_transactions_snapshot. Gibt eine virtuelle Tabelle zurück, die Momentaufnahmen anzeigt, die von den einzelnen Transaktionen erstellt wurden. Die Momentaufnahme enthält die Sequenznummer der aktiven Transaktionen, die die Zeilenversionsverwaltung verwenden. Weitere Informationen finden Sie unter [sys.dm_tran_transactions_snapshot &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-transactions-snapshot-transact-sql.md).  
  
 sys.dm_tran_current_transaction. Gibt eine einzelne Zeile zurück, die auf die Zeilenversionsverwaltung bezogene Statusinformationen der Transaktion in der aktuellen Sitzung anzeigt. Weitere Informationen finden Sie unter [sys.dm_tran_current_transaction &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-current-transaction-transact-sql.md).  
  
 sys.dm_tran_current_snapshot. Gibt eine virtuelle Tabelle zurück, die alle aktiven Transaktionen zum Zeitpunkt des Startens der aktuellen Momentaufnahmeisolation aufführt. Wenn die aktuelle Transaktion die Momentaufnahmeisolation verwendet, gibt diese Funktion keine Zeilen zurück. sys.dm_tran_current_snapshot ähnelt sys.dm_tran_transactions_snapshot, mit dem Unterschied, dass es nur die aktiven Transaktionen für die aktuelle Momentaufnahme zurückgibt. Weitere Informationen finden Sie unter [sys.dm_tran_current_snapshot &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-current-snapshot-transact-sql.md).  
  
##### <a name="performance-counters"></a>Leistungsindikatoren  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Leistungsindikatoren stellen Informationen zur Auswirkung von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Prozessen auf die Systemleistung zur Verfügung. Die folgenden Leistungsindikatoren überwachen tempdb und den Versionsspeicher sowie Transaktionen mithilfe der Zeilenversionsverwaltung. Die Leistungsindikatoren sind im SQLServer:Transaktionen-Leistungsobjekt enthalten.  
  
 **Freier Speicherplatz in tempdb (KB)** : Überwacht die Menge des freien Speicherplatzes in Kilobyte (KB), der in der tempdb-Datenbank zur Verfügung steht. Es muss genügend freier Speicherplatz in tempdb zur Verfügung stehen, um den Versionsspeicher zu bearbeiten, der die Momentaufnahmeisolation unterstützt.  
  
 Die folgende Formel ermöglicht eine grobe Schätzung der Größe des Versionsspeichers. Bei lange andauernden Transaktionen kann es sich als sinnvoll erweisen, die Generierungs- und Cleanuprate zu überwachen, um die maximale Größe des Versionsspeichers einzuschätzen.  
  
 [size of common version store] = 2 * [version store data generated per minute] \* [longest running time (minutes) of the transaction]  
  
 Die längste Ausführungszeit von Transaktionen sollte Onlineindexerstellungs-Vorgänge nicht einschließen. Da diese Vorgänge bei sehr großen Tabellen viel Zeit in Anspruch nehmen können, verwenden Onlineindexerstellungs-Vorgänge einen separaten Versionsspeicher. Die ungefähre Größe des Onlineindexerstellungs-Versionsspeichers entspricht der Menge der in der Tabelle geänderten Daten, einschließlich aller Indizes, während die Onlineindexerstellung aktiviert ist.  
  
 **Versionsspeichergröße (KB)** : Überwacht die Größe in KB aller Versionsspeicher. Mithilfe dieser Informationen können Sie die Menge des Speicherplatzes bestimmen, die in der tempdb-Datenbank für den Versionsspeicher benötigt wird. Das Überwachen dieser Indikatoren über einen gewissen Zeitraum ermöglicht eine hilfreiche Schätzung des zusätzlich für tempdb benötigten Speicherplatzes.  
  
 `Version Generation rate (KB/s)`. Überwacht die Versionsgenerierungsrate, in KB pro Sekunde, in allen Versionsspeichern.  
  
 `Version Cleanup rate (KB/s)`. Überwacht die Versionscleanuprate, in KB pro Sekunde, in allen Versionsspeichern.  
  
> [!NOTE]  
> Die Informationen aus Versionsgenerierungsrate (KB/s) und Versionscleanuprate (KB/s) können zur Vorhersage von Speicherplatzanforderungen für tempdb verwendet werden.  
  
 **Anzahl der Versionsspeichereinheiten**: Überwacht die Anzahl der Versionsspeichereinheiten.  
  
 **Erstellen von Versionsspeichereinheiten**: Überwacht die Gesamtzahl der Versionsspeichereinheiten, die für das Speichern von Zeilenversionen erstellt wurden, seitdem die Instanz gestartet wurde.  
  
 **Abschneiden von Versionsspeichereinheiten**: Überwacht die Gesamtzahl der Versionsspeichereinheiten, die abgeschnitten wurden, seitdem die Instanz gestartet wurde. Eine Versionsspeichereinheit wird abgeschnitten, wenn [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] bestimmt, dass keine der Versionszeilen, die in der Versionsspeichereinheit gespeichert sind, für die Ausführung aktiver Transaktionen benötigt wird.  
  
 **Updatekonfliktquote**: Überwacht die Quote von Update-Momentaufnahmetransaktionen, die Updatekonflikte aufweisen, im Verhältnis zur Gesamtzahl der Update-Momentaufnahmetransaktionen.  
  
 **Längste Transaktionsausführungszeit**: Überwacht die längste Ausführungszeit in Sekunden aller Transaktionen, die die Zeilenversionsverwaltung verwenden. Hiermit kann bestimmt werden, ob eine Transaktion über eine nicht vertretbare Zeitdauer ausgeführt wird.  
  
 **Transaktionen**: Überwacht die Gesamtzahl aktiver Transaktionen. Dieser Leistungsindikator schließt keine Systemtransaktionen ein.  
  
 `Snapshot Transactions`. Überwacht die Gesamtzahl aktiver Momentaufnahmetransaktionen.  
  
 `Update Snapshot Transactions`. Überwacht die Gesamtzahl aktiver Momentaufnahmetransaktionen, die Updatevorgänge ausführen.  
  
 `NonSnapshot Version Transactions`. Überwacht die Gesamtzahl aktiver Nichtmomentaufnahme-Transaktionen, die Versionsdatensätze generieren.  
  
> [!NOTE]  
> Die Summe von Update-Momentaufnahmetransaktionen und NonSnapshot-Versionstransaktionen stellt die Gesamtzahl der Transaktionen dar, die an der Versionsgenerierung teilnehmen. Die Differenz zwischen Momentaufnahmetransaktionen und Update-Momentaufnahmetransaktionen gibt die Anzahl der schreibgeschützten Momentaufnahmetransaktionen an.  
  
### <a name="row-versioning-based-isolation-level-example"></a>Beispiel für eine auf der Zeilenversionsverwaltung basierende Isolationsstufe  
 Die folgenden Beispiele zeigen die Unterschiede im Verhalten zwischen Momentaufnahmeisolationstransaktionen und Transaktionen, bei denen ein Commit vor dem Lesevorgang ausgeführt werden muss, und die die Zeilenversionsverwaltung verwenden.  
  
#### <a name="a-working-with-snapshot-isolation"></a>A. Arbeiten mit der Momentaufnahmeisolation  
 In diesem Beispiel liest eine Transaktion, die unter Momentaufnahmeisolation ausgeführt wird, Daten, die anschließend von einer anderen Transaktion geändert werden. Die Momentaufnahmetransaktion blockiert nicht den Updatevorgang, der von der anderen Transaktion ausgeführt wird, liest auch weiterhin Daten aus der versionsspezifischen Zeile und ignoriert die Datenänderung. Wenn die Momentaufnahmetransaktion jedoch versucht, die Daten zu ändern, die bereits von der anderen Transaktion geändert wurden, generiert die Momentaufnahmetransaktion einen Fehler und wird beendet.  
  
 Für Sitzung 1:  
  
```sql  
USE AdventureWorks2016;  
GO  
  
-- Enable snapshot isolation on the database.  
ALTER DATABASE AdventureWorks2016  
    SET ALLOW_SNAPSHOT_ISOLATION ON;  
GO  
  
-- Start a snapshot transaction  
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
GO  
  
BEGIN TRANSACTION;  
    -- This SELECT statement will return  
    -- 48 vacation hours for the employee.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Für Sitzung 2:  
  
```sql  
USE AdventureWorks2016;  
GO  
  
-- Start a transaction.  
BEGIN TRANSACTION;  
    -- Subtract a vacation day from employee 4.  
    -- Update is not blocked by session 1 since  
    -- under snapshot isolation shared locks are  
    -- not requested.  
    UPDATE HumanResources.Employee  
        SET VacationHours = VacationHours - 8  
        WHERE BusinessEntityID = 4;  
  
    -- Verify that the employee now has 40 vacation hours.  
    SELECT VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Für Sitzung 1:  
  
```sql  
    -- Reissue the SELECT statement - this shows  
    -- the employee having 48 vacation hours.  The  
    -- snapshot transaction is still reading data from  
    -- the versioned row.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Für Sitzung 2:  
  
```sql  
-- Commit the transaction; this commits the data  
-- modification.  
COMMIT TRANSACTION;  
GO  
```  
  
 Für Sitzung 1:  
  
```sql  
    -- Reissue the SELECT statement - this still   
    -- shows the employee having 48 vacation hours  
    -- even after the other transaction has committed  
    -- the data modification.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
  
    -- Because the data has been modified outside of the  
    -- snapshot transaction, any further data changes to   
    -- that data by the snapshot transaction will cause   
    -- the snapshot transaction to fail. This statement   
    -- will generate a 3960 error and the transaction will   
    -- terminate.  
    UPDATE HumanResources.Employee  
        SET SickLeaveHours = SickLeaveHours - 8  
        WHERE BusinessEntityID = 4;  
  
-- Undo the changes to the database from session 1.   
-- This will not undo the change from session 2.  
ROLLBACK TRANSACTION  
GO  
```  
  
#### <a name="b-working-with-read-committed-using-row-versioning"></a>B. Arbeiten mit READ COMMITTED unter Verwendung der Zeilenversionsverwaltung  
 In diesem Beispiel wird eine Transaktion, bei der ein Commit vor dem Lesevorgang ausgeführt werden muss und die Zeilenversionsverwaltung verwendet wird, gleichzeitig mit einer anderen Transaktion ausgeführt. Die Transaktion, bei der ein Commit vor dem Lesevorgang ausgeführt werden muss, verhält sich anders als eine Momentaufnahmetransaktion. Ebenso wie eine Momentaufnahmetransaktion liest die Transaktion, bei der ein Commit vor dem Lesevorgang ausgeführt werden muss, versionsspezifische Zeilen, nachdem die andere Transaktion Daten geändert hat. Im Gegensatz zu einer Momentaufnahmetransaktion gilt für die Transaktion, bei der ein Commit vor dem Lesevorgang ausgeführt werden muss, jedoch Folgendes:  
  
-   Sie liest die geänderten Daten, nachdem die andere Transaktion ein Commit der Datenänderungen vorgenommen hat.  
-   Sie ist in der Lage, die von der anderen Transaktion bearbeiteten Daten zu aktualisieren, was der Momentaufnahmetransaktion nicht möglich ist.  
  
 Für Sitzung 1:  
  
```sql  
USE AdventureWorks2016;  -- Or any earlier version of the AdventureWorks database.  
GO  
  
-- Enable READ_COMMITTED_SNAPSHOT on the database.  
-- For this statement to succeed, this session  
-- must be the only connection to the AdventureWorks2016  
-- database.  
ALTER DATABASE AdventureWorks2016  
    SET READ_COMMITTED_SNAPSHOT ON;  
GO  
  
-- Start a read-committed transaction  
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;  
GO  
  
BEGIN TRANSACTION;  
    -- This SELECT statement will return  
    -- 48 vacation hours for the employee.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Für Sitzung 2:  
  
```sql  
USE AdventureWorks2016;  
GO  
  
-- Start a transaction.  
BEGIN TRANSACTION;  
    -- Subtract a vacation day from employee 4.  
    -- Update is not blocked by session 1 since  
    -- under read-committed using row versioning shared locks are  
    -- not requested.  
    UPDATE HumanResources.Employee  
        SET VacationHours = VacationHours - 8  
        WHERE BusinessEntityID = 4;  
  
    -- Verify that the employee now has 40 vacation hours.  
    SELECT VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Für Sitzung 1:  
  
```sql  
    -- Reissue the SELECT statement - this still shows  
    -- the employee having 48 vacation hours.  The  
    -- read-committed transaction is still reading data   
    -- from the versioned row and the other transaction   
    -- has not committed the data changes yet.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
```  
  
 Für Sitzung 2:  
  
```sql  
-- Commit the transaction.  
COMMIT TRANSACTION;  
GO  
```  
  
 Für Sitzung 1:  
  
```sql  
    -- Reissue the SELECT statement which now shows the   
    -- employee having 40 vacation hours.  Being   
    -- read-committed, this transaction is reading the   
    -- committed data. This is different from snapshot  
    -- isolation which reads from the versioned row.  
    SELECT BusinessEntityID, VacationHours  
        FROM HumanResources.Employee  
        WHERE BusinessEntityID = 4;  
  
    -- This statement, which caused the snapshot transaction   
    -- to fail, will succeed with read-committed using row versioning.  
    UPDATE HumanResources.Employee  
        SET SickLeaveHours = SickLeaveHours - 8  
        WHERE BusinessEntityID = 4;  
  
-- Undo the changes to the database from session 1.   
-- This will not undo the change from session 2.  
ROLLBACK TRANSACTION;  
GO  
```  
  
### <a name="enabling-row-versioning-based-isolation-levels"></a>Aktivieren von auf der Zeilenversionsverwaltung basierenden Isolationsstufen  
 Datenbankadministratoren steuern die Einstellungen für die Zeilenversionsverwaltung auf Datenbankebene über die Datenbankoptionen `READ_COMMITTED_SNAPSHOT` und `ALLOW_SNAPSHOT_ISOLATION` in der ALTER DATABASE-Anweisung.  
  
 Wenn die `READ_COMMITTED_SNAPSHOT`-Datenbankoption auf ON festgelegt ist, werden die zur Unterstützung der Option verwendeten Mechanismen unmittelbar aktiviert. Wenn die READ_COMMITTED_SNAPSHOT-Option festgelegt wird, wird in der Datenbank nur die Verbindung zugelassen, die den `ALTER DATABASE`-Befehl ausführt. So lange ALTER DATABASE nicht abgeschlossen ist, darf keine andere offene Verbindung in der Datenbank bestehen. Die Datenbank muss sich nicht im Einzelbenutzermodus befinden.  
  
 Die folgende [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisung aktiviert `READ_COMMITTED_SNAPSHOT`:  
  
```sql  
ALTER DATABASE AdventureWorks2016  
    SET READ_COMMITTED_SNAPSHOT ON;  
```  
  
 Wenn für die `ALLOW_SNAPSHOT_ISOLATION`-Datenbankoption der Wert ON festgelegt ist, generiert die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Instanz so lange keine Zeilenversionen für geänderte Daten, bis alle aktiven Transaktionen abgeschlossen sind, durch die Daten in der Datenbank geändert werden. Wenn aktive Änderungstransaktionen vorhanden sind, legt [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] den Status der Option auf `PENDING_ON` fest. Wenn alle Änderungstransaktionen abgeschlossen sind, wird der Status der Option zu ON geändert. Die Benutzer können keine Momentaufnahmetransaktion in dieser Datenbank starten, bis die Option vollständig ON ist. Die Datenbank übergibt einen PENDING_OFF-Status, wenn der Datenbankadministrator die `ALLOW_SNAPSHOT_ISOLATION`-Option auf OFF festlegt.  
  
 Die folgende [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisung aktiviert ALLOW_SNAPSHOT_ISOLATION:  
  
```sql  
ALTER DATABASE AdventureWorks2016  
    SET ALLOW_SNAPSHOT_ISOLATION ON;  
```  
  
 In der folgenden Tabelle werden die Statusmöglichkeiten der ALLOW_SNAPSHOT_ISOLATION-Option aufgeführt und beschrieben. Der Zugriff von Benutzern auf Daten in der Datenbank wird durch das Verwenden von ALTER DATABASE mit der ALLOW_SNAPSHOT_ISOLATION-Option nicht blockiert.  
  
|Status der Momentaufnahmeisolationsumgebung der aktuellen Datenbank|BESCHREIBUNG|  
|----------------------------------------------------------------|-----------------|  
|OFF|Die Unterstützung von Momentaufnahmeisolationstransaktionen ist nicht aktiviert. Momentaufnahmeisolationtransaktionen sind nicht zulässig.|  
|PENDING_ON|Die Unterstützung von Momentaufnahmeisolationstransaktionen befindet sich in einem Übergangsstatus (von OFF nach ON). Offene Transaktionen müssen abgeschlossen werden.<br /><br /> Momentaufnahmeisolationtransaktionen sind nicht zulässig.|  
|EIN|Die Unterstützung von Momentaufnahmeisolationstransaktionen ist aktiviert.<br /><br /> Momentaufnahmeisolationtransaktionen sind zulässig.|  
|PENDING_OFF|Die Unterstützung von Momentaufnahmeisolationstransaktionen befindet sich in einem Übergangsstatus (von ON nach OFF).<br /><br /> Momentaufnahmetransaktionen, die nach diesem Zeitpunkt gestartet werden, können nicht auf die Datenbank zugreifen. Updatetransaktionen sind ist in dieser Datenbank noch durch die Versionsverwaltung eingeschränkt. Vorhandene Momentaufnahmetransaktionen können immer noch problemlos auf die Datenbank zugreifen. Der PENDING_OFF-Status wird erst OFF, wenn alle Momentaufnahmetransaktionen abgeschlossen sind, die zu dem Zeitpunkt, als der Momentaufnahmeisolationsstatus der Datenbank ON war, aktiviert waren.|  
  
 Verwenden Sie die `sys.databases`-Katalogsicht, um den Status der beiden Datenbankoptionen zur Zeilenversionsverwaltung zu bestimmen.  
  
 Alle Updates von Benutzertabellen sowie bestimmte Updates von Systemtabellen, die in master und msdb gespeichert sind, generieren Zeilenversionen.  
  
 Die `ALLOW_SNAPSHOT_ISOLATION`-Option wird in den Datenbanken master und msdb automatisch auf ON festgelegt und kann nicht deaktiviert werden.  
  
 Benutzer können die `READ_COMMITTED_SNAPSHOT`-Option in master, tempdb und msdb nicht auf ON festlegen.  
  
### <a name="using-row-versioning-based-isolation-levels"></a>Verwenden von auf Zeilenversionsverwaltung basierenden Isolationsstufen  
 Das Framework für die Zeilenversionsverwaltung ist in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] immer aktiviert und wird von mehreren Funktionen verwendet. Es stellt nicht nur auf Zeilenversionsverwaltung basierende Isolationsstufen bereit, sondern wird auch zur Unterstützung von Änderungen verwendet, die an Triggern und MARS-Sitzungen (Multiple Active Result Sets) vorgenommen werden; außerdem dient es zur Unterstützung von Datenlesevorgängen für ONLINE-Indexvorgänge.  
  
 Auf der Zeilenversionsverwaltung basierende Isolationsstufen werden auf der Datenbankebene aktiviert. Alle Anwendungen, die auf Objekte aus aktivierten Datenbanken zugreifen, können mithilfe der folgenden Isolationsstufen Abfragen ausführen:  
  
-   READ COMMITTED (Commit vor dem Lesevorgang) mit Zeilenversionsverwaltung, indem die `READ_COMMITTED_SNAPSHOT`-Datenbankoption auf `ON` (wie im folgenden Codebeispiel gezeigt) festgelegt wird:  
  
    ```sql  
    ALTER DATABASE AdventureWorks2016  
        SET READ_COMMITTED_SNAPSHOT ON;  
    ```  
  
     Wenn die Datenbank für `READ_COMMITTED_SNAPSHOT` aktiviert ist, verwenden alle Abfragen, die unter der Isolationsstufe READ COMMITTED ausgeführt werden, Zeilenversionsverwaltung. Dies bedeutet, dass die Lesevorgänge die Updatevorgänge nicht blockieren.  
  
-   Die Momentaufnahmeisolation durch Festlegen der Datenbankoption `ALLOW_SNAPSHOT_ISOLATION` auf `ON`, wie im folgenden Codebeispiel gezeigt:  
  
    ```sql  
    ALTER DATABASE AdventureWorks2016  
        SET ALLOW_SNAPSHOT_ISOLATION ON;  
    ```  
  
     Eine Transaktion, die unter Momentaufnahmeisolation ausgeführt wird, kann auf Tabellen in der Datenbank zugreifen, die für die Snapshotfunktion aktiviert wurden. Wenn auf Tabellen zugegriffen werden soll, die nicht für Momentaufnahmen aktiviert wurden, muss die Isolationsstufe geändert werden. Das folgende Codebeispiel zeigt z. B. eine `SELECT`-Anweisung, die während der Ausführung unter einer Momentaufnahmetransaktion zwei Tabellen verknüpft. Eine der Tabellen gehört zu einer Datenbank, in der Momentaufnahmeisolation nicht aktiviert ist. Wenn die `SELECT`-Anweisung unter Momentaufnahmeisolation ausgeführt wird, ist die Ausführung nicht erfolgreich.  
  
    ```sql  
    SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
    BEGIN TRAN  
        SELECT t1.col5, t2.col5  
            FROM Table1 as t1  
            INNER JOIN SecondDB.dbo.Table2 as t2  
                ON t1.col1 = t2.col2;  
    ```  
  
     Das folgende Codebeispiel zeigt die gleiche `SELECT`-Anweisung, die so bearbeitet wurde, dass die Transaktionsisolationsstufe in READ COMMITTED geändert wurde. Durch diese Änderung wird die `SELECT`-Anweisung erfolgreich ausgeführt.  
  
    ```sql  
    SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
    BEGIN TRAN  
        SELECT t1.col5, t2.col5  
            FROM Table1 as t1  
            WITH (READCOMMITTED)  
            INNER JOIN SecondDB.dbo.Table2 as t2  
                ON t1.col1 = t2.col2;  
    ```  
  
#### <a name="limitations-of-transactions-using-row-versioning-based-isolation-levels"></a>Einschränkungen von Transaktionen, die auf Zeilenversionsverwaltung basierende Isolationsstufen verwenden  
 Berücksichtigen Sie die folgenden Einschränkungen, wenn Sie mit auf Zeilenversionsverwaltung basierenden Isolationsstufen arbeiten:  
  
-   `READ_COMMITTED_SNAPSHOT` kann in tempdb, msdb oder master nicht aktiviert werden.  
-   Globale temporäre Tabellen werden in tempdb gespeichert. Wenn auf globale temporäre Tabellen in einer Momentaufnahmetransaktion zugegriffen wird, muss einer der folgenden Vorgänge erfolgen:  
    -   Festlegen der `ALLOW_SNAPSHOT_ISOLATION`-Datenbankoption auf ON in tempdb.  
    -   Verwenden eines Isolationshinweises zum Ändern der Isolationsstufe für die Anweisung.  
-   Momentaufnahmetransaktionen erzeugen einen Fehler, wenn Folgendes zutrifft:  
    -   Eine Datenbank erhält nach dem Start der Momentaufnahmetransaktion, jedoch vor dem Zugriff auf die Datenbank durch die Momentaufnahmetransaktion einen Schreibschutz.  
    -   Beim Zugriff auf Objekte aus mehreren Datenbanken wurde ein Datenbankstatus so geändert, dass die Datenbankwiederherstellung nach dem Start einer Momentaufnahmetransaktion aufgetreten ist, jedoch vor dem Zugriff auf die Datenbank durch die Momentaufnahmetransaktion. Beispiel: Die Datenbank wurde auf OFFLINE und dann auf ONLINE festgelegt, auf automatisches Schließen und Öffnen oder auf Trennen und Anfügen.  
-   Verteilte Transaktionen, z. B. Abfragen in verteilten partitionierten Datenbanken, werden unter Momentaufnahmeisolation nicht unterstützt.  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] speichert nicht mehrere Versionen von Systemmetadaten. DDL-Anweisungen (Data Definition Language) für Tabellen und andere Datenbankobjekte (Indizes, Sichten, Datentypen, gespeicherte Prozeduren und CLR-Funktionen) verändern Metadaten. Wenn eine DDL-Anweisung ein Objekt ändert, bewirkt jeder gleichzeitige Verweis auf das Objekt unter Momentaufnahmeisolation, dass die Momentaufnahmetransaktion einen Fehler erzeugt. Für READ COMMITTED-Transaktionen gilt diese Einschränkung nicht, wenn die READ_COMMITTED_SNAPSHOT-Datenbankoption auf ON festgelegt wurde.  
  
     Ein Datenbankadministrator führt z. B. die folgende `ALTER INDEX`-Anweisung aus.  
  
    ```sql  
    USE AdventureWorks2016;  
    GO  
    ALTER INDEX AK_Employee_LoginID  
        ON HumanResources.Employee REBUILD;  
    GO  
    ```  
  
     Für alle Momentaufnahmetransaktionen, die während der Ausführung der `ALTER INDEX`-Anweisung aktiviert sind, wird ein Fehler ausgegeben, wenn versucht wird, auf die `HumanResources.Employee`-Tabelle zu verweisen, nachdem die `ALTER INDEX`-Anweisung ausgeführt wurde. READ COMMITTED-Transaktionen, die Zeilenversionsverwaltung verwenden, sind nicht betroffen.  
  
    > [!NOTE]  
    > BULK INSERT-Operationen können Änderungen an den Metadaten der Zieltabelle verursachen (z. B. beim Deaktivieren von Einschränkungsprüfungen). Sollte dies der Fall sein, schlagen gleichzeitige Momentaufnahmeisolationstransaktion fehl, die auf Tabellen mit BULK INSERT zugreifen.  
  
   
## <a name="customizing-locking-and-row-versioning"></a>Anpassen von Sperren und Zeilenversionsverwaltung  
  
### <a name="customizing-the-lock-time-out"></a>Anpassen des Timeouts für Sperren  
 Wenn eine [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Instanz einer Transaktion keine Sperre für eine Ressource erteilen kann, da eine andere Transaktion bereits eine widersprüchliche Sperre für diese Ressource besitzt, wird die erste Transaktion blockiert, während sie darauf wartet, dass die vorhandene Sperre aufgehoben wird. Standardmäßig gibt es keinen obligatorischen Timeoutzeitraum und keine Möglichkeit, im Voraus zu testen, ob eine Ressource gesperrt ist, außer zu versuchen, auf die Daten zuzugreifen (und eventuell auf unbestimmte Zeit blockiert zu werden).  
  
> [!NOTE]  
> Verwenden Sie die dynamische Verwaltungssicht **sys.dm_os_waiting_tasks** in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], um zu bestimmen, ob und wodurch ein Prozess blockiert wird. Verwenden Sie die gespeicherte Systemprozedur **sp_who** in früheren Versionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 Mithilfe der `LOCK_TIMEOUT`-Einstellung kann eine Anwendung eine Zeitspanne festlegen, die angibt, wie lange eine Anweisung maximal auf eine blockierte Ressource wartet. Wenn eine Anweisung länger wartet als in der LOCK_TIMEOUT-Einstellung angegeben, wird die blockierte Anweisung automatisch abgebrochen und die Fehlermeldung 1222 (`Lock request time-out period exceeded`) an die Anwendung zurückgegeben. Für eine Transaktion, in der diese Anweisung enthalten ist, wird jedoch von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] kein Rollback ausgeführt, und sie wird nicht abgebrochen. Die Anwendung muss daher über einen Fehlerhandler verfügen, der die Fehlermeldung 1222 identifizieren kann. Ist dies nicht der Fall, kann die Anwendung fortfahren, ohne zu erkennen, dass eine einzelne Anweisung in einer Transaktion abgebrochen wurde, und Fehler können auftreten, da nachfolgende Anweisungen in der Transaktion möglicherweise von der nicht ausgeführten Anweisung abhängen.  
  
 Durch das Implementieren eines Fehlerhandlers, der die Fehlermeldung 1222 auffängt, kann eine Anwendung die Timeoutbedingung bearbeiten und Abhilfemaßnahmen ergreifen, wie etwa die vormals blockierte Anforderung automatisch erneut zu senden oder für die gesamte Transaktion einen Rollback auszuführen.  
  
 Führen Sie die `@@LOCK_TIMEOUT`-Funktion aus, um die aktuelle `LOCK_TIMEOUT`-Einstellung zu bestimmen:  
  
```sql  
SELECT @@lock_timeout;  
GO  
```  
  
### <a name="customizing-transaction-isolation-level"></a>Anpassen der Isolationsstufe von Transaktionen  
 READ COMMITTED ist die Standardisolationsstufe für [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]. Wenn für eine bestimmte Anwendung eine andere Isolationsstufe erforderlich ist, kann eine der folgenden Methoden verwendet werden, um die entsprechende Isolationsstufe anzugeben:  
  
-   Ausführen der [SET TRANSACTION ISOLATION LEVEL](../t-sql/statements/set-transaction-isolation-level-transact-sql.md)-Anweisung.  
-   In ADO.NET-Anwendungen, von denen der verwaltete Namespace System.Data.SqlClient verwendet wird, kann mithilfe der SqlConnection.BeginTransaction-Methode die Option *IsolationLevel* angeben werden.  
-   Anwendungen, die ADO verwenden, können die `Autocommit Isolation Levels`-Eigenschaft festlegen.  
-   Beim Starten einer Transaktion können Anwendungen, für die OLE DB verwendet wird, „ITransactionLocal::StartTransaction“ aufrufen, wobei *isoLevel* auf die gewünschte Transaktionsisolationsstufe festgelegt wird. Beim Angeben der Isolationsstufe im Autocommitmodus können Anwendungen, von denen OLE DB verwendet wird, die DBPROPSET_SESSION-Eigenschaft DBPROP_SESS_AUTOCOMMITISOLEVELS auf die gewünschte Transaktionsisolationsstufe festlegen.  
-   In Anwendungen, für die ODBC verwendet wird, kann mithilfe von „SQLSetConnectAttr“ das SQL_COPT_SS_TXN_ISOLATION-Attribut festlegt werden.  
  
Wenn eine Isolationsstufe angegeben ist, richtet sich das Sperrverhalten aller Abfragen und DML-Anweisungen (Data Manipulation Language, Datenbearbeitungssprache) der [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Sitzung nach dieser Isolationsstufe. Die Isolationsstufe bleibt gültig, bis die Sitzung beendet wird oder bis eine andere Isolationsstufe festgelegt wird.  
  
Im folgenden Beispiel wird die `SERIALIZABLE`-Isolationsstufe festgelegt:  
  
```sql  
USE AdventureWorks2016;  
GO  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;  
GO  
BEGIN TRANSACTION;  
SELECT BusinessEntityID  
    FROM HumanResources.Employee;  
GO  
```  
  
Die Isolationsstufe kann, falls notwendig, für einzelne Abfrage- oder DML-Anweisungen überschrieben werden, indem ein Hinweis auf Tabellenebene angegeben wird. Das Angeben eines Hinweises auf Tabellenebene wirkt sich nicht auf andere Anweisungen in der Sitzung aus. Es wird empfohlen, dass Hinweise auf Tabellenebene zum Ändern des Standardverhaltens nur dann verwendet werden, wenn dies absolut notwendig ist.  
  
[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] muss möglicherweise beim Lesen von Metadaten selbst dann Sperren einrichten, wenn die Isolationsstufe auf eine Stufe festgelegt ist, für die zum Lesen von Daten keine freigegebenen Sperren erforderlich sind. Eine in der READ UNCOMMITTED-Isolationsstufe ausgeführte Transaktion richtet beim Lesen von Daten beispielsweise keine freigegebenen Sperren ein, kann jedoch zu einem gewissen Zeitpunkt Sperren anfordern, wenn eine Systemkatalogsicht gelesen wird. Dies bedeutet, dass eine READ UNCOMMITTED-Transaktion beim Abfragen einer Tabelle Blockierungen verursachen kann, wenn eine andere Transaktion gleichzeitig die Metadaten der Tabelle ändert.  
  
Wenn Sie die derzeit für die Transaktion festgelegte Isolationsstufe ermitteln möchten, verwenden Sie die `DBCC USEROPTIONS`-Anweisung, wie im nachfolgenden Beispiel gezeigt. Das hier aufgeführte Resultset weicht möglicherweise von dem auf Ihrem System angezeigten Resultset ab.  
  
```sql  
USE AdventureWorks2016;  
GO  
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;  
GO  
DBCC USEROPTIONS;  
GO  
```  
  
[!INCLUDE[ssResult](../includes/ssresult-md.md)]  
  
```
Set Option                   Value  
---------------------------- -------------------------------------------  
textsize                     2147483647  
language                     us_english  
dateformat                   mdy  
datefirst                    7  
...                          ...  
Isolation level              repeatable read  
 
(14 row(s) affected)   
 
DBCC execution completed. If DBCC printed error messages, contact your system administrator.
```  
  
### <a name="locking-hints"></a>Sperrhinweise  
 Sperrhinweise können für einzelne Tabellenverweise in den SELECT-, INSERT-, UPDATE- und DELETE-Anweisungen angegeben werden. Diese Hinweise geben den Typ der Sperre oder die Zeilenversionsverwaltung an, die die [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]-Instanz für die Tabellendaten verwendet. Sperrhinweise auf Tabellenebene können verwendet werden, wenn eine präzisere Steuerung der Sperrentypen für ein Objekt notwendig wird. Diese Sperrhinweise überschreiben die aktuelle Transaktionsisolationsstufe für diese Sitzung.  
  
 Weitere Informationen zu bestimmten Sperrhinweisen und ihrem Verhalten finden Sie unter [Tabellenhinweise &#40;Transact-SQL&#41;](../t-sql/queries/hints-transact-sql-table.md).  
  
> [!NOTE]  
> Der [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] wählt so gut wie immer die richtige Sperrebene aus. Es wird empfohlen, dass Sperrhinweise auf Tabellenebene zur Änderung des Standardsperrverhaltens nur dann verwendet werden, wenn dies notwendig ist. Wenn eine Sperrstufe nicht zugelassen wird, kann dies negative Auswirkungen auf die Parallelität haben.  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] muss möglicherweise beim Lesen von Metadaten selbst dann Sperren aktivieren, wenn eine SELECT-Anweisung mit einem Sperrhinweis verarbeitet wird, der beim Lesen von Daten Anforderungen für freigegebene Sperren verhindert. Eine `SELECT`-Anweisung, die den `NOLOCK`-Hinweis verwendet, aktiviert beim Lesen von Daten z.B. keine freigegebenen Sperren, kann jedoch manchmal Sperren anfordern, wenn eine Systemkatalogsicht gelesen wird. Dies bedeutet, dass es möglich ist, eine `SELECT`-Anweisung zu blockieren, die `NOLOCK` verwendet.  
  
 Wenn – wie im folgenden Beispiel gezeigt – die Isolationsstufe für Transaktionen als `SERIALIZABLE` festgelegt wurde und der `NOLOCK`-Sperrhinweis auf Tabellenebene mit der `SELECT`-Anweisung verwendet wird, werden keine Schlüsselbereichssperren angewendet, die in der Regel zur Aufrechterhaltung der Serialisierbarkeit von Transaktionen verwendet werden.  
  
```sql  
USE AdventureWorks2016;  
GO  
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;  
GO  
BEGIN TRANSACTION;  
GO  
SELECT JobTitle  
    FROM HumanResources.Employee WITH (NOLOCK);  
GO  
  
-- Get information about the locks held by   
-- the transaction.  
SELECT    
        resource_type,   
        resource_subtype,   
        request_mode  
    FROM sys.dm_tran_locks  
    WHERE request_session_id = @@spid;  
  
-- End the transaction.  
ROLLBACK;  
GO  
```  
  
 Die einzige Sperre, die angewendet wird und auf `HumanResources.Employee` verweist, ist eine Sperre des Typs Sch-S (Schemastabilität). In diesem Fall kann die Serialisierbarkeit nicht mehr garantiert werden.  
  
 In [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] kann die `LOCK_ESCALATION`-Option von `ALTER TABLE` Tabellensperren als unerwünscht festlegen und HoBT-Sperren für partitionierte Tabellen aktivieren. Diese Option ist kein Sperrhinweis, kann jedoch verwendet werden, um die Sperrenausweitung zu reduzieren. Weitere Informationen finden Sie unter [ALTER TABLE &#40;Transact-SQL&#41;](../t-sql/statements/alter-table-transact-sql.md).  
  
###  <a name="customizing-locking-for-an-index"></a><a name="Customize"></a> Anpassen der Sperren für einen Index  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] verwendet eine dynamische Sperrstrategie, die für Abfragen in den meisten Fällen automatisch die am besten geeignete Granularität der Sperren auswählt. Es empfiehlt sich, die Standardeinstellungen der Sperrebenen, in denen die Seiten- und Zeilensperre aktiviert ist, nicht zu überschreiben, es sei denn, die Zugriffsmuster für Tabellen oder Indizes sind bekannt und konsistent und es liegt ein Ressourcenkonflikt vor, der behoben werden muss. Das Überschreiben einer Sperrebene kann den gleichzeitigen Zugriff auf eine Tabelle oder einen Index signifikant einschränken. Wenn beispielsweise in einer großen Tabelle, auf die viele Benutzer zugreifen, nur Sperren auf Tabellenebene angegeben werden, kann dies zu Engpässen führen, da die Benutzer die Aufhebung der Sperre auf Tabellenebene abwarten müssen, bevor sie auf die Tabelle zugreifen können.  
  
 Wenn die Zugriffsmuster gut bekannt und konsistent sind, kann das Untersagen von Seiten- oder Zeilensperren in einigen Fällen sinnvoll sein. So verwendet beispielsweise eine Datenbankanwendung eine Nachschlagetabelle, die wöchentlich in einem Batchverarbeitungsprozess aktualisiert wird. Gleichzeitige Leser greifen mit einer freigegebenen Sperre (S) auf die Tabelle zu. Das wöchentliche Batchupdate greift mit einer exklusiven Sperre (X) auf die Tabelle zu. Durch das Deaktivieren der Seiten- und Zeilensperrung für die Tabelle wird der Sperraufwand unter der Woche reduziert, indem Leser mithilfe freigegebener Tabellensperren gleichzeitig auf die Tabelle zugreifen können. Wenn der Batchauftrag ausgeführt wird, kann er das Update effizient ausführen, da er eine exklusive Tabellensperre erhält.  
  
 Die Deaktivierung der Seiten- und Zeilensperre kann, muss jedoch nicht akzeptiert werden, da das wöchentliche Batchupdate die gleichzeitigen Leser während des Updates daran hindert, auf die Tabelle zuzugreifen. Wenn durch den Batchauftrag nur einige Zeilen oder Seiten geändert werden, können Sie die Sperrebene ändern, sodass Sperren auf Zeilen- oder Seitenebene zugelassen werden. Dadurch können andere Sitzungen aus der Tabelle lesen, ohne diese zu sperren. Wenn der Batchauftrag sehr viele Updates enthält, ist es möglicherweise die beste Methode, eine exklusive Sperre für die Tabelle zu setzen, um sicherzustellen, dass der Auftrag effizient ausgeführt wird.  
  
 Gelegentlich kann es zu einem Deadlock kommen, wenn zwei gleichzeitig ausgeführte Vorgänge Sperren für die gleiche Tabelle abrufen und dann blockieren, da beide die Seite sperren müssen. Wenn keine Zeilensperren zugelassen werden, wird erzwungen, dass einer der Vorgänge wartet, um den Deadlock zu vermeiden.  
  
 Die Granularität der Sperren für einen Index kann mithilfe der Anweisungen `CREATE INDEX` und `ALTER INDEX` festgelegt werden. Die Einstellungen für die Sperre werden sowohl auf die Indexseiten als auch auf die Tabellenseiten angewendet. Darüber hinaus können die Anweisungen `CREATE TABLE` und `ALTER TABLE` dazu verwendet werden, die Granularität der Sperren für `PRIMARY KEY`- und `UNIQUE`-Einschränkungen festzulegen. Aus Gründen der Abwärtskompatibilität kann auch die gespeicherte Systemprozedur `sp_indexoption` zum Festlegen der Granularität verwendet werden. Verwenden Sie zum Anzeigen der aktuellen Sperroption für einen bestimmten Index die `INDEXPROPERTY`-Funktion. Es ist möglich, Sperren auf Seitenebene, auf Zeilenebene oder eine Kombination von Sperren auf Seiten- und Zeilenebene für einen bestimmten Index nicht zuzulassen.  
  
|Nicht zugelassene Sperren|Indexzugriff durch|  
|----------------------|-----------------------|  
|Seitenebene|Sperren auf Zeilen- und Tabellenebene|  
|Zeilenebene|Sperren auf Seiten- und Tabellenebene|  
|Seiten- und Zeilenebene|Sperren auf Tabellenebene|  
  
##  <a name="advanced-transaction-information"></a><a name="Advanced"></a> Weiterführende Themen zu Transaktionen  
  
### <a name="nesting-transactions"></a>Schachteln von Transaktionen  
 Explizite Transaktionen können geschachtelt werden. Auf diese Weise sollen in erster Linie Transaktionen in gespeicherten Prozeduren unterstützt werden, die sowohl von einem Prozess, der sich bereits in einer Transaktion befindet, als auch von Prozessen, die keine aktiven Transaktionen aufweisen, aufgerufen werden können.  
  
 Im folgenden Beispiel wird dargestellt, wie geschachtelte Transaktionen verwendet werden sollten. Die *TransProc*-Prozedur erzwingt eine Transaktion, unabhängig vom Transaktionsmodus des Prozesses, der die Prozedur ausführt. Wird *TransProc* aufgerufen, wenn eine Transaktion aktiv ist, wird die geschachtelte Transaktion in *TransProc* überwiegend ignoriert, und für ihre `INSERT`-Anweisungen wird ein Commit oder Rollback ausgeführt, je nachdem, welche endgültige Aktion für die äußere Transaktion durchgeführt wurde. Wenn `TransProc` von einem Prozess ausgeführt wird, der keine ausstehende Transaktion aufweist, führt `COMMIT TRANSACTION` am Ende der Prozedur letztendlich einen Commit für die `INSERT`-Anweisungen aus.  
  
```sql  
SET QUOTED_IDENTIFIER OFF;  
GO  
SET NOCOUNT OFF;  
GO  
CREATE TABLE TestTrans(Cola INT PRIMARY KEY,  
               Colb CHAR(3) NOT NULL);  
GO  
CREATE PROCEDURE TransProc @PriKey INT, @CharCol CHAR(3) AS  
BEGIN TRANSACTION InProc  
INSERT INTO TestTrans VALUES (@PriKey, @CharCol)  
INSERT INTO TestTrans VALUES (@PriKey + 1, @CharCol)  
COMMIT TRANSACTION InProc;  
GO  
/* Start a transaction and execute TransProc. */  
BEGIN TRANSACTION OutOfProc;  
GO  
EXEC TransProc 1, 'aaa';  
GO  
/* Roll back the outer transaction, this will  
   roll back TransProc's nested transaction. */  
ROLLBACK TRANSACTION OutOfProc;  
GO  
EXECUTE TransProc 3,'bbb';  
GO  
/* The following SELECT statement shows only rows 3 and 4 are   
   still in the table. This indicates that the commit  
   of the inner transaction from the first EXECUTE statement of  
   TransProc was overridden by the subsequent rollback. */  
SELECT * FROM TestTrans;  
GO  
```  
  
 [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] ignoriert das Ausführen von Commits für innere Transaktionen. Für die Transaktion wird entweder ein Commit oder Rollback ausgeführt, je nachdem, welche Aktion am Ende der äußersten Transaktion durchgeführt wird. Bei der Ausführung eines Commits für die äußere Transaktion wird für die inneren geschachtelten Transaktionen ebenfalls ein Commit ausgeführt. Bei der Ausführung eines Rollbacks für die äußere Transaktion wird auch für alle inneren Transaktionen ein Rollback ausgeführt, unabhängig davon, ob für jede einzelne der inneren Transaktionen ein Commit ausgeführt wurde oder nicht.  
  
 Jeder Aufruf von `COMMIT TRANSACTION` oder `COMMIT WORK` gilt für die zuletzt ausgeführte `BEGIN TRANSACTION`. Wenn die `BEGIN TRANSACTION`-Anweisungen geschachtelt sind, bezieht sich eine `COMMIT`-Anweisung nur auf die letzte geschachtelte Transaktion, also die innerste Transaktion. Selbst wenn sich eine `COMMIT TRANSACTION` *transaction_name*-Anweisung in einer geschachtelten Transaktion auf den Transaktionsnamen der äußeren Transaktion bezieht, wird der Commit ausschließlich für die innerste Transaktion ausgeführt.  
  
 Es ist nicht zulässig, dass der *transaction_name*-Parameter einer `ROLLBACK TRANSACTION`-Anweisung auf die inneren Transaktionen einer Reihe von benannten geschachtelten Transaktionen verweist. *transaction_name* kann nur auf den Transaktionsnamen der äußersten Transaktion verweisen. Wenn eine ROLLBACK TRANSACTION-*transaction_name*-Anweisung, die den Namen der äußeren Transaktion verwendet, auf einer beliebigen Ebene einer Reihe geschachtelter Transaktionen ausgeführt wird, wird für alle geschachtelten Transaktionen ein Rollback ausgeführt. Wenn eine `ROLLBACK WORK`- oder `ROLLBACK TRANSACTION`-Anweisung ohne Angabe des *transaction_name*-Parameters auf einer beliebigen Ebene einer Reihe von geschachtelten Transaktionen ausgeführt wird, wird für alle geschachtelten Transaktionen, einschließlich der äußersten Transaktion, ein Rollback ausgeführt.  
  
 Die `@@TRANCOUNT`-Funktion zeichnet die aktuelle Schachtelungsebene der Transaktion auf. Jede `BEGIN TRANSACTION`-Anweisung erhöht `@@TRANCOUNT` um den Wert 1. Jede `COMMIT TRANSACTION`- oder `COMMIT WORK`-Anweisung verringert `@@TRANCOUNT` um den Wert 1. Bei einer `ROLLBACK WORK`- oder `ROLLBACK TRANSACTION`-Anweisung ohne Transaktionsnamen wird für alle geschachtelten Transaktionen ein Rollback ausgeführt und `@@TRANCOUNT` auf 0 reduziert. Bei einer `ROLLBACK TRANSACTION`-Anweisung, die den Transaktionsnamen der äußersten Transaktion in einer Reihe geschachtelter Transaktionen verwendet, wird ein Rollback für alle geschachtelten Transaktionen ausgeführt und `@@TRANCOUNT` auf 0 reduziert. Wenn Sie nicht sicher sind, ob eine Transaktion bereits begonnen hat, können Sie mit `SELECT @@TRANCOUNT` ermitteln, ob der Wert 1 oder höher beträgt. Wenn `@@TRANCOUNT` gleich 0 ist, hat noch keine Transaktion begonnen.  
  
### <a name="using-bound-sessions"></a>Verwenden von gebundenen Sitzungen  
 Gebundene Sitzungen vereinfachen die Koordination zwischen zahlreichen Aktionen, die auf demselben Server ausgeführt werden. Sie ermöglichen, dass mehrere Sitzungen gemeinsam dieselben Transaktionen und Sperren nutzen und ohne Sperrkonflikte mit denselben Daten arbeiten können. Gebundene Sitzungen können aus mehreren Sitzungen in derselben Anwendung oder aus mehreren Anwendungen mit getrennten Sitzungen erstellt werden.  
  
 Soll eine Sitzung an einer gebundenen Sitzung beteiligt werden, muss sie `sp_getbindtoken` oder `srv_getbindtoken` (über Open Data Services) aufrufen, um ein Bindungstoken abzurufen. Ein Bindungstoken ist eine Zeichenfolge, die jede gebundene Transaktion eindeutig kennzeichnet. Das Bindungstoken wird dann an die anderen Sitzungen gesendet, die an die aktuelle Sitzung gebunden werden sollen. Die anderen Sitzungen werden durch Aufrufen von **sp_bindsession** mithilfe des Bindungstokens, das sie von der ersten Sitzung empfangen haben, an die Transaktion gebunden.  
  
> [!NOTE]  
> Eine Sitzung muss über eine aktive Benutzertransaktion verfügen, damit `sp_getbindtoken` oder `srv_getbindtoken` erfolgreich ausgeführt werden kann.  
  
 Bindungstoken müssen durch den Anwendungscode, der die erste Sitzung herstellt, an den Anwendungscode gesendet werden, der eine nachfolgende Sitzung an die erste Sitzung bindet. Es gibt keine [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisung oder API-Funktion, die eine Anwendung verwenden kann, um das Bindungstoken für eine Transaktion abzurufen, die von einem anderen Prozess gestartet wurde. Nachfolgend werden verschiedene Methoden zum Übertragen von Bindungstokens aufgeführt:  
  
-   Wenn alle Sitzungen vom selben Anwendungsprozess initiiert werden, können die Bindungstoken im globalen Speicher gespeichert oder als Parameter an Funktionen übergeben werden.  
  
-   Wenn die Sitzungen jedoch von separaten Anwendungsprozessen initiiert werden, können die Bindungstoken mithilfe der prozessübergreifenden Kommunikation (IPC, Interprocess Communication), wie z. B. von Remoteprozeduraufrufen (RPCs, Remote Procedure Calls) oder DDE (Dynamic Data Exchange), übertragen werden.  
  
-   Bindungstoken können in einer Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] in einer Tabelle gespeichert werden, die von die Bindung zur ersten Sitzung herstellenden Prozessen gelesen werden kann.  
  
 Es kann jeweils nur eine Sitzung in einer Menge von gebundenen Sitzungen aktiv sein. Wenn eine Sitzung eine Anweisung auf der Instanz ausführt oder auf Ergebnisse von der Instanz wartet, können keine anderen gebundenen Sitzungen auf die Instanz zugreifen, bis die aktuelle Sitzung die aktuelle Anweisung vollständig verarbeitet hat oder abbricht. Ist die Instanz mit der Verarbeitung einer Anweisung einer anderen gebundenen Sitzung ausgelastet, zeigt eine Fehlermeldung an, dass der Transaktionsbereich verwendet wird und der Zugriffsversuch der Sitzung später wiederholt werden kann.  
  
 Beim Binden von Sitzungen behält jede Sitzung ihre eigene Isolationsstufeneinstellung bei. Das Verwenden von SET TRANSACTION ISOLATION LEVEL zum Ändern der Isolationsstufeneinstellung einer Sitzung wirkt sich nicht auf die Einstellung anderer gebundener Sitzungen aus.  
  
#### <a name="types-of-bound-sessions"></a>Typen von gebundenen Sitzungen  
 Gebundene Sitzungen lassen sich in lokale und verteilte gebundene Sitzungen unterteilen.  
  
-   **Lokale gebundene Sitzungen**  
    Ermöglichen, dass gebundene Sitzungen den Transaktionsbereich einer einzelnen Instanz von [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] gemeinsam nutzen.  
  
-   **Verteilte gebundene Sitzungen**  
    Ermöglichen, dass gebundene Sitzungen dieselbe Transaktion auf zwei oder mehreren Instanzen gemeinsam nutzen, bis für die gesamte Transaktion mithilfe von [!INCLUDE[msCoName](../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC) ein Commit oder Rollback ausgeführt wird.  
  
 Verteilte gebundene Sitzungen werden nicht durch ein Bindungstoken in Form einer Zeichenfolge gekennzeichnet, sondern durch numerische IDs für verteilte Transaktionen. Wenn eine gebundene Sitzung an einer lokalen Transaktion beteiligt ist und mit `SET REMOTE_PROC_TRANSACTIONS ON` einen Remoteprozeduraufruf (RPC) auf einem Remoteserver ausführt, wird die lokale gebundene Transaktion automatisch von MS DTC zu einer verteilten gebundenen Transaktion heraufgestuft und eine MS DTC-Sitzung gestartet.  
  
#### <a name="when-to-use-bound-sessions"></a>Verwendung von gebundenen Sitzungen  
 In früheren Versionen von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] wurden gebundene Sitzungen hauptsächlich zum Entwickeln erweiterter gespeicherter Prozeduren verwendet, die [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisungen für den Prozess ausführen mussten, der sie aufgerufen hat. Wenn der aufrufende Prozess ein Bindungstoken als Parameter an die erweiterte gespeicherte Prozedur übergibt, ermöglicht dies der Prozedur, den Transaktionsbereich des aufrufenden Prozesses mitzunutzen. Dadurch wird die erweiterte gespeicherte Prozedur in den aufrufenden Prozess integriert.  
  
 Im [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] sind mithilfe von CLR geschriebene gespeicherte Prozeduren sicherer, skalierbarer und stabiler als erweiterte gespeicherte Prozeduren. CLR-gespeicherte Prozeduren verwenden nicht `sp_bindsession`, sondern das **SqlContext**-Objekt, um sich dem Kontext der aufrufenden Sitzung anzuschließen.  
  
 Gebundene Sitzungen können zum Entwickeln von dreistufigen Anwendungen verwendet werden. Geschäftsabläufe werden hierbei in getrennte Programme integriert, die gemeinsam für eine einzelne Geschäftstransaktion zuständig sind. Diese Programme müssen hinsichtlich der Koordination ihres Zugriffs auf die Datenbank sehr sorgfältig codiert werden. Da die beiden Sitzungen die Sperren gemeinsam nutzen, dürfen die beiden Programme dieselben Daten nicht gleichzeitig ändern. Zu einem gegebenen Zeitpunkt darf jeweils nur eine Sitzung als Teil der Transaktion Änderungen vornehmen – ein paralleles Ausführen von Vorgängen ist ausgeschlossen. Die Transaktion kann nur an bestimmten, gut definierten Zwischenergebnispunkten zwischen Sitzungen wechseln, z. B. wenn alle DML-Anweisungen abgeschlossen und deren Ergebnisse abgerufen wurden.  
  
### <a name="coding-efficient-transactions"></a>Codieren effizienter Transaktionen  
 Transaktionen sollten so kurz wie möglich gehalten werden. Wenn eine Transaktion gestartet wird, muss ein Datenbank-Managementsystem (Database Management System, DBMS) viele Ressourcen bis zum Ende der Transaktion bereitstellen, um die ACID-Eigenschaften der Transaktion zu schützen. Wenn Daten verändert werden, müssen die zu ändernden Zeilen durch exklusive Sperren geschützt werden, die verhindern, dass andere Transaktionen die Zeilen lesen. Diese exklusiven Sperren müssen so lange aufrechterhalten werden, bis für die Transaktion ein Commit oder Rollback ausgeführt wird. Abhängig von den Einstellungen der Isolationsstufen von Transaktionen können `SELECT`-Anweisungen Sperren einrichten, die bis zum Ausführen eines Commits oder Rollbacks für die Transaktion aufrechterhalten werden müssen. Vor allem bei Systemen mit zahlreichen Benutzern müssen Transaktionen so kurz wie möglich gehalten werden, um die Wahrscheinlichkeit zu reduzieren, dass bei gleichzeitigen Verbindungen Sperrkonflikte für Ressourcen auftreten. Lang andauernde, ineffiziente Transaktionen sind bei wenigen Benutzern möglicherweise nicht problematisch, in einem System mit Tausenden von Benutzern jedoch inakzeptabel. Ab [!INCLUDE[ssSQL14](../includes/sssql14-md.md)][!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] werden verzögerte dauerhafte Transaktionen unterstützt. Verzögerte dauerhafte Transaktionen gewährleisten keine Dauerhaftigkeit. Weitere Informationen finden Sie im Thema [Transaktionsdauerhaftigkeit](../relational-databases/logs/control-transaction-durability.md).  
  
#### <a name="coding-guidelines"></a><a name="guidelines"></a> Richtlinien für das Codieren  
 Im Folgenden sind Richtlinien für das Codieren von effizienten Transaktionen aufgeführt:  
  
-   Verzichten Sie auf Benutzereingaben während einer Transaktion.  
    Sorgen Sie dafür, dass alle notwendigen Eingaben von den Benutzern vor Beginn der Transaktion vorgenommen werden. Wenn zusätzliche Benutzereingaben während einer Transaktion notwendig sind, führen Sie für die aktuelle Transaktion einen Rollback aus, und starten Sie die Transaktion neu, nachdem die Benutzereingaben erfolgt sind. Selbst wenn Benutzer sofort reagieren, ist die menschliche Reaktionszeit bedeutend langsamer als die Geschwindigkeit von Computern. Alle Ressourcen, die von der Transaktion beansprucht werden, sind für besonders lange Zeit belegt, was zu Blockierungsproblemen führen kann. Wenn Benutzer nicht reagieren, bleibt die Transaktion aktiv und sperrt so lange wichtige Ressourcen, bis der Benutzer reagiert. Dies kann Minuten, sogar Stunden dauern.  
  
-   Öffnen Sie nach Möglichkeit keine Transaktion während des Durchsuchens von Daten.  
    Transaktionen sollten erst dann gestartet werden, wenn alle vorhergehenden Datenanalysen abgeschlossen sind.  
  
-   Achten Sie darauf, dass eine Transaktion so kurz wie möglich ist.  
    Wenn Sie wissen, welche Änderungen vorgenommen werden müssen, starten Sie eine Transaktion, führen Sie die Änderungsanweisungen aus, und führen Sie unmittelbar im Anschluss einen Commit oder Rollback aus. Öffnen Sie die Transaktion erst, wenn es erforderlich ist.  
  
-   Sie sollten für schreibgeschützte Abfragen gegebenenfalls eine auf Zeilenversionsverwaltung basierende Isolationsstufe verwenden, um die Möglichkeit von Blockierungen zu reduzieren.  
  
-   Setzen Sie die niedrigeren Isolationsstufen von Transaktionen sinnvoll ein.  
    Viele Anwendungen können ganz leicht so codiert werden, dass die Isolationsstufe, bei der ein Commit vor dem Lesevorgang ausgeführt sein muss, für die Transaktion verwendet wird. Nicht alle Transaktionen erfordern die Isolationsstufe SERIALIZABLE.  
  
-   Setzen Sie die niedrigen Optionen der Cursorparallelität, wie etwa die vollständige Parallelität, sinnvoll ein.  
    Wenn es in einem System relativ unwahrscheinlich ist, dass Updates gleichzeitig vorgenommen werden, kann der Aufwand, der gelegentlich für die Fehlerbehandlung entsteht, wenn Daten nach einem Lesevorgang von einem anderen Benutzer geändert werden, bedeutend geringer sein als der Aufwand, der durch das konsequente Sperren von Zeilen bei jedem Lesen entsteht.  
  
-   Während einer Transaktion sollte auf so wenige Daten wie möglich zugegriffen werden.  
    Dadurch wird die Anzahl der gesperrten Zeilen gesenkt und Konflikte zwischen Transaktionen vermieden.  
    
-   Vermeiden Sie wenn möglich einschränkende Sperrhinweise wie HOLDLOCK. 
    Hinweise wie HOLDLOCK oder eine SERIALIZABLE-Isolationsstufe können dazu führen, dass Prozesse auch bei gemeinsamen Sperren warten und die Parallelität dadurch einschränken.

-   Vermeiden Sie die Verwendung impliziter Transaktionen. Implizite Transaktionen können aufgrund ihrer Merkmale unvorhersehbares Verhalten einführen. Weitere Informationen finden Sie unter [Implizite Transaktionen und Vermeiden von Parallelitäts- und Ressourcenproblemen](#implicit-transactions-and-avoiding-concurrency-and-resource-problems).

-   Entwerfen Sie Indizes mit reduziertem [Füllfaktor](indexes/specify-fill-factor-for-an-index.md). Ein reduzierter Füllfaktor kann Sie dabei unterstützen, die Fragmentierung von Indexseiten zu vermeiden oder zu senken. Damit können auch Suchzeiten für Indizes reduziert werden, insbesondere wenn diese von einem Datenträger abgerufen werden. Zum Anzeigen der Fragmentierungsinformationen für die Daten und Indizes einer Tabelle oder Sicht können Sie sys.dm_db_index_physical_stats verwenden. 
  
#### <a name="implicit-transactions-and-avoiding-concurrency-and-resource-problems"></a>Implizite Transaktionen und Vermeiden von Parallelitäts- und Ressourcenproblemen  
 Wenn Sie Parallelitäts- und Ressourcenprobleme vermeiden möchten, sollten implizite Transaktionen sorgfältig verwaltet werden. Bei impliziten Transaktionen wird durch die nächste [!INCLUDE[tsql](../includes/tsql-md.md)]-Anweisung nach `COMMIT` oder `ROLLBACK` automatisch eine neue Transaktion gestartet. Dadurch kann eine neue Transaktion geöffnet werden, während die Anwendung Daten durchsucht oder sogar wenn Eingaben des Benutzers erforderlich sind. Nach Abschluss der letzten Transaktion, die zum Schutz von Datenänderungen erforderlich ist, sollten Sie die impliziten Transaktionen deaktivieren, bis erneut eine Transaktion benötigt wird, um Datenänderungen zu schützen. Auf diese Weise kann [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] den Autocommitmodus verwenden, während die Anwendung Daten durchsucht und Benutzereingaben vorgenommen werden.  
  
 Wenn die Momentaufnahme-Isolationsstufe aktiviert ist, obwohl eine neue Transaktion keine Sperren beibehält, verhindert außerdem eine Transaktion mit langer Ausführungszeit, dass die alten Versionen aus `tempdb` entfernt werden.  
  
### <a name="managing-long-running-transactions"></a>Verwalten lang andauernder Transaktionen  
 Eine *Transaktion mit langer Ausführungszeit* ist eine aktive Transaktion, für die kein Commit bzw. Rollback rechtzeitig ausgeführt wurde. Bei Transaktionen, deren Beginn und Ende vom Benutzer gesteuert werden, kann es vorkommen, dass der Benutzer eine Transaktion startet und dann seinen Arbeitsplatz verlässt, während die Transaktion auf eine Reaktion des Benutzers wartet. Dies ist z. B. eine typische Ursache für eine lang andauernde Transaktion.  
  
 Eine Transaktion mit langer Ausführungszeit kann für eine Datenbank schwerwiegende Probleme nach sich ziehen:  
  
-   Wenn eine Serverinstanz heruntergefahren wird, nachdem die aktive Transaktion zahlreiche Änderungen vorgenommen hat, für die kein Commit ausgeführt wurde, kann die Wiederherstellungsphase beim nachfolgenden Neustart erheblich länger dauern als durch die Serverkonfigurationsoption **Wiederherstellungsintervall** bzw. durch die `ALTER DATABASE ... SET TARGET_RECOVERY_TIME`-Option angegeben. Durch diese Option wird die Frequenz aktiver bzw. indirekter Prüfpunkte gesteuert. Weitere Informationen zu Prüfpunkttypen finden Sie unter [Datenbankprüfpunkte &#40;SQL Server&#41;](../relational-databases/logs/database-checkpoints-sql-server.md).  
  
-   Obwohl durch eine wartende Transaktion möglicherweise nur sehr wenige Protokolldaten generiert werden, wird die Protokollkürzung auf unbestimmte Zeit aufgehalten. Dies führt dazu, dass das Transaktionsprotokoll anwächst und möglicherweise irgendwann voll ist. Wenn das Transaktionsprotokoll voll ist, kann die Datenbank keine weiteren Updates mehr ausführen. Weitere Informationen finden Sie im [Handbuch zur Architektur und Verwaltung von Transaktionsprotokollen in SQL Server](../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md), unter [Problembehandlung bei einem vollständigen Transaktionsprotokoll &#40;SQL Server-Fehler 9002&#41;](../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md) sowie unter [Das Transaktionsprotokoll &#40;SQL Server&#41;](../relational-databases/logs/the-transaction-log-sql-server.md).  
  
#### <a name="discovering-long-running-transactions"></a>Ermitteln von Transaktionen mit langer Ausführungszeit  
 Verwenden Sie eine der folgenden Optionen, um nach lang andauernden Transaktionen zu suchen:  
  
-   **sys.dm_tran_database_transactions**  
  
    Diese dynamische Verwaltungssicht gibt Informationen zu Transaktionen auf Datenbankebene zurück. Bei einer Transaktion mit langer Ausführungszeit gehören der Zeitpunkt des ersten Protokolldatensatzes (**database_transaction_begin_time**), der aktuelle Status der Transaktion (**database_transaction_state**)und die Protokollfolgenummer (Log Sequence Number, LSN) des ersten Datensatzes im Transaktionsprotokoll (**database_transaction_begin_lsn**) zu den Spalten von besonderem Interesse.  
  
    Weitere Informationen finden Sie unter [sys.dm_tran_database_transactions &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md).  
  
-   `DBCC OPENTRAN`  
  
    Mithilfe dieser Anweisung können Sie die Benutzer-ID des Transaktionsbesitzers identifizieren. Auf diese Weise können Sie die Quelle der Transaktion ermitteln und die Transaktion ordnungsgemäß beenden (durch ein Commit anstelle eines Rollbacks). Weitere Informationen finden Sie unter [DBCC OPENTRAN &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-opentran-transact-sql.md).  
  
#### <a name="stopping-a-transaction"></a>Beenden einer Transaktion  
 Unter Umständen müssen Sie die KILL-Anweisung ausführen. Verwenden Sie diese Anweisung jedoch sehr vorsichtig, besonders wenn gerade kritische Prozesse ausgeführt werden. Weitere Informationen finden Sie unter [KILL &#40;Transact-SQL&#41;](../t-sql/language-elements/kill-transact-sql.md).  
  
##  <a name="additional-reading"></a><a name="Additional_Reading"></a> Zusätzliches Lesematerial   
[Mehraufwand der Zeilenversionsverwaltung](/archive/blogs/sqlserverstorageengine/overhead-of-row-versioning)   
[Erweiterte Ereignisse](../relational-databases/extended-events/extended-events.md)   
[sys.dm_tran_locks &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)     
[Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)      
[Dynamische Verwaltungssichten und Funktionen in Verbindung mit Transaktionen &#40;Transact-SQL&#41;](../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)