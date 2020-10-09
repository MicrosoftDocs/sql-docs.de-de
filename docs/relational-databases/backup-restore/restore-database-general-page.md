---
title: Datenbank wiederherstellen (Seite „Allgemein“) | Microsoft-Dokumentation
description: Verwenden Sie bei der Wiederherstellung einer Datenbank in SQL Server die Seite „Allgemein“, um Informationen zu Ziel- und Quelldatenbanken für den Datenbankwiederherstellungsvorgang anzugeben.
ms.custom: ''
ms.date: 09/28/2020
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.restoredb.general.f1
ms.assetid: 160cf58c-b06a-475f-9a69-2b051e5767ab
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 5c322f8c798a7b4a319df67e56565af54ceac20f
ms.sourcegitcommit: 9386ae1b90705a39d37d5541b70c5e8a6564f253
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2020
ms.locfileid: "91662179"
---
# <a name="restore-database-general-page"></a>Datenbank wiederherstellen (Seite 'Allgemein')

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Verwenden Sie die Seite **Allgemein** , um Informationen zu Ziel- und Quelldatenbanken für einen Wiederherstellungsvorgang der Datenbank anzugeben.  
    
-   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Definieren eines logischen Sicherungsmediums für ein Bandlaufwerk &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)  
  
> [!NOTE]  
>  Wenn Sie einen Wiederherstellungstask mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] angeben, können Sie das entsprechende [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md)-Skript ([!INCLUDE[tsql](../../includes/tsql-md.md)]) generieren, indem Sie auf **Skript** klicken und anschließend ein Ziel für das Skript auswählen.  
  
## <a name="permissions"></a>Berechtigungen  
Damit ein RESTORE-Vorgang für eine nicht vorhandene Datenbank ausgeführt werden kann, muss der Benutzer die CREATE DATABASE-Berechtigungen besitzen. Die RESTORE-Berechtigungen werden standardmäßig den Mitgliedern der festen Serverrollen **sysadmin** und **dbcreator** sowie dem Besitzer (**dbo**) der vorhandenen Datenbank erteilt.  

Die Mitgliedschaftsinformationen für die Rollen mit RESTORE-Berechtigungen sind immer auf der Instanz verfügbar.

In einer zugreifbaren und unbeschädigten Datenbank kann die Mitgliedschaft bei einer festen Datenbankrolle überprüft werden. Mitglieder der festen Datenbankrolle **db_owner** besitzen keine RESTORE-Berechtigungen.  

Die Mitgliedschaft bei einer festen Datenbankrolle kann nur überprüft werden, wenn die Datenbank zugreifbar und unbeschädigt ist. Das ist bei der Ausführung von RESTORE nicht immer der Fall. Mitglieder der festen Datenbankrolle **db_owner** besitzen keine RESTORE-Berechtigungen.  

RESTORE-Berechtigungen werden Rollen erteilt, in denen Mitgliedsinformationen immer für den Server verfügbar sind. Die Mitgliedschaft bei einer festen Datenbankrolle kann nur überprüft werden, wenn die Datenbank zugreifbar und unbeschädigt ist. Dies erfolgt bei der Ausführung von RESTORE regelmäßig. Mitglieder der festen Datenbankrolle **db_owner** besitzen keine RESTORE-Berechtigungen.  

Um eine verschlüsselte Sicherung wiederherstellen zu können, müssen **VIEW DEFINITION** -Berechtigungen für das Zertifikat oder den asymmetrischen Schlüssel verfügbar sein, die bzw. der während der Sicherung zum Verschlüsseln verwendet wurde.  
  
## <a name="options"></a>Tastatur  
  
### <a name="source"></a>`Source`  

Mit diesen Optionen kann der Speicherort der Sicherungssätze für die Datenbank ermittelt werden. Zudem kann bestimmt werden, welche Sicherungssätze wiederhergestellt werden sollen.  
  
|Begriff|Definition|  
|----------|----------------|  
|**Datenbank**|Wählen Sie die wiederherzustellende Datenbank aus der Dropdownliste aus. Die Liste enthält nur Datenbanken, die auf Grundlage des Sicherungsverlaufs von **msdb** gesichert wurden.|  
|**Device**|Wählen Sie die logischen oder physischen Sicherungsmedien (Bänder, URL oder Dateien) aus, die die Sicherung(en) enthalten, die Sie wiederherstellen möchten. Das Medium ist erforderlich, wenn die Datenbanksicherung auf einer anderen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aufgezeichnet wurde.<br /><br /> Sie können eines oder mehrere logische oder physische Sicherungsmedien auswählen, indem Sie auf die Schaltfläche zum Durchsuchen klicken, wodurch das Dialogfeld **Sicherungsmedien auswählen** geöffnet wird. In diesem Dialogfeld können Sie bis zu 64 Medien auswählen, die zu einem einzigen Mediensatz gehören. Bandmedien müssen physisch mit dem Computer verbunden sein, auf dem die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ausgeführt wird. Eine Sicherungsdatei kann sich auf einem lokalen Datenträger oder auf einem Wechseldatenträger befinden. Weitere Informationen finden Sie unter [Sicherungsmedien &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)aufgezeichnet wurde. Sie können auch **URL** als Gerätetyp für Sicherungsdateien auswählen, die in Azure Storage gespeichert sind.<br /><br /> Wenn Sie das Dialogfeld **Sicherungsmedien auswählen** schließen, wird das ausgewählte Medium in Form von schreibgeschützten Werten in der Liste **Sicherungsmedium** angezeigt.|  
|**Datenbank**|Wählen Sie in der Dropdownliste den Namen der Datenbank aus, von der die Sicherungen wiederhergestellt werden sollen.<br /><br /> Hinweis: Diese Liste steht nur zur Verfügung, wenn **Sicherungsmedium** ausgewählt ist. Nur Datenbanken mit Sicherungen auf den ausgewählten Sicherungsmedien stehen zur Verfügung.|  
  
### <a name="destination"></a>Destination  
 Mit den Optionen des Bereichs **Ziel** werden die Datenbank und der Wiederherstellungspunkt identifiziert.  
  
|Begriff|Definition|  
|----------|----------------|  
|**Datenbank**|Geben Sie die wiederherzustellende Datenbank in die Liste ein. Sie können eine neue Datenbank eingeben oder eine vorhandene Datenbank aus der Dropdownliste auswählen. Die Liste umfasst alle Datenbanken auf dem Server, mit Ausnahme der Datenbanken **master** und **tempdb**.<br /><br /> Hinweis: Verwenden Sie die [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) -Anweisung, um eine kennwortgeschützte Sicherung wiederherzustellen.|  
|**Ziel**|Das Feld **Wiederherstellen** wird standardmäßig auf „To the last backup taken“ (Bis zur zuletzt erstellten Sicherung) festgelegt. Sie können auch auf **Zeitachse** klicken, um das Dialogfeld **Sicherungszeitachse** anzuzeigen. Dieses stellt den Datenbanksicherungsverlauf in Form einer Zeitachse dar. Klicken Sie auf **Zeitachse**, um einen bestimmten **datetime**-Wert auszuwählen, für den Sie die Datenbank wiederherstellen möchten. Die Datenbank wird in dem Zustand wiederhergestellt, in dem sie sich zum ausgewählten Zeitpunkt befunden hat. Weitere Informationen finden Sie unter [Sicherungszeitachse](../../relational-databases/backup-restore/backup-timeline.md).|  
  
### <a name="restore-plan"></a>Wiederherstellungsplan  
  
|Begriff|Definition|Werte|  
|----------|----------------|------------|  
|**Wiederherzustellende Sicherungssätze**|Zeigt die verfügbaren Sicherungssätze für den angegebenen Ort an. Jeder Sicherungsvorgang erstellt einen Sicherungssatz, der auf alle Medien des Mediensatzes verteilt wird. Standardmäßig wird ein Wiederherstellungsplan vorgeschlagen, um das Ziel des Wiederherstellungsvorgangs zu erreichen, der auf der Auswahl der erforderlichen Sicherungssätze basiert. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] verwendet den Sicherungsverlauf in **msdb**. Mithilfe des Verlaufs wird festgestellt, welche Sicherungen zum Wiederherstellen einer Datenbank erforderlich sind, und ein Wiederherstellungsplan wird erstellt. Für eine Datenbankwiederherstellung wählt der Wiederherstellungsplan beispielsweise die neueste vollständige Datenbanksicherung und anschließend die neueste differenzielle Datenbanksicherung (falls vorhanden) aus. Bei Verwendung des vollständigen Wiederherstellungsmodells wählt der Wiederherstellungsplan dann alle Protokollsicherungen aus.<br /><br /> Sie können die Auswahl im Raster ändern, um den vorgeschlagenen Wiederherstellungsplan zu überschreiben. Für Sicherungen, die von einer Sicherung abhängig sind, für die die Auswahl aufgehoben wurde, wird die Auswahl automatisch aufgehoben.<br /><br /> Die Kontrollkästchen werden nur aktiviert, wenn das Kontrollkästchen **Manuelle Auswahl** aktiviert ist. Sie können auswählen, welche Sicherungssätze wiederhergestellt werden sollen.<br /><br /> Wenn das Kontrollkästchen **Manuelle Auswahl** aktiviert ist, wird die Genauigkeit des Wiederherstellungsplans bei jeder Änderung überprüft. Wenn die Abfolge der Sicherungen falsch ist, wird eine Fehlermeldung angezeigt.|**Restore** (Wiederherstellen): <br />                          Die aktivierten Kontrollkästchen zeigen die wiederherzustellenden Sicherungssätze an.<br /><br /> **Name**: <br />                          Name des Sicherungssatzes.<br /><br /> **Component:** Die gesicherte Komponente: **Datenbank**, **Datei** oder **\<blank>** (bei Transaktionsprotokollen).<br /><br /> **Typ:** Die Art der Sicherung: **Vollständig**, **Differenziell** oder **Transaktionsprotokoll**.<br /><br /> **Server**: Der Name der [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Instanz, die den Sicherungsvorgang abgeschlossen hat<br /><br /> **Datenbank**: <br />                          Name der an der Sicherungsoperation beteiligten Datenbank.<br /><br /> **Position**: Position des Sicherungssatzes auf dem Volume.<br /><br /> **Erste LSN**: <br />                          Die Protokollfolgenummer der ersten Transaktion im Sicherungssatz. Bei Dateisicherungen leer.<br /><br /> **Letzte LSN**: <br />                          Die Protokollfolgenummer der letzten Transaktion im Sicherungssatz. Bei Dateisicherungen leer.<br /><br /> **Prüfpunkt-LSN**: <br />                          Protokollsequenznummer (LSN) des letzten Prüfpunkts zum Zeitpunkt der Erstellung der Sicherung.<br /><br /> **Vollständige LSN**: <br />                          Die Protokollfolgenummer der neuesten vollständigen Datenbanksicherung.<br /><br /> **Startdatum**: <br />                          Datum und Uhrzeit des Sicherungsbeginns, entsprechend den Ländereinstellungen des Clients.<br /><br /> **Beendigungsdatum**: <br />                          Datum und Uhrzeit des Endes des Sicherungsvorgangs, entsprechend den Ländereinstellungen des Clients.<br /><br /> **Size**: <br />                          Größe des Sicherungssatzes in Byte.<br /><br /> **Benutzername**: <br />                          Der Name des Benutzers, der den Sicherungsvorgang abgeschlossen hat<br /><br /> **Ablaufdatum**: <br />                          Datum und Uhrzeit des Zeitpunkts, an dem der Sicherungssatz verfällt.|  
|**Sicherungsmedien überprüfen**|Ruft eine RESTORE VERIFY_ONLY-Anweisung für die ausgewählten Sicherungssätze auf.<br /><br /> Hinweis: Die Überprüfung ist ein zeitintensiver Vorgang. Mithilfe des Fortschrittsmonitors im Dialogfeld „Framework“ kann der Fortschritt verfolgt und der Vorgang abgebrochen werden.<br /><br /> Mit dieser Schaltfläche können Sie die Integrität der ausgewählten Sicherungsdateien vor der Wiederherstellung überprüfen.<br /><br /> Wenn die Integrität von Sicherungssätzen überprüft wird, lautet der Status links unten im Dialogfeld "Wird überprüft" anstatt "Wird ausgeführt".||  
  
## <a name="compatibility-support"></a>Kompatibilitätsunterstützung  
 In [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]können Sie eine Benutzerdatenbank von einer Datenbanksicherung wiederherstellen, die mit [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] oder einer höheren Version erstellt wurde. Sicherungen von **master**, **model** und **msdb**, die mit [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] bis [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] erstellt wurden, können mit [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] nicht wiederhergestellt werden. Auch in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] erstellte Sicherungen können nicht mit einer früheren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]wiederhergestellt werden.  
  
 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] verwendet im Vergleich zu früheren Versionen einen anderen Standardpfad. Zum Wiederherstellen einer Datenbank, die am Standardspeicherort einer früheren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]erstellt wurde, müssen Sie die MOVE-Option verwenden.  
  
 Nachdem Sie eine Datenbank einer früheren Version in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]wiederhergestellt haben, wird die Datenbank automatisch aktualisiert. In der Regel ist die Datenbank sofort verfügbar. Wenn eine [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] -Datenbank Volltextindizes aufweist, werden diese beim Upgrade entweder importiert, zurückgesetzt oder neu erstellt, je nach der Einstellung der Servereigenschaft **Volltextupgrade-Option** . Wenn die Upgradeoption auf **Importieren** oder **Neu erstellen**festgelegt ist, sind die Volltextindizes während des Upgrades nicht verfügbar. Je nach Menge der indizierten Daten kann der Importvorgang mehrere Stunden dauern, die Neuerstellung sogar bis zu zehnmal länger. Wenn die Upgradeoption auf **Importieren**festgelegt und kein Volltextkatalog verfügbar ist, werden die zugehörigen Volltextindizes neu erstellt.  
  
## <a name="restoring-from-an-encrypted-backup"></a>Wiederherstellen aus einer verschlüsselten Sicherung  
 Für die Wiederherstellung muss das Zertifikat oder der asymmetrische Schlüssel, mit dem die Sicherung ursprünglich erstellt wurde, auf der Instanz verfügbar sein, auf der die Wiederherstellung erfolgen soll. Das Konto, mit dem die Wiederherstellung ausgeführt wird, sollte über **VIEW DEFINITIONS** für das Zertifikat oder den asymmetrischen Schlüssel verfügen. Erneuern oder aktualisieren Sie keine Zertifikate, die zum Verschlüsseln der Sicherung verwendet werden.  
  
## <a name="restoring-from-microsoft-azure-storage"></a>Wiederherstellen von Microsoft Azure Storage  
Wählen Sie **URL** aus der Dropdownliste **Sicherungsmedientyp:** im Dialogfeld **Sicherungsmedien auswählen** aus.  Klicken Sie anschließend auf **Hinzufügen**, um **Speicherort der Sicherungsdatei auswählen** zu öffnen. Wählen Sie vorhandene SQL Server-Anmeldeinformationen und einen Azure-Speichercontainer aus. Fügen Sie einen neuen Azure-Speichercontainer mit einer SAS (Shared Access Signature) hinzu, oder generieren Sie eine SAS und SQL Server-Anmeldeinformationen für einen vorhandenen Speichercontainer. Sobald die Verbindung mit dem Speicherkonto hergestellt wurde, werden die Sicherungsdateien im Dialogfeld **Sicherungsdatei in Microsoft Azure suchen** angezeigt. Dort können Sie die Datei auswählen, die für die Wiederherstellung verwendet werden soll.  Weitere Informationen finden Sie unter [Connect to A Microsoft Azure Subscription](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md)(Herstellen einer Verbindung zu einem Microsoft Azure-Abonnement).
  
## <a name="see-also"></a>Weitere Informationen  
 [Sicherungsmedien &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [Wiederherstellung einer Sicherung von einem Medium &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-backup-from-a-device-sql-server.md)   
 [Wiederherstellen einer Datenbank bis zu einer markierten Transaktion &#40;SQL Server Management Studio&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)   
 [Wiederherstellen einer Transaktionsprotokollsicherung &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)   
 [Anzeigen der Inhalte eines Sicherungsbands oder einer -datei &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)   
 [Anzeigen der Eigenschaften und des Inhalts eines logischen Sicherungsmediums &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)   
 [Mediensätze, Medienfamilien und Sicherungssätze &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)   
 [RESTORE-Argumente &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md)   
 [Anwenden von Transaktionsprotokollsicherungen &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)  
  
  
