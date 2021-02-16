---
description: Integration Services-Rollen (SSIS-Dienst)
title: Integration Services-Rollen (SSIS-Dienst) | Microsoft-Dokumentation
ms.custom: security
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.dtsserver.packageroles.f1
helpviewer_keywords:
- security [Integration Services], roles
- db_ssisoperator role
- db_ssisadmin role
- fixed database roles [Integration Services]
- packages [Integration Services], security
- roles [Integration Services]
- db_ssisltduser role
ms.assetid: 9702e90c-fada-4978-a473-1b1423017d80
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d8801c1a4f7387f1ff5bc815b45897d5a4f68fdb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340782"
---
# <a name="integration-services-roles-ssis-service"></a>Integration Services-Rollen (SSIS-Dienst)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] stellt bestimmte feste Rollen auf Datenbankebene für den sicheren Zugriff auf Pakete bereit, die in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gespeichert sind. Die verfügbaren Rollen unterscheiden sich abhängig davon, ob Sie Pakete in der SSIS-Katalogdatenbank (SSISDB) oder in der msdb-Datenbank speichern.  
  
## <a name="roles-in-the-ssis-catalog-database-ssisdb"></a>Rollen in der SSIS-Katalogdatenbank (SSISDB)  
 Die SSIS-Katalogdatenbank (SSISDB) bietet die folgenden festen Rollen auf Datenbankebene für den sicheren Zugriff auf Pakete und Informationen zu Paketen.  
  
-   **ssis_admin**. Diese Rolle bietet vollen administrativen Zugriff auf die SSIS-Katalogdatenbank.  
  
-   **ssis_logreader** : Diese Rolle bietet Berechtigungen für den Zugriff auf alle Ansichten, die mit operativen SSISDB-Protokollen verbunden sind.  
  
     Die Liste der Ansichten enthält: [catalog].[projects], [catalog].[packages], [catalog].[operations], [catalog].[extended_operation_info], [catalog].[operation_messages], [catalog].[event_messages], [catalog].[execution_data_statistics], [catalog].[execution_component_phases], [catalog].[execution_data_taps], [catalog].[event_message_context], [catalog].[executions], [catalog].[executables], [catalog].[executable_statistics], [catalog].[validations], [catalog].[execution_parameter_values] und [catalog].[execution_property_override_values].  
  
## <a name="roles-in-the-msdb-database"></a>Rollen in der msdb-Datenbank  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] bietet auf Datenbankebene die drei festen Rollen **db_ssisadmin**, **db_ssisltduser** und **db_ssisoperator** zum Steuern des Paketzugriffs. Diese werden in der **msdb**-Datenbank gespeichert. Rollen weisen Sie mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]zu. Die Rollenzuweisungen werden in der **msdb** -Datenbank gespeichert.  
  
### <a name="read-and-write-actions"></a>Lese- und Schreibaktionen  
 In der folgenden Tabelle werden die Lese- und Schreibaktionen von Windows und der festen Rollen auf Datenbankebene in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]beschrieben.  
  
|Role|Leseaktion|Schreibaktion|  
|----------|-----------------|------------------|  
|**db_ssisadmin**<br /><br /> oder<br /><br /> **sysadmin**|Eigene Pakete aufzählen.<br /><br /> Alle Pakete aufzählen.<br /><br /> Eigene Pakete anzeigen.<br /><br /> Alle Pakete anzeigen.<br /><br /> Eigene Pakete ausführen.<br /><br /> Alle Pakete ausführen.<br /><br /> Eigene Pakete exportieren.<br /><br /> Alle Pakete exportieren.<br /><br /> Alle Pakete in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent ausführen.|Pakete importieren.<br /><br /> Eigene Pakete löschen.<br /><br /> Alle Pakete löschen.<br /><br /> Eigene Paketrollen ändern.<br /><br /> Alle Paketrollen ändern.<br /><br /> <br /><br /> **\*\* Warnung \*\*** Mitglieder der db_ssisadmin-Rolle und der dc_admin-Rolle können Ihre Berechtigungen möglicherweise auf sysadmin erhöhen. Diese Ausweitung von Berechtigungen ist möglich, da diese Rollen [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Pakete ändern können und [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Pakete von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe des sysadmin-Sicherheitskontexts des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents ausgeführt werden können. Konfigurieren Sie als Schutz vor dieser Ausweitung von Berechtigungen beim Ausführen von Wartungsplänen, Datensammlungssätzen und anderen [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Paketen Aufträge des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents, die Pakete ausführen, für die Verwendung eines Proxykontos mit einschränkten Berechtigungen, oder fügen Sie der db_ssisadmin-Rolle und der dc_admin-Rolle nur sysadmin-Mitglieder hinzu.|  
|**db_ssisltduser**|Eigene Pakete aufzählen.<br /><br /> Alle Pakete aufzählen.<br /><br /> Eigene Pakete anzeigen.<br /><br /> Eigene Pakete ausführen.<br /><br /> Eigene Pakete exportieren.|Pakete importieren.<br /><br /> Eigene Pakete löschen.<br /><br /> Eigene Paketrollen ändern.|  
|**db_ssisoperator**|Alle Pakete aufzählen.<br /><br /> Alle Pakete anzeigen.<br /><br /> Alle Pakete ausführen.<br /><br /> Alle Pakete exportieren.<br /><br /> Alle Pakete in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent ausführen.|Keine|  
|**Windows-Administratoren**|Ausführungsdetails zu allen ausgeführten Paketen anzeigen.|Alle derzeit ausgeführten Pakete anhalten.|  
  
### <a name="sysssispackages-table"></a>Sysssispackages-Tabelle  
 Die **sysssispackages** -Tabelle in **msdb** enthält die in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]gespeicherten Pakete. Weitere Informationen finden Sie unter [sysssispackages &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysssispackages-transact-sql.md).  
  
 Die **sysssispackages** -Tabelle enthält Spalten, die Informationen über die Rollen enthalten, die Paketen zugewiesen sind.  
  
-   In der Spalte **readerrole** wird die Rolle angegeben, die über Lesezugriff auf das Paket verfügt.  
  
-   In der Spalte **writerrole** wird die Rolle angegeben, die über Schreibzugriff auf das Paket verfügt.  
  
-   Die **ownersid** -Spalte enthält den eindeutigen Sicherheitsbezeichner des Benutzers, der das Paket erstellte. In dieser Spalte wird der Besitzer des Pakets definiert.  
  
### <a name="permissions"></a>Berechtigungen  
 Standardmäßig gelten die Berechtigungen der festen Rollen auf Datenbankebene **db_ssisadmin** und **db_ssisoperator** und die eindeutige Sicherheits-ID des Benutzers, der das Paket erstellt hat, für die Leserolle für Pakete, und die Berechtigungen der **db_ssisadmin** -Rolle und die eindeutige Sicherheits-ID des Benutzers, der das Paket erstellt hat, gelten für die Schreibrolle. Ein Benutzer muss Mitglied der Rolle **db_ssisadmin**, **db_ssisltduser** oder **db_ssisoperator** sein, um über Lesezugriff auf das Paket zu verfügen. Ein Benutzer muss Mitglied der **db_ssisadmin** -Rolle sein, um über Schreibzugriff zu verfügen.  
  
### <a name="access-to-packages"></a>Zugriff auf Pakete  
 Die festen Rollen auf Datenbankebene arbeiten mit benutzerdefinierten Rollen zusammen. Die benutzerdefinierten Rollen sind die Rollen, die Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] erstellen und dann zum Zuweisen von Berechtigungen zu Paketen verwenden. Um auf ein Paket zuzugreifen, muss ein Benutzer Mitglied der benutzerdefinierten Rolle und der entsprechenden festen [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Datenbankrolle sein. Wenn die Benutzer z.B. Mitglieder der benutzerdefinierten **AuditUsers** -Rolle sind, die einem Paket zugewiesen ist, müssen sie auch Mitglieder der **db_ssisadmin**-, **db_ssisltduser**- oder **db_ssisoperator** -Rolle sein, um Lesezugriff auf das Paket zu erhalten.  
  
 Wenn Sie Paketen keine benutzerdefinierten Rollen zuweisen, wird der Paketzugriff durch die festen Rollen auf Datenbankebene bestimmt.  
  
 Wenn Sie benutzerdefinierte Rollen verwenden möchten, müssen Sie diese der **msdb** -Datenbank hinzufügen, bevor Sie sie Paketen zuweisen. Neue Datenbankrollen können Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]erstellen.  
  
 Die [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Rollen auf Datenbankebene gewähren den [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Systemtabellen in der msdb-Datenbank Rechte.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (der MSSQLSERVER-Dienst) muss gestartet werden, bevor Sie eine Verbindung zur Datenbank-Engine herstellen und auf die **msdb** -Datenbank zugreifen können.  
  
 Um Rollen Paketen zuzuweisen, müssen Sie die folgenden Tasks ausführen.  
  
-   **Öffnen Sie den Objekt-Explorer und stellen Sie eine Verbindung mit Integration Services her.**  
  
     Bevor Sie Paketen Rollen mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]zuweisen können, müssen Sie den Objekt-Explorer in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] öffnen und eine Verbindung mit [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]herstellen.  
  
     Der [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Dienst muss gestartet werden, bevor Sie eine Verbindung mit [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]herstellen können.  
  
-   **Paketen Lese- und Schreibrollen zuweisen**  
  
     Sie können jedem Paket eine Lese- und eine Schreibrolle zuweisen.  

## <a name="assign-a-reader-and-writer-role-to-a-package"></a><a name="assign"></a> Zuweisen einer Lese- und Schreibrolle zu einem Paket
  Sie können jedem Paket eine Lese- und eine Schreibrolle zuweisen.  
  
### <a name="assign-a-reader-and-writer-role-to-a-package"></a>Zuweisen einer Lese- und Schreibrolle zu einem Paket  
  
1.  Suchen Sie im Objekt-Explorer die [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Verbindung.  
  
2.  Erweitern Sie den Ordner "Gespeicherte Pakete", und erweitern Sie dann den Unterordner, der das Paket enthält, dem Sie Rollen zuweisen möchten.  
  
3.  Klicken Sie mit der rechten Maustaste auf das Paket, dem Sie Rollen zuweisen möchten.  
  
4.  Wählen Sie im Dialogfeld **Paketrollen** eine Leserolle aus der Liste **Leserolle** und eine Schreibrolle aus der Liste **Schreibrolle** aus.  
  
5.  Klicken Sie auf **OK**.

## <a name="create-a-user-defined-role"></a><a name="create"></a> Erstellen einer benutzerdefinierten Rolle
    
### <a name="to-create-a-user-defined-role"></a>So erstellen Sie eine benutzerdefinierte Rolle  
  
1.  Öffnen Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Klicken Sie im Menü **Ansicht** auf **Objekt-Explorer** .  
  
3.  Klicken Sie auf der Symbolleiste des Objekt-Explorers auf **Verbinden**, und klicken Sie dann auf **Datenbank-Engine**.  
  
4.  Geben Sie im Dialogfeld **Verbindung mit Server herstellen** einen Servernamen ein, und wählen Sie einen Authentifizierungsmodus aus. Sie können einen Punkt (.), (local) oder **localhost** zum Angeben des lokalen Servers verwenden.  
  
5.  Klicken Sie auf **Verbinden**.  
  
6.  Erweitern Sie die Datenbanken, Systemdatenbanken, msdb, Sicherheit und Rollen.  
  
7.  Klicken Sie im Knoten „Rollen“ mit der rechten Maustaste auf „Datenbankrollen“. Klicken Sie dann auf **Neue Datenbankrolle**.  
  
8.  Geben Sie auf der Seite "Allgemein" einen Namen ein, und legen Sie wahlweise einen Besitzer sowie Schemas im Besitz fest, und fügen Sie Rollenmitglieder hinzu.  
  
9. Klicken Sie wahlweise auf **Berechtigungen** , und konfigurieren Sie die Objektberechtigungen.  
  
10. Klicken Sie wahlweise auf **Erweiterte Eigenschaften** , und konfigurieren Sie die erweiterten Eigenschaften.  
  
11. Klicken Sie auf **OK**.

## <a name="package-roles-dialog-box-ui-reference"></a><a name="roles_dialog"></a> Benutzeroberflächenverweis zum Dialogfeld „Paketrollen“
  Verwenden Sie das Dialogfeld **Paketrollen** , das in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]verfügbar ist, um die Rollen auf Datenbankebene anzugeben, die Lesezugriff auf das Paket besitzen, sowie die Rollen auf Datenbankebene, die Schreibzugriff auf das Paket besitzen. Rollen auf Datenbankebene gelten nur für Pakete, die in der -Datenbank [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **msdb** gespeichert sind.  
  
 Die im Dialogfeld aufgeführten Rollen sind die aktuellen Rollen der **msdb** -Systemdatenbank. Wenn keine Rollen ausgewählt sind, gelten die [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Standardrollen. Standardmäßig enthält die Leserolle **db_ssisadmin**, **db_ssisoperator** und den Benutzer, der das Paket erstellt hat. Ein Benutzer, der Mitglied einer dieser Rollen ist oder die Pakete erstellt hat, kann Pakete aufzählen, anzeigen, exportieren und ausführen. Standardmäßig schließt die Schreibrolle **db_ssisadmin** und den Benutzer ein, der das Paket erstellt hat. Ein Benutzer, der Mitglied dieser Rolle ist, und der Benutzer, der die Pakete erstellt hat, kann Pakete importieren, löschen und ändern.  
  
 Die **ownersid** -Spalte in der **sysssispackages** -Tabelle enthält die eindeutige Sicherheits-ID des Benutzers, der das Paket erstellt hat.  
  
### <a name="options"></a>Tastatur  
 **Paketname**  
 Gibt den Namen des Pakets an.  
  
 **Leserolle**  
 Auswählen einer Rolle in der Liste.  
  
 **Schreibrolle**  
 Auswählen einer Rolle in der Liste  
