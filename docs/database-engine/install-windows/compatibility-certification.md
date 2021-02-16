---
title: Kompatibilitätszertifizierung | Microsoft-Dokumentation
description: Mithilfe der Kompatibilitätszertifizierung wird das Risiko der Anwendungskompatibilität verringert, wodurch Sie ein Upgrade für eine SQL Server-Datenbank lokal und in der Cloud durchführen können.
ms.custom: ''
ms.date: 08/26/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- compatibility [SQL Server], databases
- compatibility levels [SQL Server], after upgrade
- Database Engine [SQL Server], upgrading
- Databases [SQL Server], upgrading
- Database Engine [SQL Server], upgrading
- compatibility [SQL Server], certification
- compatibility level [SQL Server], upgrades
ms.assetid: 3c036813-36cf-4415-a0c9-248d0a433856
author: pmasl
ms.author: pelopes
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c9a4b07a3e6db3da84845f4689ecd373b30e7306
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353841"
---
# <a name="compatibility-certification"></a>Kompatibilitätszertifizierung

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Die Kompatibilitätszertifizierung ermöglicht es Unternehmen, für eine lokale, cloudbasierte oder edgebasierte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank ein Upgrade vorzunehmen und diese zu erneuern, um Risiken im Zusammenhang mit der Anwendungskompatibilität zu vermeiden. 

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] (sowie verwaltete Instanzen) basieren auf derselben [!INCLUDE[ssde_md](../../includes/ssde_md.md)]. Mithilfe dieser gemeinsam verwendeten [!INCLUDE[ssde_md](../../includes/ssde_md.md)] kann eine Benutzerdatenbank problemlos von einer lokalen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz zu [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] migriert werden. Der Anwendungscode wird dabei in der Datenbank als [!INCLUDE[tsql](../../includes/tsql-md.md)] ausgeführt und funktioniert weiterhin so wie auf dem Quellsystem.

Bei einem neuen Release von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird der Standardkompatibilitätsgrad auf die Version der [!INCLUDE[ssDE](../../includes/ssde-md.md)] festgelegt. Der Kompatibilitätsgrad vorheriger Versionen wird jedoch beibehalten, um die Kompatibilität mit vorhandenen Anwendungen sicherzustellen. Diese Kompatibilitätsmatrix finden Sie unter [Argumente](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#supported-dbcompats).
Eine Anwendung, die für eine bestimmte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Version zertifiziert wurde, **wurde daher für den Standardkompatibilitätsgrad dieser Version zertifiziert**.

In [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] wurde beispielsweise für den Datenbank-Kompatibilitätsgrad der Standardwert 130 verwendet. Da Kompatibilitätsgrade bestimmte [!INCLUDE[tsql](../../includes/tsql-md.md)]-Verhalten in Bezug auf Funktionen und auf die Abfrageoptimierung erzwingen, war **eine Datenbank, die für [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] zertifiziert war, implizit auch für den Datenbank-Kompatibilitätsgrad 130 zertifiziert**. Eine solche Datenbank kann unverändert in einer neueren Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (z. B. [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]) und [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] verwendet werden, solange der Datenbank-Kompatibilitätsgrad 130 beibehalten wird. 

Dies ist ein grundlegendes Prinzip des Betriebsmodells Continuous Integration in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)]. Die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] wird in Azure kontinuierlich verbessert und um Upgrades ergänzt. Da allerdings vorhandene Datenbanken ihren aktuellen Kompatibilitätsgrad beibehalten, funktionieren sie auch nach dem Upgrade der zugrunde liegenden [!INCLUDE[ssde_md](../../includes/ssde_md.md)] wie vorgesehen. 

SharePoint Server 2016 und SharePoint Server 2019 führen Zertifizierungen für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] auf dieselbe Weise durch, sodass Sie jede [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] bereitstellen können, die die unterstützten Datenbank-Kompatibilitätsgrade für diese SharePoint Server-Versionen verwenden kann. Weitere Informationen finden Sie unter [Hardware- und Softwareanforderungen für SharePoint Server 2016](/sharepoint/install/hardware-and-software-requirements#minimum-requirements-for-a-database-server-in-a-farm) und [Hardware- und Softwareanforderungen für SharePoint Server 2019](/sharepoint/install/hardware-and-software-requirements-2019#minimum-requirements-for-a-database-server-in-a-farm).

## <a name="managing-upgrade-risk-with-compatibility-certification"></a>Steuern von Upgraderisiken mithilfe der Kompatibilitätszertifizierung
Die Kompatibilitätszertifizierung ist eine Methode, die sich gut zur Datenbankmodernisierung eignet. Durch eine Zertifizierung auf Grundlage des Kompatibilitätsgrads legen Entwickler die technischen Anwendungsanforderungen fest. Diese müssen erfüllt sein, damit die Anwendungen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] unterstützt werden. Gleichzeitig wird jedoch der Lebenszyklus der Anwendung vom Lebenszyklus der Datenbankplattform getrennt. Unternehmen können auf diese Weise sicherstellen, dass die SQL Server-[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] entsprechend den Lebenszyklusrichtlinien immer auf dem neuesten Stand ist. Außerdem können neue Skalierbarkeits- und Leistungsoptimierungen genutzt werden, die codeunabhängig sind. Vernetzte Anwendungen **behalten darüber hinaus durch Upgrades ihren Funktionsstatus**.

Bei einem Upgrade bestehen die wesentlichen Risikofaktoren in einer eingeschränkten Funktionalität und Leistung. Mit der Kompatibilitätszertifizierung können die folgenden Upgraderisiken problemlos gesteuert werden:

-  Wenn sich das [!INCLUDE[tsql](../../includes/tsql-md.md)]-Verhalten ändert, muss die Anwendung rezertifiziert werden, damit sie richtig funktioniert. Die Einstellung [Datenbank-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) bietet Abwärtskompatibilität mit früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Dies gilt allerdings ausschließlich für die angegebene Datenbank und nicht für den gesamten Server. Mit einem unveränderten Datenbank-Kompatibilitätsgrad wird sichergestellt, dass vorhandene Anwendungsabfragen vor und nach einem Upgrade der [!INCLUDE[ssde_md](../../includes/ssde_md.md)] weiterhin dasselbe Verhalten aufweisen. Weitere Informationen zum [!INCLUDE[tsql](../../includes/tsql-md.md)]-Verhalten und zu Kompatibilitätsgraden finden Sie unter [Verwenden von Kompatibilitätsgraden für Abwärtskompatibilität](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#backwardCompat).

-  Im Zusammenhang mit der Leistung können Abweichungen bei Abfrageplänen zwischen verschiedenen [!INCLUDE[ssde_md](../../includes/ssde_md.md)]-Versionen auftreten, da in jeder neuen Version der Abfrageoptimierer verbessert wird. Abfrageplanabweichungen, die bei einem Upgrade auftreten, führen in der Regel zu einem Risiko, wenn eine bestimmte Abfrage oder Workload durch Änderungen beeinträchtigt werden können. Dieses Risiko ist wiederum ein Grund für eine Rezertifizierung. Dies kann zu einer Verzögerung von Upgrades und zu Lebenszyklus- und Supportproblemen führen kann. 
   Verbesserungen des Abfrageoptimierers werden durch den Standardkompatibilitätsgrad eines neuen Release (d. h. der höchsten verfügbaren Kompatibilitätsstufe für jede neue Version) eingeschränkt, um die Upgraderisiken zu verringern. Die Kompatibilitätszertifizierung umfasst den **Schutz der Abfrageplanform**. Das bedeutet: Wenn der Datenbank-Kompatibilitätsgrad nach einem Upgrade der [!INCLUDE[ssde_md](../../includes/ssde_md.md)] beibehalten wird, wird dasselbe Modell zur Abfrageoptimierung in der neuen Version verwendet wie vor dem Upgrade. Die Abfrageplanform sollte sich nicht ändern. 
   Weitere Informationen finden Sie im Abschnitt [Gründe für die Form des Abfrageplans](#queryplan_shape) dieses Artikels.
   
Weitere Informationen zu Kompatibilitätsgraden finden Sie unter [Verwenden von Kompatibilitätsgraden für Abwärtskompatibilität](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#backwardCompat).
   
> [!IMPORTANT]
> Aktualisieren Sie bei vorhandenen Anwendungen, die bereits für einen bestimmten Kompatibilitätsgrad zertifiziert wurden, die [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], und **behalten** Sie den vorherigen Datenbank-Kompatibilitätsgrad bei. In diesem Szenario ist ein erneutes Zertifizieren der Anwendungen nicht erforderlich. Weitere Informationen finden Sie weiter unten in diesem Artikel unter [Kompatibilitätsgrade und Upgrades der Datenbank-Engine](#compatibility-levels-and-database-engine-upgrades).
>
> Bei Neuentwicklungen oder wenn eine vorhandene Anwendung neue Features wie die [intelligente Abfrageverarbeitung](../../relational-databases/performance/intelligent-query-processing.md) sowie eine neue Version von [!INCLUDE[tsql](../../includes/tsql-md.md)] nutzen muss, sollten Sie ein Upgrade auf den neuesten in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verfügbaren Datenbank-Kompatibilitätsgrad planen und Ihre Anwendung so erneut zertifizieren, dass sie mit diesem Kompatibilitätsgrad verwendet werden kann. Ausführlichere Informationen zu einem Upgrade des Datenbank-Kompatibilitätsgrads finden Sie unter [Bewährte Methoden zum Aktualisieren des Datenbank-Kompatibilitätsgrads](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#best-practices-for-upgrading-database-compatibility-level).
   
### <a name="why-query-plan-shape"></a><a name="queryplan_shape"></a> Gründe für die Form des Abfrageplans      
Die Abfrageplanform bezieht sich auf die visuelle Darstellung der verschiedenen Operatoren, die einen Abfrageplan bilden. Dies schließt Operatoren wie Suchvorgänge, Scans, Joins und Sortierungen sowie die Verbindungen zwischen diesen ein, die den Datenfluss und die Reihenfolge der Vorgänge angeben, die ausgeführt werden müssen, um das gewünschte Resultset zu erzeugen. Die Abfrageplanform wird durch den Abfrageoptimierer bestimmt.

Um die Abfrageleistung während eines Upgrades vorhersagbar zu halten, besteht eines der grundlegenden Ziele darin sicherzustellen, dass dieselbe Abfrageplanform verwendet wird. Dies kann erreicht werden, indem der Datenbank-Kompatibilitätsgrad nicht unmittelbar nach einem Upgrade geändert wird, auch wenn die zugrundeliegende [!INCLUDE[ssde_md](../../includes/ssde_md.md)] über unterschiedliche Versionen verfügt. Wenn im Ökosystem der Abfrageausführung nichts anderes geändert wird, z. B. bedeutende Änderungen an verfügbaren Ressourcen oder die Datenverteilung in den zugrundeliegenden Daten, sollte die Leistung einer Abfrage unverändert bleiben. 

Das Beibehalten der Form eines Abfrageplans ist jedoch nicht der einzige Faktor, der nach einem Upgrade zu Leistungseinbußen führen kann. Wenn Sie die Datenbank auf eine neuere [!INCLUDE[ssde_md](../../includes/ssde_md.md)] verschieben und auch Umgebungsänderungen vornehmen, führen Sie möglicherweise Faktoren ein, die unmittelbare Auswirkungen auf die Leistung einer Abfrage haben, auch wenn der Abfrageplan die gleiche Form in verschiedenen Versionen beibehält. Diese Umgebungsänderungen können das neue [!INCLUDE[ssde_md](../../includes/ssde_md.md)] enthalten, das mehr oder weniger Arbeitsspeicher und CPU-Ressourcen verfügbar macht, Änderungen an Server-oder Datenbank-Konfigurationsoptionen oder Änderungen an der Datenverteilung, die sich auf die Erstellung eines Abfrageplans auswirken. Daher ist es wichtig zu verstehen, dass das Beibehalten des Datenbank-Kompatibilitätsgrads vor Änderungen in der **Form** des Abfrageplans schützt, jedoch keinen Schutz vor anderen die Abfrageleistung beeinflussenden Umgebungsaspekten bietet, von denen einige vom Benutzer initiiert werden.

Weitere Informationen finden Sie im [Handbuch zur Architektur der Abfrageverarbeitung](../../relational-databases/query-processing-architecture-guide.md#optimizing-select-statements).
   
## <a name="compatibility-certification-benefits"></a>Nutzen der Kompatibilitätszertifizierung
Die Datenbankzertifizierung auf Grundlage der Kompatibilität ist in mehrfacher Hinsicht sinnvoller als eine Zertifizierung, die auf Versionsbezeichnungen basiert:

-  **Entkopplung der Anwendungszertifizierung von der Plattform:** Für Anwendungen, die [!INCLUDE[tsql](../../includes/tsql-md.md)]-Abfragen ausführen müssen, sind wegen der gemeinsam verwendeten [!INCLUDE[ssde_md](../../includes/ssde_md.md)] keine unterschiedlichen Zertifizierungsprozesse für Azure und lokale Umgebungen erforderlich.
-  **Verringerung der Upgraderisiken:** Bei der Modernisierung von Datenbankplattformen können die Upgradezyklen für die Anwendungs- und Datenbankebene getrennt werden, was zu weniger Ausfallzeiten und einem verbesserten Change Management führt.
-  **Upgrade ohne Codeänderungen:** Bei einem Upgrade auf eine neue Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] sind keine Codeänderungen erforderlich, wenn der Kompatibilitätsgrad und das Quellsystem beibehalten werden. Eine Rezertifizierung ist erst erforderlich, wenn die Anwendung Verbesserungen nutzen muss, die nur von höheren Datenbank-Kompatibilitätsgraden bereitgestellt werden.
- **Verbesserte Verwaltbarkeit und Skalierbarkeit:** Eine Anpassung von Anwendungen ist nicht erforderlich. Außerdem werden Verbesserungen nicht durch den Datenbank-Kompatibilitätsgrad eingeschränkt. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] enthält beispielsweise folgende Optimierungen: 
  - umfassende Verbesserungen bei der Überwachung und Problembehandlung mit neuen [dynamischen Systemverwaltungssichten](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md), [erweiterten Ereignissen](../../relational-databases/extended-events/extended-events.md) und der [automatischen Optimierung](../../relational-databases/automatic-tuning/automatic-tuning.md) 
  - Verbesserte Skalierbarkeit, z. B. mit [automatischer Soft-NUMA-Konfiguration](../../database-engine/configure-windows/soft-numa-sql-server.md#automatic-soft-numa), [beschleunigter Datenbankwiederherstellung](../../relational-databases/accelerated-database-recovery-concepts.md) oder [speicheroptimierten tempdb-Metadaten](../../relational-databases/in-memory-database.md#memory-optimized-tempdb-metadata)

Neue Datenbanken werden weiterhin auf den Standardkompatibilitätsgrad der [!INCLUDE[ssde_md](../../includes/ssde_md.md)]-Version festgelegt. Wenn jedoch eine Datenbank von einer früheren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Version auf eine neue Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] aktualisiert wird, behält die Datenbank den vorhandenen Kompatibilitätsgrad bei. 

> [!IMPORTANT]
> Wenn Sie eine Datenbank auf eine neue Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] aktualisieren möchten, müssen Sie vorher überprüfen, ob der Datenbank-Kompatibilitätsgrad noch unterstützt wird. Die Matrix für die unterstützten Datenbank-Kompatibilitätsgrade finden Sie unter [Argumente](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#arguments). 
>
> Beim Ausführen eines Upgrades für eine Datenbank, deren Kompatibilitätsgrad unterhalb des zulässigen Grads liegt (beispielsweise 90 in der Standardeinstellung in [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]), wird die Datenbank automatisch auf den niedrigsten zulässigen Kompatibilitätsgrad (100) festgelegt.
>
> Führen Sie eine Abfrage für die Spalte **compatibility_level** von [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) aus, um die aktuellen Kompatibilitätsgrad zu ermitteln.

## <a name="compatibility-levels-and-database-engine-upgrades"></a>Kompatibilitätsgrade und Upgrades der Datenbank-Engine
Wenn Sie für die [!INCLUDE[ssde_md](../../includes/ssde_md.md)] ein Upgrade auf die neueste Version durchführen, dabei aber den Datenbank-Kompatibilitätsgrad von vor dem Upgrade und dessen Unterstützbarkeitsstatus beibehalten möchten, empfiehlt es sich, eine **statische Funktionsüberprüfung des Oberflächenbereichs** des Anwendungscodes **in der Datenbank** (Programmierbarkeitsobjekte wie gespeicherte Prozeduren, Funktionen und Trigger) und **in der Anwendung** (mithilfe einer Arbeitsauslastungs-Ablaufverfolgung, die den von der Anwendung gesendeten dynamischen Code erfasst) durchzuführen.

Dies ist mit dem Tool [Microsoft Datenmigrations-Assistent](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) problemlos möglich. Gibt es in der Ausgabe von DMA keine Fehler hinsichtlich fehlender oder inkompatibler Funktionalität, ist zu erwarten, dass es keine funktionalen Rückschritte für die Anwendung in der neuen Zielversion gibt. Wenn Änderungen erforderlich sind, um sicherzustellen, dass Ihre Datenbank in der neuen Version funktioniert, können Sie mit dem DMA ermitteln, wo Änderungen erforderlich sind und welche Problemumgehungsmöglichkeiten es gibt. Weitere Informationen finden Sie unter [Übersicht über den Datenmigrations-Assistenten](../../dma/dma-overview.md).   

> [!TIP]
> Diese Funktionsüberprüfung ist besonders wichtig, wenn eine Datenbank von einer Legacyversion (z. B. [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] oder [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) zu einer neuen Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] migriert wird, da Ihr Anwendungscode möglicherweise eine nicht mehr unterstützte Version von [!INCLUDE[tsql](../../includes/tsql-md.md)] verwendet, die vom Datenbank-Kompatibilitätsgrad nicht umfasst ist. Wenn Sie jedoch von einer neueren Version (z. B. [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) zu [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] oder [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] wechseln, müssen Sie sich keine Gedanken über eine nicht mehr unterstützte Version von [!INCLUDE[tsql](../../includes/tsql-md.md)] machen. Weitere Informationen zu nicht mehr unterstützten Versionen von [!INCLUDE[tsql](../../includes/tsql-md.md)] finden Sie unter [Verwenden des Kompatibilitätsgrads für Abwärtskompatibilität](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#backwardCompat).

> [!NOTE]
> DMA unterstützt Datenbank-Kompatibilitätsgrad 100 und höher. [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ist als Quellversion ausgeschlossen.   

> [!IMPORTANT]
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] empfiehlt, einige Minimaltests auszuführen, um den Erfolg eines Upgrades zu überprüfen, wenn der frühere Datenbank-Kompatibilitätsgrad beibehalten wird. Sie sollten bestimmen, was „minimale Tests“ im Zusammenhang Ihrer Anwendung und Ihres Szenarios bedeutet.   

> [!IMPORTANT]
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] bietet Schutz für eine Abfrageplanform, wenn Folgendes zutrifft:
>
> - Die neue [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Version (Ziel) wird auf Hardware ausgeführt, die mit der Hardware vergleichbar ist, auf der die vorherige [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Version (Quelle) ausgeführt wurde.
> - Derselbe [unterstützte Datenbank-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#supported-dbcompats) wird sowohl in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Ziel- als auch in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Quellinstanz verwendet.
> - **Dieselbe** Datenbank und Workload werden sowohl für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Zielinstanz als auch für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Quellinstanz verwendet. 
>
> Alle Rückschritte der Abfrageplanform (im Vergleich mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Quellinstanz), die bei den oben genannten Bedingungen auftreten, werden behandelt. Wenden Sie sich an den Microsoft-Kundensupport, wenn dies der Fall ist.
  
## <a name="see-also"></a>Weitere Informationen 
[ALTER DATABASE-Kompatibilitätsgrad](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)       
[Anzeigen oder Ändern des Kompatibilitätsgrads einer Datenbank](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)       
[Bewährte Methoden zum Aktualisieren des Datenbank-Kompatibilitätsgrads](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md#best-practices-for-upgrading-database-compatibility-level)