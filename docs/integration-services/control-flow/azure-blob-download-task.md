---
description: Azure Blob-Download-Task
title: Azure-Blob-Downloadtask | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpblobdltask.f1
- sql14.dts.designer.afpblobdltask.f1
ms.assetid: 8a63bf44-71be-456d-9a5c-be7c31aff065
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 21ae3810f5f52ab8d77760cc08a887b74ab65977
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783087"
---
# <a name="azure-blob-download-task"></a>Azure Blob-Download-Task

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Der Azure-Blob-Downloadtask ermöglicht einem SSIS-Paket das Herunterladen von Dateien aus Azure Blob Storage.

Um einen **Azure Blob-Download-Task** hinzuzufügen, legen Sie ihn mittels Drag &amp; Drop auf dem SSIS-Designer ab, und doppelklicken Sie darauf, oder klicken Sie mit der rechten Maustaste, und klicken Sie dann auf **Bearbeiten** , um das folgende Dialogfeld **Azure Blob-Download-Task-Editor** anzuzeigen.  
  
 Der **Azure-Blob-Downloadtask** ist eine Komponente des [SQL Server Integration Services-Feature Packs (SSIS) für Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).  
  
 Die folgende Tabelle enthält Beschreibungen für die Felder in diesem Dialogfeld.  

|**Feld**|**Beschreibung**|  
|---|---|
|AzureStorageConnection|Geben Sie einen vorhandenen Azure Storage-Verbindungs-Manager an, oder erstellen Sie einen neuen, der sich auf ein Azure-Speicherkonto bezieht, das auf den Speicherort der Blob-Dateien verweist.|  
|BlobContainer|Gibt den Namen des Blob-Containers an, der die herunterzuladenden Blob-Dateien enthält.|  
|BlobDirectory|Gibt das Blob-Verzeichnis an, das die herunterzuladenden Blob-Dateien enthält. Das Blobverzeichnis ist eine virtuelle hierarchische Struktur.|  
|SearchRecursively|Gibt an, ob in Unterverzeichnissen rekursiv gesucht werden soll.|  
|LocalDirectory|Gibt das lokale Verzeichnis an, in dem die heruntergeladenen Blobdateien gespeichert werden sollen.|  
|FileName|Gibt einen Namensfilter an, um Dateien mit dem angegebenen Namensmuster auszuwählen. Beispielsweise enthält `MySheet*.xls\*` Dateien wie `MySheet001.xls` und `MySheetABC.xlsx`.|  
|TimeRangeFrom/TimeRangeTo|Gibt einen Zeitbereichsfilter an. Dateien, die nach **TimeRangeFrom** und vor **TimeRangeTo** geändert wurden, sind eingeschlossen.|  
