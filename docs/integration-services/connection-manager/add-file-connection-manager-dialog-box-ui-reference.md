---
description: Referenz zur Benutzeroberfläche des Dialogfelds Dateiverbindungs-Manager hinzufügen
title: Referenz zur Benutzeroberfläche des Dialogfelds „Dateiverbindungs-Manager hinzufügen“ | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.fileconnection.f1
helpviewer_keywords:
- Add File Connection Manager
ms.assetid: 9370bfb5-5993-4ad8-a9cd-2de53f320f34
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8096ccaf1fc92710e46970744a7a4f7cbe000700
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130602"
---
# <a name="add-file-connection-manager-dialog-box-ui-reference"></a>Referenz zur Benutzeroberfläche des Dialogfelds Dateiverbindungs-Manager hinzufügen

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Mithilfe des Dialogfelds **Dateiverbindungs-Manager hinzufügen** können Sie eine Verbindung zu einer Gruppe von Dateien oder Ordnern definieren.  
  
 Weitere Informationen zum Verbindungs-Manager für mehrere Dateien finden Sie unter [Multiple Files Connection Manager](../../integration-services/connection-manager/multiple-files-connection-manager.md).  
  
> [!NOTE]  
>  Die integrierten Tasks und Datenflusskomponenten in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] verwenden keinen Verbindungs-Manager für mehrere Dateien. Sie können den Verbindungs-Manager jedoch im Skripttask oder in der Skriptkomponente verwenden.  
  
## <a name="options"></a>Optionen  
 **Verwendungstyp**  
 Geben Sie den Dateityp an, der vom Verbindungs-Manager für mehrere Dateien verwendet werden soll.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Erstellen von Dateien**|Der Verbindungs-Manager erstellt die Dateien.|  
|**Vorhandene Dateien**|Der Verbindungs-Manager verwendet vorhandene Dateien.|  
|**Ordner erstellen**|Der Verbindungs-Manager erstellt die Ordner.|  
|**Vorhandene Ordner**|Der Verbindungs-Manager verwendet vorhandene Ordner.|  
  
 **Dateien / Ordner**  
 Zeigen Sie die hinzugefügten Dateien oder Ordner an, indem Sie die im Folgenden beschriebenen Schaltflächen verwenden.  
  
 **Add (Hinzufügen)**  
 Fügen Sie eine Datei hinzu, indem Sie das Dialogfeld **Dateien auswählen** verwenden, oder fügen Sie mithilfe des Dialogfelds **Ordner suchen** einen Ordner hinzu.  
  
 **Bearbeiten**  
 Wählen Sie eine Datei oder einen Ordner aus, und ersetzen Sie diese(n) dann mithilfe des Dialogfelds **Dateien auswählen** bzw. **Ordner suchen** durch eine andere Datei oder einen anderen Ordner.  
  
 **Remove**  
 Wählen Sie eine Datei oder einen Ordner aus, und klicken Sie auf die Schaltfläche **Entfernen** , um das betreffende Objekt aus der Liste zu entfernen.  
  
 **Pfeilschaltflächen**  
 Wählen Sie eine Datei oder einen Ordner aus, und verschieben Sie das Objekt dann mit den Pfeilschaltflächen nach oben oder unten, um die Reihenfolge des Zugriffs festzulegen.  
  
## <a name="see-also"></a>Siehe auch  
 [Fehler- und Meldungsreferenz von Integration Services](../../integration-services/integration-services-error-and-message-reference.md)  
  
  
