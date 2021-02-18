---
description: Erstellen oder Bearbeiten einer Servergruppe (SQL Server Management Studio)
title: Erstellen oder Bearbeiten einer Servergruppe
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.editgroup.f1
- sql13.swb.newgroup.f1
helpviewer_keywords:
- Registered Servers [SQL Server], server groups
- server groups [SQL Server]
- groups [SQL Server], server
ms.assetid: d4a942bd-2dd1-42db-ad0e-e9a9ae5b856d
author: markingmyname
ms.author: maghan
ms.reviewer: mikeray
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 81212d0e55c322467abcb47ca96770bc1ae0f5ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350519"
---
# <a name="create-or-edit-a-server-group-sql-server-management-studio"></a>Erstellen oder Bearbeiten einer Servergruppe (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

In diesem Thema wird beschrieben, wie Sie die Server in Registrierte Server in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] organisieren, indem Sie Servergruppen erstellen und die Server den Servergruppen zuordnen. Servergruppen können jederzeit in Registrierte Server erstellt werden, aber auch beim Registrieren von Servern.  

## <a name="SSMSProcedure"></a>

### <a name="to-create-a-server-group-in-registered-servers"></a>So erstellen Sie eine Servergruppe in Registrierte Server  

1. Klicken Sie in Registrierte Server auf der Symbolleiste Registrierte Server auf den Servertyp. Wenn die Option Registrierte Server nicht angezeigt wird, klicken Sie im Menü **Ansicht** auf **Registrierte Server** .  

2. Klicken Sie mit der rechten Maustaste auf einen Server oder eine Servergruppe, zeigen Sie auf **Neu**, und klicken Sie dann auf **Servergruppe**.  

3. Geben Sie im Dialogfeld **Neue Servergruppe** in das Feld **Gruppenname** einen Namen für die Servergruppe ein. Der Servergruppenname muss für die aktuelle Position in der Struktur Registrierte Server eindeutig sein.

4. Geben Sie in das Listenfeld **Gruppenbeschreibung** optional einen Anzeigenamen ein, der die Servergruppe beschreibt, wie z. B. "Finanzserver für Lateinamerika".  

5. Klicken Sie im Feld **Wählen Sie einen Speicherort für die neue Servergruppe aus** auf einen Speicherort für die Gruppe, und klicken Sie dann auf **Speichern**.  

   > [!NOTE]
   >  Sie können beim Registrieren eines Servers auch eine neue Servergruppe erstellen, indem Sie auf **Neue Gruppe** klicken und die entsprechenden Informationen im Dialogfeld **Neue Gruppe** eingeben.  

## <a name="see-also"></a>Siehe auch

[Registrieren von Servern](./register-servers.md)