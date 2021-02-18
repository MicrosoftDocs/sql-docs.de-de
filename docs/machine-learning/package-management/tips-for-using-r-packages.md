---
title: Tipps für die Verwendung von R-Paketen
titleSuffix: SQL machine learning
description: Erhalten Sie hilfreiche Tipps zur Verwendung von R-Paketen in SQL Server, wenn Sie sich mit R oder SQL Server noch nicht auskennen.
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/06/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e3dd91e29d88fc2ab6f7216f0bac38632caa7b25
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352515"
---
# <a name="tips-for-using-r-packages"></a>Tipps für die Verwendung von R-Paketen

[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Dieser Artikel bietet hilfreiche Tipps zur Verwendung von R-Paketen in SQL Server. Diese Tipps richten sich an DBAs, die mit R nicht vertraut sind, und erfahrene R-Entwickler, die sich noch nicht mit dem Paketzugriff in einer SQL Server-Instanz auskennen.

## <a name="if-youre-new-to-r"></a>Für R-Einsteiger

Als Administrator, der zum ersten Mal R-Pakete installiert, kann es hilfreich sein, sich mit einigen Grundlagen der R-Paketverwaltung vertraut zu machen.

### <a name="package-dependencies"></a>Paketabhängigkeiten

R-Pakete sind häufig von mehreren anderen Paketen abhängig, von denen einige möglicherweise nicht in der von der Instanz verwendeten R-Standardbibliothek verfügbar sind. Manchmal erfordert ein Paket eine andere Version eines abhängigen Pakets als die bereits installierte Version. Paketabhängigkeiten sind in einer in das Paket eingebetteten DESCRIPTION-Datei vermerkt, aber manchmal unvollständig. Sie können ein Paket mit dem Namen [iGraph](https://igraph.org/r/) verwenden, um das Abhängigkeitsdiagramm vollständig zu formulieren.

Wenn Sie mehrere Pakete installieren müssen oder sicherstellen möchten, dass jeder in Ihrer Organisation den richtigen Pakettyp und die richtige Version erhält, empfiehlt es sich, das Paket [miniCRAN](https://mran.microsoft.com/package/miniCRAN) zu verwenden, um die gesamte Abhängigkeitskette zu analysieren. Von miniCRAN wird ein lokales Repository erstellt, das von mehreren Benutzern oder Computern gemeinsam genutzt werden kann. Weitere Informationen finden Sie im Artikel zum Thema [Erstellen eines lokalen Paketrepositorys mit miniCRAN](create-a-local-package-repository-using-minicran.md).

### <a name="package-sources-versions-and-formats"></a>Paketquellen, -versionen und -formate

Es gibt mehrere Quellen für R-Pakete wie [CRAN](https://cran.r-project.org/) und [Bioconductor](https://www.bioconductor.org/). Auf der offiziellen Website für die R-Sprache (<https://www.r-project.org/>) sind viele dieser Ressourcen aufgeführt. Microsoft bietet [MRAN](https://mran.microsoft.com/) für die Open-Source-Distribution von R ([MRO](https://mran.microsoft.com/open)) und weitere Pakete an. Viele Pakete werden in GitHub veröffentlicht. Dort können Entwickler auch den Quellcode erhalten.

::: moniker range=">=sql-server-2016"
R-Pakete werden auf mehreren Computerplattformen ausgeführt. Stellen Sie sicher, dass es sich bei den installierten Versionen um Windows-Binärdateien handelt.
::: moniker-end

::: moniker range=">=sql-server-linux-ver15"
R-Pakete werden auf mehreren Computerplattformen ausgeführt. Stellen Sie sicher, dass es sich bei den installierten Versionen um Linux-Binärdateien handelt.
::: moniker-end

### <a name="know-which-library-youre-installing-to-and-which-packages-are-already-installed"></a>Informieren Sie sich, in welcher Bibliothek Sie die Installation durchführen und welche Pakete bereits installiert sind.

Wenn Sie die R-Umgebung zuvor auf dem Computer geändert haben, sollten Sie vor der Installation sicherstellen, dass die R-Umgebungsvariable `.libPath` nur einen Pfad verwendet.

Dieser Pfad sollte auf den Ordner „R_SERVICES“ für die Instanz verweisen. Weitere Informationen und Details dazu, wie Sie ermitteln, welche Pakete bereits installiert sind, finden Sie unter [Abrufen von Paketinformationen für R](../package-management/r-package-information.md).

## <a name="if-youre-new-to-sql-server"></a>Für SQL Server-Einsteiger

Als R-Entwickler, der an Code für die Ausführung in SQL Server arbeitet, haben Sie aufgrund der Sicherheitsrichtlinien zum Schutz des Servers nur eingeschränkte Möglichkeiten, um die R-Umgebung zu steuern. In den folgenden Tipps werden typische Situationen beschrieben, und Sie erhalten Vorschläge für die Arbeit in dieser Umgebung.

### <a name="r-user-libraries-not-supported-on-sql-server"></a>R-Benutzerbibliotheken: in SQL Server nicht unterstützt

R-Entwickler, die neue R-Pakete installieren müssen, sind es gewohnt, Pakete frei zu installieren. Dazu verwenden sie eine private Benutzerbibliothek, wenn die Standardbibliothek nicht verfügbar ist oder wenn der Entwickler auf dem Computer nicht über Administratorrechte verfügt. In einer typischen R-Entwicklungsumgebung müsste der Benutzer zum Beispiel den Speicherort des Pakets zur R-Umgebungsvariablen `libPath` hinzufügen oder wie folgt auf den vollständigen Paketpfad verweisen:

```R
library("c:/Users/<username>/R/win-library/packagename")
```

Dies funktioniert nicht, wenn R-Lösungen in SQL Server ausgeführt werden, da R-Pakete in einer bestimmten Standardbibliothek installiert werden müssen, die der Instanz zugeordnet ist. Wenn ein Paket nicht in der Standardbibliothek installiert wurde, erhalten Sie diese Fehlermeldung, wenn Sie versuchen, das Paket aufzurufen:

*Error in library(xxx) : there is no package called 'package-name'* (Fehler in Bibliothek(xxx) : kein Paket 'Paketname' vorhanden)

Informationen zum Installieren von R-Paketen in SQL Server finden Sie unter [Installieren neuer R-Pakete in SQL Server Machine Learning Services oder SQL Server R Services](install-additional-r-packages-on-sql-server.md).

### <a name="how-to-avoid-package-not-found-errors"></a>Vermeiden von Fehlern durch nicht gefundene Pakete

Mithilfe der folgenden Richtlinien können Sie Fehler vom Typ „Paket nicht gefunden“ vermeiden.

+ Beseitigen Sie Abhängigkeiten von Benutzerbibliotheken.

    Es ist keine gute Entwicklungsmethode, erforderliche R-Pakete in einer individuellen Benutzerbibliothek zu installieren. Dies kann zu Fehlern führen, wenn eine Lösung von einem anderen Benutzer ausgeführt wird, der keinen Zugriff auf den Speicherort der Bibliothek hat.

    Auch wenn ein Paket in der Standardbibliothek installiert wurde, lädt die R-Laufzeit das Paket aus der Standardbibliothek, selbst wenn Sie im R-Code eine andere Version angeben.

+ Stellen Sie sicher, dass Ihr Code in einer gemeinsam genutzten Umgebung ausgeführt werden kann.

+ Vermeiden Sie das Installieren von Paketen als Teil einer Lösung. Wenn Sie keine Berechtigungen zum Installieren von Paketen haben, kann der Code nicht ausgeführt werden. Selbst wenn Sie über Installationsberechtigungen für Pakete verfügen, sollten Sie diese getrennt von anderem Code, den Sie ausführen möchten, installieren.

+ Stellen Sie sicher, dass im Code keine Aufrufe von nicht installierten Paketen enthalten sind.

+ Aktualisieren Sie Ihren Code, um direkte Verweise auf die Pfade von R-Paketen oder R-Bibliotheken zu entfernen.

+ Informieren Sie sich darüber, welche Paketbibliothek der Instanz zugeordnet ist. Weitere Informationen finden Sie unter [Abrufen von Paketinformationen für R](../package-management/r-package-information.md).

## <a name="see-also"></a>Siehe auch

::: moniker range="<=sql-server-2017"
+ [Installieren von Paketen mit R-Tools](install-r-packages-standard-tools.md)
::: moniker-end
::: moniker range=">sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current"
+ [Installieren von neuen R-Paketen mit sqlmlutils](install-additional-r-packages-on-sql-server.md)
::: moniker-end
