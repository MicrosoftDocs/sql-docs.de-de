---
title: Ändern von Ablaufverfolgungsvorlagen
titleSuffix: SQL Server Profiler
description: In diesem Artikel erfahren Sie, wie Sie Ablaufverfolgungsvorlagen im SQL Server Profiler bearbeiten. Außerdem erfahren Sie, wie Sie Ereignisklassen und Datenspalten hinzufügen und entfernen und Filter ändern.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 75b62a54-8d16-4599-bd2d-c42cfcc209f4
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 07/12/2017
ms.openlocfilehash: 0fca2ba1e0f8b1e979ff94c9c3900ec2f73c6292
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351094"
---
# <a name="modify-trace-templates"></a>Ändern von Ablaufverfolgungsvorlagen
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Sie können Vorlagen ändern, wenn diese in einer Datei auf dem lokalen Computer gespeichert sind und auf dem Computer [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] ausgeführt wird. Sie können auch Vorlagen ändern, die aus diesen Dateien abgeleitet wurden. Beim Ändern vorhandener Vorlagen bearbeiten Sie die Eigenschaften der Vorlage, wie Ereignisklassen und Datenspalten, in derselben Reihenfolge, in der die Eigenschaften ursprünglich festgelegt wurden. Dies geschieht über die Registerkarte **Ereignisauswahl** im Dialogfeld **Ablaufverfolgungseigenschaften** . Ereignisklassen und Datenspalten können hinzugefügt oder entfernt werden, und Filter können geändert werden. Nach dem Ändern der Vorlage wird eine benutzerspezifische Vorlage erstellt, und die ursprüngliche Systemvorlage bleibt unverändert. Weitere Informationen finden Sie unter [Speichern von Ablaufverfolgungen und Ablaufverfolgungsvorlagen](../../tools/sql-server-profiler/save-traces-and-trace-templates.md).  
  
 Möglicherweise müssen Sie eine Vorlage von einer vorhandenen Ablaufverfolgungsdatei ableiten, z. B. falls Sie vergessen haben, welche Vorlage ursprünglich zum Erstellen der Ablaufverfolgung verwendet wurde (oder wenn diese Datei nicht gespeichert wurde) oder falls Sie eine Ablaufverfolgung später erneut ausführen möchten. Bei vorhandenen Ablaufverfolgungen können Sie die Eigenschaften zwar anzeigen, aber nicht ändern. Um die Eigenschaften zu ändern, müssen Sie die Ablaufverfolgung beenden oder anhalten. Weitere Informationen finden Sie unter [Ableiten einer Vorlage von einer Ablaufverfolgungsdatei oder Ablaufverfolgungstabelle &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-trace-file-or-trace-table-sql-server-profiler.md) und [Ableiten einer Vorlage von einer zurzeit ausgeführten Ablaufverfolgung &#40;SQL Server Profiler&#41;](../../tools/sql-server-profiler/derive-a-template-from-a-running-trace-sql-server-profiler.md).  
  
## <a name="to-modify-a-trace-template"></a>So ändern Sie eine Ablaufverfolgungsvorlage  
  
1.  Zeigen Sie im Menü **Datei** auf **Vorlagen**, und klicken Sie dann auf **Vorlage bearbeiten**.  
  
2.  Im Dialogfeld **Eigenschaften der Ablaufverfolgungsvorlage** können Sie auf der Registerkarte **Allgemein** den Servertyp und den Vorlagennamen ändern oder eine Standardvorlage für den Servertyp auswählen.  
  
3.  Auf der Registerkarte **Ereignisauswahl** können Sie mit dem Rastersteuerelement der Ablaufverfolgungsdatei Ereignisse und Datenspalten wie folgt hinzufügen oder daraus entfernen:  
  
    -   Um ein Ereignis hinzuzufügen, erweitern Sie die entsprechende Ereigniskategorie in der **Ereignisse** -Spalte und wählen dann den Ereignisnamen aus.  
  
    -   Wenn Sie ein Ereignis hinzufügen, werden standardmäßig alle relevanten Datenspalten eingeschlossen. Um eine Datenspalte für ein Ereignis aus einer Ablaufverfolgung zu entfernen, deaktivieren Sie das Kontrollkästchen in der Datenspalte für das Ereignis.  
  
    -   Um Filter hinzuzufügen, klicken Sie auf den Namen der Datenspalte, und geben Sie die Filterkriterien im Dialogfeld **Filter bearbeiten** an. Sie können auch mit der rechten Maustaste auf den Namen der Datenspalte und anschließend auf **Spaltenfilter bearbeiten** klicken, um das Dialogfeld **Filter bearbeiten** zu öffnen. Klicken Sie auf **OK** , um den Filter hinzuzufügen.  
  
4.  Klicken Sie auf **Speichern** oder **Speichern unter**, um die Ablaufverfolgungsvorlage unter einem anderen Namen zu speichern.  
  
## <a name="next-steps"></a>Nächste Schritte  
[Starten einer Ablaufverfolgung](../../tools/sql-server-profiler/start-a-trace.md)  
[Erstellen einer Ablaufverfolgung](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)  
[Ändern einer vorhandenen Ablaufverfolgung mit Transact-SQL](../../relational-databases/sql-trace/modify-an-existing-trace-transact-sql.md)  
[Angeben von Ereignissen und Datenspalten für eine Ablaufverfolgung mit SQL Server Profiler](../../tools/sql-server-profiler/specify-events-and-data-columns-for-a-trace-file-sql-server-profiler.md)  
[sp-trace-setevent-transact-sql](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
