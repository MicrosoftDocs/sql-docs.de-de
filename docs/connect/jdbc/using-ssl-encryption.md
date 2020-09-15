---
description: Verwenden von Verschlüsselung
title: Verwenden von Verschlüsselung
ms.custom: Learn how to establish secure communication channels using TLS encryption with your SQL database connections.
ms.date: 09/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: vanto
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 8e566243-2f93-4b21-8065-3c8336649309
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 86a36cec5930630c00501796e9c2340106cfa6c9
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88414576"
---
# <a name="using-encryption"></a>Verwenden von Verschlüsselung

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Mit der TLS-Verschlüsselung (Transport Layer Security) können verschlüsselte Daten in einem Netzwerk zwischen einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und einer Clientanwendung übertragen werden.  
  
Transport Layer Security (TLS) ist ein Protokoll zum Einrichten eines sicheren Kommunikationskanals, um das Abfangen von kritischen oder vertraulichen Informationen im Netzwerk und bei anderen Formen der Internetkommunikation zu verhindern. Durch TLS können der Client und der Server gegenseitig ihre Identität authentifizieren. Nach dem Authentifizieren der Teilnehmer stellt TLS verschlüsselte Verbindungen zwischen ihnen bereit, damit die Nachrichten sicher übertragen werden können.  
  
[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] stellt eine Infrastruktur bereit, mit der die Verschlüsselung für eine bestimmte Verbindung auf Grundlage der benutzerdefinierten Verbindungseigenschaften und der Server- und Clienteinstellungen aktiviert und deaktiviert werden kann. Der Benutzer kann den Speicherort des Zertifikatspeichers und das Kennwort sowie einen Hostnamen zum Überprüfen des Zertifikats angeben und festlegen, wann der Verbindungskanal verschlüsselt werden soll.  
  
Das Aktivieren der TLS-Verschlüsselung erhöht die Sicherheit von Daten, die netzwerkübergreifend zwischen Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und Anwendungen übertragen werden. Durch das Aktivieren der Verschlüsselung kommt es jedoch zu Leistungseinbußen.  
  
In den in diesem Abschnitt aufgeführten Artikeln werden die Unterstützung der SSL-Verschlüsselung in der [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]-Version und neue Verbindungseigenschaften sowie das Konfigurieren des Vertrauensspeichers auf Clientseite beschrieben.  
  
> [!NOTE]  
> Für das Überprüfen eines TLS-Zertifikats wird die **hostNameInCertificate**-Verbindungseigenschaft empfohlen.  

## <a name="in-this-section"></a>In diesem Abschnitt  

| Thema                                                                                                        | BESCHREIBUNG                                                                                                                                           |
| ------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Grundlegendes zur Verschlüsselungsunterstützung](../../connect/jdbc/understanding-ssl-support.md)                                 | Beschreibt die Unterstützung der TLS-Verschlüsselung in [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)].                                              |
| [Herstellen von Verbindungen mit einer Verschlüsselung](../../connect/jdbc/connecting-with-ssl-encryption.md)                       | Beschreibt das Herstellen einer Verbindung mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank mithilfe der neuen TLS-spezifischen Verbindungseigenschaften. |
| [Konfigurieren des Clients für die Verschlüsselung](../../connect/jdbc/configuring-the-client-for-ssl-encryption.md) | Beschreibt das Konfigurieren des Standardvertrauensspeichers auf Clientseite und das Importieren eines privaten Zertifikats im Vertrauensspeicher des Clientcomputers.   |
  
## <a name="see-also"></a>Weitere Informationen

[Schützen von JDBC-Treiberanwendungen](../../connect/jdbc/securing-jdbc-driver-applications.md)  
