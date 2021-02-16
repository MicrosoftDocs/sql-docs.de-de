---
description: 'Lektion 1-4: Hinzufügen von Paketkonfigurationen'
title: 'Schritt 4: Hinzufügen von Paketkonfigurationen | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: e04a5321-63d5-4ec5-85b9-cb4eaf6c87f6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ca2d3bb08a1059970b8c72da5541b2ad3d9d1c8d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354333"
---
# <a name="lesson-1-4---adding-package-configurations"></a>Lektion 1-4: Hinzufügen von Paketkonfigurationen

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


In diesem Schritt fügen Sie jedem Paket eine Konfiguration hinzu. Konfigurationen aktualisieren die Werte von Paketeigenschaften und Paketobjekten zur Laufzeit.  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] stellt eine Vielzahl von Konfigurationstypen bereit. Sie können Konfigurationen in Umgebungsvariablen, Registrierungseinträgen, benutzerdefinierten Variablen, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] -Tabellen und XML-Dateien speichern. [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] unterstützt die Verwendung von indirekten Konfigurationen und bietet damit zusätzliche Flexibilität. Das bedeutet, dass Sie eine Umgebungsvariable verwenden, um den Speicherort der Konfiguration anzugeben, die wiederum die eigentlichen Werte angibt. Bei den Paketen im Deployment Tutorial-Projekt wird eine Kombination aus XML-Konfigurationsdateien und indirekten Konfigurationen verwendet. Eine XML-Konfigurationsdatei kann Konfigurationen für mehrere Eigenschaften enthalten, und gegebenenfalls können mehrere Pakete darauf verweisen. In diesem Lernprogramm verwenden Sie eine separate Konfigurationsdatei für jedes Paket.  
  
Konfigurationsdateien enthalten oft vertrauliche Informationen wie Verbindungszeichenfolgen. Deshalb sollten Sie eine Zugriffssteuerungsliste (Access Control List, ACL) verwenden, um den Zugriff auf den Speicherort oder Ordner zu beschränken, in dem Sie die Dateien speichern, und nur den Benutzern oder Konten Zugriff erteilen, die zum Ausführen der Pakete berechtigt sind. Weitere Informationen finden Sie unter [Zugriff auf Dateien, die von Paketen verwendet werden](../integration-services/security/security-overview-integration-services.md#files).  
  
Die Pakete (DataTransfer und LoadXMLData), die Sie dem Deployment Tutorial-Projekt in der vorherigen Aufgabe hinzugefügt haben, benötigen Konfigurationen, damit sie nach der Bereitstellung auf dem Zielserver erfolgreich ausgeführt werden können. Zum Implementieren von Konfigurationen erstellen Sie zuerst die indirekten Konfigurationen für die XML-Konfigurationsdateien und dann die XML-Konfigurationsdateien.  
  
Sie erstellen die beiden Konfigurationsdateien DataTransferConfig.dtsConfig und LoadXMLData.dtsConfig. Diese Dateien enthalten Name/Wert-Paare zur Aktualisierung der Eigenschaften von Paketen, die den vom Paket verwendeten Speicherort der Daten und Protokolldateien angeben. Später aktualisieren Sie im Rahmen des Bereitstellungsprozesses die Werte in den Konfigurationsdateien, um den neuen Speicherort der Dateien auf dem Zielcomputer zu berücksichtigen.  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] erkennt, dass DataTransferConfig.dtsConfig und LoadXMLData.dtsConfig Abhängigkeiten der DataTransfer- und LoadXMLData-Pakete darstellen und fügt automatisch die Konfigurationsdateien ein, wenn Sie in der folgenden Lektion das Bereitstellungspaket erstellen.  
  
### <a name="to-create-indirect-configuration-for-the-datatransfer-package"></a>So erstellen Sie eine indirekte Konfiguration für das DataTransfer-Paket  

Überprüfen Sie das aktuelle Bereitstellungsmodell des Projekts, und legen Sie es bei Bedarf auf **Paketbereitstellungsmodell** fest. Klicken Sie im Menü **Projekt** auf **In Paketbereitstellungsmodell konvertieren**.
  
1.  Doppelklicken Sie im Projektmappen-Explorer auf DataTransfer.dtsx.  
  
2.  Klicken Sie im [!INCLUDE[ssIS](../includes/ssis-md.md)] -Designer auf eine beliebige Stelle im Vordergrund der Entwurfsoberfläche der Ablaufsteuerung.  
  
3.  Klicken Sie im Menü **SSIS** auf **Paketkonfigurationen**.  
  
4.  Wählen Sie im Dialogfeld **Paketkonfigurationsplaner** gegebenenfalls die Option **Paketkonfigurationen aktivieren** aus, und klicken Sie anschließend auf **Hinzufügen**.  
  
5.  Klicken Sie auf der Willkommensseite des Paketkonfigurations-Assistenten auf **Weiter**.  
  
6.  Wählen Sie auf der Seite Konfigurationstyp auswählen die Option **XML-Konfigurationsdatei** in der Liste **Konfigurationstyp** aus. Wählen Sie die Option **Konfigurationsspeicherort ist in einer Umgebungsvariablen gespeichert** aus, und geben Sie **DataTransfer** ein, oder wählen Sie die **DataTransfer** -Umgebungsvariable in der Liste aus.  
  
    > [!NOTE]  
    > Sie müssen möglicherweise Ihren Computer nach Hinzufügen der Variablen neu starten, um die Umgebungsvariable in der Liste verfügbar zu machen. Wenn Sie den Computer nicht neu starten möchten, können Sie den Namen der Umgebungsvariablen eingeben.  
  
7.  Klicken Sie auf **Weiter**.  
  
8.  Geben Sie auf der Seite „Assistenten abschließen“ im Feld **Konfigurationsname****DataTransfer EV Configuration** ein, prüfen Sie den Inhalt der Konfiguration im Bereich **Vorschau** , und klicken Sie anschließend auf **Fertig stellen**.  
  
9. Schließen Sie das Dialogfeld **Paketkonfigurationsplaner**.  
  
### <a name="to-create-the-xml-configuration-for-the-datatransfer-package"></a>So erstellen Sie die XML-Konfiguration für das DataTransfer-Paket  
  
1.  Doppelklicken Sie im Projektmappen-Explorer auf DataTransfer.dtsx.  
  
2.  Klicken Sie im [!INCLUDE[ssIS](../includes/ssis-md.md)] -Designer auf eine beliebige Stelle im Vordergrund der Entwurfsoberfläche der Ablaufsteuerung.  
  
3.  Klicken Sie im Menü **SSIS** auf **Paketkonfigurationen**.  
  
4.  Aktivieren Sie im Dialogfeld Paketkonfigurationsplaner das Kontrollkästchen **Paketkonfigurationen aktivieren** , und klicken Sie anschließend auf **Hinzufügen**.  
  
5.  Klicken Sie auf der Willkommensseite des Paketkonfigurations-Assistenten auf **Weiter**.  
  
6.  Wählen Sie auf der Seite Konfigurationstyp auswählen die Option **XML-Konfigurationsdatei** in der Liste **Konfigurationstyp** aus, und klicken Sie anschließend auf **Durchsuchen**.  
  
7.  Navigieren Sie im Dialogfeld **Speicherort der Konfigurationsdatei auswählen** zu „C:\DeploymentTutorial“, geben Sie im Feld **Dateiname****DataTransferConfig** ein, und klicken Sie anschließend auf **Speichern**.  
  
8.  Klicken Sie auf der Seite „Konfigurationstyp auswählen“ auf **Weiter**.  
  
9. Erweitern Sie auf der Seite Eigenschaften für den Exportvorgang auswählen DataTransfer, Connection Managers, Deployment Tutorial Log und Properties, und aktivieren Sie anschließend das Kontrollkästchen **Verbindungszeichenfolge** .  
  
10. Erweitern Sie innerhalb von Connection Managers die Option NewCustomers, und aktivieren Sie anschließend das Kontrollkästchen **Verbindungszeichenfolge** .  
  
11. Klicken Sie auf **Weiter**.  
  
12. Geben Sie auf der Seite „Assistenten abschließen“ im Feld **Konfigurationsname****DataTransfer Configuration** ein, überprüfen Sie den Inhalt der Konfiguration, und klicken Sie anschließend auf **Fertig stellen**.  
  
13. Überprüfen Sie im Dialogfeld **Paketkonfigurationsplaner** , ob DataTransfer EV Configuration als Erstes und DataTransfer Configuration als Zweites aufgeführt wird, und klicken Sie anschließend auf **Schließen**.  
  
### <a name="to-create-indirect-configuration-for-the-loadxmldata-package"></a>So erstellen Sie eine indirekte Konfiguration für das LoadXMLData-Paket  
  
1.  Doppelklicken Sie im Projektmappen-Explorer auf LoadXMLData.dtsx.  
  
2.  Klicken Sie im [!INCLUDE[ssIS](../includes/ssis-md.md)] -Designer auf eine beliebige Stelle im Vordergrund der Entwurfsoberfläche der Ablaufsteuerung.  
  
3.  Klicken Sie im Menü **SSIS** auf **Paketkonfigurationen**.  
  
4.  Klicken Sie im Dialogfeld **Paketkonfigurationsplaner** auf **Hinzufügen**.  
  
5.  Klicken Sie auf der Willkommensseite des Paketkonfigurations-Assistenten auf **Weiter**.  
  
6.  Wählen Sie auf der Seite Konfigurationstyp auswählen die Option **XML-Konfigurationsdatei** in der Liste **Konfigurationstyp** aus. Wählen Sie die Option **Konfigurationsspeicherort ist in einer Umgebungsvariablen gespeichert** aus, und geben Sie **LoadXMLData** ein, oder wählen Sie die **LoadXMLData** -Umgebungsvariable in der Liste aus.  
  
    > [!NOTE]  
    > Sie müssen möglicherweise Ihren Computer nach Hinzufügen der Variablen neu starten, um die Umgebungsvariable in der Liste verfügbar zu machen.  
  
7.  Klicken Sie auf **Weiter**.  
  
8.  Geben Sie auf der Seite „Assistenten abschließen“ im Feld **Konfigurationsname****LoadXMLData EV Configuration** ein, überprüfen Sie den Inhalt der Konfiguration, und klicken Sie anschließend auf **Fertig stellen**.  
  
### <a name="to-create-the-xml-configuration-for-the-loadxmldata-package"></a>So erstellen Sie die XML-Konfiguration für das LoadXMLData-Paket  
  
1.  Doppelklicken Sie im Projektmappen-Explorer auf LoadXMLData.dtsx.  
  
2.  Klicken Sie im [!INCLUDE[ssIS](../includes/ssis-md.md)] -Designer auf eine beliebige Stelle im Vordergrund der Entwurfsoberfläche der Ablaufsteuerung.  
  
3.  Klicken Sie im Menü **SSIS** auf **Paketkonfigurationen**.  
  
4.  Aktivieren Sie im Dialogfeld Paketkonfigurationsplaner das Kontrollkästchen **Paketkonfigurationen aktivieren** , und klicken Sie auf **Hinzufügen**.  
  
5.  Klicken Sie auf der Willkommensseite des Paketkonfigurations-Assistenten auf **Weiter**.  
  
6.  Wählen Sie auf der Seite Konfigurationstyp auswählen die Option **XML-Konfigurationsdatei** in der Liste **Konfigurationstyp** aus, und klicken Sie auf **Durchsuchen**.  
  
7.  Navigieren Sie im Dialogfeld **Speicherort der Konfigurationsdatei auswählen** zu „C:\DeploymentTutorial“, geben Sie im Feld **Dateiname****LoadXMLDataConfig** ein, und klicken Sie anschließend auf **Speichern**.  
  
8.  Klicken Sie auf der Seite „Konfigurationstyp auswählen“ auf **Weiter**.  
  
9. Erweitern Sie auf der Seite „Eigenschaften für den Exportvorgang auswählen“ LoadXMLData, Executables, Load XML Data und Properties, und aktivieren Sie anschließend die Kontrollkästchen **[XMLSource].[XMLData]** und **[XMLSource].[XMLSchemaDefinition]** .  
  
10. Klicken Sie auf **Weiter**.  
  
11. Geben Sie auf der Seite „Assistenten abschließen“ im Feld **Konfigurationsname****LoadXMLData Configuration** ein, überprüfen Sie den Inhalt der Konfiguration, und klicken Sie anschließend auf **Fertig stellen**.  
  
12. Überprüfen Sie im Dialogfeld **Paketkonfigurationsplaner** , ob „LoadXMLData EV Configuration“ als Erstes und „LoadXMLData Configuration“ als Zweites aufgeführt wird, und klicken Sie anschließend auf **Schließen**.  
  
## <a name="next-task-in-lesson"></a>Nächste Aufgabe in der Lektion  
[Schritt 5: Testen der aktualisierten Pakete](../integration-services/lesson-1-5-testing-the-updated-packages.md)  
  
## <a name="see-also"></a>Weitere Informationen  
[Paketkonfigurationen](./packages/legacy-package-deployment-ssis.md)  
[Erstellen von Paketkonfigurationen](./packages/legacy-package-deployment-ssis.md)  
[Zugriff auf Dateien, die von Paketen verwendet werden](../integration-services/security/security-overview-integration-services.md#files)