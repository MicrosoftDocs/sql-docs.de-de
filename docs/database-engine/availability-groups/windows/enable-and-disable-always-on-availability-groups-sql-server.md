---
title: Aktivieren oder Deaktivieren der Verfügbarkeitsgruppenfunktion
description: Hier finden Sie die Schritte, mit denen Sie das Feature für Always On-Verfügbarkeitsgruppen mithilfe von Transact-SQL (T-SQL), PowerShell oder SQL Server Management Studio aktivieren oder deaktivieren können.
ms.custom: seodec18
ms.date: 08/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], server instance
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], disabling
- Availability Groups [SQL Server], enabling
ms.assetid: 7c326958-5ae9-4761-9c57-905972276a8f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 419684c0b5299d9bdb500b7c1f23fdef8e4bc987
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349288"
---
# <a name="enable-or-disable-always-on-availability-group-feature"></a>Aktivieren oder Deaktivieren des Features für Always On-Verfügbarkeitsgruppen
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] muss aktiviert werden, damit eine Serverinstanz Verfügbarkeitsgruppen verwenden kann. Bevor Sie eine beliebige Verfügbarkeitsgruppe erstellen und konfigurieren können, muss die Funktion [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] für jede Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] aktiviert worden sein, auf der ein Verfügbarkeitsreplikat für mindestens eine Verfügbarkeitsgruppe gehostet wird.  
  
> [!IMPORTANT]  
>  Wenn Sie einen WSFC-Cluster löschen und neu erstellen, müssen Sie die Funktion [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] für jede Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] deaktivieren und erneut aktivieren, auf der auf dem ursprünglichen WSFC-Cluster ein Verfügbarkeitsreplikat gehostet wurde.  
  
  
##  <a name="prerequisites-for-enabling-always-on-availability-groups"></a><a name="Prerequisites"></a> Voraussetzungen zum Aktivieren von AlwaysOn-Verfügbarkeitsgruppen  
  
-   Die Serverinstanz muss sich auf einem WSFC-Knoten (Windows Server Failover Clustering) befinden.  
  
-   Auf der Serverinstanz muss eine Edition von SQL Server ausgeführt werden, die [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]unterstützt. Weitere Informationen finden Sie unter [Von den SQL Server 2016-Editionen unterstützte Funktionen](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   AlwaysOn-Verfügbarkeitsgruppen sollten jeweils nur für eine Serverinstanz aktiviert werden. Warten Sie nach der Aktivierung von AlwaysOn-Verfügbarkeitsgruppen, bis der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Dienst neu gestartet wurde, bevor Sie mit einer anderen Serverinstanz fortfahren.  
  
 Informationen zu zusätzlichen Voraussetzungen zum Erstellen und Konfigurieren von Verfügbarkeitsgruppen finden Sie unter [Voraussetzungen, Einschränkungen und Empfehlungen für AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
## <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Während AlwaysOn-Verfügbarkeitsgruppen auf einer Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]aktiviert sind, verfügt die Serverinstanz über Vollzugriff auf den WSFC-Cluster.  

 Erfordert auf dem lokalen Computer die Mitgliedschaft in der Gruppe **Administrator** und Vollzugriff auf den WSFC-Cluster. Wenn Sie AlwaysOn mit PowerShell aktivieren, öffnen Sie das Eingabeaufforderungsfenster unter Verwendung der Option **Als Administrator ausführen** .  
  
 Erfordert, dass von Active Directory Objekte erstellt und Objektberechtigungen verwaltet werden.  
  
##  <a name="determine-whether-always-on-availability-groups-is-enabled"></a><a name="IsEnabled"></a> Bestimmen, ob AlwaysOn-Verfügbarkeitsgruppen aktiviert sind  
  
-   [SQL Server Management Studio](#SSMS1Procedure)  
  
-   [Transact-SQL](#Tsql1Procedure)  
  
-   [PowerShell](#PowerShell1Procedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMS1Procedure"></a> Verwenden von SQL Server Management Studio  
 **So ermitteln Sie, ob AlwaysOn-Verfügbarkeitsgruppen aktiviert sind**  
  
1.  Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf die Serverinstanz, und klicken Sie auf **Eigenschaften**.  
  
2.  Klicken Sie im Dialogfeld **Servereigenschaften** auf die Seite **Allgemein** . Die Eigenschaft **Ist HADR-aktiviert** zeigt einen der folgenden Werte an:  
  
    -   **True**, wenn AlwaysOn-Verfügbarkeitsgruppen aktiviert sind.  
  
    -   **False**, wenn AlwaysOn-Verfügbarkeitsgruppen deaktiviert sind.  
  
###  <a name="using-transact-sql"></a><a name="Tsql1Procedure"></a> Verwenden von Transact-SQL  
 **So ermitteln Sie, ob AlwaysOn-Verfügbarkeitsgruppen aktiviert sind**  
  
1.  Verwenden Sie die folgende [SERVERPROPERTY](../../../t-sql/functions/serverproperty-transact-sql.md) -Anweisung:  
  
    ```  
    SELECT SERVERPROPERTY ('IsHadrEnabled');  
    ```  
  
     Die Einstellung der **IsHadrEnabled** -Servereigenschaft gibt folgendermaßen an, ob eine Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] für AlwaysOn-Verfügbarkeitsgruppen aktiviert ist:  
  
    -   Falls **IsHadrEnabled** = 1, sind AlwaysOn-Verfügbarkeitsgruppen aktiviert.  
  
    -   Falls **IsHadrEnabled** = 0, sind AlwaysOn-Verfügbarkeitsgruppen deaktiviert.  
  
    > [!NOTE]  
    >  Weitere Informationen zur **IsHadrEnabled** -Servereigenschaft finden Sie unter [SERVERPROPERTY &#40;Transact-SQL&#41;](../../../t-sql/functions/serverproperty-transact-sql.md)unterstützt.  
  
###  <a name="using-powershell"></a><a name="PowerShell1Procedure"></a> PowerShell  
 **So ermitteln Sie, ob AlwaysOn-Verfügbarkeitsgruppen aktiviert sind**  
  
1.  Legen Sie die Serverinstanz als Standard fest (**cd**), auf der Sie ermitteln möchten, ob [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] aktiviert ist.  
  
2.  Geben Sie den folgenden **Get-Item** -Befehl von PowerShell ein:  
  
    ```  
    PS SQLSERVER:\SQL\NODE1\DEFAULT> get-item . | select IsHadrEnabled  
    ```  
  
    > [!NOTE]  
    >  Um die Syntax eines Cmdlets anzuzeigen, verwenden Sie das **Get-Help** -Cmdlet in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell-Umgebung. Weitere Informationen finden Sie unter [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Einrichten und Verwenden des SQL Server PowerShell-Anbieters**  
  
-   [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="enable-always-on-availability-groups"></a><a name="EnableAOAG"></a> Aktivieren von AlwaysOn-Verfügbarkeitsgruppen  
 **So aktivieren Sie AlwaysOn mit der folgenden Option:**  
  
-   [SQL Server-Konfigurations-Manager](#SQLCM2Procedure)  
  
-   [PowerShell](#PScmd2Procedure)  
  
###  <a name="using-sql-server-configuration-manager"></a><a name="SQLCM2Procedure"></a> Verwenden des SQL Server-Konfigurations-Managers  
 **So aktivieren Sie Always On-Verfügbarkeitsgruppen**  
  
1.  Stellen Sie eine Verbindung mit dem WSFC-Knoten (Windows Server Failover Cluster) her, auf dem die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz gehostet wird, auf der Sie Always On-Verfügbarkeitsgruppen aktivieren möchten.  
  
2.  Zeigen Sie im Menü **Start** auf **Alle Programme**, zeigen Sie auf [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)], zeigen Sie auf **Konfigurationstools**, und klicken Sie dann auf **SQL Server Konfigurations-Manager**.  
  
3.  Klicken Sie im **SQL Server-Konfigurations-Manager** auf **SQL Server-Dienste** und mit der rechten Maustaste auf SQL Server ( **\<**_instance name_**>)** , wobei **\<**_instance name_**>** der Name einer lokalen Serverinstanz ist, für die Sie AlwaysOn-Verfügbarkeitsgruppen aktivieren möchten, und klicken Sie dann auf **Eigenschaften**.  
  
4.  Wählen Sie die Registerkarte **Hohe Verfügbarkeit mit AlwaysOn** aus.  
  
5.  Überprüfen Sie, ob das Feld **Name des Windows-Failoverclusters** den Namen des lokalen Failoverclusters enthält. Wenn dieses Feld leer ist, wird [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]von dieser Serverinstanz derzeit nicht unterstützt. Der lokale Computer ist kein Clusterknoten, der WSFC-Cluster wurde geschlossen oder diese Edition von [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] unterstützt [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]nicht.  
  
6.  Aktivieren Sie das Kontrollkästchen **AlwaysOn-Verfügbarkeitsgruppen aktivieren** , und klicken Sie auf **OK**.  
  
     [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Konfigurations-Manager speichert die Änderung. Dann müssen Sie den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienst manuell neu starten. Dies ermöglicht die Auswahl einer für die Geschäftsanforderungen optimalen Neustartzeit. Wenn der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienst neu gestartet wird, wird AlwaysOn aktiviert, und die **IsHadrEnabled** -Servereigenschaft wird auf 1 festgelegt.  
  
###  <a name="using-sql-server-powershell"></a><a name="PScmd2Procedure"></a> SQL Server PowerShell  
 **So aktivieren Sie AlwaysOn**  
  
1.  Wechseln Sie in das Verzeichnis einer Serverinstanz (**cd**), die Sie für AlwaysOn-Verfügbarkeitsgruppen aktivieren möchten.  
  
2.  Verwenden Sie das Cmdlet **Enable-SqlAlwaysOn** um Always On-Verfügbarkeitsgruppen zu aktivieren.  
  
     Um die Syntax eines Cmdlets anzuzeigen, verwenden Sie das **Get-Help** -Cmdlet in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell-Umgebung. Weitere Informationen finden Sie unter [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
    > [!NOTE]  
    >  Informationen zur Steuerung des Neustarts des **-Diensts durch das** Enable-SqlAlwaysOn [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Cmdlet finden Sie unter [Wann startet ein Cmdlet den SQL Server-Dienst neu?](#WhenCmdletRestartsSQL) weiter unten in diesem Thema.  
  
 **Einrichten und Verwenden des SQL Server PowerShell-Anbieters**  
  
-   [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md)  
  
####  <a name="example-enable-sqlalwayson"></a><a name="ExmplEnable-SqlHadrServic"></a> Beispiel: Enable-SqlAlwaysOn  
 Durch den folgenden PowerShell-Befehl wird [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] auf einer Instanz von SQL Server (*Computer*\\*Instanz*) aktiviert.  
  
```  
Enable-SqlAlwaysOn -Path SQLSERVER:\SQL\Computer\Instance  
```  
  
##  <a name="disable-always-on-availability-groups"></a><a name="DisableAOAG"></a> Deaktivieren von AlwaysOn-Verfügbarkeitsgruppen  
  
-   **Vor dem Deaktivieren von AlwaysOn:**  
  
     [Empfehlungen](#Recommendations)  
  
-   **So deaktivieren Sie AlwaysOn mit der folgenden Option**  
  
    -   [SQL Server-Konfigurations-Manager](#SQLCM3Procedure)  
  
    -   [PowerShell](#PScmd3Procedure)  
  
-   **Nachverfolgung:**  [Nach dem Deaktivieren von Always On](#FollowUp)  
  
> [!IMPORTANT]  
>  Deaktivieren Sie AlwaysOn jeweils nur auf einer Serverinstanz. Warten Sie nach der Deaktivierung von AlwaysOn-Verfügbarkeitsgruppen, bis der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienst neu gestartet wurde, bevor Sie mit einer anderen Serverinstanz fortfahren.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen  
 Bevor Sie AlwaysOn auf einer Serverinstanz deaktivieren, sollten Sie wie folgt vorgehen:  
  
1.  Wenn das primäre Replikat einer Verfügbarkeitsgruppe, die beibehalten werden soll, derzeit auf einer Serverinstanz gehostet wird, sollten Sie nach Möglichkeit manuell ein Failover für die Verfügbarkeitsgruppe zum synchronisierten sekundären Replikat ausführen. Weitere Informationen finden Sie unter [Ausführen eines geplanten manuellen Failovers einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md).  
  
2.  Entfernen Sie alle lokalen sekundären Replikate. Weitere Informationen finden Sie unter [Entfernen eines sekundären Replikats aus einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md).  
  
###  <a name="using-sql-server-configuration-manager"></a><a name="SQLCM3Procedure"></a> Verwenden des SQL Server-Konfigurations-Managers  
 **So deaktivieren Sie AlwaysOn**  
  
1.  Stellen Sie eine Verbindung mit dem WSFC-Knoten (Windows Server Failover Cluster)her, auf dem die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz gehostet wird, auf der Sie Always On-Verfügbarkeitsgruppen deaktivieren möchten.  
  
2.  Zeigen Sie im Menü **Start** auf **Alle Programme**, zeigen Sie auf [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)], zeigen Sie auf **Konfigurationstools**, und klicken Sie dann auf **SQL Server-Konfigurations-Manager**.  
  
3.  Klicken Sie im **SQL Server-Konfigurations-Manager** auf **SQL Server-Dienste** und mit der rechten Maustaste auf SQL Server ( **\<**_instance name_**>)** , wobei **\<**_instance name_**>** der Name einer lokalen Serverinstanz ist, für die Sie AlwaysOn-Verfügbarkeitsgruppen deaktivieren möchten, und klicken Sie dann auf **Eigenschaften**.  
  
4.  Deaktivieren Sie auf der Registerkarte **Hohe Verfügbarkeit mit AlwaysOn** das Kontrollkästchen **AlwaysOn-Verfügbarkeitsgruppen aktivieren**, und klicken Sie auf **OK**.  
  
     [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Konfigurations-Manager speichert die Änderung und startet den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienst neu. Beim Neustart des [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Diensts wird AlwaysOn deaktiviert. Zudem wird die **IsHadrEnabled** -Servereigenschaft auf 0 festgelegt, um anzuzeigen, dass AlwaysOn-Verfügbarkeitsgruppen deaktiviert sind.  
  
5.  Es wird empfohlen, dass Sie die Informationen unter [Nächster Schritt: Nach dem Deaktivieren von Always On](#FollowUp) weiter unten in diesem Thema lesen.  
  
###  <a name="using-sql-server-powershell"></a><a name="PScmd3Procedure"></a> SQL Server PowerShell  
 **So deaktivieren Sie AlwaysOn**  
  
1.  Wechseln Sie in das Verzeichnis (**cd**) einer derzeit aktivierten Serverinstanz, die Sie für AlwaysOn-Verfügbarkeitsgruppen deaktivieren möchten.  
  
2.  Verwenden Sie das **Disable-SqlAlwaysOn**-Cmdlet, um die Always On-Verfügbarkeitsgruppen zu aktivieren.  
  
     Beispielsweise werden durch den folgenden Befehl AlwaysOn-Verfügbarkeitsgruppen auf der SQL Serverinstanz (*Computer*\\*Instanz*) deaktiviert.  Dieser Befehl erfordert einen Neustart der Instanz, und Sie werden aufgefordert, diesen Neustart zu bestätigen.  
  
    ```  
    Disable-SqlAlwaysOn -Path SQLSERVER:\SQL\Computer\Instance  
    ```  
  
    > [!IMPORTANT]  
    >  Informationen zur Steuerung, ob das Cmdlet **Disable-SqlAlwaysOn** den [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Dienst neu startet, finden Sie unter [Wann startet ein Cmdlet den SQL Server-Dienst neu?](#WhenCmdletRestartsSQL) weiter unten in diesem Thema.  
  
     Um die Syntax eines Cmdlets anzuzeigen, verwenden Sie das **Get-Help** -Cmdlet in der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell-Umgebung. Weitere Informationen finden Sie unter [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Einrichten und Verwenden des SQL Server PowerShell-Anbieters**  
  
-   [SQL Server PowerShell-Anbieter](../../../powershell/sql-server-powershell-provider.md)  
  
###  <a name="follow-up-after-disabling-always-on"></a><a name="FollowUp"></a>Nächster Schritt: Nach dem Deaktivieren von Always On  
 Nachdem Sie AlwaysOn-Verfügbarkeitsgruppen deaktiviert haben, muss die Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] neu gestartet werden. Die Serverinstanz wird vom SQL-Konfigurations-Manager automatisch neu gestartet. Wenn Sie jedoch das **Disable-SqlAlwaysOn** -Cmdlet verwendet haben, müssen Sie die Serverinstanz manuell neu starten. Weitere Informationen finden Sie unter [sqlservr Application](../../../tools/sqlservr-application.md).  
  
 Auf der neu gestarteten Serverinstanz:  
  
-   Verfügbarkeitsdatenbanken werden bei SQL Server-Start nicht gestartet. Daher kann nicht auf sie zugegriffen werden.  
  
-   Die einzige unterstützte AlwaysOn- [!INCLUDE[tsql](../../../includes/tsql-md.md)] -Anweisung ist [DROP AVAILABILITY GROUP](../../../t-sql/statements/drop-availability-group-transact-sql.md). Die Optionen CREATE AVAILABILITY GROUP, ALTER AVAILABILITY GROUP und SET HADR von ALTER DATABASE werden nicht unterstützt.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Metadaten und [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] -Konfigurationsdaten in WSFC sind von der Deaktivierung der AlwaysOn-Verfügbarkeitsgruppen nicht betroffen.  
  
 Wenn Sie AlwaysOn-Verfügbarkeitsgruppen dauerhaft auf jeder Serverinstanz deaktivieren, die ein Verfügbarkeitsreplikat für eine oder mehrere Verfügbarkeitsgruppen hostet, empfiehlt es sich, dass Sie die folgenden Schritte ausführen:  
  
1.  Wenn Sie die lokalen Verfügbarkeitsreplikate nicht vor dem Deaktivieren von AlwaysOn entfernt haben, löschen Sie jede Verfügbarkeitsgruppe, für die die Serverinstanz ein Verfügbarkeitsreplikat hostet. Informationen zum Löschen einer Verfügbarkeitsgruppe finden Sie unter [Entfernen einer Verfügbarkeitsgruppe &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md).  
  
2.  Um die zurückgelassenen Metadaten zu entfernen, löschen Sie jede betroffene Verfügbarkeitsgruppe auf einer Serverinstanz, die Teil des ursprünglichen WSFC ist.  
  
3.  Auf die primären Datenbanken kann weiterhin anhand aller Verbindungen außer der Datensynchronisierung zwischen der primären und der sekundären Datenbank zugegriffen werden.  
  
4.  Die sekundären Datenbanken übernehmen den Status RESTORING. Sie können sie entweder löschen oder sie mit RESTORE WITH RECOVERY wiederherstellen. Wiederhergestellte Datenbanken nehmen jedoch nicht mehr an der Datensynchronisierung für Verfügbarkeitsgruppen teil.  
  
##  <a name="when-does-a-cmdlet-restart-the-sql-server-service"></a><a name="WhenCmdletRestartsSQL"></a> Wann startet ein Cmdlet den SQL Server-Dienst neu?  
 Auf einer aktuell ausgeführten Serverinstanz kann die Verwendung von **Enable-SqlAlwaysOn** oder **Disable-SqlAlwaysOn** zum Ändern der aktuellen Always On-Einstellung zu einem Neustart des SQL Server-Diensts führen. Das Neustartverhalten hängt von den folgenden Bedingungen ab:  
  
|-NoServiceRestart-Parameter angegeben|-Force-Parameter angegeben|Wurde der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Dienst neu gestartet?|  
|--------------------------------------------|---------------------------------|---------------------------------------------------------|  
|Nein|Nein|Standardmäßig. Aber das Cmdlet fordert Sie folgendermaßen auf:<br /><br /> **Um diese Aktion abzuschließen, müssen wir den SQL Server-Dienst für die Serverinstanz "<instance_name>" neu starten. Möchten Sie den Vorgang fortsetzen?**<br /><br /> **[Y] Ja  [N] Nein  [S] Anhalten  [?] Hilfe (Standard ist „Y"):**<br /><br /> Wenn Sie **N** oder **S** angeben, wird der Dienst nicht neu gestartet.|  
|Nein|Ja|Der Dienst wird neu gestartet.|  
|Ja|Nein|Der Dienst wird nicht neu gestartet.|  
|Ja|Ja|Der Dienst wird nicht neu gestartet.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht über AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [SERVERPROPERTY &#40;Transact-SQL&#41;](../../../t-sql/functions/serverproperty-transact-sql.md)  
  
