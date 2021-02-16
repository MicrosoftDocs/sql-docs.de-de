---
title: Migrieren von Datenbanken zu SQL Server für Linux
description: In diesem Artikel werden die verschiedenen Optionen zum Migrieren von Datenbanken und Daten zu SQL Server für Linux beschrieben.
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 1619489d-377a-4f32-8930-d4f536539689
ms.openlocfilehash: d267bc2861d94e46cdb10fa825d204c8110a9273
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345585"
---
# <a name="migrate-databases-and-structured-data-to-sql-server-on-linux"></a>Migrieren von Datenbanken und strukturierten Daten zu SQL Server für Linux 

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Sie können Ihre Datenbanken und Daten zu SQL Server für Linux migrieren. Die Methode, die Sie verwenden, hängt von den Quelldaten und dem jeweiligen Szenario ab. In den folgenden Abschnitten werden bewährte Methoden für verschiedene Migrationsszenarien vorgestellt.

## <a name="migrate-from-sql-server-on-windows"></a>Migrieren von SQL Server unter Windows
Wenn Sie SQL Server-Datenbanken unter Windows zu SQL Server für Linux migrieren möchten, empfiehlt es sich, die SQL Server-Sicherung und -Wiederherstellung zu verwenden.

1. Erstellen Sie eine Sicherung der Datenbank auf dem Windows-Computer.
2. Übertragen Sie die Sicherungsdatei auf den SQL Server-Linux-Zielcomputer.
3. Stellen Sie die Sicherung auf dem Linux-Computer wieder her. 

Ein Tutorial zum Migrieren einer Datenbank mit Sicherung und Wiederherstellung finden Sie im folgenden Thema:

- [Migrieren einer SQL Server-Datenbank von Windows zu Linux mit Sicherung und Wiederherstellung](sql-server-linux-migrate-restore-database.md).

Sie können Ihre Datenbank auch in eine BACPAC-Datei (eine komprimierte Datei, die Datenbankschema und Daten enthält) exportieren. Wenn Sie über eine BACPAC-Datei verfügen, können Sie diese Datei auf den Linux-Computer übertragen und anschließend in SQL Server importieren. Weitere Informationen finden Sie in den folgenden Themen:

- [Exportieren und Importieren einer Datenbank mit SSMS oder SqlPackage.exe](sql-server-linux-migrate-ssms.md)

## <a name="migrate-from-other-database-servers"></a>Migrieren von anderen Datenbankservern
Sie können Datenbanken auf anderen Datenbanksystemen zu SQL Server für Linux migrieren. Dies schließt Microsoft Access-, DB2-, MySQL-, Oracle- und Sybase-Datenbanken ein. Verwenden Sie in diesem Szenario den SQL Server Management Assistant (SSMA), um die Migration zu SQL Server für Linux zu automatisieren. Weitere Informationen finden Sie unter [Automatisieren der Datenbankmigration zu Linux mit dem SQL Server Migration Assistant](sql-server-linux-migrate-ssma.md).  

## <a name="migrate-structured-data"></a>Migrieren strukturierter Daten
Es gibt auch Techniken zum Importieren von Rohdaten. Möglicherweise verfügen Sie über strukturierte Datendateien, die aus anderen Datenbanken oder Datenquellen exportiert wurden. In diesem Fall können Sie das BCP-Tool für das Masseneinfügen der Daten verwenden. Alternativ können Sie SQL Server Integration Services unter Windows ausführen, um die Daten in eine SQL Server-Datenbank unter Linux zu importieren. Mit SQL Server Integration Services können Sie während des Imports komplexere Transformationen der Daten ausführen. 

Weitere Informationen zu diesen Techniken finden Sie unter den folgenden Themen:

- [Massenkopieren von Daten mit BCP](sql-server-linux-migrate-bcp.md)
- [Extrahieren, Transformieren und Laden von Daten für SQL Server unter Linux mit SSIS](sql-server-linux-migrate-ssis.md) 
