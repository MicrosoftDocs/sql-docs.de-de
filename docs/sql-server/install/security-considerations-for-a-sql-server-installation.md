---
title: Überlegungen zur Sicherheit
description: In diesem Artikel werden einige Best Practices zum Thema Sicherheit erläutert, die Sie vor dem Installieren und nach dem Installieren von SQL Server berücksichtigen sollten.
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- firewall systems [SQL Server]
- server message blocks [SQL Server]
- disabling protocols
- security [SQL Server], installations
- protocols [SQL Server], disabling
- NetBIOS [SQL Server]
- services [SQL Server], security
- SMB [SQL Server]
- isolating services [SQL Server]
- Setup [SQL Server], security
- low SQL Server installation privileges
- NTFS [SQL Server]
- physical security [SQL Server]
- file system security [SQL Server]
- installing SQL Server, security
ms.assetid: cf96155f-30a8-48b7-8d6b-24ce90dafdc7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5f8a540cf1f740ad2f9cb712db4bdd51f6c1a623
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336119"
---
# <a name="security-considerations-for-a-sql-server-installation"></a>Überlegungen zur Sicherheit bei SQL Server-Installationen
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

 Sicherheit ist für jedes Produkt und jedes Geschäft wichtig. Indem Sie einige einfache bewährte Methoden verwenden, können Sie viele Sicherheitsrisiken vermeiden. In diesem Artikel werden einige bewährte Sicherheitsmethoden erläutert, die Sie vor dem Installieren von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und nach dem Installieren von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] berücksichtigen sollten. Sicherheitshinweise für bestimmte Funktionen sind in den Referenzartikeln der entsprechenden Funktionen enthalten.  
  
## <a name="before-installing-ssnoversion"></a>Vor dem Installieren von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Beachten Sie beim Einrichten der Serverumgebung diese bewährten Methoden:  
  
-   [Erhöhen der physischen Sicherheit](#physical_security)  
  
-   [Verwenden von Firewalls](#firewalls)  
  
-   [Isolieren von Diensten](#isolated_services)  
  
-   [Konfigurieren eines sicheren Dateisystems](#sa_with_least_privileges)  
  
-   [Deaktivieren von NetBIOS und Server Message Block](#disabled_protocols)  
  
-   [Installieren von SQL Server auf einem Domänencontroller](../../sql-server/install/security-considerations-for-a-sql-server-installation.md#Install_DC)  
  
###  <a name="enhance-physical-security"></a><a name="physical_security"></a> Enhance Physical Security  
 Physische und logische Isolation bilden die Basis der Sicherheit von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Führen Sie die folgenden Aufgaben aus, um die physische Sicherheit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installation zu erhöhen:  
  
-   Platzieren Sie den Server in einem Raum, den nur autorisierte Benutzer betreten dürfen.  
  
-   Stellen Sie Computer, die Datenbanken hosten, an physisch geschützten Orten auf. Im Idealfall sollte dies ein verschlossener Computerraum mit Systemen für Überschwemmungsschutz und Feuererkennung bzw. Brandbekämpfung sein.  
  
-   Installieren Sie Datenbanken in der sicheren Zone des Intranets im Unternehmen, und verbinden Sie Ihre SQL Server nicht direkt mit dem Internet.  
  
-   Führen Sie regelmäßig Datensicherungen durch, und bewahren Sie die Sicherungskopien an einem Ort außerhalb des Unternehmensgebäudes auf.  
  
###  <a name="use-firewalls"></a><a name="firewalls"></a> Use Firewalls  
 Firewalls sind eine wichtige Komponente zur Sicherung der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installation. Sie bieten den wirksamsten Schutz, wenn Sie die folgenden Richtlinien beachten:  
  
-   Richten Sie zwischen Server und Internet eine Firewall ein. Aktivieren Sie die Firewall. Schalten Sie die Firewall ein, wenn sie ausgeschaltet ist. Schalten Sie die Firewall nicht aus, wenn sie eingeschaltet ist.  
  
-   Unterteilen Sie das Netzwerk in Sicherheitszonen, die durch Firewalls voneinander getrennt sind. Blockieren Sie zunächst sämtlichen Datenverkehr, und lassen Sie anschließend nur ausgewählte Verbindungen zu.  
  
-   Verwenden Sie in einer mehrstufigen Umgebung mehrere Firewalls, um Umkreisnetzwerke zu erstellen.  
  
-   Wenn Sie den Server in einer Windows-Domäne installieren, konfigurieren Sie innere Firewalls so, dass die Windows-Authentifizierung zulässig ist.  
  
-   Wenn Ihre Anwendung verteilte Transaktionen verwendet, müssen Sie die Firewall möglicherweise so konfigurieren, dass [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC)-Datenverkehr zwischen separaten MS DTC-Instanzen übermittelt werden kann. Außerdem müssen Sie die Firewall für Datenverkehr zwischen MS DTC und Ressourcen-Managern wie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]konfigurieren.  
  
 Weitere Informationen zu den Standardeinstellungen der Windows-Firewall und eine Beschreibung der TCP-Ports, die sich auf das [!INCLUDE[ssDE](../../includes/ssde-md.md)], die [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], die [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]und die [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]auswirken, finden Sie unter [Konfigurieren der Windows-Firewall für den SQL Server-Zugriff](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
###  <a name="isolate-services"></a><a name="isolated_services"></a> Isolate Services  
 Durch das Isolieren von Diensten reduzieren Sie das Risiko, dass durch einen gefährdeten Dienst andere Dienste ebenfalls gefährdet werden. Beachten Sie beim Isolieren von Diensten die folgenden Richtlinien:  
  
-   Führen Sie separate [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienste unter separaten Windows-Konten aus. Verwenden Sie für die einzelnen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienste wo immer das möglich ist separate Windows-Benutzerkonten oder separate lokale Benutzerkonten mit geringen Rechten. Weitere Informationen finden Sie unter [Konfigurieren von Windows-Dienstkonten und -Berechtigungen](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)betreffen.  
  
###  <a name="configure-a-secure-file-system"></a><a name="sa_with_least_privileges"></a> Configure a Secure File System  
 Die Verwendung des richtigen Dateisystems erhöht die Sicherheit. Folgende Aufgaben sollten für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installationen ausgeführt werden:  
  
-   Verwenden Sie das Dateisystem NTFS. NTFS ist das bevorzugte Dateisystem für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installationen, da im Gegensatz zum Dateisystem FAT eine höhere Stabilität und eine bessere Wiederherstellbarkeit gegeben ist. NTFS bietet darüber hinaus Sicherheitsfeatures wie Zugriffssteuerungslisten (ACL) für Dateien und Verzeichnisse und die Encrypting File System (EFS)-Dateiverschlüsselung. Nachdem NTFS erkannt wurde, werden während der Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entsprechende ACLs für Registrierungsschlüssel und Dateien festgelegt. Diese Berechtigungen sollten nicht geändert werden. In zukünftigen Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird die Installation auf Computern mit FAT-Dateisystemen möglicherweise nicht mehr unterstützt.  
  
    > [!NOTE]  
    >  Wenn Sie EFS verwenden, werden Datenbankdateien unter der Identität des Kontos verschlüsselt, mit dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ausgeführt wird. Die Dateien können nur mit diesem Konto entschlüsselt werden. Falls eine Änderung des Kontos erforderlich wird, unter dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ausgeführt wird, sollten Sie die Dateien zunächst unter dem alten Konto entschlüsseln und anschließend unter dem neuen Konto verschlüsseln.  
  
-   Verwenden Sie für wichtige Datendateien ein redundantes Datenträgerarray (RAID).  
  
###  <a name="disable-netbios-and-server-message-block"></a><a name="disabled_protocols"></a> Disable NetBIOS and Server Message Block  
 Für Server im Umkreisnetzwerk sollten alle nicht erforderlichen Protokolle deaktiviert sein, einschließlich NetBIOS und Server Message Block (SMB).  
  
 NetBIOS verwendet die folgenden Ports:  
  
-   UDP/137 (NetBIOS-Namensdienst)  
  
-   UDP/138 (NetBIOS-Datagrammdienst)  
  
-   TCP/139 (NetBIOS-Sitzungsdienst)  
  
 SMB verwendet die folgenden Ports:  
  
-   TCP/139  
  
-   TCP/445  
  
 Für Webserver und DNS-Server (Domain Name System) ist die Verwendung von NetBIOS oder SMB nicht erforderlich. Deaktivieren Sie auf diesen Servern beide Protokolle, um das Risiko eines Angriffs durch Benutzerenumeration zu verringern.  
  
###  <a name="installing-ssnoversion-on-a-domain-controller"></a><a name="Install_DC"></a> Installieren von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem Domänencontroller  
 Aus Sicherheitsgründen sollte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht auf einem Domänencontroller installiert werden. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Setup wird die Installation auf einem Computer, der als Domänencontroller fungiert, nicht blockieren, es gelten jedoch die folgenden Einschränkungen:  
  
-   Sie können keine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienste auf einem Domänencontroller unter einem lokalen Dienstkonto ausführen.  
  
-   Nachdem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem Computer installiert wurde, können Sie den Computer nicht von einem Domänenmitglied zu einem Domänencontroller ändern. Sie müssen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deinstallieren, bevor Sie den Hostcomputer zu einem Domänencontroller ändern.  
  
-   Nachdem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem Computer installiert wurde, können Sie den Computer nicht von einem Domänencontroller zu einem Domänenmitglied ändern. Sie müssen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deinstallieren, bevor Sie den Hostcomputer zu einem Domänenmitglied ändern.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] werden nicht unterstützt, wenn es sich bei den Clusterknoten um Domänencontroller handelt.  
  
-   Beim Setup von[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] können keine Sicherheitsgruppen erstellt oder [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienstkonten für einen schreibgeschützten Domänencontroller bereitgestellt werden. In diesem Szenario tritt ein Setupfehler auf.  
  
## <a name="during-or-after-installation-of-ssnoversion"></a>Während oder nach der Installation von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Nach der Installation können Sie die Sicherheit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Installation mit diesen bewährten Methoden bezüglich Konten und Authentifizierungsmodi verbessern:  
  
 **Dienstkonten**  
  
-   Führen Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienste mit möglichst geringen Berechtigungen aus.  
  
-   Ordnen Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienste Windows-Benutzerkonten oder lokalen Benutzerkonten mit geringen Berechtigungen zu.  
  
-   Weitere Informationen finden Sie unter [Konfigurieren von Windows-Dienstkonten und -Berechtigungen](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)betreffen.  
  
 **Authentifizierungsmodus**  
  
-   Setzen Sie die Windows-Authentifizierung für Verbindungen mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]voraus.  
  
-   Verwenden Sie Kerberos-Authentifizierung. Weitere Informationen finden Sie unter [Registrieren eines Dienstprinzipalnamens für Kerberos-Verbindungen](../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md).  
  
 **Sichere Kennwörter**  
  
-   Weisen Sie dem **sa** -Konto immer ein sicheres Kennwort zu.  
  
-   Aktivieren Sie immer die Richtlinienüberprüfung für Kennwörter, um die Sicherheit und das Ablaufdatum der Kennwörter.  
  
-   Verwenden Sie für alle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldungen sichere Kennwörter.  
  
> [!IMPORTANT]  
>  Während des Setups von [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] wird eine Anmeldung für die Gruppe BUILTIN\Users hinzugefügt. Dies ermöglicht allen authentifizierten Benutzern des Computers den Zugriff auf die Instanz von [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] als Element der öffentlichen Rolle. Die BUILTIN\Users-Anmeldung kann sicher entfernt werden, um den [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Zugriff auf Computerbenutzer zu beschränken, die eigene Logins besitzen oder Mitglieder anderer Windows-Gruppen mit Logins sind.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Hardware- und Softwareanforderungen für die Installation von SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
 [Netzwerkprotokolle und Netzwerkbibliotheken](../../sql-server/install/network-protocols-and-network-libraries.md)   
 [Registrieren eines Dienstprinzipalnamens für Kerberos-Verbindungen](../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)  
  
  
