---
description: Sichern und Wiederherstellen von Datenbanken mit Always Encrypted
title: Sichern und Wiederherstellen von Datenbanken mit Always Encrypted | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1b5f491846b51b1d70b36c73029ad96706cb1b3e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345481"
---
# <a name="backup-and-restore-databases-using-always-encrypted"></a>Sichern und Wiederherstellen von Datenbanken mit Always Encrypted 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

In diesem Artikel wird beschrieben, wie Sie Datenbanken sichern und wiederherstellen, die mit [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) geschützte Spalten enthalten.

Wenn Sie eine Datenbank sichern, enthält die daraus resultierende Sicherungsdatei verschlüsselte Metadaten, die in verschlüsselten Spalten gespeichert werden, sowie alle Metadaten für Always Encrypted-Schlüssel.

Wenn Sie eine Datenbank wiederherstellen, werden alle verschlüsselten Daten und alle Metadaten für Always Encrypted-Schlüssel wieder hergestellt. 

Wenn Sie die Datenbank auf einem anderen Server oder unter einem anderen Namen wiederhergestellt haben, können Sie die Anwendung ganz einfach für Abfragen der verschlüsselten Daten in der Zieldatenbank aktivieren, da die Schlüssel in beiden Datenbanken identisch sind.

Weitere Informationen zum Sichern und Wiederherstellen einer Datenbank finden Sie hier:
- [Backup Overview (SQL Server)](../../backup-restore/backup-overview-sql-server.md)
- [Wiederherstellen einer Datenbank in eine verwaltete Instanz](/azure/sql-database/sql-database-managed-instance-get-started-restore)

## <a name="next-steps"></a>Nächste Schritte
- [Abfragen von Spalten mit Always Encrypted und SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Entwickeln von Anwendungen mithilfe von Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-client-development.md) 

## <a name="see-also"></a>Weitere Informationen
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Exportieren und Importieren von Datenbanken mit Always Encrypted](always-encrypted-migrate-using-bacpac.md)
- [Migrieren von Daten zu oder aus Spalten mithilfe von Always Encrypted mit dem SQL Server-Import/Export-Assistenten](always-encrypted-migrate-using-import-export-wizard.md)
- [Massenladen von verschlüsselten Daten in Spalten mithilfe von Always Encrypted](migrate-sensitive-data-protected-by-always-encrypted.md)