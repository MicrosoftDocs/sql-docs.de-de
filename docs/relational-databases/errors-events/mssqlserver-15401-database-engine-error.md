---
description: MSSQLSERVER_15401
title: MSSQLSERVER_15401
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 15401 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: ab9739d17fbe36bb8a703dd06ee85a32e93e7662
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349035"
---
# <a name="mssqlserver_15401"></a>MSSQLSERVER_15401
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|15401|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|SEC_INVALIDLOGINORGROUP|
|Meldungstext|Der Windows NT-Benutzer oder die -Gruppe '%s' wurde nicht gefunden. Überprüfen Sie den Namen.|
||

## <a name="explanation"></a>Erklärung

Dieser Fehler tritt auf, wenn es [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nicht möglich ist, eine Anmeldung basierend auf einem Windows-Prinzipal (z. B. einem Domänenbenutzer oder einer Windows-Domänengruppe) zu erstellen. Dem Benutzer wird eine Fehlermeldung angezeigt, die der folgenden ähnelt:

> Fehler 15401: Der Windows NT-Benutzer oder die -Gruppe '%s' wurde nicht gefunden. Überprüfen Sie den Namen.

## <a name="cause"></a>Ursache

Der Fehler kann aus einem der folgenden Gründe auftreten:

- Die Anmeldung ist in Active Directory nicht vorhanden.
- Der Domänencontroller ist nicht verfügbar.
- Sie verwenden beim Hinzufügen eines lokalen Kontos nicht BUILTIN als Domänenname.
- Probleme mit der Namensauflösung.

## <a name="user-action"></a>Benutzeraktion

In den folgenden Abschnitten finden Sie Maßnahmen, die Sie für jede der oben genannten Ursachen ergreifen können.

### <a name="verify-the-login-you-are-trying-to-add"></a>Überprüfen Sie die Anmeldung, die Sie hinzufügen möchten.

1. Vergewissern Sie sich, dass die Windows-Anmeldung in der Domäne noch vorhanden ist. Möglicherweise hat Ihr Netzwerkadministrator die Windows-Anmeldung aus bestimmten Gründen entfernt, und Sie können dieser Anmeldung keinen Zugriff auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gewähren.
1. Vergewissern Sie sich, dass Sie den Domänen- und Anmeldenamen richtig geschrieben haben und das folgende Format verwenden: `Domain\User`
1. Wenn die Anmeldung vorhanden und korrekt ist, Sie den Fehler aber trotzdem erhalten, fahren Sie mit den folgenden Abschnitten in diesem Artikel fort.

### <a name="verify-if-the-domain-controller-is-available"></a>Überprüfen Sie, ob der Domänencontroller verfügbar ist.

Sie erhalten möglicherweise den Fehler 15401, wenn der Domänencontroller für die Domäne, in der sich die Anmeldung befindet (dieselbe oder eine andere Domäne), aus irgendeinem Grund nicht verfügbar ist.

Wenn sich die Anmeldung in einer anderen Domäne als [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] befindet, prüfen Sie, ob die richtigen Vertrauensstellungen zwischen den Domänen bestehen.

Überprüfen Sie, ob der Domänencontroller der Anmeldung erreichbar ist, indem Sie den Ping-Befehl von dem Computer aus verwenden, auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird. Überprüfen Sie die IP-Adresse und den Namen des Domänencontrollers.

### <a name="verify-the-domain-name-for-local-accounts"></a>Domänennamen für lokale Konten überprüfen

Lokale (Nicht-Domänen-) Konten erfordern eine besondere Behandlung. Wenn Sie versuchen, ein lokales Konto vom lokalen Computer aus, auf dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird, hinzuzufügen, stellen Sie sicher, dass Sie BUILTIN als Domänennamen verwenden.

### <a name="check-for-name-resolution-issues"></a>Auf Probleme mit der Namensauflösung überprüfen

Wenn Sie der Name eines am Hinzufügen der Anmeldung oder Gruppe beteiligten Computers nicht aufgelöst werden kann, erhalten Sie möglicherweise den Fehler 15401.

Vergewissern Sie sich, dass der Namensauflösungsmechanismus (z. B. WINS, DNS, HOSTS oder LMHOSTS) ordnungsgemäß konfiguriert ist.

## <a name="see-also"></a>Weitere Informationen

- [Testen eines Kanals zwischen dem lokalen Computer und seiner Domäne](/powershell/module/microsoft.powershell.management/test-computersecurechannel#example-1--test-a-channel-between-the-local-computer-and-its-domain)
- [LogonSessions v1.4](/sysinternals/downloads/logonsessions)
- [sp_change_users_login (Transact-SQL)](../system-stored-procedures/sp-change-users-login-transact-sql.md)