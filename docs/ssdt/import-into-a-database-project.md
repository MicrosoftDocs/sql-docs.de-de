---
title: Importieren in ein Datenbankprojekt
description: In diesem Artikel wird erläutert, wie Sie Objekte aus einer Livedatenbank, einer Datenschichtanwendung oder einem Skript in ein Datenbankprojekt importieren. Dabei erfahren Sie mehr über das Importieren von verschlüsselten Objekten.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- SQL.DATA.TOOLS.SQLPROJECTIMPORTSNAPSHOTSUMMARYDIALOG.DIALOG
- SQL.DATA.TOOLS.SQLPROJECTIMPORTDATABASESUMMARYDIALOG.DIALOG
- SQL.DATA.TOOLS.IMPORTSCRIPTWIZARD.SUMMARY
ms.assetid: d0a0a394-6cb6-416a-a25f-9babf8ba294a
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 0820f680382d7a126d2837c653ab7870defeb0ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017930"
---
# <a name="import-into-a-database-project"></a>Importieren in ein Datenbankprojekt

Mithilfe von Importieren können Sie ein Projekt mit neuen Objekten aus einer Livedatenbank oder einer DACPAC-Datei auffüllen bzw. im Projekt vorhandene Objekte anhand einer neuen Definition aus einem Skript aktualisieren. Bei den drei nachfolgend beschriebenen Methoden sind einige Verhaltensunterschiede zu beachten.  
  
**Menü „Importieren“**  
  
![Menü „Importieren“ in SSDT](../ssdt/media/ssdt-import.gif "Menü „Importieren“ in SSDT")  
  
**Abschnitte in diesem Thema**  
  
[Importquelle: Datenbank oder Datenschichtanwendung (*.dacpac)](#bkmk_import_source_db)  
  
[Importquelle: Skript (*.sql)](#bkmk_import_source_script)  
  
[Importieren verschlüsselter Objekte](#bkmk_import_encrypted)  
  
## <a name="import-source-database-or-data-tier-application-dacpac"></a><a name="bkmk_import_source_db"></a>Importquelle: Datenbank oder Datenschichtanwendung (*.dacpac)  
Die Fähigkeit, ein Schema aus einer Datenbank oder DACPAC-Datei zu importieren, ist nur verfügbar, wenn noch keine Schemaobjekte im Projekt definiert sind. Davon ausgeschlossen sind RefactorLog-Dateien oder Skripts vor und nach der Bereitstellung.  
  
Beim Import werden für Objektdefinitionen Skripts in Projektdateien erstellt, wobei die in SSDT enthaltenen betrieblichen Standardwerte für neue Objekte verwendet werden: neue Dateien für allgemeine Objekte, untergeordnete Hierarchieelemente, die in derselben Datei wie das übergeordnete Element definiert sind, oder inline definierte Tabellen- oder Spalteneinschränkungen (sofern möglich). Um die Sichtbarkeit und Steuerung der einzelnen Objekte zu verbessern, verwenden Sie anstelle von Importieren die Option Schemavergleich.  
  
Wenn die Importquelle Skripts vor und nach der Bereitstellung, RefactorLog-Dateien oder Definitionen von SQLCMD-Variablen enthält, werden diese in das Projekt importiert. Wenn eines dieser Artefakte bereits im Projekt enthalten ist, werden die importierten Dateien einem Projektordner namens **Beim Import ignoriert** hinzugefügt.  
  
**Ordner „Beim Import ignoriert“**  
  
![SSDT – Ordner für beim Import zu ignorierende Elemente](../ssdt/media/ssdt-ignoredonimport.gif "SSDT – Ordner für beim Import zu ignorierende Elemente")  
  
## <a name="import-source-script-sql"></a><a name="bkmk_import_source_script"></a>Importquelle: Skript (*.sql)  
Alle Objekte aus der Importquelle, die *noch nicht* im Projekt enthalten sind, werden hinzugefügt. Demgegenüber wird die Objektdefinition von allen Objekten aus der Importquelle überschrieben, die *bereits* im Projekt vorhanden sind.  
  
> [!NOTE]  
> Bei dieser Methode treten zwei bekannte Fehler auf, die in zukünftigen Versionen behoben sein werden:  
>   
> -   Wenn Tabellen- oder Spalteneinschränkungen außerhalb der CREATE TABLE-Anweisung in der Tabellendefinition des Projekts definiert sind, wird die Tabellendefinition beim Import überschrieben, sodass die Einschränkung inline ist. Dabei bleibt die Out-of-Line-Einschränkung jedoch bestehen, was zu doppelten Einschränkungen im Projekt führt.  
> -   Alle Masterschlüssel oder Datenbankverschlüsselungsschlüssel aus dem Quellskript, die bereits im Projekt vorhanden sind, werden beim Import dupliziert. Entfernen Sie Duplikate, um das Projekt zu erstellen.  
  
Beim Import aus Skripts können Skripts vor und nach der Bereitstellung, SQLCMD-Variablen oder RefactorLog-Dateien nicht ausgewertet werden. Diese und andere nicht unterstützte Konstrukte, die beim Import erkannt werden, werden projektweise in einer Datei **ScriptsIgnoredOnImport.sql** im Ordner **Skripts** abgelegt.  
  
 
## <a name="import-encrypted-objects"></a><a name="bkmk_import_encrypted"></a>Importieren verschlüsselter Objekte  
Beim Importieren verschlüsselter Objekte in ein Datenbankprojekt kann der vollständige Text der Objektdefinition nicht immer vom Server abgerufen werden. Somit kann das Importverhalten bei der Verwendung dieser Objektklasse abweichen.  
  
Wenn der vollständige Definitionstext nicht abgerufen werden kann, werden für den Objektheader/-footer Skripts zusammen mit einem Pseudotext erstellt. Mit diesem Verhalten ist zu rechnen, wenn Sie Importieren oder Schemavergleich verwenden und es sich bei der Quelle um eine Livedatenbank oder eine aus einer Datenbank extrahierte DACPAC-Datei handelt.  
  
**Skript mit einem Pseudotext**  
  
![Skript mit einem Pseudotext](../ssdt/media/ssdt-procwithencryption.gif "Skript mit einem Pseudotext")  
  
Wenn die vollständige Objektdefinition verfügbar ist und abgerufen werden kann, wird sie bei Verwendung von Importieren und Schemavergleich vollständig in das Projekt übernommen. Dies ist der Fall, wenn das Projekt anhand eines Skripts aktualisiert bzw. eine DACPAC-Datei aus einem Datenbankprojekt oder einem anderen Projekt erstellt wird.  
  
