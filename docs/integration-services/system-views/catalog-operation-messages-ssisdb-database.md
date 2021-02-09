---
description: catalog.operation_messages (SSISDB-Datenbank)
title: catalog.operation_messages (SSISDB-Datenbank) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- catalog.operation_messages view [Integration Services]
- operation_messages view [Integration Services]
ms.assetid: 0b3cbe38-ce24-47ca-83ef-6538a5299d1a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9bc7373e302effa18247882c48f7db922f74dae9
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835213"
---
# <a name="catalogoperation_messages-ssisdb-database"></a>catalog.operation_messages (SSISDB-Datenbank)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Zeigt Meldungen an, die während der Vorgänge im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Katalog protokolliert werden.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|operation_message_id|**bigint**|Der eindeutige Bezeichner (ID) der Meldung.|  
|operation_id|**bigint**|Die eindeutige ID des Vorgangs.|  
|message_time|**datetimeoffset(7)**|Zeitpunkt, zu dem die Meldung erstellt wurde.|  
|message_type|**smallint**|Typ der angezeigten Meldung.|  
|message_source_type|**smallint**|Die ID des Typs der Nachrichtenquelle.|  
|message|**nvarchar(max)**|Der Text der Meldung.|  
|extended_info_id|**bigint**|Die ID weiterer Informationen, die sich auf die Vorgangsmeldung beziehen. Diese ist in der Sicht [extended_operation_info](../../integration-services/system-views/catalog-extended-operation-info-ssisdb-database.md) enthalten.|  
  
## <a name="remarks"></a>Hinweise  
 In dieser Sicht wird eine Zeile für jede Meldung angezeigt, die während eines Vorgangs im Katalog protokolliert wird. Die Meldung kann durch den Server, den Paketausführungsprozess oder die Ausführungs-Engine generiert werden.  
  
 In dieser Sicht werden die folgenden Meldungstypen angezeigt:  
  
|Wert von **message_type**|BESCHREIBUNG|  
|-----------------------------|-----------------|  
|-1|Unknown|  
|120|Fehler|  
|110|Warnung|  
|70|Information|  
|10|Vor der Überprüfung|  
|20|Nach der Überprüfung|  
|30|Vor der Ausführung|  
|40|Nach der Ausführung|  
|60|Fortschritt|  
|50|StatusChange|  
|100|QueryCancel|  
|130|TaskFailed|  
|90|Diagnostic|  
|200|Benutzerdefiniert|  
|140|DiagnosticEx<br /><br /> Jedes Mal, wenn der Task "Paket ausführen" ein untergeordnetes Paket ausführt, wird dieses Ereignis protokolliert. Die Ereignismeldung besteht aus den Parameterwerten, die an untergeordnete Pakete übergeben wurden.<br /><br /> Der Wert der Meldungsspalte für DiagnosticEx ist XML-Text.|  
|400|NonDiagnostic|  
|80|VariableValueChanged|  
  
 In dieser Sicht werden die folgenden Meldungsquelltypen angezeigt:  
  
|**message_source_type**|Beschreibung|  
|-------------------------------|-----------------|  
|10|Eintrag-APIs, z. B. T-SQL und gespeicherte CLR-Prozeduren|  
|20|Externer Prozess, der verwendet wurde, um das Paket (ISServerExec.exe) auszuführen|  
|30|Objekte auf Paketebene|  
|40|Ablaufsteuerungsaufgaben|  
|50|Ablaufsteuerungscontainer|  
|60|Datenflusstask|  
  
## <a name="permissions"></a>Berechtigungen  
 Diese Sicht erfordert eine der folgenden Berechtigungen:  
  
-   READ-Berechtigung für den Vorgang  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
> [!NOTE]  
>  Wenn Sie über die Berechtigung verfügen, einen Vorgang auf dem Server auszuführen, verfügen Sie auch über die Berechtigung, Informationen zu dem Vorgang anzuzeigen. Sicherheit auf Zeilenebene wird erzwungen. Es werden nur Zeilen angezeigt, zu deren Anzeige Sie berechtigt sind.  
  
  
