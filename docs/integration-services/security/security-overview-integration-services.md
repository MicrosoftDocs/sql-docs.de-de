---
description: Sicherheitsübersicht (Integration Services)
title: Sicherheitsübersicht (Integration Services) | Microsoft-Dokumentation
ms.custom: security
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- SSIS packages, security
- Integration Services, security
- security [Integration Services], about security
- passwords [Integration Services]
- packages [Integration Services], security
- SQL Server Integration Services, security
- SSIS, security
- Integration Services packages, security
- SQL Server Integration Services packages, security
ms.assetid: 01aa0b88-d477-4581-9a3b-2efc3de2b133
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6b757c7c0a60ab8cff5c496c2a0774646da4c7f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340771"
---
# <a name="security-overview-integration-services"></a>Sicherheitsübersicht (Integration Services)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Die Sicherheit von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] besteht aus mehreren Schichten, die eine umfangreiche und flexible Sicherheitsumgebung bereitstellen. Diese Sicherheitsebenen umfassen die Verwendung digitaler Signaturen, Paketeigenschaften, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbankrollen und Betriebssystemberechtigungen. Die meisten dieser Sicherheitsfunktionen können den Kategorien Identität und Zugriffssteuerung zugeordnet werden.  

## <a name="threat-and-vulnerability-mitigation"></a>Mindern von Bedrohungen und Sicherheitsrisiken
  Obwohl [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] eine Reihe von Sicherheitsmechanismen bietet, können Pakete und die Dateien, die von Paketen erstellt oder verwendet werden, für böswillige Zwecke genutzt werden.  
  
 Die folgenden Tabelle erläutert die Risiken und die vorbeugenden Maßnahmen, die Sie ergreifen können, um diese Risiken zu mindern.  
  
|Bedrohung oder Sicherheitsrisiko|Definition|Minderung|  
|-----------------------------|----------------|----------------|  
|Paketquelle|Die Quelle eines Pakets ist die Einzelperson oder die Organisation, die das Paket erstellt hat. Das Ausführen eines Pakets von einer unbekannten oder nicht vertrauenswürdigen Quelle kann riskant sein.|Identifizieren Sie die Quelle eines Pakets mithilfe einer digitalen Signatur, und führen Sie nur Pakete aus, die von einer bekannten, vertrauenswürdigen Quelle stammen. Weitere Informationen finden Sie unter [Identifizieren der Quelle von Paketen mit digitalen Signaturen](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md).|  
|Paketinhalt|Paketinhalt schließt die Elemente im Paket und ihre Eigenschaften ein. Die Eigenschaften können vertrauliche Daten z. B. ein Kennwort oder eine Verbindungszeichenfolge enthalten. Paketelemente wie eine SQL-Anweisung können die Struktur Ihrer Datenbank enthüllen.|Kontrollieren Sie den Zugriff auf ein Paket und den entsprechenden Inhalten, indem Sie die folgenden Schritte ausführen:<br /><br /> 1) Um den Zugriff auf das Paket selbst zu steuern, wenden Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherheitsfunktionen für die Pakete an, die in der **msdb** -Datenbank in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz gespeichert sind. Übernehmen Sie für Pakete, die im Dateisystem gespeichert sind, Dateisystemsicherheitsfunktionen wie Zugriffssteuerungslisten.<br /><br /> 2) Um den Zugriff auf den Inhalt des Pakets zu kontrollieren, legen Sie die Schutzebene des Pakets fest.<br /><br /> Weitere Informationen finden Sie unter [Sicherheitsübersicht &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md) und [Zugriffssteuerung für vertrauliche Daten in Paketen](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).|  
|Paketausgabe|Wenn Sie für ein Paket die Verwendung von Konfigurationen, Prüfpunkten und der Protokollierung konfigurieren, werden diese Informationen außerhalb des Pakets gespeichert. Die Informationen, die außerhalb des Pakets gespeichert werden, können vertrauliche Daten enthalten.|Um Konfigurationen und Protokolle zu schützen, die das Paket in den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbanktabellen speichert, verwenden Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherheitsfunktionen.<br /><br /> Um den Zugriff auf Dateien zu kontrollieren, verwenden Sie die im Dateisystem verfügbaren Zugriffssteuerungslisten (ACLs).<br /><br /> Weitere Informationen finden Sie unter [Zugriff auf Dateien, die von Paketen verwendet werden](#files)|  
  
## <a name="identity-features"></a>Identitätsfunktionen  
 Sie können das folgende Ziel erreichen, indem Sie Identitätsfunktionen in den Paketen implementieren:  
  
 **Stellen Sie sicher, dass Sie nur Pakete von vertrauenswürdigen Quellen öffnen und ausführen**.  
  
 Um sicherzustellen, dass Sie nur Pakete von vertrauenswürdigen Quellen öffnen und ausführen, müssen Sie zunächst die Quelle der Pakete identifizieren. Sie können die Quelle identifizieren, indem Sie Pakete mit Zertifikaten signieren. Beim Öffnen oder Ausführen der Pakete können Sie dann mit [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] prüfen, ob die digitalen Signaturen vorhanden und gültig sind. Weitere Informationen finden Sie unter [Identifizieren der Quelle von Paketen mit digitalen Signaturen](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md).  
  
## <a name="access-control-features"></a>Zugriffssteuerungsfunktionen  
 Sie können das folgende Ziel erreichen, indem Sie Identitätsfunktionen in den Paketen implementieren:  
  
 **Stellen Sie sicher, dass Pakete nur von autorisierten Benutzern geöffnet und ausgeführt werden**.  
  
 Um sicherzustellen, dass Pakete nur von autorisierten Benutzern geöffnet und ausgeführt werden, müssen Sie den Zugriff auf die folgenden Informationen steuern:  
  
-   Inhalte von Paketen, insbesondere vertrauliche Daten  
  
-   Pakete und Paketkonfigurationen, die in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]gespeichert werden  
  
-   Pakete und zugehörige Dateien, wie Konfigurationen, Protokolle und Prüfpunktdateien, die im Dateisystem gespeichert werden  
  
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Dienst und Informationen über Pakete, die der Dienst in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]anzeigt  
  
### <a name="controlling-access-to-the-contents-of-packages"></a>Steuern des Zugriffs auf den Inhalt von Paketen  
 Zum Einschränken des Zugriffs auf den Inhalt eines Pakets können Sie Pakete verschlüsseln, indem Sie die ProtectionLevel-Eigenschaft des jeweiligen Pakets entsprechend festlegen. Sie können diese Eigenschaft auf die für das Paket benötigte Schutzebene festlegen. In einer Teamentwicklungsumgebung kann ein Paket beispielsweise mithilfe eines Kennworts verschlüsselt werden, das nur den Teammitgliedern bekannt ist, die an dem Paket arbeiten.  
  
 Wenn Sie die ProtectionLevel-Eigenschaft eines Pakets festlegen, erkennt [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] automatisch vertrauliche Eigenschaften und verarbeitet diese entsprechend der angegebenen Paketschutzebene. Ein Beispiel: Sie legen die ProtectionLevel-Eigenschaft eines Pakets auf eine Ebene fest, bei der vertrauliche Informationen mithilfe eines Kennworts verschlüsselt werden. Für dieses Paket verschlüsselt [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] automatisch die Werte aller vertraulichen Eigenschaften und zeigt entsprechende Daten nur an, wenn das korrekte Kennwort eingegeben wird.  
  
 In der Regel identifiziert [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Eigenschaften als vertraulich, wenn diese Eigenschaften Informationen wie ein Kennwort oder eine Verbindungszeichenfolge enthalten oder wenn diese Eigenschaften Variablen oder vom Task generierten XML-Knoten entsprechen. Ob [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] eine Eigenschaft als vertraulich einstuft oder nicht, hängt davon ab, ob der Entwickler der [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Komponente, etwa ein Verbindungs-Manager oder Task, die Eigenschaft als vertraulich gekennzeichnet hat. Benutzer können Eigenschaften weder der Liste der als vertraulich eingestuften Eigenschaften hinzufügen noch können sie sie aus dieser Liste entfernen. Wenn Sie benutzerdefinierte Tasks, Verbindungs-Manager oder Datenflusskomponenten erstellen, können Sie angeben, welche Eigenschaften [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] als vertraulich behandeln soll.  
  
 Weitere Informationen finden Sie unter [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
### <a name="controlling-access-to-packages"></a>Steuern des Zugriffs auf Pakete  
 Sie können [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Pakete in einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]in der msdb-Datenbank oder als XML-Dateien mit der Dateinamenerweiterung .dtsx im Dateisystem speichern. Weitere Informationen finden Sie unter [Speichern von Paketen](../../integration-services/save-packages.md).  
  
#### <a name="saving-packages-to-the-msdb-database"></a>Speichern von Paketen in der msdb-Datenbank  
 Das Speichern der Pakete in der msdb-Datenbank trägt zur Sicherheit auf der Server-, Datenbank- und Tabellenebene bei. In der msdb-Datenbank werden [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Pakete in der Tabelle „sysssispackages“ gespeichert. Da die Pakete in der sysssispackages- und der sysdtspackages-Tabelle der msdb-Datenbank gespeichert werden, werden die Pakete beim Sichern der msdb-Datenbank ebenfalls automatisch gesichert.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Pakete, die in der msdb-Datenbank gespeichert sind, können auch durch Anwenden der [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Rollen auf Datenbankebene geschützt werden. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] bietet auf Datenbankebene die drei festen Rollen „db_ssisadmin“, „db_ssisltduser“ und „db_ssisoperator“ zum Steuern des Paketzugriffs. Den einzelnen Paketen kann eine Lese- und eine Schreibrolle zugewiesen werden. Sie können auch benutzerdefinierte Rollen auf Datenbankebene definieren, die in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Paketen verwendet werden. Rollen können nur für Pakete implementiert werden, die in der msdb-Datenbank in einer Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]gespeichert werden. Weitere Informationen finden Sie unter [Integration Services Roles &#40;SSIS Service&#41;](../../integration-services/security/integration-services-roles-ssis-service.md) (Integration Services-Rollen [SSIS-Dienst]).  
  
#### <a name="saving-packages-to-the-file-system"></a>Speichern von Paketen im Dateisystem  
 Wenn Sie Pakete statt in der msdb-Datenbank im Dateisystem speichern, müssen Sie die Paketdateien und Ordner, die Paketdateien enthalten, sichern.  
  
### <a name="controlling-access-to-files-used-by-packages"></a>Steuern des Zugriffs auf Dateien, die von Paketen verwendet werden  
 Pakete, die für die Verwendung von Konfigurationen, Prüfpunkten und der Protokollierung konfiguriert wurden, generieren Informationen, die außerhalb des Pakets gespeichert werden. Diese Informationen können vertraulich sein und sollten geschützt werden. Prüfpunktdateien können nur auf dem Dateisystem gespeichert werden, Konfigurationen und Protokolle können im Dateisystem oder in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbanktabellen gespeichert werden. In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gespeicherte Konfigurationen und Protokolle sind durch die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherheit geschützt. Für die im Dateisystem geschriebenen Informationen sind jedoch zusätzliche Sicherheitsmaßnahmen erforderlich.  
  
 Weitere Informationen finden Sie unter [Zugriff auf Dateien, die von Paketen verwendet werden](#files).  
  
#### <a name="storing-package-configurations-securely"></a>Sicheres Speichern von Paketkonfigurationen  
 Die Paketkonfigurationen können in einer Tabelle in einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank oder im Dateisystem gespeichert werden.  
  
 Konfigurationen können in jeder [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank, nicht nur der msdb-Datenbank, gespeichert werden. So sind Sie in der Lage anzugeben, welche Datenbank als Repository von Paketkonfigurationen dient. Sie können auch den Namen der Tabelle angeben, die die Konfigurationen enthält. Die Tabelle mit der richtigen Struktur wird von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] automatisch erstellt. Das Speichern der Konfigurationen in eine Tabelle ermöglicht Sicherheit auf der Server-, Datenbank- und Tabellenebene. Außerdem werden Konfigurationen, die in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gespeichert sind, automatisch beim Sichern der Datenbank gesichert.  
  
 Wenn Sie Konfigurationen statt in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]im Dateisystem speichern, müssen Sie die Ordner, die die Paketkonfigurationsdateien enthalten, sichern.  
  
 Weitere Informationen zu Konfigurationen finden Sie unter [Package Configurations](../packages/legacy-package-deployment-ssis.md).  
  
### <a name="controlling-access-to-the-integration-services-service"></a>Steuern des Zugriffs auf den Integration Services-Dienst  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] wird der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienst zum Auflisten der gespeicherten Pakete verwendet. Damit nicht autorisierte Benutzer keine Informationen über Pakete anzeigen können, die auf lokalen und Remotecomputern gespeichert sind, und auf diese Weise Zugriff auf private Daten erhalten, müssen Sie den Zugriff auf Computer, auf denen der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienst ausgeführt wird, einschränken.  
  
 Weitere Informationen finden Sie unter [Zugriff auf den Integration Services-Dienst](#service).  

## <a name="access-to-files-used-by-packages"></a><a name="files"></a> Zugriff auf Dateien, die von Paketen verwendet werden
  Die Paketschutzebene bietet für Dateien, die außerhalb des Pakets gespeichert wurden, keinen Schutz. Hierzu gehören die folgenden Dateien:  
  
-   Konfigurationsdateien  
  
-   Prüfpunktdateien  
  
-   Protokolldateien  
  
 Diese Dateien müssen separat geschützt werden. Dies gilt insbesondere dann, wenn sie vertrauliche Informationen beinhalten.  
  
### <a name="configuration-files"></a>Konfigurationsdateien  
 Wenn in einer Konfiguration vertrauliche Informationen enthalten sind, wie z.B. der Anmeldename und das Kennwort, sollten Sie die Konfiguration in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]speichern oder eine Zugriffssteuerungsliste (ACL, Access Control List) verwenden, um den Zugriff auf den Speicherort bzw. auf den Ordner, in dem die Dateien gespeichert sind, zu beschränken und den Zugriff nur bestimmten Konten zu gewähren. In der Regel wird den Konten Zugriff gewährt, denen die Berechtigung zum Ausführen von Paketen erteilt wird, und den Konten, die Pakete verwalten und Probleme bei Paketen beheben. Hierzu gehört z. B. das Überprüfen der Inhalte der Konfiguration, des Prüfpunkts und der Protokolldateien. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bereitgestellte Speicher ist sicherer, da er Schutz auf Server- und Datenbankebene bietet. Verwenden Sie zum Speichern der Konfigurationen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Konfigurationstyp. Verwenden Sie zum Speichern im Dateisystem den XML-Konfigurationstyp.  
  
 Weitere Informationen finden Sie unter [Paketkonfigurationen](../packages/legacy-package-deployment-ssis.md), [Erstellen von Paketkonfigurationen](../packages/legacy-package-deployment-ssis.md)und [Überlegungen zur Sicherheit bei SQL Server-Installationen](../../sql-server/install/security-considerations-for-a-sql-server-installation.md).  
  
### <a name="checkpoint-files"></a>Prüfpunktdateien  
 Wenn die vom Paket verwendete Prüfpunktdatei vertrauliche Informationen enthält, sollten Sie entsprechend mithilfe einer Zugriffssteuerungsliste den Speicherort oder Ordner schützen, in dem Sie die Datei speichern. In Prüfpunktdateien werden aktuelle Statusinformationen zum Fortschritt des Pakets sowie die aktuellen Werte von Variablen gespeichert. Beispielsweise kann das Paket eine benutzerdefinierte Variable mit einer Telefonnummer enthalten. Weitere Informationen finden Sie unter [Neustarten von Paketen mit Prüfpunkten](../../integration-services/packages/restart-packages-by-using-checkpoints.md).  
  
### <a name="log-files"></a>Protokolldateien  
 Protokolleinträge, die in das Dateisystem geschrieben werden, sollten ebenfalls mithilfe einer Zugriffssteuerungsliste geschützt werden. Protokolleinträge können auch in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Tabellen gespeichert und mithilfe der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Sicherheit geschützt werden. Protokolleinträge können vertrauliche Informationen enthalten. Angenommen, das Paket enthält einen Task SQL ausführen, mit dem eine SQL-Anweisung erstellt wird, die auf eine Telefonnummer verweist. In diesem Fall enthält der Protokolleintrag der SQL-Anweisung die Telefonnummer. Die SQL-Anweisung kann auch private Informationen zu Tabellen- und Spaltennamen in Datenbanken anzeigen. Weitere Informationen finden Sie unter [Integration Services-Protokollierung &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  

## <a name="access-to-the-integration-services-service"></a><a name="service"></a> Zugriff auf den Integration Services-Dienst
  Über Paketschutzebenen kann gesteuert werden, wer ein Paket bearbeiten und ausführen darf. Zusätzlicher Schutz ist erforderlich, um die Anzahl der Personen einzuschränken, die die Liste der derzeit auf einem Server ausgeführten Pakete anzeigen und die Paketausführung in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]beenden dürfen.  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] wird der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienst zum Auflisten der ausgeführten Pakete verwendet. Mitglieder der Gruppe der Windows-Administratoren können alle aktuell ausgeführten Pakete anzeigen und ihre Ausführung beenden. Benutzer, die nicht zur Administratoren-Gruppe gehören, können nur solche ausgeführten Pakete anzeigen und deren Ausführung beenden, wenn sie selbst die Ausführung gestartet haben.  
  
 Es ist wichtig, den Zugriff auf Computer einzuschränken, auf denen ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienst ausgeführt wird. Dies gilt besonders für einen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Dienst, der Remoteordner auflisten kann. Jeder authentifizierte Benutzer kann die Auflistung von Paketen anfordern. Selbst wenn vom Dienst keine Pakete gefunden werden, listet er Ordner auf. Diese Ordnernamen können für böswillige Benutzer von Nutzen sein. Wenn ein Administrator den Dienst so konfiguriert hat, dass Ordner auf einem Remotecomputer aufgelistet werden, können die Benutzer auch Ordnernamen anzeigen, die sie normalerweise nicht anzeigen könnten.  

## <a name="related-tasks"></a>Related Tasks  
 Die folgende Liste enthält Links zu Themen, in denen die Ausführung bestimmter sicherheitsrelevanter Tasks beschrieben wird.  
  
-   [Erstellen einer benutzerdefinierten Rolle](../../integration-services/security/integration-services-roles-ssis-service.md#create)  
  
-   [Zuweisen einer Lese- und Schreibrolle zu einem Paket](../../integration-services/security/integration-services-roles-ssis-service.md#assign)  
  
-   [Implementieren einer Signaturrichtlinie durch Festlegen eines Registrierungswerts](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md#registry)  
  
-   [Signieren eines Pakets mit einem digitalen Zertifikat](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md#cert)  
  
-   [Festlegen oder Ändern der Schutzebene von Paketen](../../integration-services/security/access-control-for-sensitive-data-in-packages.md#set_protection)