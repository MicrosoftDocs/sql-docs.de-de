---
title: Verwenden von Vorlagen in SQL Server Management Studio
description: Erfahren Sie mehr über die vorgefertigten Transact-SQL-Vorlagen (T-SQL), die in SQL Server Management Studio (SSMS) verfügbar sind.
keywords: SQL Server, SSMS, SQL Server Management Studio, Vorlagen
author: MashaMSFT
ms.author: mathoma
ms.topic: tutorial
ms.prod: sql
ms.technology: ssms
ms.prod_service: sql-tools
ms.reviewer: sstein
helpviewer_keywords:
- templates [SQL Server], SQL Server Management Studio
- source controls [SQL Server Management Studio], tutorials
- Help [SQL Server], SQL Server Management Studio
- tutorials [SQL Server Management Studio]
- Transact-SQL tutorials
- SQL Server Management Studio [SQL Server], tutorials
- scripts [SQL Server], SQL Server Management Studio
ms.custom: seo-lt-2019
ms.date: 03/13/2018
ms.openlocfilehash: b3c5c26ddf8e343b2cf63bc0dde337056eb2fe74
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017230"
---
# <a name="use-templates-in-sql-server-management-studio"></a>Verwenden von Vorlagen in SQL Server Management Studio

In diesem Tutorial werden die vorgefertigten T-SQL-Vorlagen (Transact-SQL) vorgestellt, die in SQL Server Management Studio (SSMS) verfügbar sind. In diesem Artikel werden folgende Vorgehensweisen behandelt:

## <a name="prerequisites"></a>Voraussetzungen

Zur Durchführung dieses Tutorials benötigen Sie SQL Server Management Studio und Zugriff auf einen SQL-Server.

* Installieren Sie [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).

* Installieren Sie die [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).

## <a name="use-template-browser"></a>Verwenden des Vorlagenbrowsers

In diesem Abschnitt erfahren Sie, wie Sie den Vorlagenbrowser finden und verwenden.

1. Öffnen Sie SQL Server Management Studio.

2. Klicken Sie im Menü **Ansicht** auf **Vorlagenbrowser** (STRG+ALT+T):

    ![Öffnen des Vorlagenbrowsers](media/templates-ssms/templatebrowser.png)

    Im unteren Bereich des Vorlagenbrowsers können Sie aktuell verwendete Vorlagen sehen.

3. Erweitern Sie den Knoten, der Sie interessiert. Führen Sie einen Rechtsklick auf die Vorlage aus, und klicken Sie dann auf **Öffnen**:

    ![Öffnen einer Vorlage](media/templates-ssms/opentemplate.png)

    Sie können auch auf den Vorlagennamen doppelklicken, um sie zu öffnen.

4. Ein neues Abfragefenster wird geöffnet. Das T-SQL-Skript ist bereits aufgefüllt.

5. Ändern Sie die Vorlage Ihren Anforderungen entsprechend, und klicken Sie anschließend auf **Ausführen**, um die Abfrage auszuführen:

    ![Erstellen einer Datenbankvorlage](media/templates-ssms/createdbtemplate.png)

## <a name="edit-an-existing-template"></a>Bearbeiten einer vorhandenen Vorlage

Sie können vorhandene Vorlagen auch im Vorlagenbrowser bearbeiten.  

1. Navigieren Sie im Vorlagenbrowser zu der Vorlage, mit der Sie arbeiten möchten.

2. Führen Sie einen Rechtsklick auf die Vorlage aus, und klicken Sie dann auf **Bearbeiten**:

    ![Bearbeiten einer Vorlage](media/templates-ssms/edittemplate.png)

3. Nehmen Sie Ihre Änderungen in dem geöffneten Abfragefenster vor.

4. Klicken Sie zum Speichern der Vorlage auf **Datei** > **Speichern** (STRG+S).

5. Schließen Sie das Abfragefenster.

6. Öffnen Sie die Vorlage erneut. Ihre Änderungen sollten angezeigt werden.

## <a name="locate-templates-on-disk"></a>Suchen von Vorlagen auf dem Datenträger

Wenn eine Vorlage offen ist, können Sie die Vorlagen suchen, die auf dem Datenträger sind.

1. Wählen Sie im Vorlagenbrowser eine Vorlage aus, und klicken Sie dann auf **Bearbeiten**.

2. Klicken Sie mit der rechten Maustaste auf **Abfragetitel**, und klicken Sie dann auf **Übergeordneten Ordner öffnen**. Der Explorer sollte den Speicherort der Vorlagen auf dem Datenträger öffnen: 

   ![Vorlagen auf dem Datenträger](media/templates-ssms/templatesondisk.png)
  
## <a name="create-a-new-template"></a>Erstellen einer neuen Vorlage

Sie können im Vorlagenbrowser auch eine neue Vorlage erstellen. In den folgenden Schritten erfahren Sie, wie Sie einen neuen Ordner erstellen und anschließend in diesem Ordner eine neue Vorlage erstellen können. Mithilfe dieser Schritte können Sie auch eine benutzerdefinierte Vorlage in einem vorhandenen Ordner erstellen. 

1. Öffnen Sie den Vorlagenbrowser.

2. Klicken Sie mit der rechten Maustaste auf **SQL Server-Vorlagen**, und klicken Sie dann auf **Neu** > **Ordner**.

3. Geben Sie diesem Ordner den Namen **Benutzerdefinierte Vorlagen**:

    ![Erstellen eines Ordners für benutzerdefinierte Vorlagen](media/templates-ssms/creatingcustomtemplate.png)

4. Klicken Sie mit der rechten Maustaste auf den neu erstellten Ordner „Benutzerdefinierte Vorlagen“, und klicken Sie dann auf **Neu** > **Vorlage**. Geben Sie einen Namen für Ihre Vorlage ein:

    ![Erstellen einer benutzerdefinierten Vorlage](media/templates-ssms/createnewtemplate.png)

5. Führen Sie einen Rechtsklick auf die erstellte Vorlage aus, und klicken Sie dann auf **Bearbeiten**. Ein neues Abfragefenster wird geöffnet.

6. Geben Sie den T-SQL-Text ein, der gespeichert werden soll.

7. Klicken Sie im Menü **Datei** auf **Speichern**.

8. Schließen Sie das vorhandene Abfragefenster, und öffnen Sie Ihre neue benutzerdefinierte Vorlage.

## <a name="next-steps"></a>Nächste Schritte

Am besten machen Sie sich mit SSMS vertraut, indem Sie einige praktische Aufgaben durchführen. Dieses *Tutorial* und Artikel zu *Vorgehensweisen* unterstützen Sie bei der Verwendung der verschiedenen Features, die in SSMS verfügbar sind.  In diesen Artikeln erfahren Sie, wie Sie die Komponenten von SSMS verwalten und wie Sie die Funktionen finden, die Sie regelmäßig verwenden.

* [Herstellen einer Verbindung mit einer Instanz und Abfragen einer Instanz](../quickstarts/ssms-connect-query-sql-server.md)
* [Skripterstellung](../tutorials/scripting-ssms.md)
* [SSMS-Konfiguration](../tutorials/ssms-configuration.md)
* [Zusätzliche Tipps und Tricks für die Verwendung von SSMS](../tutorials/ssms-tricks.md)