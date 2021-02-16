---
description: Datenprofil-Viewer
title: Datenprofil-Viewer | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.dataprofileviewer.f1
helpviewer_keywords:
- Data Profile Viewer [Integration Services]
- Data Profiling task [Integration Services], output viewer
ms.assetid: b9043428-ce26-45bb-910c-588d07579565
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aaa5d2276908feccd86aac75012703e093b861d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352130"
---
# <a name="data-profile-viewer"></a>Datenprofil-Viewer

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Das Anzeigen und Analysieren der Datenprofile ist der nächste Schritt bei der Datenprofilerstellung. Sie können diese Profile anzeigen, nachdem Sie den Datenprofilerstellungs-Task innerhalb eines [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Pakets ausgeführt und die Datenprofile berechnet haben. Weitere Informationen zum Verwenden und Einrichten und Ausführen der Datenprofilerstellungs-Tasks finden Sie unter [Einrichten von Datenprofilerstellungs-Tasks](../../integration-services/control-flow/setup-of-the-data-profiling-task.md).  
  
> [!IMPORTANT]  
>  Die Ausgabedatei enthält möglicherweise vertrauliche Daten über Ihre Datenbank und die darin enthaltenen Daten. Vorschläge zur Verbesserung der Sicherheit dieser Datei finden Sie unter [Zugriff auf Dateien, die von Paketen verwendet werden](../../integration-services/security/security-overview-integration-services.md#files).  
  
## <a name="data-profiles"></a>Datenprofile  
 Wenn Sie die Datenprofile anzeigen möchten, konfigurieren Sie den Datenprofilerstellungs-Task so, dass dieser seine Ausgabe an eine Datei sendet, und verwenden Sie dann den eigenständigen Datenprofil-Viewer. Gehen Sie folgendermaßen vor, um den Datenprofil-Viewer zu öffnen.  
  
-   Klicken Sie mit der rechten Maustaste im **-Designer auf den Task** Datenprofilerstellung [!INCLUDE[ssIS](../../includes/ssis-md.md)] , und klicken Sie dann auf **Bearbeiten**. Klicken Sie auf der Seite **Allgemein** des **Editors für den Datenprofilerstellungs-Task** auf **Profil-Viewer öffnen**.  
  
-   Führen Sie im Ordner „ *\<drive>* :\Programme (x86) | Programme\Microsoft SQL Server\110\DTS\Binn“ die Datei „DataProfileViewer.exe“ aus.  
  
 Der Viewer verwendet mehrere Bereiche, um die von Ihnen angeforderten Profile und berechneten Ergebnisse mit optionaler Detail- und Drilldownfunktion anzuzeigen.  
  
 Bereich **Profile**  
 Der Bereich **Profile** zeigt die Profile an, die im Datenprofil-Task angefordert wurden. Wenn Sie die für das Profil berechneten Ergebnisse anzeigen möchten, wählen Sie im Bereich **Profile** das Profil aus. Die Ergebnisse werden dann in den anderen Bereichen des Viewers angezeigt.  
  
 Bereich **Ergebnisse**  
 Der Bereich **Ergebnisse** verwendet eine einzelne Zeile, um die berechneten Ergebnisse des Profils zusammenzufassen. Wenn Sie beispielsweise ein **Verteilungsprofil für Spaltenlänge** anfordern, enthält diese Zeile die minimale und maximale Länge und die Zeilenanzahl. Für die meisten Profile können Sie diese Zeile im Bereich **Ergebnisse** auswählen, um zusätzliche Details im optionalen Bereich **Details** anzuzeigen.  
  
 Bereich **Details**  
 Für die meisten Profiltypen zeigt der Bereich **Details** zusätzliche Informationen zu den Profilergebnissen an, die im Bereich **Ergebnisse** ausgewählt wurden. Wenn Sie beispielsweise ein **Verteilungsprofil für Spaltenlänge** anfordern, zeigt der Bereich **Details** jede gefundene Spaltenlänge an. Der Bereich zeigt auch die Anzahl und den Prozentwert der Zeilen an, in denen der Spaltenwert diese Spaltenlänge hat.  
  
 Für die drei Profiltypen, die für mehrere Spalten berechnet werden (Kandidatenschlüssel, funktionale Abhängigkeit und Wertinklusion), zeigt der Bereich **Details** Verletzungen der erwarteten Beziehung an. Wenn Sie beispielsweise ein Kandidatenschlüsselprofil anfordern, zeigt der Detailbereich doppelte Werte an, die die Eindeutigkeit des Kandidatenschlüssels verletzen.  
  
 Wenn die Datenquelle verfügbar ist, mit der das Profil berechnet wird, können Sie im Bereich **Details** auf eine Zeile doppelklicken, um die übereinstimmenden Datenzeilen im Bereich **Drilldown** anzuzeigen.  
  
 Bereich **Drilldown**  
 Sie können im Bereich **Details** auf eine Zeile doppelklicken, um die übereinstimmenden Datenzeilen im Bereich **Drilldown** anzuzeigen, wenn die folgenden Bedingungen erfüllt sind:  
  
-   Die Datenquelle, mit der das Profil berechnet wird, ist verfügbar.  
  
-   Sie sind berechtigt, die Daten anzuzeigen.  
  
 Um für eine Drilldownanforderung eine Verbindung mit der Quelldatenbank herzustellen, verwendet der Datenprofil-Viewer die Windows-Authentifizierung und die Anmeldeinformationen des aktuellen Benutzers. Der Datenprofil-Viewer verwendet nicht die Verbindungsinformationen, die im Paket gespeichert sind, das den Datenprofilerstellungs-Task ausgeführt hat.  
  
> [!IMPORTANT]  
>  Die Drilldownfunktion, die im Datenprofil-Viewer verfügbar ist, sendet Live-Abfragen an die ursprüngliche Datenquelle. Diese Abfragen haben möglicherweise negative Auswirkungen auf die Leistung des Servers.  
>   
>  Wenn Sie einen Drilldown für eine Ausgabedatei ausführen, die nicht vor kurzem erstellt wurde, geben die Drilldownabfragen möglicherweise nicht die Gruppe von Zeilen zurück, mit denen die ursprüngliche Ausgabe berechnet wurde.  
  
 Weitere Informationen zur Benutzeroberfläche des Datenprofil-Viewers finden Sie unter [Data Profile Viewer F1 Help]().  
  
## <a name="data-profile-viewer-f1-help"></a>Datenprofil-Viewer (F1-Hilfe)
  Verwenden Sie den Datenprofil-Viewer, um die Ausgabe des Datenprofilerstellungs-Tasks anzuzeigen.  
  
 Weitere Informationen zum Verwenden des Datenprofil-Viewers finden Sie unter [Datenprofil-Viewer](../../integration-services/control-flow/data-profile-viewer.md). Weitere Informationen zum Verwenden des Datenprofilerstellungs-Tasks, der die Profilausgabe erstellt, die Sie im Datenprofil-Viewer analysieren, finden Sie unter [Einrichten von Datenprofilerstellungs-Tasks](../../integration-services/control-flow/setup-of-the-data-profiling-task.md).  
  
### <a name="static-options"></a>Statische Optionen  
 **Öffnen**  
 Klicken Sie, um nach der gespeicherten Datei zu suchen, die die Ausgabe des Datenprofilerstellungs-Tasks enthält.  
  
 Bereich **Profile**  
 Erweitern Sie die Struktur im Bereich **Profile** , um die Profile anzuzeigen, die in der Ausgabe enthalten sind. Wählen Sie ein Profil aus, um die Ergebnisse für dieses Profil anzuzeigen.  
  
 Bereich **Meldung**  
 Zeigt Statusmeldungen an.  
  
 Bereich **Drilldown**  
 Zeigt die Datensätze an, die mit einem Wert in der Ausgabe übereinstimmen, wenn die vom Datenprofilerstellungs-Task verwendete Datenquelle verfügbar ist.  
  
 Beispiel: Wenn Sie die Ausgabe eines Verteilungsprofils für Spaltenwert für eine Spalte US-Bundesstaat anzeigen, enthält der Bereich **Detaillierte Wertverteilung** möglicherweise eine Zeile für "WA". Doppelklicken Sie auf die Zeile im Bereich **Detaillierte Wertverteilung** , um die Datenreihen anzuzeigen, bei denen der Wert der Bundesstaatspalte im Drilldownbereich „WA“ lautet.  
  
### <a name="dynamic-options"></a>Dynamische Optionen  
  
#### <a name="profile-type--column-length-distribution-profile"></a>Profiltyp = Verteilungsprofil für Spaltenlänge  
  
##### <a name="column-length-distribution-profile---column-pane"></a>Verteilungsprofil für Spaltenlänge – \<column>-Bereich  
 **Mindestlänge**  
 Zeigt die Mindestlänge der Werte in dieser Spalte an.  
  
 **Maximale Länge**  
 Zeigt die maximale Länge der Werte in dieser Spalte an.  
  
 **Führende Leerstellen ignorieren**  
 Zeigt an, ob dieses Profil mit einem **IgnoreLeadingSpaces** -Wert von TRUE oder FALSE berechnet wurde. Diese Eigenschaft wurde auf der Seite **Profilanforderungen** vom Editor für den Datenprofilerstellungs-Task festgelegt.  
  
 **Nachfolgende Leerzeichen ignorieren**  
 Zeigt an, ob dieses Profil mit einem **IgnoreTrailingSpaces** -Wert von TRUE oder FALSE berechnet wurde. Diese Eigenschaft wurde auf der Seite **Profilanforderungen** vom Editor für den Datenprofilerstellungs-Task festgelegt.  
  
 **Zeilenanzahl**  
 Zeigt die Anzahl der Zeilen in der Tabelle oder Sicht an.  
  
##### <a name="detailed-length-distribution-pane"></a>Bereich 'Detaillierte Längenverteilung'  
 **Länge**  
 Zeigt die in der Spalte, für die ein Profil erstellt wurde, gefundenen Spaltenlängen an.  
  
 **Count**  
 Zeigt die Anzahl von Zeilen an, in denen der Wert der Spalte, für die ein Profil erstellt wurde, die in der Spalte **Länge** angezeigte Länge hat.  
  
 **Percentage**  
 Zeigt den Prozentsatz der Zeilen an, in denen der Wert der Spalte, für die ein Profil erstellt wurde, die in der Spalte **Länge** angezeigte Länge hat.  
  
#### <a name="profile-type--column-null-ratio-profile"></a>Profiltyp = Profil für Spalten-NULL-Verhältnis  
  
##### <a name="column-null-ratio-profile---column-pane"></a>Profil für Spalten-NULL-Verhältnis – \<column>-Bereich  
 **NULL-Anzahl**  
 Zeigt die Anzahl der Zeilen an, in denen die Spalte, für die ein Profil erstellt wurde, einen NULL-Wert aufweist.  
  
 **NULL-Prozentsatz**  
 Zeigt den Prozentsatz der Zeilen an, in denen die Spalte, für die ein Profil erstellt wurde, einen NULL-Wert aufweist.  
  
 **Zeilenanzahl**  
 Zeigt die Anzahl der Zeilen in der Tabelle oder Sicht an.  
  
#### <a name="profile-type--column-pattern-profile"></a>Profiltyp = Spaltenmusterprofil  
  
##### <a name="column-pattern-profile---column-pane"></a>Spaltenmusterprofil – \<column>-Bereich  
 **Zeilenanzahl**  
 Zeigt die Anzahl der Zeilen in der Tabelle oder Sicht an.  
  
##### <a name="pattern-distribution-pane"></a>Bereich 'Musterverteilung'  
 **Muster**  
 Zeigt das für die Spalte, für die ein Profil erstellt wurde, berechnete Muster an.  
  
 **Percentage**  
 Zeigt den Prozentsatz der Zeilen an, deren Werte mit dem in der Spalte **Muster** angezeigten Muster übereinstimmen.  
  
#### <a name="profile-type--column-statistics-profile"></a>Profiltyp = Spaltenstatistikprofil  
  
##### <a name="column-statistics-profile---column-pane"></a>Spaltenstatistikprofil – \<column>-Bereich  
 **Mindestanforderungen**  
 Zeigt den in der Spalte, für die ein Profil erstellt wurde, gefundenen Mindestwert an.  
  
 **Maximum**  
 Zeigt den in der Spalte, für die ein Profil erstellt wurde, gefundenen Höchstwert an.  
  
 **Mean**  
 Zeigt den Durchschnitt der in der Spalte, für die ein Profil erstellt wurde, gefundenen Werte an.  
  
 **Standardabweichung**  
 Zeigt die Standardabweichung der in der Spalte, für die ein Profil erstellt wurde, gefundenen Werte an.  
  
#### <a name="profile-type--column-value-distribution-profile"></a>Profiltyp = Verteilungsprofil für Spaltenwert  
  
##### <a name="column-value-distribution-profile---column-pane"></a>Verteilungsprofil für Spaltenwerte – \<column>-Bereich  
 **Anzahl unterschiedlicher Werte**  
 Zeigt die Anzahl unterschiedlicher Werte an, die in der Spalte, für die ein Profil erstellt wurde, gefunden wurden.  
  
 **Zeilenanzahl**  
 Zeigt die Anzahl der Zeilen in der Tabelle oder Sicht an.  
  
##### <a name="detailed-value-distribution-pane"></a>Bereich 'Detaillierte Wertverteilung'  
 **Wert**  
 Zeigt die in der Spalte, für die ein Profil erstellt wurde, gefundenen unterschiedlichen Werte an.  
  
 **Count**  
 Zeigt die Anzahl von Zeilen an, in denen die Spalte, für die ein Profil erstellt wurde, den in der Spalte **Wert** angezeigten Wert aufweist.  
  
 **Percentage**  
 Zeigt den Prozentsatz der Zeilen an, in denen die Spalte, für die ein Profil erstellt wurde, den in der Spalte **Wert** angezeigten Wert aufweist.  
  
#### <a name="profile-type--candidate-key-profile"></a>Profiltyp = Kandidatenschlüsselprofil  
  
##### <a name="candidate-key-profile---table-pane"></a>Kandidatenschlüsselprofil – \<table>-Bereich  
 **Schlüsselspalten**  
 Zeigt die Spalten an, die als Kandidatenschlüssel für die Profilerstellung ausgewählt wurden.  
  
 **Schlüsselstärke**  
 Zeigt die Stärke (als Prozentsatz) der Kandidatenschlüsselspalte oder Kombination von Spalten an. Eine Schlüsselstärke von weniger als 100 % gibt an, dass doppelte Werte vorhanden sind.  
  
##### <a name="key-violations-pane"></a>Bereich 'Schlüsselverletzungen'  
 **\<column1>, \<column2> usw.**  
 Zeigt die doppelten Werte an, die in der Spalte, für die ein Profil erstellt wurde, gefunden wurden.  
  
 **Count**  
 Zeigt die Anzahl der Zeilen an, in denen die angegebene Spalte den in der ersten Spalte angezeigten Wert aufweist.  
  
#### <a name="profile-type--functional-dependency-profile"></a>Profiltyp = Funktionales Abhängigkeitsprofil  
  
##### <a name="functional-dependency-profile-pane"></a>Bereich 'Funktionales Abhängigkeitsprofil'  
 **Bestimmende Spalten**  
 Zeigt die als determinante Spalte ausgewählte(n) Spalte(n) an. In dem Beispiel, in dem dieselbe US-Postleitzahl immer denselben Bundesstaat angeben sollte, ist die Postleitzahl die determinante Spalte.  
  
 **Abhängige Spalten**  
 Zeigt die als abhängige Spalte ausgewählte(n) Spalte(n) an. In dem Beispiel, in dem dieselbe US-Postleitzahl immer denselben Bundesstaat angeben sollte, ist der Bundesstaat die abhängige Spalte.  
  
 **Funktionale Abhängigkeitsstärke**  
 Zeigt die Stärke (als Prozentsatz) der funktionalen Abhängigkeit zwischen Spalten an. Eine Schlüsselstärke von weniger als 100 % gibt an, dass es Fälle gibt, in denen der determinante Wert den abhängigen Wert nicht bestimmt. In dem Beispiel, in dem dieselbe US-Postleitzahl immer denselben Bundesstaat angeben sollte, deutet dies wahrscheinlich darauf hin, dass einige Bundesstaatenwerte ungültig sind.  
  
##### <a name="functional-dependency-violations-pane"></a>Bereich 'Funktionale Abhängigkeitsverletzungen'  
  
> [!NOTE]  
>  Ein hoher Prozentsatz an fehlerhaften Werten in den Daten könnte zu unerwarteten Werten aus einem funktionalen Abhängigkeitsprofil führen. Beispiel: 90 % der Zeilen haben den Bundesstaatenwert "WI" für den PLZ-Wert "98052". Das Profil meldet Zeilen, die den korrekten Bundesstaatenwert "WA" aufweisen, als Verletzungen.  
  
 **\<determinant column name>**  
 Zeigt den Wert der determinanten Spalte oder einer Kombination aus Spalten in dieser Instanz einer funktionalen Abhängigkeitsverletzung an.  
  
 **\<dependent column name>**  
 Zeigt den Wert der abhängigen Spalte in dieser Instanz einer funktionalen Abhängigkeitsverletzung an.  
  
 **Unterstützte Anzahl**  
 Zeigt die Anzahl der Zeilen an, in denen der Wert der determinanten Spalte die abhängige Spalte festlegt.  
  
 **Verletzungsanzahl**  
 Zeigt die Anzahl der Zeilen an, in denen der Wert der determinanten Spalte die abhängige Spalte nicht festlegt. (In diesen Zeilen ist der abhängige Wert der Wert, der in der Spalte **\<dependent column name>** angezeigt wird.)  
  
 **Unterstützter Prozentsatz**  
 Zeigt den Prozentsatz der Zeilen an, in denen die determinante Spalte die abhängige Spalte festlegt.  
  
#### <a name="profile-type--value-inclusion-profile"></a>Profiltyp = Wertinklusionsprofil  
  
##### <a name="value-inclusion-profile-pane"></a>Bereich 'Wertinklusionsprofil'  
 **Untergeordnete Spalten**  
 Zeigt die Spalte oder Kombination aus Spalten an, für die ein Profil erstellt wurde, um zu ermitteln, ob sie sich in den übergeordneten Spalten befinden.  
  
 **Übergeordnete Spalten**  
 Zeigt die Spalte oder Kombination aus Spalten an, für die ein Profil erstellt wurde, um zu ermitteln, ob sie die Werte aus den untergeordneten Spalten enthalten.  
  
 **Inklusionsstärke**  
 Zeigt die Stärke (als Prozentsatz) der Überlappung zwischen Spalten an. Eine Schlüsselstärke von weniger als 100 % gibt an, dass es Fälle gibt, in denen der untergeordnete Wert nicht in den übergeordneten Werten enthalten ist.  
  
##### <a name="inclusion-violations-pane"></a>Bereich 'Inklusionsverletzungen'  
 **\<column1>, \<column2> usw.**  
 Zeigt die Werte aus der bzw. den untergeordneten Spalte(n) an, die nicht in der bzw. den übergeordneten Spalte(n) gefunden wurden.  
  
 **Count**  
 Zeigt die Anzahl der Zeilen an, in denen die angegebene Spalte den in der ersten Spalte angezeigten Wert aufweist.  
