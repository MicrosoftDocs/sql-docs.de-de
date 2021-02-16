---
description: ADD SENSITIVITY CLASSIFICATION (Transact-SQL)
title: ADD SENSITIVITY CLASSIFICATION (Transact-SQL)
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
f1_keywords:
- ADD SENSITIVITY CLASSIFICATION
- ADD_SENSITIVITY_CLASSIFICATION
helpviewer_keywords:
- ADD SENSITIVITY CLASSIFICATION statement
- add labels
- adding labels
- adding labels
- classification [SQL]
- labels [SQL]
- information types
- data classification
- rank
ms.custom: ''
ms.date: 06/10/2020
monikerRange: " >= sql-server-linux-ver15 || >= sql-server-ver15 || = azuresqldb-current"
ms.openlocfilehash: 09bf980c0bfa6e07a9d2de35519439c8b18da039
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348585"
---
# <a name="add-sensitivity-classification-transact-sql"></a>ADD SENSITIVITY CLASSIFICATION (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Diese Anweisung fügt einer oder mehreren Datenbankspalten Metadaten zur Vertraulichkeitsklassifizierung hinzu. Die Klassifizierung kann eine Vertraulichkeitsbezeichnung und einen Informationstyp umfassen.

Für SQL Server wurde dies in SQL Server 2019 eingeführt.

Die Klassifizierung sensibler Daten in Ihrer Datenbankumgebung ermöglicht größere Transparenz und besseren Schutz. Weitere Informationen finden Sie unter [Azure SQL-Datenbank – Datenermittlung und -klassifizierung](/azure/azure-sql/database/data-discovery-and-classification-overview).

## <a name="syntax"></a>Syntax

```syntaxsql
    ADD SENSITIVITY CLASSIFICATION TO
    <object_name> [, ...n ]
    WITH ( <sensitivity_option> [, ...n ] )

<object_name> ::=
{
    [schema_name.]table_name.column_name
}

<sensitivity_option> ::=  
{
    LABEL = string |
    LABEL_ID = guidOrString |
    INFORMATION_TYPE = string |
    INFORMATION_TYPE_ID = guidOrString |
    RANK = NONE | LOW | MEDIUM | HIGH | CRITICAL
}
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente  

*object_name* ([schema_name.]table_name.column_name)

Der Name der Datenbankspalte, die klassifiziert werden soll. Derzeit wird ausschließlich die Spaltenklassifizierung unterstützt.
    - *schema_name* (optional): Der Name des Schemas, zu dem die klassifizierte Spalte gehört.
    - *table_name* (optional): Der Name der Tabelle, zu der die klassifizierte Spalte gehört.
    - *column_name*: Der Name der Spalte, die klassifiziert wird.

*LABEL*

Der lesbare Name der Vertraulichkeitsbezeichnung. Vertraulichkeitsbezeichnungen stellen die Vertraulichkeit der in der Datenbankspalte gespeicherten Daten dar.

*LABEL_ID*

Ein Bezeichner, dem die Vertraulichkeitsbezeichnung zugeordnet ist. Dieser wird häufig von zentralen Plattformen zum Schutz von Informationen verwendet, um Bezeichnungen im System eindeutig zu identifizieren.

*INFORMATION_TYPE*

Der lesbare Name des Informationstyps. Informationstypen werden verwendet, um den Typ der in der Datenbankspalte gespeicherten Daten zu beschreiben.

*INFORMATION_TYPE_ID*

Ein Bezeichner, dem der Informationstyp zugeordnet ist. Dieser wird häufig von zentralen Plattformen zum Schutz von Informationen verwendet, um Informationstypen im System eindeutig zu identifizieren.

*RANK*

Bezeichner, der auf vordefinierte Werte basiert, die die Vertraulichkeitsbewertung definieren Wird von anderen Diensten wie Advanced Threat Protection verwendet, um Anomalien basierend auf ihrer Bewertung zu erkennen.

## <a name="remarks"></a>Bemerkungen  

- Einem einzelnen Objekt kann nur eine Klassifizierung hinzugefügt werden. Wird einem bereits klassifizierten Objekt eine Klassifizierung hinzugefügt, überschreibt diese die vorhandene Klassifizierung.
- Mehrere Objekte können mit einer einzigen `ADD SENSITIVITY CLASSIFICATION`-Anweisung klassifiziert werden.
- Über die Systemsicht [sys.sensitivity_classifications](../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md) können Informationen zur Vertraulichkeitsklassifizierung für eine Datenbank abgerufen werden.

## <a name="permissions"></a>Berechtigungen

Erfordert die Berechtigung ALTER ANY SENSITIVITY CLASSIFICATION. Die Berechtigung ALTER ANY SENSITIVITY CLASSIFICATION wird von der Datenbankberechtigung ALTER oder der Serverberechtigung CONTROL SERVER impliziert.

## <a name="examples"></a>Beispiele  

### <a name="a-classifying-two-columns"></a>A. Klassifizieren von zwei Spalten

Im folgenden Beispiel werden die Spalten **dbo.sales.price** und **dbo.sales.discount** mit der Vertraulichkeitsbezeichnung **Streng vertraulich**, dem Rang **Kritisch** und dem Informationstyp **Finanzen** klassifiziert.

```sql
ADD SENSITIVITY CLASSIFICATION TO
    dbo.sales.price, dbo.sales.discount
    WITH ( LABEL='Highly Confidential', INFORMATION_TYPE='Financial', RANK=CRITICAL )
```  

### <a name="b-classifying-only-a-label"></a>B. Klassifizieren von nur einer Bezeichnung

Im folgenden Beispiel wird die Spalte **dbo.customer.comments** mit der Bezeichnung **Vertraulich** und der Bezeichnungs-ID **643f7acd-776a-438d-890c-79c3f2a520d6** klassifiziert. Der Informationstyp wird für diese Spalte nicht klassifiziert.

```sql
ADD SENSITIVITY CLASSIFICATION TO
    dbo.customer.comments
    WITH ( LABEL='Confidential', LABEL_ID='643f7acd-776a-438d-890c-79c3f2a520d6' )
```  

## <a name="see-also"></a>Weitere Informationen

- [DROP SENSITIVITY CLASSIFICATION (Transact-SQL)](../../t-sql/statements/drop-sensitivity-classification-transact-sql.md)
- [sys.sensitivity_classifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md)
- [Berechtigungen (Datenbank-Engine)](../../relational-databases/security/permissions-database-engine.md)
- [Azure SQL-Datenbank – Datenermittlung und -klassifizierung](/azure/azure-sql/database/data-discovery-and-classification-overview)