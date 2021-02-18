---
title: Speichern von Ablaufverfolgungen und Ablaufverfolgungsvorlagen
titleSuffix: SQL Server Profiler
description: Erfahren Sie, wie Sie erfasste Ereignisdaten in einer Ablaufverfolgungsdatei oder einer Datenbanktabelle in SQL Server Profiler speichern und wie benutzerdefinierte Ablaufverfolgungsvorlagen gespeichert werden.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 957e6bf8-e7a3-4a59-a1cd-0a41538a8158
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: c75f865ed31e43f2fbdec96cfc65fcbd9619c1d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345769"
---
# <a name="save-traces-and-trace-templates"></a>Speichern von Ablaufverfolgungen und Ablaufverfolgungsvorlagen

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Es ist wichtig, zwischen dem Speichern von Ablaufverfolgungsdateien und dem Speichern von Ablaufverfolgungsvorlagen zu unterscheiden. Beim Speichern einer Ablaufverfolgungsdatei werden die aufgezeichneten Ereignisdaten an einem angegebenen Speicherort gespeichert. Beim Speichern einer Ablaufverfolgungsvorlage wird die Ablaufverfolgungsdefinition gespeichert, wie z. B. angegebene Datenspalten, Ereignisklassen oder Filter.  
  
## <a name="saving-traces"></a>Speichern von Ablaufverfolgungen  
 Speichern Sie die aufgezeichneten Ereignisdaten in einer Datei oder einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Tabelle, wenn Sie die aufgezeichneten Daten später analysieren oder wiedergeben müssen. Verwenden Sie eine Ablaufverfolgungsdatei folgendermaßen:  
  
-   Verwenden Sie eine Ablaufverfolgungsdatei oder eine Ablaufverfolgungstabelle, um eine Arbeitsauslastung zu erstellen, die als Eingabe für den Datenbankoptimierungsratgeber verwendet wird.  
  
-   Verwenden Sie eine Ablaufverfolgungsdatei, um Ereignisse aufzuzeichnen und die Ablaufverfolgungsdatei an den Technischen Support zur Analyse zu senden.  
  
-   Verwenden Sie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Tools zur Abfrageverarbeitung für den Zugriff auf die Daten oder zum Anzeigen der Daten in [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Es können nur Mitglieder der festen Serverrolle **sysadmin** oder die Person, die die Tabelle erstellt hat, direkt auf die Ablaufverfolgungstabelle zugreifen.  
  
> [!NOTE]  
>  Das Aufzeichnen von Ablaufverfolgungsdaten in einer Tabelle erfolgt langsamer als in einer Datei. Alternativ können Ablaufverfolgungsdaten in einer Datei aufgezeichnet werden, die Ablaufverfolgungsdatei geöffnet und die Ablaufverfolgung dann als Ablaufverfolgungstabelle gespeichert werden.  
  
 Wenn Sie eine Ablaufverfolgungsdatei verwenden, speichert [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] die aufgezeichneten Ereignisdaten (keine Ablaufverfolgungsdefinitionen) in einer [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] -Ablaufverfolgungsdatei (\*.trc). Die Erweiterung wird automatisch an das Ende der Datei angefügt, wenn die Ablaufverfolgungsdatei gespeichert wird, unabhängig von anderen angegebenen Erweiterungen. Wenn Sie z. B. eine Ablaufverfolgungsdatei namens **Trace.dat** angeben, erhält die Datei den Namen **Trace.dat.trc**.  
  
> [!IMPORTANT]  
>  Benutzer mit den Berechtigungen SHOWPLAN, ALTER TRACE oder VIEW SERVER STATE können Abfragen anzeigen, die in der Showplan-Ausgabe erfasst werden. Diese Abfragen enthalten möglicherweise vertrauliche Informationen wie Kennwörter. Daher wird empfohlen, diese Berechtigungen nur Benutzern zu gewähren, die zum Zugreifen auf vertrauliche Informationen berechtigt sind, z.B. Mitglieder der festen Datenbankrolle **db_owner** oder Mitglieder der festen Serverrolle **sysadmin** . Darüber hinaus wird empfohlen, Showplan-Dateien oder Ablaufverfolgungsdateien, die Ereignisse mit Bezug zu Showplan enthalten, nur an einem Speicherort zu speichern, für den das NTFS-Dateisystem verwendet wird, und den Zugriff auf Benutzer zu beschränken, die zum Zugreifen auf vertrauliche Informationen berechtigt sind.  
  
## <a name="saving-templates"></a>Speichern von Vorlagen  
 Die Vorlagendefinition einer Ablaufverfolgung enthält die Ereignisklassen, Datenspalten, Filter und alle anderen Eigenschaften (außer den aufgezeichneten Ereignisdaten), mit denen eine Ablaufverfolgung erstellt werden kann. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] stellt vordefinierte Systemvorlagen für allgemeine Ablaufverfolgungstasks und für spezielle Tasks bereit, z. B. für das Erstellen einer Arbeitsauslastung, mit der der Datenbankoptimierungsratgeber den physischen Datenbankentwurf optimieren kann. Sie können auch benutzerdefinierte Vorlagen erstellen und speichern.  
  
### <a name="importing-and-exporting-templates"></a>Importieren und Exportieren von Vorlagen  
 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] ermöglicht das Importieren und Exportieren von Vorlagen zwischen Servern. Beim Exportieren einer Vorlage wird eine Kopie einer vorhandenen Vorlage in ein von Ihnen angegebenes Verzeichnis verschoben. Beim Importieren einer Vorlage wird eine Kopie einer von Ihnen angegebenen Vorlage erstellt. Wenn diese Vorlagen in [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] angezeigt werden, können Sie sie anhand des an den Vorlagennamen angefügten Begriffs „(user)“ (Benutzer) von Systemvorlagen unterscheiden. Sie können eine vordefinierte Systemvorlage nicht überschreiben oder direkt ändern.  
  
### <a name="analyzing-performance-with-templates"></a>Analysieren der Leistung mit Vorlagen  
 Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]häufig überwachen, sollten Sie zur Analyse der Leistung Vorlagen verwenden. Die Vorlagen zeichnen jedes Mal dieselben Ereignisdaten auf und verwenden dieselbe Ablaufverfolgungsdefinition zum Überwachen der gleichen Ereignisse. Sie müssen die Ereignisklassen und Datenspalten nicht jedes Mal neu definieren, wenn Sie eine Ablaufverfolgung erstellen. Darüber hinaus kann eine Vorlage an einen anderen Benutzer weitergegeben werden, der bestimmte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Ereignisse überwachen möchte. So kann z. B. der Technische Support einem Kunden eine Vorlage zur Verfügung stellen. Der Kunde verwendet die Vorlage zur Aufzeichnung der erforderlichen Ereignisdaten, die dann zur Analyse an den Anbieter für technischen Support gesendet werden.  
  
 **So speichern Sie eine Ablaufverfolgung in einer Datei**  
  
 [Speichern von Ablaufverfolgungsergebnissen in einer Datei &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/save-trace-results-to-a-file-sql-server-profiler.md)  
  
 [sp_trace_create &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Speichern von Ablaufverfolgungsergebnissen in einer Tabelle &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/save-trace-results-to-a-table-sql-server-profiler.md)   
 [Erstellen einer Ablaufverfolgungsvorlage &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/create-a-trace-template-sql-server-profiler.md)   
 [Ableiten einer Vorlage von einer zurzeit ausgeführten Ablaufverfolgung &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-running-trace-sql-server-profiler.md)   
 [Ableiten einer Vorlage von einer Ablaufverfolgungsdatei oder Ablaufverfolgungstabelle &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-trace-file-or-trace-table-sql-server-profiler.md)   
 [Exportieren einer Ablaufverfolgungsvorlage &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/export-a-trace-template-sql-server-profiler.md)   
 [Importieren einer Ablaufverfolgungsvorlage &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/import-a-trace-template-sql-server-profiler.md)  
  
  
