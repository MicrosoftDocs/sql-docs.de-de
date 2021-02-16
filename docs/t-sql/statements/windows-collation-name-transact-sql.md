---
description: Name der Windows-Sortierung (Transact-SQL)
title: Name der Windows-Sortierung (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- Windows collations [SQL Server]
- names [SQL Server], collations
- collations [SQL Server], Windows collations
- Collation Designator
ms.assetid: acceef84-2c68-46e2-a021-be019b7ab14e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2fa047e5ef018109352972f3ae139c6eff2c1b93
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100349953"
---
# <a name="windows-collation-name-transact-sql"></a>Name der Windows-Sortierung (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Gibt den Namen der Windows-Sortierung in der COLLATE-Klausel in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an. Der Name der Windows-Sortierung besteht aus dem Sortierungskennzeichner und den Vergleichsarten.

![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntax

```syntaxsql
<Windows_collation_name> :: =
<CollationDesignator>_<ComparisonStyle>

<ComparisonStyle> :: =
{ <CaseSensitivity>_<AccentSensitivity> [ _<KanatypeSensitive> ] [ _<WidthSensitive> ] [ _<VariationSelectorSensitive> ] 
}
| { _UTF8 }
| { _BIN | _BIN2 }
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente

*CollationDesignator*   
Gibt die grundlegenden in der Windows-Sortierung verwendeten Sortierungsregeln an. Zu den grundlegenden Sortierungsregeln zählen:

- Die Sortier- und Vergleichsregeln, die angewendet werden, wenn Wörterbuchsortierung angegeben wird. Sortierregeln basieren auf Alphabet oder Sprache.
- Die Codepage, die verwendet wird, um **varchar**-Daten zu speichern.

Beispiele:

- Latin1\_General oder French: Beide verwenden Codepage 1252.
- Turkish: verwendet die Codepage 1254.

*CaseSensitivity*  
**CI** gibt keine Unterscheidung nach Groß-/Kleinschreibung an. Bei **CS** erfolgt eine Unterscheidung.

*AccentSensitivity*  
**AI** gibt keine Unterscheidung nach Akzent an. Bei **AS** erfolgt eine Unterscheidung.

*KanatypeSensitive*  
Bei Weglassen dieser Option erfolgt keine Unterscheidung nach Kanatyp. Bei **KS** erfolgt eine Unterscheidung.

*WidthSensitivity*  
Bei Weglassen dieser Option erfolgt keine Unterscheidung nach Breite. Bei **WS** erfolgt eine Unterscheidung.

*VariationSelectorSensitivity*  
- **Gilt für**: Seit [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] 

- Bei Weglassen dieser Option erfolgt keine Unterscheidung nach Variantenselektor. Bei **VSS** erfolgt eine Unterscheidung.

**UTF8**  
- **Gilt für**: Seit [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]   

- Gibt an, dass für geeignete Datentypen UTF-8-Codierung verwendet werden soll. Weitere Informationen finden Sie unter [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md).

**BIN**  
Gibt die zu verwendende abwärtskompatible binäre Sortierreihenfolge an.

**BIN2**  
Gibt die binäre Sortierreihenfolge an, die die Semantik für den Codepunktvergleich verwendet.

## <a name="remarks"></a>Bemerkungen
Je nach Sortierungsversion sind für manche Codeelemente möglicherweise keine Gewichtungen und/oder Großschreibung/Kleinschreibung-Mappings angegeben. Vergleichen Sie z.B. die Ausgabe der `LOWER`-Funktion bei gleichem Zeichen, aber unterschiedlichen Versionen derselben Sortierung:

```sql
SELECT NCHAR(504) COLLATE Latin1_General_CI_AS AS [Uppercase],
       NCHAR(505) COLLATE Latin1_General_CI_AS AS [Lowercase];
-- Ǹ    ǹ


SELECT LOWER(NCHAR(504) COLLATE Latin1_General_CI_AS) AS [Version80Collation],
       LOWER(NCHAR(504) COLLATE Latin1_General_100_CI_AS) AS [Version100Collation];
-- Ǹ    ǹ
```

Für die erste Anweisung werden in der älteren Sortierung sowohl die groß- als auch die kleingeschriebene Form des Zeichens angezeigt (die Sortierung hat keinen Einfluss auf die Verfügbarkeit von Zeichen, wenn mit Unicode-Daten gearbeitet wird). Für die zweite Anweisung wird jedoch ein großgeschriebenes Zeichen ausgegeben, wenn die Sortierung auf Latin1\_General\_CI\_AS festgelegt wurde, da in dieser Sortierung kein kleingeschriebenes Mapping für dieses Codeelement angegeben wurde.

Bei der Arbeit mit bestimmten Sprachen kann es entscheidend sein, die älteren Sortierungen zu vermeiden. Das gilt beispielsweise für Telegu.

In einigen Fällen können Windows-Sortierungen und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sortierungen unterschiedliche Abfragepläne für dieselbe Abfrage generieren.

## <a name="examples"></a>Beispiele

Im Folgenden finden Sie einige Beispiele für Namen der Windows-Sortierung:

- **Latin1\_General\_100\_CI\_AS**

  Die Sortierung verwendet die Latin1 General-Wörterbuch-Sortierungsregeln und ist der Codepage 1252 zugeordnet. Es handelt sich um eine Sortierungsversion \_100, und es erfolgt keine Unterscheidung nach Groß-/Kleinschreibung (CI), aber eine Unterscheidung nach Akzenten (AS).

- **Estonian\_CS\_AS**

  Die Sortierung verwendet die estnischen Wörterbuchsortierregeln und -mappings, Codepage 1257. Es handelt sich um eine Sortierungsversion \_80 (angezeigt durch Name ohne Versionsnummer), und es erfolgt eine Unterscheidung nach Groß-/Kleinschreibung (CS) und eine Unterscheidung nach Akzenten.

- **Japanese\_Bushu\_Kakusu\_140\_BIN2**

  Die Sortierung verwendet binäre Codeelementsortierregeln und -mappings, Codepage 932. Es handelt sich um eine Sortierungsversion \_140, und die japanischen Bushu- und Kakusu-Wörterbuchsortierregeln werden ignoriert.

## <a name="windows-collations"></a>Windows-Sortierreihenfolgen

Führen Sie die folgende Abfrage aus, um die von Ihrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz unterstützten Windows-Sortierungen aufzulisten.

```sql
SELECT * FROM sys.fn_helpcollations() WHERE [name] NOT LIKE N'SQL%';
```

In der folgenden Tabelle werden alle Windows-Sortierungen aufgelistet, die in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] unterstützt werden.

|Windows-Gebietsschema|Sortierungsversion 100|Sortierungsversion 90|
|--------------------|---------------------------|--------------------------|
|Elsässisch (Frankreich)|Latin1_General_100_|Nicht verfügbar|
|Amharisch (Äthiopien)|Latin1_General_100_|Nicht verfügbar|
|Armenisch (Armenien)|Cyrillic_General_100_|Nicht verfügbar|
|Assamesisch (Indien)|Assamese_100_<sup>1</sup>|Nicht verfügbar|
|Bangla (Bangladesch)|Bengali_100_<sup>1</sup>|Nicht verfügbar|
|Baschkirisch (Russische Föderation)|Bashkir_100_|Nicht verfügbar|
|Baskisch (Baskisch)|Latin1_General_100_|Nicht verfügbar|
|Bangla (Indien)|Bengali_100_<sup>1</sup>|Nicht verfügbar|
|Bosnisch (Bosnien und Herzegowina, kyrillisch)|Bosnian_Cyrillic_100_|Nicht verfügbar|
|Bosnisch (Bosnien und Herzegowina, lateinisch)|Bosnian_Latin_100_|Nicht verfügbar|
|Bretonisch (Frankreich)|Breton_100_|Nicht verfügbar|
|Chinesisch (Macao SAR)|Chinese_Traditional_Pinyin_100_|Nicht verfügbar|
|Chinesisch (Macao SAR)|Chinese_Traditional_Stroke_Order_100_|Nicht verfügbar|
|Chinesisch (Singapur)|Chinese_Simplified_Stroke_Order_100_|Nicht verfügbar|
|Korsisch (Frankreich)|Corsican_100_|Nicht verfügbar|
|Kroatisch (Bosnien und Herzegowina, lateinisch)|Croatian_100_|Nicht verfügbar|
|Dari (Afghanistan)|Dari_100_|Nicht verfügbar|
|Englisch (Indien)|Latin1_General_100_|Nicht verfügbar|
|Englisch (Malaysia)|Latin1_General_100_|Nicht verfügbar|
|Englisch (Singapur)|Latin1_General_100_|Nicht verfügbar|
|Philippinisch (Philippinen)|Latin1_General_100_|Nicht verfügbar|
|Friesisch (Niederlande)|Frisian_100_|Nicht verfügbar|
|Georgisch (Georgien)|Cyrillic_General_100_|Nicht verfügbar|
|Grönländisch (Grönland)|Danish_Greenlandic_100_|Nicht verfügbar|
|Gujarati (Indien)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Hausa (Nigeria, lateinisch)|Latin1_General_100_|Nicht verfügbar|
|Hindi (Indien)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Igbo (Nigeria)|Latin1_General_100_|Nicht verfügbar|
|Inuktitut (Kanada, lateinisch)|Latin1_General_100_|Nicht verfügbar|
|Inuktitut (Syllabics) Kanada|Latin1_General_100_|Nicht verfügbar|
|Irisch (Irland)|Latin1_General_100_|Nicht verfügbar|
|Japanisch (Japan XJIS)|Japanese_XJIS_100_|Japanese_90_, Japanese_|
|Japanisch (Japan)|Japanese_Bushu_Kakusu_100_|Nicht verfügbar|
|Kannada (Indien)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Khmer (Kambodscha)|Khmer_100_<sup>1</sup>|Nicht verfügbar|
|K'iche (Guatemala)|Modern_Spanish_100_|Nicht verfügbar|
|Kinyarwanda (Ruanda)|Latin1_General_100_|Nicht verfügbar|
|Konkani (Indien)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Lao (Volksrepublik Laos)|Lao_100_<sup>1</sup>|Nicht verfügbar|
|Niedersorbisch (Deutschland)|Latin1_General_100_|Nicht verfügbar|
|Luxemburgisch (Luxemburg)|Latin1_General_100_|Nicht verfügbar|
|Malayalam (Indien)|Indic_General_100_<sup>1</sup>|Nicht verfügbar|
|Maltesisch (Malta)|Maltese_100_|Nicht verfügbar|
|Maori (Neuseeland)|Maori_100_|Nicht verfügbar|
|Mapudungun (Chile)|Mapudungan_100_|Nicht verfügbar|
|Marathi (Indien)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Mohawk (Kanada)|Mohawk_100_|Nicht verfügbar|
|Mongolisch (VRC)|Cyrillic_General_100_|Nicht verfügbar|
|Nepalesisch (Nepal)|Nepali_100_<sup>1</sup>|Nicht verfügbar|
|Norwegisch, Bokmål (Norwegen)|Norwegian_100_|Nicht verfügbar|
|Norwegisch (Nynorsk, Norwegen)|Norwegian_100_|Nicht verfügbar|
|Okzitanisch (Frankreich)|French_100_|Nicht verfügbar|
|Odia (Indien)|Indic_General_100_<sup>1</sup>|Nicht verfügbar|
|Paschtu (Afghanistan)|Pashto_100_<sup>1</sup>|Nicht verfügbar|
|Persisch (Iran)|Persian_100_|Nicht verfügbar|
|Punjabi (Indien)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Quechua (Bolivien)|Latin1_General_100_|Nicht verfügbar|
|Quechua (Ecuador)|Latin1_General_100_|Nicht verfügbar|
|Quechua (Peru)|Latin1_General_100_|Nicht verfügbar|
|Rätoromanisch (Schweiz)|Romansh_100_|Nicht verfügbar|
|Inari-Sami (Finnland)|Sami_Sweden_Finland_100_|Nicht verfügbar|
|Lule-Sami (Norwegen)|Sami_Norway_100_|Nicht verfügbar|
|Lule-Sami (Schweden)|Sami_Sweden_Finland_100_|Nicht verfügbar|
|Nord-Sami (Finnland)|Sami_Sweden_Finland_100_|Nicht verfügbar|
|Nord-Sami (Norwegen)|Sami_Norway_100_|Nicht verfügbar|
|Nord-Sami (Schweden)|Sami_Sweden_Finland_100_|Nicht verfügbar|
|Skolt-Sami (Finnland)|Sami_Sweden_Finland_100_|Nicht verfügbar|
|Süd-Sami (Norwegen)|Sami_Norway_100_|Nicht verfügbar|
|Süd-Sami (Schweden)|Sami_Sweden_Finland_100_|Nicht verfügbar|
|Sanskrit (Indien)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Serbisch (Bosnien und Herzegowina, kyrillisch)|Serbian_Cyrillic_100_|Nicht verfügbar|
|Serbisch (Bosnien und Herzegowina, lateinisch)|Serbian_Latin_100_|Nicht verfügbar|
|Serbisch (Serbien, kyrillisch)|Serbian_Cyrillic_100_|Nicht verfügbar|
|Serbisch (Serbien, lateinisch)|Serbian_Latin_100_|Nicht verfügbar|
|Sesotho sa Leboa/Nord-Sotho (Südafrika)|Latin1_General_100_|Nicht verfügbar|
|Setswana/Tswana (Südafrika)|Latin1_General_100_|Nicht verfügbar|
|Singhalesisch (Sri Lanka)|Indic_General_100_<sup>1</sup>|Nicht verfügbar|
|Suaheli (Kenia)|Latin1_General_100_|Nicht verfügbar|
|Syrisch (Syrien)|Syriac_100_<sup>1</sup>|Syriac_90_|
|Tadschikisch (Tadschikistan)|Cyrillic_General_100_|Nicht verfügbar|
|Tamazight (Algerien, lateinisch)|Tamazight_100_|Nicht verfügbar|
|Tamil (Indien)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Telugu (Indien)|Indic_General_100_<sup>1</sup>|Indic_General_90_|
|Tibetisch (VRC)|Tibetan_100_<sup>1</sup>|Nicht verfügbar|
|Turkmenisch (Turkmenistan)|Turkmen_100_|Nicht verfügbar|
|Uighurisch (VRC)|Uighur_100_|Nicht verfügbar|
|Obersorbisch (Deutschland)|Upper_Sorbian_100_|Nicht verfügbar|
|Urdu (Pakistan)|Urdu_100_|Nicht verfügbar|
|Walisisch (Großbritannien)|Welsh_100_|Nicht verfügbar|
|Wolof (Senegal)|French_100_|Nicht verfügbar|
|Xhosa/isiXhosa (Südafrika)|Latin1_General_100_|Nicht verfügbar|
|Sacha (Russische Föderation)|Yakut_100_|Nicht verfügbar|
|Yi (VRC)|Latin1_General_100_|Nicht verfügbar|
|Yoruba (Nigeria)|Latin1_General_100_|Nicht verfügbar|
|Zulu/isiZulu (Südafrika)|Latin1_General_100_|Nicht verfügbar|
|Veraltet, nicht verfügbar auf Serverebene in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] oder höher|Hindi|Hindi|
|Veraltet, nicht verfügbar auf Serverebene in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] oder höher|Korean_Wansung_Unicode|Korean_Wansung_Unicode|
|Veraltet, nicht verfügbar auf Serverebene in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] oder höher|Lithuanian_Classic|Lithuanian_Classic|
|Veraltet, nicht verfügbar auf Serverebene in [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] oder höher|Mazedonisch|Mazedonisch|
||||

<sup>1</sup> Nur-Unicode-Sortierungen unterstützen in Windows nur Daten auf Spalten- oder Ausdrucksebene. Sie können nicht für Sortierungen auf Server- oder Datenbankebene verwendet werden.

<sup>2</sup> Wie bei der chinesischen Sortierung (Taiwan) werden bei Chinesisch (Macau) die Regeln für Chinesisch (vereinfacht) verwendet. Im Unterschied zu Chinesisch (Taiwan) wird jedoch Codepage 950 verwendet.

## <a name="see-also"></a>Siehe auch

- [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md)
- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [Konstanten](../../t-sql/data-types/constants-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
- [Tabelle](../../t-sql/data-types/table-transact-sql.md)
- [sys.fn_helpcollations](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)
