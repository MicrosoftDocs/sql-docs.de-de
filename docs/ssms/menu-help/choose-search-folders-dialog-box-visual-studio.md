---
description: Suchordner auswählen (Dialogfeld in Visual Studio)
title: Suchordner auswählen (Dialogfeld in Visual Studio)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 2eaba888-68b2-4bc1-8f62-e96e710c3db9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 69c70680431019638dff53664393ce2256a5f01b
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88491999"
---
# <a name="choose-search-folders-dialog-box-visual-studio"></a>Suchordner auswählen (Dialogfeld in Visual Studio)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Ermöglicht es Ihnen, eigene benannte Sätze mit Suchordnern zusammenzustellen, zu speichern und zu überarbeiten sowie den Ordner anzugeben, in dem sie durchsucht werden. Zum Anzeigen dieses Dialogfelds klicken Sie im Fenster In Dateien suchen, In Dateien ersetzen oder Suchen und Ersetzen neben der Dropdownliste **Suchen in** auf die Schaltfläche **Durchsuchen (...)** .  
  
Fügen Sie der Liste **Ausgewählte Ordner** Ordner hinzu, geben Sie im Feld **Ordnersatz** einen Namen für diesen Ordner ein, und klicken Sie auf **Übernehmen** , um ihn zu speichern. Dieser benutzerdefinierte Suchbereich kann danach anhand des Namens aus den Dropdownlisten **Suchen in** in den Fenstern **Suchen in Dateien** und **Ersetzen in Dateien**ausgewählt werden. Wenn Sie einen benutzerdefinierten Ordnersatz aus den Listen **Suchen in** entfernen möchten, wählen Sie seinen Namen im Feld **Ordnersatz** aus, und klicken Sie auf **Löschen**.  
  
## <a name="options"></a>Optionen  
Mithilfe der folgenden Steuerelemente können Sie eigene Sätze mit Suchordnern zusammenstellen, speichern und überarbeiten.  
  
**Ordnersatz**  
Führt die für die Suche verfügbaren Verzeichnisse auf. Zum Erstellen eines neuen Ordnersatzes geben Sie dessen Namen ein, fügen Sie der Liste **Ausgewählte Ordner** einen Satz mit Suchordnern hinzu, und klicken Sie dann auf **Übernehmen**.  
  
**Anwenden**  
Speichert den Satz mit Suchordnern, die in der Liste **Ausgewählte Ordner** aufgeführt sind, als einen benannten Ordnersatz. Dieser Ordnersatz kann dann im Feld **Suchen in** aller Registerkarten im Fenster **Suchen und Ersetzen** ausgewählt werden. Das Dialogfeld Suchordner auswählen bleibt offen.  
  
**Löschen**  
Entfernt den ausgewählten Ordnersatz aus dem Feld **Ordnersatz** und dem Feld **Suchen in** auf allen Registerkarten des Fensters **Suchen und Ersetzen** .  
  
**Verfügbare Ordner**  
Wählen Sie in dieser Dropdownliste ein Laufwerk oder einen Ordner aus, um die **Ordnerliste**aufzufüllen.  
  
**Ordnerliste**  
Führt die auf dem ausgewählten Volume verfügbaren Laufwerke und Ordner in der Dropdownliste **Verfügbare Ordner** auf. Doppelklicken Sie, um eines der Laufwerke bzw. einen der Ordner in der Liste zu erweitern. Wählen Sie einen Ordner aus, oder halten Sie die UMSCHALT- bzw. die STRG-Taste gedrückt, um mehrere Ordner auszuwählen. Klicken Sie auf **Hinzufügen (>)** , um die ausgewählten Ordner in die Liste **Ausgewählte Ordner** aufzunehmen.  
  
**Parent**  
Verschiebt die Auswahl in der **Ordnerliste** um eine Ebene in der Ordnerhierarchie nach oben.  
  
**Hinzufügen (>)**  
Fügt der Liste **Ausgewählte Ordner** die in der **Ordnerliste** ausgewählten Ordner hinzu.  
  
**Entfernen (<)**  
Entfernt die ausgewählten Ordner aus der Liste **Ausgewählte Ordner** .  
  
**Ausgewählte Ordner**  
Führt die hinzugefügten Ordner aus der **Ordnerliste**auf. Diese Ordner werden in den benannten **Ordnersatz**eingeschlossen.  
  
**Anwenden**  
Speichert den Satz mit Suchordnern, die in der Liste **Ausgewählte Ordner** aufgeführt sind, als einen benannten Ordnersatz. Dieser Ordnersatz kann dann im Feld **Suchen in** aller Registerkarten im Fenster **Suchen und Ersetzen** ausgewählt werden. Schließt das Dialogfeld Suchordner auswählen.  
