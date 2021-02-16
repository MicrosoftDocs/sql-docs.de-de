---
description: Extrahieren einer DAC aus einer Datenbank
title: Extrahieren einer DAC aus einer Datenbank | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/18/2016
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.extractdacwizard.validationandsummary.f1
- sql13.swb.extractdacwizard.introduction.f1
- sql13.swb.extractdacwizard.selectdatapage.f1
- sql13.swb.extractdacwizard.setdacproperties.f1
- sql13.swb.extractdacwizard.buildandsave.f1
helpviewer_keywords:
- extract DAC
- How to [DAC], extract
- data-tier application [SQL Server], extract
- wizard [DAC], extract
ms.assetid: ae52a723-91c4-43fd-bcc7-f8de1d1f90e5
author: stevestein
ms.author: sstein
ms.openlocfilehash: 5091c3437d0e74f3c108e13efb3789997cadb2d3
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489464"
---
# <a name="extract-a-dac-from-a-database"></a>Extrahieren einer DAC aus einer Datenbank
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Verwenden Sie entweder den **Assistenten zum Extrahieren von Datenebenenanwendungen** oder ein Windows PowerShell-Skript, um ein Datenebenenanwendungs-Paket (DAC) aus einer vorhandenen SQL Server-Datenbank zu extrahieren. Bei der Extraktion wird eine DAC-Paketdatei erstellt, die Definitionen der Datenbankobjekte und ihrer verwandten Elemente auf Instanzebene enthält. Eine DAC-Paketdatei enthält z. B. die Datenbanktabellen, gespeicherten Prozeduren, Sichten und Benutzer zusammen mit den Anmeldenamen, die den Datenbankbenutzern zugeordnet sind.  
  
 
## <a name="before-you-begin"></a>Voraussetzungen  
 Sie können DAC aus Datenbanken extrahieren, die sich auf Instanzen von [!INCLUDE[ssSDS](../../includes/sssds-md.md)]oder [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 4 (SP4) oder höher befinden. Wenn der Extrahierungsprozess für eine Datenbank ausgeführt wird, die über eine DAC bereitgestellt wurde, werden nur die Definitionen der Objekte in der Datenbank extrahiert. Der Prozess verweist nicht auf die in **msdb** (**master** in [!INCLUDE[ssSDS](../../includes/sssds-md.md)]) registrierte DAC. Der Extrahierungsprozess registriert die DAC-Definition nicht in der aktuellen Instanz der Datenbank-Engine. Weitere Informationen zum Registrieren einer DAC finden Sie unter [Register a Database As a DAC](../../relational-databases/data-tier-applications/register-a-database-as-a-dac.md).  
  
##  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> Einschränkungen  
 Eine DAC kann nur aus einer Datenbank in [!INCLUDE[ssSDS](../../includes/sssds-md.md)]oder [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 4 (SP4) oder höher extrahiert werden. Eine DAC kann nicht registriert werden, wenn die Datenbank in einer DAC nicht unterstützte Objekte oder enthaltene Benutzer enthält. Weitere Informationen zu den in einer DAC unterstützten Objekttypen finden Sie unter [DAC Support For SQL Server Objects and Versions](/previous-versions/sql/sql-server-2012/ee210549(v=sql.110)).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Zum Registrieren einer DAC sind mindestens die ALTER ANY LOGIN-Berechtigung und die VIEW DEFINITION-Berechtigung auf Datenbankebene sowie SELECT-Berechtigungen für **sys.sql_expression_dependencies** erforderlich. Zum Extrahieren einer DAC sind nur Mitglieder der festen Serverrolle securityadmin berechtigt, die ebenfalls Mitglieder der festen Datenbankrolle database_owner in der Datenbank waren, aus der die DAC extrahiert wird. Mitglieder der festen Serverrolle „sysadmin“ oder das integrierte SQL Server-Systemadministratorkonto **sa** sind ebenfalls berechtigt, eine DAC zu extrahieren.  
  
##  <a name="using-the-extract-data-tier-application-wizard"></a><a name="UsingDACExtractWizard"></a> Verwenden des Assistenten zum Extrahieren von Datenebenenanwendungen  
 **So extrahieren Sie eine DAC mithilfe eines Assistenten**  
  
1.  Erweitern Sie im **Objekt-Explorer** den Knoten für die Instanz, die die Datenbank enthält, aus der DAC extrahiert werden soll.  
  
2.  Erweitern Sie den Knoten **Datenbanken** .  
  
3.  Klicken Sie mit der rechten Maustaste auf den Knoten für die Datenbank, aus der die DAC extrahiert werden soll, zeigen Sie auf **Tasks**, und wählen Sie dann **Datenebenenanwendung extrahieren...** aus.  
  
4.  Bearbeiten Sie die Dialogfenster des Assistenten:  
  
    1.  [Seite "Einführung"](#Introduction)  
  
    2.  [Seite "Daten auswählen"](#SelectData)  
  
    3.  [Seite "Eigenschaften festlegen"](#SetProperties)  
  
    4.  [Seite "Überprüfung und Zusammenfassung"](#ValidateSummary)  
  
    5.  [Seite "Paket erstellen"](#BuildPackage)  
  
###  <a name="wizard-introduction-page"></a><a name="Introduction"></a> Seite „Einführung“ des Assistenten  
 Auf dieser Seite werden die Schritte zum Extrahieren einer Datenebenenanwendung beschrieben.  
  
 **Diese Seite nicht mehr anzeigen.** – Aktivieren Sie dieses Kontrollkästchen, damit die Seite in Zukunft nicht mehr angezeigt wird.  
  
 **Weiter >** – Geht zur Seite **Methode auswählen** über.  
  
 **Abbrechen:** Beendet den Assistenten, ohne eine Datenebenenanwendung aus der Datenbank zu extrahieren.  
  
 [&#91;Assistent zum Extrahieren&#93;](#UsingDACExtractWizard)  
  
###  <a name="select-data-page"></a><a name="SelectData"></a> Select data page  
Wählen Sie die Verweisdaten aus, die Sie in die Paketdatei der Datenebenenanwendung (DAC) einschließen möchten. Das Einschließen von Daten in das DAC-Paket ist optional. Das DAC-Paket enthält bereits das Schema aller unterstützten Datenbankobjekte und Instanzobjekte für die Datenbank.  
  
 In die DAC-Paketdatei können Sie bis zu 10 MB Verweisdaten einschließen. Bei Tabellen, die in der DAC eingeschlossen werden sollen, dürfen in den Paketen keine Blobdatentypen (Binary Large Object) enthalten sein, z.B. **image** oder **varchar(max)**. Wenn Sie zum Übertragen in eine andere Datenbank größere Datenmengen extrahieren möchten, verwenden Sie SQL Server Integration Services, das Hilfsprogramm zum Massenkopieren oder eines der zahlreichen weiteren Datenmigrationsverfahren.  
  
 **Datenbanktabelle:** Aktivieren Sie das Kontrollkästchen neben den Datenbanktabellen mit den Daten, die Sie in das DAC-Paket einschließen möchten. Sie können bis zu zehn Tabellen mit jeweils maximal 10.000 Zeilen auswählen.  
  
 [&#91;Assistent zum Extrahieren&#93;](#UsingDACExtractWizard)  
  
###  <a name="set-properties-page"></a><a name="SetProperties"></a> Set properties page  
 Mithilfe dieser Seite des Assistenten können Sie die Datenebenenanwendung (DAC) beschreiben. Diese Eigenschaften werden verwendet, um die DAC zu identifizieren und sie von anderen zu unterscheiden.  
  
 **Name:** Dieser Name identifiziert die DAC. Er kann sich vom Namen der DAC-Paketdatei unterscheiden und sollte die Anwendung beschreiben. Wenn die Datenbank z. B. für eine Finanzanwendung verwendet wird, möchten Sie sie möglicherweise "DAC Finanzen" nennen.  
  
 **Version (xx.xx.xx.xx verwenden, wobei 'x' einer Zahl entspricht):** Ein numerischer Wert, der die Version der DAC identifiziert. Die DAC-Version wird in Visual Studio verwendet, um die Version der DAC zu identifizieren, an der die Entwickler arbeiten. Bei der Bereitstellung einer DAC wird die Version in der **msdb** -Datenbank gespeichert und kann später unter dem Knoten **Datenebenenanwendungen** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]angezeigt werden.  
  
 **Beschreibung:** Optional. Beschreibt die DAC. Bei der Bereitstellung einer DAC wird die Beschreibung in einer **msdb** -Datenbank gespeichert und kann später unter dem Knoten **Datenebenenanwendungen** in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]angezeigt werden.  
  
 **In DAC-Paketdatei speichern (Erweiterung „.dacpac“ in den Dateinamen aufnehmen):** Speichert die DAC in einer DAC-Paketdatei mit .dacpac-Erweiterung. Klicken Sie auf die Schaltfläche **Durchsuchen** , um Namen und Speicherort für die Datei anzugeben.  
  
 **Vorhandene Datei überschreiben:** Aktivieren Sie dieses Kontrollkästchen, um die DAC-Paketdatei zu ersetzen, wenn bereits eine Datei mit demselben Namen vorhanden ist.  
  
###  <a name="validation-and-summary-page"></a><a name="ValidateSummary"></a> Validation and summary page  
 Auf dieser Seite überprüft der Assistent, ob alle Datenbankobjekte von einer Datenebenenanwendung (DAC) unterstützt werden. Er überprüft auch Abhängigkeiten über Datenbankobjekte hinweg, um den Satz von Objekten zu bestimmen, die in DAC enthalten sein können. Anschließend zeigt er den Überprüfungsbericht an und fasst die Optionen zusammen, die Sie in diesem Assistenten aktiviert haben. Um eine Option zu ändern, klicken Sie auf **Zurück**. Um mit dem Extrahieren einer DAC zu beginnen, klicken Sie auf **Weiter**.  
  
> **HINWEIS!**: Wenn ein oder mehrere Objekte nicht von einer DAC unterstützt werden, wird die Schaltfläche **Weiter** deaktiviert, und der Extrahierungsprozess wird möglicherweise nicht fortgesetzt. In solchen Fällen wird empfohlen, die nicht unterstützten Objekte zu entfernen und diesen Assistenten anschließend erneut auszuführen.  
  
 **Zusammenfassung:** Unter **DAC-Eigenschaften** wird eine Übersicht zu den von Ihnen ausgewählten Optionen aufgeführt. Die Ergebnisse der Überprüfung sind unter **DAC-Objekte** aufgeführt. Es gibt drei Typen von Überprüfungsergebnissen:  
  
-   **Objekte, die erfolgreich in die DAC eingeschlossen wurden:** Diese Objekte und ihre Abhängigkeiten werden unterstützt und können erfolgreich in die DAC eingeschlossen werden.  
  
-   **Objekte, die mit Warnungen in DAC eingeschlossen wurden**: Diese Objekte werden unterstützt, sind aber von anderen Objekten abhängig, die nicht in einer DAC unterstützt werden.  
  
-   **Objekte, die nicht in DAC eingeschlossen wurden**: Diese Objekte werden nicht unterstützt und müssen vor dem Extrahieren einer DAC aus der Datenbank entfernt werden.  
  
 Der Überprüfungsprozess überprüft mehrere Ebenen von Abhängigkeiten. Wenn eine gespeicherte Prozedur z. B. von einer Tabelle abhängig ist, die den nicht unterstützten CLR-Datentyp verwendet, wird die gespeicherte Prozedur unter **Objekte, die mit Warnungen in DAC eingeschlossen wurden** aufgeführt.  
  
 Wenn ein oder mehrere Objekte nicht von einer DAC unterstützt werden, wird die Schaltfläche **Weiter** deaktiviert und der Extrahierungsprozess wird nicht fortgesetzt. In solchen Fällen wird empfohlen, die nicht unterstützten Objekte zu entfernen und diesen Assistenten anschließend erneut auszuführen.  
  
 **Bericht speichern:** Ermöglicht Ihnen das Speichern einer HTML-basierten Datei, in der alle Objekte unter dem Knoten **DAC-Objekte** in der Zusammenfassung aufgeführt sind. Dieser Bericht kann nützlich sein, wenn einige der Datenbankobjekte nicht in einer DAC unterstützt werden. Mithilfe des Berichts können Sie nicht unterstützte Objekte ändern oder entfernen, bevor Sie erneut versuchen, die DAC zu extrahieren.  
  
 ###  <a name="build-package-page"></a><a name="BuildPackage"></a> Build package page  
 Verwenden Sie diese Seite, um den Status des Assistenten zu überwachen, während die Datenebenenanwendung (DAC) extrahiert wird.  
  
 **Aktion:** Während der Aktion **DAC-Paketdatei erstellen und speichern** extrahiert der Assistent eine DAC aus der SQL Server-Datenbank. Anschließend wird ein DAC-Paket im Arbeitsspeicher erstellt und am von Ihnen angegebenen Speicherort gespeichert. Klicken Sie auf die Links in der Spalte **Ergebnis** , um das Ergebnis des entsprechenden Schritts anzuzeigen.  
  
 **Bericht speichern:** Klicken Sie hier, um den Fortschritt des Assistenten in eine Datei zu speichern.  
  
 **Fertig stellen:** Klicken Sie hier, um den Assistenten nach der Verarbeitung oder bei Auftreten eines Fehlers zu schließen.  
   
##  <a name="extract-a-dac-using-powershell"></a><a name="ExtractDACPowerShell"></a> Extrahieren einer DAC mit PowerShell  
 **So extrahieren Sie mithilfe der Extract()-Methode in einem PowerShell-Skript eine DAC aus einer Datenbank**  
  
1.  Erstellen Sie ein SMO-Serverobjekt und legen Sie dieses für die Instanz fest, die die Datenbank enthält, aus der Sie die DAC extrahieren möchten.  
  
2.  Fügen Sie eine Variable hinzu, die den Namen der Datenbank angibt.  
  
3.  Geben Sie die Metadaten für die DAC an, z. B. DAC-Namen, Version und Beschreibung.  
  
4.  Geben Sie den Pfad und den Dateinamen für die extrahierte DAC-Paketdatei an.  
  
5.  Führen Sie die Extract-Methode mit den oben angegebenen Informationen aus.  
  
### <a name="example-powershell"></a>Beispiel (PowerShell)  
 Im folgenden Beispiel wird eine DAC mit dem Namen MyApplication aus einer Datenbank mit dem Namen MyDB extrahiert.  
  
```  
## Set a SMO Server object to the default instance on the local computer.  
CD SQLSERVER:\SQL\localhost\DEFAULT  
$srv = get-item .  
  
## Specify the database to extract to a DAC.  
$dbname = "MyDB"  
  
## Specify the DAC metadata.  
$applicationname = "MyApplication"  
$version = "1.0.0.0"  
$description = "This DAC defines the database used by my application."  
  
## Specify the location and name for the extracted DAC package.  
$dacpacPath = "C:\MyDACs\MyApplication.dacpac"  
  
## Extract the DAC.  
$extractionunit = New-Object Microsoft.SqlServer.Management.Dac.DacExtractionUnit($srv, $dbname, $applicationname, $version)  
$extractionunit.Description = $description  
$extractionunit.Extract($dacpacPath)  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 - [Datenebenenanwendungen](../../relational-databases/data-tier-applications/data-tier-applications.md) 
 - [SqlPackage in Entwicklungspipelines](../../tools/sqlpackage/sqlpackage-pipelines.md)