---
description: Versionierungssystem für die SQL-Dokumentation
title: Versionierungssystem für die SQL-Dokumentation
ms.date: 08/12/2020
ms.prod: sql
ms.technology: release-landing
ms.topic: conceptual
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||=azuresqldb-mi-current||=azure-sqldw-latest||>=aps-pdw-2016||>=sql-server-linux-2017||>=sql-server-2016
ms.openlocfilehash: 56ef939790de90d02add427f14f8817df885383f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079751"
---
# <a name="versioning-system-for-sql-documentation"></a>Versionierungssystem für die SQL-Dokumentation

[!INCLUDE[includes_appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

In diesem Artikel wird das _Versionierungssystem_ für die SQL-Dokumentation erläutert. In dem Versionierungssystem werden Informationen zu Produkten und deren Versionen gespeichert. Das System bietet die Möglichkeit, das für Sie interessante Produkt in der gewünschten Version auszuwählen. Anschließend wird die zugehörige Dokumentation angezeigt.

## <a name="applies-to-products"></a>„GILT FÜR“-Produkte

Bei den meisten SQL Server-Artikeln finden sich unter dem Titel die Wörter **GILT FÜR**. In dieser Zeile folgt eine praktische Auflistung von _SQL-Produkten_ mit Angaben, ob der Artikel für das Produkt relevant ist. So könnte beispielsweise das Produkt SQL Server als relevant, Azure SQL Database jedoch als irrelevant für den Artikel angegeben sein.

Die Zeile **GILT FÜR** enthält keine _Versionsinformationen_ zu Produkten. Es wird darauf geachtet, Diskrepanzen zwischen der Zeile **GILT FÜR** und den Produkten, die Gegenstand der Konfigurationen des Versionierungssystems sind, möglichst zu vermeiden.

## <a name="history-of-separate-file-sets"></a>Versionsgeschichte von separaten Dateisätzen

Bei SQL Server 2014 und früheren Versionen gibt es für jede Version eigene, vollständig separate Dokumentationsdateien. So wurde als Dokumentation für SQL Server 2014 zunächst eine Kopie der Dokumentation für SQL Server 2012 erstellt. Die Kopie für 2014 wurde dann während des Produktentwicklungszyklus bearbeitet.

Dieser frühere Ansatz bedeutete, dass ein in der Dokumentation für 2014 entdeckter Fehler auch in der Dokumentation für 2012 und 2008 bestehen konnte. Dies erschwerte die Behebung von Fehlern und die allgemeine Dokumentenpflege.

## <a name="multiple-versions-in-the-same-files"></a>Mehrere Versionen in den gleichen Dateien

Aus diesem und anderen Gründen werden die Dokumentationsdateien für SQL Server 2016 auch für 2017, 2019 und wahrscheinlich für \<vNext\> verwendet. Diese Konsolidierung wird durch die Zuweisung von _Versionierungsmonikern_ zu den SQL Server-Dokumentationsdateien ermöglicht. Die Versionierungsmoniker werden in dem Umfang zugewiesen oder explizit eingebettet, wie es für die jeweilige Dokumentationsdatei sinnvoll ist.

## <a name="versioning-control-in-the-ui"></a>Versionskontrolle in der Benutzeroberfläche

Bei der Anzeige eines beliebigen SQL-Dokumentationsartikel über die :::no-loc text="Docs":::-Website ist der aktuell gewählte Versionierungsmoniker über dem Inhaltsverzeichnis zu sehen. Das Steuerelement ist eine Dropdownliste.

![media_versioning-control-10-sql-server-2017.png](media/versioning-control-10-sql-server-2017.png)

Wenn Sie die Dokumentation für eine andere Version von SQL Server anzeigen möchten, klicken Sie auf den Erweiterungspfeil am Ende des aktuellen Versionierungsmonikers. Wählen Sie dann auf die gewünschte Produkt- und Versionskombination aus. Wenn Sie auf eine andere Version klicken, ändert sich umgehend die angezeigte Dokumentation, um die Unterschiede für die neu gewählte Version darzustellen. Häufig gibt es auch gar keine Änderungen.

![media_versioning-control-20-expanded.png](media/versioning-control-20-expanded.png)

### <a name="https-parameter-no-loc-textview"></a>HTTPS-Parameter :::no-loc text="view=":::

Jeder Artikel, dessen Webadresse mit `https://docs.microsoft.com/sql/` beginnt, verfügt über einen Parameter namens `?view=`, der an seine Adresse angehängt ist. Dieser Parameterwert ist der Versionierungsmonikercode.

Der _Monikercode_ in der `https`-Adresse stimmt immer mit dem _Monikernamen_ überein, der in der Versionskontrolle angezeigt wird.

## <a name="products-not-editions"></a>Produkte, nicht Editionen

### <a name="editions"></a>Editionen

In den 1990er-Jahren und in den 2000er-Jahren bot Microsoft SQL Server nur ein einziges Produkt an. Es gab verschiedene _Editionen_ jeder Version von SQL Server, wie z. B. die Editionen _Developer_ und _Enterprise_ von SQL Server 2008. Die Editionen standen jeweils für einen geringfügig unterschiedlichen Funktionsumfang, aber das Kernprodukt an sich war identisch. Auch neue SQL Server-Versionen sind ggf. in mehreren Editionen erhältlich.

### <a name="products"></a>Produkte

In Folge der zunehmenden Beliebtheit von Cloud Computing und Microsoft Azure veröffentlichte Microsoft das Produkt Azure SQL-Datenbank. Obwohl es viel gemeinsamen Code gibt, der sowohl vom traditionellen SQL Server-Produkt, das lokal eingesetzt wird, als auch von Azure SQL-Datenbank genutzt wird, handelt es sich hierbei um zwei wirklich getrennte Produkte.

Bei SQL wird anhand von Versionierungsmonikern zwischen Produkten, nicht aber zwischen Editionen, unterschieden.

#### <a name="azure-cloud-sql-products"></a>SQL-Produkte in der Azure-Cloud

Artikel, deren vollständige Webadressen mit `https://docs.microsoft.com/sql/` beginnen, gelten fast alle für mindestens eine Version des Produkts „SQL Server“. Eine großer Anteil dieser Artikel gilt auch für mindestens eines der mit SQL verbundenen Dienstprodukte, die in der Azure-Cloud gehostet werden. Eines dieser SQL-Cloudprodukte heißt Azure SQL-Datenbank.

Natürlich gibt es von Azure SQL-Datenbank nur eine Version. Fast alle Webadressen der Artikel, die sich auf Azure SQL-Datenbank beziehen, aber nicht auf SQL Server, beginnen mit `https://docs.microsoft.com/azure/sql-database/`.

## <a name="scenarios-of-version-filtering"></a>Szenarios der Versionsfilterung

Das Versionierungssystem filtert alle Dokumentationsinhalte heraus, die nicht für den aktuell aktiven Moniker gelten. Jedes Mal, wenn Sie einen anderen Versionierungsmoniker wählen, ändert sich der ausgeblendete Inhalt. Durch den Filter werden Inhalte auf den folgenden Ebenen ausgeblendet:

- Abschnitte oder Sätze innerhalb eines Artikels
- Einträge für Artikel im Inhaltsverzeichnis

Als Nächstes folgen Szenarios, die veranschaulichen, wie sich die Auswahl eines anderen Monikers auswirkt.

### <a name="scenario-1-within-the-current-article"></a>Szenario 1: Innerhalb des aktuellen Artikels

Im folgenden Szenario liegt der Schwerpunkt auf den Abschnitten im aktuell angezeigten Artikel:

1. Der aktuelle Versionierungsmoniker ist **SQL Server 2017**.
2. Sie lesen einen Abschnitt, in dem ein Feature beschrieben wird, das erstmals zu Version 2017 von SQL Server hinzugefügt wurde.
3. Sie ändern den Moniker in **SQL Server 2016**.
4. Der Abschnitt, den Sie gerade gelesen haben, ist nicht mehr vorhanden.
5. Noch einmal ändern Sie den Moniker, nun jedoch in **SQL Server 2019**.
6. Jetzt sehen Sie, dass der entsprechende Abschnitt, den Sie für 2017 gelesen haben, wieder angezeigt wird.

Im vorhergehenden Szenario ist der Abschnitt über das neue Feature in der Version 2017 wahrscheinlich durch einen _Monikerbereich_ gekennzeichnet, der den folgenden Monikercode enthält:

- `>=sql-server-2017`

Als der Moniker **SQL Server 2019** gewählt wurde, erkannte das Versionierungssystem, dass 2019 größer als 2017 oder gleich 2017 ist, und zeigte den Abschnitt an.

### <a name="scenario-2-click-a-link-to-a-hidden-article"></a>Szenario 2: Klicken auf einen Link zu einem ausgeblendeten Artikel

Im folgenden ungewöhnlichen Szenario wird erklärt, was passiert, wenn Sie auf einen Link zu einem Artikel klicken, der derzeit nicht im Inhaltsverzeichnis enthalten ist. Kurz gesagt, der Link funktioniert:

1. Der aktuelle Versionierungsmoniker ist **SQL Server 2017**.
2. Im aktuellen Artikel :::no-loc text="A"::: klicken Sie auf einen Link zu einem Artikel :::no-loc text="B":::, der nur für SQL Server 2016 gilt.
    - Vor dem Klick ist der Eintrag für Artikel :::no-loc text="B"::: im Inhaltsverzeichnis ausgeblendet.
3. Nach dem Klick wird der Artikel :::no-loc text="B"::: angezeigt.
    - Die Anzeige von Artikel :::no-loc text="B"::: zwingt die Versionskontrolle, zum Moniker **SQL Server 2016** zu wechseln,
    - weil der ursprüngliche Moniker **SQL Server 2017** abgebrochen werden musste. Durch diesen Abbruch wird eine Informationsmeldung am oberen Rand der Webseite angezeigt. Die [Meldung](#anchor-message-unavailable-for-moniker) gibt an, dass der aktuelle Moniker gewechselt werden musste, um den neuen Artikel :::no-loc text="B"::: anzuzeigen.

### <a name="scenario-3-navigate-to-an-https-address"></a>Szenario 3: Navigieren zu einer HTTPS-Adresse

Der folgende Artikel wurde neu für SQL Server 2017 hinzugefügt. In diesem Artikel werden die Features beschrieben, die in Version 2017 von SQL Server hinzugefügt wurden. Die meisten oder alle dieser neuen Features gehören auch zu Version 2019. Dies sind die Attribute des Artikels.

| attribute | Wert |
| :-------- | :---- |
| Titel | Neues in SQL Server 2017 |
| Monikerbereich | `=">= sql-server-2017"` |
| `https`-Adresse | `https://docs.microsoft.com/sql/sql-server/what-s-new-in-sql-server-2017` |
| &nbsp; | &nbsp; |

Anhand der grundlegenden `https`-Adresse wird in der folgenden Tabelle erläutert, was passiert, wenn der Parameter `?view=` vom Benutzer und mit verschiedenen Werten hinzugefügt wird.

| Wert von `?view=` | Verhalten der `https`-Adressnavigation |
| :---------------- | :------------------------------ |
| _(Kein Parameter)_ | Das Versionskontrollsystem versucht, den Standardwert für den Moniker zu verwenden. Normalerweise ist dieser auf die neueste Nicht-Vorschauversion von SQL Server eingestellt.<br/><br/>Ein Standardwert von SQL Server 2017 oder 2019 würde das Attribut `>= sql-server-2017` erfüllen.<br/><br/>Der Parameter würde automatisch an die Adresse `https` angehängt, vielleicht als `?view=sql-server-2017`.<br/>Die Versionskontrolle über die Dropdownliste würde dann auf den passenden Monikernamen gesetzt werden. |
| `sql-server-2016` | Das Versionierungssystem würde erkennen, dass die Version 2016 im Monikerbereich des Artikels nicht enthalten ist.<br/><br/>Dann würde vom System ein Moniker gewählt werden, der den Bereich erfüllt.<br/><br/>Dann würde der Parameter `?view=` – wie im Fall der Version 2016 – angehängt, und der Steuerungsname würde dem Parameterwert entsprechen. |
| `sql-server-2017` | Das Versionierungssystem erkennt, dass der Parameterwert im Monikerbereich des Artikels enthalten ist.<br/><br/>Die Versionskontrolle würde auf den entsprechenden Parameterwert gesetzt werden. |
| `sql-server-2019` | Dasselbe würde für Wert `sql-server-2017` gelten, außer, dass der Parameter und die Versionskontrolle auf 2019 gesetzt werden. |
| &nbsp; | &nbsp; |

### <a name="all-sql---hide-nothing-special-moniker"></a><a name="anchor-allsql-hidenothing"></a> Alle SQL-Produkte: Nichts ausblenden, spezieller Moniker

Es gibt einen speziellen Moniker für **Alle SQL-Produkte**, und seine einzige Version lautet **Nichts ausblenden**. Dieser Moniker dient internen Testzwecken und ist zur Überprüfung bestimmter Änderungen bestimmt. Wenn dieser Moniker von einem Kunden verwendet wird, sind die angezeigten Inhalte wahrscheinlich eher irreführend als informativ.

Einige Artikel enthalten Informationen zu mehreren Versionen von SQL Server. Jeder reguläre Moniker blendet versionierte Abschnitte aus, in denen ggf. Informationen enthalten sind, die für die Monikerversion ungenau, verwirrend oder widersprüchlich sind. Der spezielle Moniker für **Alle SQL-Produkte** zeigt alle Versionsabschnitte an, und es ist möglicherweise nicht ersichtlich, dass ggf. falsche Informationen angezeigt werden.

## <a name="message-the-requested-page-is-not-available-for-moniker"></a><a name="anchor-message-unavailable-for-moniker"></a> Meldung: Die angeforderte Seite ist für \<moniker\> nicht verfügbar

Das folgende Szenario führt zur Anzeige einer Informationsmeldung am oberen Rand der :::no-loc text="Docs":::-Webseite:

1. Derzeit lautet der Versionierungsmoniker **SQL Server 2017**.
2. Sie lesen einen Artikel, der für SQL Server 2017 bestimmt ist.
    - Für das Produkt Azure SQL-Datenbank ist der Artikel _nicht_ relevant.
3. Sie versuchen, den Moniker auf **Azure SQL Database - current (Azure SQL-Datenbank – aktuell)** zu ändern.
4. Ihr Versuch wurde abgelehnt, und es wird eine Meldung angezeigt.

Am Ende dieses Szenarios wird die folgende Informationsmeldung oben auf der Docs-Webseite angezeigt:

> The requested page is not available for Azure SQL Database - current. You have been redirected to the newest product version this page is available for. (Die angeforderte Seite ist für „Azure SQL-Datenbank – aktuell“ nicht verfügbar. Sie wurden zur neuesten Produktversion umgeleitet, für die diese Seite verfügbar ist.)

Die _neueste_ Version kann Versionen ausschließen, die noch nicht vollständig freigegeben sind und nur als _Vorschauversion_ vorliegen.

![media_versioning-control-30-viewfallbackfrom.png](media/versioning-control-30-viewfallbackfrom.png)

## <a name="previous-versions-of-sql-server"></a>Frühere Versionen von SQL Server

Das Versionierungssystem ist ab SQL Server-Version 2016 vollständig implementiert.

- _2012 und früher:_ &nbsp; Das Versionierungssystem wird nicht für SQL Server 2012 oder frühere Versionen verwendet.
    - Der spezielle Moniker **SQL Server - older (SQL Server – älter)** dient dazu, fast alle Artikel auszublenden. Die seltenen Ausnahmen sind ein paar Artikel, die Kunden älterer Versionen möglicherweise einmal benötigen.
    - [Vorherige Versionen von SQL Server, 2012-2005](./previous-versions-sql-server.md)

- _2014:_ &nbsp; Das Versionierungssystem ist für SQL Server 2014 teilweise implementiert. Daher ist es möglich, auch SQL Server 2014 in der Versionierungskontrolle auswählen. Intern beziehen sich die Dateien für 2014 jedoch nur auf 2014, so wie die Dateien für 2008 nur auf 2008.
    - [Offlinedokumentation für SQL Server 2014](./sql-server-offline-documentation.md)

- _2016 und höher:_ &nbsp; Das Versionierungssystem ist für SQL Server 2016 und höhere Versionen vollständig implementiert.
    - [Willkommen bei der Dokumentation für SQL Server 2016 and höhere Versionen](./index.yml?preserve-view=true&view=sql-server-2016)
    - [Offlinedokumentation für SQL Server 2016](sql-server-offline-documentation.md)

## <a name="see-also"></a>Weitere Informationen

[Vorherige Versionen von SQL Server, 2014-2005](./previous-versions-sql-server.md)  
[Navigationsleitfaden zur SQL Server-Dokumentation](sql-docs-navigation-guide.md)  
[Mitwirken an der SQL Server-Dokumentation](sql-server-docs-contribute.md)