---
description: Verbesserte Wiederherstellung von Datenbanken
title: Schnellere Datenbankwiederherstellung | Microsoft-Dokumentation
ms.date: 05/20/2020
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- accelerated database recovery [SQL Server], recovery-only
- database recovery [SQL Server]
author: mashamsft
ms.author: mathoma
ms.reviewer: kfarlee
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 70b68fa62c523a4c65bb4e70538e1882bc67290d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350071"
---
# <a name="accelerated-database-recovery"></a>Verbesserte Wiederherstellung von Datenbanken

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Durch die schnellere Datenbankwiederherstellung (Accelerated Database Recovery, ADR) wird die Verfügbarkeit von Datenbanken enorm verbessert, insbesondere bei zeitintensiven Transaktionen. Hierfür wurde der Wiederherstellungsprozess der SQL-Datenbank-Engine vollständig überarbeitet. Die beschleunigte Datenbankwiederherstellung (ADR) ist ein neues Feature in SQL Server 2019. 

ADR ist auch für Datenbanken in Azure SQL-Datenbank, Azure SQL Managed Instance und Azure Synapse SQL verfügbar. ADR ist in SQL-Datenbank und SQL Managed Instance standardmäßig aktiviert und kann nicht deaktiviert werden. 

## <a name="overview"></a>Übersicht

Die Hauptvorteile der schnellen Datenbankwiederherstellung sind:

- **Schnelle und konsistente Datenbankwiederherstellung**

  Mit ADR haben zeitintensive Transaktionen keinen Einfluss auf die gesamte Wiederherstellungszeit und ermöglichen eine schnelle und konsistente Wiederherstellung der Datenbank, unabhängig von der Anzahl der aktiven Transaktionen im System und deren Größe.

- **Sofortiges Transaktionsrollback**

  Mit ADR erfolgt das Transaktionsrollback sofort, unabhängig davon, wann die Transaktion aktiv war oder wie viele Updates ausgeführt wurden.

- **Drastische Protokollkürzung**

  Durch ADR wird das Transaktionsprotokoll auch bei aktiven, zeitintensiven Transaktionen konsequent verkürzt, sodass der Vorgang kontrollierbar bleibt.

## <a name="the-current-database-recovery-process"></a>Der aktuelle Prozess zur Datenbankwiederherstellung

Ohne ADR folgt die Datenbankwiederherstellung in SQL Server dem [ARIES](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf)-Wiederherstellungsmodell, der aus drei Phasen besteht. Diese werden im folgenden Diagramm dargestellt und anschließend näher erläutert.

![aktueller Wiederherstellungsprozess](./media/accelerated-database-recovery-concepts/current-recovery-process.png)

- **Die Analysephase**

  SQL Server führt einen Vorwärtsscan des Transaktionsprotokolls vom Beginn des letzten erfolgreichen Prüfpunkts (oder der LSN der letzten fehlerhaften Seite) bis zum Ende durch, um den Zustand jeder Transaktion zu dem Zeitpunkt zu bestimmen, an dem SQL Server angehalten wurde.

- **Die Rollforwardphase**

  SQL Server führt einen Vorwärtsscan des Transaktionsprotokolls von der ältesten Transaktion ohne Commit bis zum Ende durch, um die Datenbank in den Zustand zu versetzen, in dem sie sich zum Zeitpunkt des Absturzes befand. Hierzu werden alle bestätigten Vorgänge noch mal ausgeführt.

- **Die Rollbackphase**

  Für jede Transaktion, die zum Zeitpunkt des Absturzes aktiv war, durchläuft SQL Server das Protokoll rückwärts und macht die Vorgänge rückgängig, die die jeweilige Transaktion ausgeführt hat.

Hiervon ausgehend ist die Zeitspanne, die die Datenbank-Engine für die Wiederherstellung nach einem unerwarteten Neustart benötigt, (ungefähr) proportional zum Umfang der längsten aktiven Transaktion im System zum Zeitpunkt des Absturzes. Die Wiederherstellung erfordert ein Rollback aller unvollständigen Transaktionen. Die erforderliche Zeitspanne verhält sich proportional zum Umfang der von der Transaktion ausgeführten Aufgaben und der Dauer ihrer Aktivität. Daher kann der SQL Server-Wiederherstellungsprozess bei zeitintensiven Transaktionen (z.B. umfangreiche Vorgänge bei Masseneinfügungen und Indexerstellungen für eine große Tabelle) sehr lange dauern.

Auch das Abbrechen oder Rollback einer umfangreichen Transaktion, die auf diesem Konzept basiert, kann ebenfalls lange dauern, da die gleiche Rollback- und Wiederherstellungsphase wie oben beschrieben durchlaufen wird.

Darüber hinaus kann die Datenbank-Engine das Transaktionsprotokoll bei zeitintensiven Transaktionen nicht abschneiden, da die entsprechenden Protokolldatensätze für die Wiederherstellungs- und Rollbackprozesse benötigt werden. Infolgedessen werden einige Transaktionsprotokolle sehr groß und belegen extrem viel Speicherplatz.

## <a name="the-accelerated-database-recovery-process"></a>Der schnellere Prozess der Datenbankwiederherstellung

ADR löst die oben genannten Probleme, indem es den Wiederherstellungsprozess der Datenbank-Engine komplett neu gestaltet:

- Wählen Sie eine konstante Zeit bzw. die umgehende Wiederherstellung, indem Sie vermeiden, dass das Protokoll vom/zum Anfang der ältesten aktiven Transaktion gescannt werden muss. Mit ADR wird das Transaktionsprotokoll nur ab dem letzten erfolgreichen Prüfpunkt (oder der Protokollfolgenummer (LSN) der ältesten fehlerhaften Seite) verarbeitet. Dadurch wird die Wiederherstellungszeit nicht durch zeitintensive Transaktionen beeinflusst.
- Minimieren Sie den erforderlichen Speicherplatz für das Transaktionsprotokoll, da das Protokoll für die gesamte Transaktion nicht mehr verarbeitet werden muss. Folglich kann das Transaktionsprotokoll beim Auftreten von Prüfpunkten und Sicherungen konsequent abgeschnitten werden.

Im Allgemeinen wird mit ADR eine schnelle Datenbankwiederherstellung erreicht, da dieses Feature alle physischen Datenbankänderungen versioniert und nur logische Vorgänge rückgängig macht, die begrenzt sind und fast sofort rückgängig gemacht werden können. Alle Transaktionen, die zum Zeitpunkt eines Absturzes aktiv waren, werden als abgebrochen markiert, sodass alle durch diese Transaktionen erzeugten Versionen von gleichzeitigen Benutzerabfragen ignoriert werden können.

Die schnelle Datenbankwiederherstellung (ADR) besteht aus den gleichen drei Phasen wie der aktuelle Wiederherstellungsprozess. Der Ablauf dieser Phasen in Verbindung mit ADR ist in der folgenden Abbildung dargestellt.

![ADR-Wiederherstellungsprozess](./media/accelerated-database-recovery-concepts/adr-recovery-process.png)

- **Die Analysephase**

  Der Prozess bleibt derselbe, wird jedoch durch die Rekonstruktion von sLog und das Kopieren von Protokolldatensätzen für nicht versionierte Vorgänge ergänzt.
  
- **Die Rollforwardphase**

  Diese Phase ist in zwei Teilphasen unterteilt:
  - Teilphase 1

      Rollforward vom sLog (älteste Transaktion ohne Commit bis zum letzten Prüfpunkt). Dies ist ein schneller Vorgang, da nur wenige Datensätze aus dem sLog verarbeitet werden müssen.

  - Teilphase 2

     Rollforward vom Transaktionsprotokoll beginnt beim letzten Prüfpunkt (anstelle der ältesten Transaktion ohne Commit).
     
- **Die Rollbackphase**

   Die Rollbackphase mit ADR wird nahezu unmittelbar abgeschlossen, indem mithilfe von sLog nicht versionierte Vorgänge und persistenter Versionsspeicher (Persisted Version Store, PVS) mit logischer Wiederherstellung rückgängig gemacht werden, um ein versionsbasiertes Rollback auf Zeilenebene durchzuführen.

Sie können sich auch dieses 8-minütige Video ansehen, in dem die schnelle Datenbankwiederherstellung erklärt wird.

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Advanced-Database-Recovery--Data-Exposed/player?WT.mc_id=dataexposed-c9-niner]

## <a name="adr-recovery-components"></a>Komponenten der ADR-Wiederherstellung

Die vier wichtigsten Komponenten von ADR sind:

- **Persistenter Versionsspeicher (PVS)**

  Der persistente Versionsspeicher ist ein Datenbank-Engine-Mechanismus, mit dem die Zeilenversionen erhalten bleiben, die in der Datenbank selbst erstellt wurden, und nicht im herkömmlichen `tempdb`-Versionsspeicher. Mithilfe des PVS lassen sich Ressourcen isolieren und die Verfügbarkeit von lesbaren sekundären Datenbanken verbessern.

- **Logische Wiederherstellung**

  Die logische Wiederherstellung ist der asynchrone Prozess, der für das versionsbasierte Rückgängigmachen auf Zeilenebene zuständig ist und somit ein unmittelbares Rollback und Zurücksetzen für alle versionierten Vorgänge ermöglicht.

  - Verfolgt alle abgebrochenen Transaktionen
  - Führt ein Rollback mit dem PVS für alle Benutzertransaktionen durch
  - Gibt alle Sperren sofort nach Transaktionsabbruch frei

- **sLog**

  sLog ist ein sekundärer In-Memory-Protokolldatenstrom, der Protokolldatensätze für nicht versionierte Vorgänge (wie z.B. die Invalidierung von Metadatencaches oder das Einrichten von Sperren) speichert. Merkmale des sLog:

  - Geringe Datenmenge und wenig Speicherbedarf
  - Persistent auf Datenträgern durch Serialisierung während des Prüfpunktverfahrens
  - Wird beim Commit von Transaktionen periodisch gekürzt
  - Beschleunigt das Rollforward und Rollback durch ausschließliche Verarbeitung von nicht versionierten Vorgängen  
  - Ermöglicht ein konsequentes Abschneiden des Transaktionsprotokolls, wobei nur die erforderlichen Protokolldatensätze beibehalten werden

- **Bereinigung**

  Die Bereinigung ist ein asynchroner Prozess, der periodisch reaktiviert wird und nicht benötigte Seitenversionen bereinigt.

## <a name="who-should-consider-accelerated-database-recovery"></a>Zielgruppe der schnelleren Datenbankwiederherstellung

ADR ist für die folgenden Kundentypen geeignet:

- Kunden, die über Workloads mit zeitintensiven Transaktionen verfügen.
- Kunden, bei denen aktive Transaktionen in manchen Fällen dazu führen, dass die Transaktionsprotokolle sehr groß werden.  
- Kunden, bei denen es vorgekommen ist, dass die Datenbank länger nicht verfügbar war, da die SQL Server-Wiederherstellung lange gedauert hat (wie bei einem unerwarteten SQL Server-Neustart oder einem manuellen Transaktionsrollback).

>[!IMPORTANT]
>ADR wird für Datenbanken, die für Datenbankspiegelung registriert sind, nicht unterstützt.

## <a name="see-also"></a>Weitere Informationen  

[Verwalten der schnelleren Datenbankwiederherstellung](accelerated-database-recovery-management.md)
