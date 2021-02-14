---
title: Gespeicherte Überprüfungsprozedur
description: Erfahren Sie, wie Sie die gespeicherte Prozedur MDM. udpvalidatemodel verwenden, um Geschäftsregeln auf alle Elemente in der Modellversion in Master Data Services anzuwenden.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 332d3c86-4440-4f12-a6cb-ffbfbccde52c
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f7afc645a25bcedcadc925ae5bf20f3fa1b894ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351880"
---
# <a name="validation-stored-procedure-master-data-services"></a>Gespeicherte Überprüfungsprozedur (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Überprüfen Sie in [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]eine Version, um Geschäftsregeln auf alle Elemente in der Modellversion anzuwenden.  
  
 Dieses Thema erklärt, wie die gespeicherte Prozedur **mdm.udpValidateModel** verwendet wird, um Daten zu überprüfen. Wenn Sie Administrator in der [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] -Webanwendung sind, können Sie stattdessen eine Überprüfung in der Benutzeroberfläche ausführen. Weitere Informationen finden Sie unter [Überprüfen einer Version anhand von Geschäftsregeln &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md).  
  
> [!NOTE]  
>  Wenn Sie Überprüfung vor dem Abschluss des Stagingprozesses aufrufen, werden Elemente nicht überprüft, für die das Staging noch nicht beendet ist.  
  
## <a name="example"></a>Beispiel  
  
```  
DECLARE @ModelName nVarchar(50) = 'Customer'   
DECLARE @Model_id int   
DECLARE @UserName nvarchar(50)= 'DOMAIN\user_name'   
DECLARE @User_ID int   
DECLARE @Version_ID int   
  
SET @User_ID = (SELECT ID    
                 FROM mdm.tblUser u   
                 WHERE u.UserName = @UserName)   
SET @Model_ID = (SELECT Top 1 Model_ID   
                 FROM mdm.viw_SYSTEM_SCHEMA_VERSION   
                 WHERE Model_Name = @ModelName)   
SET @Version_ID = (SELECT MAX(ID)   
                 FROM mdm.viw_SYSTEM_SCHEMA_VERSION   
                 WHERE Model_ID = @Model_ID)  
  
EXECUTE mdm.udpValidateModel @User_ID, @Model_ID, @Version_ID, 1  
  
```  
  
## <a name="parameters"></a>Parameter  
 Zu dieser Prozedur gehören die folgenden Parameter:  
  
|Parameter|BESCHREIBUNG|  
|---------------|-----------------|  
|UserID|Die Benutzer-ID.|  
|Model_ID|Die Modell-ID.|  
|Version_ID|Die Versions-ID.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht: Importieren von Daten aus Tabellen &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md)   
 [Überprüfen einer Version anhand von Geschäftsregeln &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
  
