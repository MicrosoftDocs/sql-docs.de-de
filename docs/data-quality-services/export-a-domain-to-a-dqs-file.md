---
description: Exportieren einer Domäne in eine DQS-Datei
title: Exportieren einer Domäne in eine DQS-Datei
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
ms.assetid: eba10d3d-b5c4-447b-8a30-fa07996cb28e
author: swinarko
ms.author: sawinark
ms.openlocfilehash: d3e37309a430b92e12493c178eeaf7a117a41552
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88484877"
---
# <a name="export-a-domain-to-a-dqs-file"></a>Exportieren einer Domäne in eine DQS-Datei

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sqlserver.md)]

  In diesem Thema wird beschrieben, wie eine Domäne in eine DQS-Datei in [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) exportiert wird. Sie können eine Domäne oder eine ganze Wissensdatenbank in eine Datendatei exportieren. Weitere Informationen zum Exportieren einer Wissensdatenbank finden Sie unter [Exportieren einer Wissensdatenbank in eine DQS-Datei](../data-quality-services/export-a-knowledge-base-to-a-dqs-file.md).  
  
 Indem Sie eine Domäne aus einer Wissensdatenbank in eine DQS-Datendatei exportieren und diese dann in eine andere Wissensdatenbank importieren, wird der Wissensgenerierungsprozess vereinfacht, Zeit gespart und der Aufwand verringert. Auf diese Weise haben Sie die Möglichkeit, eine Domäne und ihr Wissen zusammen mit anderen zu nutzen.  
  
 Sie können eine Einzeldomäne oder Verbunddomäne exportieren. Eine DQS-Datei, die eine Einzeldomäne enthält, umfasst alle Domänendaten einschließlich Domäneneigenschaften, Werten und Regeln außer Informationen zu den angefügten Verweisdaten. Eine DQS-Datei, die eine Verbunddomäne enthält, enthält alle Verbunddomänendaten, einschließlich aller Domänendaten für die Domänen, die in der Verbunddomäne enthalten sind, und die Verbunddomäneneigenschaften, -beziehungen und -regeln, aber keine Informationen über die Verweisdaten. Sie müssen die Domäne oder Verbunddomäne ggf. erneut an die die entsprechenden Verweisdatendienste anfügen, nachdem Sie die DQS-Datei importiert haben. Sowohl veröffentlichte als auch unveröffentlichte Daten werden exportiert.  
  
 Die durch den Exportvorgang erstellte DQS-Datendatei wird verschlüsselt, damit der Inhalt nicht angezeigt werden kann.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
 Um eine Domäne in eine DQS-Datei zu exportieren, müssen Sie eine Einzeldomäne oder eine Verbunddomäne mit mehreren Einzeldomänen erstellt und ausgewählt haben. Sie benötigen für den Export keine DQS-Datei. Eine DQS-Datei wird für Sie erstellt.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Sie müssen über die Rolle „dqs_kb_editor“ oder „dqs_administrator“ in der DQS_MAIN-Datenbank verfügen, um eine Domäne in eine DQS-Datei zu exportieren.  
  
##  <a name="export-a-domain-to-a-dqs-file"></a><a name="Export"></a> Exportieren einer Domäne in eine DQS-Datei  
 Sie können aus jeder Domänenverwaltungsseite exportieren. Der Exportbefehl ist in einem Steuerelement und in der Benutzeroberfläche sowie über einen Befehl im Kontextmenü des Domänenlistenbereichs verfügbar.  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)][Führen Sie die Data Quality-Client Anwendung](../data-quality-services/run-the-data-quality-client-application.md)aus.  
  
2.  Öffnen Sie im [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] -Startbildschirm in der Domänenverwaltungsaktivität eine Wissensdatenbank.  
  
3.  Wählen Sie auf der **Domänenverwaltungsseite** (eine beliebige Registerkarte kann ausgewählt sein) eine Einzeldomäne oder Verbunddomäne in der Liste **Domäne** aus.  
  
4.  Klicken Sie oberhalb der Domänenliste auf das Symbol **Daten der Wissensdatenbank exportieren** , und klicken Sie dann auf **Domäne exportieren**. Sie können auch mit der rechten Maustaste in der Liste **Domäne** auf die Domäne klicken, auf **Exportieren**zeigen und dann auf **Domäne exportieren**klicken.  
  
5.  Wechseln Sie im Dialogfeld **In Datendatei exportieren** zu dem Ordner, in dem Sie die Datei speichern möchten, benennen Sie die Datei, oder behalten Sie den Standardnamen bei, übernehmen Sie **DQS-Datendateien (\*.dqs)** unter **Dateityp**, und klicken Sie dann auf **Speichern**.  
  
6.  Überprüfen Sie im Dialogfeld **Domäne exportieren** , ob die Statuszeile im Dialogfeld angibt, dass der Export abgeschlossen wurde. Klicken Sie auf **OK**.  
  
##  <a name="follow-up-after-exporting-a-domain-to-a-dqs-file"></a><a name="FollowUp"></a> Nachverfolgung: Nach dem Exportieren einer Domäne in eine DQS-Datei  
 Nachdem Sie eine Domäne in eine DQS-Datei exportiert haben, können Sie die Domäne in eine andere Wissensdatenbank importieren.  
  
  
