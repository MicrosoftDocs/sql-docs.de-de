---
description: Erstellen eines Data Quality-Projekts
title: Erstellen eines Data Quality-Projekts
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
f1_keywords:
- sql13.dqs.dqproject.newdqproject.f1
helpviewer_keywords:
- create,data quality project
- data quality project,create
ms.assetid: 19c52d2b-d28e-4449-ab59-5fe0dc326cd9
author: swinarko
ms.author: sawinark
ms.openlocfilehash: 62dd18cb55aa57b95ab325b3e48d1ec618bd75c5
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88487967"
---
# <a name="create-a-data-quality-project"></a>Erstellen eines Data Quality-Projekts

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sqlserver.md)]

  In diesem Thema wird beschrieben, wie mit [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)]ein Data Quality-Projekt erstellt wird. Ein Data Quality-Projekt wird verwendet, um die Bereinigungs- oder Abgleichsaktivität in [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) auszuführen.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
 Sie müssen über eine relevante Wissensdatenbank verfügen, die im Data Quality-Projekt für die Bereinigungs- oder Abgleichsaktivität verwendet werden kann.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Sie müssen über die dqs_kb_editor- oder dqs_kb_operator-Rolle in der DQS_MAIN-Datenbank verfügen, um ein Data Quality-Projekt zu erstellen.  
  
##  <a name="create-a-data-quality-project"></a><a name="Create"></a> Erstellen eines Data Quality-Projekts  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)][Führen Sie die Data Quality-Client Anwendung](../data-quality-services/run-the-data-quality-client-application.md)aus.  
  
2.  Klicken Sie im [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] -Startbildschirm auf **Neues Data Quality-Projekt**.  
  
3.  Im Bildschirm **Neues Data Quality-Projekt** :  
  
    1.  Geben Sie im Feld **Name** einen Namen für das neue Data Quality-Projekt ein.  
  
    2.  (Optional) Geben Sie im Textfeld **Beschreibung** eine Beschreibung für das neue Data Quality-Projekt ein.  
  
    3.  Wählen Sie in der Liste **Wissensdatenbank verwenden** eine Wissensdatenbank aus, die für das Data Quality-Projekt verwendet werden soll. Der Bereich **Details zur Wissensdatenbank: <Name der Wissensdatenbank>** auf der rechten Seite zeigt die in der ausgewählten Wissenschaftsdatenbank verfügbaren Domänennamen an.  
  
    4.  Klicken Sie im Bereich **Aktivität auswählen** auf eine Aktivität, die Sie mit diesem Data Quality-Projekt ausführen möchten:  
  
        -   **Bereinigung**: Wählen Sie diese Aktivität, um die Quelldaten zu bereinigen.  
  
        -   **Abgleich**: Wählen Sie diese Aktivität, um einen Abgleich durchzuführen. Diese Aktivität ist nur verfügbar, wenn die für das Data Quality-Projekt ausgewählte Wissensdatenbank eine Abgleichsrichtlinie umfasst.  
  
4.  Klicken Sie auf **Erstellen** , um ein Data Quality-Projekt zu erstellen.  
  
##  <a name="follow-up-after-creating-a-data-quality-project"></a><a name="FollowUp"></a> Nachverfolgung: nach dem Erstellen eines Data Quality-Projekts  
 Nachdem Sie ein Data Quality-Projekt erstellt haben, wird ein Assistent angezeigt, mit dem Sie die ausgewählte Aktivität – Bereinigung oder Abgleich – ausführen können. Weitere Informationen zu den Bereinigungs- und Abgleichsaktivitäten finden Sie unter [Datenbereinigung](../data-quality-services/data-cleansing.md) und [Datenabgleich](../data-quality-services/data-matching.md).  
  
  
