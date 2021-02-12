---
description: Mode-Eigenschaft (ADO)
title: Mode-Eigenschaft (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::Mode
- _Stream::Mode
- _Record::Mode
helpviewer_keywords:
- Mode property [ADO]
ms.assetid: 808661eb-0d7c-4e6d-8e40-9dc3bef3d77a
author: rothja
ms.author: jroth
ms.openlocfilehash: ae638bd51ea34a81efa5fb3192592fa8f3a005c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041780"
---
# <a name="mode-property-ado"></a>Mode-Eigenschaft (ADO)
Gibt die verfügbaren Berechtigungen zum Ändern von Daten in einem [Verbindungs](./connection-object-ado.md)-, [Datensatz](./record-object-ado.md)-oder [Streamobjekt](./stream-object-ado.md) an.  
  
## <a name="settings-and-return-values"></a>Einstellungen und Rückgabewerte  
 Legt einen [connectmodeenum](./connectmodeenum.md) -Wert fest oder gibt ihn zurück. Der Standardwert für eine **Verbindung** ist **adModeUnknown**. Der Standardwert für ein **Datensatz** -Objekt ist **admoderead**. Der Standardwert für einen **Stream** , der einer zugrunde liegenden Quelle (geöffnet mit einer URL als Quelle oder als Standarddaten **Strom** eines **Datensatzes**) zugeordnet ist, ist **admoderead**. Der Standardwert für einen Daten **Strom** , der keiner zugrunde liegenden Quelle (instanziiert im Arbeitsspeicher) zugeordnet ist, ist **adModeUnknown**.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die **Mode** -Eigenschaft, um die Zugriffsberechtigungen festzulegen oder zurückzugeben, die vom Anbieter für die aktuelle Verbindung verwendet werden. Die **Mode** -Eigenschaft kann nur festgelegt werden, wenn das **Verbindungs** Objekt geschlossen wird.  
  
 Bei einem **Streamobjekt** , wenn der Zugriffsmodus nicht angegeben ist, wird es von der Quelle geerbt, die zum Öffnen des Daten **Strom** Objekts verwendet wird. Wenn beispielsweise ein **Stream** von einem **Datensatz** -Objekt aus geöffnet wird, wird er standardmäßig im gleichen Modus wie der **Datensatz** geöffnet.  
  
 Diese Eigenschaft ist Lese-/Schreibzugriff, wenn das Objekt geschlossen und schreibgeschützt ist, während das-Objekt geöffnet ist.  
  
> [!NOTE]
>  **Verwendung von Remote Datendiensten** Wenn die **Mode** -Eigenschaft für ein Client seitiges **Verbindungs** Objekt verwendet wird, kann Sie nur auf **adModeUnknown** festgelegt werden.  
  
## <a name="applies-to"></a>Gilt für  

:::row:::
    :::column:::
        [Connection-Objekt (ADO)](./connection-object-ado.md)  
    :::column-end:::
    :::column:::
        [Record-Objekt (ADO)](./record-object-ado.md)  
    :::column-end:::
    :::column:::
        [Stream-Objekt (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Weitere Informationen  
 [IsolationLevel-und Mode-Eigenschaften (Beispiel) (VB)](./isolationlevel-and-mode-properties-example-vb.md)   
 [IsolationLevel-und Mode-Eigenschaften (Beispiel) (VC + +)](./isolationlevel-and-mode-properties-example-vc.md)