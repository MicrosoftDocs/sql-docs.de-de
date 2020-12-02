---
description: SQL Server Compact Edition-Verbindungs-Manager
title: SQL Server Compact Edition-Verbindungs-Manager | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sqlmobileconnection.connection.f1
- sql13.dts.designer.sqlmobileconnection.all.f1
helpviewer_keywords:
- SQL Server Compact, connection manager
- connections [Integration Services], SQL Server Compact
- connection managers [Integration Services], SQL Server Compact
ms.assetid: ba627d4d-41f4-49fc-a921-f534cde67770
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ae61fcfcec740ee26a7fc2a57c0e03b0141e6e10
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "88425942"
---
# <a name="sql-server-compact-edition-connection-manager"></a>SQL Server Compact Edition-Verbindungs-Manager

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Mit einem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Verbindungs-Manager kann ein Paket eine Verbindung mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenbank herstellen. Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Ziel, das [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] enthält, lädt mithilfe dieses Verbindungs-Managers Daten in eine Tabelle einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenbank.  
  
> [!NOTE]  
>  Auf einem 64-Bit-Computer müssen Sie Pakete, die mit den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenquellen verbunden sind, im 32-Bit-Modus ausführen. Der Anbieter von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact, der von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] zum Herstellen einer Verbindung mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenquellen verwendet wird, ist lediglich in einer 32-Bit-Version verfügbar.  
  
## <a name="configuration-the-sql-server-compact-edition-connection-manager"></a>Konfiguration des SQL Server Compact Edition-Verbindungs-Managers  
 Wenn Sie einem Paket einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Verbindungs-Manager hinzufügen, wird von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ein Verbindungs-Manager erstellt, der zur Laufzeit zu einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Verbindung aufgelöst wird, die Eigenschaften des Verbindungs-Managers werden festgelegt, und der Verbindungs-Manager wird der **Connections** -Auflistung im Paket hinzugefügt.  
  
 Die **ConnectionManagerType** -Eigenschaft des Verbindungs-Managers ist auf **SQLMOBILE** festgelegt.  
  
 Es gibt folgende Möglichkeiten, um den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Verbindungs-Manager zu konfigurieren:  
  
-   Stellen Sie eine Verbindungszeichenfolge für den Speicherort der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenbank bereit.  
  
-   Stellen Sie ein Kennwort für eine kennwortgeschützte Datenbank bereit.  
  
-   Geben Sie den Server an, auf dem die Datenbank gespeichert ist.  
  
-   Geben Sie an, ob die im Verbindungs-Manager erstellte Verbindung zur Laufzeit beibehalten wird.  
  
 Sie können Eigenschaften mit dem [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer oder programmgesteuert festlegen.  
  
 Weitere Informationen zum programmgesteuerten Konfigurieren eines Verbindungs-Managers finden Sie unter <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> und [Programmgesteuertes Hinzufügen von Verbindungen](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md)festgelegt.  
  
## <a name="sql-server-compact-edition-connection-manager-editor-connection-page"></a>Verbindungs-Manager-Editor für SQL Server Compact Edition (Seite Verbindung)
  Mithilfe des Dialogfelds **Verbindungs-Manager-Editor für SQL Server Compact Edition** können Sie Eigenschaften für die Verbindung mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenbank angeben.  
  
 Weitere Informationen zum [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact Edition-Verbindungs-Manager finden Sie unter [Verbindungs-Manager-Editor für SQL Server Compact Edition](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager.md).  
  
### <a name="options"></a>Optionen  
 **Name und Pfad der Datenbankdatei**  
 Geben Sie den Pfad und Dateinamen der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenbank ein.  
  
 **Durchsuchen**  
 Suchen Sie die gewünschte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenbankdatei über das Dialogfeld **Select SQL Server Compact Edition database** (SQL Server Compact Edition-Datenbankdatei auswählen).  
  
 **Datenbankkennwort**  
 Geben Sie das Kennwort für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenbank ein.  
  
## <a name="sql-server-compact-edition-connection-manager-editor-all-page"></a>Verbindungs-Manager-Editor für SQL Server Compact Edition (Seite Alle)
  Mithilfe des Dialogfelds **Verbindungs-Manager-Editor für SQL Server Compact Edition** können Sie Eigenschaften für die Verbindung mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenbank angeben.  
  
 Weitere Informationen zum [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact Edition-Verbindungs-Manager finden Sie unter [Verbindungs-Manager-Editor für SQL Server Compact Edition](../../integration-services/connection-manager/sql-server-compact-edition-connection-manager.md).  
  
### <a name="options"></a>Optionen  
 **AutoShrink Threshold**  
 Gibt den freien Speicherplatz in Prozent an, der in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenbank zulässig ist, bevor der Prozess des automatischen Verkleinerns ausgeführt wird.  
  
 **Default Lock Escalation**  
 Gibt die Anzahl der Datenbanksperren an, die die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenbank abruft, bevor sie versucht, die Sperren auszuweiten.  
  
 **Default Lock Timeout**  
 Gibt das Standardintervall in Millisekunden an, das eine Transaktion auf eine Sperre wartet.  
  
 **Flush Interval**  
 Gibt das Intervall zwischen Transaktionen an, für die ein Commit erfolgt ist, und in dem die Daten auf einen Datenträger gespeichert werden. Zeiteinheit sind Sekunden.  
  
 **Locale Identifier**  
 Gibt die Gebietsschema-ID (Locale ID, LCID) der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenbank an.  
  
 **Max Buffer Size**  
 Gibt die maximale Menge an Arbeitsspeicher in Kilobyte an, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact verwendet werden, bevor die Daten auf einem Datenträger gespeichert werden.  
  
 **Max Database Size**  
 Gibt die Maximalgröße der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenbank in Megabyte an.  
  
 **Mode**  
 Gibt an, in welchem Dateimodus die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenbank zu öffnen ist. Der Standardwert für diese Eigenschaft lautet **Read Write**.  
  
 Die Option Mode kann vier Werte annehmen. Siehe dazu die folgende Tabelle.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Schreibgeschützt**|Gibt an, dass der Zugriff auf die Datenbank schreibgeschützt erfolgen kann.|  
|**Read Write**|Gibt Lese-/Schreibberechtigungen für die Datenbank an.|  
|**Exklusiv**|Gibt exklusiven Zugriff auf die Datenbank an.|  
|**Shared Read**|Gibt an, dass andere Benutzer zu selben Zeit Daten aus der Datenbank lesen können.|  
  
 **Persist Security Info**  
 Gibt an, ob als Teil der Verbindungszeichenfolge Sicherheitsinformationen zurückgegeben werden sollen. Der Standardwert für diese Option ist **False**.  
  
 **Temp File Directory**  
 Geben Sie den Pfad der temporären [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenbankdatei an.  
  
 **Data Source**  
 Geben Sie den Namen der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenbank an.  
  
 **Kennwort**  
 Geben Sie das Kennwort für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact-Datenbank ein.  
  
