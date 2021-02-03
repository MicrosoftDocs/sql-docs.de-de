---
title: Erstellen einer Serverüberwachung und einer Serverüberwachungsspezifikation
description: Hier erfahren Sie, wie Sie eine Server- und Serverüberwachungsspezifikation für SQL Server erstellen. Dazu verwenden Sie SQL Server Management Studio (SSMS) oder Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 10/16/2019
ms.prod: sql
ms.prod_service: security
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.SWB.SQLAUDIT.FILTER.F1
- sql13.swb.sqlaudit.general.f1
- sql13.swb.sqlaudit.srvaudit.general.f1
helpviewer_keywords:
- server audit [SQL Server]
- audits [SQL Server], specification
ms.assetid: 6624b1ab-7ec8-44ce-8292-397edf644394
author: DavidTrigano
ms.author: datrigan
ms.openlocfilehash: 3db484c1279546ab76ebf302582dbb984f991fa4
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250119"
---
# <a name="create-a-server-audit-and-server-audit-specification"></a>Erstellen einer Serverüberwachung und einer Serverüberwachungsspezifikation
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie eine Serverüberwachung und Serverüberwachungsspezifikation in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mithilfe von [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../../includes/tsql-md.md)]erstellt wird. Bei der *Überwachung* einer Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] oder einer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Datenbank werden Ereignisse im System verfolgt und protokolliert. Das *SQL Server Audit* -Objekt listet eine einzelne Instanz an Aktionen oder Aktionsgruppen auf Server- oder Datenbankebene auf, die überwacht werden soll. Die Überwachung wird auf [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanzebene ausgeführt. Es können mehrere Überwachungen pro [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanz vorliegen. Das *Serverüberwachungsspezifikation* -Objekt gehört zu einer Überwachung. Sie können eine Serverüberwachungsspezifikation pro Überwachung erstellen, da beide im [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanzbereich erstellt werden. Weitere Informationen finden Sie unter [SQL Server Audit &#40;Datenbank-Engine&#41;](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Einschränkungen](#Restrictions)  
  
     [Security](#Security)  
  
-   **So erstellen Sie eine Serverüberwachung und eine Serverüberwachungsspezifikation mit**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Eine Überwachung muss vorhanden sein, bevor Sie eine Serverüberwachungsspezifikation für sie erstellen. Wenn eine Serverüberwachungsspezifikation erstellt wird, befindet sie sich im deaktivierten Zustand.  
  
-   Die CREATE SERVER AUDIT-Anweisung liegt im Bereich einer Transaktion. Wird ein Rollback für die Transaktion ausgeführt, so wird auch für die Anweisung ein Rollback durchgeführt.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
  
-   Um eine Serverüberwachung zu erstellen, zu ändern oder zu löschen, benötigen Prinzipale die ALTER ANY SERVER AUDIT-Berechtigung oder die CONTROL SERVER-Berechtigung.  
  
-   Benutzer mit der Berechtigung ALTER ANY SERVER AUDIT können Serverüberwachungsspezifikationen erstellen und diese an eine beliebige Überwachung binden.  
  
-   Sobald eine Serverüberwachungsspezifikation erstellt wurde, kann sie von Prinzipalen mit den folgenden Berechtigungen eingesehen werden: CONTROL SERVER oder ALTER ANY SERVER AUDIT. Außerdem kann sie von Prinzipalen eingesehen werden, die über das „sysadmin“-Konto oder expliziten Zugriff auf die Überwachung verfügen.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-create-a-server-audit"></a>So erstellen Sie eine Serverüberwachung  
  
1.  Erweitern Sie im Objekt-Explorer den Ordner **Sicherheit** .  
  
2.  Klicken Sie mit der rechten Maustaste auf den Ordner **Überwachungen**, und wählen Sie dann **Neue Überwachung...** aus.  
  
     Die folgenden Optionen befinden sich im Dialogfeld **Überwachung erstellen** auf der Seite **Allgemein** :  
  
     **Überwachungsname**  
     Der Name der Überwachung. Der Name wird automatisch generiert, wenn Sie eine neue Überwachung erstellen, er kann jedoch bearbeitet werden.  
  
     **Warteschlangenverzögerung (in Millisekunden)**  
     Gibt in Millisekunden den Zeitraum an, der verstreichen kann, bevor die Verarbeitung von Überwachungsaktionen erzwungen wird.  Der Wert 0 steht für eine synchrone Übermittlung. Der standardmäßige Mindestwert beträgt **1000** (1 Sekunde). Der maximale Wert beträgt 2.147.483.647 (2.147.483,647 Sekunden oder 24 Tage, 20 Stunden, 31 Minuten und 23,647 Sekunden).  
  
     **Bei Überwachungsprotokollfehler:**  
     **Fortsetzen**  
     [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Vorgänge werden fortgesetzt. Überwachungsdatensätze werden nicht beibehalten. Die Überwachung versucht weiterhin, Ereignisse zu protokollieren und wird fortgesetzt, wenn die Fehlerbedingung aufgelöst wurde. Durch Auswählen der **Continue** -Option können unter Umständen unüberwachte Aktivitäten ausgeführt werden, die gegen Ihre Sicherheitsrichtlinien verstoßen. Wählen Sie diese Option aus, wenn die weitere Verwendung von [!INCLUDE[ssDE](../../../includes/ssde-md.md)] wichtiger als die Beibehaltung einer vollständigen Überwachung ist. Dies ist die Standardauswahl.  
  
     **Herunterfahren des Servers**  
     Erzwingt, dass ein Server heruntergefahren wird, wenn die Serverinstanz, die in das Ziel schreiben soll, keine Daten in das Überwachungsziel schreiben kann. Die Anmeldung, die dies ausgibt, muss über die **SHUTDOWN** -Berechtigung verfügen. Wenn die Anmeldung nicht über diese Berechtigung verfügt, schlägt diese Funktion fehl, und es wird eine Fehlermeldung ausgegeben. Es treten keine überwachten Ereignisse auf. Wählen Sie diese Option aus, wenn ein Überwachungsfehler die Sicherheit oder die Integrität des Systems beeinträchtigen konnte.  
  
     **Fehler bei Vorgang**  
     In Fällen, in denen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Audit keine Informationen in das Überwachungsprotokoll schreiben kann, bewirkt diese Option, dass Datenbankaktionen fehlschlagen, wenn sie andernfalls überwachte Ereignisse verursachen würden. Es treten keine überwachten Ereignisse auf. Aktionen, die keine überwachten Ereignisse verursachen, können fortgesetzt werden. Die Überwachung versucht weiterhin, Ereignisse zu protokollieren und wird fortgesetzt, wenn die Fehlerbedingung aufgelöst wurde. Wählen Sie diese Option aus, wenn die Beibehaltung einer vollständigen Überwachung wichtiger als der Vollzugriff auf [!INCLUDE[ssDE](../../../includes/ssde-md.md)]ist.  
  
    > [!IMPORTANT]  
    >  Wenn sich die Überwachung in einem fehlerhaften Status befindet, kann die dedizierte Administratorverbindung weiterhin überwachte Ereignisse ausführen.  
  
     **Überwachungsziel** (Liste)  
     Gibt das Ziel für die Überwachungsdaten an. Die verfügbaren Optionen sind eine Binärdatei, das Windows-Anwendungsprotokoll oder das Windows-Sicherheitsprotokoll. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] kann nicht in das Windows-Sicherheitsprotokoll schreiben, ohne zusätzliche Einstellungen in Windows zu konfigurieren. Weitere Informationen finden Sie unter [Schreiben von SQL-Serverüberwachungsereignissen in das Sicherheitsprotokoll](../../../relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log.md).  
  
     **Dateipfad**  
     Gibt den Speicherort des Ordners an, in den Überwachungsdaten geschrieben werden, wenn das **Überwachungsziel** eine Datei ist.  
  
     **Auslassungspunkte (…)**  
     Öffnet das Dialogfeld **Ordner suchen –** _Servername_, um einen Dateipfad anzugeben oder einen Ordner zu erstellen, in dem die Überwachungsdatei geschrieben werden soll.  
  
     **Maximale Grenze für Überwachungsdatei:**  
     **Maximale Anzahl Rolloverdateien**  
     Wenn die maximale Anzahl von Überwachungsdateien erreicht wird, werden die ältesten Überwachungsdateien durch neuen Dateiinhalt überschrieben.  
  
     **Maximale Dateien**  
     Wenn die maximale Anzahl von Überwachungsdateien erreicht wird, tritt bei jeder Aktion, durch die zusätzliche Überwachungsereignisse verursacht werden, ein Fehler auf.  
  
     **Unbegrenzt** (Kontrollkästchen)  
     Wenn das Kontrollkästchen **Unbegrenzt** unter **Maximale Anzahl Rolloverdateien** aktiviert ist, kann eine unbegrenzte Anzahl von Rolloverdateien erstellt werden. Das Kontrollkästchen **Unbegrenzt** ist standardmäßig aktiviert und gilt sowohl für **Maximale Anzahl Rolloverdateien** als auch für **Maximale Dateien** .  
  
     **Anzahl der Dateien** (Feld)  
     Gibt die Anzahl von Überwachungsdateien an, die erstellt werden können, und zwar bis zu 2.147.483.647. Diese Option ist nur verfügbar, wenn **Unbegrenzt** deaktiviert ist.  
  
     **Maximale Dateigröße**  
     Gibt die maximale Größe für eine Überwachungsdatei entweder in Megabyte (MB), Gigabyte (GB) oder Terabyte (TB) an. Sie können eine Zahl bis 2.147.483.647 angeben. Durch Aktivieren des Kontrollkästchen **Unbegrenzt** gibt es keine Begrenzung der Dateigröße. Das Kontrollkästchen **Unbegrenzt** ist standardmäßig aktiviert.  
  
     **Speicherplatz reservieren** (Kontrollkästchen)  
     Gibt an, dass der auf dem Datenträger vorab zugeordnete Speicherplatz der festgelegten maximalen Dateigröße entspricht. Diese Einstellung kann nur verwendet werden, wenn das Kontrollkästchen **Unbegrenzt** unter **Maximale Dateigröße** deaktiviert ist. Dieses Kontrollkästchen ist standardmäßig deaktiviert.  
  
3.  Geben Sie auf der Seite **Filter** optional ein Prädikat oder eine `WHERE` -Klausel für die Serverüberwachung ein, um Optionen anzugeben, die auf der Seite **Allgemein** nicht verfügbar sind. Schließen Sie das Prädikat in Klammern ein; z. B.: `(object_name = 'EmployeesTable')`.  
  
4.  Nachdem Sie alle Optionen ausgewählt haben, klicken Sie auf **OK**.  
  
#### <a name="to-create-a-server-audit-specification"></a>So erstellen Sie eine Serverüberwachungsspezifikation  
  
1.  Klicken Sie im Objekt-Explorer auf das Pluszeichen, um den Ordner **Sicherheit** zu erweitern.  
  
2.  Klicken Sie mit der rechten Maustaste auf den Ordner **Serverüberwachungsspezifikationen**, und wählen Sie dann **Neue Serverüberwachungsspezifikation...** aus.  
  
     Die folgenden Optionen sind im Dialogfeld **Serverüberwachungsspezifikation erstellen** verfügbar.  
  
     **Name**  
     Der Name der Serverüberwachungsspezifikation. Er wird automatisch generiert, wenn Sie eine neue Serverüberwachungsspezifikation erstellen, er kann jedoch bearbeitet werden.  
  
     **Überwachung**  
     Der Name einer vorhandenen Serverüberwachung. Geben Sie den Namen der Überwachung ein, oder wählen Sie ihn aus der Liste aus.  
  
     **Überwachungsaktionstyp**  
     Gibt die aufzuzeichnenden Überwachungsaktionsgruppen auf Serverebene und Überwachungsaktionen an. Eine Liste der Überwachungsaktionsgruppen auf Serverebene und Überwachungsaktionen sowie eine Beschreibung der darin enthaltenen Ereignisse finden Sie unter [SQL Server Audit-Aktionsgruppen und -Aktionen](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
     **Objektschema**  
     Zeigt das Schema für den angegebenen **Objektnamen** an.  
  
     **Objektnamen**  
     Der Name des zu überwachenden Objekts. Das Objekt ist nur für Überwachungsaktionen verfügbar, es gilt nicht für Überwachungsgruppen.  
  
     **Auslassungspunkte (…)**  
     Öffnet das Dialogfeld **Objekte auswählen** , in dem Sie anhand des angegebenen **Überwachungsaktionstyps** nach einem verfügbaren Objekt suchen und es auswählen können.  
  
     **Prinzipalname**  
     Das Konto, anhand dessen die Überwachung für das zu überwachende Objekt gefiltert wird.  
  
     **Auslassungspunkte (…)**  
     Öffnet das Dialogfeld **Objekte auswählen** , in dem Sie nach einem verfügbaren Objekt anhand des angegebenen **Objektnamens** suchen und es auswählen können.  
  
3.  Wenn Sie fertig sind, klicken Sie auf **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
  
#### <a name="to-create-a-server-audit"></a>So erstellen Sie eine Serverüberwachung  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../../includes/ssde-md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**. 
  
    ```  
    -- Creates a server audit called "HIPAA_Audit" with a binary file as the target and no options.  
    CREATE SERVER AUDIT HIPAA_Audit  
        TO FILE ( FILEPATH ='E:\SQLAudit\' );  
    ```  
> [!NOTE]
> Obwohl Sie einen UNC-Pfad als Überwachungsdateiziel verwenden können, sollten Sie Vorsicht walten lassen. Wenn für diese Dateifreigabe eine Netzwerklatenz auftritt, können bei SQL Server Leistungseinbußen auftreten, da Threads auf den Abschluss eines Überprüfungsschreibvorgangs warten würden, bevor sie den Vorgang fortsetzen. Im SQL Server-Fehlerprotokoll, z. B. 17894, können Sie verschiedene Fehlermeldungen beobachten:
>
>   2020-02-07 12:21:35.100 Serververteiler (0x7954) aus dem Verteilerpool „XE Engine-Hauptdispatcherpool“ Worker 0x00000058e7300000 scheint auf Knoten 0 keine Ergebnisse zu liefern.


#### <a name="to-create-a-server-audit-specification"></a>So erstellen Sie eine Serverüberwachungsspezifikation  
  
1.  Stellen Sie im **Objekt-Explorer** eine Verbindung mit einer [!INCLUDE[ssDE](../../../includes/ssde-md.md)]-Instanz her.  
  
2.  Klicken Sie in der Standardleiste auf **Neue Abfrage**.  
  
3.  Kopieren Sie das folgende Beispiel, fügen Sie es in das Abfragefenster ein, und klicken Sie auf **Ausführen**.  
  
    ```  
    /*Creates a server audit specification called "HIPAA_Audit_Specification" that audits failed logins for the SQL Server audit "HIPAA_Audit" created above.  
    */  
  
    CREATE SERVER AUDIT SPECIFICATION HIPAA_Audit_Specification  
    FOR SERVER AUDIT HIPAA_Audit  
        ADD (FAILED_LOGIN_GROUP);  
    GO  
    -- Enables the audit.   
  
    ALTER SERVER AUDIT HIPAA_Audit  
    WITH (STATE = ON);  
    GO  
    ```  
  
 Weitere Informationen finden Sie unter [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-audit-transact-sql.md) und [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-audit-specification-transact-sql.md).  
  
  
