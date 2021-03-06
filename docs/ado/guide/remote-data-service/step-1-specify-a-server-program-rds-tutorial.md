---
description: 'Schritt 1: Serverprogramm angeben (RDS-Tutorial)'
title: 'Schritt 1: Angeben eines Server Programms (RDS-Tutorial) | Microsoft-Dokumentation'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], specifying server program
ms.assetid: d8bb35b1-c02a-4231-8d55-016e56e53b95
author: rothja
ms.author: jroth
ms.openlocfilehash: d0e59c52ee311c5172b3cdc15f58c5e9448f7110
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036399"
---
# <a name="step-1-specify-a-server-program-rds-tutorial"></a>Schritt 1: Serverprogramm angeben (RDS-Tutorial)
Im Allgemeinen sollten Sie [RDS verwenden. DataSpace](../../reference/rds-api/dataspace-object-rds.md) - [Objektmethode zum](../../reference/rds-api/createobject-method-rds.md) angeben des Standard Server Programms, [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md)oder Ihres eigenen benutzerdefinierten Server Programms (Geschäftsobjekt). Ein Serverprogramm wird auf dem Server instanziiert, und es wird ein Verweis auf das Serverprogramm oder den *Proxy* zurückgegeben.  
  
 In diesem Tutorial wird das standardmäßige Serverprogramm verwendet:  
  
```vb
Sub RDSTutorial1()  
   Dim DS as New RDS.DataSpace  
   Dim DF as Object  
   Set DF = DS.("RDSServer.DataFactory", "https://yourServer")  
...  
```  
  
> [!IMPORTANT]
>  Ab Windows 8 und Windows Server 2012 sind RDS-Server Komponenten nicht mehr im Windows-Betriebssystem enthalten (weitere Details finden Sie unter Windows 8 und [Windows Server 2012 Compatibility Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). RDS-Client Komponenten werden in einer zukünftigen Version von Windows entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Anwendungen, die RDS verwenden, sollten zu [WCF Data Service](/dotnet/framework/wcf/)migriert werden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Schritt 2: Aufrufen des Server Programms (RDS-Tutorial)](./step-2-invoke-the-server-program-rds-tutorial.md)   
 [RDS-Tutorial (VBScript)](./rds-tutorial-vbscript.md)