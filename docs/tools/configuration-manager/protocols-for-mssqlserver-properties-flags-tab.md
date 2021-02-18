---
title: Protokolle für MSSQLSERVER-Eigenschaften (Registerkarte Flags)
description: In diesem Artikel erfahren Sie, wie Sie die Registerkarte „Flags“ des Dialogfelds für MSSQLSERVER-Eigenschaften verwenden, um die Protokollverschlüsselung anzuzeigen oder anzugeben und um Instanzoptionen auszublenden.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- MSSQLSERVER property protocols
ms.assetid: 4d38e6e9-f95f-4e79-ae45-89f631037528
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 7c8746571c13369b72e9c530cd4addfb1a681ad9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349694"
---
# <a name="protocols-for-mssqlserver-properties-flags-tab"></a>Protokolle für MSSQLSERVER-Eigenschaften (Registerkarte Flags)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Wenn auf dem Server ein Zertifikat installiert ist, verwenden Sie die Registerkarte **Flags** im Dialogfeld **Eigenschaften von Protokolle für 'MSSQLSERVER'** , um die Protokollverschlüsselung anzuzeigen oder anzugeben und Instanzoptionen auszublenden. [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muss neu gestartet werden, um die **ForceEncryption**-Einstellung zu aktivieren oder zu deaktivieren.  
  
 Zum Verschlüsseln von Verbindungen sollten Sie [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] mit einem Zertifikat bereitstellen. Wenn kein Zertifikat installiert ist, generiert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ein selbstsigniertes Zertifikat, sobald die Instanz gestartet wird. Dieses selbstsignierte Zertifikat kann anstelle eines Zertifikats von einer vertrauenswürdigen Zertifizierungsstelle verwendet werden. Es ermöglicht jedoch keine Authentifizierung und verhindert auch keine Nichtanerkennung.  
  
> [!CAUTION]  
>  Verschlüsselte TLS-Verbindungen (Transport Layer Security), früher als SSL (Secure Sockets Layer) bekannt, die ein selbstsigniertes Zertifikat verwenden, bieten keine hohe Sicherheit. Sie sind anfällig für Man-in-the-Middle-Angriffe. In einer Produktionsumgebung oder auf Servern, die mit dem Internet verbunden sind, sollten Sie sich nicht auf TLS mit Verwendung selbstsignierter Zertifikate verlassen.  
  
 Weitere Informationen zur Verschlüsselung finden Sie unter "Verschlüsseln von Verbindungen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]" in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Onlinedokumentation.  
  
 Der Anmeldeprozess ist immer verschlüsselt. Wenn **ForceEncryption** auf **Ja** festgelegt ist, wird jegliche Client/Server-Kommunikation verschlüsselt. Clients, die mit [!INCLUDE[ssDE](../../includes/ssde-md.md)] verbunden sind, müssen so konfiguriert werden, dass sie der Stammzertifizierungsstelle des Serverzertifikats vertrauen. Weitere Informationen finden Sie im Abschnitt „Vorgehensweise: Aktivieren von verschlüsselten Verbindungen zum [!INCLUDE[ssDE](../../includes/ssde-md.md)] ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Konfigurations-Manager)" in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Onlinedokumentation.  
  
## <a name="cluster-servers"></a>Clusterserver  
 Wenn Sie die Verschlüsselung bei einem Failovercluster verwenden möchten, müssen Sie das Serverzertifikat mit dem vollgekennzeichneten DNS-Namen des virtuellen Servers auf allen Knoten im Failovercluster installieren. Wenn Sie beispielsweise über einen Cluster mit zwei Knoten verfügen, von denen der eine Knoten „test1. *\<your company>* .com“ und der andere „test2. *\<your company>* .com“ heißt, sowie über einen virtuellen Server namens „virtsql“, müssen Sie ein Zertifikat für „virtsql. *\<your company>* .com“ auf beiden Knoten installieren. Sie können dann das Kontrollkästchen **ForceEncryption** in **SQL Server-Konfigurations-Manager** aktivieren, um den Failovercluster für die Verschlüsselung zu konfigurieren.  
  
## <a name="options"></a>Tastatur  
 **ForceEncryption**  
 Erzwingen Sie die Protokollverschlüsselung. Die Verschlüsselung ist eine Methode, sensible Informationen vertraulich zu behandeln, indem Daten in eine nicht lesbare Form geändert werden. Durch die Verschlüsselung wird die Sicherheit von Daten sichergestellt, selbst wenn die Übertragungspakete während des Übertragungsvorgangs angezeigt werden. Für die Benutzung von Channelbindung, setzen Sie **Verschlüsselung erzwingen** auf **Ein** , und konfigurieren Sie **Erweiterten Schutz** auf der Registerkarte **Erweitert** .  
  
 **HideInstance**  
 Verhindert, dass der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Browser-Dienst diese Instanz von [!INCLUDE[ssDE](../../includes/ssde-md.md)] für Clientcomputer verfügbar macht, die mithilfe der Schaltfläche **Durchsuchen** versuchen, die Instanz zu finden. Wenn benannte Instanzen auf dem Server vorhanden sind, müssen Clientanwendungen beim Verbinden die Protokollendpunktinformationen angeben. Zum Beispiel die Portnummer oder den Named Pipe-Namen wie **tcp:server,5000**. Weitere Informationen finden Sie unter [Logging In to SQL Server](../../database-engine/configure-windows/logging-in-to-sql-server.md).  
  
 Weitere Informationen finden Sie im Abschnitt „Vorgehensweise: Aktivieren von verschlüsselten Verbindungen zur Datenbank-Engine (SQL Server-Konfigurations-Manager)“ in der Onlinedokumentation.  
  
  
