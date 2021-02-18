---
title: Fehlerliste (Fenster)
description: Hier erfahren Sie, wie Sie die Fehlerliste zum Anzeigen der Fehler und Warnungen verwenden, die IntelliSense generiert, wenn Sie Transact-SQL-Skripts bearbeiten.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- error list window
- SQL Server Management Studio [SQL Server], error list window
ms.assetid: fae6327d-e268-44ae-a474-4a8f8f843129
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5045ff2aa128b9e1c34f3031ae1fcd487b511481
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354515"
---
# <a name="transact-sql-debugger---error-list-window"></a>Transact-SQL-Debugger – Fenster „Fehlerliste“

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

In der **Fehlerliste** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] werden die vom IntelliSense-Code im [!INCLUDE[ssDE](../../includes/ssde-md.md)]-Abfrage-Editor generierten Syntax- und Semantikfehler angezeigt.  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="features-of-the-error-list"></a>Funktionen der Fehlerliste  

Die **Fehlerliste** bietet die folgende Funktionalität:  
  
-   Beim Bearbeiten von Skripts werden in der **Fehlerliste** die von IntelliSense im [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor generierten Fehler und Warnungen angezeigt.  
  
-   Sie können auf eine beliebige Fehlermeldung doppelklicken, um den Fokus auf die Registerkarte für die Skriptdatei zu verschieben, die den Fehler generiert hat, und zur entsprechenden Stelle zu wechseln.  
  
-   Sie können die anzuzeigenden Einträge und die Informationsspalten, die für jeden Eintrag angezeigt werden sollen, filtern.  
  
-   Nachdem Sie einen Fehler behoben haben, wird der Fehlereintrag aus der **Fehlerliste** entfernt.  
  
-   Wenn Sie die Registerkarte für eine [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skriptdatei schließen, werden die Fehler in dieser Datei aus der **Fehlerliste** entfernt.  
  
## <a name="working-with-the-error-list"></a>Arbeiten mit der Fehlerliste  
 Führen Sie zum Anzeigen der **Fehlerliste** eine der folgenden Aktionen aus:  
  
-   Klicken Sie im Menü **Ansicht** auf **Fehlerliste**.  
  
-   Drücken Sie die Tastenkombination STRG+\\, STRG+E.  
  
 Nachdem Sie die **Fehlerliste** geöffnet haben, können Sie die Sicht anpassen, indem Sie die folgenden Aktionen ausführen:  
  
-   Zum Sortieren der Liste klicken Sie auf einen beliebigen Spaltenheader. Um erneut nach einer zusätzlichen Spalte zu sortieren, halten Sie die UMSCHALTTASTE gedrückt, und klicken Sie dann auf einen anderen Spaltenheader.  
  
-   Um auszuwählen, welche Spalten angezeigt und welche ausgeblendet werden sollen, wählen Sie im Kontextmenü **Spalten anzeigen** aus.  
  
-   Zum Ändern der Reihenfolge, in der Spalten angezeigt werden, ziehen Sie einen beliebigen Spaltenheader nach links oder rechts.  
  
 Die **Fehlerliste** enthält keine Links zu weiteren Informationen zu bestimmten Fehlern.  
  
## <a name="transact-sql-errors-in-management-studio"></a>Transact-SQL-Fehler in Management Studio  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] werden Fehler bei [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skripts an den folgenden Speicherorten angezeigt:  
  
-   Die **Fehlerliste** enthält alle von IntelliSense im [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Editor gefundenen Syntax- und Semantikfehler. Die Fehlerliste wird dynamisch aktualisiert, während Sie [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skripts bearbeiten. In der Liste sind alle Fehler aufgeführt, die in einem [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skript gefunden wurden. Die Analyse einer Datei wird nicht beendet, wenn Fehler in einem Skript gefunden wurden. In [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]unterstützt IntelliSense im [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Editor nicht alle [!INCLUDE[tsql](../../includes/tsql-md.md)] -Syntaxelemente. Die **Fehlerliste** enthält nur Fehler in der [!INCLUDE[tsql](../../includes/tsql-md.md)] -Syntax, die von IntelliSense unterstützt wird.  
  
-   Auf der Registerkarte **Meldungen** am unteren Rand des [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editorfensters werden alle Fehler und Warnungen angezeigt, die bei der Ausführung eines [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] -Skripts von [!INCLUDE[tsql](../../includes/tsql-md.md)] zurückgegeben wurden. Diese Liste ändert sich nicht, bis Sie das Skript erneut ausführen. [!INCLUDE[ssDE](../../includes/ssde-md.md)] beendet die Analyse eines Batchs, wenn ein oder zwei Kompilierungsfehler gefunden wurden. Daher werden auf der Registerkarte **Meldungen** möglicherweise nicht alle in einem Skript gefundenen Fehler aufgelistet.  
  
 Manchmal werden Fehler an beiden Orten aufgeführt. Zum Beispiel könnte in einer Skriptdatei ein Syntaxfehler vorliegen, der in der **Fehlerliste** aufgeführt wird. Wenn Sie das Skript ausführen, bevor Sie den Fehler behoben haben, findet der [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Parser möglicherweise denselben Fehler und gibt auf der Registerkarte **Meldungen** die Fehlermeldung erneut zurück.  
  
> [!NOTE]  
>  In der **Fehlerliste** werden nur Fehler aus dem [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor angezeigt. Fehler aus dem MDX-, DMX- oder XML/A-Editor werden in der Liste nicht angezeigt. Alle MDX-, DMX- und XML/A-Fehler werden in den entsprechenden Editoren auf der Registerkarte **Meldungen** angezeigt.  
  
## <a name="ui-element-list"></a>Liste der Benutzeroberflächenelemente  
 Wenn die **Fehlerliste** geöffnet ist, werden die Informationen in den folgenden Spalten angezeigt:  
  
 **Standardreihenfolge**  
 Zeigt eine ganze Zahl an, die die Reihenfolge angibt, in der ein Eintrag generiert wurde.  
  
 **Beschreibung**  
 Zeigt den Text des Fehlereintrags an. Lange Beschreibungen werden in zusätzliche Zeilen umbrochen.  
  
 **File**  
 Zeigt den Namen der Skriptdatei an, die den Fehler generiert hat.  
  
 **Linie**  
 Zeigt eine ganze Zahl an, die angibt, in welcher Codezeile der Fehler enthalten ist.  
  
 **Spalte**  
 Zeigt eine ganze Zahl an, die die Position des Fehlers in der Codezeile angibt.  
  
 **Projekt**  
 Zeigt den Namen des Projekts an, in dem die angegebene Skriptdatei enthalten ist.  
