---
description: SQL Server-Sicherung und -Wiederherstellung mit dem Microsoft Azure Blob Storage Service
title: Sichern und Wiederherstellen mit Azure Blob Storage
storage: Learn about SQL Server backup to and restore from Azure Blob storage, including the benefits of using Azure Blob storage to store SQL Server backups.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 6a0c9b6a-cf71-4311-82f2-12c445f63935
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2fb47efd89dfee8777d54b4cf2d1cf0ad2accec9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343648"
---
# <a name="sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service"></a>SQL Server-Sicherung und -Wiederherstellung mit dem Microsoft Azure Blob Storage Service
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  ![Grafik: Sicherung in Azure Blob Storage](../../relational-databases/backup-restore/media/backup-to-azure-blob-graphic.png "Grafik: Sicherung in Azure Blob Storage")  
  
 Dieses Thema bietet eine Einführung in die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherung und -Wiederherstellung unter Verwendung von [Microsoft Azure Blob Storage Service](https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/). Darüber hinaus werden die Vorteile zusammengefasst, die die Verwendung von Microsoft Azure Blob-Dienst bei der Speicherung von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherungen bietet.  
  
 SQL Server unterstützt das Speichern von Sicherungen im Microsoft Azure Blob Storage Service auf folgende Weise:  
  
-   **Verwalten von Sicherungen in Microsoft Azure:** Mithilfe derselben Methoden, die für DISK- und TAPE-Sicherungen verwendet werden, können Sie Ihre Sicherungen jetzt in Microsoft Azure Storage speichern, indem Sie die URL als Sicherungsziel angeben. Mithilfe dieser Funktion können Sie manuelle Sicherungen ausführen oder eine eigene Sicherungsstrategie konfigurieren, wie auch für einen lokalen Speicher oder andere externe Optionen. Diese Funktion wird auch als **SQL Server-Sicherung über URLs** bezeichnet. Weitere Informationen finden Sie unter [SQL Server Backup to URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md). Diese Funktion ist in SQL Server 2012 SP1 CU2 oder höher verfügbar. Diese Funktion wurde in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] erweitert, um verbesserte Leistung und Funktionalität durch die Verwendung von Blockblobs, SAS und Striping zu bieten.  
  
    > [!NOTE]  
    >  Für SQL Server-Versionen vor SQL Server 2012 SP1 CU2 können Sie das Add-In „SQL Server Backup to Microsoft Azure Tool“ verwenden, um Sicherungen schnell und einfach in Microsoft Azure Storage zu erstellen. Weitere Informationen finden Sie im [Download Center](https://go.microsoft.com/fwlink/?LinkID=324399).  
  
-   **Dateimomentaufnahmesicherungen für Datenbankdateien in Azure Blob Storage** Durch die Verwendung von Azure-Momentaufnahmen bieten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dateimomentaufnahme-Sicherungen die fast sofortigen Sicherungen und Wiederherstellungen für Datenbankdateien, die mithilfe von Azure Blob Storage Service gespeichert wurden. Diese Funktion ermöglicht Ihnen, Ihre Sicherungs- und Wiederherstellungsrichtlinien zu vereinfachen und sie unterstützt Point-in-Time-Wiederherstellung. Weitere Informationen finden Sie unter [Dateimomentaufnahme-Sicherungen für Datenbankdateien in Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md). Diese Funktion ist in SQL Server 2016 oder höher verfügbar.  
  
-   **Verwalten von SQL Server-Sicherungen in Microsoft Azure:** Konfigurieren Sie SQL Server, um die Sicherungsstrategie zu verwalten und Sicherungen für eine einzelne oder mehrere Datenbanken zu planen oder um Standardwerte auf Instanzebene festzulegen. Diese Funktion wird als **[!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]** bezeichnet. Weitere Informationen finden Sie unter [SQL Server Managed Backup für Microsoft Azure](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md). Diese Funktion ist in SQL Server 2014 oder höher verfügbar.  
  
## <a name="benefits-of-using-the-microsoft-azure-blob-service-for-ssnoversion-backups"></a>Vorteile bei der Verwendung von Microsoft Azure Blob-Dienst für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherungen  
  
-   Flexible, zuverlässige und unbegrenzte externe Speicherung: Die Speicherung von Sicherungen im Microsoft Azure Blob-Dienst ist eine bequeme, flexible und benutzerfreundliche Möglichkeit, Daten extern zu speichern. Das Einrichten eines Offsitespeichers für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherungen muss nicht schwieriger sein als das Bearbeiten vorhandener Skripts/Aufträge. Die räumliche Entfernung zwischen Offsitespeicher und Produktionsdatenbank ist in der Regel so groß, dass ein Notfall niemals Auswirkungen auf beide Standorte hat. Indem Sie den BLOB-Speicher auf geografisch verteilte Standorte replizieren, erhalten Sie bei einem Notfall, der die gesamte Region betreffen könnte, eine zusätzliche Schutzebene. Darüber hinaus sind Sicherungen an jedem Standort jederzeit verfügbar und können problemlos für die Wiederherstellung genutzt werden.  
  
    > [!IMPORTANT]  
    >  Durch die Verwendung von Blockblobs in [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]können Sie ein Striping für Ihren Sicherungssatz durchführen, um Sicherungsdateigrößen von bis zu 12,8 TB zu unterstützen.  
  
-   Sicherungsarchiv: Der Microsoft Azure Blob Storage Service bietet eine bessere Alternative zur Bandsicherung, die häufig zum Archivieren von Sicherungen verwendet wird. Bandspeichermedien müssen unter gleichzeitiger Berücksichtigung von Sicherheitsvorkehrungen u. U. physisch an einen externen Standort umgelagert werden. Die Speicherung von Sicherungen im Microsoft Azure Blob Storage ist eine direkte, hoch verfügbare und dauerhafte Archivierungslösung für Ihre Daten.  
  
-   Kein zusätzlicher Hardwareverwaltungsaufwand: Mit Microsoft Azure-Diensten entsteht kein Mehraufwand für die Hardwareverwaltung. Microsoft Azure-Dienste übernehmen die Hardwareverwaltung und bieten die Georeplikation, um Redundanz und Schutz vor Hardwareausfällen zu gewährleisten.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanzen, die auf einem virtuellen Microsoft Azure-Computer ausgeführt werden, können derzeit unter Verwendung von Microsoft Azure Blob Storage Services gesichert werden, indem angefügte Datenträger erstellt werden. Die Anzahl der Datenträger, die an einen virtuellen Computer in Microsoft Azure angefügt werden können, ist allerdings begrenzt. Bei einer sehr großen Instanz beträgt die maximale Anzahl 16 Datenträger, während kleinere Instanzen weniger Datenträger unterstützen. Die Beschränkung auf 16 Datenträger kann umgangen werden, indem Sie eine direkte Sicherung in Microsoft Azure Blob Storage ermöglichen.  
  
     Darüber hinaus steht die Sicherungsdatei, die jetzt in Microsoft Azure Blob Storage Service gespeichert wird, direkt einem lokalen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder einem anderen, auf einem virtuellen Computer in Microsoft Azure ausgeführten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zur Verfügung, ohne dass Datenbanken angefügt/getrennt oder die VHD heruntergeladen und angefügt werden muss.  
  
-   Kostenvorteile: Sie zahlen nur für den genutzten Dienst. Die Option kann genauso kosteneffizient wie eine externe Sicherungs-/Archivierungslösung sein. Weitere Informationen und Links finden Sie im Abschnitt [Überlegungen zur Abrechnung in Microsoft Azure](#Billing) .  
  
##  <a name="microsoft-azure-billing-considerations"></a><a name="Billing"></a> Überlegungen zur Abrechnung in Microsoft Azure:  
 Auf Grundlage der Speicherkosten für Microsoft Azure können Sie die Kosten für die Erstellung und Speicherung von Sicherungen in Microsoft Azure ableiten.  
  
 Der [Microsoft Azure-Preisrechner](https://go.microsoft.com/fwlink/?LinkId=277060) unterstützt Sie bei der Kostenschätzung.  
  
 **Speicher:** Gebühren richten sich nach dem genutzten Speicherplatz sowie dem Grad der Redundanz und werden gestaffelt abgerechnet. Weitere Details und aktuelle Informationen finden Sie im Abschnitt **Datenverwaltung** des Artikels [Preisdetails](https://go.microsoft.com/fwlink/?LinkId=277059) .  
  
 **Datenübertragungen:** In Microsoft Azure eingehende Datenübertragungen sind kostenlos. Ausgehende Übertragungen werden nach Bandbreitennutzung und einer regionsspezifischen Staffelung berechnet. Weitere Informationen finden Sie im Abschnitt [Datenübertragungen](https://go.microsoft.com/fwlink/?LinkId=277061) des Artikels zu Preisdetails.  
  
## <a name="see-also"></a>Siehe auch  

[SQL Server-Sicherung über URLs – bewährte Methoden und Problembehandlung](../../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)   

[Sichern und Wiederherstellen von Systemdatenbanken &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)   

[Tutorial: Verwenden des Microsoft Azure BLOB-Speicherdiensts mit SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)

[SQL Server-Sicherung über URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md)  
  
  
