---
title: SQL Server 2008 R2 SP2 Release Notes | Microsoft-Dokumentation
description: In diesem Dokument mit Versionsanmerkungen werden bekannte Probleme beschrieben, mit denen Sie sich vertraut machen sollten, bevor Sie Microsoft SQL Server 2008 R2 Service Pack 2 installieren bzw. mit der Problembehandlung beginnen.
ms.prod: sql
ms.technology: release-landing
ms.custom: ''
ms.date: 07/22/2020
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL Server 2008 R2 SP2
- Release Notes, SQL Server 2008 R2 SP2
ms.assetid: e2bd3de7-674c-4ea7-8d53-bb40bba86fae
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: 64ae8aad3ca2d5c8fc3b1efc216627a849918431
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271767"
---
# <a name="sql-server-2008-r2-sp2-release-notes"></a>SQL Server 2008 R2 SP2 Release Notes
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
In diesem Dokument mit Versionsanmerkungen werden bekannte Probleme beschrieben, mit denen Sie sich vertraut machen sollten, bevor Sie Microsoft SQL Server 2008 R2 Service Pack 2 installieren bzw. mit der Problembehandlung beginnen. Diese Versionsanmerkungen gelten für alle Editionen von SQL Server 2008 R2 SP2 und sind nur online verfügbar. Das Dokument wird regelmäßig aktualisiert.  
  
## <a name="10-whats-new-in-service-pack-2"></a>1.0 Neues in Service Pack 2  
Die dynamische Verwaltungsansicht (DMV) **sys.dm_db_stats_properties** wurde hinzugefügt. Mit dieser DMV können Statistikeigenschaften für eine angegebene Tabelle oder indizierte Sicht in der aktuellen Datenbank zurückgegeben werden. Beispielsweise gibt diese DMV die Anzahl der als Stichprobe entnommenen Zeilen und die Anzahl der Schritte im Histogramm zurück.  
  
## <a name="20-before-you-install"></a>2.0 Vor der Installation  
Informationen zum Installieren von [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)] -Updates finden Sie in der [SQL Server 2008 R2-Wartungsdokumentation](/previous-versions/sql/sql-server-2008-r2/dd638062(v=sql.105)).  
  
Allgemeine Informationen zu den ersten Schritten sowie zur Installation von SQL Server 2008 R2 finden Sie in der SQL Server 2008 R2-Infodatei. Die Infodatei steht auf dem Installationsmedium zur Verfügung.
  
### <a name="21-choose-the-correct-file-to-download-and-install"></a>2.1 Auswählen der richtigen Download- und Installationsdatei  
Bestimmen Sie mithilfe der folgenden Tabelle, welche Datei heruntergeladen und installiert werden muss. Stellen Sie sicher, dass die Systemanforderungen erfüllt sind, bevor Sie das Service Pack installieren. Die Systemanforderungen sind auf den Downloadseiten angegeben, die mit den Links in der Tabelle verknüpft sind.  
  
|Aktuell installierte Version...|Gewünschter Vorgang...|Download und Installation von...|  
|-------------------------------------------|----------------------|---------------------------|  
|32-Bit-Version einer beliebigen Edition von SQL Server 2008 R2 oder SQL Server 2008 R2 SP1|Upgrade auf die 32-Bit-Version von SQL Server 2008 R2 SP2|SQLServer2008R2SP2-KB2630458-x86-ENU über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkId=251790)|  
|32-Bit-Version von SQL Server 2008 R2 RTM Express oder SQL Server 2008 R2 SP1 Express|Upgrade auf die 32-Bit-Version von SQL Server 2008 R2 SP2|SQLServer2008R2SP2-KB2630458-x86-ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkId=251790)|  
|32-Bit-Version nur der Client- und Verwaltbarkeitstools für SQL Server 2008 R2 oder SQL Server 2008 R2 SP1 (einschließlich SQL Server 2008 R2 Management Studio)|Upgrade der Client- und Verwaltbarkeitstools auf die 32-Bit-Version von SQL Server 2008 R2 SP2|SQLServer2008R2SP2-KB2630458-x86-ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkId=251790)|  
|32-Bit-Version von SQL Server 2008 R2 Management Studio Express oder SQL Server 2008 R2 SP1 Management Studio Express|Upgrade auf die 32-Bit-Version von SQL Server 2008 R2 SP2 Management Studio Express|SQLManagementStudio_x86_ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkId=251791)|  
|32-Bit-Version einer beliebigen Edition von SQL Server 2008 R2 oder SQL Server 2008 R2 SP1 **und** 32-Bit-Version der Client- und Verwaltbarkeitstools (einschließlich SQL Server 2008 R2 RTM Management Studio)|Upgrade aller Produkte auf die 32-Bit-Version von SQL Server 2008 R2 SP2|SQLServer2008R2SP2-KB2630458-x86-ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkId=251790)|  
|32-Bit-Version von mindestens einem Tool aus dem [Microsoft SQL Server 2008 R2 RTM Feature Pack](https://www.microsoft.com/download/details.aspx?id=44272)|Upgrade der Tools auf die 32-Bit-Version von Microsoft SQL Server 2008 R2 SP2 Feature Pack|Mindestens eine Datei aus dem [Microsoft SQL Server 2008 R2 SP2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=30438)|  
|Keine 32-Bit-Installation von SQL Server 2008 R2|Installation von SQL Server 2008 R2 einschließlich SP2|Navigieren Sie zu [SQL Server 2008 R2 SP2, Express Edition](https://go.microsoft.com/fwlink/?LinkId=251791), und befolgen Sie die Anweisungen.|  
|Keine 32-Bit-Installation von SQL Server 2008 R2 Management Studio|Installation von SQL Server 2008 R2 Management Studio einschließlich SP2|SQLManagementStudio_x86_ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkId=251791) zur Installation der kostenlosen SQL Server 2008 R2 SP2 Management Studio Express Edition.|  
|64-Bit-Version einer beliebigen Edition von SQL Server 2008 R2 oder SQL Server 2008 R2 SP1|Upgrade auf die 64-Bit-Version von SQL Server 2008 R2 SP2|SQLServer2008R2SP2-KB2630458-x64-ENU oder SQLServer2008R2SP2-KB2630455-IA64-ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkId=251790)|  
|64-Bit-Version von SQL Server 2008 R2 RTM Express oder SQL Server 2008 R2 SP1 Express|Upgrade auf die 64-Bit-Version von SQL Server 2008 R2 SP2|SQLServer2008R2SP2-KB2630458-x64-ENU.exe oder SQLServer2008R2SP2-KB2630455-IA64-ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkId=251790)|  
|64-Bit-Version ausschließlich der Client- und Verwaltbarkeitstools für SQL Server 2008 R2 oder SQL Server 2008 R2 SP1 (einschließlich SQL Server 2008 R2 Management Studio)|Upgrade der Client- und Verwaltbarkeitstools auf die 64-Bit-Version von SQL Server 2008 R2 SP2|SQLServer2008R2SP2-KB2630458-x64-ENU.exe oder SQLServer2008R2SP2-KB2630455-IA64-ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkId=251790)|  
|64-Bit-Version von SQL Server 2008 R2 Management Studio Express oder SQL Server 2008 R2 SP1 Management Studio Express|Upgrade auf die 64-Bit-Version von SQL Server 2008 R2 SP2 Management Studio Express|SQLManagementStudio_x64_ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkId=251791)|  
|64-Bit-Version einer beliebigen Edition von SQL Server 2008 R2 oder SQL Server 2008 R2 SP1 **und** 64-Bit-Version der Client- und Verwaltbarkeitstools (einschließlich SQL Server 2008 R2 RTM Management Studio)|Upgrade aller Produkte auf die 64-Bit-Version von SQL Server 2008 R2 SP2|SQLServer2008R2SP2-KB2630458-x64-ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkId=251790)|  
|64-Bit-Version von mindestens einem Tool aus dem [Microsoft SQL Server 2008 R2 RTM Feature Pack](https://www.microsoft.com/download/details.aspx?id=44272)|Upgrade der Tools auf die 64-Bit-Version von Microsoft SQL Server 2008 R2 SP2 Feature Pack|Mindestens eine Datei aus dem [Microsoft SQL Server 2008 R2 SP2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=30438)|  
|Keine 64-Bit-Installation von SQL Server 2008 R2|Installation von SQL Server 2008 R2 einschließlich SP2|Navigieren Sie zu [SQL Server 2008 R2 SP2, Express Edition](https://go.microsoft.com/fwlink/?LinkId=251791), und befolgen Sie die Anweisungen.|  
|Keine 64-Bit-Installation von SQL Server 2008 R2 Management Studio|Installation von SQL Server 2008 R2 Management Studio einschließlich SP2|SQLManagementStudio_x64_ENU.exe über [diesen Link](https://go.microsoft.com/fwlink/p/?LinkId=251791) zur Installation der kostenlosen SQL Server 2008 R2 SP2 Management Studio Express Edition.|  
  
### <a name="22-setup-might-fail-if-sqagtresdll-is-locked-by-another-process"></a>2.2 Setup verursacht u. U. einen Fehler, wenn "SQAGTRES.dll" von einem anderen Prozess gesperrt wurde  
**Problem:** Ein SQL Server-Setupvorgang kann mit diesem Fehler fehlschlagen: `Upgrading of cluster resource C:\Program Files\Microsoft SQL Server\MSSQL10_50.<Instance name>\MSSQL\Binn\SQAGTRES.DLL on machine <Computer name> failed with Win32Exception. Please look at inner exception for details.` Die Grundursache liegt daran, dass „C:\Windows\system32\SQAGTRES.DLL“ von einem anderen Prozess gesperrt wurde und folglich von Setup nicht aktualisiert werden konnte.  
  
**Problemumgehung**: Benennen Sie „C:\Windows\system32\SQAGTRES.DLL“ vorübergehend beispielsweise in „C:\Windows\system32\SQAGTRES_old.DLL“ um, und wählen Sie dann für die Fehlermeldung in Setup die Option „Wiederholen“ aus. Auf diese Weise kann Setup fortgesetzt werden. Nach einem Neustart können Sie die temporäre Datei C:\Windows\system32\SQAGTRES_old.DLL löschen.  
  
## <a name="30-known-issues-fixed-in-this-service-pack"></a>3.0 Bekannte Probleme, die in diesem Service Pack behoben wurden  
Eine vollständige Liste von Fehlern und bekannten Problemen, die in diesem Service Pack behoben wurden, finden Sie in diesem [Master KB-Artikel](https://support.microsoft.com/kb/2630455).  
  
## <a name="see-also"></a>Weitere Informationen  
[Ermitteln der Version und Edition von SQL Server](https://support.microsoft.com/kb/321185)  
