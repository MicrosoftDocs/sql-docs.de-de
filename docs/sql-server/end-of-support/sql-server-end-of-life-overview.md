---
title: Optionen nach Ende des Supports
description: Erfahren Sie mehr über die verschiedenen Optionen für SQL Server-Produkte, die das Ende des Supports erreicht haben, z. B. SQL Server 2005, SQL Server 2008 und SQL Server 2008 R2.
ms.date: 08/12/2020
ms.prod: sql
ms.technology: install
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.reviewer: pmasl
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c7560fbda04ef5a14c73ead8a7bbc8337e3e38ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352902"
---
# <a name="sql-server-end-of-support-options"></a>Optionen für SQL Server bei Ende des Supports 
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

In diesem Artikel werden die Optionen für SQL Server-Produkte erläutert, die das Ende des Supports erreicht haben.

## <a name="understanding-the-sql-server-lifecycle"></a>Grundlegendes zum SQL Server-Lebenszyklus

Für jede Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird mindestens 10 Jahre Support geboten, davon fünf Jahre grundlegender Support und fünf Jahre erweiterter Support:
-  Der **grundlegende Support** umfasst Updates hinsichtlich Funktionalität, Leistung, Skalierbarkeit und Sicherheit. 
-  Der **erweiterte Support** umfasst nur Sicherheitsupdates. 

**Das Ende des Supports** (auch als Ende der Lebensdauer bezeichnet) bedeutet, dass ein Produkt das Ende seines Lebenszyklus erreicht hat, weshalb Wartung und Support für das Produkt nicht mehr zur Verfügung stehen. Weitere Informationen zum Microsoft-Lebenszyklus finden Sie auf der Seite [Microsoft Lifecycle-Richtlinie](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy).



## <a name="options"></a>Tastatur

Wenn Ihre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz das Ende der Supportphase erreicht hat, haben Sie folgende Optionen:
- Upgrade auf eine aktuelle Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
- Erwerben eines [Abonnements für erweiterte Sicherheitsupdates](https://www.microsoft.com/cloud-platform/extended-security-updates) 
- Migrieren Ihrer Workload im Istzustand auf eine Azure-VM, um [erweiterte Sicherheitsupdates kostenlos](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support) zu erhalten
- Migrieren Ihrer Workload in einen [Azure SQL-Datenbank-Dienst](/azure/sql-database/sql-database-paas-vs-sql-server-iaas) 

Weitere Informationen, Anleitungen und Tools zum Planen und Automatisieren Ihres Upgrades oder Ihrer Migration finden Sie unter [Ende des Supports für SQL Server 2005](https://www.microsoft.com/sql-server/sql-server-2005) und [Ende des Supports für SQL Server 2008](https://www.microsoft.com/cloud-platform/windows-sql-server-2008).  

![Optionen nach Ende des Supports](media/sql-server-end-of-life-overview/sql-server-upgrade-paths.png)

In diesem Artikel werden die Vorteile und Aspekte jedes Ansatzes sowie zusätzliche Ressourcen beschrieben, die Ihnen bei der Entscheidungsfindung helfen können.

## <a name="upgrade-sql-server"></a>Aktualisieren von SQL Server

Wenn Ihre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz das Ende der Supportphase erreicht hat, können Sie ein Upgrade auf eine neuere mit Support versehene Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vornehmen. Dadurch können Sie in einer einheitlichen Umgebung arbeiten, das neueste Funktionsangebot nutzen und den Supportlebenszyklus der neuen Version übernehmen.

### <a name="benefits"></a>Vorteile
- **Neueste Technologie**: Neue Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bieten Neuerungen hinsichtlich Leistung, Skalierbarkeit und Hochverfügbarkeit sowie mehr Sicherheit. 
- **Kontrolle**: Sie haben die bestmögliche Kontrolle über Features und Skalierbarkeit, da Sie sowohl Hardware als auch Software selbst verwalten.
- **Vertraute Umgebung**: Wenn Sie für eine ältere Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ein Upgrade ausführen, ist dies die Umgebung mit den meisten Gemeinsamkeiten.
- **Breite Anwendbarkeit**: Anwendbar für Datenbankanwendungen beliebiger Art, einschließlich OLTP-Systeme und Data Warehousing.
- **Niedriges Risiko für Datenbankanwendungen**: Indem die Datenbankkompatibilität auf dem gleichen Grad wie beim Vorgängersystem bleibt, werden bestehende Datenbankanwendungen vor Funktions- und Leistungsänderungen mit ggf. nachteiligen Auswirkungen geschützt. Eine Anwendung muss nur dann vollständig neu zertifiziert werden, wenn sie Funktionen verwenden muss, die durch eine neuere Datenbank-Kompatibilitätseinstellung vorgegeben sind. Weitere Informationen finden Sie unter [Kompatibilitätszertifizierung](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Überlegungen

- **Kosten**: Dieser Ansatz erfordert die höchsten Anlaufinvestitionen und die umfangreichste laufende Verwaltung. Sie müssen Ihre eigene Hardware und Software kaufen, warten und verwalten.
- **Ausfallzeit**: Je nach Upgradestrategie können Ausfallzeiten auftreten. Es besteht auch ein inhärentes Risiko, dass es während eines direkten Upgradeprozesses zu Problemen kommt.
- **Komplexität**: Wenn Sie mit Windows Server 2008 oder [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)] arbeiten, müssen Sie auch ein Upgrade des Betriebssystems durchführen, da die neueren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] von diesen Windows-Versionen möglicherweise nicht unterstützt werden. Während des Upgradeprozesses des Betriebssystems besteht ein zusätzliches Risiko, sodass die Durchführung einer parallelen Migration zwar der geeignetere, aber auch kostspieligere Ansatz sein kann. Direkte Betriebssystemupgrades werden in Failoverclusterinstanzen für Windows Server 2008 oder [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)] nicht unterstützt. 

  > [!NOTE]
  > Parallele Upgrades des Clusterbetriebssystems sind ab Windows Server 2016 möglich.

### <a name="resources"></a>Ressourcen

[Installationsmedien](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-rtm)   
[Upgrade von SQL Server mithilfe des Installations-Assistenten](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)

Neuerungen in:
- [SQL Server 2016](../what-s-new-in-sql-server-2016.md)
- [SQL Server 2017](../what-s-new-in-sql-server-2017.md) 
- [SQL Server 2019](../what-s-new-in-sql-server-ver15.md)   

Hardwareanforderungen:
- [SQL Server 2017 und früher](../install/hardware-and-software-requirements-for-installing-sql-server.md)  
- [SQL Server 2019](../install/hardware-and-software-requirements-for-installing-sql-server-ver15.md)    

Unterstützte Versions- und Editionsupgrades:
- [SQL Server 2016](../../database-engine/install-windows/supported-version-and-edition-upgrades.md?view=sql-server-2016&preserve-view=true) 
- [SQL Server 2017](../../database-engine/install-windows/supported-version-and-edition-upgrades-2017.md)
- [SQL Server 2019](../../database-engine/install-windows/supported-version-and-edition-upgrades-version-15.md)

Tools:
-  Der [Assistent für Datenbankexperimente](../../dea/database-experimentation-assistant-overview.md) kann helfen, die Zielversion von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für eine bestimmte Workload zu ermitteln. 
-  Der [Datenmigrations-Assistent](../../dma/dma-overview.md) kann helfen, Kompatibilitätsprobleme zu erkennen, die sich auf die Datenbankfunktionalität in Ihrer neuen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auswirken können. 
-  Der [Abfrageoptimierungs-Assistent](../../relational-databases/performance/upgrade-dbcompat-using-qta.md) kann bei der Optimierung von Workloads helfen, bei denen beim Upgrade der Datenbankkompatibilität nachteilige Auswirkungen auftreten können.

Die folgende Abbildung zeigt ein Beispiel für die Innovation in den verschiedenen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] im Laufe der Jahre: 

![25 Jahre Innovation für SQL Server](media/sql-server-end-of-life-overview/sql-server-version-improvements.png)

## <a name="extend-support"></a>Verlängern des Supports 

Wenn Sie noch nicht für ein Upgrade und den Umstieg auf die Cloud bereit sind, können Sie ein Abonnement für erweiterte Sicherheitsupdates erwerben, um bis zu drei Jahre nach dem Enddatum des Supports **kritische** Sicherheitsupdates zu erhalten.  

### <a name="benefits"></a>Vorteile 

- **Anwendungsunterstützung**: Dies ist die beste Option, wenn Ihre Anwendung eine Neuzertifizierung für eine neuere Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erfordert. Dies ist üblich bei Anwendungen ohne [Kompatibilitätszertifizierung](../../database-engine/install-windows/compatibility-certification.md). 
- **Konsistente Infrastruktur**: Sie müssen Ihre Infrastruktur in keiner Weise ändern. 
- **Technischer Support**: Wenn Sie Software Assurance oder einen anderen Supportplan haben, können Sie weiterhin technischen Support von [!INCLUDE[msCoName](../../includes/msconame-md.md)] für Ihr [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Produkt mit beendetem Support erhalten. Dies ist die einzige Möglichkeit, Support für [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] und [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] zu erhalten. 
- **Time**: Diese Option gilt drei Jahre und bietet Ihnen zusätzliche Zeit für die Zertifizierung Ihrer Anwendungen. 

### <a name="considerations"></a>Überlegungen 

- **Eingeschränkte Verfügbarkeit**: Diese Option ist nur für Kunden mit Software Assurance oder Abonnementlizenzen verfügbar. 
- **Kosten**: Diese Option kann sich als kostspielig erweisen, da die erweiterten Sicherheitsupdates ca. 75 % der jährlichen lokalen Lizenzkosten ausmachen.
- **Begrenzter Zeitrahmen**: Diese Option ist nur drei Jahre verfügbar, sodass Sie am Ende dieses Zeitraums dennoch ein Upgrade oder eine Migration vornehmen müssen, wenn Sie Sicherheit und Konformität gewährleisten möchten.
- **Keine Programmfehlerbehebungen**: Wenn Sie im Produkt auf einen nicht sicherheitsbezogenen Fehler stoßen, stellt [!INCLUDE[msCoName](../../includes/msconame-md.md)] keine Behebung dafür bereit. 
- **Eingeschränkter Support**: Erweiterte Sicherheitsupdates bieten keine neuen Funktionen, funktionellen Verbesserungen oder von Kunden gewünschte Korrekturen. Sicherheitsfixes sind auf diejenigen beschränkt, die vom [Microsoft Security Response Center (MSRC)](https://portal.msrc.microsoft.com/) als „Kritisch“ eingestuft werden.

### <a name="resources"></a>Ressourcen

[Übersicht über erweiterte Sicherheitsupdates (ESUs)](sql-server-extended-security-updates.md)       
[Häufig gestellte detaillierte Fragen zu ESUs](https://www.microsoft.com/cloud-platform/extended-security-updates)       
[Kostenloses Verlängern des Supports durch Migration im Istzustand zu einer Azure VM](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)       
[Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default)       

## <a name="azure-virtual-machine"></a>Virtueller Azure-Computer

Eine weitere Option ist Migrieren Ihrer Workload zu einer [Azure-VM mit ausgeführtem SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview). Sie können Ihr System im Istzustand migrieren und Ihre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz mit beendetem Support beibehalten, oder Sie können ein Upgrade auf eine neuere Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] durchführen. Am besten geeignet für Migrationen und Anwendungen, die Zugriff auf Betriebssystemebene erfordern. Virtuelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Computer sind Lift & Shift-fähig für vorhandene Anwendungen, die eine schnelle Migration in die Cloud mit minimalen oder ohne Änderungen erfordern. 

### <a name="benefits"></a>Vorteile

- **Kostenlose erweiterte Sicherheitsupdates**: Wenn Sie sich dafür entscheiden, Ihre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz unter Verwendung von [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] oder [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] im Istzustand zu belassen, können Sie nach Ende des Supports drei Jahre erweiterte Sicherheitsupdates kostenlos erhalten, selbst ohne Software Assurance. 
- **Kostenersparnis**: Sie sparen die Kosten für Hardware und Serversoftware und zahlen nur für die stündliche Nutzung. 
- **Per Lift & Shift migrieren**: Sie können Ihre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]- und Anwendungsinfrastruktur bei minimalen oder ohne Änderungen per Lift & Shift in die Cloud migrieren. 
- **Gehostete Umgebung**: Sie kommen in den Genuss der Vorteile einer gehosteten Umgebung, wie z. B. Auslagerung von Hardware und Wartung von Software. 
- **Automatisierung**: Wenn Sie [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)] und höher einsetzen, profitieren Sie vom automatisierten Patchen und automatisierten Sicherungen. 
- **Kontrolle über das Betriebssystem**: Sie haben die Kontrolle über die Betriebssystemumgebung, allerdings mit dem bekannten Funktionsumfang von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Schnelle Bereitstellung**: Die Bereitstellung kann aus einer Bibliothek mit Images virtueller Computer schnell erfolgen. 
- **Lizenzmobilität**: Sie können Ihre Lizenz einbringen, wodurch Sie Betriebskosten senken können. 
- **Hochverfügbarkeit**: Sie profitieren nicht nur von der integrierten Verfügbarkeit virtueller Computer in der Azure-Infrastruktur mit einer Verfügbarkeit von bis zu 99,99 %, sondern können auch von Hochverfügbarkeitsoptionen für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wie Failoverclusterinstanzen und Always On-Verfügbarkeitsgruppen profitieren. 
- **Niedriges Risiko für Datenbankanwendungen**: Indem die Datenbankkompatibilität auf dem gleichen Grad wie bei den Datenbanken des Vorgängersystems bleibt, sind bestehende Datenbankanwendungen vor Funktions- und Leistungsänderungen mit ggf. nachteiligen Auswirkungen geschützt. Eine Anwendung muss nur dann vollständig neu zertifiziert werden, wenn sie Funktionen verwenden muss, die durch eine neuere Datenbank-Kompatibilitätseinstellung vorgegeben sind. Weitere Informationen finden Sie unter [Kompatibilitätszertifizierung](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Überlegungen

- **Verwaltbarkeit:** Sie müssen weiterhin sowohl [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als auch die Betriebssystemsoftware verwalten. 
- **Netzwerk**: Sie müssen die VM so konfigurieren, dass sie in Ihre Netzwerk- und Active Directory-Infrastruktur integriert werden kann, was zusätzliche Komplexität bedeutet. 
- **Failoverclusterinstanz mit freigegebenem Speicher**: Virtuelle Azure-Computer unterstützen Failoverclusterinstanzen nur mithilfe des Features „Direkte Speicherplätze“ oder von Premium-Dateifreigaben. Failoverclusterinstanzen mit freigegebenem Speicher werden nicht unterstützt. Daher unterstützen virtuelle Azure-Computer Failoverclusterinstanzen nur unter Windows Server 2012 oder höher.
- **Aus Skalierbarkeit bezogene Ausfallzeiten**: Während der Änderung von CPU- und Speicherressourcen treten Ausfallzeiten auf. 
- **Größenbeschränkung**: Obwohl die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz so viele Datenbanken wie nötig unterstützen kann, beträgt die kumulative Gesamtgröße aller Datenbanken bei einer einzelnen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz 256 TB, im Gegensatz zu 524 PB bei einer lokalen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz. 

### <a name="resources"></a>Ressourcen

[Übersicht über SQL Server-VMs](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)       
[Wählen einer Azure SQL-Option](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)       
[Migrieren von SQL Server zu einer Azure-VM](/azure/virtual-machines/windows/sql/virtual-machines-windows-migrate-sql)       
[Kostenlose erweiterte Sicherheitsupdates bei Migration im Istzustand zu Azure](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)       
[Übersicht über erweiterte Sicherheitsupdates (ESUs)](sql-server-extended-security-updates.md)       
[Häufig gestellte detaillierte Fragen zu ESUs](https://www.microsoft.com/cloud-platform/extended-security-updates)       
[Automatisches Patchen virtueller SQL-Computer](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)       
[Automatisches Sichern virtueller SQL-Computer](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup-v2)       
[Hochverfügbarkeit virtueller SQL-Computer](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr)       
[Häufig gestellte Fragen zu virtuellen SQL-Computern](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-faq)       

## <a name="azure-sql-database-single-database"></a>Azure SQL-Datenbank – Singleton

Wenn Sie die Wartung auslagern, Kosten senken und künftige Upgrades vermeiden möchten, können Sie Ihre Workload in eine [Azure SQL-Datenbank-Einzeldatenbank](/azure/sql-database/sql-database-single-database) verlagern. Diese Option ist ideal für moderne Cloudanwendungen, für die Sie die neuesten stabilen [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]-Funktionen nutzen möchten und bei denen zeitliche Einschränkungen in Entwicklung und Marketing gelten. 

### <a name="benefits"></a>Vorteile

- **Kosten**: Eine Einzeldatenbank kann kostengünstig sein, da Hardware, Software und Wartung ausgelagert werden und die Nutzung sekunden- oder stundenweise abgerechnet werden kann. 
- **Flexibilität:** Eine Einzeldatenbank eignet sich besonders gut für Cloudanwendungen, wenn Entwicklerproduktivität und die schnelle Markteinführung neuer Lösungen entscheidend sind oder externer Zugriff erforderlich ist.  
- **Allgemeine Features**: Die am häufigsten verwendeten [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]-Features sind verfügbar, aber nicht so viele wie bei SQL Managed Instance.  
- **Schnelle Bereitstellung**: Sie können eine Einzeldatenbank schnell bereitstellen. 
- **Skalierbarkeit:** Sie können je nach Bedarf für Ihr Unternehmen schnell und einfach eine Hoch- und Herunterskalierung vornehmen, was zusätzliche Kosteneinsparungen bringt. 
- **Verfügbarkeit:** Die Kosten des Diensts umfassen sowohl Speicher als auch Hochverfügbarkeit, wobei eine Verfügbarkeit von 99,995 % garantiert wird.  
- **Automatisierung**: Patches und Sicherungen werden automatisch ausgeführt, sodass Sie wertvolle Wartungszeit sparen können.  
- **Intelligent Insights**: Verschaffen Sie sich mithilfe der integrierten Informationsanalyse einen Einblick in die Leistung Ihrer Datenbank.  
- **Versionslos**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ist versionslos, was bedeutet, dass Sie stets die neueste Version nutzen und sich um Upgrades oder Ausfallzeiten keine Gedanken machen müssen. Außerdem sind Sie immer auf dem neuesten und besten Stand, da unsere neuesten stabilen Features zuerst in der Cloud veröffentlicht werden.
- **Niedriges Risiko für Datenbankanwendungen**: Indem die Datenbankkompatibilität auf dem gleichen Grad wie bei der lokalen Datenbank bleibt, werden bestehende Anwendungen vor Funktions- und Leistungsänderungen mit ggf. nachteiligen Auswirkungen geschützt. Eine Anwendung muss nur dann vollständig neu zertifiziert werden, wenn sie Funktionen verwenden muss, die durch eine neuere Datenbank-Kompatibilitätseinstellung vorgegeben sind. Weitere Informationen finden Sie unter [Kompatibilitätszertifizierung](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Überlegungen

- **Eingeschränkte Migrationsoptionen**:  Sie können immer nur eine Einzeldatenbank auf einmal migrieren, nicht aber eine ganze Instanz.   
- **Eingeschränkte Features**:  Obwohl die am häufigsten verwendeten Azure SQL-Datenbankfeatures verfügbar sind, ist der Umfang für eine Einzeldatenbank nicht so umfangreich wie bei Azure SQL Managed Instance oder bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Unterschiede bei Transact-SQL**:  Es gibt einige [!INCLUDE[tsql](../../includes/tsql-md.md)]-Unterschiede (T-SQL) zwischen einer Einzeldatenbank und einer lokalen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Größenbeschränkungen**:  Eine Einzeldatenbank hat eine maximale Datenbankgröße von 100 TB verglichen mit 524 PB für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Wartungszeit**: Es gibt keine Garantie für die genaue Wartungszeit, obwohl diese nahezu transparent ist. 

### <a name="resources"></a>Ressourcen

[Übersicht über Azure SQL-Datenbank](/azure/sql-database/sql-database-technical-overview)       
[Wählen einer Azure SQL-Option](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)       
[Azure SQL-Datenbank: Featurevergleich](/azure/sql-database/sql-database-features)       
[Migrieren von SQL Server zu einer Einzeldatenbank](/azure/sql-database/sql-database-single-database-migrate)       
[Umfassende Beschreibung des Migrationsprozesses](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)       
[T-SQL-Unterschiede bei Einzeldatenbanken](/azure/sql-database/sql-database-transact-sql-information)       
Ressourcenlimits für [virtuelle Kerne](/azure/sql-database/sql-database-vcore-resource-limits-single-databases) und [DTUs](/azure/sql-database/sql-database-dtu-resource-limits-single-databases)       
[Intelligent Insights](/azure/sql-database/sql-database-intelligent-insights)       

Tools:
- [Data Migration Assistant](../../dma/dma-overview.md)
- [Database Migration Service](/azure/dms/dms-overview)

## <a name="sql-managed-instance"></a>Verwaltete SQL-Instanz

Wenn Sie die Vorteile in Bezug auf die Auslagerung von Wartung und Kosten nutzen möchten, aber den Funktionsumfang einer [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]-Einzeldatenbank zu eingeschränkt finden, können Sie zu [SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) wechseln. Eine verwaltete Instanz ähnelt sehr einer lokalen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sie müssen sich allerdings nicht um Dinge wie Hardwareausfälle oder Patches Gedanken machen. Bei SQL Managed Instance handelt es sich um eine Sammlung von System- und Benutzerdatenbanken mit einer gemeinsam genutzten Ressourcengruppe, die sich per Lift & Shift migrieren lässt und für die meisten Migrationen in die Cloud genutzt werden kann. Diese Option ist ideal für neue Anwendungen oder vorhandene lokale Anwendungen, für die Sie die neuesten stabilen [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]-Features nutzen möchten und die bei minimalen Änderungen in die Cloud migriert werden sollen. 

### <a name="benefits"></a>Vorteile

- **Kosten**: Sie können Kosten sparen, indem Sie die Wartung von Software und Hardware auslagern.  
- **Per Lift & Shift migrieren**: Sie können Ihre gesamte lokale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz per Lift & Shift in eine verwaltete Instanz migrieren, einschließlich aller Datenbanken und mit minimalen oder sogar ohne Datenbankänderungen. 
- **Features**: Der Funktionsumfang einer verwalteten Instanz entspricht weitgehend dem einer lokalen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], z. B. datenbankübergreifende Abfragen, Veröffentlichung und Verteilung von Transaktionsreplikationen, SQL-Auftragsplanung und CLR-Unterstützung. 
- **Skalierbarkeit:** Alle Datenbanken innerhalb einer verwalteten Instanz teilen sich Ressourcen, und es ist jederzeit möglich, ohne Ausfallzeiten eine Hoch- und Herunterskalierung vorzunehmen.   
- **Automatisierung**: Patches und Sicherungen werden automatisch ausgeführt, sodass Sie wertvolle Wartungszeit einsparen können.  
- **Verfügbarkeit:** Die Kosten des Diensts umfassen sowohl Speicher als auch Hochverfügbarkeit, wobei eine Verfügbarkeit von 99,99 % garantiert wird.  
- **Intelligent Insights**: Verschaffen Sie sich mithilfe der integrierten Informationsanalyse einen Einblick in die Leistung Ihrer Datenbanken.  
- **Versionslos**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ist versionslos, was bedeutet, dass Sie stets die neueste Version nutzen und sich um Upgrades oder Ausfallzeiten keine Gedanken machen müssen. Außerdem sind Sie immer auf dem neuesten und besten Stand, da unsere neuesten stabilen Features zuerst in der Cloud veröffentlicht werden.
- **Niedriges Risiko für Datenbankanwendungen**: Indem die Datenbankkompatibilität auf dem gleichen Grad wie bei den lokalen Datenbanken bleibt, werden bestehende Anwendungen vor Funktions- und Leistungsänderungen mit ggf. nachteiligen Auswirkungen geschützt. Eine Anwendung muss nur dann vollständig neu zertifiziert werden, wenn sie Funktionen verwenden muss, die durch eine neuere Datenbank-Kompatibilitätseinstellung vorgegeben sind. Weitere Informationen finden Sie unter [Kompatibilitätszertifizierung](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Überlegungen

- **Kosten**: Die Option „Verwaltete Instanz“ kann teurer als die Option „Einzeldatenbank“ sein.  
- **Unterschiede bei Transact-SQL**: Es gibt einige [!INCLUDE[tsql](../../includes/tsql-md.md)]-Unterschiede (T-SQL) zwischen einer Einzeldatenbank und einer lokalen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
- **Bereitstellung:**  Die Bereitstellung einer verwalteten Instanz kann mehr Zeit in Anspruch nehmen als bei einer Einzeldatenbank.  
- **Eingeschränkte Features**: Obwohl eine verwaltete Instanz die meisten Features von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bietet, gibt es weiterhin einige Features, die nicht unterstützt werden. 
- **Größenbeschränkung**: Die kombinierte Speichergröße für alle Datenbanken innerhalb einer verwalteten Instanz ist auf 8 TB begrenzt, im Gegensatz zu 524 PB bei lokalem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
- **Netzwerk**: Die Netzwerkanforderungen für eine verwaltete Instanz fügen Ihrer Infrastruktur eine zusätzliche Komplexitätsebene hinzu und erfordern entweder Azure ExpressRoute oder VPN Gateway.
- **Wartungszeit**: Es gibt keine Garantie für die genaue Wartungszeit, obwohl diese nahezu transparent ist. 

### <a name="resources"></a>Ressourcen

[Übersicht über Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance)       
[Wählen einer Azure SQL-Option](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)       
[Azure SQL-Datenbank: Featurevergleich](/azure/sql-database/sql-database-features)       
[Migrieren von SQL Server zu Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance-migrate)       
[Umfassende Beschreibung des Migrationsprozesses](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)       

Tools:
- [Data Migration Assistant](../../dma/dma-overview.md)
- [Database Migration Service](/azure/dms/dms-overview)

## <a name="non-sql-options"></a>Nicht-SQL-Optionen

Für bestimmte Arten von Anwendungen können Sie auch eine nicht relationale oder NoSQL-Lösung in Betracht ziehen, wie z. B. Azure Cosmos DB oder Azure Table Storage.

### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Erwägen Sie Azure Cosmos DB für moderne skalierbare Webanwendungen und mobile Anwendungen, die JSON-Daten verwenden und eine Kombination aus stabiler Abfrage- und Transaktionsdatenverarbeitung benötigen. Weitere Informationen finden Sie unter [Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). Informationen zum Importieren von Daten finden Sie unter [Import data to Cosmos DB (Importieren von Daten in Cosmos DB)](/azure/cosmos-db/import-data/).

Azure Cosmos DB bietet die folgenden Vorteile:
- Ihre Dokumente werden indiziert, und Sie können die vertraute SQL-Syntax für Abfragen darauf verwenden.
- Die Datenbank besitzt kein Schema.
- Sie können Dokumenten Eigenschaften hinzufügen, ohne die Indizes neu erstellen zu müssen.
- Sie erhalten JSON- und JavaScript-Unterstützung direkt innerhalb der Datenbank-Engine.
- Sie erhalten systemeigene Unterstützung für räumliche Daten und Integration mit anderen Azure-Services, einschließlich Azure Search, HDInsight, und Data Factory.
- Sie erhalten Speicherung mit geringer Latenz und hoher Leistung bei reservierten Durchsatzniveaus.

### <a name="azure-table-storage"></a>Azure Table Storage

Erwägen Sie Azure Table Storage, um teilweise strukturierte Daten in Petabyte-Größe in einer kostengünstigen Lösung zu speichern. Weitere Informationen finden Sie unter [Tabellenspeicher](https://azure.microsoft.com/services/storage/tables/).

Azure Table Storage bietet die folgenden Vorteile:
- Sie können Ihre Apps und Ihr Tabellenschema weiterentwickeln, ohne die Daten offline nehmen zu müssen.
- Sie können hochskalieren, ohne Ihr Dataset horizontal partitionieren zu müssen.
- Sie erhalten geografisch redundanten Speicher, der Daten über mehrere Regionen repliziert.

## <a name="lifecycle-dates"></a>Lebenszyklustermine

Die folgende Tabelle enthält ungefähre Angaben zu den Lebenszyklusterminen für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Produkte. Mehr und genauere Informationen finden Sie auf der Seite [Microsoft Lifecycle-Richtlinie](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy). 

| **Version**     | **Jahr der Veröffentlichung** | **Jahr des Endes des grundlegenden Supports** | **Jahr des Endes des erweiterten Supports** |
| :---------------| :--------------- | :------------------------------ | :---------------------------- |
| [SQL Server 2019](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202019) | 2019 | 2025 | 2030 |
| [SQL Server 2017](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202017) | 2017 | 2022 | 2027 |
| [SQL Server 2016](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202016) | 2016 | 2021 | 2026 |
| [SQL Server 2014](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202014) | 2014 | 2019 | 2024 |
| [SQL Server 2012](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202012) | 2012 | 2017 | 2022 |
| [SQL Server 2008 R2](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202008%20R2) | 2010 | 2012 | [2019](https://www.microsoft.com/sql-server/sql-server-2008) |
| [SQL Server 2008](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202008) | 2008 | 2012 | [2019](https://www.microsoft.com/sql-server/sql-server-2008) |
| [SQL Server 2005](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202005) | 2006 | 2011 | [2016](https://www.microsoft.com/sql-server/sql-server-2005) |
| [SQL Server 2000](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202000) | 2000 | 2005 | [2013](/archive/blogs/cdnitmanagers/sql-server-2000-end-of-support-april-2013) |

> [!IMPORTANT]
> Wenn eine Diskrepanz zwischen dieser Tabelle und der Seite mit dem [!INCLUDE[msCoName](../../includes/msconame-md.md)]-Lebenszyklus besteht, dann ersetzt der [!INCLUDE[msCoName](../../includes/msconame-md.md)]-Lebenszyklus die Angaben in dieser Tabelle, da diese als Referenz für angedachte Termine gedacht ist.  

## <a name="next-steps"></a>Nächste Schritte  
[SQL Server 2019](https://www.microsoft.com/sql-server/sql-server-2019)   
[SQL Server 2005-Supportende](https://www.microsoft.com/sql-server/sql-server-2005)   
[Ende des Supports für SQL Server 2008](https://www.microsoft.com/cloud-platform/windows-sql-server-2008)   
[Übersicht über erweiterte Sicherheitsupdates (ESUs)](sql-server-extended-security-updates.md)   
[Kostenlose erweiterte Sicherheitsupdates bei Migration im Istzustand zu Azure](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)   
[Übersicht über SQL Server-VMs](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)   
[Übersicht über Azure SQL-Datenbank](/azure/sql-database/sql-database-technical-overview)