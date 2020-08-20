---
description: sys.dm_clr_properties (Transact-SQL)
title: sys. dm_clr_properties (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_clr_properties
- sys.dm_clr_properties_TSQL
- dm_clr_properties_TSQL
- dm_clr_properties
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_properties dynamic management view
ms.assetid: 220d062f-d117-46e7-a448-06fe48db8163
author: CarlRabeler
ms.author: carlrab
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7db1f2a88248f01929326f02cf19cd42ac5a5e6f
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88498396"
---
# <a name="sysdm_clr_properties-transact-sql"></a>sys.dm_clr_properties (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Gibt eine Zeile für jede Eigenschaft in Verbindung mit der CLR-Integration (Common Language Runtime) von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zurück, einschließlich der Version und des Status der gehosteten CLR. Die gehostete CLR wird durch Ausführen der Anweisungen [Create Assembly](../../t-sql/statements/create-assembly-transact-sql.md), [Alter Assembly](../../t-sql/statements/alter-assembly-transact-sql.md)oder [Drop Assembly](../../t-sql/statements/drop-assembly-transact-sql.md) oder durch Ausführen einer beliebigen CLR-Routine, eines Typs oder eines Auslösers initialisiert. Die **sys. dm_clr_properties** -Sicht gibt nicht an, ob die Ausführung von Benutzer-CLR-Code auf dem Server aktiviert wurde. Die Ausführung von CLR-Benutzercode wird mithilfe der gespeicherten Prozedur [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) aktiviert, bei der die Option [CLR-fähig](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) auf 1 festgelegt ist.  
  
 Die **sys. dm_clr_properties** -Sicht enthält die Spalten **Name** und **Wert** . Jede Zeile in dieser Sicht enthält Details zu einer Eigenschaft der gehosteten CLR. Mit dieser Sicht können Sie Informationen zur gehosteten CLR zusammentragen, z. B. das CLR-Installationsverzeichnis, die CLR-Version und den aktuellen Status der gehosteten CLR. Mit dieser Sicht können Sie bestimmen, ob der CLR-Integrationscode aufgrund von Problemen mit der CLR-Installation auf dem Servercomputer nicht funktioniert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**name**|**nvarchar(128)**|Den Namen der Eigenschaft.|  
|**value**|**nvarchar(128)**|Der Wert der Eigenschaft.|  
  
## <a name="properties"></a>Eigenschaften  
 Die **Directory** -Eigenschaft gibt das Verzeichnis an, in dem der .NET Framework auf dem Server installiert wurde. Möglicherweise sind mehrere Installationen von .NET Framework auf dem Servercomputer vorhanden, und durch den Wert dieser Eigenschaft wird angegeben, welche Installation derzeit von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet wird.  
  
 Die **Version** -Eigenschaft gibt die Version der .NET Framework und der gehosteten CLR auf dem Server an.  
  
 Die dynamische verwaltete Sicht **sys. dm_clr_properties** kann sechs verschiedene Werte für die **State** -Eigenschaft zurückgeben, die den Status der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gehosteten CLR widerspiegeln. Sie lauten wie folgt:  
  
-   Mscoree is not loaded.  
  
-   Mscoree is loaded.  
  
-   Locked CLR version with mscoree.  
  
-   CLR is initialized.  
  
-   CLR initialization permanently failed.  
  
-   CLR is stopped.  
  
 **Mscoree ist nicht geladen** , und **mscoree ist geladen** und zeigt den Fortschritt der gehosteten CLR-Initialisierung beim Serverstart an und wird wahrscheinlich nicht angezeigt.  
  
 Die **Gesperrte CLR-Version mit dem Mscoree** -Status kann angezeigt werden, wenn die gehostete CLR nicht verwendet wird und daher noch nicht initialisiert wurde. Die gehostete CLR wird initialisiert, wenn eine DDL-Anweisung (z. b. [Create Assembly &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)) oder ein verwaltetes Datenbankobjekt ausgeführt wird.  
  
 Der Zustand der **CLR-Initialisierung ist** ein Hinweis darauf, dass die gehostete CLR erfolgreich initialisiert wurde. Beachten Sie, dass dadurch nicht angegeben wird, ob die Ausführung von benutzerdefiniertem CLR-Code aktiviert wurde. Wenn die Ausführung des Benutzer-CLR-Codes zuerst aktiviert und dann mithilfe der [!INCLUDE[tsql](../../includes/tsql-md.md)] gespeicherten Prozedur [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) deaktiviert wird, wird der Statuswert immer noch als **CLR initialisiert**.  
  
 Der Status der **CLR-Initialisierung ist dauerhaft fehlgeschlagen** gibt an, dass die gehostete CLR-Initialisierung Mögliche Gründe sind ungenügend Arbeitsspeicher oder ein Fehler im Hosthandshake zwischen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und der CLR. In diesem Fall wird die Fehlermeldung 6512 oder 6513 ausgegeben.  
  
 Der **Zustand der CLR** -Beendigung ist nur sichtbar, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gerade heruntergefahren wird.  
  
## <a name="remarks"></a>Bemerkungen  
 Die Eigenschaften und Werte dieser Sicht können sich in einer zukünftigen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aufgrund von Verbesserungen der CLR-Integrations Funktionalität ändern.  
  
## <a name="permissions"></a>Berechtigungen  
  
In [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ist die- `VIEW SERVER STATE` Berechtigung erforderlich.   
Bei [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] Premium-Tarifen ist die- `VIEW DATABASE STATE` Berechtigung in der Datenbank erforderlich. In [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] den Tarifen "Standard" und "Basic" ist der  **Server Administrator** oder ein **Azure Active Directory Administrator** Konto erforderlich.   

## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel werden Informationen zur gehosteten CLR abgerufen:  
  
```  
SELECT name, value   
FROM sys.dm_clr_properties;  
```  
  
## <a name="see-also"></a>Siehe auch  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungs Sichten im Zusammenhang mit der Common Language Runtime &#40;Transact-SQL-&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
  
