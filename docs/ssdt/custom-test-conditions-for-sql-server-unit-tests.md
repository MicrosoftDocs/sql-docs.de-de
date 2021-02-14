---
title: Benutzerdefinierte Testbedingungen für SQL Server-Komponententests
description: Hier erfahren Sie, wie Sie benutzerdefinierte Testbedingungen für SQL Server-Komponententests installieren. Sehen Sie sich dabei die mit dem Installieren von nicht von Ihnen erstellten Testbedingungen verbundenen Risiken an.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 32a15d61-e908-4ae1-a238-4fd0f988d8c8
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d7935744497dddce8cb2c9ce75566a9da8e13b42
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077732"
---
# <a name="custom-test-conditions-for-sql-server-unit-tests"></a>Benutzerdefinierte Testbedingungen für SQL Server-Komponententests

Sie können benutzerdefinierte Testbedingungen für SQL Server-Komponententests hinzufügen. Bevor die Testbedingung verwendet werden kann, muss sie jedoch installiert werden, unabhängig davon, ob die zu installierende Erweiterung von Ihnen oder einer anderen Person erstellt wurde.  
  
Vor der Installation einer Testbedingung, die nicht von Ihnen erstellt wurde, sollten Sie sich die folgenden Risiken vergegenwärtigen:  
  
-   Das Installationsprogramm für die Testbedingung könnte Schadsoftware sein, die je nach Ihren Installationsberechtigungen Zugriff auf geschützte Ressourcen erlangen könnte.  
  
-   Auch die Testbedingung kann schädlich sein und Kontrolle über geschützte Ressourcen erlangen, sofern der Benutzer, der die Erweiterung verwendet, über ausreichende Berechtigungen verfügt.  
  
Um das Risiko zu minimieren, sollten Sie nur benutzerdefinierte Testbedingungen aus bekannten Quellen installieren. Falls Sie eine Testbedingung aus einer nicht vertrauenswürdigen Quelle erhalten, sollten Sie den Quellcode der Testbedingung sowie deren Installationsprogramm (falls vorhanden) vor der Installation und Ausführung überprüfen.  
  
Zum Installieren einer benutzerdefinierten Testbedingung kopieren Sie die signierte Assembly (DLL) in den Ordner „%Programme%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions“. Wenn der Ordner nicht vorhanden ist, erstellen Sie ihn. Sie müssen auf Ihrem Computer über Administratorprivilegien verfügen, um Daten in dieses Verzeichnis zu kopieren.  
  
> [!NOTE]  
> In folgenden Fällen müssen Sie die Visual Studio 2010- und Visual Studio 2012-Version der Testbedingung installieren:  
>   
> -   Sie installieren benutzerdefinierte Testbedingungen auf einem Computer der u.U. zum Erstellen von SQL Server-Komponententests verwendet wird.  
> -   Die Komponententests werden in Visual Studio 2010 und Visual Studio 2012 verwendet.  
  
Weitere Informationen zu benutzerdefinierten Testbedingungen für SQL Server-Komponententests finden Sie unter:  
  
-   [Vorgehensweise: Erstellen von Testbedingungen für den SQL Server-Komponententest-Designer](../ssdt/how-to-create-test-conditions-for-the-sql-server-unit-test-designer.md)  
  
-   [Vorgehensweise: Durchführen eines Upgrades für eine benutzerdefinierte Visual Studio 2010-Testbedingung von einer älteren Version auf SQL Server Data Tools](../ssdt/how-to-upgrade-visual-studio-2010-custom-test-condition-to-ssdt.md)  
  
-   [Exemplarische Vorgehensweise: Verwenden einer benutzerdefinierten Testbedingung zur Überprüfung der Ergebnisse einer gespeicherten Prozedur](../ssdt/walkthrough-use-custom-test-condition-to-verify-stored-procedure-results.md)  
  
## <a name="see-also"></a>Weitere Informationen  
[Überprüfen des Datenbankcodes mithilfe von SQL Server-Komponententests](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
  
