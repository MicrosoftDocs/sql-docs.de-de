---
title: Architektur der Client-und serverseitigen XML-Daten (SQLXML)
description: Erfahren Sie mehr über die Architektur der Client seitigen und serverseitigen XML-Formatierung in SQLXML 4,0.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- providers [SQLXML], XML formatting architecture
- SQLOLEDB provider
- client-side XML formatting
- data providers [SQLXML], XML formatting architecture
- SQLNCLI, XML
- server-side XML formatting
- SQL Server Native Client, XML
- SQLXMLOLEDB Provider, XML formatting architecture
ms.assetid: 52440d9e-89fd-4c15-a008-a1ea99f41387
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 599efd933243c2e5bac13f825ef8a8315c7481cf
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467071"
---
# <a name="architecture-of-client-side-and-server-side-xml-formatting-sqlxml-40"></a>Architektur clientseitiger und serverseitiger XML-Formatierung (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Die folgende Abbildung zeigt die Architektur der XML-Formatierung auf Serverseite:  
  
 ![Architektur serverseitiger XML-Formatierung](../../../relational-databases/sqlxml/formatting/media/serversidexml.gif "Architektur serverseitiger XML-Formatierung")  
  
 In diesem Beispiel wird der Befehl, der auf dem Client angegeben wird, an den Server gesendet. Der Server erstellt ein XML-Dokument und sendet es an den Client zurück. In diesem Fall verfügt der Server über eine Instanz von [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Bei der serverseitigen XML-Formatierung können Sie entweder den SQLXMLOLEDB-Anbieter oder den SQLOLEDB-Anbieter verwenden.  Der SQLXMLOLEDB-Anbieter verwendet die Datei Sqlxml4.dll, die in SQLXML 4.0 enthalten ist. Bei der Verwendung des SQLOLEDB-Anbieters erhalten Sie die von Sqlxmlx.dll bereitgestellte SQLXML-Funktionalität standardmäßig. Diese Funktionalität wird in [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows oder in Microsoft Data Access Components (MDAC) 2.6 oder höher angeboten. Wenn Sie Sqlxml4.dll mit SQLOLEDB verwenden möchten, müssen Sie die SQLXML-Versions Eigenschaft für das SQLOLEDB-Verbindungs Objekt auf "SQLXML. 4.0" festlegen. In beiden Fällen erzeugt der Server das XML-Dokument und sendet es an den Client.  
  
> [!NOTE]  
>  XPath-Abfragen und Updategrams werden auf dem Client analysiert. Um die XPath-Vorlage oder Updategramfunktionalität in SQLXML 4.0 zu erhalten, verwenden Sie Sqlxml4.dll.  
  
 Die folgende Abbildung zeigt die Architektur der XML-Formatierung auf Clientseite.  
  
 ![Architektur der XML-Formatierung auf Clientseite](../../../relational-databases/sqlxml/formatting/media/clientsidexml.gif "Architektur der XML-Formatierung auf Clientseite")  
  
 In diesem Beispiel verwendet der Client den SQLXMLOLEDB-Anbieter. In der Verbindungs Zeichenfolge muss die Datenanbieter-Eigenschaft auf SQLOLEDB festgelegt werden. (Dies ist der einzige in SQLXML 4,0 akzeptierte Wert.) Der Befehl, der auf dem Client ausgeführt wird, wird an den Server gesendet. Das auf dem Server generierte Rowset wird an den Client gesendet. Die Formatierung des XML-Dokuments aus dem Rowset wird auf dem Client ausgeführt.  
  
 In SQLXML 4.0 kann entweder der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client (SQLNCLI11) oder der SQLOLEDB-Anbieter als Datenanbieter verwendet werden. Sie können potenziell auf jede Datenquelle zugreifen. Vorausgesetzt, die Abfrage gibt ein einzelnes Rowset zurück, kann die XML-Transformation auf dem Client angewendet werden.  
  
  
