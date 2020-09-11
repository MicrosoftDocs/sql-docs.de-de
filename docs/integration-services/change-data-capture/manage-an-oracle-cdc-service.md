---
description: Manage an Oracle CDC Service
title: Verwalten eines Oracle CDC Service | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- createSrv
ms.assetid: 5972cee3-b1a9-4c56-aed6-bdddf84af283
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aea4d1beb0a77e6bc7a0456c3f3437c919698bcd
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88426072"
---
# <a name="manage-an-oracle-cdc-service"></a>Manage an Oracle CDC Service

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Sie können die CDC Service Configuration Console verwenden, um einen bestimmten CDC Service zu verwalten.  
  
 **So wählen Sie den gewünschten CDC Service aus**  
  
1.  Erweitern Sie links in der CDC Service Configuration Console den Punkt **Local CDC Services**.  
  
2.  Wählen Sie den CDC-Dienst aus, den Sie verwenden möchten.  
  
     Sie können auch mit der rechten Maustaste auf den gewünschten CDC-Dienst klicken und die gewünschte Aktion auswählen. Siehe [Mögliche Verwendung eines CDC Service](../../integration-services/change-data-capture/manage-an-oracle-cdc-service.md#BKMK_WhatcandowithCDCService).  
  
 **OR**  
  
1.  Wählen Sie im linken Bereich der CDC Service Configuration Console die Option **Local CDC Services** .  
  
2.  Wählen Sie im mittleren Bereich der CDC Service Configuration Console den Dienst aus, den Sie verwenden möchten.  
  
     Sie können auch mit der rechten Maustaste auf den gewünschten CDC-Dienst klicken und die gewünschte Aktion auswählen. Siehe [Mögliche Verwendung eines CDC Service](../../integration-services/change-data-capture/manage-an-oracle-cdc-service.md#BKMK_WhatcandowithCDCService).  
  
##  <a name="what-can-you-do-with-a-cdc-service"></a><a name="BKMK_WhatcandowithCDCService"></a> Mögliche Verwendung eines CDC Service  
 Beim Verwenden eines CDC-Diensts können Sie die folgenden Aktionen ausführen.  
  
### <a name="delete-the-service"></a>Löschen des Diensts  
 Klicken Sie im **Aktionsbereich** rechts in der CDC Service Configuration Console auf **Löschen** , um den Dienst zu löschen.  
  
 Sie können auch mit der rechten Maustaste auf den CDC-Dienst klicken, den Sie löschen möchten, und dann **Löschen**wählen.  
  
 **Hinweis**: Falls der Dienst beim Löschen des Diensts ausgeführt wird, wird der Dienst vor dem Löschvorgang beendet.  
  
 Zum Löschen der Definition des Oracle CDC-Windows-Diensts benötigt das Programm Aktualisierungszugriff auf die MSXDBCDC-Datenbank in der zugeordneten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz. Wenn Sie auf OK klicken, um den Dienst zu löschen, versucht das Programm, die Oracle CDC Service-Registrierung in der MSXDBCDC-Datenbank zu löschen. Wenn das Programm die Oracle CDC Service-Registrierung nicht löschen kann, weil es nicht über die richtigen Berechtigungen verfügt, wird der Benutzer aufgefordert, eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldung mit Aktualisierungsberechtigungen für die MSXDBCDC-Datenbank einzugeben.  
  
 Informationen zu den Daten, die Sie im Dialogfeld Verbindung mit SQL Server herstellen eingeben müssen, finden Sie unter [Connection to SQL Server for Delete](../../integration-services/change-data-capture/connection-to-sql-server-for-delete.md).  
  
### <a name="edit-the-cdc-service-properties"></a>Bearbeiten der Eigenschaften des CDC Service  
 Klicken Sie im **Aktionsbereich** rechts in der CDC Service Configuration Console auf **Eigenschaften**.  
  
 Sie können auch mit der rechten Maustaste auf den CDC-Dienst klicken, für den Sie die Eigenschaften bearbeiten möchten, und dann **Eigenschaften**wählen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwalten eines lokalen CDC Service](../../integration-services/change-data-capture/how-to-manage-a-local-cdc-service.md)  
  
  
