---
title: SQL Server-Sicherungsanwendungen – Volumeschattenkopie-Dienst und SQL Writer
description: Beschreibt die SQL Writer-Komponente und ihre Rolle bei der Erstellung einer VSS-Momentaufnahme und der Wiederherstellung von SQL Server-Datenbanken.
ms.custom: ''
ms.date: 07/24/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.openlocfilehash: 36fbf9ac4010797561e0852b96b6df6fd85b640d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347301"
---
# <a name="sql-server-back-up-applications---volume-shadow-copy-service-vss-and-sql-writer"></a>SQL Server-Sicherungsanwendungen – Volumeschattenkopie-Dienst und SQL Writer
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

SQL Server unterstützt den Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS), indem ein Writer (der SQL Writer) bereitgestellt wird, mit dem Drittanbieter-Sicherungsanwendungen über das VSS-Framework Datenbankdateien sichern können. In diesem Artikel werden die SQL Writer-Komponente und ihre Rolle bei der Erstellung einer VSS-Momentaufnahme und der Wiederherstellung von SQL Server-Datenbanken beschrieben. Außerdem erfahren Sie Näheres dazu, wie Sie den SQL Writer für die Arbeit mit Sicherungsanwendungen im VSS-Framework konfigurieren und anschließend verwenden.


## <a name="introduction"></a>Einführung

SQL Server bietet Unterstützung für das Erstellen von Momentaufnahmen aus SQL Server-Daten mithilfe des Volumeschattenkopie-Diensts. Dies wird durch die Bereitstellung eines VSS-konformen Writers (des SQL Writers) erzielt, mit dem Drittanbieter-Sicherungsanwendungen über das VSS-Framework Datenbankdateien sichern können. In diesem Artikel werden die SQL Writer-Komponente und ihre Rolle bei der Erstellung einer VSS-Momentaufnahme und der Wiederherstellung von SQL Server-Datenbanken beschrieben. Außerdem erfahren Sie Näheres dazu, wie Sie den SQL Writer für die Arbeit mit Sicherungsanwendungen im Kontext des VSS-Frameworks konfigurieren und anschließend verwenden.

## <a name="definition-of-terms"></a>Begriffsdefinition

- **Virtual Backup Device Interface:** SQL Server stellt eine API namens VDI (Virtual Backup Device Interface) bereit. Diese ermöglicht es unabhängigen Softwareherstellern, SQL Server in ihre Produkte zu integrieren und so Unterstützung für Sicherungs- und Wiederherstellungsvorgänge bereitzustellen. Diese APIs wurden mit dem Ziel der maximalen Zuverlässigkeit und Leistung entworfen und unterstützen das gesamte Spektrum der in SQL Server bereitgestellten Sicherungs- und Wiederherstellungsfunktionen, einschließlich aller Funktionen von Hotbackups und Momentaufnahmesicherungen. Weitere Informationen finden Sie unter [Spezifikation zur Schnittstelle für virtuelle Sicherungsmedien in SQL Server 2005](https://www.microsoft.com/download/details.aspx?id=17282). 

- **Anforderer:** Ein Vorgang (entweder automatisiert oder in der GUI), der die Erstellung von mindestens einem Momentaufnahmesatz von mindestens einem ursprünglichen Volume anfordert. In diesem Artikel bezieht sich der Begriff „Anforderer“ außerdem auf eine Sicherungsanwendung, die eine Momentaufnahme von SQL Server-Datenbanken erstellt.

## <a name="about-vss"></a>Informationen zu VSS

VSS stellt die Systeminfrastruktur bereit, über die sich VSS-Anwendungen auf Windows-Systemen ausführen lassen. Die Funktionsweise ist sowohl für Benutzer als auch für Entwickler größtenteils transparent. Dennoch sollen hier die wichtigsten Eigenschaften von VSS genannt werden:

- Koordiniert Aktivitäten von Anbietern, Writern und Anforderern bei der Erstellung und Verwendung von Schattenkopien
- Liefert den Standardsystemanbieter
- Implementiert die Low-Level-Treiberfunktionalität, die für alle Anbieter erforderlich ist

Der VSS-Dienst startet on demand. Dies bedeutet, dass dieser Dienst erst aktiviert werden muss, damit VSS-Vorgänge ausgeführt werden können.

## <a name="vss-components"></a>VSS-Komponenten

VSS koordiniert die Aktivitäten der folgenden kooperierenden Komponenten: 

- Anbieter sind Eigentümer der Daten auf den Schattenkopien und instanziieren die Schattenkopien.
- Writer sind Anwendungen, die Daten ändern und an der Synchronisierung von Schattenkopien beteiligt sind.
- Anforderer initiieren die Erstellung und Zerstörung von Schattenkopien. Der vorliegende Entwurf geht von einem Szenario aus, in dem der Anforderer eine Sicherungsanwendung ist.

VSS ermöglicht die Koordinierung der folgenden Komponenten: 

![Diagramm, das zeigt, wie der VSS diese verschiedenen Komponenten koordiniert](media/sql-server-vss-writer-backup-guide/coordinations.png)

In dieser Abbildung werden alle Komponenten gezeigt, die typischerweise an der Erstellung einer VSS-Momentaufnahme beteiligt sind. In einem solchen Szenario fungiert SQL Server (einschließlich des SQL Writers) als Writer in einem der Writer-Felder.  Ein anderer solcher Writer kann z. B. Exchange Server sein.

Im weiteren Verlauf dieses Artikels wird davon ausgegangen, dass der Leser mit den Begriffen des VSS-Frameworks vertraut ist.  Ausführliche Informationen dazu finden Sie in der Dokumentation unter [Übersicht zum Volumeschattenkopie-Dienst](/windows/win32/vss/volume-shadow-copy-service-overview).

## <a name="about-sql-writer"></a>Informationen zum SQL Writer

Der SQL Writer ist ein VSS Writer, der von SQL Server bereitgestellt wird. Er wickelt die Interaktion zwischen VSS und SQL Server ab. Der SQL Writer ist als eigenständiger Dienst im Lieferumfang von SQL Server enthalten und wird zusammen mit SQL Server installiert. Der SQL Writer ist standardmäßig deaktiviert. Er muss also muss explizit auf dem Servercomputer ausgeführt werden.

Folgende Abbildung zeigt, welche Rolle der SQL Writer bei der Sicherung einer VSS-Momentaufnahme innehat: 

![VSS-Momentaufnahme](media/sql-server-vss-writer-backup-guide/sql-vss-snapshot.png)



## <a name="configuring-the-sql-writer"></a>Konfigurieren des SQL Writers

Der SQL Writer-Dienst wird im Rahmen von SQL Server im System installiert und ist so konfiguriert, dass er automatisch zusammen mit Windows gestartet wird. 

### <a name="sql-writer-service-account"></a>SQL Writer-Dienstkonto

Während der Installation wird das SQL Writer-Konto so installiert, dass es das lokale Systemkonto verwendet. Da der SQL Writer mithilfe exklusiver VDI-APIs mit SQL Server kommunizieren muss, muss das SQL Writer-Konto über ausreichend hohe Zugriffsrechte für SQL Server und VSS verfügen.  Wenn Sie den Dienst als lokales Systemkonto konfigurieren, erhält er ausreichend hohe Rechte und kann ordnungsgemäß ausgeführt werden.

  > [!NOTE]
  > Damit der SQL Writer-Dienst korrekt funktioniert, muss sichergestellt werden, dass das lokale Systemkonto nicht aus der SA-Rolle der SQL Server-Instanz entfernt wird.

### <a name="enabling-and-starting-sql-writer"></a>Aktivieren und Starten des SQL Writers

Zum Starten und Verwenden des SQL Writers müssen Sie wie folgt vorgehen:

Der SQL Writer-Dienst lässt sich aktivieren, indem Sie diesen Dienst als „Automatisch“ markieren. Klicken Sie zum Öffnen der Dienste auf „Start“ und dann auf „Systemsteuerung“. Doppelklicken Sie dann auf „Verwaltung“ > „Dienste“. Doppelklicken Sie im Bereich „Dienste“ auf den SQL Writer-Dienst, und ändern Sie die Eigenschaft „Starttyp“ in „Automatisch“.

Der Dienst sollte auch gestartet werden, indem Sie unter der Eigenschaft „Dienststatus“ im oben erwähnten Bildschirm mit den Diensteigenschaften die Schaltfläche „Start“ auswählen.

Der Einfachheit halber führt der SQL Writer-Dienst diese Änderungen beim ersten Start automatisch aus.

  > [!NOTE]
  > Unter bestimmten Umständen, wenn eine SQL Server Express-Instanz installiert wird und eine Anwendung das Feature „Benutzerinstanzen“ verwendet, kann der SQL Writer automatisch von SQL Server gestartet werden. Dies soll das Aufzählen dieser Benutzerinstanzen während eines VSS-Sicherungsvorgangs vereinfachen. 

## <a name="backuprestore-operations-and-supported-versions"></a>Sicherungs-/Wiederherstellungsvorgänge und unterstützte Versionen

### <a name="version-support"></a>Versionsunterstützung

Der SQL Writer wird zusammen mit SQL Server ausgeliefert und unterstützt nur SQL Server-Instanzen. 

Der SQL Writer listet auch die SQL Server Express-Instanzen auf. Benutzerinstanzen, die von SQL Server Express gestartet werden, werden ebenfalls vom SQL Writer aufgezählt.


## <a name="supported-backuprestore-operations"></a>Unterstützte Sicherungs-/Wiederherstellungsvorgänge

SQL Server unterstützt (mithilfe des SQL Writers) die folgenden Modi für VSS-basierte Sicherungsvorgänge:

- Nicht komponentenbasiert
- Komponentenbasiert

### <a name="noncomponent-based-backup-operations"></a>Nicht komponentenbasierte Sicherungsvorgänge

Bei nicht komponentenbasierten Sicherungsvorgängen werden Datenbanken implizit mithilfe der Volumeliste im Momentaufnahmesatz ausgewählt. Der SQL Writer prüft auf zerrissene Datenbanken und gibt ggf. einen Fehler aus. Eine Datenbank gilt als zerrissen, wenn von der Volumeliste nur eine Teilmenge der Dateien ausgewählt wurde.

Im nicht komponentenbasierten Modell werden nur Datenbanken mit dem einfachen Wiederherstellungsmodell unterstützt. Das Ausführen eines Rollforward nach einer Wiederherstellung ist jedoch nicht möglich.

### <a name="component-based-backup-operations"></a>Komponentenbasierte Sicherungsvorgänge

Es wird empfohlen, komponentenbasierte Sicherungen mit dem SQL Writer auszuführen, da die Anwendung (VSS-Sicherungsanwendung) die Datenbanken explizit aus den Metadaten auswählt, die vom SQL Writer zurückgegeben werden. Der Momentaufnahmesatz sollte alle Volumes enthalten, die zum Sichern dieser Datenbanken erforderlich sind. Die VSS-Infrastruktur fügt die Volumes, die für die ausgewählten Datenbanken erforderlich sind, nicht automatisch hinzu. Alle Unterstützungsvolumes sollten im Volume-Momentaufnahmesatz enthalten sein. Die Sicherungsanwendung ist dafür zuständig, sicherzustellen, dass alle Unterstützungsvolumes dem Momentaufnahmesatz hinzugefügt wurden.  Der SQL Writer erkennt zerrissene Datenbanken (deren Unterstützungsvolumes nicht im Momentaufnahmesatz enthalten sind) und lässt die Sicherung fehlschlagen.

Im weiteren Verlauf dieses Artikels wird davon ausgegangen, dass komponentenbasierte Sicherungen im Rahmen der Erstellung von VSS-Momentaufnahmen für SQL Server eingesetzt werden.

## <a name="features-supported-by-sql-writer"></a>Vom SQL Writer unterstützte Features


- **Fulltext** (Volltext): Der SQL Writer vermerkt Volltextkatalog-Container mit rekursiven Dateispezifikationen in den Datenbankkomponenten im Metadatendokument des Writers.  Diese werden automatisch bei der Sicherung berücksichtigt, wenn die Datenbankkomponente ausgewählt ist.
- **Differential Backup/Restore** (Differenzielle Sicherung/Wiederherstellung): Der SQL Writer unterstützt die differenzielle Sicherung/Wiederherstellung über zwei differenzielle VSS-Mechanismen: „Partial File“ (Teildatei) und „Differenced File by Last Modify Time“ (Differenzielle Datei nach Zeitpunkt der letzten Änderung).
- **Partial File** (Teildatei):   Der SQL Writer verwendet den VSS-Mechanismus „Partial File“, um geänderte Bytebereiche innerhalb der eigenen Datenbankdateien zu melden.  
- **Differenced File by Last Modify Time** (Differenzielle Datei nach Zeitpunkt der letzten Änderung): Der SQL Writer verwendet den VSS-Mechanismus „Differenced File by Last Modify Time“, um geänderte Dateien in Volltextkatalogen zu melden.
- **Restore with Move** (Mit Verschieben wiederherstellen): Der SQL Writer unterstützt die VSS-Spezifikation „New Target“ (Neues Ziel) während der Wiederherstellung.  Diese Spezifikation ermöglicht es, dass eine Datenbank-/Protokolldatei oder ein Volltextkatalog-Container im Rahmen eines Wiederherstellungsvorgangs verschoben werden kann.
- **Database Rename** (Datenbankumbenennung): Unter Umständen muss ein Anforderer eine SQL-Datenbank unter einem neuen Namen wiederherstellen. Dies ist vor allem dann erforderlich, wenn die Datenbank parallel zur ursprünglichen Datenbank wiederhergestellt werden soll. Der SQL Writer unterstützt das Umbenennen einer Datenbank während des Wiederherstellungsvorgangs, solange die Datenbank innerhalb der ursprünglichen SQL-Instanz verbleibt.
- **Kopiesicherung:** Manchmal ist es notwendig, aus einem besonderen Grund eine Sicherung auszuführen, zum Beispiel, wenn Sie zu Testzwecken eine Datenbankkopie erstellen.  Diese Sicherung darf sich nicht auf die üblichen Sicherungs- und Wiederherstellungsvorgänge der Datenbank auswirken. Mit der Option COPY_ONLY geben Sie an, dass es sich bei der Sicherung um eine Ausnahme handelt, die keinen Einfluss auf die normale Sicherungssequenz haben soll. Der SQL Writer unterstützt den Sicherungstyp „Kopiesicherung“ in Kombination mit SQL Server-Instanzen.
- **Autorecovery of Database Snapshot** (Automatische Wiederherstellung einer Datenbankmomentaufnahme):   Üblicherweise befindet sich eine Momentaufnahme einer SQL Server-Datenbank, die über das VSS-Framework erstellt wurde, in einem nicht wiederhergestellten Zustand. Auf die Daten in der Momentaufnahme kann nicht sicher zugegriffen werden, bevor sie nicht die Wiederherstellungsphase durchlaufen hat, in der ein Rollback für derzeit ausgeführte Transaktionen ausgeführt und die Datenbank in einen konsistenten Zustand versetzt wird. Eine VSS-Sicherungsanwendung kann im Rahmen der Erstellung einer Momentaufnahmen die automatische Wiederherstellung von Momentaufnahmen anfordern.

Diese neuen Features und ihre Verwendung werden im Abschnitt „Informationen zu Sicherungs- und Wiederherstellungsoptionen“ dieses Artikels eingehender beschrieben.

### <a name="what-is-not-supported"></a>Nicht unterstützte Features

- Protokollsicherungen werden vom SQL Writer nicht unterstützt.
- Das Sichern von Dateien/Dateigruppen wird nicht unterstützt.
- Die Seitenwiederherstellung wird nicht unterstützt.
- Datenbankmomentaufnahmen werden nicht unterstützt und werden während der Erstellung von komponenten- und nicht komponentbasierten VSS-Momentaufnahmen ignoriert. 
- „Autoclose databases“ (Datenbanken automatisch schließen) oder „Databases with shutdown“ (Datenbanken mit Herunterfahren)
- Linux stellt kein VSS-Framework bereit. Daher ist der SQL Writer unter Linux nicht verfügbar.

In der folgenden Tabelle sind die Typen von Momentaufnahmesicherungen aufgeführt, die vom SQL Writer/von SQL Server unterstützt werden und mit dem VSS-Framework in allen SQL Server-Versionen funktionieren.

| **Sicherungs-/Wiederherstellungsvorgang**                 | **Komponentenbasiert**           | **Nicht komponentenbasiert** |
|:-------------------------------------------- | :---------------------------- | :--------------------- |
|Full Data Backup (Vollständige Datensicherung) </br> (einschließlich des Volltextkatalogs)| Ja                     | Ja                    |
|Full Restore (Vollständige Wiederherstellung)                                  | Ja                           | Ja                    |
|Full Restore (NORECOVERY) (Vollständige Wiederherstellung [NORECOVERY])                    | Ja                           | Nein                     |
|Differenzielle Sicherung                           | Ja                           | Nein                     |
|Differential Restore (Differenzielle Wiederherstellung)                          | Ja                           | Nein                     |
|Restore with Move (Mit Verschieben wiederherstellen)                             | Ja                           | Nein                     |
|Database Rename (Datenbankumbenennung)                               | Ja                           | Nein                     |
|Kopiesicherung                              | Ja                           | Nein                     |
|Auto-Recovered Snapshots (Automatisch wiederhergestellte Momentaufnahmen)                       | Ja                           | Nein                     |
|Protokollsicherung                                    | Nein                            | Nein                     |
|Datenbank-Momentaufnahmen                            | Nein                            | Nein                     |
|Autoclose databases (Datenbanken automatisch schließen)</br> Databases with shutdown (Datenbanken mit Herunterfahren) | Ja                        | Nein                     |
|Availability group databases (Verfügbarkeitsgruppen-Datenbanken)                  | Ja                           | Nicht auf „secondary“        | 




## <a name="snapshot-creation-process"></a>Erstellen von Momentaufnahmen

Das VSS-Framework koordiniert die Aktivitäten eines Anforderers (einer Sicherungsanwendung) und des SQL Writers während der Erstellung von SQL Server-Momentaufnahmen. Damit diese Koordination funktioniert, definiert das VSS-Framework Schnittstellen für Anforderer und Writer.  Diese Schnittstellen sollten von beteiligten Anfordereranwendungen und Writern implementiert werden. Der SQL Writer implementiert die erforderlichen Writer-Schnittstellen. Im Verlauf der Erstellung von Momentaufnahmen werden die Schnittstellen des SQL Writers vom VSS-Framework aufgerufen. Der SQL Writer interagiert mit SQL Server-Instanzen im System, um die Erstellung von Momentaufnahmen zu ermöglichen.

Das VSS-Framework definiert verschiedene APIs, die von einer Anforderer/einer Sicherungsanwendung verwendet werden sollen. Entwickler von Sicherungsanwendungen müssen diesen API-Aufrufmustern folgen, um mit der Erstellung von Momentaufnahmen im VSS-Framework zu arbeiten. In den kommenden Abschnitten wird die Erstellung von Momentaufnahmen aus der Sicht des SQL Writers beschrieben. Außerdem werden die internen Interaktionen zwischen dem Anforderer, dem VSS-Framework, dem SQL Writer und den SQL Server-Instanzen näher erläutert. Weitere Informationen zu diesen Schritten und den Schnittstellen des VSS-Frameworks finden Sie in der Dokumentation zum Volumeschattenkopie-Dienst.    

  > [!NOTE]
  > Es wird davon ausgegangen, dass der Leser bereits mit dem VSS-Framework und der allgemeinen Erstellung von Sicherungen vertraut ist. Die folgenden Abschnitte enthalten Zusatzinformationen dazu, wie der SQL Writer an der Erstellung von VSS-Sicherungen beteiligt ist.

## <a name="snapshot-creation-workflow"></a>Workflow für die Erstellung von Momentaufnahmen

![Datenfluss](media/sql-server-vss-writer-backup-guide/dataflow-chart.png)

In der obigen Abbildung wird dargestellt, wie die Daten bei einer komponentenbasierten Erstellung/Wiederherstellung einer Momentaufnahme fließen. Zur Vereinfachung der grundlegenden Tasks, die bei einer Sicherung ablaufen, wurde dieser Überblick in die folgenden Phasen unterteilt:

- Initialisieren der Sicherung
- Ermitteln der Sicherung
- Vor der Sicherung anfallende Tasks
- Tatsächliches Sichern der Dateien
- Beenden der Sicherung



### <a name="backup-initialization"></a>Initialisieren der Sicherung

Während dieser Sicherungsphase bindet der Anforderer (Sicherungsanwendung) die Momentaufnahme-Schnittstelle **IvssBackupComponents** und initialisiert sie als Vorbereitung für die Sicherung. Des Weiteren ruft er die VSS-API **IVssGatherWriterMetadata** auf, um das VSS-Framework anzuweisen, Metadaten von allen Writern zu sammeln.

Das VSS-Framework ruft daraufhin mithilfe des Ereignisses **OnIdentify** die Writer-Metadaten von jedem der registrierten Writer ab, einschließlich des SQL Writers. Der SQL Writer fragt die SQL Server-Instanzen ab, um die Sicherungsmetadateninformationen für jede Datenbank abzurufen, und erstellt das Writer-Metadatendokument. Diese Phase wird auch als „Auflisten der Metadaten“ bezeichnet.

Das Writer-Metadatendokument enthält Informationen, die vom Writer an den Anforderer (Sicherungsanwendung) übergeben werden. Das Writer-Metadatendokument enthält die folgenden Informationen:

- Die Anwendungs-ID und den Anzeigename
- Den Speicherort der Dateien und Komponenten
- Welche Dateien bei der Sicherung berücksichtigt und welche ausgeschlossen werden sollen
- Welche Optionen bei der Wiederherstellung verwendet werden sollen
- Dies wird über das VSS-Framework zurück an den Anforderer übergeben.

### <a name="sql-writer-metadata-document"></a>SQL Writer-Metadatendokument

Dabei handelt es sich um ein XML-Dokument, das von einem Writer (in diesem Fall dem SQL Writer) mithilfe der Schnittstelle **IVssCreateWriterMetadata** erstellt wurde und Informationen zum Zustand und den Komponenten des Writers enthält. Die strukturellen Details eines Writer-Metadatendokuments werden in der Dokumentation zur VSS-API beschrieben. Nachfolgend werden einige der im SQL Writer-Metadatendokument aufgeführten Informationen genannt.

- Writer Identification Information (Informationen, die den Writer identifizieren)
    - **Writer name** (Name des Writers): L"SqlServerWriter"
    - **Writer class ID** (Writer-Klassen-ID): 0xa65faa63, 0x5ea8, 0x4ebc, 0x9d, 0xbd, 0xa0, 0xc4, 0xdb, 0x26, 0x91, 0x2a 
    - **Writer instance ID** (Writer-Instanz-ID): L"SQL Server:SQLWriter" 
    - **VSSUsageType** (VSS-Nutzungstyp): VSS_UT_USERDATA 
    - **VSSSourceType** (VSS-Quelltyp): VSS_ST_TRANSACTEDDB 
- Writer Level Information (Informationen über die Zugriffsebene des Writers): VSS_APP_BACK_END 
- Restore Method Specification (Spezifikation der Wiederherstellungsmethode): VSS_RME_RESTORE_IF_CAN_REPLACE
- Supported Backup schema (Unterstütztes Sicherungsschema): IVssCreateWriterMetadata::SetBackupSchema API
    - VSS_BS_DIFFERENTIAL: differenzielle Sicherung
    - VSS_BS_TIMESTAMPED: zeitstempelbasiert, für Volltextkatalog-Dateien
    - VSS_BS_LAST_MODIFY: differenzielle Sicherung, basiert auf dem Zeitpunkt der letzten Änderung
    - VSS_BS_WRITER_SUPPORTS_NEW_TARGET: unterstützt die Speicheroption „New Target“ (Neues Ziel)
    - VSS_BS_WRITER_SUPPORTS_RESTORE_WITH_MOVE: unterstützt „Restore with Move“ (Mit Verschieben wiederherstellen)
    - VSS_BS_COPY: unterstützt die Sicherungsoption „Kopiesicherung“
- Component Level Information (Komponentenbezogene Informationen): Dieser Abschnitt enthält komponentenspezifische Informationen, die vom SQL Writer bereitgestellt werden.
   - **Type** (Typ): VSS_CT_FILEGROUP
   - **Name**: Name der Komponente (Datenbankname)
   - **Logical path** (Logischer Pfad): logischer Pfad der Serverinstanz; weist bei benannten Instanzen das Schema „Server\Instanzname“ und bei Standardinstanzen das Schema „Server“ auf
   - **Component Flags** (Komponentenflags)
   - **VSS_CF_APP_ROLLBACK_RECOVERY**: gibt an, dass SQL Server-Momentaufnahmen immer eine Wiederherstellungsphase erfordern, damit die Dateien konsistent und für Nicht-Sicherungsszenarios (d. h. ein App-Rollback) wiederverwendbar gemacht werden können
   - Selectable (Auswählbar): TRUE
   - Selectable for Restore (Für die Wiederherstellung auswählbar): TRUE 
   - Restore methods supported (Unterstützte Wiederherstellungsmethoden): VSS_RME_RESTORE_IF_CAN_REPLACE

Die einzige Erweiterung der Komponentenstruktur in SQL Server ist die Einführung von Volltextkatalogen.  Volltextkataloge sind Containerverzeichnisse, die nicht als VSS-Datenbank- oder -Protokolldateien ausgedrückt werden können, weil die VSS-Datenbank- und -Protokolldateien über keine rekursive Spezifikation verfügen.  Aus diesem Grund verwendet der SQL Writer eine VSS-Dateigruppenkomponente (VSS_CT_FILEGROUP), um die Komponente auf Datenbankebene und die Dateigruppendateien in den Datenbank-, Protokoll- und Volltextkatalog-Dateien darzustellen.

Am Ende dieses Artikels ist ein Beispiel für ein Writer-Metadatendokument aufgeführt.

### <a name="backup-discovery"></a>Ermitteln der Sicherung
In dieser Phase untersucht ein Anforderer das Writer-Metadatendokument, erstellt ein **Sicherungskomponentendokument** und befüllt es mit jeder Komponente, die gesichert werden muss. In dem Dokument werden ebenfalls die erforderlichen Sicherungsoptionen und -parameter angegeben. Für den SQL Writer ist jede Datenbankinstanz, die gesichert werden muss, eine einzelne Komponente.

#### <a name="backup-components-document"></a>Sicherungskomponentendokument
Dabei handelt es sich um ein XML-Dokument, das von einem Anforderer (mithilfe der Schnittstelle **IVssBackupComponents**) während eines Wiederherstellungs- oder Sicherungsvorgangs erstellt wird. Das Sicherungskomponentendokument enthält eine Liste der Komponenten, die von mindestens einem Writer, der an einem Sicherungs- oder Wiederherstellungsvorgang beteiligt ist, explizit hinzugefügt wurden. Es enthält keine implizit enthaltenen Komponenteninformationen. Ein Writer-Metadatendokument enthält nur Writer-Komponenten, die an einer Sicherung beteiligt sein können. Die strukturellen Details eines Sicherungskomponentendokuments werden in der Dokumentation zur VSS-API beschrieben.

#### <a name="prebackup-tasks"></a>Vor der Sicherung anfallende Tasks
Unter VSS vor der Sicherung anfallende Tasks sind darauf ausgerichtet, eine Schattenkopie der Volumes zu erstellen, die die zu sichernden Daten enthalten. Die Sicherungsanwendung speichert Daten aus der Schattenkopie, nicht aus dem zugrunde liegenden Volume.

Anforderer warten typischerweise während der Sicherungsvorbereitung und der Erstellung der Schattenkopie auf die Writer. Wenn der SQL Writer am Sicherungsvorgang beteiligt ist, muss er seine Dateien und sich selbst so konfigurieren, dass er bereit für die Sicherung und die Schattenkopie ist.

#### <a name="prepare-for-backup"></a>Vorbereiten der Sicherung
Der Anforderer muss den Typ des Sicherungsvorgangs festlegen, der ausgeführt werden muss (**IVssBackupComponents::SetBackupState**), und anschließend die Writer über VSS darüber benachrichtigen, dass sie sich mithilfe von **IVssBackupComponents::PrepareForBackup** auf die Sicherung vorbereiten sollen.

Der SQL Writer erhält Zugriff auf das Sicherungskomponentendokument, in dem angegeben ist, welche Datenbanken gesichert werden müssen. Alle Unterstützungsvolumes sollten im Volume-Momentaufnahmesatz enthalten sein. Der SQL Writer erkennt zerrissene Datenbanken (deren Unterstützungsvolumes nicht im Momentaufnahmesatz enthalten sind) und lässt die Sicherung während des Ereignisses „PostSnapshot“ fehlschlagen.

**Initiieren der Momentaufnahme:** Der Anforderer initiiert die Erstellung der Momentaufnahme, indem er die VSS-Frameworkschnittstelle „DoSnapshotSet“ aufruft.

**Erstellen der Momentaufnahme:** In dieser Phase laufen verschiedene Interaktionen zwischen dem VSS-Framework und dem SQL Writer ab.

1. _Vorbereiten der Momentaufnahme:_ Der SQL Writer ruft SQL Server auf, um die Erstellung der Momentaufnahme vorzubereiten.
1. _Einfrieren:_ Der SQL Writer ruft SQL Server auf, damit die gesamten E/A-Vorgänge für jede der Datenbanken in der Momentaufnahme gesichert werden. Sobald das Einfrierereignis an das VSS-Framework zurückgegeben wird, erstellt VSS die Momentaufnahme.
1. _Reaktiveren:_ Bei diesem Ereignis ruft der SQL Writer die SQL Server-Instanzen auf, um die normalen E/A-Vorgänge zu reaktivieren oder sie fortzusetzen.


Die Erstellung einer Momentaufnahme ist schnell (weniger als 60 Sekunden), damit das Blockieren aller Schreibvorgänge auf die Datenbank nicht verhindert werden muss.

**Nach der Momentaufnahme:** Falls die automatische Wiederherstellung für die Momentaufnahme erforderlich ist, übernimmt der SQL Writer die automatische Wiederherstellung für jede Datenbank, die in der Momentaufnahme enthalten ist. Eine ausführliche Erläuterung finden Sie im Abschnitt „Automatisch wiederhergestellte Momentaufnahmen“.

#### <a name="actual-backup-of-files"></a>Tatsächliches Sichern der Dateien

In dieser Phase kann der Anforderer die Daten, falls erforderlich, auf ein Sicherungsmedium verschieben. In dieser Phase finden Interaktionen zwischen dem Anforderer und dem VSS-Framework statt. Der SQL Writer ist nicht involviert.

1. Get writer status (Writer-Status abrufen): Gibt den Status des Writers zurück. Der Anforderer muss möglicherweise Fehler beheben.
1. Do back up (Sichern):

Der Anforderer kann die Daten, falls erforderlich, in dieser Phase auf ein Sicherungsmedium verschieben.

#### <a name="backup-complete"></a>Backup complete (Sicherung abgeschlossen)

Dieses Ereignis zeigt an, dass die Sicherung erfolgreich abgeschlossen wurde.

Zu diesem Zeitpunkt kann der SQL Writer die Sicherung als differenzielle Basis committen, wenn es sich bei der aktuellen Sicherung um eine vollständige Sicherung der Datenbank handelt (und nicht nur um eine Kopiesicherung).


  > [!NOTE]
  > Der Anforderer sollte dieses Ereignis („Backup Complete“) explizit senden, um zuzulassen, dass der SQL Writer differenzielle Basissicherungen committet. Wenn das Ereignis nicht empfangen wird, gilt die erstellte Sicherung nicht als differenzielle Basis.

#### <a name="save-writer-metadata"></a>Speichern von Writer-Metadaten

Der Anforderer sollte das Sicherungskomponentendokument und die Sicherungsmetadaten jeder Komponente zusammen mit den Daten aus der Momentaufnahme speichern. Diese Writer-Metadaten werden vom SQL Writer/von SQL Server für Wiederherstellungsvorgänge benötigt.

#### <a name="backup-termination"></a>Beenden der Sicherung

Der Anforderer beendet die Schattenkopie, indem er die Schnittstelle **IVssBackupComponents** freigibt oder **IVssBackupComponents::DeleteSnapshots** aufruft.

## <a name="restore-process"></a>Wiederherstellungsvorgang

In diesem Abschnitt werden der Workflow für den Wiederherstellungsvorgang und die verschiedenen Schritte beschrieben, aus denen er besteht.

### <a name="restore-operation-workflow"></a>Workflow für den Wiederherstellungsvorgang

Das in der folgenden Abbildung dargestellte Datenflussdiagramm zeigt den Ablauf eines VSS-Wiederherstellungsvorgangs. Zur Vereinfachung der grundlegenden Tasks, die bei einer Wiederherstellung ablaufen, wurde dieser Überblick in die folgenden Themen unterteilt:

- Initialisieren der Wiederherstellung
- Vorbereiten der Wiederherstellung
- Tatsächliche Wiederherstellung von Dateien
- Cleanup und Beenden der Wiederherstellung

![Flow eines Wiederherstellungsvorgangs](media/sql-server-vss-writer-backup-guide/restore-process-flow.png)

In allen komponentenbasierten VSS-Wiederherstellungsszenarios wird die Datenbankwiederherstellung vom SQL Writer in zwei unterschiedlichen Phasen verarbeitet.

- **PreRestore** (vor der Wiederherstellung):  Der SQL Writer kümmert sich um die Validierung, das Schließen von Dateihandles usw.
- **PostRestore** (nach der Wiederherstellung):  Der SQL Writer fügt die Datenbank an und führt bei Bedarf eine Wiederherstellung nach Systemabsturz durch.

Zwischen diesen beiden Phasen ist die Sicherungsanwendung für das Verschieben der relevanten Daten ohne Auswirkungen auf SQL verantwortlich.

### <a name="restore-initialization"></a>Initialisieren der Wiederherstellung

Während der Initialisierungsphase einer Wiederherstellung, benötigt der Anforderer Zugriff auf die gespeicherten Sicherungskomponentendokumente.

Das Sicherungskomponentendokument, das während des Sicherungsvorgangs generiert wird, wird als Teil der Sicherungsdaten gespeichert. Die Sicherungsanwendung muss diese Daten zurück an das VSS-Framework übergeben. Der SQL Writer erhält zu Beginn des Wiederherstellungsvorgangs Zugriff auf diese Daten.

#### <a name="prepare-for-restore"></a>Vorbereiten der Wiederherstellung

Beim Vorbereiten einer Wiederherstellung ermittelt der Anforderer anhand des gespeicherten Sicherungskomponentendokuments, welche Komponenten auf welche Weise wiederhergestellt werden sollen.  Der Anforderer wählt die Komponenten aus, die wiederhergestellt werden sollen, und legt bei Bedarf geeignete Wiederherstellungsoptionen fest.

Wenn eine Sicherungsanwendung zusätzlich zum aktuellen Wiederherstellungsvorgang differenzielle Sicherungen oder Protokollsicherungen anwenden möchte (d. h., „Mit NORECOVERY wiederherstellen“ ist erforderlich), sollte die folgende Option für jede wiederherzustellende Datenbank als Teil der Komponentenerstellung festgelegt werden.

```
IVssBackupComponents::SetAdditionalRestores(true)
```

Nachdem alle erforderlichen Details im Sicherungskomponentendokument festgelegt wurden, sendet der Anforderer einen Aufruf an **IVssBackupComponents::PreRestore**, um ein PreRestore-Ereignis über VSS zu generieren, das von den Writern verarbeitet wird.

Der SQL Writer untersucht das angegebene Sicherungskomponentendokument, um die entsprechenden Datenbanken zu ermitteln, wobei alle zusätzlichen Dateien gelöscht werden, die seit dem Sicherungszeitpunkt erstellt wurden. Außerdem überprüft der Writer den Speicherplatz und schließt alle geöffneten Datenbank-Dateihandles, sodass der Anforderer die benötigten Daten während der Wiederherstellungsphase kopieren kann. Aufgrund dieser Phase können alle frühen Fehlerzustände erkannt werden, bevor der Anforderer das tatsächliche Kopieren der Dateien ausführt. SQL Server versetzt die Datenbank außerdem in den Wiederherstellungszustand.  Ab diesem Zeitpunkt kann die Datenbank erst nach einer erfolgreichen Wiederherstellung gestartet werden.

#### <a name="restore-files"></a>Wiederherstellen von Dateien

Dabei handelt es sich um eine rein anfordererspezifische Aktion. Der Anforderer (die Sicherungsanwendung) ist dafür zuständig, die erforderlichen Datenbankdateien (oder die relevanten Datenbereiche für differenzielle Wiederherstellungen) an die richtigen Speicherorte zu kopieren. Der SQL Writer ist an diesem Vorgang nicht beteiligt.

#### <a name="cleanup-and-termination"></a>Cleanup und Beenden

Nachdem alle Daten an der richtigen Stelle wiederhergestellt wurden, ruft ein Anforderer den SQL Writer auf und benachrichtigt ihn, dass der Wiederherstellungsvorgang abgeschlossen wurde (**IvssBackupComponents::PostRestore**). So erfährt der SQL Writer, dass die PostRestore-Aktionen gestartet werden können.  Der SQL Writer führt zu diesem Zeitpunkt die Rollforwardphase der Wiederherstellung nach Systemabsturz aus. Wenn die Wiederherstellung nicht angefordert wird (d. h. SetAdditionalRestores(true) wird nicht vom Anforderer angegeben), wird die Rollbackphase des Wiederherstellungsschritts ebenfalls während dieser Phase durchgeführt.

## <a name="backup-and-restore-option-details"></a>Informationen zu Sicherungs- und Wiederherstellungsoptionen

In diesem Abschnitt werden alle vom SQL Writer unterstützten Sicherungs- und Wiederherstellungsoptionen ausführlich beschrieben.

### <a name="requestor-creates-a-volume-shadow-copy"></a>Anforderer erstellt eine Volumeschattenkopie

Der SQL Writer kann an der Erstellung der Volumeschattenkopie (außerhalb des Sicherungs-/Wiederherstellungskontexts) beteiligt sein, da das bzw. die Unterstützungsvolume(s) der Datenbankdateien dem Volume-Momentaufnahmesatz hinzugefügt wurden.  In diesem Fall ist der SQL Writer nur an der Koordination der Metadatenauflistung, des Einfrierens, des Reaktivierens sowie der PrepareForSnapshot- und PostSnapshot-Ereignisse beteiligt (weitere Informationen finden Sie im Datenflussdiagramm).

### <a name="full-backuprestore"></a>Vollständige Sicherung/Wiederherstellung

Der SQL Writer unterstützt vollständige Sicherungs-/Wiederherstellungsvorgänge, sowohl im nicht komponentenbasierten als auch im komponentenbasierten Modus.

### <a name="noncomponent-based-backup-and-restore"></a>Nicht komponentenbasierte Sicherung und Wiederherstellung

Bei einer nicht komponentenbasierten Sicherung/Wiederherstellung, gibt der Anforderer einen Volume- oder Ordnerbaum an, der gesichert/wiederhergestellt werden soll. Alle Daten im angegebenen Volume/Ordner werden gesichert/wiederhergestellt.

#### <a name="backup"></a>Backup

Bei nicht komponentenbasierten Sicherungsvorgängen wählt der SQL Writer Datenbanken implizit mithilfe der Volumeliste im Momentaufnahmesatz aus.  Der Writer prüft auf zerrissene Datenbanken und gibt ggf. einen Fehler aus. Eine Datenbank gilt als zerrissen, wenn von der Volumeliste nur eine Teilmenge der Dateien ausgewählt wurde.  Ein Rollforward (differenzielle Wiederherstellungen oder Protokollwiederherstellungen) nach einer Wiederherstellung werden vom SQL Writer nicht unterstützt.

#### <a name="restore"></a>Restore

Der Anforderer stellt eine oder mehrere Datenbanken wieder her, die im nicht komponentenbasierten Modus gesichert wurden.  Auf solche Wiederherstellungen darf keine Rollforward-Wiederherstellung folgen, also z. B. keine Protokollwiederherstellung oder keine differenzielle Wiederherstellung.

Bei Wiederherstellungen im nicht komponentenbasierten Modus muss die SQL Server-Instanz offline sein, oder die Zieldatenbanken müssen entfernt/getrennt werden, damit sichergestellt ist, dass die Dateien offline sind.  Die Dateien werden direkt kopiert, und die Datenbank(en) wird bzw. werden angefügt. All dies geschieht ohne den SQL Writer.

### <a name="component-based-backup-and-restore"></a>Komponentenbasierte Sicherungen und Wiederherstellungen

Bei einer komponentenbasierten Sicherung wählt der Anforderer explizit die Datenbankkomponenten (aus den Metadaten, die der SQL Writer an den Client zurückgibt) aus, die gesichert/wiederhergestellt werden sollen.

#### <a name="backup"></a>Backup

Bei einer komponentenbasierten Sicherung sollten alle Unterstützungsvolumes für ausgewählte Datenbanken im Volume-Momentaufnahmesatz enthalten sein.  Andernfalls erkennt der SQL Writer zerrissene Datenbanken (deren Unterstützungsvolumes nicht im Momentaufnahmesatz enthalten sind) und lässt die Sicherung fehlschlagen.  Bei einer vollständigen Sicherung werden die Datenbankdaten und alle Protokolldateien gesichert, die notwendig sind, damit die Datenbank zur Wiederherstellungszeit einen konsistenten Transaktionszustand einnehmen kann.

#### <a name="full-restore-without-rollforward"></a>Vollständige Wiederherstellung ohne Rollforward

Eine vollständige Wiederherstellung der Datensicherung wird manchmal ohne zusätzliche Rollforwardvorgänge durchgeführt. Dies kann daran liegen, dass keine Metadaten für den Rollforward vorliegen oder dass ein Rollforward unter gewissen Umständen schlicht nicht erforderlich ist. In diesem Abschnitt werden beide Szenarios kurz behandelt.  

#### <a name="no-metadatano-rollforward"></a>Keine Metadaten/kein Rollforward

Wenn beim Sicherungsvorgang keine Writer-Metadaten (Metadaten aus komponentenbasierten Sicherungen) gespeichert wurden, muss während der Wiederherstellung die SQL Server-Instanz offline sein, oder die Zieldatenbanken müssen entfernt/getrennt werden, damit sichergestellt ist, dass die Dateien offline sind.  Die Dateien werden direkt kopiert, und anschließend werden die Datenbanken angefügt. All dies geschieht ohne den SQL Writer.

#### <a name="metadata-exist-but-no-additional-rollforward-is-needed"></a>Metadata liegen vor, ein zusätzlicher Rollforward ist jedoch nicht erforderlich

Der Anforderer stellt die Datenbank(en) wieder her, die im komponentenbasierten Modis gesichert wurden. Es wird jedoch kein Rollforward angefordert. In diesem Fall führt SQL Server im Rahmen der Wiederherstellung eine Wiederherstellung nach Systemabsturz für die Datenbank durch.

**Vollständige Wiederherstellung mit zusätzlichen Rollforwardvorgängen:** Der Anforderer kann eine Wiederherstellung beauftragen, indem er die Option „SetAdditionalRestores(true)“ angibt.  Diese Option gibt an, dass der Anforderer weitere Rollforward-Wiederherstellungen beauftragen wird (z. B. Protokollwiederherstellungen, differenzielle Wiederherstellungen etc.). Dies weist SQL Server an, den Wiederherstellungsschritt am Ende des Wiederherstellungsvorgangs nicht auszuführen.

Dies ist nur möglich, wenn die Writer-Metadaten während der Sicherung gespeichert wurden und dem SQL Writer zur Wiederherstellungszeit zur Verfügung stehen. Der SQL Server-Dienst muss ausgeführt werden, bevor der Anforderer VSS anweist, die Wiederherstellungsaktivität auszuführen.

Der SQL Writer erwartet die folgende Sequenz:

1. Vorbereiten der Wiederherstellung der einzelnen Datenbanken: Bei dieser Aktivität werden alle Dateihandles geschlossen, damit die Datenbankdateien von der Anfordereranwendung kopiert/eingebunden werden können.
1. Die Dateien werden von der Anfordereranwendung kopiert/eingebunden.
1. Die Wiederherstellung wird beendet (mit NORECOVERY).  Die Datenbanken werden wieder online geschaltet, erhalten jedoch den Status „Wird wiederhergestellt“.

Mithilfe von konventionellen SQL-Sicherungen (differenziell oder für Protokolle) kann anschließend über die VDI/T-SQL oder durch Anwendung der differenziellen Wiederherstellung mithilfe des VSS-Frameworks ein Rollforward für diese Datenbanken ausgeführt werden.

### <a name="full-text-support"></a>Volltextunterstützung

Der SQL Writer vermerkt Volltextkatalog-Container mit rekursiven Dateispezifikationen in den Datenbankkomponenten im Metadatendokument des Writers.  Diese werden automatisch bei der Sicherung berücksichtigt, wenn die Datenbankkomponente ausgewählt ist.

### <a name="differential-backuprestore"></a>Differenzielle Sicherung/Wiederherstellung

Bei einem differenziellen Sicherungsvorgang werden nur die Daten gesichert, die sich seit der letzten vollständigen Sicherung geändert haben. Eine differenzielle Sicherung enthält nur die Teile der Datenbankdateien, die geändert wurden. Für eine solche Sicherung benötigt die Sicherungsanwendung bzw. der Anforderer Informationen über die Stellen, an denen die Änderungen in den Datenbankdateien durchgeführt wurden, damit die richtigen Abschnitte der Datei(en) gesichert werden können. Während eines differenziellen Sicherungsvorgangs stellt der SQL Writer diese Informationen in dem Format bereit, das unter „Format für partielle Dateiinformationen“ angegeben ist. Diese Informationen machen es möglich, nur den geänderten Teil der Datenbankdateien zu sichern.

### <a name="backup"></a>Backup

Der Anforderer kann eine differenzielle Sicherung beauftragen, indem er die Option DIFFERENTIAL (**VSS_BT_DIFFERENTIAL**) im Sicherungskomponentendokument (**IVssBackupComponents::SetBackupState**) angibt, wenn ein Sicherungsvorgang mit VSS initiiert wird.  Der SQL Writer übergibt die partiellen Dateiinformationen (von SQL Server an den SQL Writer übergeben) an VSS.  Der Anforderer kann sich diese Dateiinformationen verschaffen, indem er VSS-APIs abruft (**IVssComponent::GetPartialFile**). Diese partiellen Dateiinformationen ermöglichen es dem Anforderer, für die Sicherung der Datenbankdateien nur die geänderten Bytebereiche auszuwählen.

Während der Phase „Vor der Sicherung anfallende Tasks“ stellt der SQL Writer sicher, dass für jede ausgewählte Datenbank eine einzelne differenzielle Basis vorhanden ist.

Während des PostSnapshot-Ereignisses erhält der SQL Writer die partiellen Dateiinformationen von SQL Server und fügt sie über den Aufruf von **IVssComponent::AddPartialFile** in das Sicherungskomponentendokument ein.  

  > [!NOTE]
  > Der SQL Writer unterstützt nur eine einzelne differenzielle Basis bei differenzielle Sicherungen. Mehrere Basen werden nicht unterstützt.

### <a name="partial-file-information-format"></a>Format für partielle Dateiinformationen

Für jede Datenbank, die während einer differenziellen Sicherung gesichert wird, speichert der SQL Writer die partiellen Dateiinformationen für jede Datenbankdatei. Diese Informationen verwendet der Anforderer bzw. die Sicherungsanwendung, um beim tatsächlichen Sichern der Dateien nur die relevanten Dateiteile auf das Sicherungsmedium zu kopieren. Weitere Informationen zum Format für diese partiellen Dateiinformationen finden Sie in der Dokumentation für den Volumeschattenkopie-Dienst.

Ein Anforderer findet die zu sichernden Dateien durch Aufrufen von **IVssComponent::GetPartialFileCount** und **IVssComponent::GetPartialFile**.  **IVssComponent::GetPartialFile** gibt einen Pfad und einen Dateinamen zurück, die auf die Datei verweisen, sowie eine Bereichszeichenfolge, die angibt, welcher Teil der Datei gesichert werden muss.

Weitere Informationen zum Abrufen der partiellen Dateiinformationen finden Sie in der VSS-Dokumentation unter [Überblick über den Volumeschattenkopie-Dienst](/windows/win32/vss/volume-shadow-copy-service-overview).

### <a name="back-up-files"></a>Sichern von Dateien

Während dieser Phase prüft die Sicherungsanwendung die Writer-Metadaten, die im Sicherungskomponentendokument gespeichert sind, und sichert nur die relevanten Teile der Dateien. (Bei Volltextkatalog-Dateien sollte diese Sicherung auf Basis der Dateizeitstempel durchgeführt werden. Dies wird weiter unten in diesem Artikel beschrieben.)

Bei einer differenziellen Sicherung wird immer die letzte vorhandene Sicherung der Datenbank als Basis verwendet.  Zur Wiederherstellungszeit erkennt SQL Server die Unterschiede zwischen der Basissicherung und den differenziellen Sicherungen. Die Sicherungsanwendung oder der Systemadministrator sind also dafür zuständig, sicherzustellen, dass die differenzielle Sicherung von der erwarteten vollständigen abhängt.  Wenn eine Out-of-Band-Prozedur eine weitere vollständige Sicherung erstellt hat, ist die Sicherungsanwendung möglicherweise nicht in der Lage, die differenzielle Sicherung wiederherzustellen, da sie nicht der „Eigentümer“ der Basissicherung ist.

Derzeit gibt SQL Server einen Fehler zurück, wenn die Bytebereichsinformationen (partielle Dateiinformationen) zu groß sind (mehr als 64 KB als Puffergröße), und fordert den Benutzer auf, eine vollständige Sicherung durchzuführen.

### <a name="interesting-cases-during-backup"></a>Interessante Fälle während der Sicherung

Das Hinzufügen, Entfernen, Verkleinern und Vergrößern von Dateien sowie das Ändern ihres logischen oder physischen Namens sind interessante Vorgänge bei der Sicherung.

**Dateien, die nach der Erstellung der Basis hinzugefügt wurden**

Diese Dateien werden in die partielle Spezifikation aufgenommen, weil jeder Header der SQL-Datenbankdatei in der partiellen Spezifikation enthalten sein muss.  Abgesehen von der Headerseite müssen alle zugeordneten Seiten in der partiellen Spezifikation enthalten sein.

**Dateien, die nach der Erstellung der Basis entfernt wurden**

Möglicherweise wurden nach der Erstellung der Basis Dateien entfernt.  Diese Dateien werden während der differenziellen Sicherung nicht in das Writer-Metadatendokument aufgenommen.  Des Weiteren werden keine partiellen Informationen zu diesen entfernten Dateien hinzugefügt.

**Dateien, die nach der Erstellung der Basis verkleinert wurden**

Die partiellen Dateiinformationen werden erst erfasst, wenn Dateiverkleinerungen auf dem Server deaktiviert wurden.  Dadurch wird sichergestellt, dass niemals partielle Dateiinformationen gesichert werden, die zu den bei der Verkleinerung entfallenen Bereichen einer Datendatei gehören.  

**Dateien, die nach der Erstellung der Basis vergrößert wurden**

Wenn Dateien vor der Erfassung der partiellen Informationen vergrößert wurden, dann sollten die partiellen Informationen die zugeordneten Seiten im vergrößerten Bereich enthalten.  Wenn die Vergrößerung nach der Erfassung der partiellen Informationen stattfand, sind die Änderungen am vergrößerten Bereich nicht in den partiellen Informationen enthalten.  In den nachfolgenden Abschnitten wird erläutert, wie sich solche Änderungen mithilfe eines Protokollrollforwards wiederherstellen lassen.

**Dateien, die nach der Erstellung der Basis logisch umbenannt wurden**

Eine logische Umbenennung der Datei hat keine Auswirkungen auf die Sicherung oder Wiederherstellung, da der logische Dateiname weder im Metadatendokument des Writers noch im Sicherungskomponentendokument erwähnt wird.  (Weitere Informationen finden Sie im Beispiel für ein Writer-Metadatendokument.)

**Dateien, die nach der Erstellung der Basis physisch umbenannt wurden**

Eine physische Umbenennung einer Datenbankdatei wird erst beim Neustart der Datenbank wirksam.  Aus diesem Grund basieren die Datenbankkonfigurationsinformationen oder die Dateipfadinformationen im Puffer der partiellen Dateiinformationen noch immer auf den alten physischen Pfaden. Diese sind die einzigen gültigen Pfade zu den Datenbankdateien in der Momentaufnahme.

### <a name="restore"></a>Restore

Bei einer differenziellen Wiederherstellung sind die Informationen zum Sicherungstyp in den Sicherungsmetadaten enthalten, die der Anforderer an den SQL Writer zurückgibt.  Daher muss der SQL Writer keine besonderen Aktionen ausführen.  SQL Server findet allein heraus, dass es sich um eine differenzielle Wiederherstellung handelt.  SQL Server verarbeitet eine solche differenzielle Wiederherstellung auf dieselbe Weise wie eine native differenzielle Wiederherstellung, die nicht mithilfe von VSS durchgeführt wird.

### <a name="prerestore-phase"></a>Phase vor der Wiederherstellung

Während dieser Phase ändert SQL Server auf Grundlage Dateimetadaten der differenziellen Sicherung alle Dateigrößen in die richtige Größe.  Wenn eine Datei vergrößert wurde, setzt SQL Server den vergrößerten Teil auf Null.  Wenn eine neue Datei erstellt werden muss (weil sie nach der Erstellung der Basis erstellt wurde), setzt SQL Server die neue Datei auf Null. Des Weiteren schließt SQL Server alle Dateihandles, sodass die Sicherungsanwendung die Dateien mit den wiederhergestellten Daten von den Sicherungsmedien überschreiben kann.

### <a name="restore-files"></a>Wiederherstellen von Dateien

Der Client sollte die Dateien auf Grundlage der partiellen Dateispezifikation wiederherstellen.  Die Daten sollten am selben Offset/im selben Bereich der Datenbankdatei wiederhergestellt werden, der in der partiellen Dateispezifikation in den Writer-Metadaten angegeben ist.

Das Hinzufügen, Entfernen, Verkleinern und Vergrößern von Datenbankdateien sowie das Ändern Ihres logischen oder physischen Namens sind interessante Vorgänge zur Wiederherstellungszeit.

**Wenn eine Datenbankdatei nach der Erstellung einer vollständigen Basis hinzugefügt wurde**

Solche Dateien müssen von SQL Server während der Vorbereitungsphase der Wiederherstellung vorab erstellt worden sein.  Sie müssen auf die richtige Größe angepasst und auf Null gesetzt worden sein.  Der Client muss die Daten nur der partiellen Spezifikation entsprechend einfügen (die partielle Spezifikation enthält alle zugeordneten Erweiterungen).

**Wenn eine Datenbankdatei nach der Erstellung einer vollständigen Basis entfernt wurde**

Die partiellen Informationen, die SQL Server bereitstellt, enthalten keinerlei Nachverfolgungsinformationen zu entfernten Dateien.  SQL Server ist für das Erkennen der zu löschenden Dateien verantwortlich. Dazu vergleicht SQL Server die die Metadaten der wiederhergestellten Dateien mit den vorhandenen Containern und löscht diese dann.  Dies erfolgt zur Vorbereitung vor der Wiederherstellung.

**Wenn eine Datenbankdatei nach der Erstellung einer vollständigen Basis vergrößert wurde**

Solche Dateien müssen von SQL Server während der Vorbereitungsphase der Wiederherstellung auf die richtige Größe angepasst worden sein.  Der vergrößerte Bereich muss von SQL Server ebenfalls auf Null gesetzt worden sein.  Der Client kann daher ohne Weiteres die Daten der partiellen Spezifikation entsprechen einfügen, selbst im vergrößerten Bereich.  Wenn die Datei nach der Erstellung der partiellen Informationen erstellt wurde, werden die Änderungen am vergrößerten Bereich anhand des Protokolls wiederhergestellt, das zusammen mit der differenziellen Sicherung gesichert wurde.

**Wenn eine Datenbankdatei nach der Erstellung einer vollständigen Basis verkleinert wurde**

SQL Server ist dafür zuständig, die Datei auf die entsprechend der Metadaten erforderliche Größe abzuschneiden.  Dies erfolgt zur Vorbereitung vor der Wiederherstellung.

**Wenn der logische Name einer Datenbankdatei nach der Erstellung einer vollständigen Basis geändert wurde**

Dies hat keine Auswirkungen auf die Wiederherstellung, da der logische Name weder im Metadatendokument des Writers noch im Sicherungskomponentendokument erwähnt wird.  Die Änderung des logischen Namens wird wiederhergestellt, wenn der Client die Änderung auf die primäre Datenbankdatei anwendet, die die Systemkataloginformationen enthält.

**Wenn der physische Name einer Datenbankdatei nach der Erstellung einer vollständigen Basis geändert wurde**

Wenn die Umbenennung zum Zeitpunkt einer differenziellen Sicherung noch nicht wirksam geworden ist, stellt der Client auch Daten am alten Speicherort wieder her.  Ein Neustart der Datenbank nach der Wiederherstellung führt dazu, dass die physische Umbenennung wirksam wird.  Wenn die physische Umbenennung der Datei zum Zeitpunkt der differenziellen Sicherung bereits wirksam geworden ist, dann wurden alle vorhandenen partiellen Daten aus dem neuen physischen Pfad gesichert.  

### <a name="post-restore"></a>Nach der Wiederherstellung

Während der Ereignisse nach der Wiederherstellung führt der SQL Writer den normalen Rollforwardvorgang und die Wiederherstellung (wenn SetAdditionalRestores() auf FALSE festgelegt ist) der Datenbank aus.

## <a name="differential-backuprestore-for-full-text-catalogs"></a>Differenzielle Sicherung/Wiederherstellung von Volltextkatalogen

SQL Server-Volltextkataloge gehören zu den Datenbankressourcen, die zusammen mit den übrigen Datenbankdateien gesichert oder wiederhergestellt werden müssen.  Eine differenzielle Sicherung ist bei Volltextkatalogen zeitstempelbasiert.  Die differenzielle VSS-Sicherung/Wiederherstellung in SQL Server verfügt über eine einzelne Basissicherung.  Mit anderen Worten: Es kann nicht mehrere Basen für verschiedene Container geben.  Bei der VSS-Sicherung bedeutet dies für alle Volltextkatalog-Container, dass die differenzielle Sicherung auf einem einzelnen Zeitstempel basiert. Dies ist ein Unterschied zur nativen differenziellen SQL-Sicherung, bei der eine Zeitstempelbasis pro Volltextkatalog-Container vorhanden ist.

Dieser Zeitstempel wird in VSS als komponentenübergreifende Eigenschaft ausgedrückt, die während der vollständigen Sicherung festgelegt und bei der darauffolgenden differenziellen Sicherung verwendet wird.  

### <a name="onidentify"></a>OnIdentify

Beim Ereignis „OnIdentify“ ruft der SQL Writer IVssCreateWriterMetadata::SetBackupSchema() auf, um den Wert für VSS_BS_TIMESTAMPED festzulegen.  Dies zeigt der Sicherungsanwendung, dass der SQL Writer für die Verwaltung der differenziellen Basis verantwortlich ist.

### <a name="setting-the-base-timestamp"></a>Festlegen des Basiszeitstempels

Der Basiszeitstempel wird während einer vollständigen Sicherung festgelegt.  In der Methode **OnPostSnapshot()** ruft der Writer **IVssComponent::SetBackupStamp()** auf, um den Zeitstempel mit der Komponente im Sicherungsdokument zu speichern.

### <a name="differential-backup"></a>Differenzielle Sicherung

Die Sicherungsanwendung ruft diesen Zeitstempel aus der vollständigen Basissicherung ab und macht den Zeitstempel für den Writer verfügbar, indem sie **IVssComponent::GetBackupStamp()** aufruft, um den Basiszeitstempel der vorherigen Basissicherung abzurufen.  Anschließend macht sie ihn für den Writer verfügbar, indem sie **IVssBackupComponent::SetPreviousBackupStamp()** aufruft.  Der Writer ruft daraufhin den Zeitstempel ab, indem er **IVssComponent::GetPreviousBackupStamp()** aufruft, und übersetzt ihn in einen Zeitstempel, der für **IVssComponent::AddDifferencedFilesByLastModifyTime()** verwendet wird.  

**Zuständigkeitsbereich der Sicherungsanwendung während der differenziellen Sicherung:** Während einer differenziellen Sicherung ist die Sicherungsanwendung für Folgendes zuständig:

- Sichern aller Dateien (gesamte Datei), deren Zeitstempel der letzten Änderung größer ist als der der Datei, der in der Komponente festgelegt ist
- Nachverfolgen und Erkennen von gelöschten Dateien  

**Zuständigkeitsbereiche der Sicherungsanwendung während der differenziellen Wiederherstellung:** Während einer differenziellen Wiederherstellung ist die Sicherungsanwendung für Folgendes zuständig:

- Wiederherstellen aller Dateien, die gesichert wurden – entweder durch Erstellen einer neuen Datei, falls sie noch nicht vorhanden war, oder durch Überschreiben einer bestehenden Datei, falls sie bereits vorhanden war
- Vergrößern der Datei, bevor der Inhalt eingefügt wird, wenn die wiederhergestellte Datei größer war als die bestehende
- Abschneiden der Datei auf dieselbe Größe, die die wiederhergestellte Datei hat, wenn die wiederhergestellte Datei kleiner als die bestehende ist
- Löschen aller Dateien, die gelöscht werden sollten, also all jener Dateien, die ab dem Zeitpunkt der differenziellen Sicherung nicht mehr vorhanden sein sollten

### <a name="copy-only-backup"></a>Kopiesicherung

Unter Umständen ist es erforderlich, aus einem besonderen Grund eine Sicherung auszuführen, z. B., wenn Sie zu Testzwecken eine Kopie einer Datenbank benötigen.  Diese Sicherung darf sich nicht auf die üblichen Sicherungs- und Wiederherstellungsvorgänge der Datenbank auswirken. Mit der Option COPY_ONLY geben Sie an, dass es sich bei der Sicherung um eine Ausnahme handelt, die keinen Einfluss auf die normale Sicherungssequenz haben soll. Der SQL Writer unterstützt den Sicherungstyp „Kopiesicherung“ für SQL Server-Instanzen.

Während der Ermittlungsphase der Sicherung signalisiert der SQL Writer seine Fähigkeit, eine Kopiesicherung auszuführen, indem die Option für das unterstützte Sicherungsschema VSS_BS_COPY mithilfe eines Aufrufs von **IVssCreateWriterMetadata::SetBackupSchema** festgelegt wird. Der Anforderer kann den Sicherungstyp auf „Kopiesicherung“ festlegen, indem er die Option VSS_BACKUP_TYPE mit einem Aufruf von **IVssBackupComponents::SetBackupState** auf VSS_BT_COPY festlegt.

Wenn eine Kopiesicherung ausgewählt wird, wird davon ausgegangen, dass die Dateien auf dem Datenträger (vom Anforderer) auf ein Sicherungsmedium kopiert werden, unabhängig vom Zustand des Sicherungsverlaufs der einzelnen Dateien. SQL Server wird den Sicherungsverlauf nicht aktualisieren. Dieser Sicherungstyp stellt keine Basissicherung für weitere differenzielle Sicherungsvorgänge dar und stört nicht den Verlauf von vorherigen differenziellen Sicherungen.

### <a name="restore-with-move"></a>Restore with Move (Mit Verschieben wiederherstellen)

VSS lässt zu, dass die Sicherungsanwendung (Anforderer) ein neues Wiederherstellungsziel mithilfe eines Aufrufs von **IVssComponent::SetNewTarget** angibt.  In den Methoden „PreRestore()“ und „PostRestore()“ überprüft der SQL Writer, ob mindestens ein neues Ziel angegeben wurde. Die Sicherungsanwendung ist dafür zuständig, die Datei(en) während des tatsächlichen Wiederherstellens/Kopierens von Dateien physisch an den neuen Speicherort zu kopieren.

Die Sicherungsanwendung darf nur neue Ziele für den physischen Pfad angeben, nicht für die Dateispezifikation.  Für eine Datenbankdatei, die unter dem Pfad c:\data\test.mdf gespeichert wurde, kann der tatsächliche Dateiname (test.mdf) z. B. nicht geändert werden.  Nur der Pfad (c:\data) lässt sich ändern.  Bei einem Volltextkatalog-Container, der sich unter c:\ftdata\foo befindet (da die Dateispezifikation in VSS „*“ und die Pfadspezifikation in VSS c:\ftdata\foo lautet), kann der gesamte Pfad geändert werden.  

### <a name="database-rename"></a>Database Rename (Datenbankumbenennung)

Unter Umständen muss ein Anforderer eine SQL-Datenbank unter einem neuen Namen wiederherstellen. Dies ist vor allem dann erforderlich, wenn die Datenbank parallel zur ursprünglichen Datenbank wiederhergestellt werden soll.  Diese Option kann vom Anforderer während des Wiederherstellungsvorgangs angegeben werden, indem eine benutzerdefinierte Wiederherstellungsoption mit dem VSS-Aufruf **IVssBackupComponents::SetRestoreOptions()** (im Parameter „wszRestoreOptions“) als "New Component Name" = <"New Name"> festgelegt wird.

Der SQL Writer verwendet den gesamten Inhalt des Werts von „New Component Name“ als neuen Namen der wiederhergestellten Datenbank. Falls keine Option angegeben wird, stellt SQL die Datenbank mit ihrem ursprünglichen Namen (Komponentenname) wieder her.

  > [!NOTE]
  > Der SQL Writer unterstützt derzeit nicht die Option „Rename across Instances“ (Datenbankübergreifend umbenennen), um eine Datenbank zu einer neuen Instanz zu verschieben.

### <a name="auto-recovered-snapshots"></a>Automatisch wiederhergestellte Momentaufnahmen

Üblicherweise befindet sich die Momentaufnahme einer SQL Server-Datenbank, die mithilfe des VSS-Frameworks erstellt wurde, in einem nicht wiederhergestellten Zustand. Auf die Daten in der Momentaufnahme kann nicht sicher zugegriffen werden, bevor nicht die Wiederherstellungsphase durchlaufen wird, in der ein Rollback für derzeit ausgeführte Transaktionen ausgeführt und die Datenbank in einen konsistenten Zustand versetzt wird. Da sich die Momentaufnahme in einem schreibgeschützten Zustand befindet, kann sie nicht über den normalen Vorgang des Anfügens der Datenbank wiederhergestellt werden.

Es ist möglich, die Momentaufnahmen automatisch im Rahmen der Momentaufnahmeerstellung wiederherzustellen. Im Metadatendokument des Writers gibt der SQL Writer das Komponentenflag VSS_CF_APP_ROLLBACK_RECOVERY an, um zu signalisieren, dass die Datenbank anhand der Momentaufnahme wiederhergestellt werden muss, bevor auf die Datenbank zugegriffen werden kann. Beim Angeben des Momentaufnahmesatzes kann der Anforderer angeben, dass es sich bei der Momentaufnahme um eine App-Rollback-Momentaufnahme handeln soll (d. h., dass alle Datenbankdateien in einer Momentaufnahme sich in einem für die Anwendungsnutzung konsistenten Zustand befinden müssen) oder um eine Sicherungsmomentaufnahme (eine Momentaufnahme für das Sichern von Daten, die später im Falle eines Systemfehlers wiederhergestellt werden soll).

Der Anforderer muss festlegen, dass VSS_VOLSNAP_ATTR_ROLLBACK_RECOVERY angibt, dass diese Komponente nicht zu Sicherungszwecken gesichert wird.  VSS korreliert anschließend VSS_CF_APP_ROLLBACK_RECOVERY (vom SQL Writer in der ausgewählten Komponente angegeben) mit VSS_VOLSNAP_ATTR_ROLLBACK_RECOVERY und bestimmt, dass eine automatische Wiederherstellung ausgeführt wird.  VSS ermöglicht für einen begrenzten Zeitraum den Schreibzugriff auf die Momentaufnahme und fügt automatisch das VSS_VOLSNAP_ATTR_AUTORECOVERY-Bit zum Kontext der Momentaufnahme hinzu.

In SQL Server sollte die automatische Wiederherstellung nur auf App-Rollback-Momentaufnahmen, nicht auf Sicherungsmomentaufnahmen angewendet werden. Bei App-Rollback-Momentaufnahmen wird ein automatischer Wiederherstellungsprozess vom SQL Writer während des Ereignisses „PostSnapshot“ initiiert. Bei diesem Prozess wird für jede (vom Anforderer) explizit ausgewählte SQL Server-Datenbank im Momentaufnahmesatz Folgendes ausgeführt:

- Anfügen der Momentaufnahme-Datenbank an die ursprüngliche SQL Server-Instanz (d. h., die Instanz, an die die ursprüngliche Datenbank angefügt wurde)
- Wiederherstellen der Datenbank (dies erfolgt im Rahmen des Anfügevorgangs)
- Verkleinern der Protokolldateien

    Dies reduziert die Anzahl unnötiger Kopien bei Schreibvorgängen, die vom VSS-Framework ausgeführt werden müssen, wenn der VSS-Anbieter ein Softwareanbieter ist Das Verkleinern der Protokolldateien ist das Standardverhalten. Dies lässt sich deaktivieren, indem Sie den Wert des folgenden Registrierungsschlüssels auf 1 festlegen.
    
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SQLWriter\
    Settings\DisableLogShrink
    
    Dies kann in Szenarios nützlich sein, in denen mithilfe der Momentaufnahme Daten (zu einem bestimmten Zeitpunkt) aus einer bestimmten Seite aus dem Protokoll exportiert werden sollen, um ein Problem in der Onlinedatenbank zu beheben.

- Trennen Sie die Datenbank.

Nun verfügen Sie über eine konsistente, wiederhergestellte Momentaufnahme, die zu Abfragezwecken angefügt werden kann.

### <a name="multi-database-transactions"></a>Transaktionen zwischen mehreren Datenbanken

In bestimmten Fällen können die Momentaufnahme-Datenbanken Transaktionen enthalten, die derzeit zwischen mehreren Datenbanken ausgeführt werden. Während des Wiederherstellungsvorgangs fügt der SQL Writer die Datenbank mit der Option „Presumed Abort“ (Angenommener Abbruch) an die Momentaufnahmen an. Dadurch würde ein Rollback für jede datenbankübergreifende Transaktion ausgeführt werden, die noch nicht committet wurde (einschließlich aller Transaktionen mit dem Status „Prepared to Commit“ [Bereit für Commit]). Dies kann zu gewissen Inkonsistenzen zwischen den Datenbanken im Momentaufnahmesatz führen. Angenommen, Sie haben die Datenbanken A und B. Zwischen beiden findet eine verteilte Transaktion statt, die in Datenbank A den Status „Committed“ (Committet) und in Datenbank B den Status „Prepared to Commit“ (Bereit für Commit) aufweist. Im Rahmen der automatischen Wiederherstellung wird diese Transaktion in Datenbank A committet, während in Datenbank B ein Rollback für sie ausgeführt wird. Dies kann zu gewissen Inkonsistenzen im Momentaufnahmesatz führen.

Die Komponente „Microsoft Distributed Transaction Coordinator“ (MS DTC), die vom VSS-Framework im Longhorn-Zeitrahmen veröffentlicht werden soll, wird dieses Inkonsistenzproblem bei Transaktionen beheben, die mehrere Datenbanken in mehreren SQL Server-Instanzen betreffen. Die nächste Version von SQL Server wird diese Inkonsistenzen bei Transaktionen beheben, die die Datenbanken innerhalb einer SQL Server-Instanz betreffen.

### <a name="security-implications-for-autorecovered-snapshots"></a>Auswirkungen auf die Sicherheit von automatisch wiederhergestellten Momentaufnahmen

Bei VSS-Momentaufnahmen werden die Dateien nach der automatischen Wiederherstellung mithilfe von Zugriffssteuerungslisten (Access Control Lists, ACLs) gesichert, damit nur Zugriff auf die spezielle integrierte Gruppe besteht, zu der das SQL Server-Konto gehört.  Dies impliziert, dass die Mitglieder der vordefinierten Administratorgruppe oder der besonderen Gruppe in der Lage sind, die Datenbank anzufügen. Der Client, der das Anfügen der Datenbankdateien an eine Momentaufnahme anfordert, muss entweder zum Konto „VORDEFINIERT\Administratoren“ oder dem SQL Server-Konto gehören.

### <a name="special-cases"></a>Sonderfälle

In diesem Abschnitt werden einige der Sonderfälle beschrieben, die bei auf dem SQL Writer basierenden Sicherungs- und Wiederherstellungsvorgängen auftreten können.

#### <a name="autoclose-databases"></a>Autoclose databases (Datenbanken automatisch schließen)

Bei nicht komponentenbasierten Sicherungen, werden Datenbanken automatisch geschlossen, wenn auf Zerrissenheit geprüft wird. Die automatisch geschlossenen Datenbanken werden während der Sicherungsvorgänge jedoch nicht explizit eingefroren.

Das erwartete Szenario ist in diesem Fall, dass viele geschlossene Datenbanken vorhanden sein können und die Kosten der Momentaufnahme minimiert werden sollen.  Automatisch geschlossene Datenbanken werden typischerweise in Low-End-Konfigurationen eingesetzt, in denen Ressourcen knapp sind.

#### <a name="file-list"></a>Dateiliste

Die Dateiliste der einzelnen Datenbanken wird während eines Aufzählungsschritts vor dem Ereignis „Prepare for Backup“ (Sicherung vorbereiten) ermittelt.  Wenn die Liste der Datenbankdateien zwischen der Aufzählung und dem Einfrieren geändert wird, kann die Datenbank zerrissen werden, es sei denn, die Anwendung überprüft die Dateiliste. Es wird nicht erwartet, dass dies zu Problemen führt. Nichtsdestotrotz sollten Anbieter sich dessen bewusst sein.

#### <a name="stopped-instances"></a>Angehaltene Instanzen

Wenn eine Instanz von SQL Server zum Zeitpunkt des Aufzählungsschritts nicht ausgeführt wird, kann keine der Datenbanken aus dieser Instanz ausgewählt werden.

Wenn eine Instanz im Zeitraum zwischen der Aufzählung und dem Ereignis „Prepare for Backup“ (Sicherung vorbereiten) angehalten wird, werden alle Datenbanken in der angehaltenen Instanz ignoriert.

#### <a name="system-and-user-databases"></a>System- und Benutzerdatenbanken

Zu den Systemdatenbanken in SQL Server zählen auch die Master-, die Modell- und die msdb-Datenbank. Sie alle werden mit SQL Server ausgeliefert und installiert. In diesem Abschnitt wird beschrieben, wie diese Datenbanken bei der Erstellung einer Sicherung mit VSS verarbeitet werden.

### <a name="system-databases"></a>Systemdatenbanken

Die Masterdatenbank kann nur durch Anhalten der Instanz, Ersetzen der Datenbankdateien (von der Sicherungsanwendung durchgeführt) und Neustarten der Instanz wiederhergestellt werden. Ein Rollforward ist nicht möglich.

Der SQL Writer unterstützt die Onlinewiederherstellung der Modell- und der msdb-Datenbank, ohne dass die Instanz heruntergefahren werden muss.


#### <a name="simple-recovery-model-user-databases"></a>Benutzerdatenbanken mit dem einfachen Wiederherstellungsmodell

Wenn die Masterdatenbank zusammen mit Benutzerdatenbanken wiederhergestellt wird, die das einfache Wiederherstellungsmodell verwenden, können die Benutzerdatenbanken mit derselben Methode wiederhergestellt werden wie die Masterdatenbank: nach Herunterfahren der Instanz einfach das bzw. die Volume(s) kopieren oder einbinden.  Wenn die SQL-Instanz gestartet wird, wird alles wiederhergestellt.

#### <a name="rolling-forward-user-databases"></a>Ausführen eines Rollforwardvorgangs für Benutzerdatenbanken

Wenn Benutzerdatenbanken wiederhergestellt werden sollen und zusammen mit der Wiederherstellung der Masterdatenbank ein Rollforward für sie ausgeführt wird, muss die Instanz nicht starten und die Master- und die Benutzerdatenbank zusammen wiederherstellen.

Gehen Sie wie folgt vor:

1. Stellen Sie sicher, dass die SQL Server-Instanz angehalten wurde.
1. Führen Sie die Wiederherstellung in zwei Phasen aus.
    1. Stellen Sie die Systemdatenbanken und die Benutzerdatenbanken, die gleichzeitig wiederhergestellt werden sollen, (d. h. die Benutzerdatenbanken mit dem einfachen Wiederherstellungsmodell) über „file copy /volume-mount“ (Datei kopieren/Volume einbinden) über VSS wieder her.
        1. Wenn sich die Benutzerdatenbanken, für die ein Rollforward ausgeführt werden soll, nicht auf demselben Volume befinden wie die Systemdatenbanken, sollte das Volume zu diesem Zeitpunkt nicht zurückgeholt werden. Dieses Szenario erfordert eine Planungsphase vor der Sicherung.
        1. Wenn sich die Benutzerdatenbanken auf demselben Volume wie die Systemdatenbanken befinden, müssen die Benutzerdatenbanken vor SQL Server verborgen werden.
    1. Starten Sie die SQL Server-Instanz mithilfe des Parameters -f.  (Wenn Sie die Startoption -f verwenden, kann nur die Masterdatenbank wiederhergestellt werden.)
        1. Geben Sie ALTER DATABASE \<x> SET OFFLINE für jede Datenbank aus, für die ein Rollforward ausgeführt werden soll.  (Alternativ können Sie die Datenbanken entfernen.)
        1. Halten Sie die SQL Server-Instanz an.
        1. Starten Sie die SQL Server-Instanz (die Dateien der Benutzerdatenbank, für die ein Rollforward aufgeführt werden soll, sind für SQL Server nicht sichtbar).

Verwenden Sie VSS, um die Benutzerdatenbanken mit NORECOVERY wiederherzustellen. Dies wurde im Abschnitt „Vollständige Wiederherstellung mit zusätzlichen Rollforwardvorgängen“ näher erklärt.

## <a name="appendix"></a>Anhang

### <a name="writer-metadata-document--an-example"></a>Metadatendokument des Writers:  ein Beispiel

Angenommen, Sie haben eine Beispieldatenbank namens DB1, die zur SQL Server-Instanz „Instance1“ auf dem Computer „Server1“ gehört und die die folgenden Datenbank-/Protokolldateien enthält:

- Eine Datenbankdatei namens „primary“, die unter c:\db\DB1.mdf gespeichert ist
- Eine Datenbankdatei namens „secondary“, die unter c:\db\DB1.mdf gespeichert ist
- Eine Datenbankprotokolldatei namens „log“, die unter c:\db\DB1.ldf gespeichert ist
- Einen Volltextkatalog namens „foo“, der im Verzeichnis c:\db\ftdata\foo gespeichert ist
- Einen Volltextkatalog namens „bar“, der im Verzeichnis c:\db\ftdata\bar gespeichert ist

Nachstehend sind die Writer-Metadaten der Datenbank aufgeführt:

**Database level filegroup component** (Dateigruppenkomponente auf Datenbankebene)

- ComponentType (Komponententyp): VSS_CT_FILEGROUP
- LogicalPath (Logischer Pfad): "Server1\Instance1"
- ComponentName (Komponentenname): "DB1"
- Caption (Beschriftung): NULL
- pbIcon (pb-Symbol): NULL
- cbIcon (cb-Symbol): 0
- bRestoreMetadata (b Metadaten wiederherstellen): FALSE
- NotifyOnBackupComplete (Bei abgeschlossener Sicherung benachrichtigen): TRUE
- Selectable (Auswählbar): TRUE
- SelectableForRestore (Für die Wiederherstellung auswählbar): TRUE
- ComponentFlags (Komponentenflags): VSS_CF_APP_ROLLBACK_RECOVERY

**Filegroup file** (Dateigruppendatei)

- LogicalPath (Logischer Pfad): "Server1\Instance1"
- GroupName (Gruppenname): "DB1"
- Path (Pfad): "c:\db"
- FileSpec (Dateispezifikation): "DB1.mdf"
- Recursive (Rekursiv): FALSE
- AlternatePath (Alternativer Pfad): NULL
- BackupTypeMask (Sicherungstypmaske): VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED
- Filegroup file (Dateigruppendatei)
- LogicalPath (Logischer Pfad): "Server1\Instance1"
- GroupName (Gruppenname): "DB1"
- Path (Pfad): "c:\db"
- FileSpec (Dateispezifikation): "DB1.ndf"
- Recursive (Rekursiv): FALSE
- AlternatePath (Alternativer Pfad): NULL
- BackupTypeMask (Sicherungstypmaske): VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

**Filegroup file** (Dateigruppendatei)

- LogicalPath (Logischer Pfad): "Server1\Instance1"
- GroupName (Gruppenname): "DB1"
- Path (Pfad): "c:\db"
- FileSpec (Dateispezifikation): "DB1.ldf"
- Recursive (Rekursiv): FALSE
- AlternatePath (Alternativer Pfad): NULL
- BackupTypeMask (Sicherungstypmaske): VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

**Filegroup file** (Dateigruppendatei)

- LogicalPath (Logischer Pfad): "Server1\Instance1"
- GroupName (Gruppenname): "DB1"
- Path (Pfad): "c:\db\ftdata\foo"
- FileSpec (Dateispezifikation): "*"
- Recursive (Rekursiv): TRUE
- AlternatePath (Alternativer Pfad): NULL
- BackupTypeMask (Sicherungstypmaske): VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

**Filegroup file** (Dateigruppendatei)

- LogicalPath (Logischer Pfad): "Server1\Instance1"
- GroupName (Gruppenname): "DB1"
- Path (Pfad): "c:\db\ftdata\bar"
- FileSpec (Dateispezifikation): "*"
- Recursive (Rekursiv): TRUE
- AlternatePath (Alternativer Pfad): NULL
- BackupTypeMask (Sicherungstypmaske): VSS_FSBT_ALL_BACKUP_REQUIRED | VSS_FSBT_ALL_SNAPSHOT_REQUIRED

Wenn die Serverinstanz die Standardinstanz auf dem Computer ist, ist der logische Pfad einteilig: "Server1".


    
## <a name="see-also"></a>Weitere Informationen  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Sichern und Wiederherstellen von SQL Server-Datenbanken](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [Kopiesicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md)   
 [Transaktionsprotokollsicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md)   
 [Anwenden von Transaktionsprotokollsicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)    
 [Handbuch zur Architektur und Verwaltung von Transaktionsprotokollen in SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)
  
