---
description: Bildschirm 1 des Datenquellen-Assistenten (ODBC Driver for SQL Server)
title: Anzeige 1 des Datenquellen-Assistenten (ODBC-Treiber für SQL Server) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/27/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 78009925b5d62e8a314d0a3fdc27c29acaee5c5d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88462224"
---
# <a name="data-source-wizard-screen-1"></a>Datenquellen-Assistent (Bildschirm 1)

Geben Sie den Namen und die Beschreibung der Datenquelle und den Namen des Servers an, auf dem SQL Server ausgeführt wird und mit dem die Datenquelle eine Verbindung herstellen wird. 
    
## <a name="options"></a>Optionen

### <a name="name"></a>Name

Der von einer ODBC-Anwendung verwendete Datenquellenname, wenn sie eine Verbindung mit der Datenquelle anfordert. Beispielsweise „Personal“. Der Name der Datenquelle wird im Dialogfeld ODBC-Datenquellen-Administrator angezeigt.

### <a name="description"></a>BESCHREIBUNG

(Optional) Eine Beschreibung der Datenquelle. Zum Beispiel „Einstellungsdatum, Gehaltsverlauf und aktuelle Überprüfung aller Mitarbeiter“.

### <a name="select-or-enter-a-server-name"></a>Wählen Sie einen Servernamen aus, oder geben Sie ihn ein.

Hier geben Sie den Namen einer SQL Server-Instanz in Ihrem Netzwerk ein. Sie müssen im nächsten Eingabefeld einen Server angeben.

In den meisten Fällen kann der ODBC-Treiber eine Verbindung mit der Standardprotokollreihenfolge und dem in diesem Feld bereitgestellten Servernamen herstellen. Mit dem SQL Server-Konfigurations-Manager können Sie einen Alias für den Server erstellen oder Clientnetzwerkbibliotheken konfigurieren.

Sie können „(local)“ in das Serverfeld eingeben, wenn Sie den gleichen Computer wie SQL Server verwenden. Der Benutzer kann anschließend eine Verbindung mit der lokalen Instanz von SQL Server herstellen, selbst wenn eine nicht vernetzte Version von SQL Server ausgeführt wird. Auf einem Computer können mehrere SQL Server-Instanzen ausgeführt werden. Um eine benannte Instanz von SQL Server anzugeben, wird der Servername im Format _Servername_\\_Instanzname_ angegeben.

Weitere Informationen über Servernamen für andere Typen von Netzwerken finden Sie in der SQL Server-Installationsdokumentation in der SQL Server-Onlinedokumentation.

### <a name="finish"></a>Finish

Wenn über die auf diesem Bildschirm angegebenen Informationen hinaus keine weiteren Informationen zum Herstellen einer Verbindung mit SQL Server erforderlich sind, können Sie auf **Fertig stellen** klicken. Für alle auf anderen Bildschirmen des Assistenten angegebenen Attribute werden Standardwerte verwendet.

### <a name="next"></a>Nächste

Klicken Sie auf **Weiter**, um mit der nächsten Seite des Assistenten fortzufahren.

## <a name="next-steps"></a>Nächste Schritte

[Datenquellen-Assistent (Bildschirm 2)](../../../connect/odbc/windows/dsn-wizard-2.md)
