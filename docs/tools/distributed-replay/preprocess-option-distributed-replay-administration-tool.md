---
title: preprocess-Option
titleSuffix: SQL Server Distributed Replay
description: Das Distributed Replay-Tool („DReplay.exe“) von Microsoft SQL Server ist ein Befehlszeilentool, über das Sie mit dem Distributed Replay-Controller kommunizieren können.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 34c4b1432fa9373e4952bb6b11264f5cec6e452a
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836856"
---
# <a name="preprocess-option-distributed-replay-administration-tool"></a>Vorverarbeitungsoption (Verwaltungstool "Distributed Replay")
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Das [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Verwaltungstool Distributed Replay, **DReplay.exe**, ist ein Befehlszeilentool, das Sie für die Kommunikation mit dem Distributed Replay-Controller verwenden können. In diesem Thema werden die Befehlszeilenoption **preprocess** und die entsprechende Syntax beschrieben.  
  
 Die **preprocess** -Option initiiert die Vorverarbeitungsphase. In dieser Phase bereitet der Controller die Eingabedaten der Ablaufverfolgung für die Wiedergabe anhand des Zielservers vor.  
  
 ![Artikellinksymbol](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") Weitere Informationen zu den Syntaxkonventionen für das Verwaltungstool finden Sie unter [Transact-SQL-Syntaxkonventionen &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Syntax  
  
```  
  
dreplay preprocess [-m controller] -i input_trace_file  
    -d controller_working_dir [-c config_file] [-f status_interval]  
```  
  
#### <a name="parameters"></a>Parameter  
 **-m** _Controller_  
 Gibt den Computernamen des Controllers an. Sie können mit "`localhost`" oder "`.`" auf den lokalen Computer verweisen.  
  
 Wenn der **-m** -Parameter nicht angegeben ist, wird der lokale Computer verwendet.  
  
 **-i** _Eingabedatei_der_Ablaufverfolgung_  
 Gibt den vollständigen Pfad der Eingabedatei der Ablaufverfolgung auf dem Controller an, z. B. `D:\Mytrace.trc`. Der **-i** -Parameter ist erforderlich.  
  
 Wenn Rolloverdateien im gleichen Verzeichnis vorhanden sind, werden sie geladen und automatisch verwendet. Die Dateien müssen der Dateirollover-Benennungskonvention folgen, z.B.: `Mytrace.trc`, `Mytrace_1.trc`, `Mytrace_2.trc`, `Mytrace_3.trc`, ... `Mytrace_n.trc`.  
  
> [!NOTE]  
>  Wenn Sie das Verwaltungstool auf einem anderen Computer als dem Controller verwenden, müssen Sie die Eingabedateien der Ablaufverfolgung auf den Controller kopieren, damit ein lokaler Pfad für diesen Parameter verwendet werden kann.  
  
 **-d** _controller_working_dir_  
 Gibt das Verzeichnis auf dem Controller an, in dem die Zwischendatei gespeichert wird. Der **-d** -Parameter ist erforderlich.  
  
 Es gelten die folgenden Anforderungen:  
  
-   Das Verzeichnis muss sich auf dem Controller befinden.  
  
-   Sie müssen den vollständigen Pfad angeben, der mit einem Laufwerkbuchstaben beginnen muss (z. B. `c:\WorkingDir`).  
  
-   Der Pfad darf nicht mit einem umgekehrten Schrägstrich ("`\`") enden.  
  
-   UNC-Pfade werden nicht unterstützt.  
  
 **-c** _config_file_  
 Der vollständige Pfad der Vorverarbeitungskonfigurationsdatei. Mit ihm wird der Speicherort der Vorverarbeitungskonfigurationsdatei angegeben, wenn sie an einem anderen Speicherort gespeichert wird. Dieser Parameter kann ein UNC-Pfad sein oder ein lokales Verzeichnis auf dem Computer angeben, auf dem Sie das Verwaltungstool ausführen.  
  
 Der **-c** -Parameter ist nicht erforderlich, wenn keine Filterung benötigt wird oder wenn Sie die maximale Leerlaufzeit nicht ändern möchten.  
  
 Ohne den **-c** -Parameter wird die Standardkonfigurationsdatei für die Vorverarbeitung verwendet: `DReplay.exe.preprocess.config`.  
  
 **-f** _status_interval_  
 Gibt die Häufigkeit (in Sekunden) für die Anzeige von Statusmeldungen an.  
  
 Wenn **-f** nicht angegeben wird, ist das Standardintervall 30 Sekunden.  
  
## <a name="examples"></a>Beispiele  
 In diesem Beispiel wird die Vorverarbeitungsphase mit allen Standardeinstellungen initiiert. Der Wert `localhost` gibt an, dass der Controllerdienst auf demselben Computer wie das Verwaltungstool ausgeführt wird. Der *input_trace_file* -Parameter gibt den Speicherort der Eingabedaten der Ablaufverfolgung an: `c:\mytrace.trc`. Da keine Filterung der Ablaufverfolgungsdatei erfolgt, muss der **-c** -Parameter angegeben werden.  
  
```  
dreplay preprocess -m localhost -i c:\mytrace.trc -d c:\WorkingDir  
```  
  
 In diesem Beispiel wird die Vorverarbeitungsphase initiiert, und es wird eine geänderte Vorverarbeitungskonfigurationsdatei angegeben. Im Gegensatz zum vorherigen Beispiel wird der **-c** -Parameter verwendet, um auf die geänderte Konfigurationsdatei zu zeigen, wenn Sie diese an einem anderen Speicherort gespeichert haben. Beispiel:  
  
```  
dreplay preprocess -m localhost -i c:\mytrace.trc -d c:\WorkingDir -c c:\DReplay.exe.preprocess.config  
```  
  
 Der geänderten Vorverarbeitungskonfigurationsdatei wird eine Filterbedingung hinzugefügt, die während der verteilten Wiedergabe Systemsitzungen herausfiltert. Der Filter wird durch Ändern des `<PreprocessModifiers>` -Elements in der Vorverarbeitungskonfigurationsdatei `DReplay.exe.preprocess.config`hinzugefügt.  
  
 Im folgenden Beispiel wird die geänderte Konfigurationsdatei gezeigt:  
  
```  
<?xml version='1.0'?>  
<Options>  
    <PreprocessModifiers>  
        <IncSystemSession>No</IncSystemSession>  
        <MaxIdleTime>-1</MaxIdleTime>  
    </PreprocessModifiers>  
</Options>  
```  
  
## <a name="permissions"></a>Berechtigungen  
 Sie müssen das Verwaltungstool als interaktiver Benutzer mit einem lokalen Benutzerkonto oder Domänenbenutzerkonto ausführen. Um ein lokales Benutzerkonto zu verwenden, müssen das Verwaltungstool und der Controller auf demselben Computer ausgeführt werden.  
  
 Weitere Informationen finden Sie unter [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Vorbereiten der Eingabedaten für die Ablaufverfolgung](../../tools/distributed-replay/prepare-the-input-trace-data.md)   
 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)   
 [Konfigurieren von Distributed Replay](../../tools/distributed-replay/configure-distributed-replay.md)  
  
  
