---
description: Clientzertifikatauthentifizierung für Loopbackszenarios
title: Clientzertifikatauthentifizierung für Loopbackszenarios | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: peterbae
ms.author: v-hyba
ms.openlocfilehash: 168034034e5f2abe53bd47029b99fa78c250336b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836967"
---
# <a name="client-certificate-authentication-for-loopback-scenarios"></a>Clientzertifikatauthentifizierung für Loopbackszenarios

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

In SQL Server 2016 wurde eine neue gespeicherte Prozedur mit dem Namen sp_execute_external_script (SPEES) hinzugefügt. Diese gespeicherte Prozedur ermöglicht es SQL Server, ein externes Skript außerhalb von SQL Server zu starten und auszuführen und so die Erweiterbarkeit zu verbessern. In diesem Zusammenhang wurde die Unterstützung für R- und Python-Skripts eingeführt. Beide bieten Bibliotheken, die es ermöglichen, unter Verwendung eines JDBC-Treibers eine Verbindung mit SQL Server herzustellen. Obwohl SQL Server-Instanzen unter Windows die integrierte Windows-Authentifizierung verwenden können, um diese Loopbackverbindungen mit denselben Anmeldeinformationen wie der Benutzer zu authentifizieren, der die Abfrage gestartet hat, sind SQL Server-Instanzen unter Linux nicht dazu in der Lage. Daher wird die Clientzertifikatauthentifizierung hinzugefügt, um Benutzern die Authentifizierung mit einem Zertifikat und Schlüssel zu ermöglichen.

## <a name="connecting-using-client-certificate-authentication"></a>Herstellen einer Verbindung mit der Clientzertifikatauthentifizierung

Durch den JDBC-Treiber werden dieser Funktion drei Verbindungseigenschaften hinzugefügt:

* clientCertificate: Gibt das Zertifikat an, das für die Authentifizierung verwendet werden soll. Der JDBC-Treiber unterstützt die Dateierweiterungen PFX, PEM, DER und CER.

Format
```
clientCertificate=<file_location>
``` 
Der Treiber verwendet eine Zertifikatsdatei. Für Zertifikate im PEM-, DER- und CER-Format wird das clientKey-Attribut benötigt. Der Dateispeicherort kann entweder relativ oder absolut sein.
 
* clientKey: Gibt einen Speicherort des privaten Schlüssels für PEM-, DER- und CER-Zertifikate an, die durch das clientCertificate-Attribut angegeben werden.

Format
```
clientKey=<file_location>
```
Diese Zeichenfolge gibt den Speicherort der Datei mit dem privaten Schlüssel an. Wenn die Datei mit privatem Schlüssel kennwortgeschützt ist, wird das Schlüsselwort „password“ benötigt. Der Dateispeicherort kann entweder relativ oder absolut sein.

* clientKeyPassword: eine optionale Kennwortzeichenfolge für den Zugriff auf den privaten Schlüssel der Datei „clientKey“.

Diese Funktion wird offiziell nur für Loopback-Authentifizierungsszenarios ab Linux SQL Server 2019 unterstützt.

## <a name="see-also"></a>Weitere Informationen

[Verbinden mit SQL Server mit dem JDBC-Treiber](../../connect/jdbc/connecting-to-sql-server-with-the-jdbc-driver.md)  
[sp_execute_external_script (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)
