---
title: Verwalten von Lesezeichen
description: Das Lesezeichenfenster in einem Code-Editor ermöglicht es Ihnen, Links zu Stellen im Code zu erstellen. In diesem Artikel erfahren Sie, wie Sie Lesezeichen erstellen, löschen, aktivieren und deaktivieren und wie Sie mit ihnen durch Ihren Code navigieren können.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- bookmarks [SQL Server Management Studio]
ms.assetid: 67cc3fd6-3238-4c58-a3ec-2d3b0438143a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3828647bddd0755a5f537fe0158d07a67d3c13d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271727"
---
# <a name="manage-bookmarks"></a>Verwalten von Lesezeichen

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Während der Arbeit mit einem Code-Editor können Sie mithilfe des Fensters **Lesezeichen** Links zu bestimmten Codezeilen innerhalb des Dokuments erstellen. Sie können dieses Fenster über das Menü **Ansicht** anzeigen.  
  
 Zum Erstellen von Lesezeichen und für die Navigation innerhalb der Lesezeichen klicken Sie auf die auf der Symbolleiste **Text-Editor** und die oben im Fenster **Lesezeichen** vorhandenen Schaltflächen. Sie können Lesezeichen hinzufügen und entfernen, aktivieren oder deaktivieren und in Ordnern organisieren. Bestimmte Befehle stehen auch über das Kontextmenü im Fenster **Lesezeichen** zur Verfügung. Zum Hinzufügen oder Entfernen eines Lesezeichens platzieren Sie die Einfügemarke im Editor auf der gewünschten Zeile, und klicken Sie dann auf **Lesezeichen ein/aus**. Zum Aktivieren eines Lesezeichens aktivieren Sie im Fenster **Lesezeichen** das zugehörige Kontrollkästchen; Zum Deaktivieren (aber nicht zum Entfernen) eines Lesezeichens deaktivieren Sie das Kontrollkästchen.  
  
## <a name="text-editor-toolbar"></a>Text-Editor (Symbolleiste)  
 Die folgenden Schaltflächen sind auf der Symbolleiste **Text-Editor** aktiviert, wenn im Editor ein Textdokument geöffnet ist. Um die Symbolleiste **Text-Editor** anzuzeigen, während Sie im Abfrage-Editor arbeiten, zeigen Sie im Menü **Ansicht** auf **Symbolleisten**, und klicken Sie dann auf **Text-Editor**.  
  
 **Lesezeichen für die aktuelle Zeile umschalten**  
 Fügt auf der ausgewählten Zeile des Dokuments im aktiven Editor ein Lesezeichen hinzu oder entfernt dieses. Die mit einem Lesezeichen versehene Codezeile wird nicht verändert.  
  
 **Einfügemarke zum vorherigen Lesezeichen verschieben**  
 Wählt das vorherige Lesezeichen aus, das im Fenster **Lesezeichen** aktiviert ist. Wenn Sie beim ersten Lesezeichen angelangt sind, wird als nächstes Lesezeichen das letzte angezeigt. Öffnet ggf. die Datei, in der das ausgewählte Lesezeichen im Editor auftritt. Führt im Dokument einen Bildlauf zu der mit dem Lesezeichen versehenen Zeile durch und platziert dort die Einfügemarke.  
  
 **Einfügemarke zum nächsten Lesezeichen verschieben**  
 Wählt das nächste Lesezeichen aus, das im Fenster **Lesezeichen** aktiviert ist. Wenn Sie beim letzten Lesezeichen angelangt sind, wird als nächstes Lesezeichen wieder das erste angezeigt. Öffnet ggf. die Datei, in der das ausgewählte Lesezeichen im Editor auftritt. Führt im Dokument einen Bildlauf zu der mit dem Lesezeichen versehenen Zeile durch und platziert dort die Einfügemarke.  
  
 **Alle Lesezeichen im aktuellen Dokument löschen**  
 Zeigt eine Bestätigungsmeldung an und entfernt dann alle Lesezeichen aus dem aktiven Dokument. Die Codezeilen, die mit einem Lesezeichen versehen waren, werden dabei nicht entfernt.  
  
> [!CAUTION]  
>  Dieser Vorgang lässt sich nicht rückgängig machen. Sie müssen anschließend mithilfe der Option **Lesezeichen für die aktuelle Zeile umschalten** neue Lesezeichen erstellen. Um Lesezeichen zu deaktivieren, ohne sie zu entfernen, deaktivieren Sie die zugehörigen Kontrollkästchen im Fenster **Lesezeichen** .  
  
## <a name="bookmarks-window"></a>Lesezeichen (Fenster)  
 Zum Organisieren von Lesezeichen erstellen Sie im Fenster **Lesezeichen** Lesezeichenordner. Ziehen Sie die Lesezeichen per Drag und Drop in die gewünschten Ordner. Die folgenden Schaltflächen stehen oben im Fenster **Lesezeichen** zur Verfügung.  
  
 **Lesezeichen für die aktuelle Zeile umschalten.**  
 Fügt auf der ausgewählten Zeile des Dokuments im aktiven Editor ein Lesezeichen hinzu oder entfernt dieses. Die mit einem Lesezeichen versehene Codezeile wird nicht verändert.  
  
 **Neuer Ordner**  
 Fügt im Fenster **Lesezeichen** einen neuen Ordner hinzu.  
  
> [!TIP]  
>  In einer längeren Codedatei kann es hilfreich sein, Lesezeichen in taskbezogenen Ordnern zu organisieren. Durch Auswahl eines Ordners werden die Schaltflächen **Vorheriges Lesezeichen im Ordner** und **Nächstes Lesezeichen im Ordner** aktiviert.  
  
 **Einfügemarke zum vorherigen Lesezeichen verschieben**  
 Wählt das vorherige Lesezeichen aus, das im Fenster **Lesezeichen** aktiviert ist. Wenn Sie beim ersten Lesezeichen angelangt sind, wird als nächstes Lesezeichen das letzte angezeigt. Öffnet ggf. die Datei, in der das ausgewählte Lesezeichen im Editor auftritt. Führt im Dokument einen Bildlauf zu der mit dem Lesezeichen versehenen Zeile durch und platziert dort die Einfügemarke.  
  
 **Einfügemarke zum nächsten Lesezeichen verschieben**  
 Wählt das nächste Lesezeichen aus, das im Fenster **Lesezeichen** aktiviert ist. Wenn Sie beim letzten Lesezeichen angelangt sind, wird als nächstes Lesezeichen wieder das erste angezeigt. Öffnet ggf. die Datei, in der das ausgewählte Lesezeichen im Editor auftritt. Führt im Dokument einen Bildlauf zu der mit dem Lesezeichen versehenen Zeile durch und platziert dort die Einfügemarke.  
  
 **Einfügemarke zum vorherigen Lesezeichen im aktuellen Ordner verschieben**  
 Wählt das vorherige Lesezeichen aus, das im selben Ordner im Fenster **Lesezeichen** aktiviert ist. Wenn Sie beim ersten Lesezeichen angelangt sind, wird als nächstes das letzte Lesezeichen im Ordner angezeigt. Öffnet ggf. die Datei, in der das ausgewählte Lesezeichen im Editor auftritt. Führt im Dokument einen Bildlauf zu der mit dem Lesezeichen versehenen Zeile durch und platziert dort die Einfügemarke.  
  
 **Einfügemarke zum nächsten Lesezeichen im aktuellen Ordner verschieben**  
 Wählt das nächste Lesezeichen aus, das im selben Ordner im Fenster **Lesezeichen** aktiviert ist. Wenn Sie beim letzten Lesezeichen angelangt sind, wird als nächstes das erste Lesezeichen im Ordner angezeigt. Öffnet ggf. die Datei, in der das ausgewählte Lesezeichen im Editor auftritt. Führt im Dokument einen Bildlauf zu der mit dem Lesezeichen versehenen Zeile durch und platziert dort die Einfügemarke.  
  
 **Alle Lesezeichen deaktivieren/aktivieren**  
 Deaktiviert oder aktiviert die Kontrollkästchen für alle Lesezeichen im Fenster **Lesezeichen** . Die Lesezeichen werden nicht entfernt und die mit ihnen gekennzeichneten Codezeilen nicht geändert.  
  
 **Löschen**  
 Entfernt das aktuell ausgewählte Lesezeichen aus dem Fenster **Lesezeichen** sowie aus dem Dokument, in dem es aufgetreten ist. Die Codezeile, die mit dem Lesezeichen versehen war, wird dabei nicht entfernt.  
  
 Kontrollkästchen für Lesezeichen  
 Jedes Lesezeichen besitzt ein eigenes Kontrollkästchen. Zum Aktivieren eines vorhandenen Lesezeichens aktivieren Sie im Fenster **Lesezeichen** das entsprechende Kontrollkästchen. Um ein vorhandenes Lesezeichen auszublenden (jedoch nicht zu entfernen), deaktivieren Sie im Fenster **Lesezeichen** das entsprechende Kontrollkästchen.  
  
## <a name="bookmarks-window-shortcut-menu"></a>Lesezeichen-Fenster (Kontextmenü)  
 Wenn Sie auf einen Eintrag im Fenster **Lesezeichen** mit der rechten Maustaste klicken, stehen über das Kontextmenü die folgenden Befehle zur Verfügung.  
  
 **Löschen**  
 Entfernt das aktuell ausgewählte Lesezeichen aus dem Fenster **Lesezeichen** sowie aus dem Dokument, in dem es aufgetreten ist. Die Codezeile, die mit dem Lesezeichen versehen war, wird dabei nicht entfernt.  
  
 **Umbenennen**  
 Ermöglicht Ihnen, einem Lesezeichen oder Ordner einen neuen Anzeigenamen zuzuweisen.  
  
 **Lesezeichen deaktivieren/aktivieren**  
 Deaktiviert oder aktiviert das Kontrollkästchen für das ausgewählte Lesezeichen im Fenster **Lesezeichen** . Das Lesezeichen wird nicht entfernt und die mit ihm gekennzeichnete Codezeile nicht geändert.  
  
 **Alle Lesezeichen deaktivieren/aktivieren**  
 Deaktiviert oder aktiviert die Kontrollkästchen für alle Lesezeichen im Fenster **Lesezeichen** . Die Lesezeichen werden nicht entfernt und die mit ihnen gekennzeichneten Codezeilen nicht geändert.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Tastenkombinationen für SQL Server Management Studio](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)  
