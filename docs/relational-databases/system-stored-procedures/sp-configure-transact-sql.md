---
description: sp_configure (Transact-SQL)
title: sp_configure (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_configure
- sp_configure_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_configure
ms.assetid: d18b251d-b37a-4f5f-b50c-502d689594c8
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: af05732674dcf09c6bd828125df5fe4773b62018
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071360"
---
# <a name="sp_configure-transact-sql"></a>sp_configure (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-pdw-md.md)]

  Mit dieser Prozedur können globale Konfigurationseinstellungen für den aktuellen Server angezeigt oder geändert werden.

> [!NOTE]  
> Informationen zu den Konfigurationsoptionen auf Datenbankebene finden Sie unter [ALTER DATABASE scoped Configuration &#40;Transact-SQL-&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md). Informationen zum Konfigurieren von Soft-NUMA finden Sie unter [Soft-NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md).  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql  
-- Syntax for SQL Server  
  
sp_configure [ [ @configname = ] 'option_name'   
    [ , [ @configvalue = ] 'value' ] ]  
```  
  
```  
-- Syntax for Parallel Data Warehouse  
  
-- List all of the configuration options  
sp_configure  
[;]  
  
-- Configure Hadoop connectivity  
sp_configure [ @configname= ] 'hadoop connectivity',  
             [ @configvalue = ] { 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 }  
[;]  
RECONFIGURE  
[;]  
```  
  
## <a name="arguments"></a>Argumente  
`[ @configname = ] 'option_name'` Der Name einer Konfigurationsoption. *option_name* ist vom Datentyp **varchar(35)** . Der Standardwert ist NULL. Von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] wird jede eindeutige Zeichenfolge erkannt, die Teil des Konfigurationsnamens ist. Erfolgt keine Angabe, wird die gesamte Liste der Optionen zurückgegeben.  
  
 Informationen zu den verfügbaren Konfigurationsoptionen und Ihren Einstellungen finden Sie unter [Server Configuration options &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
`[ @configvalue = ] 'value'` Ist die neue Konfigurationseinstellung. *value* ist vom Datentyp **int**. Der Standardwert ist NULL. Der Maximalwert kann je nach Option unterschiedlich sein.  
  
 Einen maximalen Wert für die einzelnen Optionen finden Sie in der Spalte **Maximum** der **sys.configurations** -Katalog Sicht.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 „0“ (erfolgreich) oder „1“ (fehlerhaft)  
  
## <a name="result-sets"></a>Resultsets  
 Bei der Ausführung ohne Parameter gibt **sp_configure** ein Resultset mit fünf Spalten zurück und sortiert die Optionen alphabetisch in aufsteigender Reihenfolge, wie in der folgenden Tabelle dargestellt.  
  
 Die Werte für **config_value** und **run_value** entsprechen nicht automatisch. Nachdem Sie eine Konfigurationseinstellung mithilfe von **sp_configure** aktualisiert haben, muss der Systemadministrator den Wert für die laufende Konfiguration entweder mithilfe von RECONFIGURE oder RECONFIGURE WITH OVERRIDE aktualisieren. Weitere Informationen finden Sie im Abschnitt mit Hinweisen.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**name**|**nvarchar(35)**|Der Name der Konfigurationsoption.|  
|**minimum**|**int**|Der Mindestwert der Konfigurationsoption.|  
|**maximum**|**int**|Der Höchstwert der Konfigurationsoption.|  
|**config_value**|**int**|Der Wert, mit dem die Konfigurationsoption mithilfe von **sp_configure** festgelegt wurde (Wert in **sys.configurations. Value**). Weitere Informationen zu diesen Optionen finden Sie unter [Server Konfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md) und [sys.configurationen &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md).|  
|**run_value**|**int**|Aktuell laufender Wert der Konfigurationsoption (Wert in **sys.configurations.value_in_use**).<br /><br /> Weitere Informationen finden Sie unter [sys.configurations &#40;Transact-SQL-&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md).|  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie **sp_configure** , um Einstellungen auf Serverebene anzuzeigen oder zu ändern. Zum Ändern von Einstellungen auf Datenbankebene können Sie ALTER DATABASE verwenden. Wenn Einstellungen geändert werden sollen, die nur die aktuelle Benutzersitzung betreffen, verwenden Sie die SET-Anweisung.  
  
### [!INCLUDE [ssbigdataclusters-ss-nover](../../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE [big-data-clusters-master-instance-ha-endpoint-requirement](../../includes/big-data-clusters-master-instance-ha-endpoint-requirement.md)]

## <a name="updating-the-running-configuration-value"></a>Aktualisieren des ausgeführten Konfigurationswerts  
 Wenn Sie einen neuen *Wert* für eine *Option* angeben, wird dieser Wert im Resultset in der **config_value** Spalte angezeigt. Dieser Wert unterscheidet sich anfänglich von dem Wert in der Spalte **run_value** , in der der aktuell laufende Konfigurations Wert angezeigt wird. Der Systemadministrator muss entweder RECONFIGURE oder RECONFIGURE WITH OVERRIDE ausführen, um den Wert für die laufende Konfiguration in der **run_value** Spalte zu aktualisieren.  
  
 RECONFIGURE und RECONFIGURE WITH OVERRIDE können für jede Konfigurationsoption verwendet werden. Die einfache RECONFIGURE-Anweisung weist jedoch Optionswerte zurück, die außerhalb eines angemessenen Bereichs liegen oder die zu Konflikten bei den Optionen führen. Beispielsweise generiert RECONFIGURE einen Fehler, wenn der Wert des **Wiederherstellungs Intervalls** größer als 60 Minuten ist oder wenn sich der **Affinitäts Masken** Wert mit dem Wert der **Affinitäts-e/a-Maske** überlappt. Von RECONFIGURE WITH OVERRIDE hingegen werden alle Optionswerte vom richtigen Datentyp akzeptiert. Eine Neukonfiguration mit dem angegebenen Wert wird dann erzwungen.  
  
> [!CAUTION]  
> Ein nicht geeigneter Optionswert kann negative Auswirkungen auf die Konfiguration der Serverinstanz haben. Verwenden Sie RECONFIGURE WITH OVERRIDE mit Sorgfalt.  
  
 Mit der RECONFIGURE-Anweisung werden einige Optionen dynamisch aktualisiert. Für andere Optionen ist jedoch das Beenden und Neustarten des Servers erforderlich. So werden z. b. die Arbeitsspeicher Optionen **Min. Server Arbeitsspeicher** und **Max. Server Arbeitsspeicher** dynamisch in aktualisiert [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Daher können Sie Sie ändern, ohne den Server neu zu starten. Im Gegensatz dazu erfordert das erneute Konfigurieren des laufenden Werts der Option **Füllfaktor** das Neustarten von [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
 Nachdem Sie RECONFIGURE für eine Konfigurationsoption ausgeführt haben, können Sie erkennen, ob die Option dynamisch aktualisiert wurde, indem Sie **sp_configure '**_option_name_*_'_* ausführen. Die Werte in den Spalten **run_value** und **config_value** sollten mit einer dynamisch aktualisierten Option verglichen werden. Sie können auch überprüfen, welche Optionen dynamisch sind, indem Sie sich die Spalte **is_dynamic** der **sys.configurations** -Katalog Sicht ansehen.  
 
 Die Änderung wird auch in das SQL Server-Fehlerprotokoll geschrieben.
  
> [!NOTE]  
>  Wenn ein angegebener *Wert* für eine Option zu hoch ist, gibt die Spalte **run_value** an, dass der standardmäßig auf dynamischer Arbeitsspeicher festgelegt ist, [!INCLUDE[ssDE](../../includes/ssde-md.md)] anstatt eine Einstellung zu verwenden, die ungültig ist.  
  
 Weitere Informationen finden Sie unter [RECONFIGURE &#40;Transact-SQL-&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md).  
  
## <a name="advanced-options"></a>Erweiterte Optionen  
 Einige Konfigurationsoptionen, wie z. b. **Affinitäts Maske** und **Wiederherstellungs Intervall**, werden als erweiterte Optionen bezeichnet. Diese Optionen stehen zum Anzeigen und Ändern nicht zur Verfügung. Um Sie verfügbar zu machen, legen Sie die Konfigurationsoption **Erweiterte Optionen anzeigen** auf 1 fest. 
 
> [!CAUTION]  
> Wenn die Option **Erweiterte Optionen anzeigen** auf 1 festgelegt ist, gilt diese Einstellung für alle Benutzer. Es wird empfohlen, diesen Status nur vorübergehend zu verwenden und zu 0 zurückzukehren, wenn der Task ausgeführt wird, der die erweiterten Optionen anzeigen muss.  
  
 Weitere Informationen zu den Konfigurationsoptionen und Ihren Einstellungen finden Sie unter [Server Konfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
## <a name="permissions"></a>Berechtigungen  
 Die Ausführungsberechtigungen für **sp_configure** ohne Parameter oder nur mit dem ersten Parameter werden standardmäßig allen Benutzern erteilt. Zum Ausführen von **sp_configure** mit beiden Parametern zum Ändern einer Konfigurationsoption oder zum Ausführen der RECONFIGURE-Anweisung muss Ihnen die ALTER SETTINGS-Berechtigung auf Serverebene erteilt werden. Die ALTER SETTINGS-Berechtigung ist in den festen Serverrollen **sysadmin** und **serveradmin** eingeschlossen.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-listing-the-advanced-configuration-options"></a>A. Auflisten der erweiterten Konfigurationsoptionen  
 Im folgenden Beispiel wird das Festlegen und Auflisten aller Konfigurationsoptionen dargestellt. Erweiterte Konfigurationsoptionen werden angezeigt, indem zunächst `show advanced option` auf `1` festgelegt wird. Nachdem diese Option geändert wurde, werden beim Ausführen von `sp_configure` ohne Parameter alle Konfigurationsoptionen angezeigt.  
  
```sql  
USE master;  
GO  
EXEC sp_configure 'show advanced option', '1';  
```  
  
 Im Folgenden wird die Meldung aufgeführt: "Die Konfigurationsoption 'show advanced options' wurde von 0 in 1 geändert. Führen Sie zum Installieren die RECONFIGURE-Anweisung aus."  
  
 Führen Sie `RECONFIGURE` aus, und zeigen Sie alle Konfigurationsoptionen an:  
  
```sql  
RECONFIGURE;  
EXEC sp_configure;  
```  
  
### <a name="b-changing-a-configuration-option"></a>B. Ändern einer Konfigurationsoption  
 Im folgenden Beispiel wird `recovery interval` für das System auf `3` Minuten festgelegt.  
  
```sql  
USE master;  
GO  
EXEC sp_configure 'recovery interval', '3';  
RECONFIGURE WITH OVERRIDE;  
```  
  
## <a name="examples-sspdw"></a>Beispiele: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-list-all-available-configuration-settings"></a>C. Auflisten aller verfügbaren Konfigurationseinstellungen  
 Im folgenden Beispiel wird dargestellt, wie alle Konfigurationsoptionen aufgelistet werden.  
  
```sql  
EXEC sp_configure;  
```  
  
 Das Ergebnis gibt den Optionsnamen zurück, gefolgt von den minimalen und maximalen Werten für die Option. Der **config_value** ist der Wert, der [!INCLUDE[ssDW](../../includes/ssdw-md.md)] verwendet wird, wenn die Neukonfiguration beendet ist. **run_value** ist der Wert, der gerade verwendet wird. **config_value** und **run_value** sind in der Regel gleich, sofern der Wert nicht gerade geändert wird.  
  
### <a name="d-list-the-configuration-settings-for-one-configuration-name"></a>D: Auflisten der Konfigurationseinstellungen für einen Konfigurationsnamen  
  
```sql  
EXEC sp_configure @configname='hadoop connectivity';  
```  
  
### <a name="e-set-hadoop-connectivity"></a>E. Festlegen der Hadoop-Konnektivität  
 Das Festlegen der Hadoop-Konnektivität erfordert neben dem Ausführen von sp_configure noch einige weitere Schritte. Das vollständige Verfahren finden Sie unter [Erstellen einer externen Datenquelle &#40;Transact-SQL-&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [SET-Anweisungen (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [Serverkonfigurationsoptionen &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Gespeicherte Systemprozeduren &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)   
 [Soft-NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md)  
  
  
