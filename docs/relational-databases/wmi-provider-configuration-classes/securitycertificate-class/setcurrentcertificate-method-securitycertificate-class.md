---
description: SetCurrentCertificate-Methode (SecurityCertificate-Klasse)
title: SetCurrentCertificate-Methode (SecurityCertificate)
ms.custom: seo-lt-2019
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: wmi
ms.topic: reference
apiname:
- SetCurrentCertificate Method (SecurityCertificate Class)
apilocation:
- sqlmgmproviderxpsp2up.mof
apitype: MOFDef
helpviewer_keywords:
- SetCurrentCertificate method
ms.assetid: 04b1a76a-932d-4824-8506-e346afe7554e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 504ee5369f193360a4e0705f9c9a1e352dbb23ff
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89545261"
---
# <a name="setcurrentcertificate-method-securitycertificate-class"></a>SetCurrentCertificate-Methode (SecurityCertificate-Klasse)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Legt das aktuelle Sicherheitszertifikat fest.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
object.SetCurrentCertificate(SHA , SQLInstance)  
```  
  
## <a name="parts"></a>Bestandteile  
 *object*  
 Ein [SecurityCertificate-Klassenobjekt](../../../relational-databases/wmi-provider-configuration-classes/securitycertificate-class/securitycertificate-class.md) , das ein Sicherheitszertifikat darstellt.  
  
#### <a name="parameters"></a>Parameter  
  
|Parameter|BESCHREIBUNG|  
|---------------|-----------------|  
|*SHA*|Ein Zeichenfolgenwert, der den Secure Hash Algorithm (SHA)-Fingerabdruck für das erforderliche Sicherheitszertifikat angibt.|  
|*SQLInstance*|Ein Zeichenfolgenwert, der die Instanz angibt, für die das Zertifikat erforderlich ist.|  
  
## <a name="property-valuereturn-value"></a>Eigenschaftswert/Rückgabewert  
 Ein uint32-Wert, der 0 beträgt, wenn der Dienst erfolgreich geändert wurde. Der Wert beträgt 1, wenn die Anforderung nicht unterstützt wird; jede andere Zahl gibt einen Fehler an.  
  
## <a name="remarks"></a>Hinweise  
  
## <a name="see-also"></a>Weitere Informationen  
 [Konfigurieren von Servernetzwerkprotokollen und Netzwerkbibliotheken](https://msdn.microsoft.com/library/ms177485\(v=sql.100\).aspx)  
  
  
