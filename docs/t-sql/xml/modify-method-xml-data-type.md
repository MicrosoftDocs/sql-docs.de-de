---
description: modify()-Methode (xml-Datentyp)
title: modify()-Methode (xml-Datentyp)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- modify() method
- modify method
ms.assetid: 52430735-51f4-46d1-a308-9aecf8648fda
author: MightyPen
ms.author: genemi
ms.openlocfilehash: f7064750b46e57572c8437858d5527f4b1e5cba4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172459"
---
# <a name="modify-method-xml-data-type"></a>modify()-Methode (xml-Datentyp)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Ändert den Inhalt eines XML-Dokuments. Verwenden Sie diese Methode, um den Inhalt einer Spalte oder Variablen des **xml** -Datentyps zu ändern. Diese Methode verwendet eine XML DML-Anweisung, um Knoten in XML-Daten einzufügen, zu aktualisieren oder zu löschen. Die **modify()** -Methode des **xml** -Datentyps kann nur in der SET-Klausel einer UPDATE-Anweisung verwendet werden.  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
modify (XML_DML)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 XML_DML  
 Ist eine Zeichenfolge in XML DML (Data Manipulation Language). Das XML-Dokument wird anhand dieses Ausdrucks aktualisiert.  
  
> [!NOTE]  
>  Es wird ein Fehler zurückgegeben, wenn die **modify()** -Methode für einen NULL-Wert aufgerufen wird oder einen NULL-Wert ergibt.  
  
## <a name="examples"></a>Beispiele  
 Da für die **modify()** -Methode eine Zeichenfolge in der XML Data Manipulation Language (DML) erforderlich ist, sind die Beispiele für **modify()** in den Themen zur Beschreibung der XML DML-Anweisungen enthalten. Diese Beispiele finden Sie unter [insert &#40;XML DML&#41;](../../t-sql/xml/insert-xml-dml.md), [delete &#40;XML DML&#41;](../../t-sql/xml/delete-xml-dml.md) und [replace value of &#40;XML DML&#41;](../../t-sql/xml/replace-value-of-xml-dml.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen von Instanzen der XML-Daten](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [XML-Datentypmethoden](../../t-sql/xml/xml-data-type-methods.md)   
 [XML DML &#40;Data Modification Language&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  
