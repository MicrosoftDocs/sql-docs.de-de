---
description: Erstellen einer Wissensdatenbank
title: Erstellen einer Wissensdatenbank
ms.date: 06/04/2013
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
f1_keywords:
- sql13.dqs.kb.selectkb.f1
- sql13.dqs.kb.newkb.f1
ms.assetid: 2733a284-975f-4650-abcc-cc2aad074cab
author: swinarko
ms.author: sawinark
ms.openlocfilehash: 1dd8e0379d25b231de873ccfd1529284e62f0f1d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88487905"
---
# <a name="create-a-knowledge-base"></a>Erstellen einer Wissensdatenbank

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sqlserver.md)]

  In diesem Thema wird beschrieben, wie eine Wissensdatenbank in [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) erstellt und auf die Domänenverwaltung, die Wissensermittlung und das Hinzufügen einer Abgleichsrichtlinie vorbereitet wird.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
 Um eine Wissensdatenbank zu erstellen, müssen Sie [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] und [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)]installiert haben.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Sie müssen über die dqs_kb_editor- oder dqs_administrator-Rolle in der DQS_MAIN-Datenbank verfügen, um eine Wissensdatenbank zu erstellen.  
  
##  <a name="create-a-knowledge-base"></a><a name="Createaknowledgebase"></a> Erstellen einer Wissensdatenbank  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)][Führen Sie die Data Quality-Client Anwendung](../data-quality-services/run-the-data-quality-client-application.md)aus.  
  
2.  Klicken Sie auf dem [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] -Startbildschirm auf **Neue Wissensdatenbank**.  
  
3.  Geben Sie einen Namen und eine Beschreibung für die neue Wissensdatenbank ein.  
  
4.  Wählen Sie unter **Wissensdatenbank erstellen aus**aus, worauf die neue Wissensdatenbank basieren soll:  
  
    -   Wählen Sie **Keine** aus, wenn die neue Wissensdatenbank nicht auf einer vorhandenen Wissensdatenbank oder Datendatei basieren soll.  
  
    -   Wählen Sie **Vorhandene Wissensdatenbank** aus, um eine Wissensdatenbank, die bereits auf [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)]erstellt wurde, oder die Standardwissensdatenbank als Grundlage für die neue Wissensdatenbank zu verwenden. Wählen Sie die Wissensdatenbank aus der Dropdownliste **Wissensdatenbank auswählen** aus, oder klicken Sie auf **Durchsuchen** , um das Dialogfeld **Wissensdatenbank auswählen** anzuzeigen, wählen Sie eine vorhandene Wissensdatenbank als Grundlage für die neue Wissensdatenbank aus, und klicken Sie dann auf **OK**. Wenn Sie eine Wissensdatenbank aus dem Tablett auswählen, werden die Domänen und Abgleichsregeln in der Wissensdatenbank im rechten Bereich des Dialogfelds angezeigt. Sie können auch die Wissensdatenbank **DQS-Daten** auswählen. Das ist die Standardwissensdatenbank, die allgemeine Standarddomänen und auf US-amerikanische Unternehmens-, Adressen- und Parteidaten bezogenes Wissen enthält.  
  
    -   Wählen Sie **Aus DQS-Datei importieren** aus, um die neue Wissensdatenbank auf eine DQS-Datei auf dem [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)]zu basieren. Klicken Sie auf **Durchsuchen**, wählen Sie eine DQS-Datendatei mit der Erweiterung ".dqs" aus, und klicken Sie dann auf **OK**.  
  
5.  Wählen Sie unter **Aktivität auswählen**die Aktivität aus, die Sie für die neue Wissensdatenbank ausführen möchten:  
  
    -   Wählen Sie **Domänenverwaltung** aus, um die Wissensdatenbank zu erstellen, und öffnen Sie dann die Bildschirme, in denen Sie die Domänen in der Wissensdatenbank ändern.  
  
    -   Wählen Sie **Wissensermittlung** aus, um die Wissensdatenbank zu erstellen und um den Assistenten zum Analysieren eines Datenbeispiels zu öffnen, und füllen Sie die Domänen der Wissensdatenbank mit den Ergebnissen auf.  
  
    -   Wählen Sie **Abgleichsrichtlinie** aus, um eine Abgleichsrichtlinie zu erstellen und sie der Wissensdatenbank hinzuzufügen.  
  
6.  Klicken Sie auf **Erstellen**.  
  
##  <a name="follow-up-after-creating-a-knowledge-base"></a><a name="FollowUp"></a> Nachverfolgung: Nach dem Erstellen einer Wissensdatenbank  
 Nachdem Sie eine Wissensdatenbank erstellt haben, werden ein Assistent für die Wissensermittlung, ein Assistent zum Erstellen einer Abgleichsrichtlinie oder Seiten für die Domänenverwaltung angezeigt. Weitere Informationen zu Wissensermittlung, Domänenverwaltung oder Abgleichsrichtlinien finden Sie unter [Durchführen der Wissensermittlung](../data-quality-services/perform-knowledge-discovery.md), [Verwalten einer Domäne](../data-quality-services/managing-a-domain.md) oder [Erstellen einer Abgleichsrichtlinie](../data-quality-services/create-a-matching-policy.md).  
  
  
