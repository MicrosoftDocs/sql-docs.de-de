---
description: MSSQLSERVER_18483
title: MSSQLSERVER_18483
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 18483 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 82b12abb1121ed8f9523a267b5dbe98464c736f5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345544"
---
# <a name="mssqlserver_18483"></a>MSSQLSERVER_18483
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|18483|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|REMLOGIN_INVALID_USER|
|Meldungstext|Zum Server "%.ls" konnte keine Verbindung hergestellt werden, weil "%. ls" nicht für die Remoteanmeldung auf dem Server definiert ist. Überprüfen Sie, ob Sie den richtigen Anmeldenamen angegeben haben. %.*ls.|
||

## <a name="explanation"></a>Erklärung

Dieser Fehler tritt auf, wenn Sie versuchen, einen Replikationsverteiler auf einem System zu konfigurieren, das mit dem Festplattenimage eines anderen Computers wiederhergestellt wurde, auf dem die SQL-Instanz ursprünglich installiert war. Dem Benutzer wird eine Fehlermeldung angezeigt, die der folgenden ähnelt:

> SQL Server Management Studio konnte „\<Server>\<Instance>“ nicht als Verteiler für „\<Server>\<Instance>“ konfigurieren. Fehler 18483: Verbindung zum Server „\<Server>\<Instance>“ konnte nicht hergestellt werden, weil „distributor_admin“ beim Server nicht als Remoteanmeldung definiert ist. Überprüfen Sie, ob Sie den richtigen Anmeldenamen angegeben haben. %.*ls.

## <a name="cause"></a>Ursache

Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] von einem Festplattenimage eines anderen Computers bereitstellen, auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installiert ist, wird der Netzwerkname des Computers, von dem das Image stammt, in der neuen Installation beibehalten. Der falsche Netzwerkname führt dazu, dass die Konfiguration des Replikationsverteilers fehlschlägt. Das gleiche Problem tritt auf, wenn Sie den Computer nach der Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] umbenennen.

## <a name="user-action"></a>Benutzeraktion

Um dieses Problem zu umgehen, ersetzen Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Servernamen durch den korrekten Netzwerknamen des Computers. Gehen Sie dazu folgendermaßen vor:

1. Melden Sie sich bei dem Computer an, auf dem Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus dem Datenträgerimage bereitgestellt haben, und führen Sie dann die folgende Transact-SQL-Anweisung in SSMS aus:

    ```sql
    -- Use the Master database
    USE master
    GO

    -- Declare local variables
    DECLARE @serverproperty_servername varchar(100),
    @servername varchar(100);

    -- Get the value returned by the SERVERPROPERTY system function
    SELECT @serverproperty_servername = CONVERT(varchar(100), SERVERPROPERTY('ServerName'));

    -- Get the value returned by @@SERVERNAME global variable
    SELECT @servername = CONVERT(varchar(100), @@SERVERNAME);

    -- Drop the server with incorrect name
    EXEC sp_dropserver @server=@servername;

    -- Add the correct server as a local server
    EXEC sp_addserver @server=@serverproperty_servername, @local='local';
    ```

2. Starten Sie den Computer, auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird, neu.
3. Um zu überprüfen, ob der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Name und der Netzwerkname des Computers übereinstimmen, führen Sie die folgende Transact-SQL-Anweisung aus:

    ```sql
    SELECT @@SERVERNAME, SERVERPROPERTY('ServerName');
    ```

## <a name="more-information"></a>Weitere Informationen

Sie können die globale `@@SERVERNAME`-Variable oder die Funktion `SERVERPROPERTY`('ServerName') in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwenden, um den Netzwerknamen des Computers zu ermitteln, auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird. Die ServerName-Eigenschaft der `SERVERPROPERTY`-Funktion meldet die Änderung des Netzwerknamens des Computers automatisch, wenn Sie den Computer und den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienst neu starten. Die globale `@@SERVERNAME`-Variable behält den ursprünglichen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Computernamen bei, bis der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Name manuell zurückgesetzt wird.

### <a name="steps-to-reproduce-the-problem"></a>Schritte zum Reproduzieren des Problems

Führen Sie auf dem Computer, auf dem Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aus einem Datenträgerimage bereitgestellt haben, die folgenden Schritte aus:

1. Starten Sie [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)].
2. Erweitern Sie im **Objekt-Explorer** den Namen Ihrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz.
3. Klicken Sie mit der rechten Maustaste auf den Ordner **Replikation**, und klicken Sie auf **Replikation „Verteilung konfigurieren“** und dann auf **Veröffentlichung, Abonnenten und Verteilung konfigurieren**.
4. Klicken Sie im Dialogfeld des Assistenten **Verteilung konfigurieren** auf **Weiter**.
5. Wählen Sie im Dialogfeld **Verteiler** auf das \<**Server**>\<**Instance**>-Element, das als sein eigener Verteiler fungieren soll. SQL Server erstellt eine Verteilungsdatenbank und ein Protokolloptionsfeld. Klicken Sie anschließend auf **Weiter**.
6. Klicken Sie im Dialogfeld **SQL Server-Agent, Start** auf **Weiter**.
7. Klicken Sie im Dialogfeld **Momentaufnahmeordner** auf **Weiter**.

    > [!NOTE]
    > Wenn Sie eine Meldung zur Bestätigung des Pfads des Momentaufnahmeordners erhalten, klicken Sie auf **Ja**.
8. Klicken Sie im Dialogfeld **Verteilungsdatenbank** auf **Weiter**.
9. Klicken Sie im Dialogfeld **Verleger** auf **Weiter**.
10. Klicken Sie im Dialogfeld **Aktionen des Assistenten** auf **Weiter**.
11. Klicken Sie im Dialogfeld **Assistenten abschließen** auf **Fertig stellen**.

## <a name="see-also"></a>Weitere Informationen

- [Umbenennen eines Computers, der eine eigenständige Instanz von SQL Server hostet](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)
- [@@SERVERNAME (Transact-SQL)](../../t-sql/functions/servername-transact-sql.md)
- [SERVERPROPERTY (Transact-SQL)](../../t-sql/functions/serverproperty-transact-sql.md)
- [sp_addserver (Transact-SQL)](../system-stored-procedures/sp-addserver-transact-sql.md)