---
description: Änderungsskript speichern (Dialogfeld) (Visual Database Tools)
title: Speichern des Änderungsskripts (Dialogfeld)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.dlgbox.generatechangescript
- vdtsql.chm:65544
ms.assetid: fc9d1639-5efa-44fe-a04f-4d4d0def2833
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 7388283d4605547a400c284c2499e4f0d9c4db46
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88468335"
---
# <a name="save-change-script-dialog-box-visual-database-tools"></a>Änderungsskript speichern (Dialogfeld) (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Dieses Dialogfeld zeigt das [!INCLUDE[tsql](../../includes/tsql-md.md)] -Skript für die Änderungen an, die Sie seit der letzten Speicherung der Tabelle vorgenommen haben. Außerdem können Sie in diesem Dialogfeld das Skript an einem auszuwählenden Speicherort als Textdatei speichern.  
  
Sie können auf dieses Dialogfeld nach dem Vornehmen nicht gespeicherter Änderungen einer Tabelle im Tabellen-Designer zugreifen. Klicken Sie im Menü **Tabellen-Designer** auf **Änderungsskript generieren**.  
  
> [!NOTE]  
> Änderungsskripts, die von Visual Database Tools zur Verfügung gestellt werden, enthalten keine Fehlerbehandlung. Dabei wird angenommmen, dass seit dem Öffnen des Tools keine Datenbankobjekte geändert wurden, und dass deshalb keine änderungsbezogenen Probleme auftreten werden. Vor dem Ausführen des Änderungsskripts sollten Sie die entsprechenden Fehlerbehandlungsanweisungen einschließen.  
  
## <a name="options"></a>Optionen  
**Änderungsskript automatisch nach jedem Speichern erstellen**  
Ist dieses Kontrollkästchen aktiviert, wird das Dialogfeld **Änderungsskript speichern** jedes Mal angezeigt, wenn Sie Änderungen an einer Tabelle speichern.  
  
**Ja**  
Öffnen Sie das Dialogfeld **Speichern** , mit dem Sie den Speicherort für die Textdatei auswählen können.  
  
**Nein**  
Brechen Sie die Erstellung des Änderungsskripts ab.  
  
