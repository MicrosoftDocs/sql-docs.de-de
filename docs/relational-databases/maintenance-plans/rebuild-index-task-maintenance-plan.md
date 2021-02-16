---
title: Task 'Index neu erstellen' (Wartungsplan) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie mithilfe des Tasks „Index neu erstellen“ die Indizes für die Tabellen in einer SQL Server-Datenbank-Instanz mit einem neuen Füllfaktor neu erstellen.
ms.custom: ''
ms.date: 07/21/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- reindex
- sql13.swb.maint.reindex.f1
helpviewer_keywords:
- Rebuild Index Task dialog box
ms.assetid: 33e2940b-139f-4563-b0cb-5683f08bd879
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 6c95f60259fb25b457e4349b6e0798ce4fa0e6fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351182"
---
# <a name="rebuild-index-task-maintenance-plan"></a>Task 'Index neu erstellen' (Wartungsplan)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Mithilfe des Dialogfelds **Task „Index neu erstellen“** können Sie Indizes für Tabellen in der Datenbank mit einem neuen Füllfaktor neu erstellen. Der Füllfaktor bestimmt die Menge an leeren Speicherplatz auf jeder Seite im Index, der Platz für zukünftige Erweiterungen bieten soll. Wenn der Tabelle Daten hinzugefügt werden, wird der freie Speicherplatz aufgefüllt, da der Wert für den Füllfaktor nicht beibehalten wird. Der freie Speicherplatz kann durch Neuorganisieren der Daten- und Indexseiten wiederhergestellt werden.  
  
 **Task „Index neu erstellen“** verwendet die ALTER INDEX-Anweisung. Weitere Informationen zu den auf dieser Seite beschriebenen Optionen finden Sie unter [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
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
  
     Generiert einen Wartungsplan, der Wartungstasks für alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Systemdatenbanken außer tempdb ausführt. Für benutzerdefinierte Datenbanken werden keine Wartungstasks ausgeführt.  
  
-   **Alle Benutzerdatenbanken**  
  
     Generiert einen Wartungsplan, der Wartungstasks für alle benutzerdefinierten Datenbanken ausführt. Für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Systemdatenbanken werden keine Wartungstasks ausgeführt.  
  
-   **Diese Datenbanken**  
  
     Generiert einen Wartungsplan, der Wartungstasks nur für die ausgewählten Datenbanken ausführt. Wenn diese Option ausgewählt wird, muss mindestens eine Datenbank in der Liste ausgewählt werden.  
  
    > [!NOTE]  
    >  Wartungspläne werden nur für Datenbanken mit Kompatibilitätsgrad 80 oder höher ausgeführt. Datenbanken mit Kompatibilitätsgrad 70 oder niedriger werden nicht angezeigt.  
  
 **Object**  
 Begrenzt das Raster **Auswahl** auf die Anzeige von Tabellen, Sichten oder beides.  
  
 **Auswahl**  
 Gibt die Tabellen oder Indizes an, auf die sich dieser Task auswirkt. Nicht verfügbar, wenn im Objektfeld der Eintrag **Tabellen und Sichten** ausgewählt ist.  
  
 **Freier Standardspeicherplatz pro Seite**  
 Löscht die Indizes für die Tabellen in der Datenbank und erstellt sie mit dem Füllfaktor, der beim Erstellen der Indizes angegeben wurde, neu.  
  
 **Freien Speicherplatz pro Seite ändern in**  
 Löscht die Indizes für die Tabellen in der Datenbank und erstellt sie mit einem neuen, automatisch berechneten Füllfaktor neu. Auf diese Weise wird der angegebene freie Speicherplatz auf den Indexseiten reserviert. Ein höherer Prozentsatz bedeutet, dass mehr freier Speicherplatz auf den Indexseiten reserviert wird und der Index entsprechend wachsen kann. Die gültigen Werte sind 0 bis 100.  
  
 **Ergebnisse in 'tempdb' sortieren**  
 Legen Sie mithilfe der Option `SORT_IN_TEMPDB` fest, wo die während der Indexerstellung generierten Zwischenergebnisse des Sortiervorgangs temporär gespeichert werden. Wenn ein Sortiervorgang nicht erforderlich ist oder im Arbeitsspeicher ausgeführt werden kann, wird die Option `SORT_IN_TEMPDB`ignoriert.  
  
 **Index mit Leerstellen auffüllen**  
 Gibt die Auffüllung von Indizes an.  
  
 **Index online**  
 Die Option `ONLINE` ermöglicht es Benutzern, während Indexvorgängen auf die zugrunde liegenden Tabellen- bzw. gruppierten Indexdaten und alle zugehörigen nicht gruppierten Indizes zuzugreifen.  
  
> [!NOTE]
>  Onlineindexvorgänge sind nicht in jeder Edition von [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verfügbar. Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Von den SQL Server 2016-Editionen unterstützte Funktionen](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 **Indizes nicht neu erstellen | Indizes offline neu erstellen**  
 Legt fest, wie Indextypen behandelt werden, die nicht neu erstellt werden können, solange sie online sind.  
  
 **MAXDOP**  
 Begrenzt die Anzahl der Prozessoren, die bei der Ausführung paralleler Pläne verwendet werden.  
  
 **Mit niedriger Priorität**  
 Wählen Sie diese Option aus, wenn auf Sperren mit niedriger Priorität gewartet werden soll.  
  
 **Nach Wartezeit abbrechen**  
 Geben Sie hier an, was nach Ablauf von **Max. Dauer** geschehen soll.  
  
 **Max. Dauer**  
 Geben Sie hier an, wie lange auf Sperren mit niedriger Priorität gewartet werden soll.  
  
 **T-SQL anzeigen**  
 Zeigt die [!INCLUDE[tsql](../../includes/tsql-md.md)] -Anweisungen an, die für diesen Task auf dem Server auf Basis der ausgewählten Optionen ausgeführt werden.  
  
> [!NOTE]  
>  Wenn die Anzahl der betroffenen Objekte groß ist, kann die Anzeige erhebliche Zeit in Anspruch nehmen.  


[!INCLUDE[index-stats-options-reorg-5589131-2999104](../../includes/paragraph-content/index-stats-options-reorganize-maintenance-plan-include.md)]

  
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
 Stellt mithilfe der Windows-Authentifizierung eine Verbindung mit einer Instanz von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] her.  
  
 **Bestimmten Benutzernamen und bestimmtes Kennwort verwenden**  
 Stellt mithilfe der [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] -Authentifizierung eine Verbindung zu einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] her. Diese Option ist nicht verfügbar.  
  
 **Benutzername**  
 Stellt eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldung für den Gebrauch bei der Authentifizierung bereit. Diese Option ist nicht verfügbar.  
  
 **Kennwort**  
 Stellt ein Kennwort für den Gebrauch bei der Authentifizierung bereit. Diese Option ist nicht verfügbar.  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [DBCC DBREINDEX &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [SORT_IN_TEMPDB-Option für Indizes](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md)   
 [Richtlinien für Onlineindexvorgänge](../../relational-databases/indexes/guidelines-for-online-index-operations.md)   
 [Funktionsweise von Onlineindexvorgängen](../../relational-databases/indexes/how-online-index-operations-work.md)   
 [Ausführen von Onlineindexvorgängen](../../relational-databases/indexes/perform-index-operations-online.md)  
  
  
