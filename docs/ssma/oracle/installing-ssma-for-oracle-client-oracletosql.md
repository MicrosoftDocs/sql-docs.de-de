---
title: Installieren von SSMA für Oracle Client (oracledesql) | Microsoft-Dokumentation
description: Informieren Sie sich über die Installations Voraussetzungen für den SQL Server Migration Assistant (SSMA) für den Oracle-Client und die Vorgehensweise zum Installieren von.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: d5d4903d-e296-4bbf-8780-63674c4d62d5
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 5bed1589c0852acc2d782028f6b9cc253301babe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073491"
---
# <a name="installing-ssma-for-oracle-client-oracletosql"></a>Installieren von SSMA für Oracle Client (oracledesql)

Der SSMA-Client besteht aus den Programmdateien, die die folgenden Aufgaben ausführen:  
  
- Stellen Sie eine Verbindung mit einer Oracle-Datenbank her.
- Verbinden mit einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
- Konvertieren Sie Oracle-Datenbankobjekte in die- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Syntax.
- Laden Sie die Objekte in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
- Migrieren von Daten zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Dieses Thema enthält die Installations Voraussetzungen und Anweisungen zum Installieren von SSMA.

## <a name="prerequisites"></a>Voraussetzungen

SSMA ist für die Verwendung mit Oracle 9 oder höheren Versionen und allen Editionen von konzipiert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Stellen Sie vor der Installation von SSMA sicher, dass der Computer die folgenden Anforderungen erfüllt:

- Windows 7 oder höher oder Windows Server 2008 oder höhere Versionen.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 oder eine höhere Version.
- Die [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] Version 4.7.2 oder eine spätere Version. Sie können Sie über das [.NET Framework Developer Center](https://go.microsoft.com/fwlink/?LinkId=48882)abrufen.
- Konnektivität zu den Oracle-Datenbanken, die Sie migrieren möchten.
- Zugriff auf und ausreichende Berechtigungen auf dem Computer, auf dem die Ziel Instanz von gehostet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] wird, oder auf [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] dem Sie Datenbankobjekte und-Daten migrieren möchten. Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit SQL Server &#40;oracleto SQL&#41;](../../ssma/oracle/connecting-to-sql-server-oracletosql.md).
- 4 GB RAM empfohlen.  
  
## <a name="installing-the-ssma-for-oracle-client"></a>Installieren des SSMA für den Oracle-Client

SSMA ist ein Webdownload. Informationen zum Herunterladen der neuesten Version finden Sie auf der [SQL Server Migration Assistant Downloadseite](https://aka.ms/ssmafororacle).

So installieren Sie den SSMA-Client:

1. Doppelklicken Sie auf **SSMAforOracle_ *n*. msi**, wobei *n* für die Buildnummer steht.
2. Klicken Sie auf der Seite **Willkommen** auf Weiter.

   Wenn die erforderlichen Komponenten nicht installiert sind, wird eine Meldung mit dem Hinweis angezeigt, dass Sie zunächst erforderliche Komponenten installieren müssen. Stellen Sie sicher, dass Sie alle erforderlichen Komponenten installiert haben, und führen Sie das Installationsprogramm dann erneut aus.  

3. Lesen Sie den End-User-Lizenzvertrag. Wenn Sie zustimmen, wählen Sie **Ich akzeptiere die Vereinbarung** aus, und klicken Sie dann auf **weiter**.
4. Klicken Sie auf der Seite Setuptyp **auswählen** auf **typisch**.
5. Auf der Seite **bereit zur Installation** können Sie bei jedem Start des Tools Telemetrie-und automatische Aktualisierungs Prüfungen aktivieren bzw. deaktivieren. Klicken Sie auf **Installieren**, um die Installation zu starten.

> [!IMPORTANT]
> Deinstallieren Sie alle früheren Versionen von SSMA für Oracle, bevor Sie die neue Version installieren.

Das Standardinstallationsverzeichnis ist `C:\Program Files\Microsoft SQL Server Migration Assistant for Oracle`.

Zusätzlich zu den SSMA-Programmdateien müssen Sie auch das SSMA für Oracle-Erweiterungspaket auf installieren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Weitere Informationen finden Sie unter [Installieren von SSMA-Komponenten auf SQL Server](../../ssma/oracle/installing-ssma-components-on-sql-server-oracletosql.md).

## <a name="see-also"></a>Weitere Informationen

- [Installieren von SSMA-Komponenten auf SQL Server](../../ssma/oracle/installing-ssma-components-on-sql-server-oracletosql.md)
- [Migrieren von Oracle-Datenbanken zu SQL Server](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)
