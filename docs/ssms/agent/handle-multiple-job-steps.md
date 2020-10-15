---
description: Handhaben mehrerer Auftragsschritte
title: Handhaben mehrerer Auftragsschritte
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- job steps [SQL Server Agent]
- ordering job steps [SQL Server]
- multiple job steps
- SQL Server Agent jobs, job steps
- control of flow for jobs [SQL Server]
ms.assetid: 7aba19ff-72b3-45f6-8e54-23f4988d63a8
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: d331ee59e933f6a9b06ea3c7280b5529f0604a37
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92037350"
---
# <a name="handle-multiple-job-steps"></a>Handhaben mehrerer Auftragsschritte
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Wenn Ihr Auftrag aus mehr als einem Auftragsschritt besteht, müssen Sie die Reihenfolge angeben, in der die Auftragsschritte ausgeführt werden. Dies wird *Ablaufsteuerung* genannt. Sie können jederzeit neue Auftragsschritte hinzufügen und den Ablauf der Auftragsschritte neu ordnen. Die Änderungen werden bei der nächsten Ausführung des Auftrags wirksam. In dieser Abbildung ist die Ablaufsteuerung eines Auftrags für die Datenbanksicherung dargestellt.  
  
![Ablaufsteuerung bei den Schritten eines SQL Server-Agent-Auftrags](../../ssms/agent/media/dbflow01.gif "Ablaufsteuerung bei den Schritten eines SQL Server-Agent-Auftrags")  
  
Der erste Schritt besteht in der Datenbanksicherung. Wenn dieser Schritt einen Fehler erzeugt, wird vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent ein Fehler an den Ausführenden berichtet, der als Empfänger der Benachrichtigung definiert ist. Wenn der Schritt zur Datenbanksicherung erfolgreich ist, wird vom Auftrag der nächste Schritt ausgeführt, das "Bereinigen" der Kundendaten. Wenn dieser Schritt einen Fehler erzeugt, wird vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent der Ablauf mit der Datenbankwiederherstellung fortgesetzt. Wenn die Bereinigung der Benutzerdaten erfolgreich ist, wird der Auftrag mit dem nächsten Schritt fortgesetzt, beispielsweise Aktualisieren der Statistik und so weiter, bis der letzte Schritt entweder einen Erfolg oder einen Fehler des Berichts zum Ergebnis hat.  
  
Sie definieren eine Ablaufsteuerungsaktion für die erfolgreiche oder fehlgeschlagene Ausführung einzelner Auftragsschritte. Sie müssen eine Aktion angeben, die durchgeführt werden soll, wenn ein Auftragsschritt erfolgreich ausgeführt wird, und eine Aktion für den Fall, dass ein Auftragsschritt einen Fehler erzeugt. Sie können zudem die Anzahl der Wiederholungsversuche und der Intervalle zwischen diesen Versuchen bei fehlgeschlagenen Auftragsschritten definieren.  
  
> [!NOTE]  
> Wenn Sie einen oder mehrere Schritte über die grafische Benutzeroberfläche (Graphical User Interface, GUI) des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents aus einem Auftrag mit mehreren Schritten löschen, entfernt die GUI alle Auftragsschritte und fügt die verbleibenden Schritte mit den richtigen Verweisen bei Erfolg oder bei Fehler wieder hinzu. Angenommen, ein Auftrag besteht aus fünf Schritten, und der erste Schritt ist so konfiguriert, dass er bei erfolgreichem Abschluss zu Schritt 4 springt. Wenn Sie Schritt 3 löschen, entfernt die GUI alle Schritte für diesen Auftrag und fügt die verbleibenden Schritte (1, 2, 4 und 5) mit den korrigierten Verweisen wieder hinzu. In diesem Fall wird der Verweis in Schritt 1 neu konfiguriert, damit der Vorgang bei erfolgreichem Abschluss von Schritt 1 mit Schritt 3 fortgesetzt wird.  
  
Auftragsschritte müssen eigenständig sein. Ein Auftrag kann keine booleschen Werte, Daten oder numerischen Werte zwischen den Auftragsschritten übergeben. Sie können allerdings Werte von einem [!INCLUDE[tsql](../../includes/tsql-md.md)] -Auftragsschritt zu einem anderen mithilfe von dauerhaften Tabellen oder globalen temporären Tabellen übergeben. Sie können Werte von Auftragsschritten, die Programme von einem Auftragsschritt zum nächsten ausführen, mithilfe von Dateien übergeben. Von einer Datei, die von einem Auftragsschritt ausgeführt wird, kann beispielsweise in eine Datei geschrieben werden, und von einer Datei, die von einem nachfolgenden Auftragsschritt ausgeführt wird, kann die Datei gelesen werden.  
  
> [!NOTE]  
> Sollten Sie Auftragsschritte erstellen, die sich in einer Schleife gegenseitig aufrufen (auf Auftragsschritt 1 folgt Auftragsschritt 2, dann kehrt Auftragsschritt 2 zu Auftragsschritt 1 zurück), wird eine Warnmeldung angezeigt, wenn der Auftrag mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]erstellt wird.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent zeichnet die Informationen von Aufträgen und Auftragsschritten im Auftragsverlauf auf.  
  
## <a name="see-also"></a>Weitere Informationen  
[sp_add_job](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  
[sysjobhistory](../../relational-databases/system-tables/dbo-sysjobhistory-transact-sql.md)  
[sysjobs (Transact-SQL)](../../relational-databases/system-tables/dbo-sysjobs-transact-sql.md)  
[sysjobsteps](../../relational-databases/system-tables/dbo-sysjobsteps-transact-sql.md)  
[Implementieren von Aufträgen](../../ssms/agent/implement-jobs.md)  
[Verwalten von Auftragsschritten](../../ssms/agent/manage-job-steps.md)  
