---
description: Parameterwert festlegen (Dialogfeld)
title: Parameterwert festlegen (Dialogfeld) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: ce9c2201-4e9a-4495-948f-b68deeaa7955
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f462d8b1f94b201a004272d65d08fc4902e33492
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350974"
---
# <a name="set-parameter-value-dialog-box"></a>Parameterwert festlegen (Dialogfeld)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Verwenden Sie das Dialogfeld **Parameterwert festlegen** , um Werte für Parameter und Eigenschaften des Verbindungs-Managers für Projekte und Pakete festzulegen.  
  
 **Was möchten Sie tun?**  
  
-   [Öffnen des Dialogfelds 'Parameterwert festlegen'](#open_dialog)  
  
-   [Konfigurieren der Optionen](#option)  
  
##  <a name="open-the-set-parameter-value-dialog-box"></a><a name="open_dialog"></a> Öffnen des Dialogfelds 'Parameterwert festlegen'  
  
1.  Stellen Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]eine Verbindung zum [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server her.  
  
     Sie stellen eine Verbindung mit der [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]-Instanz her, die die SSISDB-Datenbank hostet.  
  
2.  Erweitern Sie im Objekt-Explorer die Struktur, um den Knoten **Integration Services-Kataloge** anzuzeigen.  
  
3.  Erweitern Sie den **SSISDB** -Knoten.  
  
4.  Klicken Sie mit der rechten Maustaste auf ein Paket oder ein Projekt, wählen Sie **Konfigurieren**, und klicken Sie dann auf der Registerkarte **Parameter** oder auf der Registerkarte **Verbindungs-Manager** auf die Schaltfläche mit den Auslassungspunkten.  
  
##  <a name="configure-the-options"></a><a name="option"></a> Konfigurieren der Optionen  
 **Parameter**  
 Listet den Namen des Parameters auf.  
  
 **Typ**  
 Listet den Datentyp des Parameterwerts auf.  
  
 **Beschreibung**  
 Zeigt eine optionale Beschreibung für den Parameter an.  
  
 **Wert bearbeiten**  
 Verwenden Sie diese Option, um den Parameterwert zu bearbeiten.  
  
 **Standardwert aus Paket verwenden**  
 Aktivieren Sie diese Option, um den im Paket gespeicherten Standardwert für diesen Parameter zu verwenden.  
  
 **Umgebungsvariable verwenden**  
 Aktivieren Sie diese Option, um einen Variablenwert zu verwenden, der in der Umgebung gespeichert wurde, auf die vom Projekt oder Paket verwiesen wird. Um einen Umgebungsverweis zu einem Projekt oder Paket hinzuzufügen, verwenden Sie das Dialogfeld **Konfigurieren** . Weitere Informationen finden Sie unter [Configure Dialog Box](../../integration-services/catalog/configure-dialog-box.md).  
  
  
