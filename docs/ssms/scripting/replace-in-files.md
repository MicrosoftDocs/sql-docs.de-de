---
title: In Dateien ersetzen
description: In diesem Artikel erfahren Sie, wie Sie mit dem Feature „Replace in Files“ (In Dateien ersetzen) im Fenster „Suchen und Ersetzen“ den Text in mehreren Dateien nach einer Zeichenfolge oder einem Ausdruck durchsuchen und den gefundenen Text teilweise oder vollständig ändern.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Replace in Files dialog box
ms.assetid: 51191c0a-e022-41d6-8473-5cb3c6596862
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5f220d9d64221bb02c577547b645c28a03a5b044
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354563"
---
# <a name="replace-in-files"></a>In Dateien ersetzen
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Die Registerkarte **In Dateien ersetzen** des Fensters zum Suchen und Ersetzen ermöglicht es Ihnen, den Code eines angegebenen Satzes von Dateien nach einer Zeichenfolge oder einem Ausdruck zu durchsuchen und einige oder alle der übereinstimmenden Stellen zu ändern. Die gefundenen Übereinstimmungen und ausgeführten Aktionen werden in dem unter **Ergebnisoptionen** ausgewählten Suchergebnisfenster aufgelistet.  
  
 Das Dialogfeld **Suchen und Ersetzen** kann auch über Symbolleistenschaltflächen und Tastenkombinationen geöffnet werden.  
  
## <a name="find-what"></a>Suchen nach  
 Die folgenden Steuerelemente auf der Registerkarte **In Dateien ersetzen** ermöglichen es Ihnen, die Zeichenfolge oder den Ausdruck anzugeben, für die/den nach Übereinstimmungen gesucht werden soll.  
  
 **Suchen nach**  
 Geben Sie den Suchtext ein. Im Dialogfeld wird ein wahrscheinlicher Suchtext eingetragen. Dabei wird entweder auf den Text zurückgegriffen, der vor dem Öffnen des Dialogfelds mit dem Cursor markiert wurde, oder auf in der Nähe befindlichen Text oder auf zuvor verwendeten Suchtext. Sie können aus der Dropdownliste eine der letzten 20 Suchzeichenfolgen zur Wiederverwendung auswählen.  
  
 **[Zeichenfolge mit Platzhaltern]**  
 Wenn Sie Platzhalter wie Sternchen (`*`) und Fragezeichen (`?`) in den Suchzeichenfolgen verwenden möchten, aktivieren Sie unter **Suchoptionen** das Kontrollkästchen **Mit** , und klicken Sie dann auf **Platzhalter**.  
  
 **[regulärer Ausdruck]**  
 Wenn die Such-Engine die Suchzeichenfolge als regulären Ausdruck interpretieren soll, aktivieren Sie unter **Suchoptionen** das Kontrollkästchen **Mit**, und klicken Sie anschließend auf **Reguläre Ausdrücke**.  
  
 **Ausdrucks-Generator**  
 Die dreieckige Schaltfläche neben dem Feld **Suchen nach** steht zur Verfügung, nachdem das Kontrollkästchen **Mit** in den **Suchoptionen** aktiviert wurde. Klicken Sie auf diese Schaltfläche, um in Abhängigkeit von der für **Mit** gewählten Option eine Liste von Platzhaltern oder regulären Ausdrücken anzuzeigen. Ein aus dieser Liste ausgewähltes Element wird der Zeichenfolge hinzugefügt, die unter **Suchen nach** angegeben wurde.  
  
## <a name="replace-with"></a>Ersetzen durch  
 Mithilfe dieser Steuerelemente können Sie angeben, was anstelle der übereinstimmenden Zeichenfolge bzw. des übereinstimmenden Ausdrucks eingefügt werden soll.  
  
 **Replace with**  
 Um Instanzen der in **Suchen nach** angegebenen Zeichenfolge mit einer anderen zu ersetzen, geben Sie in diesem Feld die Zeichenfolge ein, die sie ersetzen soll. Um Instanzen der in **Suche nach** angegebenen Zeichenfolge zu löschen, lassen Sie dieses Feld leer. Wählen Sie die Dropdownliste aus, um die letzten 20 Eingaben anzuzeigen. Um reguläre Ausdrücke in die im Feld **Ersetzen durch** angegebene Zeichenfolge einzubeziehen, aktivieren Sie das Kontrollkästchen **Mit** , und klicken Sie auf die Option **Reguläre Ausdrücke** .  
  
 **Ausdrucks-Generator**  
 Die dreieckige Schaltfläche neben dem Feld **Ersetzen durch** steht zur Verfügung, nachdem das Kontrollkästchen **Mit** in den **Suchoptionen** aktiviert wurde. Klicken Sie auf diese Schaltfläche, um in Abhängigkeit von der für **Mit** gewählten Option eine Liste von Platzhaltern oder regulären Ausdrücken anzuzeigen. Ein aus dieser Liste ausgewähltes Element wird der Zeichenfolge hinzugefügt, die unter **Ersetzen durch** angegeben wurde.  
  
 **Replace**  
 Klicken Sie auf diese Schaltfläche, um die aktuelle Instanz der in **Suchen nach** angegebenen Zeichenfolge mit jener zu ersetzen, die im Feld **Ersetzen durch** angegeben ist, und um die nächste Instanz innerhalb des Bereichs zu suchen, der in **Suchen in** angegeben ist.  
  
 **Alle ersetzen**  
 Klicken Sie auf diese Schaltfläche, um alle Instanzen der in **Suchen nach** angegebenen Zeichenfolge in allen Dateien innerhalb des unter **Suchen in** angegebenen Bereichs mit jener zu ersetzen, die im Feld **Ersetzen durch** angegeben ist.  
  
> [!CAUTION]  
>  Stellen Sie sicher, dass der unter **Suchen in** festgelegte Bereich nur die Dateien umfasst, die Sie ändern möchten.  
  
 Es wird eine Erinnerung mit der Option **Geänderte Dateien geöffnet lassen** angezeigt. Um die Option **Rückgängig** beizubehalten, müssen Sie diese Option auswählen. Die Option **Rückgängig** ist nur in Dateien verfügbar, die nach einer Änderung geöffnet bleiben.  
  
 **Datei überspringen**  
 Wird verfügbar, wenn **Suchen in** mehrere Dateien umfasst. Klicken Sie auf diese Schaltfläche, wenn Sie die aktuelle Datei nicht durchsuchen oder ändern möchten. Die Suche wird dann in der nächsten Datei auf der Liste unter **Suchen in** fortgesetzt.  
  
## <a name="look-in"></a>Suchen in  
 Über die aus der Dropdownliste **Suchen in** ausgewählte Option legen Sie fest, ob die Funktion **In Dateien ersetzen** nur die zurzeit aktiven Dateien oder alle in bestimmten Ordnern gespeicherten Dateien durchsucht. Wählen Sie einen Suchbereich aus der Liste aus, geben Sie einen Ordnerpfad ein, oder klicken Sie auf die Schaltfläche **Durchsuchen** , um das Dialogfeld **Suchordner auswählen** anzuzeigen und einen Satz zu durchsuchender Ordner auszuwählen.  
  
> [!NOTE]  
>  Wenn die für **Suchen in** ausgewählte Option eine aus der Quellcodeverwaltung ausgecheckte Datei durchsuchen soll, wird nur die Version der Datei durchsucht, die auf den lokalen Computer heruntergeladen wurde.  
  
 **Look in**  
 Wählen Sie aus dieser Liste einen vordefinierten Suchbereich, oder geben Sie über das Dialogfeld **Suchordner auswählen** eine Gruppe von Verzeichnissen an.  
  
 **Aktuelles Dokument**  
 Diese Option steht zur Verfügung, wenn ein Dokument in einem Editor geöffnet ist. Dabei wird nur das aktive Dokument nach der unter **Suchen nach** angegebenen Zeichenfolge durchsucht.  
  
 **Alle offenen Dokumente**  
 Durchsucht alle zurzeit zum Bearbeiten geöffneten Dokumente.  
  
 **Aktuelles Projekt**  
 Durchsucht alle Dateien im aktuellen Projekt.  
  
 **Gesamte Projektmappe**  
 Durchsucht alle Dateien in der aktiven Projektmappe.  
  
 **Unterordner einbeziehen**  
 Gibt an, dass die Unterordner des unter **Suchen in** angegebenen Ordners durchsucht werden. Dazu ist ein benutzerdefinierter Verzeichnissatz erforderlich.  
  
 **Durchsuchen (…)**  
 Klicken Sie auf diese Schaltfläche, um das Dialogfeld **Suchordner auswählen** anzuzeigen. In diesem Dialogfeld können Sie benannte Verzeichnissätze zusammenstellen, bearbeiten, speichern und auswählen, die Sie anschließend im Feld **Suchen in** eingeben können.  
  
## <a name="find-options"></a>Mit  
 Sie können den Abschnitt **Suchoptionen** reduzieren oder erweitern. Die folgenden Optionen können aktiviert oder deaktiviert werden.  
  
 **Groß-/Kleinschreibung beachten**  
 Wenn dieses Kontrollkästchen aktiviert ist, werden im Suchergebnisfenster nur Instanzen der unter **Suchen nach** angegebenen Zeichenfolge angezeigt, bei denen neben der inhaltlichen Übereinstimmung auch die Groß-/Kleinschreibung identisch ist. Eine Suche nach **MyObject** bei aktiviertem Kontrollkästchen **Groß-/Kleinschreibung beachten** gibt "MyObject" nicht aber "myobject" oder "MYOBJECT" zurück.  
  
 **Nur ganzes Wort suchen**  
 Wenn dieses Kontrollkästchen aktiviert ist, werden im Suchergebnisfenster nur Instanzen der unter **Suchen nach** angegebenen Zeichenfolge angezeigt, bei denen jeweils das ganze Wort übereinstimmt. Eine Suche nach **MyObject** gibt beispielsweise "MyObject", nicht aber "CMyObject" oder "MyObjectC" zurück.  
  
 **Verwenden Sie**  
 Gibt an, wie in den Textfeldern **Suchen nach** oder **Ersetzen durch** eingegebene Sonderzeichen interpretiert werden sollen. Zur Auswahl stehen **Platzhalter** und **Reguläre Ausdrücke**.  
  
 **Regular Expressions**  
 Mithilfe spezieller Notationen werden zu suchende Textmuster definiert. Eine Liste finden Sie unter [Suchen von Text mit regulären Ausdrücken](./search-text-with-regular-expressions.md).  
  
 **Platzhalter**  
 Sonderzeichen, wie Sternchen (`*`) und Fragezeichen (`?`), stellen ein oder mehrere Zeichen dar. Eine Liste finden Sie unter [Suchen von Text mit Platzhaltern](./search-text-with-wildcards.md).  
  
 **Nach diesen Dateitypen suchen**  
 Diese Liste gibt die Dateitypen an, die in den unter **Suchen in** angegebenen Verzeichnissen durchsucht werden sollen. Wird dieses Feld leer gelassen, werden alle Dateien in den unter **Suchen in** angegebenen Verzeichnissen durchsucht.  
  
```  
*.[ext]; *.[ext] (manual)  
```  
  
 Um nach Dateien eines bestimmten Typs zu suchen, geben Sie ein Sternchen-Platzhalter (`*`) für den Dateinamen, gefolgt von einem Punkt (`.`) und der Dateierweiterung ein. Wenn Sie nach mehreren Dateitypen suchen möchten, geben Sie mehrere Suchzeichenfolgen getrennt durch Semikolons (`;`) ein.  
  
```  
*.[ext]; *.[ext] (from list)  
```  
  
 Wählen Sie ein beliebiges Element in der Liste aus, um eine vorkonfigurierte Suchzeichenfolge einzugeben, die nach Dateien eines bestimmten Typs sucht.  
  
## <a name="result-options"></a>Ergebnisoptionen  
 Sie können den Abschnitt **Ergebnisoptionen** reduzieren oder erweitern. Die folgenden Optionen können aktiviert oder deaktiviert werden.  
  
 **Fenster "Suchergebnisse 1"**  
 Wenn dieses Kontrollkästchen aktiviert ist, werden die Ergebnisse der aktuellen Suche an den Inhalt im Fenster "Suchergebnisse 1" angehängt. Dieses Fenster wird automatisch geöffnet, um die Suchergebnisse anzuzeigen. Zum manuellen Öffnen des Fensters klicken Sie im Menü **Ansicht** auf **Weitere Fenster** und dann auf **Suchergebnisse 1**.  
  
 **Ergebnisse suchen: 2**  
 Wenn dieses Kontrollkästchen aktiviert ist, werden die Ergebnisse der aktuellen Suche an den Inhalt im Fenster Suchergebnisse 2 angehängt. Dieses Fenster wird automatisch geöffnet, um die Suchergebnisse anzuzeigen. Zum manuellen Öffnen des Fensters klicken Sie im Menü **Ansicht** auf **Weitere Fenster** und dann auf **Suchergebnisse 2**.  
  
 **Nur Dateinamen anzeigen**  
 Zeigt einen Eintrag je Datei an, die eine Übereinstimmung für die Suchbedingung enthält, statt einen Eintrag je Treffer entweder im Fenster Suchergebnisse 1 oder Suchergebnisse 2. Diese Option steht in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]nicht zur Verfügung.  
  
 **Geänderte Dateien offen lassen, nachdem alles ersetzt wurde**  
 Wenn diese Option aktiviert ist, bleiben alle Dateien geöffnet, in denen Ersetzungen vorgenommen wurden. Auf diese Weise können Sie die Änderungen rückgängig machen oder speichern. Speichereinschränkungen können eine Begrenzung der Anzahl der Dateien, die nach einem Ersetzungsvorgang geöffnet bleiben, zur Folge haben.  
  
> [!CAUTION]  
>  Sie können die Funktion **Rückgängig** nur bei Dateien verwenden, die zum Bearbeiten geöffnet bleiben. Wenn diese Option nicht aktiviert ist, bleiben Dateien, die nicht bereits zum Bearbeiten geöffnet waren, geschlossen, und die Option **Rückgängig** steht für diese Dateien nicht zur Verfügung.  
  
## <a name="find-and-replace-views"></a>Sichten beim Suchen und Ersetzen  
 Die Registerkarten oben im Fenster Suchen und Ersetzen beinhalten Menüs, die als **Sichten** bezeichnet werden. Mithilfe dieser Menüs können Sie eine Gruppe von Feldern auswählen, die im aktiven Bereich angezeigt werden. Sie können das Fenster Suchen und Ersetzen an einem für Sie geeigneten Platz angedockt lassen und dann zwischen den Registerkarten und Sichten wechseln, um einen beliebigen Typ eines Such- oder Ersetzungsvorgangs auszuführen.  
  
 **Zur Schnellsuche wechseln**  
 Über diese Symbolleisten-Registerkarte wechseln Sie zum Dialogfeld **Schnellsuche** .  
  
 **Zur Suche in Dateien wechseln**  
 Über diese Symbolleisten-Registerkarte wechseln Sie zum Dialogfeld **In Dateien suchen** .  
  
 **Zur Symbolsuche wechseln**  
 Über diese Symbolleisten-Registerkarte wechseln Sie zum Dialogfeld **Symbol suchen** .  
  
## <a name="see-also"></a>Weitere Informationen  
 [Tastenkombinationen für SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)