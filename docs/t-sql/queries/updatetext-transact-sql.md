---
description: UPDATETEXT (Transact-SQL)
title: UPDATETEXT (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- UPDATETEXT
- UPDATETEXT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- text updates [SQL Server]
- updating data [SQL Server]
- data updates [SQL Server], UPDATETEXT statement
- UPDATETEXT statement
ms.assetid: d73c28ee-3972-4afd-af8d-ebbbd9e50793
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8444f4c3421f41cb94cdd716b1c2017f506b80c2
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "91114710"
---
# <a name="updatetext-transact-sql"></a>UPDATETEXT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Aktualisiert ein vorhandenes **text**-, **ntext**-, oder **image**-Feld. Verwenden Sie UPDATETEXT, um nur einen Teil einer vorhandenen **text**-, **ntext**- oder **image**-Spalte zu ändern. Verwenden Sie WRITETEXT, um ein ganzes **text**-, **ntext**- oder **image**-Feld zu aktualisieren und zu ersetzen.  
  
> [!IMPORTANT]
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Verwenden Sie stattdessen die Datentypen für große Werte und die **.** WRITE-Klausel der [UPDATE](../../t-sql/queries/update-transact-sql.md)-Anweisung.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
UPDATETEXT [BULK] { table_name.dest_column_name dest_text_ptr }  
  { NULL | insert_offset }  
     { NULL | delete_length }  
     [ WITH LOG ]  
     [ inserted_data  
    | { table_name.src_column_name src_text_ptr } ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 BULK  
 Aktiviert Uploadtools, um einen Binärdaten-Datenstrom hochzuladen. Der Datenstrom muss vom Tool auf TDS-Protokollebene bereitgestellt werden. Wenn der Datenstrom nicht vorhanden ist, ignoriert der Abfrageprozessor die BULK-Option.  
  
> [!IMPORTANT]  
>  Es wird empfohlen, die BULK-Option nicht in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-basierten Anwendungen zu verwenden. Diese Option kann in einer zukünftigen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] geändert oder entfernt werden.  
  
 *table_name* **.** *dest_column_name*  
 Der Name der zu aktualisierenden Tabelle oder der zu aktualisierenden Spalte vom Datentyp **text**, **ntext** oder **image**. Tabellen- und Spaltennamen müssen den Regeln für [Bezeichner](../../relational-databases/databases/database-identifiers.md) entsprechen. Das Angeben des Datenbank- und des Besitzernamens ist optional.  
  
 *dest_text_ptr*  
 Ein Textzeigerwert (von der TEXTPTR-Funktion zurückgegeben), der auf die zu aktualisierenden Daten von **text**, **ntext** oder **image** zeigt. *dest_text_ptr* muss vom Datentyp **binary(** 16 **)** sein.  
  
 *insert_offset*  
 Die nullbasierte Startposition für das Update. Für **text**- oder **image**-Spalten stellt *insert_offset* die Zahl der ab dem Beginn der vorhandenen Spalte auszulassenden Bytes dar, bevor neue Daten eingefügt werden. Für **ntext**-Spalten stellt *insert_offset* die Anzahl von Zeichen dar (jedes **ntext**-Zeichen verwendet 2 Bytes). Die vorhandenen **text**-, **ntext**- oder **image**-Daten, die an dieser nullbasierten Startposition beginnen, werden nach rechts verschoben, um Platz für die neuen Daten zu schaffen. Mit dem Wert 0 werden die neuen Daten am Beginn der vorhandenen Daten eingefügt. Mit dem Wert NULL werden die neuen Daten an den vorhandenen Datenwert angefügt.  
  
 *delete_length*  
 Die Länge der Daten, die aus der vorhandenen **text**-, **ntext**- oder **image**-Spalte gelöscht werden sollen, ausgehend von der *insert_offset*-Position. Der Wert *delete_length* wird für **text**- und **image**-Spalten in Bytes und für **ntext**-Spalten in Zeichen angegeben. Jedes **ntext**-Zeichen verwendet 2 Bytes. Mit dem Wert 0 werden keine Daten gelöscht. Mit dem Wert NULL werden alle Daten von der *insert_offset*-Position bis zum Ende der vorhandenen **text**- oder **image**-Spalte gelöscht.  
  
 WITH LOG  
 Die Protokollierung wird durch das für die Datenbank wirksame Wiederherstellungsmodell bestimmt.  
  
 *inserted_data*  
 Die Daten, die in die vorhandenen **text**-, **ntext**- oder **image**-Spalten am Speicherplatz *insert_offset* eingefügt werden sollen. Dabei handelt es sich um einen einzelnen **char**-, **nchar**-, **varchar**-, **nvarchar**-, **binary**-, **varbinary**-, **text**-, **ntext**- oder **image**-Wert. *inserted_data* kann ein Literal oder eine Variable sein.  
  
 *table_name.src_column_name*  
 Der Name der Tabelle und **text**-, **ntext**- oder **image**-Spalte, die als Quelle der eingefügten Daten verwendet wird. Tabellen- und Spaltennamen müssen den Regeln für Bezeichner entsprechen.  
  
 *src_text_ptr*  
 Ein Textzeigerwert (zurückgegeben von der TEXTPTR-Funktion), der auf eine **text**-, **ntext**- oder **image**-Spalte verweist, die als Quelle der eingefügten Daten verwendet wird.  
  
> [!NOTE]  
>  Der Wert *scr_text_ptr* darf nicht mit dem Wert *dest_text_ptr* übereinstimmen.  
  
## <a name="remarks"></a>Hinweise  
 Bei neu eingefügten Daten kann es sich um eine einzelne *inserted_data*-Konstante, einen Tabellennamen, einen Spaltennamen oder einen Textzeiger handeln.  
  
|Updateaktion|UPDATETEXT-Parameter|  
|-------------------|---------------------------|  
|Ersetzen vorhandener Daten|Geben Sie einen *insert_offset*-Wert ungleich NULL, einen *delete_length*-Wert ungleich 0 (null) und die neuen Daten an, die eingefügt werden sollten.|  
|Löschen vorhandener Daten|Geben Sie einen *insert_offset*-Wert ungleich NULL und einen *delete_length*-Wert ungleich 0 (null) an. Geben Sie keine neuen einzufügenden Daten an.|  
|Einfügen neuer Dateien|Geben Sie den *insert_offset*-Wert, einen *delete_length*-Wert von 0 (null) und die neuen Daten an, die eingefügt werden sollen.|  
  
 Für eine optimale Leistung empfiehlt es sich, Daten vom Typ **text**, **ntext** und **image** in Segmenten mit der Größe eines Vielfachen von 8,040 Bytes einzufügen oder zu aktualisieren.  
  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] können Textzeiger auf **text**-, **ntext**- oder **image**-Daten in Zeilen vorhanden sein, die jedoch möglicherweise nicht gültig sind. Informationen zur Option „text in row“ finden Sie unter [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md). Informationen dazu, wie Textzeiger ungültig gemacht werden können, finden Sie unter [sp_invalidate_textptr &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-invalidate-textptr-transact-sql.md).  
  
 Verwenden Sie WRITETEXT, um **text**-Spalten mit NULL zu initialisieren. UPDATETEXT initialisiert **text**-Spalten mit einer leeren Zeichenfolge.  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die UPDATE-Berechtigung für die angegebene Tabelle.  
  
## <a name="examples"></a>Beispiele  
 Im folgenden Beispiel wird der Textzeiger in die lokale Variable `@ptrval` eingefügt. Anschließend wird mit `UPDATETEXT` ein Rechtschreibfehler aktualisiert.  
  
> [!NOTE]  
>  Um dieses Beispiel auszuführen, müssen Sie die pubs-Datenbank installieren.  
  
```sql  
USE pubs;  
GO  
ALTER DATABASE pubs SET RECOVERY SIMPLE;  
GO  
DECLARE @ptrval BINARY(16);  
SELECT @ptrval = TEXTPTR(pr_info)   
   FROM pub_info pr, publishers p  
      WHERE p.pub_id = pr.pub_id   
      AND p.pub_name = 'New Moon Books'  
UPDATETEXT pub_info.pr_info @ptrval 88 1 'b';  
GO  
ALTER DATABASE pubs SET RECOVERY FULL;  
GO  
```  
  
## <a name="see-also"></a>Siehe auch  
 [READTEXT &#40;Transact-SQL&#41;](../../t-sql/queries/readtext-transact-sql.md)   
 [TEXTPTR &#40;Transact-SQL&#41;](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md)   
 [WRITETEXT (Transact-SQL)](../../t-sql/queries/writetext-transact-sql.md)  
  
  
