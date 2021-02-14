---
title: Untersuchen von Azure SQL-Ressourcen mit dem Azure-Ressourcen-Explorer
description: Erfahren Sie, wie Sie Azure SQL Server, Azure SQL-Datenbank und verwaltete Azure SQL-Instanzen mit dem Azure-Ressourcen-Explorer untersuchen und verwalten können.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: yanancai
ms.author: yanacai
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 5d28054e9f459bc3334a863e7ee5edb3bd98afea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048730"
---
# <a name="explore-and-manage-azure-sql-resources-with-azure-resource-explorer"></a>Untersuchen und Verwalten von Azure SQL-Ressourcen mit dem Azure-Ressourcen-Explorer

In diesem Dokument erfahren Sie, wie Sie Azure SQL Server, Azure SQL-Datenbank und verwaltete Azure SQL-Instanzen mit dem Azure-Ressourcen-Explorer in [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)] untersuchen und verwalten können.

>[!NOTE]
>Der Azure-Ressourcen-Explorer wird in SQL Server 2019 unterstützt. Danach können Sie die Erweiterung über den [Erweiterungs-Manager](./extensions/add-extensions.md) oder **Datei** > **Install Package from VSIX PackageP** (Paket aus dem VSIX-Paket installieren) installieren.

## <a name="connect-to-azure"></a>Herstellen einer Verbindung mit Azure

Nach der Installation des SQL-Plug-Ins wird ein Azure-Symbol in der Menüleiste auf der linken Seite angezeigt. Klicken Sie auf das Symbol, um den Azure-Ressourcen-Explorer zu öffnen. Wenn das Azure-Symbol nicht angezeigt wird, klicken Sie mit der rechten Maustaste auf die linke Menüleiste, und wählen Sie **Azure-Ressourcen-Explorer** aus.

### <a name="add-an-azure-account"></a>Hinzufügen eines Azure-Kontos

Zum Anzeigen der SQL-Ressourcen, die einem Azure-Konto zugeordnet sind, müssen Sie das Konto zuerst [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)] hinzufügen.

1. Öffnen Sie das Dialogfeld **Verknüpfte Konten** über das Kontoverwaltungssymbol links unten oder über den Link **Bei Azure anmelden** im Azure-Ressourcen-Explorer.

    ![Anmelden bei Azure](media/azure-resource-explorer/sign-in-to-azure.png)

2. Klicken Sie im Dialogfeld **Verknüpfte Konten** auf **Konto hinzufügen**.

    ![Hinzufügen eines Azure-Kontos](media/azure-resource-explorer/add-an-azure-account.png)

3. Klicken Sie auf **Kopieren und öffnen**, um den Browser für die Authentifizierung zu öffnen.

    ![Öffnen der Authentifizierungsseite im Browser](media/azure-resource-explorer/open-authentication-in-browser.png)

4. Fügen Sie den **Benutzercode** auf der Webseite ein, und klicken Sie auf **Weiter**, um sich zu authentifizieren.

    ![Authentifizieren im Browser](media/azure-resource-explorer/authenticate-in-browser.png)

5. In [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)] sollte nun das angemeldete Azure-Konto im Dialogfeld **Verknüpfte Konten** angezeigt werden.

    ![Angemeldetes Azure-Konto](media/azure-resource-explorer/signed-in-azure-account.png)

### <a name="add-more-azure-accounts"></a>Hinzufügen weiterer Azure-Konten

In [!INCLUDE [Azure Data Studio](../includes/name-sos-short.md)] werden mehrere Azure-Konten unterstützt. Wenn Sie weitere Azure-Konten hinzufügen möchten, klicken Sie auf die Schaltfläche rechts oben im Dialogfeld **Verknüpfte Konten**, und befolgen Sie dieselben Schritte wie im Abschnitt „Hinzufügen eines Azure-Kontos“, um weitere Azure-Konten hinzuzufügen.

![Hinzufügen weiterer Azure-Konten](media/azure-resource-explorer/add-more-azure-account.png)

### <a name="remove-an-azure-account"></a>Entfernen eines Azure-Kontos

So entfernen Sie ein vorhandenes, angemeldetes Azure-Konto:

1. Öffnen Sie das Dialogfeld **Verknüpfte Konten** über das Kontoverwaltungssymbol links unten.
2. Klicken Sie auf die Schaltfläche **X** rechts neben dem Azure-Konto, um es zu entfernen.

    ![Entfernen eines Azure-Kontos](media/azure-resource-explorer/remove-azure-account.png)

## <a name="filter-subscription"></a>Filtern von Abonnements

Nachdem Sie sich bei einem Azure-Konto angemeldet haben, werden alle Abonnements, die diesem Azure-Konto zugeordnet sind, im Azure-Ressourcen-Explorer angezeigt. Sie können für jedes Azure-Konto Abonnements filtern.

1. Klicken Sie rechts im Azure-Konto auf die Schaltfläche **Abonnement auswählen**.

   ![Filtern von Abonnements](media/azure-resource-explorer/filter-subscription.png)

2. Aktivieren Sie die Kontrollkästchen für die Kontoabonnements, die Sie durchsuchen möchten, und klicken Sie dann auf **OK**.

   ![Auswählen des Abonnements](media/azure-resource-explorer/select-subscription.png)

## <a name="explore-azure-sql-resources"></a>Untersuchen von Azure SQL-Ressourcen

Um im Azure-Ressourcen-Explorer zu einer Azure SQL-Ressource zu navigieren, erweitern Sie die Gruppe mit Azure-Konten und Ressourcentyp.

Der Azure-Ressourcen-Explorer unterstützt derzeit Azure SQL Server, Azure SQL-Datenbank und verwaltete Azure SQL-Instanzen.

## <a name="connect-to-azure-sql-resources"></a>Herstellen einer Verbindung mit Azure SQL-Ressourcen

Der Azure-Ressourcen-Explorer ermöglicht einen schnellen Zugriff, der Ihnen das Herstellen von Verbindungen mit SQL Server-Instanzen und Datenbanken für die Abfrage und Verwaltung erleichtert.

1. Untersuchen Sie in der Strukturansicht die SQL-Ressource, mit der Sie eine Verbindung herstellen möchten.
2. Klicken Sie mit der rechten Maustaste auf die Ressource, und wählen Sie **Verbinden** aus. Sie können auch die Schaltfläche „Verbinden“ auf der rechten Seite der Ressource verwenden.

   ![Herstellen einer Verbindung mit einer Azure SQL-Ressource](media/azure-resource-explorer/connect-to-azure-sql-resource.png)

3. Geben Sie im geöffneten Dialogfeld **Verbindung** Ihr Kennwort ein, und klicken Sie auf **Verbinden**.

   ![SQL-Verbindungsdialogfeld](media/azure-resource-explorer/sql-connection-dialog.png)
4. Nachdem die Verbindung erfolgreich hergestellt wurde, wird das Fenster **Server** automatisch mit der neu verbundenen SQL Server-Instanz/Datenbank geöffnet.

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von [!INCLUDE[Azure Data Studio](../includes/name-sos-short.md)], um eine Verbindung mit einer Azure SQL-Datenbank-Instanz herzustellen und sie abzufragen](quickstart-sql-database.md)
- [Schnellstart: Verwenden von [!INCLUDE[Azure Data Studio](../includes/name-sos-short.md)], um eine Verbindung mit Daten in Azure SQL Data Warehouse herzustellen und sie abzufragen](quickstart-sql-dw.md)