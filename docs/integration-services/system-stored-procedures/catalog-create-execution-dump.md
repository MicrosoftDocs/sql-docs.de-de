---
description: catalog.create_execution_dump
title: catalog.create_execution_dump | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 91319b0b-5536-4ab4-a403-9559ed9dd177
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e1364857eb48fd5b31e6808eddaae481d3e441d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346798"
---
# <a name="catalogcreate_execution_dump"></a>catalog.create_execution_dump 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Hält ein aktuell ausgeführtes Paket an und erzeugt eine Dumpdatei. Die Datei wird im Ordner *\<drive>* :\Programme\Microsoft SQL Server\130\Shared\ErrorDumps gespeichert.  
  
## <a name="syntax"></a>Syntax  
  
```sql  
catalog.create_execution_dump [ @execution_id = ] execution_id  
  
```  
  
## <a name="arguments"></a>Argumente  
 [ @execution_id = ] *execution_id*  
 Die Ausführungs-ID für das ausgeführte Paket. Der *execution_id* ist **bigint**.  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird das ausgeführte Paket mit der Ausführungs-ID 88 aufgefordert, eine Dumpdatei zu erstellen.  
  
```sql
EXEC create_execution_dump @execution_id = 88  
```  
  
## <a name="return-codes"></a>Rückgabecodes  
 0 (Erfolg)  
  
 Wenn die gespeicherte Prozedur fehlschlägt, wird ein Fehler ausgelöst.  
  
## <a name="result-set"></a>Resultset  
 Keine  
  
## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur erfordert, dass der Benutzer Mitglied der Datenbankrolle **ssis_admin** ist.  
  
## <a name="errors-and-warnings"></a>Fehler und Warnungen  
 Die folgende Liste beschreibt Bedingungen, unter denen die gespeicherte Prozedur fehlschlägt.  
  
-   Eine ungültige Ausführungs-ID wurde angegeben.  
  
-   Das Paket ist bereits abgeschlossen.  
  
-   Das Paket erstellt gerade eine Dumpdatei.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Generieren von Dumpdateien für die Paketausführung](../../integration-services/troubleshooting/generating-dump-files-for-package-execution.md)  
  
  
