---
title: Was sind SQL Server-Spracherweiterungen?
titleSuffix: ''
description: Spracherweiterungen sind ein Feature von SQL Server, das zum Ausführen von externem Code verwendet wird. SQL Server unterstützt Java, Python und R. Relationale Daten können über das Erweiterbarkeitsframework im externen Code verwendet werden.
author: dphansen
ms.author: davidph
ms.date: 11/06/2020
ms.topic: overview
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 50ebe13c330dba1067c2ccc61bcbc779f7bf0b05
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070611"
---
# <a name="what-is-sql-server-language-extensions"></a>Was sind SQL Server-Spracherweiterungen?
[!INCLUDE [SQL Server 2019 and later](../includes/applies-to-version/sqlserver2019.md)]

Spracherweiterungen sind ein Feature von SQL Server, das zum Ausführen von externem Code verwendet wird. Die relationalen Daten können über das [Erweiterbarkeitsframework](concepts/extensibility-framework.md) im externen Code verwendet werden. SQL Server 2019 unterstützt die Java-, Python- und R-Runtime.

> [!NOTE]
> Informationen zum Ausführen von Python oder R in SQL Server finden Sie in der Dokumentation zu [Machine Learning Services](../machine-learning/sql-server-machine-learning-services.md). Für SQL Server 2019 und höher können Sie eine benutzerdefinierte Python- und R-Runtime mit Sprachenerweiterungen verwenden. Weitere Informationen finden Sie in den Installationsanweisungen unter [Benutzerdefinierte Python-Runtime](../machine-learning/install/custom-runtime-python.md) und [Benutzerdefinierte R-Runtime](../machine-learning/install/custom-runtime-r.md).

## <a name="what-you-can-do-with-language-extensions"></a>Verwendungsmöglichkeiten von Spracherweiterungen

Spracherweiterungen verwenden das [Erweiterbarkeitsframework](concepts/extensibility-framework.md) zum Ausführen von externem Code. Die Codeausführung ist von den Prozessen der Kern-Engine isoliert, aber vollständig in die Ausführung von SQL Server-Abfragen integriert. Sie können Code an der Quelle der Daten ausführen und so die Notwendigkeit umgehen, Daten im Netzwerk zu übertragen.

Externe Sprachen werden mit [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md) definiert. Die gespeicherte Systemprozedur [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) dient als Schnittstelle für die Ausführung des Codes.

Spracherweiterungen bieten eine Reihe von Vorteilen:

+ Datensicherheit. Indem die externe Sprache näher an der Datenquelle ausgeführt wird, werden unsichere Datenverschiebungen vermieden.
+ Geschwindigkeit. Datenbanken sind für setbasierte Vorgänge optimiert. 
+ Einfache Bereitstellung und Integration. [!INCLUDE [ssNoVersion](../includes/ssnoversion-md.md)] ist der Dreh- und Angelpunkt für viele andere Aufgaben und Anwendungen zur Datenverwaltung. Durch Nutzung von in der Datenbank enthaltenen Daten stellen Sie sicher, dass die von der Sprachenerweiterung verwendeten Daten konsistent und aktuell sind.

## <a name="next-steps"></a>Nächste Schritte

+ Installieren der [SQL Server-Sprachenerweiterungen unter Windows](install/windows-java.md) oder [unter Linux](../linux/sql-server-linux-setup-language-extensions-java.md)
+ Installieren der [Benutzerdefinierten Python-Runtime für SQL Server](../machine-learning/install/custom-runtime-python.md)
+ Installieren der [Benutzerdefinierten R-Runtime für SQL Server](../machine-learning/install/custom-runtime-r.md)
+ Installieren Sie des [Microsoft-Erweiterbarkeits-SDKs für Java](how-to/extensibility-sdk-java-sql-server.md)
