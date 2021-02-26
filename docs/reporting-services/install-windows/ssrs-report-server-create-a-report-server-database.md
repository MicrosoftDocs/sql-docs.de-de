---
title: Erstellen einer Berichtsserver-Datenbank, Konfigurations-Manager | Microsoft-Dokumentation
description: Im einheitlichen Modus von SQL Server Reporting Services werden zwei relationale SQL Server-Datenbanken verwendet, um Berichtsserver-Metadaten und -Objekte zu speichern. Eine Datenbank, die als primärer Speicher dient, und eine zweite Datenbank zum Speichern temporärer Daten.
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.custom: seodec18
ms.date: 02/16/2021
ms.openlocfilehash: 927fafb66a9551332a0d5fcbff868ad79efa5eef
ms.sourcegitcommit: 129c084add904fd3f7e9ab35a800c3fd8b1a8927
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100564112"
---
# <a name="create-a-report-server-database-report-server-configuration-manager"></a>Erstellen einer Berichtsserver-Datenbank, Berichtsserver-Konfigurations-Manager  

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Im einheitlichen Modus von SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] werden zwei relationale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbanken verwendet, um Berichtsserver-Metadaten und -Objekte zu speichern. Eine Datenbank, die als primärer Speicher dient, und eine zweite Datenbank zum Speichern temporärer Daten. 

Die Datenbanken werden gemeinsam erstellt und sind durch ihre Namen aneinander gebunden. Bei einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Standardinstanz werden die Datenbanken als **reportserver** und **reportservertempdb** benannt. Zusammen werden die beiden Datenbanken als die **Berichtsserver-Datenbank** oder der **Berichtsserver-Katalog** bezeichnet.

::: moniker range="=sql-server-2016"

Der [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]SharePoint-Modus **von SQL Server** umfasst eine dritte Datenbank, die für Datenwarnungs-Metadaten verwendet wird. Die drei Datenbanken werden für jede SSRS-Dienstanwendung erstellt. Die Datenbanknamen enthalten standardmäßig eine GUID, die der Dienstanwendung entspricht. 

Im Folgenden finden Sie Beispielnamen der drei Datenbanken im SharePoint-Modus:

- ReportingService_90a9f37075544f22953c4a62e4a9f370  
  
- ReportingService_90a9f37075544f22953c4a62e4a9f370TempDB  
  
- ReportingService_90a9f37075544f22953c4a62e4a9f370_Alerting  

::: moniker-end
  
> [!IMPORTANT]  
> Schreiben Sie keine Anwendungen, die Abfragen für die Berichtsserver-Datenbank ausführen. Die Berichtsserver-Datenbank ist kein öffentliches Schema. Die Tabellenstruktur kann sich von einer Version zur nächsten ändern. Verwenden Sie für den Zugriff auf die Berichtsserver-Datenbank stets SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -APIs, wenn Sie eine Anwendung schreiben, die auf die Berichtsserver-Datenbank zugreifen muss.  
>
> Sichten des Ausführungsprotokolls sind Ausnahmen von dieser Regel. Weitere Informationen finden Sie unter [Berichtsserverausführungsprotokoll und die ExecutionLog3-Ansicht](../../reporting-services/report-server/report-server-executionlog-and-the-executionlog3-view.md).  
  
## <a name="ways-to-create-the-report-server-database"></a>Möglichkeiten zum Erstellen der Berichtsserver-Datenbank

 ### <a name="native-mode"></a>Einheitlicher Modus
 Sie können die Berichtsserver-Datenbank im einheitlichen Modus wie folgt erstellen:  
  
- **Automatisch**. Verwenden Sie den Setup-Assistenten für SQL Server, wenn Sie die Standardkonfigurationsoption für eine Installation auswählen. Im SQL Server-Installations-Assistenten ist diese Option die Option **Installieren und konfigurieren** auf der Seite **Berichtsserver-Installationsoptionen**. Wenn Sie die Option **Nur installieren** auswählen, müssen Sie die Datenbank mit dem SQL Server-Berichtsserver-Konfigurations-Manager erstellen.  
  
- **Manuell**. Verwenden Sie den SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Konfigurations-Manager. Erstellen Sie die Berichtsserver-Datenbank manuell, wenn Sie zum Hosten der Datenbank eine Remote-[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] verwenden. Weitere Informationen finden Sie unter [Erstellen einer Berichtsserver-Datenbank im einheitlichen Modus](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).  

::: moniker range="=sql-server-2016"
  
### <a name="sharepoint-mode"></a>SharePoint-Modus 
Die Seite **Berichtsserver-Installationsoptionen** hat nur eine Option für den SharePoint-Modus, **Nur installieren**. Diese Option installiert alle SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Dateien und den gemeinsamen SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-Dienst. Der nächste Schritt besteht darin, mindestens eine SSRS-Dienstanwendung auf eine der folgenden Arten zu erstellen:  
  
- Navigieren Sie zur Zentraladministration in SharePoint Server, um eine SSRS-Dienstanwendung zu erstellen. Weitere Informationen finden Sie im Abschnitt **Erstellen einer Dienstanwendung** von [Installieren des ersten Berichtsservers im SharePoint-Modus](../../reporting-services/install-windows/install-the-first-report-server-in-sharepoint-mode.md#bkmk_create_serrviceapplication).  
  
- Verwenden Sie SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]-PowerShell-Cmdlets, um eine Dienstanwendung und die Berichtsserver-Datenbanken zu erstellen. Weitere Informationen finden Sie im Beispiel für die Erstellung von Dienstanwendungen im Thema [PowerShell-Cmdlets für den SharePoint-Modus von Reporting Services](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md).  

::: moniker-end
  
## <a name="database-server-version-requirements"></a>Anforderungen hinsichtlich der Datenbankserverversion

 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird zum Hosten der Berichtsserver-Datenbanken verwendet. Die [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]-Instanz kann eine lokale oder eine Remoteinstanz sein. Mit den folgenden unterstützten Version von [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] können die Berichtsserver-Datenbanken gehostet werden:  
::: moniker range=">=sql-server-ver15"

- Verwaltete Azure SQL-Instanz

- SQL Server 2019

::: moniker-end
::: moniker range=">=sql-server-2017"

- SQL Server 2017  
::: moniker-end

- [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]  
  
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  

> [!NOTE] 
> SQL unter Linux ist keine unterstützte Umgebung zum Hosten einer SQL Server Reporting Services-Datenbank.

Wenn Sie die Berichtsserver-Datenbank auf einem Remotecomputer erstellen, konfigurieren Sie die Verbindung so, dass ein Domänenbenutzerkonto oder ein Dienstkonto mit Netzwerkzugriff verwendet wird. Wenn Sie eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Remoteinstanz verwenden, sollten Sie überlegen, welche Anmeldeinformationen der Berichtsserver für die Verbindung zur Instanz verwenden soll. Weitere Informationen finden Sie unter [Konfigurieren einer Berichtsserver-Datenbankverbindung &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
> [!IMPORTANT]  
> Der Berichtsserver und die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz, die die Berichtsserver-Datenbank hostet, können sich in verschiedenen Domänen befinden. Bei einer Internetbereitstellung ist es üblich, einen Server zu verwenden, der sich hinter einer Firewall befindet. 
>
> Wenn Sie einen Berichtsserver für Internetzugriff konfigurieren, verwenden Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldeinformationen, um eine Verbindung mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz herzustellen, die sich hinter der Firewall befindet. Schützen Sie die Verbindung, indem Sie IPSEC verwenden.  
  
## <a name="edition-requirements-for-a-database-server"></a>Editionsanforderungen für einen Datenbankserver 

 Wenn Sie eine Berichtsserver-Datenbank erstellen, können nicht alle Editionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet werden, um die Datenbank zu hosten. Weitere Informationen finden Sie unter [Anforderungen für die Berichtsserver-Datenbankserver-Edition](../reporting-services-features-supported-by-the-editions-of-sql-server-2016.md#edition-requirements-for-the-report-server-database) in [Von den SQL Server-Editionen unterstützte Reporting Services-Features](../reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).  

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich über den [Berichtsserver-Konfigurations-Manager](reporting-services-configuration-manager-native-mode.md).  

Haben Sie dazu Fragen? Stellen Sie Ihre Frage im [Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231).
