---
title: Implementieren von Assemblys Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit in SQL Server gehosteten Assemblys arbeiten, einschließlich Informationen zum Erstellen/Ändern von Assemblys, löschen oder Aktivieren/Deaktivieren von Assemblys und Verwalten von Versionen
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- assemblies [CLR integration], implementing
ms.assetid: c228d7bf-a906-4f37-a057-5d464d962ff8
author: rothja
ms.author: jroth
ms.openlocfilehash: 8d97ef8c7dfc617cb6cd56dbcc6d83e0540051d7
ms.sourcegitcommit: da88320c474c1c9124574f90d549c50ee3387b4c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2020
ms.locfileid: "85695363"
---
# <a name="assemblies---implementing"></a>Assemblys: Implementierung
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Dieser Abschnitt enthält Informationen zu folgenden Themen, die Sie hilfreich beim Implementieren und Arbeiten mit Assemblys in der Datenbank sind:  
  
-   Erstellen von Assemblys  
  
-   Ändern von Assemblys  
  
-   Löschen, deaktivieren und Aktivieren von Assemblys  
  
-   Verwalten von Assembly-Versionen  
  
## <a name="creating-assemblies"></a>Erstellen von Assemblys  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden Assemblys mit der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung CREATE ASSEMBLY erstellt. In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] mit dem Assembly-Editorassistenten. Außerdem wird beim Bereitstellen eines SQL Server Projekts in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] eine Assembly in der Datenbank registriert, die für das Projekt angegeben wurde. Weitere Informationen finden Sie unter [Deploying CLR Database Objects](../../relational-databases/clr-integration/deploying-clr-database-objects.md).  
  
 **Erstellen einer Assembly mit Transact-SQL**  
  
-   [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)  
  
 **Erstellen einer Assembly mit SQL Server Management Studio**  
  
-   [Assemblyeigenschaften &#40;Seite "Allgemein"&#41;](../../relational-databases/clr-integration/assemblies-properties.md)  
  
## <a name="modifying-assemblies"></a>Assemblys ändern  
 In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden Assemblys mit der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung ALTER ASSEMBLY geändert. In [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] mit dem Assembly Assisted Editor. Eine Assembly können Sie wie folgt ändern:  
  
-   Ändern der Implementierung der Assembly durch Hochladen einer neueren Version der Binärdateien der Assembly. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Verwalten](#_managing) von Assemblyversionen.  
  
-   Ändern des Berechtigungssatzes der Assembly. Weitere Informationen finden Sie unter [Entwerfen von Assemblys](../../relational-databases/clr-integration/assemblies-designing.md).  
  
-   Ändern der Sichtbarkeit der Assembly. Sichtbare Assemblys sind in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für Verweise verfügbar. Nicht sichtbare Assemblys sind nicht verfügbar, dies gilt auch dann, wenn sie in die Datenbank hochgeladen wurden. Assemblys, die in eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hochgeladen wurden, sind standardmäßig sichtbar.  
  
-   Hinzufügen oder Löschen einer der Assembly zugeordneten Debug- oder Quelldatei.  
  
 **Ändern einer Assembly mit Transact-SQL**  
  
-   [ALTER ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-assembly-transact-sql.md)  
  
 **Ändern einer Assembly mit SQL Server Management Studio**  
  
-   [Assemblyeigenschaften &#40;Seite "Allgemein"&#41;](../../relational-databases/clr-integration/assemblies-properties.md)  
  
## <a name="dropping-disabling-and-enabling-assemblies"></a>Löschen, Deaktivieren und Aktivieren von Assemblys  
 Assemblys werden mit der [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung DROP ASSEMBLY oder mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] gelöscht.  
  
 **Löschen einer Assembly mit Transact-SQL**  
  
-   [DROP ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-assembly-transact-sql.md)  
  
 **Löschen einer Assembly mit SQL Server Management Studio**  
  
-   [Objekte löschen](../../ssms/object/delete-objects.md)  
  
 Standardmäßig ist die Ausführung aller in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erstellten Assemblys deaktiviert. Sie können die Option **CLR-fähig** der gespeicherten System Prozedur **sp_configure** verwenden, um die Ausführung aller Assemblys zu deaktivieren oder zu aktivieren, die in hochgeladen werden [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Die Deaktivierung der Assemblyausführung verhindert die Ausführung von CLR-Funktionen (Common Language Runtime), gespeicherter Prozeduren, Trigger, Aggregate und benutzerdefinierter Typen – gerade ausgeführte werden beendet. Die Deaktivierung der Assemblyausführung deaktiviert nicht die Möglichkeit, Assemblys zu erstellen, zu ändern oder zu löschen. Weitere Informationen finden Sie unter [CLR-fähig (Server Konfigurations Option](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md)).  
  
 **Deaktivieren und Aktivieren der Assemblyausführung**  
  
-   [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
##  <a name="managing-assembly-versions"></a><a name="_managing"></a>Verwalten von Assembly-Versionen  
 Wenn eine Assembly in eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz hochgeladen wird, wird es in den Datenbanksystemkatalogen gespeichert und verwaltet. Alle Änderungen, die an der Definition der-Assembly in vorgenommen [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] werden, sollten an die Assembly weitergegeben werden, die im Daten Bank Katalog gespeichert ist.  
  
 Zum Ändern einer Assembly müssen Sie eine ALTER ASSEMBLY-Anweisung ausgeben, die die Assembly in der Datenbank aktualisiert. Dadurch wird die Assembly mit der aktuellsten Kopie der [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]-Module aktualisiert, die ihre Implementierung enthalten.  
  
 Mit der WITH UNCHECKED DATA-Klausel der ALTER ASSEMBLY-Anweisung wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] angewiesen, auch die Assemblys zu aktualisieren, von denen in der Datenbank persistente Daten abhängen. WITH UNCHECKED DATA müssen Sie insbesondere dann angeben, wenn folgende Bedingungen erfüllt sind:  
  
-   Persistente berechnete Spalten, die über [!INCLUDE[tsql](../../includes/tsql-md.md)]-Funktionen oder -Methoden entweder direkt oder indirekt auf Methoden der Assembly verweisen.  
  
-   Spalten eines CLR-benutzerdefinierten Typs, die von der Assembly abhängen, wobei der Typ ein Serialisierungsformat vom Typ **UserDefined** (nicht **Native**) implementiert.  
  
> [!CAUTION]  
>  Wird WITH UNCHECKED DATA nicht angegeben, versucht [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], die Ausführung von ALTER ASSEMBLY zu verhindern, wenn die neue Assemblyversion Auswirkungen auf vorhandene Daten in Tabellen, Indizes oder anderen persistenten Sites hat. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] garantiert jedoch nicht, dass berechnete Spalten, Indizes, indizierte Sichten oder Ausdrücke mit den zugrunde liegenden Routinen und Typen konsistent sein werden, wenn die CLR-Assembly aktualisiert wird. Gehen Sie beim Ausführen von ALTER ASSEMBLY mit Vorsicht vor, um sicherzustellen, dass es keine fehlende Übereinstimmung zwischen dem Ergebnis eines Ausdrucks und einem auf diesem Ausdruck basierenden Wert, der in der Assembly gespeichert ist, gibt.  
  
 Nur Mitglieder der Daten Bank Rolle " **db_owner** " und " **db_ddlowner** " können die Ausführung von Alter Assembly mithilfe der WITH-Klausel "with nicht überprüft" ausführen.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sendet eine Meldung an das Windows-Anwendungsereignisprotokoll, die angibt, dass die Assembly mit ungeprüften Daten in den Tabellen geändert wurde. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] markiert anschließend alle Tabellen mit Daten, die von der Assembly abhängen, als ungeprüft. Die **has_unchecked_assembly_data** -Spalte der **sys. Tables** -Katalog Sicht enthält den Wert 1 für Tabellen, die nicht überprüfte Daten enthalten, und 0 für Tabellen ohne überprüfte Daten.  
  
 Um die Integrität der nicht überprüften Daten zu beheben, führen Sie DBCC CHECKDB mit EXTENDED_LOGICAL_CHECKS für jede Tabelle aus, die überprüfte Daten verfügt. Wenn DBCC CHECKDB mit EXTENDED_LOGICAL_CHECKS fehlschlägt, müssen Sie entweder die ungültigen Tabellenzeilen löschen oder den Assemblycode ändern, um Probleme zu beheben, und dann zusätzliche Alter Assembly-Anweisungen ausgeben.  
  
 Mit ALTER ASSEMBLY wird die Assemblyversion geändert. Die Kultur und das öffentliche Schlüssel Token der Assembly bleiben unverändert. In SQL Server ist es nicht zulässig, verschiedene Versionen einer Assembly mit demselben Namen, derselben Kultur und demselben öffentlichen Schlüssel zu registrieren.  
  
### <a name="interactions-with-computer-wide-policy-for-version-binding"></a>Interaktionen mit der für den gesamten Computer geltenden Richtlinie zur Versionsbindung  
 Wenn in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gespeicherte Verweise auf Assemblys mit der Herausgeberrichtlinie oder einer für den gesamten Computer geltenden Administratorrichtlinie an spezifische Versionen umgeleitet werden, müssen Sie eine der folgenden Maßnahmen ergreifen:  
  
-   Stellen Sie sicher, dass sich die neue Version, auf die umgeleitet wird, in der Datenbank befindet.  
  
-   Ändern Sie alle Anweisungen der externen Richtliniendatei des Computers oder der Herausgeberrichtlinie, damit diese auf die in der Datenbank vorhandene Version verweisen.  
  
 Anderenfalls verursacht das Laden einer neuen Assemblyversion in die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz einen Fehler.  
  
 **Aktualisieren der Version einer Assembly**  
  
-   [ALTER ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-assembly-transact-sql.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Assemblys &#40;Datenbank-Engine&#41;](../../relational-databases/clr-integration/assemblies-database-engine.md)   
 [Abrufen von Informationen zu Assemblys](../../relational-databases/clr-integration/assemblies-getting-information.md)  
  
  
