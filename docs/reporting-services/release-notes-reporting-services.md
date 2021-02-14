---
title: Versionshinweise für Reporting Services 2017 und höher | Microsoft-Dokumentation
description: Hier erhalten Sie weitere Informationen zu den Änderungen in SQL Server Reporting Services (SSRS), Version 2017 und höher.
ms.date: 10/11/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.reviewer: maggies
author: casualoak
ms.author: rhys
monikerRange: '>=sql-server-2017'
ms.openlocfilehash: ccf46bca321f3b23b1cd9216aa122e2202ff8c24
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031152"
---
# <a name="release-notes-for-sql-server-reporting-services-ssrs-2017-and-later"></a>Versionshinweise für SQL Server Reporting Services (SSRS) 2017 und höher

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2017-and-later](../includes/ssrs-appliesto-2017-and-later.md)]

Dieser Artikel beschreibt Änderungen in [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] (SSRS) für die Versionen 2017 und höher.

Die Versionshinweise zu Berichts-Viewer-Steuerelementen finden Sie unter [Versionshinweise zu den Berichts-Viewer-Steuerelementen für WebForms und WinForms in SSRS](application-integration/release-notes-ssrs-application-integration.md).

<!--
We are "standardizing" all our 'Release Notes' style articles:

  - File names:   'release-notes-[TechArea-Name].md'


  - Content format:   2-column tables.  No longer using bullet lists.

    - Ideally, the 'Details' column should contain a link to another SSRS documentation article wherein the particular issue fix is discussed in any way.  Or if there is more to say beyond one sentence, the other sentences of elaboration would go into the 'Details' column.


  - H2 header names are kept short, for better display.


  - Try to keep all Release Notes in one .md file.  Avoid having multiple R.N. files whose names differ only by version or date.

    - Seriously consider erasing info about ancient releases that are so old that nobody cares about them anymore.  If you really want to retain ancient info, consider doing so in an HTML comment at the end of the .md file, just in case a Microsoft employee needs to re-examine ancient info.  In any case, this file cannot get ever longer, and some deletion or hiding of oldest info must eventually occur.


  - Do use '::: moniker range=' zones when version 2017 is no longer the only version represented inside this .md file.

    - Use the '=' operator on the moniker, not the '>=' operator.
    - In contrast, in our 'Whats New' articles we do use the '>=', rather than '='.

GeneMi, DevOps = 1467988 (MsEng > TechnicalContent) , 2019/03/19
-->
## <a name="sql-server-2019-reporting-services"></a>SQL Server 2019 Reporting Services

## <a name="15075454810-20200831"></a>15.0.7545.4810, 31.08.2020 
*(Produktversion: 15.0.1102.861)*

| Behobenes Problem | Details |
| :---------- | :------ |
| Sicherheitsupdates  | &nbsp; |
| Unterstützung von eingeschränkten Kommentaranhängen, um keine PDF-Dokumente mehr zuzulassen  | &nbsp; |
| Korrigiert wurde das Abschneiden von Dateinamen beim Exportieren von Berichten mit einem Punkt im Namen  | &nbsp; |
| Behoben wurde ein Problem im Zusammenhang mit Abonnements und dem zh-TW-Sprachcode, das zu Fehlern mit einem ungültigen Datumsformat geführt hat  | &nbsp; |
| Behoben wurde ein Problem mit bestimmten Berichten, bei denen der Zugriff auf die Parameteroption zu einer Endlosschleife führte  | &nbsp; |
| Behoben wurden Probleme in Bezug auf einfache Anführungszeichen in Berichtsnamen  | &nbsp; |
| Behoben wurde ein Problem beim URL-Zugriff, wodurch „FindString“ keine Übereinstimmungen finden konnte  | &nbsp; |
| Behoben wurde ein Problem, bei dem Alternativtext für den PDF-Export nicht korrekt für Multibytezeichen codiert wurde  | &nbsp; |
| Korrigiert wurde die unerwünschte Darstellung eines leeren Bilds unter einem linearen Element  | &nbsp; |
| Korrigiert wurde der fälschlicherweise angezeigte Fehler „Nicht unterstützt“ bei der benutzerdefinierten Authentifizierung in Web Edition  | &nbsp; |
| Behoben wurde ein Problem, bei dem die Bildschirmausgabe in einem Tablix-Bereich eine zusätzliche Zeile und eine zusätzliche Spalte gelesen hat  | &nbsp; |
| Behoben wurde ein Problem mit dem Abschneiden von Bildern beim Zoomen auf Vollbildansicht und dem Befehl „An Größe anpassen“  | &nbsp; |
| Beim Befehlszeilen-Upgrade ist das EULA-Flag nicht mehr erforderlich  | &nbsp; |

## <a name="150724337714-20191101"></a>15.0.7243.37714, 1. November 2019
*(Produktversion: 15.0.1102.675)*

Erste Veröffentlichung


## <a name="sql-server-2017-reporting-services"></a>SQL Server 2017 Reporting Services

## <a name="1406001669-20200831"></a>14.0.600.1669, 31.08.2020 

| Behobenes Problem | Details |
| :---------- | :------ |
| Sicherheitsupdates  | &nbsp; |
| Unterstützung von eingeschränkten Kommentaranhängen, um keine PDF-Dokumente mehr zuzulassen  | &nbsp; |
| Korrigiert wurde das Abschneiden von Dateinamen beim Exportieren von Berichten mit einem Punkt im Namen  | &nbsp; |
| Behoben wurde ein Problem im Zusammenhang mit Abonnements und dem zh-TW-Sprachcode, das zu Fehlern mit einem ungültigen Datumsformat geführt hat  | &nbsp; |

## <a name="1406001572-20200406"></a>14.0.600.1572, 06.04.2020 

| Behobenes Problem | Details |
| :---------- | :------ |
| JQuery-Benutzeroberfläche auf 1.12 aktualisiert  | &nbsp; |
| Groß-/Kleinschreibung von URL korrigiert  | &nbsp; |
| Platzierung von Parametern korrigiert, wenn viele Parameter vorhanden sind  | &nbsp; |
| Fehler „FindString funktioniert nicht beim HTML5-Rendering“ korrigiert  | &nbsp; |
| Analysis Services-Clientbibliotheken so aktualisiert, dass berücksichtigt wird, dass TLS 1.0/1.1 veraltet ist | &nbsp; |

## <a name="1406001451-20191113"></a>14.0.600.1451, 13.11.2019 

| Behobenes Problem | Details |
| :---------- | :------ |
| Sicherheitsupdates | &nbsp; |
| Paginierte Berichte haben nicht ordnungsgemäß mit Filterparametern funktioniert, wenn Momentaufnahmen aktiviert waren.  | &nbsp; |
| Benutzer mit Browserrolle und Standardeinstellungen hatten keine Berechtigungen zum Herunterladen von Excel-Dateien.  | &nbsp; |
| Beim Upgrade auf den Power BI-Berichtsserver von SQL Server 2016 Reporting Services ist ein Fehler aufgetreten. | &nbsp; |
| Nach dem Upgrade von SQL Server 2012 Reporting Services wurde bei Abonnements der Fehler „Im E-Mail-Header wurde ein ungültiges Zeichen gefunden: ','“ angezeigt. | &nbsp; |
| Konfigurationstool: Beim Schließen eines modalen Fensters im Abschnitt „Datenbank“ wurde der Reporting Services-Dienst neu gestartet. | &nbsp; |
| BorderStyle-Eigenschaftenausdrücke von TextBox-Steuerelementen wurden im Excel-Format nicht richtig gerendert.  | &nbsp; |
| Durch ein Paginierungsproblem wurde bei bestimmten Berichten immer wieder dieselbe Seite gerendert, sodass kein Rendern des gesamten Berichts bis zur letzten Seite möglich war. | &nbsp; |

## <a name="1406001274-20190701"></a>14.0.600.1274, 1.7.2019

| Behobenes Problem | Details |
| :---------- | :------ |
| Sicherheitsupdates | &nbsp; |
| Beim Erstellen eines freigegebenen wöchentlichen Zeitplans können keine Wochentage ausgewählt werden. | &nbsp; |
| Im Bericht werden Wagenrückläufe im Word-Format nicht ordnungsgemäß angezeigt. | &nbsp; |
| System Center Operations Manager(SCOM) 2019 funktioniert nicht mehr mit aktuellen SSRS 2017-Upgrades. | &nbsp; |
| Fehler beim Aufrufen der Autorisierungserweiterung für ein freigegebenes Dataset. | &nbsp; |
| Die Logik für die gespeicherte Prozedur „GetAllProperties“ in SSRS 2017 und PBIRS wurde geändert. Dadurch kann die ReportingService2010.GetProperties-Methode des Webdienst-Endpunkts keine Daten für den verknüpften Bericht abrufen. | &nbsp; |
| Der einfache Rasterzeilenkopf im mobilen Bericht wird nicht mehr angezeigt, wenn auf ein Element im Raster geklickt wird. | &nbsp; |
| Das Datumsfeld im datengesteuerten Abonnement kann nicht verwendet werden. | &nbsp; |
| Die geschachtelte Tablix zeigt in SSRS 2016 und höher eine kleine Schriftart oder eine partielle Schriftart an. | &nbsp; |
| Bei Abonnements mit dem DateTime-Parameter tritt ein Fehler auf, nachdem ein Benutzer mit einem anderen Gebietsschema das Abonnement ändert. | &nbsp; |
| Beim Erstellen eines datengesteuerten Abonnements mit NULL-Übermittlungserweiterung wird ein Übermittlungsfehler angezeigt. | &nbsp; |
| Die URL-Codierung ist falsch, wenn der Wert im Excel- oder Word-Format festgelegt wird. | &nbsp; |

## <a name="1406001109-20190212"></a>14.0.600.1109, 12. Februar 2019

| Behobenes Problem | Details |
| :---------- | :------ |
| Nach Änderung des Abonnements ändern sich die Zeitpläne für Cache-Berichtsmomentaufnahmen in „Berichtsspezifischer Zeitplan“. | &nbsp; |
| „rc:Toolbar=false“ hat in der Express Edition nicht funktioniert. | &nbsp; |
| Bestimmte thailändische Zeichen wurden beim Exportieren paginierter Berichte in PDF nicht ordnungsgemäß gerendert. | &nbsp; |
| Deadlock während der Benachrichtigung für abgeschlossene datengesteuerte Abonnements. | &nbsp; |
| Eingebettete Bilder wurden in bestimmten Fällen nicht angezeigt, wenn der Parameter „rc: Toolbar=False“ verwendet wurde. | &nbsp; |
| Für Berichte mit kaskadierenden Parametern konnten keine datengesteuerten Abonnements erstellt werden. | &nbsp; |
| Mit einem ungültigen Intervall konfigurierte Abonnements konnten nicht bearbeitet werden. | &nbsp; |
| Sicherheitsupdates | &nbsp; |
| Benutzeroberfläche für verknüpfte Berichte wurde nicht angezeigt. | &nbsp; |
| Bestimmte paginierte Berichte mit geschachtelten Tablix-Steuerelementen wiesen falsche Schriftarten auf. | &nbsp; |
| In bestimmten paginierten Berichten mit Tablix-Datenbereichen werden fälschlicherweise Leerzeichen hinzugefügt. | &nbsp; |
| Kopfzeilen sind beim Erweitern einfacher Datenraster in Mobilgeräteberichten verschwunden. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600906-20180912"></a>14.0.600.906, 12. September 2018

Das folgende Problem wurde behoben:

| Behobenes Problem | Details |
| :---------- | :------ |
| Benutzerdefinierte Authentifizierung, die keine richtigen Cookieinformationen zurückgegeben hat. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600892-20180831"></a>14.0.600.892, 31. August 2018

| Behobenes Problem | Details |
| :---------- | :------ |
| Ein Textfeld in „Rectangle“ hat bewirkt, dass das Rechteck nicht vertikal erweitert wurde, wenn „rc:Toolbar=False“ festgelegt und in dem Feld viel Text enthalten war. | &nbsp; |
| Die Textgröße wurde nicht skaliert, wenn „pageHeight“ unter 0,5 Zoll lag. | &nbsp; |
| Bei Verwendung mit CRM tritt in der SSRS-Katalogdatenbank ein Deadlock auf. | &nbsp; |
| Vertikal ausgerichtete Spaltenüberschriften wurden beim Herunterscrollen im Bericht falsch angezeigt. | &nbsp; |
| Für Benutzer, die der System Center Operations Manager-Berichterstellungsrolle hinzugefügt werden, ist der Zugriff auf das SSRS-Webportal blockiert. | &nbsp; |
| Thailändische Zeichen werden nicht richtig in das PDF-Format exportiert. | &nbsp; |
| Behavior Change bei der Browserrolle. | &nbsp; |
| „rc:Toolbar=false“ hat in der Express Edition nicht funktioniert. | &nbsp; |
| Im Eingabeaufforderungsbereich für Parameter fehlt die vertikale Scrollleiste. | &nbsp; |
| Die Laufzeit des Mobilgeräteberichts wurde aktualisiert. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600744-20180425"></a>14.0.600.744, 25. April 2018

| Behobenes Problem | Details |
| :---------- | :------ |
| Auf der Seite „Datengesteuertes Abonnement“ wurde nach der Erstellung die Übermittlungsoption nicht angezeigt. | &nbsp; |
| Das Upgrade von SSRS 2012 auf SSRS 2017 hat dazu geführt, dass RSManagement alle paar Sekunden eine Ausnahme auslöst. | &nbsp; |
| Die Standardwerte für mehrwertige Parameter in IE11 konnten nicht geändert werden. | &nbsp; |
| Zeitpläne waren stets leer, wenn freigegebener Zeitplan ausgeführt wurde. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600689-20180228"></a>14.0.600.689, 28. Februar 2018

| Behobenes Problem | Details |
| :---------- | :------ |
| Die Sichtbarkeit von Berichtsparametern in einem verknüpften Bericht wurde nach dem Bearbeiten der zugehörigen Eigenschaften zurückgesetzt. | &nbsp; |
| Der URL-Parameter „rc:Toolbar = false“ hat in der Express Edition nicht funktioniert. | &nbsp; |
| Wenn Ausdrücke in einem Textfeld mit der Eigenschaft „CanGrow“ auf „false“ gesetzt sind, werden die Werte nicht angezeigt. | &nbsp; |
| Der Link _Weitere Informationen_ für den Product Key wurde zum Setup hinzugefügt. | &nbsp; |
| Das Webportal mit der benutzerdefinierten Formularauthentifizierung hat Cookie mit variablem Ablauf ignoriert. | &nbsp; |
| Beim Export nach Word wurde eine ungleiche Zeilenhöhe erstellt, wenn der Zeileninhalt leer war. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="140600594-20180109"></a>14.0.600.594, 09. Januar 2018

Implementierung einiger Sicherheitsupdates.

### <a name="140600490-20171101"></a>14.0.600.490, 01. November 2017

Die Probleme beim SKU-Upgrade wurden behoben.

## <a name="140600451-20170930"></a>14.0.600.451, 30. September 2017

Erste Veröffentlichung

## <a name="next-steps"></a>Nächste Schritte

[Neuerungen in Reporting Services (SSRS)](what-s-new-in-sql-server-reporting-services-ssrs.md)

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231).
