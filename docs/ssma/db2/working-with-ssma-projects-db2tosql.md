---
description: Arbeiten mit SSMA-Projekten (DB2ToSQL)
title: Arbeiten mit SSMA-Projekten (DB2ToSQL) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 07abef8a-28e8-4a66-927c-c9a5b8c938ef
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 67c772195ea4b2a2ab29c08219f77d04a5d6833c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069315"
---
# <a name="working-with-ssma-projects-db2tosql"></a>Arbeiten mit SSMA-Projekten (DB2ToSQL)
Wenn Sie DB2-Datenbanken zu migrieren möchten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , erstellen Sie zunächst ein SSMA-Projekt. Das Projekt ist eine Datei, die die folgenden Informationen enthält:  
  
-   Metadaten zu den DB2-Datenbanken, zu denen Sie migrieren möchten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Metadaten zur Ziel Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , die die migrierten Objekte und Daten empfängt.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Verbindungsinformationen.  
  
-   Projekteinstellungen.  
  
Wenn Sie ein Projekt öffnen, wird es von DB2 und getrennt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Auf diese Weise können Sie offline arbeiten. Weitere Informationen zum erneuten Herstellen einer Verbindung mit finden Sie unter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [Herstellen einer Verbindung mit SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-sql-server-db2tosql.md).  
  
## <a name="reviewing-default-project-settings"></a>Überprüfen der Standard Projekteinstellungen  
SSMA enthält mehrere Einstellungen zum wandeln und Laden von Datenbankobjekten, zum Migrieren von Daten und zum Synchronisieren von SSMA mit DB2 und [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Die Standardeinstellungen sind für viele Benutzer geeignet. Bevor Sie jedoch ein neues SSMA-Projekt erstellen, sollten Sie die Einstellungen überprüfen. Wenn Sie möchten, können Sie die Standardeinstellungen ändern, die für alle neuen Projekte verwendet werden.  
  
**So überprüfen Sie die Standard Projekteinstellungen**  
  
1.  Klicken Sie **im Menü Extras** auf **Standard Projekteinstellungen**.  
  
2.  Wählen Sie in der Dropdown Liste **Migrations Ziel Version** den Projekttyp aus, für den Einstellungen angezeigt oder geändert werden müssen, und klicken Sie dann auf die Registerkarte **Allgemein**  
  
3.  Klicken Sie im linken Bereich auf **Konvertierung**.  
  
4.  Überprüfen Sie im rechten Bereich die Einstellungen, und ändern Sie Sie nach Bedarf. Weitere Informationen zu diesen Einstellungen finden Sie unter [Project Settings &#40;Conversion&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-conversion-db2tosql.md).  
  
5.  Wiederholen Sie die Schritte 1-3 für die Seiten Migration, Synchronisierung, Laden von System Objekten, GUI und Typzuordnung.  
  
    -   Informationen zu den Migrations Einstellungen finden Sie unter [Project Settings &#40;Migration&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-migration-db2tosql.md).  
  
    -   Weitere Informationen zu Systemobjekt Einstellungen finden Sie unter [Project Settings&#40;Load System Objects&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-loading-system-objects-db2tosql.md).  
  
    -   Informationen zu den Einstellungen für die Synchronisierung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] finden Sie unter [Project Settings&#40;Synchronisierung&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-synchronization-db2tosql.md).  
  
    -   Informationen zu den GUI-Einstellungen finden Sie unter [Project Settings &#40;GUI&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-gui-db2tosql.md).  
  
    -   Informationen zu den Einstellungen für die Datentyp Zuordnung finden Sie unter [Project Settings &#40;Type Mapping&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-type-mapping-db2tosql.md).  
  
## <a name="creating-new-projects"></a>Erstellen neuer Projekte  
Wenn Sie Daten aus DB2-Datenbanken zu migrieren möchten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , müssen Sie zunächst ein Projekt erstellen.  
  
**So erstellen Sie ein Projekt**  
  
1.  Klicken Sie im Menü **Datei** auf **Neues Projekt**.  
  
    Das Dialogfeld **Neues Projekt** wird angezeigt.  
  
2.  Geben Sie im Feld **Name** einen Namen für das Projekt ein.  
  
3.  Geben Sie im Feld **Speicherort** einen Ordner für das Projekt ein, oder wählen Sie einen Ordner aus, und klicken Sie dann auf **OK**.  
  
4.  Wählen Sie in der Dropdown Liste **Migration zu** die Version des Ziels aus, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für die Migration verwendet wird. Die verfügbaren Optionen sind:  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2014  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016  
  
    -   Azure SQL-Datenbank  
  
## <a name="customizing-project-settings"></a>Anpassen von Projekteinstellungen  
Zusätzlich zum Definieren von Standard Projekteinstellungen, die für alle neuen SSMA-Projekte gelten, können Sie die Einstellungen für jedes Projekt anpassen. Weitere Informationen finden Sie unter [Festlegen von Projektoptionen &#40;oracleto SQL&#41;](../../ssma/oracle/setting-project-options-oracletosql.md) und verwandten Abschnitten.  
  
Wenn Sie Datentyp Zuordnungen zwischen Quell-und Ziel Datenbanken anpassen, können Sie Zuordnungen auf Projekt-, Datenbank-oder Objektebene definieren. Weitere Informationen finden Sie unter [Mapping von DB2-und SQL Server-Datentypen &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md).  
  
## <a name="saving-projects"></a>Speichern von Projekten  
Wenn Sie ein Projekt speichern, behält SSMA die Projekteinstellungen und optional die Daten Bank Metadaten in der Projektdatei.  
  
**So speichern Sie ein Projekt**  
  
-   Klicken Sie im Menü **Datei** auf **Projekt speichern**.  
  
    Wenn sich Schemas im Projekt geändert haben oder nicht konvertiert wurden, werden Sie von SSMA aufgefordert, Metadaten zu laden und zu speichern. Wenn Sie Metadaten laden und speichern, können Sie offline arbeiten. Außerdem können Sie eine komplette Projektdatei an andere Personen senden, z. b. technische Supportmitarbeiter. Wenn Sie zum Speichern von Metadaten aufgefordert werden, gehen Sie wie folgt vor:  
  
    1.  Aktivieren Sie für jedes Schema, das den Status **fehlender Metadaten** anzeigt, das Kontrollkästchen neben dem Datenbanknamen.  
  
        Das Speichern von Metadaten kann einige Minuten dauern. Wenn Sie noch keine Metadaten speichern möchten, aktivieren Sie keine Kontrollkästchen.  
  
    2.  Klicken Sie auf die Schaltfläche **Save** .  
  
        SSMA analysiert die DB2-Schemas und speichert die Metadaten in der Projektdatei.  
  
## <a name="opening-projects"></a>Öffnen von Projekten  
Wenn Sie ein Projekt öffnen, wird es von DB2 und von getrennt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Auf diese Weise können Sie offline arbeiten. Laden Sie Datenbankobjekte in, um Metadaten zu aktualisieren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Zum Migrieren von Daten müssen Sie erneut eine Verbindung mit DB2 und herstellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**So öffnen Sie ein Projekt**  
  
1.  Verwenden Sie eines der folgenden Verfahren:  
  
    -   Zeigen Sie im Menü **Datei** auf **zuletzt verwendete Projekte**, und klicken Sie dann auf das Projekt, das Sie öffnen möchten.  
  
    -   Wählen Sie im Menü **Datei** die Option **Projekt öffnen** aus, suchen Sie die Projektdatei. o2ssproj, wählen Sie die Datei aus, und klicken Sie dann auf **Öffnen**.  
  
2.  Um erneut eine Verbindung mit DB2 herzustellen, klicken Sie im Menü **Datei** auf **Verbindung mit DB2 wieder** herstellen.  
  
3.  Um erneut eine Verbindung mit herzustellen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , klicken Sie im Menü **Datei** auf **Verbindung wiederherstellen, um SQL Server**.  
  
## <a name="next-step"></a>Nächster Schritt  
Der nächste Schritt des Migrations Vorgangs besteht darin, eine [Verbindung mit der DB2-Datenbank](./connecting-to-db2-database-db2tosql.md)herzustellen.  
  
## <a name="see-also"></a>Weitere Informationen  
[Migrieren von DB2-Datenbanken zu SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
[Herstellen einer Verbindung mit der DB2-Datenbank &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-db2-database-db2tosql.md)  
[Herstellen einer Verbindung mit SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-sql-server-db2tosql.md)  
