---
description: Zugriffssteuerung für vertrauliche Daten in Paketen
title: Zugriffssteuerung für sensible Daten in Paketen | Microsoft-Dokumentation
ms.custom: security
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.packageprotectionlevel.f1
- sql13.ssis.bids.projectprotectionlevel.f1
- sql13.dts.designer.packagepassword.f1
- sql13.ssis.bids.projectpassword.f1
helpviewer_keywords:
- passwords [Integration Services]
- packages [Integration Services], security
- protection levels for packages [Integration Services]
- configuration files [Integration Services]
- encryption [Integration Services]
- cryptography [Integration Services]
- security [Integration Services], protection levels
ms.assetid: d4b073c4-4238-41fc-a258-4e114216e185
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b85dc4949e363a8e62ca3d992a04365f7ce414f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353017"
---
# <a name="access-control-for-sensitive-data-in-packages"></a>Zugriffssteuerung für vertrauliche Daten in Paketen

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Sie können die Daten in einem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Paket schützen, indem Sie eine Schutzebene festlegen, mit der nur vertrauliche Daten oder alle Daten im Paket geschützt werden. Darüber hinaus können Sie diese Daten mit einem Kennwort oder Benutzerschlüssel verschlüsseln oder die Daten von der Datenbank verschlüsseln lassen. Die für ein Paket verwendete Schutzebene ist außerdem nicht unbedingt statisch, sondern ändert sich im Lebenszyklus eines Pakets. Häufig wird eine Schutzebene während der Entwicklung und eine andere Schutzebene beim Bereitstellen des Pakets festgelegt.  
  
> [!NOTE]  
>  Zusätzlich zu den in diesem Thema beschriebenen Schutzebenen können Sie feste Rollen auf Datenbankebene verwenden, um Pakete zu schützen, die auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server gespeichert werden.  
  
## <a name="definition-of-sensitive-information"></a>Definition vertraulicher Informationen  
 In einem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Paket werden die folgenden Informationen als *vertraulich* definiert:  
  
-   Der Kennwortteil einer Verbindungszeichenfolge. Wenn Sie jedoch eine Option auswählen, mit der alle Daten verschlüsselt werden, wird die gesamte Verbindungszeichenfolge als vertraulich betrachtet.  
  
-   Die vom Task generierten XML-Knoten, die als sensibel markiert sind. Die Markierung der XML-Knoten wird von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] gesteuert und kann von den Benutzern nicht geändert werden.  
  
-   Alle Variablen, die als vertraulich markiert sind. Die Markierung der Variablen wird von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]gesteuert.  
  
 Ob [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] eine Eigenschaft als vertraulich einstuft oder nicht, hängt davon ab, ob der Entwickler der [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Komponente, etwa ein Verbindungs-Manager oder Task, die Eigenschaft als vertraulich gekennzeichnet hat. Benutzer können Eigenschaften weder der Liste der als vertraulich eingestuften Eigenschaften hinzufügen, noch können sie sie aus dieser Liste entfernen.  
  
## <a name="encryption"></a>Verschlüsselung  
 Die Verschlüsselung, wie sie von Paketschutzebenen verwendet wird, wird mit der Datenschutz-API (DPAPI) von [!INCLUDE[msCoName](../../includes/msconame-md.md)] durchgeführt, die Teil der Kryptografie-API (CryptoAPI) ist.  
  
 Für die Paketschutzebenen, die Pakete mit Kennwörtern verschlüsseln, müssen Sie ebenfalls ein Kennwort angeben. Wenn Sie die Schutzebene von einer Ebene, die kein Kennwort verwendet, in eine Ebene ändern, die ein Kennwort verwendet, werden Sie zur Angabe eines Kennworts aufgefordert.  
  
 Für die Schutzebenen mit Kennwort wird in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] der Dreifach-DES-Verschlüsselungsalgorithmus mit einer Schlüssellänge von 192 Bits verwendet, der in der [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] -Bibliotheksklasse (FCL) verfügbar ist.  
  
## <a name="protection-levels"></a>Schutzebenen  
 In der folgenden Tabelle werden die in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] verfügbaren Schutzebenen beschrieben. Die in Klammern stehenden Werte stammen aus der <xref:Microsoft.SqlServer.Dts.Runtime.DTSProtectionLevel> -Enumeration. Diese Werte werden im Eigenschaftenfenster angezeigt, das Sie zum Konfigurieren der Eigenschaften des Pakets verwenden, wenn Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]mit Paketen arbeiten.  
  
|Schutzebene|BESCHREIBUNG|  
|----------------------|-----------------|  
|Vertrauliche Daten nicht speichern (**DontSaveSensitive**)|Unterdrückt die Werte vertraulicher Eigenschaften im Paket, wenn das Paket gespeichert wird. Diese Schutzebene verschlüsselt nicht, sondern verhindert stattdessen, dass als sensibel markierte Eigenschaften mit dem Paket gespeichert werden. Deshalb stehen die sensiblen Daten anderen Benutzern nicht zur Verfügung. Wenn das Paket von einem anderen Benutzer geöffnet wird, werden die sensiblen Daten durch Leerzeichen ersetzt und der Benutzer muss die sensiblen Daten angeben.<br /><br /> Bei der Verwendung mit dem **dtutil** -Hilfsprogramm (dtutil.exe) entspricht diese Schutzebene dem Wert 0.|  
|Alles mit einem Kennwort verschlüsseln (**EncryptAllWithPassword**)|Verwendet ein Kennwort zum Verschlüsseln des gesamten Pakets. Das Paket wird mit einem Kennwort verschlüsselt, das beim Erstellen oder Exportieren des Pakets vom Benutzer bereitgestellt wird. Um das Paket im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer zu öffnen oder mit dem **dtexec** -Eingabeaufforderungs-Hilfsprogramm auszuführen, muss der Benutzer das Paketkennwort angeben. Ohne Kennwort kann der Benutzer das Paket weder öffnen noch ausführen.<br /><br /> Bei der Verwendung mit dem **dtutil** -Hilfsprogramm entspricht diese Schutzebene dem Wert 3.|  
|Alles mit einem Benutzerschlüssel verschlüsseln (**EncryptAllWithUserKey**)|Verwendet einen auf dem aktuellen Benutzerprofil basierenden Schlüssel zum Verschlüsseln des gesamten Pakets. Nur der Benutzer, der das Paket erstellte oder exportierte, kann das Paket im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer öffnen oder mit dem Eingabeaufforderungs-Hilfsprogramm **dtexec** ausführen.<br /><br /> Bei der Verwendung mit dem **dtutil** -Hilfsprogramm entspricht diese Schutzebene dem Wert 4.<br /><br /> Hinweis: Für Schutzebenen, für die ein Benutzerschlüssel verwendet wird, werden in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] DPAPI-Standards verwendet. Weitere Informationen zu DPAPI finden Sie in der MSDN Library unter [https://msdn.microsoft.com/library](/previous-versions/ms995355(v=msdn.10)).|  
|Vertrauliche Daten mit einem Kennwort verschlüsseln (**EncryptSensitiveWithPassword**)|Verwendet ein Kennwort zum ausschließlichen Verschlüsseln der Werte vertraulicher Eigenschaften im Paket. Für diese Verschlüsselung wird DPAPI verwendet. Die sensiblen Daten werden als Teil des Pakets gespeichert, aber diese Daten werden mit einem Kennwort verschlüsselt, das vom aktuellen Benutzer beim Erstellen oder Exportieren des Pakets bereitgestellt wird. Um das Paket im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer zu öffnen, muss der Benutzer das Paketkennwort angeben. Falls kein Kennwort angegeben wird, wird das Paket ohne die sensiblen Daten geöffnet und der Benutzer muss neue Werte für sensible Daten angeben. Falls der Benutzer versucht, das Paket auszuführen, ohne das Kennwort anzugeben, schlägt die Paketausführung fehl. Weitere Informationen zu Kennwörtern und zur Befehlszeilenausführung finden Sie unter [dtexec (Hilfsprogramm)](../../integration-services/packages/dtexec-utility.md).<br /><br /> Bei der Verwendung mit dem **dtutil** -Hilfsprogramm entspricht diese Schutzebene dem Wert 2.|  
|Vertrauliche Daten mit einem Benutzerschlüssel verschlüsseln (**EncryptSensitiveWithUserKey**)|Verwendet einen auf dem aktuellen Benutzerprofil basierenden Schlüssel zum ausschließlichen Verschlüsseln der Werte vertraulicher Eigenschaften im Paket. Das Paket kann nur von demselben Benutzer, der dasselbe Profil verwendet, geladen werden. Wenn das Paket von einem anderen Benutzer geöffnet wird, werden die sensiblen Daten durch Leerzeichen ersetzt und der aktuelle Benutzer muss neue Werte für die sensiblen Daten angeben. Wenn der Benutzer versucht, das Paket auszuführen, schlägt die Paketausführung fehl. Für diese Verschlüsselung wird DPAPI verwendet.<br /><br /> Bei der Verwendung mit dem **dtutil** -Hilfsprogramm entspricht diese Schutzebene dem Wert 1.<br /><br /> Hinweis: Für Schutzebenen, für die ein Benutzerschlüssel verwendet wird, werden in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] DPAPI-Standards verwendet. Weitere Informationen zu DPAPI finden Sie in der MSDN Library unter [https://msdn.microsoft.com/library](/previous-versions/ms995355(v=msdn.10)).|  
|Serverspeicher für die Verschlüsselung verwenden (**ServerStorage**)|Schützt das gesamte Paket mithilfe von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbankrollen. Diese Option wird unterstützt, wenn ein Paket in der msdb-Datenbank von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gespeichert wird. Der SSISDB-Katalog verwendet zudem die Schutzebene **ServerStorage** .<br /><br /> Diese Option wird nicht unterstützt, wenn ein Paket von [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]im Dateisystem gespeichert wird.|  
  
## <a name="protection-level-setting-and-the-ssisdb-catalog"></a>Einstellung der Schutzebene und SSISDB-Katalog  
 Der SSISDB-Katalog verwendet die Schutzebene **ServerStorage** . Wenn Sie ein [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Projekt auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server bereitstellen, werden die Paketdaten und sensible Werte automatisch vom Katalog verschlüsselt. Die Daten werden vom Katalog auch automatisch entschlüsselt, wenn Sie sie abrufen.  
  
 Wenn Sie das Projekt (ISPAC-Datei) vom [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server in das Dateisystem exportieren, ändert das System die Schutzebene automatisch in **EncryptSensitiveWithUserKey**. Wenn Sie das Projekt mit dem **Integration Services-Assistenten zum Importieren von Projekten** in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]importieren, wird für die **ProtectionLevel** -Eigenschaft im Fenster **Eigenschaften** der Wert **EncryptSensitiveWithUserKey** angezeigt.  
  
## <a name="protection-level-setting-based-on-package-life-cycle"></a>Festlegen der Schutzebene auf Grundlage des Paketlebenszyklus  
 Die Schutzebene eines [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Pakets wird bei dessen Entwicklung in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] festgelegt. Wenn das Paket dann zu einem späteren Zeitpunkt bereitgestellt, von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]importiert oder daraus exportiert wird oder von [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] nach [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], in den [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Paketspeicher oder in das Dateisystem kopiert wird, können Sie die Paketschutzebene aktualisieren. Wenn Sie z. B. Pakete auf Ihrem Computer mit einer Benutzerschlüssel-Schutzebenenoption erstellen und speichern, möchten Sie eventuell die Schutzebene ändern, wenn Sie das Paket anderen Benutzern übergeben; anderenfalls können diese Benutzer das Paket nicht öffnen.  
  
 Normalerweise wird die Schutzebene wie in den folgenden Schritten beschrieben geändert:  
  
1.  Behalten Sie während der Entwicklung den Standardwert für die Schutzebene der Pakete bei ( **EncryptSensitiveWithUserKey**). Mit dieser Einstellung wird sichergestellt, dass nur der Entwickler vertrauliche Werte im Paket sehen kann. Oder Sie können die Nutzung von **EncryptAllWithUserKey** oder **DontSaveSensitive** erwägen.  
  
2.  Wenn Sie die Pakete bereitstellen möchten, müssen Sie die Schutzebene so ändern, dass sie nicht von dem Benutzerschlüssel des Entwicklers abhängig ist. Deshalb müssen Sie normalerweise **EncryptSensitiveWithPassword** oder **EncryptAllWithPassword** auswählen. Verschlüsseln Sie die Pakete, indem Sie ein temporäres sicheres Kennwort zuweisen, das auch dem Betriebsteam in der Produktionsumgebung bekannt ist.  
  
3.  Nach dem Bereitstellen der Pakete in der Produktionsumgebung kann das Betriebsteam die bereitgestellten Pakete erneut verschlüsseln, indem ein neues, nur dem Betriebsteam bekanntes sicheres Kennwort zugewiesen wird. Die bereitgestellten Pakete können auch verschlüsselt werden, indem **EncryptSensitiveWithUserKey** oder **EncryptAllWithUserKey** ausgewählt wird und die lokalen Anmeldeinformationen des Kontos verwendet werden, mit dem die Pakete ausgeführt werden.  

## <a name="set-or-change-the-protection-level-of-packages"></a><a name="set_protection"></a> Festlegen oder Ändern der Schutzebene von Paketen
  Wenn der Zugriff auf den Inhalt von Paketen und die darin enthaltenen vertraulichen Werte, z. B. Kennwörter, gesteuert werden soll, legen Sie den Wert der **ProtectionLevel** -Eigenschaft fest. Zum Erstellen des Projekts müssen die in einem Projekt enthaltenen Pakete die gleiche Schutzebene wie das Projekt aufweisen. Wenn Sie die **ProtectionLevel** -Eigenschafteneinstellung für das Projekt ändern, müssen Sie die Eigenschafteneinstellung für die Pakete manuell aktualisieren.  
  
 Eine Übersicht über die Sicherheitsfeatures in [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] finden Sie unter [Sicherheitsübersicht &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md).  
  
 In den Verfahren in diesem Thema wird die Verwendung des [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] -Befehlszeilen-Hilfsprogramms oder dtutil-Befehlszeilen-Hilfsprogramms zum Ändern der **ProtectionLevel** -Eigenschaft fest.  
  
> [!NOTE]  
>  Neben dem Verfahren in diesem Thema gibt es normalerweise die Möglichkeit, die **ProtectionLevel** -Eigenschaft eines Pakets festzulegen oder zu ändern, wenn Sie das Paket importieren oder exportieren. Sie können die **ProtectionLevel** -Eigenschaft eines Pakets auch ändern, wenn Sie ein Paket mit dem [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Import/Export-Assistenten speichern.  
  
### <a name="to-set-or-change-the-protection-level-of-a-package-in-sql-server-data-tools"></a>So legen Sie die Schutzebene eines Pakets in SQL Server-Datentools fest oder ändern sie  
  
1.  Überprüfen Sie die verfügbaren Werte für die **ProtectionLevel**-Eigenschaft im Abschnitt [Schutzebenen](#protection-levels), und bestimmen Sie den richtigen Wert für das Paket.  
  
2.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Projekt mit dem Paket.  
  
3.  Öffnen Sie das Paket im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Designer.  
  
4.  Wenn die Eigenschaften des Pakets nicht im Eigenschaftenfenster angezeigt werden, klicken Sie auf die Entwurfsoberfläche.  
  
5.  Wählen Sie im Eigenschaftenfenster in der Gruppe **Sicherheit** den richtigen Wert für die **ProtectionLevel** -Eigenschaft aus.  
  
     Wenn Sie eine Schutzebene auswählen, für die ein Kennwort erforderlich ist, geben Sie das Kennwort als Wert der **PackagePassword** -Eigenschaft an.  
  
6.  Wählen Sie im Menü **Datei** die Option **Ausgewählte Elemente speichern** aus, um das geänderte Paket zu speichern.  
  
### <a name="to-set-or-change-the-protection-level-of-packages-at-the-command-prompt"></a>So legen Sie die Schutzebene von Paketen an der Eingabeaufforderung fest oder ändern sie  
  
1.  Überprüfen Sie die verfügbaren Werte für die **ProtectionLevel**-Eigenschaft im Abschnitt [Schutzebenen](#protection-levels), und bestimmen Sie den richtigen Wert für das Paket.  
  
2.  Überprüfen Sie die Zuordnungen für die **Encrypt** -Option im Thema [dtutil Utility](../../integration-services/dtutil-utility.md), und bestimmen Sie die richtige ganze Zahl, die als Wert für die ausgewählte **ProtectionLevel** -Eigenschaft verwendet werden soll.  
  
3.  Öffnen Sie ein Eingabeaufforderungsfenster.  
  
4.  Navigieren Sie an der Eingabeaufforderung zu dem Ordner mit den Paketen, für die Sie die **ProtectionLevel** -Eigenschaft festlegen möchten.  
  
     In den Syntaxbeispielen im folgenden Schritt wird davon ausgegangen, dass dieser Ordner der aktuelle Ordner ist.  
  
5.  Verwenden Sie zum Festlegen oder Ändern der Schutzebene für die Pakete einen Befehl wie in einem der folgenden Beispiele:  
  
    -   Mit dem folgenden Befehl wird die **ProtectionLevel** -Eigenschaft eines einzelnen Pakets im Dateisystem auf Ebene 2 ("Sensible Daten mit einem Kennwort verschlüsseln") mit dem Kennwort "strongpassword" festgelegt:  
  
         `dtutil.exe /file "C:\Package.dtsx" /encrypt file;"C:\Package.dtsx";2;strongpassword`  
  
    -   Mit dem folgenden Befehl wird die **ProtectionLevel** -Eigenschaft aller Pakete in einem bestimmten Ordner auf Ebene 2 ("Sensible Daten mit einem Kennwort verschlüsseln") mit dem Kennwort "strongpassword" festgelegt:  
  
         `for %f in (*.dtsx) do dtutil.exe /file %f /encrypt file;%f;2;strongpassword`  
  
         Wenn Sie einen ähnlichen Befehl in einer Batchdatei verwenden, geben Sie den Dateiplatzhalter "%f" in der Batchdatei als "%%f" ein.  

## <a name="package-project-protection-level-dialog-box"></a><a name="protection_dialog"></a> Dialogfeld „Paket- und Projektschutzebene“
  Verwenden Sie das Dialogfeld **Paketschutzebene** , um die Schutzebene eines Pakets zu aktualisieren. Die Schutzebene bestimmt die Methode, das Kennwort oder den Benutzerschlüssel und den Bereich des Paketschutzes. Der Schutz kann alle Daten oder nur vertrauliche Daten einschließen.  
  
 Zum Verständnis der Anforderungen und Optionen für die Paketsicherheit finden Sie weitere Informationen unter [Sicherheitsübersicht &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md).  
  
### <a name="options"></a>Optionen  
 **Paketschutzebene**  
 Wählen Sie eine Schutzebene aus der Liste aus.  
  
 **Kennwort**  
 Falls die Schutzebene **Sensible Daten mit einem Kennwort verschlüsseln** oder **Alle Daten mit einem Kennwort verschlüsseln** verwendet wird, geben Sie ein Kennwort ein.  
  
 **Kennwort erneut eingeben**  
 Geben Sie das Kennwort erneut ein.  

## <a name="package-password-dialog-box"></a><a name="password_dialog"></a> Dialogfeld „Paketkennwort“
  Verwenden Sie das Dialogfeld **Paketkennwort** , um ein Kennwort für ein verschlüsseltes Paket bereitzustellen. Sie müssen ein Kennwort bereitstellen, wenn das Paket die Schutzebene **Sensible Daten mit einem Kennwort verschlüsseln** oder **Alle Daten mit einem Kennwort verschlüsseln** verwendet.  
  
### <a name="options"></a>Optionen  
 **Kennwort**  
 Geben Sie das Kennwort ein.  
  
## <a name="see-also"></a>Siehe auch  
 [Integration Services-Pakete &#40;SSIS&#41;](../../integration-services/integration-services-ssis-packages.md)   
 [Sicherheitsübersicht &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md)  
 [dtutil (Hilfsprogramm)](../../integration-services/dtutil-utility.md)  
