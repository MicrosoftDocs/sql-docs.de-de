---
title: Integration Services (SSIS) Integration Services Scale Out | Microsoft-Dokumentation
description: Dieser Artikel enthält eine Übersicht über das SSIS Scale Out-Feature, das leistungsstarke Ausführung von SSIS-Paketen ermöglicht.
ms.custom: performance
ms.date: 12/13/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: dcfbd1c5-c001-4fb7-b9ae-916e49ab6a96
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9e75be10cff0f34d132ecfe57ddff10538091a77
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338359"
---
# <a name="integration-services-ssis-scale-out"></a>Integration Services (SSIS) Scale Out

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Mithilfe von Scale Out für SQL Server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] (SSIS) können SSIS-Pakete mit hoher Leistung ausgeführt werden, indem Paketausführungen auf mehrere Computer verteilt werden. Nachdem Sie Scale Out eingerichtet haben, können Sie mehrere Paketausführungen gleichzeitig im Scale Out-Modus über SQL Server Management Studio (SSMS) ausführen.

## <a name="components"></a>Komponenten
[!INCLUDE[ssIS_md](../../includes/ssis-md.md)] Scale Out besteht aus einem Scale Out-Master für [!INCLUDE[ssIS_md](../../includes/ssis-md.md)] und mindestens einem Scale Out-Worker für [!INCLUDE[ssIS_md](../../includes/ssis-md.md)].

-   Der Scale Out-Master ist für die Scale Out-Verwaltung verantwortlich und empfängt Paketausführungsanforderungen von Benutzern. Weitere Informationen finden Sie unter [Scale Out-Master](integration-services-ssis-scale-out-master.md).

-   Die Scale Out-Worker entnehmen dem Scale Out-Master Ausführungsaufgaben und führen die Pakete aus. Weitere Informationen finden Sie unter [Scale Out-Worker](integration-services-ssis-scale-out-worker.md).

## <a name="configuration-options"></a>Konfigurationsoptionen
Sie können Scale Out in den folgenden Konfigurationen einrichten:

-   **Auf einem einzelnen Computer:** Scale Out-Master und Scale Out-Worker werden gleichzeitig auf demselben Computer ausgeführt.

-   **Auf mehreren Computern:** Die Scale Out-Worker befinden sich auf unterschiedlichen Computern.

## <a name="what-you-can-do"></a>Ihre Möglichkeiten
Nachdem Sie Scale Out eingerichtet haben, können Sie die folgenden Schritte ausführen:

-   Sie können mehrere Pakete gleichzeitig ausführen, die im SSISDB-Katalog bereitgestellt werden. Weitere Informationen finden Sie unter [Ausführen von Paketen in SSIS Scale Out](run-packages-in-integration-services-ssis-scale-out.md).

-   Verwalten Sie die Scale Out-Topologie in der App für den Scale Out-Manager. Weitere Informationen finden Sie unter [Integration Services Scale Out-Manager](integration-services-ssis-scale-out-manager.md).

## <a name="next-steps"></a>Nächste Schritte
-   [Erste Schritte mit Scale Out von SQL Server Integration Services (SSIS) auf einem einzelnen Computer](get-started-with-ssis-scale-out-onebox.md)

-   [Exemplarische Vorgehensweise: Einrichten von Integration Services Scale Out](walkthrough-set-up-integration-services-scale-out.md)
