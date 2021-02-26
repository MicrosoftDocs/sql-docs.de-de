---
title: Suchen von Text mit regulären Ausdrücken
description: Hier erfahren Sie, wie Sie einen regulären Ausdruck im Feld „Suchen nach“ des Dialogfelds „Suchen und ersetzen“ verwenden, um ein Muster anzugeben, das abgeglichen werden soll.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.regularexpressionbuilder
helpviewer_keywords:
- regular expressions [SQL Server Management Studio]
- Query Editor [SQL Server Management Studio], regular expression searches
- searches [SQL Server Management Studio], regular expressions
ms.assetid: a057690c-d118-4159-8e4d-2ed5ccfe79d3
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f09834f72bbcf5932961045a803c57dc9702a242
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344174"
---
# <a name="search-text-with-regular-expressions"></a>Suchen von Text mit regulären Ausdrücken

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Reguläre Ausdrücke sind eine präzise und flexible Notation zum Suchen und Ersetzen von Textmustern. Im Feld **Suchen nach** des [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]-Dialogfelds **Suchen und Ersetzen** kann eine Reihe von bestimmten regulären Ausdrücken verwendet werden.

## <a name="find-using-regular-expressions"></a>Suchvorgänge mithilfe von regulären Ausdrücken
1. Wenn Sie die Verwendung von regulären Ausdrücken im Feld **Suchen nach** bei den Vorgängen **Quick Find** (Schnellsuche), **Find in Files** (In Dateien suchen), **Quick Replace** (Schnellersetzung) oder **Replace in Files** (In Dateien ersetzen) aktivieren möchten, wählen Sie unter **Find Options** (Suchoptionen) die Option **Verwendung** und dann **Regular expressions** (Reguläre Ausdrücke) aus.

2. Die dreieckige Schaltfläche für die **Verweisliste** neben dem Feld **Suchen nach** ist jetzt aktiviert. Klicken Sie auf diese Schaltfläche, um eine Liste der am häufigsten verwendeten regulären Ausdrücke anzuzeigen. Wenn Sie ein Element aus dem Ausdrucks-Generator auswählen, wird es in die **Suchen nach** -Zeichenfolge eingefügt.

> [!NOTE]
> Zwischen der Syntax der regulären Ausdrücke, die in **Suchen nach** -Zeichenfolgen verwendet werden können, und der Syntax bei der [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework-Programmierung gibt es bestimmte Unterschiede. So werden z.B. geschweifte Klammern „{}“ bei **Suchen und Ersetzen** für markierte Ausdrücke verwendet. Der Ausdruck "zo{1}" entspricht allen Vorkommen von "zo", gefolgt von der Markierung 1, wie in "Alonzo1" und "Gonzo1". In .NET Framework hingegen wird die Notation {} für Quantifizierer verwendet. Der Ausdruck "zo{1}" entspricht hier allen Vorkommen von "z", gefolgt von genau einem "o", wie in "zone", nicht aber in "zoo".

Die folgende Tabelle enthält eine Beschreibung der regulären Ausdrücke, die in der **Verweisliste** verfügbar sind.

|Ausdruck|Syntax|BESCHREIBUNG|
|----------------|------------|-----------------|
|Beliebiges Zeichen|.|Entspricht jedem beliebigen einzelnen Zeichen außer einem Zeilenumbruch.|
|0 oder mehr|*|Entspricht 0 oder mehr Vorkommen des vorherigen Ausdrucks, schlägt alle möglichen Übereinstimmungen vor.|
|Ein oder mehr|+|Entspricht mindestens einem Vorkommen des vorherigen Ausdrucks.|
|Zeilenanfang|^|Verankert die übereinstimmende Zeichenfolgen am Anfang einer Zeile|
|Zeilenende|$|Verankert die übereinstimmende Zeichenfolgen am Ende einer Zeile|
|Wortanfang|\<|Es gibt nur eine Entsprechung, wenn ein Wort an dieser Stelle im Text anfängt.|
|Wortende|>|Es gibt nur eine Entsprechung, wenn ein Wort an dieser Stelle im Text endet.|
|Zeilenumbruch|\n|Entspricht einem plattformunabhängigen Zeilenumbruch. Fügt bei Ausdrücken zum Ersetzen einen Zeilenumbruch ein.|
|Beliebiges Zeichen im Zeichensatz|[]|Entspricht einem der Zeichen innerhalb von []. Um einen Bereich von Zeichen anzugeben, listen Sie den Anfangs- und Endbuchstaben getrennt durch einen Bindestrich (-) auf, z. B. [a-z].|
|Beliebiges Zeichen außerhalb des Zeichensatzes|[^...]|Entspricht einem der Zeichen außerhalb des Zeichensatzes, die nach dem ^ angegeben sind.|
|oder|&#124;|Entspricht entweder dem Ausdruck vor dem oder nach dem OR-Symbol (&#124;). Wird hauptsächlich innerhalb einer Gruppe verwendet. So entspricht z.B. „(sponge&#124;mud) bath“ sowohl „sponge bath“ als auch „mud bath“.|
|Escape|\|Entspricht dem Zeichen nach dem umgekehrten Schrägstrich (\\) als Literal. Auf diese Weise können Sie die Zeichen suchen, die in der Notation regulärer Ausdrücke verwendet werden, z. B. { und ^. Mit „ \\^“ wird z.B. nach dem Zeichen „^“ gesucht.|
|Markierter Ausdruck|{}|Entspricht Text, der mit dem eingeschlossenen Ausdruck markiert ist.|
|C/C++-Bezeichner|:i|Entspricht dem Ausdruck ([a-zA-Z_$][a-zA-Z0-9_$]*).|
|Zeichenfolge in Anführungszeichen|:q|Entspricht dem Ausdruck (("[^"]*")&#124;('[^']\*')).|
|Leerzeichen oder Tabstopp|:b|Entspricht entweder Tabstopp- oder Leerzeichen.|
|Integer|:z|Entspricht dem Ausdruck ([0-9]+).|

Die Liste aller regulären Ausdrücke, die in **Suchen und Ersetzen** -Vorgängen zulässig sind, ist zu lang, um in der **Verweisliste** angezeigt werden zu können. Sie können auch einen der folgenden regulären Ausdrücke in eine **Suchen nach** -Zeichenfolge einfügen:

|Ausdruck|Syntax|BESCHREIBUNG|
|----------------|------------|-----------------|
|Minimal - zero or more|@|Entspricht 0 oder mehr Vorkommen des vorherigen Ausdrucks, schlägt so wenig Zeichen wie möglich vor.|
|Minimal - one or more|#|Entspricht einem oder mehr Vorkommen des vorherigen Ausdrucks, schlägt so wenig Zeichen wie möglich vor.|
|n-mal wiederholen|^n|Entspricht n Vorkommen des vorherigen Ausdrucks. [0-9]^4 entspricht z. B. einer beliebigen Zeichenfolge aus vier Zahlen.|
|Gruppierung|()|Gruppiert einen untergeordneten Ausdruck.|
|n-markierter Text|\n|Gibt in einem **Suchen und Ersetzen** -Ausdruck den Text an, der dem n-ten markierten Ausdruck entspricht, wobei n eine Zahl von 1 bis 9 ist.<br /><br /> In einem **Ersetzen** -Ausdruck wird mit „\0“ der gesamte übereinstimmende Text eingefügt.|
|Rechtsbündig ausgerichtetes Feld|\\(w,n)|Richtet in einem **Ersetzen** -Ausdruck den n-ten markierten Ausdruck in einem Feld um mindestens *w* Zeichen nach rechts aus.|
|Linksbündig ausgerichtetes Feld|\\(-w,n)|Richtet in einem **Ersetzen** -Ausdruck den n-ten markierten Ausdruck in einem Feld um mindestens *w* Zeichen nach links aus.|
|Übereinstimmung verhindern|~(X)|Verhindert eine Übereinstimmung, wenn X an dieser Stelle im Ausdruck vorkommt. Beispiel: Für real~(ity) gibt es eine Übereinstimmung für "real" in "realty" und "really," aber nicht für die Zeichenfolge "real" in "reality".|
|Alphanumerisches Zeichen|:a|Entspricht dem Ausdruck ([a-zA-Z0-9]).|
|Alphabetisches Zeichen|:c|Entspricht dem Ausdruck ([a-zA-Z]).|
|Dezimalzahl|:d|Entspricht dem Ausdruck ([0-9]).|
|Hexadezimalzahl|:h|Entspricht dem Ausdruck ([0-9a-fA-F]+).|
|Rationale Zahl|:n|Entspricht dem Ausdruck (([0-9]+.[0-9]*)&#124;([0-9]\*.[0-9]+)&#124;([0-9]+)).|
|Alphabetische Zeichenfolge|:w|Entspricht dem Ausdruck ([a-zA-Z]+).|
|Escape|\e|Unicode U+001B.|
|Glocke|\g|Unicode U+0007.|
|Rücktaste|\h|Unicode U+0008.|
|Registerkarte|\t|Entspricht einem Tabstoppzeichen, Unicode U+0009.|
|Unicode-Zeichen|\x#### oder \u####|Entspricht einem Zeichen das durch einen Unicode-Wert angegeben ist, wobei #### Hexadezimalzahlen sind. Sie können ein Zeichen außerhalb der grundlegenden mehrsprachigen Ebene (d. h. einen Ersatz) mit dem ISO 10646-Codepunkt oder mit zwei Unicode-Codepunkten angeben, die die Werte des Ersatzpaars angeben.|

In der folgenden Tabelle ist die Syntax für Übereinstimmungen in Bezug auf Standardeigenschaften von Unicode-Zeichen aufgelistet. Die aus zwei Buchstaben bestehende Abkürzung ist mit der in der Datenbank für Eigenschaften von Unicode-Zeichen identisch. Die Abkürzungen lassen sich als Teil eines Zeichensatzes angeben. So entspricht z. B. der Ausdruck [:Nd:Nl:No] einer beliebigen Ziffer.

|Ausdruck|Syntax|BESCHREIBUNG|
|----------------|------------|-----------------|
|Großbuchstabe|:Lu|Entspricht einem beliebigen Großbuchstaben. So entspricht :Luhe z. B. "The", aber nicht "the".|
|Kleinbuchstabe|:Ll|Entspricht einem beliebigen Kleinbuchstaben. So entspricht :Llhe z. B. "the", aber nicht "The".|
|Titelbuchstabe|:Lt|Entspricht Zeichen, die einen Großbuchstaben und einen Kleinbuchstaben enthalten, z. B. Nj und Dz.|
|Modifikationszeichen|:Lm|Entspricht Buchstaben oder Interpunktion, z. B. Komma, Kreuz-Akzent und doppeltes gerades Anführungszeichen, zur Angabe von Änderungen am vorherigen Buchstaben.|
|Anderer Buchstabe|:Lo|Entspricht anderen Buchstaben, z. B. dem gotischen Buchstaben Ahsa.|  
|Dezimalzahl|:Nd|Entspricht Dezimalzahlen wie 0 bis 9 und deren Entsprechungen in normaler Breite.|
|Buchstabenzahlen|:Nl|Entspricht Buchstabenzahlen wie römischen Ziffern und der ideografischen Zahl Null.|
|Andere Ziffer|:No|Entspricht anderen Ziffern, z. B. der Zahl 1 im Zeichensatz Old Italic.|
|Öffnende Satzzeichen|:Ps|Entspricht öffnenden Satzzeichen, z. B. öffnende eckige oder geschweifte Klammern.|
|Schließende Satzzeichen|:Pe|Entspricht schließenden Satzzeichen, z. B. schließende eckige oder geschweifte Klammern.|
|Öffnendes Anführungszeichen|:Pi|Entspricht öffnenden doppelten Anführungszeichen.|
|Schließendes Anführungszeichen|:Pf|Entspricht einfachen Anführungszeichen und doppelten schließenden Anführungszeichen.|
|Bindestrich|:Pd|Entspricht dem Bindestrich.|
|Verbindungszeichen|:Pc|Entspricht dem Unterstrich oder der Unterlinie.|
|Andere Satzzeichen|:Po|Entspricht (,), ?, ", !, @, #, %, &, *, \\, (:), (;), ' und /.|
|Leerzeichen|:Zs|Entspricht Leerzeichen.|
|Zeilentrennzeichen|:Zl|Entspricht dem Unicode-Zeichen U+2028.|
|Absatztrennzeichen|:Zp|Entspricht dem Unicode-Zeichen U+2029.|
|Zeichen ohne Zwischenraum|:Mn|Entspricht Zeichen ohne Zwischenraum.|
|Verbindungszeichen|:Mc|Entspricht Verbindungszeichen.|
|Einschließungszeichen|:Me|Entspricht Einschließungszeichen.|
|Mathematisches Zeichen|:Sm|Entspricht +, =, ~, &#124;, \<, and >.|
|Währungssymbol|:Sc|Entspricht $ und anderen Währungssymbolen.|
|Modifikationszeichen|:Sk|Entspricht Modifikationszeichen wie Zirkumflex-Akzent, Gravis-Akzent und Macron.|
|Anderes Symbol|:So|Entspricht anderen Symbolen, z. B. dem Copyright-Zeichen, dem Pilcrow-Zeichen und dem Gradzeichen.|
|Andere Steuerelemente|:Cc|Entspricht dem Ende einer Zeile.|
|Anderes Format|:Cf|Formatierungssteuerzeichen, z. B. die bidirektionalen Steuerzeichen.|
|Ersatzzeichen|:Cs|Entspricht einer Hälfte eines Ersatzpaars.|
|Andere private Verwendung|:Co|Entspricht einem beliebigen Zeichen aus dem privaten Verwendungsbereich.|
|Andere nicht zugewiesene|:Cn|Entspricht Zeichen, die keinem Unicode-Zeichen entsprechen.|
|Angeben der Anzahl von Vorkommen des vorherigen Zeichens oder der Gruppe. Weitere Informationen finden Sie unter [Übereinstimmung mit genau n Vorkommen](/dotnet/standard/base-types/quantifiers-in-regular-expressions#match-exactly-n-times-n).|{n}, wobei n die Anzahl von Vorkommen ist.|`x(ab){2}x` findet „xababx“.<br/>`x(ab){2,3}x` findet „xababx“ und „xabababx“, jedoch nicht „xababababx“.|

Neben den Standardeigenschaften von Unicode-Zeichen können auch die folgenden zusätzlichen Eigenschaften als Teil eines Zeichensatzes angegeben werden:

|Ausdruck|Syntax|BESCHREIBUNG|
|----------------|------------|-----------------|
|Alpha|:Al|Entspricht einem beliebigen einzelnen Zeichen. So entspricht :Alhe Wörtern wie "The", "then" und "reached".|
|Numeric|:Nu|Entspricht einer beliebigen Zahl oder Ziffer.|
|Interpunktion|:Pu|Entspricht einem beliebigen Satzzeichen, z. B. ?, @, ' usw.|
|Leerzeichen|:Wh|Entspricht allen Arten von Leerzeichen, einschließlich Veröffentlichungsleerzeichen und ideografischen Leerzeichen.|
|Bidirektional|:Bi|Entspricht Zeichen aus Skripts mit Schreibrichtung von rechts nach links, z. B. Arabisch und Hebräisch.|
|Hangul|:Ha|Entspricht dem koreanischen Hangul-Alphabet und kombinierten Jamo-Lautzeichen.|
|Hiragana|:Hi|Entspricht Hiragana-Zeichen.|
|Katakana|:Ka|Entspricht Katakana-Zeichen.|
|Ideografisch/Han/Kanji|:Id|Entspricht ideografischen Zeichen, z. B. Han und Kanji.|

## <a name="see-also"></a>Weitere Informationen  
- [Suchen und Ersetzen](./search-and-replace.md)
- [Suchen von Text mit Platzhaltern](./search-text-with-wildcards.md)