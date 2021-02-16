---
title: Installieren von Updates an der Eingabeaufforderung | Microsoft-Dokumentation
description: In diesem Artikel wird die Befehlssyntax für die Installation von SQL Server-Updates beschrieben. Sie können Installationsskripts testen und ändern, um die Anforderungen Ihrer Organisation zu erfüllen.
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: bc98ba2b-aae9-4d01-aa85-d4c36428cb0b
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: ff4d078c165b91f147e39cecb3976939929cac7e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353061"
---
# <a name="installing-updates-from-the-command-prompt"></a>Installieren von Updates an der Eingabeaufforderung

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Testen und ändern Sie die Installationsskripts, um die Anforderungen Ihrer Organisation zu erfüllen. 
 
## <a name="sample-syntax-for-installation"></a>Beispielsyntax zur Installation 
Der Name des Updatepakets kann variieren und enthält möglicherweise Komponenten zu Sprache, Version und Prozessor. Anwenden eines Updates über eine Eingabeaufforderung. Dabei wird <Paketname> mit dem Namen des Updatepakets ersetzt: 
 
- Aktualisieren einer einzelnen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und aller freigegebenen Komponenten wie [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] und Verwaltungstools: Sie können die Instanz entweder mit dem InstanceName-Parameter oder dem InstanceID-Parameter angeben. Sie müssen den Parameter „InstanceID“ angeben, um eine vorbereitete Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu aktualisieren.

    ```
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /InstanceName=MyInstance
    ```
    oder 
    ```
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /InstanceID=\<Instance ID>. 
    ```

- Setup ist in der Lage, die neuesten Produktupdates in die Installation des Hauptprodukts zu integrieren, sodass das Hauptprodukt und geeignete Updates gleichzeitig installiert werden. Sie können eine Installation der Datenbank-Engine-Instanz dafür vorbereiten, ein Produktupdate einzuschließen: 

    ```
    setup.exe /q /IAcceptSQLServerLicenseTerms /ACTION=PrepareImage /UpdateEnabled=True /UpdateSource=\<path where the update is downloaded> /INSTANCEID=\<Instance ID> /FEATURES=SQLEngine. 
    ```

- Aktualisieren Sie nur freigegebene [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Komponenten wie [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] und Verwaltungstools: 

    ```
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch 
    ```

- Aktualisieren Sie alle Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf dem Computer und alle freigegebenen Komponenten wie [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] und Verwaltungstools: 

    ```
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /AllInstances. 
    ```

- Entfernen Sie ein Update von einer einzelnen Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und allen freigegebenen Komponenten wie [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] und Verwaltungstools: 

    ```
    <package_name>.exe /qs /Action=RemovePatch /InstanceName=MyInstance. 
    ```

- Entfernen Sie ein Update nur von freigegebenen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Komponenten wie [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] und Verwaltungstools: 

    ```
    <package_name>.exe /qs /Action=RemovePatch 
    ```

  > [!NOTE] 
  > Mit dem Updateinstallationsprogramm wird sichergestellt, dass die freigegebenen Komponenten stets mindestens die Version der Instanz auf höchster Ebene aufweisen. 
 
## <a name="supported-parameters"></a>Unterstützte Parameter 
 
> [!IMPORTANT] 
> Anmeldeinformationen sollten nach Möglichkeit zur Laufzeit angegeben werden. Wenn Sie Anmeldeinformationen in einer Skriptdatei speichern müssen, sollten Sie die Datei schützen, um nicht autorisierten Zugriff zu verhindern. 
 
|Schalter|BESCHREIBUNG| 
|------------|-----------------| 
|**/?**|Zeigt Hilfe zur unbeaufsichtigten Installation an der Eingabeaufforderung an.| 
|**/action=Patch oder /action=RemovePatch**|Gibt die Installationsaktion an: Patch oder RemovePatch.| 
|**/allinstances**|Wendet das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Update auf alle Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und auf alle freigegebenen, nicht instanzabhängigen Komponenten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an.| 
|**/instancename=Instanzname***|Wendet das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Update auf eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit dem Namen InstanceName und auf alle freigegebenen, nicht instanzabhängigen Komponenten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an.| 
|**/InstanceID=Inst1**|Wendet das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Update auf eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inst1 und auf alle freigegebenen, nicht instanzabhängigen Komponenten von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an.| 
|**/quiet**|Führt Setup für das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Update im unbeaufsichtigten Modus aus.| 
|**/qs**|Zeigt nur das Statusdialogfeld in der Benutzeroberfläche an.| 
|**/UpdateEnabled**|Gibt an, ob Produktupdates von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup ermittelt und eingeschlossen werden sollen. Gültige Werte sind True und False oder 1 und 0. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup schließt standardmäßig alle gefundenen Updates ein.| 
|**/IAcceptSQLServerLicenseTerms**|Nur erforderlich, wenn der /Q-Parameter oder der /QS-Parameter für die unbeaufsichtigte Installation angegeben wird.| 
 
 *Sie können diesen Parameter nicht angeben, um ein Update auf eine vorbereitete Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]anzuwenden. Sie müssen stattdessen den /instanceID-Parameter angeben. 
 
## <a name="see-also"></a>Weitere Informationen 
 [Übersicht über die SQL Server-Wartungsinstallation](./install-sql-server-servicing-updates.md) 
 
