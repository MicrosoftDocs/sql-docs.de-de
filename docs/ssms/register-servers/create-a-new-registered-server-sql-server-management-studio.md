---
description: Erstellen eines neu registrierten Servers (SQL Server Management Studio)
title: Erstellen von neuen registrierten Servern
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.registerserver.general.sqlce.f1
- sql13.swb.registerserver.general.sqlserver.f1
helpviewer_keywords:
- Registered Servers [SQL Server], creating new registered servers
ms.assetid: 716ea070-a3b5-4514-9de2-82ce8a96514b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 4c158ff93e5f5e62f8cb3d3d68333c5f48fd6338
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350538"
---
# <a name="create-a-new-registered-server-sql-server-management-studio"></a>Erstellen eines neu registrierten Servers (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

In diesem Thema wird beschrieben, wie Sie die Verbindungsinformationen für Server speichern, auf die Sie häufig zugreifen, indem Sie den Server in der Komponente Registrierte Server von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]registrieren. Ein Server kann vor dem Herstellen einer Verbindung oder bei der Verbindungsherstellung über den Objekt-Explorer registriert werden. Es gibt eine spezielle Menüoption zum Registrieren der Serverinstanzen auf dem lokalen Computer.  
  
 Es gibt zwei Arten von registrierten Servern:  
  
-   Lokale Servergruppen  
  
     Verwenden Sie lokale Servergruppen, um mühelos eine Verbindung zu Servern herzustellen, die Sie häufig verwalten. Sowohl lokale als auch nicht lokale Server sind in lokalen Servergruppen registriert. Lokale Servergruppen sind für jeden Benutzer eindeutig. Informationen zum Freigeben registrierter Serverinformationen finden Sie unter [Exportieren von Informationen zum registrierten Server &#40;SQL Server Management Studio&#41;](./export-registered-server-information-sql-server-management-studio.md) und [Importieren von Informationen zum registrierten Server &#40;SQL Server Management Studio&#41;](./import-registered-server-information-sql-server-management-studio.md)registrieren.  
  
    > [!NOTE]  
    >  Sie sollten nach Möglichkeit immer die Windows-Authentifizierung verwenden.  
  
-   Zentrale Verwaltungsserver  
  
     Zentrale Verwaltungsserver speichern Serverregistrierungen im zentralen Verwaltungsserver anstatt im Dateisystem. Zentrale Verwaltungsserver und untergeordnete registrierte Server können nur mithilfe der Windows-Authentifizierung registriert werden. Wenn ein zentraler Verwaltungsserver registriert wurde, werden seine zugeordneten registrierten Server automatisch angezeigt. Weitere Informationen zu zentralen Verwaltungsservern finden Sie unter [Verwalten mehrerer Server mithilfe von zentralen Verwaltungsservern](../../relational-databases/administer-multiple-servers-using-central-management-servers.md). Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , die älter sind als [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] , können nicht als zentraler Verwaltungsserver festgelegt werden.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-automatically-register-the-local-server-instances"></a>So registrieren Sie die lokalen Serverinstanzen  
  
-   Klicken Sie unter Registrierte Server mit der rechten Maustaste auf einen der Knoten in der Struktur Registrierte Server. Klicken Sie dann auf **Lokale Serverregistrierung aktualisieren**.  
  
#### <a name="to-create-a-new-registered-server"></a>So erstellen Sie einen neu registrierten Server  
  
1.  Wenn die Option Registrierte Server in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]nicht angezeigt wird, klicken Sie im Menü **Ansicht** auf **Registrierte Server**.  
  
     **Servertyp**  
     Wenn Sie einen Server über Registrierte Server registrieren, ist das Feld **Servertyp** schreibgeschützt, wobei der Feldeintrag mit dem im Bereich Registrierte Server angezeigten Servertyp übereinstimmt. Wenn Sie einen anderen Servertyp registrieren möchten, klicken Sie, bevor Sie mit dem Registrieren eines neuen Servers beginnen, auf der Symbolleiste **Registrierte Server** auf **Datenbank-Engine**, **Analysis-Server**, **Reporting Services** oder **Integration Services** .  
  
     **Servername**  
     Wählen Sie die zu registrierende Serverinstanz in folgendem Format aus: *\<servername>* [\\ *\<instancename>* ].  
  
     **Authentifizierung**  
     Beim Herstellen einer Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]sind zwei Authentifizierungsmodi verfügbar:  
  
     **Windows-Authentifizierung**  
     Mit dem Windows-Authentifizierungsmodus können Benutzer die Verbindung über ein [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Benutzerkonto herstellen.  
  
     **SQL Server-Authentifizierung**  
     Wenn ein Benutzer eine Verbindung mit einem angegebenen Benutzernamen und einem Kennwort von einer nicht vertrauenswürdigen Verbindung herstellt, führt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Authentifizierung durch, indem überprüft wird, ob ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldekonto eingerichtet wurde und ob das angegebene Kennwort mit dem zuvor aufgezeichneten übereinstimmt. Wenn kein Anmeldekonto in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eingerichtet wurde, schlägt die Authentifizierung fehl, und der Benutzer erhält eine Fehlermeldung.  
  
    > [!IMPORTANT]  
    >  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)] Weitere Informationen finden Sie unter [Auswählen eines Authentifizierungsmodus](../../relational-databases/security/choose-an-authentication-mode.md).  
  
     **Benutzername**  
     Zeigt den aktuellen Benutzernamen an, zu dem die Verbindung hergestellt wird. Diese schreibgeschützte Option ist nur verfügbar, wenn Sie zum Herstellen der Verbindung die Windows-Authentifizierung ausgewählt haben. Um **Benutzername** zu ändern, melden Sie sich bei dem Computer als ein anderer Benutzer an.  
  
     **Anmeldung**  
     Geben Sie den Anmeldenamen für die Verbindung ein. Diese Option ist nur verfügbar, wenn Sie zum Verbinden die Authentifizierung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgewählt haben.  
  
     **Kennwort**  
     Geben Sie das Kennwort für die Anmeldung ein. Diese Option kann nur bearbeitet werden, wenn Sie zum Verbinden die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung ausgewählt haben.  
  
     **Kennwort speichern**  
     Aktivieren Sie dieses Kontrollkästchen, damit das eingegebene Kennwort von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verschlüsselt und gespeichert wird. Diese Option wird nur angezeigt, wenn Sie zum Verbinden die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung ausgewählt haben.  
  
    > [!NOTE]  
    >  Wenn Sie das Kennwort gespeichert haben und für die Zukunft nicht mehr speichern möchten, deaktivieren Sie das Kontrollkästchen, und klicken Sie auf **Speichern**.  
  
     **Name des registrierten Servers**  
     Der Name, der unter Registrierte Server angezeigt werden soll. Dieser Name muss mit dem Eintrag im Feld **Servername** nicht übereinstimmen.  
  
     **Beschreibung des registrierten Servers**  
     Geben Sie eine optionale Beschreibung des Servers ein.  
  
     **Test**  
     Klicken Sie hier, um die Verbindung mit dem unter **Servername** ausgewählten Server zu testen.  
  
     **Speichern**  
     Klicken Sie hier, um die Einstellungen des registrierten Servers zu speichern.  
  
## <a name="multiserver-queries"></a>Multiserverabfragen  
 Das Abfrage-Editorfenster in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] kann gleichzeitig eine Verbindung zu mehreren Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] herstellen und diese abfragen. Die von der Abfrage zurückgegebenen Ergebnisse können in einem einzigen Ergebnisbereich zusammengefasst oder in gesonderten Ergebnisbereichen ausgegeben werden. Als Option kann der Abfrage-Editor Spalten umfassen, die den Namen des jede Zeile generierenden Servers und den Anmeldenamen liefern, der zum Herstellen der Verbindung mit dem diese Zeile generierenden Server verwendet wurde. Weitere Informationen zum Ausführen von Multiserverabfragen finden Sie unter [Gleichzeitiges Ausführen von Anweisungen für mehrere Server &#40;SQL Server Management Studio&#41;](./execute-statements-against-multiple-servers-simultaneously.md).  
  
 Um Abfragen für alle Server in einer lokalen Servergruppe auszuführen, klicken Sie mit der rechten Maustaste auf die Servergruppe, zeigen Sie auf **Verbinden**, und klicken Sie anschließend auf **Neue Abfrage**. Wenn Abfragen im neuen Abfrage-Editor-Fenster ausgeführt werden, werden sie unter Verwendung der gespeicherten Verbindungsinformationen, einschließlich des Kontexts für die Benutzerauthentifizierung, für alle Server in der Gruppe ausgeführt. Server, die mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Authentifizierung registriert wurden, das Kennwort jedoch nicht speichern, können keine Verbindung herstellen.  
  
 Um Abfragen für alle Server auszuführen, die bei einem zentralen Verwaltungsserver registriert sind, erweitern Sie den zentralen Verwaltungsserver, klicken Sie mit der rechten Maustaste auf die Servergruppe, zeigen Sie auf **Verbinden**, und klicken Sie dann auf **Neue Abfrage**. Wenn Abfragen im neuen Abfrage-Editorfenster ausgeführt werden, werden sie anhand der gespeicherten Verbindungsinformationen und des Kontexts für die Windows-Authentifizierung, für alle Server in der Servergruppe ausgeführt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ausblenden von Systemobjekten im Objekt-Explorer](../object/hide-system-objects-in-object-explorer.md)   
 [Exportieren von Informationen zum registrierten Server &#40;SQL Server Management Studio&#41;](./export-registered-server-information-sql-server-management-studio.md)   
 [Importieren von Informationen zum registrierten Server &#40;SQL Server Management Studio&#41;](./import-registered-server-information-sql-server-management-studio.md)  
  
