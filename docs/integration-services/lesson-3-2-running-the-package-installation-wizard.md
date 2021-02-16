---
description: 'Lektion 3-2: Ausführen des Paketinstallations-Assistenten'
title: 'Schritt 2: Ausführen des Paketinstallations-Assistenten | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: f91fbb89-4626-4c47-b96d-56052dc45861
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 66895007a304ee10878b54f1761b5e72792e1d57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354938"
---
# <a name="lesson-3-2---running-the-package-installation-wizard"></a>Lektion 3-2: Ausführen des Paketinstallations-Assistenten

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


In diesem Schritt führen Sie den Paketinstallations-Assistenten aus, um die Pakete aus dem Deployment Tutorial-Projekt auf einer Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]bereitzustellen. In der sysssispackages-Tabelle der msdb-Datenbank von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] können nur Pakete installiert werden. Die unterstützenden Dateien, die das Bereitstellungspaket enthält, werden im Dateisystem bereitgestellt.  
  
Vom Paketinstallations-Assistenten werden Sie durch die Schritte der Paketinstallation und -konfiguration geführt. Sie installieren die Pakete auf einer Instanz von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auf dem Zielcomputer (der Computer, auf den Sie das Bereitstellungspaket kopiert haben). Weiterhin erstellen Sie den Ordner C:\DeploymentTutorialInstall, in dem der Assistent die Nichtpaketdateien installieren wird.  
  
In einer früheren Lektion haben Sie die Pakete des Lernprogramms so geändert, dass sie Konfigurationen verwenden. Sie bearbeiten diese Konfigurationen mithilfe des Paketinstallations-Assistenten, sodass Pakete erfolgreich in der Installationszielumgebung ausgeführt werden können.  
  
### <a name="to-install-the-packages"></a>So installieren Sie die Pakete  
  
1.  Suchen Sie das Bereitstellungspaket auf dem Zielcomputer.  
  
    Wenn Sie den Standardwert, bin\Deployment, als Speicherort für das Bereitstellungshilfsprogramm verwendet haben, befindet sich das Bereitstellungspaket im Ordner „Deployment“ des Deployment Tutorial-Projekts.  
  
2.  Doppelklicken Sie im Ordner Deployment auf die Manifestdatei Deployment Tutorial.SSISDeploymentManifest.  
  
3.  Klicken Sie auf der Seite Willkommen des Paketinstallations-Assistenten auf **Weiter**.  
  
4.  Wählen Sie auf der Seite SSIS-Pakete bereitstellen die Option **Bereitstellung in SQL Server** aus, aktivieren Sie das Kontrollkästchen **Pakete nach der Installation überprüfen** , und klicken Sie dann auf **Weiter**.  
  
5.  Geben Sie auf der Seite Zielserver mit SQL Server angeben **(local)** im Feld **Servername** an.  
  
6.  Wenn die SQL Server-Instanz die Windows-Authentifizierung unterstützt, wählen Sie **Windows-Authentifizierung verwenden** aus. Andernfalls wählen Sie **SQL Server-Authentifizierung verwenden** aus und geben einen Benutzernamen und ein Kennwort an.  
  
7.  Überprüfen Sie, ob das Kontrollkästchen **Serverspeicher für die Verschlüsselung verwenden** deaktiviert ist.  
  
8.  Klicken Sie auf **Weiter**.  
  
9. Klicken Sie auf der Seite Installationsordner auswählen auf **Durchsuchen**.  
  
10. Erweitern Sie im Dialogfeld **Nach Ordner suchen** die Option **Arbeitsplatz** , und klicken Sie anschließend auf **Lokaler Datenträger (C:)**.  
  
11. Klicken Sie auf **Neuen Ordner erstellen** , und ersetzen Sie den Standardnamen des neuen Ordners, **Neuer Ordner**, durch **DeploymentTutorialInstall**.  
  
    > [!IMPORTANT]  
    > Auf diesen Namen wird im Wert der Umgebungsvariablen verwiesen, die in Konfigurationen verwendet werden. Der Name des Ordners und der Verweis müssen übereinstimmen. Andernfalls kann das Paket nicht ausgeführt werden.  
  
12. Klicken Sie auf **OK**.  
  
13. Überprüfen Sie auf der Seite Installationsordner auswählen, ob das Feld Ordner den Eintrag **C:\DeploymentTutorialInstall** enthält, und klicken Sie anschließend auf **Weiter**.  
  
14. Klicken Sie auf der Seite Installation bestätigen auf **Weiter**.  
  
    Die Pakete werden vom Assistenten installiert. Nachdem die Installation abgeschlossen ist, wird die Seite Pakete konfigurieren geöffnet.  
  
15. Überprüfen Sie auf der Seite Pakete konfigurieren, ob im Feld **Konfigurationsdatei** die Dateien datatransferconfig.dtsconfig und loadxmldataconfig.dtsconfig aufgelistet sind.  
  
16. Klicken Sie in der Liste **Konfigurationsdatei** auf **datatransferconfig.dtsconfig**, erweitern Sie Eigenschaft in der **Pfad** -Spalte des Felds **Konfigurationen** , und aktualisieren Sie die **Wert** -Spalte mit den folgenden Werten:  
  
    |Eigenschaft|Wert|Aktualisierter Wert|  
    |------------|---------|-----------------|  
    |\Package.Connections[Deployment Tutorial Log].Properties[ConnectionString]|C:\Programme\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Completed Packages\Deployment Tutorial Log|C:\DeploymentTutorialInstall\Deployment Tutorial Log|  
    |\Package.Connections[NewCustomers].Properties[ConnectionString]|C:\Programme\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\NewCustomers.txt|C:\DeploymentTutorialInstall\NewCustomers.txt|  
  
17. Klicken Sie in der Liste **Konfigurationsdatei** auf loadxmldataconfig.dtsconfig, erweitern Sie Eigenschaft in der **Pfad** -Spalte des Felds **Konfigurationen** , und aktualisieren Sie die **Wert** -Spalte mit den folgenden Werten:  
  
    |Eigenschaft|Wert|Aktualisierter Wert|  
    |------------|---------|-----------------|  
    |\Package.LoadXMLData.Properties[[XML Source].[XMLData]]|C:\Programme\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\orders.xml|C:\DeploymentTutorialInstall\orders.xml|  
    |\Package.LoadXMLData.Properties[[XML Source].[XMLSchemaDefinition]]|C:\Programme\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\orders.xsd|C:\DeploymentTutorialInstall\orders.xsd|  
  
18. Auf der Seite zur Paketüberprüfung werden die Überprüfungsergebnisse jedes installierten Pakets angezeigt. Klicken Sie auf **Weiter**.  
  
    Da die Werte der Umgebungsvariablen auf dem Zielcomputer von den Werten der Umgebungsvariablen auf dem Entwicklungscomputer abweichen, werden auf der Seite zur Paketüberprüfung mehrere Warnungen angezeigt. Vier Warnungen sollten angezeigt werden:  
  
    -   Der Konfigurationsdateiname "C:\DeploymentTutorial\DataTransferConfig.dtsConfig" ist ungültig. Überprüfen Sie den Konfigurationsdateinamen.  
  
    -   Fehler beim Laden von mindestens einem Konfigurationseintrag für das Paket. Überprüfen Sie die Konfigurationseinträge und vorherige Warnungen, um festzustellen, bei welcher Konfiguration der Fehler aufgetreten ist.  
  
    -   Der Konfigurationsdateiname "C:\DeploymentTutorial\LoadXMLDataConfig.dtsConfig" ist ungültig. Überprüfen Sie den Konfigurationsdateinamen.  
  
    -   Fehler beim Laden von mindestens einem Konfigurationseintrag für das Paket. Überprüfen Sie die Konfigurationseinträge und vorherige Warnungen, um festzustellen, bei welcher Konfiguration der Fehler aufgetreten ist.  
  
    Diese Warnungen haben keinen Einfluss auf die Paketinstallation.  
  
    Wenn Sie die Option **Pakete nach der Installation überprüfen** auf der Seite SSIS-Pakete bereitstellen nicht ausgewählt haben, werden die Seiten für die Paketüberprüfung nicht geöffnet, und im Assistenten werden nach der Installation keine Informationen zur Überprüfung angezeigt.  
  
19. Lesen Sie die Installationszusammenfassung auf der Seite Fertigstellen des Assistenten, und klicken Sie dann auf **Fertig stellen**.  
  
    > [!NOTE]  
    > Es wird eine temporäre Protokolldatei erstellt, die für die Paketüberprüfung verwendet wird. Diese Datei wird bei der Ausführung des Pakets nicht verwendet.  
  
## <a name="next-task-in-lesson"></a>Nächste Aufgabe in der Lektion  
[Schritt 3: Testen der bereitgestellten Pakete](../integration-services/lesson-3-3-testing-the-deployed-packages.md)  
  
## <a name="see-also"></a>Weitere Informationen  
[Integration Services-Dienst &#40;SSIS-Dienst&#41;](../integration-services/service/integration-services-service-ssis-service.md)  
