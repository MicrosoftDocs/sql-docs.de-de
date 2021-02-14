---
title: Schützen einer Master Data Manager-Webanwendung
description: In SQL Server können Sie die Master Data Manager-Webanwendung mit HTTPS sichern. Sie müssen ein Administrator sein, und auf dem Webserver muss MDS installiert sein.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: e360ba3a-e96b-4f85-b588-ed1f767fa973
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 5e941ce4f06961f19e0d5f4a0dc96aff31039647
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344754"
---
# <a name="secure-a-master-data-manager-web-application"></a>Schützen einer Master Data Manager-Webanwendung

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Sie können die [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Webanwendung per HTTPS schützen.  
  
> [!NOTE]  
>  Für die [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Webanwendung kann entweder HTTP oder HTTPS verwendet werden, jedoch nicht beides.  
  
## <a name="prerequisites"></a>Voraussetzungen  
 So führen Sie die Prozedur aus  
  
-   Sie müssen Administrator auf dem Webserver sein, auf dem [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] installiert ist.  
  
-   Auf dem Webserver muss MDS installiert sein, und es muss eine Webanwendung vorhanden sein. Weitere Informationen finden Sie unter [Installieren von Master Data Services](../../master-data-services/install-windows/install-master-data-services.md) und [Erstellen einer Master Data Manager-Webanwendung &#40;Master Data Services&#41;](../../master-data-services/install-windows/create-a-master-data-manager-web-application-master-data-services.md).  

- Der [Erweiterte IIS-Schutz für die Windows-Authentifizierung](/iis/configuration/system.webserver/security/authentication/windowsauthentication/extendedprotection/) sollte nicht aktiviert werden. 

- Konfigurieren Sie den Webserver so, dass er an allen verfügbaren IP-Adressen lauscht. Konfigurieren Sie den Webserver nicht so, dass er an einer bestimmten IP-Adresse lauscht. 

### <a name="to-secure-the-master-data-manager-web-application-with-https"></a>So schützen Sie die Master Data Manager-Webanwendung per HTTPS  
  
1.  Nachdem Sie sich vergewissert haben, dass die [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Webanwendung per HTTP ordnungsgemäß konfiguriert wurde, erstellen Sie in IIS ein Zertifikat. Weitere Informationen finden Sie unter [Konfigurieren von Serverzertifikaten in IIS 7.0](https://technet.microsoft.com/library/cc732230\(WS.10\).aspx).  
  
2.  Klicken Sie im Bereich **Verbindungen** unter **Websites** auf die Website, auf der die [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] -Webanwendung gehostet wird.  
  
3.  Klicken Sie im Bereich **Aktionen** auf **Bindungen**.  
  
4.  Klicken Sie auf **Hinzufügen**.  
  
5.  Wählen Sie in der Liste die Option **https**.  
  
6.  Wählen Sie das TLS/SSL-Zertifikat aus.  
  
7.  Klicken Sie auf **OK**.  
  
8.  Dies ist optional. Um HTTP zu entfernen, damit Benutzer nur per HTTPS auf die Website zugreifen können, klicken Sie in der Liste auf die Zeile mit **http**. Klicken Sie auf **Entfernen** , und klicken Sie im Bestätigungsdialogfeld auf **Ja**.  
  
    > [!IMPORTANT]  
    >  Nachdem HTTP entfernt wurde, müssen die basicHttp- und wsHttpBinding-Konfiguration geändert werden.  
  
9. Klicken Sie zum Schließen des Dialogfelds **Sitebindungen** auf **Schließen**.  
  
10. Öffnen Sie die Datei „web.config“ unter *Laufwerk*:\Programme\Microsoft SQL Server\130\Master Data Services\WebApplication.  
  
11. Suchen Sie nach der Zeichenfolge `<security mode="Message">` , und ändern Sie diese in `<security mode="Transport">`.  

12. Ändern Sie `<serviceMetadata httpGetEnable="true" httpsGetEnabled="false">` in `<serviceMetadata httpGetEnable="false" httpsGetEnabled="true">`, um Probleme zu vermeiden, die im Silverlight-Client auftreten können.

13. Speichern und schließen Sie die Datei. Wenn Sie einen Fehler erhalten, kann dies daran liegen, dass Sie die Benutzerkontensteuerung aktiviert haben. Benutzer sollten jetzt per HTTPS auf die Website zugreifen können.  

  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen einer Master Data Manager-Webanwendung &#40;Master Data Services&#41;](../../master-data-services/install-windows/create-a-master-data-manager-web-application-master-data-services.md)  
  
  
