---
title: Analysis-Server-Eigenschaften (Registerkarte Dienst)
description: In diesem Artikel erhalten Sie Informationen zu den Optionen auf der Registerkarte „Dienst“ des Dialogfelds für Eigenschaften des Analysis-Servers, z. B. der Binärpfad, der Hostname und der Startmodus.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 8dbe4bc5-6aa9-48ee-857e-0b4ea764b9cb
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: f2b058c4068f491fa7d0ceed812fd57603927f45
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349886"
---
# <a name="analysis-server-properties-service-tab"></a>Analysis-Server-Eigenschaften (Registerkarte Dienst)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Hierbei handelt es sich um den [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]-Dienst. Dieser Dienst ist für die ordnungsgemäße Ausführung von [!INCLUDE[ssAS](../../includes/ssas-md.md)] zwingend erforderlich. Die ausgegrauten Eigenschaftswerte können nicht mithilfe dieser Anwendung geändert werden.  
  
## <a name="options"></a>Tastatur  
 **Binärpfad**  
 Zeigt den Speicherort der Programmdateien an, die von diesem Dienst verwendet werden.  
  
 **Fehlersteuerung**  
 1 steht für `SERVICE_ERROR_NORMAL`. Wenn der Dienst nicht zusammen mit dem Computer gestartet werden kann, wird der Fehler vom Startprogramm protokolliert und eine Popupmeldung angezeigt, der Startvorgang aber fortgesetzt. Dieser Wert kann nicht geändert werden.  
  
 **Exitcode**  
 Bei einem Fehler wird die dazu gehörende Nummer in diesem Feld angezeigt. Verwenden Sie diese Nummer für die Problembehandlung. Stellen Sie die Fehlernummer dem technischen Support zur Verfügung, oder durchsuchen Sie die [!INCLUDE[msCoName](../../includes/msconame-md.md)] Knowledge Base.  
  
 **HostName**  
 Zeigt den Namen des Computers oder Clusters an, auf dem [!INCLUDE[ssAS](../../includes/ssas-md.md)]ausgeführt wird.  
  
 **Name**  
 Zeigt den Anzeigenamen des Dienstes an.  
  
 **Prozess-ID**  
 Zeigt die Nummer an, die von [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows zum Nachverfolgen der Prozesse dieses Programms verwendet wird.  
  
 **SQL-Diensttyp**  
 Zeigt den für aufrufende Prozesse bereitgestellten Diensttyp an. [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installiert mehrere Dienste.  
  
 **Startmodus**  
 Richten Sie den Dienst mit den folgenden Auswahlmöglichkeiten ein:  
  
-   Manuell: Dieser Dienst wird nicht automatisch zusammen mit dem Computer gestartet. Zum Starten des Dienstes verwenden Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Manager oder ein anderes Tool.  
  
-   Automatisch: Dieser Dienst wird zusammen mit dem Computer gestartet.  
  
-   Deaktiviert: Der Dienst kann nicht gestartet werden.  
  
 **State**  
 Zeigt an, ob dieser Dienst ausgeführt wird, angehalten oder deaktiviert ist.  
  
  
