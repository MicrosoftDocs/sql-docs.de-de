---
description: Programmgesteuertes Erstellen von Paketen
title: Programmgesteuertes Erstellen von Paketen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
ms.assetid: 7474b1f4-7607-4f28-a6fd-67f7db1dd3f8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: daf54aa01dcea6d5c63fba21a3b4ffaf61f2cdd9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351013"
---
# <a name="building-packages-programmatically"></a>Programmgesteuertes Erstellen von Paketen

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Wenn Sie Pakete dynamisch erstellen oder [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Pakete außerhalb der Entwicklungsumgebung verwalten und ausführen müssen, können Sie Pakete programmgesteuert ändern. Dieser Ansatz bietet Ihnen eine breite Palette von Optionen:  
  
-   Laden und Ausführen eines vorhandenen Pakets ohne Änderung  
  
-   Laden, Neukonfigurieren (z. B. für eine andere Datenquelle) und Ausführen eines vorhandenen Pakets  
  
-   Erstellen eines neuen Pakets, Hinzufügen und Konfigurieren von Komponenten Objekt um Objekt und Eigenschaft um Eigenschaft, Speichern und Ausführen des Pakets  
  
 Sie können das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Objektmodell verwenden, um in einer beliebigen verwalteten Programmiersprache Code zu schreiben, mit dem Pakete erstellt, konfiguriert und ausgeführt werden. Möglicherweise möchten Sie metadatengesteuerte Pakete erstellen, die ihre Verbindungen oder Datenquellen, Transformationen und Ziele basierend auf der gewählten Datenquelle und ihren Tabellen und Spalten konfigurieren.  
  
 In diesem Abschnitt wird beschrieben und veranschaulicht, wie Pakete programmgesteuert Zeile um Zeile erstellt und konfiguriert werden. Als einfache Möglichkeit der Paketprogrammierung bietet es sich an, ein vorhandenes Paket ohne Änderungen zu laden und auszuführen. Dies wird unter [Programmgesteuerte Ausführung und Verwaltung von Paketen](../../integration-services/run-manage-packages-programmatically/running-and-managing-packages-programmatically.md) beschrieben.  
  
 Eine etwas kompliziertere und hier nicht erläuterte Möglichkeit besteht darin, ein vorhandenes Paket als Vorlage zu laden, diese neu zu konfigurieren (beispielsweise für eine andere Datenquelle) und das Paket dann auszuführen. Anhand der Informationen in diesem Abschnitt können Sie auch die vorhandenen Objekte in einem Paket ändern.  
  
> [!NOTE]  
>  Wenn Sie ein bestehendes Paket als Vorlage verwenden und vorhandene Spalten im Datenfluss ändern, müssen Sie möglicherweise vorhandene Spalten entfernen und die <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A>-Methode der betroffenen Komponenten aufrufen.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
 [Programmgesteuertes Erstellen eines Pakets](../../integration-services/building-packages-programmatically/creating-a-package-programmatically.md)  
 Beschreibt, wie ein Paket programmgesteuert erstellt wird.  
  
 [Programmgesteuertes Hinzufügen von Tasks](../../integration-services/building-packages-programmatically/adding-tasks-programmatically.md)  
 Beschreibt, wie dem Paket Tasks hinzugefügt werden.  
  
 [Programmgesteuertes Verbinden von Tasks](../../integration-services/building-packages-programmatically/connecting-tasks-programmatically.md)  
 Beschreibt, wie die Ausführung von Containern und Tasks in einem Paket basierend auf dem Ergebnis der Ausführung eines vorhergehenden Tasks oder Containers gesteuert wird.  
  
 [Programmgesteuertes Hinzufügen von Verbindungen](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md)  
 Beschreibt, wie Verbindungs-Manager zu einem Paket hinzugefügt werden.  
  
 [Programmgesteuertes Arbeiten mit Variablen](../../integration-services/building-packages-programmatically/working-with-variables-programmatically.md)  
 Beschreibt, wie Variablen während der Paketausführung hinzugefügt und verwendet werden.  
  
 [Programmgesteuerte Behandlung von Ereignissen](../../integration-services/building-packages-programmatically/handling-events-programmatically.md)  
 Beschreibt, wie Paket- und Taskereignisse behandelt werden.  
  
 [Programmgesteuertes Aktivieren der Protokollierung](../../integration-services/building-packages-programmatically/enabling-logging-programmatically.md)  
 Beschreibt, wie die Protokollierung für ein Paket oder einen Task aktiviert wird und wie benutzerdefinierte Filter auf Protokollereignisse angewendet werden.  
  
 [Programmgesteuertes Hinzufügen des Datenflusstasks](../../integration-services/building-packages-programmatically/adding-the-data-flow-task-programmatically.md)  
 Beschreibt, wie der Datenflusstask und seine Komponenten hinzugefügt und konfiguriert werden.  
  
 [Programmgesteuertes Auffinden von Datenflusskomponenten](../../integration-services/building-packages-programmatically/discovering-data-flow-components-programmatically.md)  
 Beschreibt, wie die Komponenten, die auf dem lokalen Computer installiert sind, erkannt werden.  
  
 [Programmgesteuertes Hinzufügen von Datenflusskomponenten](../../integration-services/building-packages-programmatically/adding-data-flow-components-programmatically.md)  
 Beschreibt, wie eine Komponente zu einem Datenflusstask hinzugefügt wird.  
  
 [Programmgesteuertes Verbinden von Datenflusskomponenten](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md)  
 Beschreibt, wie zwei Datenflusskomponenten verbunden werden.  
  
 [Programmgesteuertes Auswählen von Eingabespalten](../../integration-services/building-packages-programmatically/selecting-input-columns-programmatically.md)  
 Beschreibt, wie aus den Eingabespalten, die einer Komponente von Upstreamkomponenten im Datenfluss bereitgestellt werden, Spalten ausgewählt werden.  
  
 [Programmgesteuertes Speichern von Paketen](../../integration-services/building-packages-programmatically/saving-a-package-programmatically.md)  
 Beschreibt, wie ein Paket programmgesteuert gespeichert wird.  
  
## <a name="reference"></a>Verweis  
 [Fehler- und Meldungsreferenz von Integration Services](../../integration-services/integration-services-error-and-message-reference.md)  
 Listet die vordefinierten [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Fehlercodes mit ihren symbolischen Namen und Beschreibungen auf.  
  
## <a name="related-sections"></a>Verwandte Abschnitte  
 [Erweitern von Paketen mit Skripts](../../integration-services/extending-packages-scripting/extending-packages-with-scripting.md)  
 Erläutert, wie die Ablaufsteuerung mithilfe des Skripttasks und der Datenfluss mithilfe der Skriptkomponente erweitert werden können.  
  
 [Erweitern von Paketen mit benutzerdefinierten Objekten](../../integration-services/extending-packages-custom-objects/extending-packages-with-custom-objects.md)  
 Beschreibt, wie benutzerdefinierte Tasks, Datenflusskomponenten und andere Paketobjekte für die Verwendung in mehreren Paketen erstellt und programmiert werden.  
  
 [Programmgesteuerte Ausführung und Verwaltung von Paketen](../../integration-services/run-manage-packages-programmatically/running-and-managing-packages-programmatically.md)  
 Erläutert, wie Pakete und die Ordner, in denen sie gespeichert sind, aufgezählt, ausgeführt und verwaltet werden.  
  
## <a name="external-resources"></a>Externe Ressourcen  
  
-   CodePlex-Beispiele, [Integration Services Product Samples](https://go.microsoft.com/fwlink/?LinkID=131204), auf www.codeplex.com/MSFTISProdSamples  
  
-   Blogeintrag [Leistungsprofilerstellung für benutzerdefinierte Erweiterungen](https://techcommunity.microsoft.com/t5/sql-server-integration-services/performance-profiling-your-custom-extensions/ba-p/387490)auf blogs.msdn.com.  

## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Integration Services](../../integration-services/sql-server-integration-services.md)  
  
  
