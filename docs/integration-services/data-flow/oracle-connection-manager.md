---
description: Oracle-Verbindungs-Manager
title: Oracle-Verbindungs-Manager | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 258cce05d25b02b046c9fc4e79bd031d6b27fc22
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343257"
---
# <a name="oracle-connection-manager"></a>Oracle-Verbindungs-Manager

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Ein Oracle-Verbindungs-Manager ermöglicht einem Paket, Daten aus Oracle-Datenbanken zu extrahieren und in Oracle-Datenbanken zu laden.

Die **ConnectionManagerType**-Eigenschaft des Oracle-Verbindungs-Managers ist auf **ORACLE** festgelegt.

## <a name="configuring-the-oracle-connection-manager"></a>Konfigurieren des Oracle-Verbindungs-Managers

Änderungen der Konfiguration des Oracle-Verbindungs-Managers werden von Integration Services zur Runtime aufgelöst. Im Dialogfeld **Oracle-Verbindungs-Manager-Editor** können Sie einer Oracle-Datenquelle eine Verbindung hinzufügen.

![Verbindungs-Manager](media/oracle-connection-manager.png)

### <a name="options"></a>Optionen

#### <a name="connection-manager-information"></a>Informationen zum Verbindungs-Manager

Geben Sie Informationen zur Oracle-Verbindung ein.

**Name**

Geben Sie einen Namen für die Oracle-Verbindung ein. Der Standardname ist „Oracle-Verbindungs-Manager“. 

**Beschreibung** 

Geben Sie eine Beschreibung der Verbindung ein. Diese Eingabe ist optional.

**TNS-Dienstname**

Geben Sie den Namen der Oracle-Datenbank ein, mit der Sie arbeiten. Der TNS-Dienstname könnte wie folgt lauten:

- Der Verbindungsdeskriptorname, der in der Datei „tnsnames.ora“ definiert ist, die sich im Ordner „admin“ des Oracle-Clients befindet.

- EzConnect-Format: [//]Host[:Port][/Dienstname]

Weitere Informationen finden Sie in der Oracle-Dokumentation.

#### <a name="connection-manager-logging"></a>Verbindungs-Manager-Protokollierung

Wählen Sie eine der folgenden Optionen aus:

- **Windows-Authentifizierung verwenden**: Wählen Sie diese Option aus, wenn Windows-Authentifizierung verwendet werden soll.

- **Oracle-Authentifizierung verwenden**: Wählen Sie diese Option aus, um die Oracle-Datenbank-Authentifizierung zu verwenden. Wenn Sie diese Authentifizierung verwenden, geben Sie Ihre Oracle-Anmeldeinformationen wie folgt ein:  
    **Benutzername**: Geben Sie den Benutzernamen zum Herstellen der Verbindung mit der Oracle-Datenbank ein.  
    **Kennwort**: Geben Sie das Oracle-Datenbank-Kennwort für den Benutzer ein, der im Benutzernamenfeld eingegeben wird.

> [!NOTE]
>
>Die Windows-Authentifizierung wird für Oracle Server 18c nicht unterstützt.

**Verbindung testen**

Klicken Sie auf **Verbindung testen**, um zu überprüfen, ob die angegebenen Informationen korrekt sind. Sie erhalten die Meldung **Verbindungstest erfolgreich**, wenn Sie mit den eingegebenen Informationen eine Verbindung mit der Oracle-Datenbank herstellen können.

### <a name="custom-properties"></a>Benutzerdefinierte Eigenschaften

Im Oracle-Verbindungs-Manager können Sie die folgenden benutzerdefinierten Verbindungs-Manager-Eigenschaften festlegen:

- **EnableDetailedTracing**: Wird nicht verwendet.

- **OracleHome**: Geben Sie den vom Connector zu verwendenden 32-Bit-Oracle Home-Namen oder -Ordner an. (Optional)

- **OracleHome64**: Geben Sie den vom Connector im 64-Bit-Modus zu verwendenden 64-Bit-Oracle Home-Namen oder -Ordner an. (Optional)

Benutzerdefinierte Eigenschaften werden im Oracle-Verbindungs-Manager-Editor nicht aufgeführt. So legen Sie die Eigenschaften **OracleHome** und **OracleHome64** fest:

1. Klicken Sie im Bereich „Verbindungs-Manager“ mit der rechten Maustaste auf den Oracle-Verbindungs-Manager, mit dem Sie arbeiten, und wählen Sie **Eigenschaften** aus.

2. Legen Sie im Bereich **Eigenschaften** die **OracleHome**- oder **OracleHome64**-Eigenschaft mit dem vollständigen Pfad zum Oracle-Basisverzeichnis fest.

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren Sie die [Oracle-Quelle](oracle-source.md).
- Konfigurieren Sie das [Oracle-Ziel](oracle-destination.md).
- Wenn Sie Fragen haben, besuchen Sie die [TechCommunity](https://aka.ms/AA5u35j).
