---
title: Erforderliche Berechtigungen
description: Hier erfahren Sie mehr über die Berechtigungen, die Sie für unterschiedliche Aufgaben in SQL Server Data Tools benötigen, zum Beispiel für das Erstellen einer Datenbank, das Ausführen von Komponententests und das Vergleichen von Schemas.
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: b27038c4-94ab-449c-90b7-29d87ce37a8b
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.openlocfilehash: 6e519a3786b45d87344f8f5f5355353fd5865d62
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072454"
---
# <a name="required-permissions-for-sql-server-data-tools"></a>Erforderliche Berechtigungen für SQL Server Data Tools

Bevor Sie eine Aktion für eine Datenbank in Visual Studio ausführen können, müssen Sie sich mit einem Konto anmelden, das für diese Datenbank über bestimmte Berechtigungen verfügt. Welche Berechtigungen Sie benötigen, hängt von der Aktion ab, die Sie ausführen möchten. In den folgenden Abschnitten werden die einzelnen Aktionen beschrieben, die Sie ausführen können, sowie die dafür benötigten Berechtigungen.  
  
-   [Berechtigungen zum Erstellen oder Bereitstellen einer Datenbank](#DatabaseCreationAndDeploymentPermissions)  
  
-   [Berechtigungen zum Umgestalten einer Datenbank](#DatabaseRefactoringPermissions)  
  
-   [Permissions to Perform Unit Tests on a SQL Server Database (Berechtigungen zum Ausführen von Komponententests in einer SQL Server-Datenbank)](#DatabaseUnitTestingPermissions)  
  
-   [Berechtigungen zum Generieren von Daten](#DataGenerationPermissions)  
  
-   [Berechtigungen zum Vergleichen von Schemas und Daten](#SchemaAndDataComparePermissions)  
  
-   [Berechtigungen zum Ausführen des Transact-SQL-Editors](#Transact-SQLEditorPermissions)  
  
-   [Berechtigungen für SQL CLR-Projekte (SQL Server Common Language Runtime)](#SQLCLRPermissions)  
  
## <a name="permissions-to-create-or-deploy-a-database"></a><a name="DatabaseCreationAndDeploymentPermissions"></a>Berechtigungen zum Erstellen oder Bereitstellen einer Datenbank  
Zum Erstellen oder Bereitstellen einer Datenbank müssen Sie über folgende Berechtigungen verfügen.  
  
|Aktionen|Erforderliche Berechtigungen|  
|-|-|  
|Importieren von Datenbankobjekten und -einstellungen|Sie müssen sich mit der Quelldatenbank verbinden können.<br /><br />Wenn die Quelldatenbank auf SQL Server 2005 basiert, müssen Sie außerdem für jedes Objekt über die Berechtigung **VIEW DEFINITION** verfügen.<br /><br />Wenn die Quelldatenbank auf SQL Server 2008 oder höher basiert, müssen Sie außerdem für jedes Objekt über die Berechtigung **VIEW DEFINITION** verfügen. Für Ihre Anmeldung muss die Berechtigung **VIEW SERVER STATE** vorliegen (für Datenbankverschlüsselungsschlüssel).|  
|Importieren von Serverobjekten und -einstellungen|Sie müssen sich mit der Masterdatenbank auf dem angegebenen Server verbinden können.<br /><br />Wenn auf dem Server SQL Server 2005 ausgeführt wird, müssen Sie für den Server über die Berechtigung **VIEW ANY DEFINITION** verfügen.<br /><br />Wenn die Quelldatenbank auf SQL Server 2008 oder höher basiert, müssen Sie für den Server über die Berechtigung **VIEW ANY DEFINITION** verfügen. Für Ihre Anmeldung muss die Berechtigung **VIEW SERVER STATE** vorliegen (für Datenbankverschlüsselungsschlüssel).|  
|Erstellen oder Aktualisieren eines Datenbankprojekts|Zum Erstellen oder Ändern eines Datenbankprojekts benötigen Sie keine Datenbankberechtigungen.|  
|Bereitstellen einer neuen Datenbank oder Bereitstellen mit der Option **Datenbank immer neu erstellen**|Sie müssen entweder die Berechtigung **CREATE DATABASE** besitzen oder Mitglied der Rolle **dbcreator** auf dem Zielserver sein.<br /><br />Wenn Sie eine Datenbank erstellen, stellt Visual Studio eine Verbindung mit der Modelldatenbank her und kopiert deren Inhalt. Für die erste Anmeldung (z.B. *MeineAnmeldung*), mit der Sie sich mit der Zieldatenbank verbinden, müssen die Berechtigungen **db_creator** und **CONNECT SQL** vorliegen. Diese Anmeldung muss über eine Benutzerzuordnung in der Modelldatenbank verfügen. Wenn Sie **sysadmin**-Berechtigungen besitzen, können Sie die Zuordnung durch Ausgabe der folgenden Transact\-SQL-Anweisungen erstellen:<br /><br />`USE [model] CREATE USER yourUser FROM LOGIN yourLogin`<br /><br />Der Benutzer (in diesem Beispiel yourUser) muss über die Berechtigungen **CONNECT** und **VIEW DEFINITION** für die Modelldatenbank verfügen. Wenn Sie **sysadmin**-Berechtigungen besitzen, können Sie diese Berechtigungen durch Ausgabe der folgenden Transact\-SQL-Anweisungen gewähren:<br /><br />`USE [model] GRANT CONNECT to yourUser GRANT VIEW DEFINITION TO yourUser`<br /><br />Wenn Sie eine Datenbank bereitstellen, die unbenannte Einschränkungen enthält, und wenn die Option **CheckNewContraints** aktiviert ist (standardmäßig aktiviert), müssen Sie die Berechtigungen **db_owner** oder **sysadmin** besitzen, andernfalls schlägt die Bereitstellung fehl. Dies gilt nur für unbenannte Einschränkungen. Weitere Informationen zur Option **CheckNewConstraints** finden Sie unter [Database Project Settings](../ssdt/database-project-settings.md).|  
|Bereitstellen von Aktualisierungen für eine vorhandene Datenbank|Sie müssen ein gültiger Datenbankbenutzer sein. Sie müssen außerdem Mitglied der Rolle **db_ddladmin**, Besitzer des Schemas oder der Objekte sein, die Sie für die Zieldatenbank erstellen oder ändern möchten. Sie benötigen zusätzliche Berechtigungen, um mit erweiterten Merkmalen zu arbeiten, wie z. B. Anmeldungen oder verknüpfte Server in Ihren Skripts vor oder nach der Bereitstellung.<br /><br />**HINWEIS:** Wenn Sie eine Bereitstellung für die Masterdatenbank vornehmen, müssen Sie auch über die Berechtigung **VIEW ANY DEFINITION** für den betreffenden Server verfügen.|  
|Verwenden einer Assembly mit der Option EXTERNAL_ACCESS in einem Datenbankprojekt|Sie müssen die Eigenschaft TRUSTWORTHY für Ihr Datenbankprojekt festlegen. Für Ihre SQL Server-Anmeldung muss die Berechtigung EXTERNAL ACCESS ASSEMBLY vorliegen.|  
|Bereitstellen von Assemblys für eine neue oder vorhandene Datenbank|Sie müssen Mitglied der sysadmin-Rolle auf dem Zielserver der Bereitstellung sein.|  
  
Weitere Informationen finden Sie in der SQL Server-Onlinedokumentation.  
  
## <a name="permissions-to-refactor-a-database"></a><a name="DatabaseRefactoringPermissions"></a> Berechtigungen zum Umgestalten einer Datenbank  
Eine *Datenbankumgestaltung* findet nur innerhalb des Datenbankprojekts statt. Sie müssen berechtigt sein, das Datenbankprojekt zu verwenden. Sie benötigen erst dann Berechtigungen für eine Zieldatenbank, wenn Sie Änderungen daran bereitstellen.  
  
## <a name="permissions-to-perform-unit-testing-on-a-sql-server-database"></a><a name="DatabaseUnitTestingPermissions"></a>Berechtigungen zum Ausführen von Komponententests in einer SQL Server-Datenbank  
Zum Durchführen von Komponententests für eine Datenbank müssen Sie über folgende Berechtigungen verfügen.  
  
|Aktionen|Erforderliche Berechtigungen|  
|-|-|   
|Ausführen einer Testaktion|Sie müssen die Datenbankverbindung für den Ausführungskontext verwenden. Weitere Informationen finden Sie unter [Übersicht über Verbindungszeichenfolgen und Berechtigungen](../ssdt/overview-of-connection-strings-and-permissions.md).|  
|Ausführen einer Vortest- oder Nachtestaktion|Sie müssen die Datenbankverbindung für den privilegierten Kontext verwenden. Diese Datenbankverbindung verfügt über mehr Berechtigungen als die Verbindung für den Ausführungskontext.|  
|Ausführen der Skripts TestInitialize und TestCleanup|Sie müssen die Datenbankverbindung für den privilegierten Kontext verwenden.|  
|Bereitstellen von Datenbankänderungen vor Ausführung von Tests|Sie müssen die Datenbankverbindung für den privilegierten Kontext verwenden. Weitere Informationen finden Sie unter [Vorgehensweise: Konfigurieren der Ausführung von SQL Server-Komponententests](../ssdt/how-to-configure-sql-server-unit-test-execution.md).|  
|Generieren von Daten vor Ausführung von Tests|Sie müssen die Datenbankverbindung für den privilegierten Kontext verwenden. Weitere Informationen finden Sie unter [Vorgehensweise: Konfigurieren der Ausführung von SQL Server-Komponententests](../ssdt/how-to-configure-sql-server-unit-test-execution.md).|  
  
## <a name="permissions-to-generate-data"></a><a name="DataGenerationPermissions"></a>Berechtigungen zum Generieren von Daten  
Sie müssen die Berechtigungen **INSERT** und **SELECT** für die Objekte in der Zieldatenbank besitzen, um Testdaten mithilfe des Datengenerators zu generieren. Wenn Sie vor der Datengenerierung eine Bereinigung durchführen, müssen Sie außerdem die **DELETE**-Berechtigungen für die Objekte in der Zieldatenbank besitzen. Um die Spalte **IDENTITY** in einer Tabelle zurückzusetzen, müssen Sie Besitzer der Tabelle oder Mitglied der Rolle „db_owner“ oder „db_ddladmin“ sein.  
  
## <a name="permissions-to-compare-schemas-and-data"></a><a name="SchemaAndDataComparePermissions"></a>Berechtigungen zum Vergleichen von Schemas und Daten  
Zum Vergleichen von Schemas oder Daten müssen Sie über folgende Berechtigungen verfügen.  
  
|Aktionen|Erforderliche Berechtigungen|  
|-|-|   
|Vergleichen der Schemas von zwei Datenbanken|Sie müssen die Berechtigungen zum Importieren von Objekten und Einstellungen aus den Datenbanken wie unter [Berechtigungen zum Erstellen oder Bereitstellen einer Datenbank](#DatabaseCreationAndDeploymentPermissions) beschrieben besitzen.|  
|Vergleichen der Schemas einer Datenbank und eines Datenbankprojekts|Sie müssen die Berechtigungen zum Importieren von Objekten und Einstellungen aus der Datenbank wie unter [Berechtigungen zum Erstellen oder Bereitstellen einer Datenbank](#DatabaseCreationAndDeploymentPermissions) beschrieben besitzen. Außerdem muss das Datenbankprojekt in Visual Studio geöffnet sein.|  
|Schreiben von Aktualisierungen in eine Zieldatenbank|Sie müssen die Berechtigungen zum Bereitstellen von Aktualisierungen für die Zieldatenbank wie unter [Berechtigungen zum Erstellen oder Bereitstellen einer Datenbank](#DatabaseCreationAndDeploymentPermissions) beschrieben besitzen.|  
|Vergleichen der Daten von zwei Datenbanken|Neben den Berechtigungen, die Sie zum Vergleichen der Schemas von zwei Datenbanken benötigen, müssen Sie auch die Berechtigung **SELECT** für alle Tabellen besitzen, die Sie vergleichen möchten, sowie die Berechtigung **VIEW DATABASE STATE**.|  
  
Weitere Informationen finden Sie in der SQL Server-Onlinedokumentation.  
  
## <a name="permissions-to-run-the-transact-sql-editor"></a><a name="Transact-SQLEditorPermissions"></a>Berechtigungen zum Ausführen des Transact\-SQL-Editors  
Welche Aktionen Sie im Transact\-SQL-Editor ausführen können, hängt von Ihrem Ausführungskontext für die Zieldatenbank ab.  
  
## <a name="permissions-for-sql-server-common-language-run-time-projects"></a><a name="SQLCLRPermissions"></a>Berechtigungen für SQL Server-CLR-Projekte (SQL Server Common Language Runtime)  
Die folgende Tabelle enthält die Berechtigungen, die Sie besitzen müssen, um CLR-Projekte bereitzustellen oder zu debuggen:  
  
|Aktionen|Erforderliche Berechtigungen|  
|-----------|------------------------|  
|Bereitstellen (Erstbereitstellung oder inkrementelle Bereitstellung) einer Assembly mit SAFE-Berechtigungssatz|db_DDLAdmin – gewährt CREATE- und ALTER-Berechtigungen für die Assemblys und Objekttypen, die Sie bereitstellen<br /><br />VIEW DEFINITION auf Datenbankebene – erforderlich für Bereitstellung<br /><br />CONNECT auf Datenbankebene – gewährt die Fähigkeit, eine Verbindung zur Datenbank herzustellen|  
|Bereitstellen einer Assembly mit EXTERNAL ACCESS-Berechtigungssatz|db_DDLAdmin – gewährt CREATE- und ALTER-Berechtigungen für die Assemblys und Objekttypen, die Sie bereitstellen<br /><br />VIEW DEFINITION auf Datenbankebene – erforderlich für Bereitstellung<br /><br />CONNECT auf Datenbankebene – gewährt die Fähigkeit, eine Verbindung zur Datenbank herzustellen<br /><br />Weitere Voraussetzungen:<br /><br />TRUSTWORTHY-Datenbankoption muss aktiviert sein.<br /><br />Für die Anmeldung für die Bereitstellung muss die Serverberechtigung EXTERNAL ACCESS ASSEMBLY vorliegen.|  
|Bereitstellen einer Assembly mit UNSAFE-Berechtigungssatz|db_DDLAdmin – gewährt CREATE- und ALTER-Berechtigungen für die Assemblys und Objekttypen, die Sie bereitstellen<br /><br />VIEW DEFINITION auf Datenbankebene – erforderlich für Bereitstellung<br /><br />CONNECT auf Datenbankebene – gewährt die Fähigkeit, eine Verbindung zur Datenbank herzustellen<br /><br />Weitere Voraussetzungen:<br /><br />TRUSTWORTHY-Datenbankoption muss aktiviert sein.<br /><br />Für die Anmeldung, die Sie für die Bereitstellung verwenden, muss die Serverberechtigung UNSAFE ASSEMBLY vorliegen.|  
|Remote-Debuggen einer SQL CLR-Assembly|Sie müssen über die feste Rollenberechtigung "sysadmin" verfügen.|  
  
> [!IMPORTANT]  
> In allen Fällen muss der Assemblybesitzer der Benutzer sein, den Sie verwenden, um die Assembly bereitzustellen, oder der Besitzer muss eine Rolle innehaben, von der dieser Benutzer Mitglied ist. Diese Anforderung gilt auch für alle Assemblys, auf die von der von Ihnen bereitgestellten Assembly verwiesen wird.  
  
## <a name="see-also"></a>Weitere Informationen  
[Erstellen und Definieren von SQL Server-Komponententests](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[SQL Server Data Tools](../ssdt/sql-server-data-tools.md)  
  
