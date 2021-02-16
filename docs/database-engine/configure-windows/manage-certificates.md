---
title: Zertifikatverwaltung (SQL Server-Konfigurations-Manager)
description: Erfahren Sie, wie Sie Zertifikate in verschiedenen SQL Server-Konfigurationen installieren. Beispiele hierfür sind einzelne Instanzen, Failovercluster und Always On-Verfügbarkeitsgruppen.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- connections [SQL Server], encrypted
- SSL [SQL Server]
- Secure Sockets Layer (SSL)
- encryption [SQL Server], connections
- cryptography [SQL Server], connections
- certificates [SQL Server], installing
- requesting encrypted connections
- installing certificates
- security [SQL Server], encryption
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: ''
ms.date: 01/12/2021
ms.openlocfilehash: 9d6eebebb411c4d3939de8dd09b094b245efc1d2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355338"
---
# <a name="certificate-management-sql-server-configuration-manager"></a>Zertifikatverwaltung (SQL Server-Konfigurations-Manager)

[!INCLUDE [sql-windows-only](../../includes/applies-to-version/sql-windows-only.md)]

In diesem Thema erfahren Sie, wie Zertifikate in Ihrer Topologie der SQL Server-Always On-Failovercluster bzw. -Verfügbarkeitsgruppen bereitgestellt und verwaltet werden.

SSL/TLS-Zertifikate werden oft dazu verwendet, den Zugriff auf SQL Server zu sichern. Bei früheren Versionen von SQL Server war es für Organisationen mit großen SQL Server-Umgebungen viel Aufwand, ihre Infrastruktur der SQL Server-Zertifikate zu pflegen. Hierzu wurden häufig Skripts entwickelt und manuelle Befehle ausgeführt. Ab SQL Server 2019 ist die Zertifikatverwaltung im SQL Server-Konfigurations-Manager integriert, was allgemeine Aufgaben wie die Folgenden vereinfacht: 

* Anzeigen und Überprüfen der auf einer SQL Server-Instanz installierten Zertifikate 
* Identifizieren der bald ablaufenden Zertifikate 
* Bereitstellen von Zertifikaten für Computer von Always On-Verfügbarkeitsgruppen auf dem Knoten mit dem primären Replikat 
* Bereitstellen von Zertifikaten für Computer einer Always On-Failoverclusterinstanz auf dem aktiven Knoten

> [!NOTE]
> Sie können die Zertifikatverwaltung im SQL Server-Konfigurations-Manager mit früheren Versionen von SQL Server verwenden, beginnend mit SQL Server 2008.

##  <a name="to-install-a-certificate-for-a-single-sql-server-instance"></a><a name="provision-single-server-cert"></a>So installieren Sie ein Zertifikat für eine einzelne SQL Server-Instanz  

::: moniker range=">=sql-server-ver15"
1. Erweitern Sie im SQL Server-Konfigurations-Manager im Konsolenbereich den Knoten **SQL Server-Netzwerkkonfiguration**.  

2. Klicken Sie mit der rechten Maustaste auf **Protokolle für** *&lt;Instanzname&gt;* , und klicken Sie dann auf **Eigenschaften**.  

3. Wählen Sie die Registerkarte **Zertifikat** und anschließend **Importieren** aus.  

4. Klicken Sie auf **Durchsuchen** und dann auf die Zertifikatsdatei.  

5. Klicken Sie auf **Weiter**, um das Zertifikat zu überprüfen. Wenn keine Fehler vorliegen, klicken Sie auf **Weiter**, um das Zertifikat in die lokale Instanz zu importieren.  
::: moniker-end

::: moniker range="<= sql-server-2017"
1. Erweitern Sie im SQL Server-Konfigurations-Manager im Konsolenbereich den Knoten **SQL Server-Netzwerkkonfiguration**.  

2. Klicken Sie mit der rechten Maustaste auf **Protokolle für** *&lt;Instanzname&gt;* , und klicken Sie dann auf **Eigenschaften**.  

3. Wählen Sie im Dropdownmenü **Zertifikat** ein Zertifikat aus, und klicken Sie dann auf **Anwenden**.  

4. Klicken Sie auf **OK**. 
::: moniker-end

##  <a name="to-install-a-certificate-in-a-failover-cluster-instance-configuration"></a><a name="provision-failover-cluster-cert"></a> So installieren Sie ein Zertifikat in einer Konfiguration einer Failoverclusterinstanz  
  
1. Erweitern Sie im SQL Server-Konfigurations-Manager im Konsolenbereich den Knoten **SQL Server-Netzwerkkonfiguration**.
  
2. Klicken Sie mit der rechten Maustaste auf **Protokolle für** *&lt;Instanzname&gt;* , und klicken Sie dann auf **Eigenschaften**. 

3. Wählen Sie die Registerkarte **Zertifikat** und anschließend **Importieren** aus.

4. Wählen Sie den Zertifikattyp aus, und geben Sie an, ob dieser nur für den aktuellen Knoten oder für jeden einzelnen Clusterknoten importiert werden soll.

5. Zur Installation für einen einzelnen Knoten klicken Sie auf **Durchsuchen** und anschließend auf die Zertifikatdatei. Fahren Sie dann mit Schritt 8 fort.

6. Klicken Sie zur Installation eines Zertifikats für jeden Knoten auf **Weiter**, um die Knoten möglicher Besitzer aufzulisten. Mögliche Besitzer der aktuellen Failoverclusterinstanz sind vorab ausgewählt.

7. Klicken Sie auf **Weiter**, um das zu importierende Zertifikat auswählen.

8. Geben Sie bei Aufforderung das Kennwort ein. Achten Sie nach der Überprüfung auf mögliche Warn- oder Fehlermeldungen.

9. Klicken Sie auf **Weiter**, um die ausgewählten Zertifikate zu importieren.

> [!NOTE]
> Führen Sie diese Schritte im aktiven Knoten der Always On-Failoverclusterinstanz aus. Benutzer müssen über Administratorberechtigungen auf allen Clusterknoten verfügen.

##  <a name="to-install-a-certificate-in-an-always-on-availability-group-configuration"></a><a name="provision-availability-group-cert"></a>So installieren Sie ein Zertifikat in einer Always On-Verfügbarkeitsgruppenkonfiguration  
  
1. Erweitern Sie im SQL Server-Konfigurations-Manager im Konsolenbereich den Knoten **SQL Server-Netzwerkkonfiguration**.
  
2. Klicken Sie mit der rechten Maustaste auf **Protokolle für** *&lt;Instanzname&gt;* , und klicken Sie dann auf **Eigenschaften**.  
  
3. Wählen Sie die Registerkarte **Zertifikat** und anschließend **Importieren** aus.  
  
4. Klicken Sie auf den Zertifikattyp und anschließend auf **Weiter**, um aus der Liste der bekannten Verfügbarkeitsgruppen auszuwählen.  

5. Klicken Sie auf **Weiter**, um Zertifikate für einen bestimmten Replikatknoten auszuwählen. Zertifikate müssen einen Dateinamen aufweisen, der den NetBIOS-Namen der Knoten entspricht.

6. Klicken Sie auf **Weiter**, um das Zertifikat für jeden Knoten zu importieren.


> [!NOTE]
> Führen Sie diese Schritte im Knoten mit dem primären Replikat der Verfügbarkeitsgruppe aus. Benutzer müssen über Administratorberechtigungen auf allen Clusterknoten verfügen.

