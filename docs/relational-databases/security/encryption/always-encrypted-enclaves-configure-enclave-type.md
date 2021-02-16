---
title: Konfigurieren der Secure Enclave-Instanz in SQL Server
description: Konfigurieren Sie die Secure Enclave für Always Encrypted mit Secure Enclaves in SQL Server.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: b2bd51dffb858f30e29b4a1b60e1c728afdbeb93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067255"
---
# <a name="configure-the-secure-enclave-in-sql-server"></a>Konfigurieren der Secure Enclave-Instanz in SQL Server

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Vor der Verwendung von [Always Encrypted mit Secure Enclaves](always-encrypted-enclaves.md) in SQL Server müssen Sie Ihre Instanz so konfigurieren, dass die Secure Enclave während des Starts initialisiert wird. Standardmäßig initialisiert SQL Server die Secure Enclave nicht. Sie können dies ändern, indem Sie die Serverkonfigurationsoption **column encryption enclave type** auf den Wert festlegen, der einem gültigen Enclave-Typ für Ihre Umgebung entspricht.

> [!NOTE]
> Die Rolle, die für die Konfiguration der Secure Enclave verantwortlich ist, ist der DBA. Weitere Informationen finden Sie unter [Rollen und Verantwortlichkeiten beim Konfigurieren des Nachweises mit HGS](always-encrypted-enclaves-host-guardian-service-plan.md#roles-and-responsibilities-when-configuring-attestation-with-hgs).

Der unterstützte Enclave-Typ für [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] ist VBS (virtualisierungsbasierte Sicherheit). Vor dem Konfigurieren des VBS-Enclave-Typs wird empfohlen, dass Sie den Nachweis mit dem Host-Überwachungsdienst (Host Guardian Service, HGS) für den Computer einrichten, der Ihre Instanz hostet Informationen zu den ersten Schritten mit HGS finden Sie unter [Planen des Nachweises des Host-Überwachungsdiensts](always-encrypted-enclaves-host-guardian-service-plan.md). Das Einrichten des Nachweises ermöglicht auch die virtualisierungsbasierte Sicherheit, die für eine VBS-Enclave erforderlich ist, damit sie ordnungsgemäß initialisiert werden kann. Weitere Informationen finden Sie unter [Überprüfen, ob die virtualisierungsbasierte Sicherheit ausgeführt wird](always-encrypted-enclaves-host-guardian-service-register.md#step-2-verify-virtualization-based-security-is-running).

Ausführliche Anweisungen zum Konfigurieren des Enclave-Typs finden Sie unter [Konfigurieren des Enclave-Typs für die Always Encrypted-Serverkonfigurationsoption](../../../database-engine/configure-windows/configure-column-encryption-enclave-type.md).

## <a name="next-steps"></a>Nächste Schritte

 [Verwalten von Schlüsseln für Always Encrypted mit Secure Enclaves](always-encrypted-enclaves-manage-keys.md)

## <a name="see-also"></a>Weitere Informationen  
 
 [Serverkonfigurationsoptionen (SQL Server)](../../../database-engine/configure-windows/server-configuration-options-sql-server.md)
