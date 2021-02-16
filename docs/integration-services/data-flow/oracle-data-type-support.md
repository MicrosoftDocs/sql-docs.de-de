---
description: 'Microsoft Connector für Oracle: Unterstützung von Datentypen'
title: 'Microsoft Connector für Oracle: Unterstützung von Datentypen | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8ef719fc0fe41120fe42d324beb563b06bb81060
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343209"
---
# <a name="microsoft-connector-for-oracle-data-type-support"></a>Microsoft Connector für Oracle: Unterstützung von Datentypen

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

SSIS-Komponenten für Oracle unterstützen nicht alle Oracle-Datentypen. Zu Spalten mit nicht unterstützten Datentypen wird beim Entwerfen von Paketen in SSDT eine Warnung angezeigt, und die Spalten werden aus den Zuordnungsspalten gelöscht. Daten können nicht in eine Spalte mit einem nicht unterstützten Datentyp geladen werden.

## <a name="data-type-mapping"></a>Datentypzuordnung

In der folgenden Tabelle werden die Datentypen der Oracle-Datenbank und deren Standardzuordnung zu SSIS-Datentypen angezeigt. Außerdem werden die nicht unterstützten Oracle-Datentypen angezeigt.

|Datentyp der Oracle-Datenbank|SSIS-Datentyp|Kommentare|
|:-|:-|:-|
|VARCHAR2|DT_STR||
|NVARCHAR2|DT_WSTR||
|CHAR|DT_STR||
|NUMBER|DT_R8|Dies kann mit spezifischer Genauigkeit und Skalierung in DT_NUMERIC geändert werden. Genauigkeit und Skalierung werden je nach Bedarf vom Benutzer definiert. Bei der Ausgabe handelt es sich um die Spaltendaten als Zahl mit fester Genauigkeit und Skalierung.|
|NUMBER(P, S)| Wenn die Skalierung 0 ist, der Genauigkeit (P) entsprechend <li> DT_I1 <Li> DT_I2 <Li> DT_I4 <Li> DT_NUMBERIC(P,0)||
||DT_NUMERIC(P,S)||
|DATE|DT_DBTIMESTAMP||
|<li>timestamp <li>TIMESTAMP WITH TIME ZONE <li>INTERVAL YEAR TO MONTH <li>INTERVAL DAY TO SECOND <li>TIMESTAMP WITH LOCAL TIME ZONE|DT_STR||
|RAW|DT_BYTES||
|CLOB|DT_TEXT|CLOB-, NCLOB- und BLOB-Datentypen werden nur im Arraymodus unterstützt, nicht im Modus für schnelles Laden.|
|NCLOB|DT_NTEXT||
|BLOB|DT_IMAGE||
|UROWID|Nicht unterstützt||
|REF|Nicht unterstützt||
|BFILE|Nicht unterstützt||
|LONG|Nicht unterstützt||
|LONG RAW|Nicht unterstützt||
|ROWID|Nicht unterstützt||
|Benutzerdefinierter Typ (Objekttyp, VARRAY, geschachtelte Tabelle)|Nicht unterstützt||

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren Sie den [Oracle-Verbindungs-Manager](oracle-connection-manager.md).
- Konfigurieren Sie die [Oracle-Quelle](oracle-source.md).
- Konfigurieren Sie das [Oracle-Ziel](oracle-destination.md).
- Wenn Sie Fragen haben, besuchen Sie die [TechCommunity](https://aka.ms/AA5u35j).
