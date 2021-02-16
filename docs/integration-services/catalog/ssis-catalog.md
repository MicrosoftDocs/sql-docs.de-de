---
description: SSIS-Katalog
title: SSIS-Katalog | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/17/2020
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.iscreatecatalog.f1
- sql13.ssis.ssms.iscatalogprop.general.f1
- sql13.ssis.dbupgradewizard.f1
ms.assetid: 24bd987e-164a-48fd-b4f2-cbe16a3cd95e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fa8c9cb8d507acd5766e441cbd7387161a283bbf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339289"
---
# <a name="ssis-catalog"></a>SSIS-Katalog

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Der **SSISDB**-Katalog ist der zentrale Punkt zum Arbeiten mit [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)]-Projekten (SSIS), die Sie auf dem [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)]-Server bereitgestellt haben. Sie legen beispielsweise Projekt- und Paketparameter fest, konfigurieren Umgebungen, um Laufzeitwerte für Pakete anzugeben, führen Pakete aus, behandeln Paketprobleme und verwalten [!INCLUDE[ssISnoversion_md](../../includes/ssisnoversion-md.md)] -Servervorgänge.  
 
> [!NOTE]
> In diesem Artikel wird der SSIS-Katalog allgemein beschrieben, und es wird auf einen lokalen SSIS-Katalog im Speziellen eingegangen. Sie können auch einen SSIS-Katalog in Azure SQL-Datenbank erstellen und SSIS-Pakete in Azure bereitstellen. Weitere Informationen finden Sie unter [Migration von SQL Server Integration Services-Workloads in die Cloud per Lift und Shift](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).
>
> Sie können SSIS-Pakete zwar unter Linux ausführen, der SSIS-Katalog wird allerdings nicht unter Linux unterstützt. Weitere Informationen finden Sie unter [Extrahieren, Transformieren und Laden von Daten unter Linux mit SSIS](../../linux/sql-server-linux-migrate-ssis.md).
 
 Die Objekte, die im **SSISDB** -Katalog gespeichert werden, umfassen Projekte, Pakete, Parameter, Umgebungen und Verwendungsverläufe.  
  
 Sie können Sichten in der **SSISDB** -Datenbank abfragen, um im **SSISDB** -Katalog gespeicherte Objekte, Einstellungen und operative Daten zu überprüfen. Sie verwalten die Objekte, indem Sie gespeicherte Prozeduren in der **SSISDB** -Datenbank oder über die Benutzeroberfläche des **SSISDB** -Katalogs aufrufen. In vielen Fällen kann der gleiche Task in der Benutzeroberfläche oder durch das Aufrufen einer gespeicherten Prozedur ausgeführt werden.  
  
 Zur Verwaltung der Datenbank **SSISDB** wird empfohlen, Standardunternehmensrichtlinien für die Verwaltung von Benutzerdatenbanken anzuwenden. Informationen zum Erstellen von Wartungsplänen finden Sie unter [Maintenance Plans](../../relational-databases/maintenance-plans/maintenance-plans.md).  
  
 Der **SSISDB** -Katalog und die **SSISDB** -Datenbank unterstützen Windows PowerShell. Weitere Informationen zum Verwenden von SQL Server mit Windows PowerShell finden Sie unter [SQL Server PowerShell](../../powershell/sql-server-powershell.md). Beispiele zur Verwendung von Windows PowerShell zum Abschließen von Tasks, z. B. zum Bereitstellen eines Projekts, finden Sie im Blogeintrag zu [SSIS und PowerShell in SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-and-powershell-in-sql-server-2012/ba-p/388015)auf blogs.msdn.com.  
  
 Weitere Informationen zum Anzeigen von Vorgangsdaten finden Sie unter [Ausführen von Paketen und andere Vorgänge überwachen](../../integration-services/performance/monitor-running-packages-and-other-operations.md).  
  
 Sie können den **SSISDB** -Katalog in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] aufrufen, indem Sie eine Verbindung zur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank-Engine herstellen und dann den Knoten **Integration Services-Kataloge** im Objekt-Explorer erweitern. In **greifen Sie auf die SSISDB-Datenbank zu, indem Sie den Knoten** Datenbanken [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] im Objekt-Explorer erweitern.  
  
> [!NOTE]
> Sie können die **SSISDB** -Datenbank nicht umbenennen.  
  
> [!NOTE]
> Wenn die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz, an die die **SSISDB** -Datenbank angefügt wurde, beendet wird oder nicht reagiert, wird der Prozess ISServerExec.exe beendet. Eine Meldung wird in ein Windows-Ereignisprotokoll geschrieben.  
>   
>  Wenn für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ressourcen ein Failover als Teil eines Clusterfailovers durchgeführt wird, werden die ausgeführten Pakete nicht neu gestartet. Sie können Prüfpunkte verwenden, um Pakete neu zu starten. Weitere Informationen finden Sie unter [Neustarten von Paketen mit Prüfpunkten](../../integration-services/packages/restart-packages-by-using-checkpoints.md).  
  
## <a name="features-and-capabilities"></a>Features und Funktionen  
  
-   [Katalogobjektbezeichner](../../integration-services/catalog/ssis-catalog.md#CatalogObjectIdentifiers)  
  
-   [Katalogkonfiguration](../../integration-services/catalog/ssis-catalog.md#Configuration)  
  
-   [Berechtigungen](../../integration-services/catalog/ssis-catalog.md#Permissions)  
  
-   [Ordner](../../integration-services/catalog/ssis-catalog.md#Folders)  
  
-   [Projekte und Pakete](../../integration-services/catalog/ssis-catalog.md#ProjectsAndPackages)  
  
-   [Parameter](../../integration-services/catalog/ssis-catalog.md#Parameters)  
  
-   [Serverumgebungen, Servervariablen und Serverumgebungsverweise](../../integration-services/catalog/ssis-catalog.md#ServerEnvironments)  
  
-   [Ausführungen und Überprüfungen](../../integration-services/catalog/ssis-catalog.md#Executions)  

##  <a name="catalog-object-identifiers"></a><a name="CatalogObjectIdentifiers"></a> Katalogobjektbezeichner  
 Wenn Sie im Katalog ein neues Objekt erstellen, weisen Sie dem Objekt einen Namen zu. Der Objektname ist der Bezeichner. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bestimmt Regeln, welche Zeichen in einem Bezeichner verwendet werden können. Namen für die folgenden Objekte müssen den Regeln für Bezeichner entsprechen.  
  
-   Ordner  
  
-   Project  
  
-   Environment  
  
-   Parameter  
  
-   Umgebungsvariable  
  
###  <a name="folder-project-environment"></a><a name="Folder"></a> Ordner, Projekt, Umgebung  
 Beachten Sie die folgenden Regeln, wenn Sie einen Ordner, ein Projekt oder eine Umgebung umbenennen.  
  
-   Zu ungültige Zeichen zählen die ASCII/Unicode-Zeichen 1 bis 31, Anführungszeichen ("), kleiner als (\<), greater than (>), senkrechter Strich (|), Rücktaste (\b), null (\0) und Tab (\t).  
  
-   Der Name darf keine führenden oder nachgestellten Leerzeichen enthalten.  
  
-   \@ ist nicht als erstes Zeichen zulässig. Allerdings kann \@ für nachfolgende Zeichen verwendet werden.  
  
-   Die Länge des Namens muss größer als oder gleich 0 und kleiner als oder gleich 128 sein.  
  
###  <a name="parameter"></a><a name="Parameter"></a> Parameter  
 Beachten Sie die folgenden Regeln, wenn Sie einen Parameter benennen.  
  
-   Das erste Zeichen des Namens muss ein Buchstabe gemäß Unicode-Standard 2.0 oder ein Unterstrich (_) sein.  
  
-   Bei nachfolgenden Zeichen kann es sich um Buchstaben oder Zahlen gemäß Unicode-Standard 2.0 oder um einen Unterstrich (_) handeln.  
  
###  <a name="environment-variable"></a><a name="EnvironmentVariable"></a> Umgebungsvariable  
 Beachten Sie die folgenden Regeln, wenn Sie eine Umgebungsvariable benennen.  
  
-   Zu ungültige Zeichen zählen die ASCII/Unicode-Zeichen 1 bis 31, Anführungszeichen ("), kleiner als (\<), greater than (>), senkrechter Strich (|), Rücktaste (\b), null (\0) und Tab (\t).  
  
-   Der Name darf keine führenden oder nachgestellten Leerzeichen enthalten.  
  
-   \@ ist nicht als erstes Zeichen zulässig. Allerdings kann \@ für nachfolgende Zeichen verwendet werden.  
  
-   Die Länge des Namens muss größer als oder gleich 0 und kleiner als oder gleich 128 sein.  
  
-   Das erste Zeichen des Namens muss ein Buchstabe gemäß Unicode-Standard 2.0 oder ein Unterstrich (_) sein.  
  
-   Bei nachfolgenden Zeichen kann es sich um Buchstaben oder Zahlen gemäß Unicode-Standard 2.0 oder um einen Unterstrich (_) handeln.  
  
##  <a name="catalog-configuration"></a><a name="Configuration"></a> Katalogkonfiguration  
 Sie können anpassen, wie sich der Katalog verhält, indem Sie die Katalogeigenschaften einstellen. Die Katalogeigenschaften definieren, wie sensible Daten verschlüsselt werden und wie Vorgänge und Versionsdaten für Projekte beibehalten werden. Verwenden Sie das Dialogfeld **Katalogeigenschaften**, oder rufen Sie die gespeicherte [catalog.configure_catalog &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md)-Prozedur auf, um Katalogeigenschaften festzulegen. Verwenden Sie zum Anzeigen der Eigenschaft das Dialogfeld, oder fragen Sie [catalog.catalog_properties &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md) ab. Sie greifen auf das Dialogfeld zu, indem Sie im Objekt-Explorer mit der rechten Maustaste auf **SSISDB** klicken.  
  
###  <a name="operations-and-project-version-cleanup"></a><a name="Cleanup"></a> Bereinigung von Projektversionen und Vorgängen  
 Statusdaten für viele der Vorgänge im Katalog werden in internen Datenbanktabellen gespeichert. Beispielsweise wird im Katalog der Status von Paketausführungen und Projektbereitstellungen nachverfolgt. Um die Größe der Vorgangsdaten zu verwalten, wird der **SSIS-Server-Wartungsauftrag** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] verwendet, um alte Daten zu entfernen. Dieser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agentauftrag wird bei der Installation von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] erstellt.  
  
 Sie können ein [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Projekt durch das Bereitstellen unter dem gleichen Namen im gleichen Ordner im Katalog aktualisieren oder erneut bereitstellen. Jedes Mal, wenn Sie ein Projekt erneut bereitstellen, behält der **SSISDB** -Katalog standardmäßig die frühere Version des Projekts bei. Um die Größe der Vorgangsdaten zu verwalten, wird der **SSIS-Server-Wartungsauftrag** verwendet, um alte Versionen von Projekten zu entfernen.  
 
Zum Ausführen des **SSIS-Serverwartungsauftrags** erstellt SSIS die SQL Server-Anmeldung **##MS_SSISServerCleanupJobLogin##** . Diese Anmeldung gilt nur für den internen Gebrauch durch SSIS.
  
 Die folgenden **SSISDB** -Katalogeigenschaften definieren, wie sich dieser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agentauftrag verhält. Sie können die Eigenschaften anzeigen oder ändern, indem Sie das Dialogfeld **Katalogeigenschaften** verwenden, oder [catalog.catalog_properties &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md) und [catalog.configure_catalog &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md).  
  
 **Protokolle regelmäßig bereinigen**  
 Der Auftragsschritt für die Vorgangsbereinigung wird ausgeführt, wenn diese Eigenschaft auf **TRUE** festgelegt ist.  
  
 **Beibehaltungsdauer (Tage)**  
 Definiert das maximale Alter von zulässigen Vorgangsdaten (in Tagen). Ältere Daten werden entfernt.  
  
 Der Mindestwert ist ein Tag. Der Höchstwert wird nur durch den Höchstwert für Daten vom Typ **int** in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] beschränkt. Informationen zu diesem Datentyp finden Sie unter [int, bigint, smallint und tinyint &#40;Transact-SQL&#41;](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md).  
  
 **Alte Versionen regelmäßig entfernen**  
 Der Auftragsschritt für die Projektversionsbereinigung wird ausgeführt, wenn diese Eigenschaft auf **TRUE** festgelegt ist.  
  
 **Maximale Anzahl der Versionen pro Projekt**  
 Definiert, wie viele Versionen eines Projekts im Katalog gespeichert sind. Frühere Versionen von Projekten werden entfernt.  
  
###  <a name="encryption-algorithm"></a><a name="Encryption"></a> Verschlüsselungsalgorithmus  
 Die Eigenschaft **Verschlüsselungsalgorithmus** gibt den Verschlüsselungstyp an, der zur Verschlüsselung der sensiblen Parameterwerte verwendet wird. Sie haben die Wahl zwischen den folgenden Verschlüsselungstypen:  
  
-   AES_256 (Standard)  
  
-   AES_192  
  
-   AES_128  
  
-   DESX  
  
-   TRIPLE_DES_3KEY  
  
-   TRIPLE_DES  
  
-   DES  
  
 Wenn Sie ein [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Projekt auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server bereitstellen, werden die Paketdaten und sensible Werte automatisch vom Katalog verschlüsselt. Die Daten werden vom Katalog auch automatisch entschlüsselt, wenn Sie sie abrufen. Der SSISDB-Katalog verwendet die Schutzebene **ServerStorage** . Weitere Informationen finden Sie unter [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
 Eine Änderung des Verschlüsselungsalgorithmus ist sehr aufwändig. Der Server muss zuerst alle Konfigurationswerte mithilfe des zuvor angegebenen Algorithmus entschlüsseln. Anschließend müssen die Werte vom Server mithilfe des neuen Algorithmus wieder verschlüsselt werden. In diesem Zeitraum können keine anderen [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Vorgänge auf dem Server ausgeführt werden. Damit [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Vorgänge weiterhin ohne Unterbrechung ausgeführt werden können, ist der Wert für den Verschlüsselungsalgorithmus im [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]-Dialogfeld schreibgeschützt.  
  
 Um die Eigenschafteneinstellung **Verschlüsselungsalgorithmus** zu ändern, legen Sie die **SSISDB** -Datenbank auf den Einzelbenutzermodus fest und rufen dann die gespeicherte Prozedur „catalog.configure_catalog“ auf. Verwenden Sie ENCRYPTION_ALGORITHM für das *property_name* -Argument. Die unterstützten Eigenschaftswerte finden Sie unter [catalog.catalog_properties &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md). Weitere Informationen zur gespeicherten Prozedur finden Sie unter [catalog.configure_catalog &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md).  
  
 Weitere Informationen zum Einzelbenutzermodus finden Sie unter [Festlegen des Einzelbenutzermodus für eine Datenbank](../../relational-databases/databases/set-a-database-to-single-user-mode.md). Informationen zur Verschlüsselung sowie zu Algorithmen für die Verschlüsselung in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]finden Sie in den Themen im Abschnitt [SQL Server-Verschlüsselung](../../relational-databases/security/encryption/sql-server-encryption.md).  
  
 Ein Datenbank-Hauptschlüssel wird für die Verschlüsselung verwendet. Der Schlüssel wird erstellt, wenn Sie den Katalog erstellen.  
  
 In der folgenden Tabelle sind die im Dialogfeld **Katalogeigenschaften** aufgeführten Eigenschaftsnamen und die zugehörigen Eigenschaften in der Datenbanksicht aufgelistet.  
  
|Eigenschaftsname (Dialogfeld **Katalogeigenschaften** )|Eigenschaftsname (Datenbanksicht)|  
|---------------------------------------------------------|-------------------------------------|  
|Name des Verschlüsselungsalgorithmus|ENCRYPTION_ALGORITHM|  
|Protokolle regelmäßig bereinigen|OPERATION_CLEANUP_ENABLEDâ€‹|  
|Beibehaltungsdauer (Tage)|RETENTION_WINDOW|  
|Alte Versionen regelmäßig entfernen|VERSION_CLEANUP_ENABLED|  
|Maximale Anzahl der Versionen pro Projekt|MAX_PROJECT_VERSIONS|  
|Serverweiter Standardprotokolliergrad|SERVER_LOGGING_LEVEL|  
  
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Projekte, Umgebungen und Pakete sind in Ordnern enthalten, bei denen es sich um sicherungsfähige Objekte handelt. Sie können einem Ordner Berechtigungen gewähren, einschließlich der MANAGE_OBJECT_PERMISSIONS-Berechtigung. MANAGE_OBJECT_PERMISSIONS ermöglicht es Ihnen, die Verwaltung von Ordnerinhalten an einen Benutzer zu delegieren, ohne dass Sie dem Benutzer die Mitgliedschaft in der ssis_admin-Rolle erteilen müssen. Sie haben auch die Möglichkeit, Projekten, Umgebungen und Vorgängen Berechtigungen zu erteilen. Vorgänge schließen das Initialisieren von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], das Bereitstellen von Projekten, das Erstellen und Starten von Ausführungen, das Überprüfen von Projekten und Paketen und das Konfigurieren des **SSISDB** -Katalogs ein.  
  
 Weitere Informationen zu Datenbankrollen finden Sie unter [Rollen auf Datenbankebene](../../relational-databases/security/authentication-access/database-level-roles.md).  
  
 Der SSISDB-Katalog erzwingt die Integrität der Berechtigungsinformationen für SSIS-sicherungsfähige Elemente mithilfe eines DDL-Triggers, ddl_cleanup_object_permissions. Der Trigger wird ausgelöst, wenn ein Datenbankprinzipal, z. B. ein Datenbankbenutzer, eine Datenbankrolle oder eine Datenbankanwendungsrolle, aus der SSISDB-Datenbank entfernt wird.  
  
 Wenn der Prinzipal anderen Prinzipalen Berechtigungen erteilt oder verweigert hat, widerrufen Sie die vom Berechtigenden erteilten Berechtigungen, bevor der Prinzipal entfernt werden kann. Andernfalls wird eine Fehlermeldung zurückgegeben, wenn das System versucht, den Prinzipal zu entfernen. Der Trigger entfernt alle Berechtigungsdatensätze, bei denen der Datenbankprinzipal ein Empfänger ist.  
  
 Es wird empfohlen, den Trigger nicht zu deaktivieren, da er sicherstellt, dass keine verwaisten Berechtigungsdatensätze vorhanden sind, nachdem ein Datenbankprinzipal aus der **SSISDB** -Datenbank gelöscht wurde.  
  
### <a name="managing-permissions"></a>Verwalten von Berechtigungen  
 Sie können Berechtigungen mit der [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] -Benutzeroberfläche, gespeicherten Prozeduren und dem <xref:Microsoft.SqlServer.Management.IntegrationServices> -Namespace verwalten.  
  
 Verwenden Sie die folgenden Dialogfelder, um Berechtigungen mithilfe der Benutzeroberfläche von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] zu verwalten: 
  
-   Verwenden Sie für einen Ordner die Seite **Berechtigungen** im [Folder Properties Dialog Box](../../integration-services/catalog/folder-properties-dialog-box.md).  
  
-   Verwenden Sie für ein Projekt die Seite **Berechtigungen** im [Project Properties Dialog Box](../../integration-services/catalog/project-properties-dialog-box.md).  

 Rufen Sie zum Verwalten von Berechtigungen mithilfe von Transact-SQL [catalog.grant_permission &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-grant-permission-ssisdb-database.md), [catalog.deny_permission &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-deny-permission-ssisdb-database.md) und [catalog.revoke_permission &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-revoke-permission-ssisdb-database.md) auf. Fragen Sie [catalog.effective_object_permissions &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-effective-object-permissions-ssisdb-database.md) ab, um effektive Berechtigungen für das aktuelle Prinzipal für alle Objekte anzuzeigen. Dieses Thema enthält Beschreibungen der verschiedenen Berechtigungstypen. Fragen Sie [catalog.explicit_object_permissions &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-explicit-object-permissions-ssisdb-database.md) ab, um Berechtigungen anzuzeigen, die explizit dem Benutzer zugewiesen wurden.  
  
##  <a name="folders"></a><a name="Folders"></a> Ordner  
 Ein Ordner enthält ein oder mehrere Projekte und Umgebungen im **SSISDB** -Katalog. Sie können die Ansicht [catalog.folders &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-folders-ssisdb-database.md) verwenden, um auf Informationen über Ordner im Katalog zuzugreifen. Sie können folgende gespeicherte Prozeduren zum Verwalten von Ordnern verwenden:  
  
-   [catalog.create_folder &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-create-folder-ssisdb-database.md)  
  
-   [catalog.delete_folder &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-delete-folder-ssisdb-database.md)  
  
-   [catalog.rename_folder &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-rename-folder-ssisdb-database.md)  
  
-   [catalog.set_folder_description &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-set-folder-description-ssisdb-database.md)  
  
##  <a name="projects-and-packages"></a><a name="ProjectsAndPackages"></a> Projekte und Pakete  
 Jedes Projekt kann mehrere Pakete enthalten. Sowohl Projekte als auch Pakete können Parameter und Umgebungsverweise enthalten. Sie können über das [Configure Dialog Box](../../integration-services/catalog/configure-dialog-box.md)auf die Parameter und die Umgebungsverweise zugreifen.  
  
 Sie können weitere Projekttasks ausführen, indem Sie die folgenden gespeicherten Prozeduren aufrufen: 
  
-   [catalog.delete_project &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-delete-project-ssisdb-database.md)  
  
-   [catalog.deploy_project &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-deploy-project-ssisdb-database.md)  
  
-   [catalog.get_project &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-get-project-ssisdb-database.md)  
  
-   [catalog.move_project &#40;&#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-move-project-ssisdb-database.md)  
  
-   [catalog.restore_project &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-restore-project-ssisdb-database.md)  
  
 Diese Sichten enthalten Details zu Paketen, Projekten und Projektversionen.  
  
-   [catalog.projects &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-projects-ssisdb-database.md)  
  
-   [catalog.packages &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-packages-ssisdb-database.md)  
  
-   [catalog.object_versions &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-object-versions-ssisdb-database.md)  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parameter  
 Mit Parametern können Sie Paketeigenschaften zum Zeitpunkt der Paketausführung Werte zuweisen. Rufen Sie [catalog.set_object_parameter_value &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-set-object-parameter-value-ssisdb-database.md) und [catalog.clear_object_parameter_value &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-clear-object-parameter-value-ssisdb-database.md) auf, um den Wert des Paket- oder Projektparameters festzulegen und den Wert zu löschen. Rufen Sie zum Festlegen des Wert eines Parameters für eine Instanz der Ausführung [catalog.set_execution_parameter_value &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md) auf. Sie können Standardparameterwerte abrufen, indem Sie [catalog.get_parameter_values &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-get-parameter-values-ssisdb-database.md) aufrufen.  
  
 Diese Sichten zeigen die Parameter für alle Pakete und Projekte sowie Parameterwerte, die für eine Instanz der Ausführung verwendet werden, an.  
  
-   [catalog.object_parameters &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-object-parameters-ssisdb-database.md)  
  
-   [catalog.execution_parameter_values &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-execution-parameter-values-ssisdb-database.md)  
  
##  <a name="server-environments-server-variables-and-server-environment-references"></a><a name="ServerEnvironments"></a> Serverumgebungen, Servervariablen und Serverumgebungsverweise  
 Serverumgebungen enthalten Servervariablen. Die Variablenwerte können verwendet werden, wenn ein Paket ausgeführt wird oder auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server überprüft wird.  
  
 Die folgenden gespeicherten Prozeduren ermöglichen es Ihnen, zahlreiche weitere Verwaltungstasks für Umgebungen und Variablen auszuführen.  
  
-   [catalog.create_environment &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-create-environment-ssisdb-database.md)  
  
-   [catalog.delete_environment &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-delete-environment-ssisdb-database.md)  
  
-   [catalog.move_environment &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-move-environment-ssisdb-database.md)  
  
-   [catalog.rename_environment &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-rename-environment-ssisdb-database.md)  
  
-   [catalog.set_environment_property &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-property-ssisdb-database.md)  
  
-   [catalog.create_environment_variable &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-create-environment-variable-ssisdb-database.md)  
  
-   [catalog.delete_environment_variable &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-delete-environment-variable-ssisdb-database.md)  
  
-   [catalog.set_environment_variable_property &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-variable-property-ssisdb-database.md)  
  
-   [catalog.set_environment_variable_value &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-variable-value-ssisdb-database.md)  
  
 Durch Aufrufen der gespeicherten Prozedur [catalog.set_environment_variable_protection &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-variable-protection-ssisdb-database.md) können Sie das Vertraulichkeitsbit für eine Variable festlegen.  
  
 Um den Wert einer Servervariablen zu verwenden, geben Sie den Verweis zwischen dem Projekt und der Serverumgebung an. Sie können die folgenden gespeicherten Prozeduren verwenden, um Verweise zu erstellen und zu löschen. Sie können auch angeben, ob sich die Umgebung im selben Ordner wie das Projekt oder in einem anderen Ordner befinden kann.  
  
-   [catalog.create_environment_reference &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-create-environment-reference-ssisdb-database.md)  
  
-   [catalog.delete_environment_reference &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-delete-environment-reference-ssisdb-database.md)  
  
-   [catalog.set_environment_reference_type &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-set-environment-reference-type-ssisdb-database.md)  
  
 Weitere Details zu Umgebungen und Variablen können Sie aus diesen Sichten abfragen.  
  
-   [catalog.environments &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-environments-ssisdb-database.md)  
  
-   [catalog.environment_variables &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-environment-variables-ssisdb-database.md)  
  
-   [catalog.environment_references &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-environment-references-ssisdb-database.md)  
  
##  <a name="executions-and-validations"></a><a name="Executions"></a> Ausführungen und Überprüfungen  
 Eine Ausführung ist eine Instanz einer Paketausführung. Rufen Sie [catalog.create_execution &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md) und [catalog.start_execution &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md) auf, um eine Ausführung zu erstellen und zu starten. Rufen Sie [catalog.stop_operation &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-stop-operation-ssisdb-database.md) auf, um eine Ausführung oder eine Paket-/Projektüberprüfung zu beenden.  
  
 Rufen Sie die gespeicherte Prozedur catalog.create_execution_dump auf, um ein ausgeführtes Paket anzuhalten und eine Dumpdatei zu erstellen. Eine Dumpdatei enthält Informationen zur Ausführung eines Pakets, die Ihnen helfen können, Ausführungsprobleme zu beheben. Weitere Informationen zum Erstellen und Konfigurieren von Dumpdateien finden Sie unter [Generating Dump Files for Package Execution](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md).  
  
 Details zu Ausführungen, Überprüfungen, Meldungen, die während der Vorgänge protokolliert werden, und Kontextinformationen zu Fehlern können Sie aus diesen Sichten abfragen.  
  
-   [catalog.executions &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md)  
  
-   [catalog.operations &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md)  
  
-   [catalog.operation_messages &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md)  
  
-   [catalog.extended_operation_info &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-extended-operation-info-ssisdb-database.md)  
  
-   [catalog.event_messages](../../integration-services/system-views/catalog-event-messages.md)  
  
-   [catalog.event_message_context](../../integration-services/system-views/catalog-event-message-context.md)  
  
 Sie können Projekte und Pakete überprüfen, indem Sie die gespeicherten Prozeduren [catalog.validate_project &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-validate-project-ssisdb-database.md) und [catalog.validate_package &#40;SSISDB Database&#41;](../../integration-services/system-stored-procedures/catalog-validate-package-ssisdb-database.md) aufrufen. Die [catalog.validations &#40;SSISDB Database&#41;](../../integration-services/system-views/catalog-validations-ssisdb-database.md)-Sicht enthält Details zu Überprüfungen, z.B. die Serverumgebungsverweise, die in der Überprüfung berücksichtigt werden, ob es eine Abhängigkeitsüberprüfung oder eine vollständige Überprüfung ist, und ob die 32-Bit-Laufzeit oder die 64-Bit-Laufzeit verwendet wird, um das Paket auszuführen.  

## <a name="create-the-ssis-catalog"></a>Erstellen des SSIS-Katalogs
  Nachdem Sie Pakete in [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]entworfen und getestet haben, können Sie die Projekte, die die Pakete enthalten, auf einem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server bereitstellen. Bevor Sie die Projekte auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server bereitstellen können, muss der **SSISDB** -Katalog auf dem Server vorhanden sein. Der Katalog wird vom Installationsprogramm für [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] nicht automatisch erstellt, Sie müssen den Katalog nach folgenden Anweisungen manuell erstellen.  
  
 Sie können den SSISDB-Katalog in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]erstellen. Sie können den Katalog auch programmgesteuert mit Windows PowerShell erstellen.  
  
### <a name="to-create-the-ssisdb-catalog-in-sql-server-management-studio"></a>So erstellen Sie den SSISDB-Katalog in SQL Server Management Studio  
  
1.  Öffnen Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Stellen Sie eine Verbindung mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank-Engine her.  
  
3.  Erweitern Sie im Objekt-Explorer den Serverknoten, klicken Sie mit der rechten Maustaste auf den Knoten **Integration Services-Kataloge** , und klicken Sie dann auf **Katalog erstellen**.  
  
4.  Klicken Sie auf **CLR-Integration aktivieren**.  
  
     Für den Katalog werden gespeicherte CLR-Prozeduren verwendet.  
  
5.  Klicken Sie auf **Automatische Ausführung gespeicherter Integration Services-Prozeduren beim Starten von SQL Server aktivieren** , um die gespeicherte Prozedur [catalog.startup](../../integration-services/system-stored-procedures/catalog-startup.md) jedes Mal ausführen zu lassen, wenn die [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Serverinstanz neu gestartet wird.  
  
     Durch die gespeicherte Prozedur wird der Status von Vorgängen für den SSISDB-Katalog verwaltet. Dabei wird der Status aller Pakete korrigiert, die während des Ausfalls der [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Serverinstanz ausgeführt wurden.  
  
6.  Geben Sie ein Kennwort ein, und klicken Sie dann auf **OK**.  
  
     Das Kennwort schützt den Datenbank-Hauptschlüssel, der zum Verschlüsseln der Katalogdaten verwendet wird. Bewahren Sie das Kennwort sicher auf. Es wird empfohlen, auch den Datenbank-Hauptschlüssel zu sichern. Weitere Informationen finden Sie unter [Back Up a Database Master Key](../../relational-databases/security/encryption/back-up-a-database-master-key.md).  
  
### <a name="to-create-the-ssisdb-catalog-programmatically"></a>So erstellen Sie den SSISDB-Katalog programmgesteuert  
  
1.  Führen Sie das folgende PowerShell-Skript aus:  
  
    ```  
    # Load the IntegrationServices Assembly  
    [Reflection.Assembly]::LoadWithPartialName("Microsoft.SqlServer.Management.IntegrationServices")  
  
    # Store the IntegrationServices Assembly namespace to avoid typing it every time  
    $ISNamespace = "Microsoft.SqlServer.Management.IntegrationServices"  
  
    Write-Host "Connecting to server ..."  
  
    # Create a connection to the server  
    $sqlConnectionString = "Data Source=localhost;Initial Catalog=master;Integrated Security=SSPI;"  
    $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $sqlConnectionString  
  
    # Create the Integration Services object  
    $integrationServices = New-Object $ISNamespace".IntegrationServices" $sqlConnection  
  
    # Provision a new SSIS Catalog  
    $catalog = New-Object $ISNamespace".Catalog" ($integrationServices, "SSISDB", "P@assword1")  
    $catalog.Create()  
  
    ```  
  
     Weitere Beispiele zum Verwenden von Windows PowerShell und des <xref:Microsoft.SqlServer.Management.IntegrationServices>-Namespaces finden Sie auf blogs.msdn.com im Blogeintrag [SSIS and PowerShell in SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-and-powershell-in-sql-server-2012/ba-p/388015) (SSIS und PowerShell in SQL Server 2012). Eine Übersicht über den Namespace und Codebeispiele finden Sie im Blogeintrag [A Glimpse of the SSIS Catalog Managed Object Model](https://techcommunity.microsoft.com/t5/sql-server-integration-services/a-glimpse-of-the-ssis-catalog-managed-object-model/ba-p/387892)(Übersicht über das SSIS-Katalogmodell verwalteter Objekte) auf „blogs.msdn.com“.  

## <a name="catalog-properties-dialog-box"></a>Katalogeigenschaften (Dialogfeld)
  Verwenden Sie das Dialogfeld Katalogeigenschaften, um den SSISDB-Katalog zu konfigurieren. Die Katalogeigenschaften definieren, wie sensible Daten verschlüsselt werden, wie Vorgänge und Versionsdaten für Projekte beibehalten werden und zu welchem Zeitpunkt für Überprüfungsvorgänge ein Timeout erfolgt. Der SSISDB-Katalog ist ein zentraler Speicher- und Verwaltungspunkt für Projekte, Pakete, Parameter und Umgebungen von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Sie können Katalogeigenschaften auch in der `catalog.catalog_properties`-Ansicht anzeigen und die Eigenschaften mit der gespeicherten `catalog.configure_catalog`-Prozedur festlegen. Weitere Informationen finden Sie unter [catalog.catalog_properties &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md) und [catalog.configure_catalog &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-configure-catalog-ssisdb-database.md).  
  
 **Was möchten Sie tun?**  
  
-   [Öffnen des Dialogfelds "Katalogeigenschaften"](#open_dialog)  
  
-   [Konfigurieren der Optionen](#options)  
  
###  <a name="open-the-catalog-properties-dialog-box"></a><a name="open_dialog"></a> Öffnen des Dialogfelds "Katalogeigenschaften"  
  
1.  Öffnen Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
2.  Stellen Sie eine Verbindung mit einer Microsoft SQL Server-Datenbank-Engine her.  
  
3.  Erweitern Sie im Objekt-Explorer den Knoten **Integration Services** , klicken Sie mit der rechten Maustaste auf den Knoten **SSISDB**, und klicken Sie anschließend auf **Eigenschaften**.  
  
###  <a name="configure-the-options"></a><a name="options"></a> Konfigurieren der Optionen  
  
#### <a name="options"></a>Tastatur  
 In der folgenden Tabelle werden spezifische Eigenschaften im Dialogfeld und die entsprechenden Eigenschaften in der `catalog.catalog_properties`-Ansicht beschrieben.  
  
|Eigenschaftsname (Dialogfeld Katalogeigenschaften)|Eigenschaftsname (catalog.catalog_properties-Ansicht)|BESCHREIBUNG|  
|-----------------------------------------------------|------------------------------------------------------|-----------------|  
|Name des Verschlüsselungsalgorithmus|ENCRYPTION_ALGORITHM|Gibt den Verschlüsselungstyp an, der zur Verschlüsselung der sensiblen Parameterwerte im Katalog verwendet wird. Folgende Werte sind möglich:<br /><br /> DES<br /><br /> TRIPLE_DES<br /><br /> TRIPLE_DES_3KEY<br /><br /> DESPX<br /><br /> AES_128<br /><br /> AES_192<br /><br /> AES_256 (Standard)|  
|Maximale Anzahl der Versionen pro Projekt|MAX_PROJECT_VERSIONS|Gibt an, wie viele Versionen eines Projekts im Katalog gespeichert sind. Wenn die maximale Anzahl überschritten wird, werden frühere Versionen von Projekten bei der Projektversionsbereinigung entfernt.|  
|Protokolle regelmäßig bereinigen|OPERATION_CLEANUP_ENABLED|Legen Sie die Eigenschaft auf "True" fest, um anzugeben, dass der SQL Server-Agentauftrag, die Vorgangsbereinigung, ausgeführt wird. Legen Sie die Eigenschaft andernfalls auf "False" fest.|  
|Beibehaltungsdauer (Tage)|RETENTION_WINDOW|Geben Sie das maximale Alter von zulässigen Vorgangsdaten (in Tagen) an. Daten, die älter als die angegebene Anzahl von Tagen sind, werden vom SQL-Agent-Auftrag durch die Vorgangsbereinigung entfernt.|  

## <a name="back-up-restore-and-move-the-ssis-catalog"></a>Sichern, Wiederherstellen und Verschieben des SSIS-Katalogs
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] schließt die SSISDB-Datenbank ein. Sie können Sichten in der SSISDB-Datenbank abfragen, um im **SSISDB** -Katalog gespeicherte Objekte, Einstellungen und operative Daten zu überprüfen. Dieses Thema enthält Anweisungen zum Sichern und Wiederherstellen der Datenbank.  
  
 Der **SSISDB**-Katalog speichert die Pakete, die Sie auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Server bereitgestellt haben. Weitere Informationen zum Katalog finden Sie unter [SSIS-Katalog](../../integration-services/catalog/ssis-catalog.md).  
  
###  <a name="to-back-up-the-ssis-database"></a><a name="backup"></a> So sichern Sie die SSIS-Datenbank  
  
1.  Öffnen Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , und stellen Sie eine Verbindung zu einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]her.  
  
2.  Sichern Sie den Hauptschlüssel für die SSISDB-Datenbank, indem Sie die Anweisung BACKUP MASTER KEY (Transact-SQL) verwenden. Der Schlüssel wird in einer von Ihnen angegebenen Datei gespeichert. Verwenden Sie ein Kennwort, um den Hauptschlüssel in der Datei zu verschlüsseln.  
  
     Weitere Informationen zur Anweisung finden Sie unter [BACKUP MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/backup-master-key-transact-sql.md).  
  
     Im folgenden Beispiel wird der Hauptschlüssel in die Datei `c:\temp directory\RCTestInstKey` exportiert. Das `LS2Setup!` -Kennwort wird zum Verschlüsseln des Hauptschlüssels verwendet.  
  
    ```  
    backup master key to file = 'c:\temp\RCTestInstKey'  
           encryption by password = 'LS2Setup!'  
  
    ```  
  
3.  Sichern Sie die SSISDB-Datenbank mit dem Dialogfeld **Datenbank sichern** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Weitere Informationen finden Sie unter [Vorgehensweise: Sichern einer Datenbank (SQL Server Management Studio)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
4.  Gehen Sie wie folgt vor, um das CREATE LOGIN-Skript für ##MS_SSISServerCleanupJobLogin## zu generieren. Weitere Informationen finden Sie unter [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md).  
  
    1.  Erweitern Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]im Objekt-Explorer die Knoten **Sicherheit** und **Anmeldungen** .  
  
    2.  Klicken Sie mit der rechten Maustaste auf **##MS_SSISServerCleanupJobLogin##** , und klicken Sie dann auf **Skript für Anmeldenamen als** > **CREATE in** > **Neues Abfrage-Editor-Fenster**.  
  
5.  Wenn Sie die SSISDB-Datenbank auf einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz wiederherstellen, auf der der SSISDB-Katalog nie erstellt wurde, generieren Sie das CREATE PROCEDURE-Skript für „sp_ssis_startup“ wie folgt. Weitere Informationen finden Sie unter [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md).  
  
    1.  Erweitern Sie im Objekt-Explorer den Knoten **Datenbanken** und dann die Knoten **master** > **Programmierbarkeit** > **Gespeicherte Prozeduren** .  
  
    2.  Klicken Sie mit der rechten Maustaste auf **dbo.sp_ssis_startup**, und klicken Sie dann auf **Skript für gespeicherte Prozeduren als** > **Erstellen in** > **Neues Abfrage-Editor-Fenster**.  
  
6.  Bestätigen Sie, dass der SQL Server-Agent gestartet wurde.  
  
7.  Wenn Sie die SSISDB-Datenbank auf einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz wiederherstellen, auf der der SSISDB-Katalog nie erstellt wurde, generieren Sie wie folgt ein Skript für den SSIS-Server-Wartungsauftrag. Das Skript wird bei Erstellung des SSISDB-Katalogs automatisch im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent erstellt. Der Auftrag hilft beim Bereinigen von Bereinigungsvorgangsprotokollen außerhalb der Beibehaltungsdauer und entfernt ältere Projektversionen.  
  
    1.  Erweitern Sie im Objekt-Explorer den Knoten **SQL Server-Agent** und dann den Knoten **Aufträge** .  
  
    2.  Klicken Sie mit der rechten Maustaste auf den SSIS-Serverwartungsauftrag, und klicken Sie dann auf **Skript für Auftrag als** > **Erstellen in** > **Neues Abfrage-Editor-Fenster**.  
  
### <a name="to-restore-the-ssis-database"></a>So stellen Sie die SSIS-Datenbank wieder her  
  
1.  Wenn Sie die SSISDB-Datenbank auf einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz wiederherstellen, auf der der SSISDB-Katalog nie erstellt wurde, aktivieren Sie Common Language Runtime (clr), indem Sie die gespeicherte Prozedur `sp_configure` ausführen. Weitere Informationen finden Sie unter [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) und [CLR-fähig (Option)](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md).  
  
    ```  
    use master   
           sp_configure 'clr enabled', 1  
           reconfigure  
  
    ```  
  
2.  Wenn Sie die SSISDB-Datenbank auf einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz wiederherstellen, auf der der SSISDB-Katalog nie erstellt wurde, erstellen Sie den asymmetrischen Schlüssel und den Anmeldenamen aus dem asymmetrischen Schlüssel, und gewähren Sie dem Anmeldenamen die UNSAFE-Berechtigung.  

    ```  
    Create Asymmetric Key MS_SQLEnableSystemAssemblyLoadingKey  
           FROM Executable File = 'C:\Program Files\Microsoft SQL Server\YourSQLServerDefaultCompatibilityLevel\DTS\Binn\Microsoft.SqlServer.IntegrationServices.Server.dll'  
    ```  

    Sie finden den Wert für `YourSQLServerDefaultCompatibilityLevel` in einer [Liste mit SQL Server-Standardkompatibilitätsgraden](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#arguments).
  
    [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -CLR-gespeicherte Prozeduren erfordern UNSAFE-Berechtigungen, die der Anmeldung gewährt werden müssen, da die Anmeldung einen zusätzlichen Zugriff auf eingeschränkte Ressourcen (z. B. die Microsoft Win32-API) benötigt. Weitere Informationen zur UNSAFE-Codeberechtigung finden Sie unter [Creating an Assembly](../../relational-databases/clr-integration/assemblies/creating-an-assembly.md).  

    ```  
    Create Login ##MS_SQLEnableSystemAssemblyLoadingUser## FROM Asymmetric Key MS_SQLEnableSystemAssemblyLoadingKey   
    Grant Unsafe Assembly to ##MS_SQLEnableSystemAssemblyLoadingUser##    
    ```  
  
3.  Stellen Sie die SSISDB-Datenbank über die Sicherung wieder her. Verwenden Sie dazu das Dialogfeld **Datenbank wiederherstellen** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Weitere Informationen finden Sie in den folgenden Themen:  
  
    -   [Datenbank wiederherstellen &#40;Seite „Allgemein“&#41;](../../relational-databases/backup-restore/restore-database-general-page.md)  
  
    -   [Datenbank wiederherstellen &#40;Seite Dateien&#41;](../../relational-databases/backup-restore/restore-database-files-page.md)  
  
    -   [Datenbank wiederherstellen &#40;Seite Optionen&#41;](../../relational-databases/backup-restore/restore-database-options-page.md)  
  
4.  Führen Sie die Skripts aus, die Sie in [So sichern Sie die SSIS-Datenbank](#backup) für ##MS_SSISServerCleanupJobLogin##, sp_ssis_startup und den SSIS-Serverwartungsauftrag erstellt haben. Bestätigen Sie, dass der SQL Server-Agent gestartet wurde.  
  
5.  Führen Sie die folgende Anweisung aus, um die Prozedur „sp_ssis_startup“ für die automatische Ausführung festzulegen. Weitere Informationen finden Sie unter [sp_procoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md).  
  
    ```  
    EXEC sp_procoption N'sp_ssis_startup','startup','on'  
    ```  
  
6.  Ordnen Sie den SSISDB-Benutzer ##MS_SSISServerCleanupJobUser## (SSISDB-Datenbank) ##MS_SSISServerCleanupJobLogin## zu, indem Sie das Dialogfeld **Anmeldungseigenschaften** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]verwenden.  
  
7.  Verwenden Sie zum Sichern des Hauptschlüssels eine der folgenden Methoden. Weitere Informationen zur Verschlüsselung finden Sie unter [Encryption Hierarchy](../../relational-databases/security/encryption/encryption-hierarchy.md).  
  
    -   **Methode 1:**  
  
         Verwenden Sie diese Methode, wenn Sie bereits eine Sicherung des Datenbank-Hauptschlüssels ausgeführt haben und Sie über das Kennwort verfügen, das verwendet wurde, um den Hauptschlüssel zu verschlüsseln.  
  
        ```  
               Restore master key from file = 'c:\temp\RCTestInstKey'  
               Decryption by password = 'LS2Setup!' -- 'Password used to encrypt the master key during SSISDB backup'  
               Encryption by password = 'LS3Setup!' -- 'New Password'  
               Force  
  
        ```  
  
        > [!NOTE]  
        >  Bestätigen Sie, dass das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienstkonto über Berechtigungen verfügt, die Sicherungsschlüsseldatei zu lesen.  
  
        > [!NOTE]  
        >  Es wird die folgende Warnmeldung in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] angezeigt, wenn der Datenbank-Hauptschlüssel noch nicht vom Diensthauptschlüssel verschlüsselt wurde. Ignorieren Sie die Warnmeldung.  
        >   
        >  **Der aktuelle Hauptschlüssel kann nicht entschlüsselt werden. Dieser Fehler wurde ignoriert, weil die Option FORCE angegeben war.**  
        >   
        >  Das FORCE-Argument gibt an, dass der Wiederherstellungsprozess fortfahren sollte, auch wenn der aktuelle Datenbank-Hauptschlüssel nicht geöffnet ist. Diese Meldung wird für den SSISDB-Katalog angezeigt, da der Datenbank-Hauptschlüssel noch nicht auf der Instanz geöffnet wurde, auf der Sie die Datenbank wiederherstellen.  
  
    -   **Methode 2:**  
  
         Verwenden Sie diese Methode, wenn Sie über das ursprüngliche Kennwort verfügen, das verwendet wurde, um SSISDB zu erstellen.  
  
        ```  
        open master key decryption by password = 'LS1Setup!' --'Password used when creating SSISDB'  
               Alter Master Key Add encryption by Service Master Key  
        ```  
  
8.  Bestimmen Sie, ob das SSISDB-Katalogschema und die [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Binärdateien (ISServerExec-Assembly und SQLCLR-Assembly) kompatibel sind, indem Sie [catalog.check_schema_version](../../integration-services/system-stored-procedures/catalog-check-schema-version.md)ausführen.  
  
9. Um zu bestätigen, dass die SSISDB-Datenbank erfolgreich wiederhergestellt wurde, führen Sie Vorgänge für den SSISDB-Katalog aus, beispielsweise das Ausführen von Paketen, die auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server bereitgestellt wurden. Weitere Informationen finden Sie unter [Run Integration Services (SSIS) Packages (Ausführen von Integration Services-Paketen (SSIS))](../../integration-services/packages/run-integration-services-ssis-packages.md).  
  
### <a name="to-move-the-ssis-database"></a>So verschieben Sie die SSIS-Datenbank  
  
-   Folgen Sie den Anweisungen zum Verschieben von Benutzerdatenbanken. Weitere Informationen finden Sie unter [Move User Databases](../../relational-databases/databases/move-user-databases.md).  
  
     Stellen Sie sicher, dass Sie den Hauptschlüssel für die SSISDB-Datenbank sichern und die Sicherungsdatei schützen. Weitere Informationen finden Sie unter [So sichern Sie die SSIS-Datenbank](#backup).  
  
     Stellen Sie sicher, dass die für Integration Services (SSIS) relevanten Objekte in der neuen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz erstellt werden, auf der der SSISDB-Katalog noch nicht erstellt wurde.  

## <a name="upgrade-the-ssis-catalog-ssisdb"></a>Aktualisieren des SSIS-Katalogs (SSISDB)
  Führen Sie den SSISDB-Upgrade-Assistenten aus, um die Datenbank SSIS-Katalogdatenbank (SSISDB) zu aktualisieren, falls diese älter ist als die aktuelle Version der SQL Server-Instanz. Die Datenbank ist möglicherweise älter, wenn eine der folgenden Bedingungen zutrifft.  
  
-   Sie haben die Datenbank aus einer älteren Version von SQL Server wiederhergestellt.  
  
-   Sie haben die Datenbank vor der Aktualisierung der SQL Server-Instanz nicht aus einer Always On-Verfügbarkeitsgruppe entfernt. Diese Bedingung verhindert das automatische Upgrade der Datenbank. Weitere Informationen finden Sie unter [Upgrading SSISDB in an availability group](#Upgrade).  
  
 Der Assistent kann die Datenbank nur auf einer lokalen Serverinstanz aktualisieren.  
  
### <a name="upgrade-the-ssis-catalog-ssisdb-by-running-the-ssisdb-upgrade-wizard"></a>Aktualisieren des SSIS-Katalogs (SSISDB) mit dem SSISDB-Upgrade-Assistenten  
  
1.  Sichern Sie die SSIS-Katalogdatenbank (SSISDB).  
  
2.  Erweitern Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]den lokalen Server und dann **Integration Services-Katalog**.  
  
3.  Klicken Sie mit der rechten Maustaste auf **SSISDB**, und wählen Sie **Datenbankupgrade** aus, um den SSISDB-Upgrade-Assistenten zu starten. Oder Starten Sie den SSISDB-Upgrade-Assistenten, indem Sie `C:\Program Files\Microsoft SQL Server\140\DTS\Binn\ISDBUpgradeWizard.exe` mit erweiterten Berechtigungen auf dem lokalen Server ausführen.
  
     ![Starten des SSISDB-Upgrade-Assistenten](../../integration-services/service/media/ssisdb-upgrade-wizard-1.png)

4.  Wählen Sie auf der Seite **Instanz auswählen** eine SQL Server-Instanz auf dem lokalen Server aus.  
  
    > [!IMPORTANT]  
    >  Der Assistent kann die Datenbank nur auf einer lokalen Server-Instanz aktualisieren.  
  
     Aktivieren Sie das Kontrollkästchen, mit dem Sie bestätigen, dass Sie die SSISDB-Datenbank vor der Ausführung des Assistenten gesichert haben.  
  
     ![Auswählen des Servers im SSISDB-Upgrade-Assistenten](../../integration-services/service/media/ssisdb-upgrade-wizard-2.png "Auswählen des Servers im SSISDB-Upgrade-Assistenten")  
  
5.  Wählen Sie **Upgrade** , um die SSIS-Katalogdatenbank zu aktualisieren.  
  
6.  Überprüfen Sie die Ergebnisse auf der Seite **Ergebnis** .  
  
     ![Überprüfen der Ergebnisse im SSISDB-Upgrade-Assistenten](../../integration-services/service/media/ssisdb-upgrade-wizard-3.png "Überprüfen der Ergebnisse im SSISDB-Upgrade-Assistenten")  

## <a name="always-on-for-ssis-catalog-ssisdb"></a>Always On für den SSIS-Katalog (SSISDB)
  Das Feature der Always On-Verfügbarkeitsgruppen ist eine Lösung für hohe Verfügbarkeit und Notfallwiederherstellung, die eine Alternative zur Datenbankspiegelung auf Unternehmensebene bietet. Eine Verfügbarkeitsgruppe unterstützt eine Failoverumgebung für einen diskreten Satz von Benutzerdatenbanken. Diese werden auch als Verfügbarkeitsdatenbanken bezeichnet, die zusammen ein Failover ausführen. Weitere Informationen finden Sie unter [Always On-Verfügbarkeitsgruppen](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md).  
  
 Um eine hohe Verfügbarkeit für den SSIS-Katalog (SSISDB) und dessen Inhalt (Projekte, Pakete, Ausführungsprotokolle usw.) zu gewährleisten, können Sie die SSISDB-Datenbank wie jede andere Benutzerdatenbank zu einer Always On-Verfügbarkeitsgruppe hinzufügen. Wenn ein Failover auftritt, übernimmt einer der sekundären Knoten automatisch die Rolle eines primären Knoten.  
 
 > [!IMPORTANT]
 > Wenn ein Failover auftritt, werden Pakete, die ausgeführt wurden, nicht neu gestartet oder fortgesetzt. 
 
 **In diesem Abschnitt:**  
  
1.  [Voraussetzungen](#prereq)  
  
2.  [Konfigurieren der SSIS-Unterstützung für Always On](#Firsttime)  
  
3.  [Upgraden von SSISDB in einer Verfügbarkeitsgruppe](#Upgrade)  
  
###  <a name="prerequisites"></a><a name="prereq"></a> Voraussetzungen  
Bevor Sie die Always On-Unterstützung für die SSIS-Datenbank aktivieren, müssen Sie die folgenden Schritte ausführen.  
  
1.  Erstellen Sie einen Windows-Failovercluster. Weitere Anweisungen finden Sie im Blogbeitrag [Installing the Failover Cluster Feature and Tools for Windows Server 2012](https://techcommunity.microsoft.com/t5/failover-clustering/installing-the-failover-cluster-feature-and-tools-in-windows/ba-p/371733) (Failoverclusterfunktion und Tools für Windows Server 2012 installieren). Installieren Sie die Funktion und die Tools auf allen Clusterknoten.  
  
2.  Installieren Sie SQL Server 2016 mit der Funktion Integration Services (SSIS) auf jedem Clusterknoten.  
  
3.  Aktivieren Sie Always On-Verfügbarkeitsgruppen für alle SQL Server-Instanzen. Weitere Informationen finden Sie unter [Aktivieren von Always On-Verfügbarkeitsgruppen](../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md) .  
  
###  <a name="configure-ssis-support-for-always-on"></a><a name="Firsttime"></a> Konfigurieren der SSIS-Unterstützung für Always On  
  
-   [Schritt 1: Erstellen des Integration Services-Katalogs](#Step1)  
  
-   [Schritt 2: Hinzufügen von SSISDB zu einer Always On-Verfügbarkeitsgruppe](#Step2)  
  
-   [Schritt 3: Aktivieren der SSIS-Unterstützung für Always On](#Step3)  
  
> [!IMPORTANT]  
> -   Sie müssen diese Schritte auf dem **Primärknoten** der Verfügbarkeitsgruppe ausführen.
> -   Sie müssen die **SSIS-Unterstützung für Always On** *nach* dem Hinzufügen von SSISDB zu einer Always On-Verfügbarkeitsgruppe aktivieren.  

> [!NOTE]
> Weitere Informationen zu diesem Vorgang finden Sie in der folgenden exemplarischen Vorgehensweise des Data Platform-MVP Marcos Freccia, die auch zusätzliche Screenshots umfasst: [Adding SSISDB to AG for SQL Server 2016 (Hinzufügen von SSISDB zu AG für SQL Server 2016)](https://marcosfreccia.com/2017/04/28/adding-ssisdb-to-ag-for-sql-server-2016/).

####  <a name="step-1-create-integration-services-catalog"></a><a name="Step1"></a>Schritt 1: Erstellen des Integration Services-Katalogs  
  
1.  Starten Sie **SQL Server Management Studio** , und stellen Sie eine Verbindung mit einer SQL Server-Instanz in dem Cluster her, den Sie als **primären Knoten** der Always On-Gruppe mit Hochverfügbarkeit für SSISDB festlegen möchten.  
  
2.  Erweitern Sie im Objekt-Explorer den Serverknoten, klicken Sie mit der rechten Maustaste auf den Knoten **Integration Services-Kataloge** , und klicken Sie dann auf **Katalog erstellen**.  
  
3.  Klicken Sie auf **CLR-Integration aktivieren**. Für den Katalog werden gespeicherte CLR-Prozeduren verwendet.  
  
4.  Klicken Sie auf **Automatische Ausführung gespeicherter Integration Services-Prozeduren beim Starten von SQL Server aktivieren** , um die gespeicherte [catalog.startup](../system-stored-procedures/catalog-startup.md) -Prozedur jedes Mal ausführen zu lassen, wenn die SSIS-Serverinstanz neu gestartet wird. Durch die gespeicherte Prozedur wird der Status von Vorgängen für den SSISDB-Katalog verwaltet. Dabei wird der Status aller Pakete korrigiert, die während des Ausfalls der SSIS-Serverinstanz (falls zutreffend) ausgeführt wurden.  
  
5.  Geben Sie ein **Kennwort** ein, und klicken Sie dann auf **OK**. Das Kennwort schützt den Datenbank-Hauptschlüssel, der zum Verschlüsseln der Katalogdaten verwendet wird. Bewahren Sie das Kennwort sicher auf. Es wird empfohlen, auch den Datenbank-Hauptschlüssel zu sichern. Weitere Informationen finden Sie unter [Back Up a Database Master Key](../../relational-databases/security/encryption/back-up-a-database-master-key.md).  
  
####  <a name="step-2-add-ssisdb-to-an-always-on-availability-group"></a><a name="Step2"></a>Schritt 2: Hinzufügen von SSISDB zu einer Always On-Verfügbarkeitsgruppe  
Das Hinzufügen der SSISDB-Datenbank zu einer Always On-Verfügbarkeitsgruppe ist fast identisch mit dem Hinzufügen einer anderen Benutzerdatenbank zu einer Verfügbarkeitsgruppe. Weitere Informationen finden Sie unter [Verwenden des Assistenten für Verfügbarkeitsgruppen (SQL Server Management Studio)](../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md).  
  
Geben Sie das Kennwort an, das Sie beim Erstellen des SSIS-Katalogs auf der Seite **Datenbanken auswählen** im Assistenten für die **Neue Verfügbarkeitsgruppe** angegeben haben.

![Neue Verfügbarkeitsgruppe](../../integration-services/service/media/ssis-newavailabilitygroup.png "Neue Verfügbarkeitsgruppe")  
  
####  <a name="step-3-enable-ssis-support-for-always-on"></a><a name="Step3"></a>Schritt 3: Aktivieren der SSIS-Unterstützung für Always On  
 Nach dem Erstellen des Integration Service-Katalogs klicken Sie mit der rechten Maustaste auf den Knoten **Kataloge des Integrationsdiensts**, und klicken Sie auf **Always On-Unterstützung aktivieren**. Daraufhin sollte das Dialogfeld **Unterstützung für Always On aktivieren** angezeigt werden. Wenn dieses Menüelement deaktiviert ist, vergewissern Sie sich, dass Sie alle erforderlichen Komponenten installiert haben, und klicken Sie auf **Aktualisieren**.  
  
 ![Aktivieren der Always On-Unterstützung](../../integration-services/service/media/ssis-enablesupportforalwayson.png)  
  
> [!WARNING]  
>  Das automatische Failover für die SSISDB-Datenbank wird nur unterstützt, wenn Sie die SSIS-Unterstützung für Always On aktiviert haben.  
  
 Die neu hinzugefügten sekundären Replikate aus der Always On-Verfügbarkeitsgruppe werden in der Tabelle angezeigt. Klicken Sie für jedes Replikat in der Liste auf die Schaltfläche **Verbinden...** , und geben Sie Anmeldeinformationen für die Authentifizierung ein, um eine Verbindung mit dem Replikat herzustellen. Das Benutzerkonto muss auf jedem Replikat Mitglied der Gruppe „sysadmin“ sein, um die SSIS-Unterstützung für Always On zu aktivieren. Klicken Sie nach der erfolgreichen Herstellung einer Verbindung mit jedem Replikat auf **OK** , um die SSIS-Unterstützung für Always On zu aktivieren.  
 
Wenn im Kontextmenü angezeigt wird, dass die Option **Always On-Unterstützung aktivieren** deaktiviert ist, nachdem Sie die erforderlichen Schritte abgeschlossen haben, versuchen Sie Folgendes:
1.  Aktualisieren Sie das Kontextmenü, indem Sie auf die Option **Aktualisieren** klicken.
2.  Stellen Sie sicher, dass eine Verbindung mit dem Primärknoten besteht. Sie müssen die Always On-Unterstützung auf dem primären Knoten aktivieren.
3.  Stellen Sie sicher, dass Sie die Version 13.0 oder höher von SQL Server installiert haben. SSIS unterstützt Always On nur für SQL Server 2016 und höher.

###  <a name="upgrading-ssisdb-in-an-availability-group"></a><a name="Upgrade"></a> Upgraden von SSISDB in einer Verfügbarkeitsgruppe  
 Wenn Sie SQL Server von einer früheren Version aktualisieren und sich SSISDB in einer Always On-Verfügbarkeitsgruppe befindet, wird Ihr Upgrade möglicherweise durch die Regel „Überprüfung ,SSISDB in Always On-Verfügbarkeitsgruppe‘“ blockiert. Diese Blockierung tritt auf, da die Aktualisierung im Einzelbenutzermodus ausgeführt wird, während eine Verfügbarkeitsdatenbank eine Mehrbenutzerdatenbank sein muss. Daher werden während des Upgradens oder Patchens alle Verfügbarkeitsdatenbanken, einschließlich SSISDB, offline geschaltet und werden daher nicht upgegradet oder gepatcht. Um das Upgrade fortzusetzen, entfernen Sie zuerst SSISDB aus der Verfügbarkeitsgruppe, aktualisieren oder reparieren Sie dann jeden Knoten, und fügen Sie SSISDB dann wieder der Verfügbarkeitsgruppe hinzu.  
  
 Wenn das Upgrade durch die Regel „Überprüfung von SSISDB in Always On-Verfügbarkeitsgruppen“ blockiert wird, führen Sie diese Schritte aus, um SQL Server zu aktualisieren.  
  
1.  Entfernen Sie die SSISDB-Datenbank aus der Verfügbarkeitsgruppe. Weitere Informationen finden Sie unter [Entfernen einer sekundären Datenbank aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md) und [Entfernen einer primären Datenbank aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md).  
  
2.  Klicken Sie im Upgrade-Assistenten auf **Erneut ausführen**. Die Regel „Überprüfung von SSISDB in Always On-Verfügbarkeitsgruppen“ wird nun erfolgreich ausgeführt.  
  
3.  Klicken Sie auf **Weiter** , um das Upgrade fortzusetzen.  
  
4.  Nachdem Sie alle Knoten aktualisiert haben, fügen Sie die SSISDB-Datenbank wieder zur Always On-Verfügbarkeitsgruppe hinzu. Weitere Informationen finden Sie unter [Hinzufügen einer Datenbank zu einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/availability-group-add-a-database.md).  
  
 Wenn das Upgrade von SQL Server nicht blockiert wird und SSISDB sich in einer Always On-Verfügbarkeitsgruppe befindet, aktualisieren Sie SSISDB nach dem Upgrade der SQL Server-Datenbank-Engine gesondert. Verwenden Sie den SSIS-Upgrade-Assistenten, um die SSISDB upzugraden, wie im folgenden Verfahren beschrieben.  
  
1.  Verschieben Sie die SSISDB-Datenbank aus der Verfügbarkeitsgruppe, oder löschen Sie die Verfügbarkeitsgruppe, falls SSISDB die einzige Datenbank in der Verfügbarkeitsgruppe ist. Sie müssen **SQL Server Management Studio** auf dem **Primärknoten** der Verfügbarkeitsgruppe starten, um diese Aufgabe auszuführen.  
  
2.  Entfernen Sie die SSISDB-Datenbank von allen **Replikatknoten**.  
  
3.  Upgraden Sie die SSISDB-Datenbank auf dem **Primärknoten**. Erweitern Sie im **Objekt-Explorer** in SQL Server Management Studio die **Integration Services-Kataloge**, klicken Sie mit der rechten Maustaste auf **SSISDB**, und wählen Sie dann **Datenbankupgrade** aus. Führen Sie die Anweisungen im **SSISDB-Upgrade-Assistenten** aus, um die Datenbank upzugraden. Starten Sie den **SSIDB-Upgrade-Assistenten** lokal auf dem **Primärknoten**.  
  
4.  Führen Sie die Anweisungen unter [Schritt 2: Hinzufügen von SSISDB zu einer Always On-Verfügbarkeitsgruppe](#Step2) aus, um die SSISDB wieder zu einer Verfügbarkeitsgruppe hinzuzufügen.  
  
5.  Führen Sie die Anweisungen unter [Schritt 3: Aktivieren der SSIS-Unterstützung für Always On](#Step3) aus.  


## <a name="ssisdb-catalog-and-delegation-in-double-hop-scenarios"></a>SSISDB-Katalog und Delegierung in Doppel-Hop-Szenarios

Der Remoteaufruf von SSIS-Paketen, die im SSISDB-Katalog gespeichert werden, unterstützt die Delegierung von Anmeldeinformationen standardmäßig nicht. Dies wird manchmal als Doppel-Hop bezeichnet. 

Stellen Sie sich ein Szenario vor, in dem ein Benutzer sich beim Clientcomputer „A“ anmeldet und SQL Server Management Studio (SSMS) startet. Der Benutzer stellt über SSMS eine Verbindung mit einer SQL Server-Instanz her, die auf Computer „B“ gehostet wird, der über den SSISDB-Katalog verfügt. Das SSIS-Paket wird in diesem SSISDB-Katalog gespeichert. Daraufhin stellt das Paket eine Verbindung mit einem SQL Server-Dienst her, der auf dem Computer „C“ ausgeführt wird (das Paket könnte auch auf andere Dienste zugreifen). Wenn der Benutzer die Ausführung des SSIS-Pakets über Computer „A“ aufruft, übergibt SSMS zunächst die Benutzeranmeldeinformationen von Computer „A“ an Computer „B“ (auf dem der SSIS-Laufzeitprozess das Paket ausführt). Der Laufzeitprozess der SSIS-Ausführung („ISServerExec.exe“) ist nun erforderlich, um die Benutzeranmeldeinformationen von Computer „B“ an Computer „C“ zu delegieren, damit die Ausführung erfolgreich abgeschlossen werden kann. Die Delegierung von Anmeldeinformationen ist jedoch nicht standardmäßig aktiviert.

Ein Benutzer kann die Delegierung von Anmeldeinformationen aktivieren, indem er dem SQL Server-Dienstkonto (auf Computer „B“) die Berechtigung *Benutzer bei Delegierungen aller Dienste vertrauen (nur Kerberos)* gewährt, wodurch „ISServerExec.exe“ als untergeordneter Prozess gestartet wird. Dieser Vorgang wird als Einrichtung der uneingeschränkten Delegierung oder der offenen Delegierung für ein SQL Server-Dienstkonto bezeichnet. Beachten Sie, dass diese Berechtigung die Sicherheitsanforderungen Ihrer Organisation erfüllt, bevor Sie sie erteilen.

SSISDB unterstützt die eingeschränkte Delegierung nicht. Wenn das Dienstkonto der SQL Server-Instanz, die den SSISDB-Katalog hostet (Computer „B“ in diesem Beispiel), für die eingeschränkte Delegierung eingerichtet wird, ist „ISServerExec.exe“ in einer Doppel-Hop-Umgebung nicht dazu in der Lage, die Anmeldeinformationen an den dritten Computer (Computer „C“) zu delegieren. Dies gilt für Szenarios, in denen Windows Defender Credential Guard aktiviert ist. In diesen Szenarios ist die eingeschränkte Delegierung erforderlich.

  
##  <a name="related-content"></a><a name="RelatedContent"></a> Verwandte Inhalte  
  
-   Blogeintrag zu [SSIS und PowerShell in SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-and-powershell-in-sql-server-2012/ba-p/388015), auf blogs.msdn.com.  
  
-   Blogeintrag zu [SSIS Catalog Access Control Tips](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-catalog-access-control-tips/ba-p/388057)(Tipps zur SSIS-Katalogzugriffssteuerung) auf blogs.msdn.com.  
  
-   Blogeintrag zu [Überblick über das SSIS-Katalogmodell verwalteter Objekte](https://techcommunity.microsoft.com/t5/sql-server-integration-services/a-glimpse-of-the-ssis-catalog-managed-object-model/ba-p/387892)auf blogs.msdn.com.