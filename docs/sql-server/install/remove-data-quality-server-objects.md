---
title: Entfernen von Data Quality Server-Objekten| Microsoft-Dokumente
description: Wenn Sie Data Quality Server von einer SQL Server-Instanz deinstallieren, müssen einige Data Quality Server-Objekte, z. B. die DQS-Datenbanken, manuell gelöscht werden.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 1b7c6dbb-b40e-4822-9caa-608e1056af8e
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6b9e66d821600b29157c0942a2da3a23403a250d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336080"
---
# <a name="remove-data-quality-server-objects"></a>Entfernen von Data Quality Server-Objekten
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Durch Deinstallieren von [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] von einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]oder durch das vollständige Entfernen einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , die [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] aufweist, werden einige [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] -Objekte nicht gelöscht, einschließlich der DQS-Datenbanken. Dies bedeutet, dass Sie die DQS-Daten nicht verlieren, wenn Sie [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] mithilfe des SQL Server-Setups deinstallieren. Sie müssen diese [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] -Objekte manuell löschen, nachdem der Deinstallationsvorgang abgeschlossen wurde.  
  
> [!NOTE]
>  -   Erwägen Sie vor dem Deinstallieren von [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)], all Ihre vorhandenen Wissensdatenbanken zu sichern, indem Sie die DQSB-Datei exportieren und sie später zum Importieren aller Wissensdatenbanken in eine neue [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] -Installation verwenden. Das Exportieren und Importieren sämtlicher DQS-Wissensdatenbanken ist nur möglich, indem "DQSInstaller.exe" mit den entsprechenden Befehlszeilenparametern mithilfe der der Eingabeaufforderung ausgeführt wird. Weitere Informationen finden Sie unter [Export und Importieren von DQS-Wissensdatenbanken mit DQSInstaller.exe](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md).  
> -   Erwägen Sie vor dem Löschen der DQS-Datenbanken, die Datenbanken zu sichern, wenn Sie sie beibehalten möchten und später zum Wiederherstellen der Daten verwenden möchten. Weitere Informationen hierzu finden Sie unter [Verwalten von DQS-Datenbanken](../../data-quality-services/manage-dqs-databases.md).  
  
## <a name="uninstall-data-quality-server-from-a-sql-server-instance"></a>Deinstallieren von Data Quality Server von einer SQL Server-Instanz  
 Wenn Sie nur den [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] von einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz deinstallieren, müssen Sie die folgenden [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] -Objekte manuell von der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz löschen, nachdem die Deinstallation des [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] abgeschlossen wurde:  
  
-   DQS_MAIN-, DQS_PROJECTS- und DQS_STAGING_DATA-Datenbanken.  
  
-   \#Anmeldenamen MS_dqs_db_owner_login## und ##MS_dqs_service_login##.  
  
-   Gespeicherte DQInitDQS_MAIN-Prozedur aus der master-Datenbank.  
  
 Sie können diese Objekte in SQL Server Management Studio löschen, indem Sie mit der rechten Maustaste auf das Objekt klicken und im Kontextmenü **Löschen** wählen.  
  
> [!IMPORTANT]  
>  Wenn Sie [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] nur mit dem `-uninstall` -Befehlszeilenparameter über die Eingabeaufforderung aus einer SQL Server-Instanz deinstallieren, werden im Rahmen des Deinstallationsvorgangs alle DQS-Objekte gelöscht. Sie müssen sie nicht manuell löschen, nachdem Sie [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]deinstalliert haben. Um [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] über die Eingabeaufforderung zu deinstallieren, geben Sie den folgenden Befehl an der Eingabeaufforderung ein, und drücken Sie die EINGABETASTE:   
> `dqsinstaller.exe -uninstall`  
  
## <a name="uninstall-sql-server-instance-containing-data-quality-server"></a>Deinstallieren der SQL Server-Instanz, die Data Quality Server enthält  
 Wenn Sie eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz mit [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)]vollständig deinstallieren, müssen Sie die Datenbanken DQS_MAIN, DQS_PROJECTS und DQS_STAGING_DATA manuell vom Computer löschen, nachdem die Deinstallation abgeschlossen wurde. Bei einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Standardinstallation sind die Dateien der Datenbanken DQS_MAIN, DQS_PROJECTS und DQS_STAGING_DATA unter „C:\Programme\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA“ verfügbar.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Vorgehensweise: Deinstallieren einer vorhandenen SQL Server-Instanz &#40;Setup&#41;](../../sql-server/install/uninstall-an-existing-instance-of-sql-server-setup.md)   
 [Deinstallieren von SQL Server 2016](../../sql-server/install/uninstall-sql-server.md)  
  
  
