---
description: sp_db_selective_xml_index (Transact-SQL)
title: sp_db_selective_xml_index (Transact-SQL)
ms.custom: ''
ms.date: 02/11/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_db_selective_xml_index_TSQL
- sp_db_selective_xml_index
dev_langs:
- TSQL
helpviewer_keywords:
- sp_db_selective_xml_index procedure
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ed30d2b8df6141b308d32dee8f2fdcec0c2eceb1
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525168"
---
# <a name="sp_db_selective_xml_index-transact-sql"></a>sp_db_selective_xml_index (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aktiviert und deaktiviert die Funktion des selektiven XML-Indexes in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank. Wenn diese ohne Parameter aufgerufen wird, gibt die gespeicherte Prozedur 1 zurück, wenn der selektive XML-Index in einer bestimmten Datenbank aktiviert ist.  

> [!NOTE]  
> Ab [!INCLUDE[sssql14-md](../../includes/sssql14-md.md)] kann die selektive XML-Index Funktion nicht deaktiviert werden. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] In [!INCLUDE[sssql11-md](../../includes/sssql11-md.md)] muss die Datenbank mithilfe des Befehls [ALTER DATABASE Set options &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) in das einfache Wiederherstellungs Modell eingefügt werden, um die selektive XML-Index Funktion mithilfe dieser gespeicherten Prozedur zu deaktivieren.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
  
      sys.sp_db_selective_xml_index[[ @dbname = ] 'dbname'],   
[[ @selective_xml_index = ] 'selective_xml_index']  
```  
  
## <a name="arguments"></a>Argumente  
`[ @ dbname = ] 'dbname'` Der Name der Datenbank, für die der selektive XML-Index aktiviert oder deaktiviert werden soll. Wenn *dbname* NULL ist, wird die aktuelle Datenbank angenommen. *@dbname* ist vom **Datentyp vom Datentyp sysname**.


`[ @selective_xml_index = ] 'selective_xml_index'` Bestimmt, ob der Index aktiviert oder deaktiviert werden soll. Zulässige Werte: ' on ', ' off ', ' true ', ' false '. Wenn ein anderer Wert mit Ausnahme von ' on ', ' true ', ' off ' oder ' false ' überschritten wird, wird ein Fehler ausgelöst. *@selective_xml_index* ist vom Datentyp **varchar (6)**.

  
## <a name="return-code-values"></a>Rückgabecodewerte  
 **1** , wenn der selektive XML-Index für eine bestimmte Datenbank aktiviert ist, **0** , wenn er deaktiviert ist.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-enable-selective-xml-index-functionality"></a>A. Aktivieren der Funktion für den selektiven XML-Index  
 Im folgenden Beispiel wird der selektive XML-Index in der aktuellen Datenbank aktiviert.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = NULL  
  , @selective_xml_index = N'on';  
GO  
```  
  
 Im folgenden Beispiel wird der selektive XML-Index in der AdventureWorks2012-Datenbank aktiviert.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = N'AdventureWorks2012'  
  , @selective_xml_index = N'true';  
GO  
```  
  
### <a name="b-disable-selective-xml-index-functionality"></a>B. Deaktivieren der Funktion für den selektiven XML-Index  
 Im folgenden Beispiel wird der selektive XML-Index in der aktuellen Datenbank deaktiviert.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = NULL  
  , @selective_xml_index = N'off';  
GO  
```  
  
 Im folgenden Beispiel wird der selektive XML-Index in der der AdventureWorks2012-Datenbank deaktiviert.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = N'AdventureWorks2012'  
  , @selective_xml_index = N'false';  
GO  
```  
  
### <a name="c-detect-if-selective-xml-index-is-enabled"></a>C. Ermitteln, ob der selektive XML-Index aktiviert ist  
 Im folgenden Beispiel wird ermittelt, ob der selektive XML-Index aktiviert ist. Gibt 1 zurück, wenn der selektive XML-Index aktiviert ist.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index;  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Selektive XML-Indizes &#40;SXI&#41;](../../relational-databases/xml/selective-xml-indexes-sxi.md)  
   