---
description: Projekteinstellungen (Typzuordnung) (OracleToSQL)
title: Projekteinstellungen (Typzuordnung) (oracleto SQL) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 4bb8466e-2199-4f00-8513-b04e9586723d
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: cbb47d74535af0dea97842bdf46680a4376c2ba7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067732"
---
# <a name="project-settings-type-mapping-oracletosql"></a>Projekteinstellungen (Typzuordnung) (OracleToSQL)
Die Seite Typzuordnung des Dialog Felds **Projekteinstellungen** enthält Einstellungen, die anpassen, wie SSMA Oracle-Datentypen in- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datentypen konvertiert.  
  
Die Seite Typzuordnung ist in den Dialogfeldern **Projekteinstellungen** und **Standard Projekteinstellungen** verfügbar.  
  
-   Wenn Sie Einstellungen für alle zukünftigen SSMA-Projekte angeben möchten, klicken Sie **im Menü Extras** auf **Standard Projekteinstellungen**, wählen Sie den Migrations Projekttyp aus, für den die Einstellungen in der Dropdown Liste **Migrations Ziel Version** angezeigt oder geändert werden müssen, und klicken Sie dann unten im linken Bereich auf **Typzuordnung** .  
  
-   Um Einstellungen für das aktuelle Projekt anzugeben, klicken Sie **im Menü Extras** auf **Projekteinstellungen**, und klicken Sie dann unten im linken Bereich auf **Typzuordnung** .  
  
Verwenden Sie die Registerkarte **Typzuordnung** im primären SSMA-Fenster, um Einstellungen für das aktuelle Objekt oder die Klasse von Objekten anzugeben.  
  
## <a name="options"></a>Tastatur  
In der folgenden Tabelle werden die Registerkarten Optionen für die **Typzuordnung** angezeigt:  
  
**Quellentyp**  
Der zugeordnete Oracle-Datentyp.  
  
**Zieltyp**  
Der Ziel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datentyp für den angegebenen Oracle-Datentyp.  
  
Weitere Informationen finden Sie in den Tabellen im nächsten Abschnitt für die Standard-SSMA für Oracle-Typzuordnungen.  
  
**Add (Hinzufügen)**  
Klicken Sie hier, um der Zuordnungsliste einen Datentyp hinzuzufügen.  
  
**Bearbeiten**  
Klicken Sie hierauf, um den ausgewählten Datentyp in der Liste Zuordnung zu bearbeiten.  
  
**Remove**  
Klicken Sie hierauf, um die ausgewählte Datentyp Zuordnung aus der Liste Zuordnung zu entfernen.  
  
**Standard wiederherstellen**  
Klicken Sie hier, um die Liste Typzuordnung auf die SSMA-Standardwerte zurückzusetzen.  
  
## <a name="default-type-mappings"></a>Standardtyp Zuordnungen  
In SSMA für Oracle können Sie benutzerdefinierte Typzuordnungen für Argumente, Spalten, lokale Variablen und Rückgabewerte festlegen. Die Standard Zuordnung für Argumente und Rückgabe Typen ist nahezu identisch.  
  
### <a name="default-argument-type-and-return-value-type-mapping"></a>Standard Argumenttyp und Rückgabe Werttyp Zuordnung  
Die folgende Tabelle enthält die standardmäßige Datentyp Zuordnung für Argumente und Rückgabewerte.  
  
|Oracle-Datentyp|Standard [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datentyp|  
|--------------------|-------------------------------------------------------------------------|  
|BFILE|varbinary(max)|  
|binary_double|float [53]|  
|binary_float|float [53]|  
|binary_integer|INT|  
|Blob|varbinary(max)|  
|boolean|bit|  
|char|varchar(max)|  
|char varying|varchar(max)|  
|character|varchar(max)|  
|character varying|varchar(max)|  
|CLOB|varchar(max)|  
|date|datetime2 [0]|  
|dec|Dez [38] [0]|  
|Decimal|float [53]|  
|double precision|float [53]|  
|float|float [53]|  
|INT|INT|  
|integer|INT|  
|long|varchar(max)|  
|lange Rohdaten|varbinary(max)|  
|lange Rohdaten [ \* .. 8000]<sup>*</sup>|varbinary [*]|  
|Long RAW [8001. \* ]<sup>*</sup>|varbinary(max)|  
|National Char|nvarchar(max)|  
|nationale char-Variation|nvarchar(max)|  
|Länder Zeichen|nvarchar(max)|  
|unterschiedliches Zeichen<sup>**</sup>|nvarchar(max)|  
|unterschiedliches Zeichen<sup>*</sup>|nvarchar(max)|  
|NCHAR|nvarchar(max)|  
|NCLOB|nvarchar(max)|  
|Zahl|float [53]|  
|NUMERIC|float [53]|  
|nvarchar2|nvarchar(max)|  
|pls_integer|INT|  
|raw|varbinary(max)|  
|real|float [53]|  
|ROWID|UNIQUEIDENTIFIER|  
|signtype|SMALLINT|  
|SMALLINT|SMALLINT|  
|Zeichenfolge|varchar(max)|  
|timestamp|datetime2|  
|Zeitstempel mit lokaler Zeitzone|datetimeoffset|  
|Zeitstempel mit Zeitzone|datetimeoffset|  
|UROWID|UNIQUEIDENTIFIER|  
|varchar|varchar(max)|  
|VARCHAR2|varchar(max)|  
|XmlType|Xml|  
  
<sup>*</sup> Gilt nur für die Rückgabe Werttyp Zuordnung.  
  
<sup>**</sup> Gilt nur für die Argument Typzuordnung.  
  
### <a name="default-column-type-mapping"></a>Standardmäßige Spaltentyp Zuordnung  
Die folgende Tabelle enthält die Standardtyp Zuordnung für-Spalten.  
  
|Oracle-Datentyp|Standard [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datentyp|  
|--------------------|-------------------------------------------------------------------------|  
|BFILE|varbinary(max)|  
|binary_double|float [53]|  
|binary_float|float [53]|  
|Blob|varbinary(max)|  
|char|char|  
|char-Variation [*.. \* ]|varchar [*]|  
|Char [*.. \* ]|Char [*]|  
|character|char|  
|variierendes Zeichen [*.. \* ]|varchar [*]|  
|Zeichen [*.. \* ]|Char [*]|  
|CLOB|varchar(max)|  
|date|datetime2 [0]|  
|dec|Dez [38] [0]|  
|Dez [*.. \* ]|Dez [*] [0]|  
|Dez [*.. \* ] [\*..\*]|Dez [*] [ \* ]|  
|Decimal|Dezimalzahl [38] [0]|  
|Dezimal [*.. \* ]|Dezimalzahl [*] [0]|  
|Dezimal [*.. \* ] [\*..\*]|Dezimalzahl [*] [ \* ]|  
|double precision|float [53]|  
|float|float [53]|  
|float [*. 53]|float [*]|  
|float [54.. *]|float [53]|  
|INT|INT|  
|integer|INT|  
|long|varchar(max)|  
|lange Rohdaten|varbinary(max)|  
|lange Rohdaten [*. 8000]|varbinary [*]|  
|Long RAW [8001.. *]|varbinary(max)|  
|long varchar|varchar(max)|  
|Long [*. 8000]|varchar [*]|  
|Long [8001.. *]|varchar(max)|  
|National Char|NCHAR|  
|National char Variation [*.. \* ]|nvarchar [*]|  
|National Char [*.. \* ]|NCHAR [*]|  
|Länder Zeichen|NCHAR|  
|unterschiedliches Zeichen [*.. \* ]|nvarchar [*]|  
|Länder Zeichen [*.. \* ]|NCHAR [*]|  
|NCHAR|NCHAR|  
|NCHAR [*]|NCHAR [*]|  
|NCLOB|nvarchar(max)|  
|Zahl|float [53]|  
|Zahl [*.. \* ]|numerisch [*]|  
|Zahl [*.. \* ] [\*..\*]|numerisch [*] [ \* ]|  
|NUMERIC|NUMERIC|  
|numerisch [*.. \* ]|numerisch [*]|  
|numerisch [*.. \* ] [\*..\*]|numerisch [*] [ \* ]|  
|NVARCHAR2 [*.. \* ]|nvarchar [*]|  
|RAW [*.. \* ]|varbinary [*]|  
|real|float [53]|  
|ROWID|UNIQUEIDENTIFIER|  
|SMALLINT|SMALLINT|  
|timestamp|datetime2|  
|Zeitstempel mit lokaler Zeitzone|datetimeoffset|  
|Zeitstempel mit lokaler Zeitzone [*. \* ]|DateTimeOffset [*]|  
|Zeitstempel mit Zeitzone|datetimeoffset|  
|Zeitstempel mit Zeitzone [*. \* ]|DateTimeOffset [*]|  
|Zeitstempel [*.. \* ]|datetime2 [*]|  
|UROWID|UNIQUEIDENTIFIER|  
|UROWID [*.. \* ]|UNIQUEIDENTIFIER|  
|varchar [*.. \* ]|varchar [*]|  
|VARCHAR2 [*.. \* ]|varchar [*]|  
|XmlType|Xml|  
  
### <a name="default-local-variable-type-mapping"></a>Standardmäßige Zuordnung von lokalen Variablen Typen  
Die folgende Tabelle enthält die Standardtyp Zuordnung für lokale Variablen.  
  
|Oracle-Datentyp|Standard [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datentyp|  
|--------------------|-------------------------------------------------------------------------|  
|Bfile|varbinary(max)|  
|binary_double|float [53]|  
|binary_float|float [53]|  
|binary_interger|INT|  
|Blob|varbinary(max)|  
|Boolean|bit|  
|Char|char|  
|char-Variation [*. 8000]|varchar [*]|  
|char-Variation [8001.. *]|varchar(max)|  
|Char [*.. 8000]|Char [*]|  
|Char [8001.. *]|varchar(max)|  
|Zeichen|char|  
|variierendes Zeichen [*. 8000]|varchar [*]|  
|variierendes Zeichen [8001.. *]|varchar(max)|  
|Zeichen [*.. 8000]|Char [*]|  
|Zeichen [8001.. *]|varchar(max)|  
|CLOB|varchar(max)|  
|date|datetime2 [0]|  
|dec|Dez [38] [0]|  
|Dez [*.. \* ]|Dez [*] [0]|  
|Dez [*.. \* ] [\*..\*]|Dez [*] [ \* ]|  
|Decimal|Dezimalzahl [38] [0]|  
|Dezimal [*.. \* ]|Dezimalzahl [*] [0]|  
|Dezimal [*.. \* ] [\*..\*]|Dezimalzahl [*] [ \* ]|  
|double precision|float [53]|  
|Float|float [53]|  
|float [*. 53]|float [*]|  
|float [54.. *]|float [53]|  
|Int|INT|  
|Integer|INT|  
|Ganzzahl [*.. \* ]|numerisch [*] [0]|  
|Long|varchar(max)|  
|lange Rohdaten|varbinary(max)|  
|lange Rohdaten [*. 8000]|varbinary [*]|  
|Long RAW [8001.. *]|varbinary(max)|  
|National Char|NCHAR|  
|National char Variation [*.. 4000]|nvarchar [*]|  
|National char Variation [4001.. *]|nvarchar(max)|  
|National Char [*.. 4000]|NCHAR [*]|  
|National Char [4001.. *]|nvarchar(max)|  
|Länder Zeichen|NCHAR|  
|Länder Zeichen [*. 4000]|nvarchar [*]|  
|Länder Zeichen [4001.. *]|nvarchar(max)|  
|unterschiedlicher nationaler Zeichen [*. 4000]|nvarchar [*]|  
|unterschiedliche Länder Zeichen [4001.. *]|nvarchar(max)|  
|Nchar|NCHAR|  
|NCHAR [*.. 4000]|NCHAR [*]|  
|NCHAR [4001.. *]|nvarchar(max)|  
|NCHAR variiert [*. 4000]|nvarchar [*]|  
|NCHAR-Variation [4001.. *]|nvarchar(max)|  
|NCLOB|nvarchar(max)|  
|Number|float [53]|  
|Zahl [*.. \* ]|numerisch [*]|  
|Zahl [*.. \* ] [\*..\*]|numerisch [*] [ \* ]|  
|Numeric|numerisch [38] [0]|  
|numerisch [*.. \* ]|numerisch [*]|  
|numerisch [*.. \* ] [\*..\*]|numerisch [*] [ \* ]|  
|nvarchar2[*.. 4000]|nvarchar [*]|  
|NVARCHAR2 [4001.. *]|nvarchar(max)|  
|pls_integer|INT|  
|RAW [*... 8000]|varbinary [*]|  
|RAW [8001.. *]|varbinary(max)|  
|Real|float [53]|  
|Rowid|UNIQUEIDENTIFIER|  
|Signtype|SMALLINT|  
|Smallint|SMALLINT|  
|Zeichenfolge [*.. 8000]|varchar [*]|  
|Zeichenfolge [8001.. *]|varchar(max)|  
|timestamp|datetime2|  
|Zeitstempel mit lokaler Zeitzone|datetimeoffset|  
|Zeitstempel mit Zeitzone|datetimeoffset|  
|Zeitstempel mit lokaler Zeitzone [*. \* ]|DateTimeOffset [*]|  
|Zeitstempel mit Zeitzone [*. \* ]|DateTimeOffset [*]|  
|Zeitstempel [*.. \* ]|datetime2 [*]|  
|UROWID|UNIQUEIDENTIFIER|  
|UROWID [*.. \* ]|UNIQUEIDENTIFIER|  
|varchar [*.. 8000]|varchar [*]|  
|varchar [8001.. *]|varchar(max)|  
|VARCHAR2 [*.. 8000]|varchar [*]|  
|VARCHAR2 [8001.. *]|varcha (max)|  
|XmlType|Xml|  
  
## <a name="see-also"></a>Weitere Informationen  
[Referenz zur Benutzeroberfläche &#40;oracletosql&#41;](../../ssma/oracle/user-interface-reference-oracletosql.md)  
  
