---
title: Erstellen eines Repositorys mithilfe von miniCRAN
titleSuffix: SQL machine learning
description: Erfahren Sie, wie Sie R-Pakete offline installieren, indem Sie das miniCRAN-Paket verwenden, um ein lokales Repository aus Paketen und Abhängigkeiten zu erstellen.
ms.prod: sql
ms.technology: machine-learning
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: f93a1180f25f764330db44306457e775961bc0da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350692"
---
# <a name="create-a-local-r-package-repository-using-minicran"></a>Erstellen eines lokalen R-Paketrepositorys mithilfe von miniCRAN
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

In diesem Artikel erfahren Sie, wie Sie R-Pakete offline installieren, indem Sie [miniCRAN](https://cran.r-project.org/web/packages/miniCRAN/index.html) verwenden, um ein lokales Repository aus Paketen und Abhängigkeiten zu erstellen. **miniCRAN** identifiziert Pakete und Abhängigkeiten und lädt diese in einen einzelnen Ordner herunter, den Sie für die Offlineinstallation von R-Paketen auf andere Computer kopieren.

Sie können ein oder mehrere Pakete angeben, und **miniCRAN** liest die Abhängigkeitsstruktur für diese Pakete rekursiv. Anschließend werden nur die aufgelisteten Pakete und ihre Abhängigkeiten von CRAN oder ähnlichen Repositorys heruntergeladen.

Wenn der Vorgang abgeschlossen ist, erstellt **miniCRAN** ein intern konsistentes Repository, das aus den ausgewählten Paketen und allen erforderlichen Abhängigkeiten besteht. Sie können dieses lokale Repository auf den Server verschieben und mit der Installation der Pakete ohne Internetverbindung fortfahren.

Erfahrene R-Benutzer suchen häufig in der Beschreibungsdatei eines heruntergeladenen Pakets nach der Liste der abhängigen Pakete. Unter **Importe** aufgeführte Pakete könnten jedoch über Abhängigkeiten der zweiten Ebene verfügen. Aus diesem Grund wird **miniCRAN** für die vollständige Sammlung der erforderlichen Pakete empfohlen.

## <a name="why-create-a-local-repository"></a>Wozu ein lokales Repository erstellen?

Das Ziel beim Erstellen eines lokalen Paketrepositorys liegt in der Bereitstellung eines einzelnen Speicherorts, den ein Serveradministrator oder andere Benutzer im Unternehmen zum Installieren neuer R-Pakete auf einem Server verwenden können, insbesondere ohne Internetzugang. Nachdem Sie das Repository erstellt haben, können Sie es ändern, indem Sie neue Pakete hinzufügen oder die Version vorhandener Pakete aktualisieren.

Paketrepositorys sind in folgenden Szenarios nützlich:

- **Sicherheit**: Viele R-Benutzer sind daran gewöhnt, neue R-Pakete bei Bedarf über CRAN oder eine dessen gespiegelten Sites herunterzuladen und zu installieren. Jedoch verfügen Produktionsserver, die [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ausführen, aus Sicherheitsgründen typischerweise über keine Internetverbindung.

- **Einfachere Offlineinstallation**: Zum Installieren eines Pakets auf einem Offlineserver müssen Sie auch alle Paketabhängigkeiten herunterladen. Die Verwendung von miniCRAN vereinfacht das Abrufen aller Abhängigkeiten im richtigen Format und das Vermeiden von Abhängigkeitsfehlern.

- **Verbesserte Versionsverwaltung**: In einer Mehrbenutzerumgebung gibt es gute Gründe dafür, die nicht genehmigte Installation mehrerer Paketversionen auf dem Server zu vermeiden. Verwenden Sie ein lokales Repository, um eine konsistente Gruppe von Paketen für Ihre Benutzer bereitzustellen.

## <a name="install-minicran"></a>Installieren von miniCRAN

Das **miniCRAN**-Paket selbst ist abhängig von 18 anderen CRAN-Paketen. Darunter befindet sich das Paket **RCurl**, das über eine Systemabhängigkeit vom Paket **curl-devel** verfügt. Ebenso verfügt das Paket **XML** über eine Abhängigkeit von **libxml2-devel**. Es wird empfohlen, dass Sie Ihr lokales Repository anfangs auf einem Computer mit vollständigem Internetzugriff erstellen, um Abhängigkeiten zu lösen.

Führen Sie die folgenden Befehle auf einem Computer mit R-Basisfunktionen, R-Tools und Internetverbindung aus. Es wird davon ausgegangen, dass es sich hierbei nicht um Ihren SQL Server-Computer handelt. Mit den folgenden Befehlen werden die Pakete **miniCRAN** und **igraph** installiert. In diesem Beispiel wird überprüft, ob das Paket bereits installiert wurde. Sie können jedoch die `if`-Anweisungen umgehen und die Pakete direkt installieren.

```R
if(!require("miniCRAN")) install.packages("miniCRAN") 
if(!require("igraph")) install.packages("igraph") 
library("miniCRAN")
```

## <a name="set-the-cran-mirror-and-mran-snapshot"></a>Festlegen des CRAN-Spiegels und der MRAN-Momentaufnahme

Geben Sie eine gespiegelte Site an, die zum Herunterladen von Paketen verwendet wird. Beispielsweise können Sie die MRAN-Site oder eine beliebige andere Site in Ihrer Region verwenden, die die erforderlichen Pakete enthält. Wenn bei einem Download ein Fehler auftritt, versuchen Sie es mit einer anderen gespiegelten Site.

```R
CRAN_mirror <- c(CRAN = "https://cran.cnr.berkeley.edu")
```

## <a name="create-a-local-folder"></a>Erstellen eines lokalen Ordners

Erstellen Sie einen lokalen Ordner, in dem die gesammelten Pakete gespeichert werden sollen. Wenn Sie dies oft wiederholen, möchten Sie möglicherweise einen beschreibenden Namen wie „miniCRANZooPackages“ oder „miniCRANMyRPackageV2“ verwenden.

Geben Sie den Ordner als lokales Repository an. Die R-Syntax verwendet im Gegensatz zu den Windows-Konventionen einen Schrägstrich für Pfadnamen.

```R
local_repo <- "C:/miniCRANZooPackages"
```

## <a name="add-packages-to-the-local-repo"></a>Hinzufügen von Paketen zum lokalen Repository

Nachdem **miniCRAN** installiert und geladen wurde, erstellen Sie eine Liste mit den zusätzlichen Paketen, die Sie herunterladen möchten.

Fügen Sie dieser anfänglichen Liste **keine** Abhängigkeiten hinzu. Das von **miniCRAN** verwendete Paket **igraph** generiert die Liste der Abhängigkeiten automatisch. Weitere Informationen zur Verwendung des generierten Abhängigkeitsdiagramms finden Sie unter [Verwenden von miniCRAN zur Identifizierung von Paketabhängigkeiten](https://cran.r-project.org/web/packages/miniCRAN/vignettes/miniCRAN-dependency-graph.html).

1. Fügen Sie die Zielpakete „zoo“ und „forecast“ einer Variablen hinzu.

    ```R
    pkgs_needed <- c("zoo", "forecast")
    ```

2. Optional können Sie das Abhängigkeitsdiagramm erstellen. Dies ist nicht erforderlich, kann aber informativ sein.

    ```R
    plot(makeDepGraph(pkgs_needed))
    ```

3. Erstellen Sie das lokale Repository. Stellen Sie sicher, dass Sie die R-Version falls nötig ändern, damit sie der auf Ihrer SQL Server-Instanz installierten Version entspricht. Wenn Sie ein Komponentenupgrade durchgeführt haben, ist die Version möglicherweise neuer als die ursprüngliche Version. Weitere Informationen finden Sie unter [Abrufen von Paketinformationen für R](../package-management/r-package-information.md).

    ```R
    pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);
    makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.3");
    ```

   Aus diesen Informationen erstellt das miniCRAN-Paket die Ordnerstruktur, die Sie benötigen, um die Pakete später auf [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] zu kopieren.

An diesem Punkt sollten Sie über einen Ordner mit den für Sie erforderlichen Paketen und zusätzlichen erforderlichen Paketen verfügen. Der Ordner sollte eine Sammlung gezippter Pakete enthalten. Entpacken Sie die Pakete nicht, und benennen Sie keine Dateien um.

Führen Sie optional den folgenden Code aus, um die Pakete aufzulisten, die im lokalen miniCRAN-Repository enthalten sind.

```R
pdb <- as.data.frame(pkgAvail(local_repo, type = "win.binary", Rversion = "3.3"), stringsAsFactors = FALSE);
head(pdb);
pdb$Package;
pdb[, c("Package", "Version", "License")]
```

## <a name="add-packages-to-the-instance-library"></a>Hinzufügen von Paketen zur Instanzbibliothek

Nachdem Sie über ein lokales Repository mit den für Sie erforderlichen Paketen verfügen, verschieben Sie das Paketrepository auf den SQL Server-Computer. Im folgenden Verfahren wird beschrieben, wie Sie die Pakete mithilfe von R-Tools installieren.

::: moniker range=">sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current"
> [!NOTE]
> Die empfohlene Methode zum Installieren von Paketen ist die Verwendung von **sqlmlutils**. Weitere Informationen finden Sie unter [Installieren von neuen R-Paketen mit sqlmlutils](install-additional-r-packages-on-sql-server.md).
::: moniker-end

1. Kopieren Sie den Ordner, der das miniCRAN-Repository enthält, vollständig auf den Server, auf dem die Pakete installiert werden sollen. Der Ordner weist in der Regel die folgende Struktur auf: 

   `<miniCRAN root>/bin/windows/contrib/version/<all packages>`

   In diesem Verfahren wird davon ausgegangen, dass es sich um einen Ordner des Stammlaufwerks handelt.

2. Öffnen Sie ein R-Tool, das der Instanz zugeordnet ist (z. B. könnten Sie „Rgui.exe“ verwenden). Klicken Sie mit der rechten Maustaste, und wählen Sie **Als Administrator ausführen** aus, damit das Tool Updates für Ihr System vornehmen kann.

   ::: moniker range="=sql-server-2016"
   - Der Standard-Dateispeicherort für RGUI ist beispielsweise `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64`.
   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   - Der Dateispeicherort für RGUI ist beispielsweise `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64`.
   ::: moniker-end

   ::: moniker range=">sql-server-2017"
   - Der Dateispeicherort für RGUI ist beispielsweise `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES\bin\x64`.
   ::: moniker-end

3. Rufen Sie den Pfad für die Instanzbibliothek ab, und fügen Sie diesen zur Liste der Bibliothekspfade hinzu.

   ::: moniker range="=sql-server-2016"
   Beispiel:

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL13.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

   ::: moniker range="=sql-server-2017"
   Beispiel:

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL14.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

   ::: moniker range=">sql-server-2017"
   Beispiel:

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL15.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

4. Geben Sie den neuen Speicherort auf dem Server an, auf den Sie das **miniCRAN**-Repository als `server_repo` kopiert haben.

    In diesem Beispiel wird davon ausgegangen, dass Sie das Repository in einen temporären Ordner auf dem Server kopiert haben.

    ```R
    inputlib <- "C:/miniCRANZooPackages"
    ```

5. Da Sie in einem neuen R-Arbeitsbereich auf dem Server arbeiten, müssen Sie auch die Liste der zu installierenden Pakete bereitstellen.

    ```R
    mypackages <- c("zoo", "forecast")
    ```

6. Installieren Sie die Pakete, und geben Sie dabei den Pfad zur lokalen Kopie des miniCRAN-Repositorys an.

    ```R
    install.packages(mypackages, repos = file.path("file://", normalizePath(inputlib, winslash = "/")), lib = outputlib, type = "win.binary", dependencies = TRUE);
    ```

7. In der Instanzbibliothek können Sie die installierten Pakete mit einem Befehl wie dem folgenden anzeigen:

    ```R
    installed.packages()
    ```

## <a name="see-also"></a>Weitere Informationen

+ [Abrufen von R-Paketinformationen](../package-management/r-package-information.md)
+ [R-Tutorials](../tutorials/r-tutorials.md)