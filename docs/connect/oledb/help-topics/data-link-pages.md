---
title: Konfigurieren von Universal Data Link (UDL) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die Registerkarte „Verbindung“ verwenden, um anzugeben, wie über den OLE DB-Treiber für SQL Server eine Verbindung mit Ihren Daten hergestellt werden soll.
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.author: v-beaziz
author: bazizi
ms.openlocfilehash: e3a7367daa776da3641c1b9a506f8f8335ce05d9
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837465"
---
# <a name="universal-data-link-udl-configuration"></a>Universal Data Link (UDL)-Konfiguration
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

## <a name="connection-tab"></a>Registerkarte „Verbindung“
Geben Sie auf der Registerkarte „Verbindung“ an, wie Sie über den Microsoft OLE DB-Treiber für SQL Server eine Verbindung mit Ihren Daten hergestellt werden soll.

![Screenshot der OLE DB-Seiten für Datenverbindungen – Registerkarte „Verbindung“](../media/data-link-pages-connection-tab.png)

Diese Registerkarte „Verbindung“ ist anbieterspezifisch und zeigt nur die Verbindungseigenschaften an, die für den Microsoft OLE DB-Treiber für SQL Server erforderlich sind.

|Option|BESCHREIBUNG|
|---   |---        |
|Wählen Sie einen Servernamen aus, oder geben Sie ihn ein.|Wählen Sie den Namen eines Servers aus der Dropdownliste, oder geben Sie den Speicherort des Servers ein, auf dem sich die gewünschte Datenbank befindet. Das Auswählen der Datenbank auf dem Server ist eine separate Aktion. Aktualisieren Sie die Liste, indem Sie auf „Aktualisieren“ klicken.
|Geben Sie Informationen zum Anmelden am Server ein|Sie können in der Dropdownliste die folgenden Authentifizierungsoptionen auswählen: <ul><li>`Windows Authentication:` Authentifizierung gegenüber SQL Server über die Anmeldeinformationen des Windows-Kontos, das aktuell angemeldet ist.</li><li>`SQL Server Authentication:` Authentifizierung über eine Anmelde-ID und ein Kennwort.</li><li>`Active Directory - Integrated:` Integrierte Authentifizierung mit einer Azure Active Directory-Identität. Dieser Modus kann auch für die Windows-Authentifizierung gegenüber SQL Server verwendet werden.</li><li>`Active Directory - Password:` Authentifizierung mit Benutzer-ID und Kennwort über eine Azure Active Directory-Identität.</li><li>`Active Directory - Universal with MFA support:` Interaktive Authentifizierung mit einer Azure Active Directory-Identität. Dieser Modus unterstützt die mehrstufige Authentifizierung (MFA) von Azure Active Directory.</li><li>`Active Directory - Service Principal:` Authentifizierung mit einem Azure Active Directory-Dienstprinzipal. Als **Benutzername** sollte die ID der Anwendung (des Clients) festgelegt werden. Als **Kennwort** sollte das Geheimnis der Anwendung (des Clients) festgelegt werden.</li></ul>|
|Server-SPN|Wenn Sie eine vertrauenswürdige Verbindung verwenden, können Sie einen Dienstprinzipalnamen (SPN) für den Server angeben.|
|Benutzername|Geben Sie die Benutzer-ID ein, die bei der Anmeldung bei der Datenquelle für die Authentifizierung verwendet werden soll.|
|Kennwort|Geben Sie das Kennwort ein, das bei der Anmeldung bei der Datenquelle für die Authentifizierung verwendet werden soll.|
|Leeres Kennwort|Wenn diese Option aktiviert ist, kann der angegebene Anbieter in der Verbindungszeichenfolge ein leeres Kennwort verwenden.|
|Speichern von Kennwort zulassen|Wenn diese Option aktiviert ist, kann das Kennwort mit der Verbindungszeichenfolge gespeichert werden. Es hängt von den Funktionen der aufrufenden Anwendung ab, ob das Kennwort in der Verbindungszeichenfolge enthalten ist. <br/><br/>**HINWEIS:** Das Kennwort wird ohne Maskierung und Verschlüsselung zurückgegeben und gespeichert.|
|Starke Verschlüsselung für Daten verwenden|Wenn diese Option aktiviert ist, werden die Daten, die über die Verbindung übermittelt werden, verschlüsselt.|
|Serverzertifikat vertrauen|Wenn dieses Kontrollkästchen aktiviert ist, wird das Serverzertifikat überprüft. Das Serverzertifikat muss über den richtigen Hostnamen des Servers verfügen und von einer vertrauenswürdigen Zertifizierungsstelle ausgestellt worden sein.|
|Wählen Sie die Datenbank aus.|Wählen Sie den Namen der Datenbank aus, auf die Sie zugreifen möchten, oder geben Sie den Namen ein.|
|Datenbankdatei als Datenbankname anfügen|Gibt den Namen der primären Datei für eine anfügbare Datenbank an. Diese Datenbank wird angefügt und als Standarddatenbank für die Datenquelle verwendet. Geben Sie im ersten Textfeld unter diesem Abschnitt den Datenbanknamen ein, der für die angefügte Datenbankdatei verwendet werden soll.<br/><br/>Geben Sie im Textfeld `Using the filename` den vollständigen Pfad der Datenbankdatei ein, die angefügt werden soll, oder klicken Sie auf `...`, um nach der Datenbankdatei zu suchen.|
|Ändern des Kennworts|Zeigt das Dialogfeld „SQL Server-Kennwort ändern“ an. |
|Testen der Verbindung|Klicken Sie auf diese Option, um eine Verbindung mit der angegebenen Datenquelle herzustellen. Wenn die Verbindung fehlschlägt, stellen Sie sicher, dass die Einstellungen richtig sind. Zum Beispiel können Rechtschreibfehler und die Unterscheidung nach Groß-/Kleinschreibung das Fehlschlagen von Verbindungen verursachen.|

## <a name="advanced-tab"></a>Registerkarte „Erweitert“
Auf der Registerkarte „Erweitert“ können zusätzliche Initialisierungseigenschaften angezeigt und festgelegt werden.

![Screenshot der OLE DB-Seiten für Datenverbindungen – Registerkarte „Erweitert“](../media/data-link-pages-advanced-tab.png)

|Option|BESCHREIBUNG|
|---   |---        |
| Connect timeout | Gibt den Zeitraum (in Sekunden) an, den der Microsoft OLE DB-Treiber für SQL Server wartet, bis die Initialisierung abgeschlossen wurde. Wenn es bei der Initialisierung zu einem Timeout kommt, wird ein Fehler zurückgegeben, und die Verbindung wird nicht hergestellt.|


> [!NOTE]  
>  Weitere allgemeine Informationen zu Data Link-Verbindungen finden Sie in der [Übersicht über die Data Link-API](/previous-versions/windows/desktop/ms718102(v=vs.85)).

## <a name="next-steps"></a>Nächste Schritte
- [Authentifizierung gegenüber Azure Active Directory](../features/using-azure-active-directory.md) unter Verwendung des OLE DB-Treibers.

- [Auffordern von Benutzern zur Eingabe von Anmeldeinformationen zur Authentifizierung](../help-topics/sql-server-login-dialog.md) unter Verwendung des OLE DB-Treibers.