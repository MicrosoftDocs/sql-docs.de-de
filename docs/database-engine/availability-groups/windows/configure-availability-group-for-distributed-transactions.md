---
title: Konfigurieren von verteilten Transaktionen für Verfügbarkeitsgruppen
description: 'In diesem Artikel wird beschrieben, wie Sie verteilte Transaktionen für Datenbanken in einer Always On-Verfügbarkeitsgruppe konfigurieren können. '
ms.custom: seodec18
ms.date: 02/06/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- cross-database transactions [SQL Server]
- transactions [database mirroring]
- Availability Groups [SQL Server], interoperability
- troubleshooting [SQL Server], cross-database transactions
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: c05da95541e728d981745d43f4da864c2e8b07a8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343552"
---
# <a name="configure-distributed-transactions-for-an-always-on-availability-group"></a>Konfigurieren von verteilten Transaktionen für Always On-Verfügbarkeitsgruppen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[SQL2017](../../../includes/sssql17-md.md)] unterstützt alle verteilten Transaktionen, einschließlich Datenbanken in einer Verfügbarkeitsgruppe. In diesem Artikel erfahren Sie, wie Sie eine Verfügbarkeitsgruppe für verteilte Transaktionen konfigurieren.  

Um verteilte Transaktionen gewährleisten zu können, muss die Verfügbarkeitsgruppe so konfiguriert sein, dass Datenbanken als Ressourcenmanager verteilter Transaktionen registriert werden.  

>[!NOTE]
>[!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] Service Pack 2 und höher stellt die vollständige Unterstützung für verteilte Transaktionen in Verfügbarkeitsgruppen bereit. In früheren [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)]-Versionen vor Service Pack 2 werden datenbankübergreifende verteilte Transaktionen nicht unterstützt, wenn sie eine Datenbank in einer Verfügbarkeitsgruppe enthalten. Bei diesen Transaktionen werden nur Datenbanken auf der gleichen SQL Server-Instanz verwendet. Diese Einschränkung besteht in [!INCLUDE[SQL2017](../../../includes/sssql17-md.md)] nicht. 
>
>Die Einrichtungsschritte für [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] sind identisch mit denen für [!INCLUDE[SQL2017](../../../includes/sssql17-md.md)].

Bei einer verteilten Transaktion arbeiten Clientanwendungen mit dem Microsoft Distributed Transaction Coordinator (MS DTC oder auch DTC) zusammen, um Transaktionskonsistenz über mehrere Datenquellen hinweg zu gewährleisten. Der DTC ist ein Dienst für Betriebssysteme, die auf Windows Server basieren. Bei einer verteilten Transaktion agiert der DTC als *Transaktionskoordinator*. Normalerweise agiert eine SQL Server-Instanz als *Ressourcenmanager*. Wenn sich eine Datenbank in einer Verfügbarkeitsgruppe befindet, benötigt sie ihren eigenen Ressourcenmanager. 

In [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] werden verteilte Transaktionen für Datenbanken in einer Verfügbarkeitsgruppe nicht verhindert – selbst wenn die Verfügbarkeitsgruppe nicht für verteilte Transaktionen konfiguriert ist. Wenn eine Verfügbarkeitsgruppe jedoch nicht für verteilte Transaktionen konfiguriert ist, kann es vorkommen, dass das Failover in manchen Situationen fehlschlägt. Insbesondere neue [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]-Instanz des primären Replikats ist möglicherweise nicht in der Lage, Transaktionsergebnisse vom DTC abzurufen. Um der [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]-Instanz nach einem Failover das Abrufen der Ergebnisse unsicherer Transaktionen vom DTC zu ermöglichen, konfigurieren Sie die Verfügbarkeitsgruppe für verteilte Transaktionen. 

DTC ist nicht an der Verarbeitung von Verfügbarkeitsgruppen beteiligt, es sei denn, eine Datenbank ist auch Mitglied eines Failoverclusters. Innerhalb einer Verfügbarkeitsgruppe wird die Konsistenz zwischen Replikaten von der Verfügbarkeitsgruppenlogik verwaltet: Das primäre Replikat schließt den Commit nicht ab und bestätigt den Commit an die aufrufende Funktion, bis das sekundäre Replikat bestätigt hat, dass die Protokolldatensätze im permanenten Speicher dauerhaft gespeichert wurden. Erst dann deklariert das primäre Replikat die Transaktion als abgeschlossen. Im asynchronen Modus wird nicht gewartet, bis das sekundäre Replikat die Bestätigung durchführt, und es besteht explizit die Wahrscheinlichkeit, dass eine geringe Datenmenge verloren geht.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie eine Verfügbarkeitsgruppe so konfigurieren, dass verteilte Transaktionen unterstützt werden, müssen die folgenden Voraussetzungen erfüllt sein:

* Alle Instanzen von [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)], die an verteilten Transaktionen teilnehmen, müssen unter [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] oder höher laufen.

* Verfügbarkeitsgruppen müssen auf Windows Server 2016 oder Windows Server 2012 R2 ausgeführt werden. Für Windows Server 2012 R2 müssen Sie das Update in KB3090973 installieren, das unter [https://support.microsoft.com/kb/3090973](https://support.microsoft.com/kb/3090973) verfügbar ist.  

## <a name="create-an-availability-group-for-distributed-transactions"></a>Erstellen einer Verfügbarkeitsgruppe für verteilte Transaktionen

Konfigurieren Sie eine Verfügbarkeitsgruppe so, dass sie verteilte Transaktionen unterstützt. Legen Sie für die Verfügbarkeitsgruppe fest, dass sich jede Datenbank als Ressourcenmanager registrieren kann. In diesem Artikel wird erläutert, wie Sie eine Verfügbarkeitsgruppe so konfiguriert werden kann, dass jede Datenbank im DTC Ressourcenmanager sein kann.



Die Erstellung von Verfügbarkeitsgruppen für verteilte Transaktionen ist ab [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] möglich. Um eine Verfügbarkeitsgruppe für verteilte Transaktionen zu erstellen, müssen Sie `DTC_SUPPORT = PER_DB` in die Definition einer Verfügbarkeitsgruppe einschließen. Das folgende Skript erstellt eine Verfügbarkeitsgruppe für verteilte Transaktionen. 

```sql
CREATE AVAILABILITY GROUP MyAG
   WITH (
      DTC_SUPPORT = PER_DB  
      )
   FOR DATABASE DB1, DB2
   REPLICA ON
      'Server1' WITH (
         ENDPOINT_URL = 'TCP://SERVER1.corp.com:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         ),
      'Server2' WITH (
         ENDPOINT_URL = 'TCP://SERVER2.corp.com:5022',  
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,  
         FAILOVER_MODE = AUTOMATIC  
         )
```

>[!NOTE]
>Dieses Skript ist ein einfaches Beispiel für eine Verfügbarkeitsgruppe und nicht für eine bestimmte Produktionsumgebung gedacht. 

## <a name="alter-an-availability-group-for-distributed-transactions"></a>Anpassen einer Verfügbarkeitsgruppe für verteilte Transaktionen

Ab [!INCLUDE[SQL2017](../../../includes/sssql17-md.md)] lasen sich Verfügbarkeitsgruppen für verteilte Transaktionen ändern. Um eine Verfügbarkeitsgruppe für verteilte Transaktionen zu ändern, nehmen Sie `DTC_SUPPORT = PER_DB` in das Skript `ALTER AVAILABILITY GROUP` auf. Das Beispielskript aktiviert für eine Verfügbarkeitsgruppe die Unterstützung verteilter Transaktionen. 

```sql
ALTER AVAILABILITY GROUP MyaAG
   SET (
      DTC_SUPPORT = PER_DB  
      );
```

>[!NOTE]
>Ab [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)] Service Pack 2 können Sie Verfügbarkeitsgruppen für verteilte Transaktionen ändern. Für [!INCLUDE[SQL2016](../../../includes/sssql16-md.md)]-Versionen vor Service Pack 2 müssen Sie die Verfügbarkeitsgruppe löschen und dann mit der Einstellung `DTC_SUPPORT = PER_DB` erneut erstellen. 

Um verteilte Transaktionen zu deaktivieren, verwenden Sie den folgenden Transact-SQL-Befehl:

```sql
ALTER AVAILABILITY GROUP MyaAG
   SET (
      DTC_SUPPORT = NONE  
      );
```

## <a name="distributed-transactions---technical-concepts"></a><a name="distTran"/>Verteilte Transaktionen – Technisches Prinzip

Bei einer verteilten Transaktion sind mindestens zwei Datenbanken beteiligt. Der DTC als Transaktionsmanager koordiniert die Transaktion zwischen SQL Server-Instanzen und anderen Datenquellen. Jeder Instanz der [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]-Datenbank-Engine kann als Ressourcenmanager operieren. Wenn bei einer Verfügbarkeitsgruppe die Einstellung `DTC_SUPPORT = PER_DB` verwendet wird, können die Datenbanken als Ressourcenmanager operieren. Weitere Informationen finden Sie in der MS DTC-Dokumentation.

Bei einer Transaktion mit zwei oder mehr Datenbanken in einer einzelnen Instanz der Datenbank-Engine handelt es sich eigentlich um eine verteilte Transaktion. Die Instanz verwaltet die verteilte Transaktion jedoch intern; für den Benutzer entsteht der Eindruck, es handele sich um eine lokale Transaktion. [!INCLUDE[SQL2017](../../../includes/sssql17-md.md)] leitet alle datenbankübergreifenden Transaktionen an den DTC weiter, wenn die Datenbanken sich in einer Verfügbarkeitsgruppe mit der Einstellung `DTC_SUPPORT = PER_DB` befinden. Dies gilt auch, innerhalb einer einzelnen SQL Server-Instanz. 

Auf der Anwendungsebene wird eine verteilte Transaktion beinahe so wie eine lokale Transaktion verwaltet. Am Ende der Transaktion fordert die Anwendung die Transaktion auf, entweder einen Commit oder Rollback auszuführen. Ein verteilter Commit darf vom Transaktions-Manager nicht auf dieselbe Art verwaltet werden, um das Risiko zu minimieren, dass einige Ressourcen-Manager bei einem Netzwerkfehler den Commit erfolgreich ausführen, während andere für die Transaktion einen Rollback ausführen. Dies wird dadurch erreicht, dass der Commitvorgang in zwei Phasen verläuft (die Vorbereitungsphase und die Commitphase), bekannt als Zweiphasencommit.

- **Vorbereitungsphase**
   
   Wenn der Transaktions-Manager eine Anforderung für ein Commit erhält, sendet er einen Vorbereitungsbefehl an alle Ressourcen-Manager, die an der Transaktion beteiligt sind. Jeder Ressourcen-Manager trifft dann die notwendigen Vorbereitungen, um die Transaktion beständig zu machen, und alle Puffer, die Images von Protokollen für die Transaktion enthalten, werden auf den Datenträger geleert. Wenn die Ressourcen-Manager die Vorbereitungsphase beenden, geben sie jeweils eine Information über den Erfolg oder das Fehlschlagen der Vorbereitung an den Transaktions-Manager zurück.

- **Commitphase**
   
   Wenn der Transaktions-Manager von der erfolgreichen Vorbereitung aller Ressourcen-Manager in Kenntnis gesetzt wird, sendet er Commitbefehle an alle Ressourcen-Manager. Die Ressourcen-Manager können dann den Commit beenden. Wenn alle Ressourcen-Manager eine erfolgreiche Ausführung des Commits melden, sendet der Transaktions-Manager eine Benachrichtigung über die erfolgreiche Ausführung an die Anwendung. Wenn einer der Ressourcen-Manager einen Fehler bei der Vorbereitung ausgibt, sendet der Transaktions-Manager einen Rollbackbefehl an alle Ressourcen-Manager und benachrichtigt die Anwendung über die fehlgeschlagene Ausführung des Commits.

### <a name="detailed-steps"></a>Ausführliche Schritte

In den nachfolgenden Schritten wird erläutert, wie die Anwendung mit dem DTC zusammenarbeitet, um verteilte Transaktionen abzuschließen.

1. Die SQL Server-Instanz trägt sich bei der DTC-Transaktion ein. Dies kann passieren, wenn es für eine Transaktion mehr als einen Ressourcenmanager gibt oder wenn ein Client fordert, dass eine Transaktion zu einer DTC-Transaktion hochgestuft wird.
2. Der Client führt in der SQL Server-Instanz im Rahmen der DTC-Transaktion mehrere Aufgaben aus.
3. Der Client gibt vor, dass die DTC-Transaktion committet oder abgebrochen werden soll.
    - Wenn der Client den Abbruch vorgibt, wird die Transaktion sofort abgebrochen.
    - Wenn der Client den Commit vorgibt, startet der DTC mit dem Zweiphasencommit, indem alle Ressourcenmanager in der Transaktion gebeten werden, sie vorzubereiten.
4. Der DTC bittet alle Ressourcenmanager, die Transaktion zu committen, nachdem sie den Abschluss der Vorbereitungsphase erfolgreich bestätigt haben. Wenn die Bestätigung aus welchem Grund auch immer nicht erfolgen kann, bricht der DTC die Transaktion ab. 

### <a name="effects-of-configuring-an-availability-group-for-distributed-transactions"></a>Auswirkungen des Konfigurierens einer Verfügbarkeitsgruppe für verteilte Transaktionen

Jede an einer verteilten Transaktion teilnehmende Entität wird als Ressourcenmanager bezeichnet. Beispiele für Ressourcen-Manager:

* Eine [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]-Instanz. 
* Eine Datenbank in einer Verfügbarkeitsgruppe, die für verteilte Transaktionen konfiguriert wurde.
* Der DTC-Dienst – auch dieser kann ein Transaktionsmanager sein.
* Andere Datenquellen 

Um an einer verteilten Transaktion teilzunehmen, trägt sich eine [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]-Instanz beim DTC ein. Normalerweise trägt sich die [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]-Instanz beim DTC auf dem lokalen Server ein. Jede [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]-Instanz erstellt einen Ressourcenmanager mit einer eindeutigen Ressourcenmanager-ID (RMID) und registriert sie beim DTC. Standardmäßig verwenden alle Datenbanken in einer [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]-Instanz dieselbe RMID. 

Wenn sich eine Datenbank in einer Verfügbarkeitsgruppe befindet, wird die Lese-/Schreibkopie einer Verfügbarkeitsdatenbank, auch als primäres Replikat bezeichnet, möglicherweise in eine andere [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]-Instanz verschoben. Damit während dieses Verschiebevorgangs verteilte Transaktionen unterstützt werden, sollte jede Datenbank als separater Ressourcenmanager agieren und benötigt eine einmalige RMID. Wenn für eine Verfügbarkeitsgruppe die Einstellung `DTC_SUPPORT = PER_DB` gewählt wurde, erstellt [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)] einen Ressourcenmanager für jede Datenbank und führt die DTC-Registrierung mit der eindeutigen RMID durch. In dieser Konfiguration fungiert die Datenbank als Ressourcenmanager für DTC-Transaktionen.

Weitere Informationen zu verteilten Transaktionen in [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)], finden Sie unter [verteilte Transaktionen](#distTran)

## <a name="manage-unresolved-transactions"></a>Verwalten nicht aufgelöster Transaktionen

Das Ergebnis aktiver Transaktion, das während eines RMID-Wechsels besteht, kann nach einem Failover nicht wiederhergestellt werden. Dies liegt daran, dass der für die Eintragung verwendete RMID-SQL-Server und der für die Wiederherstellung verwendete RMID-SQL-Server nicht identisch sind. Zu einem RMID-Wechsel kann es in folgenden Fällen kommen:

* Änderung an der Einstellung `DTC_SUPPORT` einer Verfügbarkeitsgruppe 
* Hinzufügen oder Entfernen einer Datenbank aus einer Verfügbarkeitsgruppe 
* Verwerfen einer Verfügbarkeitsgruppe

In diesen Fällen versucht die Instanz, den DTC zu kontaktieren und das Transaktionsergebnis zu identifizieren, wenn ein Failover vom primären Replikat auf eine neue [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]-Instanz ausgeführt wird. Der DTC kann das Ergebnis nicht zurückgeben, da die RMID, die die Datenbank während der Wiederherstellung bei unsicheren Transaktionen für den Ergebnisabruf verwendet, bisher nicht in der Liste eingetragen war. Deshalb gilt für die Datenbank der Status SUSPECT.

Im neuen [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]-Fehlerprotokoll wird ein Eintrag ähnlich diesem Beispiel angezeigt:

```
Microsoft Distributed Transaction Coordinator (MS DTC) 
failed to reenlist citing that the database RMID does 
not match the RMID [xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx] 
associated with the transaction.  Please manually resolve
the transaction.
    
SQL Server detected a DTC/KTM in-doubt transaction with UOW 
{yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy}.Please resolve it 
following the guideline for Troubleshooting DTC Transactions.
```

Das vorstehende Beispiel zeigt, dass der DTC die Datenbank aus dem neuen primären Replikat nicht wieder in die Transaktion aufnehmen konnte, die nach dem Failover erstellt worden war. Da die [!INCLUDE[SQLServer](../../../includes/ssnoversion-md.md)]-Instanz das Ergebnis der verteilten Transaktion nicht mehr bestimmten kann, wird die Datenbank als verdächtig markiert. Die Transaktion wird als eine Arbeitseinheit (Unit of Work – UOW) markiert und erhält einen GUI. Um die Datenbank wiederherzustellen, müssen Sie entweder einen Commit oder einen manuellen Rollback ausführen. 

>[!WARNING]
>Dies kann sich auf eine Anwendung auswirken. Vergewissern Sie sich, dass der Commit oder Rollback den Anforderungen Ihrer Anwendung entspricht. 

Führen Sie nur eines der folgenden Skripte aus:

   * Um Transaktionen zu committen, aktualisieren Sie das folgende Skript, indem Sie `yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy` durch die UOW der unsicheren Transaktion aus der vorherigen Fehlermeldung ersetzen, und führen Sie es aus:

   ```sql
   KILL 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy' WITH COMMIT
   ```

   * Um einen Rollback für die Transaktion durchzuführen, aktualisieren Sie das folgende Skript, indem Sie `yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy` durch UOW der unsicheren Transaktion aus der vorherigen Fehlermeldung ersetzen, und führen Sie es aus:

   ```sql
   KILL 'yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy' WITH ROLLBACK
   ```

Nach dem Rollback oder Commit der Transaktion können Sie die Datenbank mit `ALTER DATABASE` online schalten. Aktualisieren Sie das folgende Skript, indem Sie als Datenbankname den Namen der verdächtigen Datenbank angeben:

   ```sql
   ALTER DATABASE [DB1] SET ONLINE
   ```

Weitere Informationen zum Auflösen unsicherer Transaktionen finden Sie unter [Resolve Transactions manually](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754134(v=ws.10)) (Transaktionen manuell auflösen).

## <a name="next-steps"></a>Nächste Schritte  

[Distributed Transactions](/dotnet/framework/data/adonet/distributed-transactions) (Verteilte Transaktionen)

[Always On-Verfügbarkeitsgruppen: Interoperabilität &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)  
  
[Transactions - Always On availability groups and Database Mirroring](transactions-always-on-availability-and-database-mirroring.md) (Transaktionen – Always On-Verfügbarkeitsgruppen und Datenbankspiegelung  

[Supporting XA Transactions](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753563(v=ws.10)) (Unterstützung von XA-Transaktionen)

[How It Works: Session/SPID (–2) for DTC Transactions (Funktionsweise: Session/SPID (–2) für DTC-Transaktionen)](/archive/blogs/bobsql/how-it-works-sessionspid-2-for-dtc-transactions)