---
description: MSSQLSERVER_17892
title: MSSQLSERVER_17892
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17892 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 052c8c5670ba079cac980151f123cd8c35fd53b1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345561"
---
# <a name="mssqlserver_17892"></a>MSSQLSERVER_17892
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|17892|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|SRV_LOGON_FAILED_BY_TRIGGER|
|Meldungstext|Fehler beim Anmelden für den Anmeldenamen \<Login Name> wegen Triggerausführung.|
||

## <a name="explanation"></a>Erklärung

Die Fehlermeldung 17892 wird angezeigt, wenn ein Anmeldetriggercode nicht erfolgreich ausgeführt werden kann. [Anmeldetrigger](../triggers/logon-triggers.md) lösen gespeicherte Prozeduren als Antwort auf ein LOGON-Ereignis aus. Dieses Ereignis wird ausgelöst, wenn eine Benutzersitzung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]erstellt wird. Dem Benutzer wird eine Fehlermeldung wie die folgende angezeigt:

> Meldung 17892, Ebene 14, Status 1, Server \<Server Name>, Zeile 1  
Fehler beim Anmelden für den Anmeldenamen \<Login Name> wegen Triggerausführung.

## <a name="possible-causes"></a>Mögliche Ursachen

Das Problem kann auftreten, wenn beim Ausführen des Triggercodes für das entsprechende Benutzerkonto ein Fehler aufgetreten ist. Einige mögliche Szenarios:

- Der Trigger versucht, Daten in eine Tabelle einzufügen, die nicht vorhanden ist.
- Der Benutzer verfügt über keine Berechtigung für das Objekt, auf das der Anmeldetrigger verweist.

## <a name="user-action"></a>Benutzeraktion

Abhängig von dem Szenario, in dem Sie sich befinden, können Sie eine der folgenden Lösungen verwenden.

- **Szenario 1**: Sie haben derzeit über ein Administratorkonto Zugriff auf eine geöffnete [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sitzung.

  In diesem Fall können Sie die erforderlichen Korrekturmaßnahmen zum Korrigieren des Triggercodes ergreifen.

  - Beispiel 1: Wenn ein Objekt, auf das der Triggercode verweist, nicht vorhanden ist, erstellen Sie dieses Objekt, damit der Anmeldetrigger erfolgreich ausgeführt werden kann.

  - Beispiel 2: Wenn ein Objekt, auf das der Triggercode verweist, vorhanden ist, die Benutzer jedoch über keine entsprechenden Berechtigungen verfügen, geben Sie diesen die für den Zugriff auf das Objekt erforderlichen Berechtigungen.  
  
  Alternativ können Sie einfach den Anmeldetrigger verwerfen oder deaktivieren, sodass sich Benutzer weiterhin bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] anmelden können.  

- **Szenario 2**: Sie haben derzeit keine Sitzung mit Administratorrechten geöffnet, die dedizierte Administratorverbindung (DAC) ist auf der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz jedoch aktiviert.

    In diesem Fall können Sie die dedizierte Administratorverbindung verwenden, um die gleichen Schritte durchzuführen, die im Fall 1 erläutert werden, da sich Anmeldetrigger nicht auf dedizierte Administratorverbindungen auswirken. Weitere Informationen zur dedizierten Administratorverbindung finden Sie unter folgendem Link: [Diagnoseverbindung für Datenbankadministratoren](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)

    Sie können das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Fehlerprotokoll auf eine Meldung ähnlich der folgenden überprüfen, um herauszufinden, ob die DAC auf Ihrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz aktiviert ist:

    > 2020-02-09 16:17:44.150 Die Unterstützung für dedizierte Administratorverbindungen wurde zum lokalen Lauschen an Port 1434 eingerichtet.  

- **Szenario 3:** Sie haben weder die DAC auf Ihrem Server aktiviert noch verfügen Sie über eine bestehende [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Administratorsitzung.

    In diesem Szenario sind die folgenden Schritte die einzige Möglichkeit, das Problem zu beheben:
  
    1. Halten Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und zugehörige Dienste an.
    2. Starten Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] über die [Eingabeaufforderung](/previous-versions/sql/sql-server-2008-r2/ms180965(v=sql.105)), indem Sie die Startparameter `-c`, `-m` und `-f` verwenden. Dadurch wird der Anmeldetrigger deaktiviert, und Sie können dieselben Korrekturmaßnahmen ergreifen, die bereits unter **Fall 1** oben erläutert wurden.
  
        > [!NOTE]
        > Das oben beschriebene Verfahren erfordert eine *SA* oder ein entsprechendes Administratorkonto.
  
         Weitere Informationen zu diesen und anderen Startoptionen finden Sie unter dem folgenden Link: [Startoptionen für den Datenbank-Engine-Dienst](../../database-engine/configure-windows/database-engine-service-startup-options.md)

## <a name="more-information"></a>Weitere Informationen

Eine weitere Situation, in der bei Anmeldetriggern Fehler auftreten, ist bei Verwendung der `EVENTDATA`-Funktion. Diese Funktion gibt XML zurück, und die Groß-/Kleinschreibung wird berücksichtigt.  Wenn Sie also den folgenden Anmeldetrigger erstellen, durch den Sie den Zugriff basierend auf der IP-Adresse blockieren, kann folgendes Problem auftreten:

``` sql
 CREATE TRIGGER tr_logon_CheckIP  
 ON ALL SERVER  
 FOR LOGON  
 AS
 BEGIN
  IF IS_SRVROLEMEMBER ( 'sysadmin' ) = 1  
     BEGIN
         DECLARE @IP NVARCHAR ( 15 );  
         SET @IP = ( SELECT EVENTDATA ().value ( '(/EVENT_INSTANCE/ClientHost)[1]' , 'NVARCHAR(15)' ));  
         IF NOT EXISTS( SELECT IP FROM DBAWork.dbo.ValidIP WHERE IP = @IP )  
         ROLLBACK ;  
     END ;  
 END ;  
 GO
```

Der Benutzer hat beim Kopieren dieses Skripts aus dem Internet für diesen Teil des Triggers die Groß-/Kleinschreibung nicht berücksichtigt:

```sql
 SELECT EVENTDATA ().value ( '(/event_instance/clienthost)[1]' , 'NVARCHAR(15)' ));  
```

Folglich hat `EVENTDATA` immer **NULL** zurückgegeben, und allen SA-entsprechenden Benutzern wurde der Zugriff verweigert. In diesem Fall war die dedizierte Administratorverbindung nicht aktiviert. Es blieb daher keine andere Wahl als den Server mit den oben aufgeführten Startparametern neu zu starten, um den Trigger zu verwerfen.