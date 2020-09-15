---
description: Aufheben der Unterdrückung von Warnungen für das Ausführen von benutzerdefinierten Berichten
title: Aufheben der Unterdrückung von Warnungen für das Ausführen von benutzerdefinierten Berichten
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], custom reports
ms.assetid: 0deed900-c910-4d12-aac0-6ab9e39eb068
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c6fd8adef633e50591088c8859fd8071c96da992
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88317046"
---
# <a name="unsuppress-run-custom-report-warnings"></a>Aufheben der Unterdrückung von Warnungen für das Ausführen von benutzerdefinierten Berichten
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Für benutzerdefinierte Berichte gibt es zwei Warndialogfelder. In diesem Thema wird beschrieben, wie die Unterdrückung der Anzeige dieser Felder in [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]aufgehoben werden kann.  
  
Standardmäßig wird das Dialogfeld **Benutzerdefinierten Bericht ausführen** vor dem Ausführen eines benutzerdefinierten Berichts angezeigt. Wenn Sie das Kontrollkästchen **Diese Meldung nicht mehr anzeigen** aktivieren, wird das Dialogfeld nicht mehr angezeigt. Standardmäßig wird das Dialogfeld **Benutzerdefinierten Bericht ausführen** auch dann angezeigt, wenn Sie einen benutzerdefinierten Bericht öffnen und dann auf einen Link klicken, um einen anderen benutzerdefinierten Bericht zu öffnen. In diesem Dialogfeld wird der vollständige Pfad zur benutzerdefinierten Drillthroughberichtsdatei angezeigt. Wenn Sie das Kontrollkästchen **Diese Meldung nicht mehr anzeigen** aktivieren, wird das Dialogfeld nicht mehr angezeigt.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Verwenden von SQL Server Management Studio  
  
#### <a name="to-unsuppress-the-main-custom-report-warning-dialog-box"></a>So heben Sie die Unterdrückung des Warndialogfelds für den benutzerdefinierten Hauptbericht auf  
  
1.  Stellen Sie eine Verbindung mit \<*Server*>\\<*Freigabe*>|\<*Drive*>\Dokumente und Einstellungen\\<UserProfile>\Anwendungsdaten\Microsoft\Microsoft SQL Server\130\Tools\Shell\reports.xml her.  
  
2.  Klicken Sie mit der rechten Maustaste auf **reports.xml**, und klicken Sie dann auf **Bearbeiten**.  
  
3.  Ändern Sie **<SuppressWarning>true\<\/SuppressWarning> in <SuppressWarning>false\<\/SuppressWarning>** .  
  
4.  Starten Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] neu.  
  
#### <a name="to-unsuppress-the-drill-through-custom-report-warning-dialog-box"></a>So heben Sie die Unterdrückung des Warndialogfelds für den benutzerdefinierten Drillthroughbericht auf  
  
1.  Stellen Sie eine Verbindung mit \<*Server*>\\<*Freigabe*>|\<*Drive*>\Dokumente und Einstellungen\\<UserProfile>\Anwendungsdaten\Microsoft\Microsoft SQL Server\130\Tools\Shell\reports.xml her.  
  
2.  Klicken Sie mit der rechten Maustaste auf **reports.xml**, und klicken Sie auf **Bearbeiten**.  
  
3.  Ändern Sie **<SuppressDrillthroughWarning>true\<\/SuppressDrillthroughWarning> in <SuppressDrillthroughWarning>false\<\/SuppressDrillthroughWarning>** .  
  
4.  Starten Sie [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] neu.  
  
## <a name="see-also"></a>Weitere Informationen  
[Benutzerdefinierte Berichte in Management Studio](../../ssms/object/custom-reports-in-management-studio.md)  
[Hinzufügen eines benutzerdefinierten Berichts zu Management Studio](../../ssms/object/add-a-custom-report-to-management-studio.md)  
[Verwenden benutzerdefinierter Berichte mit Eigenschaften von Objekt-Explorer-Knoten](../../ssms/object/use-custom-reports-with-object-explorer-node-properties.md)  
  
