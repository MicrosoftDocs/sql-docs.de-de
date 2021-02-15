---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 91252fc0789825beda34467d117d7a840d074acb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072766"
---
## <a name="prerequisites"></a>Voraussetzungen

Vor der Installation einer benutzerdefinierten R-Laufzeit müssen Sie Folgendes installieren:

+ Installieren Sie SQL Server 2019 für Linux. Sie könne SQL Server unter Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) und Ubuntu installieren. Weitere Informationen finden Sie im [Leitfaden für die Installation von SQL Server unter Linux](../../../linux/sql-server-linux-setup.md).

+ Führen Sie ein Upgrade auf das kumulative Update (CU) 3 oder höher für SQL Server 2019 durch. Führen Sie die folgenden Schritte aus:
    1. Konfigurieren Sie die Repositorys für kumulative Updates. Weitere Informationen finden Sie unter [Konfigurieren von Repositorys zum Installieren und Upgraden von SQL Server für Linux](../../../linux/sql-server-linux-change-repo.md).

    1. Aktualisieren Sie das Paket **mssql-server** auf das neueste kumulative Update. Weitere Informationen finden Sie [im Abschnitt „Update oder Upgrade von SQL Server“ im Leitfaden für die Installation von SQL Server unter Linux](../../../linux/sql-server-linux-setup.md#upgrade).
