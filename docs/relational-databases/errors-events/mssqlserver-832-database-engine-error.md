---
description: MSSQLSERVER_832
title: MSSQLSERVER_832
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 832 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: ffd0cce51495b497cfb1a3ac2948f01a2a242e52
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348681"
---
# <a name="mssqlserver_832"></a>MSSQLSERVER_832
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|832|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|B_CONSTPAGECHANGED|
|Meldungstext|Eine Seite, die konstant sein sollte, wurde geändert (erwartete Prüfsumme: \<expected value>, tatsächliche Prüfsumme: \<actual value>, Datenbank \<dbid>, Datei \'<filename>', Seite \<pageno>). Dies weist normalerweise auf einen Arbeitsspeicherfehler oder auf Schäden an der Hardware oder dem Betriebssystem hin.|
||

## <a name="explanation"></a>Erklärung

Ein externer Faktor hat bewirkt, dass eine Datenbankseite außerhalb des normalen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Engine-Codes geändert wird, der zum Ändern von Datenbankseiten verwendet wird.  Die Bedingungen können wie folgt lauten:  

- Ein Thread, der im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Prozess ausgeführt wird und auf einer Datenbankseite falsche Schreibvorgänge durchführt. Dieser wird häufig als „Scribbler“ bezeichnet.
- Ein Problem mit der Hardware oder dem Betriebssystem, bei dem der Arbeitsspeicher, der die Datenbankseite stützt, nicht korrekt geändert wurde oder beschädigt ist.  

Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] diesen Fehler erkennt, wird der Fehler 832 ausgelöst.

## <a name="user-action"></a>Benutzeraktion

Sie haben die folgenden Möglichkeiten, um die Ursache des Fehlers zu ermitteln:

- Sie sollten normale Hardware- oder Systemüberprüfungen durchführen, um herauszufinden, ob ein Problem mit dem Arbeitsspeicher oder der CPU oder ein anderes Hardwareproblem vorliegt. Stellen Sie sicher, dass Sie über alle Systemtreiber verfügen und Ihr Betriebssystem und Ihre Hardware auf dem neusten Stand sind. Führen Sie ggf. eine beliebige Diagnose eines Hardwareherstellers einschließlich speicherbezogener Tests durch.
- Prüfen Sie, welche externen DLLs in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] geladen werden können, auf die dieses Problem zurückzuführen sein könnte, einschließlich erweiterter gespeicherter Prozeduren, COM-Objekte oder anderer DLLs, die möglicherweise falsche Änderungen am [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Arbeitsspeicher vornehmen, der für Datenbankseiten reserviert ist.  

Wenn dieser Fehler angezeigt wird, sollten Sie sofort `DBCC CHECKDB` für die Datenbank ausführen, auf die von \<dbid> in der Fehlermeldung verwiesen wird.

## <a name="more-information"></a>Weitere Informationen

Dieser Fehler wird von dem Hintergrundtask erkannt, der häufig als „LazyWriter“ bezeichnet wird. (Der „Befehl“ für diesen Task lautet LAZY WRITER.) Daher wird dieser Fehler nicht an eine Clientanwendung zurückgegeben. Der Fehler wird in das Windows-Anwendungsereignisprotokoll als EventID=832 geschrieben.  

Nur Seiten, die derzeit nicht im Cache geändert werden, werden geprüft. Der Grund hierfür ist, dass die Meldung den Begriff „konstant“ verwendet, da die Seite nie geändert wurde, nachdem sie vom Datenträger gelesen wurde. Außerdem wurde die Seite fehlerfrei vom Datenträger gelesen, da sie über einen Prüfsummenwert auf der Seite verfügt und kein Prüfsummenfehler aufgetreten ist (Meldung 824). Die Seite kann jedoch zu einem bestimmten Zeitpunkt nach dem Auftreten dieses Fehlers geändert und dann mit der falschen Änderung auf den Datenträger geschrieben werden. Wenn dieser Fehler auftritt, wird basierend auf allen Änderungen eine neue Prüfsumme berechnet, bevor sie auf den Datenträger geschrieben wird. Deshalb kann die Seite zwar auf dem Datenträger beschädigt worden sein, aber ein anschließender Lesevorgang vom Datenträger löst möglicherweise keinen Prüfsummenfehler aus. Es ist wichtig, `DBCC CHECKDB` für jede Datenbank auszuführen, auf die dieser Fehler verweist.  

Es ist möglich, dass auch der Befehl `DBCC CHECKDB` für eine Seite in diesem Zustand keinen Fehler meldet, nachdem dieser auf den Datenträger geschrieben wurde. Der Grund hierfür ist, dass sich die falsche Änderung an Stellen auf der Seite befinden kann, die weder Daten noch wichtige Informationen zur Seiten- oder Zeilenstruktur enthalten. Alternativ kann es sich auch um Änderungen an Daten handelt, die CHECKDB nicht erkennen kann.  

Weitere Details und Informationen zur Meldung 832 finden Sie im Whitepaper [E/A-Grundlagen für SQL Server, Kapitel 2](/previous-versions/sql/sql-server-2005/administrator/cc917726(v=technet.10)).