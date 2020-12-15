---
title: Zuordnung von XSD-Datentypen zu XPath-Datentypen (SQLXML)
description: Informationen zum Zuordnen von XSD-Datentypen zu XPath-Datentypen beim Ausführen einer XPath-Abfrage in SQLXML 4,0.
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- mapping data types [SQLXML]
- data types [SQLXML], converting
- annotated XSD schemas, mapping data types
- XPath queries [SQLXML], mapping data types
- converting data types [SQLXML]
- data types [SQLXML], mapping data types
- XPath data types [SQLXML]
- XSD schemas [SQLXML], mapping data types
ms.assetid: ced1a95e-18d4-4a5a-8da8-dbb6d58bbd45
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 75a7ef44e18566781215bab806caa43860c57cb7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97415732"
---
# <a name="mapping-xsd-data-types-to-xpath-data-types-sqlxml-40"></a>Zuordnen von XSD-Datentypen zu XPath-Datentypen (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Wenn eine XPath-Abfrage für ein XSD-Schema ausgeführt wird und der XSD-Typ im **xsd: Type** -Attribut angegeben ist, verwendet XPath den Datentyp, der beim Verarbeiten der Abfrage angegeben ist.  
  
 Der XPath-Datentyp eines Knotens wird vom XSD-Datentyp in dem Schema abgeleitet, wie in der folgenden Tabelle dargestellt. (Der Knoten "EmployeeID" dient zur Veranschaulichung.)  
  
|XSD-Datentyp|XDR-Datentyp|Entsprechung<br /><br /> XPath-Datentyp|SQL Server<br /><br /> verwendete Konvertierung|  
|-------------------|-------------------|------------------------------------|--------------------------------------------|  
|**Base64Binary**<br /><br /> **HexBinary**|**Keine**<br /><br /> **bin. base64bin. Hex**|**Nicht zutreffend**|Keine<br /><br /> EmployeeID|  
|**Boolean**|**boolean**|**boolean**|CONVERT(bit, EmployeeID)|  
|**Decimal, Integer, float, Byte, Short, int, Long, float, Double, unsignedByte, unsignedshort, unsignetdint, unsignedLong**|**number, int, float,i1, i2, i4, i8,r4, r8ui1, ui2, ui4, ui8**|**Zahl**|CONVERT(float(53), EmployeeID)|  
|**ID, IDREF, IDREF sentity, Entities, Notation, NMTOKEN, NMTOKENS, DateTime, String, anyURI**|**ID, IDREF, IDREF sentity, Entities, Enumeration, Notation, NMTOKEN, NMTOKENS, Char, DateTime, DateTime.TZ, String, Uri, UUID**|**string**|CONVERT(nvarchar(4000), EmployeeID, 126)|  
|**decimal**|**fixed14.4**|**Nicht zutreffend (es gibt keinen Datentyp in XPath, der dem festgelegten Datentyp "14,4 XDR" entspricht.)**|CONVERT(money, EmployeeID)|  
|**date**|**date**|**string**|LEFT(CONVERT(nvarchar(4000), EmployeeID, 126), 10)|  
|**time**|**time**<br /><br /> **time.tz**|**string**|SUBSTRING(CONVERT(nvarchar(4000), EmployeeID, 126), 1 + CHARINDEX(N'T', CONVERT(nvarchar(4000), EmployeeID, 126)), 24)|  
  
  
