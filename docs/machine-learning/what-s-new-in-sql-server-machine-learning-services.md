---
title: Neuerungen in SQL Server-Machine Learning Services
titleSuffix: ''
description: Ankündigungen neuer Funktionen für die verschiedenen Versionen von SQL Server-Machine Learning Services und SQL Server 2016 R Services.
ms.date: 11/17/2020
ms.topic: overview
author: dphansen
ms.author: davidph
ms.custom: sqlseattle
ms.prod: sql
ms.technology: machine-learning-services
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 31ecd76efc4796aebe2a3ee40a8bf40891d38a83
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272931"
---
# <a name="whats-new-in-sql-server-machine-learning-services"></a>Neuerungen in SQL Server-Machine Learning Services
[!INCLUDE [SQL Server 2016 and later](../includes/applies-to-version/sqlserver2016.md)]

In diesem Artikel wird beschrieben, welche neuen Funktionen und Features in den einzelnen Versionen von [SQL Server Machine Learning Services](sql-server-machine-learning-services.md) enthalten sind. Funktionen für maschinelles Lernen werden SQL Server in jedem Release hinzugefügt, während wir die Integration zwischen der Datenplattform, erweiterten Analysen und Data Science weiter ausbauen, erweitern und vertiefen. 

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
## <a name="new-in-sql-server-2019"></a>Neues in SQL Server 2019

Mit diesem Release wird SQL Server um die am häufigsten nachgefragten Funktionen für Vorgänge des maschinellen Lernens mit Python und R ergänzt. Weitere Informationen zu allen Features in diesem Release finden Sie unter [Neuerungen in SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md) und [Versionshinweise für SQL Server 2019](../sql-server/sql-server-version-15-release-notes.md).

> [!NOTE]
> Die Dokumentation zu den Neuerungen für Java in SQL Server 2019 finden Sie unter [Neuerungen in SQL Server-Spracherweiterungen](../language-extensions/language-extensions-whats-new.md).

Im Folgenden werden die neuen Features für SQL Server Machine Learning Services aufgeführt, die für **Windows** und **Linux** verfügbar sind:

- Die Unterstützung der Linux-Plattform wurde in Machine Learning Services für Python und R hinzugefügt. Informationen zu den ersten Schritten finden Sie unter [Installieren von SQL Server Machine Learning Services unter Linux](../linux/sql-server-linux-setup-machine-learning.md).
- [Loopbackverbindung zu SQL Server über ein Python- oder R-Skript](connect/loopback-connection.md) 
- [CREATE EXTERNAL LIBRARY (Transact-SQL)](../t-sql/statements/create-external-library-transact-sql.md) für Python und R.
- Mit [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) werden zwei neue Parameter eingeführt, mit denen Sie problemlos mehrere Modelle aus partitionierten Daten generieren können. Weitere Informationen hierzu finden Sie im Tutorial [Erstellen von partitionsbasierten Modellen in R](tutorials/r-tutorial-create-models-per-partition.md).
- Die Failoverclusterunterstützung ist für den Launchpad-Dienst verfügbar, sofern der SQL Server-Launchpad-Dienst auf allen Knoten gestartet wird. Weitere Informationen hierzu finden Sie unter [SQL Server-Failoverclusterinstallation](../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md).
- Es stehen Änderungen am Isolationsmechanismus für Machine Learning Services zur Verfügung. Weitere Informationen finden Sie unter [SQL Server 2019 für Windows: Isolationsänderungen für Machine Learning Services“](install/sql-server-machine-learning-services-2019.md).

::: moniker-end

::: moniker range=">=sql-server-2017"
## <a name="new-in-sql-server-2017"></a>Neuerungen in SQL Server 2017

Ab dieser Version werden [Python-Unterstützung und branchenführende Algorithmen für maschinelles Lernen](https://cloudblogs.microsoft.com/sqlserver/2017/04/19/python-in-sql-server-2017-enhanced-in-database-machine-learning/) bereitgestellt. SQL Server 2017 wurde umbenannt, um den neuen Anwendungsbereich widerzuspiegeln. Gleichzeitig geht hiermit die Einführung von [SQL Server Machine Learning Services (datenbankintern)](sql-server-machine-learning-services.md) einher, einschließlich der Sprachunterstützung für Python und R. 

Ankündigungen sämtlicher neuer Funktionen finden Sie unter [Neues in SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md).

### <a name="r-enhancements"></a>R-Erweiterungen

Bei der R-Komponente von SQL Server Machine Learning Services handelt es sich um die nächste Generation von SQL Server 2016 R Services, einschließlich aktualisierter Versionen von Basis-R, RevoScaler und anderen Paketen.

Zu den neuen Funktionen für R zählen die [**Paketverwaltung**](package-management/install-r-packages-with-tsql.md) mit den folgenden Highlights: 

+ Datenbankrollen helfen DBAs bei der Verwaltung von Paketen und der Zuweisung von Berechtigungen für die Paketinstallation.
+ [CREATE EXTERNAL LIBRARY](../t-sql/statements/create-external-library-transact-sql.md) hilft DBAs bei der Verwaltung von Paketen in der vertrauten Sprache T-SQL.
+ [RevoScaleR](package-management/install-r-packages-with-revoscaler.md)-Funktionen helfen bei der Installation, Entfernung oder Auflistung von Paketen, die sich im Besitz von Benutzern befinden. Weitere Informationen finden Sie unter [Verwenden von RevoScaleR-Funktionen zum Suchen oder Installieren von R-Paketen unter SQL Server](package-management/install-r-packages-with-revoscaler.md).

### <a name="r-libraries"></a>R-Bibliotheken

| Paket | BESCHREIBUNG |
|---------|-------------|
| [**MicrosoftML**](r/ref-r-microsoftml.md) | In diesem Release ist MicrosoftML in einer Standard-R-Installation integriert, wodurch der in den vorherigen SQL Server 2016 R Services erforderliche Upgradeschritt entfällt. MicrosoftML bietet modernste maschinelle Lernalgorithmen und Datentransformationen, die skaliert oder in Remotecomputekontexten ausgeführt werden können. Zu den Algorithmen gehören anpassbare Deep Neural Networks, Schnellentscheidungsstrukturen und -entscheidungswälder, lineare Regression und logistische Regression.  |

### <a name="python-integration-for-in-database-analytics"></a>Python-Integration für datenbankinterne Analysen

Python ist eine Sprache, die eine große Flexibilität und Leistungsfähigkeit für eine Vielzahl von Aufgaben des maschinellen Lernens bietet. Open-Source-Bibliotheken für Python umfassen verschiedene Plattformen für anpassbare neuronale Netze sowie beliebte Bibliotheken für die Verarbeitung natürlicher Sprache. 

Da Python in die Datenbank-Engine integriert ist, können Sie die Analysen datennah durchführen und die mit der Datenverschiebung verbundenen Kosten und Sicherheitsrisiken eliminieren. Sie können Lösungen des maschinellen Lernens auf Basis von Python mit Tools wie Visual Studio bereitstellen. Ihre Produktionsanwendungen können mit Hilfe von SQL Server-Datenzugriffsmethoden Vorhersagen, Modelle oder Visualisierungen aus der Python 3.5-Runtime abrufen.

Die T-SQL- und Python-Integration wird durch die gespeicherte Systemprozedur [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) unterstützt. Mit dieser gespeicherten Prozedur können Sie beliebigen Python-Code abrufen. Der Code wird in einer sicheren, dualen Architektur ausgeführt, die eine unternehmensweite Bereitstellung von Python-Modellen und -Skripts ermöglicht, welche über eine einfache gespeicherte Prozedur aus einer Anwendung aufgerufen werden können. Weitere Leistungssteigerungen werden durch das Streaming von Daten aus SQL an Python-Prozesse und durch die Parallelisierung von MPI-Ringen erzielt.

Sie können die T-SQL-Funktion [PREDICT](../t-sql/queries/predict-transact-sql.md) verwenden, um eine [native Bewertung](predictions/native-scoring-predict-transact-sql.md) für ein vortrainiertes Modell vorzunehmen, das vorab im erforderlichen Binärformat gespeichert wurde.

### <a name="python-libraries"></a>Python-Bibliotheken

| Paket | BESCHREIBUNG |
|---------|-------------|
|[**revoscalepy**](python/ref-py-revoscalepy.md)| Dies ist die Python-Entsprechung zu RevoScaleR. Sie können Python-Modelle für lineare und logistische Regressionen, Entscheidungsstrukturen, verstärkte Strukturen und zufällige Gesamtstrukturen erstellen, die alle parallelisierbar sind und in Remotecomputekontexten ausgeführt werden können. Dieses Paket unterstützt die Verwendung mehrerer Datenquellen und Remotecomputekontexte. Der Datenanalyst oder -entwickler kann Python-Code auf einer SQL-Server-Remoteinstanz ausführen, um Daten zu untersuchen oder Modelle zu erstellen, ohne Daten zu verschieben. |
|[**microsoftml**](python/ref-py-microsoftml.md) |Dies ist die Python-Entsprechung zum MicrosoftML R-Paket. |

### <a name="pre-trained-models"></a>Vortrainierte Modelle

[**Vortrainierte Modelle**](install/sql-pretrained-models-install.md) sind sowohl für Python als auch für R verfügbar. Verwenden Sie diese Modelle für die Bilderkennung und die Stimmungsanalyse (positiv/negativ), um Vorhersagen für Ihre eigenen Daten zu erstellen. 

### <a name="standalone-server-as-a-shared-feature-in-sql-server-setup"></a>Eigenständiger Server als freigegebene Funktion im SQL Server-Setup

Mit diesem Release wird auch [SQL Server Machine Learning Server (Standalone)](r/r-server-standalone.md) eingeführt, ein vollständig unabhängiger Data Science-Server, der statistische Analysen und Vorhersageanalysen in R und Python unterstützt. Wie bei R Services handelt es sich bei diesem Server um die nächste Version von SQL Server 2016 R Server (Standalone). Mit dem eigenständigen Server können Sie R- oder Python-Lösungen ohne Abhängigkeiten auf SQL Server verteilen und skalieren.
::: moniker-end

::: moniker range=">=sql-server-2016"
## <a name="new-in-sql-server-2016"></a>Neues in SQL Server 2016

Mit diesem Release wurden Funktionen des maschinellen Lernens mithilfe von **SQL Server 2016 R Services** in SQL Server eingeführt. Bei diesen Services handelt es sich um eine datenbankinterne Analyse-Engine für die Verarbeitung von R-Skripts mit residenten Daten innerhalb einer Datenbank-Engine-Instanz.

Darüber hinaus wurde **SQL Server 2016 R Server (Standalone)** als Möglichkeit zum Installieren von R Server auf einem Windows-Server freigegeben. Anfangs stellte SQL Server-Setup die einzige Möglichkeit dar, R Server for Windows zu installieren. In späteren Versionen konnten Entwickler und Datenanalysten, die R Server unter Windows wünschten, ein anderes eigenständiges Installationsprogramm verwenden, um das gleiche Ziel zu erreichen. Der eigenständige Server in SQL Server ist funktional gleichwertig zum eigenständigen Serverprodukt [Microsoft R Server for Windows](/machine-learning-server/install/r-server-install-windows).

Ankündigungen sämtlicher neuer Funktionen finden Sie unter [Neues in SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md).

| Release |Featureupdate |
|---------|----------------|
| CU-Ergänzungen | Die [**Echtzeitbewertung**](predictions/real-time-scoring.md) basiert auf nativen C++ Bibliotheken, um ein in einem optimierten Binärformat gespeichertes Modell zu lesen und dann Vorhersagen zu generieren, ohne die R-Runtime aufrufen zu müssen. Dadurch werden Bewertungsvorgänge erheblich beschleunigt. Mit der Echtzeitbewertung können Sie eine gespeicherte Prozedur ausführen oder die Echtzeitbewertung aus R-Code durchführen. Die Echtzeitbewertung ist auch für SQL Server 2016 verfügbar, wenn für die Instanz ein Upgrade auf die neueste Version von [!INCLUDE[rsql-platform-md](../includes/rsql-platform-md.md)] durchgeführt wurde. |
| Erste Veröffentlichung | [**R-Integration für datenbankinterne Analysen**](r/sql-server-r-services.md). <br/><br/> R-Pakete zum Aufrufen von R-Funktionen in T-SQL und umgekehrt. RevoScaleR-Funktionen bieten skalierte R-Analysen, indem sie Daten in einzelne Komponenten zerlegen, die verteilte Verarbeitung koordinieren und verwalten und die Ergebnisse aggregieren. In SQL Server 2016 R Services (datenbankintern) ist die RevoScaleR-Engine in eine Instanz der Datenbank-Engine integriert, sodass Daten und Analysen in demselben Verarbeitungskontext zusammengefasst werden. <br/><br/>T-SQL- und R-Integration über [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Mit dieser gespeicherten Prozedur können Sie beliebigen R-Code abrufen. Diese sichere Infrastruktur erlaubt eine unternehmensweite Bereitstellung von Rn-Modellen und -Skripts, welche über eine einfache gespeicherte Prozedur aus einer Anwendung aufgerufen werden können. Weitere Leistungssteigerungen werden durch das Streaming von Daten aus SQL an R-Prozesse und durch die Parallelisierung von MPI-Ringen erzielt. <br/><br/>Sie können die T-SQL-Funktion [PREDICT](../t-sql/queries/predict-transact-sql.md) verwenden, um eine [native Bewertung](predictions/native-scoring-predict-transact-sql.md) für ein vortrainiertes Modell vorzunehmen, das vorab im erforderlichen Binärformat gespeichert wurde.|

::: moniker-end

::: moniker range=">=sql-server-2017"
## <a name="linux-support"></a>Linux-Unterstützung

In SQL Server 2019 wird eine Linux-Unterstützung für R und Python bereitgestellt, wenn Sie die Pakete für das maschinelle Lernen mit einer Datenbank-Engine-Instanz installieren. Weitere Informationen finden Sie unter [Installieren von SQL Server Machine Learning Services unter Linux](../linux/sql-server-linux-setup-machine-learning.md).

Unter Linux verfügt SQL Server 2017 nicht über eine R- oder Python-Integration, Sie können jedoch die [native Bewertung](predictions/native-scoring-predict-transact-sql.md) unter Linux verwenden, da diese Funktion über die T-SQL-Funktion [PREDICT](../t-sql/queries/predict-transact-sql.md) verfügbar ist, die unter Linux ausgeführt wird. Die native Bewertung ermöglicht eine leistungsstarke Bewertung aus einem vortrainierten Modell, ohne dass eine R-Runtime aufgerufen oder gar benötigt wird.
::: moniker-end

## <a name="next-steps"></a>Nächste Schritte

+ [Installieren von SQL Server Machine Learning Services (datenbankintern)](install/sql-machine-learning-services-windows-install.md)
