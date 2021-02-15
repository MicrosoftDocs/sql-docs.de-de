---
title: Eigenschaften für neue Auftragsschritte (Seite „Erweitert“)
description: Auftragsschritt-Eigenschaften – Neuer Auftragsschritt (Seite „Erweitert“)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.job.stepadvanced.f1
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 2120c2f5d86c4d8ad4c549f95ba8090574506015
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978836"
---
# <a name="job-step-properties---new-job-step-advanced-page"></a>Auftragsschritt-Eigenschaften – Neuer Auftragsschritt (Seite „Erweitert“)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> In [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) werden derzeit die meisten, aber nicht alle, SQL Server-Agent-Features unterstützt. Details dazu finden Sie unter [T-SQL-Unterschiede zwischen Azure SQL Managed Instance und SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Mithilfe dieser Seite können Sie die Eigenschaften für einen [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]-Agentauftragsschritt anzeigen und ändern.  
  
## <a name="options"></a>Optionen  
**Aktion bei Erfolg**  
Legt fest, welche Aktion der [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Agent bei erfolgreicher Auftragsausführung ausführen soll.  
  
**Wiederholungsversuche**  
Legt fest, wie oft vom [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Agent versucht werden soll, einen fehlgeschlagenen Auftragsschritt erneut auszuführen.  
  
**Wiederholungsintervall (Min)**  
Legt fest, wie lange der [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Agent bis zum nächsten Wiederholungsversuch warten soll.  
  
**Aktion bei Fehler**  
Legt fest, welche Aktion der [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Agent bei Fehlschlagen des Auftrags ausführen soll.  
  
## <a name="options-for-transact-sql-job-steps"></a>Optionen für Transact-SQL-Auftragsschritte  
**Ausgabedatei**  
Legt fest, welche Datei für die Ausgabe aus dem Auftragsschritt verwendet werden soll. Diese Option ist nur für Mitglieder der festen Serverrolle **sysadmin** verfügbar.  
  
**...**  
Nach der Datei, die für die Ausgabe aus dem Auftragsschritt verwendet werden soll, können Sie suchen.  
  
**Ansicht**  
In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]ist diese Schaltfläche für die Anzeige von Ausgabedateien deaktiviert. Nutzen Sie stattdessen den Editor für die Anzeige von Auftragsschritt-Ausgabedateien.  
  
**Ausgabe an vorhandene Datei anfügen**  
Fügt die Ausgabe an den vorhandenen Inhalt der Datei an. Andernfalls wird der vorige Inhalt der Datei bei jeder Ausführung des Auftragsschritts überschrieben.  
  
**In Tabelle protokollieren**  
Protokolliert die Ausgabedaten des Auftragsschritts in der **sysjobstepslogs**-Tabelle der **msdb**-Datenbank.  
  
**Ansicht**  
Klicken Sie nach Ausführung des Auftragsschritts auf **Anzeigen**, um seine Ausgabe in der Tabelle anzuzeigen.  
  
**Ausgabe an vorhandenen Eintrag in Tabelle anfügen**  
Fügt die Ausgabe an den vorhandenen Inhalt der Tabelle an. Andernfalls wird der vorige Inhalt der Tabelle bei jeder Ausführung des Auftragsschritts überschrieben.  
  
**Schrittausgabe in Verlauf einschließen**  
Wählen Sie diese Option aus, wenn die Ausgabe des Auftragsschritts in den Auftragsverlauf aufgenommen werden soll.  
  
**Ausführen als Benutzer**  
Wenn Sie Mitglied der festen Serverrolle **sysadmin** sind, können Sie für die Ausführung dieses Auftragsschritts einen anderen SQL-Anmeldenamen auswählen.  
  
## <a name="options-for-operating-system-cmdexec-job-steps"></a>Optionen für Auftragsschritte des Betriebssystems (CmdExec)  
**Ausgabedatei**  
Legt fest, welche Datei für die Ausgabe aus dem Auftragsschritt verwendet werden soll.  
  
**...**  
Nach der Datei, die für die Ausgabe aus dem Auftragsschritt verwendet werden soll, können Sie suchen.  
  
**Ansicht**  
In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]ist diese Schaltfläche für die Anzeige von Ausgabedateien deaktiviert. Nutzen Sie stattdessen den Editor für die Anzeige von Auftragsschritt-Ausgabedateien.  
  
**Ausgabe an vorhandene Datei anfügen**  
Fügt die Auftragsschrittausgabe bei jeder Ausführung des Schritts an den vorhandenen Inhalt der Datei an  
  
**In Tabelle protokollieren**  
Protokolliert die Ausgabedaten des Auftragsschritts in der **sysjobstepslogs**-Tabelle der **msdb**-Datenbank.  
  
**Ansicht**  
Klicken Sie nach Ausführung des Auftragsschritts auf **Anzeigen**, um seine Ausgabe in der Tabelle anzuzeigen.  
  
**Ausgabe an vorhandenen Eintrag in Tabelle anfügen**  
Fügt die Ausgabe an den vorhandenen Inhalt der Tabelle an. Andernfalls wird der vorige Inhalt der Tabelle bei jeder Ausführung des Auftragsschritts überschrieben.  
  
**Schrittausgabe in Verlauf einschließen**  
Wählen Sie diese Option aus, wenn die Ausgabe des Auftragsschritts in den Auftragsverlauf aufgenommen werden soll.  
  
## <a name="options-for-powershell-job-steps"></a>Optionen für PowerShell-Auftragsschritte  
**Ausgabedatei**  
Legt fest, welche Datei für die Ausgabe aus dem Auftragsschritt verwendet werden soll.  
  
**...**  
Nach der Datei, die für die Ausgabe aus dem Auftragsschritt verwendet werden soll, können Sie suchen.  
  
**Ansicht**  
In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]ist diese Schaltfläche für die Anzeige von Ausgabedateien deaktiviert. Nutzen Sie stattdessen den Editor für die Anzeige von Auftragsschritt-Ausgabedateien.  
  
**Ausgabe an vorhandene Datei anfügen**  
Fügt die Auftragsschrittausgabe bei jeder Ausführung des Schritts an den vorhandenen Inhalt der Datei an  
  
**In Tabelle protokollieren**  
Protokolliert die Ausgabedaten des Auftragsschritts in der **sysjobstepslogs**-Tabelle der **msdb**-Datenbank.  
  
**Ansicht**  
Klicken Sie nach Ausführung des Auftragsschritts auf **Anzeigen**, um seine Ausgabe in der Tabelle anzuzeigen.  
  
**Ausgabe an vorhandenen Eintrag in Tabelle anfügen**  
Fügt die Ausgabe an den vorhandenen Inhalt der Tabelle an. Andernfalls wird der vorige Inhalt der Tabelle bei jeder Ausführung des Auftragsschritts überschrieben.  
  
**Schrittausgabe in Verlauf einschließen**  
Wählen Sie diese Option aus, wenn die Ausgabe des Auftragsschritts in den Auftragsverlauf aufgenommen werden soll.  
  
## <a name="options-for-replication-queue-reader-job-steps"></a>Optionen für Auftragsschritte des Replikation-Warteschlangenlesers  
**Server**  
Legt fest, welcher Server für einen Auftragsschritt des Replikation-Warteschlangenlesers verwendet werden soll.  
  
**Datenbank**  
Legt fest, welche Datenbank für einen Auftragsschritt des Replikation-Warteschlangenlesers verwendet werden soll.  
  
## <a name="options-for-sql-server-analysis-services-job-steps"></a>Optionen für Auftragsschritte von SQL Server Analysis Services  
**Ausgabedatei**  
Legt fest, welche Datei für die Ausgabe aus dem Auftragsschritt verwendet werden soll. Diese Option ist nur für Mitglieder der festen Serverrolle **sysadmin** verfügbar.  
  
**...**  
Nach der Datei, die für die Ausgabe aus dem Auftragsschritt verwendet werden soll, können Sie suchen.  
  
**Ansicht**  
In [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]ist diese Schaltfläche für die Anzeige von Ausgabedateien deaktiviert. Nutzen Sie stattdessen den Editor für die Anzeige von Auftragsschritt-Ausgabedateien.  
  
**Ausgabe an vorhandene Datei anfügen**  
Fügt die Ausgabe an den vorhandenen Inhalt der Datei an. Andernfalls wird der vorige Inhalt der Datei bei jeder Ausführung des Auftragsschritts überschrieben.  
  
**In Tabelle protokollieren**  
Protokolliert die Ausgabedaten des Auftragsschritts in der **sysjobstepslogs**-Tabelle der **msdb**-Datenbank.  
  
**Ansicht**  
Klicken Sie nach Ausführung des Auftragsschritts auf **Anzeigen**, um seine Ausgabe in der Tabelle anzuzeigen.  
  
**Ausgabe an vorhandenen Eintrag in Tabelle anfügen**  
Fügt die Ausgabe an den vorhandenen Inhalt der Tabelle an. Andernfalls wird der vorige Inhalt der Tabelle bei jeder Ausführung des Auftragsschritts überschrieben.  
  
**Schrittausgabe in Verlauf einschließen**  
Wählen Sie diese Option aus, wenn die Ausgabe des Auftragsschritts in den Auftragsverlauf aufgenommen werden soll.  
  
## <a name="see-also"></a>Weitere Informationen

- [Verwalten von Auftragsschritten](manage-job-steps.md)
