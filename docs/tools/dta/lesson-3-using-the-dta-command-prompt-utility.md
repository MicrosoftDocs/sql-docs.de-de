---
title: Verwenden des dta-Befehlszeilenprogramms
description: Hier erfahren Sie mehr über die Funktionen, die das Befehlszeilenprogramm dta zusätzlich zu den vom SQL Server-Datenbankoptimierungsratgeber bereitgestellten Funktionen bietet.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], tutorials
ms.assetid: 30f27f4d-8852-4b12-ba62-57f63e496f1d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: aed8df33148a93d75fc5133f12be8e24b35bf599
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353501"
---
# <a name="lesson-3-using-the-dta-command-prompt-utility"></a>Lektion 3: Verwenden des dta-Befehlszeilenprogramms
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Mit dem Befehlszeilenprogramm **dta** wird die Funktionalität des Datenbankoptimierungsratgebers erweitert.  
  
Sie können mit Ihren bevorzugten XML-Tools Eingabedateien für das Befehlszeilenprogramm erstellen und dabei das XML-Schema des Datenbankoptimierungsratgebers verwenden. Dieses Schema wird zusammen mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installiert. Es befindet sich unter: C:\Programme (x86)\Microsoft SQL Server\110\Tools\Binn\schemas\sqlserver\2004\07\dta\dtaschema.xsd.  
  
Das XML-Schema des Datenbankoptimierungsratgebers ist auch online auf [dieser Microsoft-Website](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409)verfügbar.  
  
Das XML-Schema des Datenbankoptimierungsratgebers ermöglicht mehr Flexibilität beim Festlegen von Optimierungsoptionen. So ermöglicht es z. B. die Durchführung einer "Was-wäre-wenn-Analyse". Für eine "Was-wäre-wenn-Analyse" wird für die Datenbank, die optimiert werden soll, eine Gruppe vorhandener sowie hypothetischer physischer Entwurfsstrukturen angegeben. Diese werden dann mit dem Datenbankoptimierungsratgeber analysiert, um herauszufinden, welche dieser hypothetischen Entwurfsstrukturen die Abfrageverarbeitung verbessert. Diese Art einer Analyse hat den Vorteil, dass die neue Konfiguration ausgewertet werden kann, ohne dass eine Implementierung erforderlich ist. Wenn die hypothetische physische Entwurfsstruktur nicht die gewünschten Leistungsverbesserungen erbringt, können Sie sie einfach ändern und erneut analysieren, bis die Konfiguration erreicht ist, mit der die gewünschten Ergebnisse erzielt werden.  
  
Außerdem können Sie mit dem XML-Schema des Datenbankoptimierungsratgebers und mit dem Befehlszeilenprogramm **dta** Funktionen des Datenbankoptimierungsratgebers in Skripts übernehmen und diese in anderen Datenbankentwicklungstools verwenden.  
  
Auf die Verwendung der XML-Eingabefunktionen des Datenbankoptimierungsratgebers wird in dieser Lektion nicht eingegangen.  
  
 In dieser Aufgabe erfahren Sie, wie Sie das Hilfsprogramm **dta** starten, die dazugehörige Hilfe anzeigen und es anschließend zur Optimierung einer Arbeitsauslastung über die Eingabeaufforderung verwenden. Dabei wird die Arbeitsauslastung MyScript.sql verwendet, die Sie in der Übung zur grafischen Benutzeroberfläche des Datenbankoptimierungsratgebers [Optimieren einer Arbeitsauslastung](lesson-2-using-database-engine-tuning-advisor.md#tuning-a-workload)angelegt haben.  
  
In diesem Tutorial wird die Beispieldatenbank „AdventureWorks2017“ verwendet. Aus Sicherheitsgründen werden die Beispieldatenbanken nicht standardmäßig installiert. Informationen zur Installation der Beispieldatenbanken finden Sie unter [Installieren der SQL Server-Beispiele und -Beispieldatenbanken](../../samples/adventureworks-install-configure.md).  
  
Im Folgenden werden folgende Schritte erläutert: Öffnen einer Eingabeaufforderung, Starten des Befehlszeilen-Hilfsprogramms **dta** , Anzeigen der Syntaxhilfe und Optimieren der einfachen Arbeitsauslastung MyScript.sql, die Sie in [Optimieren einer Arbeitsauslastung](./lesson-2-using-database-engine-tuning-advisor.md)angelegt haben.  

## <a name="prerequisites"></a>Voraussetzungen 

Zur Durchführung dieses Tutorials benötigen Sie SQL Server Management Studio, Zugriff auf einen Server, auf dem SQL-Server ausgeführt wird, und eine AdventureWorks-Datenbank.

- Installieren Sie die [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Laden Sie die [AdventureWorks 2017-Beispieldatenbank](../../samples/adventureworks-install-configure.md) herunter.


Anweisungen zum Wiederherstellen von Datenbanken in SSMS finden Sie hier: [Wiederherstellen einer Datenbank](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md).

  >[!NOTE]
  > Dieses Tutorial richtet sich an Benutzer, die mit der Verwendung von SQL Server Management Studio und mit grundlegenden Datenbankverwaltungsaufgaben vertraut sind. 

## <a name="access-dta-command-prompt-utility-help-menu"></a>Zugreifen auf das Hilfemenü des DTA-Befehlszeilenprogramms
  
  
1.  Zeigen Sie im **Startmenü** auf **Alle Programme**, zeigen Sie auf **Zubehör**, und klicken Sie anschließend auf **Eingabeaufforderung**.  
  
2.  Geben Sie an der Eingabeaufforderung die folgende ein, und drücken Sie die EINGABETASTE:  
  
    ```  
    dta -? | more  
    ```  
  
    Der folgende Teil des Befehls ist optional: `| more` . Sie können mit seiner Hilfe jedoch die Syntaxhilfe des Hilfsprogramms besser durchblättern. Drücken Sie die EINGABETASTE, um im Hilfetext jeweils eine weitere Zeile anzuzeigen, oder drücken Sie die LEERTASTE, um auf die nächste Seite zu wechseln.  

  ![Hilfe im DTA-Befehlszeilenprogramm verwenden](media/dta-tutorials/dta-cmd-help.png)

## <a name="tune-simple-workload-using-the-dta-command-prompt-utility"></a>Optimieren einfacher Arbeitsauslastungen mithilfe des DTA-Befehlszeilenprogramms  


  
1.  Navigieren Sie an der Eingabeaufforderung zu dem Verzeichnis, in dem Sie die Datei MyScript.sql gespeichert haben.  
  
2.  Geben Sie an der Eingabeaufforderung Folgendes ein. Drücken Sie danach die EINGABETASTE, um den Befehl auszuführen und die Optimierungssitzung zu starten (beachten Sie, dass das Hilfsprogramm beim Analysieren von Befehlen die Groß- und Kleinschreibung berücksichtigt):  
  
    ```  
    dta -S YourServerName\YourSQLServerInstanceName -E -D AdventureWorks2012 -if MyScript.sql -s MySession2 -of MySession2OutputScript.sql -ox MySession2Output.xml -fa IDX_IV -fp NONE -fk NONE  
    ```  
  
    Dabei gibt `-S` den Namen Ihres Servers und die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz an, in der die [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] -Datenbank installiert ist. Die Einstellung `-E` gibt an, dass Sie eine vertrauenswürdige Verbindungsart mit der Instanz verwenden möchten. Dies ist der geeignete Verbindungstyp, wenn Sie eine Verbindung mit einem Windows-Domänenkonto herstellen. Die Einstellung `-D` gibt die Datenbank an, die Sie optimieren möchten, `-if` gibt die Arbeitsauslastungsdatei an, `-s` gibt den Sitzungsnamen an, `-of` gibt die Datei an, in die das Tool das Skript mit den [!INCLUDE[tsql](../../includes/tsql-md.md)] -Empfehlungen schreiben soll, und `-ox` gibt die Datei an, in die das Tool die Empfehlungen im XML-Format schreiben soll. Die letzten drei Schalter legen folgende Optimierungsoptionen fest: `-fa IDX_IV` gibt an, dass der Datenbankoptimierungsratgeber nur das Hinzufügen von Indizes (gruppiert und nicht gruppiert) und von indizierten Sichten berücksichtigen soll; `-fp NONE` gibt an, dass bei der Analyse keine Partitionsstrategie berücksichtigt werden soll; und `-fk NONE` gibt an, dass in der Datenbank vorhandene physische Entwurfsstrukturen nicht beibehalten werden müssen, wenn der Datenbankoptimierungsratgeber seine Empfehlungen abgibt.  

  ![CMD mit DTA verwenden](media/dta-tutorials/dta-cmd.png)
  
3.  Wenn der Datenbankoptimierungsratgeber mit dem Optimieren der Arbeitsauslastung fertig ist, zeigt er eine Meldung an, die besagt, dass die Optimierungssitzung erfolgreich abgeschlossen wurde. Sie können die Optimierungsergebnisse anzeigen. Verwenden Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] zum Öffnen der Dateien MySession2OutputScript.sql und MySession2Output.xml. Alternativ dazu können Sie auch die Optimierungssitzung MySession2 in der grafischen Benutzeroberfläche des Datenbankoptimierungsratgebers öffnen und die Empfehlungen und Berichte so anzeigen, wie in den Abschnitten [Anzeigen von Empfehlungen für die Optimierung](./lesson-2-using-database-engine-tuning-advisor.md) und [Anzeigen von Optimierungsberichten](./lesson-2-using-database-engine-tuning-advisor.md)erläutert.  
  
 
## <a name="after-you-finish-this-tutorial"></a>Weiterführende Informationen nach Abschluss dieses Lernprogramms  
Wenn Sie die Lektionen in diesem Lernprogramm durchgearbeitet haben, finden Sie unter folgenden Themen weitere Informationen zum Datenbankoptimierungsratgeber:  
  
-   [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md) enthält Beschreibungen zum Ausführen von Tasks mit diesem Tool. 
-   [dta Utility](../../tools/dta/dta-utility.md) enthält Referenzmaterial zum Eingabeaufforderungs-Hilfsprogramm und der optionalen XML-Datei, mit der Sie die Ausführung des Hilfsprogramms steuern können.  
  
Mit dem folgenden Link können Sie zum Anfang des Tutorials zurückkehren: [Tutorial: Datenbankoptimierungsratgeber](../../tools/dta/tutorial-database-engine-tuning-advisor.md).  
  
## <a name="see-also"></a>Weitere Informationen  
[Lernprogramme zur Datenbank-Engine](../../relational-databases/database-engine-tutorials.md)  
