---
description: Datensammlersicherheit
title: Datensammlersicherheit | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- data collection [SQL Server]
- security [data collector]
- data collector [SQL Server], security
ms.assetid: e75d6975-641e-440a-a642-cb39a583359a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a61b6d819fd014183c68485b1b9a8f0a7c362598
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343680"
---
# <a name="data-collector-security"></a>Datensammlersicherheit
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Der Datensammler verwendet das von dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agent implementierte rollenbasierte Sicherheitsmodell. Mit diesem Modell kann der Datenbankadministrator die verschiedenen Datensammlertasks in einem Sicherheitskontext ausführen, der nur über die zum Ausführen dieses Tasks erforderlichen Berechtigungen verfügt. Dieser Ansatz wird auch für Vorgänge mit internen Tabellen verwendet, auf die nur unter Verwendung einer gespeicherten Prozedur oder Sicht zugegriffen werden kann. Internen Tabellen werden keine Berechtigungen erteilt. Stattdessen werden Berechtigungen für den Benutzer der gespeicherten Prozedur oder Sicht überprüft, die zum Zugreifen auf eine Tabelle verwendet wird.  
  
> [!IMPORTANT]  
>  Ein anderer Hauptaspekt dieses Sicherheitsmodells sind konzentrische Berechtigungen. Mit konzentrischen Berechtigungen erben Rollen mit höheren Berechtigungen die Berechtigungen von Rollen mit niedrigeren Berechtigungen für Objekte (einschließlich Warnungen, Operatoren, Aufträgen, Zeitplänen und Proxys). Weitere Informationen finden Sie unter [SQL Server Agent Fixed Database Roles](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 In den folgenden Abschnitten werden die Datensammlungssicherheit im Allgemeinen sowie die Rollen beschrieben, die Sie Benutzern erteilen müssen, damit diese den Datensammler konfigurieren und verwenden und mit dem Management Data Warehouse verbundene Tasks ausführen können.  
  
## <a name="general-security"></a>Allgemeine Sicherheit  
 Der Datensammler wird gemäß der für [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]festgelegten dokumentierten Standards installiert.  
  
### <a name="network-security"></a>Netzwerksicherheit  
 Vertrauliche Informationen können zwischen Zielinstanzen, der mit dem Konfigurationsserver verbundenen relationalen Instanz, den ausgeführten Sammlungssätzen und dem Server, der das Verwaltungs-Data Warehouse hostet, übergeben werden.  
  
 Zum Schützen von Daten, die über ein Netzwerk übertragen werden, werden die Standardsicherheitsmechanismen implementiert, beispielsweise Protokollverschlüsselung für [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="permissions-for-configuring-and-using-the-data-collector"></a>Berechtigungen zum Konfigurieren und Verwenden des Datensammlers  
 Je nach Task müssen Benutzer Mitglieder von mindestens einer festen Datenbankrolle sein, die für den Datensammler bereitgestellt wurde. Die Rollen lauten, geordnet von den höchsten bis zu den niedrigsten Zugriffsberechtigungen, wie folgt:  
  
-   **dc_admin**  
  
-   **dc_operator**  
  
-   **dc_proxy**  
  
 Diese Rollen werden in der msdb-Datenbank gespeichert. Standardmäßig ist kein Benutzer Mitglied dieser Datenbankrollen. Die Benutzermitgliedschaft in diesen Rollen muss explizit erteilt werden.  
  
 Benutzer, die Mitglieder der festen Serverrolle **sysadmin** sind, haben vollen Zugriff auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agentobjekte und -Datensammlersichten. Sie müssen jedoch explizit Datensammlerrollen hinzugefügt worden sein.  
  
> [!IMPORTANT]  
>  Mitglieder der db_ssisadmin-Rolle und der dc_admin-Rolle können Ihre Berechtigungen möglicherweise auf sysadmin erhöhen. Diese Ausweitung von Berechtigungen ist möglich, da diese Rollen [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Pakete ändern können und [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Pakete von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mithilfe des sysadmin-Sicherheitskontexts des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents ausgeführt werden können. Konfigurieren Sie als Schutz vor dieser Ausweitung von Berechtigungen beim Ausführen von Wartungsplänen, Datensammlungssätzen und anderen [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Paketen Aufträge des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Agents, die Pakete ausführen, für die Verwendung eines Proxykontos mit einschränkten Berechtigungen, oder fügen Sie der db_ssisadmin-Rolle und der dc_admin-Rolle nur sysadmin-Mitglieder hinzu.  
  
### <a name="dc_admin-role"></a>dc_admin-Rolle  
 Der **dc_admin** -Rolle zugewiesene Benutzer haben vollen Administratorzugriff (Erstellen, Lesen, Aktualisieren und Löschen) auf die Datensammlerkonfiguration auf einer Serverinstanz. Mitglieder dieser Rolle können die folgenden Vorgänge ausführen:  
  
-   Festlegen von Eigenschaften auf Sammlerebene.  
  
-   Hinzufügen neuer Sammlungssätze.  
  
-   Installieren neuer Sammlungstypen.  
  
-   Ausführen aller Vorgänge, die der **dc_operator** -Rolle erteilt wurden.  
  
 Die **dc_admin** -Rolle ist ein Mitglied der folgenden Rollen:  
  
-   **SQLAgentUserRole**. Diese Rolle ist zum Erstellen von Zeitplänen und zum Ausführen von Aufträgen erforderlich.  
  
    > [!NOTE]  
    >  Für den Datensammler erstellte Proxys müssen Zugriff auf **dc_admin** erteilen, damit sie erstellt und für alle Schritte eines Auftrags verwendet werden können, für die ein Proxy erforderlich ist.  
  
-   **dc_operator**. Mitglieder von **dc_admin** erben die Berechtigungen, die **dc_operator** erteilt wurden.  
  
### <a name="dc_operator-role"></a>dc_operator-Rolle  
 Mitglieder der **dc_operator-Rolle** verfügen über Lese- und Aktualisierungszugriff. Diese Rolle unterstützt Vorgangstasks in Bezug auf das Ausführen und Konfigurieren von Sammlungssätzen. Mitglieder dieser Rolle können die folgenden Vorgänge ausführen:  
  
-   Starten oder Beenden eines Sammlungssatzes.  
  
-   Auflisten vorhandener Sammlungssätze.  
  
-   Anzeigen der mit einem Sammlungssatz verbundenen ausführlichen Informationen (beispielsweise Sammelelemente und Sammlungshäufigkeit).  
  
-   Ändern der Hochladehäufigkeit für vorhandene Sammlungssätze.  
  
-   Ändern der Sammlungshäufigkeit für Sammlungselemente, die Teil eines vorhandenen Sammlungssatzes sind.  
  
 Die **dc_operator** -Rolle ist ein Element der folgenden Rollen, die zum Aufzählen und Anzeigen von Datensammlerpaketen erforderlich sind:  
  
-   **db_ssisltduser**  
  
-   **db_ssisoperator**  
  
 Weitere Informationen finden Sie unter [Integration Services Roles &#40;SSIS Service&#41;](../../integration-services/security/integration-services-roles-ssis-service.md) (Integration Services-Rollen [SSIS-Dienst]).  
  
### <a name="dc_proxy-role"></a>dc_proxy-Rolle  
 Mitglieder der **dc_proxy** -Rolle verfügen über Lesezugriff auf Sammlungssätze des Datensammlers und Eigenschaften auf Sammlerebene. Die Mitglieder dieser Rolle können auch Aufträge anzeigen und ausführen, deren Besitzer sie sind, und Auftragsschritte erstellen, die als bereits vorhandenes Proxykonto ausgeführt werden.  
  
 Mitglieder dieser Rolle können die folgenden Vorgänge ausführen:  
  
-   Anzeigen der mit einem Sammlungssatz verbundenen Konfigurationsinformationen (beispielsweise Eingabeparameter für Sammelelemente und die Sammlungshäufigkeit für diese Elemente).  
  
-   Abrufen interner verschlüsselter Informationen, auf die nur von einer signierten gespeicherten Prozedur zugegriffen werden kann (beispielsweise Informationen über eine Data Warehouse-Verbindung, die zum Hochladen von Daten verwendet wird).  
  
-   Protokollieren von für eine Sammlung festgelegte Ereignisse zur Laufzeit.  
  
 Die **dc_proxy** -Rolle ist ein Element der folgenden Rollen, die zum Aufzählen und Anzeigen von Datensammlerpaketen erforderlich sind:  
  
-   **db_ssisltduser**.  
  
-   **db_ssisoperator**  
  
 Weitere Informationen finden Sie unter [Integration Services Roles &#40;SSIS Service&#41;](../../integration-services/security/integration-services-roles-ssis-service.md) (Integration Services-Rollen [SSIS-Dienst]).  
  
## <a name="permissions-for-configuring-and-using-the-management-data-warehouse"></a>Berechtigungen zum Konfigurieren und Verwenden des Management Data Warehouse  
 Je nach Task müssen Benutzer Mitglieder von mindestens einer festen Datenbankrolle sein, die für den Zugriff auf das Verwaltungs-Data Warehouse bereitgestellt wurde. Die Rollen lauten, geordnet von den höchsten bis zu den niedrigsten Zugriffsberechtigungen, wie folgt:  
  
-   **mdw_admin**  
  
-   **mdw_writer**  
  
-   **mdw_reader**  
  
 Diese Rollen werden in der msdb-Datenbank gespeichert. Standardmäßig ist kein Benutzer Mitglied dieser Datenbankrollen. Die Benutzermitgliedschaft in diesen Rollen muss explizit erteilt werden.  
  
 Benutzer, die Mitglieder der festen Serverrolle **sysadmin** sind, haben vollen Zugriff auf Datensammlersichten. Sie müssen jedoch explizit Datenbankrollen hinzugefügt werden, um weitere Operationen durchführen zu können.  
  
### <a name="mdw_admin-role"></a>mdw_admin-Rolle  
 Mitglieder der **mdw_admin** -Rolle verfügen über Lese-, Schreib-, Aktualisierungs- und Löschzugriff auf das Verwaltungs-Data Warehouse.  
  
 Mitglieder dieser Rolle können die folgenden Vorgänge ausführen:  
  
-   Ändern des Verwaltungs-Data Warehouse-Schemas, falls erforderlich (beispielsweise Hinzufügen einer neuen Tabelle, wenn ein neuer Sammlertyp installiert wird).  
  
    > [!NOTE]  
    >  Bei einer Schemaänderung muss ein Benutzer auch Mitglied der **dc_admin** -Rolle sein, um einen neuen Sammlertyp installieren zu können, da für diese Aktion eine Berechtigung zum Aktualisieren der Datensammlerkonfiguration in msdb erforderlich ist.  
  
-   Ausführen von Wartungsaufträgen für das Verwaltungs-Data Warehouse, beispielsweise Archivierung oder Cleanup.  
  
### <a name="mdw_writer-role"></a>mdw_writer-Rolle  
 Mitglieder der **mdw_writer** -Rolle können Daten in das Verwaltungs-Data Warehouse hochladen und schreiben. Jeder Datensammler, der Daten in dem Verwaltungs-Data Warehouse speichert, muss ein Mitglied dieser Rolle sein.  
  
### <a name="mdw_reader-role"></a>mdw_reader-Rolle  
 Mitglieder der **mdw_reader** -Rolle verfügen über Lesezugriff auf das Verwaltungs-Data Warehouse. Da diese Rolle zur Unterstützung bei der Problembehandlung dient, indem sie Zugriff auf Verlaufsdaten bietet, können Mitglieder dieser Rolle andere Elemente des Verwaltungs-Data Warehouse-Schemas nicht anzeigen.  
  
## <a name="see-also"></a>Siehe auch  
 [Implementieren der SQL Server-Agent-Sicherheit](../../ssms/agent/implement-sql-server-agent-security.md)  
  
  
