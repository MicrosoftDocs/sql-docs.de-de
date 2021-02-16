---
description: Task 'Index neu organisieren' (Wartungsplan)
title: Task „Index neu organisieren“ (Wartungsplan) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/21/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.swb.maint.defrag.f1
helpviewer_keywords:
- Reorganize Index Task dialog box
ms.assetid: e9cbebbd-f36f-4176-9832-382a46ac946c
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 19a8a14383b11d97f2cc598c0b51159bc4c96622
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351170"
---
# <a name="reorganize-index-task-maintenance-plan"></a>Task 'Index neu organisieren' (Wartungsplan)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Verschieben Sie mithilfe des Dialogfelds **Task 'Index neu organisieren'** Indexseiten, sodass eine effizientere Suchreihenfolge entsteht. Dieser Task verwendet die `ALTER INDEX REORGANIZE` -Anweisung mit [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] -Datenbanken.  
  
## <a name="options"></a>Tastatur  
 **Connection**  
 Wählen Sie die Serververbindung aus, die bei der Ausführung dieses Tasks verwendet werden soll.  
  
 **Neu**  
 Erstellen Sie eine neue Serververbindung, die bei der Ausführung dieses Tasks verwendet werden soll. Das Dialogfeld **Neue Verbindung** wird im Folgenden beschrieben.  
  
 **Datenbanken**  
 Gibt die Datenbanken an, die von dieser Aufgabe betroffen sind.  
  
-   **Alle Datenbanken**  
  
     Generiert einen Wartungsplan, der Wartungstasks für alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbanken außer tempdb ausführt.  
  
-   **Alle Systemdatenbanken**  
  
     Generiert einen Wartungsplan, der Wartungstasks für alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Systemdatenbanken außer **tempdb** ausführt. Für benutzerdefinierte Datenbanken werden keine Wartungstasks ausgeführt.  
  
-   **Alle Benutzerdatenbanken**  
  
     Generiert einen Wartungsplan, der Wartungstasks für alle benutzerdefinierten Datenbanken ausführt. Für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Systemdatenbanken werden keine Wartungstasks ausgeführt.  
  
-   **Diese Datenbanken**  
  
     Generiert einen Wartungsplan, der Wartungstasks nur für die ausgewählten Datenbanken ausführt. Wenn diese Option ausgewählt wird, muss mindestens eine Datenbank in der Liste ausgewählt werden.  
  
 **Object**  
 Begrenzt das Raster **Auswahl** auf die Anzeige von Tabellen, Sichten oder beides.  
  
 **Auswahl**  
 Gibt die Tabellen oder Indizes an, auf die sich dieser Task auswirkt. Nicht verfügbar, wenn im Feld **Objekt** der Eintrag **Tabellen und Sichten** ausgewählt ist.  
  
 **Große Objekte komprimieren**  
 Hebt die Speicherplatzzuordnung für Tabellen und Sichten nach Möglichkeit auf. Diese Option verwendet `ALTER INDEX LOB_COMPACTION = ON`.  


[!INCLUDE[index-stats-options-reorg-5589131-2999104](../../includes/paragraph-content/index-stats-options-reorganize-maintenance-plan-include.md)]

  
 **T-SQL anzeigen**  
 Zeigt die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen an, die für diesen Task auf dem Server auf Basis der ausgewählten Optionen ausgeführt werden.  
  
> [!NOTE]  
>  Wenn die Anzahl der betroffenen Objekte groß ist, kann die Anzeige erhebliche Zeit in Anspruch nehmen.  

  
## <a name="new-connection-dialog-box"></a>Neue Verbindung (Dialogfeld)  
 **Verbindungsname**  
 Geben Sie einen Namen für die neue Verbindung ein.  
  
 **Wählen Sie einen Servernamen aus, oder geben Sie ihn ein.**  
 Wählen Sie den Server aus, zu dem bei der Ausführung dieses Tasks eine Verbindung hergestellt werden soll.  
  
 **Aktualisieren**  
 Mithilfe dieser Option aktualisieren Sie die Liste der verfügbaren Server.  
  
 **Geben Sie Informationen zum Anmelden am Server ein**  
 Legt fest, wie die Authentifizierung gegenüber dem Server stattfindet.  
  
 **Integrierte Sicherheit von Windows NT verwenden**  
 Stellt mithilfe der [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows-Authentifizierung eine Verbindung mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] her.  
  
 **Bestimmten Benutzernamen und bestimmtes Kennwort verwenden**  
 Stellt mithilfe der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Authentifizierung eine Verbindung zu einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] her. Diese Option ist nicht verfügbar.  
  
 **Benutzername**  
 Stellt eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldung für den Gebrauch bei der Authentifizierung bereit. Diese Option ist nicht verfügbar.  
  
 **Kennwort**  
 Stellt ein Kennwort für den Gebrauch bei der Authentifizierung bereit. Diese Option ist nicht verfügbar.  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [DBCC INDEXDEFRAG &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-indexdefrag-transact-sql.md)  
  
  
