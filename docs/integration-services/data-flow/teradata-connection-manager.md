---
description: Verwenden des Teradata-Verbindungs-Managers
title: Verwenden des Teradata-Verbindungs-Managers | Microsoft-Dokumentation
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 85465e017126a116289c396787d7d8ccecba042f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339887"
---
# <a name="use-the-teradata-connection-manager"></a>Verwenden des Teradata-Verbindungs-Managers

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Mit dem Teradata-Verbindungs-Manager können Sie ein Paket zum Extrahieren von Daten aus Teradata-Datenbanken und zum Laden von Daten in Teradata-Datenbanken aktivieren.

Legen Sie die `ConnectionManagerType`-Eigenschaft des Teradata-Verbindungs-Managers auf *TERADATA* fest.

## <a name="configure-the-teradata-connection-manager"></a>Konfigurieren des Teradata-Verbindungs-Managers

Änderungen der Konfiguration des Verbindungs-Managers werden von Integration Services zur Runtime aufgelöst. Um einer Teradata-Datenquelle eine Verbindung hinzuzufügen, vervollständigen Sie die Informationen im Bereich **Teradata-Verbindungs-Manager-Editor**.

![Der Bereich „Teradata-Verbindungs-Manager-Editor“](media/teradata-connection-manager.png)

1. Geben Sie im Feld **Name** einen Namen für die Verbindung ein. Der Standardname ist **Teradata-Verbindungs-Manager**.

1. Geben Sie (optional) im Feld **Beschreibung** eine Beschreibung der Verbindung ein.

1. Geben Sie im Feld **Servername** den Namen des Teradata-Servers ein, mit dem eine Verbindung hergestellt werden soll.

1. Führen Sie unter **Authentifizierung** einen der folgenden Schritte aus:

   - Um die Windows-Authentifizierung zu verwenden, wählen Sie **Windows-Authentifizierung verwenden** aus.
   - Wählen Sie zum Verwenden der Teradata-Datenbankauthentifizierung **Teradata-Authentifizierung verwenden** aus, und geben Sie dann die folgenden Anmeldeinformationen für diesen Authentifizierungstyp ein:
     - Geben Sie im Feld **Mechanismus** den Sicherheitsüberprüfungsmechanismus ein, den Sie verwenden möchten. Gültige Mechanismuswerte sind TD1, TD2, LDAP, KRB5, KRB5C, NTLM und NTLMC.
     - Geben Sie im Feld **Parameter** die Parametertypen ein, die für den von Ihnen eingegebenen Sicherheitsüberprüfungsmechanismus erforderlich sind.
     - Geben Sie im Feld **Benutzername** den Benutzernamen ein, den Sie zum Herstellen der Verbindung mit der Teradata-Datenbank verwenden.  
     - Geben Sie im Feld **Kennwort** das Teradata-Datenbank-Kennwort des Benutzers ein.

1. (Optional) Wählen Sie in der Dropdownliste **Standarddatenbank** die Teradata-Datenbank aus, mit der eine Verbindung hergestellt werden soll. Wenn diese Datenbankzugriffsberechtigung falsch ist, wird ein Fehler angezeigt, und Sie können den Datenbanknamen dann manuell eingeben.

1. (Optional) Geben Sie im Feld **Konto** den Namen des Kontos ein, das dem Benutzernamen entspricht. Wenn hier kein Wert angegeben ist, wird das Konto für den unmittelbaren Besitzer der Datenbank verwendet.
1. Klicken Sie auf **OK**.

## <a name="custom-property"></a>Benutzerdefinierte Eigenschaft

Die benutzerdefinierte Eigenschaft `UseUTF8CharSet` gibt an, ob der UTF-8-Zeichensatz verwendet wird. Der Standardwert ist *True*.

So legen Sie die Eigenschaft fest:

1. Öffnen Sie die SQL Server Data Tools (SSDT).
1. Klicken Sie im Bereich **Verbindungs-Manager** mit der rechten Maustaste auf **Teradata-Verbindungs-Manager**, und wählen Sie anschließend **Eigenschaften** aus.
1. Wählen Sie im Bereich **Eigenschaften** für die Eigenschaft `UseUTF8CharSet` entweder *True* oder *False* aus.

## <a name="troubleshoot-the-teradata-connection-manager"></a>Problembehandlung beim Teradata-Verbindungs-Manager

Zum Protokollieren von Aufrufen des Teradata-Verbindungs-Managers an den Teradata-ODBC-Treiber (Open Database Connectivity) aktivieren Sie die Windows-ODBC-Ablaufverfolgung im Windows-ODBC-Datenquellen-Administrator.

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren Sie die [Teradata-Quelle](teradata-source.md).
- Konfigurieren Sie das [Teradata-Ziel](teradata-destination.md).
- Wenn Sie Fragen haben, besuchen Sie die [Tech Community](https://aka.ms/AA5u35j).
