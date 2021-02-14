---
title: Installieren von SSMA-Komponenten auf SQL Server (oracleto SQL) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie das SSMA-Erweiterungspaket und Oracle-Anbieter auf dem Computer installieren, auf dem SQL Server ausgeführt wird, um die Oracle-Datenbankkonvertierung
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Installing the extension pack
- SQL Server Database Objects
ms.assetid: 33070e5f-4e39-4b70-ae81-b8af6e4983c5
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 81d3f542e64030dbf6a0bf70067307222bef5a8b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344213"
---
# <a name="installing-ssma-components-on-sql-server-oracletosql"></a>Installieren von SSMA-Komponenten auf SQL Server (oracleto SQL)

Zusätzlich zur Installation von SSMA müssen Sie auch Komponenten auf dem Computer installieren, auf dem ausgeführt wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Diese Komponenten umfassen das SSMA-Erweiterungspaket, das die Datenmigration unterstützt, und Oracle-Anbieter, um die Server-zu-Server-Konnektivität zu ermöglichen.

## <a name="ssma-for-oracle-extension-pack"></a>SSMA für Oracle-Erweiterungspaket

Das SSMA-Erweiterungspaket stellt erweiterte gespeicherte Prozeduren bereit und fügt die Datenbanken **sysdb** und **ssmatesterdb** zur angegebenen Instanz von hinzu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Erweiterte gespeicherte Prozeduren stellen Funktionen bereit, die zum Emulieren von Features und behaiov von Oracle erforderlich sind, während die **sysdb** -Datenbank die zum Migrieren der Daten erforderlichen Tabellen und gespeicherten Prozeduren enthält Die **ssmatesterdb** -Datenbank enthält die Tabellen und Prozeduren, die von der Tester-Komponente (sofern installiert) benötigt werden.

Beim Migrieren von Daten zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden von SSMA außerdem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agentaufträge erstellt, wenn die serverseitige Daten Migrations-Engine zum Migrieren der Daten verwendet wird.

### <a name="prerequisites"></a>Voraussetzungen

Stellen Sie vor der Installation der SSMA für Oracle-Serverkomponenten unter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sicher, dass das System die folgenden Anforderungen erfüllt:

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Instanz ist installiert.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 oder eine höhere Version.
- Die [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] Version 4.7.2 oder eine spätere Version. Sie können Sie über das [.NET Framework Developer Center](https://go.microsoft.com/fwlink/?LinkId=48882)abrufen.
- Der OLE DB-Anbieter für Oracle (bei Verwendung OLE DB) und die Verbindung mit der Oracle-Datenbank, die Sie migrieren möchten. Sie können Anbieter auf dem Oracle-Produkt Medium oder auf der Oracle-Website installieren.
- Der- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser-Dienst muss während der Installation ausgeführt werden. Diese wird verwendet, um eine Liste der Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] im Setup-Assistenten aufzufüllen. Sie können den- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser Dienst nach der Installation deaktivieren.

  > [!NOTE]
  > Wenn der- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser-Dienst ausgeführt wird, im Setup aber immer noch keine Liste der Instanzen angezeigt wird, müssen Sie die Blockierung des UDP-Ports 1434 deaktivieren. Mit der Windows-Firewall können Sie den Port vorübergehend entsperren, oder Sie können die Windows-Firewall temporär deaktivieren. Möglicherweise müssen Sie die Antivirussoftware auch vorübergehend deaktivieren. Stellen Sie sicher, dass Sie nach der Installation Firewalls und Antivirensoftware aktivieren.

### <a name="installing-the-extension-pack"></a>Installieren des Erweiterungspakets

Sie können das Erweiterungspaket jederzeit installieren, bevor Sie Daten zu migrieren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

> [!IMPORTANT]
> Um das Erweiterungspaket zu installieren, müssen Sie Mitglied der **sysadmin** -Server Rolle auf der-Instanz sein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

So installieren Sie das Erweiterungspaket:

1. Kopieren Sie **SSMAforOracleExtensionPack_ *n*. msi** (wobei *n* die Buildnummer ist) auf den Computer, auf dem ausgeführt wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
2. Doppelklicken Sie auf **SSMAforOracleExtensionPack_ *n*. msi**.
3. Klicken Sie auf der Seite **Willkommen** auf **Weiter**.
4. Lesen Sie auf der Seite **Endbenutzer-Lizenzvertrag** den Lizenzvertrag. Wenn Sie zustimmen, aktivieren Sie **die Option Ich akzeptiere die Vereinbarung** , und klicken Sie dann auf **weiter**.
5. Wählen Sie auf der Seite Setuptyp **auswählen** die Option **typisch** aus.
6. Wählen Sie auf der Seite **bereit zum Installieren** die Option **Installieren** aus.
7. Wählen Sie auf der Seite **der erste Schritt der Installation ist abgeschlossen** die Option **weiter** aus.
  
   Ein neues Dialogfeld wird angezeigt. Wählen Sie den Typ des Erweiterungspakets aus.
  
8. Wählen Sie den gewünschten Installationstyp aus, und klicken Sie auf **weiter**.

   > [!IMPORTANT]
   > Die Remote Option sollte nur verwendet werden, wenn das Erweiterungspaket bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Ausführung unter Linux oder als Zielplattform installiert wird [!INCLUDE[ssAzureMi](../../includes/ssazuremi_md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Installationen, die unter Windows ausgeführt werden, sollten das Erweiterungspaket immer lokal installieren. [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] und Azure Synapse Analytics unterstützen das Erweiterungspaket nicht.

   Wenn Sie das Erweiterungspaket auf einer lokalen Instanz von installieren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , können Sie auf der nächsten Seite eine lokale Instanz von auswählen, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für die Sie Oracle-Schemas migrieren möchten. Wählen Sie eine Instanz aus, und klicken Sie dann auf **weiter**.

   Die Standard Instanz hat denselben Namen wie der Computer. Auf benannte Instanzen folgt ein umgekehrter Schrägstrich und der Instanzname.

9. Wählen Sie auf der Seite Verbindung die Authentifizierungsmethode aus, und klicken Sie dann auf **weiter**.

   Die Windows-Authentifizierung verwendet Ihre Windows-Anmelde Informationen, um sich bei der Instanz von anzumelden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Wenn Sie Server Authentifizierung auswählen, müssen Sie einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Anmelde Namen und ein Kennwort eingeben.

10. Im nächsten Schritt müssen Sie das Kennwort für einen Hauptschlüssel festlegen, der zum Verschlüsseln vertraulicher Daten verwendet wird, die während der serverseitigen Datenmigration in der Erweiterungspaket-Datenbank gespeichert werden. Geben **Sie ein** sicheres Kennwort ein, und klicken Sie auf

11. Wählen Sie auf der nächsten Seite **Utilities Database *n* installieren aus, und installieren Sie Extension Pack-Bibliotheken**, wobei *n* die Versionsnummer ist. Wenn Sie das Tester-Feature verwenden möchten, aktivieren Sie das Kontrollkästchen **Tester Datenbank installieren** , und klicken Sie dann auf **weiter**.

    Die **sysdb** -Datenbank wird mit den Tabellen und gespeicherten Prozeduren erstellt, die für die Datenmigration erforderlich sind (mithilfe der serverseitigen Daten Migrations-Engine), die in dieser Datenbank erstellt werden.

    Wenn die Option zum **Installieren von Tester Database** aktiviert ist, wird die **ssmatesterdb** -Datenbank erstellt.

12. Sobald die Installation fertiggestellt ist, wird eine Eingabeaufforderung angezeigt, in der Sie gefragt werden, ob Sie die hilfsprogrammdatenbank auf einer anderen Instanz von installieren möchten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , wählen Sie **Ja** aus, und klicken Sie dann auf **weiter**, um den Assistenten zu beenden, und **Wählen Sie** dann **Beenden**

13. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]Führen Sie in oder mit dem `sqlcmd` Hilfsprogramm das folgende Skript aus, um CLR zu aktivieren:

    ```sql
    sp_configure 'clr enabled', 1
    GO
    RECONFIGURE
    GO
    ```

    Wenn CLR nicht aktiviert ist, erhalten Sie den folgenden Fehler, wenn SSMA eine Verbindung mit herstellt [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :

    > SSMA konnte die Versionsinformationen der Erweiterungspaket-Assembly nicht abrufen. Installieren Sie das Erweiterungspaket auf dem Datenbankserver erneut.

### <a name="sql-server-database-objects"></a>Datenbankobjekte SQL Server

Nachdem Sie das Erweiterungspaket installiert haben, wird eine **ssma_oracle. bcp _migration_packages** -Tabelle in der **sysdb** -Datenbank angezeigt.

Jedes Mal, wenn Sie Daten zu migrieren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , erstellt SSMA einen- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent-Auftrag. Diese Aufträge werden **ssma_oracle Daten Migrationspaket {GUID}** benannt und sind im [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Knoten Agent von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] im Ordner Aufträge sichtbar.

Außerdem werden der **Master** -Datenbank die folgenden erweiterten gespeicherten Prozeduren hinzugefügt:

- `xp_ora2ms_exec2`
- `xp_ora2ms_exec2_ex`
- `xp_ora2ms_versioninfo2`

## <a name="see-also"></a>Weitere Informationen

- [Installieren von SSMA für den Oracle-Client](../../ssma/oracle/installing-ssma-for-oracle-client-oracletosql.md)
- [Migrieren von Oracle-Datenbanken zu SQL Server](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)
