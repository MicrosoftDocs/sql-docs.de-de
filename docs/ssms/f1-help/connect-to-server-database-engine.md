---
description: Verbindung mit Server herstellen (Datenbank-Engine)
title: Verbindung mit Server herstellen (Datenbank-Engine)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.connectoserverunknownservertype.f1
- sql13.swb.connection.login.sqlce.f1
- sql13.swb.connecttoce.f1
- SQL13.SWB.CONNECTION.LOGIN.SQLSERVER.F1
- sql13.swb.connection.login.sqlserver.f1
- sql13.swb.manageSS2k.f1
- connect-to-server-database-engine
ms.assetid: ee9017b4-8a19-4360-9003-9e6484082d41
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 04/07/2020
ms.openlocfilehash: 9f0eca39db873b277d0c698407c5c02afa520b35
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342663"
---
# <a name="connect-to-server-database-engine"></a>Verbindung mit Server herstellen (Datenbank-Engine)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Verwenden Sie dieses Dialogfeld, um Optionen bei der Verbindungsherstellung mit [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)] anzuzeigen oder anzugeben. In den meisten Fällen können Sie eine Verbindung herstellen, indem Sie im Feld **Servername** den Computernamen des Datenbankservers eingeben und dann auf **Verbinden** klicken. Wenn Sie eine Verbindung mit einer benannte Instanz herstellen, verwenden Sie den Computernamen, gefolgt von einem umgekehrten Schrägstrich und dem Namen der Instanz. Beispiel: `mycomputer\myinstance`. Geben Sie beim Herstellen der Verbindung mit [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)]den Computernamen gefolgt von **\sqlexpress** an.
  
Viele Faktoren können Auswirkungen auf die Fähigkeit zum Herstellen der Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]haben. Hilfe finden Sie in den folgenden Artikeln:

- [Tutorial, Lektion 1: Herstellen einer Verbindung mit der Datenbank-Engine](../../relational-databases/lesson-1-connecting-to-the-database-engine.md)  

- [Beheben von Verbindungsfehlern mit der SQL Server-Datenbank-Engine](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md)  

- [Beheben von Fehlern bei der Konnektivität mit SQL Server](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server)   
  
## <a name="options"></a>Tastatur

**Servertyp**  
Wenn Sie einen Server über den Objekt-Explorer registrieren, wählen Sie den Typ des Servers aus, mit dem die Verbindung hergestellt werden soll: [!INCLUDE[ssDE](../../includes/ssde_md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)], [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]oder [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Im verbleibenden Bereich des Dialogfelds werden nur die Optionen angezeigt, die auf den ausgewählten Servertyp zutreffen. Wenn Sie einen Server über „Registrierte Server“ registrieren, ist das Feld **Servertyp** schreibgeschützt, wobei der Feldeintrag mit dem in der Komponente „Registrierte Server“ angezeigten Servertyp übereinstimmt. Zum Registrieren eines anderen Servertyps wählen Sie auf der Symbolleiste Registrierte Server [!INCLUDE[ssDE](../../includes/ssde_md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion_md.md)], [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], [!INCLUDE[ssEW](../../includes/ssew-md.md)]oder [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] aus, bevor Sie mit der Registrierung eines neuen Servers beginnen.  
  
**Servername**  
Wählen Sie die Serverinstanz aus, mit der eine Verbindung hergestellt werden soll. Standardmäßig wird die Serverinstanz angezeigt, mit der zuletzt eine Verbindung bestanden hat.  
  
> [!NOTE]  
> Um eine Verbindung mit einer aktiven Benutzerinstanz von [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)] mit dem Named Pipes-Protokoll herzustellen, geben Sie den Pipenamen an, z.B. `np:\\.\pipe\3C3DF6B1-2262-47\tsql\query`. Weitere Informationen finden Sie in der Dokumentation zu [!INCLUDE[ssExpress](../../includes/ssexpress_md.md)].  

> [!NOTE]  
> Verbindungen werden in der Regel im Verlauf „Zuletzt verwendet“ (Most Recently Used, MRU) gespeichert. Wenn Sie Einträge aus dem MRU-Verlauf entfernen möchten, klicken Sie einfach auf das Kombinationsfeld **Servername**, wählen Sie den Namen des zu entfernenden Servers aus, und drücken Sie dann die **ENTF**-Taste.  

**Authentifizierung**  
Die aktuelle Version von SSMS stellt fünf verschiedene Authentifizierungsmodi beim Verbinden mit einer Instanz von [!INCLUDE[ssDE](../../includes/ssde_md.md)] bereit. Wenn das Authentifizierungsdialogfeld nicht mit der folgenden Liste übereinstimmt, laden Sie die aktuellste Version von SSMS unter [Herunterladen von SQL Server Management Studio (SSMS)](../download-sql-server-management-studio-ssms.md) herunter.  

> **Windows-Authentifizierung**  
> [!INCLUDE[msCoName](../../includes/msconame_md.md)] Der Windows Authentifizierungsmodus ermöglicht Benutzern die Verbindung über ein Windows-Benutzerkonto.  
> 
> **SQL Server-Authentifizierung**  
> Wenn ein Benutzer eine Verbindung mit einem angegebenen Benutzernamen und einem Kennwort von einer nicht vertrauenswürdigen Verbindung herstellt, führt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Authentifizierung durch, indem überprüft wird, ob ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldekonto eingerichtet wurde und ob das angegebene Kennwort mit dem zuvor aufgezeichneten übereinstimmt. Wenn kein Anmeldekonto in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eingerichtet wurde, schlägt die Authentifizierung fehl, und der Benutzer erhält eine Fehlermeldung. Verwenden Sie nach Möglichkeit die Windows-Authentifizierung oder die Active Directory-Kennwortauthentifizierung.  
> 
> **Azure Active Directory: universell mit MFA-Unterstützung**  
> Universelle Active Directory-Authentifizierung mit MFA ist ein interaktiver Workflow, in dem Azure Multi-Factor Authentication (MFA) unterstützt wird. Azure MFA bietet Schutz für den Zugriff auf Daten sowie Anwendungen und erfüllt gleichzeitig Benutzeranforderungen nach einem einfachen Anmeldevorgang. MFA stellt strenge Authentifizierung mit einigen einfachen Überprüfungsoptionen bereit (Telefonanruf, SMS, Smartcards mit PIN oder eine Benachrichtigung über eine mobile App), sodass Benutzer die von ihnen bevorzugte Methode auswählen können. Ist ein Benutzerkonto für MFA konfiguriert, ist für den interaktiven Authentifizierungswokflow eine zusätzliche Benutzerinteraktion über Popupdialogfelder, Verwenden von Smartcards usw. erforderlich. Ist ein Benutzerkonto für MFA konfiguriert, muss der Benutzer die Option für universelle Azure-Authentifizierung auswählen, um eine Verbindung herzustellen. Erfordert das Benutzerkonto keine MFA, kann der Benutzer weiterhin die beiden anderen Azure Active Directory-Authentifizierungsoptionen verwenden. Weitere Informationen finden Sie unter [SSMS-Unterstützung für Azure AD MFA mit SQL-Datenbank und Azure Synapse Analytics](/azure/azure-sql/database/authentication-mfa-ssms-overview). Falls nötig können Sie die Domäne ändern, die die Anmeldung authentifiziert, indem Sie auf **Optionen** und dann auf die Registerkarte **Verbindungseigenschaften** klicken und anschließend das Feld **AD-Domänenname oder Mandanten-ID** ausfüllen.  
> 
> **Azure Active Directory: Kennwort**  
> Die Azure Active Directory-Authentifizierung ist ein Mechanismus zum Herstellen einer Verbindung mit [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] unter Verwendung von Identitäten in Azure Active Directory (Azure AD).  Verwenden Sie diese Methode zum Herstellen von Verbindungen mit [!INCLUDE[ssSDS](../../includes/sssds-md.md)], wenn Sie bei Windows mit Anmeldeinformationen aus einer Domäne angemeldet sind, die nicht mit Azure verbunden ist, oder wenn die Azure AD-Authentifizierung mithilfe von Azure AD auf Basis der ursprünglichen oder der Clientdomäne verwendet wird. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](/azure/azure-sql/database/authentication-aad-overview).  
> 
> **Active Directory – Integriert**  
> Die Azure Active Directory-Authentifizierung ist ein Mechanismus zum Herstellen einer Verbindung mit [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] unter Verwendung von Identitäten in Azure Active Directory (Azure AD). Verwenden Sie die Methode zum Verbinden mit [!INCLUDE[ssSDS](../../includes/sssds-md.md)], wenn Sie bei Windows mit Ihren Azure AD-Anmeldeinformationen aus einer Verbunddomäne angemeldet sind. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank unter Verwendung der Azure Active Directory-Authentifizierung](/azure/azure-sql/database/authentication-aad-overview).  
  
**Benutzername**  
Der Windows-Benutzername zum Herstellen der Verbindung. Diese Option ist nur verfügbar, wenn Sie die Authentifizierung **Azure Active Directory: Kennwort** ausgewählt haben, um eine Verbindung herzustellen. Sie ist schreibgeschützt, wenn Sie die Authentifizierungen **Windows-Authentifizierung** oder **Azure Active Directory: integriert** auswählen.  
  
**Anmeldung**  
Geben Sie den Anmeldenamen für die Verbindung ein. Diese Option ist nur verfügbar, wenn Sie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung oder die Active Directory-Kennwortauthentifizierung ausgewählt haben, um eine Verbindung herzustellen.  
  
> [!NOTE]  
> Verbindungen werden in der Regel im Verlauf „Zuletzt verwendet“ (Most Recently Used, MRU) gespeichert. Wenn Sie Einträge aus dem MRU-Verlauf entfernen möchten, klicken Sie einfach auf das Kombinationsfeld **Servername**, wählen Sie den Namen des zu entfernenden Servers aus, und drücken Sie dann die **ENTF**-Taste. Dies wurde mit SSMS 18.5 eingeführt.

**Kennwort**  
Geben Sie das Kennwort für die Anmeldung ein. Diese Option ist nur bearbeitbar, wenn Sie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung oder die Active Directory-Kennwortauthentifizierung ausgewählt haben, um eine Verbindung herzustellen.  

**Herstellen einer Verbindung**  
Klicken Sie darauf, um eine Verbindung mit dem Server herzustellen.

**Optionen**  
Klicken Sie darauf, um die **Verbindungseigenschaften** und die Registerkarte **Zusätzliche Verbindungsparameter** anzuzeigen.