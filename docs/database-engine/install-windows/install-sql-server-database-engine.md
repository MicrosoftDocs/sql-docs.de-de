---
title: Installation der Datenbank-Engine von SQL Server | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über Features, die installiert werden können, wenn Sie SQL Server-Datenbank-Engine im SQL Server-Installations-Assistenten unter „Zu installierende Komponenten“ auswählen.
ms.custom: ''
ms.date: 07/26/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- Database Engine [SQL Server], installing
ms.assetid: d0876e7f-aa52-4dd7-bd5c-029e2ffded5f
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 76ccb8e197303695af12b306e4974899226bd1a3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336373"
---
# <a name="install-sql-server-database-engine"></a>Installieren der SQL Server-Datenbank-Engine

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

## <a name="overview"></a>Übersicht
Bei der Komponente [!INCLUDE[ssDE](../../includes/ssde-md.md)] von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] handelt es sich um den zentralen Dienst zum Speichern, Verarbeiten und Sichern von Daten. Das [!INCLUDE[ssDE](../../includes/ssde-md.md)] bietet gesteuerten Zugriff und eine zügige Transaktionsverarbeitung und erfüllt damit die Anforderungen selbst der anspruchsvollsten datenlastigen Anwendungen des Unternehmens.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt bis zu 50 Instanzen von [!INCLUDE[ssDE](../../includes/ssde-md.md)] auf einem einzelnen Computer. Um eine typische [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Installation durchzuführen, gehen Sie auf die Seite [Install SQL Server from the Installation Wizard &#40;Setup&#41; (Installieren von SQL Server mit dem Installations-Assistenten &#40;Setup&#41;)](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md).  
  
>[!IMPORTANT]
>Bei lokalen Installationen müssen Sie das Setup als Administrator ausführen. Wenn Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] von einer Remotefreigabe installieren, müssen Sie ein Domänenkonto verwenden, das Lese- und Ausführungsberechtigungen auf der Remotefreigabe hat.  

## <a name="features"></a>Features
Wenn Sie im Installations-Assistenten von **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf der Seite Zu installierende Komponenten die Option** -Datenbank-Engine[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auswählen, werden folgenden Funktionen installiert:  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)]  
  
-   [SQL Server-Replikation](../../relational-databases/replication/sql-server-replication.md) – optionale Komponente  

::: moniker range=">=sql-server-ver15"
-   [Machine Learning Services](../../machine-learning/install/sql-machine-learning-services-windows-install.md) (R und Python) und [Spracherweiterungen](../..//language-extensions/install/windows-java.md) (Java) – optionale Komponente
::: moniker-end

::: moniker range=">=sql-server-2017 <=sql-server-2017"
-   [Machine Learning Services (datenbankintern)](../../machine-learning/install/sql-machine-learning-services-windows-install.md) (R und Python) – optionale Komponente
::: moniker-end

::: moniker range=">=sql-server-2016 <=sql-server-2016"
-   [R Services (datenbankintern)](../../machine-learning/install/sql-r-services-windows-install.md) – optionale Komponente
::: moniker-end

-   Volltextsuche – optionale Komponente  
  
-   Data Quality Services: optionale Komponente  
  
    > [!NOTE]  
    >  In dieser Version wird durch Aktivieren des Kontrollkästchens **Data Quality Services** während des Setups DQS (Data Quality Services)-Server nicht installiert. Sie müssen nach der Installation weitere Schritte ausführen, um DQS-Server zu installieren. Weitere Informationen finden Sie unter [Install Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md).  
    
- [PolyBase-Abfragedienst für externe Daten](../../relational-databases/polybase/polybase-guide.md) – optionale Komponente Ab SQL Server 2019 ist der Java-Connector auch für HDFS-Datenquellen (Hadoop Distributed File System) verfügbar.

  
 In vielen typischen Benutzerszenarien sind die folgenden zusätzlichen Funktionen als Optionen verfügbar:  
  
-   Data Quality Client
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]
-   Konnektivitätskomponenten
-   Programmmodelle
-   Verwaltungstools
-   [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]
-   Dokumentationskomponenten  
  

> [!NOTE]  
>  Standardmäßig werden Beispieldatenbanken und Beispielcode nicht als Teil des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setups installiert. Informationen zur Installation von Beispieldatenbanken und Beispielcode finden Sie unter [Microsoft SQL Server Samples (Beispiele für Microsoft SQL Server)](../../samples/sql-samples-where-are.md). Ältere Beispiele finden Sie auf der [CodePlex](https://go.microsoft.com/fwlink/?LinkId=87843)-Seite.  

  
## <a name="see-also"></a>Weitere Informationen  
 [Editionen und unterstützten Funktionen von SQL Server 2017](~/sql-server/editions-and-components-of-sql-server-2017.md)   
 [Planen einer SQL Server-Installation](../../sql-server/install/planning-a-sql-server-installation.md)   
 [Lösungen mit hoher Verfügbarkeit &#40;SQL Server&#41;](../sql-server-business-continuity-dr.md)   
 [Upgrade SQL Server Using the Installation Wizard (Setup) (Upgrade von SQL Server mithilfe des Installations-Assistenten (Setup))](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)  
  
