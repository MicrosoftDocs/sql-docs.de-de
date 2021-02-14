---
description: Konfigurieren kompatibler SQL Server-Features mit Stretch Database
title: Konfigurieren kompatibler SQL Server-Features
ms.date: 03/14/2017
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: c8121ede-1aec-459b-b7b0-1408bb3e62fb
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: c1f2389ea412b1cdc542f87ee25f3a95c34fa256
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080211"
---
# <a name="configure-compatible-sql-server-features-with-stretch-database"></a>Konfigurieren kompatibler SQL Server-Features mit Stretch Database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


Führen Sie einfache Schritte aus, um die folgenden SQL Server-Features für die Arbeit mit Stretch Database zu konfigurieren.
-   Always On
-   Always Encrypted
-   TDE (Transparent Data Encryption)
-   Temporale Tabellen

## <a name="configure-always-on-with-stretch-database"></a>Konfigurieren von Always On mit Stretch Database
Wenn Sie Always On mit Stretch Database verwenden, müssen Sie sicherstellen, dass der Datenbank-Hauptschlüssel auf den sekundären Replikaten verfügbar ist. Stretch Database verwendet den Datenbank-Hauptschlüssel zum Sichern der Anmeldeinformationen, die für die Verbindung mit der Azure-Remotedatenbank verwendet werden.

Nachdem Sie die Always On-Verfügbarkeitsgruppe eingerichtet haben, führen Sie die gespeicherte Prozedur **sp_control_dbmasterkey_password** auf jedem sekundären Replikat aus, und stellen Sie das Kennwort für die Stretch-fähige Datenbank bereit. Weitere Informationen und Beispiele finden Sie unter [sp_control_dbmasterkey_password](../../relational-databases/system-stored-procedures/sp-control-dbmasterkey-password-transact-sql.md). 

## <a name="configure-always-encrypted-with-stretch-database"></a>Konfigurieren von Always Encrypted mit Stretch Database
Wenn Sie Always Encrypted und Stretch Database zusammen verwenden möchten, müssen Sie die Verschlüsselung für die ausgewählten Spalten konfigurieren, bevor Sie Stretch Database für die Tabelle aktivieren.

Wenn Sie Stretch Database für die Tabelle bereits aktiviert haben und Sie Always Encrypted-Spalten verwenden möchten, müssen Sie die folgenden Schritte ausführen.
1.   Deaktivieren Sie Stretch Database für die Tabelle, und holen Sie die Remotedaten aus Azure zurück. Weitere Informationen finden Sie unter [Deaktivieren von Stretch Database und Zurückholen von Remotedaten](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).
2.   Konfigurieren Sie Always Encrypted für die ausgewählten Spalten.
3. Aktivieren Sie Stretch Database für die Tabelle erneut. Weitere Informationen finden Sie unter [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).

## <a name="configure-transparent-data-encryption-tde-with-stretch-database"></a>Konfigurieren der transparenten Datenverschlüsselung (TDE) mit Stretch Database

Wenn TDE für die lokale Datenbank aktiviert ist, wird sie auf dem Remoteendpunkt für Stretch-Datenbank nicht automatisch aktiviert. Sie müssen daran denken, dass Sie TDE für den Remoteendpunkt aktivieren, nachdem Sie Stretch für Ihre Datenbank aktiviert haben.

## <a name="configure-temporal-tables-with-stretch-database"></a>Konfigurieren temporaler Tabellen mit Stretch Database
Wenn Sie temporale Tabellen verwenden, können Sie Stretch Database für die Verlaufstabelle, aber nicht für die aktuelle Tabelle aktivieren.
-   Hinweise zur Verwendung von temporalen Tabellen mit Stretch Database finden Sie unter [Verwalten der Beibehaltung von Verlaufsdaten in temporalen Tabellen mit Systemversionsverwaltung](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md).
-   Informationen zum Filtern von Zeilen für die Migration aus der Verlaufstabelle mithilfe von gleitenden Fenstern finden Sie unter [Auswählen zu migrierender Zeilen mithilfe einer Filterfunktion](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).
-   Sie können Stretch Database in der temporalen Verlaufstabelle nicht aktivieren, wenn die Tabelle speicheroptimiert ist. Speicheroptimierte Tabellen werden nicht unterstützt.
