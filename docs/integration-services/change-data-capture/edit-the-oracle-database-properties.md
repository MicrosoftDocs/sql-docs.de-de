---
description: Bearbeiten der Oracle-Datenbankeigenschaften
title: Bearbeiten der Oracle-Datenbankeigenschaften | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- oraProp
ms.assetid: 58dc99f1-ee6b-4508-bb66-2bc589611ff7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b1d5eb987c5dfd62a67bbe6b536b4c2258463741
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130663"
---
# <a name="edit-the-oracle-database-properties"></a>Bearbeiten der Oracle-Datenbankeigenschaften

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Verwenden Sie die Registerkarte Oracle im Eigenschaften-Editor, um Änderungen an der Beschreibung vorzunehmen, die Sie im Assistenten für neue Instanzen auf der Seite Create CDC database angegeben haben, und um Änderungen an den Verbindungsinformationen zur Oracle Log Mining-Datenbank vorzunehmen.  
  
 Im Folgenden werden die Informationen auf der Registerkarte **Oracle** beschrieben.  
  
 **Name**  
 Der Name der CDC-Instanz, die Sie im Assistenten für neue Instanzen auf der Seite Create CDC Database eingegeben haben. Dieses Feld ist schreibgeschützt, und Sie können diese Informationen nicht bearbeiten.  
  
 **Beschreibung**  
 Sie können die Beschreibung der neuen Instanz bearbeiten oder eine Beschreibung hinzufügen, falls Sie diesen Schritt beim Erstellen der CDC-Instanz nicht ausgeführt haben.  
  
 **Oracle Connect String**  
 Dies ist die Oracle-Verbindungszeichenfolge zum Computer mit der verwendeten Oracle-Datenbank. Dieses Feld ist schreibgeschützt, und Sie können diese Informationen nicht bearbeiten. Dies liegt daran, dass bei bestimmten Änderungen an der Verbindungszeichenfolge für die Oracle CDC-Instanz möglicherweise auf eine völlig andere Oracle-Datenbank verwiesen wird, sodass der in der Tabelle **cdc.xdbcdc_config** gespeicherte CDC-Instanzstatus beschädigt wird. Wenn kleinere Änderungen erforderlich sind, können Sie die Verbindungszeichenfolge mithilfe von SQL Server Management Studio direkt in der Konfigurationstabelle ändern.  
  
 **Oracle Log Mining Authentication**  
 Um die Authentifizierungsinformationen für die Oracle-Datenbank einzugeben, in der die Log Mining-Komponente enthalten ist, wählen Sie unter **Authentifizierung** eine der folgenden Optionen aus:  
  
-   **Windows-Authentifizierung**: Wählen Sie diese Option, um die aktuellen Anmeldeinformationen für die Windows-Domäne zu verwenden. Sie können diese Option nur verwenden, wenn die Oracle-Datenbank für die Nutzung der Windows-Authentifizierung konfiguriert ist.  
  
-   **Oracle Authentication**: Wenn Sie diese Option aktivieren, müssen Sie **Benutzername** und **Kennwort** für den Benutzer der Oracle-Datenbank eingeben, mit der Sie eine Verbindung herstellen.  
  
 Sie können die Oracle-Datenbankeigenschaften im Viewer anzeigen. Beim Verwenden des Viewers sind die Informationen schreibgeschützt. Der Viewer enthält auch eine Liste der aufgezeichneten Spalten in der Tabelle. Informationen zum Zugriff auf den Viewer finden Sie unter [How to Manage a CDC Instance](../../integration-services/change-data-capture/how-to-manage-a-cdc-instance.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwalten eines CDC Service über die CDC Designer Console](../../integration-services/change-data-capture/how-to-manage-a-cdc-service-from-the-cdc-designer-console.md)   
 [Herstellen einer Verbindung mit einer Oracle-Quelldatenbank](../../integration-services/change-data-capture/connect-to-an-oracle-source-database.md)   
 [Herstellen einer Verbindung mit Oracle](../../integration-services/change-data-capture/connect-to-oracle.md)  
  
  
