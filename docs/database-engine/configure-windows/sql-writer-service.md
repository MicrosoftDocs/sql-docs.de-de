---
title: SQL Writer-Dienst | Microsoft-Dokumentation
description: Informationen zum SQL Writer-Dienst Hier erfahren Sie, wie dieser das zusätzliche Sichern und Wiederherstellen in SQL Server über das VSS-Framework (Volume Shadow Copy Service, Volumeschattenkopie-Dienst) ermöglicht.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- VDI [SQL Server]
- restoring [SQL Server], SQL Writer Service
- backups [SQL Server], while SQL Server running
- Volume Shadow Copy Service
- volume backups while running [SQL Server]
- Virtual Backup Device Interface [SQL Server]
- SQL Writer Service
- services [SQL Server], SQL Writer
- MSDE Writer
- VSS
ms.assetid: 0f299867-f499-4c2a-ad6f-b2ef1869381d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 99f91a0a5f3f018b10a4ac0ff18c4ecf75b9c790
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100349216"
---
# <a name="sql-writer-service"></a>SQL Writer-Dienst
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Der SQL Writer-Dienst bietet zusätzliche Funktionalität zum Sichern und Wiederherstellen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] über dass VSS-Framework (Volume Shadow Copy Service, Volumeschattenkopie-Dienst).  
  
 Der SQL Writer Service wird automatisch installiert. Er muss ausgeführt werden, wenn die Anwendung des Volumenschattenkopie-Diensts (VSS, Volume Shadow Copy Service) eine Sicherung oder Wiederherstellung anfordert. Verwenden Sie das [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows Services-Applet, um den Dienst zu konfigurieren. Der SQL Writer-Dienst kann auf allen Betriebssystemen installiert werden.  
  
## <a name="purpose"></a>Zweck  
 Beim Ausführen errichtet [!INCLUDE[ssDE](../../includes/ssde-md.md)] eine Sperre und verfügt somit über den alleinigen Zugriff auf die Datendateien. Wenn der SQL Writer Service nicht ausgeführt wird, haben unter Windows ausgeführte Sicherungsprogramme keinen Zugriff auf die Datendateien, und die Sicherung muss mithilfe der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherung erfolgen.  
  
 Verwenden Sie den SQL Writer Service, um Windows-Sicherungsprogrammen das Kopieren von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datendateien zu ermöglichen, während [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird.  
  
## <a name="volume-shadow-copy-service"></a>Volumenschattenkopie-Dienst  
 Die in VSS bereitgestellte Gruppe von COM APIs implementiert ein Framework, das die Sicherung von Volumes ermöglicht, während die auf dem System ausgeführten Anwendungen weiter Daten auf die betreffenden Datenträger schreiben. Der VSS stellt eine konsistente Oberfläche bereit, mit der die Benutzeranwendungen, die Daten auf dem Datenträger aktualisieren (Verfasser), und die Benutzeranwendungen zum Sichern der Anwendungen (Anforderer) koordiniert werden können.  
  
 Der VSS kopiert und zeichnet dauerhafte Bilder zur Sicherung der laufenden Systeme, insbesondere der Server, auf, ohne dass dadurch Leistung und Stabilität der bereitgestellten Dienste übermäßig beeinträchtigt werden. Weitere Informationen zum VSS finden Sie in der Windows-Dokumentation.  

> [!NOTE]
> Wenn Sie VSS zum Sichern eines virtuellen Computers verwenden, der eine Basis-Verfügbarkeitsgruppe und zurzeit Datenbanken hostet, die sich in einem sekundären Zustand befinden, werden diese Datenbanken ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU2 und [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU9 *nicht* mit dem virtuellen Computer gesichert.  Der Grund hierfür ist, dass für Basis-Verfügbarkeitsgruppen das Sichern von Datenbanken auf dem sekundären Replikat nicht unterstützt wird.  Vor diesen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wurde durch die Sicherung ein Fehler verursacht.
  
## <a name="virtual-backup-device-interface-vdi"></a>VDI (Virtual Backup Device Interface)  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt ein API namens VDI (Virtual Backup Device Interface) bereit, mit dem unabhängige Softwarehersteller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in ihre Produkte integrieren können, um so Unterstützung für Sicherungs- und Wiederherstellungsvorgänge bereitzustellen. Diese APIs wurden mit dem Ziel der maximalen Zuverlässigkeit und Leistung entworfen und unterstützen das gesamte Spektrum der in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bereitgestellten Sicherungs- und Wiederherstellungsfunktionen, einschließlich aller Funktionen von Hotbackups und Momentaufnahmesicherungen. Fordert die Anwendung eines Drittanbieters eine VSS-Momentaufnahmesicherung an, ruft der SQL Writer-Dienst die API-Funktionen der VDI auf, um die eigentlichen Sicherungen durchzuführen. Beachten Sie, dass die API der VDI von VSS unabhängig ist und häufig in Softwarelösungen verwendet wird, die keine VSS-APIs verwenden.
  
## <a name="permissions"></a>Berechtigungen  
 Der SQL Writer-Dienst muss unter dem Konto **Lokales System** ausgeführt werden. Der SQL Writer-Dienst verwendet die Anmeldung für **NT-Dienst\SQLWriter** , um eine Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]herzustellen. Mithilfe der Anmeldung für **NT-Dienst\SQLWriter** kann der SQL Writer-Prozess mit einer niedrigeren Berechtigungsstufe unter einem Konto ausgeführt werden, das **nicht als Anmeldekonto** geführt wird. Auf diese Weise wird das Sicherheitsrisiko verringert. Wenn der SQL Writer-Dienst deaktiviert ist, werden Hilfsprogramme, die auf VSS-Momentaufnahmen basieren, z. B. System Center Data Protection Manager sowie einige andere Drittanbieterprodukte, funktionsunfähig. Im schlimmsten Fall besteht die Gefahr, dass Sicherungen von inkonsistenten Datenbanken erstellt werden. Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], das System, auf dem die Software ausgeführt wird, und das Hostsystem (im Falle eines virtuellen Computers) keine anderen Komponenten als die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Sicherung erfordern, kann der SQL Writer-Dienst gefahrlos deaktiviert und die Anmeldung entfernt werden.  Beachten Sie, dass der SQL Writer-Dienst durch eine Sicherung auf Volume- oder Systemebene initiiert werden kann, unabhängig davon, ob die Sicherung direkt auf einer Momentaufnahme basiert oder nicht. Einige Systemsicherungsprodukte verwenden VSS, um Blockierungen durch geöffnete oder gesperrte Dateien zu verhindern. Der SQL Writer-Dienst erfordert erhöhte Berechtigungen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , da im Verlauf der Dienstaktivitäten sämtliche E/A-Vorgänge für die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]kurzzeitig eingefroren werden.  
  
## <a name="features"></a>Features  
 SQL Writer unterstützt:  
  
-   Vollständige Sicherung und Wiederherstellung von Datenbanken, einschließlich der Volltextkataloge  
  
-   Differenzielle Sicherung und Wiederherstellung  
  
-   Wiederherstellung mit MOVE-Klausel  
  
-   Datenbankumbenennung  
  
-   Kopiesicherung  
  
-   Automatische Wiederherstellung einer Datenbankmomentaufnahme  
  
 SQL Writer unterstützt nicht:  
  
-   Protokollsicherungen  
  
-   Datei- und Dateigruppensicherung  
  
-   Seitenwiederherstellung  
  
## <a name="remarks"></a>Bemerkungen
Der SQL Writer-Dienst ist ein von der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Engine separater Dienst, der in den verschiedenen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Versionen und in verschiedenen Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf demselben Server gemeinsam genutzt wird.  Die SQL Writer-Dienstdatei ist im Lieferumfang des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Installationspakets enthalten und mit derselben Versionsnummer versehen wie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Engine, mit der sie geliefert wird.  Wenn eine neue Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem Server installiert oder eine vorhandene Instanz aktualisiert wird und die Versionsnummer der installierten oder aktualisierten Instanz höher als die Versionsnummer des auf dem Server befindlichen SQL Writer-Diensts ist, wird diese Datei durch die Datei aus dem Installationspaket ersetzt.  Wenn der SQL Writer-Dienst durch ein Service Pack oder ein kumulatives Update aktualisiert wurde und eine RTM-Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installiert wird, kann eine neuere Version des SQL Writer-Diensts durch eine ältere ersetzt werden. Die Voraussetzung hierfür ist, dass die Installation eine höhere Hauptversionsnummer aufweist.  Beispiel: Der SQL Writer-Dienst wurde in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU2 aktualisiert.  Wenn diese Instanz auf [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] RTM aktualisiert wird, wird der aktualisierte SQL Writer-Dienst durch eine ältere Version ersetzt.  In diesem Fall müssten Sie das neueste kumulative Update auf die neue Instanz anwenden, um die neuere Version des SQL Writer-Dienst zu erhalten.

