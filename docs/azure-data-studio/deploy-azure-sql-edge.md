---
title: Bereitstellen von Azure SQL Edge mit Azure Data Studio
description: Hier erfahren Sie, wie Sie Azure SQL Edge-Instanzen in Azure Data Studio bereitstellen.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: ''
ms.date: 09/22/2020
ms.openlocfilehash: b0dd541ce4be57dd9b065b93413ea567c7f121b7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052221"
---
# <a name="deploy-azure-sql-edge-with-azure-data-studio-preview"></a>Bereitstellen von Azure SQL Edge mit Azure Data Studio (Vorschau)

[Azure SQL Edge](/azure/azure-sql-edge/overview) ist eine optimierte Engine für relationale Datenbanken, die für IoT- und Azure IoT Edge-Bereitstellungen optimiert ist. Azure SQL Database Edge bietet die Möglichkeit zur Erstellung eines Hochleistungs-Datenspeichers und einer Verarbeitungsschicht für IoT-Anwendungen und -Lösungen. In diesem Artikel erfahren Sie, wie Sie eine Azure SQL Edge-Instanz mit Azure Data Studio und den Bereitstellungsszenarios bereitstellen, die vom Bereitstellungs-Assistenten unterstützt werden.  

Die folgenden Szenarios werden vom Bereitstellungs-Assistenten in Azure Data Studio unterstützt:

- Lokale Containerinstanz
- Remotecontainerinstanz
- Neue Azure IoT Hub-Instanz und VM
- Vorhandenes Gerät einer Azure IoT Hub-Instanz
- Mehrere Geräte einer Azure IoT Hub-Instanz

| Erforderliche Tools | `Docker` | `Azure CLI` |
| ------------- | :---: | :---: |
| Lokale Containerinstanz | x | |
| Remotecontainerinstanz | | |
| Neue Azure IoT Hub-Instanz und VM | | x |
| Vorhandenes Gerät einer Azure IoT Hub-Instanz |  | x |
| Mehrere Geräte einer Azure IoT Hub-Instanz |   |  x |

> [!NOTE]
> Azure SQL Edge-Bereitstellungen (Vorschau) sind über die Erweiterung „Azure SQL Edge Deployment Extension“ (Bereitstellungserweiterung für Azure SQL Edge) verfügbar, solange diese Features sich in der Vorschau befinden. Installieren Sie die Erweiterung in Azure Data Studio, um Zugriff auf die Features zu erhalten.

Wenn Sie eine Bereitstellung in Azure Data Studio beginnen möchten, öffnen Sie das Menü in der Ansicht **Verbindungen**, und klicken Sie auf die Option **Neue Bereitstellung...** .  Klicken Sie für alle Azure SQL Edge-Szenarios auf dem folgenden Bildschirm auf **Azure SQL Edge**, und wählen Sie das gewünschte Szenario aus der Dropdownliste **Bereitstellungsziel** aus. Der Bereitstellungs-Assistent generiert ein T-SQL-Notebook, durch dessen Ausführung die Bereitstellung abgeschlossen werden kann. Beachten Sie, dass die Verbindungsinformationen, einschließlich des SQL-Administratorkennworts, standardmäßig im Notebook enthalten sind.

![Übersicht über die Bereitstellung](media/deploy-azure-sql-edge/deploy-overview.png)

## <a name="local-container-instance"></a>Lokale Containerinstanz

Azure SQL Edge kann in einem Docker-Container auf dem lokalen Rechner (localhost) bereitgestellt werden, indem der Containername, das Benutzerkennwort `sa` und der Hostport für die Azure SQL Edge-Konnektivität angegeben werden.  Nach Abschluss der Bereitstellung sind unten im Notebook mehrere Links verfügbar, um weitere Aktionen durchführen zu können:

- **Select here to connect to the Azure SQL Edge instance** (Hier klicken, um eine Verbindung zur Azure SQL Edge-Instanz herzustellen): Stellt in Azure Data Studio eine Verbindung mit der neuen Azure SQL Edge-Instanz her
- **Open the terminal window** (Terminalfenster öffnen): Öffnet ein Terminal (vorhanden oder neu) in Azure Data Studio
- **Stop the container** (Container beenden): Kopiert einen Befehl in das Terminal, der nach Ausführung durch einen Benutzer den Container beendet
- **Remove the container** (Container entfernen): Kopiert einen Befehl in das Terminal, der nach Ausführung durch einen Benutzer den Container entfernt

## <a name="remote-container-instance"></a>Remotecontainerinstanz

Azure SQL Edge kann in einem Docker-Container auf einem Remotehost bereitgestellt werden, auf dem Docker installiert ist, indem die Containerinformationen und die Informationen zum Ziel-/Hostcomputer angegeben werden.  Nach Abschluss der Bereitstellung ist unten im Notebook ein Verbindungslink verfügbar.  Aufgrund der Remotecontainerhostumgebung müssen Befehle zur Ausführung in eine Remoteshell kopiert werden, um den Container zu beenden oder zu entfernen.

## <a name="new-azure-iot-hub-and-vm"></a>Neue Azure IoT Hub-Instanz und VM

Der Bereitstellungs-Assistent für Azure SQL Edge kann mehrere Azure-Ressourcen erstellen, die für die Bereitstellung eines Edge-fähigen virtuellen Computers erforderlich sind, der mit einer Azure IoT Hub-Instanz verbunden ist. Ein aktives Azure-Abonnement ist erforderlich. Diese Bereitstellung erstellt:

- eine Ressourcengruppe (wenn die aktuelle Ressourcengruppe nicht ausgewählt ist)
- Netzwerksicherheitsgruppe
- Virtueller Computer
- Statische öffentliche IP-Adressen

Optional können Sie eine DACPAC-Datei als Ordner zippen und auf der neuen Azure SQL Edge-Instanz als Teil dieses Prozesses bereitstellen.  Wenn eine DACPAC-Datei bereitgestellt wird, wird ein Azure Blob Storage-Konto in derselben Ressourcengruppe erstellt.

## <a name="existing-device-of-an-azure-iot-hub"></a>Vorhandenes Gerät einer Azure IoT Hub-Instanz

Wenn Sie bereits über einen IoT-Hub und ein verbundenes Gerät verfügen, kann Azure SQL Edge anhand der Ressourcengruppe, des Namens des IoT-Hubs und der Geräte-ID auf dem Gerät bereitgestellt werden.
Mithilfe der im Bereitstellungs-Assistenten angegebenen IP-Adresse wird am unteren Rand des Notebooks ein Link für die schnelle Verbindungsherstellung generiert.

Optional können Sie eine DACPAC-Datei als Ordner zippen und auf der neuen Azure SQL Edge-Instanz als Teil dieses Prozesses bereitstellen.  Wenn eine DACPAC-Datei bereitgestellt wird, wird ein Azure Blob Storage-Konto in derselben Ressourcengruppe erstellt.

## <a name="multiple-devices-of-an-azure-iot-hub"></a>Mehrere Geräte einer Azure IoT Hub-Instanz

Wenn Sie bereits über einen bestehenden IoT-Hub und verbundene Geräte verfügen, kann Azure SQL Edge anhand der Ressourcengruppe, des Namens des IoT-Hubs und einer [Zielbedingung](/azure/iot-edge/module-deployment-monitoring#target-condition) für die Auswahl von mindestens einem Gerät auf dem Gerät bereitgestellt werden.
Mithilfe der im Bereitstellungs-Assistenten angegebenen IP-Adresse wird am unteren Rand des Notebooks ein Link für die schnelle Verbindungsherstellung generiert.

Optional können Sie eine DACPAC-Datei als Ordner zippen und auf der neuen Azure SQL Edge-Instanz als Teil dieses Prozesses bereitstellen.  Wenn eine DACPAC-Datei bereitgestellt wird, wird ein Azure Blob Storage-Konto in derselben Ressourcengruppe erstellt.

## <a name="next-steps"></a>Nächste Schritte

- [Weitere Informationen zu Azure SQL Edge](/azure/azure-sql-edge/)