---
description: 'ListReportServersInDatabase-Methode (WMI: MSReportServer_ConfigurationSetting)'
title: 'ListReportServersInDatabase-Methode (WMI: MSReportServer_ConfigurationSetting) | Microsoft-Dokumentation'
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: wmi-provider-library-reference
ms.topic: conceptual
apiname:
- ListReportServersInDatabase (WMI MSReportServer_ConfigurationSetting Class)
apilocation:
- reportingservices.mof
apitype: MOFDef
helpviewer_keywords:
- ListReportServersInDatabase method
ms.assetid: a4bf5968-c46f-484f-a510-65e2dde65a0d
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 67d5ee6e505a706248490f9fb6c74913657cd455
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88480622"
---
# <a name="configurationsetting-method---listreportserversindatabase"></a>ConfigurationSetting-Methode: ListReportServersInDatabase
  Gibt die Liste von Berichtsserverinstallationen zurück, die in der Berichtsserver-Datenbank vorhanden sind. Dies geschieht unabhängig davon, ob diese Installationen Zugriff auf sichere Informationen haben.  
  
## <a name="syntax"></a>Syntax  
  
```vb  
Public Sub ListReportServersInDatabase(ByRef MachineNames() As String, _  
    ByRef InstanceNames() As String, ByRef InstallationIDs() As String, _  
    ByRef IsInitialized() As Boolean, ByRef Length As Int32, _  
    ByRef HRESULT As Int32, ByRef ExtendedErrors() As String)  
```  
  
```csharp  
public void ListReportServersInDatabase (out string[] MachineNames,   
    out string[] InstanceNames, out string[] InstallationIDs,   
    out Boolean[] IsInitialized,out Int32 Length, out Int32 HRESULT,    
    out string[] ExtendedErrors);  
```  
  
## <a name="parameters"></a>Parameter  
 *MachineNames[]*  
 [out] Ein Array, das die Computernamen für die Berichtsserverinstallationen in der Datenbank enthält  
  
 *InstanceNames[]*  
 [aus] Ein Array mit den Instanznamen der einzelnen Berichtsserverinstallationen in der Datenbank.  
  
 *InstallationIDs[]*  
 [out] Ein Array, das die Installations-IDs für jede Berichtsserverinstallation in der Datenbank enthält  
  
 *IsInitialized[]*  
 [out] Ein Array, das den Initialisierungsstatus für jede Berichtsserverinstallation in der Datenbank enthält  
  
 *Länge*  
 [out] Die Länge der von der Methode zurückgegebenen Arrays Alle zurückgegebenen Arrays haben die gleiche Länge.  
  
 *HRESULT*  
 [out] Wert, der angibt, ob der Aufruf erfolgreich war oder zu einem Fehler geführt hat.  
  
 *ExtendedErrors[]*  
 [out] Ein Zeichenfolgenarray, das zusätzliche Fehler enthält, die durch den Aufruf zurückgegeben werden  
  
## <a name="return-value"></a>Rückgabewert  
 Gibt *HRESULT* zurück, wodurch der Erfolg oder das Fehlschlagen des Methodenaufrufs angegeben wird. Der Wert 0 (null) gibt an, dass der Methodenaufruf erfolgreich war. Ein Wert ungleich 0 (null) gibt an, dass ein Fehler aufgetreten ist.  
  
## <a name="remarks"></a>Bemerkungen  
 ListReportServersInDatabase listet die Berichtsserverinstallationen auf, die in der Berichtsserver-Datenbank vorhanden sind. Dies geschieht unabhängig davon, ob diese Installationen Zugriff auf sichere Informationen haben, und es wird ein übereinstimmender Satz von Arrays mit Informationen zu jeder Installation zurückgegeben.  
  
## <a name="requirements"></a>Requirements (Anforderungen)  
 **Namespace:** [!INCLUDE[ssRSWMInmspcA](../../includes/ssrswminmspca-md.md)]  
  
## <a name="see-also"></a>Weitere Informationen  
 [MSReportServer_ConfigurationSetting Members (MSReportServer_ConfigurationSetting-Member)](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-members.md)  
  
  
