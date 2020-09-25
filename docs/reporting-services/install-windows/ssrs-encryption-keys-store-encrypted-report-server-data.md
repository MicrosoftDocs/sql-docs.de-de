---
description: Speichern verschlüsselter Berichtsserverdaten (Konfigurations-Manager)
title: Speichern verschlüsselter Berichtsserverdaten (Konfigurations-Manager) | Microsoft-Dokumentation
ms.date: 10/24/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
helpviewer_keywords:
- report servers [Reporting Services], encryption
- credentials [Reporting Services]
- cryptography [Reporting Services]
- confidential reports [Reporting Services]
- encryption [Reporting Services]
- databases [Reporting Services], encryption
ms.assetid: ac0f4d4d-fc4b-4c62-a693-b86e712e75f2
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 70ca863145814a8d0e4e2e07903a69352af2b812
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88446060"
---
# <a name="ssrs-encryption-keys---store-encrypted-report-server-data"></a>SSRS-Verschlüsselungsschlüssel: Speichern verschlüsselter Berichtsserverdaten
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] speichert verschlüsselte Werte in der Berichtsserver-Datenbank und in Konfigurationsdateien. Die meisten verschlüsselten Werte stellen Anmeldeinformationen für den Zugriff auf externe Datenquellen dar, die Daten für Berichte bereitstellen. In diesem Thema werden die verschlüsselten Werte, die in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]verwendeten Verschlüsselungsfunktionen sowie andere Arten von gespeicherten, vertraulichen Daten beschrieben, die Sie kennen sollten.  
  
## <a name="encrypted-values"></a>Verschlüsselte Werte  
 In der folgenden Liste sind die Werte beschrieben, die in einer [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Installation gespeichert werden.  
  
-   Verbindungsinformationen und Anmeldeinformationen, mit denen ein Berichtsserver eine Verbindung zu einer Berichtsserver-Datenbank herstellt, die interne Serverdaten speichert.  
  
     Diese Werte werden während der Installation oder Berichtsserverkonfiguration angegeben und verschlüsselt. Mit dem Reporting Services-Konfigurationstool oder dem Hilfsprogramm **rsconfig** können Sie die Verbindungsinformationen jederzeit aktualisieren. Die Verschlüsselung der Konfigurationseinstellungen wird mithilfe des auf dem lokalen Computer verwendeten Computerschlüssels durchgeführt, der für alle Benutzer verfügbar ist. Die verschlüsselten Verbindungsinformationen für den Berichtsserver werden in der Datei rsreportserver.config gespeichert (verschlüsselte Einstellungen sind in keiner weiteren Konfigurationsdatei gespeichert). Weitere Informationen finden Sie unter [Konfigurieren einer Verbindung mit der Berichtsserver-Datenbank &#40;SSRS-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
-   Gespeicherte Anmeldeinformationen, mit denen ein Berichtsserver eine Verbindung zu externen Datenquellen herstellt, die Daten für einen Bericht bereitstellen.  
  
     Diese Werte werden definiert, wenn Sie Datenquelleninformationen für einen Bericht konfigurieren. Sie werden als verschlüsselte Werte in einer Berichtsserver-Datenbank gespeichert. Zum Ver- und Entschlüsseln dieser Daten verwendet der Berichtsserver einen symmetrischen Schlüssel. Weitere Informationen zu gespeicherten Anmeldeinformationen finden Sie unter [Angeben der Anmeldeinformationen und Verbindungsinformationen für Berichtsdatenquellen](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).  
  
-   Ein unbeaufsichtigtes Benutzerkonto, mit dem der Berichtsserver eine Verbindung mit anderen Computern herstellt, um externe Bilddateien oder Daten abzurufen, die in einem Bericht verwendet werden.  
  
     Dieses Konto wird verwendet, wenn eine Verbindung zu einem Remotecomputer erforderlich ist und keine Anmeldeinformationen verfügbar sind, um die Verbindung herzustellen. Dieses Konto wird hauptsächlich verwendet, um die unbeaufsichtigte Berichtsverarbeitung für Berichte zu unterstützen, die für den Zugriff auf Datenquellen keine Anmeldeinformationen verwenden. Wenn Sie Berichte auf der Basis von Datenquellen erstellen, die für den Zugriff auf Daten keine Anmeldeinformationen benötigen oder verwenden, müssen Sie den Berichtsserver so konfigurieren, dass er dieses Konto verwendet.  
  
     Dieses Konto ist unter bestimmten Umständen erforderlich und kann nur mithilfe des Reporting Services-Konfigurationstools oder mit dem Hilfsprogramm **rsconfig**erstellt werden. Dieser Wert wird in der Datei rsreportserver.config gespeichert. Sie müssen das Konto manuell erstellen. Weitere Informationen zu diesem Konto und seiner Verwendung finden Sie unter [Konfigurieren des unbeaufsichtigten Ausführungskontos Ausführung (SSRS-Konfigurations-Manager)](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
-   Der symmetrische Schlüssel für die Verschlüsselung.  
  
     Dieser Wert wird während der Installation oder Serverkonfiguration erstellt und dann als verschlüsselter Wert in der Berichtsserver-Datenbank gespeichert. Der Windows-Dienst des Berichtsservers verwendet diesen Schlüssel zum Ver- und Entschlüsseln von Daten, die in der Berichtsserver-Datenbank gespeichert wurden.  
  
## <a name="encryption-functionality-in-reporting-services"></a>Verschlüsselungsfunktionen in Reporting Services  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] verwendet Kryptografiefunktionen, die Teil des Windows-Betriebssystems sind. Die symmetrische und die asymmetrische Verschlüsselung werden verwendet.  
  
 Daten in der Berichtsserver-Datenbank werden mithilfe eines symmetrischen Schlüssels verschlüsselt. Es gibt nur einen symmetrischen Schlüssel für jede Berichtsserver-Datenbank. Dieser symmetrische Schlüssel wird selbst durch den öffentlichen Schlüssel eines von Windows generierten asymmetrischen Schlüsselpaares verschlüsselt. Der private Schlüssel wird vom Berichtsserver-Windows-Dienstkonto gehalten.  
  
 In einer Berichtsserverbereitstellung für horizontales Skalieren, bei der mehrere Berichtsserverinstanzen dieselbe Berichtsserver-Datenbank verwenden, wird ein einzelner symmetrischer Schlüssel von allen Berichtsserverknoten verwendet. Jeder Knoten muss über eine Kopie des freigegebenen symmetrischen Schlüssels verfügen. Eine Kopie des symmetrischen Schlüssels wird automatisch für jeden Knoten erstellt, wenn die Bereitstellung für horizontales Skalieren konfiguriert wird. Jeder Knoten verschlüsselt seine Kopie des symmetrischen Schlüssels mithilfe des öffentlichen Schlüssels eines für das jeweilige Windows-Dienstkonto spezifischen Schlüsselpaares. Informationen zur Erstellung des symmetrischen Schlüssels für die Bereitstellung durch einzelne Instanzen oder horizontales Skalieren finden Sie unter [Initialisieren eines Berichtsservers (SSRS-Konfigurations-Manager)](../../reporting-services/install-windows/ssrs-encryption-keys-initialize-a-report-server.md).  
 
 Außerdem kann die Berichtsserver-Datenbank ab 2019 mit Transparent Data Encryption in SQL Server konfiguriert werden, um einen zusätzlichen Schutz für Ihre ruhenden Daten bereitzustellen.
  
> [!NOTE]  
>  Wenn Sie das Report Server-Windows-Dienstkonto ändern, können die asymmetrischen Schlüssel ihre Gültigkeit verlieren und Serveroperationen dadurch unterbrochen werden. Um dieses Problem zu vermeiden, verwenden Sie immer das Reporting Services-Konfigurationstool, um die Einstellungen des Dienstkontos zu ändern. Wenn Sie das Konfigurationstool verwenden, werden die Schlüssel automatisch aktualisiert. Weitere Informationen finden Sie unter [Konfigurieren des Berichtsserver-Dienstkontos &#40;SSRS-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).  
  
## <a name="other-sources-of-confidential-data"></a>Weitere Quellen vertraulicher Daten  
 Auf einem Berichtsserver werden weitere Daten gespeichert, die nicht verschlüsselt sind, aber möglicherweise vertrauliche Informationen enthalten, die Sie schützen möchten. Insbesondere Berichtsverlaufs-Momentaufnahmen und Berichtsausführungs-Momentaufnahmen enthalten Abfrageergebnisse mit Daten, die nur für autorisierte Benutzer gedacht sind. Beim Verwenden von Momentaufnahmefunktionen für Berichte mit vertraulichen Daten sollten Sie beachten, dass Benutzer, die Tabellen in einer Berichtsserver-Datenbank öffnen können, möglicherweise auf Teile eines gespeicherten Berichts zugreifen können, indem sie den Inhalt der Tabelle überprüfen.  
  
> [!NOTE]  
>  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] unterstützt weder die Zwischenspeicherung noch den Berichtsverlauf für Berichte, die Parameter basierend auf der Sicherheits-ID des Benutzers verwenden.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Konfigurieren und Verwalten von Verschlüsselungsschlüsseln &#40;SSRS-Konfigurations-Manager&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)  
  
  
