---
description: Überprüfen von replizierten Daten
title: Überprüfen von replizierten Daten | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Subscribers [SQL Server replication], data validation
- replication [SQL Server], validating data
- transactional replication, validating data
- validating data
- merge replication data validation [SQL Server replication], SQL Server Management Studio
ms.assetid: 215b4c9a-0ce9-4c00-ac0b-43b54151dfa3
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: c83a02c9c2b0c8c22a62f1765c839a1c15534405
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "88470185"
---
# <a name="validate-replicated-data"></a>Überprüfen von replizierten Daten
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  In diesem Thema wird beschrieben, wie Daten beim Abonnenten in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]oder Replikationsverwaltungsobjekten (RMO) überprüft werden.  
  
Bei der Transaktions- und der Mergereplikation können Sie überprüfen, ob die Daten auf dem Abonnenten mit denen auf dem Verleger übereinstimmen. Die Überprüfung kann für bestimmte Abonnements oder für alle Abonnements für eine Veröffentlichung ausgeführt werden. Geben Sie einen der folgenden Überprüfungstypen an. Bei der nächsten Ausführung des Verteilungs-Agents oder des Merge-Agents werden die Daten dann überprüft:  
  
-   **Nur Zeilenanzahl.** Bei diesem Typ wird lediglich überprüft, ob die Tabelle auf dem Abonnenten dieselbe Zeilenanzahl besitzt wie die Tabelle auf dem Verleger. Der Inhalt der Zeilen wird nicht geprüft. Die Überprüfung der Zeilenanzahl bietet einen Überprüfungsansatz, der kaum Ressourcen beansprucht und Sie Probleme bei den Daten erkennen lässt.   
-   **Zeilenanzahl und binäre Prüfsumme.** Bei dieser Form der Überprüfung wird zusätzlich zum Vergleich der Zeilenanzahl auf dem Verleger und dem Abonnenten mithilfe des Prüfsummenalgorithmus eine Prüfsumme aller Daten berechnet. Ist die Zeilenanzahl fehlerhaft, wird die Berechnung der Prüfsumme nicht ausgeführt.  
  
 Bei der Mergereplikation kann außer der Übereinstimmung der Daten auf dem Abonnenten und dem Verleger auch überprüft werden, ob die Daten auf den einzelnen Abonnenten richtig partitioniert sind. Weitere Informationen finden Sie unter [Überprüfen von Partitionsinformationen für einen Mergeabonnenten](../../relational-databases/replication/validate-partition-information-for-a-merge-subscriber.md).  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
   
## <a name="how-data-validation-works"></a>Funktionsweise der Datenüberprüfung  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] überprüft Daten, indem es auf dem Verleger die Zeilenanzahl bzw. eine Prüfsumme berechnet und dann diese Werte mit der für den Abonnenten berechneten Zeilenanzahl bzw. Prüfsumme vergleicht. Bei der Berechnung der Prüfsumme wird ein Wert für die gesamte Veröffentlichungstabelle und ein Wert für die gesamte Abonnementtabelle berechnet (die Daten in der **text**-, **ntext**- bzw. **image** -Spalte fließen nicht mit ein).  
  
 Während diese Berechnungen ausgeführt werden, werden die Tabellen, deren Zeilenanzahl bzw. Prüfsumme berechnet wird, vorübergehend für den gemeinsamen Zugriff gesperrt. Diese Berechnungen dauern aber nicht lange, sodass die Sperren meist schon innerhalb weniger Sekunden wieder aufgehoben werden.  
  
 Bei der Überprüfung anhand von binären Prüfsummen werden nicht die physischen Zeilen auf der Datenseite, sondern die einzelnen Spalten einer 32-Bit-Redundanzüberprüfung (Redundancy Check, CRC) unterzogen. Dadurch können die Spalten mit der Tabelle in einer beliebigen physischen Reihenfolge auf der Datenseite dargestellt werden, es wird aber trotzdem dieselbe CRC für die Zeile berechnet. Die Überprüfung anhand von binären Prüfsummen kann verwendet werden, wenn Zeilen- oder Spaltenfilter auf die Veröffentlichung angewendet wurden.  

 Der Prozess der Datenüberprüfung besteht aus drei Teilen:  
  
1.  Zunächst müssen die Abonnements für eine Veröffentlichung, die überprüft werden sollen, *gekennzeichnet* werden. Die Abonnements, die überprüft werden sollen, können Sie in den Dialogfeldern **Abonnement überprüfen**, **Abonnements überprüfen** und **Alle Abonnements überprüfen** , die über den Ordner **Lokale Veröffentlichungen** und **Lokale Abonnements** in [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]verfügbar sind. Abonnements können darüber hinaus auch auf der Registerkarte **Alle Abonnements** , auf der Registerkarte **Überwachungsliste für Abonnements** und über den Veröffentlichungsknoten im Replikationsmonitor gekennzeichnet werden. Informationen zum Starten des Replikationsmonitors finden Sie unter [Starten des Replikationsmonitors](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
2.  Ein Abonnement wird bei der nächsten Synchronisierung durch den Verteilungs-Agent (für Transaktionsreplikationen) oder durch den Merge-Agent (für Mergereplikationen) überprüft. Der Verteilungs-Agent wird in der Regel kontinuierlich ausgeführt. In diesem Fall erfolgt die Überprüfung sofort. Der Merge-Agent wird in der Regel bei Bedarf ausgeführt, und die Überprüfung erfolgt nach der Ausführung des Agents.  
  
3.  Anzeigen der Überprüfungsergebnisse:   
    -   In den Detailfenstern im Replikationsmonitor auf der Registerkarte **Verlauf Verteiler zu Abonnent** für die Transaktionsreplikation und auf der Registerkarte **Synchronisierungsverlauf** für die Mergereplikation.    
    -   Im Dialogfeld **Synchronisierungsstatus anzeigen** in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
 
## <a name="considerations-and-restrictions"></a>Überlegungen und Einschränkungen  
 Bedenken Sie im Zusammenhang mit dem Überprüfen von Daten Folgendes:  
  
-   Vor dem Überprüfen von Daten müssen Sie sämtliche Updateaktivitäten auf den Abonnenten beenden (die Aktivitäten auf dem Verleger müssen für die Überprüfung nicht beendet werden).  
-   Weil Prüfsummen und binäre Prüfsummen beim Überprüfen eines großen Datensatzes große Mengen der Prozessorressourcen benötigen, sollten Sie die Überprüfung so planen, dass sie zum Zeitpunkt der niedrigsten Aktivität auf den Servern ausgeführt wird, die für die Replikation verwendet werden.   
-   Die Replikation überprüft lediglich Tabellen. Ein Abgleich reiner Schemaartikel (wie z. B. gespeicherter Prozeduren) auf dem Verleger und dem Abonnenten erfolgt nicht.   
-   Binäre Prüfsummen können für jede veröffentlichte Tabelle verwendet werden. Die Prüfsumme kann jedoch keine Tabellen mit Spaltenfiltern bzw. logische Tabellenstrukturen überprüfen, bei denen sich die Spaltenoffsets unterscheiden (Ursache dafür sind ALTER TABLE-Anweisungen, die Spalten löschen oder hinzufügen).   
-   Die Überprüfung der Replikation verwendet die **checksum** -Funktion und die **binary_checksum** -Funktion. Informationen zum Verhalten finden Sie unter [CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md) und [BINARY_CHECKSUM  &#40;Transact-SQL&#41;](../../t-sql/functions/binary-checksum-transact-sql.md).   
-   Die Überprüfung durch binäre Prüfsummen oder Prüfsummen kann dann fälschlicherweise einen Fehler ausgeben, wenn auf dem Abonnenten andere Datentypen vorhanden sind als auf dem Verleger. Dieser Fall kann eintreten, wenn Sie eine der folgenden Aufgaben ausführen:    
    -   Sie legen explizit Schemaoptionen zum Zuordnen von Datentypen für frühere Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]fest.  
    -   Sie legen den Veröffentlichungskompatibilitätsgrad für eine Mergeveröffentlichung auf eine frühere Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]fest, und veröffentlichte Tabellen enthalten mindestens einen Datentyp, der für diese Version zugeordnet werden muss.    
    -   Sie initialisieren ein Abonnement manuell und verwenden auf dem Abonnenten andere Datentypen.   
-   Bei der Transaktionsreplikation werden Überprüfungen mithilfe der binären Prüfsumme bzw. der Prüfsumme für transformierbare Abonnements nicht unterstützt.   
-   Bei Daten, die auf Nicht-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Abonnenten repliziert wurden, kann keine solche Überprüfung stattfinden.    
-   Die Prozeduren für den Replikationsmonitor sind nur für Pushabonnements geeignet, da Pullabonnements im Replikationsmonitor nicht synchronisiert werden können. Sie können ein Abonnement jedoch für die Überprüfung markieren und die Überprüfungsergebnisse für Pullabonnements im Replikationsmonitor anzeigen.    
-   In den Überprüfungsergebnissen wird angezeigt, ob die Überprüfung erfolgreich war oder fehlgeschlagen ist, es wird jedoch nicht angegeben, in welchen Zeilen die Überprüfung beim Auftreten eines Fehlers fehlgeschlagen ist. Verwenden Sie das Hilfsprogramm [tablediff Utility](../../tools/tablediff-utility.md), um die Daten auf dem Verleger und auf dem Abonnenten zu vergleichen. Informationen zum Verwenden dieses Hilfsprogramms finden Sie unter [Überprüfen replizierter Tabellen auf Unterschiede &#40;Replikationsprogrammierung&#41;](../../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md).  


## <a name="data-validation-results"></a>Ergebnisse der Datenüberprüfung  
 Nach abgeschlossener Überprüfung protokolliert der Verteilungs-Agent bzw. der Merge-Agent Meldungen zum Erfolg bzw. Misserfolg der Überprüfung (die Replikation vermerkt nicht, bei welchen Zeilen das Ergebnis negativ war). Diese Meldungen können in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], im Replikationsmonitor und in den Replikationssystemtabellen angezeigt werden. Im oben aufgeführten Themen zur Vorgehensweise wird erläutert, wie Sie die Überprüfung ausführen und die Ergebnisse anzeigen können.  
  
 Bei negativem Überprüfungsergebnis sollten folgende Punkte bedacht werden:  
  
-   Konfigurieren Sie die Replikationswarnung **Replikation: Fehler bei der Datenüberprüfung auf dem Abonnenten** , damit Sie bei einem negativen Überprüfungsergebnis benachrichtigt werden. Weitere Informationen finden Sie unter [Konfigurieren von vordefinierten Replikationswarnungen &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/administration/configure-predefined-replication-alerts-sql-server-management-studio.md)  
  
-   Ist ein negatives Überprüfungsergebnis ein Problem für Ihre Anwendung? Falls ja, aktualisieren Sie die Daten manuell, damit sie synchronisiert sind, oder initialisieren Sie das Abonnement erneut:  
  
    -   Daten können mit dem [Hilfsprogramm "tablediff"](../../tools/tablediff-utility.md)aktualisiert werden. Weitere Informationen zum Verwenden dieses Hilfsprogramms finden Sie unter [Überprüfen replizierter Tabellen auf Unterschiede &#40;Replikationsprogrammierung&#41;](../../relational-databases/replication/administration/compare-replicated-tables-for-differences-replication-programming.md).  
  
    -   Weitere Informationen zur erneuten Initialisierung finden Sie unter [Erneutes Initialisieren von Abonnements](../../relational-databases/replication/reinitialize-subscriptions.md).  

  
  
## <a name="articles-in-transactional-replication"></a>Artikel in der Transaktionsreplikation 

### <a name="using-sql-server-management-studio"></a>Verwendung von SQL Server Management Studio
  
1.  Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]eine Verbindung mit dem Verleger her, und erweitern Sie dann den Serverknoten.    
2.  Erweitern Sie den Ordner **Replikation** , und erweitern Sie dann den Ordner **Lokale Veröffentlichungen** .    
3.  Klicken Sie mit der rechten Maustaste auf die Veröffentlichung, deren Abonnements Sie überprüfen möchten, und klicken Sie dann auf **Abonnements überprüfen**.    
4.  Wählen Sie im Dialogfeld **Abonnements überprüfen** die zu überprüfenden Abonnements aus:   
    -   Wählen Sie **Alle SQL Server-Abonnements überprüfen** aus.    
    -   Wählen Sie **Folgende Abonnements überprüfen** aus, und wählen Sie dann ein oder mehrere Abonnements aus.    
5.  Um den Typ der durchzuführenden Überprüfung (Zeilenanzahl oder Zeilenanzahl und Prüfsumme) anzugeben, klicken Sie auf **Überprüfungsoptionen**, und geben Sie dann im Dialogfeld **Optionen für die Abonnementüberprüfung** die gewünschten Optionen an.  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]   
7.  Zeigen Sie die Überprüfungsergebnisse Im Replikationsmonitor oder im Dialogfeld **Synchronisierungsstatus anzeigen** an: Führen Sie für jedes Abonnement folgende Vorgänge aus:   
    1.  Erweitern Sie die Veröffentlichung, klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Synchronisierungsstatus anzeigen**.    
    2.  Wenn der Agent nicht ausgeführt wird, klicken Sie im Dialogfeld **Synchronisierungsstatus anzeigen** auf **Start** . Im Dialogfeld werden Meldungen mit Informationen zur Überprüfung angezeigt.    
     Wenn keine Meldungen bezüglich der Überprüfung angezeigt werden, hat der Agent bereits eine nachfolgende Meldung protokolliert. Zeigen Sie die Überprüfungsergebnisse in diesem Fall im Replikationsmonitor an. Weitere Informationen finden Sie in den Prozeduren zu den Verfahrensweisen im Replikationsmonitor in diesem Thema.  

### <a name="using-transact-sql"></a>Verwenden von Transact-SQL

#### <a name="all-articles"></a>Alle Artikel 
  
1.  Führen Sie beim Verleger für die Veröffentlichungsdatenbank [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md) aus. Geben Sie `@publication` und einen der folgenden Werte für `@rowcount_only` an:  
  
    -   **1** - nur Überprüfung der Zeilenanzahl (Standardeinstellung)    
    -   **2** - Zeilenanzahl und binäre Prüfsumme  
  
    > [!NOTE]  
    >  Beim Ausführen von [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md), wird [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md) für jeden Artikel in der Publikation ausgeführt. Um [sp_publication_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md) erfolgreich auszuführen, benötigen Sie SELECT-Berechtigungen für alle Spalten in den veröffentlichten Basistabellen.    
2.  (Optional) Starten Sie den Verteilungs-Agent für jedes Abonnement, wenn er nicht bereits ausgeführt wird. Weitere Informationen finden Sie unter [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) und [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).    
3.  Überprüfen Sie die Agentausgabe für das Ergebnis der Überprüfung. 
  
#### <a name="single-article"></a>Einzelner Artikel  
  
1.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md) aus. Geben Sie `@publication`, den Namen des Artikels für `@article` und einen der folgenden Werte für `@rowcount_only` an:  
  
    -   **1** - nur Überprüfung der Zeilenanzahl (Standardeinstellung)    
    -   **2** - Zeilenanzahl und binäre Prüfsumme  
  
    > [!NOTE]  
    >  Um [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md) erfolgreich auszuführen, benötigen Sie SELECT-Berechtigungen für alle Spalten in den veröffentlichten Basistabellen.  
  
2.  (Optional) Starten Sie den Verteilungs-Agent für jedes Abonnement, wenn er nicht bereits ausgeführt wird. Weitere Informationen finden Sie unter [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) und [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).    
3.  Überprüfen Sie die Agentausgabe für das Ergebnis der Überprüfung.
  
#### <a name="single-subscriber"></a>Einzelner Abonnent 
  
1.  Öffnen Sie auf dem Verleger für die Veröffentlichungsdatenbank eine explizite Transaktion mit [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md).    
2.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_marksubscriptionvalidation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-marksubscriptionvalidation-transact-sql.md) aus. Geben Sie die Veröffentlichung für `@publication`, den Namen des Abonnenten für `@subscriber` und den Namen der Abonnementdatenbank für `@destination_db` an.    
3.  (Optional) Wiederholen Sie Schritt 2 für jedes zu überprüfende Abonnement.    
4.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md) aus. Geben Sie `@publication`, den Namen des Artikels für `@article` und einen der folgenden Werte für `@rowcount_only` an:    
    -   **1** - nur Überprüfung der Zeilenanzahl (Standardeinstellung)    
    -   **2** - Zeilenanzahl und binäre Prüfsumme  
  
    > [!NOTE]  
    >  Um [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md) erfolgreich auszuführen, benötigen Sie SELECT-Berechtigungen für alle Spalten in den veröffentlichten Basistabellen.  
  
5.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank mit [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md) einen Commit für die Transaktion aus.    
6.  (Optional) Wiederholen Sie die Schritte 1 bis 5 für jeden zu überprüfenden Artikel.   
7.  (Optional) Starten Sie den Verteilungs-Agent, wenn er nicht bereits ausgeführt wird. Weitere Informationen finden Sie unter [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) und [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).    
8.  Überprüfen Sie die Agentausgabe für das Ergebnis der Überprüfung. Weitere Informationen finden Sie unter [Validate Data at the Subscriber](../../relational-databases/replication/validate-data-at-the-subscriber.md).  

## <a name="all-push-subscriptions-to-a-transactional-publication"></a>Aller Pushabonnements an Transaktionsveröffentlichung 

### <a name="using-replication-monitor"></a>Verwenden des Replikationsmonitors
  
1.  Erweitern Sie im Replikationsmonitor im linken Bereich eine Verlegergruppe, und erweitern Sie dann einen Verleger.   
2.  Klicken Sie mit der rechten Maustaste auf die Veröffentlichung, deren Abonnements Sie überprüfen möchten, und klicken Sie dann auf **Abonnements überprüfen**.   
3.  Wählen Sie im Dialogfeld **Abonnements überprüfen** die zu überprüfenden Abonnements aus:  
  
    -   Wählen Sie **Alle SQL Server-Abonnements überprüfen** aus.    
    -   Wählen Sie **Folgende Abonnements überprüfen** aus, und wählen Sie dann ein oder mehrere Abonnements aus.    
4.  Um den Typ der durchzuführenden Überprüfung (Zeilenanzahl oder Zeilenanzahl und Prüfsumme) anzugeben, klicken Sie auf **Überprüfungsoptionen**, und geben Sie dann im Dialogfeld **Optionen für die Abonnementüberprüfung** die gewünschten Optionen an.    
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
6.  Klicken Sie auf die Registerkarte **Alle Abonnements** .  
7.  Zeigen Sie die Überprüfungsergebnisse an. Führen Sie für jedes Pushabonnement folgende Vorgänge aus:    
    1.  Wenn der Agent nicht ausgeführt wird, klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Synchronisierung starten**.    
    2.  Klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Details anzeigen**.   
    3.  Zeigen Sie die Informationen auf der Registerkarte **Verlauf Verteiler zu Abonnent** im Textbereich **Aktionen in der ausgewählten Sitzung** an.  
  
## <a name="for-a-single-subscription-to-a-merge-publication"></a>Einzelabonnement an Mergeveröffentlichung

### <a name="using-sql-server-management-studio"></a>Verwendung von SQL Server Management Studio
  
1.  Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]eine Verbindung mit dem Verleger her, und erweitern Sie dann den Serverknoten.    
2.  Erweitern Sie den Ordner **Replikation** , und erweitern Sie dann den Ordner **Lokale Veröffentlichungen** .   
3.  Erweitern Sie die Veröffentlichung, für die Sie Abonnements überprüfen möchten, klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Abonnement überprüfen**.    
4.  Wählen Sie im Dialogfeld **Abonnement überprüfen** die Option **Dieses Abonnement überprüfen** aus.    
5.  Um den Typ der durchzuführenden Überprüfung (Zeilenanzahl oder Zeilenanzahl und Prüfsumme) anzugeben, klicken Sie auf **Optionen**, und geben Sie dann im Dialogfeld **Optionen für die Abonnementüberprüfung** die gewünschten Optionen an.    
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
7.  Zeigen Sie die Überprüfungsergebnisse Im Replikationsmonitor oder im Dialogfeld **Synchronisierungsstatus anzeigen** an:  
  
    1.  Erweitern Sie die Veröffentlichung, klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Synchronisierungsstatus anzeigen**.    
    2.  Wenn der Agent nicht ausgeführt wird, klicken Sie im Dialogfeld **Synchronisierungsstatus anzeigen** auf **Start** . Im Dialogfeld werden Meldungen mit Informationen zur Überprüfung angezeigt.  
  
     Wenn keine Meldungen bezüglich der Überprüfung angezeigt werden, hat der Agent bereits eine nachfolgende Meldung protokolliert. Zeigen Sie die Überprüfungsergebnisse in diesem Fall im Replikationsmonitor an. Weitere Informationen finden Sie in den Prozeduren zu den Verfahrensweisen im Replikationsmonitor in diesem Thema.  
  
## <a name="for-all-subscriptions-to-a-merge-publication"></a>Alle Abonnements an Mergeveröffentlichung

### <a name="using-sql-server-management-studio"></a>Verwendung von SQL Server Management Studio  
1.  Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]eine Verbindung mit dem Verleger her, und erweitern Sie dann den Serverknoten.    
2.  Erweitern Sie den Ordner **Replikation** , und erweitern Sie dann den Ordner **Lokale Veröffentlichungen** .   
3.  Klicken Sie mit der rechten Maustaste auf die Veröffentlichung, deren Abonnements Sie überprüfen möchten, und klicken Sie dann auf **Alle Abonnements überprüfen**.    
4.  Geben Sie im Dialogfeld **Alle Abonnements überprüfen** den Typ der durchzuführenden Überprüfung (Zeilenanzahl oder Zeilenanzahl und Prüfsumme) an.   
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
6.  Zeigen Sie die Überprüfungsergebnisse Im Replikationsmonitor oder im Dialogfeld **Synchronisierungsstatus anzeigen** an: Führen Sie für jedes Abonnement folgende Vorgänge aus:    
    1.  Erweitern Sie die Veröffentlichung, klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Synchronisierungsstatus anzeigen**.    
    2.  Wenn der Agent nicht ausgeführt wird, klicken Sie im Dialogfeld **Synchronisierungsstatus anzeigen** auf **Start** . Im Dialogfeld werden Meldungen mit Informationen zur Überprüfung angezeigt.  
  
     Wenn keine Meldungen bezüglich der Überprüfung angezeigt werden, hat der Agent bereits eine nachfolgende Meldung protokolliert. Zeigen Sie die Überprüfungsergebnisse in diesem Fall im Replikationsmonitor an. Weitere Informationen finden Sie in den Prozeduren zu den Verfahrensweisen im Replikationsmonitor in diesem Thema.  
  
 
## <a name="for-a-single-push-subscription-to-a-merge-publication"></a>Einzelpushabonnement an Mergeveröffentlichung 

### <a name="using-replication-monitor"></a>Verwenden des Replikationsmonitors  
1.  Erweitern Sie im Replikationsmonitor im linken Bereich eine Verlegergruppe, erweitern Sie einen Verleger, und klicken Sie dann auf eine Veröffentlichung.    
2.  Klicken Sie auf die Registerkarte **Alle Abonnements** .    
3.  Klicken Sie mit der rechten Maustaste auf das Abonnement, das Sie überprüfen möchten, und klicken Sie dann auf **Abonnement überprüfen**.    
4.  Wählen Sie im Dialogfeld **Abonnement überprüfen** die Option **Dieses Abonnement überprüfen** aus.    
5.  Um den Typ der durchzuführenden Überprüfung (Zeilenanzahl oder Zeilenanzahl und Prüfsumme) anzugeben, klicken Sie auf **Optionen**, und geben Sie dann im Dialogfeld **Optionen für die Abonnementüberprüfung** die gewünschten Optionen an.    
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
7.  Klicken Sie auf die Registerkarte **Alle Abonnements** .    
8.  Zeigen Sie die Überprüfungsergebnisse an:    
    1.  Wenn der Agent nicht ausgeführt wird, klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Synchronisierung starten**.    
    2.  Klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Details anzeigen**.    
    3.  Zeigen Sie die Informationen auf der Registerkarte **Synchronisierungsverlauf** im Textbereich **Letzte Meldung der ausgewählten Sitzung** an.  

### <a name="using-transact-sql"></a>Verwenden von Transact-SQL
1.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_validatemergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validatemergesubscription-transact-sql.md) aus. Geben Sie `@publication`, den Namen des Abonnenten für `@subscriber`, den Namen der Abonnementdatenbank für `@subscriber_db` und einen der folgenden Werte für `@level` an:   
    -   **1** - Nur Überprüfung der Zeilenzählung    
    -   **3** - Überprüfung der Zeilenzählung und binären Prüfsumme  
  
     Dadurch wird das ausgewählte Abonnement zur Überprüfung gekennzeichnet.  
  
2.  Starten Sie den Merge-Agent für jedes Abonnement. Weitere Informationen finden Sie unter [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) und [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
3.  Überprüfen Sie die Agentausgabe für das Ergebnis der Überprüfung.   
4.  Wiederholen Sie die Schritte 1 bis 3 für jedes zu überprüfende Abonnement.  
  
> [!NOTE]  
>  Ein Abonnement für eine Mergeveröffentlichung kann auch am Ende einer Synchronisierung überprüft werden. Dazu geben Sie den **-Validate** -Parameter an, wenn der [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md)ausgeführt wird.  
  
## <a name="for-all-push-subscriptions-to-a-merge-publication"></a>Alle Pushabonnements an Mergeveröffentlichung
  
### <a name="using-replication-monitor"></a>Verwenden des Replikationsmonitors    
1.  Erweitern Sie im Replikationsmonitor im linken Bereich eine Verlegergruppe, und erweitern Sie dann einen Verleger.    
2.  Klicken Sie mit der rechten Maustaste auf die Veröffentlichung, deren Abonnements Sie überprüfen möchten, und klicken Sie dann auf **Alle Abonnements überprüfen**.    
3.  Geben Sie im Dialogfeld **Alle Abonnements überprüfen** den Typ der durchzuführenden Überprüfung (Zeilenanzahl oder Zeilenanzahl und Prüfsumme) an.    
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]    
5.  Klicken Sie auf die Registerkarte **Alle Abonnements** .    
6.  Zeigen Sie die Überprüfungsergebnisse an. Führen Sie für jedes Pushabonnement folgende Vorgänge aus:    
    1.  Wenn der Agent nicht ausgeführt wird, klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Synchronisierung starten**.    
    2.  Klicken Sie mit der rechten Maustaste auf das Abonnement, und klicken Sie dann auf **Details anzeigen**.    
    3.  Zeigen Sie die Informationen auf der Registerkarte **Synchronisierungsverlauf** im Textbereich **Letzte Meldung der ausgewählten Sitzung** an. 
  
### <a name="using-transact-sql"></a>Verwenden von Transact-SQL
1.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_validatemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validatemergepublication-transact-sql.md) aus. Geben Sie `@publication` und einen der folgenden Werte für `@level` an:    
    -   **1** - Nur Überprüfung der Zeilenzählung   
    -   **3** - Überprüfung der Zeilenzählung und binären Prüfsumme  
  
     Dadurch werden alle Abonnements zur Überprüfung gekennzeichnet.   
2.  Starten Sie den Merge-Agent für jedes Abonnement. Weitere Informationen finden Sie unter [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) und [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
3.  Überprüfen Sie die Agentausgabe für das Ergebnis der Überprüfung. Weitere Informationen finden Sie unter [Validate Data at the Subscriber](../../relational-databases/replication/validate-data-at-the-subscriber.md).  

  
## <a name="validate-data-using-merge-agent-parameters"></a>Überprüfen von Daten mithilfe von Merge-Agent-Parametern  
  
1.  Starten Sie auf eine der folgenden Arten den Merge-Agent auf dem Abonnenten (Pullabonnement) oder auf dem Verteiler (Pushabonnement) von der Befehlszeile.   
    -   Durch Angeben eines Werts von **1** (Zeilenanzahl) oder **3** (Zeilenanzahl und binäre Prüfsumme) für den **-Validate** -Parameter.    
    -   Durch Angeben von **Zeilenanzahlüberprüfung** oder **Überprüfung der Zeilenanzahl und Prüfsumme** für den **-ProfileName** -Parameter.  
  
     Weitere Informationen finden Sie unter [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) oder [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Verwenden von Replikationsverwaltungsobjekten (RMO)  
 Die Replikation ermöglicht Ihnen mithilfe von Replikationsverwaltungsobjekten (RMO), programmgesteuert zu überprüfen, ob die Daten auf dem Abonnenten mit den Daten auf dem Verleger übereinstimmen. Welche Objekte Sie verwenden, hängt vom Typ der Replikationstopologie ab. Für die Transaktionsreplikation ist eine Überprüfung aller Abonnements für eine Veröffentlichung erforderlich.  
  
> [!NOTE]  
>  Ein Beispiel hierzu finden Sie unter [Beispiel (RMO)](#RMOExample)weiter unten in diesem Abschnitt.  
  
#### <a name="to-validate-data-for-all-articles-in-a-transactional-publication"></a>So überprüfen Sie die Daten für alle Artikel in einer Transaktionsveröffentlichung  
  
1.  Erstellen Sie eine Verbindung mit dem Verleger, indem Sie die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> -Klasse verwenden.  
  
2.  Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.TransPublication>-Klasse. Legen Sie die <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> -Eigenschaft und die <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> -Eigenschaft für die Veröffentlichung fest. Legen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> -Eigenschaft auf die in Schritt 1 erstellte Verbindung fest.  
  
3.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> -Methode auf, um die restlichen Objekteigenschaften abzurufen. Wenn diese Methode **false** zurückgibt, sind die Veröffentlichungseigenschaften in Schritt 2 falsch definiert, oder die Veröffentlichung ist nicht vorhanden.  
  
4.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.TransPublication.ValidatePublication%2A> -Methode auf. Übergeben Sie die folgenden Werte:  
  
    -   <xref:Microsoft.SqlServer.Replication.ValidationOption>  
  
    -   <xref:Microsoft.SqlServer.Replication.ValidationMethod>  
  
    -   Ein boolescher Wert, der angibt, ob der Verteilungs-Agent nach Abschluss der Überprüfung beendet werden soll.  
  
     Dadurch werden die Artikel zur Überprüfung gekennzeichnet.  
  
5.  Starten Sie den Verteilungs-Agent, falls er noch nicht ausgeführt wird, um alle Abonnements zu synchronisieren. Weitere Informationen finden Sie unter [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md) oder [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md). Das Ergebnis der Überprüfung wird in den Agentverlauf geschrieben. Weitere Informationen finden Sie unter [Monitoring Replication](../../relational-databases/replication/monitor/monitoring-replication.md).  
  
#### <a name="to-validate-data-in-all-subscriptions-to-a-merge-publication"></a>So überprüfen Sie die Daten in allen Abonnements für eine Mergeveröffentlichung  
  
1.  Erstellen Sie eine Verbindung mit dem Verleger, indem Sie die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> -Klasse verwenden.  
  
2.  Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.MergePublication>-Klasse. Legen Sie die <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> -Eigenschaft und die <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> -Eigenschaft für die Veröffentlichung fest. Legen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> -Eigenschaft auf die in Schritt 1 erstellte Verbindung fest.  
  
3.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> -Methode auf, um die restlichen Objekteigenschaften abzurufen. Wenn diese Methode **false** zurückgibt, sind die Veröffentlichungseigenschaften in Schritt 2 falsch definiert, oder die Veröffentlichung ist nicht vorhanden.  
  
4.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.MergePublication.ValidatePublication%2A> -Methode auf. Übergeben Sie die gewünschte <xref:Microsoft.SqlServer.Replication.ValidationOption>.  
  
5.  Führen Sie für jedes Abonnement den Merge-Agent aus, um die Überprüfung zu starten, oder warten Sie die nächste geplante Ausführung des Agents ab. Weitere Informationen finden Sie unter [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) und [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md). Das Ergebnis der Überprüfung wird in den Agentverlauf geschrieben. Diesen können Sie mithilfe des Replikationsmonitors anzeigen. Weitere Informationen finden Sie unter [Monitoring Replication](../../relational-databases/replication/monitor/monitoring-replication.md).  
  
#### <a name="to-validate-data-in-a-single-subscription-to-a-merge-publication"></a>So überprüfen Sie die Daten in einem einzelnen Abonnement für eine Mergeveröffentlichung  
  
1.  Erstellen Sie eine Verbindung mit dem Verleger, indem Sie die <xref:Microsoft.SqlServer.Management.Common.ServerConnection> -Klasse verwenden.  
  
2.  Erstellen Sie eine Instanz der <xref:Microsoft.SqlServer.Replication.MergePublication>-Klasse. Legen Sie die <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> -Eigenschaft und die <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> -Eigenschaft für die Veröffentlichung fest. Legen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> -Eigenschaft auf die in Schritt 1 erstellte Verbindung fest.  
  
3.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> -Methode auf, um die restlichen Objekteigenschaften abzurufen. Wenn diese Methode **false** zurückgibt, sind die Veröffentlichungseigenschaften in Schritt 2 falsch definiert, oder die Veröffentlichung ist nicht vorhanden.  
  
4.  Rufen Sie die <xref:Microsoft.SqlServer.Replication.MergePublication.ValidateSubscription%2A> -Methode auf. Übergeben Sie den Namen des Abonnenten und der Abonnementdatenbank, der bzw. die überprüft wird, und die gewünschte <xref:Microsoft.SqlServer.Replication.ValidationOption>.  
  
5.  Führen Sie für das Abonnement den Merge-Agent aus, um die Überprüfung zu starten, oder warten Sie die nächste geplante Ausführung des Agents ab. Weitere Informationen finden Sie unter [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) und [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md). Das Ergebnis der Überprüfung wird in den Agentverlauf geschrieben. Diesen können Sie mithilfe des Replikationsmonitors anzeigen. Weitere Informationen finden Sie unter [Monitoring Replication](../../relational-databases/replication/monitor/monitoring-replication.md).  
  
###  <a name="example-rmo"></a><a name="RMOExample"></a> Beispiel (RMO)  
 In diesem Beispiel werden alle Abonnements für eine Transaktionsveröffentlichung für die Zeilenanzahlüberprüfung gekennzeichnet.  
  
 [!code-cs[HowTo#rmo_ValidateTranPub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_validatetranpub)]  
  
 [!code-vb[HowTo#rmo_vb_ValidateTranPub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_validatetranpub)]  
  
 In diesem Beispiel wird ein bestimmtes Abonnement für eine Mergeveröffentlichung für die Zeilenanzahlüberprüfung gekennzeichnet.  
  
 [!code-cs[HowTo#rmo_ValidateMergeSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_validatemergesub)]  
  
 [!code-vb[HowTo#rmo_vb_ValidateMergeSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_validatemergesub)]  
  
 ## <a name="see-also"></a>Weitere Informationen  
[Best Practices for Replication Administration](../../relational-databases/replication/administration/best-practices-for-replication-administration.md)  
  
  
