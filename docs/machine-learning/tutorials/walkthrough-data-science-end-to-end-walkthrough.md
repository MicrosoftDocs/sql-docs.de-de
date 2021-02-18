---
title: 'R-Tutorial: Entwickeln eines Modells in SQL'
description: Erfahren Sie, wie Sie eine umfassende Lösung für die Vorhersagemodellierung auf Grundlage der R-Unterstützung in SQL Server 2016 oder SQL Server 2017 erstellen.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/11/2019
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: d9bfe9ec4f81ca61bacbb4dc3526ea5b7a1a87f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272961"
---
# <a name="tutorial-sql-development-for-r-data-scientists"></a>Tutorial: SQL-Entwicklung für R-Data-Scientists
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

In diesem Tutorial für Data Scientists erfahren Sie, wie Sie eine umfassende Lösung für die Vorhersagemodellierung auf Grundlage der R-Unterstützung in SQL Server 2016 oder SQL Server 2017 erstellen. Für dieses Tutorial wird die Datenbank [NYCTaxi_sample](demo-data-nyctaxi-in-sql.md) in SQL Server verwendet. 

Sie verwenden eine Kombination aus R-Code, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Daten und benutzerdefinierten SQL-Funktionen, um ein Klassifikationsmodell zu erstellen, das die Wahrscheinlichkeit angibt, dass der Fahrer ein Trinkgeld für eine bestimmte Taxifahrt erhält. Sie stellen außerdem Ihr R-Modell für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bereit und verwenden Serverdaten, um Bewertungen basierend auf dem Modell zu erstellen.

Dieses Beispiel kann einfach auf alle möglichen realen Situationen übertragen werden, wie die Vorhersage von Reaktionen von Kunden auf Verkaufsaktionen oder die Ausgaben oder Teilnehmerzahlen bei Veranstaltungen. Da das Modell aus einer gespeicherten Prozedur aufgerufen werden kann, können Sie es auch in eine Anwendung einbetten.

Da die exemplarische Vorgehensweise der Einführung von R-Entwicklern in [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] dient, wird R so häufig wie möglich verwendet. Das bedeutet jedoch nicht, dass R zwangsläufig das beste Tool für die einzelnen Aufgaben ist. In vielen Fällen stellt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eine bessere Leistung bereit, besonders für Aufgaben wie Datenaggregation und Featureentwicklung.  Solche Aufgaben profitieren besonders von neuen Funktionen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], wie z.B. von speicheroptimierten Columnstore-Indizes. Nebenbei wird auf mögliche Optimierungen hingewiesen.

## <a name="prerequisites"></a>Voraussetzungen

+ [SQL Server Machine Learning Services mit R-Integration](../install/sql-machine-learning-services-windows-install.md#verify-installation) oder [SQL Server 2016 R Services](../install/sql-r-services-windows-install.md)

+ [Datenbankberechtigungen](../security/user-permission.md) und eine Benutzeranmeldung für SQL Server-Datenbank

+ [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md)

+ [Demodatenbank für Taxifahrten in New York City](demo-data-nyctaxi-in-sql.md)

+ Eine R-IDE wie z. B. RStudio oder das in R enthaltene integrierte RGUI-Tool

Es wird empfohlen, diese exemplarische Vorgehensweise an einer Clientarbeitsstation durchzuführen. Sie müssen eine Verbindung zu einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Computer im selben Netzwerk herstellen können, auf dem SQL Server und R aktiviert sind. Anweisungen zur Konfiguration der Arbeitsstation finden Sie unter [Einrichten eines Data-Science-Clients für die Entwicklung in R](../r/set-up-a-data-science-client.md).

Alternativ können Sie die exemplarische Vorgehensweise auf einem Computer durchführen, auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und eine R-Entwicklungsumgebung vorhanden sind. Diese Konfiguration wird jedoch für eine Produktionsumgebung nicht empfohlen. Befinden sich Client und Server notwendigerweise auf demselben Computer, müssen Sie einen zweiten Satz Microsoft R-Bibliotheken installieren, um R-Skripts von einem Remoteclient senden zu können. Verwenden Sie nicht die R-Bibliotheken, die in der SQL Server-Instanz unter „Programme“ installiert sind. Insbesondere, wenn Sie nur einen Computer verwenden, benötigen Sie die RevoScaleR-Bibliothek an beiden der folgenden Speicherorten, um Client- und Servervorgänge zu unterstützen:

+ C:\Programme\Microsoft\R Client\R_SERVER\library\RevoScaleR 
+ C:\Programme\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR

> [!NOTE]
> Wenn Sie [Machine Learning Server](/machine-learning-server/) oder [Data Science Virtual Machine](/azure/machine-learning/data-science-virtual-machine/) anstelle des R-Clients verwenden, lautet der Pfad zu RevoScaleR C:\Programme\Microsoft\ML Server\R_SERVER\library\RevoScaleR.

<a name="add-packages"></a>

## <a name="additional-r-packages"></a>Zusätzliche R-Pakete

Für diese exemplarische Vorgehensweise sind mehrere R-Bibliotheken erforderlich, die nicht standardmäßig als Teil von [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] installiert sind. Sie müssen die Pakete jeweils auf dem Client installieren, auf dem Sie die Lösung entwickeln, und auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Computer, auf dem Sie die Lösung bereitstellen.

### <a name="on-a-client-workstation"></a>Auf einer Clientarbeitsstation

Kopieren Sie in Ihrer R-Umgebung folgende Zeilen, und führen Sie den Code in einem Konsolenfenster (in RGUI oder einer IDE) aus. Von einigen Paketen werden auch erforderliche Pakete installiert. Insgesamt werden etwa 32 Pakete installiert. Für diesen Schritt benötigen Sie eine Internetverbindung.
    
  ```R
  # Install required R libraries, if they are not already installed.
  if (!('ggmap' %in% rownames(installed.packages()))){install.packages('ggmap')}
  if (!('mapproj' %in% rownames(installed.packages()))){install.packages('mapproj')}
  if (!('ROCR' %in% rownames(installed.packages()))){install.packages('ROCR')}
  if (!('RODBC' %in% rownames(installed.packages()))){install.packages('RODBC')}
  ```

### <a name="on-the-server"></a>Auf dem Server

Es gibt mehrere Möglichkeiten, Pakete in SQL Server zu installieren. SQL Server bietet beispielsweise ein Feature für die [Verwaltung von R-Paketen](../package-management/install-additional-r-packages-on-sql-server.md), mit dem Datenbankadministratoren ein Paketrepository erstellen und Benutzern die Rechte erteilen können, eigene Pakete zu installieren. Wenn Sie jedoch Administrator auf dem Computer sind, können Sie neue Pakete mit R installieren, solange Sie diese in der richtigen Bibliothek installieren.

> [!NOTE]
> Führen Sie auf dem Server **keine** Installation in einer Benutzerbibliothek aus, auch wenn Sie dazu aufgefordert werden. Wenn Sie eine Installation in einer Benutzerbibliothek durchführen, kann die SQL Server-Instanz die Pakete nicht finden und ausführen. Weitere Informationen finden Sie unter [Installieren neuer R-Pakete in SQL Server](../package-management/install-additional-r-packages-on-sql-server.md).

1. Öffnen Sie auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Computer **als Administrator** RGui.exe.  Wenn Sie SQL Server R Services mithilfe der Standardeinstellungen installiert haben, finden Sie „Rgui.exe“ unter C:\Programme\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64).

2. Führen Sie an einer R-Eingabeaufforderung die folgenden R-Befehle aus:
  
  ```R
  install.packages("ggmap", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  install.packages("mapproj", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  install.packages("ROCR", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  install.packages("RODBC", lib=grep("Program Files", .libPaths(), value=TRUE)[1])
  ```
  In diesem Beispiel wird die grep-Funktion in R verwendet, um den Vektor der verfügbaren Pfade zu durchsuchen und den Pfad zu finden, der „Programme“ enthält. Weitere Informationen finden Sie unter [https://www.rdocumentation.org/packages/base/functions/grep](https://www.rdocumentation.org/packages/base/functions/grep).

  Wenn Sie sicher sind, dass die Pakete bereits installiert wurden, überprüfen Sie die Liste der installierten Pakete, indem Sie `installed.packages()` ausführen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Durchsuchen und Zusammenfassen der Daten](walkthrough-view-and-summarize-data-using-r.md)