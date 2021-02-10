---
description: Erstellen der Serververbindungsdateien (OracleToSQL)
title: Erstellen der Server Verbindungs Dateien (oracleto SQL) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Server Connection File Creation
- Server Connection File, Server Connection File Validation
ms.assetid: 002f129e-0868-48ad-a4b4-c68b5007e12e
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 32d8bb51bd602a1da6fec11d3a101f984cab18a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058505"
---
# <a name="creating-the-server-connection-files-oracletosql"></a>Erstellen der Serververbindungsdateien (OracleToSQL)
Server Informationen können entweder im Abschnitt Server der Skriptdatei oder in einer separaten Server Verbindungs Datei angegeben werden. Der Befehlszeilenparameter für die Server Verbindungs Datei ist, `-c <serverconnectionfile>` . Wenn dieselbe Server-ID sowohl in der Skriptdatei als auch in der Server Verbindungs Datei vorhanden ist, wird die Server Definition in der Skriptdatei berücksichtigt.  
  
**Beispiel: 1**  
  
```  
<!--Sample of server connection file commands -->  
  
<oracle name="<source-server-unique-name>">  
  
  <tns-name-mode>  
  
    <connection-provider value="OracleClient"/>  
  
    <service-name value="(DESCRIPTION =(ADDRESS_LIST =(ADDRESS = (PROTOCOL = TCP)(HOST = <host-name>)(PORT = 1521)))(CONNECT_DATA =(SERVICE_NAME = <service-name>)))"/>  
  
    <user-id value="<user-name>"/>  
  
    <password value="<password>"/>  
  
  </tns-name-mode>  
  
</oracle>  
```  
  
```  
<sql-server name="<target-server-unique-name>">  
  
  <sql-server-authentication>  
  
    <server value="<server-name>"/>  
  
    <database value="<database-name>"/>  
  
    <user-id value="<user-name>"/>  
  
    <password value="<password>"/>  
  
    <encrypt value="<true/false>"/>  
  
    <trust-server-certificate value="<true/false>"/>  
  
  </sql-server-authentication>  
  
</sql-server>  
```  
**Beispiel: 2**  
  
```  
<!--Sample of server connection file commands -->  
  
<oracle name="<source-server-unique-name>">  
  
  <connection-string-mode>  
  
    <connection-provider value="OleDB Provider"/>  
  
    <custom-connection-string value="Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<host-name>)(PORT=1521))(CONNECT_DATA=(SID=<instance-name>)));User ID=<user-name>;Password=<password>"/>  
  
  </connection-string-mode>  
  
</oracle>  
```  
  
```  
<sql-server name="<target-server-unique-name>">  
  
  <sql-server-authentication>  
  
    <server value="<server-name>"/>  
  
    <database value="<database-name>"/>  
  
    <user-id value="<user-name>"/>  
  
    <password value="<password>"/>  
  
  </sql-server-authentication>  
  
</sql-server>  
```  
  
## <a name="next-step"></a>Nächster Schritt  
Der nächste Schritt beim Betrieb der Konsole ist [die Ausführung der SSMA-Konsole &#40;oracleto SQL&#41;](../../ssma/oracle/executing-the-ssma-console-oracletosql.md)  
  
## <a name="see-also"></a>Weitere Informationen  
[Ausführen der SSMA-Konsole](executing-the-ssma-console-oracletosql.md)  
  
