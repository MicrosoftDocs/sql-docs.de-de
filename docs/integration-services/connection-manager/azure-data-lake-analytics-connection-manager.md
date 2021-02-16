---
title: Azure Data Lake Analytics-Verbindungs-Manager | Microsoft-Dokumentation
description: Ein SSIS-Paket (SQL Server Integration Services) kann den Azure Data Lake Analytics-Verbindungs-Manager verwenden, um ein Data Lake Analytics-Konto zu verbinden.
ms.custom: ''
ms.date: 05/18/2018
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPADLSCM.F1
- sql14.dts.designer.afpadlscm.f1
ms.assetid: f4c44553-0f08-4731-ac47-7534990b8c8d
author: yanancai
ms.author: yanacai
ms.reviewer: maghan
ms.openlocfilehash: ff7ad8e742469f7afea8770a7323352e99ee41ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338456"
---
# <a name="azure-data-lake-analytics-connection-manager"></a>Azure Data Lake Analytics-Verbindungs-Manager

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Ein SSIS-Paket (SQL Server Integration Services) kann den Azure Data Lake Analytics-Verbindungs-Manager verwenden, um ein Data Lake Analytics-Konto mit einem der beiden folgenden Authentifizierungstypen zu verbinden:
-   Azure Active Directory (Azure AD)-Benutzeridentität
-   Azure AD-Dienstidentität 

Der Data Lake Analytics-Verbindungs-Manager ist eine Komponente von [SQL Server Integration Services (SSIS) Feature Pack für Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).
 
## <a name="configure-the-connection-manager"></a>Konfigurieren des Verbindungs-Managers

1. Wählen Sie im Dialogfeld **SSIS-Verbindungs-Manager hinzufügen** die Option **AzureDataLakeAnalytics** > **Hinzufügen** aus. Das Dialogfeld **Azure Data Lake Analytics-Verbindungs-Manager-Editor** wird geöffnet.
  
2. Geben Sie im Dialogfeld **Azure Data Lake Analytics-Verbindungs-Manager-Editor** im Feld **ADLA-Kontoname** den Kontonamen des Data Lake Analytics-Kontos ein. Beispiel: meinADLAKontoname.
  
3. Wählen Sie im Feld **Authentifizierung** den entsprechenden Authentifizierungstyp für den Zugriff auf Daten in Data Lake Analytics aus.

   a. Wenn Sie die Authentifizierungsoption **Azure AD-Benutzeridentität** ausgewählt haben, gehen Sie wie folgt vor:
   
      i. Geben Sie Werte für die Felder **Benutzername** und **Kennwort** an.    
      ii. Klicken Sie auf **Verbindung testen**, um die Verbindung zu testen. Wenn Sie oder der Mandantenadministrator dem Zugriff von SSIS auf Ihre Data Lake Analytics-Daten nicht zugestimmt haben, klicken Sie bei Aufforderung auf **Zustimmen**. Weitere Informationen über diese Zustimmungsoberfläche finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](/azure/active-directory/manage-apps/plan-an-application-integration#integrating-applications-with-azure-ad).
    
   > [!NOTE] 
   > Bei Auswahl der Authentifizierungsoption **Azure AD-Benutzeridentität** werden die mehrstufige Authentifizierung und die Authentifizierung mittels Microsoft-Konto nicht unterstützt.
    
   b. Wenn Sie die Authentifizierungsoption **Azure AD-Dienstidentität** ausgewählt haben, gehen Sie wie folgt vor:
   
      i. Erstellen Sie eine Azure AD-Anwendung und einen Dienstprinzipal für den Zugriff auf das Data Lake Analytics-Konto. Weitere Informationen zu dieser Authentifizierungsoption finden Sie unter [Use portal to create Active Directory application and service principal that can access resources (Verwenden von Portal zum Erstellen von Active Directory-Anwendungen und Dienstprinzipal zum Zugriff auf Ressourcen)](/azure/azure-resource-manager/resource-group-create-service-principal-portal).    
      ii. Weisen Sie der Azure AD-Anwendung die entsprechenden Berechtigungen zu, damit diese auf Ihr Data Lake Analytics-Konto zugreifen kann. Erfahren Sie, wie Sie Berechtigungen für Ihr Data Lake Analytics-Konto [mithilfe des Assistenten zum Hinzufügen eines Benutzers](/azure/data-lake-analytics/data-lake-analytics-manage-use-portal#add-a-new-user) gewähren.    
      iii. Geben Sie Werte für die Felder **Anwendungs-ID**, **Authentifizierungsschlüssel** und **Mandanten-ID** an.    
      iv. Klicken Sie auf **Verbindung testen**, um die Verbindung zu testen.  

4. Klicken Sie auf **OK**, um das Dialogfeld **Azure Data Lake Analytics-Verbindungs-Manager-Editor** zu schließen.  

## <a name="view-the-properties-of-the-connection-manager"></a>Anzeigen der Eigenschaften des Verbindungs-Managers
Die Eigenschaften des Verbindungs-Managers, die Sie im Fenster **Eigenschaften** erstellt haben, werden angezeigt.  
  
