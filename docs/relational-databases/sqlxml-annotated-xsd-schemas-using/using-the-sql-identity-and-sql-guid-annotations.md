---
title: Verwenden der Anmerkungen 'sql:identity' und 'sql:guid'
description: 'Erfahren Sie, wie Sie die Anmerkungen "SQL: Identity" und "SQL: GUID" in einem XSD-Schema verwenden, um das Verhalten eines XML-Update grams zu definieren.'
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- sql:guid
- annotated XSD schemas, IDENTITY-type columns
- annotated XSD schemas, GUID values
- DiffGrams [SQLXML], annotations
- identity annotation
- XSD schemas [SQLXML], GUID values
- GUIDs [SQLXML]
- guid annotation
- sql:identity
- IDENTITY-type column
- XSD schemas [SQLXML], IDENTITY-type columns
- updategrams [SQLXML], GUID values
ms.assetid: 7661dfd0-6573-4692-a8f1-3597adcd33c4
author: MightyPen
ms.author: genemi
ms.reviewer: ''
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7c66bac3f83aff00a458d3fed14296d2b03071f7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479321"
---
# <a name="using-the-sqlidentity-and-sqlguid-annotations"></a>Verwenden der Anmerkungen 'sql:identity' und 'sql:guid'
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Sie können die Anmerkungen " **SQL: Identity** " und " **SQL: GUID** " in einem XSD-Schema auf allen Knoten angeben, die einer Daten Bank Spalte in zugeordnet sind [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Das Update Gram-Format unterstützt zwar die Attribute **updg: at-Identity** und **updg: GUID** , aber das DiffGram-Format nicht. Das **updg: at-Identity** -Attribut definiert das Verhalten beim Aktualisieren einer Spalte vom Typ Identity. Mithilfe des **updg: GUID** -Attributs können Sie einen GUID-Wert von abrufen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und im Update Gram verwenden. Weitere Informationen und funktionierende Beispiele finden Sie unter [Einfügen von Daten mit XML-Update grams &#40;SQLXML 4,0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/inserting-data-using-xml-updategrams-sqlxml-4-0.md).  
  
 Die Anmerkungen " **SQL: Identity** " und " **SQL: GUID** " erweitern diese Funktionalität auf DiffGrams.  
  
 Wenn Sie ein DiffGram ausführen, wird es zunächst in ein Updategram konvertiert, dann wird das Updategram ausgeführt. Wenn Sie die Anmerkungen " **SQL: Identity** " und " **SQL: GUID** " im XSD-Schema angeben, definieren Sie tatsächlich das Verhalten eines Update grams. Daher werden alle Anmerkungen im Kontext eines Updategrams beschrieben. Die Anmerkungen können sowohl für DiffGrams als auch für Updategrams verwendet werden. Updategrams stellen jedoch ein leistungsfähigeres Verfahren zur Verarbeitung von Identitäts- und GUID-Werten dar.  
  
 Die Anmerkungen " **SQL: Identity** " und " **SQL: GUID** " können für ein komplexes Inhalts Element definiert werden.  
  
## <a name="sqlidentity-annotation"></a>'sql:identity'-Anmerkung  
 Sie können die **SQL: Identity** -Anmerkung im XSD-Schema auf jedem Knoten angeben, der einer Daten Bank Spalte vom Typ IDENTITY zugeordnet ist. Der für diese Anmerkung angegebene Wert definiert, wie die Identity-Type-Spalte aktualisiert wird (entweder mit dem im Update Gram angegebenen Wert, um die Spalte zu ändern, oder indem der Wert ignoriert wird. in diesem Fall wird ein von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generierter Wert für diese Spalte verwendet).  
  
 Der **SQL: Identity** -Anmerkung können zwei Werte zugewiesen werden:  
  
 ignore  
 Weist das Updategram an, einen für diese Spalte im Updategram vorhandenen Wert zu ignorieren und es [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu überlassen, den Identitätswert zu generieren.  
  
 useValue  
 Weist das Updategram an, den im Updategram angegebenen Wert zur Aktualisierung der Spalte vom Typ IDENTITY zu verwenden. Ein Updategram prüft nicht, ob es sich bei der Spalte um eine Identitätsspalte handelt.  
  
 Wenn das Update Gram einen Wert für die Spalte Identity-Type angibt, muss **SQL: Identity = "useValue"** im Schema angegeben werden.  
  
## <a name="sqlguid-annotation"></a>'sql:guid'-Anmerkung  
 Ein Updategram kann [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] veranlassen, einen GUID-Wert zu generieren, der dann im Updategram verwendet wird. Im Kontext von DiffGrams können Sie mithilfe der **SQL: GUID** -Anmerkung angeben, ob ein von SQL Server generierter GUID-Wert verwendet oder der im Update Gram für diese Spalte angegebene Wert verwendet werden soll.  
  
 Der **SQL: GUID** -Anmerkung können zwei Werte zugewiesen werden:  
  
 generate  
 Gibt an, dass der von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generierte GUID-Wert im Updatevorgang für diese Spalte verwendet werden soll.  
  
 useValue  
 Gibt an, dass der im Updategram angegebene Wert für die Spalte verwendet werden soll. Dies ist der Standardwert.  
  
  
