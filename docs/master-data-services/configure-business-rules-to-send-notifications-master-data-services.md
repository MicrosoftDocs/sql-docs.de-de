---
description: Konfigurieren von Geschäftsregeln für das Senden von Benachrichtigungen (Master Data Services)
title: Konfigurieren von Geschäftsregeln für das Senden von Benachrichtigungen
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], configuring notifications
- e-mail [Master Data Services], configuring business rules
- notifications [Master Data Services], configuring business rules
ms.assetid: b24f7b11-ab53-4642-999c-e17b543b3558
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 0b7bbf98bd65399a6eb2235a0acb73c28af277f9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272771"
---
# <a name="configure-business-rules-to-send-notifications-master-data-services"></a>Konfigurieren von Geschäftsregeln für das Senden von Benachrichtigungen (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  In [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]konfigurieren Sie Geschäftsregeln für das Senden von Benachrichtigungen, wenn Sie Benutzer über Änderungen von Attributwerten in Kenntnis setzen möchten.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie diese Prozedur aus  
  
-   Sie müssen über die Berechtigung für den Zugriff auf die Funktionsbereiche **Systemverwaltung** und **Benutzer- und Gruppenberechtigungen** verfügen. Wenn Sie nicht über die Berechtigung für den Funktionsbereich **Benutzer- und Gruppenberechtigungen** verfügen, können Sie die Liste der Benutzer und Gruppen nicht anzeigen, an die Benachrichtigungen gesendet werden sollen.  
  
-   Sie müssen ein Modelladministrator sein. Weitere Informationen finden Sie unter [Administratoren &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Es muss bereits eine Geschäftsregel vorhanden sein, die eine Überprüfungsaktion verwendet. Weitere Informationen finden Sie unter [Erstellen und Veröffentlichen einer Geschäftsregel &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md).  
  
-   Der Benutzer oder die Gruppe, der bzw. die die Benachrichtigung empfängt, muss mindestens die **Leseberechtigung** für das Attribut haben, dessen Überprüfung einen Fehler ergibt. Benutzer oder Gruppen, denen die Berechtigung für das Attribut explizit oder implizit verweigert wird, empfangen die E-Mail, können jedoch in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]nicht auf das Attribut zugreifen.  
  
-   Wenn eine E-Mail an eine Gruppe gesendet wird, wird die E-Mail nur Mitgliedern der Gruppe zugestellt, die auf den [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] zugegriffen haben.  
  
### <a name="to-configure-business-rules-to-send-notifications"></a>So konfigurieren Sie Geschäftsregeln für das Senden von Benachrichtigungen  
  
1.  Klicken Sie in [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]auf **Systemverwaltung**.  
  
2.  Zeigen Sie auf der Menüleiste auf **Verwalten** , und klicken Sie auf **Geschäftsregeln**.  
  
3.  Wählen Sie auf der Seite **Geschäftsregeln** in der Liste **Modell** ein Modell aus.  
  
4.  Wählen Sie in der Dropdownliste **Entität** eine Entität aus.  
  
5.  Wählen Sie in der Dropdownliste **Member Types** (Elementtypen) einen Elementtyp aus.  
  
6.  Klicken Sie im Raster auf die Zeile der Geschäftsregel, die Sie bearbeiten möchten, und klicken Sie auf **Bearbeiten**.  
  
7.  Aktivieren Sie das Kontrollkästchen **Benachrichtigungen senden** , und wählen Sie in der Dropdownliste einen Benutzer oder eine Gruppe aus, an die die E-Mail-Benachrichtigung gesendet wird.  
  
8.  Klicken Sie auf **Speichern**.  
  
9. Klicken Sie auf **Alle veröffentlichen**.  
  
10. Klicken Sie im Bestätigungsdialogfeld auf **OK**. Der Wert in der Spalte **Geschäftsregelstatus** wurde in **Aktiv** geändert. Die Spalte **Benachrichtigung** zeigt den ausgewählten Benutzer oder die Gruppe, an die die Benachrichtigung gesendet werden soll.  
  
## <a name="next-steps"></a>Nächste Schritte  
  
-   Führen Sie zum Anwenden von Geschäftsregeln auf Daten eine der folgenden Prozeduren aus:  
  
    -   [Überprüfen von bestimmten Elementen auf Geschäftsregeln &#40;Master Data Services&#41;](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)  
  
    -   [Überprüfen einer Version anhand von Geschäftsregeln &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)  
  
-   Konfigurieren Sie das E-Mail-Protokoll wie folgt:  
  
    -   [Konfigurieren von E-Mail-Benachrichtigungen &#40;Master Data Services&#41;](../master-data-services/configure-email-notifications-master-data-services.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Benachrichtigungen &#40;Master Data Services&#41;](../master-data-services/notifications-master-data-services.md)   
 [Konfigurieren von E-Mail-Benachrichtigungen &#40;Master Data Services&#41;](../master-data-services/configure-email-notifications-master-data-services.md)  
  
  
