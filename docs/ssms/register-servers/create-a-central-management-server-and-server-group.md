---
description: Erstellen eines zentralen Verwaltungsservers und einer Servergruppe
title: Erstellen eines zentralen Verwaltungsservers
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- configuration server
ms.assetid: da265482-3953-440a-ac23-0ab7e42a55eb
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 843411069a27d658cf9e2b157170d8ca0f14c91c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350549"
---
# <a name="create-a-central-management-server-and-server-group"></a>Erstellen eines zentralen Verwaltungsservers und einer Servergruppe

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

In diesem Thema wird beschrieben, wie Sie mithilfe von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine Instanz von [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] als zentralen Verwaltungsserver in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]festlegen. Zentrale Verwaltungsserver speichern eine Liste von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanzen, die in ein oder mehrere Gruppen zentraler Verwaltungsserver unterteilt sind. Aktionen, die unter Verwendung einer Gruppe zentraler Verwaltungsserver ausgeführt werden, wirken sich auf alle Server in der Servergruppe aus. Dazu gehören das Herstellen von Verbindungen mit Servern mithilfe des Objekt-Explorers und das Ausführen von [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen und richtlinienbasierten Verwaltungsrichtlinien auf mehreren Servern gleichzeitig.  
  
> [!NOTE]  
>  Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , die älter sind als [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] , können nicht als zentraler Verwaltungsserver festgelegt werden.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Security](#Security)  
  
-   **Erstellen eines zentralen Verwaltungsservers und einer Servergruppe mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Zwei Datenbankrollen in der msdb-Datenbank gewähren Zugriff auf die zentralen Verwaltungsserver. Nur Mitglieder der Rolle ServerGroupAdministratorRole können den zentralen Verwaltungsserver verwalten. Die Mitgliedschaft in der Rolle ServerGroupReaderRole ist erforderlich, um eine Verbindung mit einem zentralen Verwaltungsserver herzustellen.  
  
 Da die durch einen zentralen Verwaltungsserver verwalteten Verbindungen im Kontext des Benutzers unter Verwendung der Windows-Authentifizierung ausgeführt werden, können die gültigen Berechtigungen auf den registrierten Servern variieren. Der Benutzer kann beispielsweise für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz A ein Mitglied der festen Serverrolle sysadmin sein, für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz B jedoch über eingeschränkte Berechtigungen verfügen.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 Die folgenden Verfahren beschreiben, wie Sie die folgenden Schritte ausführen.  
  
1.  Erstellen Sie einen zentralen Verwaltungsserver.  
  
2.  Fügen Sie dem zentralen Verwaltungsserver eine oder mehrere Servergruppen hinzu, und fügen Sie den Servergruppen einen oder mehrere registrierte Server hinzu.  
  
#### <a name="create-a-central-management-server"></a>Erstellen eines zentralen Verwaltungsservers  
  
1.  Klicken Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]im Menü **Ansicht** auf **Registrierte Server**.  
  
2.  Erweitern Sie im Fenster „Registrierte Server“ den Knoten **Datenbank-Engine**, klicken Sie mit der rechten Maustaste auf **Zentrale Verwaltungsserver**, und klicken Sie dann auf **Zentralen Verwaltungsserver registieren**.  
  
3.  Wählen Sie im Dialogfeld **Neuen Server registrieren** aus der Dropdownliste von Servern die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus, die der zentrale Verwaltungsserver werden soll. Sie müssen für den zentralen Verwaltungsserver die Windows-Authentifizierung verwenden.  
  
4.  Geben Sie im Feld **Registrierter Server** einen Servernamen und ggf. eine Beschreibung ein.  
  
5.  Prüfen oder ändern Sie auf der Registerkarte **Verbindungseigenschaften** die Netzwerk- und Verbindungseigenschaften. Weitere Informationen finden Sie unter [Verbinden mit SQL Server-Datenbank-Engine &#40;Eigenschaftenseite Verbindung&#41;](../f1-help/connect-to-server-connection-properties-page-database-engine.md).  
  
6.  Klicken Sie auf **Testen**, um die Verbindung zu testen.  
  
7.  Klicken Sie auf **Speichern**. Die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird unter dem Ordner **Zentrale Verwaltungsserver** angezeigt.  
  
#### <a name="create-a-new-server-group-and-add-servers-to-the-group"></a>Erstellen einer neuen Servergruppe und Hinzufügen von Servern zur Gruppe  
  
1.  Erweitern Sie im Fenster **Registrierte Server****Zentrale Verwaltungsserver**. Klicken Sie mit der rechten Maustaste auf die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , die im obigen Verfahren hinzugefügt wurde, und wählen Sie **Neue Servergruppe** aus.  
  
2.  Geben Sie in **-Eigenschaften für neue Servergruppe** einen Gruppennamen und ggf. eine Beschreibung ein.  
  
3.  Klicken Sie im Fenster **Registrierte Server** mit der rechten Maustaste auf die Servergruppe, und klicken Sie auf **Neue Serverregistrierung**.  
  
4.  Wählen Sie im Fenster "Neue Serverregistrierung" eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus. Weitere Informationen finden Sie unter [Erstellen eines neu registrierten Servers &#40;SQL Server Management Studio&#41;](./create-a-new-registered-server-sql-server-management-studio.md). Fügen Sie weitere Server nach Bedarf hinzu.  
  
#### <a name="to-execute-queries-against-several-configuration-targets-at-the-same-time"></a>So führen Sie Abfragen für mehrere Konfigurationsziele gleichzeitig aus  
  
-   Nachdem Sie einen zentralen Verwaltungsserver, ein oder mehrere Servergruppen und ein oder mehrere registrierte Server erstellt haben, können Sie Abfragen für die ganze Gruppe gleichzeitig ausführen. Weitere Informationen zum gleichzeitigen Ausführen von [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisungen auf den Servern in einer Servergruppe finden Sie unter [Gleichzeitiges Ausführen von Anweisungen für mehrere Server &#40;SQL Server Management Studio&#41;](./execute-statements-against-multiple-servers-simultaneously.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwalten mehrerer Server mithilfe von zentralen Verwaltungsservern](../../relational-databases/administer-multiple-servers-using-central-management-servers.md)  
  
