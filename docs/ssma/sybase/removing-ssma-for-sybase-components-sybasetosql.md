---
description: Entfernen von SSMA-Komponenten für Sybase (SybaseToSQL)
title: Entfernen von SSMA für Sybase-Komponenten (sybasedesql) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: aec09593-17d9-4ec2-ac56-3cd8851406fd
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 02a782e583463e5a986e3c7c1a42152fc27b14cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069943"
---
# <a name="removing-ssma-for-sybase-components-sybasetosql"></a>Entfernen von SSMA-Komponenten für Sybase (SybaseToSQL)
Wenn Sie die Migration von Datenbanken von Sybase Adaptive Server Enterprise (ASE) zu abgeschlossen haben [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , können Sie SSMA-Komponenten deinstallieren. Sie können die Client Komponenten jederzeit deinstallieren. Sie sollten das Erweiterungspaket jedoch nicht von deinstallieren, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es sei denn, Sie sind sicher, dass die migrierten Datenbanken nicht mehr Funktionen im **ssma_syb** Schema der **sysdb** -Datenbank verwenden.  
  
## <a name="uninstalling-the-ssma-for-sybase-client"></a>Deinstallieren von SSMA für den Sybase-Client  
Sie können SSMA **mithilfe der** Option Software deinstallieren.  
  
**So deinstallieren Sie SSMA**  
  
1.  Öffnen Sie in der Systemsteuerung **Programme hinzufügen/entfernen**.  
  
2.  Wählen Sie **Microsoft SQL Server Migration Assistant für Sybase aus**, und klicken Sie dann auf **Entfernen**.  
  
3.  Um zu bestätigen, dass Sie SSMA deinstallieren möchten, klicken Sie auf **Ja**.  
  
## <a name="uninstalling-the-extension-pack"></a>Deinstallieren des Erweiterungspakets  
Wenn Sie sicher sind, dass die migrierten Datenbanken keine Objekte im **sysdb.ssma_syb** Schema verwenden, können Sie das Erweiterungspaket mithilfe der Option "Software **" entfernen.**  
  
So deinstallieren Sie das Erweiterungspaket  
  
1.  Öffnen Sie in der Systemsteuerung **Programme hinzufügen/entfernen**.  
  
2.  Wählen Sie **Microsoft SQL Server Migration Assistant für das Sybase-Erweiterungspaket aus**, und klicken Sie dann auf **Entfernen**.  
  
3.  Um zu bestätigen, dass Sie das Erweiterungspaket deinstallieren möchten, klicken Sie auf **Ja**.  
  
4.  Klicken Sie auf der Seite Instanzen mit Daten Bank Skripts auf **weiter**.  
  
5.  Wählen Sie auf der Seite Verbindungsparameter die Authentifizierungsmethode aus, und klicken Sie dann auf **weiter**.  
  
    Die Windows-Authentifizierung verwendet Ihre Windows-Anmelde Informationen, um sich bei der Instanz von SQL Server anzumelden. Wenn Sie SQL Server Authentifizierung auswählen, müssen Sie einen SQL Server Anmelde Namen und ein Kennwort eingeben.  
  
6.  Klicken Sie auf der Seite Vorgang abgeschlossen auf **OK**.  
  
7.  Klicken Sie auf der Seite Fertigstellen **auf beenden.**  
  
Nachdem Sie deinstalliert haben, können Sie überprüfen, ob das **sysdb.ssma_syb** Schema und möglicherweise die gesamte **sysdb** -Datenbank mithilfe von entfernt wurde [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . Wenn Sie jedoch andere SSMA-Produkte verwenden, verwenden Sie auch die **sysdb** -Datenbank. Wenn die Datenbank vorhanden ist und Sie sicher sind, dass keine anderen Datenbanken auf Objekte in dieser Datenbank verweisen, können Sie die Datenbank trennen.  
  
## <a name="see-also"></a>Weitere Informationen  
[Installieren von SSMA für den Sybase-Client &#40;sybasedesql&#41;](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md)  
[Installieren von SSMA-Komponenten auf SQL Server &#40;sybasedesql-&#41;](../../ssma/sybase/installing-ssma-components-on-sql-server-sybasetosql.md)  
  
