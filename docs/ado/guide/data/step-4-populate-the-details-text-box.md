---
description: 'Schritt 4: Auffüllen des Textfelds „Details“'
title: 'Schritt 4: füllen Sie das Textfeld "Details" auf | Microsoft-Dokumentation'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: cb4273e2-c907-4a86-a621-3bf110088228
author: rothja
ms.author: jroth
ms.openlocfilehash: 88a1b2f9487c80f6b92e2d95b1b667cf1618ba27
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036919"
---
# <a name="step-4-populate-the-details-text-box"></a>Schritt 4: Auffüllen des Textfelds „Details“
Um das Textfeld Details aufzufüllen, erstellen Sie eine neue Unterroutine namens **recfields** , und fügen Sie den folgenden Code ein:  
  
```  
Sub recFields(r As Record, l As ListBox, t As TextBox)  
    Dim f As Field  
    Dim s As Stream  
    Set s = New Stream  
    Dim str As String  
  
    For Each f In r.Fields  
        l.AddItem f.Name & ": " & f.Value  
    Next  
    t.Text = ""  
    If r!RESOURCE_CONTENTCLASS = "text/plain" Then  
        s.Open r, adModeRead, adOpenStreamFromRecord  
        str = s.ReadText(1)  
        s.Position = 0  
        If Asc(Mid(str, 1, 1)) = 63 Then '//63 = "?"  
            s.Charset = "ascii"  
            s.Type = adTypeText  
        End If  
        t.Text = s.ReadText(adReadAll)  
    End If  
End Sub  
```  
  
 Dieser Code füllt `lstDetails` die Felder und Werte des einfachen Datensatzes auf, der an weitergegeben wird `recFields` . Wenn es sich bei der Ressource um eine Textdatei handelt, wird ein Textstream aus dem Ressourcen Daten Satz geöffnet. Der Code bestimmt, ob der Zeichensatz ASCII ist, und kopiert den Datenstrom Inhalt in `txtDetails` .  
  
## <a name="see-also"></a>Weitere Informationen  
 [Szenario für die Internet Veröffentlichung](../../../ado/guide/data/internet-publishing-scenario.md)   
 [Schritt 3: Auffüllen des Listenfelds „Fields“](../../../ado/guide/data/step-3-populate-the-fields-list-box.md)
