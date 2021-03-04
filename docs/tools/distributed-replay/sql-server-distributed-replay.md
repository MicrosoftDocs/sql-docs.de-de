---
title: SQL Server Distributed Replay
titleSuffix: SQL Server Distributed Replay
description: Mit der Funktion SQL Server Distributed Replay können Sie die Auswirkungen zukünftiger Upgrades auf SQL Server, die Hardware, das Betriebssystem und die SQL Server-Optimierung bewerten.
ms.prod: sql
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: mikeray
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: ac77b9ead15eb4171dbcaee38235a9f934aaa172
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837778"
---
# <a name="sql-server-distributed-replay"></a>SQL Server Distributed Replay

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Das Distributed Replay-Feature von [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt Sie beim Bewerten der Auswirkungen zukünftiger Upgrades von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Mit dem Hilfsprogramm können Sie auch die Auswirkungen von Hardware- und Betriebssystemupgrades sowie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Optimierungen bewerten.

## <a name="benefits-of-distributed-replay"></a>Vorteile von Distributed Replay

Ähnlich wie mit dem SQL Server Profiler können Sie mithilfe von Distributed Replay eine aufgezeichnete Ablaufverfolgung in einer aktualisierten Testumgebung wiedergeben. Im Gegensatz zum SQL Server Profiler ist das Distributed Replay nicht auf die Wiedergabe der Workload eines einzelnen Computers beschränkt.

Das Distributed Replay bietet eine stärker skalierbare Lösung als der SQL Server Profiler. Mit Distributed Replay können Sie eine Arbeitsauslastung von mehreren Computern wiedergeben und eine unternehmenskritische Arbeitsauslastung besser simulieren.

Das [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay-Feature kann Ablaufverfolgungsdaten mithilfe mehrerer Computer verwenden und eine unternehmenskritische Arbeitsauslastung simulieren. Verwenden des Distributed Replay für Anwendungskompatibilitätstests, Leistungstests oder die Kapazitätsplanung.

## <a name="when-to-use-distributed-replay"></a>Verwendungsbereiche von Distributed Replay

Der SQL Server Profiler und das Distributed Replay überschneiden sich in manchen Punkten.

Mit dem SQL Server Profiler können Sie eine aufgezeichnete Ablaufverfolgung in einer aktualisierten Testumgebung wiedergeben. Sie können auch die Wiedergabeergebnisse analysieren, um nach möglichen Funktions- und Leistungsinkompatibilitäten zu suchen. Mit dem SQL Server Profiler kann jedoch nur die Workload eines einzelnen Computers wiedergegeben werden. Wenn Sie eine ressourcenintensive OLTP-Anwendung mit zahlreichen gleichzeitig aktiven Verbindungen oder einem hohen Durchsatz wiedergeben, kann der SQL Server Profiler zu einem Ressourcenengpass werden.

Das Distributed Replay bietet eine stärker skalierbare Lösung als der SQL Server Profiler. Mit Distributed Replay können Sie eine Arbeitsauslastung von mehreren Computern wiedergeben und eine unternehmenskritische Arbeitsauslastung besser simulieren.

In der folgenden Tabelle ist beschrieben, wann jedes Tool verwendet werden sollte.

|Tool|Verwenden, wenn ...|
|----------|---------------|
| SQL Server Profiler | Sie möchten den herkömmlichen Wiedergabemechanismus auf einem einzelnen Computer verwenden. Insbesondere benötigen Sie zeilenweise Debugfunktionen, z.B. die Befehle **Schritt**, **Ausführen bis Cursorposition** und **Haltepunkt ein/aus** .<br /><br /> Sie möchten eine [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Ablaufverfolgung wiedergeben. |
| Distributed Replay |Sie möchten die Anwendungskompatibilität auswerten. Sie möchten z. B. Upgradeszenarien für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und das Betriebssystem, Hardwareupgrades oder die Indexoptimierung testen.<br /><br /> Die Parallelität in der aufgezeichneten Ablaufverfolgung ist so stark, dass mit einem einzelnen Wiedergabeclient keine ausreichende Simulation erzielt werden kann.|  

## <a name="distributed-replay-concepts"></a>Konzepte von Distributed Replay

Die folgenden Komponenten bilden die Distributed Replay-Umgebung:  

- **Distributed Replay–Verwaltungstool:** Eine Konsolenanwendung (**DReplay.exe**), die zur Kommunikation mit Distributed Replay Controller verwendet werden kann. Verwenden Sie das Verwaltungstool zum Steuern der verteilten Wiedergabe.  

- **Distributed Replay-Controller:** Ein Computer, auf dem der Windows-Dienst „[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay-Controller“ ausgeführt wird. Der Distributed Replay Controller koordiniert die Aktionen der Distributed Replay Clients. Es kann in jeder Distributed Replay-Umgebung jeweils nur eine Controllerinstanz geben.  

- **Distributed Replay-Clients:** Mindestens ein Computer (physisch oder virtuell), auf dem der Windows-Dienst „[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay-Client“ ausgeführt wird. Die Distributed Replay Clients simulieren gemeinsam Arbeitsauslastungen auf einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. In jeder Distributed Replay-Umgebung kann sich mindestens ein Client befinden.  

- **Zielserver**: Eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mit der Distributed Replay-Clients Ablaufverfolgungsdaten wiedergeben können. Es wird empfohlen, den Zielserver in einer Testumgebung zu platzieren.

Distributed Replay-Verwaltungstool, Controller und Client können auf verschiedenen Computern oder demselben Computer installiert werden. Auf demselben Computer kann nur eine Instanz des Distributed Replay Controller oder Client-Diensts ausgeführt werden.

In der folgenden Abbildung ist die physische Architektur von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay dargestellt:  

![Distributed Replay-Architektur](../../tools/distributed-replay/media/distributedreplayarch.gif "Distributed Replay-Architektur")  

## <a name="distributed-replay-tasks"></a>Tasks von Distributed Replay

|Taskbeschreibung|Thema|  
|----------------------|-----------|  
| Beschreibt, wie Distributed Replay konfiguriert wird. | [Konfigurieren von Distributed Replay](../../tools/distributed-replay/configure-distributed-replay.md) |
| Beschreibt, wie die Eingabedaten der Ablaufverfolgung vorbereitet werden. | [Vorbereiten der Eingabedaten für die Ablaufverfolgung](../../tools/distributed-replay/prepare-the-input-trace-data.md) |
| Beschreibt, wie die Ablaufverfolgungsdaten wiedergegeben werden. |[Wiedergeben von Ablaufverfolgungsdaten](../../tools/distributed-replay/replay-trace-data.md) | | Beschreibt, wie die Ergebnisse der Ablaufverfolgungsdaten von Distributed Replay überprüft werden. |[Überprüfen der Wiedergabeergebnisse](../../tools/distributed-replay/review-the-replay-results.md)|
| Beschreibt, wie das Verwaltungstool zum Initiieren, Überwachen und Abbrechen von Vorgängen auf dem Controller verwendet wird. | [Befehlszeilenoptionen für das Verwaltungstool &#40;Distributed Replay Utility&#41;](../../tools/distributed-replay/administration-tool-command-line-options-distributed-replay-utility.md) |

## <a name="see-also"></a>Weitere Informationen

[Forum für das Distributed Replay im Zusammenhang mit SQL Server](https://social.technet.microsoft.com/Forums/sl/sqldru/)