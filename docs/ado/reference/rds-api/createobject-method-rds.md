---
description: CreateObject-Methode (RDS)
title: Methode "kreateobject" (RDS) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- CreateObject method [ADO]
ms.assetid: dec96be6-0b31-4953-9c9a-e962b5afcd18
author: rothja
ms.author: jroth
ms.openlocfilehash: 1808130ed87bf3448c9aad12b7dac92244bfcd68
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053271"
---
# <a name="createobject-method-rds"></a>CreateObject-Methode (RDS)
Erstellt den Proxy für das Ziel Geschäftsobjekt und gibt einen Zeiger darauf zurück. Der Proxy verpackt und Marshalls Daten zum serverseitigen Stub für die Kommunikation mit dem Geschäftsobjekt, um Anforderungen und Daten über das Internet zu senden. Für in-Process-Komponenten Objekte werden keine Proxys verwendet. es wird lediglich ein Zeiger auf das-Objekt bereitgestellt.  
  
> [!IMPORTANT]
>  Ab Windows 8 und Windows Server 2012 sind RDS-Server Komponenten nicht mehr im Windows-Betriebssystem enthalten (weitere Details finden Sie unter Windows 8 und [Windows Server 2012 Compatibility Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). RDS-Client Komponenten werden in einer zukünftigen Version von Windows entfernt. Nutzen Sie diese Funktionen bei Neuentwicklungen nicht mehr, und planen Sie die Änderung von Anwendungen, die diese Funktion zurzeit verwenden. Anwendungen, die RDS verwenden, sollten zu [WCF Data Service](/dotnet/framework/wcf/)migriert werden.  
  
## <a name="syntax"></a>Syntax  
 Der Remote Datendienst unterstützt die folgenden Protokolle: http, HTTPS (http über Secure Socket Layer), DCOM und in-Process.  
  
|Protocol|Syntax|  
|--------------|------------|  
|HTTP|Set Object = DataSpace. kreateobject ("ProgID", "HTTPS \: //awebsrvr")|  
|HTTPS|Set Object = DataSpace. kreateobject ("ProgID", "HTTPS \: //awebsrvr")|  
|DCOM|Set Object = DataSpace. kreateobject ("ProgID", "Computername")|  
|In-Process|Set Object = DataSpace. kreateobject ("ProgID", "")|  
  
## <a name="parameters"></a>Parameter  
 *Object*  
 Eine Objekt Variable, die ein Objekt ergibt, bei dem es sich um den in *ProgID* angegebenen Typ handelt.  
  
 *DataSpace*  
 Eine Objekt Variable, die einen [RDS darstellt. DataSpace](./dataspace-object-rds.md) -Objekt, das verwendet wird, um eine Instanz des neuen-Objekts zu erstellen.  
  
 *ProgID*  
 Ein **Zeichen** folgen Wert, der den programmatischen Bezeichner enthält, der ein serverseitiges Geschäftsobjekt angibt, das die Geschäftsregeln Ihrer Anwendung implementiert.  
  
 *awebsrvr* oder *Computername*  
 Ein **Zeichen** folgen Wert, der eine URL darstellt, die den Internetinformationsdienste (IIS)-Webserver identifiziert, auf dem eine Instanz des Server Geschäftsobjekts erstellt wird.  
  
## <a name="remarks"></a>Bemerkungen  
 Das *http-Protokoll* ist das Standardweb Protokoll. *Https* ist ein sicheres Webprotokoll. Verwenden Sie das *DCOM-Protokoll* , wenn Sie ein lokales Netzwerk ohne http ausführen. Das *in-Process-* Protokoll ist eine lokale Dynamic Link Library (dll). Es wird kein Netzwerk verwendet.  
  
## <a name="applies-to"></a>Gilt für  
 [DataSpace-Objekt (RDS)](./dataspace-object-rds.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [DataFactory-Objekt, Abfrage Methode und Methode der Beispiel Methode (VBScript)](./datafactory-object-query-method-and-createobject-method-example-vbscript.md)   
 [DataSpace-Objekt und Beispiel für eine kreateobject-Methode (VBScript)](./dataspace-object-and-createobject-method-example-vbscript.md)   
 [CreateRecordset-Methode (RDS)](./createrecordset-method-rds.md)