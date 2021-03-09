---
description: catalog.check_schema_version
title: catalog.check_schema_version | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: e0d5e9f5-59c6-4118-87b5-4aa5c37a7df6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aa47e5107551ec2d4ded0832ff52922ffb4bfd55
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247523"
---
# <a name="catalogcheck_schema_version"></a>catalog.check_schema_version 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Bestimmt, ob das SSISDB-Katalogschema und die [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Binärdateien (ISServerExec- und SQLCLR-Assembly) kompatibel sind.  
  
 ISServerExec.exc protokolliert eine Fehlermeldung, wenn das Schema und die Binärdateien nicht kompatibel sind.  
  
 Die SSISDB-Schemaversion wird inkrementiert, wenn sich das Schema während der Anwendung von Patches und während der Upgrades ändert. Es wird empfohlen, dass Sie diese gespeicherte Prozedur ausführen, nachdem eine SSISDB-Sicherung wiederhergestellt wurde, um sicherzustellen, dass das Schema und die Binärdateien kompatibel sind.  
  
## <a name="syntax"></a>Syntax  
  
```sql  
catalog.check_schema_version [ @use32bitruntime = ] use32bitruntime  
```  
  
## <a name="arguments"></a>Argumente  
 [ @use32bitruntime= ] *use32bitruntime*  
 Wenn der Parameter auf **1** festgelegt wird, wird die 32-Bit-Version von dtexec aufgerufen. *use32bitruntime* ist **int**.  
  
 
## <a name="return-code-value"></a>Rückgabecodewert 
Gibt 0 (null) für Erfolg zurück 

## <a name="result-set"></a>Resultset  

Gibt eine Tabelle zurück, die das folgende Format aufweist:

| Spaltenname | Datentyp | BESCHREIBUNG |
|---|---|---|
| SERVER_BUILD | **decimal** | Dies ist die SQL Server-Version. Ein Server, für den SQL Server 2014 ausgeführt wird, ist beispielsweise `14.0.3335.7`. |
| SCHEMA_VERSION | **tinyint** | Dies ist die SQL Server-Versionsnummer. Beispielsweise sind SQL Server 2017 uns 2019 jeweils `6` und `7`.|
| SCHEMA_BUILD | **string** | Schemabuild |
| ASSEMBLY_BUILD | **string** | Assemblybuild |
| SHARED_COMPONENT_VERSION | **string** | Freigegebene Komponentenversion | 

## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur erfordert die folgende Berechtigung:  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin** .  
  
  
