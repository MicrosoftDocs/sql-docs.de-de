---
description: Herstellen einer Verbindung mit MySQL (MySqlToSql)
title: Herstellen einer Verbindung mit MySQL (mysqlto SQL) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 94099d01-ab19-4d58-a172-340c86b4a0f3
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 33017262982effedd9fa85ea26818fdb7b2ae8ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068953"
---
# <a name="connect-to-mysql-mysqltosql"></a>Herstellen einer Verbindung mit MySQL (MySqlToSql)
Verwenden Sie das Dialogfeld **mit MySQL verbinden** , um eine Verbindung mit der MySQL-Datenbank herzustellen, die Sie migrieren möchten.  
  
Um auf dieses Dialogfeld zuzugreifen, wählen Sie im Menü **Datei** die Option **mit MySQL verbinden** aus. Wenn Sie bereits eine Verbindung hergestellt haben, wird **mit dem Befehl erneut eine Verbindung mit MySQL hergestellt**.  
  
## <a name="options"></a>Tastatur  
**Anbieter**  
  
Verfügbarer MySQL-Anbieter ist der MySQL-ODBC 5,1-Treiber (vertrauenswürdig).  
  
**Mode**  
  
Der Standardmodus ist der Standardmodus. Im Standard Modus geben Sie Werte für MySQL, Servername, Serverport, Benutzername und Kennwort ein, oder wählen Sie Sie aus.  
  
**Servername**  
  
Geben Sie den Namen des MySQL-Servers ein. Dies ist eine Standard Modus-Option.  
  
**Serverport**  
  
Geben Sie den Serverport ein. Der Standardserverport ist 3306. Dies ist eine Standard Modus-Option.  
  
**Benutzername**  
  
Geben Sie den Benutzernamen ein, der von SSMA für die Verbindung mit der MySQL-Datenbank verwendet werden soll.  
  
**Kennwort**  
  
Geben Sie das Kennwort für den Benutzernamen ein.  
  
**SSL**  
  
Wenn Sie eine sichere Verbindung mit MySQL herstellen möchten, verwenden Sie SSL (Secure Socket Layer), indem Sie das Kontrollkästchen **SSL** aktivieren.  
  
**Konfigurieren**  
  
Es bietet eine Option zum Konfigurieren der Verbindung mit MySQL über Secure Socket Layer (SSL).  
  
> [!NOTE]  
> Zum Aktivieren von **configure** muss SSL auf **true** festgelegt werden.  
  
Wenn Sie auf die Schaltfläche "Konfigurieren" klicken, wird ein Dialogfeld angezeigt. Wenn Sie beim Herstellen einer Verbindung mit einer MySQL-Datenbank die Verschlüsselung verwenden möchten, müssen Sie den Pfad zu den drei folgenden im Dialogfeld vorhandenen Zertifikat Dateien definieren [Privacy Enhanced Mail Zertifikate (PEM)]:  
  
-   **SSL-Zertifizierungsstelle:** Gibt den Pfad zu einer Datei mit einer Liste mit vertrauenswürdigen SSL-Zertifizierungsstellen an.  
  
-   **SSL-Zertifikat:** Gibt den Namen der SSL-Zertifikatsdatei an, die zum Herstellen einer sicheren Verbindung verwendet werden soll.  
  
-   **SSL-Schlüssel:** Gibt den Namen der SSL-Schlüsseldatei an, die zum Herstellen einer sicheren Verbindung verwendet werden soll.  
  
> [!NOTE]  
> -   Die Schaltfläche **OK** ist aktiviert, wenn die erforderlichen Informationen angegeben wurden. Wenn einer der Dateipfade ungültig ist, bleibt die Schaltfläche "OK" deaktiviert.  
> -   Mit der Schaltfläche **Abbrechen** wird das Dialogfeld geschlossen, und die Option SSL **wird** aus dem Haupt Verbindungs Formular deaktiviert.  
  
