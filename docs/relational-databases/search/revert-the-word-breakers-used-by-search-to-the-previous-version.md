---
description: Wiederherstellen der von der Suche verwendeten Wörtertrennungen auf die vorherige Version (SQL Server-Suche)
title: Wiederherstellen der von der Suche verwendeten Wörtertrennungen auf die vorherige Version
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
ms.assetid: 29b4488e-4c6a-4bf0-a64d-19e2fdafa7ae
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-lt-2019
ms.openlocfilehash: 8362788a2bf1092e815a9df43c3b65298935d555
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344461"
---
# <a name="revert-word-breakers-used-by-search-to-previous-version-sql-server-search"></a>Wiederherstellen der von der Suche verwendeten Wörtertrennungen auf die vorherige Version (SQL Server-Suche)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] installiert und aktiviert eine Version der Wörtertrennung und Wortstammerkennung für alle von der Volltextsuche unterstützten Sprachen, mit Ausnahme von Koreanisch. Dieser Artikel beschreibt, wie Sie von der neuen Version dieser Komponenten zur früheren Version wechseln und von der früheren Version zur neuen Version zurückwechseln.  
  
 Die folgenden Sprachen werden in diesem Artikel nicht berücksichtigt:  
  
- **Englisch**. Informationen zum Wiederherstellen der englischen Komponenten finden Sie unter [Ändern der für Englisch (USA) und Englisch (Vereinigtes Königreich) verwendeten Wörtertrennung](../../relational-databases/search/change-the-word-breaker-used-for-us-english-and-uk-english.md).  
  
- **Dänisch, Polnisch und Türkisch**. Die Wörtertrennungen von Drittanbietern für Dänisch, Polnisch und Türkisch, die in vorherigen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] enthalten waren, wurden durch [!INCLUDE[msCoName](../../includes/msconame-md.md)] -Komponenten ersetzt.  
  
- **Tschechisch und Griechisch**. Es gibt neue Wörtertrennungen für Tschechisch und Griechisch. Vorherige Versionen der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Volltextsuche unterstützten diese zwei Sprachen nicht.  
  
- **Koreanisch**. Die Wörtertrennung und die Wortstammerkennung für die koreanische Sprache werden in dieser Version nicht aktualisiert.  
  
 Allgemeine Informationen zur Wörtertrennung und Wortstammerkennung finden Sie unter [Konfigurieren und Verwalten von Wörtertrennungen und Wortstammerkennungen für die Suche](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md).  
  
##  <a name="overview-of-reverting-and-restoring-word-breakers-and-stemmers"></a><a name="overview"></a> Übersicht über das Wiederherstellen von Wörtertrennungen und Wortstammerkennungen  
 Die Anweisungen zum Wiederherstellen von Wörtertrennungen und Wortstammerkennungen hängen von der Sprache ab. In der folgenden Tabelle werden die drei Sätze von Aktionen zusammengefasst, die möglicherweise erforderlich sind, um zur früheren Version der Komponenten zurückzuwechseln.  
  
|Aktuelle Datei|Vorherige Datei|Anzahl betroffener Sprachen|Aktion für Dateien|Aktion für Registrierungseinträge|  
|------------------|-------------------|----------------------------------|----------------------|---------------------------------|  
|NaturalLanguage6.dll|NaturalLanguage6.dll|34|Erhalten und installieren Sie eine frühere Version von NaturalLanguage6.dll und überschreiben Sie die aktuelle Version der Datei.|Keine weiteren Maßnahmen erforderlich.<br /><br /> Die Registrierungsschlüssel und Werte haben sich für diese Version nicht geändert.|  
|(Anderer Dateiname)|NaturalLanguage6.dll|5|Erhalten und installieren Sie eine frühere Version von NaturalLanguage6.dll und überschreiben Sie die aktuelle Version der Datei.|Ändern Sie einen Satz von Registrierungseinträgen, um die frühere Version der Komponenten anzugeben.|  
|(Anderer Dateiname)|(Anderer Dateiname)|6|Keine weiteren Maßnahmen erforderlich.<br /><br /> [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Setup kopiert sowohl die aktuelle als auch die früheren Versionen der Komponenten in den Ordner "Binn".|Ändern Sie einen Satz von Registrierungseinträgen, um die frühere Version der Komponenten anzugeben.|  
  
> [!WARNING]  
>  Wenn Sie die aktuelle Version der Datei NaturalLanguage6.dll mit einer anderen Version ersetzen, dann ist das Verhalten aller Sprachen, die diese Datei verwenden, davon betroffen.  
  
 Bei den in diesem Artikel beschriebenen Dateien handelt es sich um DLL-Dateien, die im `MSSQL\Binn`-Ordner für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz installiert sind. Der vollständige Pfad ist in der Regel der folgende Pfad:  
  
 `C:\Program Files\Microsoft SQL Server\<instance>\MSSQL\Binn`  
  
##  <a name="languages-for-which-the-file-name-of-both-the-current-and-previous-word-breaker-is-naturallanguage6dll"></a><a name="nl6nl6"></a> Sprachen, für die der Dateiname sowohl für die aktuelle als auch die vorherige Wörtertrennung NaturalLanguage6.dll ist  
 Für die Sprachen in der folgenden Tabelle ist der Dateiname sowohl für die aktuelle als auch die vorherige Wörtertrennung NaturalLanguage6.dll. Um diese Komponenten wiederherzustellen, müssen Sie NaturalLanguage6.dll mit einer anderen Version der gleichen Datei überschreiben. Sie müssen keine Registrierungseinträge ändern, da sich die Registrierungseinträge für diese Version nicht geändert haben.  
  
> [!WARNING]  
>  Wenn Sie die aktuelle Version der Datei NaturalLanguage6.dll mit einer anderen Version ersetzen, dann ist das Verhalten aller Sprachen, die diese Datei verwenden, davon betroffen.  
  
 **Liste betroffener Sprachen**  
  
|Sprache|Abkürzung<br />verwendet in<br />Registrierung|LCID|  
|--------------|---------------------------------------|----------|  
|Bengali|`ben`|1093|  
|Bulgarisch|`bgr`|1026|  
|Katalanisch|`cat`|1027|  
|Spanisch|`esn`|3082|  
|Französisch|`fra`|1036|  
|Gujarati|`guj`|1095|  
|Hebräisch|`heb`|1037|  
|Hindi|`hin`|1081|  
|Kroatisch|`hrv`|1050|  
|Indonesisch|`ind`|1057|  
|Isländisch|`isl`|1039|  
|Italienisch|`ita`|1040|  
|Kannada|`kan`|1099|  
|Litauisch|`lth`|1063|  
|Lettisch|`lvi`|1062|  
|Malayalam|`mal`|1100|  
|Marathi|`mar`|1102|  
|Malaiisch|`msl`|1086|  
|Neutral|`Neutral`|0000|  
|Norwegial Bokmaal|`nor`|1044|  
|Pandschabi|`pan`|1094|  
|Portugiesisch (Brasilien)|`ptb`|1046|  
|Portugiesisch|`ptg`|2070|  
|Rumänisch|`rom`|1048|  
|Slowakisch|`sky`|1051|  
|Slowenisch|`slv`|1060|  
|Serbisch (Kyrillisch)|`srb`|3098|  
|Serbisch (Lateinisch)|`srl`|2074|  
|Schwedisch|`sve`|1053|  
|Tamilisch|`tam`|1097|  
|Telugu|`tel`|1.098|  
|Ukrainisch|`ukr`|1058|  
|Urdu|`urd`|1056|  
|Vietnamesisch|`vit`|1066|  
  
 Die obige Tabelle ist nach der Spalte "Abkürzung" alphabetisch sortiert.  
  
###  <a name="to-revert-to-the-previous-components"></a><a name="nl6nl6revert"></a> So stellen Sie die vorherigen Komponenten wieder her  
  
1.  Navigieren Sie zum Ordner "Binn", der oben beschrieben wurde.  
  
2.  Sichern Sie die [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Version von NaturalLanguage6.dll an einem anderen Speicherort.  
  
3.  Kopieren Sie die frühere Version von NaturalLanguage6.dll aus dem Ordner "Binn" einer Instanz von [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] oder [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] in den Ordner "Binn" der [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Instanz.  
  
    > [!WARNING]  
    >  Diese Änderung wirkt sich auf alle Sprachen aus, die NaturalLanguage6.dll sowohl in der aktuellen als auch der früherer Version verwenden.  
  
4.  Starten Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] neu.  

###  <a name="to-restore-the-current-components"></a><a name="nl6nl6restore"></a> So stellen Sie aktuelle Komponenten wieder her  
  
1.  Navigieren Sie zum Speicherort, an dem Sie die [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Version von NaturalLanguage6.dll gesichert haben.  
  
2.  Kopieren Sie die aktuelle Version von NaturalLanguage6.dll vom Sicherungsspeicherort in den Ordner "Binn" der [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Instanz.  
  
    > [!WARNING]  
    >  Diese Änderung wirkt sich auf alle Sprachen aus, die NaturalLanguage6.dll sowohl in der aktuellen als auch der früherer Version verwenden.  
  
3.  Starten Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] neu.  
  
##  <a name="languages-for-which-the-file-name-of-the-previous-word-breaker-only-is-naturallanguage6dll"></a><a name="newnl6"></a> Sprachen, für die nur der Dateiname der vorherigen Wörtertrennung NaturalLanguage6.dll ist  
 Für die Sprachen in der folgenden Tabelle unterscheidet sich der Dateiname für die vorherige Wörtertrennung vom Dateinamen der neuen Version. Der vorherige Dateiname ist NaturalLanguage6.dll. Um zur früheren Version wiederherzustellen, müssen Sie die aktuelle Version von NaturalLanguage6.dll mit einer früheren Version der gleichen Datei überschreiben. Sie müssen auch einen Satz von Registrierungseinträgen ändern, um die vorherige oder aktuelle Version der Komponenten anzugeben.  
  
> [!WARNING]  
>  Wenn Sie die aktuelle Version der Datei NaturalLanguage6.dll mit einer anderen Version ersetzen, dann ist das Verhalten aller Sprachen, die diese Datei verwenden, davon betroffen.  
  
 **Liste betroffener Sprachen**  
  
|Sprache|Abkürzung<br />verwendet in<br />Registrierung|LCID|  
|--------------|---------------------------------------|----------|  
|Arabisch|ara|1025|  
|Deutsch|deu|1031|  
|Japanisch|jpn|1041|  
|Niederländisch|nld|1043|  
|Russisch|rus|1049|  
  
 Die obige Tabelle ist nach der Spalte "Abkürzung" alphabetisch sortiert.  
  
 Verwenden Sie die folgenden Anweisungen zusammen mit der Liste der Werte im Abschnitt [Dateinamen und Registrierungswerte zum Wiederherstellen von Wörtertrennungen und Wortstammerkennungen](#newnl6values).  
  
###  <a name="to-revert-to-the-previous-components"></a><a name="newnl6revert"></a> So stellen Sie die vorherigen Komponenten wieder her  
  
1.  Navigieren Sie zum Ordner "Binn", der oben beschrieben wurde.  
  
2.  Entfernen Sie die Dateien für die aktuelle Version der Komponenten nicht aus dem Ordner "Binn".  
  
3.  Sichern Sie die [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Version von NaturalLanguage6.dll an einem anderen Speicherort.  
  
4.  Kopieren Sie die frühere Version von NaturalLanguage6.dll aus dem Ordner "Binn" einer Instanz von [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] oder [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] in den Ordner "Binn" der neuen [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]-Instanz.  
  
    > [!WARNING]  
    >  Diese Änderung wirkt sich auf alle Sprachen aus, die NaturalLanguage6.dll sowohl in der aktuellen als auch der früherer Version verwenden.  
  
5.  Navigieren Sie in der Registrierung zu folgendem Knoten: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID**.  
  
6.  Fügen Sie mithilfe der folgenden Schritte für die COM ClassIDs für die Schnittstellen der früheren Wörtertrennung und die Wortstammerkennung für die ausgewählte Sprache neue Schlüssel hinzu:  
  
    1.  Fügen Sie einen neuen Schlüssel mit den Wert aus der Tabelle für die frühere Wörtertrennung hinzu.  
  
    2.  Aktualisieren Sie die (Standard-)Daten dieses Schlüsselwerts auf den Dateinamen der vorherigen Wörtertrennung aus der Tabelle.  
  
    3.  Wenn die ausgewählte Sprache eine Wortstammerkennung verwendet, fügen Sie dann einen neuen Schlüssel mit dem Wert aus der Tabelle für die vorherige Wortstammerkennung hinzu.  
  
    4.  Wenn die ausgewählte Sprache eine Wortstammerkennung verwendet, aktualisieren Sie dann die (Standard-)Daten dieses Schlüsselwerts auf den Dateinamen der vorherigen Wortstammerkennung aus der Tabelle.  
  
7.  Navigieren Sie in der Registrierung zu folgendem Knoten: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>** . *<Sprachschlüssel>* stellt die Abkürzung für die Sprache dar, die in der Registrierung verwendet wird (z.B. „fra“ für Französisch und „esn“ für Spanisch).  
  
8.  Aktualisieren Sie den **WBreakerClass** -Schlüsselwert auf den Wert aus der Tabelle für die aktuelle Wörtertrennung.  
  
9. Wenn die ausgewählte Sprache eine Wortstammerkennung verwendet, aktualisieren Sie dann den **StemmerClass** -Schlüsselwert auf den Wert aus der Tabelle für die aktuelle Wortstammerkennung.  
  
10. Starten Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] neu.  
  
###  <a name="to-restore-the-current-components"></a><a name="newnl6restore"></a> So stellen Sie aktuelle Komponenten wieder her  
  
1.  Navigieren Sie zum Speicherort, an dem Sie die [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Version von NaturalLanguage6.dll gesichert haben.  
  
2.  Kopieren Sie die aktuelle Version von NaturalLanguage6.dll vom Sicherungsspeicherort in den Ordner "Binn" der [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Instanz.  
  
    > [!WARNING]  
    >  Diese Änderung wirkt sich auf alle Sprachen aus, die NaturalLanguage6.dll sowohl in der aktuellen als auch der früherer Version verwenden.  
  
3.  Navigieren Sie in der Registrierung zu folgendem Knoten: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID**.  
  
4.  Wenn die folgenden Schlüssel nicht vorhanden sind, gehen Sie folgendermaßen vor, um neue Schlüssel für die COM ClassIDs für die Schnittstellen der aktuellen Wörtertrennung und der Wortstammerkennung für die ausgewählte Sprache hinzuzufügen:  
  
    1.  Fügen Sie einen neuen Schlüssel mit den Wert aus der Tabelle für die aktuelle Wörtertrennung hinzu.  
  
    2.  Aktualisieren Sie die (Standard-)Daten dieses Schlüsselwerts auf den Dateinamen der aktuellen Wörtertrennung aus der Tabelle.  
  
    3.  Wenn die ausgewählte Sprache eine Wortstammerkennung verwendet, fügen Sie dann einen neuen Schlüssel mit dem Wert aus der Tabelle für die aktuelle Wortstammerkennung hinzu.  
  
    4.  Wenn die ausgewählte Sprache eine Wortstammerkennung verwendet, aktualisieren Sie dann die (Standard-)Daten dieses Schlüsselwerts auf den Dateinamen der aktuellen Wortstammerkennung aus der Tabelle.  
  
5.  Navigieren Sie in der Registrierung zu folgendem Knoten: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>** . *<Sprachschlüssel>* stellt die Abkürzung für die Sprache dar, die in der Registrierung verwendet wird (z.B. „fra“ für Französisch und „esn“ für Spanisch).  
  
6.  Aktualisieren Sie den **WBreakerClass** -Schlüsselwert auf den Wert aus der Tabelle für die vorherige Wörtertrennung.  
  
7.  Wenn die ausgewählte Sprache eine Wortstammerkennung verwendet, aktualisieren Sie dann den **StemmerClass** -Schlüsselwert auf den Wert aus der Tabelle für die vorherige Wortstammerkennung.  
  
8.  Starten Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] neu.  
  
###  <a name="file-names-and-registry-values-for-reverting-and-restoring-word-breakers-and-stemmers"></a><a name="newnl6values"></a> Dateinamen und Registrierungswerte zum Wiederherstellen von Wörtertrennungen und Wortstammerkennungen  
 Verwenden Sie die folgende Liste von Dateinamen und Registrierungseinträgen zusammen mit den Anweisungen im vorangehenden Abschnitt. Verwenden Sie die vorherigen Werte, um die frühere Version wiederherzustellen, oder verwenden Sie die aktuellen Werte, um die aktuelle Version der Komponenten wiederherzustellen.  
  
 Die folgende Liste ist alphabetisch nach der für jede Sprache verwendeten Abkürzung sortiert.  
  
 **Arabisch (ara), LCID 1025**  
  
|Komponente|Wörtertrennung|Wortstammerkennung|  
|---------------|------------------|-------------|  
|Vorherige CLSID|7EFD3C7E-9E4B-4a93-9503-DECD74C0AC6D|483B0283-25DB-4c92-9C15-A65925CB95CE|  
|Vorheriger Dateiname|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|Aktuelle CLSID|04b37e30-c9a9-4a7d-8f20-792fc87ddf71|Keine|  
|Aktueller Dateiname|MSWB7.dll|Keine|  
  
 **Deutsch (deu), LCID 1031**  
  
|Komponente|Wörtertrennung|Wortstammerkennung|  
|---------------|------------------|-------------|  
|Vorherige CLSID|45EACA36-DBE9-4e4a-A26D-5C201902346D|65170AE4-0AD2-4fa5-B3BA-7CD73E2DA825|  
|Vorheriger Dateiname|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|Aktuelle CLSID|dfa00c33-bf19-482e-a791-3c785b0149b4|8a474d89-6e2f-419c-8dd5-9b50edc8c787|  
|Aktueller Dateiname|MSWB7.dll|MSWB7.dll|  
  
 **Japanisch (jpn), LCID 1041**  
  
|Komponente|Wörtertrennung|Wortstammerkennung|  
|---------------|------------------|-------------|  
|Vorherige CLSID|E1E8F15E-8BEC-45df-83BF-50FF84D0CAB5|3D5DF14F-649F-4cbc-853D-F18FEDE9CF5D|  
|Vorheriger Dateiname|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|Aktuelle CLSID|04096682-6ece-4e9e-90c1-52d81f0422ed|Keine|  
|Aktueller Dateiname|MsWb70011.dll|Keine|  
  
 **Niederländisch (nld), LCID 1043**  
  
|Komponente|Wörtertrennung|Wortstammerkennung|  
|---------------|------------------|-------------|  
|Vorherige CLSID|2C9F6BEB-C5B0-42b6-A5EE-84C24DC0D8EF|F7A465EE-13FB-409a-B878-195B420433AF|  
|Vorheriger Dateiname|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|Aktuelle CLSID|69483c30-a9af-4552-8f84-a0796ad5285b|CF923CB5-1187-43ab-B053-3E44BED65FFA|  
|Aktueller Dateiname|MSWB7.dll|MSWB7.dll|  
  
 **Russisch (rus), LCID 1049**  
  
|Komponente|Wörtertrennung|Wortstammerkennung|  
|---------------|------------------|-------------|  
|Vorherige CLSID|2CB6CDA4-1C14-4392-A8EC-81EEF1F2E079|E06A0DDD-E81A-4e93-8A8D-F386C3A1B670|  
|Vorheriger Dateiname|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|Aktuelle CLSID|aaa3d3bd-6de7-4317-91a0-d25e7d3babc3|d42c8b70-adeb-4b81-a52f-c09f24f77dfa|  
|Aktueller Dateiname|MSWB7.dll|MSWB7.dll|  
  
##  <a name="languages-for-which-neither-the-previous-nor-the-current-file-name-is-naturallanguage6dll"></a><a name="newnew"></a> Sprachen, für die weder der vorherige noch der aktuelle Dateiname NaturalLanguage6.dll ist  
 Für die Sprachen in der folgenden Tabelle unterscheiden sich die Dateinamen der vorherigen Wörtertrennungen und Wortstammerkennungen von den Dateinamen der neuen Versionen. Weder der vorherige noch der aktuelle Dateiname ist NaturalLanguage6.dll. Sie müssen keine Dateien ersetzen, da [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Setup sowohl die aktuellen als auch die früheren Versionen der Komponenten in den Ordner "Binn" kopiert. Sie müssen jedoch einen Satz von Registrierungseinträgen ändern, um die vorherige oder aktuelle Version der Komponenten anzugeben.  
  
 **Liste betroffener Sprachen**  
  
|Sprache|Abkürzung<br />verwendet in<br />Registrierung|LCID|  
|--------------|---------------------------------------|----------|  
|Chinesisch (vereinfacht)|chs|2052|  
|Chinesisch (traditionell)|cht|1028|  
|Thailändisch|tha|1054|  
|Chinesisch (traditionell)|zh-hk|3076|  
|Chinesisch (traditionell)|zh-mo|5124|  
|Chinesisch (vereinfacht)|zh-sg|4100|  
  
 Die obige Tabelle ist nach der Spalte "Abkürzung" alphabetisch sortiert.  
  
 Verwenden Sie die folgenden Anweisungen zusammen mit der Liste der Werte im Abschnitt [Dateinamen und Registrierungswerte zum Wiederherstellen von Wörtertrennungen und Wortstammerkennungen](#newnewvalues).  
  
###  <a name="to-revert-to-the-previous-components"></a><a name="newnewrevert"></a> So stellen Sie die vorherigen Komponenten wieder her  
  
1.  Entfernen Sie die Dateien für die aktuelle Version der Komponenten nicht aus dem Ordner "Binn".  
  
2.  Navigieren Sie in der Registrierung zu folgendem Knoten: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID**.  
  
3.  Fügen Sie mithilfe der folgenden Schritte für die COM ClassIDs für die Schnittstellen der früheren Wörtertrennung und die Wortstammerkennung für die ausgewählte Sprache neue Schlüssel hinzu:  
  
    1.  Fügen Sie einen neuen Schlüssel mit den Wert aus der Tabelle für die frühere Wörtertrennung hinzu.  
  
    2.  Aktualisieren Sie die (Standard-)Daten dieses Schlüsselwerts auf den Dateinamen der vorherigen Wörtertrennung aus der Tabelle.  
  
    3.  Wenn die ausgewählte Sprache eine Wortstammerkennung verwendet, fügen Sie dann einen neuen Schlüssel mit dem Wert aus der Tabelle für die vorherige Wortstammerkennung hinzu.  
  
    4.  Wenn die ausgewählte Sprache eine Wortstammerkennung verwendet, aktualisieren Sie dann die (Standard-)Daten dieses Schlüsselwerts auf den Dateinamen der vorherigen Wortstammerkennung aus der Tabelle.  
  
4.  Navigieren Sie in der Registrierung zu folgendem Knoten: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>** . *<Sprachschlüssel>* stellt die Abkürzung für die Sprache dar, die in der Registrierung verwendet wird (z.B. „fra“ für Französisch und „esn“ für Spanisch).  
  
5.  Aktualisieren Sie den **WBreakerClass** -Schlüsselwert auf den Wert aus der Tabelle für die aktuelle Wörtertrennung.  
  
6.  Wenn die ausgewählte Sprache eine Wortstammerkennung verwendet, aktualisieren Sie dann den **StemmerClass** -Schlüsselwert auf den Wert aus der Tabelle für die aktuelle Wortstammerkennung.  
  
7.  Starten Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] neu.  
  
###  <a name="to-restore-the-previous-components"></a><a name="newnewrestore"></a> So stellen Sie die vorherigen Komponenten wieder her  
  
1.  Entfernen Sie die Dateien für die vorherige Version der Komponenten nicht aus dem Ordner "Binn".  
  
2.  Navigieren Sie in der Registrierung zu folgendem Knoten: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID**.  
  
3.  Wenn die folgenden Schlüssel nicht vorhanden sind, gehen Sie folgendermaßen vor, um neue Schlüssel für die COM ClassIDs für die Schnittstellen der aktuellen Wörtertrennung und der Wortstammerkennung für die ausgewählte Sprache hinzuzufügen:  
  
    1.  Fügen Sie einen neuen Schlüssel mit den Wert aus der Tabelle für die aktuelle Wörtertrennung hinzu.  
  
    2.  Aktualisieren Sie die (Standard-)Daten dieses Schlüsselwerts auf den Dateinamen der aktuellen Wörtertrennung aus der Tabelle.  
  
    3.  Wenn die ausgewählte Sprache eine Wortstammerkennung verwendet, fügen Sie dann einen neuen Schlüssel mit dem Wert aus der Tabelle für die aktuelle Wortstammerkennung hinzu.  
  
    4.  Wenn die ausgewählte Sprache eine Wortstammerkennung verwendet, aktualisieren Sie dann die (Standard-)Daten dieses Schlüsselwerts auf den Dateinamen der aktuellen Wortstammerkennung aus der Tabelle.  
  
4.  Navigieren Sie in der Registrierung zu folgendem Knoten: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>** . *<Sprachschlüssel>* stellt die Abkürzung für die Sprache dar, die in der Registrierung verwendet wird (z.B. „fra“ für Französisch und „esn“ für Spanisch).  
  
5.  Aktualisieren Sie den **WBreakerClass** -Schlüsselwert auf den Wert aus der Tabelle für die vorherige Wörtertrennung.  
  
6.  Wenn die ausgewählte Sprache eine Wortstammerkennung verwendet, aktualisieren Sie dann den **StemmerClass** -Schlüsselwert auf den Wert aus der Tabelle für die vorherige Wortstammerkennung.  
  
7.  Starten Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] neu.  
  
###  <a name="file-names-and-registry-values-for-reverting-and-restoring-word-breakers-and-stemmers"></a><a name="newnewvalues"></a> Dateinamen und Registrierungswerte zum Wiederherstellen von Wörtertrennungen und Wortstammerkennungen  
 Verwenden Sie die folgende Liste von Dateinamen und Registrierungseinträgen zusammen mit den Anweisungen im vorangehenden Abschnitt. Verwenden Sie die vorherigen Werte, um die frühere Version wiederherzustellen, oder verwenden Sie die aktuellen Werte, um die aktuelle Version der Komponenten wiederherzustellen.  
  
 Die folgende Liste ist alphabetisch nach der für jede Sprache verwendeten Abkürzung sortiert.  
  
 **Chinesisch vereinfacht (chs), LCID 2052**  
  
|Komponente|Wörtertrennung|  
|---------------|------------------|  
|Vorherige CLSID|12CE94A0-DEFB-11D2-B31D-00600893A857|  
|Vorheriger Dateiname|chsbrkr.dll|  
|Aktuelle CLSID|E0831C90-BAB0-4ca5-B9BD-EA254B538DAC|  
|Aktueller Dateiname|MsWb70804.dll|  
  
 **Chinesisch traditionell (cht), LCID 1028**  
  
|Komponente|Wörtertrennung|  
|---------------|------------------|  
|Vorherige CLSID|1680E7C3-9430-4A51-9B82-1E7E7AEE5258|  
|Vorheriger Dateiname|chtbrkr.dll|  
|Aktuelle CLSID|E9B1DF65-08F1-438b-8277-EF462B23A792|  
|Aktueller Dateiname|MsWb70404.dll|  
  
 **Thailändisch (tha), LCID 1054**  
  
|Komponente|Wörtertrennung|Wortstammerkennung|  
|---------------|------------------|-------------|  
|Vorherige CLSID|CCA22CF4-59FE-11D1-BBFF-00C04FB97FDA|CEDC01C7-59FE-11D1-BBFF-00C04FB97FDA|  
|Vorheriger Dateiname|Thawbrkr.dll|Thawbrkr.dll|  
|Aktuelle CLSID|F70C0935-6E9F-4ef1-9F06-7876536DB900|Keine|  
|Aktueller Dateiname|MsWb7001e.dll|Keine|  
  
 **Chinesisch traditionell (zh-hk), LCID 3076**  
  
|Komponente|Wörtertrennung|  
|---------------|------------------|  
|Vorherige CLSID|1680E7C3-9430-4A51-9B82-1E7E7AEE5258|  
|Vorheriger Dateiname|chtbrkr.dll|  
|Aktuelle CLSID|E9B1DF65-08F1-438b-8277-EF462B23A792|  
|Aktueller Dateiname|MsWb70404.dll|  
  
 **Chinesisch traditionell (zh-mo), LCID 5124**  
  
|Komponente|Wörtertrennung|  
|---------------|------------------|  
|Vorherige CLSID|1680E7C3-9430-4A51-9B82-1E7E7AEE5258|  
|Vorheriger Dateiname|chtbrkr.dll|  
|Aktuelle CLSID|E9B1DF65-08F1-438b-8277-EF462B23A792|  
|Aktueller Dateiname|MsWb70404.dll|  
  
 **Chinesisch vereinfacht (zh-sg), LCID 4100**  
  
|Komponente|Wörtertrennung|  
|---------------|------------------|  
|Vorherige CLSID|12CE94A0-DEFB-11D2-B31D-00600893A857|  
|Vorheriger Dateiname|chsbrkr.dll|  
|Aktuelle CLSID|E0831C90-BAB0-4ca5-B9BD-EA254B538DAC|  
|Aktueller Dateiname|MsWb70804.dll|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ändern der für Englisch (USA) und Englisch (Vereinigtes Königreich) verwendeten Wörtertrennung](../../relational-databases/search/change-the-word-breaker-used-for-us-english-and-uk-english.md)   
 [Verhaltensänderungen der Volltextsuche](./full-text-search.md)
