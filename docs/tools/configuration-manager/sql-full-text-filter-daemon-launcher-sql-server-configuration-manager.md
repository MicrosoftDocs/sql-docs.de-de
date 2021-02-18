---
title: Startprogramm für SQL-Volltextfilterdaemon (SQL Server-Konfigurations-Manager)
description: In diesem Artikel erhalten Sie Informationen zum Startprogramm für SQL-Volltextfilterdaemon, einem Dienst, der von SQL Server verwendet wird, um einen Prozess zu starten, der für eine Volltextsuche erforderlich ist.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: d0dc03db-5f76-4558-b041-1ac7b9b5bb16
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 7c1886af971a4e28cccaa86efd39c96c8e5f1fc9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346007"
---
# <a name="sql-full-text-filter-daemon-launcher-sql-server-configuration-manager"></a>Startprogramm für SQL-Volltextfilterdaemon (SQL Server-Konfigurations-Manager)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Ab [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]wird der FDHOST (SQL-Volltextfilterdaemon)-Startprogrammdienst von der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Volltextsuche zum Starten des Filterdaemon-Hostprozesses verwendet, der für das Filtern bei der Volltextsuche und die Wörtertrennung verantwortlich ist. Dieser Dienst muss ausgeführt werden, damit die Volltextsuche verwendet werden kann. Der FDHOST-Startprogrammdienst ist ein instanzabhängiger Dienst, dem eine bestimmte Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]zugeordnet ist. Der FDHOST-Startprogrammdienst gibt die Dienstkontoinformationen an jeden gestarteten Filterdaemon-Hostprozess weiter. Informationen über die Prozesse des Filterdaemonhosts finden Sie unter "Architektur der Volltextsuche" in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Onlinedokumentation.  
  
  
