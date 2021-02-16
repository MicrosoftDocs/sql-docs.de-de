---
description: Verbindungsserver (Datenbank-Engine)
title: Verbindungsserver (Datenbank-Engine) | Microsoft-Dokumentation
ms.date: 06/16/2020
ms.prod: sql
ms.technology: ''
ms.prod_service: database-engine
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB, linked servers
- OLE DB provider, linked servers
- server management [SQL Server], linked servers
- linked servers [SQL Server]
- distributed queries [SQL Server], linked servers
- servers [SQL Server], linked
- remote servers [SQL Server], linked servers
- linked servers [SQL Server], about linked servers
ms.assetid: 6ef578bf-8da7-46e0-88b5-e310fc908bb0
author: stevestein
ms.author: sstein
ms.custom: seo-dt-2019
ms.openlocfilehash: 09738de0caf9b7da5fcf3bec7c413d53d5d46981
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351188"
---
# <a name="linked-servers-database-engine"></a>Verbindungsserver (Datenbank-Engine)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Mithilfe von Verbindungsservern können [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] Daten aus Remotedatenquellen lesen und Befehle für Remotedatenbankserver (z. B. OLE DB-Datenquellen) außerhalb der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausführen. In der Regel werden Verbindungsserver so konfiguriert, um [!INCLUDE[ssDE](../../includes/ssde-md.md)] für die Ausführung einer [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung, die Tabellen in einer anderen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]enthält, oder ein anderes Datenbankprodukt z. B. Oracle zu aktivieren. Viele Typen von OLE DB-Datenquellen können als Verbindungsserver konfiguriert werden, einschließlich [!INCLUDE[msCoName](../../includes/msconame-md.md)] Access, Excel und Azure CosmosDB.

> [!NOTE]
> Verbindungsserver sind in [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] verfügbar. Sie sind in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] Singleton und in Pools für elastische Datenbanken nicht aktiviert. [Einschränkungen in einer verwalteten Instanz finden Sie hier](/azure/sql-database/sql-database-managed-instance-transact-sql-information#linked-servers). 

## <a name="when-to-use-linked-servers"></a>Wann sollten Verbindungsserver verwendet werden?

  Verbindungsserver ermöglichen das Implementieren verteilter Datenbanken, die Daten in anderen Datenbanken abrufen und aktualisieren können. Sie sind eine gute Lösung für Szenarien, in denen Sie Datenbanksharding implementieren müssen, ohne benutzerdefinierten Anwendungscode erstellen oder direktes Laden aus Remotedatenquellen ausführen zu müssen. Verbindungsserver bieten die folgenden Vorteile:  
  
-   Die Fähigkeit, auf Daten von außerhalb von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]zuzugreifen.  
  
-   Die Fähigkeit, verteilte Abfragen, Updates, Befehle und Transaktionen auf heterogenen Datenquellen im gesamten Unternehmen auszugeben.  
  
-   Die Möglichkeit, verschiedene Datenquellen ähnlich zu adressieren.  
  
Ein Verbindungsserver kann mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder mithilfe der Anweisung [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) konfiguriert werden. OLE DB-Anbieter variieren stark in Hinblick auf Typ und Anzahl der erforderlichen Parameter. Bei manchen Anbietern müssen Sie beispielsweise mit [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)zu aktivieren. Einige OLE DB-Anbieter ermöglichen es [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , Daten in der OLE DB-Quelle zu aktualisieren. Andere Anbieter stellen nur schreibgeschützten Datenzugriff bereit. Informationen zu den einzelnen OLE DB-Anbietern finden Sie in der jeweiligen Dokumentation des OLE DB-Anbieters.  
  
## <a name="linked-server-components"></a>Verbindungsserverkomponenten  
 Eine Verbindungsserverdefinition gibt die folgenden Objekte an:  
  
-   Einen OLE DB-Anbieter.  
  
-   Eine OLE DB-Datenquelle.  
  
Ein *OLE DB-Anbieter* ist eine DLL (Dynamic Link Library), die mit einer bestimmten Datenquelle interagiert und sie verwaltet. Eine *OLE DB-Datenquelle* identifiziert die spezielle Datenbank, auf die über OLE DB zugegriffen werden kann. Obwohl es sich bei Datenquellen, die über Verbindungsserverdefinitionen abgefragt werden, normalerweise um Datenbanken handelt, sind OLE DB-Anbieter für eine Vielzahl von Dateien und Dateiformaten verfügbar. Dazu gehören Textdateien, Kalkulationstabellendaten und die Ergebnisse aus Volltextsuchläufen.  
  
Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ist der [Microsoft OLE DB-Treiber für SQL Server (MSOLEDBSQL)](../../connect/oledb/oledb-driver-for-sql-server.md) (PROGID: MSOLEDBSQL) der OLE DB-Standardanbieter. In früheren Versionen war dies der [SQL Server Native Client OLE DB-Anbieter (SQLNCLI)](../../relational-databases/native-client/sql-server-native-client.md) (PROGID: SQLNCLI11).
  
> [!NOTE]  
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sind jedoch so entworfen, dass sie mit jedem OLE DB-Anbieter zusammenarbeiten, der die erforderlichen OLE DB-Schnittstellen implementiert. Getestet wurde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] jedoch für den OLE DB-Standardanbieter.  
  
## <a name="linked-server-details"></a>Einzelheiten zu Verbindungsservern  
 Die folgende Abbildung zeigt die Grundlagen einer Verbindungsserverkonfiguration.  
  
 ![Client-, Server- und Datenbankserverebene](../../relational-databases/linked-servers/media/lsvr.gif "Client-, Server- und Datenbankserverebene")  
  
Verbindungsserver werden in der Regel für die Verarbeitung verteilter Abfragen verwendet. Führt eine Clientanwendung eine verteilte Abfrage über einen Verbindungsserver aus, analysiert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] den Befehl und sendet Anforderungen an OLE DB. Für eine Rowsetanforderung kann eine Abfrage für den Anbieter ausgeführt oder eine Basistabelle vom Anbieter geöffnet werden.  

> [!NOTE]
> Damit eine Datenquelle Daten über einen Verbindungsserver zurückgibt, muss der OLE DB-Anbieter (DLL) für diese Datenquelle auf demselben Server wie die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]vorhanden sein.  
 
> [!IMPORTANT]
> Wenn ein OLE DB-Anbieter verwendet wird, müssen dem Konto, mit dem der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienst ausgeführt wird, Lese- und Ausführungsberechtigungen für das Verzeichnis (und alle Unterverzeichnisse) erteilt worden sein, in dem der Anbieter installiert ist. Dies schließt den Anbieter von Microsoft und alle Anbieter von Drittanbietern ein.

> [!NOTE]
> Verbindungsserver unterstützen bei der vollständigen Delegierung die Passthrough-Authentifizierung von Active Directory. Ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU17 wird auch die Passthrough-Authentifizierung mit eingeschränkter Delegierung unterstützt. Die [ressourcenbasierte eingeschränkte Delegierung](/windows-server/security/kerberos/kerberos-constrained-delegation-overview) wird jedoch nicht unterstützt.

## <a name="managing-providers"></a>Verwalten von Anbietern  
Eine Gruppe von Optionen steuert, wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] OLE DB-Anbieter lädt und verwendet, die in der Registrierung angegeben werden.  
  
## <a name="managing-linked-server-definitions"></a>Verwalten von Verbindungsserverdefinitionen  
Beim Einrichten eines Verbindungsservers sollten die Verbindungsinformationen und Datenquelleninformationen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]registriert werden. Nach der Registrierung kann über einen einzelnen logischen Namen auf diese Datenquelle verwiesen werden.  
  
Sie können gespeicherte Prozeduren und Katalogsichten zum Verwalten von Verbindungsserverdefinitionen verwenden:  
  
-   Erstellen Sie eine Verbindungsserverdefinition, indem Sie **sp_addlinkedserver** ausführen.  
  
-   Zeigen Sie Informationen zu den in einer bestimmten Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definierten Verbindungsservern an, indem Sie eine Abfrage der **sys.servers** -Systemkatalogsichten ausführen.  
  
-   Löschen Sie eine Verbindungsserverdefinition, indem Sie **sp_dropserver** ausführen. Sie können mit dieser gespeicherten Prozedur auch einen Remoteserver entfernen.  
  
Sie können Verbindungsserver auch mithilfe von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]definieren. Klicken Sie im Objekt-Explorer mit der rechten Maustaste auf **Serverobjekte**, klicken Sie auf **Neu**, und klicken Sie dann auf **Verbindungsserver**. Sie können eine Verbindungsserverdefinition löschen, indem Sie mit der rechten Maustaste auf den Namen des Verbindungsservers und dann auf **Löschen** klicken.  
  
 Wenn Sie eine verteilte Abfrage auf einem Verbindungsserver ausführen, sollten Sie einen vollqualifizierten vierteiligen Tabellennamen für jede Datenquelle einschließen, die abgefragt werden soll. Dieser vierteilige Name muss dieses Format haben: _linked\_server\_name.catalog_ **.** _schema_ **.** _object\_name_.  
  
> [!NOTE]  
> Verbindungsserver können so definiert werden, dass sie zurück auf den Server zeigen, auf dem sie definiert sind (zurücklaufen = loop back). Loopbackserver sind sehr nützlich, um eine Anwendung, von der verteilte Abfragen verwendet werden, in einem Netzwerk mit einem einzelnen Server zu testen. Loopbackverbindungsserver sind für Tests bestimmt und werden für viele Vorgänge, z. B. verteilte Transaktionen, nicht unterstützt.  
  
## <a name="related-tasks"></a>Related Tasks  
 [Erstellen von Verbindungsservern &#40;SQL Server-Datenbank-Engine&#41;](../../relational-databases/linked-servers/create-linked-servers-sql-server-database-engine.md)    
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)    
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)    
 [sp_dropserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropserver-transact-sql.md)    
  
## <a name="related-content"></a>Verwandte Inhalte  
 [sys.servers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-servers-transact-sql.md)    
 [sp_linkedservers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)