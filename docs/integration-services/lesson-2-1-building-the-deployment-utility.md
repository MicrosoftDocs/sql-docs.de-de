---
description: 'Lektion 2-1: Erstellen des Bereitstellungshilfsprogramms'
title: 'Schritt 1: Erstellen des Bereitstellungs-Hilfsprogramms | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 1ff4dcff-89b3-4b99-a725-5f7963e98abf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3e1b6c318bd971e5352ad0191ccad51e20f5560a
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "92193800"
---
# <a name="lesson-2-1---building-the-deployment-utility"></a>Lektion 2-1: Erstellen des Bereitstellungshilfsprogramms

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


In dieser Aufgabe konfigurieren und erstellen Sie ein Bereitstellungsprogramm für das Deployment Tutorial-Projekt.  
  
Vor dem Erstellen des Bereitstellungshilfsprogramms müssen Sie die Eigenschaften des Deployment Tutorial-Projekts ändern. Sie können diese Eigenschaften im Dialogfeld **Deployment Tutorial-Eigenschaftenseiten** konfigurieren. In diesem Dialogfeld müssen Sie die Fähigkeit aktivieren, Konfigurationen während der Bereitstellung zu aktualisieren, und angeben, dass beim Erstellungsvorgang ein Bereitstellungshilfsprogramm generiert werden soll. Nach dem Festlegen der Eigenschaften erstellen Sie das Projekt.  
  
[!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] stellt eine Reihe von Fenstern zum Debuggen von Paketen bereit. Sie können Fehler-, Warn- und Informationsmeldungen anzeigen, den Status von Paketen zur Laufzeit nachverfolgen oder den Fortschritt und die Ergebnisse von Erstellungsvorgängen anzeigen. In dieser Lektion verwenden Sie das Ausgabefenster, um den Fortschritt und die Ergebnisse beim Erstellen des Bereitstellungshilfsprogramms anzuzeigen.  
  
### <a name="to-set-the-deployment-utility-properties"></a>So legen Sie die Eigenschaften des Bereitstellungshilfsprogramms fest  
  
1.  Wenn [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] noch nicht geöffnet ist, klicken Sie auf **Start**, zeigen Sie auf **Alle Programme**, zeigen Sie auf **Microsoft SQL Server 2005**, und klicken Sie dann auf **Business Intelligence Development Studio**.  
  
2.  Klicken Sie im Menü **Datei** auf **Öffnen**&gt; **Projekt/Projektmappe**. Wählen Sie den Ordner **Deployment Tutorial** aus, klicken Sie auf **Öffnen** und anschließend doppelt auf **Deployment Tutorial.sln**.  
  
3.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf „Deployment Tutorial“ und anschließend auf **Eigenschaften**.  
  
4.  Erweitern Sie im Dialogfeld **Deployment Tutorial-Eigenschaftenseiten** den Knoten Konfigurationseigenschaften, und klicken Sie auf Bereitstellungshilfsprogramm.  
  
5.  Überprüfen Sie im rechten Bereich des Dialogfelds **Deployment Tutorial-Eigenschaftenseiten** , ob **AllowConfigurationChanges** auf **true** festgelegt ist, legen Sie **CreateDeploymentUtility** auf **true** fest, und aktualisieren Sie optional den Standardwert von **DeploymentOutputPath**.  
  
6.  Klicken Sie auf **OK**.  
  
### <a name="to-build-the-deployment-utility"></a>So erstellen Sie das Bereitstellungshilfsprogramm  
  
1.  Klicken Sie im Projektmappen-Explorer auf **Deployment Tutorial**.  
  
2.  Klicken Sie im Menü **Ansicht** auf **Ausgabe**. Das Ausgabefenster befindet sich standardmäßig in der unteren linken Ecke von [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)].  
  
3.  Klicken Sie im Menü **Erstellen** auf **Deployment Tutorial erstellen**.  
  
4.  Überprüfen Sie im Ausgabefenster die folgenden Informationen:  
  
    Erstellung gestartet: SQL Integration Services-Projekt: Inkrementell ...  
  
    Bereitstellungshilfsprogramm wird erstellt...  
  
    Das Bereitstellungshilfsprogramm wurde erstellt.  
  
    Erstellung abgeschlossen -- 0 Fehler, 0 Warnungen  
  
    ========== Erstellen: 0 erfolgreich, Fehler bei 0, 1 aktuell, 0 übersprungen ==========  
  
5.  Klicken Sie im Menü **Datei** auf **Beenden**. Wenn Sie zum Speichern der Änderungen an Deployment Tutorial-Elementen aufgefordert werden, klicken Sie auf **Ja**.  
  
## <a name="next-task-in-lesson"></a>Nächste Aufgabe in der Lektion  
[Schritt 2: Überprüfen des Bereitstellungspakets](../integration-services/lesson-2-2-verifying-the-deployment-bundle.md)  
  
## <a name="see-also"></a>Weitere Informationen  
[Erstellen eines Bereitstellungs-Hilfsprogramms](./packages/legacy-package-deployment-ssis.md)  
  
  
