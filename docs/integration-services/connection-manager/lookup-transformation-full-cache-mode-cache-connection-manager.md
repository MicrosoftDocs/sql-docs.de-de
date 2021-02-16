---
description: Suchtransformation im Vollcachemodus – Cacheverbindungs-Manager
title: Suchtransformation im Vollcachemodus – Cacheverbindungs-Manager | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Lookup transformation [Integration Services]
ms.assetid: 58bc7611-5fb5-4113-9742-10959e06b94c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c6560951e789dc16c78788b278579abb906fa3e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350332"
---
# <a name="lookup-transformation-full-cache-mode---cache-connection-manager"></a>Suchtransformation im Vollcachemodus – Cacheverbindungs-Manager

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Sie können die Transformation für Suche so konfigurieren, dass der Vollcachemodus und ein Cacheverbindungs-Manager verwendet werden. Im Vollcachemodus wird das Verweisdataset in den Cache geladen, bevor die Transformation für Suche ausgeführt wird.  
  
> [!NOTE]  
>  Der Cacheverbindungs-Manager unterstützt die BLOB-Datentypen (Binary Large Object) DT_TEXT, DT_NTEXT und DT_IMAGE nicht. Wenn das Verweisdataset einen BLOB-Datentyp enthält, schlägt die Komponente fehl, wenn Sie das Paket ausführen. Sie können den **Cacheverbindungs-Manager-Editor** verwenden, um Spaltendatentypen zu ändern. Weitere Informationen finden Sie unter [Cache Connection Manager Editor](./cache-connection-manager.md).  
  
 Die Transformation für Suche führt Suchvorgänge aus, indem Daten in Eingabespalten aus einer verbundenen Datenquelle mit Spalten in einem Verweisdataset verknüpft werden. Weitere Informationen finden Sie unter [Lookup Transformation](../../integration-services/data-flow/transformations/lookup-transformation.md).  
  
 Sie verwenden eines der folgenden Elemente, um ein Verweisdataset zu generieren:  
  
-   Cachedatei (.caw)  
  
     Sie konfigurieren den Cacheverbindungs-Manager so, dass Daten aus einer vorhandenen Cachedatei gelesen werden.  
  
-   Verbundene Datenquelle im Datenfluss  
  
     Sie verwenden eine Transformation für Cachetransformation, um Daten aus einer verbundenen Datenquelle im Datenfluss in einen Cacheverbindungs-Manager zu schreiben. Die Daten werden immer im Arbeitsspeicher abgelegt.  
  
     Sie müssen die Transformation für Suche einem separaten Datenfluss hinzufügen. Dadurch kann die Cachetransformation Daten in den Cacheverbindungs-Manager laden, bevor die Transformation für Suche ausgeführt wird. Die Datenflüsse können sich im selben Paket oder in zwei separaten Paketen befinden.  
  
     Befinden sich die Datenflüsse im selben Paket, verwenden Sie eine Rangfolgeneinschränkung, um die Datenflüsse zu verbinden. Dadurch kann die Cachetransformation ausgeführt werden, bevor die Transformation für Suche ausgeführt wird.  
  
     Befinden sich die Datenflüsse in separaten Paketen, können Sie den Task Paket ausführen verwenden, um die untergeordneten Pakete vom übergeordneten Paket aufzurufen. Sie können mehrere untergeordnete Pakete aufrufen, indem Sie mehrere Tasks Paket einem Task Sequenzcontainer im übergeordneten Paket hinzufügen.  
  
 Sie können das im Cache abgelegte Verweisdataset in mehreren Transformationen für Suche mit einer der folgenden Methoden gemeinsam nutzen:  
  
-   Konfigurieren Sie die Transformationen für Suche in einem Paket, sodass sie denselben Cacheverbindungs-Manager verwenden.  
  
-   Konfigurieren Sie die Cacheverbindungs-Manager in verschiedenen Paketen, um die gleiche Cachedatei zu verwenden.  
  
 Weitere Informationen finden Sie unter den folgenden Themen:  
  
-   [Cachetransformation](../../integration-services/data-flow/transformations/cache-transform.md)  
  
-   [Cacheverbindungs-Manager](../../integration-services/connection-manager/cache-connection-manager.md)  
  
-   [Rangfolgeneinschränkungen](../../integration-services/control-flow/precedence-constraints.md)  
  
-   [Paket ausführen (Task)](../../integration-services/control-flow/execute-package-task.md)  
  
-   [Sequenzcontainer](../../integration-services/control-flow/sequence-container.md)  
  
 Ein Video zur Veranschaulichung der Implementierung einer Transformation für Suche im Vollcachemodus mithilfe des Cacheverbindungs-Managers finden Sie unter [Gewusst wie: Implementieren einer Suchtransformation im Vollcachemodus (SQL Server-Video)](/previous-versions/sql/sql-server-2008/cc952929(v=sql.100)).  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-in-one-package-by-using-cache-connection-manager-and-a-data-source-in-the-data-flow"></a>So implementieren Sie eine Transformation für Suche im Vollcachemodus in einem Paket mit dem Cacheverbindungs-Manager und einer Datenquelle im Datenfluss  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]ein [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Projekt, und öffnen Sie dann ein Paket.  
  
2.  Fügen Sie auf der Registerkarte **Ablaufsteuerung** zwei Datenflusstasks hinzu, und verbinden Sie dann die Tasks mit einem grünen Konnektor:  
  
3.  Fügen Sie im ersten Datenfluss eine Transformation für Cachetransformation hinzu, und verbinden Sie dann die Transformation mit einer Datenquelle.  
  
     Konfigurieren Sie die Datenquelle nach Bedarf.  
  
4.  Doppelklicken Sie auf die Transformation für Cachetransformation, und klicken Sie dann im **Cachetransformations-Editor** auf der Seite **Verbindungs-Manager** auf **Neu** , um einen neuen Cacheverbindungs-Manager zu erstellen.  
  
5.  Klicken Sie auf die Registerkarte **Spalten** im Dialogfeld **Editor für den Cacheverbindungs-Manager** , und geben Sie dann mit der Option **Indexposition** an, welche Spalten die Indexspalten sind.  
  
     Für Nicht-Index-Spalten ist die Indexposition 0. Für Indexspalten ist die Indexposition eine fortlaufende positive Zahl.  
  
    > [!NOTE]  
    >  Wenn die Transformation für Suche so konfiguriert ist, dass sie einen Cacheverbindungs-Manager verwendet, können nur Indexspalten im Verweisdataset Eingabespalten zugeordnet werden. Auch müssen alle Indexspalten zugeordnet werden. Weitere Informationen finden Sie unter [Cache Connection Manager Editor](./cache-connection-manager.md).  
  
6.  Um den Cache in einer Datei zu speichern, konfigurieren Sie den Cacheverbindungs-Manager im **Editor für den Cacheverbindungs-Manager** auf der Registerkarte **Allgemein** , indem Sie die folgenden Optionen festlegen:  
  
    -   Wählen Sie **Dateicache verwenden** aus.  
  
    -   Im Feld **Dateiname** geben Sie entweder den Dateipfad ein, oder klicken Sie auf **Durchsuchen** , um die Datei auszuwählen.  
  
         Wenn Sie einen Pfad für eine Datei eingeben, die nicht vorhanden ist, erstellt das System die Datei, wenn Sie das Paket ausführen.  
  
    > [!NOTE]  
    >  Die Schutzebene des Pakets gilt nicht für die Cachedatei. Wenn die Cachedatei vertrauliche Informationen enthält, schränken Sie den Zugriff auf den Speicherort oder Ordner, in dem Sie die Datei speichern, mithilfe einer Zugriffssteuerungsliste ein. Sie sollten nur bestimmten Konten den Zugriff ermöglichen. Weitere Informationen finden Sie unter [Zugriff auf Dateien, die von Paketen verwendet werden](../../integration-services/security/security-overview-integration-services.md#files).  
  
7.  Konfigurieren Sie die Cacheumwandlung nach Bedarf. Weitere Informationen finden Sie unter [Cachetransformations-Editor &#40;Seite „Verbindungs-Manager“&#41;](../data-flow/transformations/cache-transform.md) und [Cachetransformations-Editor &#40;Seite „Zuordnungen“&#41;](../data-flow/transformations/cache-transform.md).  
  
8.  Fügen Sie im zweiten Datenfluss eine Transformation für Suche hinzu, und konfigurieren Sie dann die Transformation, indem Sie die folgenden Tasks ausführen:  
  
    1.  Verbinden Sie die Suchtransformation mit dem Datenfluss, indem Sie einen Konnektor von einer Quelle oder einer vorherigen Transformation auf die Suchtransformation ziehen.  
  
        > [!NOTE]  
        >  Eine Suchtransformation erzeugt möglicherweise einen Fehler, wenn sie sich mit einem Flatfile verbindet, das ein leeres Datenfeld enthält. Die Gültigkeit der Transformation hängt davon ab, ob der Verbindungs-Manager für das Flatfile so konfiguriert wurde, dass NULL-Werte beibehalten werden. Um sicherzustellen, dass die Suchtransformation gültig ist, wählen Sie im **Quelleneditor für Flatfiles** auf der **Seite Verbindungs-Manager** die Option **NULL-Werte aus der Quelle als NULL-Werte im Datenfluss beibehalten** .  
  
    2.  Doppelklicken Sie auf die Quelle oder die vorherige Transformation, um die Komponente zu konfigurieren.  
  
    3.  Doppelklicken Sie auf die Transformation für Suche, und wählen Sie dann im **Transformations-Editor für Suche** auf der Seite **Allgemein** die Option **Vollcache** aus.  
  
    4.  Wählen Sie im Bereich **Verbindungstyp****Cacheverbindungs-Manager** aus.  
  
    5.  Wählen Sie für die Liste **Angeben, wie Zeilen ohne übereinstimmende Einträge behandelt werden sollen** eine Fehlerbehandlungsoption aus.  
  
    6.  Wählen Sie auf der Seite **Verbindung** aus der Liste **Cacheverbindungs-Manager** einen Cacheverbindungs-Manager aus.  
  
    7.  Klicken Sie auf die Seite **Spalten** , und ziehen Sie mindestens eine Spalte aus der Liste **Verfügbare Eingabespalten** in eine Spalte in der Liste **Verfügbare Suchspalten** .  
  
        > [!NOTE]  
        >  Die Transformation für Suche ordnet automatisch Spalten mit dem gleichen Namen und dem gleichen Datentyp zu.  
  
        > [!NOTE]  
        >  Die Spalten müssen übereinstimmende Datentypen aufweisen, damit sie zugeordnet werden. Weitere Informationen finden Sie unter [Integration Services Datentypen](../../integration-services/data-flow/integration-services-data-types.md).  
  
    8.  Wählen Sie Spalten aus der Liste **Verfügbare Suchspalten** aus. Geben Sie dann in der Liste **Suchvorgang** an, ob die Werte aus den Suchspalten Werte in der Eingabespalte ersetzen oder in eine neue Spalte geschrieben werden.  
  
    9. Um die Fehlerausgabe zu konfigurieren, klicken Sie auf die Seite **Fehlerausgabe** , und legen Sie die Fehlerbehandlungsoptionen fest. Weitere Informationen finden Sie unter [Transformations-Editor für Suche &#40;Seite Fehlerausgabe&#41;](../data-flow/transformations/lookup-transformation.md).  
  
    10. Klicken Sie auf **OK** , um die Änderungen an der Transformation für Suche zu speichern.  
  
9. Führen Sie das Paket aus.  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-in-two-packages-by-using-cache-connection-manager-and-a-data-source-in-the-data-flow"></a>So implementieren Sie eine Transformation für Suche im Vollcachemodus in zwei Paketen mit dem Cacheverbindungs-Manager und einer Datenquelle im Datenfluss  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]ein [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Projekt, und öffnen Sie dann ein zwei Pakete.  
  
2.  Fügen Sie auf der Registerkarte Ablaufsteuerung in jedem Paket einen Datenflusstask hinzu.  
  
3.  Fügen Sie im übergeordneten Paket dem Datenfluss eine Transformation für Cachetransformation hinzu, und verbinden Sie dann die Transformation mit einer Datenquelle.  
  
     Konfigurieren Sie die Datenquelle nach Bedarf.  
  
4.  Doppelklicken Sie auf die Transformation für Cachetransformation, und klicken Sie dann im **Cachetransformations-Editor** auf der Seite **Verbindungs-Manager** auf **Neu** , um einen neuen Cacheverbindungs-Manager zu erstellen.  
  
5.  Konfigurieren Sie den Cacheverbindungs-Manager im **Editor für Cacheverbindungs-Manager** auf der Registerkarte **Allgemein** , indem Sie die folgenden Optionen festlegen:  
  
    -   Wählen Sie **Dateicache verwenden** aus.  
  
    -   Im Feld **Dateiname** geben Sie entweder den Dateipfad ein, oder klicken Sie auf **Durchsuchen** , um die Datei auszuwählen.  
  
         Wenn Sie einen Pfad für eine Datei eingeben, die nicht vorhanden ist, erstellt das System die Datei, wenn Sie das Paket ausführen.  
  
    > [!NOTE]  
    >  Die Schutzebene des Pakets gilt nicht für die Cachedatei. Wenn die Cachedatei vertrauliche Informationen enthält, schränken Sie den Zugriff auf den Speicherort oder Ordner, in dem Sie die Datei speichern, mithilfe einer Zugriffssteuerungsliste ein. Sie sollten nur bestimmten Konten den Zugriff ermöglichen. Weitere Informationen finden Sie unter [Zugriff auf Dateien, die von Paketen verwendet werden](../../integration-services/security/security-overview-integration-services.md#files).  
  
6.  Klicken Sie auf die Registerkarte **Spalten** , und geben Sie dann mit der Option **Indexposition** an, welche Spalten die Indexspalten sind.  
  
     Für Nicht-Index-Spalten ist die Indexposition 0. Für Indexspalten ist die Indexposition eine fortlaufende positive Zahl.  
  
    > [!NOTE]  
    >  Wenn die Transformation für Suche so konfiguriert ist, dass sie einen Cacheverbindungs-Manager verwendet, können nur Indexspalten im Verweisdataset Eingabespalten zugeordnet werden. Auch müssen alle Indexspalten zugeordnet werden. Weitere Informationen finden Sie unter [Cache Connection Manager Editor](./cache-connection-manager.md).  
  
7.  Konfigurieren Sie die Cacheumwandlung nach Bedarf. Weitere Informationen finden Sie unter [Cachetransformations-Editor &#40;Seite „Verbindungs-Manager“&#41;](../data-flow/transformations/cache-transform.md) und [Cachetransformations-Editor &#40;Seite „Zuordnungen“&#41;](../data-flow/transformations/cache-transform.md).  
  
8.  Führen Sie einen der folgenden Schritte aus, um Daten in den Cacheverbindungs-Manager zu laden, der im zweiten Paket verwendet wird:  
  
    -   Führen Sie das übergeordnete Paket aus.  
  
    -   Doppelklicken Sie auf den Cacheverbindungs-Manager, den Sie in Schritt 4 erstellt haben, klicken Sie auf **Spalten**, wählen Sie die Zeilen aus, und drücken Sie STRG+C, um die Spaltenmetadaten zu kopieren.  
  
9. Erstellen Sie im untergeordneten Paket einen Cacheverbindungs-Manager, indem Sie mit der rechten Maustaste in den Bereich **Verbindungs-Manager** klicken, auf **Neue Verbindung** klicken, im Dialogfeld **SSIS-Verbindungs-Manager hinzufügen** die Option **CACHE** auswählen und dann auf **Hinzufügen** klicken.  
  
     Der Bereich **Verbindungs-Manager** wird unten auf den Registerkarten **Ablaufsteuerung**, **Datenfluss** und **Ereignishandler** des [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Designers angezeigt.  
  
10. Konfigurieren Sie den Cacheverbindungs-Manager im **Editor für Cacheverbindungs-Manager** auf der Registerkarte **Allgemein** so, dass die Daten aus der ausgewählten Cachedatei gelesen werden, indem Sie die folgenden Optionen festlegen:  
  
    -   Wählen Sie **Dateicache verwenden** aus.  
  
    -   Im Feld **Dateiname** geben Sie entweder den Dateipfad ein, oder klicken Sie auf **Durchsuchen** , um die Datei auszuwählen.  
  
    > [!NOTE]  
    >  Die Schutzebene des Pakets gilt nicht für die Cachedatei. Wenn die Cachedatei vertrauliche Informationen enthält, schränken Sie den Zugriff auf den Speicherort oder Ordner, in dem Sie die Datei speichern, mithilfe einer Zugriffssteuerungsliste ein. Sie sollten nur bestimmten Konten den Zugriff ermöglichen. Weitere Informationen finden Sie unter [Zugriff auf Dateien, die von Paketen verwendet werden](../../integration-services/security/security-overview-integration-services.md#files).  
  
11. Wenn Sie in Schritt 8 die Spaltenmetadaten kopiert haben, klicken Sie auf **Spalten**, wählen Sie die leere Zeile aus, und drücken Sie STRG+V, um die Spaltenmetadaten einzufügen.  
  
12. Fügen Sie eine Transformation für Suche hinzu, und konfigurieren Sie dann die Transformation, indem Sie die folgenden Tasks ausführen:  
  
    1.  Verbinden Sie die Suchtransformation mit dem Datenfluss, indem Sie einen Konnektor von einer Quelle oder einer vorherigen Transformation auf die Suchtransformation ziehen.  
  
        > [!NOTE]  
        >  Eine Suchtransformation erzeugt möglicherweise einen Fehler, wenn sie sich mit einem Flatfile verbindet, das ein leeres Datenfeld enthält. Die Gültigkeit der Transformation hängt davon ab, ob der Verbindungs-Manager für das Flatfile so konfiguriert wurde, dass NULL-Werte beibehalten werden. Um sicherzustellen, dass die Suchtransformation gültig ist, wählen Sie im **Quelleneditor für Flatfiles** auf der **Seite Verbindungs-Manager** die Option **NULL-Werte aus der Quelle als NULL-Werte im Datenfluss beibehalten** .  
  
    2.  Doppelklicken Sie auf die Quelle oder die vorherige Transformation, um die Komponente zu konfigurieren.  
  
    3.  Doppelklicken Sie auf die Transformation für Suche, und wählen Sie dann **Vollcache** auf der Seite **Allgemein** im **Transformations-Editor für Suche** aus.  
  
    4.  Wählen Sie im Bereich **Verbindungstyp****Cacheverbindungs-Manager** aus.  
  
    5.  Wählen Sie für die Liste **Angeben, wie Zeilen ohne übereinstimmende Einträge behandelt werden sollen** eine Fehlerbehandlungsoption für Zeilen ohne übereinstimmende Einträge aus.  
  
    6.  Wählen Sie auf der Seite **Verbindung** aus der Liste **Cacheverbindungs-Manager** den Cacheverbindungs-Manager aus, den Sie hinzugefügt haben.  
  
    7.  Klicken Sie auf die Seite **Spalten** , und ziehen Sie mindestens eine Spalte aus der Liste **Verfügbare Eingabespalten** in eine Spalte in der Liste **Verfügbare Suchspalten** .  
  
        > [!NOTE]  
        >  Die Transformation für Suche ordnet automatisch Spalten mit dem gleichen Namen und dem gleichen Datentyp zu.  
  
        > [!NOTE]  
        >  Die Spalten müssen übereinstimmende Datentypen aufweisen, damit sie zugeordnet werden. Weitere Informationen finden Sie unter [Integration Services Datentypen](../../integration-services/data-flow/integration-services-data-types.md).  
  
    8.  Wählen Sie Spalten aus der Liste **Verfügbare Suchspalten** aus. Geben Sie dann in der Liste **Suchvorgang** an, ob die Werte aus den Suchspalten Werte in der Eingabespalte ersetzen oder in eine neue Spalte geschrieben werden.  
  
    9. Um die Fehlerausgabe zu konfigurieren, klicken Sie auf die Seite **Fehlerausgabe** , und legen Sie die Fehlerbehandlungsoptionen fest. Weitere Informationen finden Sie unter [Transformations-Editor für Suche &#40;Seite Fehlerausgabe&#41;](../data-flow/transformations/lookup-transformation.md).  
  
    10. Klicken Sie auf **OK** , um die Änderungen an der Transformation für Suche zu speichern.  
  
13. Öffnen Sie das übergeordnete Paket, fügen Sie einen Task Paket ausführen zur Ablaufsteuerung hinzu, und konfigurieren Sie den Task so, dass das untergeordnete Paket aufgerufen wird. Weitere Informationen finden Sie unter [Execute Package Task](../../integration-services/control-flow/execute-package-task.md).  
  
14. Führen Sie die Pakete aus.  
  
### <a name="to-implement-a-lookup-transformation-in-full-cache-mode-by-using-cache-connection-manager-and-an-existing-cache-file"></a>So implementieren Sie eine Transformation für Suche im Vollcachemodus mit dem Cacheverbindungs-Manager und einer vorhandenen Cachedatei  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]ein [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Projekt, und öffnen Sie dann ein Paket.  
  
2.  Klicken Sie mit der rechten Maustaste in den Bereich **Verbindungs-Manager** , und klicken Sie dann auf **Neue Verbindung**.  
  
     Der Bereich **Verbindungs-Manager** wird unten auf den Registerkarten **Ablaufsteuerung**, **Datenfluss** und **Ereignishandler** des [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Designers angezeigt.  
  
3.  Wählen Sie im Dialogfeld **SSIS-Verbindungs-Manager hinzufügen****CACHE** aus, und klicken Sie dann auf **Hinzufügen** , um einen Cacheverbindungs-Manager hinzuzufügen.  
  
4.  Doppelklicken Sie auf den Cacheverbindungs-Manager, um den **Editor für den Cacheverbindungs-Manager** zu öffnen.  
  
5.  Konfigurieren Sie den Cacheverbindungs-Manager im **Editor für Cacheverbindungs-Manager** auf der Registerkarte **Allgemein** , indem Sie die folgenden Optionen festlegen:  
  
    -   Wählen Sie **Dateicache verwenden** aus.  
  
    -   Im Feld **Dateiname** geben Sie entweder den Dateipfad ein, oder klicken Sie auf **Durchsuchen** , um die Datei auszuwählen.  
  
    > [!NOTE]  
    >  Die Schutzebene des Pakets gilt nicht für die Cachedatei. Wenn die Cachedatei vertrauliche Informationen enthält, schränken Sie den Zugriff auf den Speicherort oder Ordner, in dem Sie die Datei speichern, mithilfe einer Zugriffssteuerungsliste ein. Sie sollten nur bestimmten Konten den Zugriff ermöglichen. Weitere Informationen finden Sie unter [Zugriff auf Dateien, die von Paketen verwendet werden](../../integration-services/security/security-overview-integration-services.md#files).  
  
6.  Klicken Sie auf die Registerkarte **Spalten** , und geben Sie dann mit der Option **Indexposition** an, welche Spalten die Indexspalten sind.  
  
     Für Nicht-Index-Spalten ist die Indexposition 0. Für Indexspalten ist die Indexposition eine fortlaufende positive Zahl.  
  
    > [!NOTE]  
    >  Wenn die Transformation für Suche so konfiguriert ist, dass sie einen Cacheverbindungs-Manager verwendet, können nur Indexspalten im Verweisdataset Eingabespalten zugeordnet werden. Auch müssen alle Indexspalten zugeordnet werden. Weitere Informationen finden Sie unter [Cache Connection Manager Editor](./cache-connection-manager.md).  
  
7.  Fügen Sie dem Paket auf der Registerkarte **Ablaufsteuerung** einen Datenflusstask hinzu, und fügen Sie dem Datenfluss dann eine Transformation für Suche hinzu.  
  
8.  Konfigurieren Sie die Transformation für Suche, indem Sie die folgenden Schritte ausführen:  
  
    1.  Verbinden Sie die Suchtransformation mit dem Datenfluss, indem Sie einen Konnektor von einer Quelle oder einer vorherigen Transformation auf die Suchtransformation ziehen.  
  
        > [!NOTE]  
        >  Eine Suchtransformation erzeugt möglicherweise einen Fehler, wenn sie sich mit einem Flatfile verbindet, das ein leeres Datenfeld enthält. Die Gültigkeit der Transformation hängt davon ab, ob der Verbindungs-Manager für das Flatfile so konfiguriert wurde, dass NULL-Werte beibehalten werden. Um sicherzustellen, dass die Suchtransformation gültig ist, wählen Sie im **Quelleneditor für Flatfiles** auf der **Seite Verbindungs-Manager** die Option **NULL-Werte aus der Quelle als NULL-Werte im Datenfluss beibehalten** .  
  
    2.  Doppelklicken Sie auf die Quelle oder die vorherige Transformation, um die Komponente zu konfigurieren.  
  
    3.  Doppelklicken Sie auf die Transformation für Suche, und wählen Sie dann im **Transformations-Editor für Suche** auf der Seite **Allgemein** die Option **Vollcache** aus.  
  
    4.  Wählen Sie im Bereich **Verbindungstyp****Cacheverbindungs-Manager** aus.  
  
    5.  Wählen Sie für die Liste **Angeben, wie Zeilen ohne übereinstimmende Einträge behandelt werden sollen** eine Fehlerbehandlungsoption für Zeilen ohne übereinstimmende Einträge aus.  
  
    6.  Wählen Sie auf der Seite **Verbindung** aus der Liste **Cacheverbindungs-Manager** den Cacheverbindungs-Manager aus, den Sie hinzugefügt haben.  
  
    7.  Klicken Sie auf die Seite **Spalten** , und ziehen Sie mindestens eine Spalte aus der Liste **Verfügbare Eingabespalten** in eine Spalte in der Liste **Verfügbare Suchspalten** .  
  
        > [!NOTE]  
        >  Die Transformation für Suche ordnet automatisch Spalten mit dem gleichen Namen und dem gleichen Datentyp zu.  
  
        > [!NOTE]  
        >  Die Spalten müssen übereinstimmende Datentypen aufweisen, damit sie zugeordnet werden. Weitere Informationen finden Sie unter [Integration Services Datentypen](../../integration-services/data-flow/integration-services-data-types.md).  
  
    8.  Wählen Sie Spalten aus der Liste **Verfügbare Suchspalten** aus. Geben Sie dann in der Liste **Suchvorgang** an, ob die Werte aus den Suchspalten Werte in der Eingabespalte ersetzen oder in eine neue Spalte geschrieben werden.  
  
    9. Um die Fehlerausgabe zu konfigurieren, klicken Sie auf die Seite **Fehlerausgabe** , und legen Sie die Fehlerbehandlungsoptionen fest. Weitere Informationen finden Sie unter [Transformations-Editor für Suche &#40;Seite Fehlerausgabe&#41;](../data-flow/transformations/lookup-transformation.md).  
  
    10. Klicken Sie auf **OK** , um die Änderungen an der Transformation für Suche zu speichern.  
  
9. Führen Sie das Paket aus.  
  
## <a name="see-also"></a>Siehe auch  
 [Implementieren einer Suchtransformation im Vollcachemodus mit dem OLE DB-Verbindungs-Manager](../../integration-services/connection-manager/lookup-transformation-full-cache-mode-ole-db-connection-manager.md)   
 [Implementieren einer Suche im Modus „Kein Cache“ oder „Teilcache“](../../integration-services/data-flow/transformations/implement-a-lookup-in-no-cache-or-partial-cache-mode.md)   
 [SQL Server Integration Services-Transformationen](../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
