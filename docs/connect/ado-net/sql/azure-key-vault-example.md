---
description: Beispiel zur Veranschaulichung der Verwendung von Always Encrypted mit einem Azure Key Vault-Anbieter
title: Beispiel zur Veranschaulichung der Verwendung von Always Encrypted mit einem Azure Key Vault-Anbieter | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: 48c9fbbe5afbc5115aa52dbb13d258e6595b6efc
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464860"
---
# <a name="example-demonstrating-use-of-azure-key-vault-provider-with-always-encrypted"></a>Beispiel zur Veranschaulichung der Verwendung von Always Encrypted mit einem Azure Key Vault-Anbieter

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

Dieses Beispiel veranschaulicht die Verwendung eines Azure Key Vault-Anbieters beim Zugriff auf verschlüsselte Spalten.

## <a name="azurekeyvaultprovider-v20"></a>AzureKeyVaultProvider v2.0+

[!code-csharp [Azure Key Vault Provider 2.0 Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderExample_2_0.cs#1)]

## <a name="azurekeyvaultprovider-v1x"></a>AzureKeyVaultProvider v1.x

[!code-csharp [Azure Key Vault Provider Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderExample.cs#1)]

> [!NOTE]
>
> - Um das Feature Always Encrypted ohne sichere Enclaves für die Anwendung .NET Standard zu verwenden, ist mindestens Version 2.1.0 von **Microsoft.Data.SqlClient** erforderlich. Die unterstützte .NET Standard-Version ist 2.0 oder höher.
>
> - Um das Feature Always Encrypted unter Linux und macOS zu verwenden, ist mindestens Version 2.1.0 von **Microsoft.Data.SqlClient** erforderlich.

## <a name="see-also"></a>Siehe auch

- [Beispiel zur Veranschaulichung der Verwendung von Always Encrypted mit Secure Enclaves mit einem Azure Key Vault-Anbieter](azure-key-vault-enclave-example.md)
- [Tutorial: Entwickeln einer .NET-Anwendung mithilfe von Always Encrypted mit Secure Enclaves](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Verwenden von Always Encrypted mit dem Microsoft .NET-Datenanbieter für SQL Server](sqlclient-support-always-encrypted.md)
