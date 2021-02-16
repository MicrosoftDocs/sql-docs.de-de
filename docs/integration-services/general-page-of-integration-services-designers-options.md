---
description: Seite „Allgemein“ der Integration Services-Designer-Optionen
title: Seite „Allgemein“ der Integration Services-Designer-Optionen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Business_Intelligence_Designers.Data_Transformation_Designers.General
ms.assetid: d695690a-923b-4036-945e-7621e8651deb
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 76be801b071c85d26b8b18ed23e2c478641fa0cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347798"
---
# <a name="general-page-of-integration-services-designers-options"></a>Seite „Allgemein“ der Integration Services-Designer-Optionen

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


  Verwenden Sie die Seite **Allgemein** der Seite **Integration Services-Designer** im Dialogfeld **Optionen** , um die Optionen für das Laden, Anzeigen und Aktualisieren von Paketen festzulegen.  
  
 Um die Seite **Allgemein** in [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]zu öffnen, klicken Sie im Menü **Extras** auf **Optionen**, erweitern **Business Intelligence-Designer** und wählen **Integration Services-Designer** aus.  
  
## <a name="options"></a>Optionen  
 **Digitale Signatur beim Laden eines Pakets überprüfen**  
 Wählen Sie diese Option aus, damit [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] die digitale Signatur beim Laden eines Pakets überprüft. [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] überprüft nur, ob die digitale Signatur vorhanden und gültig ist und von einer vertrauenswürdigen Quelle stammt. [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] überprüft nicht, ob das Paket seit der Signierung geändert wurde.  
  
 Wenn Sie den **BlockedSignatureStates** -Registrierungswert festlegen, überschreibt dieser Registrierungswert die Option **Digitale Signatur beim Laden eines Pakets überprüfen** . Weitere Informationen finden Sie unter [Implementieren einer Signaturrichtlinie durch Festlegen eines Registrierungswerts](./security/identify-the-source-of-packages-with-digital-signatures.md).  
  
 Weitere Informationen zu digitalen Zertifikaten und Paketen finden Sie unter [Identifizieren der Quelle von Paketen mit digitalen Signaturen](../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md).  
  
 **Warnung anzeigen, wenn ein Paket nicht signiert ist**  
 Wählen Sie diese Option aus, wenn beim Laden eines unsignierten Pakets eine Warnung angezeigt werden soll.  
  
 **Bezeichnungen für Rangfolgeneinschränkungen anzeigen**  
 Wählen Sie aus, welche Bezeichnung – Success, Failure oder Completion (Erfolg, Fehler oder Beendet) – bei Rangfolgeneinschränkungen angezeigt werden soll, wenn Sie Pakete in [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] anzeigen.  
  
 **"Skriptsprache"**  
 Wählen Sie die Standardskriptsprache für neue Skripttasks und Skriptkomponenten aus.  
  
 **Verbindungszeichenfolgen zum Verwenden neuer Anbieternamen aktualisieren**  
 Wenn Sie [!INCLUDE[ssISversion2005](../includes/ssisversion2005-md.md)] -Pakete öffnen oder aktualisieren, werden die Verbindungszeichenfolgen für die aktuelle Version von [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]dahingehend aktualisiert, dass sie die Namen für die folgenden Anbieter verwenden:  
  
-   [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] OLE DB-Anbieter  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Native Client  
  
 Der [!INCLUDE[ssIS](../includes/ssis-md.md)] -Paketupgrade-Assistent aktualisiert nur Verbindungszeichenfolgen, die in Verbindungs-Managern gespeichert sind. Der Assistent aktualisiert keine Verbindungszeichenfolgen, die dynamisch mithilfe der [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] -Ausdruckssprache oder mithilfe von Code in einem Skripttask erstellt wurden.  
  
 **Neue Paket-ID erstellen**  
 Erstellen Sie beim Upgrade von [!INCLUDE[ssISversion2005](../includes/ssisversion2005-md.md)] -Paketen neue Paket-IDs für die aktualisierten Versionen der Pakete.  
  
## <a name="see-also"></a>Siehe auch  
 [Sicherheitsübersicht &#40;Integration Services&#41;](../integration-services/security/security-overview-integration-services.md)   
 [Erweitern von Paketen mit Skripts](../integration-services/extending-packages-scripting/extending-packages-with-scripting.md)  
  
