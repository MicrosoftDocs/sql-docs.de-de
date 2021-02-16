---
title: Gründe für Verbindungsfehler zwischen Verfügbarkeitsreplikaten
description: In diesem Artikel werden verschiedene mögliche Gründe für Verbindungsfehler zwischen den Replikaten einer Always On-Verfügbarkeitsgruppe beschrieben.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- troubleshooting [SQL Server], HADR
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], troubleshooting
ms.assetid: cd613898-82d9-482f-a255-0230a6c7d6fe
author: cawrites
ms.author: chadam
ms.openlocfilehash: 478724d2081e95deb10bd2f0e44a6cd330fe94ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350774"
---
# <a name="determine-possible-reason-for-connectivity-failures-between-availability-replicas"></a>Bestimmen möglicher Gründe für Verbindungsfehler zwischen Verfügbarkeitsreplikaten
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
Physische, Betriebssystem- oder [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Probleme können einen Fehler in einer Sitzung zwischen zwei Verfügbarkeitsreplikaten verursachen. Ein Verfügbarkeitsreplikat überprüft Komponenten, auf denen Sqlservr.exe beruht, nicht regelmäßig, um festzustellen, ob sie ordnungsgemäß ausgeführt werden oder nicht. Bei einigen Fehlertypen meldet die betroffene Komponente der Sqlservr.exe jedoch einen Fehler. Ein von einer anderen Komponente gemeldeter Fehler wird als *schwerwiegender Fehler* bezeichnet. Um andere Fehler zu erkennen, die andernfalls unbemerkt blieben, implementiert [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] einen eigenen Sitzungstimeoutmechanismus. Gibt den Zeitraum für das Sitzungstimeout in Sekunden an. Dieser Timeoutzeitraum ist die maximale Wartezeit einer Serverinstanz auf den Erhalt einer PING-Meldung von einer anderen Instanz, bevor sie annimmt, dass keine Verbindung zur anderen Instanz besteht. Wenn ein Sitzungstimeout zwischen zwei Verfügbarkeitsreplikaten auftritt, gehen die Verfügbarkeitsreplikate davon aus, dass ein Fehler aufgetreten ist, und deklarieren einen *Softwarefehler*.  
  
> [!IMPORTANT]  
>  Fehler in anderen Datenbanken als die primäre Datenbank werden nicht erkannt. Des Weiteren ist es unwahrscheinlich, dass ein Datenträgerfehler erkannt wird, es sei denn, die Datenbank wird aufgrund eines Datenträgerfehlers neu gestartet.  
  
 Die Geschwindigkeit der Fehlererkennung und somit die Reaktionszeit auf den Fehler hängen davon ab, ob es sich um einen Hardware- oder Softwarefehler handelt. Bestimmte Hardwarefehler, wie z. B. Netzwerkfehler, werden sofort gemeldet. In bestimmten Fällen kann jedoch die Meldung von Hardwarefehlern durch komponentenspezifische Timeoutzeitspannen verzögert werden. Bei Softwarefehlern bestimmt der Zeitraum für das Sitzungstimeout die Geschwindigkeit der Fehlererkennung. Standardmäßig sind hierfür 10 Sekunden festgelegt. Dies ist der empfohlene Mindestwert.  
  
## <a name="failures-due-to-hard-errors"></a>Fehler aufgrund von Hardwarefehlern  
 Die folgenden Bedingungen stellen u. a. mögliche Ursachen für Hardwarefehler dar:  
  
-   Eine unterbrochene Verbindung oder ein fehlerhaftes Kabel  
  
-   Eine fehlerhafte Netzwerkkarte  
  
-   Eine Änderung des Routers  
  
-   Änderungen an der Firewall  
  
-   Endpunktneukonfigurationen  
  
-   Ausfall des Laufwerks mit dem Transaktionsprotokoll  
  
-   Ein Betriebssystem- oder Prozessfehler  
  
 Wenn beispielsweise das Protokolllaufwerk in der primären Datenbank nicht mehr reagiert und ausfällt, informiert das Betriebssystem Sqlservr.exe, dass ein schwerwiegender Fehler aufgetreten ist.  
  
 Einige Komponenten, wie z. B. Netzwerkkomponenten und bestimmte E/A-Subsysteme, verfügen über eigene Timeouts zur Fehlerbestimmung. Diese Timeouts sind völlig unabhängig von [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], das keinerlei Informationen über deren Existenz oder Verhalten erhält. In diesen Fällen verlängert der Timeoutzeitraum die Verzögerung zwischen dem Auftreten eines Fehlers und dem Erhalt des daraus entstehenden Hardwarefehlers durch das Verfügbarkeitsreplikat.  
  
> [!NOTE]  
>  Die einzige Art der aktiven Fehlerüberprüfung von Verfügbarkeitsreplikaten findet bei Softwarefehlern statt. Weitere Informationen finden Sie nachfolgend in diesem Thema unter "Fehler aufgrund von Softwarefehlern".  
  
 Um die im Netzwerk auftretenden Fehlerbedingungen interpretieren zu können, erkundigen Sie sich bei einem Netzwerkingenieur, welche Fehlermeldungen an einen Port gesendet werden, wenn die folgenden Ereignisse bei einer TCP-Verbindung auftreten:  
  
-   DNS (Domain Name System) ist nicht funktionsfähig.  
  
-   Die Kabel sind nicht angeschlossen.  
  
-   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows verfügt über eine Firewall, die einen bestimmten Port blockiert.  
  
-   Die Anwendung, die einen Port überwacht, erzeugt einen Fehler.  
  
-   Ein Windows-Server wurde umbenannt.  
  
-   Ein Windows-Server wurde neu gestartet.  
  
> [!NOTE]  
>  [!INCLUDE[ssHADRc](../../../includes/sshadrc-md.md)] bietet keinen Schutz vor Problemen im Hinblick auf den Zugriff des Clients auf die Server. Nehmen wir beispielsweise an, eine öffentliche Netzwerkkarte verwaltet Clientverbindungen zum primären Replikat, während eine private Netzwerkschnittstellenkarte den Datenverkehr zwischen den Serverinstanzen verwaltet, die die Replikate einer Verfügbarkeitsgruppe hosten. Wenn in einem solchen Fall die öffentliche Netzwerkkarte ausfällt, können die Clients nicht mehr auf die Datenbanken zugreifen.  
  
## <a name="failures-due-to-soft-errors"></a>Fehler aufgrund von Softwarefehlern  
 Die folgenden Bedingungen stellen u. a. mögliche Ursachen für Sitzungstimeouts dar:  
  
-   Netzwerkfehler, wie z. B. Timeouts für TCP-Verbindungen, gelöschte oder beschädigte Pakete oder Pakete in falscher Reihenfolge.  
  
-   Ein Betriebssystem, ein Server oder eine Datenbank, die nicht reagieren.  
  
-   Ein Windows-Servertimeout.  
  
-   Unzureichende Verarbeitungsressourcen, wie z. B. Überlastung der CPU oder des Datenträgers, volles Transaktionsprotokoll oder unzureichender Arbeitsspeicher oder nicht genügend Threads. In diesen Fällen müssen Sie den Timeoutzeitraum erhöhen, die Arbeitsauslastung reduzieren oder die Hardware an die Arbeitsauslastung anpassen.  
  
### <a name="the-session-timeout-mechanism"></a>Der Sitzungstimeoutmechanismus  
 Da Softwarefehler von einer Serverinstanz nicht direkt erkannt werden können, kann ein Softwarefehler bewirken, dass ein Verfügbarkeitsreplikat in einer Sitzung unbegrenzt auf eine Antwort vom anderen Verfügbarkeitsreplikat wartet. Um dies zu verhindern, implementiert [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] einen Sitzungstimeoutmechanismus, der darauf basiert, dass die verbundenen Verfügbarkeitsreplikate in regelmäßigen Intervallen einen Pingbefehl über alle geöffneten Verbindungen senden. Durch den Empfang eines Pings innerhalb des Timeoutzeitraums wird angezeigt, dass die Verbindung weiterhin offen ist und dass die Serverinstanzen über diese Verbindung kommunizieren. Ein Replikat setzt nach dem Empfangen eines Pings den Timeoutzähler dieser Verbindung wieder zurück. Informationen zur Beziehung zwischen Verfügbarkeitsmodus und Sitzungstimeouts finden Sie unter [Verfügbarkeitsmodi &#40;Always On-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
 Die primären und sekundären Replikate signalisieren einander mithilfe von Pingbefehlen, dass sie noch aktiv sind, und eine Sitzungstimeoutgrenze verhindert, dass eines der Replikate unbegrenzt auf ein Ping vom anderen Replikat wartet. Die Sitzungstimeoutgrenze ist eine vom Benutzer konfigurierbare Replikateigenschaft mit einem Standardwert von 10 Sekunden. Durch den Empfang eines Pings innerhalb des Timeoutzeitraums wird angezeigt, dass die Verbindung weiterhin offen ist und dass die Serverinstanzen über diese Verbindung kommunizieren. Beim Empfang eines Pings setzt ein Verfügbarkeitsreplikat seinen Timeoutzähler für diese Verbindung zurück.  
  
 Wenn innerhalb des Zeitraums für das Sitzungstimeout kein Ping vom anderen Replikat empfangen wird, tritt für die Verbindung ein Timeout ein. Die Verbindung wird geschlossen, und der Status des Replikats mit dem Timeout ändert sich in DISCONNECTED. Auch wenn ein nicht verbundenes Replikat für den Modus für synchrone Commits konfiguriert ist, warten Transaktionen nicht darauf, dass dieses Replikat erneut verbunden und synchronisiert wird.  
  
## <a name="responding-to-an-error"></a>Reagieren auf Fehler  
 Ungeachtet des Fehlertyps reagiert eine Serverinstanz, die einen Fehler erkennt, abhängig von ihrer Rolle, dem Verfügbarkeitsmodus der Sitzung und dem Status der anderen Verbindungen in der Sitzung. Informationen darüber, was beim Verlust eines Partners geschieht, finden Sie unter [Verfügbarkeitsmodi &#40;Always On-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
## <a name="related-tasks"></a>Related Tasks  
 **So ändern Sie den Timeoutwert (nur Verfügbarkeitsmodus mit synchronem Commit)**  
  
-   [Ändern des Sitzungstimeouts für ein Verfügbarkeitsreplikat &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
 **So zeigen Sie den aktuellen Timeoutwert an**  
  
-   Abfrage **session_timeout** in [sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Übersicht zu AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
