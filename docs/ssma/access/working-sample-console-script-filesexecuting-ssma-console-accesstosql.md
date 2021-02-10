---
description: Arbeiten mit dem Beispiel Konsolen Skript filesexecuting der SSMA-Konsole (accesstosql)
title: Arbeiten mit Beispiel Konsolen Skript filesexecuting der SSMA-Konsole | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: ad75b648-d119-4119-98f0-d18f058be68d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 519706bd731c9590676cfef3c0363f49ea903e8a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066046"
---
# <a name="working-with-the-sample-console-script-filesexecuting-the-ssma-console-accesstosql"></a>Arbeiten mit dem Beispiel Konsolen Skript filesexecuting der SSMA-Konsole (accesstosql)
Einige Beispieldateien wurden zusammen mit dem Produkt für die Benutzer Referenz und-Verwendung bereitgestellt. In diesem Abschnitt wird beschrieben, wie Sie diese Skripts problemlos an die Anforderungen der Endbenutzer anpassen können.  
  
## <a name="sample-console-script-files"></a>Beispieldateien für Konsolen Skripts  
Die folgenden Beispiel-Konsolen Skriptdateien, die verschiedene Szenarien abdecken, wurden als Benutzer Verweis bereitgestellt:  
  
-   ServersConnectionFileSample.xml  
  
-   VariableValueFileSample.xml  
  
-   AssessmentReportGenerationSample.xml  
  
-   ConversionAndDataMigrationSample.xml  
  
-   **ServersConnectionFileSample.xml:**  
  
    -   In diesem Beispiel werden die verschiedenen Verbindungs Modi für die Quell-und Zieldatenbank zur Verfügung gestellt, und der Benutzer kann je nach Anforderung einen beliebigen Modus auswählen. Dieses Beispiel enthält die Server Definitionen.  
  
    -   Der Benutzer kann eine Verbindung mit der erforderlichen Datenbank herstellen, indem er einfach die Werte in die erforderlichen Quell-und Zielserver Definitionen ändert. Im Beispiel wurden alle Werte als Variablen Werte bereitgestellt, die im **VariableValueFileSample.xml** verfügbar sind. Alle anderen Verbindungsparameter können aus der Verbindungs Datei des Arbeits Servers des Benutzers entfernt werden.  
  
    -   Weitere Informationen zum Herstellen einer Verbindung mit dem Quell-und Zielserver finden Sie unter [Erstellen der Server Verbindungs Dateien &#40;accesstosql&#41;](../../ssma/access/creating-the-server-connection-files-accesstosql.md) .  
  
-   **VariableValueFileSample.xml:** Alle Variablen, die in den Skriptdateien der Beispiel Konsole verwendet wurden und `ServersConnectionFileSample.xml` in dieser Datei sortiert wurden. Um die Beispiel Konsolen Skripts auszuführen, muss der Benutzer einfach die Beispiel Variablen Werte durch benutzerdefinierte Werte ersetzen und diese Datei als zusätzliches Befehlszeilenargument zusammen mit der Skriptdatei übergeben.  
  
    Weitere Informationen zu Variablen Wert Dateien finden Sie unter [Erstellen von Variablen Wert Dateien &#40;Access Token&#41;](../../ssma/access/creating-variable-value-files-accesstosql.md).  
  
-   **AssessmentReportGenerationSample.xml:** In diesem Beispiel kann der Benutzer einen XML-Bewertungsbericht generieren, der vom Benutzer für die Analyse verwendet werden kann, bevor er mit dem konvertieren und Migrieren von Daten beginnt.  
  
    Im `generate-assessment-report` Befehl muss der Benutzer den Variablen Wert (siehe **VariableValueFileSample.xml**) im- `object-name` Attribut auf den Datenbanknamen, der vom Benutzer verwendet wird, zwingend ändern. Abhängig von der Art des angegebenen Objekts muss der `object-type` Wert ebenfalls geändert werden.  
  
    Wenn der Benutzer mehrere Objekte/Datenbanken bewerten muss, kann er mehrere `metabase-object` Knoten angeben, wie im `generate-assessment-report` Beispiel 4 der Beispiel-Konsolen Skriptdatei im Beispiel 4 veranschaulicht.  
  
    Weitere Informationen zum Erstellen von Berichten finden Sie unter [Erstellen von Berichten &#40;accesstosql&#41;](../../ssma/access/generating-reports-accesstosql.md).  
  
    > [!NOTE]  
    > -   Stellen Sie sicher, dass das Befehlszeilenargument der Variablen Wert Datei an die Konsolenanwendung und VariableValueFileSample.xml mit den vom Benutzer angegebenen Werten aktualisiert wird.  
    > -   Stellen Sie sicher, dass das Befehlszeilenargument der Server Verbindungs Datei an die Konsolenanwendung übergeben wird und der ServersConnectionFileSample.xml mit den korrekten Server Parameterwerten aktualisiert wird.  
  
-   **ConversionAndDataMigrationSample.xml:** Dieses Beispiel ermöglicht dem Benutzer, eine End-to-End-Migration von der Konvertierung in die Datenmigration durchzuführen. Die Liste der obligatorischen Attributwerte, die geändert werden müssen, ist im folgenden aufgeführt:  
  
    |Befehlsname|BESCHREIBUNG|attribute|  
    |----------------|---------------|-------------|  
    |`map-schema`|Schema Zuordnung der Quelldatenbank zum Ziel Schema.|`source-schema:` Gibt die Quelldatenbank an, die für die Konvertierung erforderlich ist.<br /><br />`sql-server-schema`: Gibt die Zieldatenbank an, zu der migriert werden soll.|  
    |`convert-schema`|Führt eine Schema Konvertierung von der Quelle in das Ziel Schema durch.<br /><br />Wenn der Benutzer mehrere Objekte/Datenbanken bewerten muss, kann er mehrere `metabase-object` Knoten angeben, wie im `convert-schema` Beispiel 4 der Beispiel-Konsolen Skriptdatei im Beispiel 4 veranschaulicht.|`object-name`: Geben Sie die Quelldatenbank/den Objektnamen an, der für die Konvertierung erforderlich ist. Stellen Sie sicher, dass das entsprechende- `object-type` Objekt basierend auf dem Objekttyp geändert wird, der im `object-name`|  
    |`synchronize-target`|Synchronisiert die Zielobjekte mit der Zieldatenbank.<br /><br />Wenn der Benutzer mehrere Objekte/Datenbanken bewerten muss, kann er mehrere `metabase-object` Knoten angeben, wie im `synchronize-target` Beispiel 3 der Beispiel Konsolen Skriptdatei des Befehls dargestellt.|`object-name:` Geben Sie den SQL Server-Datenbank-/Objektnamen an, der erstellt werden soll. Stellen Sie sicher, dass das entsprechende- `object-type` Objekt basierend auf dem Objekttyp geändert wird, der im `object-name`|  
    |`migrate-data`|Migriert die Quelldaten zum Ziel.<br /><br />Wenn der Benutzer mehrere Objekte/Datenbanken bewerten muss, kann er mehrere `metabase-object` Knoten angeben, wie in `migrate-data` Beispiel 2 der Beispiel-Konsolen Skriptdatei des Befehls veranschaulicht.|`object-name:` Gibt den Namen der Quelldatenbank/-Tabelle an, die für die Migration erforderlich ist. Stellen Sie sicher, dass das entsprechende- `object-type` Objekt basierend auf dem Objekttyp geändert wird, der im `object-name`|  
  
## <a name="see-also"></a>Weitere Informationen  
[Erstellen von Variablen Wert Dateien &#40;Access Token&#41;](../../ssma/access/creating-variable-value-files-accesstosql.md)  
[Erstellen der Server Verbindungs Dateien &#40;Access Token&#41;](../../ssma/access/creating-the-server-connection-files-accesstosql.md)  
[Erstellen von Berichten &#40;Access Token&#41;](../../ssma/access/generating-reports-accesstosql.md)  
  
