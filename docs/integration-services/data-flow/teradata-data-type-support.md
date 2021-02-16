---
description: Unterstützung von Teradata-Datentypen
title: Datentypunterstützung mit Teradata | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cb58e1d27e955009cd420a089f8d8f938f26c10f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346524"
---
# <a name="data-type-support"></a>Datentypunterstützung

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

SSIS-Komponenten verwenden die TPT-API (Teradata Parallel Transporter) zum Laden und Übertragen von Daten aus und in die Teradata-Datenbank, sodass nur von der TPT-API unterstützte Datentypen in SSIS verwendet werden können.

> [!NOTE]
>
> Die Datentypen TIME, TIMESTAMP und INTERVAL in Teradata werden von der TPT-API als Zeichenfolgen mit festgelegter Größe behandelt. Sie werden von den SSIS-Komponenten für Teradata als Zeichenfolge behandelt.

## <a name="data-type-mapping"></a>Datentypzuordnung

In der folgenden Tabelle werden die Datentypen der Teradata-Datenbank und deren Standardzuordnung zu SSIS-Datentypen angezeigt. Außerdem werden die nicht unterstützten Teradata-Datentypen angezeigt.

|SQL-Datentyp|SSIS-Datentyp|
|:-|:-|
|DECIMAL/NUMERIC|DT_NUMERIC|
|BYTEINT|DT_I1|
|SMALLINT|DT_I2|
|INTEGER|DT_I4|
|FLOAT/REAL/DOUBLE PRECISION|DT_R8|
|DATE|DT_DBDATE|
|TIME<br>TIME(n)|DT_STR|
|timestamp<br>TIMESTAMP (n)|DT_STR|
|TIME WITH TIMEZONE<br>TIME(n) WITH TIMEZONE|DT_STR|
|TIMESTAMP WITH TIMEZONE<br>TIMESTAMP(n) WITH TIMEZONE|DT_STR|
|INTERVAL YEAR<br>INTERVAL YEAR (n)|DT_STR|
|INTERVAL YEAR TO MONTH<br>INTERVAL YEAR (n) TO MONTH|DT_STR|
|INTERVAL MONTH<br>INTERVAL MONTH (n)|DT_STR|
|INTERVAL DAY<br>INTERVAL DAY (n)|DT_STR|
|INTERVAL DAY TO HOUR<br>INTERVAL DAY (n) TO HOUR|DT_STR|
|INTERVAL DAY TO MINUTE<br>INTERVAL DAY (n) TO MINUTE|DT_STR|
|INTERVAL DAY TO SECOND<br>INTERVAL DAY (n) TO SECOND<br>INTERVAL DAY TO SECOND (m)<br>INTERVAL DAY (n) TO SECOND (m)|DT_STR|
|INTERVAL HOUR<br>INTERVAL HOUR (n)|DT_STR|
|INTERVAL HOUR TO MINUTE<br>INTERVAL HOUR (n) TO MINUTE|DT_STR
|INTERVAL HOUR TO SECOND<br>INTERVAL HOUR (n) TO SECOND<br>INTERVAL HOUR TO SECOND (m)<br>INTERVAL HOUR (n) TO SECOND (m)|DT_STR|
|INTERVAL MINUTE<br>INTERVAL MINUTE (n)|DT_STR|
|INTERVAL MINUTE TO SECOND<br>INTERVAL MINUTE (n) TO SECOND<br>INTERVAL MINUTE TO SECOND (m)<br>INTERVAL MINUTE (n) TO SECOND (m)|DT_STR|
|INTERVAL SECOND<br>INTERVAL SECOND (n)<br>INTERVAL SECOND (n,m)|DT_STR|
|PERIOD(DATE)|DT_STR|
|PERIOD(TIME)|DT_STR|
|NUMBER|DT_STR|
|CHARACTER|DT_STR|
|VARCHAR|DT_STR (DT_WSTR für Unicode-Zeichensatz)<br>**Hinweise**:<br> Die von VARCHAR unterstützte maximale Länge ist 32.000. <br> Die maximal zulässige Länge von DT_STR ist 8.000 Zeichen und von DT_WSTR 4.000 Zeichen. Die Daten werden abgeschnitten, wenn sie diese Länge überschreiten.|
|LONG VARCHAR|Nicht unterstützt|
|CLOB|Nicht unterstützt|
|BYTE|DT_BYTES<br>**Hinweis**: Die maximal zulässige Länge ist 8.000 Bytes. Die Daten werden abgeschnitten, wenn sie diese Länge überschreiten.|
|VARBYTE|DT_BYTES<br>**Hinweis**: Die maximal zulässige Länge ist 8.000 Bytes. Die Daten werden abgeschnitten, wenn sie diese Länge überschreiten.|
|BLOB|Nicht unterstützt|

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren des [Teradata-Verbindungs-Managers](teradata-connection-manager.md)
- Konfigurieren der [Teradata-Quelle](teradata-source.md)
- Konfigurieren des [Teradata-Ziels](teradata-destination.md)
- Wenn Sie Fragen haben, besuchen Sie die [Tech Community](https://aka.ms/AA6iwdw).
