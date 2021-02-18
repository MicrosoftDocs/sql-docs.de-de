---
description: Lokale Überwachung für SSMS-Nutzungs- und -Diagnosedatensammlung
title: Verbrauchs- und Diagnosedaten
ms.custom: seo-lt-2019
ms.date: 04/16/2019
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b2e188200daf0469f80cf74db901bcc240fd8706
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340695"
---
# <a name="local-audit-for-ssms-usage-and-diagnostic-data-collection"></a>Lokale Überwachung für SSMS-Nutzungs- und -Diagnosedatensammlung
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SQL Server Management Studio (SSMS) enthält internetfähige Features, die anonyme Featurenutzungs- und Diagnosedaten sammeln und an Microsoft senden können. SSMS erfasst möglicherweise Standardinformationen zu Ihrem Computer und Informationen zur Nutzung und Leistung, die möglicherweise an Microsoft übermittelt und analysiert werden, um die Qualität, Sicherheit und Zuverlässigkeit von SSMS zu optimieren. Wir erfassen nicht Ihren Namen, Ihre Adresse oder andere Kontaktinformationen. Ausführliche Informationen finden Sie in den [Datenschutzbestimmungen von Microsoft](https://privacy.microsoft.com/privacystatement) und den [Ergänzenden Datenschutzbestimmungen zu SQL Server](../sql-server/sql-server-privacy.md).

## <a name="audit-feature-usage-and-diagnostic-data"></a>Überwachen der Verwendung von Features und Diagnosedaten

Führen Sie die folgenden Schritte aus, um die von SSMS erfassten Daten zur Nutzung von Features anzuzeigen:

1.  Starten Sie SSMS.
2.  Klicken Sie auf **View** (Ansicht), und klicken Sie anschließend im Hauptmenü auf **Output** (Ausgabe), um das Fenster **Output** (Ausgabe) anzuzeigen. 
3.  Wenn das Fenster **Output** (Ausgabe) angezeigt wird, wählen Sie im Menü **Show output from:** (Ausgabe anzeigen von:) **Telemetry** (Telemetrie) aus.

Während Sie SSMS verwenden, um mit Ihrer Datenbank zu interagieren, zeigt das Fenster **Output** (Ausgabe) die erfassten Daten an.

## <a name="enable-or-disable-usage-and-diagnostic-data-collection-in-ssms"></a>Aktivieren oder Deaktivieren der Sammlung von Nutzung- und Diagnosedaten in SSMS

So aktivieren oder deaktivieren Sie die Sammlung von Nutzungsdaten für SSMS

- Für SQL Server Management Studio 17:

  `Subkey = HKEY_CURRENT_USER\Software\Microsoft\SQL Server Management Studio\14.0`

  RegEntry name = `UserFeedbackOptIn`

  Eintragstyp `DWORD`: `0` steht für Deaktivieren und `1` für Aktivieren.

  Außerdem basiert SSMS 17.x auf der Visual Studio 2015-Shell, und die Visual Studio-Installation ermöglicht standardmäßig Kundenfeedback.  

  Um Visual Studio so zu konfigurieren, dass Kundenfeedback auf einzelnen Computern deaktiviert ist, ändern Sie den Wert des folgenden Registrierungsunterschlüssels in die Zeichenfolge `0`: `HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\VisualStudio\SQM OptIn`

  Beispiel: Ändern Sie den Unterschlüssel wie folgt:  
  `HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\VisualStudio\SQM OptIn `=` 0`

  Die registrierungsbasierte Gruppenrichtlinie dieser Registrierungsschlüssel wird von der Nutzungs- und Diagnosedatenerfassung von SQL Server 2017 berücksichtigt.

- Für SQL Server Management Studio 18:

  `Subkey = HKEY_CURRENT_USER\Software\Microsoft\SQL Server Management Studio\18.0_IsoShell`

  RegEntry name = `UserFeedbackOptIn`

  Eintragstyp `DWORD`: `0` steht für Deaktivieren und `1` für Aktivieren.

## <a name="see-also"></a>Siehe auch

- [Konfigurieren der Nutzungs- und Diagnosedatensammlung für SQL Server](../sql-server/usage-and-diagnostic-data-configuration-for-sql-server.md)
- [Lokale Überwachung für SQL Server-Nutzungs- und -Diagnosedatensammlung](../sql-server/usage-and-diagnostic-data-in-local-audit.md)