---
description: Festlegen des DCOM-Datenstrom-Marshalling-Formats
title: Festlegen des DCOM-streamingformats | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- dcom stream marshaling format in rds [ADO]
ms.assetid: 46664ac5-d6e6-4457-8bae-3a98300f2a41
author: rothja
ms.author: jroth
ms.openlocfilehash: 8286ae626f4c505daa4b74e31dcc72a49e12d677
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036370"
---
# <a name="setting-dcom-stream-marshaling-format"></a>Festlegen des DCOM-Datenstrom-Marshalling-Formats
Ein Client Computer, der Komponenten aus RDS 1,5 oder früher verwendet, ist nicht mit einem Server kompatibel, der Komponenten aus RDS 2,0 oder höher verwendet. Wenn DCOM als zugrunde liegendes Protokoll verwendet wird, ist die Unterstützung für RDS 2,0 oder höher beim Transportieren von [recordsetobjekten](../../reference/ado-api/recordset-object-ado.md) effizienter. Wenn auf dem Client Komponenten aus RDS 1,5 oder einer früheren Version ausgeführt werden, können Sie den Server so einrichten, dass er mit der vorherigen RDS-Unterstützung (RDS 1,0) oder der neueren RDS-Unterstützung (RDS 2,0 oder höher) arbeitet. Legen Sie einen der folgenden Registrierungseinträge fest:  
  
> [!IMPORTANT]
>  Ab Windows 8 und Windows Server 2012 sind RDS-Server Komponenten nicht mehr im Windows-Betriebssystem enthalten (weitere Details finden Sie unter Windows 8 und [Windows Server 2012 Compatibility Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). RDS-Client Komponenten werden in einer zukünftigen Version von Windows entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Anwendungen, die RDS verwenden, sollten zu [WCF Data Service](/dotnet/framework/wcf/)migriert werden.  
  
```console
[HKEY_CLASSES_ROOT]  
\CLSID\[58ECEE30-E715-11CF-B0E3-00AA003F000F}\ADTGOptions]"MarshalFormat"="RDS10"  
```  
  
 - oder -  
  
```console
[HKEY_CLASSES_ROOT]  
\CLSID\[58ECEE30-E715-11CF-B0E3-00AA003F000F}\ADTGOptions]"MarshalFormat"="RDS20"  
```