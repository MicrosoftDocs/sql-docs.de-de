---
description: 'HelloData: Eine einfache ADO-Anwendung'
title: 'HelloData: eine einfache ADO-Anwendung | Microsoft-Dokumentation'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- HelloData sample application [ADO]
- ADO, samples
ms.assetid: de4bcd56-dac2-45e6-95ab-9fd7f25878fc
author: rothja
ms.author: jroth
ms.openlocfilehash: ec46ed0f94ffe9bd50d0576ab7b3855011bcf775
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033160"
---
# <a name="hellodata-a-simple-ado-application"></a>HelloData: Eine einfache ADO-Anwendung
Diese einfache Anwendung führt Sie durch jeden der vier wichtigsten ADO-Vorgänge: das erhalten, untersuchen, bearbeiten und Aktualisieren von Daten. Diese Vorgänge werden für die Northwind-Beispieldatenbank ausgeführt, die in Microsoft® SQL Server enthalten ist. Um sich auf die Grundlagen von ADO zu konzentrieren und Code Übersichtlichkeit zu verhindern, ist die Fehlerbehandlung im Beispiel minimal.  
  
### <a name="to-run-hellodata"></a>So führen Sie HelloData aus  
  
1.  Erstellen Sie eine neue Standard-exe-Visual Basic Projekt, das auf die ADO-Bibliothek verweist. Weitere Informationen finden Sie unter [verweisen auf die ADO-Bibliotheken](../referencing-the-ado-libraries.md).  
  
2.  Erstellen Sie im oberen Bereich des Formulars vier Befehls Schaltflächen, und legen Sie die Eigenschaften " **Name** " und " **Caption** " auf die Werte fest, die in der Tabelle am Ende dieses Themas angezeigt werden.  
  
3.  Fügen Sie unter den Schaltflächen ein **Microsoft DataGrid-Steuer** Element (msdatgrd. ocx) hinzu. Die Datei msdatgrd. ocx ist in Visual Basic enthalten und befindet sich im Verzeichnis "\Windows\System32" oder "\winnt\system32". Wenn Sie das DataGrid-Steuerelement dem Bereich Visual Basic Toolbox hinzufügen möchten, wählen Sie im Menü **Projekt** die Option **Komponenten...** aus. Aktivieren Sie dann das Kontrollkästchen neben "Microsoft DataGrid Control 6,0 (SP3) (OleDb)", und klicken Sie dann auf **OK**. Um dem Projekt das Steuerelement hinzuzufügen, ziehen Sie das DataGrid-Steuerelement aus der Toolbox in das Visual Basic Formular.  
  
4.  Erstellen Sie ein **Textfeld** im Formular unterhalb des Rasters, und legen Sie dessen Eigenschaften wie in der Tabelle gezeigt fest. Wenn Sie fertig sind, sollte das Formular der folgenden Abbildung ähneln.  
  
5.  Kopieren Sie schließlich den im [HelloData-Code](./hellodata-code.md)aufgelisteten Code, und fügen Sie ihn in das Fenster "Code-Editor" im Formular ein. Drücken Sie **F5** , um den Code auszuführen.  
  
> [!NOTE]
>  Im folgenden Beispiel und im gesamten Handbuch wird die Benutzer-ID "MyID" mit dem Kennwort "123aBc" verwendet, um sich beim Server zu authentifizieren. Sie sollten diese Werte durch gültige Anmelde Informationen für Ihren Server ersetzen. Ersetzen Sie außerdem den Wert "MySqlServer" durch den Namen Ihres Servers.  
  
 Eine ausführliche Beschreibung des Codes finden Sie in den [Kommentaren zu HelloData](./comments-on-hellodata.md).  
  
 ![Zeigt Form1 für die HelloData VB-Anwendung an](../../../ado/guide/data/media/hellodata.gif "HelloData")  
  
|Steuerelementtyp|Eigenschaft|Wert|  
|------------------|--------------|-----------|  
|Formular|Name|Form1|  
||Höhe|6500|  
||Breite|6500|  
|MS DataGrid|Name|grdDisplay1|  
|TextBox|Name|txtDisplay1|  
||Mehrzeilig|true|  
|Befehls Schaltfläche|Name|cmdgetdata|  
||Caption|Get Data|  
|Befehls Schaltfläche|Name|cmdexaminedata|  
||Caption|Daten überprüfen|  
|Befehls Schaltfläche|Name|cmdeditdata|  
||Caption| Bearbeiten von Daten|  
|Befehls Schaltfläche|Name|cmdupdatedata|  
||Caption|Aktualisierungsdaten|