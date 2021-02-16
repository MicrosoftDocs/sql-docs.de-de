---
title: Verbessertes Failover für Verfügbarkeitsgruppen
description: Mit diesen Schritten aktivieren Sie ein verbessertes Datenbankfailover, bei dem ein Failover ausgelöst wird, wenn eine Datenbank in einer Always On-Verfügbarkeitsgruppe keine Transaktionen mehr schreiben kann.
ms.custom: seodec18
ms.date: 06/03/2020
ms.prod: sql
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], enhanced database failover
- Availability Groups [SQL Server], failover
ms.assetid: ''
author: cawrites
ms.reviewer: mikeray
ms.author: chadam
ms.openlocfilehash: 9122be35804569519bb3945d34f6950cd0dafb78
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340438"
---
# <a name="enable-enhanced-database-failover-to-a-database-in-an-always-on-availability-group"></a>Aktivieren eines verbesserten Datenbankfailovers für eine Datenbank in einer Always On-Verfügbarkeitsgruppe
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

SQL Server 2012 und 2014: Wenn eine Datenbank, die in einer Verfügbarkeitsgruppe auf dem primären Replikat enthalten ist, keine Transaktionen mehr schreiben kann, wird dadurch kein Failover ausgelöst, selbst wenn die Replikate synchronisiert und für automatisches Failover konfiguriert sind.

Mit SQL Server 2016 wird ein neues optionales Verhalten namens *verbessertes Datenbank-Failover* eingeführt, das entweder durch den Assistenten oder Transact-SQL festgelegt werden kann. Wenn diese Option aktiviert und automatisches Failover konfiguriert ist und eine Datenbank, die in einer Verfügbarkeitsgruppe enthalten ist, keine Protokolle mehr schreiben kann, wird ein Failover auf ein synchronisiertes sekundäres Replikat ausgelöst.

**Szenario 1**

Eine Verfügbarkeitsgruppe wird zwischen Instanz A und Instanz B konfiguriert und enthält eine Datenbank namens DB1. Die Datendatei von DB1 befindet sich auf Laufwerk E und die Transaktionsprotokolldatei auf Laufwerk F. Der Verfügbarkeitsmodus ist auf synchronen Commit mit automatischem Failover festgelegt. Die neue Option für verbessertes Datenbank-Failover wird in der Verfügbarkeitsgruppe konfiguriert. Die beiden Replikate befinden sich aktuell in einem synchronisierten Status. Ein Problem verursacht einen Ausfall von Laufwerk E. Dieses Szenario wird kein verbessertes Datenbank-Failover verursachen, da sich das Transaktionsprotokoll nicht auf Laufwerk E befindet.  

**Szenario 2**

Dieses verfügt über die gleiche Verfügbarkeitsgruppenkonfiguration wie in Szenario 1. Dieses Mal fällt das Laufwerk F statt E aus, auf dem sich das Transaktionsprotokoll befindet. Dadurch wird ein Failover ausgelöst, da die Bedingung für ein verbessertes Datenbank-Failover erfüllt ist: Das Transaktionsprotokoll ist nicht erreichbar, was bedeutet, dass die Datenbank keine Transaktionen schreiben kann.

**Szenario 3**

Eine Verfügbarkeitsgruppe wird zwischen Instanz A und Instanz B konfiguriert und enthält zwei Datenbanken: DB1 und DB2. Der Verfügbarkeitsmodus ist auf synchronen Commit mit automatischem Failover-Modus festgelegt und das verbesserte Datenbank-Failover ist aktiviert. Der Zugriff auf die Festplatte, die die Daten und Transaktionsprotokolldateien von DB2 enthält, ist unterbrochen. Wenn das Problem erkannt wird, führt die Verfügbarkeitsgruppe automatisch ein Failover zu Instanz B aus.

## <a name="configure-enhanced-failover"></a>Konfigurieren von verbessertem Failover

Das verbesserte Datenbank-Failover kann mithilfe von SQL Server Management Studio oder Transact-SQL konfiguriert werden. Die PowerShell-Cmdlets sind dazu derzeit nicht in der Lage. Standardmäßig ist das verbesserte Datenbank-Failover deaktiviert.

### <a name="sql-server-management-studio"></a>SQL Server Management Studio

Das Aktivieren des verbesserten Datenbank-Failovers ist während der Erstellung einer Verfügbarkeitsgruppe mithilfe von SQL Server Management Studio möglich. Die einzige Möglichkeit, um das Verhalten nach dem Erstellen der Verfügbarkeitsgruppe zu ändern, ist die Verwendung von Transact-SQL.

*Manuelles Erstellen von Verfügbarkeitsgruppen*

Verwenden Sie zum Erstellen der Verfügbarkeitsgruppe die Anweisungen im Artikel [Use the New Availability Group Dialog Box (SQL Server Management Studio) (Verwenden des neuen Dialogfelds für Verfügbarkeitsgruppen (SQL Server Management Studio))](use-the-new-availability-group-dialog-box-sql-server-management-studio.md). Um den verbesserten Datenbank-Failover zu aktivieren, aktivieren Sie das Kontrollkästchen neben *Integritätserkennung auf Datenbankebene*.

*Verwenden des Assistenten für Verfügbarkeitsgruppen*

Verwenden Sie die Anweisungen im Artikel [Use the Availability Group Wizard (SQL Server Management Studio) (Verwenden des Assistenten zum Hinzufügen von Datenbanken zu Verfügbarkeitsgruppen (SQL Server))](use-the-availability-group-wizard-sql-server-management-studio.md). Die Option zum Aktivieren des erweiterten Datenbank-Failovers befindet sich im Dialogfeld „Namen der Verfügbarkeitsgruppe angeben“. Um diese zu aktivieren, aktivieren Sie das Kontrollkästchen neben *Integritätserkennung auf Datenbankebene*.

### <a name="transact-sql"></a>Transact-SQL

Um das Verhalten des verbesserten Datenbank-Failovers während der Erstellung einer Verfügbarkeitsgruppe zu konfigurieren, muss „DB_FAILOVER“ wie im Folgenden auf „ON“ festgelegt sein:

```SQL
CREATE AVAILABILITY GROUP [AGNAME]
WITH ( DB_FAILOVER = ON)
...
```
Verwenden Sie den Befehl „ALTER AVAILABILITY GROUP“, um das Verhalten nach dem Konfigurieren einer Verfügbarkeitsgruppe hinzuzufügen:
```SQL
ALTER AVAILABILITY GROUP [AGNAME] SET (DB_FAILOVER = ON)
```
Führen Sie den folgenden Befehl „ALTER AVAILABILITY GROUP“ aus, um das Verhalten zu deaktivieren:
```SQL
ALTER AVAILABILITY GROUP [AGNAME] SET (DB_FAILOVER = OFF)
```
### <a name="dynamic-management-view"></a>Dynamische Verwaltungssicht
Fragen Sie die dynamische Verwaltungssicht `sys.availability_groups` ab, um anzuzeigen, ob das verbesserte Datenbank-Failover in einer Verfügbarkeitsgruppe aktiviert ist. Die Spalte `db_failover` enthält eine 0 (Null), wenn das Verhalten deaktiviert ist oder eine 1, wenn es aktiviert ist. 

## <a name="next-steps"></a>Nächste Schritte 

- [Konfigurieren von Datenbank-Integritätserkennung](sql-server-always-on-database-health-detection-failover-option.md)

- [Verwenden des Assistenten zum Hinzufügen von Datenbanken zu Verfügbarkeitsgruppen (SQL Server)](use-the-availability-group-wizard-sql-server-management-studio.md)

- [Verwenden des Dialogfelds Neue Verfügbarkeitsgruppe (SQL Server Management Studio)](use-the-new-availability-group-dialog-box-sql-server-management-studio.md)
 
- [Erstellen einer Verfügbarkeitsgruppe mit Transact-SQL](create-an-availability-group-transact-sql.md)

