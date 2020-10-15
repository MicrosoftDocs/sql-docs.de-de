---
title: Integrierte globale Variablen und Benutzerverweise (Berichts-Generator) | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie mehr über die Werte in den Ausdrücken, mit denen die Berichtsergebnisse im Berichts-Generator gefiltert werden können.
ms.date: 08/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 5f5e1149-c967-454d-9a63-18ec4a33d985
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4861b5688c485ca0f8245bdf149ed995841ecdee
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91935446"
---
# <a name="built-in-collections---built-in-globals-and-users-references-report-builder"></a>Integrierte Auflistungen – Integrierte globale Variablen und Benutzerverweise (Berichts Generator)
  Die integrierte Feldsammlung, die sowohl die **Globals** -Sammlung als auch die **User** -Sammlung umfasst, stellt globale Werte dar, die von Reporting Services beim Verarbeiten eines Berichts bereitgestellt werden. Die **Globals** -Auflistung enthält Werte wie den Namen des Berichts, die Startzeit der Berichtsverarbeitung und die aktuellen Seitenzahlen für den Berichtskopf oder -fuß. Die **User** -Auflistung stellt die Benutzer-ID und Spracheinstellungen bereit. Diese Werte können in Ausdrücken verwendet werden, um Ergebnisse in einem Bericht zu filtern.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="using-the-globals-collection"></a>Verwenden der Globals-Auflistung  
 Die **Globals** -Auflistung enthält die globalen Variablen für den Bericht. Auf der Entwurfsoberfläche werden diese Variablen mit einem & (kaufmännisches Und-Zeichen) als Präfix angezeigt, z.B. `[&ReportName]`. In der folgenden Tabelle sind die Elemente der **Globals** -Auflistung beschrieben.  
  
|**Member**|**Typ**|**Beschreibung**|  
|----------------|--------------|---------------------|  
|ExecutionTime|**DateTime**|Das Datum und die Uhrzeit, zu der die Berichtsausführung begann.|  
|PageNumber|**Integer**|Die aktuelle Seitenzahl in Relation zu Seitenumbrüchen, mit denen die Seitenzahl zurückgesetzt wird. Am Anfang der Berichtsverarbeitung wird der Anfangswert auf 1 festgelegt. Die Seitenzahl wird mit jeder gerenderten Seite inkrementiert.<br /><br /> Um Seiten innerhalb der Seitenumbrüche bei einem Rechteck, einem Datenbereich, einer Datenbereichsgruppe oder einer Karte zu zählen, legen Sie für die PageBreak-Eigenschaft die ResetPageNumber-Eigenschaft auf **TRUE**fest. Dies wird nicht in den Hierarchiegruppen von Tablix-Spalten unterstützt.<br /><br /> PageNumber kann nur in einem Ausdruck in einem Seitenkopf oder Seitenfuß verwendet werden.|  
|ReportFolder|**String**|Der vollständige Pfad des Ordners mit dem Bericht. Dies schließt nicht die Berichtsserver-URL ein.|  
|ReportName|**String**|Der Name, unter dem der Bericht in der Berichtsserver-Datenbank gespeichert wird.|  
|ReportServerUrl|**String**|Die URL des Berichtsservers, auf dem der Bericht ausgeführt wird.|  
|TotalPages|**Integer**|Die Gesamtseitenzahl in Relation zu Seitenumbrüchen, mit denen PageNumber zurückgesetzt wird. Wenn keine Seitenumbrüche festgelegt werden, entspricht dieser Wert dem OverallTotalPages-Wert.<br /><br /> TotalPages kann nur in einem Ausdruck in einem Seitenkopf oder Seitenfuß verwendet werden.|  
|PageName|**String**|Der Name der Seite. Zu Beginn der Berichtsverarbeitung wird der Anfangswert von InitialPageName, einer Berichtseigenschaft, festgelegt. Wenn die einzelnen Berichtselemente verarbeitet werden, wird dieser Wert vom entsprechenden Wert von PageName eines Rechtecks, eines Datenbereichs, einer Datenbereichsgruppe oder einer Karte ersetzt. Dies wird nicht in den Hierarchiegruppen von Tablix-Spalten unterstützt.<br /><br /> PageName kann nur in einem Ausdruck in einem Seitenkopf oder Seitenfuß verwendet werden.|  
|OverallPageNumber|**Integer**|Die Seitenzahl der aktuellen Seite im ganzen Bericht. ResetPageNumber hat keine Auswirkungen auf diesen Wert.<br /><br /> OverallPageNumber kann nur in einem Ausdruck in einem Seitenkopf oder Seitenfuß verwendet werden.|  
|OverallTotalPages|**Integer**|Die Gesamtanzahl der Seiten für den gesamten Bericht. ResetPageNumber hat keine Auswirkungen auf diesen Wert.<br /><br /> OverallTotalPages kann nur in einem Ausdruck in einem Seitenkopf oder Seitenfuß verwendet werden.|  
|RenderFormat|**Renderformat**|Informationen zur aktuellen Renderinganforderung.<br /><br /> Weitere Informationen finden Sie unter "RenderFormat" im nächsten Abschnitt.|  
  
 Elemente der **Globals** -Auflistung geben eine Variante zurück. Wenn Sie ein Element dieser Auflistung in einem Ausdruck verwenden möchten, der einen bestimmten Datentyp erfordert, müssen Sie die Variable zunächst umwandeln. Zum Konvertieren der Ausführungszeitvariante in ein Datumsformat können Sie beispielsweise `=CDate(Globals!ExecutionTime)`verwenden. Weitere Informationen finden Sie unter [Datentypen in Ausdrücken &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md).  
  
### <a name="renderformat"></a>RenderFormat  
 In der folgenden Tabelle werden die Elemente von **RenderFormat**beschrieben.  
  
|Member|Typ|BESCHREIBUNG|  
|------------|----------|-----------------|  
|Name|**String**|Der Name des Renderers laut Registrierung in der RSReportServer-Konfigurationsdatei.<br /><br /> Verfügbar während bestimmter Teile des Berichtsverarbeitungs-/Renderingzyklus.|  
|IsInteractive|**Boolescher Wert**|Gibt an, ob die aktuelle Renderinganforderung ein interaktives Renderingformat verwendet.|  
|DeviceInfo|Schreibgeschützte Namens-/Werteauflistung|Schlüssel-Wert-Paare für die deviceinfo-Parameter der aktuellen Renderinganforderung.<br /><br /> Zeichenfolgenwerte können mit dem Schlüssel oder einem Index in der Auflistung angegeben werden.|  
  
### <a name="examples"></a>Beispiele  
 In den folgenden Beispielen wird gezeigt, wie ein Verweis auf die **Globals** -Auflistung in einem Ausdruck verwendet wird:  
  
-   Dieser Ausdruck ist in einem Textfeld in der Fußzeile eines Berichts platziert und stellt die Seitenzahl und die Gesamtseitenzahl im Bericht bereit:  
  
     `=Globals.PageNumber & " of " & Globals.TotalPages`  
  
-   Dieser Ausdruck stellt den Namen des Berichts und die Zeit seiner Ausführung bereit. Die Zeit wird mit der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]-Formatzeichenfolge für ein kurzes Datum formatiert:  
  
     `=Globals.ReportName & ", dated " & Format(Globals.ExecutionTime, "d")`  
  
-   Wenn dieser Ausdruck für eine ausgewählte Spalte im Dialogfeld **Spaltensichtbarkeit** platziert wird, wird die Spalte nur angezeigt, wenn der Bericht nach Excel exportiert wird. Andernfalls ist die Spalte ausgeblendet.  
  
     `EXCELOPENXML` bezieht sich auf das Excel-Format aus Office 2007. `EXCEL` bezieht sich auf das Excel-Format aus Office 2003.  
  
     `=IIF(Globals!RenderFormat.Name = "EXCELOPENXML" OR Globals!RenderFormat.Name = "EXCEL", false, true)`  
  
## <a name="using-the-user-collection"></a>Verwenden der User-Auflistung  
 Die **User** -Auflistung enthält Daten zu dem Benutzer, der den Bericht ausführt. Mit dieser Auflistung können Sie die in einem Bericht angezeigten Daten filtern, sodass z. B. nur die Daten des aktuellen Benutzers angezeigt werden, oder beispielsweise die Benutzer-ID in einem Berichtstitel anzeigen. Auf der Entwurfsoberfläche werden diese Variablen mit einem & (kaufmännisches Und-Zeichen) als Präfix angezeigt, z.B. `[&UserID]`.  
  
 In der folgenden Tabelle sind die Elemente der **User** -Auflistung beschrieben.  
  
|**Member**|**Typ**|**Beschreibung**|  
|----------------|--------------|---------------------|  
|**Sprache**|**String**|Die Sprache des Benutzers, der den Bericht ausführt. Beispiel: `en-US`.|  
|**UserID**|**String**|Die Benutzer-ID des Benutzers, der den Bericht ausführt. Wenn Sie die Windows-Authentifizierung verwenden, stellt dieser Wert das Domänenkonto des aktuellen Benutzers dar. Der Wert wird von der [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Sicherheitserweiterung bestimmt, für die die Windows-Authentifizierung oder die benutzerdefinierte Authentifizierung verwendet werden kann.|  
 
### <a name="using-locale-settings"></a>Verwenden von Gebietsschemaeinstellungen  
 Sie können Ausdrücke verwenden, um auf die Gebietsschemaeinstellungen auf einem Clientcomputer zu verweisen. Mithilfe des **User.Language** -Wertes können Sie bestimmen, wie ein Bericht für den Benutzer angezeigt wird. Beispielsweise können Sie einen Bericht erstellen, in dem je nach Gebietsschemawert ein unterschiedlicher Abfrageausdruck verwendet wird. Die Abfrage kann z. B. so geändert werden, dass abhängig von der zurückgegebenen Sprache lokalisierte Informationen aus unterschiedlichen Spalten abgerufen werden. Sie können auch in den Spracheinstellungen des Berichts oder Berichtselements einen Ausdruck verwenden, der auf dieser Variablen basiert.  
  
> [!NOTE]  
>  Wenn Sie die Spracheinstellungen eines Berichts ändern, müssen Sie darauf achten, dass dies keine Probleme bei der Anzeige verursacht. Durch das Ändern der Gebietsschemaeinstellung des Berichts kann beispielsweise das Datumsformat im Bericht geändert werden, gleichzeitig ändert sich jedoch möglicherweise auch das Währungsformat. Falls kein Konvertierungsprozess für die Währung installiert ist, wird möglicherweise das falsche Währungssymbol im Bericht angezeigt. Sie können dies vermeiden, indem Sie die Sprachinformationen für die einzelnen Elemente festlegen, die Sie ändern möchten, oder indem Sie das Element mit den Währungsdaten auf eine bestimmte Sprache festlegen.  
  
### <a name="identifying-userid-for-snapshot-or-history-reports"></a>Identifizieren von UserID für Momentaufnahmen oder Verlaufsberichte  
 In manchen Fällen zeigen Berichte, die die *User!UserID* -Variable enthalten, spezifische Berichtsdaten für den aktuellen Benutzer, der den Bericht anzeigt, nicht an.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Ausdrücke &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)   
 [Dialogfeld „Ausdruck“ (Berichts-Generator)](./expressions-report-builder-and-ssrs.md)   
 [Datentypen in Ausdrücken (Berichts-Generator und SSRS)](../../reporting-services/report-design/data-types-in-expressions-report-builder-and-ssrs.md)   
 [Formatieren von Zahlen und Datumsangaben &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/formatting-numbers-and-dates-report-builder-and-ssrs.md)   
 [Beispiele für Ausdrücke &#40;Berichts-Generator und SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)  
  
