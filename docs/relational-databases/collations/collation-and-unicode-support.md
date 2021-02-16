---
description: Unterstützung von Sortierungen und Unicode
title: Unterstützung von Sortierungen und Unicode | Microsoft-Dokumentation
ms.custom: ''
ms.date: 12/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- binary collations [SQL Server]
- expression-level collations [SQL Server]
- Windows collations [SQL Server]
- globalization [SQL Server], about globalization
- supplementary character support
- code pages [SQL Server], about code pages
- collations [SQL Server], about collations
- Unicode [SQL Server], collations
- database-level collations [SQL Server]
- reading order
- column-level collations
- locales [SQL Server], about locales
- locales [SQL Server]
- code pages [SQL Server]
- SQL Server collations
- UTF-8
- UTF-16
- UTF8
- UTF16
- UCS2
- server-level collations [SQL Server]
ms.assetid: 92d34f48-fa2b-47c5-89d3-a4c39b0f39eb
author: pmasl
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 43b6fe61e9a7d1344f1f57ceb725a881d685daff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339569"
---
# <a name="collation-and-unicode-support"></a>Unterstützung von Sortierungen und Unicode
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Sortierungen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bieten Sortierregeln und die Berücksichtigung von Groß-/Kleinschreibung und Akzenten für die Daten. Sortierungen, die mit Zeichendatentypen wie **char** und **varchar** verwendet werden, geben die Codeseite und die entsprechenden Zeichen vor, die für den jeweiligen Datentyp dargestellt werden können. 

Bei der Installation einer neuen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], bei der Wiederherstellung einer Datenbanksicherung oder bei der Verbindung von Servern mit Clientdatenbanken ist es wichtig, dass Sie die Gebietsschemaanforderungen, die Sortierreihenfolge und das Verhalten in Bezug auf die Groß-/Kleinschreibung und Akzente der Daten kennen, mit denen Sie arbeiten. Informationen zum Auflisten der Sortierungen, die in Ihrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz verfügbar sind, finden Sie unter [sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md).    
    
Wenn Sie eine Sortierung für Ihren Server, Ihre Datenbank, Ihre Spalte oder Ihren Ausdruck auswählen, weisen Sie Ihren Daten bestimmte Merkmale zu. Diese Merkmale wirken sich auf die Ergebnisse vieler Vorgänge in der Datenbank aus. Wenn Sie beispielsweise eine Abfrage mit `ORDER BY` erstellen, kann die Sortierreihenfolge des Resultsets von der Sortierung abhängen, die auf die Datenbank angewendet wurde oder die auf Ausdrucksebene in einer `COLLATE`-Klausel der Abfrage vorgegeben ist.    
    
Für die optimale Nutzung der Sortierungsunterstützung in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sollten Sie die in diesem Artikel definierten Begriffe und ihre Verbindung mit den Eigenschaften Ihrer Daten verstehen.    
    
##  <a name="collation-terms"></a><a name="Terms"></a> Sortierungsbegriffe    
    
-   [Sortierung](#Collation_Defn) 
    - [Sortierungssätze](#Collation_sets)
    - [Sortierungsebenen](#Collation_levels)
-   [Gebietsschema](#Locale_Defn)    
-   [Codepage](#Code_Page_Defn)    
-   [Sortierreihenfolge](#Sort_Order_Defn)    
    
###  <a name="collation"></a><a name="Collation_Defn"></a> Sortierung    
Eine Sortierung gibt die Bitmuster an, die die jeweiligen Zeichen in einem Dataset darstellen. Sortierungen legen außerdem die Regeln fest, nach denen Daten sortiert und verglichen werden. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt das Speichern von Objekten mit verschiedenen Sortierungen in einer Datenbank. Bei Nicht-Unicode-Spalten gibt die Sortierungseinstellung die Codepage für die Daten und die Zeichen an, die dargestellt werden können. Die Daten, die Sie zwischen Nicht-Unicode-Spalten verschieben, müssen von der Quellcodepage in die Zielcodepage konvertiert werden.    
    
[!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungsergebnisse können unterschiedlich sein, wenn die Anweisung im Kontext verschiedener Datenbanken ausgeführt wird, die jeweils andere Sortierungseinstellungen haben. Wenn möglich, sollte in Unternehmen eine standardisierte Sortierung verwendet werden. Auf diese Weise müssen Sie die Sortierung nicht für jedes Zeichen oder jeden Unicode-Ausdruck angeben. Bei Objekten mit abweichenden Sortierungs- oder Codepageeinstellungen codieren Sie Ihre Abfragen so, dass diese den Regeln der Sortierungspriorität entsprechen. Weitere Informationen finden Sie unter [Rangfolge der Sortierungen (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md).    
    
Die einer Sortierung zugeordneten Optionen sind die Berücksichtigung von Groß-/Kleinschreibung, Akzenten, Kana, Breite und Variierungsauswahlzeichen. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] führt eine zusätzliche Option für die [UTF-8](https://www.wikipedia.org/wiki/UTF-8)-Codierung ein. 

Sie können diese Optionen festlegen, indem Sie sie an den Namen der Sortierung anfügen. Beispiel: Bei der Sortierung **Japanese_Bushu_Kakusu_100_CS_AS_KS_WS_UTF8** wird nach Groß-/Kleinschreibung, Akzenten, Kana und Breite unterschieden und die UTF-8-Codierung verwendet. Im Gegensatz dazu berücksichtigt die Sortierung **Japanese_Bushu_Kakusu_140_CI_AI_KS_WS_VSS** beispielsweise die Groß-/Kleinschreibung und Akzente nicht, dafür aber Kana, Breite und Variierungsauswahlzeichen, und sie verwendet eine Nicht-Unicode-Codierung. 

In der folgenden Tabelle wird das den verschiedenen Optionen zugeordnete Verhalten beschrieben:    
    
|Option|BESCHREIBUNG|    
|------------|-----------------|    
|Unterscheidung nach Groß-/Kleinschreibung (\_CS)|Unterscheidet zwischen Groß- und Kleinbuchstaben. Wenn diese Option aktiviert wird, stehen Kleinbuchstaben in der Sortierreihenfolge vor ihren entsprechenden Großbuchstaben. Wenn diese Option nicht aktiviert wird, wird die Groß- und Kleinschreibung bei der Sortierung nicht berücksichtigt. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] betrachtet also beim Sortieren die groß- und die kleingeschriebenen Versionen von Buchstaben als identisch. Sie können die Nichtunterscheidung nach Groß-/Kleinbuchstaben durch Angeben von „\_CI“ explizit auswählen.|   
|Unterscheidung nach Akzent (\_AS)|Unterscheidet zwischen Zeichen mit Akzent und Zeichen ohne Akzent. Beispielsweise entspricht „a“ nicht „ấ“. Wenn diese Option nicht aktiviert wird, wird bei der Sortierung nicht nach Akzenten unterschieden. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] betrachtet also beim Sortieren die Versionen von Buchstaben mit und ohne Akzent als identisch. Sie können die Nichtunterscheidung nach Akzent durch Angeben von „\_AI“ explizit auswählen.|    
|Unterscheidung nach Kana (\_KS)|Unterscheidet zwischen zwei japanischen Kana-Zeichen: Hiragana und Katakana. Wenn diese Option nicht aktiviert wird, wird bei der Sortierung nicht nach Kana unterschieden. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] betrachtet also beim Sortieren Hiragana- und Katakana-Zeichen als identisch. Das Auslassen dieser Option ist die einzige Möglichkeit, die Nichtbeachtung von Kana anzugeben.|   
|Unterscheidung nach Breite (\_WS)|Unterscheidet zwischen Zeichen halber Breite und Zeichen normaler Breite. Wenn diese Option nicht aktiviert wird, betrachtet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] beim Sortieren die Darstellung in halber Breite und in normaler Breite desselben Zeichens als identisch. Das Weglassen dieser Option ist die einzige Möglichkeit, die Nichtbeachtung der Breite anzugeben.|  
|Mit Unterscheidung nach Variierungsauswahlzeichen (\_VSS)|Unterscheidet zwischen verschiedenen Variierungsauswahlzeichen für Ideogramme in den japanischen Sortierungen **Japanese_Bushu_Kakusu_140** und **Japanese_XJIS_140**, die erstmals in [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] eingeführt wurden. Eine Variierungssequenz besteht aus einem Basiszeichen plus einem ergänzenden Variierungsauswahlzeichen. Wenn diese \_VSS-Option nicht aktiviert ist, berücksichtigt die Sortierung die Variierung nicht, und das Variierungsauswahlzeichen wird im Vergleich nicht berücksichtigt. Das bedeutet, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Zeichen, die auf dem gleichen Basiszeichen aufbauen, aber verschiedene Variierungsauswahlzeichen aufweisen, für Sortierungszwecke als identisch betrachtet werden. Weitere Informationen finden Sie in der [Unicode Ideographic Variation Database](https://www.unicode.org/reports/tr37/) (Unicode-Datenbank für Ideogrammvariationen).<br/><br/> Variierungsauswahlzeichen unterstützende Sortierungen (\_VSS) werden in Indizes für die Volltextsuche nicht unterstützt. Indizes für die Volltextsuche unterstützen nur Optionen für die Unterscheidung nach Akzent (\_AS), Kana (\_KS) und Breite (\_WS). XML- und CLR-Engines von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützen Variierungsauswahlzeichen (\_VSS) nicht.|      
|Binär (\_BIN)<sup>1</sup>|Sortiert und vergleicht Daten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Tabellen basierend auf den für jedes Zeichen definierten Bitmustern. Die binäre Sortierreihenfolge unterscheidet nach Groß- und Kleinschreibung und nach Akzent. Die Option Binär ist zudem die schnellste Sortierreihenfolge. Weitere Informationen finden Sie im Abschnitt [Binäre Sortierungen](#Binary-collations) dieses Artikels.|      
|Binärcodepunkt (\_BIN2)<sup>1</sup> | Sortiert und vergleicht Daten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Tabellen basierend auf Unicode-Codepunkten für Unicode-Daten. Für Nicht-Unicode-Daten verwendet der Binärcodepunkt Vergleiche, die mit denen für binäre Sortierungen identisch sind.<br/><br/> Der Vorteil beim Verwenden einer Binär-Codepunkt-Sortierreihenfolge liegt darin, dass in Anwendungen, die sortierte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Daten vergleichen, eine Neusortierung der Daten nicht erforderlich ist. Folglich ermöglicht eine Binär-Codepunkt-Sortierreihenfolge eine einfachere Entwicklung von Anwendungen und eine Steigerung der Leistung. Weitere Informationen finden Sie im Abschnitt [Binäre Sortierungen](#Binary-collations) dieses Artikels.|
|UTF-8 (\_UTF8)|Ermöglicht das Speichern von mit UTF-8 codierten Daten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Wenn diese Option nicht aktiviert ist, verwendet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das standardmäßige Nicht-Unicode-Codierungsformat für die entsprechenden Datentypen. Weitere Informationen finden Sie im Abschnitt [Unterstützung für UTF-8](#utf8) dieses Artikels.| 

<sup>1</sup> Wenn „Binär“ oder der Binär-Codepunkt ausgewählt ist, sind die Optionen Unterscheidung nach Groß-/Kleinschreibung (\_CS), Unterscheidung nach Akzent (\_AS), Unterscheidung nach Kana (\_KS) und Unterscheidung nach Breite (\_WS) nicht verfügbar.      

#### <a name="examples-of-collation-options"></a>Beispiele für Sortierungsoptionen
Jede Sortierung setzt sich aus einer Kombination von Suffixen zur Festlegung der Unterscheidung nach Groß-/Kleinschreibung, Akzenten, Breite oder Kana zusammen. Die folgenden Beispiele beschreiben das Verhalten der Sortierreihenfolge bei verschiedenen Suffixkombinationen.

|Suffix der Windows-Sortierung|Beschreibung der Sortierreihenfolge|
|------------|-----------------| 
|\_BIN<sup>1</sup>|Binäre Sortierung.|
|\_BIN2<sup>1, 2</sup>|Binärcodepunkt-Sortierreihenfolge.|
|\_CI_AI<sup>2</sup>|Keine Unterscheidung nach Groß-/Kleinschreibung, Akzenten, Kana und Breite.|
|\_CI_AI_KS<sup>2</sup>|Keine Unterscheidung nach Groß-/Kleinschreibung, Akzent und Breite. Unterscheidung nach Kana.|
|\_CI_AI_KS_WS<sup>2</sup>|Keine Unterscheidung nach Groß-/Kleinschreibung und Akzent. Unterscheidung nach Kana und Breite|
|\_CI_AI_WS<sup>2</sup>|Keine Unterscheidung nach Groß-/Kleinschreibung, Akzent und Kana. Unterscheidung nach Breite.|
|\_CI_AS<sup>2</sup>|Keine Unterscheidung nach Groß-/Kleinschreibung, Kana und Breite. Unterscheidung nach Akzent.|
|\_CI_AS_KS<sup>2</sup>|Keine Unterscheidung nach Groß-/Kleinschreibung und Breite. Unterscheidung nach Akzent und Kana.|
|\_CI_AS_KS_WS<sup>2</sup>|Keine Unterscheidung nach Groß-/Kleinschreibung. Unterscheidung nach Akzent, Kana und Breite.|
|\_CI_AS_WS<sup>2</sup>|Keine Unterscheidung nach Groß-/Kleinschreibung und Kana. Unterscheidung nach Akzent und Breite.|
|\_CS_AI<sup>2</sup>|Keine Unterscheidung nach Akzent, Kana und Breite. Unterscheidung nach Groß-/Kleinschreibung.|
|\_CS_AI_KS<sup>2</sup>|Keine Unterscheidung nach Akzent und Breite. Unterscheidung nach Groß-/Kleinschreibung und Kana.|
|\_CS_AI_KS_WS<sup>2</sup>|Keine Unterscheidung nach Akzent. Unterscheidung nach Groß-/Kleinschreibung, Kana und Breite.|
|\_CS_AI_WS<sup>2</sup>|Keine Unterscheidung nach Akzent und Kana. Unterscheidung nach Groß-/Kleinschreibung und Breite.|
|\_CS_AS<sup>2</sup>|Keine Unterscheidung nach Kana und Breite. Unterscheidung nach Groß-/Kleinschreibung und Akzent.|
|\_CS_AS_KS<sup>2</sup>|Keine Unterscheidung nach Breite. Unterscheidung nach Groß-/Kleinschreibung, Akzent und Kana.|
|\_CS_AS_KS_WS<sup>2</sup>|Unterscheidung nach Groß-/Kleinschreibung, Akzent, Kana und Breite.|
|\_CS_AS_WS<sup>2</sup>|Keine Unterscheidung nach Kana. Unterscheidung nach Groß-/Kleinschreibung, Akzent und Breite.|

<sup>1</sup> Wenn „Binär“ oder „Binärcodepunkt“ aktiviert wird, sind die Optionen „Unterscheidung nach Groß-/Kleinschreibung“ (\_CS), „Unterscheidung nach Akzenten“ (\_AS), „Unterscheidung nach Kana“ (\_KS) und „Unterscheidung nach Breite“ (\_WS) nicht verfügbar.    

<sup>2</sup> Durch Hinzufügen der UTF-8-Option (\_UTF8) können Sie Unicode-Daten mit UTF-8 codieren. Weitere Informationen finden Sie im Abschnitt [Unterstützung für UTF-8](#utf8) dieses Artikels. 

### <a name="collation-sets"></a><a name="Collation_sets"></a> Sortierungssätze

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt die folgenden Sortierungssätze:    

-  [Windows-Sortierreihenfolgen](#Windows-collations)
-  [Binäre Sortierungen](#Binary-collations)
-  [SQL Server-Sortierungen](#SQL-collations)
    
#### <a name="windows-collations"></a><a name="Windows-collations"></a> Windows-Sortierreihenfolgen    
Windows-Sortierungen definieren Regeln zum Speichern von Zeichendaten, die auf dem zugehörigen Windows-Systemgebietsschema basieren. Bei einer Windows-Sortierung können Sie mithilfe desselben Algorithmus wie für Unicode-Daten einen Vergleich von Nicht-Unicode-Daten implementieren. Die grundlegenden Regeln für die Windows-Sortierung geben vor, welches Alphabet oder welche Sprache verwendet wird, wenn Wörterbuchsortierung angewendet wird. Die Regeln geben auch die Codepage vor, die zum Speichern von Nicht-Unicode-Zeichendaten verwendet wird. Sowohl die Unicode- als auch die Nicht-Unicode-Sortierung sind kompatibel mit Zeichenfolgenvergleichen in einer bestimmten Version von Windows. Dadurch wird die Konsistenz der Datentypen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ermöglicht, und Entwickler erhalten so die Möglichkeit, Zeichenfolgen in ihren Anwendungen mithilfe der gleichen Regeln zu sortieren, die auch in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet werden. Weitere Informationen finden Sie unter [Name der Windows-Sortierung (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md).    
    
#### <a name="binary-collations"></a><a name="Binary-collations"></a> Binäre Sortierungen    
Binäre Sortierungen sortieren Daten basierend auf der Reihenfolge codierter Werte, die vom Gebietsschema und Datentyp definiert werden. Sie beachten die Groß-/Kleinschreibung. Eine binäre Sortierung in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definiert das zu verwendende Gebietsschema sowie die zu verwendende ANSI-Codepage. Dies erzwingt eine binäre Sortierreihenfolge. Da binäre Sortierungen relativ einfach sind, tragen sie dazu bei, die Anwendungsleistung zu verbessern. Bei Nicht-Unicode-Datentypen basieren Datenvergleiche auf den in der ANSI-Codepage definierten Codepunkten. Bei Unicode-Datentypen basieren Datenvergleiche auf den Unicode-Codepunkten. Bei binären Sortierungen von Unicode-Datentypen wird das Gebietsschema bei Datensortierungen nicht berücksichtigt. Beispielsweise führen **Latin_1_General_BIN** und **Japanese_BIN** bei Unicode-Daten zu den gleichen Sortierergebnissen. Weitere Informationen finden Sie unter [Name der Windows-Sortierung (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md).   
    
In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]gibt es zwei Arten von binären Sortierungen:

-  Frühere **BIN**-Sortierungen, die für Unicode-Daten einen unvollständigen Codepunkt-zu-Codepunkt-Vergleich ausführten. Die früheren binären Sortierungen verglichen die ersten Zeichen als WCHAR und die folgenden byteweise. In einer BIN-Sortierung wird nur der erste Buchstabe nach dem Codepunkt sortiert, die verbleibenden Zeichen werden entsprechend ihren Bytewerten sortiert.

-  Die neueren **BIN2**-Sortierungen, die einen reinen Vergleich nach Codepunkt implementieren. In einer BIN2-Sortierung werden alle Zeichen entsprechend ihren Codepunkten sortiert. Da die Intel Plattform eine Little-Endian-Architektur aufweist, werden Unicode-Zeichen immer mit vertauschten Bytes gespeichert.     
    
#### <a name="sql-server-collations"></a><a name="SQL-collations"></a> SQL Server-Sortierungen    
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sortierungen (SQL_\*) sind hinsichtlich der Sortierreihenfolge mit älteren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kompatibel. Die Wörterbuch-Sortierungsregeln für Nicht-Unicode-Daten sind nicht kompatibel mit den von Windows-Betriebssystem bereitgestellten Sortierroutinen. Das Sortieren von Unicode-Daten ist jedoch kompatibel mit einer bestimmten Version der Windows-Sortierregeln. Da [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sortierungen unterschiedliche Vergleichsregeln für Nicht-Unicode- und Unicode-Daten verwenden, werden bei Vergleichen derselben Daten, je nach dem zugrunde liegenden Datentyp, unterschiedliche Ergebnisse angezeigt. Weitere Informationen finden Sie unter [Name der SQL Server-Sortierung (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md). 

Die Standardinstallationseinstellung wird während des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Setups durch das Gebietsschema des Betriebssystems bestimmt. Sie können die Sortierung auf Serverebene entweder während des Setups ändern oder, indem Sie das Gebietsschema des Betriebssystems vor der Installation ändern. Aus Gründen der Abwärtskompatibilität ist die Standardsortierung auf die älteste verfügbare Version festgelegt, die einem jeweiligen Gebietsschema zugeordnet ist. Aus diesem Grund wird diese Sortierung nicht immer empfohlen. Um die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Funktionen optimal zu können, ändern Sie die Standardinstallationseinstellungen für die Verwendung von Windows-Sortierungen. Beispielsweise ist für das Betriebssystem-Gebietsschema „Englisch (USA)“ (Codepage 1252) die Standardsortierung während des Setups **SQL_Latin1_General_CP1_CI_AS**, welche in die nächste entsprechende Windows-Sortierung **Latin1_General_100_CI_AS_SC** geändert werden kann.
    
> [!NOTE]    
> Wenn Sie ein Upgrade für eine englischsprachige Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] durchführen, können Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sortierungen (SQL_\*) festlegen, die mit vorhandenen Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kompatibel sind. Da die Standardsortierung einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] während des Setups festgelegt wird, geben Sie unbedingt die Sortierungseinstellungen sorgfältig an, wenn folgende Bedingungen vorliegen:    
>     
> -   Der Anwendungscode ist abhängig vom Verhalten der vorherigen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sortierungen.    
> -   Sie müssen Zeichendaten speichern, die mehrere Sprachen wiedergeben.    
    
### <a name="collation-levels"></a><a name="Collation_levels"></a> Sortierungsebenen
Das Festlegen von Sortierungen wird bei einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]für die folgenden Ebenen unterstützt:    

-  [Sortierungen auf Serverebene](#Server-level-collations)
-  [Sortierungen auf Datenbankebene](#Database-level-collations)
-  [Sortierungen auf Spaltenebene](#Column-level-collations)
-  [Sortierungen auf Ausdrucksebene](#Expression-level-collations)

#### <a name="server-level-collations"></a><a name="Server-level-collations"></a> Sortierungen auf Serverebene   
Die Standardserversortierung wird während des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Setups festgelegt und als Standardsortierung der Systemdatenbanken und aller Benutzerdatenbanken vorgegeben. 

In der folgenden Tabelle werden die standardmäßig verwendeten Sortierbezeichnungen angezeigt, die vom Gebietsschema des Betriebssystems bestimmt werden, einschließlich der entsprechenden Windows- und SQL-Sprachcode-IDs (LCID):

|Windows-Gebietsschema|Windows-LCID|SQL-LCID|Standardsortierung|
|---------------|---------|---------|---------------|
|Afrikaans (Südafrika)|0x0436|0x0409|Latin1_General_CI_AS|
|Albanisch (Albanien)|0x041c|0x041c|Albanian_CI_AS|
|Elsässisch (Frankreich)|0x0484|0x0409|Latin1_General_CI_AS|
|Amharisch (Äthiopien)|0x045e|0x0409|Latin1_General_CI_AS|
|Arabisch (Algerien)|0x1401|0x0401|Arabic_CI_AS|
|Arabisch (Bahrain)|0x3c01|0x0401|Arabic_CI_AS|
|Arabisch (Ägypten)|0x0c01|0x0401|Arabic_CI_AS|
|Arabisch (Irak)|0x0801|0x0401|Arabic_CI_AS|
|Arabisch (Jordanien)|0x2c01|0x0401|Arabic_CI_AS|
|Arabisch (Kuwait)|0x3401|0x0401|Arabic_CI_AS|
|Arabisch (Libanon)|0x3001|0x0401|Arabic_CI_AS|
|Arabisch (Libyen)|0x1001|0x0401|Arabic_CI_AS|
|Arabisch (Marokko)|0x1801|0x0401|Arabic_CI_AS|
|Arabisch (Oman)|0x2001|0x0401|Arabic_CI_AS|
|Arabisch (Katar)|0x4001|0x0401|Arabic_CI_AS|
|Arabisch (Saudi-Arabien)|0x0401|0x0401|Arabic_CI_AS|
|Arabisch (Syrien)|0x2801|0x0401|Arabic_CI_AS|
|Arabisch (Tunesien)|0x1c01|0x0401|Arabic_CI_AS|
|Arabisch (Vereinigte Arabische Emirate)|0x3801|0x0401|Arabic_CI_AS|
|Arabisch (Jemen)|0x2401|0x0401|Arabic_CI_AS|
|Armenisch (Armenien)|0x042b|0x0419|Latin1_General_CI_AS|
|Assamisch (Indien)|0x044d|0x044d|Auf Serverebene nicht verfügbar|
|Aserbaidschanisch (Aserbaidschan, kyrillisch)|0x082c|0x082c|Veraltet, auf Serverebene nicht verfügbar|
|Aserbaidschanisch (Aserbaidschan, lateinisch)|0x042c|0x042c|Veraltet, auf Serverebene nicht verfügbar|
|Baschkirisch (Russische Föderation)|0x046d|0x046d|Latin1_General_CI_AI|
|Baskisch (Baskisch)|0x042d|0x0409|Latin1_General_CI_AS|
|Belarussisch (Belarus)|0x0423|0x0419|Cyrillic_General_CI_AS|
|Bangla (Bangladesch)|0x0845|0x0445|Auf Serverebene nicht verfügbar|
|Bangla (Indien)|0x0445|0x0439|Auf Serverebene nicht verfügbar|
|Bosnisch (Bosnien und Herzegowina, kyrillisch)|0x201a|0x201a|Latin1_General_CI_AI|
|Bosnisch (Bosnien und Herzegowina, lateinisch)|0x141a|0x141a|Latin1_General_CI_AI|
|Bretonisch (Frankreich)|0x047e|0x047e|Latin1_General_CI_AI|
|Bulgarisch (Bulgarien)|0x0402|0x0419|Cyrillic_General_CI_AS|
|Katalanisch (Katalanisch)|0x0403|0x0409|Latin1_General_CI_AS|
|Chinesisch (Hongkong SAR, VR China)|0x0c04|0x0404|Chinese_Taiwan_Stroke_CI_AS|
|Chinesisch (Macao SAR)|0x1404|0x1404|Latin1_General_CI_AI|
|Chinesisch (Macau)|0x21404|0x21404|Latin1_General_CI_AI|
|Chinesisch (VRC)|0x0804|0x0804|Chinese_PRC_CI_AS|
|Chinesisch (VRC)|0x20804|0x20804|Chinese_PRC_Stroke_CI_AS|
|Chinesisch (Singapur)|0x1004|0x0804|Chinese_PRC_CI_AS|
|Chinesisch (Singapur)|0x21004|0x20804|Chinese_PRC_Stroke_CI_AS|
|Chinesisch (Taiwan)|0x30404|0x30404|Chinese_Taiwan_Bopomofo_CI_AS|
|Chinesisch (Taiwan)|0x0404|0x0404|Chinese_Taiwan_Stroke_CI_AS|
|Korsisch (Frankreich)|0x0483|0x0483|Latin1_General_CI_AI|
|Kroatisch (Bosnien und Herzegowina, lateinisch)|0x101a|0x041a|Croatian_CI_AS|
|Kroatisch (Kroatien)|0x041a|0x041a|Croatian_CI_AS|
|Tschechisch (Tschechische Republik)|0x0405|0x0405|Czech_CI_AS|
|Dänisch (Dänemark)|0x0406|0x0406|Danish_Norwegian_CI_AS|
|Dari (Afghanistan)|0x048c|0x048c|Latin1_General_CI_AI|
|Divehi (Malediven)|0x0465|0x0465|Auf Serverebene nicht verfügbar|
|Niederländisch (Belgien)|0x0813|0x0409|Latin1_General_CI_AS|
|Niederländisch (Niederlande)|0x0413|0x0409|Latin1_General_CI_AS|
|Englisch (Australien)|0x0c09|0x0409|Latin1_General_CI_AS|
|Englisch (Belize)|0x2809|0x0409|Latin1_General_CI_AS|
|Englisch (Kanada)|0x1009|0x0409|Latin1_General_CI_AS|
|Englisch (Karibik)|0x2409|0x0409|Latin1_General_CI_AS|
|Englisch (Indien)|0x4009|0x0409|Latin1_General_CI_AS|
|Englisch (Irland)|0x1809|0x0409|Latin1_General_CI_AS|
|Englisch (Jamaika)|0x2009|0x0409|Latin1_General_CI_AS|
|Englisch (Malaysia)|0x4409|0x0409|Latin1_General_CI_AS|
|Englisch (Neuseeland)|0x1409|0x0409|Latin1_General_CI_AS|
|Englisch (Philippinen)|0x3409|0x0409|Latin1_General_CI_AS|
|Englisch (Singapur)|0x4809|0x0409|Latin1_General_CI_AS|
|Englisch (Südafrika)|0x1c09|0x0409|Latin1_General_CI_AS|
|Englisch (Trinidad und Tobago)|0x2c09|0x0409|Latin1_General_CI_AS|
|Walisisch (Großbritannien)|0x0809|0x0409|Latin1_General_CI_AS|
|Englisch (USA)|0x0409|0x0409|SQL_Latin1_General_CP1_CI_AS|
|Englisch (Zimbabwe)|0x3009|0x0409|Latin1_General_CI_AS|
|Estnisch (Estland)|0x0425|0x0425|Estonian_CI_AS|
|Färöisch (Färöer-Inseln)|0x0438|0x0409|Latin1_General_CI_AS|
|Philippinisch (Philippinen)|0x0464|0x0409|Latin1_General_CI_AS|
|Finnisch (Finnland)|0x040b|0x040b|Finnish_Swedish_CI_AS|
|Französisch (Belgien)|0x080c|0x040c|French_CI_AS|
|Französisch (Kanada)|0x0c0c|0x040c|French_CI_AS|
|Französisch (Frankreich)|0x040c|0x040c|French_CI_AS|
|Französisch (Luxemburg)|0x140c|0x040c|French_CI_AS|
|Französisch (Monaco)|0x180c|0x040c|French_CI_AS|
|Französisch (Schweiz)|0x100c|0x040c|French_CI_AS|
|Friesisch (Niederlande)|0x0462|0x0462|Latin1_General_CI_AI|
|Galizisch|0x0456|0x0409|Latin1_General_CI_AS|
|Georgisch (Georgien)|0x10437|0x10437|Georgian_Modern_Sort_CI_AS|
|Georgisch (Georgien)|0x0437|0x0419|Latin1_General_CI_AS|
|Deutsch (Telefonbuchsortierung DIN)|0x10407|0x10407|German_PhoneBook_CI_AS|
|Deutsch (Österreich)|0x0c07|0x0409|Latin1_General_CI_AS|
|Deutsch (Deutschland)|0x0407|0x0409|Latin1_General_CI_AS|
|Deutsch (Liechtenstein)|0x1407|0x0409|Latin1_General_CI_AS|
|Deutsch (Luxemburg)|0x1007|0x0409|Latin1_General_CI_AS|
|Deutsch (Schweiz)|0x0807|0x0409|Latin1_General_CI_AS|
|Griechisch (Griechenland)|0x0408|0x0408|Greek_CI_AS|
|Grönländisch (Grönland)|0x046f|0x0406|Danish_Norwegian_CI_AS|
|Gujarati (Indien)|0x0447|0x0439|Auf Serverebene nicht verfügbar|
|Hausa (Nigeria, lateinisch)|0x0468|0x0409|Latin1_General_CI_AS|
|Hebräisch (Israel)|0x040d|0x040d|Hebrew_CI_AS|
|Hindi (Indien)|0x0439|0x0439|Auf Serverebene nicht verfügbar|
|Ungarisch (Ungarn)|0x040e|0x040e|Hungarian_CI_AS|
|Ungarisch (Technische Sortierung)|0x1040e|0x1040e|Hungarian_Technical_CI_AS|
|Isländisch (Island)|0x040f|0x040f|Icelandic_CI_AS|
|Igbo (Nigeria)|0x0470|0x0409|Latin1_General_CI_AS|
|Indonesisch (Indonesien)|0x0421|0x0409|Latin1_General_CI_AS|
|Inuktitut (Kanada, lateinisch)|0x085d|0x0409|Latin1_General_CI_AS|
|Inuktitut (Syllabics) Kanada|0x045d|0x045d|Latin1_General_CI_AI|
|Irisch (Irland)|0x083c|0x0409|Latin1_General_CI_AS|
|Italienisch (Italien)|0x0410|0x0409|Latin1_General_CI_AS|
|Italienisch (Schweiz)|0x0810|0x0409|Latin1_General_CI_AS|
|Japanisch (Japan XJIS)|0x0411|0x0411|Japanese_CI_AS|
|Japanisch (Japan)|0x040411|0x40411|Latin1_General_CI_AI|
|Kannada (Indien)|0x044b|0x0439|Auf Serverebene nicht verfügbar|
|Kasachisch (Kasachstan)|0x043f|0x043f|Kazakh_90_CI_AS|
|Khmer (Kambodscha)|0x0453|0x0453|Auf Serverebene nicht verfügbar|
|K'iche (Guatemala)|0x0486|0x0c0a|Modern_Spanish_CI_AS|
|Kinyarwanda (Ruanda)|0x0487|0x0409|Latin1_General_CI_AS|
|Konkani (Indien)|0x0457|0x0439|Auf Serverebene nicht verfügbar|
|Koreanisch (koreanische Wörterbuchsortierung)|0x0412|0x0412|Korean_Wansung_CI_AS|
|Kirgisisch (Kirgisistan)|0x0440|0x0419|Cyrillic_General_CI_AS|
|Lao (Volksrepublik Laos)|0x0454|0x0454|Auf Serverebene nicht verfügbar|
|Lettisch (Lettland)|0x0426|0x0426|Latvian_CI_AS|
|Litauisch (Litauen)|0x0427|0x0427|Lithuanian_CI_AS|
|Niedersorbisch (Deutschland)|0x082e|0x0409|Latin1_General_CI_AS|
|Luxemburgisch (Luxemburg)|0x046e|0x0409|Latin1_General_CI_AS|
|Mazedonisch (Nordmazedonien)|0x042f|0x042f|Macedonian_FYROM_90_CI_AS|
|Malaiisch (Brunei Darussalam)|0x083e|0x0409|Latin1_General_CI_AS|
|Malaiisch (Malaysia)|0x043e|0x0409|Latin1_General_CI_AS|
|Malayalam (Indien)|0x044c|0x0439|Auf Serverebene nicht verfügbar|
|Maltesisch (Malta)|0x043a|0x043a|Latin1_General_CI_AI|
|Maori (Neuseeland)|0x0481|0x0481|Latin1_General_CI_AI|
|Mapudungun (Chile)|0x047a|0x047a|Latin1_General_CI_AI|
|Marathi (Indien)|0x044e|0x0439|Auf Serverebene nicht verfügbar|
|Mohawk (Kanada)|0x047c|0x047c|Latin1_General_CI_AI|
|Mongolisch (Mongolei)|0x0450|0x0419|Cyrillic_General_CI_AS|
|Mongolisch (VRC)|0x0850|0x0419|Cyrillic_General_CI_AS|
|Nepalesisch (Nepal)|0x0461|0x0461|Auf Serverebene nicht verfügbar|
|Norwegisch, Bokmål (Norwegen)|0x0414|0x0414|Latin1_General_CI_AI|
|Norwegisch (Nynorsk, Norwegen)|0x0814|0x0414|Latin1_General_CI_AI|
|Okzitanisch (Frankreich)|0x0482|0x040c|French_CI_AS|
|Odia (Indien)|0x0448|0x0439|Auf Serverebene nicht verfügbar|
|Paschtu (Afghanistan)|0x0463|0x0463|Auf Serverebene nicht verfügbar|
|Persisch (Iran)|0x0429|0x0429|Latin1_General_CI_AI|
|Polnisch (Polen)|0x0415|0x0415|Polish_CI_AS|
|Portugiesisch (Brasilien)|0x0416|0x0409|Latin1_General_CI_AS|
|Portugiesisch (Portugal)|0x0816|0x0409|Latin1_General_CI_AS|
|Punjabi (Indien)|0x0446|0x0439|Auf Serverebene nicht verfügbar|
|Quechua (Bolivien)|0x046b|0x0409|Latin1_General_CI_AS|
|Quechua (Ecuador)|0x086b|0x0409|Latin1_General_CI_AS|
|Quechua (Peru)|0x0c6b|0x0409|Latin1_General_CI_AS|
|Rumänisch (Rumänien)|0x0418|0x0418|Romanian_CI_AS|
|Rätoromanisch (Schweiz)|0x0417|0x0417|Latin1_General_CI_AI|
|Russisch (Russische Föderation)|0x0419|0x0419|Cyrillic_General_CI_AS|
|Sacha (Russische Föderation)|0x0485|0x0485|Latin1_General_CI_AI|
|Inari-Sami (Finnland)|0x243b|0x083b|Latin1_General_CI_AI|
|Lule-Sami (Norwegen)|0x103b|0x043b|Latin1_General_CI_AI|
|Lule-Sami (Schweden)|0x143b|0x083b|Latin1_General_CI_AI|
|Nord-Sami (Finnland)|0x0c3b|0x083b|Latin1_General_CI_AI|
|Nord-Sami (Norwegen)|0x043b|0x043b|Latin1_General_CI_AI|
|Nord-Sami (Schweden)|0x083b|0x083b|Latin1_General_CI_AI|
|Skolt-Sami (Finnland)|0x203b|0x083b|Latin1_General_CI_AI|
|Süd-Sami (Norwegen)|0x183b|0x043b|Latin1_General_CI_AI|
|Süd-Sami (Schweden)|0x1c3b|0x083b|Latin1_General_CI_AI|
|Sanskrit (Indien)|0x044f|0x0439|Auf Serverebene nicht verfügbar|
|Serbisch (Bosnien und Herzegowina, kyrillisch)|0x1c1a|0x0c1a|Latin1_General_CI_AI|
|Serbisch (Bosnien und Herzegowina, lateinisch)|0x181a|0x081a|Latin1_General_CI_AI|
|Serbisch (Serbien, kyrillisch)|0x0c1a|0x0c1a|Latin1_General_CI_AI|
|Serbisch (Serbien, lateinisch)|0x081a|0x081a|Latin1_General_CI_AI|
|Sesotho sa Leboa/Nord-Sotho (Südafrika)|0x046c|0x0409|Latin1_General_CI_AS|
|Setswana/Tswana (Südafrika)|0x0432|0x0409|Latin1_General_CI_AS|
|Sinhala (Sri Lanka)|0x045b|0x0439|Auf Serverebene nicht verfügbar|
|Slowakisch (Slowakei)|0x041b|0x041b|Slovak_CI_AS|
|Slowenisch (Slowenien)|0x0424|0x0424|Slovenian_CI_AS|
|Spanisch (Argentinien)|0x2c0a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Bolivien)|0x400a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Chile)|0x340a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Kolumbien)|0x240a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Costa Rica)|0x140a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Dominikanische Republik)|0x1c0a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Ecuador)|0x300a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (El Salvador)|0x440a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Guatemala)|0x100a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Honduras)|0x480a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Mexiko)|0x080a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Nicaragua)|0x4c0a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Panama)|0x180a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Paraguay)|0x3c0a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Peru)|0x280a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Puerto Rico)|0x500a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Spanien)|0x0c0a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Spanien, traditionelle Sortierung)|0x040a|0x040a|Traditional_Spanish_CI_AS|
|Spanisch (USA)|0x540a|0x0409|Latin1_General_CI_AS|
|Spanisch (Uruguay)|0x380a|0x0c0a|Modern_Spanish_CI_AS|
|Spanisch (Venezuela)|0x200a|0x0c0a|Modern_Spanish_CI_AS|
|Suaheli (Kenia)|0x0441|0x0409|Latin1_General_CI_AS|
|Schwedisch (Finnland)|0x081d|0x040b|Finnish_Swedish_CI_AS|
|Schwedisch (Schweden)|0x041d|0x040b|Finnish_Swedish_CI_AS|
|Syrisch (Syrien)|0x045a|0x045a|Auf Serverebene nicht verfügbar|
|Tadschikisch (Tadschikistan)|0x0428|0x0419|Cyrillic_General_CI_AS|
|Tamazight (Algerien, lateinisch)|0x085f|0x085f|Latin1_General_CI_AI|
|Tamil (Indien)|0x0449|0x0439|Auf Serverebene nicht verfügbar|
|Tatarisch (Russische Föderation)|0x0444|0x0444|Cyrillic_General_CI_AS|
|Telugu (Indien)|0x044a|0x0439|Auf Serverebene nicht verfügbar|
|Thai (Thailand)|0x041e|0x041e|Thai_CI_AS|
|Tibetisch (VRC)|0x0451|0x0451|Auf Serverebene nicht verfügbar|
|Türkisch (Türkei)|0x041f|0x041f|Turkish_CI_AS|
|Turkmenisch (Turkmenistan)|0x0442|0x0442|Latin1_General_CI_AI|
|Uighurisch (VRC)|0x0480|0x0480|Latin1_General_CI_AI|
|Ukrainisch (Ukraine)|0x0422|0x0422|Ukrainian_CI_AS|
|Obersorbisch (Deutschland)|0x042e|0x042e|Latin1_General_CI_AI|
|Urdu (Pakistan)|0x0420|0x0420|Latin1_General_CI_AI|
|Usbekisch (Usbekistan, kyrillisch)|0x0843|0x0419|Cyrillic_General_CI_AS|
|Usbekisch (Usbekistan, lateinisch)|0x0443|0x0443|Uzbek_Latin_90_CI_AS|
|Vietnamesisch (Vietnam)|0x042a|0x042a|Vietnamese_CI_AS|
|Walisisch (Großbritannien)|0x0452|0x0452|Latin1_General_CI_AI|
|Wolof (Senegal)|0x0488|0x040c|French_CI_AS|
|Xhosa/isiXhosa (Südafrika)|0x0434|0x0409|Latin1_General_CI_AS|
|Yi (VRC)|0x0478|0x0409|Latin1_General_CI_AS|
|Yoruba (Nigeria)|0x046a|0x0409|Latin1_General_CI_AS|
|Zulu/isiZulu (Südafrika)|0x0435|0x0409|Latin1_General_CI_AS|

Nachdem Sie dem Server eine Sortierung zugewiesen haben, können Sie diese nur ändern, indem Sie alle Datenbankobjekte und Daten exportieren, die *Masterdatenbank* neu erstellen und alle Datenbankobjekte und Daten wieder importieren. Anstatt die Standardsortierung einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu ändern, können Sie die gewünschte Sortierung beim Erstellen einer neuen Datenbank oder Datenbankspalte festlegen.    

Zum Abfragen der Serversortierung einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwenden Sie die `SERVERPROPERTY`-Funktion:

```sql
SELECT CONVERT(varchar, SERVERPROPERTY('collation'));
```

Verwenden Sie die folgende integrierte `fn_helpcollations()`-Funktion, um alle verfügbaren Sortierungen vom Server abzufragen.

```sql
SELECT * FROM sys.fn_helpcollations();
```
    
#### <a name="database-level-collations"></a><a name="Database-level-collations"></a> Sortierungen auf Datenbankebene    
Beim Erstellen oder Ändern einer Datenbank können Sie die `COLLATE`-Klausel der `CREATE DATABASE`- oder `ALTER DATABASE`-Anweisung verwenden, um die Standardsortierung für die Datenbank festzulegen. Wenn keine Sortierung angegeben wird, wird die Datenbank der Serversortierung zugeordnet.    
    
Sie können die Sortierung von Systemdatenbanken nicht ändern, es sei denn, Sie ändern die Sortierung für den Server.
    
Die Datenbanksortierung wird für alle Metadaten in der Datenbank verwendet und ist der Standard für alle Zeichenfolgenspalten, temporären Objekte, Variablennamen und anderen in der Datenbank verwendeten Zeichenfolgen. Wenn Sie die Sortierung einer Benutzerdatenbank ändern, treten möglicherweise Sortierungskonflikte auf, wenn Abfragen in der Datenbank auf temporäre Tabellen zugreifen. Temporäre Tabellen werden immer in der Systemdatenbank *tempdb* gespeichert, die die Sortierung für die Instanz verwendet. Abfragen, die Zeichendaten aus der Benutzerdatenbank und *tempdb* vergleichen, können fehlschlagen, wenn die Sortierungen einen Konflikt beim Auswerten der Zeichendaten verursachen. Sie können dieses Problem beheben, indem Sie die `COLLATE`-Klausel in der Abfrage angeben. Weitere Informationen finden Sie unter [COLLATE (Transact-SQL)](~/t-sql/statements/collations.md).    

> [!NOTE]
> Sie können die Sortierung nicht ändern, nachdem die Datenbank in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] erstellt wurde.

Sie können die Sortierung einer Benutzerdatenbank ändern, indem Sie eine `ALTER DATABASE`-Anweisung wie die folgende verwenden:

```sql
ALTER DATABASE myDB COLLATE Greek_CS_AI;
```

> [!IMPORTANT]
> Das Ändern der Sortierung auf Datenbankebene hat keinen Einfluss auf Sortierungen auf Spalten- oder Ausdrucksebene.

Sie können die aktuelle Sortierung einer Datenbank abrufen, indem Sie eine Anweisung wie die folgende verwenden:

```sql
SELECT CONVERT (VARCHAR(50), DATABASEPROPERTYEX('database_name','collation'));
```

#### <a name="column-level-collations"></a><a name="Column-level-collations"></a> Sortierungen auf Spaltenebene    
Wenn Sie eine Tabelle erstellen oder ändern, können Sie mithilfe der `COLLATE`-Klausel Sortierungen für alle Zeichenfolgenspalten festlegen. Wenn Sie keine Sortierung festlegen, wird der Spalte die Standardsortierung der Datenbank zugewiesen.    

Sie können die Sortierung einer Spalte ändern, indem Sie eine `ALTER TABLE`-Anweisung ähnlich der folgenden verwenden:

```sql
ALTER TABLE myTable ALTER COLUMN mycol NVARCHAR(10) COLLATE Greek_CS_AI;
```
    
#### <a name="expression-level-collations"></a><a name="Expression-level-collations"></a> Sortierungen auf Ausdrucksebene    
Sortierungen auf Ausdrucksebene werden zum Zeitpunkt der Ausführung einer Anweisung festgelegt. Sie haben Auswirkungen auf die Art und Weise, wie ein Resultset zurückgegeben wird. Dadurch können `ORDER BY`-Sortierergebnisse gebietsschemaspezifisch sein. Verwenden Sie eine `COLLATE`-Klausel wie die folgende, um Sortierungen auf Ausdrucksebene zu implementieren:    
    
```sql    
SELECT name FROM customer ORDER BY name COLLATE Latin1_General_CS_AI;    
```    
    
###  <a name="locale"></a><a name="Locale_Defn"></a> Gebietsschema    
Ein Gebietsschema besteht aus Informationen, die zu einem Ort oder einer Kultur gehören. Diese Informationen können den Namen und Bezeichner der gesprochenen Sprache, die Schrift zum Schreiben der Sprache und kulturelle Konventionen umfassen. Sortierungen können einem oder mehreren Gebietsschemas zugeordnet sein. Weitere Informationen finden Sie unter [Von Microsoft zugewiesene Gebietsschemabezeichner (LCIDs)](/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c).    
    
###  <a name="code-page"></a><a name="Code_Page_Defn"></a> Codepage    
Eine Codepage ist ein geordneter Satz von Zeichen in einem vorgegebenen Skript, in dem ein numerischer Index, oder Codepunktwert, mit jedem Zeichen verbunden ist. Eine Windows-Codepage wird in der Regal als *Zeichensatz* oder *charset* bezeichnet. Codepages werden zur Unterstützung der Zeichensätze und Tastaturlayouts verschiedener Windows-Systemgebietsschemas verwendet.     
 
###  <a name="sort-order"></a><a name="Sort_Order_Defn"></a> Sortierreihenfolge    
Die Sortierreihenfolge gibt an, wie Datenwerte sortiert werden. Die Reihenfolge wirkt sich auf die Ergebnisse von Datenvergleichen aus. Daten werden mithilfe von Sortierungen sortiert, die mit Indizes optimiert werden können.    
    
##  <a name="unicode-support"></a><a name="Unicode_Defn"></a> Unicode-Unterstützung    
Unicode ist ein Standard zum Zuordnen von Codepunkten zu Zeichen. Da dieser Standard entwickelt wurde, um alle Zeichen aus allen Sprachen der Welt zu unterstützen, benötigen Sie keine unterschiedlichen Codepages zur Verarbeitung von verschiedenen Zeichensätzen.

### <a name="unicode-basics"></a>Grundlagen zu Unicode
Das Speichern von Daten in mehreren Sprachen innerhalb einer Datenbank ist schwierig zu verwalten, wenn nur Zeichendaten und Codepages verwendet werden. Es ist außerdem schwierig, eine Codepage für die Datenbank zu finden, die alle erforderlichen sprachenspezifischen Zeichen speichern kann. Außerdem ist es schwierig, die richtige Übersetzung von Sonderzeichen zu gewährleisten, wenn sie von verschiedenen Clients gelesen oder geändert werden, die verschiedene Codepages ausführen. Datenbanken, die internationale Clients unterstützen, sollten immer Unicode-Datentypen anstelle von Nicht-Unicode-Datentypen verwenden.

Eine Datenbank für Kunden in Nordamerika muss z. B. drei Hauptsprachen verarbeiten:

-  Spanische Namen und Adressen für Mexiko.
-  Französische Namen und Adressen für Quebec.
-  Englische Namen und Adressen für den übrigen Teil Kanadas und die USA.

Wenn Sie nur Zeichenspalten und Codepages verwenden, müssen Sie sicherstellen, dass die Datenbank mit einer Codepage installiert wird, die die Zeichen aller drei Sprachen verarbeiten kann. Außerdem müssen Sie darauf achten, dass die ordnungsgemäße Übersetzung von Zeichen aus einer der Sprachen sichergestellt ist, wenn die Zeichen von Clients gelesen werden, die eine Codepage für eine andere Sprache ausführen.
   
> [!NOTE]
> Die Codepages, die ein Client verwendet, werden durch die Einstellungen des Betriebssystems (OS) bestimmt. Verwenden Sie zum Festlegen von Clientcodepages unter Windows die Option **Ländereinstellungen** in der Systemsteuerung.    

Es wäre schwierig, eine Codepage für Zeichendatentypen auszuwählen, die alle Zeichen unterstützt, die weltweit verwendet werden. Die einfachste Möglichkeit, Zeichendaten in internationalen Datenbanken zu verwalten, besteht darin, immer einen Datentyp zu verwenden, der Unicode unterstützt. 

### <a name="unicode-data-types"></a>Unicode-Datentypen
Wenn Sie Zeichendaten speichern, die mehrere Sprachen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] widerspiegeln ([!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] und höher), verwenden Sie Unicode-Datentypen (**nchar**, **nvarchar** und **ntext**) anstelle von Nicht-Unicode-Datentypen (**char**, **varchar** und **text**). 

> [!NOTE]
> Die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] kann bei Unicode-Datentypen bis zu 65.535 Zeichen mit UCS-2 oder den gesamten Unicode-Bereich (1.114.111 Zeichen) darstellen, wenn ergänzende Zeichen verwendet werden. Weitere Informationen zum Aktivieren von ergänzenden Zeichen finden Sie unter [Ergänzende Zeichen](#Supplementary_Characters).

Alternativ dazu gilt ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] Folgendes: Wenn eine für UTF-8 aktivierte Sortierung (\_UTF8) verwendet wird, werden Datentypen, die zuvor kein Unicode waren (**char** und **varchar**) zu Unicode-Datentypen mit UTF-8-Codierung. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ändert das Verhalten der bereits vorhandenen Unicode-Datentypen (**nchar**, **nvarchar** und **ntext**) nicht, die weiterhin USC-2- oder UTF-16-Codierung verwenden. Weitere Informationen finden Sie unter [Speicherunterschiede zwischen UTF-8 und UTF-16](#storage_differences).

### <a name="unicode-considerations"></a>Weitere Überlegungen
Nicht-Unicode-Datentypen verfügen über umfassende Einschränkungen. Das liegt daran, dass ein Nicht-Unicode-Computer auf die Verwendung einer einzelnen Codepage beschränkt ist. Es kann sein, dass Sie mit Unicode eine bessere Systemleistung erzielen, da weniger Codepagekonvertierungen erforderlich sind. Unicode-Sortierungen müssen auf der Datenbank-, Spalten- oder Ausdrucksebene einzeln ausgewählt werden, weil sie auf Serverebene nicht unterstützt werden.    

Wenn Sie Daten von einem Server auf einen Client verschieben, wird die Serversortierung von älteren Clienttreibern möglicherweise nicht erkannt. Dies kann passieren, wenn Sie Daten von einem Unicode-Server auf einen Nicht-Unicode-Client verschieben. Die beste Möglichkeit, dieses Problem zu beheben, besteht in einem Update des Clientbetriebssystems, wobei die zugrunde liegenden Systemsortierungen aktualisiert werden. Wenn auf dem Client Datenbankclient-Software installiert ist, sollten Sie ein Serviceupdate der Datenbankclient-Software in Erwägung ziehen.    
    
> [!TIP]
> Sie können auch versuchen, eine andere Sortierung für die Daten auf dem Server zu verwenden. Wählen Sie eine Sortierung aus, die einer Codepage auf dem Client zugeordnet werden kann.    
>
> Sie können eine der Sortierungen mit ergänzenden Zeichen (\_SC) oder eine der Version 140-Sortierungen auswählen, um die in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verfügbaren UTF-16-Sortierungen ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher) zu verwenden, um die Suche und die Sortierung einiger Unicode-Zeichen (nur Windows-Sortierungen) zu verbessern.    
 
Sie müssen Sortierungen mit UTF-8-Codierung (\_UTF8) auswählen, um die in [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] verfügbaren UTF-8-Sortierungen verwenden zu können und die Suche und Sortierung einiger Unicode-Zeichen (nur Windows-Sortierungen) zu verbessern.
 
-   Das UTF8-Flag kann auf folgende Sortierungen angewendet werden:    
    -   Linguistische Sortierungen, die bereits ergänzende Zeichen (\_SC) oder Variierungsauswahlzeichen (\_VSS) unterstützen
    -   Die binäre Sortierung BIN2<sup>1</sup>
    
-   Das UTF8-Flag kann auf folgende Sortierungen nicht angewendet werden:    
    -   Linguistische Sortierungen, die keine ergänzenden Zeichen (\_SC) oder Variierungsauswahlzeichen (\_VSS) unterstützen
    -   Die binären Sortierungen BIN und BIN2<sup>2</sup>
    -   Die SQL\_*-Sortierungen  
    
<sup>1</sup> Ab [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CTP 2.3. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CTP 3.0 hat die Sortierung **UTF8_BIN2** durch **Latin1_General_100_BIN2_UTF8** ersetzt.        
<sup>2</sup> Bis [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] CTP 2.3.    
    
Zum Abwägen der Vor- und Nachteile von Unicode- und Nicht-Unicode-Datentypen müssen Sie das Szenario testen und die Leistungsunterschiede in Ihrer Umgebung messen. Es wird empfohlen, die Sortierung zu standardisieren, die auf Systemen in Ihrem Unternehmen verwendet wird, und nach Möglichkeit Unicode-Server und -Clients bereitzustellen.    
    
In vielen Fällen interagiert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit anderen Servern oder Clients, und Ihr Unternehmen verwendet möglicherweise mehrere Standards für den Datenzugriff zwischen Anwendungen und Serverinstanzen. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Clients stellen einen der beiden Haupttypen dar:    
    
-   **Unicode-Clients**, die OLE DB und Open Database Connectivity (ODBC) Version 3.7 oder höher verwenden.    
-   **Nicht-Unicode-Clients**, die DB Library und ODBC Version 3.6 oder früher verwenden.    
    
Die folgende Tabelle enthält Informationen zur Verwendung mehrsprachiger Daten mit verschiedenen Kombinationen von Unicode- und Nicht-Unicode-Servern:    
    
|Server|Client|Vorteile oder Einschränkungen|    
|------------|------------|-----------------------------|    
|Unicode|Unicode|Da Unicode-Daten im gesamten System verwendet werden, verfügt dieses Szenario über die beste Systemleistung und den besten Schutz vor Beschädigung der abgerufenen Daten. Dies ist der Fall bei ActiveX Data Objects (ADO), OLE DB und ODBC Version 3.7 oder höher.|    
|Unicode|Nicht-Unicode|In diesem Szenario sind Einschränkungen oder Fehler beim Verschieben von Daten auf einen Clientcomputer möglich. Dies gilt besonders für Verbindungen zwischen einem Server, auf dem ein neueres Betriebssystem ausgeführt wird, und einem Client, auf dem eine ältere Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder ein älteres Betriebssystem installiert ist. Die Unicode-Daten auf dem Server versuchen, eine Zuordnung zu einer entsprechenden Codeseite auf dem Nicht-Unicode-Client zu erstellen, um die Daten zu konvertieren.|    
|Nicht-Unicode|Unicode|Dies ist keine gute Konfiguration für die Verwendung mehrsprachiger Daten. Sie können keine Unicode-Daten auf den Nicht-Unicode-Server schreiben. Es ist wahrscheinlich, dass Probleme auftreten, wenn Daten an Server gesendet werden, die von der Codepage des Servers nicht erfasst werden.|    
|Nicht-Unicode|Nicht-Unicode|Dieses Szenario ist bei mehrsprachigen Daten mit zahlreichen Beschränkungen verbunden. Sie können nur eine Codepage verwenden.|    
    
##  <a name="supplementary-characters"></a><a name="Supplementary_Characters"></a> Ergänzende Zeichen    
Das Unicode Consortium hat jedem Zeichen einen eindeutigen Codepunkt zugeordnet, der einem Wert im Bereich von 000000 bis 10FFFF entspricht. Die am häufigsten verwendeten Zeichen haben Codepunktwerte im Bereich von 000000 bis 00FFFF (65.535 Zeichen), die in ein 8-Bit- oder 16-Bit-Wort im Arbeitsspeicher und auf dem Datenträger passen. Dieser Bereich wird in der Regel als der Basic Multilingual Plane (BMP) festgelegt. 

Aber das Unicode Consortium hat 16 zusätzliche „Ebenen“ von Zeichen eingerichtet, die jeweils genauso groß sind wie die BMP. Durch diese Definition kann Unicode 1.114.112 Zeichen (d. h. 2<sup>16</sup> × 17 Zeichen) innerhalb des Codepunktbereichs von 000000 bis 10FFFF darstellen. Zeichen mit Codepunktwerten größer als 00FFFFFF erfordern zwei bis vier aufeinanderfolgende 8-Bit-Wörter (UTF-8) oder zwei aufeinanderfolgende 16-Bit-Wörter (UTF-16). Diese Zeichen, die sich außerhalb der BMP befinden, werden als *ergänzende Zeichen* bezeichnet, und die zusätzlichen aufeinanderfolgenden 8-Bit- oder 16-Bit-Wörter werden als *Ersatzzeichenpaare* bezeichnet. Weitere Informationen zu ergänzenden Zeichen, Ersatzzeichen und Ersatzzeichenpaaren finden Sie im [Unicode-Standard](http://www.unicode.org/standard/standard.html).    

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt Datentypen wie **nchar** und **nvarchar** bereit, um Unicode-Daten im BMP-Bereich (000000 bis 00FFFF) zu speichern, die die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] mit UCS-2 codiert. 

Mit [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] wurde eine neue Gruppe von Sortierungen von ergänzenden Zeichen (\_SC) eingeführt, die nur mit den Datentypen **nchar**, **nvarchar** und **sql_variant** verwendet werden können, um den gesamten Unicode-Zeichenbereich (000000 bis 10FFFF) darzustellen. Beispiel: **Latin1_General_100_CI_AS_SC** oder **Japanese_Bushu_Kakusu_100_CI_AS_SC**, wenn Sie eine japanische Sortierung verwenden. 
 
Mit [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] wurde die Unterstützung für ergänzende Zeichen mit den neuen UTF-8-fähigen Sortierungen ([\_UTF8](#utf8)) auf die Datentypen **char** und **varchar** erweitert. Diese Datentypen können auch den gesamten Unicode-Zeichenbereich darstellen.   

> [!NOTE]
> Ab [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] unterstützen alle neuen \_140-Sortierungen ergänzende Zeichen automatisch.

Bei Verwendung ergänzender Zeichen:    
    
-   Ergänzende Zeichen können bei Sortier- und Vergleichsvorgängen für Sortierungsversionen ab 90 verwendet werden.    
-   Alle Sortierungen von Version 100 unterstützen die linguistische Sortierung mit ergänzenden Zeichen.    
-   Die Verwendung von ergänzenden Zeichen wird in Metadaten nicht unterstützt, z. B. in Namen von Datenbankobjekten.    
-   Das SC-Flag kann in folgenden Versionen angewendet werden:    
    -   Sortierungen von Version 90    
    -   Sortierungen von Version 100    
    
-   Das SC-Flag auf folgende Sortierungen nicht angewendet werden:    
    -   Nicht versionierte Windows-Sortierungen der Version 80    
    -   Die binären Sortierungen BIN und BIN2    
    -   Die SQL\*-Sortierungen    
    -   Sortierungen der Version 140 (sie benötigen das SC-Flag nicht, da sie ergänzende Zeichen bereits unterstützen)    
    
In der folgenden Tabelle wird das Verhalten einiger Zeichenfolgenfunktionen und Zeichenfolgenoperatoren verglichen, wenn diese ergänzende Zeichen mit und ohne SCA-Sortierung (supplementary character-aware) verwenden:    
    
|Zeichenfolgenfunktion oder Operator|Mit SCA-Sortierung|Ohne SCA-Sortierung|    
|---------------------------------|--------------------------|-----------------------------|    
|[CHARINDEX](../../t-sql/functions/charindex-transact-sql.md)<br /><br /> [LEN](../../t-sql/functions/len-transact-sql.md)<br /><br /> [PATINDEX](../../t-sql/functions/patindex-transact-sql.md)|Das UTF-16-Ersatzzeichenpaar wird als einzelner Codepunkt betrachtet.|Das UTF-16-Ersatzzeichenpaar wird als zwei Codepunkte betrachtet.|    
|[LEFT](../../t-sql/functions/left-transact-sql.md)<br /><br /> [REPLACE](../../t-sql/functions/replace-transact-sql.md)<br /><br /> [REVERSE](../../t-sql/functions/reverse-transact-sql.md)<br /><br /> [RIGHT](../../t-sql/functions/right-transact-sql.md)<br /><br /> [SUBSTRING](../../t-sql/functions/substring-transact-sql.md)<br /><br /> [STUFF](../../t-sql/functions/stuff-transact-sql.md)|Diese Funktionen behandeln jedes Ersatzzeichenpaar als einzelnen Codepunkt und funktionieren wie erwartet.|Diese Funktionen können Ersatzzeichenpaare möglicherweise aufteilen und zu unerwarteten Ergebnissen führen.|    
|[NCHAR](../../t-sql/functions/nchar-transact-sql.md)|Gibt das Zeichen zurück, das dem angegebenen Unicode-Codepunktwert im Bereich von 0 bis 0x10FFFF entspricht. Wenn der angegebene Wert im Bereich von 0 bis 0xFFFF liegt, wird ein Zeichen zurückgegeben. Bei höheren Werten wird das entsprechende Ersatzzeichenpaar zurückgegeben.|Ein Wert größer als 0xFFFF gibt anstelle des entsprechenden Ersatzzeichens NULL zurück.|    
|[UNICODE](../../t-sql/functions/unicode-transact-sql.md)|Gibt einen UTF-16-Codepunkt im Bereich von 0 bis 0x10FFFF zurück.|Gibt einen UCS-2-Codepunkt im Bereich von 0 bis 0xFFFF zurück.|    
|[Einzelnes zu suchendes Zeichen](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)<br /><br /> [Platzhalterzeichen - nicht zu suchende(s) Zeichen](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)|Ergänzende Zeichen werden für alle Platzhaltervorgänge unterstützt.|Ergänzende Zeichen werden für diese Platzhaltervorgänge nicht unterstützt. Andere Platzhalteroperatoren werden unterstützt.|    
    
## <a name="gb18030-support"></a><a name="GB18030"></a> GB18030-Unterstützung    
GB18030 ist ein separater Standard, der in der Volksrepublik China zur Codierung von chinesischen Schriftzeichen verwendet wird. In GB18030 können Zeichen 1, 2 oder 4 Bytes lang sein. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bietet Unterstützung für GB18030-codierte Zeichen, indem diese erkannt werden, wenn Sie den Server von einer clientseitigen Anwendung erreichen, und in systemeigene Unicode-Zeichen konvertiert und als solche gespeichert werden. Nachdem sie auf dem Server gespeichert werden, werden sie in darauffolgenden Vorgängen als Unicode-Zeichen behandelt. 

Sie können eine beliebige chinesische Sortierung verwenden. Empfohlen wird die aktuelle Version 100. Alle \_100-Ebenen-Sortierungen unterstützen die linguistische Sortierung mit GB18030-Zeichen. Wenn die Daten ergänzende Zeichen (Ersatzzeichenpaare) enthalten, können Sie Such- und Sortiervorgänge mithilfe der in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] verfügbaren SC-Sortierungen optimieren.    

> [!NOTE]
> Stellen Sie sicher, dass Ihre Clienttools wie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] die Schriftart Dengxian verwenden, um Zeichenfolgen mit GB18030-codierten Zeichen richtig anzuzeigen.
    
## <a name="complex-script-support"></a><a name="Complex_script"></a> Unterstützung komplexer Skripts    
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann das Eingeben, Speichern, Ändern und Anzeigen komplexer Skripts unterstützen. Zu komplexen Skripts gehören folgende Typen:    
    
-   Skripts, die sowohl die Kombination von Text von rechts nach links und Text von links nach rechts enthalten, als auch die Kombination von arabischem und englischem Text.    
-   Skripts, deren Zeichen die Form abhängig von ihrer Position ändern, oder abhängig davon, ob sie mit anderen Zeichen kombiniert werden, wie beispielsweise arabische, indische und thailändische Zeichen.    
-   Sprachen, wie beispielsweise Thailändisch, die ein internes Wörterbuch zum Erkennen von Wörtern erfordern, da keine Abstände zwischen den Wörtern vorhanden sind.    
    
Datenbankanwendungen, die mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] interagieren, müssen über Steuerelemente verfügen, die komplexe Skripts unterstützen. Standardmäßige Windows-Formularsteuerelemente, die in verwaltetem Code erstellt werden, sind für komplexe Skripts aktiviert.    

## <a name="japanese-collations-added-in--sssql17-md"></a><a name="Japanese_Collations"></a> In [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] hinzugefügte japanische Sortierungen
 
Ab [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] werden neue japanische Sortierungsfamilien unterstützt, bei denen es sich um Abwandlungen verschiedener Optionen (\_CS, \_AS, \_KS, \_WS und \_VSS) handelt. 

Um diese Sortierungen aufzulisten, können Sie die [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] abfragen:      

```sql 
SELECT Name, Description FROM fn_helpcollations()  
WHERE Name LIKE 'Japanese_Bushu_Kakusu_140%' OR Name LIKE 'Japanese_XJIS_140%'
``` 

Alle neuen Sortierungen verfügen über integrierte Unterstützung ergänzender Zeichen, sodass keine der neuen **\_140**-Sortierungen ein SC-Flag benötigt (oder besitzt).

Diese Sortierungen werden in Indizes der [!INCLUDE[ssde_md](../../includes/ssde_md.md)], in für den Arbeitsspeicher optimierten Tabellen, in Columnstore-Indizes und in nativ kompilierten Modulen unterstützt.

<a name="ctp23"></a>

## <a name="utf-8-support"></a><a name="utf8"></a> Unterstützung für UTF-8
Mit [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] wird die vollständige Unterstützung für die weit verbreitete Zeichencodierung UTF-8 als Import- oder Exportcodierung oder als Sortierung für Zeichenfolgedaten auf Datenbank- und Spaltenebene eingeführt. UTF-8 ist für die Datentypen **char** and **varchar** zulässig und aktiviert, wenn Sie die Sortierung eines Objekts erstellen oder in eine Sortierung ändern, die ein *UTF8*-Suffix aufweist. Ein Beispiel hierfür ist das Ändern von **LATIN1_GENERAL_100_CI_AS_SC** in **LATIN1_GENERAL_100_CI_AS_SC_UTF8**. 

UTF-8 ist nur für Windows-Sortierungen verfügbar, die ergänzende Zeichen gemäß der Einführung in [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] unterstützen. Die Datentypen **nchar** und **nvarchar** ermöglichen nur die UCS-2- oder UTF-16-Codierung und bleiben unverändert.

### <a name="storage-differences-between-utf-8-and-utf-16"></a><a name="storage_differences"></a> Speicherunterschiede zwischen UTF-8 und UTF-16
Das Unicode Consortium hat jedem Zeichen einen eindeutigen Codepunkt zugeordnet, der einem Wert im Bereich von 000000 bis 10FFFF entspricht. Mit [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] stehen sowohl UTF-8- als auch UTF-16-Codierungen zur Darstellung des gesamten Bereichs zur Verfügung:    
-  Bei der UTF-8-Codierung benötigen Zeichen im ASCII-Bereich (000000 bis 00007F) 1 Byte, die Codepunkte von 000080 bis 0007FF 2 Bytes, die Codepunkte von 000800 bis 00FFFF 3 Bytes und die Codepunkte von 0010000 bis 0010FFFFFF 4 Bytes. 
-  Bei der UTF-16-Codierung benötigen die Codepunkte von 000000 bis 00FFFF 2 Bytes und die Codepunkte von 0010000 bis 0010FFFF 4 Bytes. 

In der folgenden Tabelle werden die Codierungsspeicherbytes für jeden Zeichenbereich und Codierungstyp aufgeführt:

|Codebereich (hexadezimal)|Codebereich (dezimal)|Speicherplatz in Bytes<sup>1</sup> mit UTF-8|Speicherplatz in Bytes<sup>1</sup> mit UTF-16|    
|---------------------------------|---------------------------------|--------------------------|-----------------------------|   
|000000–00007F|0–127|1|2|
|000080–00009F<br />0000A0–0003FF<br />000400–0007FF|128–159<br />160–1,023<br />1,024–2,047|2|2|
|000800–003FFF<br />004000–00FFFF|2,048–16,383<br />16,384–65,535|3|2|
|010000–03FFFF<sup>2</sup><br /><br />040000–10FFFF<sup>2</sup>|65,536–262,143<sup>2</sup><br /><br />262,144–1,114,111<sup>2</sup>|4|4|

<sup>1</sup> Die *Speicherbytes* beziehen sich auf die codierte Bytelänge, nicht auf die Datentypgröße beim Speichern auf dem Datenträger. Weitere Informationen zu den Speichergrößen auf dem Datenträger finden Sie unter [nchar und nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) und [char und varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md).

<sup>2</sup> Der Codepunktbereich für [ergänzende Zeichen](#Supplementary_Characters).

> [!TIP]   
> Üblicherweise denkt man in [CHAR(*n*) und VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md) oder in [NCHAR(*n*) und NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), wobei *n* für die Anzahl von Zeichen steht. Das liegt daran, dass in einer CHAR(10)-Spalte beispielsweise 10 ASCII-Zeichen im Bereich von 0 bis 127 gespeichert werden können, indem eine Sortierung wie **Latin1_General_100_CI_AI** angewendet wird, da jedes Zeichen in diesem Bereich nur 1 Byte nutzt.
>    
> Allerdings definiert *n* die Zeichenfolgenlänge bei [CHAR(*n*) und VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md) in *Bytes* (0–8,000) und bei [NCHAR(*n*) und NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) definiert *n* die Zeichenfolgenlänge in *Bytepaaren* (0–4,000). *n* definiert niemals die Anzahl von Zeichen, die gespeichert werden können.

Wie bereits erwähnt, kann die Wahl der geeigneten Unicode-Codierung und des geeigneten Datentyps je nach verwendetem Zeichensatz zu erheblichen Einsparungen beim Speicherplatz führen oder den aktuellen Speicherbedarf erhöhen. Wenn Sie beispielsweise eine UTF-8-fähige lateinische Sortierung wie **Latin1_General_100_CI_AI_SC_UTF8** verwenden, speichert eine `CHAR(10)`-Spalte 10 Bytes und kann 10 ASCII-Zeichen im Bereich von 0 bis 127 enthalten. Sie kann jedoch nur 5 Zeichen im Bereich von 128 bis 2047 und nur 3 Zeichen im Bereich von 2048 bis 65535 enthalten. Im Gegensatz dazu kann eine `NCHAR(10)`-Spalte, weil sie 10 Bytepaare (20 Bytes) speichert, 10 Zeichen im Bereich von 0 bis 65535 enthalten.  

Bevor Sie entscheiden, ob Sie UTF-8- oder UTF-16-Codierung für eine Datenbank oder Spalte verwenden, sollten Sie die Verteilung der Zeichenfolgendaten berücksichtigen, die gespeichert werden:
-  Wenn sie sich hauptsächlich im ASCII-Bereich von 0 bis 127 befinden (z. B. Englisch), erfordert jedes Zeichen 1 Byte bei UTF-8 und 2 Bytes bei UTF-16. Die Verwendung von UTF-8 hat Speichervorteile. Die Änderung eines vorhandenen Spaltendatentyps mit ASCII-Zeichen im Bereich von 0 bis 127 von `NCHAR(10)` in `CHAR(10)` bei Verwendung einer UTF-8-fähigen Sortierung führt zu einer Verringerung der Speicheranforderungen von 50 %. Diese Verringerung ist darauf zurückzuführen, dass `NCHAR(10)` 20 Byte für den Speicher erfordert, wohingegen `CHAR(10)` 10 Byte für die Darstellung der gleichen Unicode-Zeichenfolge erfordert.    
-  Oberhalb des ASCII-Bereichs benötigen fast alle lateinischen Schriftzeichen sowie Griechisch, Kyrillisch, Koptisch, Armenisch, Hebräisch, Arabisch, Syrisch, Tāna und N'Ko jeweils 2 Byte pro Zeichen in UTF-8 und UTF-16. In diesen Fällen gibt es für vergleichbare Datentypen keine signifikanten Speicherunterschiede (z. B. zwischen der Verwendung von **char** oder **nchar**).
-  Wenn es sich hauptsächlich um ostasiatische Schriftzeichen handelt (z. B. Koreanisch, Chinesisch und Japanisch), benötigt jedes Zeichen 3 Byte mit UTF-8 und 2 Byte mit UTF-16. Die Verwendung von UTF-16 hat Speichervorteile. 
-  Zeichen im Bereich von 010000 bis 10FFFFFF benötigen jeweils 4 Byte in UTF-8 und UTF-16. In diesen Fällen gibt es keine Speicherunterschiede für vergleichbare Datentypen (z. B. zwischen der Verwendung von **char** oder **nchar**).

Weitere Überlegungen zur finden Sie unter [Schreiben internationaler Transact-SQL-Anweisungen](../../relational-databases/collations/write-international-transact-sql-statements.md).

### <a name="converting-to-utf-8"></a><a name="converting"></a> Konvertieren in UTF-8
Da das *n* in [CHAR(*n*) und VARCHAR(*n*)](../../t-sql/data-types/char-and-varchar-transact-sql.md) oder in [NCHAR(*n*) und NVARCHAR(*n*)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) die Größe des Bytespeichers und nicht die Anzahl der Zeichen definiert, die gespeichert werden können, ist es wichtig, die Größe des Datentyps zu ermitteln, in den konvertiert werden soll, um das Abschneiden von Daten zu verhindern. 

Nehmen wir zum Beispiel eine Spalte, die als **NVARCHAR(100)** definiert ist und in der 180 Bytes japanischer Zeichen gespeichert werden. In diesem Beispiel werden die Spaltendaten derzeit mithilfe von UCS-2 oder UTF-16 codiert, wobei 2 Bytes pro Zeichen verwendet werden. Das Umstellen des Spaltentyps in **VARCHAR(200)** reicht nicht aus, um das Abschneiden von Daten zu verhindern, da der neue Datentyp nur 200 Bytes speichern kann, in UTF-8 codierte japanische Zeichen jedoch 3 Bytes benötigen. Daher muss die Spalte als **VARCHAR(270)** definiert werden, um Datenverluste durch das Abschneiden von Daten zu vermeiden.

Deshalb müssen Sie die voraussichtliche Bytegröße für die Spaltendefinition wissen, bevor Sie die vorhandenen Daten in UTF-8 konvertieren, und die Größe des neuen Datentyps entsprechend anpassen. Weitere Informationen finden Sie im [!INCLUDE[tsql](../../includes/tsql-md.md)]-Skript oder im SQL-Notebook in den [GitHub-Datenstichproben](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/unicode), die die Funktion [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md) und die Anweisung [COLLATE](../../t-sql/statements/collations.md) verwenden, um die Anforderungen an die richtige Datenlänge für UTF-8-Konvertierungsvorgänge in einer vorhandenen Datenbank zu bestimmen.

Verwenden Sie eine der in [Festlegen oder Ändern der Spaltensortierung](../../relational-databases/collations/set-or-change-the-column-collation.md) beschriebenen Methoden, um die Spaltensortierung und den Datentyp in einer vorhandenen Tabelle zu ändern.

Informationen zum Ändern der Datenbanksortierung, sodass neue Objekte die Datenbanksortierung standardmäßig erben können, oder zum Ändern der Serversortierung, sodass neue Datenbanken die Systemsortierung standardmäßig erben können, finden Sie im Abschnitt [Verwandte Aufgaben](#Related_Tasks) in diesem Artikel. 

##  <a name="related-tasks"></a><a name="Related_Tasks"></a> Related tasks    
    
|Aufgabe|Thema|    
|----------|-----------|    
|Beschreibt, wie die Sortierung der Instanz von SQL Server festgelegt oder geändert wird. Beachten Sie, dass die Änderung der Serversortierung die Sortierung vorhandener Datenbanken nicht ändert.|[Festlegen oder Ändern der Serversortierung](../../relational-databases/collations/set-or-change-the-server-collation.md)|    
|Beschreibt, wie die Sortierung einer Benutzerdatenbank festgelegt oder geändert wird. Beachten Sie, dass die Änderung der Datenbanksortierung die Sortierung vorhandener Tabellenspalten nicht ändert.|[Festlegen oder Ändern der Datenbanksortierung](../../relational-databases/collations/set-or-change-the-database-collation.md)|    
|Beschreibt, wie die Sortierung einer Spalte in der Datenbank festgelegt oder geändert wird.|[Festlegen oder Ändern der Spaltensortierung](../../relational-databases/collations/set-or-change-the-column-collation.md)|    
|Beschreibt, wie Sortierungsinformationen auf Server-, Datenbank- oder Spaltenebene zurückgegeben werden.|[Anzeigen von Sortierungsinformationen](../../relational-databases/collations/view-collation-information.md)|    
|Beschreibt, wie Transact-SQL-Anweisungen geschrieben werden, die die Übertragung von einer Sprache in eine andere verbessern, oder wie mehrere Sprachen einfacher unterstützt werden.|[Schreiben internationaler Transact-SQL-Anweisungen](../../relational-databases/collations/write-international-transact-sql-statements.md)|    
|Beschreibt, wie die Sprache von Fehlermeldungen und die bevorzugte Verwendungs- und Anzeigeweise von Datums-, Zeit-, und Währungsdaten geändert werden.|[Festlegen einer Sitzungssprache](../../relational-databases/collations/set-a-session-language.md)|    
    
##  <a name="related-content"></a><a name="Related_Content"></a> Related content    
Weitere Informationen finden Sie in folgenden verwandten Inhalten:
* [Bewährte Vorgehensweisen zur Sortierungsänderung bei SQL Server](https://go.microsoft.com/fwlink/?LinkId=113891)  
* [Verwenden des Unicode-Zeichenformats zum Importieren und Exportieren von Daten (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)
* [Schreiben internationaler Transact-SQL-Anweisungen](../../relational-databases/collations/write-international-transact-sql-statements.md)  
* [Bewährte Methode für die Migration zu Unicode in SQL Server](https://go.microsoft.com/fwlink/?LinkId=113890) (wird nicht mehr aktualisiert)   
* [Unicode Consortium](https://go.microsoft.com/fwlink/?LinkId=48619)  
* [Unicode Standard](http://www.unicode.org/standard/standard.html)  
* [UTF-8-Unterstützung im OLE DB-Treiber für SQL Server](../../connect/oledb/features/utf-8-support-in-oledb-driver-for-sql-server.md)  
* [SQL Server-Sortierungsname (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md)  
* [Name der Windows-Sortierung (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md)  
* [Einführung der UTF-8-Unterstützung für SQL Server](https://techcommunity.microsoft.com/t5/SQL-Server/Introducing-UTF-8-support-for-SQL-Server/ba-p/734928)      
* [COLLATE (Transact-SQL)](../../t-sql/statements/collations.md)      
* [Rangfolge von Sortierungen](../../t-sql/statements/collation-precedence-transact-sql.md)    
    
## <a name="see-also"></a>Weitere Informationen    
[Contained Database Collations](../../relational-databases/databases/contained-database-collations.md)     
[Auswählen einer Sprache beim Erstellen eines Volltextindex](../../relational-databases/search/choose-a-language-when-creating-a-full-text-index.md)     
[sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)       
[Einzelbyte- und Mehrbyte-Zeichensätze](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets)