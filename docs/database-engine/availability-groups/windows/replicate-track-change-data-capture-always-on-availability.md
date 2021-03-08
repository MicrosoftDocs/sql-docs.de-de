---
title: Replikation, Änderungsnachverfolgung, Change Data Capture und Verfügbarkeitsgruppen
description: Erfahren Sie mehr über die Interoperabilität von Replikation, Änderungsnachverfolgung und Change Data Capture bei der Verwendung mit Always On-Verfügbarkeitsgruppen von SQL Server.
ms.custom: seo-lt-2019
ms.date: 02/23/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- change tracking [SQL Server], AlwaysOn Availability Groups
- change data capture [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: e17a9ca9-dd96-4f84-a85d-60f590da96ad
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1ee6a097a60930064ee23389d1d54e965336e3d7
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837691"
---
# <a name="replication-change-tracking--change-data-capture---always-on-availability-groups"></a>Replikation, Änderungsnachverfolgung und Change Data Capture (Always On-Verfügbarkeitsgruppen)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Replikation, Change Data Capture (CDC) und Änderungsnachverfolgung (CT) werden unter [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]unterstützt. [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] bietet Hochverfügbarkeit und zusätzliche Funktionen zur Datenbankwiederherstellung.  
  
##  <a name="overview-of-replication-with-availability-groups"></a><a name="Overview"></a> Übersicht über die Replikation in Verfügbarkeitsgruppen  
  
###  <a name="publisher-redirection"></a><a name="PublisherRedirect"></a> Verlegerumleitung  
 Wenn eine veröffentlichte Datenbank [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]-fähig ist, wird der Verteiler, der den Agentzugriff auf die Veröffentlichungsdatenbank bereitstellt, mit redirected_publishers-Einträgen konfiguriert. Diese Einträge leiten das ursprünglich konfigurierte Verleger-Datenbank-Paar um und nutzen einen Verfügbarkeitsgruppenlistener-Namen zum Herstellen der Verbindung mit dem Verleger und der Veröffentlichungsdatenbank. Für über den Verfügbarkeitsgruppenlistener-Namen hergestellte Verbindungen kann kein Failover ausgeführt werden. Wenn der Replikations-Agent neu gestartet wird, wird die Verbindung nach dem Failover automatisch an das neue primäre Element umgeleitet.  
  
 In einer Verfügbarkeitsgruppe kann eine sekundäre Datenbank nicht als Verleger fungieren. Erneutes Veröffentlichen wird nur unterstützt, wenn Transaktionsreplikation mit [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]kombiniert wird.  
  
 Wenn eine veröffentlichte Datenbank Mitglied einer Verfügbarkeitsgruppe ist und der Verleger umgeleitet wird, muss er an einen mit der Verfügbarkeitsgruppe verknüpften Verfügbarkeitsgruppenlistener-Namen umgeleitet werden. Er kann nicht an einen expliziten Knoten umgeleitet werden.  
  
> [!NOTE]  
>  Der Replikationsmonitor kann den Namen der Veröffentlichungsinstanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] nach einem Failover zu einem sekundären Replikat nicht anpassen und zeigt weiterhin Replikationsinformationen unter dem Namen der ursprünglichen primären Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]an. Nach einem Failover kann kein Überwachungstoken mit dem Replikationsmonitor eingegeben werden, ein mit [!INCLUDE[tsql](../../../includes/tsql-md.md)]auf dem neuen Verleger eingegebenes Überwachungstoken wird jedoch im Replikationsmonitor angezeigt.  
  
###  <a name="general-changes-to-replication-agents-to-support-availability-groups"></a><a name="Changes"></a> Allgemeine Änderungen an Replikations-Agents zur Unterstützung von Verfügbarkeitsgruppen  
 Drei Replikations-Agents wurden geändert, um [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]zu unterstützen. Die Protokolllese-, Momentaufnahme- und Merge-Agents wurden geändert, um die Verteilungsdatenbank für den umgeleiteten Verleger abzufragen und den zurückgegebenen Verfügbarkeitsgruppenlistener-Namen zu verwenden, wenn ein umgeleiteter Verleger deklariert wurde, um eine Verbindung mit dem Datenbankverleger herzustellen.  
  
 Wenn die Agents den Verteiler abfragen, um zu bestimmen, ob der ursprüngliche Verleger umgeleitet wurde, wird die Eignung des aktuellen Ziels oder der Umleitung standardmäßig überprüft, bevor der umgeleitete Host an den Agent zurückgegeben wird. Dabei handelt es sich um ein empfohlenes Verhalten. Wenn der Agent jedoch sehr häufig gestartet wird, könnte der mit der gespeicherten Überprüfungsprozedur verbundene Arbeitsaufwand als zu kostenintensiv angesehen werden. Der neue Befehlszeilenschalter *BypassPublisherValidation* wurde den Protokolllese-, Momentaufnahmen- und Merge-Agents hinzugefügt. Wenn der Schalter verwendet wird, wird der umgeleitete Verleger sofort an den Agent zurückgegeben, und die Ausführung der überprüfungsgespeicherten Prozedur wird umgangen.  
  
 In der überprüfungsgespeicherten Prozedur zurückgegebene Fehler werden in den Agentverlaufsprotokollen protokolliert. Fehler mit einem Schweregrad größer oder gleich 16 beenden den Agent. Einige Wiederholungsfunktionen wurden in die Agents integriert, um das erwartete Trennen einer Verbindung von einer veröffentlichten Datenbank bei einem Failover auf eine neu primäres Element zu verarbeiten.  
  
#### <a name="log-reader-agent-modifications"></a>Änderungen des Protokolllese-Agents  
 Der Protokolllese-Agent weist die folgenden Änderungen auf.  
  
-   **Replizierte Datenbankkonsistenz**  
  
     Wenn eine veröffentlichte Datenbank Element einer Verfügbarkeitsgruppe ist, verarbeitet der Protokollleser standardmäßig keine Protokolldatensätze, die nicht bereits für alle sekundären Replikate der Verfügbarkeitsgruppe festgeschrieben wurden. Dadurch wird sichergestellt, dass bei einem Failover alle auf einen Abonnenten replizierten Zeilen auch im neuen primären Element vorhanden sind.  
  
     Wenn der Verleger nur über zwei Verfügbarkeitsreplikate (ein primäres und ein sekundäres) verfügt und ein Failover erfolgt, bleibt das ursprünglich primäre Replikat inaktiv, da sich der Protokollleser erst dann vorwärts bewegt, nachdem alle sekundären Datenbanken wieder online geschaltet bzw. die fehlerhaften sekundären Replikate aus der Verfügbarkeitsgruppe entfernt wurden. Der Protokollleser, der nun für die sekundäre Datenbank ausgeführt wird, wird nicht vorwärts bewegt, da Always On Änderungen nicht in sekundären Datenbanken festschreiben kann. Um dem Protokollleser das Fortfahren zu ermöglichen und weiter über Kapazität für eine Notfallwiederherstellung zu verfügen, entfernen Sie das ursprünglich primäre Replikat mit ALTER AVAILABITY GROUP <Gruppenname> REMOVE REPLICA aus der Verfügbarkeitsgruppe. Fügen Sie der Verfügbarkeitsgruppe dann ein neues sekundäres Replikat hinzu.  
  
-   **Ablaufverfolgungsflag 1448**  
  
     Das Ablaufverfolgungsflag 1448 aktiviert den Replikationsprotokollleser, sodass er weitergehen kann, auch wenn die asynchronen sekundären Replikate den Empfang einer Änderung nicht bestätigt haben. Auch wenn dieses Ablaufverfolgungsflag aktiviert ist, wartet der Protokollleser immer auf die synchronen sekundären Replikate. Der Protokollleser geht nicht über "min ack" für die synchronen sekundären Replikate hinaus. Dieses Ablaufverfolgungsflag gilt für die Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]und nicht nur für eine Verfügbarkeitsgruppe, eine Verfügbarkeitsdatenbank oder eine Protokollleserinstanz. Dieses Ablaufverfolgungsflag wird ohne Neustart sofort wirksam. Es kann im Voraus oder beim Fehlschlagen eines asynchronen sekundären Replikats aktiviert werden.  
  
###  <a name="stored-procedures-supporting-availability-groups"></a><a name="StoredProcs"></a> Gespeicherte Prozeduren, die Verfügbarkeitsgruppen unterstützen  
  
-   **sp_redirect_publisher**  
  
     Die gespeicherte Prozedur **sp_redirect_publisher** wird verwendet, um einen umgeleiteten Verleger für ein vorhandenes Verleger-/Datenbank-Paar anzugeben. Wenn die Verlegerdatenbank zu einer Verfügbarkeitsgruppe gehört, ist der umgeleitete Verleger der Verfügbarkeitsgruppenlistener-Name.  
  
-   **sp_get_redirected_publisher**  
  
     Die gespeicherte Prozedur **sp_get_redirected_publisher** wird von Replikations-Agents zum Abfragen eines Verteilers verwendet, um zu bestimmen, ob ein Verleger-/Datenbank-Paar über einen definierten umgeleiteten Verleger verfügt. Diese gespeicherte Prozedur dient zwei Zwecken. Zuerst ermöglicht sie dem Agent zu bestimmen, ob der ursprüngliche Verleger umgeleitet wurde. Zweitens kann sie auch möglicherweise die Ausführung einer gespeicherten Validierungsprozedur beim Verteiler (**sp_validate_redirected_publisher**) initiieren, die die Eignung des Zielknotens der Umleitung als Verleger für die benannte Datenbank überprüft.  
  
     Zum Ausführen dieser gespeicherten Prozedur muss der Aufrufer entweder ein Element der **sysadmin** -Serverrolle, der **db_owner** -Datenbankrolle für die Verteilungsdatenbank oder ein Element einer **Veröffentlichungszugriffsliste** für eine der Verlegerdatenbank zugeordnete definierte Veröffentlichung sein.  
  
-   **sp_validate_redirected_publisher**  
  
     Diese gespeicherte Prozedur versucht zu überprüfen, ob der aktuelle Verleger die veröffentlichte Datenbank hosten kann. Sie kann jederzeit aufgerufen werden, um zu überprüfen, ob der aktuelle Host für die veröffentlichte Datenbank die Replikation unterstützen kann.  
  
-   **sp_validate_replicate_hosts_as_publishers**  
  
     Es kann für die Agents nützlich sein sicherzustellen, dass das aktuelle primäre Element als Replikationsverleger für eine Verlegerdatenbank agieren kann. Andererseits wird eine allgemeinere Überprüfungsfunktion benötigt, um die Gültigkeit einer gesamten Replikationstopologie für eine Always On-Verfügbarkeitsdatenbank festzulegen. Die gespeicherte Prozedur **sp_validate_replica_hosts_as_publishers** soll diese Anforderung erfüllen.  
  
     Diese gespeicherte Prozedur wird immer manuell ausgeführt. Der Aufrufer muss entweder **sysadmin** auf dem Verteiler, **dbowner** der Verteilungsdatenbank oder ein Element der **Veröffentlichungszugriffsliste** einer Veröffentlichung der Verlegerdatenbank sein. Außerdem muss die Anmeldung des Aufrufers eine gültige Anmeldung für alle Hosts des Verfügbarkeitsreplikats sein und über SELECT-Privilegien für die der Verlegerdatenbank zugeordnete Verfügbarkeitsdatenbank verfügen.  
  
###  <a name="change-data-capture"></a><a name="CDC"></a> Change Data Capture  
 Für Change Data Capture (CDC) aktivierte Datenbanken können mit [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] nicht nur sicherstellen, dass die Datenbank auch bei einem Fehler verfügbar bleibt, sondern auch, dass Änderungen der Datenbanktabellen weiterhin überwacht und in den CDC-Änderungstabellen abgelegt werden. Die Reihenfolge, in der CDC und [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] konfiguriert werden, ist irrelevant. CDC-fähige Datenbanken können [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]hinzugefügt werden, und Datenbanken, die Mitglieder einer Always On-Verfügbarkeitsgruppe sind, können für CDC aktiviert werden. In beiden Fällen wird die CDC-Konfiguration jedoch immer auf dem aktuellen oder vorgesehenen primären Replikat ausgeführt. CDC verwendet den Protokolllese-Agent. Es gelten die gleichen Einschränkungen wie im Abschnitt **Änderungen des Protokolllese-Agents** weiter oben in diesem Thema beschrieben.  
  
-   **Sammeln von Änderungen für Change Data Capture ohne Replikation**  
  
     Wenn CDC für eine Datenbank aktiviert ist, die Replikation jedoch nicht, wird der zum Sammeln von Änderungen aus dem Protokoll und zum Ablegen in CDC-Änderungstabellen verwendete Aufzeichnungsprozess auf dem CDC-Host als eigener SQL Agent-Auftrag ausgeführt.  
  
     Um das Sammeln von Änderungen nach dem Failover fortzusetzen, muss die gespeicherte Prozedur **sp_cdc_add_job** auf dem neuen primären Element ausgeführt werden, um den lokalen Aufzeichnungsauftrag zu erstellen.  
  
     Im folgenden Beispiel wird der Aufzeichnungsauftrag erstellt.  
  
    ```sql  
    EXEC sys.sp_cdc_add_job @job_type = 'capture';  
    ```  
  
-   **Sammeln von Änderungen für Change Data Capture mit Replikation**  
  
     Wenn CDC und Replikation für eine Datenbank aktiviert sind, behandelt der Protokollleser die Auffüllung der CDC-Änderungstabellen. In diesem Fall stellen die von der Replikation verwendeten Techniken für [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sicher, dass Änderungen nach einem Failover weiterhin aus dem Protokoll gesammelt und in CDC-Änderungstabellen abgelegt werden. Für CDC sind in dieser Konfiguration keine weiteren Aktionen erforderlich, um sicherzustellen, dass die Änderungstabellen aufgefüllt sind.  
  
-   **Change Data Capture – Cleanup**  
  
     Außerdem muss immer ein lokaler Cleanupauftrag erstellt werden, um sicherzustellen, dass der entsprechende Cleanup auch für die neue primäre Datenbank ausgeführt wird. Im folgenden Beispiel wird der Cleanupauftrag erstellt.  
  
    ```sql  
    EXEC sys.sp_cdc_add_job @job_type = 'cleanup';  
    ```  
  
    > [!NOTE]  
    >  Sie müssen die Aufträge nach dem Failover im neuen primären Replikat erstellen. Die CDC-Aufträge, die in der alten primären Datenbank ausgeführt werden, müssen deaktiviert werden, wenn die lokale Datenbank zur sekundären Datenbank wird. Wenn das Replikat danach wieder zum primären Replikat wird, müssen Sie die CDC-Aufträge wieder für das Replikat aktivieren. Verwenden Sie zum Deaktivieren und Aktivieren von Aufträgen die *\@enabled*-Option von [sp_update_job &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md). Weitere Informationen zum Erstellen von CDC-Aufträgen finden Sie unter [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md)unterstützt.  
  
-   **Hinzufügen von CDC-Rollen zu einem primären Always On-Datenbankreplikat**  
  
     Wenn eine Tabelle für CDC aktiviert wird, ist es möglich, der Aufzeichnungsinstanz eine Datenbankrolle zuzuordnen. Wenn eine Rolle angegeben wird, muss der Benutzer, der die CDC-Tabellenwertfunktionen verwenden möchte, um auf Änderungen für die Tabelle zuzugreifen, nicht nur über einen SELECT-Zugriff auf die nachverfolgten Tabellenspalten verfügen. Er muss auch ein Mitglied der benannten Rolle sein. Wenn die angegebene Rolle nicht bereits vorhanden ist, wird die Rolle erstellt. Wenn einer primären Always On-Datenbank automatisch Datenbankrollen hinzugefügt werden, werden die Rollen auch an die sekundären Datenbanken der Verfügbarkeitsgruppe weitergegeben.  
  
-   **Clientanwendungen, die auf CDC-Änderungsdaten zugreifen, und AlwaysOn**  
  
     Clientanwendungen, die die Tabellenwertfunktionen (TVFs) oder Verbindungsserver verwenden, um auf Änderungstabellendaten zuzugreifen, müssen auch in der Lage sein, einen entsprechenden CDC-Host nach dem Failover zu finden. Der Verfügbarkeitsgruppenlistener-Name ist der von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] bereitgestellte Mechanismus, der es Verbindungen transparent ermöglicht, zu einem anderen Host umgeleitet zu werden. Sobald ein Verfügbarkeitsgruppenlistener-Name einer Verfügbarkeitsgruppe zugeordnet ist, ist er in TCP-Verbindungszeichenfolgen verfügbar. Zwei verschiedene Verbindungsszenarien werden durch den Verfügbarkeitsgruppenlistener-Namen unterstützt.  
  
    -   In einem Szenario wird sichergestellt, dass die Verbindungsanforderungen immer an das aktuelle primäre Replikat weitergeleitet werden.  
  
    -   Im anderen Szenario wird sichergestellt, dass die Verbindungsanforderungen an das schreibgeschützte sekundäre Replikat weitergeleitet werden.  
  
     Um nach einem schreibgeschützten sekundären Replikat zu suchen, muss für die Verfügbarkeitsgruppe zudem eine Liste für schreibgeschütztes Routing definiert werden. Weitere Informationen zum Routing des Zugriffs auf lesbare sekundäre Replikate finden Sie unter [So konfigurieren Sie Verfügbarkeitsreplikate für das schreibgeschützte Routing](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md).  
  
    > [!NOTE]  
    >  Es tritt eine gewisse Weitergabeverzögerung auf, die mit der Erstellung eines Verfügbarkeitsgruppenlistener-Namens und dessen Verwendung von Clientanwendungen verbunden ist, um auf ein Verfügbarkeitsgruppen-Datenbankreplikat zuzugreifen.  
  
     Verwenden Sie die folgenden Abfrage, um zu bestimmen, ob ein Verfügbarkeitsgruppenlistener-Name für die Verfügbarkeitsgruppe definiert wurde, die die CDC-Datenbank hostet. Die Abfrage gibt den Verfügbarkeitsgruppenlistener-Namen zurück, wenn er erstellt wurde.  
  
    ```sql  
    SELECT dns_name   
    FROM sys.availability_group_listeners AS l  
    INNER JOIN sys.availability_databases_cluster AS d  
        ON l.group_id = d.group_id  
    WHERE d.database_name = N'MyCDCDB';  
    ```  
  
-   **Umleiten der Abfragelast an ein lesbares sekundäres Replikat**  
  
     Obwohl eine Clientanwendung in vielen Fällen immer eine Verbindung mit dem aktuellen primären Replikat herstellen möchte, ist dies nicht die einzige Möglichkeit, [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] zu nutzen. Wenn eine Verfügbarkeitsgruppe für die Unterstützung von lesbaren sekundären Replikaten konfiguriert wurde, können Änderungsdaten auch von sekundären Knoten erfasst werden.  
  
     Bei der Konfiguration einer Verfügbarkeitsgruppe wird das der SECONDARY_ROLE zugeordnete ALLOW_CONNECTIONS-Attribut verwendet, um den Typ des unterstützten sekundären Zugriffs anzugeben. Bei der Konfiguration als ALL werden alle Verbindungen zum sekundären Replikat zugelassen. Es sind jedoch nur die Verbindungen erfolgreich, die einen schreibgeschützten Zugriff erfordern. Bei der Konfiguration als READ_ONLY ist es bei der Verbindung mit der sekundären Datenbank erforderlich, eine schreibgeschützte Absicht anzugeben, damit die Verbindung erfolgreich hergestellt werden kann. Weitere Informationen finden Sie unter [Konfigurieren des schreibgeschützten Zugriffs auf ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)besitzen.  
  
     Die folgende Abfrage kann verwendet werden, um zu bestimmen, ob eine schreibgeschützte Absicht benötigt wird, um eine Verbindung mit einem lesbaren sekundären Replikat herzustellen.  
  
    ```sql  
    SELECT g.name AS AG, replica_server_name, secondary_role_allow_connections_desc  
    FROM sys.availability_replicas AS r  
    JOIN sys.availability_groups AS g  
        ON r.group_id = g.group_id  
    WHERE g.name = N'MY_AG_NAME';  
    ```  
  
     Zum Suchen des sekundären Replikats kann der Verfügbarkeitsgruppenlistener-Name oder der Name des expliziten Knotens verwendet werden. Wenn der Verfügbarkeitsgruppenlistener-Name verwendet wird, wird der Zugriff für alle passenden sekundären Replikate gewährt.  
  
     Wenn **sp_addlinkedserver** zum Erstellen eines Verbindungsservers verwendet wird, um auf das sekundäre Element zuzugreifen, wird der *\@datasrc*-Parameter für den Verfügbarkeitsgruppenlistener-Namen oder den expliziten Servernamen verwendet, und der *\@provstr*-Parameter wird verwendet, um die schreibgeschützte Absicht anzugeben.  
  
    ```sql  
    EXEC sp_addlinkedserver   
    @server = N'linked_svr',   
    @srvproduct=N'SqlServer',  
    @provider=N'SQLNCLI11',   
    @datasrc=N'AG_Listener_Name',   
    @provstr=N'ApplicationIntent=ReadOnly',   
    @catalog=N'MY_DB_NAME';  
    ```  
  
-   **Clientzugriff auf CDC-Änderungsdaten und Domänenanmeldungen**  
  
     Im Allgemeinen sollten Sie Domänenanmeldungen für Clientzugriff verwenden, um sich in Datenbanken befindliche Daten zu ändern, die Mitglieder von Always On-Verfügbarkeitsgruppen sind. Damit ein ununterbrochener Zugriff auf Änderungsdaten nach einem Failover gewährleistet ist, muss der Domänenbenutzer über Zugriffsberechtigungen für alle Hosts verfügen, die die Verfügbarkeitsgruppenreplikate unterstützen. Wenn einer Datenbank in einem primären Replikat ein Datenbankbenutzer hinzugefügt wird und der Benutzer einer Domänenanmeldung zugeordnet wird, wird der Datenbankbenutzer an sekundäre Datenbanken weitergegeben und weiterhin der angegebenen Domänenanmeldung zugeordnet. Wenn der neue Datenbankbenutzer einer SQL Server-Authentifizierungsanmeldung zugeordnet ist, wird der Benutzer bei den sekundären Datenbanken ohne Anmeldung weitergegeben. Obwohl die zugeordnete [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Authentifizierungsanmeldung verwendet werden könnte, um auf Änderungsdaten auf dem primären Element zuzugreifen, auf dem die Datenbank ursprünglich definiert wurde, wäre dieser Knoten der einzige, auf dem der Zugriff möglich wäre. Die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Authentifizierungsanmeldung wäre nicht in der Lage, auf Daten von anderen sekundären Datenbanken zuzugreifen und auch nicht auf Daten neuer primärer Datenbanken. Sie könnte nur auf Daten der ursprünglichen Datenbank, auf der die Datenbank definiert wurde, zugreifen.  
     
-   **Deaktivieren von Change Data Capture**  
Wenn Change Data Capture in einer Datenbank deaktiviert werden muss, die Teil einer Always On-Verfügbarkeitsgruppe ist, müssen Sie zusätzliche Schritte durchführen, um sicherzustellen, dass die Protokollkürzung nicht beeinträchtigt wird. Sie müssen einen der folgenden Schritte implementieren, um Change Data Capture daran zu hindern, die Protokollkürzung zu blockieren, nachdem Change Data Capture deaktiviert wurde:
    - Starten Sie den SQL Server-Dienst auf jeder sekundären Replikatinstanz neu
    - ODER entfernen Sie die Datenbank aus allen sekundären Replikatinstanzen der Verfügbarkeitsgruppe, und fügen Sie diese der Replikatinstanz der Verfügbarkeitsgruppe mithilfe des automatischen oder manuellen Seeding hinzu.
  
###  <a name="change-tracking"></a><a name="CT"></a> Änderungsnachverfolgung  
 Eine zur Änderungsnachverfolgung (CT) aktivierte Datenbank kann Teil einer Always On-Verfügbarkeitsgruppe sein. Es ist keine zusätzliche Konfiguration erforderlich. Clientanwendungen für die Änderungsnachverfolgung, die die CDC-Tabellenwertfunktionen (TVFs) verwenden, um auf Änderungsdaten zuzugreifen, müssen in der Lage sein, das primäre Replikat nach einem Failover zu suchen. Wenn die Clientanwendung über den Verfügbarkeitsgruppenlistener-Namen eine Verbindung herstellt, werden die Verbindungsanforderungen immer entsprechend an das aktuelle primäre Replikat weitergeleitet.  
  
> [!NOTE]  
>  Änderungsnachverfolgungsdaten müssen immer vom primären Replikat abgerufen werden. Wenn Sie versuchen, auf Änderungsdaten auf einem sekundären Replikat zuzugreifen, tritt der folgende Fehler auf:  
>   
>  Meldung 22117, Ebene 16, Status 1, Zeile 1  
>   
>  Für Datenbanken, die Mitglieder eines sekundären Replikats sind (d. h. für sekundäre Datenbanken), wird die Änderungsnachverfolgung nicht unterstützt. Führen Sie Abfragen zur Änderungsnachverfolgung in den Datenbanken im primären Replikat aus.  
  
##  <a name="prerequisites-restrictions-and-considerations-for-using-replication"></a><a name="Prereqs"></a> Voraussetzungen, Einschränkungen und Überlegungen zum Verwenden der Replikation  
 Dieser Abschnitt enthält Überlegungen zum Bereitstellen der Replikation mit [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]einschließlich Voraussetzungen, Einschränkungen und Empfehlungen.  
  
### <a name="prerequisites"></a>Voraussetzungen  
  
-   Wenn sich bei Verwendung der Transaktionsreplikation die Veröffentlichungsdatenbank in einer Verfügbarkeitsgruppe befindet, muss auf dem der Verleger und Verteiler mindestens [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]ausgeführt werden. Auf dem Abonnenten kann eine frühere [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Version verwendet werden.  
  
-   Verwendung der Mergereplikation, während die Veröffentlichungsdatenbank einer Verfügbarkeitsgruppe angehört:  
  
    -   Pushabonnement: Sowohl auf dem Verleger als auch auf dem Verteiler muss mindestens [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] ausgeführt werden.  
  
    -   Pullabonnement: Die Verleger-, Verteiler- und Abonnentendatenbanken müssen mindestens unter [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] ausgeführt werden. Das liegt daran, dass der Merge-Agent des Abonnenten verstehen muss, wie eine Verfügbarkeitsgruppe ein Failover auf die sekundäre Datenbank ausführen kann.  
  
-   Die Verlegerinstanzen erfüllen alle zur Teilnahme an einer Always On-Verfügbarkeitsgruppe erforderlichen Voraussetzungen. Weitere Informationen finden Sie unter [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)unterstützt.  
  
### <a name="restrictions"></a>Beschränkungen  
 Unterstützte Kombinationen der Replikation in [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]:  
  
|Replikation|Herausgeber|Verteiler|Subscriber|  
|-|-|-|-|  
|**Transaktion**|Ja<br /><br /> Hinweis: Bietet keine Unterstützung für die bidirektionale und wechselseitige Transaktionsreplikation.|Ja|Ja| 
|**P2P**|Nein|Nein|Nein|  
|**Merge** (Zusammenführen)|Ja|Nein|Nein|  
|**Momentaufnahme**|Ja|Nein|Ja|
  
 **Die Verteilerdatenbank wird für die Verwendung mit der Datenbankspiegelung nicht unterstützt.  
  
### <a name="considerations"></a>Überlegungen  
  
-   Die Verteilungsdatenbank wird nicht für die Verwendung mit Datenbankspiegelung unterstützt. Sie wird jedoch mit bestimmten Einschränkungen mit [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] unterstützt. Weitere Informationen finden Sie unter [Konfigurieren der Verteilung in Verfügbarkeitsgruppen](../../../relational-databases/replication/configure-distribution-availability-group.md#limitations-or-exclusions). Die Replikationskonfiguration ist mit der SQL Server-Instanz verknüpft, auf der der Verteiler konfiguriert wird. Daher kann die Verteilungsdatenbank nicht gespiegelt oder repliziert werden. Sie können auch einen SQL Server-Failovercluster verwenden, um Hochverfügbarkeit für den Verteiler bereitzustellen. Weitere Informationen finden Sie unter [Always On-Failoverclusterinstanzen &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)unterstützt.  
  
-   Abonnentenfailover zu einer sekundären Datenbank wird zwar unterstützt, ist aber eine manuelle Prozedur für Abonnenten für Mergereplikation. Die Prozedur ist im Wesentlichen identisch mit der Methode zum Ausführen des Failovers einer gespiegelten Abonnentendatenbank identisch. Abonnenten einer Transaktionsreplikation erfordern keine besonderen Aktionen, solange sie zu [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]gehören. Abonnenten müssen [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] oder höher ausführen, um an einer Verfügbarkeitsgruppe teilzunehmen.  Weitere Informationen finden Sie unter [Replikationsabonnenten und AlwaysOn-Verfügbarkeitsgruppen (SQL Server)](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md).
  
-   Metadaten und Objekte, die außerhalb der Datenbank vorhanden sind, werden nicht an sekundäre Replikate weitergegeben. Das betrifft Benutzernamen, Aufträge, Verbindungsserver usw. Wenn Sie die Metadaten und Objekte in der neuen primären Datenbank nach einem Failover benötigen, müssen Sie diese manuell kopieren. Weitere Informationen finden Sie unter [Verwaltung von Anmeldungen und Aufträgen für die Datenbanken einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/logins-and-jobs-for-availability-group-databases.md)ausgetauscht werden.  

### <a name="distributed-availability-groups"></a>Verteilte Verfügbarkeitsgruppen

Die Verleger- oder Verteilungsdatenbank in einer Verfügbarkeitsgruppe kann nicht als Teil einer verteilten Verfügbarkeitsgruppe konfiguriert werden. Sowohl die Verlegerdatenbank als auch die Verteilungsdatenbank benötigen in einer Verfügbarkeitsgruppe einen Listenerendpunkt, damit sie ordnungsgemäß konfiguriert und verwendet werden können. Es ist jedoch nicht möglich, für eine verteilte Verfügbarkeitsgruppe einen Listenerendpunkt zu konfigurieren.
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
 **Replikation**  
  
-   [Konfigurieren der Replikation für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)  
  
-   [Warten einer Always On-Veröffentlichungsdatenbank &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/maintaining-an-always-on-publication-database-sql-server.md)  
  
-   [Häufig gestellte Fragen für Replikationsadministratoren](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)  
  
 **Erfassung geänderter Daten**  
  
-   [Aktivieren und Deaktivieren von Change Data Capture &#40;SQL Server&#41;](../../../relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server.md)  
  
-   [Verwalten und Überwachen von Change Data Capture &#40;SQL Server&#41;](../../../relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server.md)  
  
-   [Arbeiten mit Änderungsdaten &#40;SQL Server&#41;](../../../relational-databases/track-changes/work-with-change-data-sql-server.md)  
  
 **Änderungsnachverfolgung**  
  
-   [Aktivieren und Deaktivieren der Änderungsnachverfolgung &#40;SQL Server&#41;](../../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)  
  
-   [Verwalten der Änderungsnachverfolgung &#40;SQL Server&#41;](../../../relational-databases/track-changes/manage-change-tracking-sql-server.md)  
  
-   [Verwenden der Änderungsnachverfolgung &#40;SQL Server&#41;](../../../relational-databases/track-changes/work-with-change-tracking-sql-server.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikationsabonnenten und Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)   
 [Voraussetzungen, Einschränkungen und Empfehlungen für Always On-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Always On-Verfügbarkeitsgruppen: Interoperabilität &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [Always On-Failoverclusterinstanzen (SQL Server)](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)   
 [Über Change Data Capture &#40;SQL Server&#41;](../../../relational-databases/track-changes/about-change-data-capture-sql-server.md)   
 [Informationen zur Änderungsnachverfolgung &#40;SQL Server&#41;](../../../relational-databases/track-changes/about-change-tracking-sql-server.md)   
 [SQL Server-Replikation](../../../relational-databases/replication/sql-server-replication.md)   
 [Nachverfolgen von Datenänderungen &#40;SQL Server&#41;](../../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [sys.sp_cdc_add_job (Transact-SQL)](../../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md)  
  
  
