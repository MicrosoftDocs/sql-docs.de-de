---
title: Konfigurieren der Nutzungs- und Diagnosedatensammlung für SQL Server-Tools (CEIP) | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, welche Informationen CEIP von Benutzern sammelt, um Produkte zu verbessern. Außerdem erfahren Sie, wie Sie sich für das Programm in SQL Server Data Tools (SSDT) registrieren bzw. davon abmelden.
ms.custom: ''
ms.date: 10/21/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: baf3a205-a6bb-4564-8b64-3a0475bb9273
author: stevestein
ms.author: sstein
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016'
ms.openlocfilehash: 4c0dd7d5371d298a07439f193763b7b394b160bc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353558"
---
# <a name="configure-usage-and-diagnostic-data-collection-for-sql-server-tools-ceip"></a>Konfigurieren der Nutzungs- und Diagnosedatensammlung für SQL Server-Tools (CEIP)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Erfahren Sie, wie das Programm zur Verbesserung der Benutzerfreundlichkeit (Customer Experience Improvement Program; CEIP) uns dabei hilft, unsere Software zu verbessern.  Sie können Tools konfigurieren, um Ihr Einverständnis jederzeit zu widerrufen bzw. neu zu erteilen.  
  
> [!NOTE]  
> Eine Erklärung der Datensammlungs- und Datennutzungsverfahren der Microsoft SQL Server-Releases finden Sie in diesen [Datenschutzbestimmungen](./sql-server-privacy.md).  
  
## <a name="opting-in-and-out-of-ceip-for-sql-server-data-tools"></a>Abonnieren und Kündigen des CEIP für SQL Server Data Tools  

 Das Programm zur Verbesserung der Benutzerfreundlichkeit soll Microsoft dabei helfen, seine Produkte im Lauf der Zeit zu verbessern. Dieses Programm sammelt Informationen zur Computerhardware und dazu, wie Benutzer unser Produkt verwenden, ohne die Benutzer bei ihren Aufgaben am Computer zu unterbrechen. Die gesammelten Informationen helfen Microsoft dabei, zu identifizieren, welche Funktionen verbessert werden müssen. In diesem Dokument wird beschrieben, wie Sie Ihr Einverständnis im CEIP für SQL Server Data Tools (SSDT) in Visual Studio 2017, Visual Studio 2015 und Visual Studio 2013 widerrufen bzw. neu erteilen können.  Informationen zum Deaktivieren von CEIP für SQL Server finden Sie unter [Aktivieren oder Deaktivieren der lokalen Überwachung](usage-and-diagnostic-data-in-local-audit.md#turning-local-audit-on-or-off).

### <a name="choice-and-control-over--ceip-and-sql-server-data-tools-for-visual-studio-2017"></a>Auswahl und Kontrolle über das CEIP und SQL Server Data Tools in Visual Studio 2017

 SSDT für Visual Studio 2017 ist das Datenmodellierungstool, das mit SQL Server 2017 geliefert wird. Es verwendet die CEIP-Optionen, die in Visual Studio 2017 integriert sind. Weitere Informationen zum Übermitteln von Feedback über das CEIP in Visual Studio 2017 finden Sie in diesem [Hilfedokument von Visual Studio](https://www.visualstudio.com/docs/work/connect/give-feedback).  
  
 In Vorschauversionen von SQL Server 2017 ist das CEIP standardmäßig aktiviert. Sie können es mithilfe der folgenden Anweisungen deaktivieren oder wieder aktivieren.  
  
 **In Visual Studio (gilt für vollständige Sprachinstallationen von Visual Studio 2017)**  
  
 Wenn Sie das SSDT-Setup auf einem Computer ausführen, auf dem Visual Studio bereits installiert ist, werden nur die SQL Server- und Business Intelligence-Projektvorlagen hinzugefügt. In diesem Szenario können von Visual Studio bereitgestellte Kunden-Feedbackoptionen verwendet werden, um das Einverständnis im CEIP zu widerrufen bzw. neu zu erteilen.  
  
1.  Starten Sie Visual Studio.  
  
2.  Wählen Sie im Hilfemenü **Feedback senden** > **Einstellungen** aus.  
  
3.  Um CEIP zu deaktivieren, klicken Sie auf **Nein, ich möchte nicht teilnehmen**, und klicken Sie dann auf **OK**.  
  
     Um CEIP zu aktivieren, klicken Sie auf **Ja, ich möchte teilnehmen**, und klicken Sie dann auf **OK**.  
  

  
 **Verwenden einer registrierungsbasierten Richtlinie oder Gruppenrichtlinie**  
  
 Wenn Sie das SSDT-Setup auf einem Computer ausführen, auf dem Visual Studio 2017 nicht installiert ist, wird nur die Visual Studio-Shell installiert. Die Shell stellt keine Kunden-Feedbackoptionen bereit. In diesem Fall stellt ein Registrierungsupdate die einzige Option zum Konfigurieren des CEIP dar.  
  
 Unternehmenskunden können durch Festlegen einer registrierungsbasierten Richtlinie für SQL Server 2017 eine Gruppenrichtlinie erstellen, um das Einverständnis zu widerrufen bzw. neu zu erteilen.  
  
 Der entsprechende Registrierungsschlüssel und die entsprechenden Einstellungen lauten wie folgt:  
  
- 64-Bit-Betriebssystem, Key = HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\VSCommon\15.0\SQM
- 32-Bit-Betriebssystem, Key = HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\VSCommon\15.0\SQM

Wenn eine Gruppenrichtlinie aktiviert wird: Key = HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\VisualStudio\SQM 

Eintrag = OptIn

Wert = (DWORD)
- 0 ist deaktiviert (VSCEIP deaktivieren)
- 1 ist aktivieren (VSCEIP aktivieren)

  
> [!CAUTION]  
>  Durch eine fehlerhafte Bearbeitung der Registrierung können schwerwiegende Schäden am System verursacht werden. Bevor Sie Änderungen an der Registrierung vornehmen, sollten Sie alle wichtigen Computerdaten sichern. Sie können auch die Startoption Letzte als funktionierend bekannte Konfiguration verwenden, wenn nach dem Übernehmen manueller Änderungen Probleme auftreten.  
  
 Weitere Informationen zu den vom CEIP gesammelten, verarbeiteten oder übertragenen Informationen finden Sie in den [Datenschutzbestimmungen](./sql-server-privacy.md).  
 
### <a name="choice-and-control-over-ceip-and-sql-server-data-tools-for-visual-studio-2015"></a>Auswahl und Kontrolle über das CEIP und SQL Server Data Tools für Visual Studio 2015  
 SSDT für Visual Studio 2015 ist das Datenmodellierungstool, das mit SQL Server 2016 geliefert wird. Es verwendet die CEIP-Optionen, die in Visual Studio 2015 integriert sind. Weitere Informationen zum Übermitteln von Feedback über das CEIP in Visual Studio 2015 finden Sie in diesem [Hilfedokument von Visual Studio](/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017).  
  
 In Vorschauversionen von SQL Server 2016 ist das CEIP standardmäßig aktiviert. Sie können es mithilfe der folgenden Anweisungen deaktivieren oder wieder aktivieren.  
  
 **In Visual Studio (gilt für vollständige Sprachinstallationen von Visual Studio 2015)**  
  
 Wenn Sie das SSDT-Setup auf einem Computer ausführen, auf dem Visual Studio bereits installiert ist, werden nur die SQL Server- und Business Intelligence-Projektvorlagen hinzugefügt. In diesem Szenario können von Visual Studio bereitgestellte Kunden-Feedbackoptionen verwendet werden, um das Einverständnis im CEIP zu widerrufen bzw. neu zu erteilen.  
  
1.  Starten Sie Visual Studio.  
  
2.  Wählen Sie im Hilfemenü **Feedback senden** > **Einstellungen** aus.  
  
3.  Um CEIP zu deaktivieren, klicken Sie auf **Nein, ich möchte nicht teilnehmen**, und klicken Sie dann auf **OK**.  
  
     Um CEIP zu aktivieren, klicken Sie auf **Ja, ich möchte teilnehmen**, und klicken Sie dann auf **OK**.  
  

  
 **Verwenden einer registrierungsbasierten Richtlinie oder Gruppenrichtlinie**  
  
 Wenn Sie das SSDT-Setup auf einem Computer ausführen, auf dem Visual Studio 2015 nicht installiert ist, wird nur die Visual Studio-Shell installiert. Die Shell stellt keine Kunden-Feedbackoptionen bereit. In diesem Fall stellt ein Registrierungsupdate die einzige Option zum Konfigurieren des CEIP dar.  
  
 Unternehmenskunden können durch Festlegen einer registrierungsbasierten Richtlinie für SQL Server 2016 eine Gruppenrichtlinie erstellen, um das Einverständnis zu widerrufen bzw. neu zu erteilen.  
  
 Der entsprechende Registrierungsschlüssel und die entsprechenden Einstellungen lauten wie folgt:  
  
 Key = HKEY_CURRENT_USER\Software\Microsoft\VSCommon\14.0\SQM  
  
 RegEntry name = OptIn  
  
 DWORD-Eintrag:  
  
-   0 bedeutet keine Teilnahme  
  
-   1 bedeutet einschalten  
  
> [!CAUTION]  
>  Durch eine fehlerhafte Bearbeitung der Registrierung können schwerwiegende Schäden am System verursacht werden. Bevor Sie Änderungen an der Registrierung vornehmen, sollten Sie alle wichtigen Computerdaten sichern. Sie können auch die Startoption Letzte als funktionierend bekannte Konfiguration verwenden, wenn nach dem Übernehmen manueller Änderungen Probleme auftreten.  
  
 Weitere Informationen zu den vom CEIP gesammelten, verarbeiteten oder übertragenen Informationen finden Sie in den [Datenschutzbestimmungen](./sql-server-privacy.md).  
  
### <a name="choice-and-control-for-ceip-and-sql-server-data-tools---bi-ssdt-bi"></a>Auswahl und Steuerung für das CEIP und SQL Server Data Tools – BI (SSDT-BI)  
 Falls Sie SSDT-BI verwenden, erhalten Sie während der Installation die Gelegenheit zur Teilnahme am CEIP. Später können Sie die CEIP-Konfiguration für SSDT-BI durch Clienttools oder durch Ändern der Registrierungseinstellungen ändern.  
  
 **In SSDT und SSDT-BI für Visual Studio 2013**  
  
1.  Starten Sie das Tool, und öffnen Sie ein neues oder vorhandenes Projekt für Analysis Services oder Integration Services.  
  
2.  Wählen Sie im Menü „Hilfe“ **Kundenfeedbackoptionen für Microsoft SQL Server** aus.  
  
3.  Zum Deaktivieren des CEIP klicken Sie auf **Nein, ich möchte nicht teilnehmen**.  
  
     Zum Aktivieren des CEIP klicken Sie auf **Ja, ich möchte teilnehmen**.  
  
4.  Klicken Sie auf **OK**.  
  
 **Verwenden einer registrierungsbasierten Richtlinie oder Gruppenrichtlinie**  
  
 Unternehmenskunden können durch Festlegen einer registrierungsbasierten Richtlinie für SQL Server 2014 eine Gruppenrichtlinie erstellen, um das Einverständnis zu widerrufen bzw. neu zu erteilen.  
  
 Der entsprechende Registrierungsschlüssel und die entsprechenden Einstellungen lauten wie folgt:  
  
 Schlüssel = HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\120  
  
 Name des Registrierungseintrags = CustomerFeedback  
  
 DWORD-Eintrag:  
  
-   0 bedeutet keine Teilnahme  
  
-   1 bedeutet einschalten  
  
