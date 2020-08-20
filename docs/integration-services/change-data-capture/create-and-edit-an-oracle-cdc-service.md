---
description: Erstellen und Bearbeiten eines Oracle CDC Service
title: Erstellen und Bearbeiten eines Oracle CDC Service | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- createSrv
ms.assetid: 10cd612e-d8f1-4af2-97d3-a0c22e1e2326
author: chugugrace
ms.author: chugu
ms.openlocfilehash: bcccb89d1af55f990388b389087c16c003d12c39
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88496258"
---
# <a name="create-and-edit-an-oracle-cdc-service"></a>Erstellen und Bearbeiten eines Oracle CDC Service

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Sie erstellen und bearbeiten einen neuen Oracle CDC-Windows-Dienst über die CDC Service Configuration Console.  
  
 Um einen neuen Oracle CDC-Windows-Dienst zu erstellen, wählen Sie im linken Bereich **Local CDC Services** aus und klicken dann im **Aktionsbereich** auf **Neuer Dienst** . Sie können auch mit der rechten Maustaste auf **Local CDC Services** (Lokale CDC-Dienste) klicken und **Neuer Dienst**auswählen. Das Dialogfeld New Oracle CDC Windows Service wird geöffnet.  
  
 **OR**  
  
 Um die Eigenschaften des CDC-Diensts zu bearbeiten, wählen Sie den Dienst aus, für den Sie die Eigenschaften bearbeiten möchten, und klicken im **Aktionsbereich** auf **Eigenschaften** . Sie können auch mit der rechten Maustaste auf den Dienst klicken, den Sie verwenden, und **Eigenschaften**wählen. Das Dialogfeld CDC Service Properties wird geöffnet.  
  
 Geben Sie im Dialogfeld New Oracle CDC Windows Service oder CDC Service Properties die folgenden Informationen ein.  
  
**Dienstname**  
 Geben Sie den Namen des neuen Oracle CDC-Windows-Diensts ein. Sie sollten nach Möglichkeit keine langen Namen verwenden. Die Zeichen "/" und "\" dürfen im Dienstnamen nicht verwendet werden.  
  
> [!NOTE]  
> Beim Bearbeiten des Diensts ist diese Option nicht verfügbar. Sie können den Namen eines Windows-Diensts, der bereits vorhanden ist, nicht ändern.  
  
 **Beschreibung**  
 Geben Sie eine Beschreibung des Diensts als Hilfe zur Identifizierung ein.  
  
 **Dienstkonto**  
 Wählen Sie eine der folgenden Optionen aus, um zu bestimmen, unter welchem Konto der Dienst ausgeführt werden soll:  
  
-   **Lokales Systemkonto**  
  
     Dies ist nicht zu empfehlen, da der Dienst dabei zu viele Berechtigungen erhält.  
  
-   **Dieses Konto**  
  
     Unter Windows Vista oder Windows Server 2008 ist das Standarddienstkonto das Konto NETZWERKDIENST.  
  
     Unter Windows 7 und Windows Server 2008 R2 und höher ist das Standarddienstkonto NT-Dienst\\<Dienstname>.  
  
     Bei Verwendung dieser Konten können Sie ohne Kennwörter arbeiten, weil für diese Konten kein Kennwort erforderlich ist. Außerdem stellen diese Konten jeweils nur die Berechtigungen bereit, die für die Ausführung des Oracle CDC Service erforderlich sind.  
  
     Sie können ein lokales Windows-Konto oder ein Windows-Domänenkonto für das Dienstkonto verwenden. In diesem Fall müssen Sie das **Kennwort** für dieses Konto eingeben. Dieses Konto kann für den lokalen Host gelten oder ein Domänenkonto sein. Beachten Sie, dass Sie das Kennwort, falls es sich ändert, in der Windows-Systemsteuerung unter Lokale Dienste aktualisieren müssen.  
  
 **Servername**: Wählen Sie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Zielinstanz aus, mit der eine Verbindung hergestellt werden soll, z.B. **\\\\<Computername\>\\<Instanzname\>** . Standardmäßig wird die Serverinstanz angezeigt, mit der zuletzt eine Verbindung bestanden hat.  
  
 **Authentifizierung**  
 Wählen Sie eines der folgenden Szenarien aus:  
  
-   **Windows-Authentifizierung**: Wenn Sie diese Option aktivieren, stellt der Oracle CDC Service mithilfe der Dienstkontoidentität eine Verbindung mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Zielinstanz her. Wenn die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz auf einem anderen Computer ausgeführt wird, muss die Windows-Authentifizierung mit Domänenkonten verwendet werden.  
  
-   **SQL Server-Authentifizierung**: Wenn Sie diese Option aktivieren, müssen Sie den **Benutzernamen** und das **Kennwort** für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldung eingeben, die Sie verwenden möchten. Der Oracle CDC Service verwendet diese Anmeldeinformationen beim Herstellen einer Verbindung mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Zielinstanz.  
  
 Die vom Oracle CDC Service verwendete [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldung muss lediglich Mitglied der festen Serverrolle public sein. Es sind keine anderen Berechtigungen erforderlich. Nachdem neue Oracle CDC-Instanzen hinzugefügt wurden, wird über diese Anmeldung der **db_owner** -Zugriff auf die zugeordneten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -CDC-Datenbanken gewährt.  
  
 Zum Erstellen der Definition des Oracle CDC-Windows-Diensts benötigt das Programm Aktualisierungszugriff auf die MSXDBCDC-Datenbank in der zugeordneten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz. Wenn Sie auf **OK**klicken, wird der Benutzer in einem Dialogfeld aufgefordert, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldeinformationen mit Aktualisierungszugriff auf die MSXDBCDC-Datenbank einzugeben.  
  
 Informationen zu den Daten, die Sie im Dialogfeld Verbindung mit SQL Server herstellen eingeben müssen, finden Sie unter [Connection to SQL Server](../../integration-services/change-data-capture/connection-to-sql-server.md).  
  
 **Optionen**  
 Klicken Sie auf den Pfeil, um die verfügbaren Optionen anzuzeigen, die konfiguriert werden sollen. Sie können für diese Optionen auch die Standardwerte unverändert lassen. Verfügbare Optionen:  
  
-   **Verbindungstimeout**: Geben Sie den Zeitraum (in Sekunden) ein, wie lange der CDC Service for Oracle auf eine Verbindung zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] warten soll, bevor ein Timeout eintritt. Der Standardwert lautet **15**.  
  
-   **Ausführungstimeout**: Geben Sie den Zeitraum (in Sekunden) ein, wie lange der Oracle CDC-Windows-Dienst auf die Ausführung eines Befehls wartet, bis ein Timeout eintritt. Der Standardwert ist **30**.  
  
-   **Verbindung verschlüsseln**: Wählen Sie **Verbindung verschlüsseln** für die Kommunikation zwischen dem Oracle CDC Service und der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Zielinstanz aus, bei der eine verschlüsselte Verbindung verwendet werden soll.  
  
-   **Erweitert**: Geben Sie zusätzliche Verbindungseigenschaften ein, falls erforderlich.  
  
 **Master Password**  
 Geben Sie ein Kennwort ein, das vom Oracle CDC-Windows-Dienst zum Schützen der Oracle-Log Mining-Anmeldeinformationen verwendet wird.  
  
 Das gleiche Masterkennwort muss auch verwendet werden, wenn andere Instanzen des gleichen Diensts auf anderen Knoten eines Clusters in der Konfiguration für hohe Verfügbarkeit eingerichtet werden. Wenn Sie das Masterkennwort vergessen oder ändern, müssen alle in Oracle CDC-Instanzdatenbanken gespeicherten Log Mining-Kennwörter über die CDC Designer Console neu eingegeben werden.  
  
## <a name="see-also"></a>Siehe auch  
 [Erstellen und Bearbeiten eines CDC Service](../../integration-services/change-data-capture/how-to-create-and-edit-a-cdc-service.md)  
  
  
