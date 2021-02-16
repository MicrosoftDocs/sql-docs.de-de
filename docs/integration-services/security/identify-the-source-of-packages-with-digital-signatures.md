---
description: Identifizieren der Quelle von Paketen mit digitalen Signaturen
title: Identifizieren der Quelle von Paketen mit digitalen Signaturen | Microsoft-Dokumentation
ms.custom: security
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.digitalsigning.f1
helpviewer_keywords:
- signing packages [Integration Services]
- certificates [Integration Services]
- packages [Integration Services], security
- security [Integration Services], certificates
- signing policies [Integration Services]
ms.assetid: a433fbef-1853-4740-9d5e-8a32bc4ffbb2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f4607ffe77d7fe93aff86d8dc707bb86e515d67d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340792"
---
# <a name="identify-the-source-of-packages-with-digital-signatures"></a>Identifizieren der Quelle von Paketen mit digitalen Signaturen

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Ein [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Paket kann mit einem digitalen Zertifikat signiert werden, um seine Quelle zu identifizieren. Nachdem das Paket mit einem digitalen Zertifikat signiert wurde, können Sie die digitale Signatur vor dem Laden des Pakets mit [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] überprüfen. Damit [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] die Signatur prüft, legen Sie eine Option entweder in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] oder im **dtexec** -Hilfsprogramm (dtexec.exe) fest, oder geben Sie einen optionalen Registrierungswert an.  
  
## <a name="sign-a-package-with-a-digital-certificate"></a>Signieren eines Pakets mit einem digitalen Zertifikat  
 Bevor Sie ein Paket mit einem digitalen Zertifikat signieren können, müssen Sie zunächst ein Zertifikat abrufen oder erstellen. Sobald Sie das Zertifikat vorliegen haben, können Sie es zum Signieren des Pakets verwenden. Weitere Informationen zum Abrufen eines Zertifikats und zum Signieren eines Pakets mit diesem Zertifikat finden Sie unter [Signieren eines Pakets mit einem digitalen Zertifikat](#cert).  
  
## <a name="set-an-option-to-check-the-package-signature"></a>Festlegen einer Option zum Überprüfen der Paketsignatur  
 Sowohl [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] als auch das **dtexec** -Hilfsprogramm bieten eine Option, mit der [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] für die Überprüfung der digitalen Signatur eines signierten Pakets konfiguriert wird. Ob Sie [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] oder das **dtexec** -Hilfsprogramm verwenden, hängt davon ab, ob Sie alle Pakete oder nur bestimmte Pakete überprüfen möchten:  
  
-   Wählen Sie die Option **Digitale Signatur beim Laden eines Pakets überprüfen** in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]aus, um die digitale Signatur aller Pakete vor dem Laden der Pakete zum Entwurfszeitpunkt zu überprüfen. Diese Option ist eine globale Einstellung für alle Pakete in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].
  
-   Zum Überprüfen der digitalen Signatur eines einzelnen Pakets legen Sie die Option **/VerifyS[igned]** fest, wenn Sie das Paket mit dem **dtexec** -Hilfsprogramm ausführen. Weitere Informationen finden Sie [hier](../../integration-services/packages/dtexec-utility.md).  
  
## <a name="set-a-registry-value-to-check-package-signature"></a>Festlegen eines Registrierungswerts zum Überprüfen der Paketsignatur  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] unterstützt außerdem den optionalen Registrierungswert **BlockedSignatureStates**, mit dem Sie die Richtlinie einer Organisation zum Laden von signierten und nicht signierten Paketen verwalten können. Der Registrierungswert kann das Laden von Paketen verhindern, wenn die Pakete nicht signiert sind, oder ungültige oder nicht vertrauenswürdige Signaturen enthalten. Weitere Informationen zum Festlegen des Registrierungswerts finden Sie unter [Implementieren einer Signaturrichtlinie durch Festlegen eines Registrierungswerts](#registry).  
  
> **HINWEIS:** Der optionale Registrierungswert **BlockedSignatureStates** kann eine Einstellung angeben, die restriktiver ist als die Option für die digitale Signatur, die in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] oder in der **dtexec**-Befehlszeile festgelegt wurde. In dieser Situation überschreibt die restriktivere Registrierungseinstellung die andere Einstellung.  

## <a name="implement-a-signing-policy-by-setting-a-registry-value"></a><a name="registry"></a> Implementieren einer Signaturrichtlinie durch Festlegen eines Registrierungswerts
  Sie können einen optionalen Registrierungswert zum Verwalten einer Organisationsrichtlinie verwenden, um signierte und nicht signierte Pakete zu laden. Wenn Sie diesen Registrierungswert verwenden, müssen Sie ihn auf jedem Computer erstellen, auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Pakete ausgeführt werden und auf dem Sie die Richtlinie durchsetzen möchten. Nachdem der Registrierungswert festgelegt wurde, überprüft oder verifiziert [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] die Signaturen vor dem Laden der Pakete.  
  
 Die Prozedur in diesem Thema beschreibt, wie Sie den optionalen DWORD-Wert **BlockedSignatureStates** zum Registrierungsschlüssel HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\100\SSIS hinzufügen. Der Datenwert in **BlockedSignatureStates** bestimmt, ob ein Paket blockiert werden soll, wenn es eine nicht vertrauenswürdige Signatur, eine ungültige Signatur oder keine Signatur aufweist. Hinsichtlich des Status der zum Signieren von Paketen verwendeten Signaturen werden für den Registrierungswert **BlockedSignatureStates** folgende Definitionen verwendet:  
  
-   Eine *gültige Signatur* ist eine Signatur, die erfolgreich gelesen werden kann.  
  
-   Eine *ungültige Signatur* ist eine Signatur, bei der die entschlüsselte Prüfsumme (der mit einem privaten Schlüssel verschlüsselte unidirektionale Hash des Paketcodes) nicht der entschlüsselten Prüfsumme entspricht, die während des Ladevorgangs von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Paketen berechnet wird.  
  
-   Eine *vertrauenswürdige Signatur* ist eine Signatur, die mithilfe eines von einer vertrauenswürdigen Stammzertifizierungsstelle signierten digitalen Zertifikats erstellt wird. Für diese Einstellung ist es nicht erforderlich, dass der Unterzeichner in der Liste vertrauenswürdiger Herausgeber des Benutzers vorhanden ist.  
  
-   Eine *nicht vertrauenswürdige Signatur* ist eine Signatur, bei der nicht überprüft werden kann, ob sie von einer vertrauenswürdigen Stammzertifizierungsstelle ausgestellt wurde, oder eine Signatur, die nicht aktuell ist.  
  
 In der folgenden Tabelle werden die gültigen Werte der DWORD-Daten und ihre verbundenen Richtlinien aufgelistet.  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|0|Keine administrative Einschränkung.|  
|1|Blockieren von ungültigen Signaturen.<br /><br /> Bei dieser Einstellung werden nicht signierte Pakete nicht blockiert.|  
|2|Blockieren von ungültigen und nicht vertrauenswürdigen Signaturen.<br /><br /> Bei dieser Einstellung werden zwar nicht signierte Pakete nicht blockiert, selbst generierte Signaturen werden jedoch blockiert.|  
|3|Blockieren von ungültigen und nicht vertrauenswürdigen Signaturen und nicht signierten Paketen.<br /><br /> Bei dieser Einstellung werden auch selbst generierte Signaturen blockiert.|  
  
> [!NOTE]  
>  Die empfohlene Einstellung für **BlockedSignatureStates** ist 3. Diese Einstellung bietet den besten Schutz vor nicht signierten Paketen oder Signaturen, die entweder ungültig oder nicht vertrauenswürdig sind. Die empfohlene Einstellung eignet sich jedoch möglicherweise nicht in allen Fällen. Weitere Informationen zum Signieren von Digital Assets finden Sie im Thema "[Introduction to Code Signing](/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537361(v=vs.85))" (in Englisch) in der MSDN Library.  
  
### <a name="to-implement-a-signing-policy-for-packages"></a>So implementieren Sie eine Signaturrichtlinie für ein Paket  
  
1.  Klicken Sie im Menü **Start** auf **Ausführen**.  
  
2.  Geben Sie im Dialogfeld **Regedit** ein, und klicken Sie dann auf **OK**.  
  
3.  Suchen Sie den Schlüssel HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\100\SSIS.  
  
4.  Klicken Sie mit der rechten Maustaste auf **MSDTS**, zeigen Sie auf **Neu**, und klicken Sie anschließend auf **DWORD-Wert**.  
  
5.  Aktualisieren Sie den Namen des neuen Werts auf **BlockedSignatureStates**.  
  
6.  Klicken Sie mit der rechten Maustaste auf **BlockedSignatureStates** , und klicken Sie anschließend auf **Ändern**.  
  
7.  Geben Sie im Dialogfeld **DWORD-Wert bearbeiten** den Wert 0, 1, 2, oder 3 ein.  
  
8.  Klicken Sie auf **OK**.  
  
9. Klicken Sie im Menü **Datei** auf **Beenden**.    

## <a name="sign-a-package-by-using-a-digital-certificate"></a><a name="cert"></a> Signieren eines Pakets mit einem digitalen Zertifikat
  In diesem Thema wird beschrieben, wie ein [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Paket mit einem digitalen Zertifikat signiert wird. Mit einer digitalen Signatur in Verbindung mit anderen Einstellungen können Sie verhindern, dass ein ungültiges Paket geladen und ausgeführt wird.  
  
 Bevor Sie ein [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Paket signieren können, müssen Sie folgende Schritte ausführen:  
  
-   Erstellen oder besorgen Sie sich einen privaten Schlüssel für die Zuordnung zum Zertifikat, und speichern Sie diesen Schlüssel auf dem lokalen Computer.  
  
-   Besorgen Sie sich ein Zertifikat zum Zwecke der Codesignierung von einer vertrauenswürdigen Zertifizierungsstelle. Sie können eine der folgenden Methoden verwenden, um ein Zertifikat zu erhalten oder zu erstellen:  
  
    -   Rufen Sie ein Zertifikat von einer öffentlichen, kommerziellen Zertifizierungsstelle ab, die Zertifikate ausgibt.  
  
    -   Rufen Sie ein Zertifikat von einem Zertifikatserver ab, mit dem eine Organisation Zertifikate intern ausstellen kann. Sie müssen das Stammzertifikat, mit dem das Zertifikat signiert wird, zum Speicher für **Vertrauenswürdige Stammzertifizierungsstellen** hinzufügen. Zum Hinzufügen des Stammzertifikats verwenden Sie das Zertifikate-Snap-In für die [!INCLUDE[msCoName](../../includes/msconame-md.md)] -Verwaltungskonsole (MMC). Weitere Informationen finden Sie im Thema "[Certificate Services](/windows/win32/seccrypto/certificate-services)" (möglicherweise nur in englischer Sprache) in der MSDN Library.  
  
    -   Erstellen Sie ein eigenes Zertifikat nur für Testzwecke. Das Certificate Creation-Tool (Makecert.exe) generiert X.509-Zertifikate für Testzwecke. Weitere Informationen finden Sie im Thema „[Certificate Creation Tool (Makecert.exe)](/previous-versions/dotnet/netframework-2.0/bfsktky3(v=vs.80))“ in der MSDN Library.  
  
     Weitere Informationen über Zertifikate finden Sie in der Onlinehilfe für das Zertifikate-Snap-In. Weitere Informationen zum Signieren von Digital Assets finden Sie im Thema "[Signing and Checking Code with Authenticode](/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537364(v=vs.85))" (möglicherweise nur in englischer Sprache) in der MSDN Library.  
  
-   Stellen Sie sicher, dass das Zertifikat für Codesignaturen aktiviert wurde. Überprüfen Sie die Eigenschaften des Zertifikats im Zertifikate-Snap-In, um festzustellen, ob ein Zertifikat zum Signieren von Code aktiviert ist.  
  
-   Legen Sie das Zertifikat im Speicher Persönlich ab.  
  
 Nachdem Sie die vorherigen Schritte ausgeführt haben, können Sie das folgende Verfahren verwenden, um ein Paket zu signieren.  
  
### <a name="to-sign-a-package"></a>So signieren Sie ein Paket  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Projekt mit dem Paket, das Sie signieren möchten.  
  
2.  Doppelklicken Sie im Projektmappen-Explorer auf das Paket, um es zu öffnen.  
  
3.  Klicken Sie in [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer im Menü **SSIS** auf **Digitale Signatur**.  
  
4.  Klicken Sie im Dialogfeld **Digitale Signatur** auf **Signieren**.  
  
5.  Wählen Sie im Dialogfeld **Zertifikat auswählen** ein Zertifikat aus.  
  
6.  (Optional) Klicken Sie auf **Zertifikat anzeigen**, um Zertifikatinformationen anzuzeigen.  
  
7.  Klicken Sie auf **OK** , um das Dialogfeld **Zertifikat auswählen** zu schließen.  
  
8.  Klicken Sie auf **OK** , um das Dialogfeld **Digitale Signatur** zu schließen.  
  
9. Klicken Sie im Menü **Datei** auf **Ausgewählte Elemente speichern** , um das aktualisierte Paket zu speichern.  
  
     Obwohl das Paket signiert wurde, müssen Sie [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] nun so konfigurieren, dass die digitale Signatur vor dem Laden des Pakets geprüft oder verifiziert wird.  

## <a name="digital-signing-dialog-box-ui-reference"></a><a name="signing_dialog"></a> Benutzeroberflächen-Verweis zum Dialogfeld „Digitale Signatur“
  Mithilfe des Dialogfelds **Digitale Signatur** können Sie ein Paket mit einer digitalen Signatur signieren oder die Signatur löschen. Das Dialogfeld **Digitale Signatur** steht in **unter der Option** Digitale Signatur **im Menü** SSIS [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]zur Verfügung.  
  
 Weitere Informationen finden Sie unter [Signieren eines Pakets mit einem digitalen Zertifikat](#cert).  
  
### <a name="options"></a>Tastatur  
 **Signieren**  
 Klicken Sie auf diese Option, um das Dialogfeld **Zertifikat auswählen** zu öffnen und das zu verwendende Zertifikat auszuwählen.  
  
 **Remove**  
 Klicken Sie hierauf, um die digitale Signatur zu entfernen.  

## <a name="see-also"></a>Weitere Informationen  
 [Integration Services-Pakete &#40;SSIS&#41;](../../integration-services/integration-services-ssis-packages.md)   
 [Sicherheitsübersicht &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md)  
  
