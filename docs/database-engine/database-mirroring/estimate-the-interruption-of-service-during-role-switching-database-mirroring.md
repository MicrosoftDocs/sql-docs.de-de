---
title: Schätzung der Dienstunterbrechung während eines Spiegelungsfailovers
description: Schätzen Sie die Dienstunterbrechung ein, wenn ein Failover für eine Datenbankspiegelung von der primären zur sekundären Rolle durchgeführt wird.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- parallel redo [SQL Server]
- role switching [SQL Server]
- database mirroring [SQL Server], queues
- failover [SQL Server], database mirroring
- redo [database mirroring]
- database mirroring [SQL Server], failover
ms.assetid: 586a6f25-672b-491b-bc2f-deab2ccda6e2
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: df6dcbbe97759b539a9b971f023cefb0c14f29a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342390"
---
# <a name="estimate-the-interruption-of-service-during-role-switching-database-mirroring"></a>Einschätzen der Unterbrechung des Diensts während des Rollenwechsels (Datenbankspiegelung)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Während eines Rollenwechsels ist der Zeitraum, für den die Datenbankspiegelung außer Dienst ist, abhängig von der Art des Rollenwechsels und vom Grund für den Rollenwechsel.  
  
-   Beim automatischen Failover tragen zwei Faktoren zur Dauer der Dienstunterbrechung bei: zum einen die benötigte Zeit, bis der Spiegelserver erkennt, dass in der Prinzipalserverinstanz ein Fehler aufgetreten ist, also die Fehlererkennungszeit, zum anderen die Zeit, die für das Failover der Datenbank benötigt wird, also die Failoverzeit.  
  
-   Obwohl ein Fehler aufgetreten ist, ist bei einem Erzwingen des Diensts das Erkennen des Fehlers und das Reagieren darauf abhängig von der menschlichen Reaktionszeit. Das Schätzen der potenziellen Dienstunterbrechung ist jedoch beschränkt auf das Schätzen der Zeit, die der Spiegelserver für den Rollenwechsel benötigt, nachdem der Befehl für den erzwungenen Dienst ausgegeben wurde.  
  
    > [!NOTE]  
    >  Um die zum Erkennen bestimmter Bedingungen, z. B. einiger Fehlertypen, erforderliche Zeit zu reduzieren, können Sie Warnungen für diese Bedingungen definieren.  
  
-   Bei einem manuellen Failover wird nur die für das Failover der Datenbank erforderliche Zeit nach dem Failoverbefehl ausgegeben.  
  
## <a name="error-detection"></a>Fehlererkennung  
 Die Zeit, die das System zum Erkennen eines Fehlers benötigt, hängt von der Art des Fehlers ab. Beispielsweise wird ein Netzwerkfehler nahezu sofort bemerkt, während es 10 Sekunden dauert, um festzustellen, dass ein Server nicht reagiert (bei Standardtimeout).  
  
 Informationen zu Fehlern, die während einer Datenbank-Spiegelungssitzung zu Problemen führen können, und zur Timeouterkennung im Modus für hohe Sicherheit mit automatischem Failover finden Sie unter [Mögliche Fehler während der Datenbankspiegelung](../../database-engine/database-mirroring/possible-failures-during-database-mirroring.md).  
  
## <a name="failover-time"></a>Failoverzeit  
 Die Failoverzeit setzt sich hauptsächlich aus der Zeit zusammen, die der frühere Spiegelserver benötigt, um ein Rollforward für Protokolle in der Wiederholungswarteschlange auszuführen, sowie einem kurzen zusätzlichen Zeitraum (weitere Informationen zur Verarbeitung von Protokolldatensätzen durch den Spiegelserver finden Sie unter [Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)). Informationen zum Schätzen der Failoverzeit finden Sie unter "Schätzen der Rollforwardrate für das Failover" weiter unten in diesem Thema.  
  
> [!IMPORTANT]  
>  Wenn ein Failover während einer Transaktion auftritt, in der ein Index oder eine Tabelle erstellt und anschließend geändert wird, kann das Failover mehr Zeit als gewöhnlich in Anspruch nehmen.  So kann ein Failover während der folgenden Folge von Operationen die Failoverzeit verlängern: BEGIN TRANSACTION, CREATE INDEX in einer Tabelle und SELECT INTO für die Tabelle. Die Möglichkeit einer erhöhten Failoverzeit während einer solchen Transaktion bleibt bis zu ihrem Abschluss durch eine COMMIT TRANSACTION- oder ROLLBACK TRANSACTION-Anweisung bestehen.  
  
### <a name="the-redo-queue"></a>Wiederholungswarteschlange  
 Um ein Rollforward für die Datenbank auszuführen, müssen die Protokolldatensätze angewandt werden, die sich derzeit in der Wiederholungswarteschlange auf dem Spiegelserver befinden. Die *Wiederholungswarteschlange* besteht aus den Protokolldatensätzen, die auf den Datenträger des Spiegelservers geschrieben wurden, für die aber noch kein Rollforward in der Spiegeldatenbank ausgeführt wurde.  
  
 Die Failoverzeit für die Datenbank hängt davon ab, wie schnell der Spiegelserver ein Rollforward für das Protokoll in der Wiederholungswarteschlange ausführen kann. Dies wird wiederum in erster Linie durch die Systemhardware und die aktuelle Arbeitsauslastung bestimmt. Eine Prinzipaldatenbank kann potenziell so stark ausgelastet sein, dass der Prinzipalserver den Protokollversand zum Spiegelserver wesentlich schneller als ein Rollforward des Protokolls ausführen kann. In dieser Situation kann das Failover viel Zeit in Anspruch nehmen, während der Spiegelserver ein Rollforward für das Protokoll in der Wiederholungswarteschlange ausführt. Um die aktuelle Größe der Wiederholungswarteschlange festzustellen, verwenden Sie den Leistungsindikator **Wiederholungswarteschlange** im Datenbankspiegelungs-Leistungsobjekt. Weitere Informationen finden Sie unter [SQL Server, Database Mirroring Object](../../relational-databases/performance-monitor/sql-server-database-mirroring-object.md).  
  
### <a name="estimating-the-failover-redo-rate"></a>Schätzen der Rollforwardrate für das Failover  
 Sie können die erforderliche Zeitdauer für ein Rollforward von Protokolldatensätzen (die *Rollforwardrate*) mithilfe einer Testkopie der Produktionsdatenbank messen.  
  
 Die Methode zum Schätzen der Rollforwardzeit hängt von der Anzahl von Threads ab, die vom Spiegelserver während der Rollforwardphase verwendet werden. Die Anzahl von Threads hängt von folgendem Faktoren ab:  
  
-   In [!INCLUDE[ssStandard](../../includes/ssstandard-md.md)]verwendet der Spiegelserver immer einen einzigen Thread für das Rollforward der Datenbank.  
  
-   In [!INCLUDE[ssEnterprise](../../includes/ssenterprise-md.md)]verwenden Spiegelserver auf Computern mit weniger als fünf CPUs ebenfalls nur einen einzigen Thread. Bei mindestens fünf CPUs verteilt ein Spiegelserver während eines Failovers die Rollforwardvorgänge auf mehrere Threads (dies wird als *paralleles Rollforward* bezeichnet). Das parallele Rollforward ist für die Verwendung eines Threads für jeweils vier CPUs optimiert.  
  
#### <a name="estimating-the-single-threaded-redo-rate"></a>Schätzen der Rollforwardrate für Rollforwards mit einem einzelnen Thread  
 Bei einem Rollforward mit einem einzelnen Thread dauert das Rollforward der Spiegeldatenbank während des Failovers etwa genauso lange wie eine Wiederherstellung einer Protokollsicherung benötigt, um ein Rollforward für den gleichen Protokollumfang auszuführen. Um die Failoverzeit zu schätzen, erstellen Sie eine Testdatenbank in der Umgebung, in der Sie die Spiegelung ausführen möchten. Anschließend verwenden Sie eine Protokollsicherung aus der Produktionsdatenbank. Um die Rollforwardrate für diese Protokollsicherung zu schätzen, messen Sie, wie lange Sie benötigen, um die Protokollsicherung mit WITH NORECOVERY in der Testdatenbank wiederherzustellen.  
  
 Sobald Sie die Rollforwardrate des Spiegelservers kennen, können Sie den Zeitaufwand für das Failover der Datenbank zu einem bestimmten Zeitpunkt schätzen, indem Sie den aktuellen Protokollumfang auf dem Spiegelserver, für den ein Rollforward ausgeführt werden muss (gemessen mithilfe des **Wiederholungswarteschlange** -Leistungsindikators) durch die Rollforwardrate dividieren. Wenn der Spiegelserver unter normalen Bedingungen mit der Arbeitsauslastung vom Prinzipalserver Schritt halten kann, ist der Wert für **Wiederholungswarteschlange** niedrig oder liegt bei Null, und ein Failover dauert deshalb nur wenige Sekunden.  
  
#### <a name="estimating-the-parallel-redo-rate"></a>Schätzen der Rollforwardrate für parallele Rollforwards  
 In [!INCLUDE[ssEnterprise](../../includes/ssenterprise-md.md)]ist die parallele Wiederholung für die Verwendung eines Threads für jeweils vier CPUs optimiert. Um die Rollforwardzeit für das parallele Rollforward zu schätzen, stellt der Zugriff auf ein laufendes Testsystem genauere Ergebnisse bereit als lediglich eine Testdatenbank. Erhöhen Sie beim Überwachen der Wiederholungswarteschlange auf dem Spiegelserver die Arbeitsauslastung auf dem Prinzipalserver. Im Normalbetrieb liegt der Wert der Wiederholungswarteschlange bei Null. Erhöhen Sie die Arbeitsauslastung auf dem Prinzipalserver, bis die Größe der Wiederholungswarteschlange ständig zunimmt. Das System hat dann seine maximale Rollforwardrate erreicht, und der **Bytes wiederholen/Sekunde** -Leistungsindikator repräsentiert dann die maximale Rollforwardrate. Weitere Informationen finden Sie unter [SQL Server, Database Mirroring Object](../../relational-databases/performance-monitor/sql-server-database-mirroring-object.md).  
  
## <a name="estimating-interruption-of-service-during-automatic-failover"></a>Schätzen der Dienstunterbrechung beim automatischen Failover  
 In der folgende Abbildung wird veranschaulicht, wie die Fehlererkennung und die Failoverzeit zur Gesamtzeit beitragen, die zum Abschließen eines automatischen Failovers auf **Partner_B** erforderlich ist. Das Failover benötigt Zeit zum Ausführen des Rollforwards für die Datenbank (die Rollforwardphase) sowie zusätzlich ein wenig Zeit, um die Datenbank online zu schalten. In der Rollbackphase wird ein Rollback für Transaktionen ohne Commit ausgeführt wird. Diese Phase erfolgt, nachdem die Prinzipaldatenbank online geschaltet wurde, und wird nach dem Failover fortgesetzt. Die Datenbank ist während der Rollbackphase verfügbar.  
  
 ![Fehlererkennung und Failoverzeit](../../database-engine/database-mirroring/media/dbm-failovauto-time.gif "Fehlererkennung und Failoverzeit")  
  
## <a name="see-also"></a>Weitere Informationen  
 [Betriebsmodi der Datenbankspiegelung](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)   
 [Rollenwechsel während einer Datenbank-Spiegelungssitzung &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [Überwachen der Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)  
  
  
