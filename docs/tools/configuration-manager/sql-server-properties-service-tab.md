---
title: SQL-Server-Eigenschaften (Registerkarte Dienst)
description: In diesem Artikel erhalten Sie Informationen zu den Optionen auf der Registerkarte „Dienst“ des Dialogfelds zu den MSSQLSERVER-Eigenschaften, z. B. der Binärpfad, der Hostname und der Startmodus.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: e4ae0c6b-6fd8-4325-b54e-1758fc659958
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 8eeb97e73ee57b3798bf1e7d7026c75e4fac092b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345990"
---
# <a name="sql-server-properties-service-tab"></a>SQL-Server-Eigenschaften (Registerkarte Dienst)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Verwenden Sie im Dialogfeld **MSSQLSERVER-Eigenschaften** die Registerkarte **Dienst** , um die folgenden Optionen anzuzeigen oder anzugeben.  
  
## <a name="options"></a>Tastatur  
 **Binärpfad**  
 Führt den Speicherort der Programmdateien auf, die von diesem Dienst verwendet werden.  
  
 **Fehlersteuerung**  
 1 steht für `SERVICE_ERROR_NORMAL`. Wenn der Dienst nicht zusammen mit dem Computer gestartet werden kann, wird der Fehler vom Startprogramm protokolliert und eine Popupmeldung angezeigt, der Startvorgang aber fortgesetzt. Dieser Wert kann nicht geändert werden.  
  
 **Exitcode**  
 Bei einem Fehler wird die dazu gehörende Nummer in diesem Feld angezeigt. Verwenden Sie diese Nummer für die Problembehandlung. Stellen Sie die Fehlernummer dem technischen Support zur Verfügung, oder durchsuchen Sie die [!INCLUDE[msCoName](../../includes/msconame-md.md)] Knowledge Base.  
  
 **HostName**  
 Zeigt den Namen des Computers oder Clusters an, auf dem der [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienst ausgeführt wird  
  
 **Name**  
 Zeigt den Anzeigenamen des Dienstes an.  
  
 **Prozess-ID**  
 Zeigt die Prozess-ID von Windows an.  
  
 **Diensttyp**  
 Zeigt den für aufrufende Prozesse bereitgestellten Diensttyp an. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden mehrere Dienste installiert.  
  
 **Startmodus**  
 Richten Sie den Dienst mit den folgenden Auswahlmöglichkeiten ein:  
  
-   Manuell: Dieser Dienst wird nicht automatisch zusammen mit dem Computer gestartet. Zum Starten des Dienstes verwenden Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Manager oder ein anderes Tool.  
  
-   Automatisch: Dieser Dienst wird zusammen mit dem Computer gestartet.  
  
-   Deaktiviert: Der Dienst kann nicht gestartet werden.  
  
 **State**  
 Zeigt an, ob dieser Dienst ausgeführt wird, angehalten oder deaktiviert ist. „ **…** “ gibt einen ausstehenden Statuswechsel an.  
  
  
