---
title: Aktivieren von verschlüsselten Verbindungen | Microsoft-Dokumentation
ms.custom: contperf-fy20q4
ms.date: 08/29/2019
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
description: Verschlüsseln von Daten über Kommunikationskanäle Hier erfahren Sie, wie Sie verschlüsselte Verbindungen für eine Instanz der SQL Server-Datenbank-Engine mithilfe des SQL Server-Konfigurations-Managers aktivieren.
helpviewer_keywords:
- connections [SQL Server], encrypted
- SSL [SQL Server]
- Secure Sockets Layer (SSL)
- TLS [SQL Server]
- Transport Layer Security (TLS)
- encryption [SQL Server], connections
- cryptography [SQL Server], connections
- certificates [SQL Server], installing
- requesting encrypted connections
- installing certificates
- security [SQL Server], encryption
- TLS certificates
ms.assetid: e1e55519-97ec-4404-81ef-881da3b42006
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 4139b8b53a50aa58329f6a5a5c490d99ee9f0524
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186254"
---
# <a name="enable-encrypted-connections-to-the-database-engine"></a>Aktivieren von verschlüsselten Verbindungen zur Datenbank-Engine

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Hier erfahren Sie, wie Sie Daten über Kommunikationskanäle verschlüsseln.  Aktivieren Sie verschlüsselte Verbindungen für eine Instanz der [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], und geben Sie mithilfe des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Konfigurations-Managers ein Zertifikat an.
 
 Der Servercomputer muss über ein Zertifikat verfügen. [Importieren Sie das Zertifikat in Windows](#single-server), um es für den Servercomputer bereitzustellen. Der Clientcomputer muss so eingerichtet werden, dass er die [Stammzertifizierungsstelle des Zertifikats als vertrauenswürdig einstuft](#about).  
  
> [!IMPORTANT]
> Seit [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] wird Secure Sockets Layer (SSL) nicht mehr unterstützt. Verwenden Sie stattdessen Transport Layer Security (TLS).

## <a name="transport-layer-security-tls"></a>Transport Layer Security (TLS)

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] kann Transport Layer Security (TLS) zum Verschlüsseln der Daten verwendet werden, die über ein Netzwerk zwischen einer Clientanwendung und einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] übertragen werden. Die TLS-Verschlüsselung erfolgt auf Protokollebene und steht allen unterstützten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Clients zur Verfügung.

TLS kann für die Serverüberprüfung verwendet werden, wenn die Verschlüsselung von einer Clientverbindung angefordert wird. Wird die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem Computer ausgeführt, dem ein Zertifikat von einer öffentlichen Zertifizierungsstelle zugewiesen wurde, wird die Identität des Computers und der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] durch die Zertifikatkette sichergestellt, die zur vertrauenswürdigen Stammzertifizierungsstelle führt. Für diese Serverüberprüfung wird vorausgesetzt, dass der Computer, auf dem die Clientanwendung ausgeführt wird, so konfiguriert ist, dass der Stammzertifizierungsstelle des vom Server verwendeten Zertifikats vertraut wird. 

Die Verschlüsselung mit einem selbstsignierten Zertifikat ist möglich und wird im folgenden Abschnitt beschrieben, selbstsignierte Zertifikate bieten jedoch nur begrenzt Schutz.
Die von TLS verwendete Verschlüsselungsstufe, 40 Bit oder 128 Bit, hängt von der Version des Microsoft Windows-Betriebssystems ab, unter der die Anwendungs- und Datenbankcomputer ausgeführt werden.

> [!WARNING]
> Die Verwendung der 40-Bit-Verschlüsselungsstufe gilt als unsicher.

> [!WARNING]
> TLS-Verbindungen, die mithilfe eines selbstsignierten Zertifikats verschlüsselt werden, bieten keine hohe Sicherheit. Sie sind anfällig für Man-in-the-Middle-Angriffe. In einer Produktionsumgebung oder auf Servern, die mit dem Internet verbunden sind, sollten Sie sich nicht auf TLS mit Verwendung selbstsignierter Zertifikate verlassen.

Das Aktivieren der TLS-Verschlüsselung erhöht die Sicherheit von Daten, die netzwerkübergreifend zwischen Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und Anwendungen übertragen werden. Wenn der gesamte Datenverkehr jedoch zwischen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und einer Clientanwendung mithilfe von TLS verschlüsselt wird, sind die folgenden zusätzlichen Verarbeitungsschritte erforderlich:
-  Ein zusätzlicher Netzwerkroundtrip ist zum Zeitpunkt des Verbindungsaufbaus erforderlich.
-  Die von der Anwendung an die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz gesendeten Pakete müssen vom Client-TLS-Stapel verschlüsselt und vom Server-TLS-Stapel entschlüsselt werden.
-  Die von der Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] an die Anwendung gesendeten Pakete müssen vom Server-TLS-Stapel verschlüsselt und vom Client-TLS-Stapel entschlüsselt werden.

## <a name="about-certificates"></a><a name="about"></a> Informationen zu Zertifikaten

 Das Zertifikat muss für die **Serverauthentifizierung** ausgegeben sein. Der Name des Zertifikats muss der vollqualifizierte Domänenname (FQDN) des Computers sein.  
  
 Zertifikate werden lokal für diesen Benutzer auf dem Computer gespeichert. Sie müssen den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Configuration Manager mit einem Konto mit lokalen Administratorberechtigungen ausführen, um ein Zertifikat zu installieren, das von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet werden soll.

 Der Client muss in der Lage sein, den Besitzer des vom Server verwendeten Zertifikats zu überprüfen. Wenn der Client über das Zertifikat für öffentliche Schlüssel der Zertifizierungsstelle verfügt, die das Serverzertifikat signiert hat, sind keine weiteren Konfigurationsschritte erforderlich. [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows enthält die Zertifikate für öffentliche Schlüssel von vielen Zertifizierungsstellen. Wenn das Serverzertifikat von einer öffentlichen oder privaten Zertifizierungsstelle signiert wurde, für die der Client kein öffentliches Schlüsselzertifikat besitzt, müssen Sie das Zertifikat für öffentliche Schlüssel der Zertifizierungsstelle installieren, die das Serverzertifikat signiert hat.  
  
> [!NOTE]  
> Wenn Sie die Verschlüsselung bei einem Failovercluster verwenden möchten, müssen Sie das Serverzertifikat mit dem vollqualifizierten DNS-Namen des virtuellen Servers auf allen Knoten im Failovercluster installieren. Wenn Sie z. B. über einen Cluster mit zwei Knoten (mit den Namen ***test1.\*\<your company>\*.com*** und **_test2.\_\<your company>\*.com***) und einen virtuellen Server mit dem Namen **_virtsql_*_ verfügen, müssen Sie auf beiden Knoten ein Zertifikat für _ *_virtsql.\_\<your company>\*.com*** installieren. Sie können den Wert der Option **ForceEncryption** im Eigenschaftsfeld **Protokolle für virtsql** von **SQL Server-Netzwerkkonfiguration** auf **Ja** setzen.

> [!NOTE]
> Wenn Sie eine verschlüsselte Verbindung für einen Azure Search-Indexer zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einer Azure-VM erstellen wollen, finden Sie weitere Informationen unter [Konfigurieren einer Verbindung eines Azure Search-Indexers mit SQL Server auf einer Azure-VM](/azure/search/search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers). 

## <a name="certificate-requirements"></a>Zertifikatanforderungen

Damit in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ein TLS-Zertifikat geladen wird, müssen für das Zertifikat folgende Bedingungen erfüllt sein:

- Das Zertifikat muss sich im Zertifikatspeicher des lokalen Computers oder im Zertifikatspeicher des aktuellen Benutzers befinden.

- Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienstkonto muss über die erforderliche Berechtigung für den Zugriff auf das TLS-Zertifikat verfügen.

- Die aktuelle Systemzeit muss nach der Eigenschaft **Gültig ab** und vor der Eigenschaft **Gültig bis** des Zertifikats liegen.

> [!NOTE]  
> Die Gültigkeit des Zertifikats wird beim Herstellen einer Verbindung zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mit den Clientverbindungen ausgewertet, die initiiert werden und die Verschlüsselungsoption auf „true“ festlegen, es sei denn, sie werden von der Einstellung „Vertrauenswürdiges Serverzertifikat“ außer Kraft gesetzt. 

- Das Zertifikat muss für die Serverauthentifizierung vorgesehen sein. Dazu muss für die **Enhanced Key Usage**-Eigenschaft des Zertifikats **Server Authentification (1.3.6.1.5.5.7.3.1)** angegeben sein.

- Das Zertifikat muss mit der **KeySpec**-Option von **AT_KEYEXCHANGE** erstellt werden. Normalerweise enthält die Schlüsselverwendungseigenschaft (**KEY_USAGE**) des Zertifikats auch die Schlüsselverschlüsselung (**CERT_KEY_ENCIPHERMENT_KEY_USAGE**).

- Mit der **Subject**-Eigenschaft des Zertifikats muss angegeben werden, dass der allgemeine Name (Common Name, CN) mit dem Hostnamen oder dem vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) des Servercomputers übereinstimmt. Wenn Sie den Hostnamen verwenden, muss das DNS-Suffix im Zertifikat angegeben werden. Wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf einem Failovercluster ausgeführt wird, muss der allgemeine Name mit dem Hostnamen oder FQDN des virtuellen Servers übereinstimmen, und die Zertifikate müssen auf allen Knoten im Failovercluster bereitgestellt werden.

- [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] und der [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] Native Client (SNAC) unterstützen Platzhalterzertifikate. SNAC wurde als veraltet markiert und durch den [Microsoft OLE DB-Treiber für SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) und [Microsoft ODBC Driver for SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md) ersetzt. Andere Clients unterstützen möglicherweise keine Platzhalterzertifikate.      
  Platzhalterzertifikate können nicht mithilfe des SQL Server-Konfigurations-Managers ausgewählt werden. Sie müssen den Registrierungsschlüssel `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQLServer\SuperSocketNetLib` bearbeiten und den Fingerabdruck des Zertifikats ohne Leerraum zum Wert des **Zertifikats** hinzufügen, um ein Platzhalterzertifikat zu verwenden.  

  > [!WARNING]  
  > [!INCLUDE[ssnoteregistry_md](../../includes/ssnoteregistry-md.md)]  

## <a name="install-on-single-server"></a><a name="single-server"></a>Installieren auf einem einzelnen Server

Mit [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ist die Zertifikatverwaltung im SQL Server-Konfigurations-Manager integriert. Der SQL Server-Konfigurations-Manager für [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] kann mit früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet werden. Informationen zum Hinzufügen eines Zertifikats auf einer einzelnen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz finden Sie unter [Zertifikatverwaltung (SQL Server-Konfigurations-Manager)](../../database-engine/configure-windows/manage-certificates.md).

Wenn Sie [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] über [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] verwenden und der SQL Server-Konfigurations-Manager für [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] nicht verfügbar ist, führen Sie die folgenden Schritte aus:

1. Klicken Sie im Menü **Start** auf **Ausführen**, geben Sie in das Feld **Öffnen** den Wert **MMC** ein, und klicken Sie dann auf **OK**.  
  
2. Klicken Sie in der MMC-Konsole im Menü **Datei** auf **Snap-In hinzufügen/entfernen**.  
  
3. Klicken Sie im Dialogfeld **Snap-In hinzufügen/entfernen** auf **Hinzufügen**.  
  
4. Klicken Sie im Dialogfeld **Eigenständiges Snap-In hinzufügen** auf **Zertifikate**, und klicken Sie dann auf **Hinzufügen**.  
  
5. Klicken Sie im **Dialogfeld Zertifikate-Snap-In** auf **Computerkonto**, und klicken Sie dann auf **Fertig stellen**.  
  
6. Klicken Sie im Dialogfeld **Eigenständiges Snap-In hinzufügen** auf **Schließen**.  
  
7. Klicken Sie im Dialogfeld **Snap-In hinzufügen/entfernen** auf **OK**.  
  
8. Erweitern Sie im Dialogfeld **Zertifikate-Snap-In** die Option **Zertifikate**, erweitern Sie **Eigene Zertifikate**, und klicken Sie dann mit der rechten Maustaste auf **Zertifikate**, zeigen Sie auf **Alle Aufgaben**, und klicken Sie anschließend auf **Importieren**.  

9. Klicken Sie mit der rechten Maustaste auf das importierte Zertifikat, zeigen Sie auf **Alle Aufgaben**, und klicken Sie dann auf **Privatschlüssel verwalten**. Fügen Sie im Dialogfeld **Sicherheit** die Leseberechtigung für das Benutzerkonto hinzu, das vom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienstkonto verwendet wird.  
  
10. Ergänzen Sie die Angaben im **Zertifikatimport-Assistenten**, um dem Computer ein Zertifikat hinzuzufügen, und schließen Sie dann die MMC-Konsole. Weitere Informationen zum Hinzufügen von Zertifikaten zu einem Computer finden Sie in der Windows-Dokumentation.  

> [!IMPORTANT]
> In Produktionsumgebungen wird die Verwendung eines von einer Zertifizierungsstelle ausgestellten vertrauenswürdigen Zertifikats empfohlen.    
> Zu Testzwecken kann auch ein selbstsigniertes Zertifikat verwendet werden. Informationen zum Erstellen eines selbstsignierten Zertifikats finden Sie in den Artikeln zum [PowerShell-Cmdlet New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate) bzw. dem [certreq-Befehl](/windows-server/administration/windows-commands/certreq_1).
  
## <a name="install-across-multiple-servers"></a>Installieren auf mehreren Servern

Mit [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] ist die Zertifikatverwaltung im SQL Server-Konfigurations-Manager integriert. Der SQL Server-Konfigurations-Manager für [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] kann mit früheren Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet werden. Weitere Informationen zum Hinzufügen eines Zertifikats in einer Failoverclusterkonfiguration oder in einer Verfügbarkeitsgruppenkonfiguration finden Sie unter [Zertifikatverwaltung (SQL Server-Konfigurations-Manager)](../../database-engine/configure-windows/manage-certificates.md).

Wenn Sie [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] über [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] verwenden und der SQL Server-Konfigurations-Manager für [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] nicht verfügbar ist, führen Sie für jeden Server die Schritte im Abschnitt [So stellen Sie ein Zertifikat auf einem einzelnen Server bereit bzw. installieren Sie ein Zertifikat](#single-server) aus.

## <a name="export-server-certificate"></a>Exportieren des Serverzertifikats  
  
1. Erweitern Sie im **Zertifikate** -Snap-In den Ordner **Zertifikate** / **Eigene Zertifikate** , klicken Sie mit der rechten Maustaste auf das **Zertifikat**, zeigen Sie auf **Alle Tasks**, und klicken Sie dann auf **Exportieren**.  
  
2. Führen Sie den **Zertifikatexport-Assistenten** aus, und speichern Sie die Zertifikatsdatei in einem geeigneten Speicherort.  
  
## <a name="configure-server"></a>Konfigurieren des Servers

Konfigurieren Sie den Server, sodass er verschlüsselte Verbindungen erzwingt.

> [!IMPORTANT]
> Das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienstkonto muss über Leseberechtigungen für das Zertifikat verfügen, das zum Erzwingen der Verschlüsselung auf der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz verwendet wird. Für ein nicht privilegiertes Dienstkonto müssen dem Zertifikat Leseberechtigungen hinzugefügt werden. Ist dies nicht der Fall, kann beim Neustart des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Diensts ein Fehler auftreten.
  
1. Erweitern Sie im **SQL Server-Konfigurations-Manager** den Eintrag **SQL Server-Netzwerkkonfiguration**, klicken Sie mit der rechten Maustaste auf **Protokolle für** _\<server instance>_ , und klicken Sie dann auf **Eigenschaften**.  
  
2. Wählen Sie auf der Registerkarte **Zertifikat** im Dialogfeld **Eigenschaften** unter **Protokolle für** _\<instance name>_ das gewünschte Zertifikat aus der Dropdownliste für das Feld **Zertifikat** aus, und klicken Sie dann auf **OK**.  
  
3. Aktivieren Sie auf der Registerkarte **Flags** im Feld **ForceEncryption** die Option **Ja**, und klicken Sie dann auf **OK** , um das Dialogfeld zu schließen.  
  
4. Starten Sie den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienst neu.  

> [!NOTE]
> Konfigurieren Sie den Client so, dass verschlüsselte Verbindungen angefordert werden, um eine sichere Verbindung zwischen Client und Server zu gewährleisten. Weitere Informationen finden Sie [weiter unten in diesem Artikel](#configure-client).

## <a name="configure-client"></a><a name="configure-client"></a>Konfigurieren des Clients

Konfigurieren Sie den Client, sodass er verschlüsselte Verbindungen anfordert.

1. Kopieren Sie entweder das Originalzertifikat oder die exportierte Zertifikatsdatei auf den Clientcomputer.  
  
2. Installieren Sie auf dem Clientcomputer mithilfe des **Zertifikate** -Snap-Ins entweder das Stammzertifikat oder die exportierte Zertifikatsdatei.  
  
3. Klicken Sie im SQL Server-Konfigurations-Manager mit der rechten Maustaste auf **SQL Server Native Client-Konfiguration**, und klicken Sie dann auf **Eigenschaften**.  
  
4. Klicken Sie auf der Seite **Flags** im Feld **Protokollverschlüsselung erzwingen** auf **Ja**.  
  
## <a name="use-sql-server-management-studio"></a>Verwenden von SQL Server Management Studio
  
So verschlüsseln Sie eine Verbindung von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]:  

1. Klicken Sie auf der Symbolleiste des Objekt-Explorers auf **Verbinden**, und klicken Sie dann auf **Datenbank-Engine**.  
  
2. Vervollständigen Sie im Dialogfeld **Verbindung mit Server herstellen** die Verbindungseinstellungen, und klicken Sie dann auf **Optionen**.  
  
3. Klicken Sie auf der Registerkarte **Verbindungseigenschaften** auf **Verbindung verschlüsseln**.  

## <a name="internet-protocol-security-ipsec"></a>Internetprotokollsicherheit (Internet Protocol Security, IPsec)
Mithilfe von IPSec können [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Daten während der Übertragung verschlüsselt werden. IPSec wird von Client- und Serverbetriebssystemen bereitgestellt und muss in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht konfiguriert werden. Informationen zu IPSec finden Sie in der Windows-Dokumentation oder in der Netzwerkdokumentation.

## <a name="next-steps"></a>Nächste Schritte

+ [TLS 1.2-Unterstützung für Microsoft SQL Server](https://support.microsoft.com/kb/3135244)     
+ [Konfigurieren der Windows-Firewall für den SQL Server-Zugriff](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)     
+ [Powershell-Cmdlet New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate)
