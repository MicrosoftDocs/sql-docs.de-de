---
description: Projekteinstellungen (Synchronisierung) (DB2ToSQL)
title: Projekteinstellungen (Synchronisierung) (DB2ToSQL) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: a5629a72-8c17-46a4-bb4d-19d51a0b98a2
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 4d96e3e3caeb1c40a9f4529f8d2c0ea7e0b1f717
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072124"
---
# <a name="project-settingssynchronization-db2tosql"></a>Projekteinstellungen (Synchronisierung) (DB2ToSQL)
Die Seite Synchronisierung des Dialog Felds **Projekteinstellungen** enthält Einstellungen, mit denen Sie anpassen können, wie SSMA Datenbankobjekte (z. b. Tabellen und gespeicherte Prozeduren) in lädt und aktualisiert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Mit den Standard Aktions Optionen werden die Standardeinstellungen für die Aktualisierung von Objekten aus der DB2-Datenbank und für die Synchronisierung von Objekten mit der SQL Server Datenbank angegeben. Weitere Informationen finden Sie unter [Aktualisieren von Datenbanken &#40;DB2ToSQL&#41;](../../ssma/db2/refresh-from-database-db2tosql.md).  
  
Sie können auf zwei verschiedene Synchronisierungs Seiten zugreifen, die die gleichen Einstellungen enthalten:  
  
-   Wenn Sie Einstellungen für alle zukünftigen SSMA-Projekte angeben möchten, klicken Sie **im Menü Extras** auf **Standard Projekteinstellungen**, und klicken Sie dann unten im linken Bereich auf **Synchronisierung** .  
  
-   Um Einstellungen für das aktuelle Projekt anzugeben, klicken Sie **im Menü Extras** auf **Projekteinstellungen**, und klicken Sie dann unten im linken Bereich auf **Synchronisierung** .  
  
## <a name="miscellaneous-options"></a>Sonstige Optionen  
**Versuche**  
Gibt die Anzahl der Versuche an, die SSMA beim Laden von Objekten in durchführen soll [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Objekte, die im aktuellen Versuch nicht in geladen werden, werden wiederholt, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bis SSMA die maximale Anzahl von versuchen im aktuellen Synchronisierungs Vorgang erreicht. Der Standardwert ist " **2** ".  
  
## <a name="synchronization-for-db2-options"></a>Synchronisierung für DB2-Optionen  
**Aktion bei lokaler und Remote Objekt Änderung**  
Gibt die Standardeinstellung im Dialogfeld "Synchronisierung" an, wenn die Objektdefinition in SSMA und auf dem Datenbankserver geändert wird. Der Standardwert ist " **Aktualisieren" aus der Datenbank**.  
  
-   Wenn Sie **aus Datenbank aktualisieren** auswählen, lädt SSMA Daten Bank Definitionen in die Metadaten, wenn die Bedingung erfüllt ist.  
  
-   Wenn Sie **Skip** auswählen, führt SSMA keine Aktualisierungs Aktionen aus.  
  
**Aktion bei lokaler Objekt Änderung**  
Gibt die Standardeinstellung im Dialogfeld "Synchronisierung" an, wenn das Objekt in SSMA geändert wird. Der Standardwert ist " **Skip**".  
  
-   Wenn Sie **aus Datenbank aktualisieren** auswählen, lädt SSMA Daten Bank Definitionen in die Metadaten, wenn die Bedingung erfüllt ist.  
  
-   Wenn Sie **Skip** auswählen, führt SSMA keine Aktualisierungs Aktionen aus.  
  
**Aktion bei Remote Objekt Änderung**  
Gibt die Standardeinstellung im Dialogfeld "Synchronisierung" an, wenn sich die Objekte auf dem Datenbankserver ändern. Der Standardwert ist " **Aktualisieren" aus der Datenbank**.  
  
-   Wenn Sie **aus Datenbank aktualisieren** auswählen, lädt SSMA Daten Bank Definitionen in die Metadaten, wenn die Bedingung erfüllt ist.  
  
-   Wenn Sie **Skip** auswählen, führt SSMA keine Aktualisierungs Aktionen aus.  
  
**Aktion, wenn lokale Objekt Metadaten fehlen**  
Gibt die Standardeinstellung im Dialogfeld "Synchronisierung" an, wenn lokale Metadaten fehlen. Der Standardwert ist " **Aktualisieren" aus der Datenbank**.  
  
-   Wenn Sie **aus Datenbank aktualisieren** auswählen, lädt SSMA Daten Bank Definitionen in die Metadaten, wenn die Bedingung erfüllt ist.  
  
-   Wenn Sie **Skip** auswählen, führt SSMA keine Aktualisierungs Aktionen aus.  
  
## <a name="synchronization-for-sql-server-options"></a>Synchronisierung für SQL Server Optionen  
**Aktion bei lokaler und Remote Objekt Änderung**  
Gibt die Standardeinstellung im Dialogfeld "Synchronisierung" an, wenn die Objektdefinition in SSMA und auf dem Datenbankserver geändert wird. Der Standardwert ist " **in Datenbank schreiben**".  
  
-   Wenn Sie **aus Datenbank aktualisieren** auswählen, lädt SSMA Daten Bank Definitionen in die Metadaten, wenn die Bedingung erfüllt ist.  
  
-   Wenn Sie in **Datenbank schreiben** auswählen, werden die Objekte in der Datenbank von SSMA gemäß dem Inhalt der SSMA-Metadaten aktualisiert, wenn die Bedingung erfüllt ist.  
  
-   Wenn Sie **Skip** auswählen, führt SSMA keine Aktualisierungs Aktionen aus.  
  
**Aktion bei lokaler Objekt Änderung**  
Gibt die Standardeinstellung im Dialogfeld "Synchronisierung" an, wenn das Objekt in SSMA geändert wird. Der Standardwert ist " **in Datenbank schreiben**".  
  
-   Wenn Sie **aus Datenbank aktualisieren** auswählen, lädt SSMA Daten Bank Definitionen in die Metadaten, wenn die Bedingung erfüllt ist.  
  
-   Wenn Sie in **Datenbank schreiben** auswählen, wird das Objekt in der Datenbank von SSMA gemäß dem Inhalt der SSMA-Metadaten aktualisiert, wenn die Bedingung erfüllt ist.  
  
-   Wenn Sie **Skip** auswählen, führt SSMA keine Aktualisierungs Aktionen aus.  
  
**Aktion bei Remote Objekt Änderung**  
Gibt die Standardeinstellung im Dialogfeld "Synchronisierung" an, wenn sich die Objekte auf dem Datenbankserver ändern.  Der Standardwert ist " **Aktualisieren" aus der Datenbank**.  
  
-   Wenn Sie **aus Datenbank aktualisieren** auswählen, lädt SSMA Daten Bank Definitionen in die Metadaten, wenn die Bedingung erfüllt ist.  
  
-   Wenn Sie in **Datenbank schreiben** auswählen, wird das Objekt in der Datenbank von SSMA gemäß dem Inhalt der SSMA-Metadaten aktualisiert, wenn die Bedingung erfüllt ist.  
  
-   Wenn Sie **Skip** auswählen, führt SSMA keine Aktualisierungs Aktionen aus.  
  
**Aktion, wenn lokale Objekt Metadaten fehlen**  
Gibt die Standardeinstellung im Dialogfeld "Synchronisierung" an, wenn lokale Metadaten fehlen. Der Standardwert ist " **Aktualisieren" aus der Datenbank**.  
  
-   Wenn Sie **aus Datenbank aktualisieren** auswählen, wählt SSMA die Option **aus Datenbank aktualisieren aus** , wenn die Bedingung erfüllt ist.  
  
-   Wenn Sie **in Datenbank schreiben** auswählen, wird das Objekt von SSMA aus der Datenbank gelöscht, wenn die Bedingung erfüllt ist.  
  
-   Wenn Sie **Skip** auswählen, führt SSMA keine Aktualisierungs Aktionen aus.  
  
