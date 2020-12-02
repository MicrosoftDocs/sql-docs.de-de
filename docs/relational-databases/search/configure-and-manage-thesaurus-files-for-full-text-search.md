---
description: Konfigurieren und Verwalten von Thesaurusdateien für die Volltextsuche
title: Konfigurieren und Verwalten von Thesaurusdateien für die Volltextsuche
ms.date: 07/01/2020
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text indexes [SQL Server], thesaurus files
- thesaurus [full-text search], configuring
- thesaurus [full-text search]
ms.assetid: 3ef96a63-8a52-45be-9a1f-265bff400e54
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.custom: seo-lt-2019
ms.openlocfilehash: d713b4eb49a527f2cbbbf871cce9d01d4449443d
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130947"
---
# <a name="configure-and-manage-thesaurus-files-for-full-text-search"></a>Konfigurieren und Verwalten von Thesaurusdateien für die Volltextsuche
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Bei -Volltextabfragen kann ein *Thesaurus* der Volltextsuche verwendet werden, um nach Synonymen der vom Benutzer angegebenen Begriffe zu suchen. Jeder Thesaurus definiert Synonyme für eine bestimmte Sprache. Indem Sie einen Thesaurus entwickeln, der genau auf Ihre Volltextdaten abgestimmt ist, können Sie den Bereich der Volltextabfragen für diese Daten effektiv erweitern.

Der Thesaurusvergleich erfolgt für alle [FREETEXT](../../t-sql/queries/freetext-transact-sql.md)- und [FREETEXTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md)-Abfragen sowie für alle [CONTAINS](../../t-sql/queries/contains-transact-sql.md)- und [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md)-Abfragen, in denen die `FORMSOF THESAURUS`-Klausel angegeben ist.
  
Ein Thesaurus der Volltextsuche ist eine XML-Textdatei.
  
##  <a name="whats-in-a-thesaurus"></a><a name="tasks"></a> In Thesaurus enthaltene Funktionen  
 Bevor bei Volltextsuchabfragen nach Synonymen in einer bestimmten Sprache gesucht werden kann, müssen Sie Thesauruszuordnungen (Synonyme) für diese Sprache definieren. Jeder Thesaurus muss manuell konfiguriert werden, um Folgendes zu definieren:  
  
-   Erweiterungssatz  
  
     Ein Erweiterungssatz enthält eine Gruppe von Synonymen wie "writer", "author" und "journalist", die bei einer Volltextabfrage ausgetauscht werden. Abfragen, die eine Übereinstimmung für ein beliebiges Synonym in einem Erweiterungssatz enthalten, werden erweitert, um jedes andere Synonym im Erweiterungssatz einzubeziehen.  
  
     Weitere Informationen finden Sie unter [XML-Struktur eines Erweiterungssatzes](#expansion) weiter unten in diesem Thema.  
  
-   Ersetzungssatz  
  
     Ein Ersetzungssatz enthält ein durch einen Substitutionssatz zu ersetzendes Textmuster. Ein Beispiel dafür finden Sie im Abschnitt [XML-Struktur eines Ersetzungssatzes](#replacement) weiter unten in diesem Thema. 

-   Einstellung für diakritische Zeichen  
  
     Bei einem Thesaurus werden bei allen Suchmustern diakritische Zeichen – beispielsweise Tilde ( **~** ), Akut-Akzentzeichen ( **&acute;** ) oder Umlaut ( **&uml;** ) – entweder berücksichtigt oder nicht berücksichtigt (d.h., es erfolgt eine *Unterscheidung nach Akzent*, oder es erfolgt *keine Unterscheidung nach Akzent*). Angenommen, Sie haben in einer Volltextsuchabfrage angegeben, dass das Suchmuster „caf&eacute;“ durch ein anderes Muster ersetzt werden soll. Wenn im Thesaurus nicht nach Akzent unterschieden wird, ersetzt die Volltextsuche die Muster „caf&eacute;“ und „cafe“. Wenn im Thesaurus nach Akzent unterschieden wird, ersetzt die Volltextsuche nur das Muster „caf&eacute;“. Standardmäßig wird bei einem Thesaurus nicht nach Akzent unterschieden.  
  
##  <a name="default-thesaurus-files"></a><a name="initial_thesaurus_files"></a> Thesaurus-Standarddateien
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt einen Satz von XML-Thesaurusdateien bereit, und zwar eine für jede unterstützte Sprache. Diese Dateien sind im Wesentlichen leer. Sie enthalten nur die XML-Hauptstruktur, die alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Thesaurusdateien aufweisen, sowie einen auskommentierten Beispielthesaurus.  
  
##  <a name="location-of-thesaurus-files"></a><a name="location"></a> Speicherort der Thesaurusdateien  
 Der Standardspeicherort der Thesaurusdateien lautet folgendermaßen:  
  
`<SQL_Server_data_files_path>\MSSQL13.MSSQLSERVER\MSSQL\FTDATA\`
  
Dieser Standardspeicherort enthält die folgenden Dateien:  
  
-   **Sprachspezifische** Thesaurusdateien  

    Beim Setup werden am oben genannten Speicherort leere Thesaurusdateien installiert. Für jede unterstützte Sprache wird eine separate Datei bereitgestellt. Ein Systemadministrator kann diese Dateien anpassen.  
  
    Die Standarddateinamen der Thesaurusdateien haben das folgende Format:  
  
    `'ts' + <three-letter language-abbreviation> + '.xml'`
  
    Der Name einer Thesaurusdatei für eine bestimmte Sprache ist in der Registrierung im folgenden Wert angegeben:
     
    `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<instance-name>\MSSearch\<language-abbrev>`
  
-   Die **globale** Thesaurusdatei  
  
    Eine leere globale Thesaurusdatei mit dem Namen „tsGlobal.xml“.  

### <a name="change-the-location-of-a-thesaurus-file"></a>Ändern des Speicherorts einer Thesaurusdatei 
Sie können den Speicherort und den Namen einer Thesaurusdatei ändern, indem Sie den zugehörigen Registrierungsschlüssel ändern. Der Speicherort der Thesaurusdatei für jede einzelne Sprache ist im folgenden Wert in der Registrierung angegeben:  
  
`HKLM\SOFTWARE\Microsoft\Microsoft SQL Server\<instance name>\MSSearch\Language\<language-abbreviation>\TsaurusFile`
  
 Die globale Thesaurusdatei entspricht der neutralen Sprache mit LCID 0. Dieser Wert kann nur von Administratoren geändert werden.  

##  <a name="how-full-text-queries-use-the-thesaurus"></a><a name="how_queries_use_tf"></a> Verwenden des Thesaurus durch Volltextabfragen  
Für jede Thesaurusabfrage wird zuerst ein sprachspezifischer Thesaurus und dann der globale Thesaurus verwendet.
1.  Zuerst wird die sprachspezifische Datei gesucht und (falls erforderlich) zur Verarbeitung geladen. Die Abfrage wird um die durch die Erweiterungssatz- und Ersetzungssatz-Regeln in der Thesaurusdatei angegebenen Synonyme erweitert. 
2.  Anschließend werden diese Schritte für den globalen Thesaurus wiederholt. Wenn für einen Begriff in der sprachspezifischen Thesaurusdatei bereits eine Übereinstimmung gefunden wurde, werden die globalen Synonyme des Begriffs ignoriert.  

##  <a name="structure-of-a-thesaurus-file"></a><a name="structure"></a> Struktur einer Thesaurusdatei  
 Jede Thesaurusdatei definiert einen XML-Container, dessen ID `Microsoft Search Thesaurus` lautet, sowie einen Kommentar, `<!--` … `-->`, der einen Beispielthesaurus enthält. Der Thesaurus wird in einem `<thesaurus>`-Element definiert, das Beispiele für die untergeordneten Elemente enthält, in denen die Einstellung für diakritische Zeichen, Erweiterungssätze und Ersetzungssätze definiert werden.

Eine typische leere Thesaurusdatei enthält den folgenden XML-Text:  
  
```xml  
<XML ID="Microsoft Search Thesaurus">  
  
<!--  Commented out  
  
    <thesaurus xmlns="x-schema:tsSchema.xml">  
<diacritics_sensitive>0</diacritics_sensitive>  
        <expansion>  
            <sub>Internet Explorer</sub>  
            <sub>IE</sub>  
            <sub>IE5</sub>  
        </expansion>  
        <replacement>  
            <pat>NT5</pat>  
            <pat>W2K</pat>  
            <sub>Windows 2012</sub>  
        </replacement>  
        <expansion>  
            <sub>run</sub>  
            <sub>jog</sub>  
        </expansion>  
    </thesaurus>  
-->  
</XML>  
```

### <a name="xml-structure-of-an-expansion-set"></a><a name="expansion"></a> XML structure of an expansion set  
  
 Jeder Erweiterungssatz ist in ein `<expansion>`-Element eingeschlossen. Innerhalb dieses Elements geben Sie eine oder mehrere Substitutionen in einem `<sub>`-Element an. Im Erweiterungssatz können Sie eine Gruppe von Substitutionen angeben, die Synonyme zueinander sind.  
  
 Sie können beispielsweise den expansion-Abschnitt bearbeiten, um die Substitutionen "writer", "author" und "journalist" als Synonyme zu behandeln. Volltextsuchabfragen, die Übereinstimmungen in einer Substitution enthalten, werden erweitert, um alle weiteren im Erweiterungssatz angegebenen Substitutionen einzubeziehen. Wenn Sie daher im vorherigen Beispiel eine FORMS OF THESAURUS- oder eine FREETEXT-Abfrage nach dem Wort "author" ausführen, gibt die Volltextsuche auch Suchergebnisse zurück, die die Wörter "writer" und "journalist" enthalten.  
  
Der Erweiterungssatzabschnitt für das oben genannte Beispiel würde wie folgt aussehen:  
  
```xml  
<expansion>  
        <sub>writer</sub>  
        <sub>author</sub>  
        <sub>journalist</sub>  
</expansion>  
```  
  
### <a name="xml-structure-of-a-replacement-set"></a><a name="replacement"></a> XML structure of a replacement set  
  
Jeder Ersetzungssatz ist in ein `<replacement>`-Element eingeschlossen. Innerhalb dieses Elements können Sie ein oder mehrere Muster in einem `<pat>`-Element und keine oder beliebig viele Substitutionen in `<sub>`-Elementen (einem pro Synonym) angeben. Sie können ein durch einen Substitutionssatz zu ersetzendes Muster angeben. Muster und Substitutionen können ein Wort oder eine Wortfolge enthalten. Wenn für ein Muster keine Substitution angegeben wird, ist die Wirkung dieselbe, als würde das Muster aus der Benutzerabfrage entfernt.  
  
Angenommen, Sie möchten, dass Abfragen nach "Win8" (das Muster) durch "Windows Server 2012" oder "Windows 8.0" (die Substitutionen) ersetzt werden. Wenn Sie eine Volltextabfrage nach "Win8" ausführen, gibt die Volltextsuche nur Suchergebnisse zurück, die "Windows Server 2012" oder "Windows 8.0" enthalten. Sie gibt keine Ergebnisse zurück, die "Win8" enthalten. Dies liegt daran, dass das Muster "Win8" durch die Muster "Windows Server 2012" und "Windows 8.0" "ersetzt" wurde.  
  
Der Ersetzungssatzabschnitt für das oben genannte Beispiel würde wie folgt aussehen:  
  
```xml  
<replacement>  
        <pat>Win8</pat>  
        <sub>Windows Server 2012</sub>  
        <sub>Windows 8.0</sub>  
</replacement>  
```  
  
Wenn zwei Ersetzungssätze mit ähnlichen Mustern für die Übereinstimmung verwendet werden, hat der längere der beiden Vorrang. Wenn Sie beispielsweise eine FORMS OF THESAURUS-Abfrage nach "Internet Explorer online community" ausführen und die folgenden Ersetzungssätze haben, hat der "Internet Explorer"-Ersetzungssatz Vorrang vor dem "Internet"-Ersetzungssatz. Die Abfrage wird demzufolge als "IE online community" oder "IE 9 online community" verarbeitet.  
  
```xml  
<replacement>  
         <pat>Internet</pat>  
         <sub>intranet</sub>  
</replacement>  
```  
  
and  
  
```xml  
<replacement>  
         <pat>Internet Explorer</pat>  
         <sub>IE</sub>  
         <sub>IE 9</sub>  
</replacement>  
```

### <a name="xml-structure-of-the-diacritics-setting"></a>XML-Struktur der Einstellung für diakritische Zeichen  
  
Die Einstellung eines Thesaurus für diakritische Zeichen wird in einem einzelnen `<diacritics_sensitive>`-Element angegeben. Dieses Element enthält einen ganzzahligen Wert, der die Unterscheidung nach Akzent folgendermaßen steuert:  
  
|Einstellung für diakritische Zeichen|Wert|XML|  
|------------------------|-----------|---------|  
|keine Unterscheidung nach Akzent|0|`<diacritics_sensitive>0</diacritics_sensitive>`|  
|Unterscheidung nach Akzent|1|`<diacritics_sensitive>1</diacritics_sensitive>`|  
  
> [!NOTE]  
>  Diese Einstellung kann nur ein einziges Mal in der Datei vorgenommen werden und gilt für alle Suchmuster in der Datei. Diese Einstellung kann nicht für einzelne Muster angegeben werden.  

##  <a name="edit-a-thesaurus-file"></a><a name="editing"></a> Bearbeiten einer Thesaurusdatei  
Der Thesaurus für eine bestimmte Sprache kann durch Bearbeiten der zugehörigen Thesaurusdatei (einer XML-Datei) konfiguriert werden. Beim Setup werden leere Thesaurusdateien installiert, die nur den `<xml>`-Container und ein auskommentiertes Beispiel für ein `<thesaurus`>-Element enthalten. Damit Volltextsuchabfragen, mit denen nach Synonymen gesucht wird, ordnungsgemäß ausgeführt werden, müssen Sie ein tatsächliches `<thesaurus`>-Element erstellen, das eine Gruppe von Synonymen definiert. Sie können zwei Formen von Synonymen definieren, nämlich Erweiterungssätze und Ersetzungssätze.  

### <a name="edit-a-thesaurus-file"></a>Bearbeiten einer Thesaurusdatei  
  
1.  Öffnen Sie die Thesaurusdatei im Editor oder einem anderen Text-Editor.  
  
2.  Wenn Sie die Thesaurusdatei zum ersten Mal bearbeiten, entfernen Sie die folgenden Kommentarzeilen am Anfang bzw. Ende der Datei:  
  
    ```xml  
    <!--Commented out  
    -->  
    ```  
  
3.  Fügen Sie einen Ersetzungs- oder Erweiterungssatz hinzu, ändern oder löschen Sie ihn.  
  
4.  Speichern Sie die Datei, und schließen Sie Editor.  
  
5.  Verwenden Sie [sp_fulltext_load_thesaurus_file](../../relational-databases/system-stored-procedures/sp-fulltext-load-thesaurus-file-transact-sql.md) , um den Inhalt der Thesaurusdatei in tempdb zu laden, und geben Sie den Gebietsschemabezeichner (LCID) an, der der Sprache der Thesaurusdatei entspricht. So lautet z. B. für die englische Thesaurusdatei "tsenu.xml" der LCID 1033.  
  
    ```sql  
    USE AdventureWorks;  
    EXEC sys.sp_fulltext_load_thesaurus_file 1033;  
    GO
    ```

### <a name="recommendations-for-editing-thesaurus-files"></a>Empfehlungen für das Bearbeiten von Thesaurusdateien  
  
 Einträge in der Thesaurusdatei sollten keine Sonderzeichen enthalten. Dies wird deshalb empfohlen, weil die Wörtertrennung auf Sonderzeichen sehr fein reagiert. Wenn ein Thesauruseintrag Sonderzeichen enthält, kann die Verwendung der Wörtertrennung in Kombination mit diesem Eintrag schwer erkennbare Auswirkungen auf das Verhalten einer Volltextabfrage haben.  
  
 Es wird empfohlen, in `<sub>`-Einträgen keine Stoppwörter zu verwenden, da Stoppwörter im Volltextindex ausgelassen werden. Abfragen werden erweitert, um die `<sub>`-Einträge in einer Thesaurusdatei einzubeziehen, und wenn ein `<sub>`-Eintrag Stoppwörter enthält, nimmt die Abfrage unnötigerweise an Größe zu.  

### <a name="restrictions-for-editing-thesaurus-files"></a>Einschränkungen für das Bearbeiten von Thesaurusdateien  
  
 Beim Bearbeiten einer Thesaurusdatei gelten die folgenden Einschränkungen:  
  
-   Nur Systemadministratoren können Thesaurusdateien aktualisieren, ändern und löschen.  
  
-   Wenn Sie Thesaurusdateien mithilfe von Text-Editor-Tools bearbeiten, müssen die Dateien im Unicode-Format gespeichert und Bytereihenfolgemarken (Byte Order Marks, BOMs) angegeben werden.  
  
-   Thesauruseinträge dürfen nicht leer sein oder eine Wörtertrennung zu einer leeren Zeichenfolge aufweisen.  
  
-   Ausdrücke in der Thesaurusdatei dürfen aus höchstens 512 Zeichen bestehen.  
  
-   Ein Thesaurus darf in den `<sub>`-Einträgen von Erweiterungssätzen und in den `<pat>`-Elementen von Ersetzungssätzen keine doppelten Einträge enthalten.  

## <a name="see-also"></a>Weitere Informationen  
 [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md)   
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [FREETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/freetext-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)     
 [sp_fulltext_load_thesaurus_file &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-load-thesaurus-file-transact-sql.md)  
 [sys.dm_fts_parser &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-parser-transact-sql.md)
 
