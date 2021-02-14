---
description: Globale Einstellungen (Protokollierung) (SybaseToSQL)
title: Globale Einstellungen (Protokollierung) (sybasedesql) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 4cb4da20-3b99-4aae-8c80-329ee23e796e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 90b331e197bef4958e3df6242a71e589c95b2f7f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078811"
---
# <a name="global-settings-logging-sybasetosql"></a>Globale Einstellungen (Protokollierung) (SybaseToSQL)
Verwenden Sie das Dialogfeld **globale Einstellungen** , um die Protokollierungs Einstellungen für SSMA anzugeben. In der Regel ändern Sie diese Einstellungen nur, wenn Sie mit dem Produktsupport arbeiten.  
  
Um auf dieses Dialogfeld zuzugreifen, wählen Sie **im Menü Extras die Option** **globale Einstellungen** aus, und klicken Sie dann unten im linken Bereich auf die Schaltfläche **Protokollierung** .  
  
## <a name="options"></a>Tastatur  
**Nachrichten Ebene**  
Die folgenden Optionen sind unter **Nachrichten Ebene** verfügbar:  
  
|Option|BESCHREIBUNG|  
|----------|---------------|  
|**[alle Kategorien]**|Wird verwendet, um den Protokolliergrad für alle folgenden Optionen festzulegen.|  
|**Collector**|Sammelt Metadaten über das Quell Schema und speichert Sie im Projekt.|  
|**Converter**|Konvertiert Strukturen von Quelldaten Bank Objekten, z. b. Tabellen und gespeicherte Prozeduren, in entsprechende [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Strukturen.|  
|**Datenmigrierer**|Migriert Daten von der Quelldatenbank zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**Formatierungsprogramm**|Unterkomponente des Konverters, der Skripts für das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Schema generiert.|  
|**Grafische Benutzeroberfläche**|Meldungen, die angezeigt werden, wenn Sie das SSMA-Tool verwenden.|  
|**Linker**|Löst SQL-Bezeichner auf und stellt Informationen für andere Komponenten bereit.|  
|**Andere**|Alle Nachrichten, die nicht in einer anderen Kategorie vorhanden sind.|  
|**Parser**|Analysiert das Quell Schema.|  
|**Synchronisierung**|Lädt Quelldaten Bank Objekte in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**Treeconverter**|Konvertiert Objekte in den Quell Metadaten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Metadaten.|  
  
Konfigurieren Sie für jede Option unter **Nachrichten Ebene** einen der folgenden Protokolliergrade für SSMA:  
  
|||  
|-|-|  
|**Schwerwiegender Fehler**|Schreiben Sie nur schwerwiegende Fehlermeldungen in das Protokoll.|  
|**Fehler**|Schreiben Sie Fehler-und schwerwiegende Fehlermeldungen in das Protokoll.|  
|**Warnung**|Schreiben Sie Warnungs-, Fehler-und schwerwiegende Fehlermeldungen in das Protokoll.|  
|**Info**|Schreiben Sie Informations-, Warnungs-, Fehler-und schwerwiegende Fehlermeldungen in das Protokoll.|  
|**Debuggen**|Schreiben Sie alle Meldungen, einschließlich des Debuggens von Nachrichten, in das Protokoll.|  
  
**Protokolldatei Pfad**  
Der Dateipfad und der Name der SSMA-Protokolldateien. Wenn Sie einen anderen Namen angeben möchten, klicken Sie auf den aktuellen Pfad, und klicken Sie dann auf die Schaltfläche zum Durchsuchen (**..**.).  
  
**Protokolldatei Größe**  
Die maximale Größe der Protokolldatei in KB. Die Mindestgröße beträgt 10 KB. Die Standardgröße ist 10240 KB.  
  
**Gesamtanzahl der Protokolldateien**  
Wenn ein Protokoll ausgefüllt wird, benennt SSMA die Protokolldatei um und startet ein neues. Geben Sie mit dieser Einstellung die maximale Anzahl von Protokolldateien an, die aufbewahrt werden sollen. Der Minimalwert ist 2.  
  
