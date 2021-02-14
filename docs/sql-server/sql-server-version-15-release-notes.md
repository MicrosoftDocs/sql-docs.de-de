---
title: SQL Server 2019 Release Notes | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu den Einschränkungen von SQL Server 2019 (15.x), bekannten Problemen, Hilfsressourcen und anderen Versionshinweisen.
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: article
ms.assetid: 13942af8-5a40-4cef-80f5-918386767a47
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = sql-server-ver15
ms.openlocfilehash: e8a9f06dc33acab6862027cd2fdb55bb4aca4dfc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080431"
---
# <a name="sql-server-2019-release-notes"></a>Versionshinweise zu [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)]
[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In diesem Artikel werden Einschränkungen und bekannte Probleme für [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] beschrieben. Verwandte Informationen

> [Neues in [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)]](../sql-server/what-s-new-in-sql-server-ver15.md)

## [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]

[!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] ist das neueste öffentliche Release von [!INCLUDE[SQL Server 2019](../includes/ssnoversion-md.md)].

Ausführliche Informationen zur Lizenzierung finden Sie im Ordner `License Terms` des Installationsmediums.

## <a name="documentation"></a>Dokumentation

- **Problem und Kundenbeeinträchtigung**: [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Dokumentation kann nach Version gefiltert werden. Verwenden Sie das Steuerelement oben links auf jeder Dokumentationsseite, um nach Ihren Anforderungen zu filtern.

## <a name="build-number"></a>Buildnummer

Die RTM-Buildnummer für SQL Server 2019 lautet `15.0.2000.5`.

[!INCLUDE [sql-server-servicing-updates-version-15](../includes/sql-server-servicing-updates-version-15.md)]

## <a name="sql-server-installation-may-fail-if-ssms-18x-is-installed"></a>Die SQL Server-Installation schlägt möglicherweise fehl, wenn SSMS 18.x installiert ist

- **Problem und Kundenbeeinträchtigung:** : Die Installation von [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] schlägt fehl, wenn die folgenden Installationen in dieser Reihenfolge erfolgen:
  1. SQL Server Management Studio (SSMS) 18.0, 18.1, 18.2 oder 18.3 ist auf dem Server installiert.
  1. Die Installation von [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] wird über Wechselmedien versucht. Das Installationsmedium ist beispielsweise eine DVD.

- **Problemumgehung**:
  1. Deinstallieren Sie alle Versionen von SSMS vor SSMS 18.3.1.
  1. Installieren Sie eine neuere Version von SSMS (ab 18.3.1). Die neueste Version finden Sie unter [Herunterladen von SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md).
  1. Installieren Sie [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] normal.

- **Gilt für:** [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)]

## <a name="utf-8-collations"></a>UTF-8-Sortierungen

- **Problem und Kundenbeeinträchtigung:** UTF-8-fähige Sortierungen können nicht mit folgenden Features verwendet werden:
  - [In-Memory-OLTP-Tabellen](../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md)
  - [Always Encrypted mit Secure Enclaves](../relational-databases/security/encryption/always-encrypted-enclaves.md) (wird Secure Enclaves nicht verwendet, kann [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md) UTF-8 verwenden)

  > [!WARNING]
  > Das Erstellen einer [BACPAC-Datei](../relational-databases/data-tier-applications/data-tier-applications.md#bacpac) einer Datenbank schlägt fehl, die [CHAR- oder VARCHAR-Tabellenspalten](../t-sql/data-types/char-and-varchar-transact-sql.md) enthalten, die über 4.000 Byte verwenden.
  
  > [!NOTE]
  > Derzeit gibt es keine Benutzeroberflächenunterstützung, um UTF-8-fähige Sortierungen in Azure Data Studio oder SQL Server Data Tools (SSDT) auszuwählen. Die neueste [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]-Version (SSMS) 18 unterstützt die Auswahl von UTF-8-fähigen Sortierungen auf der Benutzeroberfläche.

- **Gilt für**: [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] RTM

## <a name="master-data-service-notification-email-contains-broken-link"></a>Benachrichtigungs-E-Mail von Master Data Services enthält fehlerhaften Link

- **Problem und Kundenbeeinträchtigung:** Die Benachrichtigungs-E-Mail von Master Data Services (MDS) enthält einen fehlerhaften Link. Über den Link gelangen Sie zu einer Seite, die etwa folgende Fehlermeldung zurückgibt:

   `The view 'Index' or its master was not found or no view engine supports the searched locations.`

- **Problemumgehung**: Öffnen Sie das MDS-Portal, und wechseln Sie manuell zu der Ressource.

- **Gilt für**: [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] RTM

## <a name="see-also"></a>Weitere Informationen

- [Hardware- und Softwareanforderungen für die Installation von SQL Server](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server-ver15.md)

## <a name="machine-learning-services"></a>Machine Learning Services

Informationen zu Problemen in SQL Server Machine Learning Services finden Sie unter [Bekannte Probleme in SQL Server Machine Learning Services](../machine-learning/troubleshooting/known-issues-for-sql-server-machine-learning-services.md).

[!INCLUDE[get-help-options-msft-only](../includes/paragraph-content/get-help-options.md)]
