---
title: SQL Server-Eigenschaften (Registerkarte "Startparameter")
description: Verwenden Sie die Registerkarte „Startparameter“ des Dialogfelds „SQL Server Properties“ (SQL Server-Eigenschaften), um Startparameter hinzuzufügen oder zu entfernen, die sich auf die Leistung der Datenbank-Engine auswirken.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 16942624-5374-446c-8de4-ee6ed34d6e94
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a80683b2e2092cf6a0eff2519edbcd893c2357ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349592"
---
# <a name="sql-server-properties-startup-parameters-tab"></a>SQL Server-Eigenschaften (Registerkarte "Startparameter")
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Verwenden Sie dieses Dialogfeld, um Startparameter für [!INCLUDE[ssDE](../../includes/ssde-md.md)]hinzuzufügen oder zu entfernen. Startparameter können große Auswirkungen auf die Leistung von [!INCLUDE[ssDE](../../includes/ssde-md.md)] haben. Lesen Sie vor dem Hinzufügen oder Ändern von Startparametern das Thema "Verwenden der Startoptionen für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienst" in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Onlinedokumentation.  
  
## <a name="options"></a>Tastatur  
 **Startparameter angeben**  
 Um einen Parameter hinzuzufügen, geben Sie den Parameter ein, und klicken Sie anschließend auf **Hinzufügen**.  
  
 Um einen der erforderlichen Parameter zu ändern, wählen Sie den Parameter im Feld **Vorhandene Parameter** aus, ändern Sie die Werte im Feld **Startparameter angeben** , und klicken Sie dann auf **Aktualisieren**.  
  
 **Vorhandene Parameter**  
 Um einen Parameter zu entfernen, wählen Sie den Parameter aus, und klicken Sie anschließend auf **Entfernen**.  
  
## <a name="parameter-format"></a>Parameterformat  
 Geben Sie kein Trennzeichen zwischen Parametern ein. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Manager fügt das Trennzeichen automatisch hinzu. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurations-Manager erzwingt die folgenden Parameteranforderungen.  
  
-   Führende und nachfolgende Leerzeichen jedes Startparameters werden abgeschnitten.  
  
-   Alle Startparameter beginnen mit „–“ (Bindestrich), und der zweite Wert ist ein Buchstabe.  
  
## <a name="required-parameters"></a>Erforderliche Parameter  
 Die folgenden Parameter sind erforderlich. Sie können geändert, jedoch nicht entfernt werden.  
  
-   „-d“ ist der Pfad der Datei **master.mdf** (die Datendatei der Masterdatenbank).  
  
-   „-l“ ist der Pfad der Datei **master.ldf** (die Protokolldatei der Masterdatenbank).  
  
-   „-e“ ist der Pfad der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Fehlerprotokolldateien.  
  
> [!CAUTION]  
>  Wenn die Dateipfadparameter falsch sind, wird [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] möglicherweise nicht gestartet.  
  
 Weitere Informationen zum Verschieben der master-Datenbank finden Sie im Thema "Verschieben von Systemdatenbanken" in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Onlinedokumentation.  
  
## <a name="optional-parameters"></a>Optionale Parameter  
 Alle unterstützten Startparameter sind im Thema "Verwenden der Startoptionen für den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienst" in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Onlinedokumentation beschrieben. Der Startparameter -T *trace#* gibt an, dass beim Starten einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ein angegebenes Ablaufverfolgungsflag (*trace#* ) aktiviert sein muss. Ablaufverfolgungsflags werden verwendet, um den Server mit nicht standardmäßigem Verhalten zu starten. Weitere Informationen zu Ablaufverfolgungsflags finden Sie im Thema „Ablaufverfolgungsflags ([!INCLUDE[tsql](../../includes/tsql-md.md)])“ in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Onlinedokumentation.  
  
> [!CAUTION]  
>  Möglicherweise treffen Sie auf zusätzliche nicht dokumentierte Startparameter und Ablaufverfolgungsflags, die im Internet beschrieben werden. Nicht dokumentierte Startparameter und Ablaufverfolgungsflags werden erstellt, um ungewöhnliche Probleme zu behandeln oder bestimmte für Tests erforderliche Bedingungen zu erzwingen. Die Verwendung von nicht dokumentierten Startparametern kann zu unerwarteten Ergebnissen führen. Verwenden Sie nicht dokumentierte Parameter nur, wenn Sie von Microsoft Support Services dazu aufgefordert werden.  
  
 In der folgenden Liste werden einige allgemeine optionale Parameter beschrieben.  
  
|Parameter|Kurze Beschreibung|  
|---------------|-----------------------|  
|-M|Startet eine Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] im Einzelbenutzermodus.|  
|-T1204|Gibt die an einem Deadlock beteiligten Ressourcen und Sperrentypen sowie den aktuell betroffenen Befehl zurück.|  
|-T1224|Deaktiviert die Sperrenausweitung basierend auf der Anzahl von Sperren.|  
|-T3608|Verhindert, dass [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] automatisch gestartet wird und andere Datenbanken als die master-Datenbank wiederhergestellt werden|  
|-T7806|Aktiviert eine dedizierte Administratorverbindung (Dedicated Administrator Connection, DAC) in [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)].|  
  
> [!CAUTION]  
>  Einige optionale Parameter können Serververhalten ändern und die Leistung beeinträchtigen.  
  
## <a name="permissions"></a>Berechtigungen  
 Die Verwendung dieser Seite ist auf Benutzer beschränkt, die die entsprechenden Einträge in der Registrierung ändern können. Dazu gehören folgende Benutzer.  
  
-   Mitglieder der lokalen Administratorgruppe.  
  
-   Das Domänenkonto, das von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]verwendet wird, wenn [!INCLUDE[ssDE](../../includes/ssde-md.md)] für die Ausführung unter einem Domänenkonto konfiguriert ist.  
  
## <a name="books-online-references"></a>Referenzen in der Onlinedokumentation  
 Weitere Informationen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Startparametern finden Sie in „Vorgehensweise: Konfigurieren von Serverstartoptionen ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Konfigurations-Manager)“ in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Onlinedokumentation.  
  
  
