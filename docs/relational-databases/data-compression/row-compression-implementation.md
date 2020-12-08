---
title: Implementierung von Zeilenkomprimierung | Microsoft-Dokumentation
description: Erfahren Sie, wie die SQL Server-Datenbank-Engine die Zeilenkomprimierung implementiert, um Sie bei der Planung des Speicherplatzes zu unterstützen, den Sie für Ihre Daten benötigen.
ms.custom: ''
ms.date: 06/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- compression [SQL Server], row
- row compression [Database Engine]
ms.assetid: dcd97ac1-1c85-4142-9594-9182e62f6832
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 829229371dfecd55a56fdbb9a6530635a6170904
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506352"
---
# <a name="row-compression-implementation"></a>Row Compression Implementation
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  In diesem Thema wird zusammengefasst, wie [!INCLUDE[ssDE](../../includes/ssde-md.md)] Zeilenkomprimierung implementiert. Diese Zusammenfassung enthält grundlegende Informationen, die Ihnen bei der Planung des Speicherplatzes helfen, den Sie für Ihre Daten benötigten.  
  
 Durch die Aktivierung der Komprimierung wird nur das physische Speicherformat der Daten eines bestimmten Datentyps geändert, jedoch nicht deren Syntax oder Semantik. Änderungen der Anwendung sind nicht erforderlich, wenn eine oder mehrere Tabellen für die Komprimierung aktiviert werden. Das neue Datensatzspeicherformat bewirkt die folgenden wichtigen Änderungen:  
  
-   Es verringert den mit dem Datensatz verbundenen Metadaten-Overhead. Diese Metadaten umfassen Informationen zu Spalten, deren Längen und Offsets. In einigen Fällen ist der Metadaten-Overhead möglicherweise größer als das alte Speicherformat.  
  
-   Es verwendet ein Speicherformat variabler Länge für numerische Datentypen (z.B. **integer**, **decimal** und **float**) und die Datentypen, die auf numerischen Werten basieren (z.B. **datetime** und **money**).  
  
-   Es speichert feste Zeichenfolgen in einem Format variabler Länge, ohne die Leerzeichen zu speichern.  
  
> [!NOTE]  
>  NULL- und 0-Werte werden für alle Datentypen optimiert und beanspruchen keine Bytes.  
  
## <a name="how-row-compression-affects-storage"></a>Wie Zeilenkomprimierung den Speicherplatz beeinflusst  
 Die folgende Tabelle beschreibt, wie Zeilenkomprimierung die vorhandenen Typen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)]beeinflusst. Die Tabelle schließt nicht die Speicherplatzersparnis ein, die mit Seitenkomprimierung erreicht werden kann.  
  
|Datentyp|Wird der Speicherplatz beeinflusst?|BESCHREIBUNG|  
|---------------|--------------------------|-----------------|  
|**tinyint**|Nein|1 Byte ist der minimal benötigte Speicherplatz.|  
|**smallint**|Ja|Wenn 1 Byte für den Wert ausreicht, wird nur 1 Byte verwendet.|  
|**int**|Ja|Verwendet nur so viele Bytes wie nötig. Wenn ein Wert z. B. in 1 Byte gespeichert werden kann, wird nur 1 Byte Speicherplatz belegt.|  
|**bigint**|Ja|Verwendet nur so viele Bytes wie nötig. Wenn ein Wert z. B. in 1 Byte gespeichert werden kann, wird nur 1 Byte Speicherplatz belegt.|  
|**decimal**|Ja|Dieser Speicherplatz entspricht genau dem vardecimal-Speicherformat.|  
|**numeric**|Ja|Dieser Speicherplatz entspricht genau dem vardecimal-Speicherformat.|  
|**bit**|Ja|Der Metadaten-Overhead benötigt 4 Bits.|  
|**smallmoney**|Ja|Verwendet die Datendarstellung mit ganzen Zahlen mit einer 4 Bytes langen ganzen Zahl. Der Währungswert wird mit 10000 multipliziert und nachdem alle Stellen nach dem Dezimalkomma entfernt wurden, wird der resultierende ganzzahlige Wert gespeichert. Die Speicheroptimierung dieses Typs ähnelt der für ganzzahlige Typen.|  
|**money**|Ja|Verwendet die Datendarstellung mit ganzen Zahlen mit einer 8 Bytes langen ganzen Zahl. Der Währungswert wird mit 10000 multipliziert und nachdem alle Stellen nach dem Dezimalkomma entfernt wurden, wird der resultierende ganzzahlige Wert gespeichert. Dieser Typ hat einen größeren Bereich als **smallmoney**. Die Speicheroptimierung dieses Typs ähnelt der für ganzzahlige Typen.|  
|**float**|Ja|Geringwertige Bytes mit Nullen werden nicht gespeichert. Die **float** -Komprimierung eignet sich hauptsächlich für nicht gebrochene Werte in Mantissen.|  
|**real**|Ja|Geringwertige Bytes mit Nullen werden nicht gespeichert. Die **real** -Komprimierung eignet sich hauptsächlich für nicht gebrochene Werte in Mantissen.|  
|**smalldatetime**|Nein|Verwendet die Datendarstellung mit ganzen Zahlen mit 2 Bytes langen ganzen Zahlen. Das Datum benötigt 2 Bytes. Das Datum ist die Anzahl der Tage seit dem 1.1.1901. Es benötigt 2 Bytes ab 1902. Deshalb gibt es keine Speicherplatzersparnis nach diesem Punkt.<br /><br /> Die Zeit ist die Anzahl von Minuten seit Mitternacht. Zeitwerte, die geringfügig nach 4:00 Uhr liegen, verwenden das zweite Byte.<br /><br /> Wenn ein **smalldatetime** nur für die Darstellung eines Datums verwendet wird (geschieht häufig), ist die Zeit 0.0. Mit der Komprimierung werden 2 Bytes gespart, indem die Zeit im höchstwertigen Byteformat für Zeilenkomprimierung gespeichert wird.|  
|**datetime**|Ja|Verwendet die Datendarstellung mit ganzen Zahlen mit 4 Bytes langen ganzen Zahlen. Der ganzzahlige Wert stellt die Anzahl von Tagen mit dem Basisdatum 1.1.1900 dar. Die ersten 2 Bytes können die Jahre bis 2079 darstellen. Mit der Komprimierung können hier bis zu diesem Punkt immer 2 Bytes gespart werden. Jeder ganzzahlige Wert stellt 3,33 Millisekunden dar. Die Komprimierung schöpft die ersten 2 Bytes in den ersten fünf Minuten aus und benötigt das vierte Byte nach 16:00 Uhr. Deshalb wird mit der Komprimierung nach 16:00 Uhr nur 1 Byte gespart. Wenn **datetime** wie jede andere ganze Zahl komprimiert wird, werden mit der Komprimierung 2 Bytes beim Datum gespart.|  
|**date**|Nein|Verwendet die Datendarstellung mit ganzen Zahlen mit 3 Byte. Dies stellt das Datum ab 1.1.0001 dar. Für heutige Datumsangaben verwendet die Zeilenkomprimierung alle 3 Bytes. Dadurch wird keine Speicherplatzersparnis erreicht.|  
|**time**|Nein|Verwendet die Datendarstellung mit ganzen Zahlen mit 3 bis 6 Bytes. Es gibt verschiedene Genauigkeiten, die mit 0 bis 9 beginnen, die 3 bis 6 Bytes benötigen können. Komprimierter Speicherplatz wird folgendermaßen verwendet:<br /><br /> **Genauigkeit = 0. Bytes = 3**. Jeder ganzzahlige Wert stellt eine Sekunde dar. Die Komprimierung kann mit 2 Bytes Zeit bis 18:00 Uhr darstellen, wodurch potenziell 1 Byte gespart wird.<br /><br /> **Genauigkeit = 1. Bytes = 3**. Jeder ganzzahlige Wert stellt 1/10 Sekunde dar. Die Komprimierung verwendet das dritte Byte vor 2:00 Uhr. Resultiert in einer geringen Speicherplatzersparnis.<br /><br /> **Genauigkeit = 2. Bytes = 3**. Ähnlich wie beim vorherigen Fall wird wahrscheinlich keine Speicherplatzersparnis erreicht.<br /><br /> **Genauigkeit = 3. Bytes = 4**. Geringe Speicherplatzersparnis, da die ersten 3 Bytes bis 5:00 Uhr genommen werden.<br /><br /> **Genauigkeit = 4. Bytes = 4**. Die ersten 3 Bytes werden in den ersten 27 Sekunden genommen. Es wird keine Speicherplatzersparnis erwartet.<br /><br /> **Genauigkeit = 5, Bytes = 5**. Das fünfte Byte wird nach 12:00 Uhr verwendet.<br /><br /> **Genauigkeit = 6 und 7, Bytes = 5**. Es wird keine Speicherplatzersparnis erreicht.<br /><br /> **Genauigkeit = 8, Bytes = 6**. Das sechste Byte wird nach 3:00 Uhr verwendet.<br /><br /> <br /><br /> Bei der Zeilenkomprimierung ergibt sich keine Änderung des Speicherplatzes. Allgemein ist bei der Komprimierung des **time** -Datentyps keine große Speicherplatzersparnis zu erwarten.|  
|**datetime2**|Ja|Verwendet die Datendarstellung mit ganzen Zahlen mit 6 bis 9 Bytes. Die ersten 4 Bytes stellen das Datum dar. Die Anzahl der von der Zeit in Anspruch genommenen Bytes hängt von der Genauigkeit der Zeit ab, die angegeben wird.<br /><br /> Der ganzzahlige Wert stellt die Anzahl von Tagen ab dem 1.1.0001 mit der Obergrenze 31.12.9999 dar. Die Komprimierung nimmt 3 Bytes ein, um ein Datum im Jahr 2005 darzustellen.<br /><br /> Bei der Zeit gibt es keine Speicherplatzersparnis, da hier 2 bis 4 Bytes für verschiedene Zeitgenauigkeiten zulässig sind. Deshalb verwendet die Komprimierung für eine Zeitgenauigkeit von einer Sekunde 2 Bytes für die Zeit, wobei das zweite Byte nach 255 Sekunden genommen wird.|  
|**datetimeoffset**|Ja|Ähnelt **datetime2** mit Ausnahme der vorhandenen 2 Bytes für die Zeitzone mit dem Format (HH:MM).<br /><br /> Wie bei **datetime2** können mit der Komprimierung 2 Bytes gespart werden.<br /><br /> Für Zeitzonenwerte ist der MM-Wert in den meisten Fällen möglicherweise 0. Deshalb kann mit der Komprimierung möglicherweise 1 Byte gespart werden.<br /><br /> Bei der Zeilenkomprimierung ändert sich der Speicherplatz nicht.|  
|**char**|Ja|Nachfolgende Auffüllungszeichen werden entfernt. Beachten Sie, dass [!INCLUDE[ssDE](../../includes/ssde-md.md)] unabhängig von der verwendeten Sortierung immer dasselbe Auffüllungszeichen einfügt.|  
|**varchar**|Nein|Keine Auswirkung.|  
|**text**|Nein|Keine Auswirkung.|  
|**nchar**|Ja|Nachfolgende Auffüllungszeichen werden entfernt. Beachten Sie, dass [!INCLUDE[ssDE](../../includes/ssde-md.md)] unabhängig von der verwendeten Sortierung immer dasselbe Auffüllungszeichen einfügt.|  
|**nvarchar**|Nein|Keine Auswirkung.|  
|**ntext**|Nein|Keine Auswirkung.|  
|**binary**|Ja|Nachfolgende Nullen werden entfernt.|  
|**varbinary**|Nein|Keine Auswirkung.|  
|**image**|Nein|Keine Auswirkung.|  
|**Cursor**|Nein|Keine Auswirkung.|  
|**timestamp** / **rowversion**|Ja|Verwendet die Datendarstellung mit ganzen Zahlen mit 8 Byte. Es gibt für jede Datenbank einen Zeitstempelzähler, dessen Wert bei 0 beginnt. Dieser Wert kann wie jeder andere ganzzahlige Wert komprimiert werden.|  
|**sql_variant**|Nein|Keine Auswirkung.|  
|**uniqueidentifier**|Nein|Keine Auswirkung.|  
|**Tabelle**|Nein|Keine Auswirkung.|  
|**xml**|Nein|Keine Auswirkung.|  
|Benutzerdefinierte Typen|Nein|Dies wird intern als **varbinary** dargestellt.|  
|FILESTREAM|Nein|Dies wird intern als **varbinary** dargestellt.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenkomprimierung](../../relational-databases/data-compression/data-compression.md)   
 [Implementierung von Seitenkomprimierung](../../relational-databases/data-compression/page-compression-implementation.md)  
  
  
