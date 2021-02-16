---
title: Angeben einer Servernetzwerkadresse (Datenbankspiegelung)
description: Hier erfahren Sie, wie Sie eine Servernetzwerkadresse für einen Datenbankspiegelungs-Endpunkt angeben. Für eine Datenbankspiegelungs-Sitzung benötigt jede Serverinstanz eine Adresse.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- database mirroring [SQL Server], endpoint
- endpoints [SQL Server], database mirroring
- server network addresses [SQL Server]
ms.assetid: a64d4b6b-9016-4f1e-a310-b1df181dd0c6
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: f8e778699add5318ca7c42f36ae9cb2c5243d2cf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352234"
---
# <a name="specify-a-server-network-address-database-mirroring"></a>Angeben einer Servernetzwerkadresse (Datenbankspiegelung)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Beim Einrichten einer Datenbank-Spiegelungssitzung ist für jede Serverinstanz eine Server-Netzwerkadresse erforderlich. Die Server-Netzwerkadresse der Serverinstanz muss die Instanz eindeutig identifizieren, indem sie eine Systemadresse und die Nummer des Ports angibt, den die Instanz überwacht.  
  
 Bevor Sie einen Port in einer Server-Netzwerkadresse angeben können, muss der Endpunkt der Datenbankspiegelung auf der Serverinstanz vorhanden sein. Weitere Informationen finden Sie unter [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md).  
  
  
##  <a name="syntax-for-a-server-network-address"></a><a name="Syntax"></a> Syntax für eine Server-Netzwerkadresse  
 Die Syntax für eine Server-Netzwerkadresse lautet:  
  
 TCP <strong>://</strong> *\<system-address>* <strong>:</strong> *\<port>*  
  
 Hierbei gilt:  
  
-   *\<system-address>* ist eine Zeichenfolge, die das Zielcomputersystem eindeutig identifiziert. In der Regel handelt es sich bei der Serveradresse um einen Systemnamen (wenn sich die Systeme in derselben Domäne befinden), einen vollqualifizierten Domänennamen oder eine IP-Adresse:  
  
    -   Befinden sich die Systeme in derselben Domäne, können Sie den Namen des Computersystems verwenden, z. B. `SYSTEM46`.  
  
    -   Wenn Sie eine IP-Adresse verwenden möchten, muss diese in Ihrer Umgebung eindeutig sein. Wir empfehlen die Verwendung einer IP-Adresse nur, wenn diese statisch ist. Die IP-Adresse kann im IPv4-Format (IP Version 4) oder im IPv6-Format (IP Version) vorliegen. Eine IPv6-Adresse muss in eckige Klammern gesetzt werden, z.B. **[** _<IPv6-Adresse>_ **]** .  
  
         Um die IP-Adresse eines Systems zu ermitteln, geben Sie an der Windows-Eingabeaufforderung den Befehl **ipconfig** ein.  
  
    -   Der vollqualifizierte Domänenname funktioniert auf alle Fälle. Hierbei handelt es sich um eine lokal definierte Adresszeichenfolge, die an unterschiedlichen Stellen unterschiedliche Formen annimmt. Häufig, jedoch nicht immer, ist ein vollqualifizierter Domänenname ein zusammengesetzter Name, der den Computernamen und eine Reihe von Domänensegmenten enthält, die durch Punkte voneinander getrennt sind, z. B.:  
  
         _Computername_ **.** _Domänensegment_[... **.** _Domänensegment_]  
  
         Dabei steht *Computername* für den Netzwerknamen des Computers, auf dem die Serverinstanz ausgeführt wird, und *Domänensegment*[... **.** _Domänensegment_] für die übrigen Domäneninformationen des Servers. Beispiel: `localinfo.corp.Adventure-Works.com`.  
  
         Inhalt und Anzahl von Domänenelementen werden innerhalb des Unternehmens oder der Organisation bestimmt. Wenn Sie den vollqualifizierten Domänennamen des Servers nicht kennen, wenden Sie sich an den Systemadministrator.  
  
        > [!NOTE]  
        >  Informationen zum Ermitteln eines vollqualifizierten Domänennamens finden Sie nachfolgend in diesem Thema unter "Ermitteln des vollqualifizierten Domänennamens".  
  
-   *\<port>* ist die Portnummer, die vom Spiegelungsendpunkt der Partnerserverinstanz verwendet wird. Weitere Informationen zum Angeben eines Endpunkts finden Sie unter [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md).  
  
     Ein Datenbankspiegelungs-Endpunkt kann jeden verfügbaren Port im Computersystem verwenden. Jede Portnummer in einem Computersystem darf nur einem Endpunkt zugeordnet werden, und jeder Endpunkt ist einer einzelnen Serverinstanz zugeordnet. Daher lauschen unterschiedliche Serverinstanzen auf dem gleichen Server an unterschiedliche Endpunkten mit unterschiedlichen Ports. Aus diesem Grund leitet der Port, den Sie beim Einrichten einer Datenbank-Spiegelungssitzung in der Server-Netzwerkadresse angeben, die Sitzung immer an die Serverinstanz weiter, deren Endpunkt dem jeweiligen Port zugeordnet ist.  
  
     In der Server-Netzwerkadresse einer Serverinstanz unterscheidet nur die Nummer des Ports, die ihrem Spiegelungsendpunkt zugeordnet ist, diese Instanz von anderen Instanzen auf dem Computer. Die folgende Abbildung veranschaulicht die Server-Netzwerkadresse von zwei Serverinstanzen auf einem Computer. In der Standardinstanz wird Port `7022` verwendet, und in der benannten Instanz wird Port `7033`verwendet. Die Server-Netzwerkadressen für diese beiden Serverinstanzen lauten `TCP://MYSYSTEM.Adventure-works.MyDomain.com:7022` bzw. `TCP://MYSYSTEM.Adventure-works.MyDomain.com:7033`. Beachten Sie, dass die Adresse nicht den Namen der Serverinstanz enthält.  
  
     ![Servernetzwerkadressen einer Standardinstanz](../../database-engine/availability-groups/windows/media/dbm-2-instances-ports-1-system.gif "Servernetzwerkadressen einer Standardinstanz")  
  
     Verwenden Sie die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisung, um den Port zu identifizieren, der derzeit dem Endpunkt der Datenbankspiegelung einer Serverinstanz zugeordnet ist:  
  
    ```  
    SELECT type_desc, port FROM sys.tcp_endpoints  
    ```  
  
     Suchen Sie die Zeile, deren **type_desc** -Wert „DATABASE_MIRRORING“ lautet, und verwenden Sie die entsprechende Portnummer.  
  
### <a name="examples"></a>Beispiele  
  
#### <a name="a-using-a-system-name"></a>A. Verwenden eines Systemnamens  
 Die folgende Server-Netzwerkadresse gibt den Systemnamen `SYSTEM46`und Port `7022`an.  
  
```  
ALTER DATABASE AdventureWorks SET PARTNER ='tcp://SYSTEM46:7022';  
```  
  
#### <a name="b-using-a-fully-qualified-domain-name"></a>B. Verwenden eines vollqualifizierten Domänennamens  
 Die folgende Server-Netzwerkadresse gibt den vollqualifizierten Domänennamen `DBSERVER8.manufacturing.Adventure-Works.com`und Port `7024`an.  
  
```  
ALTER DATABASE AdventureWorks SET PARTNER ='tcp://DBSERVER8.manufacturing.Adventure-Works.com:7024';  
```  
  
#### <a name="c-using-ipv4"></a>C. Verwenden von IPv4  
 Die folgende Server-Netzwerkadresse gibt die IPv4-Adresse `10.193.9.134`und Port `7023`an.  
  
```  
ALTER DATABASE AdventureWorks SET PARTNER ='tcp://10.193.9.134:7023';  
```  
  
#### <a name="d-using-ipv6"></a>D: Verwenden von IPv6  
 Die folgende Server-Netzwerkadresse gibt die IPv6-Adresse `2001:4898:23:1002:20f:1fff:feff:b3a3`und Port `7022`an.  
  
```  
ALTER DATABASE AdventureWorks SET PARTNER ='tcp://[2001:4898:23:1002:20f:1fff:feff:b3a3]:7022';  
```  
  
## <a name="finding-the-fully-qualified-domain-name"></a>Ermitteln des vollqualifizierten Domänennamens  
 Um den vollqualifizierten Domänennamen eines Systems zu ermitteln, geben Sie an der Windows-Eingabeaufforderung des Systems den folgenden Befehl ein:  
  
 **IPCONFIG /ALL**  
  
 Wenn Sie den vollqualifizierten Domänennamen bilden möchten, verketten Sie die Werte von *<host_name>* bzw. *<Primary_Dns_Suffix>* wie folgt:  
  
 _<host_name>_ **.** _<Primary_Dns_Suffix>_  
  
 Beispielsweise entspricht die IP-Konfiguration  
  
 `Host Name  .  .  .  .  .  .  : MYSERVER`  
  
 `Primary Dns Suffix  .  .  .  : mydomain.Adventure-Works.com`  
  
 dem folgenden vollqualifizierten Domänennamen:  
  
 `MYSERVER.mydomain.Adventure-Works.com`  
  
##  <a name="examples"></a><a name="Examples"></a> Beispiele  
 Im folgenden Beispiel wird die Server-Netzwerkadresse für eine Serverinstanz auf einem Computersystem namens `REMOTESYSTEM3` in einer anderen Domäne dargestellt. Die Domäneninformationen lauten `NORTHWEST.ADVENTURE-WORKS.COM`, und der Port des Datenbank-Spiegelungsendpunkts ist `7025`. Auf der Grundlage dieser Beispielkomponenten lautet die Server-Netzwerkadresse:  
  
 `TCP://REMOTESYSTEM3.NORTHWEST.ADVENTURE-WORKS.COM:7025`  
  
 Im folgenden Beispiel wird die Server-Netzwerkadresse für eine Serverinstanz auf einem Computersystem namens `DBSERVER1`dargestellt. Dieses System befindet sich in der lokalen Domäne und wird durch seinen Systemnamen eindeutig identifiziert. Der Port des Datenbank-Spiegelungsendpunkts ist `7022`.  
  
 `TCP://DBSERVER1:7022`  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Verwandte Aufgaben  
  
-   [Erstellen eines Endpunkts der Datenbankspiegelung für Windows-Authentifizierung &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Der Datenbankspiegelungs-Endpunkt &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
  
  
