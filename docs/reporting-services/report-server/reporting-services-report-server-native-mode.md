---
title: Reporting Services-Berichtsserver (einheitlicher Modus) | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie mehr über für den einheitlichen Modus konfigurierte Berichtsserver (einschließlich Informationen zu Verwaltungsinhalten, zum Verwalten einer Ressource und zum Verweisen auf eine Bildressource in einem Bericht).
ms.date: 04/21/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- administering Reporting Services
- administering [Reporting Services]
- Reporting Services, administration
ms.assetid: fa0d84e2-4c21-432c-aa7c-23517da75253
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 651abd2ca0261deae83d77a15b88840499dce957
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482235"
---
# <a name="reporting-services-report-server-native-mode"></a>Reporting Services-Berichtsserver (einheitlicher Modus)
  Ein für den einheitlichen Modus konfigurierter Berichtsserver läuft als Anwendungsserver, der alle Verarbeitungs- und Verwaltungsfunktionen ausschließlich über [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Komponenten bereitstellt.  
  
 Sie können entweder [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] oder das Webportal verwenden, um [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Berichte zu verwalten. Verwenden Sie den Konfigurations-Manager für [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , um einen Berichtsserver im einheitlichen Modus zu verwalten.  
  
 Wenn der Berichtsserver für den SharePoint-Modus konfiguriert ist, müssen Sie die Inhaltsverwaltungsseiten auf der SharePoint-Website zum Verwalten von Berichten, freigegebenen Datenquellen und anderen Berichtsserverelementen verwenden.  
  
 Dieser Artikel enthält die folgenden Informationen:  
  
-   [Zusammenfassung des einheitlichen Modus](#bkmk_sum)  
  
-   [Verwalten von Inhalt](#bkmk_managecontent)  
  
-   [Schützen und Verwalten einer Ressource](#bkmk_manageresources)  
  
-   [Verweisen auf eine Bildressource von einem Bericht](#bkmk_referenceimage)  
  
##  <a name="summary-of-native-mode"></a><a name="bkmk_sum"></a> Zusammenfassung des einheitlichen Modus  
 Eine [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Installation im einheitlichen Modus besteht aus mehreren serverseitigen Funktionen, die Sie verwalten und warten müssen. Zu den Serverfunktionen gehören folgende:  
  
-   Der Berichtsserver-Webdienst, der innerhalb des Berichtsserverdiensts ausgeführt wird.  
  
-   Die Hintergrundverarbeitungsanwendungen, die geplante Vorgänge und Berichtsübermittlung verarbeiten.  
  
-   Die Berichtsserver-Datenbank.  
  
 Wenn Sie eine [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Installation vollständig verwalten möchten, müssen Sie über die folgenden Berechtigungen verfügen:  
  
-   Mitgliedschaft in der lokalen Administratorgruppe auf dem Berichtsservercomputer. Falls Ihre Installation Serverfunktionen enthält, die auf Remotecomputern ausgeführt werden, müssen Sie auf diesen Computern über Administratorberechtigungen verfügen, wenn Sie diese Server über eine Remoteverbindung verwalten möchten.  
  
-   Datenbankadministrator-Berechtigungen für die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz, die die Datenbank hostet.  

::: moniker range="=sql-server-2016"

-   Wenn Sie [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] auf einem Domänencontroller installieren, müssen Sie der Domänenadministrator sein.  

::: moniker-end

##  <a name="managing-content"></a><a name="bkmk_managecontent"></a> Verwalten von Inhalt  
 In [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]bezieht sich die Inhaltsverwaltung auf die Verwaltung von Berichten, Modellen, Ordnern, Ressourcen und freigegebenen Datenquellen. Diese Elemente können jeweils unabhängig voneinander über Eigenschaften und Sicherheitseinstellungen verwaltet werden. Alle Elemente können an einen anderen Speicherort im Ordnernamespace des Berichtsservers verschoben werden. Zur effektiven Verwaltung der Elemente muss Ihnen bekannt sein, welche Aufgaben von einem Inhalts-Manager ausgeführt werden.  
  
> [!NOTE]  
>  Die Inhaltsverwaltung unterscheidet sich von der Berichtsserververwaltung. Weitere Informationen zum Verwalten einer Umgebung, in der ein Berichtsserver ausgeführt wird, finden Sie unter [Konfiguration und Verwaltung eines Berichtsservers (SharePoint-Modus von Reporting Services)](../../reporting-services/report-server-sharepoint/configuration-and-administration-of-a-report-server.md).  
  
 Die Inhaltsverwaltung umfasst die folgenden Aufgaben:  
  
-   Schützen der Berichtsserversite und der Elemente durch Anwenden der rollenbasierten Sicherheit von [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
-   Strukturieren der Ordnerhierarchie des Berichtsservers durch Hinzufügen, Ändern und Löschen von Ordnern.  
  
-   Festlegen von Standardwerten und Eigenschaften für Elemente, die vom Berichtsserver verwaltet werden. Beispielsweise können Sie Höchstwerte für die Speicherrichtlinien von Berichtsverläufen festlegen.  
  
-   Erstellen freigegebener Datenquellenelemente, die anstelle von berichtsspezifischen Datenquellenverbindungen verwendet werden können. Ein Verleger oder Inhalts-Manager kann eine andere als die ursprünglich für einen Bericht angegebene Datenquelle auswählen. Beispielsweise, um einen Verweis auf eine Testdatenbank durch einen Verweis auf eine Produktionsdatenbank zu ersetzen.  
  
-   Erstellen freigegebener Zeitpläne, die anstelle von berichtsspezifischen und abonnementspezifischen Zeitplänen verwendet werden können und die Verwaltung von Zeitplaninformationen vereinfachen.  
  
-   Erstellen datengesteuerter Abonnements, die Empfängerlisten durch Abrufen von Daten aus einem Datenspeicher generieren.  
  
-   Ausgleichen der Anforderungen hinsichtlich der Verarbeitung von Berichten auf dem Server und Angeben der Berichte, die bei Bedarf ausgeführt werden bzw. die aus dem Cache geladen werden.  
  
 Die Berechtigung zum Ausführen von Verwaltungsaufgaben wird über zwei vordefinierte Rollen bereitgestellt: **Systemadministrator** und **Inhalts-Manager**. Eine effektive Verwaltung des Berichtsserverinhalts erfordert, dass Sie beiden Rollen zugewiesen sind. Weitere Informationen zu diesen vordefinierten Rollen finden Sie unter [Rollen und Berechtigungen (Reporting Services)](../../reporting-services/security/roles-and-permissions-reporting-services.md).  
  
 Tools zum Verwalten von Berichtsserverinhalt schließen [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] bzw. das Webportal ein. [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ermöglicht es Ihnen, Standards festzulegen und Funktionen zu aktivieren. Das Webportal wird verwendet, um Benutzern Zugriff auf Berichtsserverelemente und -vorgänge zu gewähren und Berichte und andere Inhaltstypen sowie alle freigegebenen Elemente und Berichtsverteilungsfunktionen anzuzeigen und zu verwenden.  
  
##  <a name="securing-and-managing-a-resource"></a><a name="bkmk_manageresources"></a> Schützen und Verwalten einer Ressource  
 Eine Ressource ist ein verwaltetes Element, das auf einem Berichtsserver gespeichert wird, jedoch nicht vom Berichtsserver verarbeitet wird. In der Regel stellt eine Ressource externen Inhalt für die Benutzerberichterstattung bereit. Beispiele sind ein Bild in einer JPG-Datei oder eine HTML-Datei, die die in einem Bericht verwendeten Geschäftsregeln beschreibt. Die JPG- oder HTML-Datei wird auf dem Berichtsserver gespeichert, wobei der Berichtsserver die Datei jedoch direkt an den Browser weiterleitet, ohne sie zuerst zu verarbeiten.  
  
 Um einem Berichtsserver eine Ressource hinzuzufügen, laden Sie eine Datei hoch oder veröffentlichen sie:  
  
|Vorgang|Dateityp|  
|---------------|---------------|  
|Upload|Alle Dateien werden als Ressourcen hochgeladen, außer Berichtsdefinitions (.rdl)- und Berichtsmodell (.smdl)-Dateien.<br /><br /> Zum Hochladen einer Ressource verwenden Sie das Webportal, wenn der Berichtsserver im einheitlichen Modus ausgeführt wird, oder eine Anwendungsseite auf einer SharePoint-Website, wenn der Berichtsserver im integrierten SharePoint-Modus ausgeführt wird. Weitere Informationen finden Sie unter [Hochladen einer Datei oder eines Berichts im Berichtsserver](../../reporting-services/reports/upload-a-file-or-report-report-manager.md) oder [Hochladen von Dokumenten in eine SharePoint-Bibliothek (Reporting Services im SharePoint-Modus)](../../reporting-services/report-server-sharepoint/upload-documents-to-a-sharepoint-library-reporting-services-in-sharepoint-mode.md).|  
|Veröffentlichen|Alle Dateien in einem Projekt werden als Ressourcen hochgeladen, außer RDL-, SMDL- und RDS-Datenquellendateien. Fügen Sie zum Veröffentlichen einer Ressource einem Projekt im Berichts-Designer ein vorhandenes Objekt hinzu, und veröffentlichen Sie das Projekt dann auf einem Berichtsserver.|  
  
 Alle Ressourcen stehen als Dateien, die anschließend auf einen Berichtsserver hochgeladen werden, in einem Dateisystem bereit. Es gibt keine Einschränkungen hinsichtlich der Dateiformate, die Sie hochladen können, und die Dateigröße kann bis zu 1 GB betragen. Zum Veröffentlichen als Ressource auf einem Berichtsserver sind jedoch einige Dateitypen, die über entsprechende MIME-Typen verfügen, besser geeignet als andere. Ressourcen, die auf HTML- und JPG-Dateien basieren, werden z.B. in einem Browserfenster geöffnet, wenn der Benutzer auf die Ressource klickt. Die HTML-Datei wird als Webseite und die JPG-Datei als Bild gerendert. Andererseits können Ressourcen ohne entsprechende MIME-Typen, die auf Desktopanwendungsdateien basieren, beispielsweise im Browserfenster möglicherweise nicht gerendert werden.  
  
 Ob eine Ressource von den Benutzern eines Berichts angezeigt werden kann, hängt von den Anzeigemöglichkeiten des Browsers ab. Da Ressourcen nicht vom Berichtsserver verarbeitet werden, muss der Browser die Ansichtsfunktionen zum Rendern eines bestimmten MIME-Typs bereitstellen. Falls der Browser den Inhalt nicht rendern kann, können Benutzer, die die Ressource anzeigen, nur die allgemeinen Eigenschaften der Ressource sehen.  
  
 Ressourcen sind neben Berichten, freigegebenen Datenquellen, freigegebenen Zeitplänen und Ordnern als benannte Objekte in der Ordnerhierarchie des Berichtservers vorhanden. Sie können nach Ressourcen suchen, sie anzeigen, sichern und Einstellungen festlegen, wie es für jedes andere auf einem Berichtsserver gespeicherte Objekt möglich ist. Zum Anzeigen oder Verwalten einer Ressource muss Ihre Rollenzuweisung die Aufgaben zum Anzeigen und Verwalten von Ressourcen umfassen.  
  
##  <a name="referencing-an-image-resource-from-a-report"></a><a name="bkmk_referenceimage"></a> Verweisen auf eine Bildressource von einem Bericht  
 Ressourcen können ein Bild enthalten, auf das Sie in einem Bericht verweisen. Wenn die Berichtsanforderungen die Verwendung von externen Bildern umfassen, ziehen Sie die folgenden Vorteile beim Speichern des Bilds als Ressource in Betracht:  
  
-   Zentrale Speicherung in der Berichtsserver-Datenbank: Wenn Sie die Berichtsserver-Datenbank und ihren Inhalt auf einen anderen Computer übertragen, wird das externe Bild im Bericht beibehalten. Sie müssen keine Bilddateien nachverfolgen, die auf einem Datenträger auf unterschiedlichen Computern gespeichert sind.  
  
-   Gesichert durch Rollenzuweisungen statt Dateisystemsicherheit: Die gleichen Berechtigungen, die zum Anzeigen eines Berichts verwendet werden, können auch auf die Ressource angewendet werden. Im Gegensatz dazu müssen Sie beim Speichern des Bilds auf einem Datenträger sicherstellen, dass entweder das anonyme Benutzerkonto oder das Konto für die unbeaufsichtigte Ausführung über die Berechtigung zum Zugreifen auf die Datei verfügt.  
  
 Sie können eine Bildressource in einem Bericht verwenden, indem Sie die Bilddatei dem Projekt hinzufügen und es mit dem Bericht veröffentlichen. Nach dem Veröffentlichen des Bilds können Sie den Bildverweis im Bericht aktualisieren, sodass er auf die Ressource auf dem Berichtsserver verweist, und dann nur den Bericht erneut veröffentlichen, um die Änderungen zu speichern. Sie können anschließend das Bild unabhängig vom Bericht aktualisieren, indem Sie die Ressource erneut veröffentlichen. Der Bericht verwendet die aktuelle Version des Bilds, die auf dem Berichtsserver verfügbar ist.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Konfigurieren und Verwalten eines Berichtsservers &#40;einheitlicher SSRS-Modus&#41;](../../reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode.md)   
 [Troubleshoot a Reporting Services Installation (Problembehandlung für eine Reporting Services-Installation)](../../reporting-services/install-windows/troubleshoot-a-reporting-services-installation.md)  
  
  
