---
description: Registrierte Server (F1-Hilfe)
title: Registrierte Server (F1-Hilfe)
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: reference
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], Help
- Registered Servers [SQL Server], help
- SQL Server Management Studio Help [SQL Server], registered servers
ms.assetid: 59f76b28-ba78-4a1a-b5d5-8b581f30114d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0d4b3a2e75a00e3688ec53c5e64e6ab7727375a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341044"
---
# <a name="registered-servers-f1-help"></a>Registrierte Server (F1-Hilfe)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Dieser Abschnitt enthält die F1-Hilfe zur Komponente Registrierte Server in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Er beschreibt die verschiedenen Optionen.
  
 Informationen zu registrierten Servern und Links zu den mit ihnen möglichen Aktionen finden Sie im Thema [Registrieren von Servern](./register-servers.md) . 
 

 Klicken Sie hier, um die Einstellungen für die registrierten Server zu speichern. 
 
 ## <a name="reporting-services-new-or-edit-server-registration-general-tab"></a>Reporting Services Neue Serverregistrierung und Serverregistrierung bearbeiten (Registerkarte Allgemein) 
  Auf dieser Registerkarte können Optionen festgelegt werden, wenn eine Instanz von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]registriert wird.  
  
 Um auf die Seite zuzugreifen, klicken Sie auf der Symbolleiste **Registrierte Server** auf **Reporting Services** , klicken Sie mit der rechten Maustaste auf eine registrierte Servergruppe wie **Reporting Services**, zeigen Sie auf **Neu**, und klicken Sie anschließend auf **Serverregistrierung**.  
  
### <a name="options"></a>Tastatur  
 **Servertyp**  
 Wenn Sie einen Server über Registrierte Server registrieren, ist das Feld **Servertyp** schreibgeschützt, wobei der Feldeintrag mit dem im Bereich **Registrierte Server** angezeigten Servertyp übereinstimmt. Um einen anderen Servertyp zu registrieren, klicken Sie auf der Symbolleiste **Registrierte Server** auf den gewünschten Server, bevor Sie mit der Registrierung eines neuen Servers beginnen.  
  
 **Servername**  
 Geben Sie die Berichtsserverinstanz an, zu der eine Verbindung hergestellt werden soll. In [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]können Sie über den Instanznamen des Berichtsservers auf einen Berichtsserver zugreifen. Für jede SQL Server-Instanz, die Sie installieren, ist eine Berichtsserverinstanz zulässig. Wenn Sie die Standardinstanz verwenden, geben Sie den Namen der SQL Server-Instanz ein. Wenn Sie eine benannte Instanz verwenden, geben Sie zum Verbinden mit dem Berichtsserver die benannte Instanz im Format MSSQL$InstanceName ein.  
  
 **Authentifizierung**  
 Die Authentifizierung bei einem Berichtsserver erfolgt durch Internetinformationsdienste (IIS). Wählen Sie beim Herstellen einer Verbindung mit Reporting Services einen der folgenden Authentifizierungsmodi aus:  
  
 **Windows-Authentifizierungsmodus (Windows-Authentifizierung)**  
 Stellt die Verbindung zur Berichtsserverinstanz mithilfe der Anmeldeinformationen von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows her.  
  
 **Standardauthentifizierung**  
 Stellen Sie die Verbindung mit **Standardauthentifizierung** her, wenn die Reporting Services-Installation für die Verwendung der Standardauthentifizierung konfiguriert ist.  
  
 **Formularauthentifizierung**  
 Stellen Sie die Verbindung mit **Formularauthentifizierung** her, wenn die Reporting Services-Installation für die Verwendung der Formularauthentifizierung konfiguriert ist.  
  
 **Benutzername**  
 Geben Sie den Benutzernamen ein, der für die Verbindung verwendet werden soll. Diese Option ist nur verfügbar, wenn Sie **Standardauthentifizierung** oder **Formularauthentifizierung** ausgewählt haben.  
  
 **Kennwort**  
 Geben Sie das Kennwort für den Benutzernamen ein. Diese Option kann nur bearbeitet werden, wenn Sie **Standardauthentifizierung** oder **Formularauthentifizierung** ausgewählt haben.  
  
 **Kennwort speichern**  
 Speichert das eingegebene Kennwort. Diese Option ist nur verfügbar, wenn Sie **Standardauthentifizierung** oder **Formularauthentifizierung** ausgewählt haben.  
  
> **HINWEIS:** Wenn Sie das Kennwort gespeichert haben und für die Zukunft nicht mehr speichern möchten, deaktivieren Sie das Kontrollkästchen, und klicken Sie auf **Speichern**.  
  
 **Name des registrierten Servers**  
 Der Name, der unter Registrierte Server angezeigt werden soll. Dieser Name muss mit dem Eintrag im Feld **Servername** nicht übereinstimmen.  
  
 **Beschreibung des registrierten Servers**  
 Geben Sie eine optionale Beschreibung des Servers ein.  
  
 **Test**  
 Klicken Sie hier, um die Verbindung mit dem unter **Servername** ausgewählten Server zu testen.  
  
 
 ## <a name="analysis-services---multidimensional-data-new-or-edit-server-registration-general-tab"></a>Analysis Services – Mehrdimensionale Daten Neue Serverregistrierung und Serverregistrierung bearbeiten (Registerkarte Allgemein)
 
  Auf dieser Registerkarte können Optionen festgelegt werden, wenn eine Instanz von [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]registriert wird.  
  
 Um auf diese Seite zuzugreifen, klicken Sie auf der Symbolleiste für Registrierte Server auf **Analysis Services** , klicken Sie mit der rechten Maustaste auf eine registrierte Servergruppe (z.B. **Analysis Services**), zeigen Sie auf **Neu**, und klicken Sie anschließend auf **Serverregistrierung**.  
  
### <a name="options"></a>Tastatur  
 **Servertyp**  
 Wenn Sie einen Server über Registrierte Server registrieren, ist das Feld **Servertyp** schreibgeschützt, wobei der Feldeintrag mit dem im Bereich Registrierte Server angezeigten Servertyp übereinstimmt. Um einen anderen Servertyp zu registrieren, klicken Sie auf der Symbolleiste **Registrierte Server** auf den gewünschten Server, bevor Sie mit der Registrierung eines neuen Servers beginnen.  
  
 **Servername**  
 Wählen Sie die Serverinstanz aus, mit der eine Verbindung hergestellt werden soll. Standardmäßig wird die Serverinstanz angezeigt, mit der zuletzt eine Verbindung bestanden hat.  
  
 **Authentifizierung**  
 Durch Windows-Authentifizierung können Benutzer mithilfe der [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Anmeldeinformationen eine Verbindung als Windows-Benutzer oder als Mitglied einer Windows-Gruppe herzustellen.  
  
 **Benutzername**  
 Diese Option steht in dieser Version nicht zur Verfügung.  
  
 **Kennwort**  
 Diese Option steht in dieser Version nicht zur Verfügung.  
  
 **Kennwort speichern**  
 Diese Option steht in dieser Version nicht zur Verfügung.  
  
 **Name des registrierten Servers**  
 Der Name, der unter Registrierte Server angezeigt werden soll. Dieser Name muss mit dem Eintrag im Feld **Servername** nicht übereinstimmen.  
  
 **Beschreibung des registrierten Servers**  
 Geben Sie eine optionale Beschreibung des Servers ein.  
  
 **Test**  
 Klicken Sie hier, um die Verbindung mit dem unter **Servername** ausgewählten Server zu testen. 
 
 ## <a name="ssis-new-or-edit-server-registration-general-tab"></a>SSIS Neue Serverregistrierung und Serverregistrierung bearbeiten (Registerkarte Allgemein) 
 
 Auf dieser Registerkarte können Optionen angegeben werden, wenn [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]registriert wird.  
  
 Um auf die Seite zuzugreifen, klicken Sie auf der Symbolleiste **Registrierte Server** auf **Integration Services** , klicken Sie mit der rechten Maustaste auf eine registrierte Servergruppe, zeigen Sie auf **Neu**, und klicken Sie anschließend auf **Serverregistrierung**.  
  
### <a name="options"></a>Tastatur  
 **Servertyp**  
 Wenn Sie einen Server über „Registrierte Server“ registrieren, ist das Feld **Servertyp** schreibgeschützt, wobei der Feldeintrag mit dem im Bereich für registrierte Server angezeigten Servertyp übereinstimmt. Wenn Sie einen anderen Servertyp registrieren möchten, klicken Sie, bevor Sie mit dem Registrieren eines neuen Servers beginnen, auf der Symbolleiste **Registrierte Server** auf **Datenbank-Engine**, **Analysis-Server**, **Reporting Services**, **SQL Server Compact** **Edition** oder **Integration Services**.  
  
 **Servername**  
 Wählen Sie den Server aus, mit dem eine Verbindung hergestellt werden soll. Standardmäßig wird der Server angezeigt, mit dem zuletzt eine Verbindung hergestellt wurde.  
  
 **Authentifizierung**  
 Mit dem Windows-Authentifizierungsmodus können Benutzer die Verbindung über ein [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Benutzerkonto herstellen.  
  
 **Benutzername**  
 Diese Option steht in dieser Version nicht zur Verfügung.  
  
 **Kennwort**  
 Diese Option steht in dieser Version nicht zur Verfügung.  
  
 **Kennwort speichern**  
 Diese Option steht in dieser Version nicht zur Verfügung.  
  
 **Name des registrierten Servers**  
 Der Name, der unter **Registrierte Server** angezeigt werden soll. Dieser Name muss mit dem Eintrag im Feld **Servername** nicht übereinstimmen.  
  
 **Beschreibung des registrierten Servers**  
 Geben Sie eine optionale Beschreibung des Servers ein.  
  
 **Test**  
 Klicken Sie hier, um die Verbindung mit dem unter **Servername** ausgewählten Server zu testen. 
  

 
 
