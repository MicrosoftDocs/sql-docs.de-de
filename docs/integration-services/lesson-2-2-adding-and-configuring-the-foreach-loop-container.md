---
description: 'Lektion 2-2: Hinzufügen und Konfigurieren des Foreach-Schleifencontainers'
title: 'Schritt 2: Hinzufügen und Konfigurieren des Foreach-Schleifencontainers | Microsoft-Dokumentation'
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 88a973cc-0f23-4ecf-adb6-5b06279c2df6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8c63288019fa6938f4481bbded2e5b29c29198d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354314"
---
# <a name="lesson-2-2-add-and-configure-the-foreach-loop-container"></a>Lektion 2-2: Hinzufügen und Konfigurieren des Foreach-Schleifencontainers

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



In dieser Aufgabe fügen Sie die Möglichkeit zum Schleifendurchlauf für einen Ordner von Flatfiles hinzu und wenden die Datenflusstransformation aus Lektion 1 auf jede dieser Flatfiles an. Dies geschieht durch das Hinzufügen eines Foreach-Schleifencontainers zur Ablaufsteuerung und dessen Konfigurierung.  
  
Für den von Ihnen hinzugefügten Foreach-Schleifencontainer muss es möglich sein, eine Verbindung mit jeder Flatfile im Ordner herzustellen. Da alle Dateien im Ordner das gleiche Format aufweisen, kann vom Foreach-Schleifencontainer der gleiche Flatfile-Verbindungs-Manager zum Herstellen einer Verbindung mit jeder dieser Dateien verwendet werden. Der Flatfile-Verbindungs-Manager, den der Container verwendet wird, ist derjenige, den Sie in Lektion 1 erstellt haben.  
  
Zurzeit wird vom Flatfile-Verbindungs-Manager aus Lektion 1 nur eine Verbindung mit einer bestimmten Flatfile hergestellt. Um iterativ Verbindungen mit jedem Flatfile im Ordner herzustellen, müssen Sie sowohl den Foreach-Schleifencontainer als auch den Flatfile-Verbindungs-Manager wie folgt konfigurieren:  
  
-   **Foreach-Schleifencontainer:** Sie ordnen den aufgezählten Wert des Containers einer benutzerdefinierten Paketvariablen zu. Der Container verwendet dann diese Variable, um die **ConnectionString**-Eigenschaft des Flatfile-Verbindungs-Managers dynamisch zu ändern und iterativ Verbindungen mit jeder Flatfile im Ordner herzustellen.  
  
-   **Verbindungs-Manager für Flatfiles:** Sie ändern den Verbindungs-Manager, der in Lektion 1 erstellt wurde, indem Sie eine benutzerdefinierte Variable zum Auffüllen der **ConnectionString**-Eigenschaft des Verbindungs-Managers verwenden.  
  
Die Vorgänge in diesem Task verdeutlichen, wie Sie den Foreach-Schleifencontainer erstellen und ändern, um eine benutzerdefinierte Paketvariable zu verwenden, und wie der Schleife der Datenflusstask hinzugefügt wird. Sie erfahren, wie der Flatfile-Verbindungs-Manager so geändert wird, dass er diese benutzerdefinierte Variable in der nächsten Aufgabe verwendet.  
  
Nachdem Sie diese Änderungen am Paket vorgenommen haben, iteriert der Foreach-Schleifencontainer beim Ausführen des Pakets durch alle Dateien im Ordner „Sample Data“. Jedes Mal, wenn eine mit dem Kriterium übereinstimmende Datei gefunden wird, wird die neue Variable vom Foreach-Schleifencontainer mit dem Dateinamen aufgefüllt, wird diese Variable zur **ConnectionString**-Eigenschaft des Flatfile-Verbindungs-Managers für Sample Currency Data zugeordnet und wird der Datenfluss anschließend für diese Datei ausgeführt. Dies führt dazu, dass bei jeder Iteration der Foreach-Schleife im Datenflusstask eine andere Flatfile verarbeitet wird.  
  
> [!NOTE]  
> Weil [!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] die Ablaufsteuerung vom Datenfluss trennt, erfordert keine der Schleifen, die Sie der Ablaufsteuerung hinzufügen, Änderungen am Datenfluss. Daher muss der Datenfluss aus Lektion 1 nicht geändert werden.  
  
## <a name="add-a-foreach-loop-container"></a>Hinzufügen eines Foreach-Schleifencontainers  
  
1.  Wählen Sie in **SQL Server Data Tools** die Registerkarte **Ablaufsteuerung** aus.  
  
2.  Erweitern Sie **Container** in der **SSIS-Toolbox**, und ziehen Sie anschließend **Foreach-Schleifencontainer** auf die Entwurfsoberfläche der Registerkarte **Ablaufsteuerung** .  
  
3.  Klicken Sie mit der rechten Maustaste auf den neuen **Foreach-Schleifencontainer**, und wählen Sie **Bearbeiten** aus.  
  
4.  Geben Sie im Dialogfeld **Foreach-Schleifen-Editor** auf der Seite **Allgemein** für **Name** den Namen **Foreach File in Folder** ein. Klicken Sie auf **OK**.  
  
5.  Klicken Sie mit der rechten Maustaste auf den Foreach-Schleifencontainer, wählen Sie **Eigenschaften** aus, und vergewissern Sie sich im **Eigenschaften**-Fenster, dass die Eigenschaft **LocaleID** auf **Englisch (USA)** festgelegt ist.  
  
## <a name="configure-the-enumerator-for-the-foreach-loop-container"></a>Konfigurieren des Enumerators für den Foreach-Schleifencontainer  
  
1.  Doppelklicken Sie auf **Foreach File in Folder**, um den **Foreach-Schleifen-Editor** erneut zu öffnen.  
  
2.  Wählen Sie **Sammlung** aus.  
  
3.  Wählen Sie auf der Seite **Sammlung** **Foreach-Dateienumerator** aus.  
  
4.  Wählen Sie in der Gruppe **Enumeratorkonfiguration** die Option **Durchsuchen** aus.  
  
5.  Suchen Sie im Dialogfeld **Ordner suchen** nach dem Ordner auf Ihrem Computer, der die „Currency_*.txt“-Dateien enthält, die zu den Beispieldateien gehören.

6.  Geben Sie in das Feld **Dateien** die Zeichenfolge **Currency_\*.txt** ein.  
  
## <a name="map-the-enumerator-to-a-user-defined-variable"></a>Zuordnen des Enumerators zu einer benutzerdefinierten Variablen  
  
1.  Wählen Sie **Variablenzuordnungen** aus.  
  
2.  Wählen Sie auf der Seite **Variablenzuordnungen** in der Spalte **Variable** die leere Zelle aus, und wählen Sie **\<New Variable...>** aus.  
  
3.  Geben Sie im Dialogfeld **Variable hinzufügen** für **Name** den Namen **varFileName** ein.  
  
    > [!NOTE]  
    > Bei Variablennamen wird nach Groß-/Kleinschreibung unterschieden.  
  
4.  Klicken Sie auf **OK**.  
  
5.  Wählen Sie erneut **OK** aus, um das Dialogfeld **Foreach-Schleifen-Editor** zu schließen.  
  
## <a name="add-the-data-flow-task-to-the-loop"></a>Hinzufügen des Datenflusstasks zur Schleife  
  
-   Ziehen Sie den Datenflusstask **Extract Sample Currency Data** in den Foreach-Schleifencontainer **Foreach File in Folder**.  
  
## <a name="go-to-next-task"></a>Weiter zur nächsten Aufgabe  
[Schritt 3: Ändern des Verbindungs-Managers für Flatfiles](../integration-services/lesson-2-3-modifying-the-flat-file-connection-manager.md)  
  
## <a name="see-also"></a>Weitere Informationen  
[Konfigurieren eines Foreach-Schleifencontainers](./control-flow/foreach-loop-container.md)  
[Verwenden von Variablen in Paketen](./integration-services-ssis-variables.md)  
  
  
