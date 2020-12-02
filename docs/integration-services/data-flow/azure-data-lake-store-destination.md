---
description: Azure Data Lake Store Destination
title: Azure Data Lake Store Destination | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPADLSDEST.F1
- sql14.dts.designer.afpadlsdest.f1
ms.assetid: 4c4f504f-dd2b-42c5-8a20-1a8ad9a5d632
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 55a30f509074401bcf940639e9e4df14d0a58add
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127356"
---
# <a name="azure-data-lake-store-destination"></a>Azure Data Lake Store Destination

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Die Komponente **Azure Data Lake Store Destination** ermöglicht einem SSIS-Paket das Schreiben von Daten in Azure Data Lake Store. Die folgenden Dateiformate werden unterstützt: Text, Avro und ORC. 
  
 **Azure Data Lake Store Destination** ist eine Komponente des [SQL Server Integration Services-Feature Packs (SSIS) für Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).
 
> [!NOTE]
> Um sicherzustellen, dass der Azure Data Lake Store-Verbindungsmanager und die Komponenten, die ihn verwenden – das bedeutet Azure Data Lake Store Source und Azure Data Lake Store Destination – eine Verbindung zu Diensten herstellen können, stellen Sie sicher, dass Sie die neueste Version von Azure Feature Pack [hier](https://www.microsoft.com/download/details.aspx?id=49492)herunterladen. 

**Konfigurieren von Azure Data Lake Storage Destination**

1. Ziehen Sie **Azure Data Lake Store Destination** auf den Datenfluss-Designer, und doppelklicken Sie darauf, um den Code-Editor aufzurufen.  

2.  Geben Sie im Feld **Azure Data Lake Store connection manager** (Azure Data Lake Store-Verbindungs-Manager) einen vorhandenen Azure Data Lake Store-Verbindungs-Manager an, oder erstellen Sie einen neuen, der auf einen Azure Data Lake Store-Dienst verweist.  
  
    1.  Geben Sie im Feld **Dateipfad** den Namen der Datei an, in die Daten geschrieben werden sollen. Wenn diese Datei bereits vorhanden ist, wird ihr Inhalt überschrieben.  
  
    2.  Geben Sie im Feld **Dateiformat** das gewünschte Dateiformat an.  
  
       Wenn es sich um Textformat handelt, geben Sie den Wert für das **Spaltentrennzeichen** ein. Aktivieren Sie außerdem **Spaltennamen in der ersten Datenzeile** , wenn die erste Zeile in der Datei Spaltennamen enthält.  

       Wenn das ORC-Dateiformat vorliegt, ist Java erforderlich. Ausführliche Informationen finden Sie [hier](../../integration-services/azure-feature-pack-for-integration-services-ssis.md#dependency-on-java).
  
3.  Nachdem Sie die Verbindungsinformationen angegeben haben, wechseln Sie zur Seite **Spalten** , um Quellspalten zu den Zielspalten für den SSIS-Datenfluss zuzuordnen.  
