---
description: Microsoft Connector für Teradata
title: Microsoft Connector für Teradata | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5f82833ec5498c107155a5284d7a419ba6289239
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346547"
---
# <a name="microsoft-connector-for-teradata"></a>Microsoft Connector für Teradata

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Microsoft Connector für Teradata ermöglicht das Exportieren von Daten aus und das Laden von Daten in Teradata-Datenbanken in einem SSIS-Paket.

Dieser neue Connector unterstützt Datenbanken mit Tabellen mit 1 MB-Unterstützung.

## <a name="version-support"></a>Versionsunterstützung

Die folgenden Microsoft SQL Server-Produkte werden von Microsoft Connector für Teradata unterstützt:

- Microsoft SQL Server 2019
- Microsoft SQL Server Data Tools (SSDT) 15.8.1 oder höher für Visual Studio 2017
- Microsoft SQL Server Data Tools (SSDT) für Visual Studio 2019

Der Microsoft Connector für Teradata verwendet die Teradata Parallel Transporter-Anwendungsprogrammiersprach-Schnittstelle zum Laden von Daten in und zum Exportieren von Daten aus der Teradata-Datenbank. Die folgenden Versionen werden unterstützt:

- Teradata Parallel Transporter (Teradata PT) 16.10
- Teradata Parallel Transporter (Teradata PT) 16.20

Die folgenden Teradata-Datenbankversionen der Datenquelle werden unterstützt:

- Teradata Database 16.20
- Teradata Database 16.10
- Teradata Database 15.10
- Teradata Database 15.00

In der [Teradata-Dokumentation](https://docs.teradata.com/) finden Sie Details im Programmierhandbuch zur Teradata Parallel Transporter-Anwendungsprogrammierschnittstelle.

## <a name="installation"></a>Installation

Installieren Sie auf 32-Bit-Computern die folgenden Treiber aus dem [Teradata Tools und Hilfsprogramme – Windows-Installationspaket](https://downloads.teradata.com/download/tools/teradata-tools-and-utilities-windows-installation-package):

- Teradata ODBC-Treiber (32-Bit)
- Teradata PT-API (32-Bit)

Installieren Sie auf 64 Bit-Computern die folgenden Treiber:

- Teradata ODBC-Treiber (64-Bit)
- Teradata PT-API (64-Bit)

Laden Sie das Installationsprogramm [der neuesten Version von Microsoft Connector für Teradata](https://www.microsoft.com/download/details.aspx?id=100599) herunter, und führen Sie es aus, um den Connector für Teradata-Datenbanken zu installieren. Folgen Sie anschließend den Anweisungen im Installations-Assistenten.

Nachdem Sie den Connector installiert haben, müssen Sie den SQL Server Integration Services-Dienst neu starten, um sicherzustellen, dass die Teradata-Quelle und das Ziel ordnungsgemäß funktionieren.

## <a name="design-and-execute-ssis-packages"></a>Entwerfen und Ausführen von SSIS-Paketen

Microsoft Connector für Teradata bietet ähnliche Funktionen wie Attunity Teradata Connector. Benutzer können wie mit der vorherigen Benutzeroberfläche mithilfe von SSDT für Visual Studio 2017 oder Visual Studio 2019 *für SQL Server 2019* neue Pakete entwerfen.

Die Teradata-Quelle und das Teradata-Ziel befinden sich in der Kategorie „Allgemein“.

![Teradata-Komponente](media/teradata-component.png)

Der Teradata-Verbindungs-Manager wird als „TERADATA“ angezeigt.

![Manager-Typ „Teradata-Verbindungs-Manager“](media/teradata-connection-manager-type.png)

Vorhandene SSIS-Pakete, die mit Attunity Teradata Connector entworfen wurden, werden automatisch auf die Verwendung von Microsoft Connector für Teradata aktualisiert. Auch die Symbole werden entsprechend geändert.

Zum Ausführen des SSIS-Pakets *für SQL Server 2017 und früher* müssen Sie die entsprechende Version von **Microsoft Connector für Teradata von Attunity** über den unten angegebenen Link installieren:

- [SQL Server 2017: Microsoft Connector Version 5.0 für Teradata von Attunity](https://www.microsoft.com/download/details.aspx?id=55179)
- [SQL Server 2016: Microsoft Connector Version 4.0 für Teradata von Attunity](https://www.microsoft.com/download/details.aspx?id=52950)
- [SQL Server 2014: Microsoft Connector Version 3.0 für Teradata von Attunity](https://www.microsoft.com/download/details.aspx?id=44582)
- [SQL Server 2012: Microsoft Connector Version 2.0 für Teradata von Attunity](https://www.microsoft.com/download/details.aspx?id=29283)

Zum Entwerfen des SSIS-Pakets *für SQL Server 2017 und früher* in SSDT müssen Sie **Microsoft Connector für Teradata** bereits installiert haben und die entsprechende Version von **Microsoft Connector für Teradata von Attunity** installieren.

## <a name="limitations-and-known-issues"></a>Einschränkungen und bekannte Probleme

- Im Teradata-Quellen-/Ziel-Editor funktioniert die Eigenschaft **Default database** (Standarddatenbank) nicht ordnungsgemäß.  Geben Sie im Dropdownfeld den Namen der Datenbank zum Anzeigen oder Filtern der Tabelle ein, um das Problem zu umgehen.

- Im Teradata-Quellen-/Ziel-Editor funktioniert der Zuordnungsschritt bei Eingabe von \<database>.<table/view> nicht. Geben Sie stattdessen \<database>.<table/view> ein, und klicken Sie auf die Dropdownschaltfläche.

- Im Teradata-Quellen-Editor kann die Ansicht nicht angezeigt werden, wenn der Datenzugriffsmodus auf „Tabellenname – TPT-Export“ festgelegt ist. Verwenden Sie den erweiterten Teradata-Quellen-Editor, um das Problem zu umgehen.

- Für das Teradata-Ziel kann das Attribut „PackMaximum“ nicht auf TRUE festgelegt werden.  Andernfalls tritt ein Fehler auf.

## <a name="uninstallation"></a>Deinstallation

Sie können den Deinstallations-Assistenten ausführen, um den **Microsoft Connector für Teradata** zu entfernen.

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren des [Teradata-Verbindungs-Managers](teradata-connection-manager.md)
- Konfigurieren der [Teradata-Quelle](teradata-source.md)
- Konfigurieren des [Teradata-Ziels](teradata-destination.md)
- Wenn Sie Fragen haben, besuchen Sie die [Tech Community](https://aka.ms/AA6iwdw).
