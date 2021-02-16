---
description: Microsoft Connectors für Oracle und Teradata von Attunity für Integration Services (SSIS)
title: Microsoft Connectors für Oracle und Teradata von Attunity | Microsoft-Dokumentation
ms.date: 08/16/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.custom: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: ''
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 012d95d493709ab07741134340141af66399fb53
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353729"
---
# <a name="microsoft-connectors-for-oracle-and-teradata-by-attunity-for-integration-services-ssis"></a>Microsoft Connectors für Oracle und Teradata von Attunity für Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]

> [!NOTE]
> Atunity-Connectors für Oracle und Teradata unterstützen SQL Server 2017 und niedriger.
>
> Holen Sie sich hier die neuesten Connectors für Oracle und Teradata von SQL Server 2019:
> - [Microsoft Connector für Oracle](data-flow/oracle-connector.md)
> - [Microsoft Connector für Teradata](data-flow/teradata-connector.md)

Sie können Connectors von Attunity für Integration Services herunterladen, die die Leistung verbessern, wenn Daten in oder aus Oracle oder Teradata in ein SSIS-Paket geladen werden.

## <a name="download-the-latest-attunity-connectors"></a>Herunterladen der neuesten Attunity-Connectors

Laden Sie die neuesten Versionen der Connectors unter folgendem Link herunter:  
[Microsoft Connectors v5.0 for Oracle and Teradata (Microsoft Connectors Version 5.0 für Oracle und Teradata)](https://www.microsoft.com/download/details.aspx?id=55179)

## <a name="issue---the-attunity-connectors-arent-visible-in-the-ssis-toolbox"></a>Problem: Die Attunity-Connectors werden in der SSIS-Toolbox nicht angezeigt

Damit die Attunity-Connectors in der SSIS-Toolbox angezeigt werden, müssen Sie stets die Version der Connectors installieren, die auf dieselbe SQL Server-Version ausgerichtet ist wie die auf Ihrem Computer installierten SQL Server Data Tools (SSDT). (Es dürfen auch frühere Versionen der Connectors installiert sein.) Diese Anforderung steht nicht mit der SQL Server-Version in Zusammenhang, auf die Ihre SSIS-Projekte und -Pakete ausgerichtet sind.

Wenn z.B. die neueste SSDT-Version installiert ist, ist Version 17 von SSDT mit einer Buildnummer installiert, die mit der Zahl 14 beginnt. In dieser Version von SSDT wird die SQL Server 2017-Unterstützung hinzugefügt. Sie müssen die neuste Version von Attunity-Connectors (Version 5.0) installieren, wenn Sie Attunity Connectors in der SSIS-Paketentwicklung abrufen und verwenden möchten, sogar wenn Sie diese auf eine frühere SQL Server-Version ausrichten möchten. In dieser Version der Connectors wird außerdem die SQL Server 2017-Unterstützung hinzugefügt.

Überprüfen Sie die in Visual Studio installierte Version von SSDT unter **Hilfe** | **Info zu Microsoft Visual Studio** oder unter **Programme und Funktionen** in den Einstellungen. Installieren Sie anschließend die zugehörige Version der Attunity-Connectors aus der folgenden Tabelle.

|SSDT-Version|SSDT-Buildnummer|Zielversion von SQL Server|Erforderliche Connectors-Version|
|---------|---------|---------|---------|
|17|Beginnt mit 14|SQL Server 2017|[Microsoft Connectors v5.0 for Oracle and Teradata (Microsoft Connectors Version 5.0 für Oracle und Teradata)](https://www.microsoft.com/download/details.aspx?id=55179)|
|16|Beginnt mit 13|SQL Server 2016|[Microsoft Connectors v4.0 for Oracle and Teradata (Microsoft Connectors Version 4.0 für Oracle und Teradata)](https://www.microsoft.com/download/details.aspx?id=52950)|
||||

## <a name="download-the-latest-sql-server-data-tools-ssdt"></a>Herunterladen der neuesten SQL Server Data Tools (SSDT)

Laden Sie die neueste Version von SSDT unter folgendem Link herunter:  
[Download der neuesten SQL Server-Datatools](..//ssdt/download-sql-server-data-tools-ssdt.md)
