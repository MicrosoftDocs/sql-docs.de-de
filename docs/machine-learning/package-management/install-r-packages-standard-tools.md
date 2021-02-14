---
title: Installieren von Paketen mit R-Tools
description: Erfahren Sie, wie Sie mit R-Standardtools neue R-Pakete in einer Instanz von SQL Server Machine Learning Services oder SQL Server R Services installieren können.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
monikerRange: =sql-server-2016||=sql-server-2017
ms.openlocfilehash: e54df6240cf1bf4dcaee7813eaf614a84c31a0f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082621"
---
# <a name="install-packages-with-r-tools"></a>Installieren von Paketen mit R-Tools

[!INCLUDE [SQL Server 2016 and 2017 only](../../includes/applies-to-version/sqlserver2016-2017-only.md)]

In diesem Artikel erfahren Sie, wie Sie mit R-Standardtools neue R-Pakete in einer Instanz von SQL Server Machine Learning Services oder SQL Server R Services installieren können. Sie können Pakete in einer SQL Server-Instanz installieren, die über eine Internetverbindung verfügt, sowie in einer vom Internet isolierten Instanz.

Zusätzlich zu den R-Standardtools können Sie R-Pakete installieren mit:

+ [RevoScaleR](install-r-packages-with-revoscaler.md)
::: moniker range="=sql-server-2017"
+ [T-SQL](install-r-packages-with-tsql.md) (CREATE EXTERNAL LIBRARY)
::: moniker-end

## <a name="general-considerations"></a>Allgemeine Hinweise

+ In SQL Server ausgeführter R-Code kann nur Pakete verwenden, die in der Standardinstanzbibliothek installiert sind. SQL Server kann auch dann keine Pakete aus externen Bibliotheken laden, wenn sich die entsprechenden Bibliotheken auf demselben Computer befinden.
Das gilt auch für R-Bibliotheken, die in anderen Microsoft-Produkten installiert sind.

+ Die R-Paketbibliothek befindet sich im Ordner „Programme“ Ihrer SQL Server-Instanz, und für die Installation in diesem Ordner sind standardmäßig Administratorberechtigungen erforderlich. Weitere Informationen finden Sie unter [Speicherort der Paketbibliothek](../package-management/r-package-information.md#default-r-library-location).

  ::: moniker range="=sql-server-2017"
  Nicht-Administratoren können Pakete mit RevoScaleR 9.0.1 und höher oder CREATE EXTERNAL LIBRARY installieren. Der Benutzer **dbo_owner** oder ein Benutzer mit der Berechtigung CREATE EXTERNAL LIBRARY kann R-Pakete in die aktuelle Datenbank installieren. Weitere Informationen finden Sie unter
  + [Verwenden von RevoScaleR zum Installieren von R-Paketen](install-r-packages-with-revoscaler.md)
  + [Verwenden von T-SQL (CREATE EXTERNAL LIBRARY) zum Installieren von R-Paketen in SQL Server](install-r-packages-with-tsql.md)
  ::: moniker-end

  ::: moniker range="=sql-server-2016"
  Nicht-Administratoren können Pakete mithilfe von RevoScaleR 9.0.1 und höher installieren. Der Benutzer **dbo_owner** kann R-Pakete in die aktuelle Datenbank installieren. Weitere Informationen finden Sie unter [Verwenden von RevoScaleR zum Installieren von R-Paketen](install-r-packages-with-revoscaler.md).
  ::: moniker-end

+ In einer gesicherten SQL Server-Umgebung sollten Sie Folgendes vermeiden:
  + Pakete, für die Netzwerkzugriff erforderlich ist
  + Pakete, für die erhöhte Zugriffsrechte für das Dateisystem erforderlich sind
  + Pakete, die für die Webentwicklung oder andere Aufgaben verwendet werden und nicht von der Ausführung in SQL Server profitieren

## <a name="online-installation-with-internet-access"></a>Online-Installation (mit Internetzugriff)

Wenn die SQL Server-Instanz Zugriff auf das Internet hat, können Sie R-Pakete mit Standardtools zur Paketinstallation installieren.

1. Bestimmen Sie den Speicherort der Instanzbibliothek (weitere Informationen finden Sie unter [ Abrufen von Informationen zu R-Paketen](../package-management/r-package-information.md)), und navigieren Sie zum Ordner, in dem die R-Tools installiert sind.

   ::: moniker range="=sql-server-2016"
   Der Standardpfad einer Standardinstanz von SQL Server lautet z. B. wie folgt:

   `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   Der Standardpfad einer Standardinstanz von SQL Server lautet z. B. wie folgt:

   `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

1. Führen Sie **R** oder **Rgui** in diesem Ordner als Administrator aus.

1. Führen Sie den R-Befehl `install.packages` aus, und geben Sie den Paketnamen an. Wenn das Paket Abhängigkeiten aufweist, lädt das Installationsprogramm diese automatisch herunter und installiert sie.

Wenn Sie über mehrere parallele Instanzen von SQL Server verfügen, führen Sie die Installation für jede Instanz, in der Sie das Paket verwenden möchten, separat aus. Pakete können nicht von mehreren Instanzen gemeinsam genutzt werden.

## <a name="offline-installation-no-internet-access"></a><a name = "bkmk_offlineInstall"></a> Offline-Installation (ohne Internetzugriff)

Häufig haben Server, die Produktionsdatenbanken hosten, keine Internetverbindung. Um R-Pakete in dieser Umgebung zu installieren, müssen Sie Pakete und Abhängigkeiten im Voraus (als ZIP-Dateien) herunterladen und vorbereiten und dann die Dateien in einen Ordner auf dem Server kopieren. Sobald die Dateien an Ort und Stelle sind, können die Pakete offline installiert werden.

Das Ermitteln aller Abhängigkeiten ist kompliziert. Für R empfehlen wir, mit [**miniCRAN**](https://andrie.github.io/miniCRAN/) ein lokales Repository zu erstellen.
**miniCRAN** verwendet eine Liste der Pakete, die Sie installieren möchten, analysiert Abhängigkeiten und stellt alle notwendigen ZIP-Dateien zusammen. Anschließend erstellt das Tool ein einzelnes Repository, das Sie in die isolierte SQL Server-Instanz kopieren können. Das Paket [**igraph**](https://igraph.org/r/) ist auch bei der Analyse von Paketabhängigkeiten hilfreich.

Weitere Informationen finden Sie im Artikel zum Thema [Erstellen eines lokalen R-Paketrepositorys mit miniCRAN](create-a-local-package-repository-using-minicran.md).

Sobald sich die ZIP-Datei in der SQL Server-Instanz befindet, können Sie sie mithilfe von R-Standardtools auf dem Server installieren.

1. Bestimmen Sie den Speicherort der Instanzbibliothek (weitere Informationen finden Sie unter [ Abrufen von Informationen zu R-Paketen](../package-management/r-package-information.md)), und navigieren Sie zum Ordner, in dem die R-Tools installiert sind. 

   ::: moniker range="=sql-server-2016"
   Der Standardpfad einer Standardinstanz von SQL Server lautet z. B. wie folgt:

   `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   Der Standardpfad einer Standardinstanz von SQL Server lautet z. B. wie folgt:

   `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64\`
   ::: moniker-end

1. Führen Sie **R** oder **Rgui** in diesem Ordner als Administrator aus.

1. Führen Sie den R-Befehl `install.packages` aus, und geben Sie den Paket- oder Repositorynamen und den Speicherort der ZIP-Dateien an. Beispiel:

   ```R
   install.packages("C:\\Temp\\Downloaded packages\\mynewpackage.zip", repos=NULL)
   ```

   Dieser Befehl extrahiert das R-Paket `mynewpackage` aus der lokalen ZIP-Datei und installiert das Paket. Wenn das Paket Abhängigkeiten aufweist, prüft das Installationsprogramm, ob bereits Pakete in der Bibliothek vorhanden sind. Haben Sie ein Repository erstellt, das die Abhängigkeiten enthält, installiert das Installationsprogramm auch die erforderlichen Pakete.

   > [!NOTE]
   > Wenn erforderliche Pakete nicht in der Instanzbibliothek vorhanden sind und nicht in den ZIP-Dateien gefunden werden, schlägt die Installation des Zielpakets fehl.

Alternativ zu **miniCRAN** können Sie diese Schritte auch manuell durchführen:

1. Bestimmen Sie alle Paketabhängigkeiten.
1. Prüfen Sie, ob alle erforderlichen Pakete bereits auf dem Server installiert sind. Wenn das Paket installiert ist, prüfen Sie, ob die Version stimmt.
1. Laden Sie das Paket und alle Abhängigkeiten auf einen separaten Computer mit Internetzugriff herunter.
1. Legen Sie das Paket und die Abhängigkeiten in einem einzelnen Paketarchiv ab.
1. Zippen Sie das Archiv, wenn es nicht bereits in einem komprimierten Format vorliegt.
1. Verschieben Sie die Dateien in einen Ordner, auf den der Server zugreifen kann.
1. Führen Sie einen unterstützten Installationsbefehl oder eine DDL-Anweisung aus, um das Paket in die Instanzbibliothek zu installieren.

## <a name="see-also"></a>Weitere Informationen

+ [Abrufen von R-Paketinformationen](r-package-information.md)
+ [Tipps für die Verwendung von R-Paketen](tips-for-using-r-packages.md)
+ [Tutorials für die Programmiersprache R in SQL Server](../tutorials/r-tutorials.md)