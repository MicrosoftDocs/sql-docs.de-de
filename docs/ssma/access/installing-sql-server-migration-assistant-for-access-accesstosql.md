---
title: Installieren von SQL Server Migration Assistant für den Zugriff (Access Token) | Microsoft-Dokumentation
description: Informieren Sie sich über die Installations Voraussetzungen für SQL Server Migration Assistant (SSMA) für den Zugriff und das installieren, lizenzieren, aktualisieren und deinstallieren.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- installing SSMA
- instructions, installation
- instructions, upgrade
- licensing SSMA
- prerequisites for installing SSMA
- procedure, installation
- procedure, licensing
- procedure, upgrading
- removing SSMA
- Setup
- uninstalling SSMA
- upgrading SSMA
ms.assetid: dd50eebd-75df-4e0d-8c4d-88b511aae4c7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 3e34f70a41816dcf9a6009f8b38feb016f409a3a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078951"
---
# <a name="installing-sql-server-migration-assistant-for-access-accesstosql"></a>Installieren von SQL Server Migration Assistant für den Zugriff (Access Token)

[!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Migration Assistant (SSMA) für den Zugriff wird mithilfe eines Windows Installer basierten Assistenten installiert. Dieses Thema enthält Informationen zu Installations Voraussetzungen, einen Link zur neuesten Version von SSMA sowie Anweisungen zum Installieren, lizenzieren, deinstallieren und Aktualisieren von SSMA.

## <a name="prerequisites"></a>Voraussetzungen

Stellen Sie vor der Installation von SSMA sicher, dass Ihr System die folgenden Anforderungen erfüllt:

- Windows 7 oder höher oder Windows Server 2008 oder eine höhere Version.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 oder eine höhere Version.
- Die [!INCLUDE[msCoName](../../includes/msconame_md.md)] .NET Framework Version 4.7.2 oder eine spätere Version. Der .NET Framework ist in [Microsoft .net Handbuch](/dotnet/framework/)verfügbar.
- Zugriff auf und ausreichende Berechtigungen auf dem Computer, der die Ziel Instanz von hostet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] auf die Datenbankobjekte und-Daten migriert werden sollen.
- Microsoft Data Access Object (DAO)-Anbieter Version 12,0 oder 14,0. Sie können den DAO-Anbieter aus Microsoft Office 2010/2007-Produkt installieren oder von der Microsoft-Website herunterladen.
- 4 GB RAM (empfohlen).

## <a name="installing-ssma"></a>Installieren von SSMA

SSMA ist ein Webdownload. Informationen zum Herunterladen der neuesten Version finden Sie auf der [SQL Server Migration Assistant Downloadseite](https://aka.ms/ssmaforaccess).

> [!IMPORTANT]
> Deinstallieren Sie alle früheren Versionen von SSMA für den Zugriff, bevor Sie die neue Version installieren.

So installieren Sie SSMA:
  
1. Doppelklicken Sie auf **SSMAforAccess_ *n*. msi**, wobei *n* für die Buildnummer steht.
2. Klicken Sie auf der Seite **Willkommen** auf Weiter.

   Wenn die erforderlichen Komponenten nicht installiert sind, wird eine Meldung mit dem Hinweis angezeigt, dass Sie zunächst erforderliche Komponenten installieren müssen. Stellen Sie sicher, dass Sie alle erforderlichen Komponenten installiert haben, und führen Sie das Installationsprogramm dann erneut aus.

3. Lesen Sie den End-User-Lizenzvertrag; Wenn Sie zustimmen, wählen Sie **Ich akzeptiere die Vereinbarung** aus, und klicken Sie dann auf **weiter**.
4. Klicken Sie auf der Seite Setuptyp **auswählen** auf **typisch**.
5. Auf der Seite **bereit zur Installation** können Sie bei jedem Start des Tools Telemetrie-und automatische Aktualisierungs Prüfungen aktivieren bzw. deaktivieren. Klicken Sie auf **Installieren**, um die Installation zu starten.
  
Das Standardinstallationsverzeichnis ist `C:\Program Files\Microsoft SQL Server Migration Assistant for Access`.

## <a name="uninstalling-ssma-for-access"></a>Deinstallieren von SSMA für den Zugriff

Deinstallieren **Sie SSMA mithilfe der** Option Software in der Systemsteuerung. Beachten Sie, dass beim Deinstallieren des Programms keine SSMA-Projektdateien oder-Protokolldateien gelöscht werden.

So deinstallieren Sie SSMA:

1. Klicken Sie auf **Start**, klicken Sie auf **Systemsteuerung**, und klicken Sie dann auf **Programme hinzufügen oder entfernen**.
2. Wählen Sie **Microsoft SQL Server Migration Assistant für den Zugriff aus**, und klicken Sie dann auf **Entfernen**.

## <a name="upgrading-to-a-later-version"></a>Aktualisieren auf eine höhere Version

Wenn Sie für den Zugriff auf eine höhere Version von SSMA aktualisieren möchten, müssen Sie zunächst SSMA für den Zugriff deinstallieren und dann die neuere Version installieren. Befolgen Sie die Anweisungen im Abschnitt Deinstallieren von SSMA for Access, um diesen Vorgang abzuschließen.

Wenn Sie ein Projekt öffnen, das in einer früheren Version von SSMA für den Zugriff erstellt wurde, fragt SSMA möglicherweise, ob Sie das Projekt in die neuere Version konvertieren möchten. Klicken Sie auf **Ja** , um mit dem Projekt in der neueren Version von SSMA zu arbeiten.

## <a name="see-also"></a>Weitere Informationen

- [Vorbereiten der Zugriffs Datenbanken für die Migration](preparing-access-databases-for-migration-accesstosql.md)
- [Migration von Access-Datenbanken zu SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)
- [Verknüpfen von Zugriffs Anwendungen mit SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md)