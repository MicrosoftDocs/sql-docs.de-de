---
title: 'Lektion 1: Erstellen einer Beispiel-Abonnentendatenbank | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie eine kleine Abonnentendatenbank erstellen, um Abonnementdaten zu speichern, die von einem datengesteuerten Abonnement verwendet werden.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 47a882b7-efe5-4ee6-bef4-06118eb56903
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8be94f5ae47a7b7545cd553299673c823993402a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062985"
---
# <a name="lesson-1-creating-a-sample-subscriber-database"></a>Lektion 1: Erstellen einer Beispiel-Abonnentendatenbank

In dieser Lektion des [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] -Tutorials erstellen Sie eine kleine „subscriber“-Datenbank, um Abonnementdaten zu speichern, die von einem datengesteuerten Abonnement verwendet werden. Wenn das Abonnement verarbeitet wird, werden diese Daten vom Berichtsserver abgerufen und dazu verwendet, die Berichtsausgabe anzupassen. Beispielsweise enthalten die Datenzeilen spezifische Auftragsnummern, die für Filter verwendet werden sollen, sowie die Angabe des Dateiformats der generierten Berichte, wenn sie erstellt werden.  
  
Für diese Lektion wird angenommen, dass Sie [!INCLUDE[ssManStudioFull_md](../includes/ssmanstudiofull-md.md)] zur Erstellung einer SQL Server-Datenbank verwenden.  
  
### <a name="to-create-a-sample-subscriber-database"></a>So erstellen Sie eine Beispiel-Abonnentendatenbank  
  
1.  Starten Sie [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)], und stellen Sie eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion_md](../includes/ssdenoversion-md.md)]her.  
  
2.  Klicken Sie mit der rechten Maustaste auf „Datenbanken“, und wählen Sie **Neue Datenbank...** aus.  
  
3.  Geben Sie im Dialogfeld „Neue Datenbank“ in das Feld **Datenbankname***Subscribers* ein. 
4. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
5.  Klicken Sie auf der Symbolleiste auf die Schaltfläche **Neue Abfrage** .  
  
6.  Kopieren Sie die folgenden [!INCLUDE[tsql](../includes/tsql-md.md)] -Anweisungen in die leere Abfrage:  
  
    ```  
    Use Subscribers  
    CREATE TABLE [dbo].[OrderInfo] (  
        [SubscriptionID] [int] NOT NULL PRIMARY KEY ,  
        [Order] [nvarchar] (20) NOT NULL,  
        [FileType] [bit],  
        [Format] [nvarchar] (20) NOT NULL ,  
    ) ON [PRIMARY]  
    GO  
  
    INSERT INTO [dbo].[OrderInfo] (SubscriptionID, [Order], FileType, Format)   
    VALUES ('1', 'so43659', '1', 'IMAGE')  
    INSERT INTO [dbo].[OrderInfo] (SubscriptionID, [Order], FileType, Format)   
    VALUES ('2', 'so43664', '1', 'MHTML')  
    INSERT INTO [dbo].[OrderInfo] (SubscriptionID, [Order], FileType, Format)   
    VALUES ('3', 'so43668', '1', 'PDF')  
    INSERT INTO [dbo].[OrderInfo] (SubscriptionID, [Order], FileType, Format)   
    VALUES ('4', 'so71949', '1', 'Excel')  
    GO  
    ```  
  
7.  Klicken Sie auf **! Führen Sie**  in der Symbolleiste aus.  
  
8.  Verwenden Sie eine SELECT-Anweisung, um sicherzustellen, dass drei Datenzeilen vorhanden sind. Beispiel: `select * from OrderInfo`  
  
## <a name="next-steps"></a>Nächste Schritte  
+ Sie haben erfolgreich die Abonnementdaten erstellt, welche die Berichtsverteilung steuern und über die die Berichtsausgabe für jeden Abonnenten angepasst werden kann. 
+ Ändern Sie anschließend die Datenquelleneigenschaften des Berichts, um gespeicherte Anmeldeinformationen zu verwenden. 
+ Sie ändern auch den Berichtsentwurf, um einen Parameter einzuschließen, den das Abonnement mit den Abonnentendaten verwendet. [Lektion 2: Ändern der Eigenschaften der Berichtsdatenquelle](../reporting-services/lesson-2-modifying-the-report-data-source-properties.md).  

## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines datengesteuerten Abonnements](../reporting-services/create-a-data-driven-subscription-ssrs-tutorial.md)  
[Erstellen einer Datenbank](../relational-databases/databases/create-a-database.md)  
[Erstellen eines einfachen Tabellenberichts](../reporting-services/create-a-basic-table-report-ssrs-tutorial.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
