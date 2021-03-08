---
title: Hinzufügen von Funktionen zu einer Instanz von SQL Server (Setup)
description: Dieser Artikel bietet eine ausführliche Anleitung zum Hinzufügen instanzabhängiger Features zu einer Instanz von SQL Server 2019.
ms.prod: sql
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- feature adding [SQL Server]
- " SQL Server, features"
- adding features to  SQL Server
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.custom: ''
ms.date: 02/05/2021
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: b8b520e3c5f44342970881e3e67b3dbf95ce9853
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837572"
---
# <a name="add-features-to-an-instance-of-sql-server-setup"></a>Hinzufügen von Funktionen zu einer Instanz von SQL Server (Setup)

[!INCLUDE [ SQL Server - Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Dieser Artikel bietet eine Schrittanleitung zum Hinzufügen von Funktionen zu einer Instanz von [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Einige Komponenten oder Dienste von SQL Server sind spezifisch für eine SQL Server-Instanz. Sie werden auch als instanzabhängig bezeichnet. Sie nutzen die gleiche Version wie ihre Hostinstanz und werden ausschließlich für diese Instanz verwendet. Sie können einer SQL Server-Instanz die instanzabhängigen Komponenten zusammen mit den gemeinsam genutzten Komponenten hinzufügen, wenn sie nicht bereits installiert sind. Eine Liste der Features, die von den verschiedenen SQL Server-Editionen unterstützt werden, finden Sie unter [Editionen und unterstütze Features für SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md) oder [SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md).

Informationen zum Hinzufügen von Funktionen zu einer Instanz von SQL Server über die Eingabeaufforderung finden Sie unter [Installieren von SQL Server über die Eingabeaufforderung](./install-sql-server-from-the-command-prompt.md).

> [!CAUTION]
> Durch Hinzufügen von Features zu einer vorhandenen Installation von SQL Server werden die Features auf der Versionsebene des Installationsmediums installiert, die hinter der Versionsebene anderer Features von SQL Server liegen kann. Dies kann zu unerwartetem Verhalten oder Fehlern führen. Stellen Sie stets im Anschluss an das erfolgreiche Setup von SQL Server das neue Feature auf die gleiche Versionsebene um. Installieren Sie bei Bedarf Service Packs (SPs), kumulative Updates (CUs) und/oder allgemeine Vertriebsversionen (General Distribution Releases, GDRs). Informationen zum Ermitteln der Version von Features, die einer Installation von SQL Server hinzugefügt wurden, finden Sie unter [Bestimmen der Version, Edition und Updateebene von SQL Server und zugehöriger Komponenten](https://docs.microsoft.com/troubleshoot/sql/general/determine-version-edition-update-level).

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie den Vorgang fortsetzen, lesen Sie die Artikel unter [Planen einer SQL Server-Installation](../../sql-server/install/planning-a-sql-server-installation.md).

> [!NOTE]
> Bei lokalen Installationen müssen Sie das Setup als Administrator ausführen. Wenn Sie SQL Server über eine Remotefreigabe installieren, müssen Sie ein Domänenkonto verwenden, das Lese- und Ausführungsberechtigungen für diese hat.  
  
> [!NOTE]
> Wenn Sie einer Instanz von [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]Funktionen hinzufügen, werden die vorhandenen Einstellungen für Verwendungsberichte auf die neu hinzugefügten Funktionen angewendet. Um diese Einstellungen zu ändern, verwenden Sie im SQL Server-Menü **Konfigurationstools** das Tool **Fehler- und Verwendungsberichterstellung von SQL Server**.

## <a name="procedures"></a>Prozeduren

#### <a name="to-add-features-to-an-instance-of-sscurrent"></a>Informationen zum Hinzufügen einer Instanz von [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]

1. Legen Sie das SQL Server-Installationsmedium ein. Doppelklicken Sie im Stammordner auf setup.exe. Bei einer Installation über eine Netzwerkfreigabe navigieren Sie zum Stammordner der Freigabe, und doppelklicken Sie auf setup.exe. Wenn das Dialogfeld zum Einrichten von [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] angezeigt wird, klicken Sie auf **OK**, um die erforderlichen Komponenten zu installieren. Wählen Sie dann **Abbrechen**, um die Installation von [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] abzubrechen.  

2. Das SQL Server-Installationscenter wird vom Installations-Assistenten gestartet. Um einer vorhandenen Instanz von [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] eine neue Funktion hinzuzufügen, klicken Sie im linken Navigationsbereich auf **Installation** und anschließend auf **Neue eigenständige SQL Server-Installation oder Hinzufügen von Funktionen zu einer vorhandenen Installation**.

3. Die Systemkonfigurationsprüfung führt einen Ermittlungsvorgang auf dem Computer aus. Wählen Sie **Details anzeigen** aus, um die Überprüfungsdetails anzuzeigen. Wählen Sie **OK** aus, um fortzufahren.

4. Auf der Seite „Produktupdates“ werden die neuesten verfügbaren SQL Server-Produktupdates angezeigt. Wenn Sie die Updates nicht einschließen möchten, deaktivieren Sie das Kontrollkästchen **SQL Server-Produktupdates einschließen**. Wenn keine Produktupdates ermittelt wurden, zeigt SQL Server-Setup diese Seite nicht an und geht automatisch zur Seite **Setupdateien installieren** über.

5. Auf der Seite Setupdateien installieren wird der Status angezeigt, während die Setupdateien heruntergeladen, extrahiert und installiert werden. Wenn ein Update für SQL Server-Setup gefunden und angegeben wird, dass das Update eingeschlossen werden soll, wird dieses Update ebenfalls installiert. Wählen Sie **Installieren** aus, um die Unterstützungsdateien für Setup zu installieren.  

6. Die Systemkonfigurationsprüfung überprüft den Systemstatus des Computers, bevor Setup fortgesetzt wird.  

7. Wählen Sie auf der Seite „Installationstyp“ die Option **Funktionen zu einer vorhandenen [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]-Instanz hinzufügen**, und wählen Sie dann die Instanz aus, die Sie aktualisieren möchten.

8. Wählen Sie auf der Seite Funktionsauswahl die Komponenten für die Installation aus. Nach Auswahl des Funktionsnamens wird im rechten Bereich eine Beschreibung für die einzelnen Komponentengruppen angezeigt. Sie können jede beliebige Kombination von Kontrollkästchen aktivieren. Weitere Informationen finden Sie unter [Editionen und unterstützte Features von SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md) und [SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md). Jede Komponente kann in einer bestimmten Instanz von SQL Server nur einmal installiert werden. Wenn Sie mehrere Komponenten installieren möchten, müssen Sie eine zusätzliche Instanz von SQL Server installieren.

    Die erforderlichen Komponenten für die ausgewählten Funktionen werden im rechten Bereich angezeigt. SQL Server-Setup installiert die erforderlichen Komponenten, die nicht bereits während des im weiteren Verlauf dieser Prozedur beschriebenen Installationsschritts installiert werden.

    Die Systemkonfigurationsprüfung überprüft den Systemstatus des Computers, bevor Setup fortgesetzt wird. Wählen Sie **Weiter** aus, um den Vorgang fortzusetzen.

9. Auf der Seite Erforderlicher Speicherplatz wird der für die angegebenen Funktionen erforderliche Speicherplatz berechnet, und die Anforderungen werden mit dem Speicherplatz verglichen, der auf dem Computer verfügbar ist, auf dem Setup ausgeführt wird.

10. Der Ablauf für die weiteren Vorgänge in diesem Artikel richtet sich nach den Funktionen, die Sie für Ihre Installation angegeben haben. Je nach Auswahl werden möglicherweise nicht alle Seiten angezeigt.

11. Geben Sie auf der Seite „Serverkonfiguration > Dienstkonten“ Anmeldekonten für SQL Server-Dienste an. Welche Dienste tatsächlich auf dieser Seite konfiguriert werden, hängt von den Funktionen ab, die Sie für die Installation ausgewählt haben.

    Sie können allen SQL Server-Diensten dasselbe Anmeldekonto zuweisen oder jedes Dienstkonto einzeln konfigurieren. Außerdem können Sie angeben, ob Dienste automatisch starten sollen, manuell gestartet werden oder deaktiviert sind. Microsoft empfiehlt, Dienstkonten einzeln zu konfigurieren, um möglichst geringe Rechte für jeden Dienst zu gewähren. Dabei erhalten SQL Server-Dienste die Berechtigungen, die mindestens erforderlich sind, um ihre Tasks auszuführen. Weitere Informationen finden Sie unter [Serverkonfiguration – Dienstkonten](./install-sql-server.md) und [Konfigurieren von Windows-Dienstkonten und -Berechtigungen](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).

    Um für alle Dienstkonten in dieser Instanz von SQL Server dasselbe Anmeldekonto anzugeben, geben Sie im Feld unten auf dieser Seite die entsprechenden Anmeldeinformationen ein.

    **Sicherheitshinweis** : [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]

    Wenn Sie die Angabe der Anmeldeinformationen für SQL Server-Dienste abgeschlossen haben, wählen Sie **Weiter** aus.

12. Geben Sie auf der Registerkarte **Server Konfiguration – Sortierung** nicht standardmäßige Sortierungen für die Datenbank-Engine und Analysis Services an. Weitere Informationen finden Sie unter [Serverkonfiguration – Sortierung](./install-sql-server.md).

13. Geben Sie auf der Seite Datenbank-Engine-Konfiguration - Kontobereitstellung folgende Informationen an  

    - Sicherheitsmodus: Wählen Sie für die Instanz von SQL Server die Windows-Authentifizierung oder die Authentifizierung im gemischten Modus aus. Bei Auswahl der Authentifizierung im gemischten Modus müssen Sie ein sicheres Kennwort für das integrierte SQL Server-Systemadministratorkonto (sa) angeben.

        Nachdem ein Gerät erfolgreich eine Verbindung mit SQL Server hergestellt hat, wird für die Windows-Authentifizierung und den gemischten Modus derselbe Sicherheitsmechanismus verwendet. Weitere Informationen finden Sie unter [Konfiguration der Datenbank-Engine – Serverkonfiguration](./install-sql-server.md).  

    - SQL Server-Administratoren: Sie müssen mindestens einen Systemadministrator für die Instanz von SQL Server angeben. Um das Konto hinzuzufügen, unter dem das SQL Server-Setup ausgeführt wird, klicken Sie auf **Aktuellen Benutzer hinzufügen**. Um Konten zur Liste der Systemadministratoren hinzuzufügen bzw. daraus zu entfernen, klicken Sie auf **Hinzufügen** bzw. **Entfernen**. Bearbeiten Sie anschließend die Liste der Benutzer, Gruppen bzw. Computer, die Administratorrechte für die SQL Server-Instanz haben sollen. Weitere Informationen finden Sie unter [Konfiguration der Datenbank-Engine – Serverkonfiguration](./install-sql-server.md).

    Wenn Sie die Bearbeitung der Liste abgeschlossen haben, wählen Sie **OK** aus. Überprüfen Sie die Liste der Administratoren im Konfigurationsdialogfeld. Wenn die Liste vollständig ist, wählen Sie **Weiter** aus.

14. Geben Sie auf der Seite „Konfiguration der Datenbank-Engine – Datenverzeichnisse“ andere Installationsverzeichnisse als das Standardinstallationsverzeichnis an. Wenn die Installation in Standardverzeichnissen erfolgen soll, wählen Sie **Weiter** aus.  

    > [!IMPORTANT]
    > Wenn Sie keine Standardverzeichnisse angeben, sollten Sie sicherstellen, dass die Installationsordner nur für die SQL Server-Instanz verwendet werden. Keines der Verzeichnisse in diesem Dialogfeld sollte gemeinsam mit Verzeichnissen anderer Instanzen von SQL Server genutzt werden.

    Weitere Informationen finden Sie unter [Konfiguration der Datenbank-Engine – Serverkonfiguration](./install-sql-server.md).

15. Aktivieren Sie auf der Seite „Konfiguration der Datenbank-Engine – FILESTREAM“ FILESTREAM für Ihre Instanz von SQL Server. Weitere Informationen zu FILESTREAM finden Sie unter [Konfiguration der Datenbank-Engine - Filestream](./install-sql-server.md). Wählen Sie zum Fortsetzen des Vorgangs Weiter aus.

16. Geben Sie auf der Seite „Analysis Services-Konfiguration – Kontenbereitstellung“ den Servermodus und die Benutzer oder Konten an, die über Administratorberechtigungen für Analysis Services verfügen sollen. Durch den Servermodus wird bestimmt, welcher Arbeitsspeicher und welche Speichersubsysteme auf dem Server verwendet werden. Die unterschiedlichen Projektmappentypen werden in verschiedenen Servermodi ausgeführt. Wenn Sie beabsichtigen, mehrdimensionale Cubedatenbanken auf dem Server auszuführen, wählen Sie die Standardoption, den mehrdimensionalen und Data Mining-Servermodus, aus. Damit Administratorberechtigungen vorhanden sind, müssen Sie mindestens einen Systemadministrator für Analysis Services angeben. Um das Konto hinzuzufügen, unter dem das SQL Server-Setup ausgeführt wird, klicken Sie auf **Aktuellen Benutzer hinzufügen**. Um Konten zur Liste der Systemadministratoren hinzuzufügen bzw. daraus zu entfernen, klicken Sie auf **Hinzufügen** bzw. **Entfernen**. Bearbeiten Sie anschließend die Liste der Benutzer, Gruppen bzw. Computer, die Administratorrechte für Analysis Services haben sollen. Weitere Informationen zu Servermodus und Administratorberechtigungen finden Sie unter [Analysis Services-Konfiguration – Kontobereitstellung](./install-sql-server.md).

    Wenn Sie die Bearbeitung der Liste abgeschlossen haben, wählen Sie **OK** aus. Überprüfen Sie die Liste der Administratoren im Konfigurationsdialogfeld. Wenn die Liste vollständig ist, wählen Sie **Weiter** aus.

17. Geben Sie auf der Seite „Analysis Services-Konfiguration – Datenverzeichnisse“ andere Installationsverzeichnisse als das Standardinstallationsverzeichnis an. Wenn die Installation in Standardverzeichnissen erfolgen soll, wählen Sie **Weiter** aus.

    > [!IMPORTANT]
    > Wenn Sie keine Standardverzeichnisse angeben, sollten Sie sicherstellen, dass die Installationsordner nur für die SQL Server-Instanz verwendet werden. Keines der Verzeichnisse in diesem Dialogfeld sollte gemeinsam mit Verzeichnissen anderer Instanzen von SQL Server genutzt werden.

    Weitere Informationen finden Sie unter [Konfigurationseigenschaften von Analysis Services – Datenverzeichnisse](./install-sql-server.md).

18. Geben Sie auf der Seite „Reporting Services-Konfiguration“ den Typ der zu erstellenden Reporting Services-Konfiguration an. Weitere Informationen zu Konfigurationsmodi für Reporting Services finden Sie unter [Reporting Services-Konfigurationsoptionen &#40;SSRS&#41;](./install-sql-server.md).

19. Verwenden Sie die Seite für die Distributed Replay Controller-Konfiguration, um die Benutzer anzugeben, denen Sie Administratorberechtigungen für den Distributed Replay Controller-Dienst erteilen möchten. Benutzer, die über Administratorberechtigungen verfügen, besitzen unbeschränkten Zugriff auf den Distributed Replay Controller-Dienst.

    Wählen Sie die Schaltfläche **Aktuellen Benutzer hinzufügen** aus, um die Benutzer hinzuzufügen, denen Sie Zugriffsberechtigungen für den Dienst Distributed Replay Controller erteilen möchten. Wählen Sie die Schaltfläche **Hinzufügen** aus, um Zugriffsberechtigungen für den Dienst Distributed Replay Controller hinzuzufügen. Wählen Sie die Schaltfläche **Entfernen** aus, um Zugriffsberechtigungen für den Dienst Distributed Replay Controller zu entfernen.

    Wählen Sie zum Fortsetzen des Vorgangs **Weiter** aus.

20. Verwenden Sie die Seite für die Distributed Replay Client-Konfiguration, um die Benutzer anzugeben, denen Sie Administratorberechtigungen für den Distributed Replay Client-Dienst erteilen möchten. Benutzer, die über Administratorberechtigungen verfügen, besitzen unbeschränkten Zugriff auf den Distributed Replay Client-Dienst.

    **Controllername** ist ein optionaler Parameter, und der Standardwert ist \<*blank*>. Geben Sie den Namen des Controllers ein, mit dem der Clientcomputer für den Distributed Replay Client-Dienst kommuniziert. Beachten Sie Folgendes:

    - Wenn Sie bereits einen Controller eingerichtet haben, geben Sie den Namen des Controllers beim Konfigurieren jedes Clients ein.

    - Wenn Sie noch keinen Controller eingerichtet haben, können Sie das Feld für den Controllernamen leer lassen. Sie müssen den Controllernamen jedoch in der **Clientkonfigurationsdatei** manuell eingeben.

    Geben Sie das **Arbeitsverzeichnis** für den Distributed Replay Client-Dienst an. Das Standardarbeitsverzeichnis ist \<*drive letter*>:\Program Files\Microsoft\SQL Server\DReplayClient\WorkingDir.

    Geben Sie das **Ergebnisverzeichnis** für den Distributed Replay Client-Dienst an. Das Standardergebnisverzeichnis ist \<*drive letter*>:\Program Files\Microsoft\SQL Server\DReplayClient\ResultDir.

    Wählen Sie zum Fortsetzen des Vorgangs **Weiter** aus.

21. Geben Sie auf der Seite „Fehlerberichterstellung“ die Informationen an, die Sie an Microsoft senden möchten, um zur Verbesserung von SQL Server beizutragen. Die Option für Fehlerberichte ist standardmäßig aktiviert.

22. Die Systemkonfigurationsprüfung führt einen weiteren Regelsatz aus, um die Konfiguration des Computers anhand der von Ihnen angegebenen SQL Server-Funktionen zu überprüfen.  

23. Auf der Seite Installationsbereit wird eine Strukturansicht der Installationsoptionen angezeigt, die während des Setups angegeben wurden. Auf dieser Setupseite ist neben der letzten Updateversion auch angegeben, ob die Produktupdatefunktion aktiviert oder deaktiviert ist. Nachdem Sie „Installieren“ ausgewählt haben, werden von SQL Server-Setup zunächst die erforderlichen Komponenten für die ausgewählten Funktionen und anschließend die Funktionen installiert.  

24. Während der Installation wird auf der Seite Installationsstatus der Status angegeben, sodass Sie während der Installation den Installationsstatus überwachen können.  

25. Nach der Installation bietet die Seite Abgeschlossen einen Link zur zusammenfassenden Protokolldatei für die Installation und andere wichtige Hinweise. Klicken Sie auf **Schließen**, um die Installation von SQL Server abzuschließen.

26. Starten Sie den Computer neu, falls Sie dazu aufgefordert werden. Nachdem das Setup abgeschlossen ist, sollten Sie unbedingt die vom Installations-Assistenten ausgegebene Meldung lesen. Weitere Informationen finden Sie unter [Lesen und Anzeigen der Setupprotokolldateien von SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

## <a name="next-steps"></a>Nächste Schritte

Konfigurieren der SQL Server-Installation

- Zum Reduzieren der Angriffsfläche eines Systems installiert und aktiviert SQL Server selektiv die entsprechenden Dienste und Funktionen. Weitere Informationen finden Sie unter [Oberflächenkonfiguration](../../relational-databases/security/surface-area-configuration.md).

## <a name="see-also"></a>Weitere Informationen
- [Lesen und Anzeigen der Setupprotokolldateien von SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
- [Überprüfen einer SQL Server-Installation](../../database-engine/install-windows/validate-a-sql-server-installation.md)
- [Reparieren von Fehlern bei einer SQL Server 2016-Installation](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)
- [Upgrade SQL Server Using the Installation Wizard (Setup) (Upgrade von SQL Server mithilfe des Installations-Assistenten (Setup))](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)
- [Installieren von SQL Server von der Eingabeaufforderung](./install-sql-server-from-the-command-prompt.md)
- [Neueste Updates für SQL Server](latest-updates-for-microsoft-sql-server.md)