---
title: Hilfe zum Installations-Assistenten | Microsoft-Dokumentation
description: Geben Sie über die Instanzkonfiguration im Installations-Assistenten für SQL Server an, ob eine Standardinstanz oder eine benannte Instanz von SQL Server erstellt werden soll.
ms.custom: ''
ms.date: 08/16/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
f1_keywords:
- instance configuration, Setup
helpviewer_keywords:
- Instance Name page [SQL Server Installation Wizard]
- SQL Server Installation Wizard, Instance Name page
ms.assetid: 5bf822fc-6dec-4806-a153-e200af28e9a5
author: cawrites
ms.author: chadam
robots: noindex,nofollow
ms.openlocfilehash: 7b678ac25174b75db198739e0e9bc191d29f18d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336162"
---
# <a name="installation-wizard-help"></a>Hilfe zum Installations-Assistenten

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In diesem Artikel werden einige der Konfigurationsseiten im Installations-Assistenten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] beschrieben.

## <a name="instance-configuration-page"></a>Seite „Instanzkonfiguration“

Verwenden Sie die Seite **Instanzkonfiguration** des Installations-Assistenten für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , um anzugeben, ob eine Standardinstanz oder eine benannte Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]erstellt werden soll. Wenn noch keine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf dem Server installiert wurde, wird eine Standardinstanz erstellt, es sei denn Sie geben eine benannte Instanz an.  
  
Jede [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz besteht aus einem Satz von Diensten mit bestimmten Einstellungen für Sortierungen und andere Optionen. Verzeichnisstruktur, Registrierungsstruktur und Dienstnamen spiegeln den Instanznamen und die von Ihnen bei der Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] angegebene Instanz-ID wider.  
  
Bei der Instanz kann es sich um eine Standardinstanz oder um eine benannte Instanz handeln. Der Name der Standardinstanz lautet MSSQLSERVER. Für den Standardnamen ist kein Client erforderlich, um den Instanznamen zur Herstellung einer Verbindung anzugeben. Eine benannte Instanz wird vom Benutzer während des Setups festgelegt. Sie können [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als benannte Instanz installieren, ohne zuvor die Standardinstanz zu installieren. Es kann jeweils nur eine Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], unabhängig von der Version, als Standardinstanz fungieren.  
  
> [!NOTE]  
> Bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep können Sie den Instanznamen angeben, wenn Sie auf der Seite **Instanzkonfiguration** eine vorbereitete Instanz abschließen. Sie können die vorbereitete Instanz, die Sie abschließen, als Standardinstanz konfigurieren, wenn sich auf dem Computer keine vorhandene Standardinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] befindet.  
  
### <a name="multiple-instances"></a>Mehrere Instanzen
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt mehrere Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem einzelnen Server oder Prozessor, es kann jedoch nur eine einzige Instanz als Standardinstanz fungieren. Alle anderen müssen benannte Instanzen sein. Auf einem Computer können mehrere Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gleichzeitig ausgeführt werden, und jede Instanz wird unabhängig von den anderen Instanzen ausgeführt.  
  
 Weitere Informationen finden Sie unter [Spezifikationen der maximalen Kapazität für SQL Server](../maximum-capacity-specifications-for-sql-server.md).  
  
### <a name="options"></a>Tastatur

**Nur Failoverclusterinstanzen**: Geben Sie den Netzwerkname für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Failovercluster an. Anhand dieses Namens wird die Failoverclusterinstanz im Netzwerk identifiziert.  
  
**Entscheiden Sie, ob eine Standardinstanz oder eine benannte Instanz verwendet werden soll**: Bei der Entscheidung der Frage, ob eine Standardinstanz oder eine benannte Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installiert werden soll, müssen folgende Informationen beachtet werden:  
  
* Wenn Sie eine einzelne Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem Datenbankserver installieren möchten, sollte dies eine Standardinstanz sein.  
  
* Verwenden Sie eine benannte Instanz , wenn Sie mehrere Instanzen auf dem gleichen Computer verwenden möchten. Ein Server kann nur als Host für eine einzelne Standardinstanz dienen.  
* Jede Anwendung, die [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] installiert, sollte die Installation als benannte Instanz vornehmen. Dadurch werden die Konflikte für den Fall minimiert, dass mehrere Anwendungen auf demselben Computer installiert werden.
  
**Standardinstanz**: Wählen Sie diese Option aus, um eine Standardinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu installieren. Ein Computer kann nur als Host für eine Standardinstanz dienen, alle anderen Instanzen müssen benannt sein. Wenn eine Standardinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installiert ist, ist es jedoch möglich, demselben Computer eine Standardinstanz von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] hinzufügen.  
  
**Benannte Instanz**: Wählen Sie diese Option aus, um eine neue benannte Instanz zu erstellen. Beachten Sie beim Benennen einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] folgende Informationen:  
  
* Bei Instanznamen wird die Groß-/Kleinschreibung nicht berücksichtigt.  
  
* Instanznamen dürfen nicht mit einem Unterstrich (_) beginnen oder enden.  
  
* Instanznamen dürfen keine Begriffe wie „Standard“ oder andere reservierte Schlüsselwörter enthalten. Wenn in einem Instanznamen ein reserviertes Schlüsselwort verwendet wird, tritt ein Setupfehler auf. Weitere Informationen finden Sie unter [Reservierte Schlüsselwörter &#40;Transact-SQL&#41;](../../t-sql/language-elements/reserved-keywords-transact-sql.md).  
  
* Wenn Sie MSSQLSERVER als Instanznamen angeben, wird eine Standardinstanz erstellt.  
  
* Eine Installation von [!INCLUDE[ssGeminiLong](../../includes/ssgeminilong-md.md)] wird immer als benannte Instanz von „[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]“ installiert. Sie können keinen anderen Instanznamen für diese Funktionsrolle angeben.  
  
* Instanznamen können maximal 16 Zeichen enthalten.  
  
* Das erste Zeichen eines Instanznamens muss ein Buchstabe sein. Akzeptable Buchstaben sind diejenigen, die durch Unicode Standard 2.0 definiert werden. Zu diesen Buchstaben gehören die lateinischen Zeichen a-z, A-Z sowie Buchstaben aus anderen Sprachen.  
  
* Bei den anderen Zeichen kann es sich um Buchstaben des Unicode-Standards 2.0, Dezimalzeichen aus dem lateinischen Grundalphabet oder anderen nationalen Schriften, das Dollarzeichen ($) oder einen Unterstrich (_) handeln.  
  
* Eingebettete Leerzeichen und sonstige Sonderzeichen sind in Instanznamen nicht zugelassen. Umgekehrte Schrägstriche (\\), Kommas (,), Doppelpunkte (:), Strichpunkte (;), einfache Anführungszeichen ('), kaufmännische Und-Zeichen (&), Bindestriche (-) und das At-Zeichen (@) sind ebenfalls nicht zulässig.  
  
  In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanznamen können nur gültige Zeichen der aktuellen Windows-Codepage verwendet werden. Wenn ein nicht unterstütztes Unicode-Zeichen verwendet wird, tritt ein Setupfehler auf.  
  
**Erkannte Instanzen und Funktionen**: Zeigt eine Liste der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanzen und Komponenten an, die auf dem Computer installiert sind, auf dem das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup ausgeführt wird.  
  
**Instanz-ID**: In der Standardeinstellung wird der Instanzname als Instanz-ID verwendet. Mit der ID werden Installationsverzeichnisse und Registrierungsschlüssel für die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identifiziert. Dasselbe Verhalten tritt für Standardinstanzen und benannte Instanzen auf. Bei einer Standardinstanz lauten Instanzname und Instanz-ID MSSQLSERVER. Um eine nicht standardmäßige Instanz-ID zu verwenden, geben Sie sie im Feld **Instanz-ID** an.  
  
> [!IMPORTANT]  
>  Bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep handelt es sich bei der auf der Seite **Instanzkonfiguration** angezeigten Instanz-ID um die Instanz-ID, die Sie während des Schritts Image vorbereiten des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep-Prozesses angegeben haben. Sie sind nicht in der Lage, während des Schritts Image abschließen eine andere Instanz-ID anzugeben.

> [!NOTE]  
>  Instanz-IDs, die mit einem Unterstrich (_) beginnen oder das Nummernzeichen (#) oder Dollarzeichen ($) enthalten, werden nicht unterstützt.  
  
Weitere Informationen zu Verzeichnissen, Dateispeicherorten und Namen für Instanz-IDs finden Sie unter [Dateispeicherorte für Standard- und benannte Instanzen von SQL Server](file-locations-for-default-and-named-instances-of-sql-server.md).  
  
Alle Komponenten einer gegebenen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden als Einheit verwaltet. Alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Service Packs und Updates werden für jede Komponente einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] übernommen.  
  
Alle Komponenten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , die denselben Instanznamen verwenden, müssen die folgenden Kriterien erfüllen:  
  
* Gleiche Version
* Gleiche Edition
* Gleiche Spracheinstellungen
* Gleicher gruppierter Status
* Gleiches Betriebssystem  
  
> [!NOTE]  
> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ist nicht clusterabhängig.  

## <a name="analysis-services-configuration---account-provisioning-page"></a>Analysis Services-Konfiguration – Kontobereitstellung
  
Verwenden Sie diese Seite zum Einrichten des Servermodus und zum Zuweisen von administrativen Berechtigungen an Benutzer oder Dienste, die uneingeschränkten Zugriff auf [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] benötigen. Während des Setups wird die lokale Windows-Gruppe „VORDEFINIERT\Administratoren“ der [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Serveradministratorrolle der Instanz, die Sie installieren, nicht automatisch hinzugefügt. Wenn Sie der Serveradministratorrolle die lokale Gruppe "Administratoren" hinzufügen möchten, müssen Sie diese Gruppe explizit angeben.  
  
Bei der Installation von [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] sollten Sie Administratoren von SharePoint-Farmen oder -Diensten, die für eine [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]-Serverbereitstellung in einer [!INCLUDE[SPS2010](../../includes/sps2010-md.md)]-Farm zuständig sind, Administratorrechte gewähren.  
  
### <a name="options"></a>Tastatur

**Servermodus**: Der Servermodus gibt den Typ von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Datenbanken an, die auf dem Server bereitgestellt werden können. Servermodi werden während des Setups bestimmt und können später nicht mehr geändert werden. Die einzelnen Modi schließen einander aus. Das bedeutet, dass Sie zwei Instanzen von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] benötigen, die jeweils für einen anderen Modus konfiguriert sind, um beide klassische Lösungen – die analytische Onlineverarbeitung (OLAP) und das tabellarische Modell – zu unterstützen.  
  
**Administratoren angeben**: Sie müssen wenigstens einen Serveradministrator für die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] angeben. Die Benutzer oder die Gruppen, die Sie angeben, werden Mitglieder der Serveradministratorrolle der [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Instanz, die Sie installieren. Diese Mitglied müssen Windows-Domänenbenutzerkonten in der gleichen Domäne wie der Computer sein, auf dem Sie die Software installieren.  
  
> [!NOTE]  
> Die Benutzerkontensteuerung (User Account Control, UAC) ist eine Windows-Sicherheitsfunktion, bei der ein Administrator administrative Aktionen oder Anwendungen einzeln genehmigen muss, bevor diese ausgeführt werden können. Da die Benutzerkontensteuerung in den Standardeinstellungen aktiviert ist, werden Sie aufgefordert, einzelne Vorgänge, für die erhöhte Berechtigungen erforderlich sind, zu genehmigen. Sie können die Benutzerkontensteuerung konfigurieren, um das Standardverhalten zu ändern, oder sie für bestimmte Programme anpassen. Weitere Informationen über die Benutzerkontensteuerung und deren Konfiguration finden Sie unter [Schritt-für-Schritt-Anleitung zur Benutzerkontensteuerung](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc709691(v=ws.10)) und [Benutzerkontensteuerung (Wikipedia)](https://go.microsoft.com/fwlink/?linkid=196351).  
  
### <a name="see-also"></a>Weitere Informationen
  
* [Konfigurieren von Dienstkonten &#40;Analysis Services&#41;](/analysis-services/instances/configure-service-accounts-analysis-services)
* [Konfigurieren von Windows-Dienstkonten und -Berechtigungen](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)  

## <a name="analysis-services-configuration---data-directories-page"></a>Konfigurationseigenschaften von Analysis Services – Seite „Datenverzeichnisse“

Die in der folgenden Tabelle angegebenen Standardverzeichnisse können beim Setup von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vom Benutzer konfiguriert werden: Die Berechtigung zum Zugriff auf diese Dateien wird den lokalen Administratoren und Mitgliedern der Sicherheitsgruppe „SQLServerMSASUser$\<instance>“ gewährt, die während des Setups erstellt und bereitgestellt wird.  
  
### <a name="ui-element-list"></a>Liste der Benutzeroberflächenelemente  
  
|BESCHREIBUNG|Standardverzeichnis|Empfehlungen|  
|-----------------|-----------------------|---------------------|  
|**Datenstammverzeichnis**|\<Drive:>\Programme\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Data\ |Stellen Sie sicher, dass der Ordner \Programme\Microsoft SQL Server\ durch entsprechend eingeschränkte Berechtigungen geschützt wird. Die Leistung von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ist in vielen Konfigurationen von der Leistung des Speichers abhängig, in dem sich das Datenverzeichnis befindet. Platzieren Sie dieses Verzeichnis im Speicher mit der höchsten Leistung, der mit dem System verknüpft ist. Stellen Sie für Failoverclusterinstallationen sicher, dass Datenverzeichnisse auf dem freigegebenen Datenträger platziert werden.|  
|**Protokolldateiverzeichnis**|\<Drive:>\Programme\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Log\ |Dies ist das Verzeichnis für [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Protokolldateien, das auch das FlightRecorder-Protokoll enthält. Wenn Sie die Dauer von Flight Recorder erhöhen, stellen Sie sicher, dass für das Protokollverzeichnis genügend Speicherplatz zur Verfügung steht.|  
|**Temporäres Verzeichnis**|\<Drive:>\Programme\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Temp\ |Platzieren Sie das temporäre Verzeichnis in einem leistungsstarken Speichersubsystem.|  
|**Sicherungsverzeichnis**|\<Drive:>\Programme\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Backup\ |Dies ist das Verzeichnis für Standardsicherungsdateien von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Bei einer [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] für SharePoint-Installation werden in diesem Verzeichnis zudem die [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]-Datendateien des [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]-Systemdiensts zwischengespeichert.<br /><br /> Stellen Sie sicher, dass zur Vermeidung von Datenverlusten entsprechende Berechtigungen festgelegt wurden und die Benutzergruppe für [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] über die erforderlichen Berechtigungen zum Schreiben im Sicherungsverzeichnis verfügt. Die Verwendung eines zugeordneten Laufwerks für Sicherungsverzeichnisse wird nicht unterstützt.|  
  
### <a name="considerations"></a>Überlegungen  
  
* Auf einer SharePoint-Farm bereitgestellte [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Instanzen speichern Anwendungsdateien, Datendateien und Eigenschaften in Inhaltsdatenbanken und Dienstanwendungsdatenbanken.  
  
* Wenn Sie Funktionen zu einer vorhandenen Installation hinzufügen, können Sie den Speicherort einer vorher installierten Funktion nicht ändern und auch keinen Speicherort für eine neue Funktion angeben.  

* Möglicherweise müssen Sie Softwareanwendungen wie beispielsweise Viren- und Spywarescanner so konfigurieren, dass SQL Server-Ordner und -Dateitypen ausgeschlossen werden. Weitere Informationen finden Sie im folgenden Support-Artikel: [Auswählen, wie Antivirussoftware auf Computern ausgeführt werden soll, auf denen SQL Server ausgeführt wird](https://support.microsoft.com/kb/309422).
  
* Wenn Sie andere Installationsverzeichnisse als die Standardverzeichnisse angeben, müssen Sie sicherstellen, dass die Installationsordner nur für diese Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet werden. Keines der Verzeichnisse in diesem Dialogfeld darf gemeinsam mit Verzeichnissen anderer Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genutzt werden. Die [!INCLUDE[ssDE](../../includes/ssde-md.md)]- und [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Komponenten in einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sollten ebenfalls in separaten Verzeichnissen installiert werden.  
  
* Programmdateien und Datendateien können in den folgenden Situationen nicht installiert werden:  
  
  * Auf einem Wechseldatenträger  
  * In einem Dateisystem, in dem die Komprimierung verwendet wird  
  * In einem Verzeichnis, in dem sich Systemdateien befinden  
  
### <a name="see-also"></a>Weitere Informationen

Weitere Informationen zu Verzeichnissen, Dateispeicherorten und Namen für Instanz-IDs finden Sie unter [Dateispeicherorte für Standard- und benannte Instanzen von SQL Server](file-locations-for-default-and-named-instances-of-sql-server.md).  
  
### <a name="analysis-services-configuration---data-directories-page"></a>Konfigurationseigenschaften von Analysis Services – Seite „Datenverzeichnisse“

Die in der folgenden Tabelle angegebenen Standardverzeichnisse können beim Setup von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vom Benutzer konfiguriert werden: Die Berechtigung zum Zugriff auf diese Dateien wird den lokalen Administratoren und Mitgliedern der Sicherheitsgruppe „SQLServerMSASUser$\<instance>“ gewährt, die während des Setups erstellt und bereitgestellt wird.  
  
#### <a name="ui-element-list"></a>Liste der Benutzeroberflächenelemente
  
|BESCHREIBUNG|Standardverzeichnis|Empfehlungen|  
|-----------------|-----------------------|---------------------|  
|**Datenstammverzeichnis** |\<Drive:>\Programme\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Data |Stellen Sie sicher, dass der Ordner \Programme\Microsoft SQL Server\ durch entsprechend eingeschränkte Berechtigungen geschützt wird. Die Leistung von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ist in vielen Konfigurationen von der Leistung des Speichers abhängig, in dem sich das Datenverzeichnis befindet. Platzieren Sie dieses Verzeichnis im Speicher mit der höchsten Leistung, der mit dem System verknüpft ist. Stellen Sie für Failoverclusterinstallationen sicher, dass Datenverzeichnisse auf dem freigegebenen Datenträger platziert werden.|  
|**Protokolldateiverzeichnis**|\<Drive:>\Programme\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Log |Dies ist das Verzeichnis für [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Protokolldateien, das auch das FlightRecorder-Protokoll enthält. Wenn Sie die Dauer von Flight Recorder erhöhen, stellen Sie sicher, dass für das Protokollverzeichnis genügend Speicherplatz zur Verfügung steht.|  
|**Temporäres Verzeichnis**|\<Drive:>\Programme\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Temp |Platzieren Sie das temporäre Verzeichnis in einem leistungsstarken Speichersubsystem.|  
|**Sicherungsverzeichnis**|\<Drive:>\Programme\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Backup |Dies ist das Verzeichnis für Standardsicherungsdateien von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Bei einer [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] für SharePoint-Installation werden in diesem Verzeichnis zudem die [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]-Datendateien des [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]-Systemdiensts zwischengespeichert.<br /><br /> Stellen Sie sicher, dass zur Vermeidung von Datenverlusten entsprechende Berechtigungen festgelegt wurden und die Benutzergruppe für den [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] -Dienst über die erforderlichen Berechtigungen zum Schreiben im Sicherungsverzeichnis verfügt. Die Verwendung eines zugeordneten Laufwerks für Sicherungsverzeichnisse wird nicht unterstützt.|  
  
#### <a name="considerations"></a>Überlegungen
  
* Auf einer SharePoint-Farm bereitgestellte [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Instanzen speichern Anwendungsdateien, Datendateien und Eigenschaften in Inhaltsdatenbanken und Dienstanwendungsdatenbanken.  
  
* Wenn Sie Funktionen zu einer vorhandenen Installation hinzufügen, können Sie den Speicherort einer vorher installierten Funktion nicht ändern und auch keinen Speicherort für eine neue Funktion angeben.  

* Möglicherweise müssen Sie Softwareanwendungen wie beispielsweise Viren- und Spywarescanner so konfigurieren, dass SQL Server-Ordner und -Dateitypen ausgeschlossen werden. Weitere Informationen finden Sie unter [Antivirensoftware auf Computern mit SQL Server](https://support.microsoft.com/kb/309422).
  
* Wenn Sie andere Installationsverzeichnisse als die Standardverzeichnisse angeben, müssen Sie sicherstellen, dass die Installationsordner nur für diese Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet werden. Keines der Verzeichnisse in diesem Dialogfeld darf gemeinsam mit Verzeichnissen anderer Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genutzt werden. Die [!INCLUDE[ssDE](../../includes/ssde-md.md)]- und [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Komponenten in einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sollten ebenfalls in separaten Verzeichnissen installiert werden.  
  
* Programmdateien und Datendateien können in den folgenden Situationen nicht installiert werden:  
  
  * Auf einem Wechseldatenträger  
  * In einem Dateisystem, in dem die Komprimierung verwendet wird  
  * In einem Verzeichnis, in dem sich Systemdateien befinden  
  
#### <a name="see-also"></a>Weitere Informationen

* Weitere Informationen zu Verzeichnissen, Dateispeicherorten und Namen für Instanz-IDs finden Sie unter [Dateispeicherorte für Standard- und benannte Instanzen von SQL Server](file-locations-for-default-and-named-instances-of-sql-server.md).  
* [Share- und NTFS-Berechtigung für einen Dateiserver](/iis/web-hosting/configuring-servers-in-the-windows-web-platform/configuring-share-and-ntfs-permissions)

## <a name="database-engine-configuration---server-configuration-page"></a><a name="serverconfig"></a> Konfiguration der Datenbank-Engine – Seite „Serverkonfiguration“

Auf dieser Seite können Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sicherheitsmodus festlegen und Windows-Benutzer oder -Gruppen als Administratoren von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]hinzufügen.  
  
### <a name="considerations-for-running-sscurrent"></a>Überlegungen für das Ausführen von [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]

In früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wurde die Gruppe „VORDEFINIERT\Administratoren“ für die Anmeldung bei [!INCLUDE[ssDE](../../includes/ssde-md.md)] bereitgestellt, und Mitglieder der lokalen Administratorgruppe konnten sich mit ihren Administratoranmeldeinformationen anmelden. Die Verwendung von erhöhten Berechtigungen ist aber keine bewährte Methode.

In [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] wird die Gruppe „VORDEFINIERT\Administratoren“ nicht für die Anmeldung bereitgestellt. Erstellen Sei für jeden Administratorbenutzer eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldung, und fügen Sie diese während der Installation einer neuen Instanz von [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] der festen Serverrolle **sysadmin** hinzu. Gehen Sie genauso vor für Windows-Konten, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Agent-Aufträge ausführen, einschließlich Replikations-Agent-Aufträge.  
  
### <a name="options"></a>Tastatur

**Sicherheitsmodus**: Wählen Sie für die Installation die **Windows-Authentifizierung** oder die **Authentifizierung im gemischten Modus** aus.  
  
**Windows-Prinzipal-Bereitstellung**: In früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] befand sich die lokale Windows-Gruppe „VORDEFINIERT\Administratoren“ in der Serverrolle **sysadmin** von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], wodurch Windows-Administratoren effektiv Zugriff auf die Instanz [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erhielten. In [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] wird die Gruppe „VORDEFINIERT\Administratoren“ in der Serverrolle **sysadmin** nicht bereitgestellt. Stattdessen sollten Sie während des Setups explizit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Administratoren für Neuinstallationen festlegen.  
  
> [!IMPORTANT]  
> Sie müssen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Administratoren für Neuinstallationen während des Setups explizit festlegen. Sie können den Setup-Vorgang erst nach Ausführung dieses Schrittes fortsetzen.
  
**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Administratoren angeben**: Sie müssen wenigstens einen Windows-Prinzipal für die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] angeben. Um das Konto hinzuzufügen, unter dem das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Setup ausgeführt wird, klicken Sie auf die Schaltfläche **Aktuellen Benutzer hinzufügen**. Um Konten zur Liste der Systemadministratoren hinzuzufügen bzw. daraus zu entfernen, klicken Sie auf **Hinzufügen** bzw. **Entfernen**, und bearbeiten Sie anschließend die Liste der Benutzer, Gruppen bzw. Computer, die Administratorrechte für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz haben sollen.  
  
Wenn Sie die Bearbeitung der Liste abgeschlossen haben, klicken Sie auf **OK** und überprüfen anschließend die Liste der Administratoren im Konfigurationsdialogfeld. Wenn die Liste vollständig ist, wählen Sie **Weiter** aus.  
  
Bei Auswahl der **Authentifizierung im gemischten Modus** müssen Sie Anmeldeinformationen für das vordefinierte Systemadministratorkonto (**sa**) von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] angeben.  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
**Windows-Authentifizierungsmodus**: Wenn ein Benutzer eine Verbindung über ein Windows-Benutzerkonto herstellt, überprüft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Kontonamen und Kennwort mithilfe des Tokens für den Windows-Prinzipal im Betriebssystem. Die Windows-Authentifizierung ist der Standardauthentifizierungsmodus und bietet eine höhere Sicherheit als die Authentifizierung im gemischten Modus. Die Windows-Authentifizierung verwendet das Kerberos-Sicherheitsprotokoll, stellt Maßnahmen zur Durchsetzung von Kennwortrichtlinien, z. B. zur Überprüfung der Komplexität sicherer Kennwörter, bereit und bietet Unterstützung für Kontosperrungen und Ablauf von Kennwörtern.  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  

> [!IMPORTANT]  
> [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)] Legen Sie auf keinen Fall ein leeres oder unsicheres **sa**-Kennwort fest.  
  
**Gemischter Modus (Windows-Authentifizierung oder [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung)** : Ermöglicht Benutzern das Herstellen einer Verbindung mithilfe der Windows-Authentifizierung bzw. der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung. Benutzer, die mithilfe eines Windows-Benutzerkontos Verbindungen herstellen, können vertrauenswürdige Verbindungen verwenden, die von Windows überprüft werden.  
  
Wenn Sie die Authentifizierung im gemischten Modus auswählen und beim Ausführen älterer Anwendungen ausschließlich SQL-Anmeldungen verwenden, müssen Sie für alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Konten sichere Kennwörter festlegen.  
  
> [!NOTE]  
> Die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung wird nur aus Gründen der Abwärtskompatibilität bereitgestellt. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
**Kennwort eingeben**: Geben Sie die Systemadministratoranmeldung (**sa**) ein, und bestätigen Sie sie. Kennwörter dienen als erste Schutzmaßnahme gegen Eindringlinge, daher ist die Festlegung sicherer Kennwörter von entscheidender Bedeutung für die Systemsicherheit. Legen Sie auf keinen Fall ein leeres oder unsicheres **sa**-Kennwort fest.  
  
> [!NOTE]  
> Kennwörter für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] können zwischen 1 und 128 Zeichen (Kombinationen aus Buchstaben, Sonderzeichen und Ziffern) enthalten. Wenn Sie die Authentifizierung im gemischten Modus auswählen, müssen Sie ein sicheres **sa**-Kennwort eingeben, bevor Sie den Vorgang auf der nächsten Seite des Installations-Assistenten fortsetzen können.  
  
#### <a name="strong-password-guidelines"></a>Richtlinien für sichere Kennwörter
  
Sichere Kennwörter können von anderen nur schwer erraten und selbst von Computerprogrammen nicht ohne größeren Aufwand ausgespäht werden. Beim Festlegen von sicheren Kennwörtern dürfen keine verbotenen Richtlinien oder Bestimmungen verwendet werden, z.B.:  
  
* Leeres Kennwort oder NULL
* Password
* "Admin"
* "Administrator"
* "sa"
* "sysadmin"

Beim Festlegen von sicheren Kennwörtern dürfen keine Angaben verwendet werden, die auf den Computer mit der Installation verweisen:  
  
* Der Name des Benutzers, der zurzeit auf dem Computer angemeldet ist.
* Der Computername  
  
Ein sicheres Kennwort muss mehr als 8 Zeichen enthalten und wenigstens drei der folgenden vier Kriterien erfüllen:  
  
* Es muss Großbuchstaben enthalten.
* Es muss Kleinbuchstaben enthalten.  
* Es muss Zahlen enthalten.
* Es muss nicht alphanumerische Zeichen, z. B. #, % oder ^, enthalten.  
  
Auf dieser Seite eingegebene Kennwörter müssen die Anforderungen der Richtlinien für sichere Kennwörter erfüllen. Bei Automatisierungen, die die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung verwenden, sollten Sie sicherstellen, dass das Kennwort die Anforderungen der Richtlinien für sichere Kennwörter erfüllt.  
  
### <a name="see-also"></a>Weitere Informationen

Weitere Informationen zum Auswählen von Windows-Authentifizierung im Vergleich zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung finden Sie im Thema [Auswählen eines Authentifizierungsmodus](../../relational-databases/security/choose-an-authentication-mode.md).  

Weitere Informationen zur Auswahl eines Kontos für die Ausführung von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]finden Sie unter [Konfigurieren von Windows-Dienstkonten und -Berechtigungen](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).

## <a name="database-engine-configuration---data-directories-page"></a><a name ="datadir"></a> Konfiguration der Datenbank-Engine – Seite „Datenverzeichnisse“

Verwenden Sie diese Seite, um den Installationspfad für das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Programm und die Datendateien anzugeben. Auf Grundlage des Typs der Installation schließt der unterstützte Speicher möglicherweise einen lokalen Datenträger, freigegebenen Speicher oder SMB-Dateiserver ein.  
  
Um eine SMB-Dateifreigabe als Verzeichnis anzugeben, müssen Sie den unterstützten UNC-Pfad manuell eingeben. Das Navigieren zu einer SMB-Dateifreigabe wird nicht unterstützt. Das folgende Format ist ein unterstütztes UNC-Pfadformat einer SMB-Dateifreigabe:

`\\<ServerName>\<ShareName>\...`

### <a name="standalone-instance-of-ssnoversion"></a>Eigenständige Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
  
In der folgenden Tabelle sind die unterstützten Speichertypen und Standardverzeichnisse für eigenständige Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aufgeführt, die Sie während des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Setups konfigurieren können:  
  
### <a name="ui-element-list"></a>Liste der Benutzeroberflächenelemente
  
|BESCHREIBUNG|Unterstützte Speichertypen|Standardverzeichnis|Empfehlungen|  
|-----------------|----------------------------|-----------------------|---------------------|  
|**Datenstammverzeichnis**|Lokaler Datenträger, SMB-Dateiserver, freigegebener Speicher* |\<Drive:>\Programme\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\ |[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Das Setup konfiguriert Zugriffssteuerungslisten (ACLs) für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verzeichnisse und unterbricht während der Konfiguration die Vererbung.|  
|**Benutzerdatenbankverzeichnis**|Lokaler Datenträger, SMB-Dateiserver, freigegebener Speicher*|\<Drive:>\Programme\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data |Welche Vorgehensweise für Benutzerdatenverzeichnisse empfohlen wird, hängt von der Arbeitsauslastung und den Leistungsanforderungen ab.|  
|**Benutzerdatenbank-Protokollverzeichnis**|Lokaler Datenträger, SMB-Dateiserver, freigegebener Speicher*|\<Drive:>\Programme\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data|Stellen Sie sicher, dass ausreichend Speicherplatz für das Protokollverzeichnis verfügbar ist.|  
|**Sicherungsverzeichnis**|Lokaler Datenträger, SMB-Dateiserver, freigegebener Speicher*|\<Drive:>\Programme\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Backup|Legen Sie zur Vermeidung von Datenverlusten entsprechende Berechtigungen fest, und stellen Sie sicher, dass das Benutzerkonto für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienst über die erforderlichen Berechtigungen zum Schreiben im Sicherungsverzeichnis verfügt. Die Verwendung eines zugeordneten Laufwerks für Sicherungsverzeichnisse wird nicht unterstützt.|  
  
\* Obwohl freigegebene Datenträger unterstützt werden, empfehlen wir nicht, diese für eine eigenständige Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu verwenden.  
  
### <a name="failover-cluster-instance-of-ssnoversion"></a>Failoverclusterinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

In der folgenden Tabelle sind die unterstützten Speichertypen und Standardverzeichnisse für eine Failoverclusterinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aufgeführt, die Sie während des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Setups konfigurieren können.  
  
|BESCHREIBUNG|Unterstützte Speichertypen|Standardverzeichnis|Empfehlungen|  
|-----------------|----------------------------|-----------------------|---------------------|  
|**Datenstammverzeichnis**|Freigegebener Speicher, SMB-Dateiserver|\<Drive:>\Programme\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\<br /><br /> **Tipp**: Wenn Sie **Freigegebener Datenträger** auf der Seite zur **Datenträgerauswahl für Cluster** ausgewählt haben, ist der Standardwert der erste freigegebene Datenträger. Dieses Feld ist standardmäßig leer, wenn keine Auswahl auf der Seite **Datenträgerauswahl für Cluster** getroffen wurde.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Das Setup konfiguriert ACLs für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verzeichnisse und unterbricht während der Konfiguration die Vererbung.|  
|**Benutzerdatenbankverzeichnis**|Freigegebener Speicher, SMB-Dateiserver|\<Drive:>Programme\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data<br /><br /> **Tipp**: Wenn Sie **Freigegebener Datenträger** auf der Seite zur **Datenträgerauswahl für Cluster** ausgewählt haben, ist der Standardwert der erste freigegebene Datenträger. Dieses Feld ist standardmäßig leer, wenn keine Auswahl auf der Seite **Datenträgerauswahl für Cluster** getroffen wurde.|Welche Vorgehensweise für Benutzerdatenverzeichnisse empfohlen wird, hängt von der Arbeitsauslastung und den Leistungsanforderungen ab.|  
|**Benutzerdatenbank-Protokollverzeichnis**|Freigegebener Speicher, SMB-Dateiserver|\<Drive:>\Programme\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data<br /><br /> **Tipp**: Wenn Sie **Freigegebener Datenträger** auf der Seite zur **Datenträgerauswahl für Cluster** ausgewählt haben, ist der Standardwert der erste freigegebene Datenträger. Dieses Feld ist standardmäßig leer, wenn keine Auswahl auf der Seite **Datenträgerauswahl für Cluster** getroffen wurde.|Stellen Sie sicher, dass ausreichend Speicherplatz für das Protokollverzeichnis verfügbar ist.|  
|**Sicherungsverzeichnis**|Lokaler Datenträger, freigegebener Speicher, SMB-Dateiserver|\<Drive:>\Programme\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Backup<br /><br /> **Tipp**: Wenn Sie **Freigegebener Datenträger** auf der Seite zur **Datenträgerauswahl für Cluster** ausgewählt haben, ist der Standardwert der erste freigegebene Datenträger. Dieses Feld ist standardmäßig leer, wenn keine Auswahl auf der Seite **Datenträgerauswahl für Cluster** getroffen wurde.|Legen Sie zur Vermeidung von Datenverlusten entsprechende Berechtigungen fest, und stellen Sie sicher, dass das Benutzerkonto für den SQL Server-Dienst über die erforderlichen Berechtigungen zum Schreiben im Sicherungsverzeichnis verfügt. Die Verwendung eines zugeordneten Laufwerks für Sicherungsverzeichnisse wird nicht unterstützt.|  
  
### <a name="security-considerations"></a>Sicherheitshinweise
  
Das Setup konfiguriert Zugriffssteuerungslisten für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Verzeichnisse und unterbricht während der Konfiguration die Vererbung.  
  
Die folgenden Empfehlungen gelten für den SMB-Dateiserver:  
  
* Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienstkonto muss ein Domänenkonto sein, wenn ein SMB-Dateiserver verwendet wird.  
  
* Das Konto, das verwendet wurde, um [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu installieren, muss über FULL CONTROL NTFS-Berechtigungen für den als Datenverzeichnis verwendeten SMB-Dateifreigabeordner verfügen.  
  
* Dem Konto, das für die Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet wird, müssen SeSecurityPrivilege-Berechtigungen auf dem SMB-Dateiserver zugewiesen werden. Diese Berechtigung weisen Sie zu, indem Sie die Konsole "Lokale Sicherheitsrichtlinie" auf dem Dateiserver verwenden, um der Richtlinie zum [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verwalten von Überwachungs- und Sicherheitsprotokollen **das** -Setupkonto hinzuzufügen. Diese Einstellung ist in der Konsole für lokale Sicherheitsrichtlinien im Abschnitt **Zuweisen von Benutzerrechten** unter **Lokale Richtlinien** verfügbar.

### <a name="considerations"></a>Überlegungen
  
* Wenn Sie Funktionen zu einer vorhandenen Installation hinzufügen, können Sie den Speicherort einer vorher installierten Funktion nicht ändern und auch keinen Speicherort für eine neue Funktion angeben.  
  
* Wenn Sie andere Installationsverzeichnisse als die Standardverzeichnisse angeben, müssen Sie sicherstellen, dass die Installationsordner nur für diese Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet werden. Keines der Verzeichnisse in diesem Dialogfeld darf gemeinsam mit Verzeichnissen anderer Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genutzt werden. Die [!INCLUDE[ssDE](../../includes/ssde-md.md)]- und [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Komponenten in einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sollten ebenfalls in separaten Verzeichnissen installiert werden.  
  
* Programmdateien und Datendateien können in den folgenden Situationen nicht installiert werden:  
  
  * Auf einem Wechseldatenträger
  * In einem Dateisystem, in dem die Komprimierung verwendet wird
  * In einem Verzeichnis, in dem sich Systemdateien befinden
  * Auf einem zugeordneten Netzlaufwerk auf einer Failoverclusterinstanz  
  
## <a name="a-nametempdb-database-engine-configuration---tempdb-page"></a><a name="tempdb"><a/> Konfiguration der Datenbank-Engine – Seite „tempdb“

Verwenden Sie diese Seite, um den Speicherort, die Größe und die Vergrößerungseinstellungen für **tempdb**-Daten und -Protokolle anzugeben, ebenso wie die Anzahl an Dateien für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Auf Grundlage des Typs der Installation schließt der unterstützte Speicher möglicherweise einen lokalen Datenträger, freigegebenen Speicher oder SMB-Dateiserver ein.  
  
Um eine SMB-Dateifreigabe als Verzeichnis anzugeben, müssen Sie den unterstützten UNC-Pfad manuell eingeben. Das Navigieren zu einer SMB-Dateifreigabe wird nicht unterstützt. Das folgende Format ist ein unterstütztes UNC-Pfadformat einer SMB-Dateifreigabe:

`\\<ServerName>\<ShareName>\....`
  
### <a name="data-and-log-directories-for-a-standalone-instance-of-ssnoversion"></a>Daten- und Protokollverzeichnisse für eine eigenständige Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

In der folgenden Tabelle sind die unterstützten Speichertypen und Standardverzeichnisse für eigenständige Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aufgeführt, die Sie während des Setups konfigurieren können.  
  
|BESCHREIBUNG|Unterstützter Speichertyp|Standardverzeichnis|Empfehlungen|  
|-----------------|----------------------------|-----------------------|---------------------|  
|**Datenverzeichnisse**|Lokaler Datenträger, SMB-Dateiserver, freigegebener Speicher* |\<Drive:>\Programme\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Das Setup konfiguriert ACLs für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verzeichnisse und unterbricht während der Konfiguration die Vererbung.<br /><br /> Welche Vorgehensweise für die **tempdb**-Verzeichnisse empfohlen wird, hängt von der Arbeitsauslastung und den Leistungsanforderungen ab. Geben Sie verschiedenen Ordner oder Laufwerke an, um die Datendateien über mehrere Volumes zu verteilen.|  
|**Protokollverzeichnis**|Lokaler Datenträger, SMB-Dateiserver, freigegebener Speicher*|\<Drive:>\Programme\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data|Stellen Sie sicher, dass ausreichend Speicherplatz für das Protokollverzeichnis verfügbar ist.|  
  
\* Obwohl freigegebene Datenträger unterstützt werden, empfehlen wir nicht, diese für eine eigenständige Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu verwenden.  
  
### <a name="data-and-log-directories-for-a-failover-cluster-instance-of-ssnoversion"></a>Daten- und Protokollverzeichnisse für eine Failoverclusterinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

In der folgenden Tabelle sind die unterstützten Speichertypen und Standardverzeichnisse für eine Failoverclusterinstanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aufgeführt, die Sie während des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Setups konfigurieren können.  
  
|BESCHREIBUNG|Unterstützter Speichertyp|Standardverzeichnis|Empfehlungen|  
|-----------------|----------------------------|-----------------------|---------------------|  
|**tempdb-Datenverzeichnis**|Lokaler Datenträger, freigegebener Speicher, SMB-Dateiserver|\<Drive:>\Programme\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\Data<br /><br /> **Tipp**: Wenn Sie **Freigegebener Datenträger** auf der Seite zur **Datenträgerauswahl für Cluster** ausgewählt haben, ist der Standardwert der erste freigegebene Datenträger. Dieses Feld ist standardmäßig leer, wenn keine Auswahl auf der Seite **Datenträgerauswahl für Cluster** getroffen wurde.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Das Setup konfiguriert ACLs für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verzeichnisse und unterbricht während der Konfiguration die Vererbung.<br /><br /> Stellen Sie sicher, dass das angegebene Verzeichnis oder die angegebenen Verzeichnisse (falls mehrere Dateien angegeben sind) für alle Clusterknoten gültig sind. Falls die **tempdb** -Verzeichnisse auf dem Failoverzielknoten während des Failovers nicht verfügbar sind, wird die SQL Server-Ressource nicht online geschaltet.|  
|**tempdb-Protokollverzeichnis**|Lokaler Datenträger, freigegebener Speicher, SMB-Dateiserver|\<Drive:>\Programme\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data<br /><br /> **Tipp**: Wenn Sie **Freigegebener Datenträger** auf der Seite zur **Datenträgerauswahl für Cluster** ausgewählt haben, ist der Standardwert der erste freigegebene Datenträger. Dieses Feld ist standardmäßig leer, wenn keine Auswahl auf der Seite **Datenträgerauswahl für Cluster** getroffen wurde.|Welche Vorgehensweise für Benutzerdatenverzeichnisse empfohlen wird, hängt von der Arbeitsauslastung und den Leistungsanforderungen ab.<br /><br /> Stellen Sie sicher, dass das angegebene Verzeichnis für alle Clusterknoten gültig ist. Falls die **tempdb** -Verzeichnisse auf dem Failoverzielknoten während des Failovers nicht verfügbar sind, wird die SQL Server-Ressource nicht online geschaltet.<br /><br /> Stellen Sie sicher, dass ausreichend Speicherplatz für das Protokollverzeichnis verfügbar ist.|  
  
### <a name="ui-element-list"></a>Liste der Benutzeroberflächenelemente

Konfigurieren Sie die Einstellungen für **tempdb** entsprechend Ihrer Arbeitsauslastung und den entsprechenden Anforderungen. Die folgenden Einstellungen gelten für **tempdb** -Datendateien:  
  
* **Anzahl von Dateien** ist die Gesamtzahl der Datendateien für **tempdb**. Der Standardwert ist der niedrigere der folgenden beiden Werte: entweder acht, oder die Anzahl der logischen Kerne, die vom Setup erkannt wurden. Folgende Faustregel gilt: Wenn die Anzahl von logischen Prozessoren kleiner oder gleich acht ist, verwenden Sie genauso viele Datendateien wie logische Prozessoren. Wenn die Anzahl der logischen Prozessoren größer als 8 ist, verwenden Sie 8 Datendateien. Falls daraufhin weiterhin ein Konflikt besteht, erhöhen Sie die Anzahl von Datendateien um ein Vielfaches von vier (bis zur Anzahl der logischen Prozessoren), bis der Konflikt auf ein akzeptables Ausmaß reduziert ist, oder ändern Sie die Arbeitsauslastung bzw. den Code.
  
* **Anfangsgröße (MB)** ist die Anfangsgröße in Megabyte für jede **tempdb**-Datendatei. Der Standardwert ist 8 MB (oder 4 MB für [!INCLUDE[ssexpress](../../includes/ssexpress_md.md)]). [!INCLUDE[sssql17](../../includes/sssql17-md.md)]führt eine maximale anfängliche Dateigröße von 262.144 MB (256 GB) ein. [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hat eine maximale anfängliche Dateigröße von 1024 MB. Alle **tempdb** -Datendateien haben die gleichen Anfangsgröße. Da **tempdb** bei jedem Start von SQL Server und bei jedem, von SQL Server durchgeführten Failover neu erstellt wird, sollten Sie eine Größe angeben, die der Größe nahekommt, die Ihre Arbeitsauslastung im Normalbetrieb erfordert. Aktivieren Sie die [Sofortige Datenbankdateiinitialisierung](../../relational-databases/databases/database-instant-file-initialization.md), um die Erstellung von **tempdb** weiter zu optimieren.  
  
* **Gesamtanfangsgröße (MB)** ist die kumulierte Größe aller **tempdb**-Datendateien.  
  
* **Automatische Vergrößerung (MB)** ist die Menge des Speicherplatzes in Megabyte, um die sich jede **tempdb** -Datendatei automatisch vergrößert, wenn es keinen Speicherplatz mehr gibt. In [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] und später vergrößern sich alle Datendateien gleichzeitig um die in dieser Einstellung angegebene Menge.  
  
* **Gesamte automatische Vergrößerung (MB)** ist die kumulierte Größe der einzelnen Ereignisse der automatischen Vergrößerung.  
* **Datenverzeichnisse** zeigen alle Verzeichnisse, die **tempdb** -Datendateien enthalten. Wenn mehrere Verzeichnisse vorhanden sind, werden die Datendateien reihum in den Verzeichnissen platziert. Falls Sie beispielsweise drei Verzeichnisse erstellen und acht Datendateien angeben, werden die Datendateien 1, 4 und 7 im ersten Verzeichnis erstellt. Die Datendateien 2, 5 und 8 werden im zweiten Verzeichnis erstellt. Die Datendateien 3 und 6 befinden sich im dritten Verzeichnis.  
  
* Klicken Sie auf **Hinzufügen…** , um Verzeichnisse hinzufügen.  
  
* Um ein Verzeichnis zu entfernen, wählen Sie das Verzeichnis aus, und klicken Sie auf **Entfernen**.  
  
**tempdb-Protokolldatei** ist der Name der Protokolldatei. Diese Datei wird automatisch erstellt. Die folgenden Einstellungen gelten nur für **tempdb** -Protokolldateien:  
  
* **Anfangsgröße (MB)** ist die Anfangsgröße der **tempdb** -Protokolldatei. Der Standardwert ist 8 MB (oder 4 MB für [!INCLUDE[ssexpress](../../includes/ssexpress_md.md)]). [!INCLUDE[sssql17](../../includes/sssql17-md.md)]führt eine maximale anfängliche Dateigröße von 262.144 MB (256 GB) ein. [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hat eine maximale anfängliche Dateigröße von 1024 MB. Da **tempdb** bei jedem Start von SQL Server und bei jedem, von SQL Server durchgeführten Failover neu erstellt wird, sollten Sie eine Größe angeben, die der Größe nahekommt, die Ihre Arbeitsauslastung im Normalbetrieb erfordert. Aktivieren Sie die [Sofortige Datenbankdateiinitialisierung](../../relational-databases/databases/database-instant-file-initialization.md), um die Erstellung von **tempdb** weiter zu optimieren.  
  
  > [!NOTE]
  > **tempdb** verwendet minimale Protokollierung. Die **tempdb**-Protokolldatei kann nicht gesichert werden. Es wird jedes Mal neu erstellt, wenn der SQL Server startet oder wenn eine Clusterinstanz einen Failover ausführt.

* **Automatische Vergrößerung (MB)** ist das Vergrößerungsinkrement des **tempdb** -Protokolls in Megabyte.  Der Standardwert von 64 MB erstellt die optimale Anzahl der virtuellen Protokolldateien während der Initialisierung.  

  > [!NOTE]
  > **tempdb**-Protokolldateien verwenden keine sofortige Datenbankdateiinitialisierung.
  
* **Protokollverzeichnis** ist das Verzeichnis, in dem **tempdb** -Protokolldateien erstellt werden. Es gib nur ein **tempdb**-Protokollverzeichnis.  
  
### <a name="security-considerations"></a>Sicherheitshinweise
  
Das Setup konfiguriert ACLs für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verzeichnisse und unterbricht während der Konfiguration die Vererbung.  

Die folgenden Empfehlungen gelten für den SMB-Dateiserver:  
  
* Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienstkonto muss ein Domänenkonto sein, wenn ein SMB-Dateiserver verwendet wird.  
  
* Das zum Installieren von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendete Konto muss über die NTFS-Berechtigung **Vollzugriff** für den SMB-Dateifreigabeordner verfügen, der als Datenverzeichnis verwendet wird.  
  
* Dem Konto, das für die Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet wird, müssen SeSecurityPrivilege-Berechtigungen auf dem SMB-Dateiserver zugewiesen werden. Diese Berechtigung weisen Sie zu, indem Sie die Konsole "Lokale Sicherheitsrichtlinie" auf dem Dateiserver verwenden, um der Richtlinie zum [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verwalten von Überwachungs- und Sicherheitsprotokollen **das** -Setupkonto hinzuzufügen. Diese Einstellung ist in der Konsole für lokale Sicherheitsrichtlinien im Abschnitt **Zuweisen von Benutzerrechten** unter **Lokale Richtlinien** verfügbar.  
  
> [!NOTE]
> Wenn Sie andere Installationsverzeichnisse als die Standardverzeichnisse angeben, müssen Sie sicherstellen, dass die Installationsordner nur für diese Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet werden. Keines der Verzeichnisse in diesem Dialogfeld sollte gemeinsam mit Verzeichnissen anderer Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]genutzt werden. Die [!INCLUDE[ssDE](../../includes/ssde-md.md)]- und [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Komponenten in einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sollten ebenfalls in separaten Verzeichnissen installiert werden.
  
### <a name="see-also"></a>Weitere Informationen

* [Konfigurieren von Windows-Dienstkonten und -Berechtigungen](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)
* [Share- und NTFS-Berechtigung für einen Dateiserver](/iis/web-hosting/configuring-servers-in-the-windows-web-platform/configuring-share-and-ntfs-permissions)  

<!--
The MaxDOP setting applies only to SQL Server 2019 and later.
-->

::: moniker range=">=sql-server-ver15"

## <a name="a-namemaxdop-database-engine-configuration---maxdop-page"></a><a name="maxdop"><a/> Konfiguration der Datenbank-Engine – Seite „MaxDOP“

**Der maximale Grad an Parallelität (MaxDOP)** bestimmt die maximale Anzahl von Prozessoren, die eine einzelne Anweisung verwenden kann. Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] kann diese Option während der Installation konfiguriert werden. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] erkennt außerdem basierend auf der Anzahl von Kernen automatisch die empfohlene MaxDOP-Einstellung für den Server.  

Wird diese Seite während des Setups übersprungen, wird als MaxDOP-Standardwert nicht der [!INCLUDE[ssde_md](../../includes/ssde_md.md)]-Standardwert für vorherige Versionen (0), sondern der auf dieser Seite angezeigte empfohlene Wert verwendet. Sie können diese Einstellung auf dieser Seite auch manuell konfigurieren, und Sie können diese Einstellung nach der Installation ändern. 

### <a name="ui-element-list"></a>Liste der Benutzeroberflächenelemente

* **Max. Grad an Parallelität (MaxDOP)** ist der Wert für die maximale Anzahl von Prozessoren, die während der parallelen Ausführung einer einzelnen Anweisung verwendet werden sollen. Der Standardwert entspricht den Richtlinien für die maximale Nebenläufigkeit in [Konfigurieren der Serverkonfigurationsoption „Max. Grad an Parallelität“](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md#Guidelines).

## <a name="a-namememory-database-engine-configuration---memory-page"></a><a name="memory"><a/> Konfiguration der Datenbank-Engine – Seite „Arbeitsspeicher“

**Min. Serverarbeitsspeicher** bestimmt das untere Arbeitsspeicherlimit, das [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] für den Pufferpool und andere Caches verwendet. Der Standardwert lautet 0, und der empfohlene Wert lautet ebenfalls 0. Weitere Informationen zu den Auswirkungen von **Min. Serverarbeitsspeicher** finden Sie im [Handbuch zur Architektur der Speicherverwaltung](../../relational-databases/memory-management-architecture-guide.md#effects-of-min-and-max-server-memory).

**Max. Serverarbeitsspeicher** bestimmt das obere Arbeitsspeicherlimit, das [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] für den Pufferpool und andere Caches verwendet. Der Standardwert lautet 2.147.483.647 Megabyte (MB), und die berechneten empfohlenen Werte entsprechen den Richtlinien für die Arbeitsspeicherkonfiguration in den [Konfigurationsoptionen für den Serverarbeitsspeicher](../../database-engine/configure-windows/server-memory-server-configuration-options.md#manually) für eine eigenständige [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz, basierend auf dem vorhandenen Systemarbeitsspeicher. Weitere Informationen zu den Auswirkungen von **Max. Serverarbeitsspeicher** finden Sie im [Handbuch zur Architektur der Speicherverwaltung](../../relational-databases/memory-management-architecture-guide.md#effects-of-min-and-max-server-memory).

Wird diese Seite während des Setups übersprungen, entspricht der Standardwert für **Max. Serverarbeitsspeicher** dem Standardwert für [!INCLUDE[ssde_md](../../includes/ssde_md.md)] (2.147.483.647 MB). Sie können diese Einstellungen auf dieser Seite manuell konfigurieren, nachdem Sie das Optionsfeld **Empfohlen** ausgewählt haben, und Sie können diese Einstellung nach der Installation ändern. Weitere Informationen finden Sie unter [Konfigurationsoptionen für den Serverarbeitsspeicher](../../database-engine/configure-windows/server-memory-server-configuration-options.md).

### <a name="ui-element-list"></a>Liste der Benutzeroberflächenelemente
  
**Standard:** Dieses Optionsfeld ist standardmäßig ausgewählt und legt die Einstellungen **Min. Serverarbeitsspeicher** und **Max. Serverarbeitsspeicher** auf die Standardwerte für [!INCLUDE[ssde_md](../../includes/ssde_md.md)] fest. 

**Empfohlen:** Dieses Optionsfeld muss ausgewählt werden, um die berechneten empfohlenen Werte zu übernehmen oder um die berechneten Werte in vom Benutzer konfigurierte Werte zu ändern.  
  
**Min. Serverarbeitsspeicher (MB)** : Wenn der berechnete empfohlene Wert in einen vom Benutzer konfigurierten Wert geändert wird, geben Sie den Wert für **Min. Serverarbeitsspeicher** an.  
  
**Max. Serverarbeitsspeicher (MB)** : Wenn der berechnete empfohlene Wert in einen vom Benutzer konfigurierten Wert geändert wird, geben Sie den Wert für **Max. Serverarbeitsspeicher** an.  

**Klicken Sie hier, um die empfohlenen Arbeitsspeicherkonfigurationen für die SQL Server-Datenbank-Engine zu akzeptieren**: Aktivieren Sie dieses Kontrollkästchen, um die berechneten empfohlenen Arbeitsspeicherkonfigurationen für diesen Server zu akzeptieren. Nach Auswahl des Optionsfelds **Empfohlen** kann das Setup nur fortgesetzt werden, wenn dieses Kontrollkästchen aktiviert wurde.

::: moniker-end

## <a name="database-engine-configuration---filestream-page"></a>Konfiguration der Datenbank-Engine – Seite „FILESTREAM“

Verwenden Sie diese Seite, um FILESTREAM für diese Installation von [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] zu aktivieren. FILESTREAM integriert [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] in ein NTFS-Dateisystem, indem Blobdaten (Binary Large Object) vom Typ **varbinary(max)** als Dateien im Dateisystem gespeichert werden. [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen können FILESTREAM-Daten eingefügt, aktualisiert, abgefragt, gesucht und gesichert werden. Die Microsoft Win32-Dateisystemschnittstellen stellen Streamingzugriff auf die Daten bereit. 
  
### <a name="ui-element-list"></a>Liste der Benutzeroberflächenelemente
  
**FILESTREAM für Transact-SQL-Zugriff aktivieren**: Wählen Sie diese Option aus, um FILESTREAM für den [!INCLUDE[tsql](../../includes/tsql-md.md)] -Zugriff zu aktivieren. Dieses Kontrollkästchen muss ausgewählt werden, bevor die anderen Optionen zur Verfügung stehen.  
  
**FILESTREAM für E/A-Streamingzugriff auf Datei aktivieren**: Wählen Sie diese Option aus, um den Win32-Streamingzugriff für FILESTREAM zu aktivieren.  
  
**Windows-Freigabename**: Geben Sie den Namen der Windows-Freigabe an, in der die FILESTREAM-Daten gespeichert werden.  
  
**Streamingzugriff von Remoteclients auf FILESTREAM-Daten zulassen**: Aktivieren Sie dieses Kontrollkästchen, damit Remoteclients auf diese FILESTREAM-Daten auf diesem Server zugreifen können.  
  
### <a name="see-also"></a>Weitere Informationen

* [Aktivieren und Konfigurieren von FILESTREAM](../../relational-databases/blob/enable-and-configure-filestream.md)
* [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  

## <a name="database-engine-configuration---user-instance-page"></a>Konfiguration der Datenbank-Engine – Seite „Benutzerinstanz“

Auf der Seite **Benutzerinstanz** haben Sie folgende Möglichkeiten:

* Generieren Sie eine separate [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz für Benutzer ohne Administratorberechtigungen.
* Fügen Sie Benutzer zur Administratorrolle hinzu.  
  
### <a name="options"></a>Tastatur
  
**Benutzerinstanzen aktivieren**: Der Standardwert ist ON. Um die Möglichkeit zum Aktivieren von Benutzerinstanzen zu deaktivieren, deaktivieren Sie das Kontrollkästchen.  
  
Die Benutzerinstanz, auch bekannt als untergeordnete Instanz oder Clientinstanz, ist eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , die durch die übergeordnete Instanz (die primäre Instanz, die wie [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]als Dienst ausgeführt wird) für den Benutzer generiert wurde. Die Benutzerinstanz wird als Benutzerprozess in dem Sicherheitskontext des Benutzers ausgeführt. Die Benutzerinstanz ist von der übergeordneten Instanz und anderen Benutzerinstanzen, die auf dem Computer ausgeführt werden, isoliert. Im Zusammenhang mit der Benutzerinstanzfunktion wird auch der Ausdruck „Ausführen von Instanzen als normaler Benutzer“ (RANU, Run As Normal User) verwendet.  
  
> [!NOTE]  
> Während des Setups als Mitglieder der festen Serverrolle **sysadmin** bereitgestellte Anmeldungen werden in der Vorlagendatenbank als Administratoren bereitgestellt. Sie sind so lange Mitglieder der festen Serverrolle **sysadmin** auf der Benutzerinstanz, bis sie entfernt werden.  
  
**Benutzer zur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Administratorrolle hinzufügen**:  Die Standardeinstellung ist OFF. Aktivieren Sie dieses Kontrollkästchen, um den aktuellen Setupbenutzer der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Administratorrolle hinzuzufügen.  
  
[!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)]-Benutzer, die Mitglieder von „VORDEFINIERT\Administratoren“ sind, werden nicht automatisch der festen Serverrolle **sysadmin** hinzugefügt, wenn sie eine Verbindung mit [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] herstellen. Nur [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)]-Benutzer, die explizit einer Administratorrolle auf Serverebene hinzugefügt wurden, können [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] verwalten. Mitglieder der Gruppe „VORDEFINIERT\Administratoren“ können eine Verbindung mit der [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]-Instanz herstellen, verfügen jedoch nur über begrenzte Berechtigungen für Datenbankaufgaben. Daher müssen Benutzern, deren [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]-Berechtigungen in früheren Versionen von Windows von „VORDEFINIERT\Administratoren“ und VORDEFINIERT\Benutzer vererbt wurden, unter [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] explizit Administratorberechtigungen in Instanzen von [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)]erteilt werden.  
  
Um nach Ende des Installationsprogramms Änderungen an den Benutzerrollen vorzunehmen, verwenden Sie [SQL Server Management Studio](../../relational-databases/security/authentication-access/join-a-role.md) oder [Transact-SQL](../../t-sql/statements/alter-role-transact-sql.md).