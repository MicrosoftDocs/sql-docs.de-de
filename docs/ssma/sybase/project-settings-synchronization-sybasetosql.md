---
description: Projekteinstellungen (Synchronisierung) (SybaseToSQL)
title: Projekteinstellungen (Synchronisierung) (sybasedesql) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 2cd6bc01-b8e5-4312-83a4-eac66dc1d460
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a5db262979e7c7e267491e34b54302c0a5b36999
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070505"
---
# <a name="project-settings-synchronization-sybasetosql"></a>Projekteinstellungen (Synchronisierung) (SybaseToSQL)
Die Seite Synchronisierung des Dialog Felds **Projekteinstellungen** enthält Einstellungen, die anpassen, wie SSMA Datenbankobjekte, z. b. Tabellen und gespeicherte Prozeduren, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder SQL Azure lädt.  
  
Sie können auf zwei verschiedene Synchronisierungs Seiten zugreifen, die die gleichen Einstellungen enthalten:  
  
-   Wenn Sie Einstellungen für alle zukünftigen SSMA-Projekte angeben möchten, wählen Sie **im Menü Extras** die Option **Standard Projekteinstellungen** aus, wählen Sie den Migrations Projekttyp aus, für den die Einstellungen in der Dropdown Liste **Migrations Ziel Version** angezeigt oder geändert werden müssen, und wählen Sie dann unten im linken Bereich die Option **Synchronisierung** aus.  
  
-   Um Einstellungen für das aktuelle Projekt anzugeben, wählen Sie **im Menü Extras** die Option **Projekteinstellungen** aus, und wählen Sie dann unten im linken Bereich die Option **Synchronisierung** aus.  
  
## <a name="options"></a>Tastatur  
**Versuche**  
Gibt die Anzahl der Versuche an, die SSMA beim Laden von Objekten in durchführen soll [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Objekte, die im aktuellen Versuch nicht in geladen werden, werden wiederholt, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bis SSMA die maximale Anzahl von versuchen im aktuellen Synchronisierungs Vorgang erreicht.  
  
## <a name="synchronization-for-sql-server"></a>Synchronisierung für SQL Server  
**Lokales Objekt bei lokaler und Remote Objekt Änderung aktualisieren**  
Gibt an, ob SSMA die Metadaten des lokalen Objekts durch Remote Objekt Metadaten ersetzen soll, wenn sich sowohl das lokale als auch das Remote Objekt ändern.  
Wenn Sie **aus Datenbank aktualisieren** auswählen, lädt SSMA Daten Bank Definitionen in die Metadaten, wenn die Bedingung erfüllt ist.  
Wenn Sie in **Datenbank schreiben** auswählen, werden die Objekte in der Datenbank von SSMA gemäß dem Inhalt der SSMA-Metadaten aktualisiert, wenn die Bedingung erfüllt ist.  
Wenn Sie **Skip** auswählen, führt SSMA keine Aktualisierungs Aktionen aus.   
Der Standard Options Satz ist " **in Datenbank schreiben".**  
  
**Lokales Objekt bei Änderung des lokalen Objekts aktualisieren**  
Gibt an, ob SSMA die Metadaten des lokalen Objekts durch Remote Objekt Metadaten ersetzen soll, wenn sich das Remote Objekt ändert.  
Wenn Sie **aus Datenbank aktualisieren** auswählen, lädt SSMA Daten Bank Definitionen in die Metadaten, wenn die Bedingung erfüllt ist.  
Wenn Sie in **Datenbank schreiben** auswählen, wird das Objekt in der Datenbank von SSMA gemäß dem Inhalt der SSMA-Metadaten aktualisiert, wenn die Bedingung erfüllt ist.  
Wenn Sie **Skip** auswählen, führt SSMA keine Aktualisierungs Aktionen aus.   
Der Standard Options Satz ist " **in Datenbank schreiben**".  
  
**Lokales Objekt bei Remote Objekt Änderung aktualisieren**  
Gibt an, ob SSMA die Metadaten des lokalen Objekts durch Remote Objekt Metadaten ersetzen soll, wenn sich das Remote Objekt ändert.  
Wenn Sie **aus Datenbank aktualisieren** auswählen, lädt SSMA Daten Bank Definitionen in die Metadaten, wenn die Bedingung erfüllt ist.  
Wenn Sie in **Datenbank schreiben** auswählen, wird das Objekt in der Datenbank von SSMA gemäß dem Inhalt der SSMA-Metadaten aktualisiert, wenn die Bedingung erfüllt ist.  
Wenn Sie **Skip** auswählen, führt SSMA keine Aktualisierungs Aktionen aus.   
Der Standard Options Satz ist **Aktualisieren von der Datenbank**.  
  
**Aktualisieren, wenn Metadaten des lokalen Objekts fehlen**  
Gibt an, ob SSMA lokale Metadaten erstellen soll, wenn ein Objekt in der Zieldatenbank vorhanden ist, jedoch nicht lokal.  
Wenn Sie **aus Datenbank aktualisieren** auswählen, wählt SSMA die Option aus Datenbank aktualisieren aus, wenn die Bedingung erfüllt ist.  
Wenn Sie **in Datenbank schreiben** auswählen, wird das Objekt von SSMA aus der Datenbank gelöscht, wenn die Bedingung erfüllt ist.  
Wenn Sie **Skip** auswählen, führt SSMA keine Aktualisierungs Aktionen aus.   
Der Standard Options Satz ist **Aktualisieren von der Datenbank**.  
  
