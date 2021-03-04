---
title: Dialogfeld „SQL Server-Anmeldung“ (ODBC)
description: Das Dialogfeld „SQL Server-Anmeldung“ kann angezeigt werden, wenn von einer Anwendung eine ODBC-Verbindung hergestellt wird und nicht genügend Informationen für die Verbindung mit der Datenbank angegeben werden.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 716e95a477bdc517cde7fa0980a2a2b2c7b65eb8
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837277"
---
# <a name="sql-server-login-dialog-box-odbc"></a>Dialogfeld „SQL Server-Anmeldung“ (ODBC)

Wenn Sie eine ODBC-Verbindung aufrufen, ohne ausreichende Informationen anzugeben, damit der Treiber eine Verbindung mit einem SQL-Server herstellen kann, zeigt der ODBC-Treiber das Dialogfeld **SQL Server-Anmeldung** an.

## <a name="options"></a>Tastatur

### <a name="server"></a>Server

Hier geben Sie den Namen einer SQL Server-Instanz in Ihrem Netzwerk ein. Wählen Sie einen Server-/Instanzennamen aus der Liste aus, oder geben Sie den Server-/Instanzennamen in das Feld **Server** ein. Optional können Sie mit dem **SQL Server-Konfigurations-Manager** einen Serveralias auf dem Clientcomputer erstellen und diesen Namen in das Feld **Server** eingeben.

Sie können „(local)“ eingeben, wenn Sie den gleichen Computer wie SQL Server verwenden. Sie können anschließend eine Verbindung mit einer lokalen Instanz von SQL Server herstellen, selbst wenn eine nicht vernetzte Version von SQL Server ausgeführt wird.

Weitere Informationen über Servernamen für andere Typen von Netzwerken finden Sie in der SQL Server-Installationsdokumentation in der SQL Server-Onlinedokumentation.

### <a name="authentication-mode"></a>Authentifizierungsmodus

Wählen Sie hier einen der folgenden Authentifizierungsmodi aus:
- **SQL Server** mit Anmelde-ID und Kennwort
- **Integrierte Windows-Authentifizierung** mit dem Konto des aktuell angemeldeten Benutzers
- **Active Directory-Kennwort** mit Anmelde-ID und Kennwort
- **Integrierte Active Directory Authentifizierung** mit dem Konto des aktuell angemeldeten Benutzers
- **Interaktive Active Directory-Authentifizierung** mit Anmelde-ID
- Authentifizierung mithilfe der **verwalteten Dienstidentität** (verwalteten Identität)
- **Authentifizierung mit Dienstprinzipal für Active Directory** mit Azure Active Directory-Dienstprinzipal

Weitere Informationen zu den Authentifizierungsmodi finden Sie unter [Datenquellen-Assistent (Bildschirm 2)](../../../connect/odbc/windows/dsn-wizard-2.md).

### <a name="server-spn"></a>Server-SPN

Wenn Sie eine vertrauenswürdige Verbindung verwenden, können Sie einen Dienstprinzipalnamen (SPN) für den Server angeben.

### <a name="login-id"></a>Login ID

Wenn Sie als **Authentifizierungsmodus** die Option **SQL Server**, **Active Directory-Kennwort**, **Interaktive Active Directory-Authentifizierung**, **Verwaltete Dienstidentität** oder **Active Directory-Dienstprinzipal** ausgewählt haben, geben Sie hier die SQL Server- oder Azure Active Directory-Anmelde-ID an, die für die Verbindung verwendet werden soll. Haben Sie eine andere Option ausgewählt, ist das Feld **Anmelde-ID** deaktiviert.

### <a name="password"></a>Kennwort

Wenn Sie als **Authentifizierungsmethode** die Option **SQL Sever** oder **Active Directory-Kennwort** ausgewählt haben, geben Sie hier das entsprechende Kennwort der SQL Server- oder Azure Active Directory-Anmelde-ID an, die für die Verbindung verwendet wird. Haben Sie eine andere Option ausgewählt, ist das Feld **Kennwort** deaktiviert.

### <a name="options"></a>Tastatur

Zeigt die Gruppe **Optionen** an oder blendet sie aus. Die Schaltfläche **Optionen** wird aktiviert, wenn **Server** über einen Wert verfügt.

### <a name="change-password"></a>Ändern des Kennworts

Wenn dieses Kästchen aktiviert wird, werden die Felder **Neues Kennwort** und **Neues Kennwort bestätigen** angezeigt.

### <a name="new-password"></a>Neues Kennwort

Gibt das neue Kennwort an.

### <a name="confirm-new-password"></a>Neues Kennwort bestätigen

Gibt das neue Kennwort zur Bestätigung ein zweites Mal an.

### <a name="database"></a>Datenbank

Gibt die Standarddatenbank an, die für die Verbindung verwendet werden soll. Diese Einstellung überschreibt die für die Anmeldung auf dem Server festgelegte Standarddatenbank. Wenn keine Datenbank angegeben ist, wird für die Verbindung die Standarddatenbank verwendet, die für die Anmeldung auf dem Server angegeben ist.

### <a name="mirror-server"></a>Spiegelserver

Gibt den Namen des Failoverpartners der zu spiegelnden Datenbank an.

### <a name="mirror-spn"></a>Spiegelserver-SPN

Optional können Sie einen SPN für den Spiegelserver angeben. Der SPN für den Spiegelserver wird zur gegenseitigen Authentifizierung zwischen Client und Server verwendet.

### <a name="language"></a>Sprache

Gibt die Landessprache an, die für SQL Server-Systemmeldungen verwendet werden soll. Auf dem Computer, auf dem SQL Server ausgeführt wird, muss dies Sprache installiert sein. Diese Einstellung überschreibt die für die Anmeldung auf dem Server festgelegte Standardsprache. Wenn keine Sprache angegeben ist, wird für die Verbindung die Standardsprache verwendet, die für die Anmeldung auf dem Server angegeben ist.

### <a name="application-name"></a>Anwendungsname

(Optional) Gibt den Anwendungsnamen an, der in der Spalte **program_name** in der Zeile für diese Verbindung in **sys.sysprocesses** gespeichert werden soll.

### <a name="workstation-id"></a>Workstation ID

(Optional) Gibt die Workstation ID an, die in der Spalte **hostname** in der Zeile für diese Verbindung in **sys.sysprocesses** gespeichert werden soll.

### <a name="use-strong-encryption-for-data"></a>Starke Verschlüsselung für Daten verwenden

Wenn diese Option ausgewählt ist, werden die Daten, die über die Verbindung übermittelt werden, verschlüsselt. Anmeldungen werden standardmäßig verschlüsselt, auch wenn das Kontrollkästchen deaktiviert wird.

### <a name="trust-server-certificate"></a>Serverzertifikat vertrauen

Diese Option ist nur verfügbar, wenn die Option **Starke Verschlüsselung für Daten verwenden** aktiviert ist. Wenn Sie diese Option auswählen, wird nicht geprüft, ob das Zertifikat den richtigen Hostnamen des Servers hat und von einer vertrauenswürdigen Zertifizierungsstelle ausgestellt wurde.

## <a name="see-also"></a>Weitere Informationen

[Microsoft ODBC Driver for SQL Server unter Windows](../../../connect/odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows.md)
