---
title: Referenz zur Schnittstelle für virtuelle Geräte
titlesuffix: SQL Server VDI reference
description: Dieser Artikel bietet einen Überblick über die Referenz zur Schnittstelle für virtuelle Geräte für die SQL Server-Sicherung.
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9e4e415ec768344208f7f9c2573ce0ad9a910135
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341752"
---
# <a name="virtual-device-interface-vdi-reference"></a>Referenz zur Schnittstelle für virtuelle Geräte

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Dieser Abschnitt enthält die Spezifikationen für die SQL Server-Anwendungsprogrammierschnittstellen, die für die Verwendung durch Drittanbieter von Sicherungssoftware bestimmt sind.

## <a name="overview"></a>Übersicht

Die Schnittstelle für virtuelle Geräte (Virtual Device Interface, VDI) bietet für Onlinesicherungen den höchsten Durchsatz bei minimaler Beeinträchtigung der Transaktionsworkload sowie die schnellstmöglichen Wiederherstellungszeiten. Sie ermöglicht es Drittanbietern, die gleichen Leistungsmerkmale wie die native SQL Server-Sicherung und -Wiederherstellung zu erreichen, und stellt die gesamte Bandbreite der Sicherungs- und Wiederherstellungsfunktionen zur Verfügung. Die VDI wurde mit SQL Server 7.0 eingeführt und wird auch in höheren Versionen unterstützt und erweitert.

Die VDI unterstützt zwei der am häufigsten verwendeten Sicherungstechnologien:

- Herkömmliche Onlinesicherungen, bei denen der gesamte Inhalt des Sicherungsdatensatzes gelesen und auf die Sicherungsmedien übertragen wird.

- Sicherungen von Momentaufnahmen, die mithilfe der zugrunde liegenden Split-Mirror- oder Copy-on-Write-Technologie durchgeführt werden.

Bei herkömmlichen Onlinesicherungen über die VDI kann der volle Umfang der Sicherungs- und Wiederherstellungsfunktionen in SQL Server genutzt werden. Sicherungen von Momentaufnahmen sind auf vollständige Datenbank- und Datei-/Dateigruppensicherungen beschränkt. Allerdings kann für Sicherungen von Momentaufnahmen ein Rollforward mit herkömmlichen differenziellen Datenbank-, differenziellen Datei- und Transaktionsprotokollsicherungen ausgeführt werden.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die VDI-Referenzdokumentation in diesem Abschnitt.

Laden Sie die vollständige Spezifikation und die unterstützenden Quelldateien herunter:

[SQL Server 2005 Virtual Backup Device Interface (VDI) Specification (Spezifikation zur Schnittstelle für virtuelle Geräte bei SQL Server 2005-Sicherungen)](https://www.microsoft.com/download/details.aspx?id=17282)