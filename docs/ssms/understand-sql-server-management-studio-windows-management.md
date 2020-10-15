---
description: Grundlegendes zur SQL Server Management Studio-Fensterverwaltung
title: Grundlegendes zur SQL Server Management Studio-Fensterverwaltung
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- autohide [SQL Server Management Studio]
- SQL Server Management Studio [SQL Server], tool windows
- push pin [SQL Server Management Studio]
- tool windows [SQL Server Management Studio]
ms.assetid: bebf8383-dcaf-466e-84f5-63b81c9cfe52
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ae810e74288610f0d0484501c24188ec69458376
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92035919"
---
# <a name="understand-sql-server-management-studio-windows-management"></a>Grundlegendes zur SQL Server Management Studio-Fensterverwaltung
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Die Toolfenster in [!INCLUDE[msCoName](../includes/msconame_md.md)] [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] bilden ein funktionsreiches, flexibles und effizientes System für folgende Aufgaben:  
  
-   Maximieren des Benutzerarbeitsbereichs für Entwicklungs- und Verwaltungsaufgaben.  
  
-   Reduzieren der Anzahl von nicht verwendeten Fenstern, die gleichzeitig angezeigt werden.  
  
-   Einfaches Anpassen der Benutzerumgebung.  
  
Das Bearbeiten von Fenstern ist für die [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] -Umgebung äußerst wichtig. Die Benutzer können auf einfache Weise auf häufig verwendete Tools und Fenster zugreifen. Die Benutzer können steuern, wie viel Platz sie den unterschiedlichen Informationen zuordnen möchten, und die Umgebung maximiert entsprechend den verfügbaren Platz zur Bearbeitung von Abfragen. Fenster können auf dem Bildschirm an verschiedene Positionen verschoben werden. Viele Fenster können aus dem [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] -Rahmen abgedockt und herausgezogen werden. Dies ist vor allem nützlich, wenn Sie mehr als einen Bildschirm verwenden.  
  
Um den Bearbeitungsbereich bei gleichzeitiger Beibehaltung der Funktionalität zu erweitern, ist in allen Fenstern eine Funktion zum automatischen Ausblenden vorhanden, mit der das Fenster als Registerkarte auf einer Leiste am Rand der [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] -Hauptumgebung angezeigt wird. Wenn der Zeiger über eine dieser Registerkarten bewegt wird, wird das entsprechende Fenster angezeigt. Sie können das automatische Ausblenden eines Fensters umschalten, indem Sie auf die Schaltfläche **Automatisch ausblenden** klicken, die als Pin in der oberen rechten Fensterecke angezeigt wird. Darüber hinaus ist im Menü **Fenster** die Option **Alle automatisch ausblenden** vorhanden.  
  
Einige Komponenten lassen sich sowohl im Registerformatmodus konfigurieren, in dem Komponenten als Registerkarten an derselben Andockposition angezeigt werden, als auch im MDI-Modus (Multiple Document Interface), in dem jedes Dokument in einem eigenen Fenster angezeigt wird. Klicken Sie zum Konfigurieren dieser Funktion im Menü **Extras** auf **Optionen**, dann auf **Umgebung**und anschließend auf **Allgemein**.  
  
> [!IMPORTANT]  
> Wenn für einen Anmeldenamen (oder den Benutzer einer eigenständigen Datenbank) eine Verbindung hergestellt und dieser authentifiziert wird, werden von der Verbindung Identitätsinformationen zum Anmeldenamen gespeichert. Bei einer Anmeldung unter Verwendung der Windows-Authentifizierung umfassen die Informationen Angaben zur Mitgliedschaft in Windows-Gruppen. Die Identität der Anmeldung bleibt für die Dauer der Verbindung authentifiziert. Um Identitätsänderungen zu erzwingen, z. B. das Zurücksetzen eines Kennworts oder eine Änderung der Windows-Gruppenmitgliedschaft, muss die Anmeldung von der Authentifizierungsstelle (Windows oder [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]) abgemeldet und erneut angemeldet werden. Ein Mitglied der festen Serverrolle **sysadmin** oder eine beliebige Anmeldung mit der **ALTER ANY CONNECTION** -Berechtigung kann den **KILL** -Befehl verwenden, um eine Verbindung zu beenden und zu erzwingen, dass für eine Anmeldung eine erneute Verbindung hergestellt wird. [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] ist in der Lage, Verbindungsinformationen wiederzuverwenden, wenn mehrere Verbindungen mit dem Fenster Objekt-Explorer und Abfrage-Editor hergestellt werden. Schließen Sie alle Verbindungen, um eine erneute Verbindung zu erzwingen.  
  
> [!IMPORTANT]  
> Wenn für eine Anmeldung (oder den Benutzer einer eigenständigen Datenbank) eine Verbindung hergestellt und diese authentifiziert wird, werden von der Verbindung Identitätsinformationen zur Anmeldung zwischengespeichert. Bei einer Anmeldung unter Verwendung der Windows-Authentifizierung umfassen die Informationen Angaben zur Mitgliedschaft in Windows-Gruppen. Die Identität der Anmeldung bleibt für die Dauer der Verbindung authentifiziert. Um Identitätsänderungen zu erzwingen, z. B. das Zurücksetzen eines Kennworts oder eine Änderung der Windows-Gruppenmitgliedschaft, muss die Anmeldung von der Authentifizierungsstelle (Windows oder [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]) abgemeldet und erneut angemeldet werden. Ein Mitglied der festen Serverrolle **sysadmin** oder eine beliebige Anmeldung mit der **ALTER ANY CONNECTION** -Berechtigung kann den **KILL** -Befehl verwenden, um eine Verbindung zu beenden und zu erzwingen, dass für eine Anmeldung eine erneute Verbindung hergestellt wird. [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] ist in der Lage, Verbindungsinformationen wiederzuverwenden, wenn mehrere Verbindungen mit dem Fenster Objekt-Explorer und Abfrage-Editor hergestellt werden. Schließen Sie alle Verbindungen, um eine erneute Verbindung zu erzwingen.  
  
## <a name="see-also"></a>Weitere Informationen  
[Verwenden von SQL Server Management Studio](./sql-server-management-studio-ssms.md)  
[Die SQL Server Management Studio-Umgebung](../ssms/the-sql-server-management-studio-environment.md)  
