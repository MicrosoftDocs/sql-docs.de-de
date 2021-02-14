---
description: Projekteinstellungen (Migration) (SybaseToSQL)
title: Projekteinstellungen (Migration) (sybasedesql) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 82f8857f-7ab1-4738-ab6e-b1e95ea94924
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 96b5550449e11cd314e736a36e3845ba92f542c7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069922"
---
# <a name="project-settings-migration-sybasetosql"></a>Projekteinstellungen (Migration) (SybaseToSQL)
Die Seite Migration des Dialog Felds **Projekteinstellungen** enthält Einstellungen, mit denen Sie anpassen können, wie SSMA Daten von Sybase Adaptive Server Enterprise (ASE) zu migriert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Der Bereich Migration ist sowohl in den Dialogfeldern **Projekteinstellungen** als auch **Standard Projekteinstellungen** verfügbar.  
  
-   Wenn Sie Einstellungen für alle SSMA-Projekte angeben möchten, wählen Sie **im Menü Extras** die Option **Standard Projekteinstellungen** aus, wählen Sie den Migrations Projekttyp aus, für den Einstellungen angezeigt werden sollen/Changed klicken Sie in der Dropdown Liste **Migrations Ziel Version** unten im linken Bereich auf **Allgemein** , und klicken Sie dann auf **Migration**.  
  
-   Um Einstellungen für das aktuelle Projekt anzugeben, wählen Sie **im Menü Extras** die **Option Projekteinstellungen** aus, klicken Sie unten im linken Bereich auf **Allgemein** , und klicken Sie dann auf **Migration**.  
  
## <a name="date-correction-options"></a>Datums Korrektur Optionen  
  
|Begriff|Definition|  
|--------|--------------|  
|**Nicht unterstützte Datumsangaben ersetzen**|Gibt an, ob SSMA Datumsangaben vor dem frühesten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **DateTime** -Datum (01. Januar 1753) korrigieren soll.<br /><br />Um die aktuellen Datumswerte beizubehalten, wählen Sie keine Aktion durch **führen** aus. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] akzeptiert keine Datumsangaben vor 01. Januar 1753 in einer datetime-Spalte. Wenn Sie ältere Datumsangaben verwenden, müssen Sie die DateTime-Werte in Zeichen Werte konvertieren.<br /><br />Um Datumsangaben vor 01. Januar 1753 in NULL zu konvertieren, wählen Sie **durch Null Ersetzen** aus.<br /><br />Um Datumsangaben vor 01. Januar 1753 durch ein unterstütztes Datum zu ersetzen, wählen Sie **durch das nächste unterstützte Datum ersetzen** aus.<br /><br />**Standardmodus**: nichts tun<br /><br />**Optimistischer Modus**: nichts tun<br /><br />**Vollständiger Modus**: durch das nächste unterstützte Datum ersetzen|  
  
## <a name="migration-engine"></a>Migrations-Engine  
  
|Begriff|Definition|  
|--------|--------------|  
|**Migrations-Engine**|Gibt die während der Daten Migration verwendete Datenbank-Engine an. Bei der Client seitigen Datenmigration wird der SSMA-Client zum Abrufen der Daten aus der Quelle und zum Massen Einfügen dieser Daten in SQL Server bezeichnet. Die Server seitige Datenmigration bezieht sich auf die SSMA-Datenmigrations-Engine (Massen Kopier Programm), die im SQL Server Feld ausgeführt wird, als SQL-Agentauftrag, der Daten aus der Quelle abruft und direkt in SQL Server einfügt, wodurch ein zusätzlicher Client-Hop (bessere Leistung) vermieden wird.<br /><br />**Standardmodus**: Client seitiges Datenmigrations-Engine<br /><br />**Optimistischer Modus**: Client seitiges Datenmigrations-Engine<br /><br />**Vollständiger Modus**: Client seitiges Datenmigrations-Engine|  
  
> [!IMPORTANT]  
> Wenn die Option **Migrations-Engine** auf **serverseitiges Datenmigrations-Engine** festgelegt ist, wird eine neue Projekt Einstellungs Option **Use 32-Bit Server seitiges Data Migration Engine** angezeigt. Hiermit wird angegeben, ob zum Migrieren von Daten das 32-Bit-oder 64-Bit-Hilfsprogramm für das Massen Kopier Programm verwendet wird.  
  
## <a name="miscellaneous-options"></a>Sonstige Optionen  
  
|Begriff|Definition|  
|--------|--------------|  
|**Batch Size**|Gibt die Batch Größe an, die während der Datenmigration verwendet wird.<br /><br />**Standardmodus**: 10000<br /><br />**Optimistischer Modus**: 10000<br /><br />**Vollständiger Modus**: 10000|  
|**Check-Einschränkungen**|Gibt an, ob SSMA Einschränkungen überprüfen soll, wenn Daten in SQL Server Tabellen eingefügt werden.<br /><br />**Standardmodus**: false<br /><br />**Optimistischer Modus**: false<br /><br />**Vollständiger Modus**: false|  
|**Timeout bei der Daten Migration**|Gibt das während der Datenmigration verwendete Timeout an.<br /><br />**Standardmodus**: 15<br /><br />**Optimistischer Modus**: 15<br /><br />**Vollständiger Modus**: 15|  
|**Erweiterte Daten Migrations Optionen**|Zeigt zusätzliche Daten Migrations Optionen für jede Tabelle auf einer separaten Detail Registerkarte an.<br /><br />**Standardmodus**: Ausblenden<br /><br />**Optimistischer Modus**: Ausblenden<br /><br />**Vollmodus**: Ausblenden|  
|**Trigger auslösen**|Gibt an, ob SSMA Einfügetrigger auslösen soll, wenn Daten SQL Server Tabellen hinzugefügt werden.<br /><br />**Standardmodus**: false<br /><br />**Optimistischer Modus**: false<br /><br />**Vollständiger Modus**: false|  
|**Identität beibehalten**|Gibt an, ob SSMA Sybase-Identitäts Werte beibehält, wenn Daten SQL Server hinzugefügt werden. Der Wert false bewirkt, dass Identitäts Werte vom Ziel zugewiesen werden.<br /><br />**Standardmodus**: true<br /><br />**Optimistischer Modus**: true<br /><br />**Vollständiger Modus**: true|  
|**NULL-Werte beibehalten**|Gibt an, ob SSMA beim Hinzufügen von Daten zu SQL Server NULL-Werte in den Quelldaten beibehält, unabhängig von den in SQL Server angegebenen Standardwerten.<br /><br />**Standardmodus**: true<br /><br />**Optimistischer Modus**: true<br /><br />**Vollständiger Modus**: true|  
|**On Error**|Beendet die Datenmigration, wenn ein Fehler auftritt. Es gibt drei Optionen:<br /><br />**Migration Abbrechen:** Beendet den Daten Migrations Vorgang.<br /><br />**Weiter zur nächsten Tabelle:** Beendet die Datenmigration zur aktuellen Tabelle und geht zum nächsten<br /><br />**Zum nächsten Batch wechseln:** Beendet die Datenmigration zum aktuellen Batch und geht zum nächsten<br /><br />**Standardmodus**: fahren Sie mit dem nächsten Batch fort<br /><br />**Optimistischer Modus**: mit dem nächsten Batch fortfahren<br /><br />**Vollständiger Modus**: mit dem nächsten Batch fortfahren|  
|**Roundbruchteile von Zahlen**|Gibt an, ob die Bruchteile von dezimalen und numerischen Daten während der Migration zu ganzzahligen Typen abgeschnitten werden sollen oder ob eine Fehlermeldung angezeigt werden soll, wenn ein Bruchteil nicht trivial ist.<br /><br />**Standardmodus**: Nein<br /><br />**Optimistischer Modus**: Nein<br /><br />**Vollmodus**: Nein|  
|**Sybase-Unicode--Klasse**|Gibt den Endian-Typ für die Sybase-Unicode-Zeichen folgen an. Die folgenden Optionen können für diese spezielle Einstellung festgelegt werden:<br /><br />Little-d<br /><br />Big-tedian<br /><br />**Standardmodus**: Little-Endian<br /><br />**Optimistischer Modus**: Little-Endian<br /><br />**Vollständiger Modus**: Little-Endian|  
|**Tabellensperre**|Gibt an, ob SSMA Tabellen sperrt, wenn während der Datenmigration Daten zu Tabellen hinzugefügt werden. Erhält eine Massen Aktualisierungs Sperre für die Dauer des Massen Kopiervorgangs. Wenn der Wert false ist, wird auf Zeilenebene eine Sperre festgelegt.<br /><br />**Standardmodus**: true<br /><br />**Optimistischer Modus**: true<br /><br />**Vollständiger Modus**: true|  
|**Cursor verwenden**|Wenn diese Option festgelegt ist, werden die Daten mithilfe von Cursorn aus der Quelldatenbank abgerufen.<br /><br />**Standardmodus**: false<br /><br />**Optimistischer Modus**: false<br /><br />**Vollständiger Modus**: false|  
  
## <a name="parallel-data-migration"></a>Parallele Daten Migration  
  
|Begriff|Definition|  
|--------|--------------|  
|**Paralleler Daten Migrations Modus**|Gibt den Modus an, der zum Verzweigen von Threads verwendet wird, um die parallele Datenmigration Im Auto-Modus wählt SSMA die Anzahl der Threads (standardmäßig 10) aus, die zum Migrieren von Daten verzweigt werden. Im benutzerdefinierten Modus kann der Benutzer die Anzahl der Threads angeben, die für die Datenmigration verzweigt werden sollen (der Minimalwert ist 1 und der Höchstwert 100). Derzeit unterstützt nur die Client seitige Daten Migrations-Engine die parallele Datenmigration.<br /><br />**Standardmodus**: automatisch<br /><br />**Optimistischer Modus**: automatisch<br /><br />**Vollständiger Modus**: automatisch|  
  
> [!IMPORTANT]  
> Wenn die Option " **paralleler Daten Migrations Modus** " auf " **Benutzer** definiert" festgelegt ist, wird eine neue Projekt Einstellungs Option **Thread Anzahl** angezeigt. Gibt die Anzahl von Threads an, die für die Datenmigration verwendet werden.  
  
