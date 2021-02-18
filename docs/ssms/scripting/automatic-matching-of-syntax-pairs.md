---
title: Automatischer Vergleich von Syntaxpaaren
description: In diesem Artikel erfahren Sie mehr über den automatischen Abgleich von Syntaxpaaren im Abfrage-Editor (Trennzeichenvergleich), im XMLA-Abfrage-Editor (zugehörige Klammer) sowie mit MDX und DMX (Klammernvergleich).
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- IntelliSense [SQL Server], delimiter highlighting
- IntelliSense [SQL Server], syntax pair matching
ms.assetid: bfc54cda-bfd6-4545-a5b9-f9db2ae13769
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cbc0daf1948c72f66f0c4a5b12d4d21af535ca0f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354787"
---
# <a name="automatic-matching-of-syntax-pairs"></a>Automatischer Vergleich von Syntaxpaaren
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Durch den automatischen Vergleich von Syntaxpaaren erhalten Sie unmittelbar Aufschluss darüber, ob Syntaxelemente, die paarweise codiert werden müssen, ordnungsgemäß miteinander kombiniert sind. Im Abfrage-Editor von [!INCLUDE[ssDE](../../includes/ssde-md.md)] wird dieser Vorgang als Trennzeichenvergleich, im XMLA-Abfrage-Editor von Analysis Services als Klammernvergleich und im MDX- sowie im DMX-Editor als Klammernvergleich bezeichnet.  
  
## <a name="database-engine-query-editor-delimiter-matching"></a>Trennzeichenvergleich im Datenbank-Engine-Abfrage-Editor  
 Der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor vergleicht die Trennzeichen, die die Begrenzungen von Codeblöcken angeben. Der Vergleich erfolgt auf zwei verschiedene Weisen:  
  
-   Der Editor hebt beide Trennzeichen in einem Paar hervor, sobald Sie das zweite Trennzeichen des Paars eingegeben haben.  
  
-   Immer, wenn sich der Cursor auf einem der Trennzeichen eines Paars befindet, können Sie mithilfe der Tastenkombination STRG+] zum entsprechenden Trennzeichen springen.  
  
### <a name="delimiter-pairs"></a>Trennzeichenpaare  
 Beim automatischen Trennzeichenvergleich werden die folgenden Trennzeichen erkannt:  
  
|Vorangestelltes Trennzeichen|Schließendes Trennzeichen|  
|--------------------|-----------------------|  
|**(**|**)**|  
|**BEGIN**|**END**|  
|**BEGIN TRY**|**END TRY**|  
|**BEGIN CATCH**|**END CATCH**|  
  
 Beim automatischen Trennzeichenvergleich werden die Trennzeichen für Bezeichner in Klammern ([ObjectName]) oder Bezeichner in Anführungszeichen ("ObjectName") nicht erkannt. Beim Paarvergleich werden die einzelnen Anführungszeichen als Trennzeichen für Zeichenfolgenliterale ('string') nicht verglichen, weil die Farbcodierung bereits erkennen lässt, ob die Zeichenfolge ein Trennzeichen aufweist.  
  
### <a name="delimiter-highlighting"></a>Hervorhebung von Trennzeichen  
 Beim Vergleich werden sowohl das vorangestellte als auch das schließende Element eines Paars von Trennzeichen hervorgehoben. Auf diese Weise können Sie Codeblöcke visuell identifizieren und überprüfen, ob nicht übereinstimmende Paare von Trennzeichen vorhanden sind.  
  
 Trennzeichen werden hervorgehoben, wenn Sie den letzten Buchstaben des Paars eingeben. Wenn Sie z. B. bei einem BEGIN…END-Paar zuerst BEGIN und dann END eingeben, wird die Hervorhebung aktiviert, sobald Sie den letzten Buchstaben von END eingeben. Es ist nicht erforderlich, zuerst das vorangestellte und dann das schließende Trennzeichen einzugeben, um die Hervorhebung zu aktivieren. Wenn Sie zuerst END eingeben und dann im Skript einen Bildlauf nach oben ausführen und BEGIN eingeben, wird die Hervorhebung aktiviert, sobald Sie den letzten Buchstaben von BEGIN eingeben. Der letzte eingegebene Buchstabe muss nicht der Buchstabe sein, mit dem das Trennzeichen endet. Wenn Sie z. B. BEGIN zuerst falsch schreiben und BEIN eingeben, wird das BEGIN…END-Paar hervorgehoben, sobald Sie das fehlende G eingeben.  
  
 Das Trennzeichenpaar bleibt so lange hervorgehoben, bis Sie den Cursor verschieben. Die Hervorhebung wird auch dann beim Verschieben des Cursors deaktiviert, wenn der Cursor an eine Position im gleichen Trennzeichen verschoben wird. Sie können die Hervorhebung wieder aktivieren, indem Sie einen beliebigen Buchstaben in einem Element des Paars löschen und erneut eingeben.  
  
## <a name="analysis-services-xmla-query-editor-brace-matching"></a>Klammernvergleich im XMLA-Abfrage-Editor von Analysis Services  
 Beim Klammernvergleich im XMLA-Abfrage-Editor können Sie erkennen, ob Sie die Elemente geschlossen haben, da öffnende und schließende Klammern hervorgehoben werden. Sie können auch mithilfe der Tastenkombination STRG+] von einer Klammer zur jeweils entsprechenden Klammer springen.  
  
 Im XMLA-Abfrage-Editor wird der Klammernvergleich für die folgenden Ausdrücke ausgeführt:  
  
-   Übereinstimmende Start- und Endtags  
  
-   Jedes beliebige Paar aus eckigen Klammern: \<" and ">  
  
-   Anfang und Ende von Kommentaren.  
  
-   Anfang und Ende von Verarbeitungsanweisungen.  
  
-   Anfang und Ende von CDATA-Blöcken.  
  
-   Anfang und Ende von DTD-Deklarationen.  
  
-   Öffnende und schließende Anführungszeichen für Attribute.  
  
## <a name="mdx-and-dmx-editor-parenthesis-matching"></a>Klammernvergleich im MDX- und im DMX-Editor  
 Der MDX-Editor (Multidimensional Expressions) und der DMX-Editor (Data Mining Expressions) gleichen automatisch Klammernpaare in Funktionen ab.
