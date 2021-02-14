---
title: Bereitstellen von Azure SQL Database mithilfe eines Notebooks
description: In diesem Tutorial erfahren Sie, wie Sie eine Azure SQL-Datenbank erstellen können.
author: ninarn
ms.author: ninarn
ms.reviewer: alayu, markingmyname
ms.topic: tutorial
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.custom: ''
ms.date: 10/16/2020
ms.openlocfilehash: 8bac1e0d10208283e40843288ab7dbe7d042b8ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052211"
---
# <a name="create-an-azure-sql-database-using-azure-data-studio"></a>Erstellen einer Azure SQL-Datenbank mithilfe von Azure Data Studio

Sie können eine Azure SQL-Datenbank mithilfe von Azure Data Studio über den Bereitstellungs-Assistenten und Notebooks erstellen.

## <a name="pre-requisites"></a>Voraussetzungen

 - [Azure Data Studio](download-azure-data-studio.md) ist installiert
 - Ein aktives Azure-Konto und ein Azure-Abonnement. Falls Sie noch kein Konto besitzen, [erstellen Sie ein kostenloses Konto](https://azure.microsoft.com/free/).

## <a name="use-the-deployment-wizard"></a>Verwenden des Bereitstellungs-Assistenten

Führen Sie die folgenden Schritte aus, um den Bereitstellungs-Assistenten zu verwenden, der Sie mit einer einfachen Benutzeroberfläche durch die erforderlichen Einstellungen führt.

Suchen Sie zunächst „Azure SQL-Datenbank“ im Bereitstellungs-Assistenten, und wählen Sie es aus.

 1. Wählen Sie in Azure Data Studio das Viewlet „Verbindungen“ im Navigationsbereich auf der linken Seite aus.
 2. Wählen Sie oben im Bereich „Verbindungen“ die Schaltfläche mit den drei Auslassungspunkten **...** und dann **Neue Bereitstellung...** aus.
 3. Wählen Sie im Bereitstellungs-Assistenten die Kachel **Azure SQL-Datenbank** aus, und aktivieren Sie das Kontrollkästchen zum Akzeptieren der Bedingungen.
 4. Wählen Sie in der Dropdownliste der Ressourcentypen aus, was Sie erstellen möchten: eine Datenbank, einen Datenbankserver oder einen Pool für elastische Datenbanken. Wenn Sie nicht über SQL-Datenbanken in Azure verfügen, empfehlen wir, zunächst eine Datenbank zu erstellen.
 5. Wählen Sie **Im Azure-Portal erstellen** aus, wenn Sie sich für die Erstellung eines Servers oder Pools entschieden haben, oder wählen Sie **Auswählen** aus, wenn Sie stattdessen eine Datenbank erstellen möchten.

Wenn Sie sich für die Erstellung einer Datenbank entschieden haben, führen Sie die Schritte unten aus.

 1. Melden Sie sich bei Ihrem Azure-Konto an, wenn das noch nicht geschehen ist. Sie können Ihre Verbindung aktualisieren, wenn auf dieser Seite des Assistenten Probleme auftreten.
 2. Wählen Sie Ihr bevorzugtes Abonnement und den gewünschten Server aus. Wählen Sie **Weiter** aus.
 3. Geben Sie einen Datenbanknamen ein.
 4. Geben Sie einen Namen für die Firewallregel und den IP-Bereich Ihres lokalen Clientcomputers ein, auf dem Azure Data Studio ausgeführt wird. Dies dient dazu, dass Sie sofort nach der Erstellung aus ADS auf den Server und die Datenbank zugreifen können.
 5. Wählen Sie **Weiter** aus.
 6. Überprüfen Sie die eingegebenen Parameter, und wählen Sie dann **Skript in Notebook schreiben** aus.

Sobald das Notebook geöffnet wird, können Sie die Inhalte und den Code überprüfen und ggf. Änderungen vornehmen. Insbesondere können Sie die Compute- und Speichereinstellungen gegenüber den Standardeinstellungen ändern, wenn Ihnen bestimmte Leistungs- oder Kostenaspekte wichtig sind. Beachten Sie jedoch, dass alle Änderungen, die Sie am Notebook vornehmen, zu Validierungsfehlern führen können.

Wählen Sie als letzten Schritt **Run all** (Alle ausführen) aus, um alle Zellen im Notebook auszuführen. Sobald dies abgeschlossen ist, sollten Sie über eine vollständig erstellte und ausgeführte SQL-Datenbank verfügen, zu der Sie eine Verbindung aus ADS herstellen und die Sie abfragen können.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Ihre Datenbank, Ihren Server oder Ihren Pool erstellt haben, können Sie zu Ihrer Datenbank in Azure Data Studio [eine Verbindung herstellen und sie abfragen](quickstart-sql-database.md).
