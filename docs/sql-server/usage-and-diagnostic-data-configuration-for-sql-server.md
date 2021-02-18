---
title: Konfigurieren der Nutzungs- und Diagnosedatensammlung für SQL Server (CEIP) | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, welche Informationen SQL Server von Benutzern sammelt, um Produkte zu verbessern. Außerdem erfahren Sie, wie Sie SQL Server so konfigurieren, dass diese Informationen nicht gesendet werden.
author: MikeRayMSFT
ms.author: mikeray
ms.date: 03/27/2019
ms.topic: conceptual
ms.prod: sql
ms.custom: ''
ms.technology: configuration
ms.openlocfilehash: 0fc8437d6ffb3382dbcc53edc816b00ec22b7a07
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353575"
---
# <a name="configure-usage-and-diagnostic-data-collection-for-sql-server-ceip"></a>Konfigurieren der Sammlung von Nutzungs- und Diagnosedaten für SQL Server (CEIP)

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

## <a name="summary"></a>Zusammenfassung

Microsoft SQL Server erfasst standardmäßig Informationen darüber, wie Kunden die Anwendung verwenden. Dies bedeutet, dass SQL Server Informationen zur Installationserfahrung, Nutzung und Leistung sammelt. Mit diesen Informationen kann Microsoft besser an die Bedürfnisse der Kunden anpassen. Microsoft erfasst z.B. Informationen zu Fehlercodes von Kunden, sodass wir damit verknüpfte Probleme beheben, die Dokumentation zu SQL Server verbessern und bestimmen können, ob wir dem Produkt weitere Funktionen hinzufügen müssen, um die Benutzererfahrung zu optimieren.

Microsoft sendet nicht die folgenden Informationen mit diesem Mechanismus:
- Werte aus Benutzertabellen
- Anmeldeinformationen oder andere Authentifizierungsinformationen
- Personenbezogene Informationen (Personally Identifiable Information, PII)

Die folgenden Beispielszenarios beinhalten Informationen zur Nutzung von Funktionen, die zur Verbesserung des Produkts beitragen.

SQL Server 2017 unterstützt Columnstore-Indizes für Szenarios, in denen schnelle Analysen erforderlich sind. Columnstore-Indizes vereinen eine herkömmliche B-Baumindexstruktur für neu eingefügte Daten mit einer besonderen spaltenorientierten, komprimierten Struktur zur Komprimierung von Daten und Beschleunigung der Abfrageausführung. Das Produkt enthält Heuristik zum Migrieren von Daten aus der B-Baumstruktur zur komprimierten Struktur, was im Hintergrund durchgeführt wird. Dadurch werden zukünftige Abfrageergebnisse beschleunigt.

Wenn der Vorgang im Hintergrund nicht mit der Rate an eingefügten Daten mithalten kann, ist die Abfrageleistung möglicherweise langsamer als erwartet. Um das Produkt zu verbessern, erfasst Microsoft Informationen darüber, wie gut SQL Server mit dem automatischen Kompressionsprozess mithalten kann. Das Produktteam verwendet diese Informationen, um die Häufigkeit und Parallelität von Code zu optimieren, der die Kompression durchführt. Diese Abfrage wird gelegentlich ausgeführt, um diese Informationen zu erfassen, damit wir von Microsoft die Datenverschiebungsrate bewerten können. Damit können wir die Produktheuristik optimieren.  

```sql
SELECT object_id, type_desc, data_space_id, db_id() AS database_id FROM sys.indexes WITH(nolock) WHERE type = 5 or type = 6 
```

```sql
SELECT cntr_value as merge_policy_evaluation
FROM sys.dm_os_performance_counters WITH(nolock)
WHERE object_name LIKE '%columnstore%' 
AND counter_name ='Total Merge Policy Evaluations' 
AND instance_name = '_Total'
```

Beachten Sie, dass dieser Prozess auf den erforderlichen Mechanismus für die Lieferung von Werten an den Kunden konzentriert ist. Das Produktteam schaut sich die Daten im Index nicht an und sendet diese auch nicht an Microsoft. SQL Server 2017 erfasst und sammelt kontinuierlich Informationen zur Installationserfahrung des Einrichtungsvorgangs, damit wir schnell Installationsprobleme erkennen und beheben können, die der Kunde hat. SQL Server 2017 kann so konfiguriert werden, dass es keine Informationen (pro Serverinstanz) an Microsoft mit den folgenden Mechanismen sendet:
- Mit der Anwendung „Fehler- und Verwendungsberichterstellung“
- Mit dem Festlegen von Registrierungsunterschlüsseln auf dem Server

Schauen Sie sich [Kundenfeedback zu SQL Server unter Linux](../linux/usage-and-diagnostic-data-configuration-for-sql-server-linux.md) an, um Informationen zu SQL Server unter Linux zu erhalten.

> [!NOTE]
> Sie können das Senden von Informationen an Microsoft nur in bezahlten Versionen von SQL Server deaktivieren.

## <a name="remarks"></a>Bemerkungen
 - Das Entfernen oder Deaktivieren des CEIP-Diensts für SQL wird nicht unterstützt. 
 - Das Entfernen von CEIP-Ressourcen für SQL aus der Clustergruppe wird nicht unterstützt. 

Informationen zur Deaktivierung der Datensammlung finden Sie unter [Aktivieren oder Deaktivieren der lokalen Überwachung](usage-and-diagnostic-data-in-local-audit.md#turning-local-audit-on-or-off).

## <a name="error-and-usage-reporting-application"></a>Anwendung „Fehler- und Verwendungsberichterstellung“ 

Nach dem Setup kann die Einstellung für die Nutzungs- und Diagnosedatensammlung für SQL Server-Komponenten und -Instanzen über die Anwendung „Fehler- und Verwendungsberichterstellung“ geändert werden. Diese Anwendung ist mit der Installation von SQL Server erhältlich. Mit diesem Tool kann für jede SQL Server-Instanz eine eigene Einstellung für Verwendungsberichte konfiguriert werden.

> [!NOTE]
> Die Anwendung „Fehler- und Verwendungsberichterstellung“ ist unter den Konfigurationstools von SQL Server aufgelistet. Mit diesem Tool können Sie Ihre Einstellungen für die Fehlerberichterstattung und die Nutzungs- und Diagnosedatensammlung auf dieselbe Weise wie in SQL Server 2017 verwalten. Die Fehlerberichterstattung ist unabhängig von der Nutzungs- und Diagnosedatensammlung und kann daher getrennt von der Nutzungs- und Diagnosedatensammlung aktiviert oder deaktiviert werden. Bei der Fehlerberichterstattung werden Absturzabbilder erfasst, die an Microsoft gesendet werden, und die möglicherweise vertrauliche Informationen enthalten, wie in den [Datenschutzbestimmungen](./sql-server-privacy.md) erläutert wird.

Um die Fehler- und Verwendungsberichterstellung von SQL Server zu starten, klicken oder tippen Sie auf **Start**, und suchen Sie anschließend im Suchfeld nach „Fehler“. Das SQL-Server-Element „Fehler- und Verwendungsberichterstellung“ wird angezeigt. Nachdem Sie das Tool gestartet haben, können Sie die Nutzungs- und Diagnosedaten und schwerwiegende Fehler verwalten, die für Instanzen und Komponenten gesammelt wurden, die auf diesem Computer installiert sind.

Verwenden Sie in kostenpflichtigen Versionen die Kontrollkästchen „Verwendungsberichte“, um zu bestimmen, welche Nutzungs- und Diagnosedaten an Microsoft gesendet werden.

Verwenden Sie für bezahlte oder kostenlose Versionen die Kontrollkästchen „Fehlerberichte“, um zu bestimmen, welche Daten zu schwerwiegenden Fehlern an Microsoft gesendet werden.

## <a name="set-registry-subkeys-on-the-server"></a>Festlegen von Registrierungsunterschlüsseln auf dem Server

Enterprise-Kunden können die Einstellungen für Gruppenrichtlinien konfigurieren, um die Nutzungs- und Diagnosedatensammlung zu aktivieren oder zu deaktivieren. Dies erfolgt mit dem Konfigurieren einer registrierungsbasierten Richtlinie. Der entsprechende Registrierungsunterschlüssel und die entsprechenden Einstellungen lauten wie folgt:

- Für Funktionen einer SQL-Server-Instanz:
    
    Unterschlüssel = HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\{InstanzID}\CPE
    
    Name des Registrierungseintrags = CustomerFeedback
    
    DWORD-Eintrag: 0 bedeutet „deaktiviert“, 1 bedeutet „aktiviert“
    
    {InstanzID} ist der Instanztyp und die Instanz, wie in folgendem Beispiel:

    - MSSQL14.CANBERRA für SQL Server 2017-Datenbank-Engine mit dem Instanznamen „CANBERRA“
    - MSAS14.CANBERRA für SQL Server 2017-Analysis Services mit dem Instanznamen „CANBERRA“
    - MSRS14.CANBERRA für SQL Server 2017-Reporting Services mit dem Instanznamen „CANBERRA“

- Für alle freigegebenen Funktionen:
    
    Unterschlüssel = HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\{Hauptversion}
    
    Name des Registrierungseintrags = CustomerFeedback
    
    DWORD-Eintrag: 0 bedeutet „deaktiviert“, 1 bedeutet „aktiviert“

> [!NOTE]
> {Hauptversion} ist die Version von SQL Server, z. B. 140 für SQL Server 2017

- Informationen zu SQL Server Management Studio 17 und SQL Server Management Studio 18 finden Sie unter [Benutzerunterstützung in SQL Server Management Studio](../ssms/sql-server-management-studio-telemetry-ssms.md).

## <a name="set-registry-subkeys-for-crash-dump-collection"></a>Festlegen des Registrierungsunterschlüssels für die Absturzabbilderfassung

Kunden von SQL Server 2017 Enterprise können, ähnlich zum Verhalten früherer Versionen von SQL Server, Einstellungen für Gruppenrichtlinien auf dem Server konfigurieren, um die Absturzabbilderfassung zu aktivieren oder zu deaktivieren. Dies erfolgt mit dem Konfigurieren einer registrierungsbasierten Richtlinie. Die entsprechenden Registrierungsschlüssel und Einstellungen lauten wie folgt: 

- Für Funktionen einer SQL-Server-Instanz:

    Unterschlüssel = HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\{InstanzID}\CPE

    Name des Registrierungseintrags = EnableErrorReporting

    DWORD-Eintrag: 0 bedeutet „deaktiviert“, 1 bedeutet „aktiviert“
 
    {InstanzID} ist der Instanztyp und die Instanz, wie in folgendem Beispiel: 

    - MSSQL14.CANBERRA für SQL Server 2017-Datenbank-Engine mit dem Instanznamen „CANBERRA“
    - MSAS14.CANBERRA für SQL Server 2017-Analysis Services mit dem Instanznamen „CANBERRA“
    - MSRS14.CANBERRA für SQL Server 2017-Reporting Services mit dem Instanznamen „CANBERRA“
 

- Für alle freigegebenen Funktionen:
    
    Unterschlüssel = HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\{Hauptversion}

    Name des Registrierungseintrags = EnableErrorReporting

    DWORD-Eintrag: 0 bedeutet „deaktiviert“, 1 bedeutet „aktiviert“

> [!NOTE]
> {Hauptversion} ist die Version von SQL Server. „140“ ist z.B. SQL Server 2017.

Die registrierungsbasierte Gruppenrichtlinie dieses Registrierungsschlüssels wird von der Absturzabbilderfassung von SQL Server 2017 berücksichtigt. 

## <a name="crash-dump-collection-for-ssms"></a>Absturzabbilderfassung für SSMS
SSMS erfasst nicht seine eigenen Absturzabbilder. Jedes Absturzabbild, das mit SSMS in Verbindung steht, wird im Rahmen der Windows-Fehlerberichterstattung erfasst.

Wie Sie diese Funktion deaktivieren oder aktivieren können, hängt von der Version Ihres Betriebssystems ab. Halten Sie sich zum Aktivieren oder Deaktivieren dieser Funktion an die Schritte im entsprechenden Artikel für Ihr Betriebssystem.
 
- Windows Server 2016 und Windows 10

    [Konfigurieren von Windows-Diagnosedaten in Ihrem Unternehmen](/windows/privacy/configure-windows-diagnostic-data-in-your-organization)
- Windows 2008 R2 und Windows Server 7

    [WER Settings (WER-Einstellungen)](/windows/desktop/wer/wer-settings)
 
## <a name="feedback-for-analysis-services"></a>Feedback zu Analysis Services

Während der Installation fügt SQL Server 2016 Analysis Services ein gesondertes Konto zu Ihrer Instanz von Analysis Services hinzu. Dieses Konto ist ein Member der Rolle „Administrator des Analysis Services-Server“. Das Konto wird dazu verwendet, Informationen für das Senden von Feedback für die Instanz von Analysis Services zu sammeln.  

Sie können Ihren Dienst so konfigurieren, dass keine Nutzungs- und Diagnosedaten gesendet werden. Die Vorgehensweise ist im Abschnitt „Festlegen von Registrierungsunterschlüsseln auf dem Server“ beschrieben. Wenn Sie dies machen, wird das Dienstkonto jedoch nicht entfernt. 
 
[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]