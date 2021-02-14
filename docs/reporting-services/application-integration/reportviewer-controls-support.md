---
title: Unterstützung für Report Viewer-Versionen
description: Das Steuerelement „Microsoft Report Viewer“ ist mit SQL Server Reporting Services und dem Power BI-Berichtsserver kompatibel, die sich nach der aktuellen Lifecycle-Supportrichtlinie richten.
author: maggiesMSFT
ms.custom: seo-lt-2019
ms.author: maggies
ms.reviewer: jonhp
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.topic: reference
ms.date: 12/01/2020
ms.openlocfilehash: fac7ebfc88432ef3bd8abde6c0e4e62115311520
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015640"
---
# <a name="support-for-report-viewer-current-branch-versions"></a>Unterstützung für aktuelle Branch-Versionen des Report Viewer

**_Gilt für: Microsoft Report Viewer Version 150.900.148 und höher_**

Das **Microsoft Report Viewer-Steuerelement** ist mit SQL Server Reporting Services und dem Power BI-Berichtsserver kompatibel, die sich nach der aktuellen [Microsoft Lifecycle-Richtlinien](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy) richten. Die Informationen gelten sowohl für die Version **ASP.NET** als auch für die Version **WinForms**, die über [NuGet](https://www.nuget.org/) verteilt werden. Alle veröffentlichten Versionen sind über [NuGet](https://www.nuget.org/) verfügbar. Für Patches, Features oder andere Updates wird ein Rollforward auf die neueste Version ausgeführt. Die neuesten Versionen müssen angewendet werden, damit Änderungen vorgenommen werden können. Für Report Viewer werden weiterhin **Sicherheits- und kritische Updates** durchgeführt. Bei Änderungen der Unterstützungsrichtlinien werden Sie mindestens ein Jahr im Voraus benachrichtigt.

Einen Versionsverlauf des Report Viewer-Steuerelements finden Sie unter folgenden Links:

- [Windows Forms](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.Winforms/)
- [ASP.NET Web Forms](https://www.nuget.org/packages/Microsoft.ReportingServices.ReportViewerControl.WebForms/)

## <a name="application-server-and-report-server-combinations"></a>Kombinationen aus Anwendungsserver und Berichtsserver

Einige Funktionen des Berichts-Viewer-Steuerelements basieren auf dem Standardverhalten des Betriebssystems. Daher kann es erforderlich sein, dass auf dem Client (der Anwendungsserver, über den das Berichts-Viewer-Steuerelement ausgeführt wird) und auf dem Server (der Server, über den Reporting Services ausgeführt wird) dieselbe Version ausgeführt wird. Die folgenden Kombinationen aus Anwendungsserver und Berichtsserver werden unterstützt:

| Anwendungsserver | Berichtsserver |
| :----------------- | :------ |
| Windows Server 2012 | Windows Server 2012 |
| Windows Server 2012 | Windows Server 2012 R2 |
| Windows Server 2012 R2 | Windows Server 2012 R2 |
| Windows Server 2012 R2 | Windows Server 2012 |
| Windows Server 2016 und höher | Windows Server 2016 und höher |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Berichts-Viewer-Steuerelement finden Sie unter [Integrieren von Reporting Services mit den Berichts-Viewer-Steuerelementen – Erste Schritte](integrating-reporting-services-using-reportviewer-controls-get-started.md).
