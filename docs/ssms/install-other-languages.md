---
title: Installieren nicht englischsprachiger Versionen
description: Installieren von nicht englischsprachigen Versionen von SQL Server Management Studio (SSMS). Dieser Artikel gilt für SSMS 17.x.
ms.prod: sql
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 04/25/2019
ms.openlocfilehash: 8a70ef94e5491a807414abbd69b1f8ef71b9ca7d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064895"
---
# <a name="install-non-english-language-versions-of-sql-server-management-studio-ssms"></a>Installieren von nicht englischsprachigen Versionen von SQL Server Management Studio (SSMS)

SSMS ist in einer Reihe von Sprachen erhältlich, jedoch blockiert das SSMS-Installationsprogramm die Installation auf Computern, wenn deren Systemgebietsschema nicht mit der Sprache von SSMS übereinstimmt.

> [!NOTE]
> Dieser Artikel gilt für SSMS 17.x. Für SSMS 18.x wurde das Blockieren für das Einrichten gemischter Sprachen entfernt, und Sie können nun beispielsweise die deutsche Version von SSMS in einer französischen Version von Windows installieren. Stimmt die Sprache des Betriebssystems nicht mit der Sprache von SSMS überein, legen Sie die Sprache unter **Tools** > **Optionen** > **Internationale Einstellungen** fest. Andernfalls wird in SSMS die englische Benutzeroberfläche angezeigt.

Die folgenden Anweisungen unterscheiden sich je nach der verwendeten Windows-Version. Die hier folgenden beziehen sich auf Windows 10.

## <a name="install-non-english-ssms-on-a-computer-running-an-english-operating-system-os"></a>Installieren von SSMS in einer anderen Sprache auf einem Computer, der ein englischsprachiges Betriebssystem (OS) ausführt

1. Installieren Sie das Windows-Sprachpaket für die Sprache, die SSMS verwenden soll:
   - **Einstellungen** > **Zeit und Sprache** > **Region und Sprache** > **Sprache hinzufügen**
2. Legen Sie jetzt das Gebietsschema des Systems so fest, dass das im vorhergehenden Schritt installierte Sprachpaket verwendet wird, indem Sie auf die soeben installierte Sprache klicken, und wählen Sie dann **Als Standard festlegen** aus. (Nach dem Installieren von SSMS können Sie das Gebietsschema des Systems wieder auf Englisch zurücksetzen.)
3. Nachdem das Betriebssystem in der gewünschten Sprache ausgeführt wird, installieren Sie die SSMS-Version der gleichen Sprache. Verwenden Sie bei der erstmaligen Installation einer neuen SSMS-Sprache das vollständige Paket. Das Upgradepaket können Sie für nachfolgende Installationsvorgänge verwenden.
4. Führen Sie SSMS aus; es sollte in der Sprache angezeigt werden, die Sie im vorhergehenden Schritt installiert haben.
5. Legen Sie das Systemgebietsschema Ihres Computers wieder auf Englisch fest.

## <a name="install-ssms-in-a-language-other-than-the-language-of-the-installed-os"></a>Installieren von SSMS in einer anderen Sprache als der des installierten Betriebssystems

1. Installieren Sie das Windows-Sprachpaket für die Sprache, die SSMS verwenden soll:
   - **Einstellungen** > **Zeit und Sprache** > **Region und Sprache** > **Sprache hinzufügen**
2. Legen Sie jetzt das Gebietsschema des Systems so fest, dass das im vorhergehenden Schritt installierte Sprachpaket verwendet wird, indem Sie auf die soeben installierte Sprache klicken, und wählen Sie dann **Als Standard festlegen** aus.
3. Nachdem das Betriebssystem in der gewünschten Sprache ausgeführt wird, installieren Sie die SSMS-Version der gleichen Sprache. Verwenden Sie bei der erstmaligen Installation einer neuen SSMS-Sprache das vollständige Paket. Das Upgradepaket können Sie für nachfolgende Installationsvorgänge verwenden.
4. Für jede Sprache, die Sie installieren möchten, die nicht mit der Sprache der installierten ersten Version von SSMS übereinstimmt, installieren Sie das entsprechende Sprachpaket für Visual Studio 2015 Shell (Isoliert):
   - Navigieren Sie zu [https://connect.microsoft.com/VisualStudio/ExtendVS](https://connect.microsoft.com/VisualStudio/ExtendVS). Sie müssen sich möglicherweise anmelden und den Prozess zur *Connect-Registrierung* abschließen.
   - Laden Sie das gewünschte Sprachpaket für Visual Studio 2015 Shell (Isoliert) herunter, und installieren Sie es.

   > [!IMPORTANT]
   > Befolgen Sie die vorstehenden Schritte, um das Sprachpaket für Visual Studio 2015 Shell (Isoliert) zu installieren, verwenden Sie nicht den Link **Zusätzliche Sprachen abrufen** unter **Extras** | **Optionen** | **Internationale Einstellungen**.

5. Führen Sie SSMS aus, und wählen Sie hier die Sprache aus, die Sie verwenden möchten:
   - **Extras** | **Optionen** | **Internationale Einstellungen**
6. Schließen Sie SSMS, und starten Sie es erneut.

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: SQL Server Management Studio](./quickstarts/ssms-connect-query-sql-server.md)