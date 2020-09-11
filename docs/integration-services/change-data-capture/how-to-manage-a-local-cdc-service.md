---
description: Verwalten eines lokalen CDC Service
title: Verwalten eines lokalen CDC Service | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 7f9be649-cd93-40c1-bc48-0480106f207c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1d103c224088d2dfa22fe1d8dcb36306177aac54
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88496180"
---
# <a name="how-to-manage-a-local-cdc-service"></a>Verwalten eines lokalen CDC Service

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  In diesem Verfahren wird beschrieben, wie Sie die CDC Service Configuration Console zum Verwalten bestimmter CDC-Dienste verwenden.  
  
### <a name="to-manage-a-specific-cdc-service"></a>So verwalten Sie einen bestimmten CDC Service  
  
1.  Wählen Sie im Menü **Start** die Option **CDC Service Configuration for Oracle**.  
  
2.  Erweitern Sie links in der CDC Service Configuration Console den Punkt **Local CDC Services**.  
  
3.  Wählen Sie den CDC-Dienst aus, den Sie verwenden möchten.  
  
     Sie können auch mit der rechten Maustaste auf den gewünschten CDC-Dienst klicken und die gewünschte Aktion auswählen.  
  
     **OR**  
  
     Wählen Sie links in der CDC Service Configuration Console die Option **Local CDC Services** und dann im mittleren Bereich der CDC Service Configuration Console den Dienst aus, den Sie verwenden möchten.  
  
     Sie können auch mit der rechten Maustaste auf den gewünschten CDC-Dienst klicken und die gewünschte Aktion auswählen.  
  
4.  Beim Verwenden eines CDC-Diensts können Sie die folgenden Tasks ausführen.  
  
    -   **Löschen des Diensts**  
  
         Klicken Sie im **Aktionsbereich** rechts in der CDC Service Configuration Console auf **Löschen** , um den Dienst zu löschen.  
  
         Sie können auch mit der rechten Maustaste auf den CDC-Dienst klicken, den Sie löschen möchten, und dann **Löschen**wählen.  
  
         **Hinweis**: Falls der Dienst beim Löschen des Diensts ausgeführt wird, wird der Dienst vor dem Löschvorgang beendet.  
  
         Zum Löschen der Definition des Oracle CDC-Windows-Diensts benötigt das Programm Aktualisierungszugriff auf die MSXDBCDC-Datenbank in der zugeordneten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz. Wenn Sie auf **OK** klicken, um den Dienst zu löschen, versucht das Programm, die Oracle CDC Service-Registrierung in der MSXDBCDC-Datenbank zu löschen. Falls bei dem Vorgang aufgrund fehlender Berechtigungen ein Fehler auftritt, wird der Benutzer über ein angezeigtes Dialogfeld aufgefordert, eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldung mit Aktualisierungszugriff auf die MSXDBCDC-Datenbank einzugeben.  
  
         Informationen zu den Daten, die Sie im Dialogfeld Verbindung mit SQL Server herstellen eingeben müssen, finden Sie unter [Manage an Oracle CDC Service](../../integration-services/change-data-capture/manage-an-oracle-cdc-service.md) und [Connection to SQL Server for Delete](../../integration-services/change-data-capture/connection-to-sql-server-for-delete.md).  
  
    -   **Bearbeiten der Eigenschaften des CDC Service**  
  
         Klicken Sie im **Aktionsbereich** rechts in der CDC Service Configuration Console auf **Eigenschaften**.  
  
         Sie können auch mit der rechten Maustaste auf den CDC-Dienst klicken, für den Sie die Eigenschaften bearbeiten möchten, und dann **Eigenschaften**wählen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Manage an Oracle CDC Service](../../integration-services/change-data-capture/manage-an-oracle-cdc-service.md)  
  
  
