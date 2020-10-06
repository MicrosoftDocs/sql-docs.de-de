---
description: Ändern von Domänenwerten
title: Ändern von Domänenwerten
ms.date: 11/08/2011
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
f1_keywords:
- sql13.dqs.dm.values.f1
ms.assetid: 8c90ab70-3aea-4eaf-a174-4159485c87d3
author: swinarko
ms.author: sawinark
ms.openlocfilehash: bf85cb8b432dcc6bf72208c12b934291bc72b049
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91724644"
---
# <a name="change-domain-values"></a>Ändern von Domänenwerten

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sqlserver.md)]

  In diesem Thema wird beschrieben, wie die Metadaten in einer Wissensdatenbank in [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) geändert und erweitert werden. Nachdem Sie Wissen durch die Wissensermittlung generiert haben, Wissen in die Wissensdatenbank oder Domänen importiert haben, oder eine Wissensdatenbank basierend auf einer anderen Wissensdatenbank erstellt haben, können Sie die Datenwerte interaktiv ändern. Die Generierung der Wissensdatenbank nutzt nicht nur computerunterstützte Prozesse, sondern ermöglicht Ihnen,, eigenes Wissen zu verwenden, um die Datenwerte zu überprüfen und sie wie folgt zu ändern:  
  
-   Hinzufügen eines Domänenwerts zur Wertliste oder Auswählen eines Werts und Löschen des Werts aus der Liste  
  
-   Ändern des Status eines Domänenwerts, der durch den DQS-Erkennungsprozess festgelegt wurde, in „Richtig“, „Fehler“ oder „Ungültig“  
  
-   Geben Sie für einen falschen oder ungültigen Wert einen Ersetzungswert ein. Ein Wert ist ungültig, wenn er nicht in die Domäne gehört, z. B., wenn er dem Domänendatentyp nicht entspricht oder eine Domänenregel nicht einhält. Ein Wert ist fehlerhaft, wenn er zwar in die Domäne gehört, aber einen Syntaxfehler aufweist.  
  
-   Legen Sie zwei oder mehr Werte als Synonyme fest, und ändern Sie den vom Erkennungsprozess festgelegten führenden Wert, so dass der führende Wert den Synonymwert ersetzt, wenn die Eigenschaft **Führenden Wert verwenden** beim Erstellen der Domäne festgelegt wurde.  
  
-   Importieren von Domänenwerten aus einer Excel-Datei  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Voraussetzungen  
 Um einen Domänenwert zu ändern, müssen Sie eine Wissensdatenbank und eine Domäne in der Domänenverwaltungsaktivität geöffnet haben.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Sie müssen über die Rolle „dqs_kb_editor“ oder „dqs_administrator“ in der DQS_MAIN-Datenbank verfügen, um Domänenwerte zu ändern.  
  
##  <a name="change-domain-values"></a><a name="Change"></a> Ändern von Domänen Werten  
 In der Tabelle **Wert** wird der Wissensdatenbank für eine einzelne Domäne hinzugefügtes Wissen angezeigt. Sie können jederzeit in der Domänenliste eine andere Domäne auswählen, um die Werte für diese Domäne anzuzeigen. Das Feld enthält die folgenden Spalten:  
  
-   In der Spalte **Wert** werden alle Werte angezeigt, die während des Ermittlungsprozesses der ausgewählten Domäne aus einem Feld im Datenbeispiel hinzugefügt wurden. Alle Werte, die als Fehler projiziert werden, werden als Synonym zu einem Wert angezeigt, der als richtig projiziert wird.  
  
-   In der Spalte **Typ** wird der Status des Werts angezeigt, wie vom Ermittlungsprozess bestimmt. Ein grünes Häkchen zeigt an, dass der Wert richtig ist oder korrigiert wurde. Ein rotes Kreuz zeigt an, dass der Wert einen Fehler aufweist. Ein orangefarbenes Dreieck mit einem Ausrufezeichen zeigt an, dass der Wert nicht gültig ist. Ein Wert, der nicht gültig ist, entspricht nicht den Datenanforderungen für die Domäne. Ein Wert, der einen Fehler aufweist, kann gültig sein, ist aber aus Datengründen nicht der richtige Wert.  
  
-   In der Spalte **Korrigieren in** wird ein richtiger Wert angezeigt, in den der Originalwert geändert wird, der als fehlerhaft oder ungültig gekennzeichnet wurde. DQS kann in Folge des Ermittlungsprozesses den richtigen Wert vorschlagen.  
  
 Um Werte zu ändern, gehen Sie wie folgt vor:  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)][Führen Sie die Data Quality-Client Anwendung](../data-quality-services/run-the-data-quality-client-application.md)aus.  
  
2.  Öffnen oder erstellen Sie im [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] -Startbildschirm eine Wissensdatenbank. Wählen Sie **Domänenverwaltung** als Aktivität aus, und klicken Sie dann auf **Öffnen** oder **Erstellen**. Weitere Informationen finden Sie unter [Erstellen einer Wissensdatenbank](../data-quality-services/create-a-knowledge-base.md) oder [Öffnen einer Wissensdatenbank](../data-quality-services/open-a-knowledge-base.md).  
  
    > [!NOTE]  
    >  Die Domänenverwaltung wird auf einer Seite des Data Quality Service-Clients ausgeführt, der fünf Registerkarten für separate Domänenverwaltungsvorgänge enthält. Es ist kein assistentengesteuerter Prozess; jeder Verwaltungsvorgang kann getrennt ausgeführt werden.  
  
3.  Wählen Sie aus der **Domänenliste** auf der Seite **Domänenverwaltung** die Domäne aus, in der Sie Werte ändern möchten, oder erstellen Sie eine neue Domäne. Wenn Sie eine neue Domäne erstellen müssen, finden Sie unter [Domäne erstellen](../data-quality-services/create-a-domain.md)weitere Informationen. Klicken Sie auf die Registerkarte **Domänenwerte** .  
  
4.  Zeigen Sie die zu ändernden Werte in der Tabelle **Wert** an. Weitere Informationen finden Sie weiter unten unter [So zeigen Sie die entsprechenden Werte an](#Display) .  
  
5.  Um den Status eines Werts zu ändern, gehen Sie wie folgt vor:  
  
    -   **Ausgewählte Domänen Werte als korrigiert festlegen**: um den Status eines Werts von "Fehler" oder "ungültig" in "richtig" zu ändern, wählen Sie den Wert aus, und klicken Sie dann auf der Symbolleiste in der Symbolleiste oder in der Dropdown Liste Typ auf die Option **ausgewählte Domänen Werte als korrigiert festlegen** (überprüfen). Wenn der fehlerhafte oder ungültige Wert mit einem richtigen Wert gruppiert ist, löschen Sie diesen Wert nach dem Vorgang.  
  
    -   **Ausgewählte Domänen Werte als Fehler festlegen**: um den Status eines Werts von "richtig" oder "ungültig" in "Fehler" zu ändern, wählen Sie den Wert aus, und klicken Sie dann auf das Symbol **ausgewählte Domänen Werte als Fehler festlegen** (Kreuz) aus dem Pfeil nach unten auf der Symbolleiste oder in der Dropdown Liste Typ. Sie können eine Korrektur in die Spalte **Korrigieren in** eingeben oder diese leer lassen.  
  
    -   **Ausgewählte Domänenwerte als ungültig festlegen**: Wählen Sie zum Ändern des Status eines Werts von „Richtig“ oder „Fehler“ in „Ungültig“ den Wert aus, und klicken Sie anschließend nach Auswahl des Abwärtspfeils in der Symbolleiste oder in der Dropdownliste „Typ“ auf das Symbol **Ausgewählte Domänenwerte als ungültig festlegen** (Dreieck). Sie können eine Korrektur in die Spalte **Korrigieren in** eingeben oder diese leer lassen.  
  
    -   **Korrigieren in**: Nachdem Sie einen Wert als fehlerhaft oderungültig festgelegt haben, geben Sie in die Spalte **Korrigieren in** einen neuen Wert ein. DQS fügt eine neue Zeile für den Ersatzwert hinzu, legt diesen als richtig fest und gruppiert dann die zwei Werte. Der neue Wert wird als führender Wert angezeigt. Dabei wird der führende Wert fett und der fehlerhafte oder ungültige Wert eingezogen dargestellt.  
  
6.  Um Werte als Gruppe von Synonymen festzulegen, wählen Sie mehrere richtige Werte aus, und gehen Sie dann wie folgt vor:  
  
    -   **Ausgewählte Domänenwerte als Synonyme festlegen**: Wählen Sie zum Festlegen von Synonymen mehrere richtige Werte aus, und klicken Sie dann auf das Symbol **Ausgewählte Domänenwerte als Synonyme festlegen** . DQS gruppiert die Werte und legt einen der Werte als führenden Wert fest, durch den die anderen Werte ersetzt werden. Wenn zwei Werte gruppiert werden, aber einer der Werte in der Gruppe fehlerhaft oder ungültig ist, sind die Werte keine Synonyme.  
  
        > [!NOTE]  
        >  Wenn Sie zwei oder mehr Werte in einer Gruppe und einen anderen Wert außerhalb der Gruppe auswählen und diese dann als Synonyme festlegen, erhalten Sie eine falsche Fehlermeldung. Die Werte werden ordnungsgemäß als Synonyme festgelegt, nachdem Sie die Fehlermeldung geschlossen haben.  
  
    -   **Beziehung zwischen ausgewählten Synonymen aufheben**: Um die Synonymzuordnung zwischen zwei oder mehr Werten aufzuheben, wählen Sie die Werte aus, und klicken Sie dann auf das Symbol **Beziehung zwischen ausgewählten Synonymen aufheben** . Die Werte müssen gruppiert und beide korrekt sein, damit die Gruppierung als Synonyme aufzuheben.  
  
    -   **Ausgewählten Domänenwert als führenden Wert der zugehörigen Gruppe festlegen**: Um den führenden Wert der Gruppe zu ändern, wählen Sie einen Wert in der gruppe aus, der nicht als führender Wert festgelegt ist, und klicken Sie dann auf die Schaltfläche **Ausgewählten Domänenwert als führenden Wert der zugehörigen Gruppe festlegen** . Dies legt den führenden Wert als Ersatz für den anderen Wert fest. Dieser Vorgang kann nur verwendet werden, wenn Sie mindestens zwei gruppierte Werte festgelegt haben und Sie den von DQS festgelegten führenden Wert ändern möchten. Beachten Sie, dass der führende Wert durch eine blaue Zeile mit dem Wert in Fettdruck gekennzeichnet wird.  
  
7.  **Rechtschreibprüfung**: Wenn ein Wert eine wellige rote Unterstreichung aufweist, schlägt die Rechtschreibprüfung eine Korrektur für den Wert vor. Klicken Sie mit der rechten Maustaste auf den unterstrichenen Wert, und wählen Sie eine Korrektur aus, sofern zutreffend. Als Werttyp wird „Fehler“ festgelegt (oder beibehalten), und die Korrektur wird zur Spalte **Korrigieren in** hinzugefügt. Klicken Sie auf den Abwärtspfeil, um weitere vorgeschlagene Korrekturen anzuzeigen. Geben Sie eine Korrektur manuell ein, um sie dem Rechtschreibprüfungswörterbuch hinzuzufügen und als Korrektur auswählen zu können. Weitere Informationen finden Sie unter [Verwenden der DQS-Rechtschreibprüfung](../data-quality-services/use-the-dqs-speller.md) und [Domain-Eigenschaften festlegen](../data-quality-services/set-domain-properties.md).  
  
    > [!NOTE]  
    >  Um die Rechtschreibprüfung zu verwenden, können Sie diese auf der Seite **Domäneneigenschaften** aktivieren. Wenn sie auf der Seite **Domäneneigenschaften** deaktiviert ist, können Sie auf das Symbol **Rechtschreibprüfung aktivieren/deaktivieren** auf der Seite **Domänenwerte** klicken, um sie auf dieser Seite zu aktivieren.  
  
8.  **Neuen Domänenwert hinzufügen**: Klicken Sie hierauf, um am Ende der Tabelle eine Zeile hinzuzufügen. Nachdem Sie einen Wert eingegeben haben, wird die Zeile in alphabetischer Reihenfolge neu angeordnet und durch ein vorangestelltes Sternsymbol als neuer Eintrag gekennzeichnet.  
  
9. **Domänenwerte aus Excel importieren**: Um neue Werte aus einer Excel-Tabellenkalkulation hinzuzufügen, klicken Sie auf den Abwärtspfeil für das Symbol **Werte importieren** , und wählen Sie dann **Domänenwerte aus Excel importieren**aus. Geben Sie den Dateinamen ein, wählen Sie **Erste Zeile als Header verwenden** aus, sofern zutreffend, und klicken Sie dann auf **OK**. Weitere Informationen finden Sie unter [Importieren von Werten aus einer Excel-Datei in eine Domäne](../data-quality-services/import-values-from-an-excel-file-into-a-domain.md).  
  
10. **Projektwerte importieren**: Um neue Werte aus einem Data Quality-Projekt hinzuzufügen, klicken Sie auf den Abwärtspfeil für das Symbol **Werte importieren** , und wählen Sie dann **Projektwerte importieren**aus. Geben Sie den Dateinamen ein, wählen Sie **Erste Zeile als Header verwenden** aus, sofern zutreffend, und klicken Sie dann auf **OK**. Wählen Sie das Projekt aus, aus dem Sie Werte importieren möchten, und klicken Sie auf **OK**. Die importierten Werte werden angezeigt. Klicken Sie auf **Fertig stellen**. Weitere Informationen finden Sie unter „Importieren von Projektwerten in eine Domäne“.  
  
11. **Ausgewählte Domänenwerte löschen**: Um zwei oder mehr vorhandene Werte aus einer Domäne zu löschen, wählen Sie diese in der Werttabelle aus, und klicken Sie dann auf das Symbol **Ausgewählte Domänenwerte löschen** . Ein Eintrag von DQS_NULL kann nicht gelöscht werden. Wenn Sie mehrere zu löschende Werte auswählen und einer davon DQS_NULL ist, schlägt der Vorgang daher fehl.  
  
12. Klicken Sie auf **Fertig stellen** , um die Domänenverwaltungsaktivität abzuschließen, wie in [Beenden der Domänenverwaltungsaktivität](/previous-versions/sql/sql-server-2016/hh510411(v=sql.130))beschrieben.  
  
##  <a name="follow-up-after-changing-domain-values"></a><a name="FollowUp"></a> Nachverfolgung: Nach dem Ändern von Domänenwerten  
 Nachdem Sie Domänenwerte geändert haben, können Sie andere Domänenverwaltungstasks in der Domäne ausführen, Sie können die Wissensermittlung durchführen, um der Domäne Wissen hinzuzufügen, oder Sie können der Domäne eine Abgleichsrichtlinie hinzufügen. Weitere Informationen finden Sie unter [Durchführen der Wissensermittlung](../data-quality-services/perform-knowledge-discovery.md), [Verwalten einer Domäne](../data-quality-services/managing-a-domain.md) oder [Erstellen einer Abgleichsrichtlinie](../data-quality-services/create-a-matching-policy.md).  
  
##  <a name="the-meaning-of-correct-error-and-invalid-values"></a><a name="Meaning"></a> Die Bedeutung von richtigen, fehlerhaften und ungültigen Werten  
 Jedem Wert in der Tabelle **Wert** der Seite **Domänenwerte** wird eine Einstellung für **Typ** von **Richtig**, **Fehler**oder **Ungültig**zugewiesen. Der Typ des Werts wird anfänglich von der Wissensermittlungsaktivität generiert, Sie können diesen jedoch nach Bedarf ändern. Der abschließende sowohl auf der Ermittlung als auch auf interaktiven Änderungen basierende Typ wird durch die Bereinigungsaktivität generiert. Diese Einstellungen haben die folgenden Bedeutungen:  
  
-   **Richtig:** Dieser Wert gehört zur Domäne und weist keine Syntaxfehler auf. Beispiel: „Chicago“ ist in einer Ortsdomäne richtig.  
  
-   **Fehler:** Dieser Wert gehört zur Domäne, ist aber fehlerhaft. Beispiel: „Shicago“ statt „Chicago“ ist in einer Ortsdomäne fehlerhaft. DQS kennzeichnet einen Wert als fehlerhaft, wenn ein Syntaxfehler und eine zugehörige Korrektur im Ermittlungsprozess erkannt wird. Syntaxfehler schließen auch orthographische Fehler ein.  
  
-   **Ungültig:** Dieser Wert gehört nicht zur Domäne, und es ist keine Korrektur vorhanden. Beispiel: Der Wert „12345“ in einer Ortsdomäne ist ungültig. DQS kennzeichnet einen Wert als ungültig, wenn er eine Domänenregel nicht erfüllt.  
  
 Sie können den Typ eines Werts manuell in einen der beiden anderen Werte ändern. DQS erzwingt bei manuellen Vorgängen keine Gültigkeits- und Fehlersemantik. Sie können eine Korrektur für einen ungültigen Wert eingeben, ohne den Status zu ändern. Sie können einen Wert als ungültig festlegen, auch wenn er keiner Domänenregel widerspricht. Sie können einen Wert als fehlerhaft festlegen, auch wenn der Ermittlungsprozess nicht angegeben hat, dass er einen Syntaxfehler aufweist. Sie können außerdem eine Korrektur eines fehlerhaften Werts entfernen, der als richtig markiert wurde, ohne den Status zu ändern.  
  
 Wenn Sie eine interaktive Datenbereinigung auf der Seite **Ergebnisse verwalten und anzeigen** der Aktivität **Bereinigung** ausführen, werden sowohl ungültige als auf fehlerhafte Werte auf der Registerkarte **Ungültig** der Seite **Ergebnisse verwalten und anzeigen** angezeigt.  
  
##  <a name="how-to-display-the-appropriate-values"></a><a name="Display"></a> So zeigen Sie die entsprechenden Werte an  
 Sie können die Anzeige wie folgt ändern:  
  
-   **Filtern** Sie die Ergebnisse, die in der Tabelle angezeigt werden sollen, basierend auf deren Status, indem Sie in der Dropdownliste **Filter** den Status auswählen.  
  
-   **Suchen** Sie die Daten, die Sie prüfen oder ändern möchten, indem Sie einen oder mehrere zu suchende Buchstaben in das Textfeld **Suchen** eingeben. Hierdurch werden die Buchstaben hervorgehoben, wenn diese in einem beliebigen angezeigten Wert vorkommen.  
  
-   Klicken Sie aus **Nur neue anzeigen** , um die in der Tabelle angezeigten Werte auf die Werte zu beschränken, die in der aktuellen Sitzung ermittelt wurden, und um vorherige Werte auszuschließen.  
  
-   Klicken Sie die Schaltfläche **Alle erweitern** , um alle Werte in einer beliebigen Gruppe von Synonymen anzuzeigen, wenn der aktuelle Status reduziert ist.  
  
-   Klicken Sie die Schaltfläche **Alle reduzieren** , um alle Werte mit Ausnahme des führenden Werts in einer beliebigen Gruppe von Synonymen auszublenden, wenn der aktuelle Status erweitert ist.  
  
-   Klicken Sie auf die Schaltfläche **Bereich mit dem Verlauf der Domänenwertänderungen anzeigen/ausblenden** , um unterhalb der Werttabelle den Vorschaubereich einzublenden, in dem die letzten Änderungen an der Domänenwertsammlung angezeigt werden.  
  
##  <a name="how-to-handle-null-equivalents"></a><a name="Null"></a> So verarbeiten Sie NULL-Entsprechungen  
 Jede Werttabelle auf der Registerkarte **Domänenwerte** enthält einen DQS_NULL-Wert. Ein NULL-Wert in einer Datenquelle wird in der Werttabelle als SQL_NULL angezeigt. Sie können eine oder mehrere NULL-Entsprechungen als Synonyme für DQS_NULL festlegen. In diesem fall werden alle NULL-Werte und NULL-Entsprechungen als DQS_NULL verarbeitet.  
  
