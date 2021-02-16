---
title: 'Konfigurieren des Sicherheits-Assistenten: Auswählen von Servern'
description: Hier werden die Eigenschaften beschrieben, die sich auf der Seite „Server auswählen“ des Assistenten zum Konfigurieren der Sicherheit für die Datenbankspiegelung befinden.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.configdbmsecurwiz.choosesrvrs.f1
ms.assetid: 59e23ff3-d7ee-4e32-9629-0b54d3a258f7
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c2561f81c55ca387f8038ca8ab10f82ea6fce704
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350714"
---
# <a name="configure-database-mirroring-wizard-choose-servers-to-configure"></a>Konfigurieren des Assistenten für die Datenbankspiegelung: Auswählen des zu konfigurierenden Servers 
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Mithilfe dieser Seite können Sie angeben, welche Serverinstanzen nachfolgend konfiguriert werden sollen. Sie müssen mindestens eine Serverinstanz auswählen, bevor Sie den Assistenten fortsetzen können.  
  
 Wenn Sie ein Kontrollkästchen für eine Serverinstanz deaktivieren, nimmt der Assistent keine Änderung an dieser Instanz vor. Sie werden jedoch vom Assistenten aufgefordert, Informationen zur Instanz einzugeben und diese Informationen als Teil der Konfiguration der übrigen Serverinstanzen zu speichern. Wenn Sie beispielsweise das Kontrollkästchen für die Zeugenserverinstanz deaktivieren, werden Sie vom Assistenten aufgefordert, das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienstkonto des Zeugen einzugeben, da eine Anmeldung für dieses Konto als Teil der Sicherheitskonfiguration erstellt werden muss, die in den Prinzipal- und Spiegelserverinstanzen gespeichert wird.  
  
 **So konfigurieren Sie die Datenbankspiegelung mithilfe von SQL Server Management Studio**  
  
-   [Einrichten einer Datenbank-Spiegelungssitzung mithilfe der Windows-Authentifizierung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Starten des Assistenten zum Konfigurieren der Sicherheit für die Datenbankspiegelung &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
## <a name="options"></a>Tastatur  
 **Prinzipalserverinstanz**  
 Wählen Sie diese Option aus, um die Sicherheit für den Prinzipalserver zu konfigurieren.  
  
 **Spiegelserverinstanz**  
 Wählen Sie diese Option aus, um die Sicherheit für den Spiegelserver zu konfigurieren.  
  
 **Zeugenserverinstanz**  
 Wählen Sie diese Option aus, um die Sicherheit für den Zeugenserver zu konfigurieren (falls vorhanden).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Datenbankeigenschaften &#40;Seite Wird gespiegelt&#41;](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [Datenbankspiegelung &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
