---
description: Erstellen und Bearbeiten eines CDC Service
title: Erstellen und Bearbeiten eines CDC Service | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 1b3d47a5-dc89-482d-bbc7-fff04f194c43
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6c0ecefc040d07405e700ee879b2220ff820e0cf
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88484643"
---
# <a name="how-to-create-and-edit-a-cdc-service"></a>Erstellen und Bearbeiten eines CDC Service

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  In diesen Verfahren wird beschrieben, wie Sie über die CDC Service Configuration Console einen neuen Oracle CDC Service erstellen und bearbeiten.  
  
 Für dieses Verfahren ist ein Windows-Benutzer mit Administratorrechten für den Computer erforderlich, auf dem der Oracle CDC Service konfiguriert wird.  
  
### <a name="to-create-a-new-cdc-service"></a>So erstellen Sie einen neuen CDC-Dienst  
  
1.  Wählen Sie im Menü **Start** die Option **CDC Service Configuration for Oracle**.  
  
2.  Klicken im linken Bereich mit der rechten Maustaste auf Local CDC Services und wählen Sie Neuer Dienst aus.  
  
     Sie können auch im **Aktionsbereich** auf **Neuer Dienst** klicken.  
  
3.  Geben Sie die erforderlichen Informationen im Dialogfeld New Oracle CDC Service ein. Informationen zum Eingeben von Informationen im Dialogfeld New Oracle CDC Service finden Sie unter [Create and Edit an Oracle CDC Service](../../integration-services/change-data-capture/create-and-edit-an-oracle-cdc-service.md) .  
  
     Die im Dialogfeld New Oracle CDC Service angegebenen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen werden vom Oracle CDC Service verwendet, wenn der Dienst ausgeführt wird. Diese Anmeldung muss lediglich Mitglied der festen Serverrolle public sein. Es sind keine weiteren Berechtigungen erforderlich. Nachdem neue Oracle CDC-Instanzen hinzugefügt wurden, wird über diese Anmeldung der **db_owner** -Zugriff auf die zugeordneten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CDC-Datenbanken gewährt.  
  
4.  Klicken Sie auf **OK**, nachdem Sie die erforderlichen Informationen eingegeben haben.  
  
     Zum Erstellen der Definition des Oracle CDC-Windows-Diensts benötigt das Programm Aktualisierungszugriff auf die MSXDBCDC-Datenbank in der zugeordneten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz. Wenn Sie auf **OK**klicken, wird der Benutzer in einem Dialogfeld aufgefordert, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen mit Aktualisierungszugriff auf die MSXDBCDC-Datenbank einzugeben.  
  
     Informationen zu den Daten, die Sie im Dialogfeld Verbindung mit SQL Server herstellen eingeben müssen, finden Sie unter [Connection to SQL Server](../../integration-services/change-data-capture/connection-to-sql-server.md).  
  
5.  Klicken Sie auf **OK** , um das Dialogfeld New Oracle CDC Service zu schließen.  
  
### <a name="to-edit-a-cdc-service"></a>So bearbeiten Sie einen CDC-Dienst  
  
1.  Wählen Sie im Menü **Start** die Option **CDC Service Configuration for Oracle**.  
  
2.  Wählen Sie im linken Bereich **Local CDC Services** , klicken Sie anschließend mit der rechten Maustaste auf den lokalen Dienst, den Sie bearbeiten möchten, und wählen Sie **Eigenschaften**.  
  
     Sie können den Dienst, den Sie verwenden möchten, auch im mittleren Bereich auswählen und dann im **Aktionsbereich** auf **Eigenschaften**klicken.  
  
3.  Geben Sie die erforderlichen Informationen im Dialogfeld CDC Service Properties ein. Informationen zum Eingeben von Informationen im Dialogfeld CDC Service Properties finden Sie unter [Create and Edit an Oracle CDC Service](../../integration-services/change-data-capture/create-and-edit-an-oracle-cdc-service.md) .  
  
4.  Klicken Sie nach dem Eingeben der erforderlichen Informationen auf **OK**, um das Dialogfeld Verbindung mit SQL Server herstellen zu öffnen.  
  
     Wenn mit einer Anmeldung ohne Schreibberechtigung für die MSXDBDCDC-Datenbank versucht wird, eine neue Oracle CDC-Instanz zu erstellen, wird eine Fehlermeldung angezeigt. Klicken Sie in diesem Dialogfeld auf **OK** , um das Dialogfeld Verbindung mit SQL Server herstellen anzuzeigen. In diesem Dialogfeld müssen Sie die Anmeldeinformationen für eine Anmeldung eingeben, die über die Schreibberechtigung für die MSXDBCDC-Datenbank verfügt, z.B. die Datenbankrolle **db_owner** .  
  
     Informationen zu den Daten, die Sie im Dialogfeld Verbindung mit SQL Server herstellen eingeben müssen, finden Sie unter [Connection to SQL Server](../../integration-services/change-data-capture/connection-to-sql-server.md).  
  
5.  Klicken Sie im Dialogfeld Verbindung mit Oracle herstellen auf **OK** . Beide Dialogfelder werden geschlossen, und der Dienst wird aktualisiert und registriert.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Change Data Capture Designer für Oracle von Attunity](../../integration-services/change-data-capture/change-data-capture-designer-for-oracle-by-attunity.md)   
 [Erstellen und Bearbeiten eines Oracle CDC Service](../../integration-services/change-data-capture/create-and-edit-an-oracle-cdc-service.md)  
  
  
