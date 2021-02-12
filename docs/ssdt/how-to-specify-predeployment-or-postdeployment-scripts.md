---
title: Angeben von Skripts vor oder nach der Bereitstellung
description: Hier erfahren Sie, wie Sie Skripts vor und nach der Bereitstellung verwenden, um Transact-SQL-Anweisungen vor und nach der Ausführung des Hauptbereitstellungsskripts auszuführen.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 7f78f517-f13d-4f4b-84b9-e804cb490b2c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d4565e9b6ab4efc133aef84ff9d4d6dbbd6c501a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018110"
---
# <a name="how-to-specify-predeployment-or-postdeployment-scripts"></a>Gewusst wie: Festlegen von Skripts vor und nach der Bereitstellung

Skripts vor und nach der Bereitstellung führen Transact\-SQL-Anweisungen vor und nach dem Hauptbereitstellungsskript aus, das vom Datenbankprojekt generiert wird. Das Skript vor der Bereitstellung wird beim Aktualisieren von Zielen aus den Ergebnissen des Schemavergleichs in Visual Studio nicht ausgeführt. Ein Projekt kann nur ein Skript vor und ein Skript nach der Bereitstellung aufweisen. Diese Skripts können für mehrere Zwecke verwendet werden. Beispiel:  
  
-   Ein Skript vor der Bereitstellung kann Daten aus einer Tabelle kopieren, die in eine temporäre Tabelle geändert wird, bevor die Daten neu formatiert und auf die geänderte Tabelle in einem Skript nach der Bereitstellung angewendet werden.  
  
-   Sie können Verweisdaten einfügen, die in einer Tabelle in einem Skript nach der Bereitstellung vorhanden sein müssen. Bevor Sie die Daten einfügen, können Sie testen, ob die Tabelle bereits Daten enthält. Wenn die Tabelle Daten enthält, müssen Sie diese Daten löschen bzw. angeben, dass die Datenbank vor der Bereitstellung immer neu erstellt werden soll. Sie können dem Skript nach der Bereitstellung Anweisungen wie die folgende hinzufügen:  
  
```  
IF (EXISTS(SELECT * FROM [dbo].[MyReferenceTable]))  
BEGIN  
    DELETE FROM [dbo].[MyReferenceTable]  
END  
```  

## <a name="to-add-and-modify-a-pre--or-post-deployment-script"></a>So fügen Sie ein Skript vor oder nach der Bereitstellung hinzu bzw. ändern es  
  
1.  Erweitern Sie Ihr Datenbankprojekt im **Projektmappen-Explorer**, um den Ordner „Skripts“ anzuzeigen.  
  
2.  Klicken Sie mit der rechten Maustaste auf den Ordner "Skripts", und wählen Sie "Hinzufügen" aus.  
  
3.  Wählen Sie "Skripts" im Kontextmenü aus.  
  
4.  Wählen Sie "Skript vor der Bereitstellung" oder "Skript nach der Bereitstellung" aus. Sie können auch einen nicht-standardmäßigen Namen eingeben. Klicken Sie auf "Hinzufügen", um den Vorgang abzuschließen.  
  
5.  Doppelklicken Sie auf die Datei im Ordner "Skripts".  
  
    Der Transact\-SQL-Editor wird geöffnet und zeigt den Inhalt der Datei an.  
  
Sie können SQLCMD-Syntax und -Variablen in Ihren Skripten verwenden und diese in den Eigenschaften des Datenbankprojekts festlegen. Beispiel:  
  
-   Sie können SQLCMD-Syntax verwenden, um den Inhalt einer Datei in ein Skript vor oder nach der Bereitstellung einzuschließen. Die Dateien werden in der Reihenfolge, in der Sie sie festlegen, eingeschlossen und ausgeführt: `:r .\myfile.sql`  
  
-   Sie können SQLCMD-Syntax verwenden, um auf eine Variable im Skript nach der Bereitstellung zu verweisen. Sie können die SQLCMD-Variable in den Projekteigenschaften oder in einem Veröffentlichungsprofil festlegen:  
  
    ```  
    :setvar TableName MyTable  
    SELECT * FROM [$(TableName)]  
    ```  
  
Weitere Informationen zur Verwendung von SQLCMD in Skripts finden Sie unter [Datenbankprojekteinstellungen](../ssdt/database-project-settings.md).  
  
## <a name="see-also"></a>Weitere Informationen  
[Projektorientierte Offlinedatenbankentwicklung](../ssdt/project-oriented-offline-database-development.md)  
  
