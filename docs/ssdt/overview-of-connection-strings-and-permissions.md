---
title: Verbindungszeichenfolgen und Berechtigungen
description: Informationen zu den Verbindungszeichenfolgen, Konten und Berechtigungen, die Sie zum Ausführen von SQL Server-Komponententests benötigen Informationen zum Konfigurieren der Verbindungszeichenfolgen
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: ceff114e-a738-46ad-9785-b6647a2247f9
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 70387a9e18888a47522613b7a76a2483aec0d287
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080811"
---
# <a name="overview-of-connection-strings-and-permissions"></a>Übersicht über Verbindungszeichenfolgen und Berechtigungen

Zum Ausführen von SQL Server-Komponententests muss eine Verbindung mit einem Datenbankserver hergestellt werden, indem eine oder zwei spezifische Verbindungszeichenfolgen verwendet werden. Jede Verbindungszeichenfolge stellt ein Konto dar. Das Konto muss über spezifische Berechtigungen zum Ausführen einer oder mehrerer Aufgaben in einem bestimmten Skript im Rahmen des Tests verfügen. Sie können diese Zeichenfolgen im Dialogfeld **SQL Server-Testkonfiguration** angeben oder die Datei „app.config“ für das Testprojekt manuell bearbeiten.  
  
## <a name="connection-strings"></a>Verbindungszeichenfolgen  
Im Dialogfeld **SQL Server-Testkonfiguration** können Sie Verbindungszeichenfolgen für jedes der folgenden Konten angeben.  
  
> [!NOTE]  
> Der Ausführungskontext und der privilegierte Kontext unterscheiden sich nur bei Verwendung der SQL Server-Authentifizierung. Bei Verwendung der Windows-Authentifizierung werden für beide Verbindungszeichenfolgen dieselben Anmeldeinformationen verwendet.  
  
-   Ausführungskontext (erforderlich): Ein Benutzerkonto zum Ausführen des Testskripts. Diese Verbindungszeichenfolge sollte über dieselben Anmeldeinformationen verfügen, die auch für Ihre Benutzer vorgesehen sind. Auf diese Weise wird sichergestellt, dass die geeigneten Berechtigungen auf die Datenbank angewendet wurden. Weitere Informationen finden Sie unter [Vorgehensweise: Konfigurieren der Ausführung von SQL Server-Komponententests](../ssdt/how-to-configure-sql-server-unit-test-execution.md).  
  
    In der Datei app.config des Testprojekts entspricht dies dem `ExecutionContext`-Element.  
  
-   Privilegierter Kontext (optional): Ein Konto für dieselbe Datenbank, das über höhere Berechtigungen zum Ausführen der Vortest- und Nachtestaktion sowie des TestInitialize- und TestCleanup-Skripts verfügt. Durch diese Skripts wird der Datenbankzustand festgelegt. Außerdem können sie in der Nachtestaktion zum Überprüfen von Objekten in der Datenbank verwendet werden. Darüber hinaus wird diese Verbindungszeichenfolge zum Bereitstellen von Datenbankänderungen und Generieren von Daten verwendet.  
  
    In der Datei app.config des Testprojekts entspricht dies dem `PrivilegedContext`-Element. Wenn in den SQL Server-Komponententests nur das Testskript ausgeführt wird, muss kein privilegierter Kontext angegeben werden.  
  
Die im Dialogfeld Projektkonfiguration angegebenen Zeichenfolgen werden in der Datei app.config des Testprojekts gespeichert. Sie können diese Datei auch direkt bearbeiten und das Projekt neu erstellen. In diesem Fall werden die neuen Werte im Dialogfeld angezeigt.  
  
## <a name="windows-authentication-versus-sql-server-authentication"></a>Vergleich zwischen der Windows-Authentifizierung und der SQL Server-Authentifizierung  
Bei der Angabe von Verbindungszeichenfolgen müssen Sie festlegen, ob die Windows- oder SQL-Authentifizierung verwendet werden soll. Ein Grund für die Wahl der Windows-Authentifizierung liegt darin, dass sie die Verwendung von Tests im Team besser unterstützt als die SQL Server-Authentifizierung. Bei Verwendung der SQL Server-Authentifizierung werden die Verbindungszeichenfolgen basierend auf den Benutzeranmeldeinformationen mithilfe der Datenschutz-API (Data Protection API, DPAPI) verschlüsselt. Dies bedeutet, dass Tests in diesem Testprojekt nur für Sie, nicht aber für Teammitglieder ausgeführt werden, die Tests nach dem Einchecken über das Quellcodeverwaltungssystem erhalten. Zum Ausführen von Test in diesem Testprojekt müssen andere Mitglieder Ihres Teams das Testprojekt umkonfigurieren und dafür ihre eigenen Anmeldeinformationen verwenden. Dazu könnten sie ihr Exemplar der Datei app.config bearbeiten oder das Dialogfeld Projektkonfiguration verwenden.  
  
## <a name="permissions"></a>Berechtigungen  
Das Testskript wird mit der Berechtigungsstufe "Ausführungskontext" ausgeführt, die der Berechtigungsstufe für Benutzerbefehle bei einer normalen Datenbanknutzung entspricht. Die Vortest- und Nachtestaktion sowie das TestInitialize- und TestCleanup-Skript werden mit der Berechtigungsstufe "Privilegierter Kontext" ausgeführt.  
  
Da für die Verbindung, die für das Skript der Nachtestaktion verwendet wird, höhere Berechtigungen gelten, können Sie in diesem Rahmen eine Überprüfung ausführen. Außerdem können Sie in diesem Skript Skriptbefehle zum Testen von Berechtigungen ausführen. Weitere Informationen zu Berechtigungen finden Sie im Artikel [Erforderliche Berechtigungen für SQL Server Data Tools](../ssdt/required-permissions-for-sql-server-data-tools.md) im Abschnitt zu SQL Server-Komponententests.  
  
## <a name="see-also"></a>Weitere Informationen  
[Erstellen und Definieren von SQL Server-Komponententests](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Skripts in SQL Server-Komponententests](../ssdt/scripts-in-sql-server-unit-tests.md)  
[SQL Server-Komponententestdateien](../ssdt/sql-server-unit-test-files.md)  
  
