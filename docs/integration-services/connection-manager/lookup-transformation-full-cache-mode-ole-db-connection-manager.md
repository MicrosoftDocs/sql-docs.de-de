---
description: Suchtransformation im Vollcachemodus – OLE DB-Verbindungs-Manager
title: Suchtransformation im Vollcachemodus – OLE DB-Verbindungs-Manager | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Lookup transformation [Integration Services]
ms.assetid: c4150e1b-bdff-4f7a-af4c-3401c34def83
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7abcc50e3400e37219c73d8fbee0746557cc33da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350354"
---
# <a name="lookup-transformation-full-cache-mode---ole-db-connection-manager"></a>Suchtransformation im Vollcachemodus – OLE DB-Verbindungs-Manager

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Sie können die Transformation für Suche so konfigurieren, dass der Vollcachemodus und ein OLE DB-Verbindungs-Manager verwendet werden. Im Vollcachemodus wird das Verweisdataset in den Cache geladen, bevor die Transformation für Suche ausgeführt wird.  
  
 Die Transformation für Suche führt Suchvorgänge aus, indem Daten in Eingabespalten aus einer verbundenen Datenquelle mit Spalten in einem Verweisdataset verknüpft werden. Weitere Informationen finden Sie unter [Lookup Transformation](../../integration-services/data-flow/transformations/lookup-transformation.md).  
  
 Wenn Sie die Transformation für Suche für einen OLE DB-Verbindungs-Manager konfigurieren, wählen Sie eine Tabelle, Sicht oder SQL-Abfrage zum Generieren des Verweisdatasets aus.  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-by-using-ole-db-connection-manager"></a>So implementieren Sie eine Transformation für Suche im Vollcachemodus mit dem OLE DB-Verbindungs-Manager  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Projekt mit dem gewünschten Paket, und doppelklicken Sie dann im Projektmappen-Explorer auf das Paket.  
  
2.  Ziehen Sie auf der Registerkarte **Datenfluss** die Transformation für Suche aus der **Toolbox** auf die Entwurfsoberfläche.  
  
3.  Verbinden Sie die Suchtransformation mit dem Datenfluss, indem Sie einen Konnektor von einer Quelle oder einer vorherigen Transformation auf die Suchtransformation ziehen.  
  
    > [!NOTE]  
    >  Eine Suchtransformation erzeugt möglicherweise einen Fehler, wenn sie sich mit einem Flatfile verbindet, das ein leeres Datenfeld enthält. Die Gültigkeit der Transformation hängt davon ab, ob der Verbindungs-Manager für das Flatfile so konfiguriert wurde, dass NULL-Werte beibehalten werden. Um sicherzustellen, dass die Suchtransformation gültig ist, wählen Sie im **Quelleneditor für Flatfiles** auf der **Seite Verbindungs-Manager** die Option **NULL-Werte aus der Quelle als NULL-Werte im Datenfluss beibehalten** .  
  
4.  Doppelklicken Sie auf die Quelle oder die vorherige Transformation, um die Komponente zu konfigurieren.  
  
5.  Doppelklicken Sie auf die Transformation für Suche, und wählen Sie dann im **Transformations-Editor für Suche** auf der Seite **Allgemein** die Option **Vollcache** aus.  
  
6.  Wählen Sie im Bereich **Verbindungstyp****OLE DB-Verbindungs-Manager** aus.  
  
7.  Wählen Sie in der Liste **Angeben, wie Zeilen ohne übereinstimmende Einträge behandelt werden sollen** eine Fehlerbehandlungsoption für Zeilen ohne übereinstimmende Einträge aus.  
  
8.  Wählen Sie auf der Seite Verbindung einen Verbindungs-Manager aus der Liste **OLE DB-Verbindungs-Manager** aus, oder klicken Sie auf **Neu** , um einen neuen Verbindungs-Manager zu erstellen. Weitere Informationen finden Sie unter [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
9. Führen Sie eine der folgenden Aufgaben aus:  
  
    -   Klicken Sie auf **Tabelle oder Sicht verwenden**, und wählen Sie dann eine Tabelle oder eine Sicht aus, oder klicken Sie auf **Neu** , um eine Tabelle oder Sicht zu erstellen.  
  
         - oder -  
  
    -   Klicken Sie auf **Ergebnisse einer SQL-Abfrage verwenden**, und erstellen Sie im Fenster **SQL-Befehl** eine Abfrage, oder klicken Sie auf **Abfrage erstellen** , um eine Abfrage mithilfe der grafischen Tools des **Abfrage-Generators** zu erstellen.  
  
         - oder -  
  
    -   Klicken Sie alternativ auf **Durchsuchen** , um eine SQL-Anweisung aus einer Datei zu importieren.  
  
     Um die SQL-Abfrage zu überprüfen, klicken Sie auf **Abfrage analysieren**.  
  
     Um ein Beispiel der Daten anzuzeigen, klicken Sie auf **Vorschau**.  
  
10. Klicken Sie auf die Seite **Spalten** , und ziehen Sie mindestens eine Spalte aus der Liste **Verfügbare Eingabespalten** in eine Spalte in der Liste **Verfügbare Suchspalten** .  
  
    > [!NOTE]  
    >  Die Transformation für Suche ordnet automatisch Spalten mit dem gleichen Namen und dem gleichen Datentyp zu.  
  
    > [!NOTE]  
    >  Die Spalten müssen übereinstimmende Datentypen aufweisen, damit sie zugeordnet werden. Weitere Informationen finden Sie unter [Integration Services Datentypen](../../integration-services/data-flow/integration-services-data-types.md).  
  
11. Schließen Sie Suchspalten in die Ausgabe ein, indem Sie die folgenden Schritte ausführen:  
  
    1.  In der Liste **Verfügbare Suchspalten** . wählen Sie Spalten aus.  
  
    2.  Geben Sie in der Liste **Suchvorgang** an, ob die Werte aus den Suchspalten Werte in der Eingabespalte ersetzen oder ob sie in eine neue Spalte geschrieben werden.  
  
12. Um die Fehlerausgabe zu konfigurieren, klicken Sie auf die Seite **Fehlerausgabe** , und legen Sie die Fehlerbehandlungsoptionen fest. Weitere Informationen finden Sie unter [Transformations-Editor für Suche &#40;Seite Fehlerausgabe&#41;](../data-flow/transformations/lookup-transformation.md).  
  
13. Klicken Sie auf **OK** , um die Änderungen an der Suchtransformation zu speichern, und führen Sie dann das Paket aus.  
  
## <a name="see-also"></a>Siehe auch  
 [Implementieren einer Suchtransformation im Vollcachemodus mit dem Cacheverbindungs-Manager](../../integration-services/connection-manager/lookup-transformation-full-cache-mode-cache-connection-manager.md)   
 [Implementieren einer Suche im Modus „Kein Cache“ oder „Teilcache“](../../integration-services/data-flow/transformations/implement-a-lookup-in-no-cache-or-partial-cache-mode.md)   
 [SQL Server Integration Services-Transformationen](../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
