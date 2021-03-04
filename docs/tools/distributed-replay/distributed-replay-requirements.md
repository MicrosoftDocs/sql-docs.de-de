---
description: Distributed Replay Requirements
title: 'Distributed Replay: Anforderungen'
titleSuffix: SQL Server Distributed Replay
ms.prod: sql
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/18/2018
ms.openlocfilehash: 237a2ca3b7488688162b38ea3e7f916e2a4d3c6d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836887"
---
# <a name="distributed-replay-requirements"></a>Distributed Replay Requirements

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Bevor Sie die [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay-Funktion verwenden, sollten Sie die in diesem Thema beschriebenen Produktanforderungen berücksichtigen.  

## <a name="input-trace-requirements"></a>Anforderungen an die Eingabedatei für die Ablaufverfolgung  

Für die erfolgreiche Wiedergabe von Ablaufverfolgungsdaten müssen die Anforderungen an Version und Format erfüllt und die erforderlichen Ereignisse und Spalten enthalten sein.  

### <a name="input-trace-versions"></a>Versionen der Eingabedatei für die Ablaufverfolgung  

Distributed Replay unterstützt Eingabedaten für die Ablaufverfolgung, die in den folgenden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Versionen erfasst wurden:  

- [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)], kumulatives Update 1 und höher Weitere Informationen finden Sie unter [SQL Server 2017 (kumulative Updates)](https://aka.ms/sql2017cu).
- [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]   
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
- [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
- [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]    
- [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]  
  
### <a name="input-trace-formats"></a>Formate für die Eingabedatei der Ablaufverfolgung

Die Eingabedaten der Ablaufverfolgung können in einen der folgenden Formate vorliegen:  

- Einzelne Ablaufverfolgungsdatei mit der Erweiterung `.trc` .  

- Ein Satz von Rollover-Ablaufverfolgungsdateien, die der Dateirollover-Benennungskonvention folgen, z. B.: `<TraceFile>.trc`, `<TraceFile>_1.trc`, `<TraceFile>_2.trc`, `<TraceFile>_3.trc`, ... `<TraceFile>_n.trc`.

### <a name="input-trace-events-and-columns"></a>Ereignisse und Spalten in der Eingabedatei für die Ablaufverfolgung

Die Eingabedaten der Ablaufverfolgung müssen bestimmte Ereignisse und Spalten enthalten, die von Distributed Replay wiedergegeben werden sollen. Die Vorlage **TSQL_Replay** in [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] enthält neben zusätzlichen Informationen alle erforderlichen Ereignisse und Spalten. Weitere Informationen zu dieser Vorlage finden Sie unter [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md).  
  
> [!WARNING]
> Wenn Sie Eingabedaten der Ablaufverfolgung nicht mithilfe der Vorlage **TSQL_Replay** erfassen oder die Anforderungen an die Eingabedaten nicht erfüllt sind, können unerwartete Wiedergabeergebnisse zurückgegeben werden.  

Solange folgende Ereignisse enthalten sind, können Sie auch eine benutzerdefinierte Ablaufverfolgungsvorlage für die Wiedergabe von Ereignissen mit Distributed Replay verwenden:
  
- Audit Login  
  
- Audit Logout  
  
- ExistingConnection  
  
- RPC Output Parameter  
  
- RPC:Completed  
  
- RPC:Starting  
  
- SQL:BatchCompleted  
  
- SQL:BatchStarting  
  
 Wenn Sie serverseitige Cursor wiedergeben, sind auch die folgenden Ereignisse erforderlich:  
  
- CursorClose  
  
- CursorExecute  
  
- CursorOpen  
  
- CursorPrepare  
  
- CursorUnprepare  
  
 Wenn Sie serverseitige vorbereitete SQL-Anweisungen wiedergeben, sind auch die folgenden Ereignisse erforderlich:  
  
- Exec Prepared SQL  
  
- Prepare SQL  
  
 Alle Eingabedaten der Ablaufverfolgung müssen die folgenden Spalten enthalten:  
  
- Ereignisklasse  
  
- EventSequence  
  
- TextData  
  
- Anwendungsname  
  
- LoginName  
  
- DatabaseName  
  
- Datenbank-ID  
  
- HostName  
  
- Binary Data  
  
- SPID  
  
- Startzeit  
  
- EndTime  
  
- IsSystem  
  
## <a name="supported-input-trace-and-target-server-combinations"></a>Unterstützte Kombinationen aus Eingabedatei der Ablaufverfolgung und Zielserver  

In der folgenden Tabelle finden Sie die unterstützten Versionen der Ablaufverfolgungsdaten aufgeführt, sowie jeweils die unterstützten Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , für die Daten wiedergegeben werden können.  
  
|Version der Eingabedaten für die Ablaufverfolgung|Unterstützte Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für die Zielserverinstanz|  
|---------------------------------|------------------------------------------------------------------------------------|  
|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)],[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
  
## <a name="operating-system-requirements"></a>Betriebssystemanforderungen

Zum Ausführen des Verwaltungstools und der Controller- und Clientdienste werden dieselben Betriebssysteme wie für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz unterstützt. Weitere Informationen dazu, welche Betriebssysteme für Ihre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz unterstützt werden, finden Sie unter [Hardware- und Softwareanforderungen für die Installation von SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  

Distributed Replay-Funktionen werden unter x86-basierten und x64-basierten Betriebssystemen unterstützt. Bei x64-basierten Betriebssystemen wird nur der WOW-Modus (Windows on Windows) unterstützt.  

## <a name="installation-limitations"></a>Installationseinschränkungen

Jeder einzelne Computer kann nur eine einzelne Instanz jeder installierten Distributed Replay-Funktion enthalten. Die folgende Tabelle legt dar, wie viele Installationen der einzelnen Funktionen in einer einzelnen Distributed Replay-Umgebung zulässig sind.  
  
|Distributed Replay-Funktion|Maximale Installationen pro Wiedergabeumgebung|  
|--------------------------------|--------------------------------------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller-Dienst|1|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Client-Dienst|16 (physische oder virtuelle Computer)|  
|Verwaltungstool|Unbegrenzt|  
  
> [!NOTE]  
>  Zwar kann auf einem einzelnen Computer nur eine Instanz des Verwaltungstools installiert werden, doch können Sie mehrere Instanzen des Verwaltungstools starten. Von mehreren Verwaltungstools ausgegebene Befehle werden in der Reihenfolge aufgelöst, in der sie empfangen werden.  
  
## <a name="data-access-provider"></a>Datenzugriffsanbieter

 Distributed Replay unterstützt nur den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC-Datenzugriffsanbieter.  

## <a name="target-server-preparation-requirements"></a>Anforderungen bei der Vorbereitung des Zielservers  

Es wird empfohlen, den Zielserver in einer Testumgebung zu platzieren. Wenn Sie Ablaufverfolgungsdaten für eine andere Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wiedergeben möchten als ursprünglich festgelegt, stellen Sie sicher, dass auf dem Zielserver die folgenden Aktionen ausgeführt wurden:  

- Alle in den Ablaufverfolgungsdaten verzeichneten Anmeldungen und Benutzer müssen auch in der gleichen Datenbank auf dem Zielserver vorhanden sein.  

- Alle Benutzernamen und Benutzer auf dem Zielserver müssen über dieselben Berechtigungen wie auf dem ursprünglichen Server verfügen.  

- Die Datenbank-IDs auf dem Ziel sollten mit denen auf der Quelle übereinstimmen. Andernfalls können Sie basierend auf **DatabaseName** einen Abgleich ausführen, sofern dieser in der Ablaufverfolgung enthalten ist.

- Die Standarddatenbank für jeden in den Ablaufverfolgungsdaten enthaltenen Benutzernamen muss (auf dem Zielserver) auf die entsprechende Zieldatenbank des Benutzernamens festgelegt werden. Angenommen, die wiederzugebenden Ablaufverfolgungsdaten enthalten Aktivitäten für den Benutzernamen **Fred** in der Datenbank **Fred_Db** der ursprünglichen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Dann muss auf dem Zielserver die Standarddatenbank für den Benutzernamen **Fred** auf die Datenbank festgelegt werden, die mit **Fred_Db** übereinstimmt (auch, wenn sich der Datenbankname unterscheidet). Legen Sie mithilfe der gespeicherten Systemprozedur `sp_defaultdb` die Standarddatenbank für den Benutzernamen fest.  
  
 Beim Wiedergeben von Ereignissen, die fehlende oder fehlerhafte Benutzernamen aufweisen, können Wiedergabefehler auftreten, die Wiedergabe wird jedoch fortgesetzt.  
  
## <a name="see-also"></a>Weitere Informationen

- [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)
- [Distributed Replay-Sicherheit](../../tools/distributed-replay/distributed-replay-security.md)
- [Installieren von Distributed Replay – Übersicht](../../tools/distributed-replay/install-distributed-replay-overview.md)