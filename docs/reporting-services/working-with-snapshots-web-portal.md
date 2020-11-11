---
title: Arbeiten mit Momentaufnahmen (Webportal) | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie eine Momentaufnahme eines Zwischenspeichers erstellen, um ein schnelleres Laden bestimmter Ausführungseigenschaften zu ermöglichen. Mithilfe von Verlaufsmomentaufnahmen können Sie auch zu bestimmten Zeitpunkten Momentaufnahmen durchführen.
ms.date: 07/02/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 9ae20556-e243-4a60-b076-9fd9e82c7355
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: c17541ff08f136b3c8c26418117df63c71b9195b
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243809"
---
# <a name="working-with-snapshots-web-portal"></a>Arbeiten mit Momentaufnahmen (Webportal)

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)]

Sie können steuern, ob Momentaufnahmen für einen Bericht erstellt werden, indem Sie die **Auslassungspunkte (...)** eines Berichts gefolgt von **Verwalten** und **Caching** oder **Verlaufsmomentaufnahmen** auswählen.  
  
> [!NOTE]
> Der SQL Server-Agent-Dienst muss gestartet werden.  
   
Sie können eine Cachemomentaufnahme für schnelleres Laden von bestimmten Ausführungseigenschaften erstellen. Mithilfe von Verlaufsmomentaufnahmen können Sie auch zu bestimmten Zeitpunkten Momentaufnahmen durchführen.  
  
## <a name="creating-a-cache-snapshot"></a>Erstellen einer Cachemomentaufnahme  
  
Sie können eine Momentaufnahme erstellen, indem Sie folgende Schritte ausführen.  
  
![Screenshot: Bildschirm „Caching“ des Dialogfelds „Company Sales bearbeiten“ mit drei pinkfarbenen Kreisen, die die Zahlen 1, 2 und 3 enthalten, um die drei Schritte zu kennzeichnen, die unterhalb des Screenshots beschrieben werden](../reporting-services/media/ssrswebportal-report-caching4.png)  
  
1.  Wählen Sie auf der Seite **Caching** den Befehl **Diesen Bericht immer für vorher generierte Momentaufnahmen ausführen** aus, um die Optionen zum Erstellen einer Momentaufnahme zu aktivieren.  
  
2.  Wählen Sie **Cachemomentaufnahmen nach Zeitplan erstellen** aus, wenn Sie eine wiederholte Momentaufnahme planen. Sie können einen freigegebenen Zeitplan verwenden oder einen benutzerdefinierten Zeitplan definieren, um die Momentaufnahme zu aktualisieren.  
  
3.  Wählen Sie **Cachemomentaufnahme erstellen, wenn ich auf dieser Seite auf "Anwenden" klicke** aus, wenn Sie sofort eine Cachemomentaufnahme erstellen möchten. Wenn Sie nur diese Option auswählen, wird die Momentaufnahme nicht aktualisiert.  
  
## <a name="create-modify-and-delete-history-snapshots"></a>Erstellen, Ändern und Löschen von Verlaufsmomentaufnahmen  
  
Zum Arbeiten mit Berichtsverlaufs-Momentaufnahmen, verwalten Sie einen Bericht, und wählen Sie **Verlaufsmomentaufnahmen** aus.  
  
Mithilfe der Seite **Verlaufsmomentaufnahmen** können Sie die im Laufe der Zeit generierten und gespeicherten Berichtsmomentaufnahmen anzeigen. Abhängig von den für den Berichtsserver festgelegten Optionen enthält der Verlauf möglicherweise nur die neuesten Momentaufnahmen.  
  
Der Berichtsverlauf wird immer im Kontext des Berichts angezeigt, aus dem er stammt. Es ist nicht möglich, den Verlauf aller Berichte auf einem Berichtsserver zentral anzuzeigen.  
  
Wenn Sie die Verlaufsmomentaufnahme generieren möchten, muss der Bericht unbeaufsichtigt ausgeführt werden können, d.h., er muss gespeicherte Anmeldeinformationen verwenden, und parametrisierte Berichte müssen Standardparameterwerte für alle Parameter enthalten. Der Berichtsverlauf kann manuell oder als geplante Operation generiert werden. Verlaufseigenschaften im Bericht bestimmen die Art und Weise, in der der Berichtsverlauf erstellt werden kann.  
  
![Screenshot: Bildschirm „Verlaufsmomentaufnahmen“ des Dialogfelds „Company Sales bearbeiten“ mit drei pinkfarbenen Kreisen, die die Zahlen 1, 2 und 3 enthalten, um die drei Schritte zu kennzeichnen, die unterhalb des Screenshots beschrieben werden](../reporting-services/media/ssrswebportal-historysnapshots1.png)  
   
1.  Wählen Sie zum Erstellen einer Verlaufsmomentaufnahme **Neue Verlaufsmomentaufnahme** aus. Dadurch wird der Bericht verarbeitet und ein Eintrag zur Liste hinzugefügt.  
  
2.  Sie können nun in den Einstellungen die Zeitpläne und Beibehaltungsrichtlinien definieren.  
  
3.  Sie können eine Verlaufsmomentaufnahme auswählen, um diese anzuzeigen. Die im Berichtsverlauf angezeigten Momentaufnahmen unterscheiden sich nur durch das Datum und die Uhrzeit ihrer Erstellung. Es gibt keinen grafischen Hinweis, ob eine Momentaufnahme als Folge eines Zeitplans oder durch einen manuellen Vorgang generiert wurde.  
  
### <a name="schedule-and-settings"></a>Zeitplan und Einstellungen  
  
Die Auswahl **Zeitplan und Einstellungen** stellt zusätzliche Optionen zur Planung und Steuerung der Beibehaltung von erstellten Momentaufnahmen bereit.  
  
![Screenshot der Abschnitte „Zeitplan“ und „Erweitert“](../reporting-services/media/ssrswebportal-historysnapshots2.png)  
   
Optional können Sie einen Zeitplan für die Momentaufnahmen erstellen, damit diese erstellt werden. Sie können auch verhindern, dass andere Personen neue Momentaufnahmen erstellen. Wenn Sie die Option **Benutzern das manuelle Erstellen von Momentaufnahmen gestatten** deaktivieren, wird die Schaltfläche **+ Neue Verlaufsmomentaufnahme** (Neuer Momentaufnahmenverlauf) deaktiviert.  
  
Sie können auch definieren, wie Momentaufnahmen beibehalten werden sollen.  
  
**Cachemomentaufnahmen auch in Berichtsverlauf speichern**  
  
Mit dieser Option wird eine Berichtsmomentaufnahme, die auf Grundlage von Berichtsausführungseigenschaften generiert wird, in den Berichtsverlauf kopiert. Sie können Berichtsausführungseigenschaften festlegen, um einen Bericht aus einer generierten Momentaufnahme auszuführen. Wenn Sie diese Eigenschaft für den Berichtsverlauf festlegen, können Sie einen Datensatz mit allen im Laufe der Zeit generierten Berichtsmomentaufnahmen speichern, indem Sie die Kopien der Berichtsmomentaufnahmen im Berichtsverlauf platzieren.

## <a name="next-steps"></a>Nächste Schritte

[Web portal (Webportal)](../reporting-services/web-portal-ssrs-native-mode.md)  
[Working with paginated reports (Arbeiten mit paginierten Berichten)](working-with-paginated-reports-web-portal.md)  
[Work with shared datasets (Arbeiten mit freigegebenen Datasets)](../reporting-services/work-with-shared-datasets-web-portal.md)

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
