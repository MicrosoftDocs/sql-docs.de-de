---
description: Ausführen eines inkrementellen Ladens von mehreren Tabellen
title: Ausführen eines inkrementellen Ladens von mehreren Tabellen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- incremental load [Integration Services],multiple tables
ms.assetid: 39252dd5-09c3-46f9-a17b-15208cfd336d
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 229809b060a9a0d3103acdb9a562a3dc639b91e8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338478"
---
# <a name="perform-an-incremental-load-of-multiple-tables"></a>Ausführen eines inkrementellen Ladens von mehreren Tabellen

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Im Thema [Verbessern des inkrementellen Ladens mit Change Data Capture](../../integration-services/change-data-capture/change-data-capture-ssis.md)veranschaulicht das Diagramm ein einfaches Paket, das ein inkrementelles Laden in nur einer Tabelle ausführt. Das Laden einer Tabelle ist jedoch nicht so üblich wie das Ausführen eines inkrementellen Ladens von mehreren Tabellen.  
  
 Wenn Sie ein inkrementelles Laden von mehreren Tabellen ausführen, müssen einige Schritte einmal für alle Tabellen ausgeführt werden, und andere Schritte müssen für jede Quelltabelle wiederholt werden. Sie haben mehr als eine Option zum Implementieren dieser Schritte in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]:  
  
-   Verwenden Sie ein übergeordnetes Paket und untergeordnete Pakete.  
  
-   Verwenden Sie mehrere Datenflusstasks in einem einzelnen Paket.  
  
## <a name="loading-multiple-tables-by-using-a-parent-package-and-multiple-child-packages"></a>Laden von mehreren Tabellen mit einem übergeordneten Paket und mehreren untergeordneten Paketen  
 Sie können ein übergeordnetes Paket verwenden, um jene Schritte auszuführen, die nur einmal gemacht werden müssen. Die untergeordneten Pakete führen jene Schritte aus, die für jede Quelltabelle gemacht werden müssen.  
  
#### <a name="to-create-a-parent-package-that-performs-those-steps-that-only-have-to-be-done-once"></a>So erstellen Sie ein übergeordnetes Paket, das jene Schritte ausführt, die nur einmal gemacht werden müssen  
  
1.  Erstellen eines übergeordneten Pakets.  
  
2.  Verwenden Sie in der Ablaufsteuerung einen Task "SQL ausführen" oder [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Ausdrücke, um die Endpunkte zu berechnen.  
  
     Ein Beispiel zur Berechnung von Endpunkten finden Sie unter [Angeben eines Intervalls von Änderungsdaten](../../integration-services/change-data-capture/specify-an-interval-of-change-data.md).  
  
3.  Verwenden Sie bei Bedarf einen For-Schleifencontainer, um die Ausführung so lange zu verzögern, bis die Änderungsdaten für den ausgewählten Zeitraum bereit sind.  
  
     Ein Beispiel eines solchen For-Schleifencontainers finden Sie unter [Bestimmen, ob die Änderungsdaten bereit sind](../../integration-services/change-data-capture/determine-whether-the-change-data-is-ready.md).  
  
4.  Verwenden Sie mehrere Tasks "Paket ausführen", um untergeordnete Pakete für jede zu ladende Tabelle auszuführen. Übergeben Sie die im übergeordneten Paket berechneten Endpunkte an jedes untergeordnete Paket, indem Sie die Variablenkonfiguration für übergeordnete Pakete verwenden.  
  
     Weitere Informationen finden Sie unter [Paket ausführen (Task)](../../integration-services/control-flow/execute-package-task.md) und [Verwenden der Werte von Variablen und Parametern in einem untergeordneten Paket](../../integration-services/packages/legacy-package-deployment-ssis.md#child).  
  
#### <a name="to-create-child-packages-to-perform-those-steps-that-have-to-be-done-for-each-source-table"></a>So erstellen Sie untergeordnete Pakete, um jene Schritte auszuführen, die für jede Quelltabelle gemacht werden müssen  
  
1.  Erstellen Sie für jede Quelltabelle ein untergeordnetes Paket.  
  
2.  Verwenden Sie in der Ablaufsteuerung einen Skripttask oder einen Task "SQL ausführen", um die SQL-Anweisung zusammenzustellen, mit der Änderungen abgefragt werden.  
  
     Ein Beispiel zum Zusammenstellen der Abfrage finden Sie unter [Vorbereiten zur Abfrage der Änderungsdaten](../../integration-services/change-data-capture/prepare-to-query-for-the-change-data.md).  
  
3.  Verwenden Sie in jedem untergeordneten Paket einen einzelnen Datenflusstask, um die Änderungsdaten zu laden und diese auf das Ziel anzuwenden. Konfigurieren Sie den Datenfluss, wie in den folgenden Stufen beschrieben:  
  
    1.  Verwenden Sie im Datenfluss eine Quellkomponente, um die Änderungstabellen nach den Änderungen abzufragen, die innerhalb der ausgewählten Endpunkte liegen.  
  
         Ein Beispiel zur Abfrage der Änderungstabellen finden Sie unter [Abrufen und Verstehen der Änderungsdaten](../../integration-services/change-data-capture/retrieve-and-understand-the-change-data.md).  
  
    2.  Verwenden Sie eine Transformation für bedingtes Teilen, um Einfügungen, Updates und Löschungen an andere Ausgaben zur entsprechenden Verarbeitung weiterzuleiten.  
  
         Ein Beispiel für die Konfiguration dieser Transformation zur Weiterleitung einer Ausgabe finden Sie unter [Verarbeiten von Einfügungen, Updates und Löschungen](../../integration-services/change-data-capture/process-inserts-updates-and-deletes.md).  
  
    3.  Verwenden Sie eine Zielkomponente, um die Einfügungen auf das Ziel anzuwenden. Verwenden Sie Transformationen für OLE DB-Befehl mit parametrisierten UPDATE- und DELETE-Anweisungen, um Updates und Löschungen auf das Ziel anzuwenden.  
  
         Ein Beispiel für die Verwendung dieser Transformation zur Anwendung von Updates und Löschungen finden Sie unter [Anwenden der Änderungen auf das Ziel](../../integration-services/change-data-capture/apply-the-changes-to-the-destination.md).  
  
## <a name="loading-multiple-tables-by-using-multiple-data-flow-tasks-in-a-single-package"></a>Laden von mehreren Tabellen mit mehreren Datenflusstasks in einem einzelnen Paket  
 Alternativ können Sie ein einzelnes Paket verwenden, das für jede zu ladende Quelltabelle einen separaten Datenflusstask enthält.  
  
#### <a name="to-load-multiple-tables-by-using-multiple-data-flow-tasks-in-a-single-package"></a>So laden Sie mehrere Tabellen mit mehreren Datenflusstasks in einem einzelnen Paket  
  
1.  Erstellen eines einzelnen Pakets.  
  
2.  Verwenden Sie in der Ablaufsteuerung einen Task "SQL ausführen" oder [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Ausdrücke, um die Endpunkte zu berechnen.  
  
     Ein Beispiel zur Berechnung von Endpunkten finden Sie unter [Angeben eines Intervalls von Änderungsdaten](../../integration-services/change-data-capture/specify-an-interval-of-change-data.md).  
  
3.  Verwenden Sie bei Bedarf einen For-Schleifencontainer, um die Ausführung so lange zu verzögern, bis die Änderungsdaten für das ausgewählte Intervall bereit sind.  
  
     Ein Beispiel eines solchen For-Schleifencontainers finden Sie unter [Bestimmen, ob die Änderungsdaten bereit sind](../../integration-services/change-data-capture/determine-whether-the-change-data-is-ready.md).  
  
4.  Verwenden Sie einen Skripttask oder einen Task "SQL ausführen", um die SQL-Anweisung zusammenzustellen, mit der Änderungen abgefragt werden.  
  
     Ein Beispiel zum Zusammenstellen der Abfrage finden Sie unter [Vorbereiten zur Abfrage der Änderungsdaten](../../integration-services/change-data-capture/prepare-to-query-for-the-change-data.md).  
  
5.  Verwenden Sie mehrere Datenflusstasks, um die Änderungsdaten von jeder Quelltabelle zu laden und diese auf das Ziel anzuwenden. Konfigurieren Sie jeden Datenflusstask, wie in den folgenden Schritten beschrieben:  
  
    1.  Verwenden Sie in jedem Datenfluss eine Quellkomponente, um die Änderungstabellen nach den Änderungen abzufragen, die innerhalb der ausgewählten Endpunkte liegen.  
  
         Ein Beispiel zur Abfrage der Änderungstabellen finden Sie unter [Abrufen und Verstehen der Änderungsdaten](../../integration-services/change-data-capture/retrieve-and-understand-the-change-data.md).  
  
    2.  Verwenden Sie eine Transformation für bedingtes Teilen, um Einfügungen, Updates und Löschungen an andere Ausgaben zur entsprechenden Verarbeitung weiterzuleiten.  
  
         Ein Beispiel für die Konfiguration dieser Transformation zur Weiterleitung einer Ausgabe finden Sie unter [Verarbeiten von Einfügungen, Updates und Löschungen](../../integration-services/change-data-capture/process-inserts-updates-and-deletes.md).  
  
    3.  Verwenden Sie eine Zielkomponente, um die Einfügungen auf das Ziel anzuwenden. Verwenden Sie Transformationen für OLE DB-Befehl mit parametrisierten UPDATE- und DELETE-Anweisungen, um Updates und Löschungen auf das Ziel anzuwenden.  
  
         Ein Beispiel für die Verwendung dieser Transformation zur Anwendung von Updates und Löschungen finden Sie unter [Anwenden der Änderungen auf das Ziel](../../integration-services/change-data-capture/apply-the-changes-to-the-destination.md).  
  
  
