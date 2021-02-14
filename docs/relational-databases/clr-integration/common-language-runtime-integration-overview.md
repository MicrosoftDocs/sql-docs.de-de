---
title: Übersicht über die Common Language Runtime (CLR)
description: Die CLR-Integration mit SQL Server ermöglicht es Ihnen, einige Funktionen mit einer beliebigen .NET Framework Sprache als SQL Server Server seitigen Modulen zu implementieren.
ms.custom: seo-lt-2019
ms.date: 06/20/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- managed code [SQL Server]
- common language runtime [SQL Server], about CLR integration
- cross-language integration
- integrating CLR [SQL Server]
- .NET Framework [SQL Server], common language runtime
- code access security [CLR integration]
- managed code [SQL Server], CLR integration
ms.assetid: 7be9e644-36a2-48fc-9206-faf59fdff4d7
author: rothja
ms.author: jroth
ms.openlocfilehash: a9ffcef000e043195d46bf98a11253205b5bc15b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2021
ms.locfileid: "100348033"
---
# <a name="common-language-runtime-integration"></a>Common Language Runtime-Integration
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]und [Azure SQL verwaltete Instanz](/azure/sql-database/sql-database-managed-instance-index) ermöglichen es Ihnen, einige der Funktionen mithilfe von .NET-Sprachen zu implementieren, indem Sie die native Common Language Runtime (CLR)-Integration als SQL Server Server seitige Module (Prozeduren, Funktionen und Trigger) verwenden. Die CLR-Komponente stellt verwalteten Code mit Diensten bereit, wie z. B. sprachübergreifende Integration, Codezugriffssicherheit, Verwaltung der Objektlebensdauer und Debug- und Profilerstellungsunterstützung. Für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Benutzer und -Anwendungsentwickler bedeutet die CLR-Integration, dass sie nunmehr gespeicherte Prozeduren, Trigger, benutzerdefinierte Typen, benutzerdefinierte Funktionen (Skalar- und Tabellenwertfunktionen) sowie benutzerdefinierte Aggregatfunktionen mit einer beliebigen .NET Framework-Sprache, einschließlich [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic .NET und [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C#, schreiben können. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ist .NET Framework, Version 4, vorinstalliert.  

> [!WARNING]
>  CLR verwendet die Codezugriffssicherheit (Code Access Security, CAS) im .NET Framework, die nicht länger als Sicherheitsbegrenzung unterstützt wird. Eine CLR-Assembly, die mit `PERMISSION_SET = SAFE` erstellt wurde, kann womöglich auf externe Systemressourcen zugreifen, nicht verwalteten Code aufrufen und sysadmin-Privilegien erwerben. Ab [!INCLUDE[sssql17](../../includes/sssql17-md.md)] wird eine `sp_configure`-Option mit der Bezeichnung `clr strict security` eingeführt, um die Sicherheit von CLR-Assemblys zu erhöhen. `clr strict security` ist standardmäßig aktiviert und behandelt `SAFE`- und `EXTERNAL_ACCESS`-Assemblys so, als wären Sie als `UNSAFE` gekennzeichnet. Die Option `clr strict security` kann für die Abwärtskompatibilität deaktiviert werden, es wird jedoch nicht empfohlen. Microsoft empfiehlt, dass alle Assemblys durch ein Zertifikat oder einen asymmetrischen Schlüssel mit einem entsprechenden Anmeldenamen signiert werden, dem `UNSAFE ASSEMBLY`-Berechtigung für die Masterdatenbank gewährt wurde. Weitere Informationen finden Sie unter [CLR Strict Security](../../database-engine/configure-windows/clr-strict-security.md). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Administratoren können auch Assemblys einer Liste von Assemblys hinzufügen, der die Datenbank-Engine vertrauen sollte. Weitere Informationen finden Sie unter [sys.sp_add_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-add-trusted-assembly-transact-sql.md).

Sie können sich auch dieses 6-minütige Video ansehen, das Ihnen zeigt, wie Sie CLR in Azure SQL-verwaltete Instanz verwenden:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Its-just-SQL-CLR-in-Azure-SQL-Database-Managed-Instance/player?WT.mc_id=dataexposed-c9-niner]



## <a name="when-to-use-clr-modules"></a>Wann sollten CLR-Module verwendet werden?

Mithilfe der CLR-Integration können Sie komplexe Funktionen implementieren, die in .NET Framework verfügbar sind, wie z. b. reguläre Ausdrücke, Code für den Zugriff auf externe Ressourcen (Server, Webdienste, Datenbanken), benutzerdefinierte Verschlüsselung usw. Zu den Vorteilen der serverseitigen CLR-Integration gehören:
  
-   **Ein besseres Programmiermodell.** Die .NET Framework-Sprachen sind in vielerlei Hinsicht umfassender als Transact-SQL. Sie bieten Konstrukte und Fähigkeiten, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Entwicklern zuvor nicht zur Verfügung standen. Entwickler können zudem die leistungsfähigen Funktionen der .NET Framework-Bibliothek nutzen, die einen umfassenden Satz Klassen bereitstellt. Diese ermöglichen es, Programmierungsprobleme schnell und effizient zu lösen.  
  
-   **Verbesserte Sicherheit und Zuverlässigkeit.** Verwalteter Code wird in einer von der Datenbank-Engine gehosteten Common Language Runtime-Umgebung ausgeführt. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nutzt dies, um eine sicherere Alternative zu den erweiterten gespeicherten Prozeduren zu bieten, die in früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet wurden.  
  
-   **Fähigkeit, Datentypen und Aggregatsfunktionen zu definieren.** Benutzerdefinierte Typen und benutzerdefinierte Aggregate sind zwei neue verwaltete Datenbankobjekte, die die Speicher-und Abfragefunktionen von erweitern [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   **Rationalisierte Entwicklung durch eine standardisierte Umgebung.** Die Datenbankentwicklung ist in zukünftige Versionen der [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Studio .NET-Entwicklungsumgebung integriert. Entwickler verwenden für das Entwickeln und Debuggen von Datenbankobjekten und Skripts dieselben Tools wie für das Schreiben von .NET Framework-Komponenten und -Diensten auf mittlerer Ebene oder Clientebene.  
  
-   **Potenziell verbesserte Leistung und Skalierbarkeit.** In vielen Situationen sorgen die Kompilierungs- und Ausführungsmodelle der .NET Framework-Sprachen für eine verbesserte Leistungsfähigkeit gegenüber Transact-SQL.  
  
 In der folgenden Tabelle sind die Themen in diesem Abschnitt aufgeführt.  
  
 [Übersicht über die CLR-Integration](../../relational-databases/clr-integration/clr-integration-overview.md)  
 Beschreibt, welche Objekte mit der CLR-Integration erstellt werden können, und beschreibt die Anforderungen zur Erstellung von Datenbankobjekten mithilfe der CLR-Integration.  
  
 [Neuigkeiten bei der CLR-Integration](../../relational-databases/clr-integration/clr-integration-what-s-new.md)  
 Beschreibt die neuen Funktionen in dieser Version.  
  
 [Architektur der CLR-Integration](./clr-integration-architecture-clr-hosted-environment.md)  
 Beschreibt die Entwurfsziele der CLR-Integration.  
  
 [Aktivieren der CLR-Integration](../../relational-databases/clr-integration/clr-integration-enabling.md)  
 Beschreibt, wie die CLR-Integration aktiviert wird.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Installieren des .NET Framework](https://technet.microsoft.com/library/ms166014\(v=SQL.105\).aspx)  ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nur)   
 [Leistungsfähigkeit der CLR-Integration](../../relational-databases/clr-integration/clr-integration-architecture-performance.md)  
  
