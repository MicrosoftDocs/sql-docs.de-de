---
description: Bereitstellen von SQL Server Integration Services-Projekten und Paketen (SSIS)
title: Bereitstellen von SQL Server Integration Services-Projekten und -Paketen (SSIS) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 09/26/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: vanto
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.bids.converttolegacydeployment.f1
- sql13.ssis.deploymentwizard.f1
- sql13.ssis.ssms.isenvprop.permissions.f1
- sql13.ssis.ssms.isenvprop.general.f1
- sql13.ssis.ssms.iscreateenv.f1
- sql13.ssis.ssms.isenvprop.variables.f1
- sql13.ssis.migrationwizard.f1
ms.assetid: bea8ce8d-cf63-4257-840a-fc9adceade8c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5ee14ee789174ad12db2cea297202fc63ee68d66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346928"
---
# <a name="deploy-integration-services-ssis-projects-and-packages"></a>Bereitstellen von SQL Server Integration Services-Projekten und Paketen (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] unterstützt zwei Bereitstellungsmodelle: das Projektbereitstellungsmodell und das Legacy-Paketbereitstellungsmodell. Mithilfe des Projektbereitstellungsmodells können Sie Projekte auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server bereitstellen.  
  
Weitere Informationen zum Legacy-Paketbereitstellungsmodell finden Sie unter [Legacy-Paketbereitstellung &#40;SSIS&#41;](../../integration-services/packages/legacy-package-deployment-ssis.md).  
  
> [!NOTE]  
>  Das Projektbereitstellungsmodell wurde in [!INCLUDE[ssISversion11](../../includes/ssisversion11-md.md)]eingeführt. Mit diesem Bereitstellungsmodell konnten Sie keine Pakete bereitstellen, ohne das gesamte Projekt bereitzustellen. In [!INCLUDE[ssISversion13](../../includes/ssisversion13-md.md)] wurde die Funktion für inkrementelle Paketbereitstellung eingeführt, mit der Sie ein oder mehrere Pakete bereitstellen können, ohne das gesamte Projekt bereitzustellen.

> [!NOTE]
> In diesem Artikel wird beschrieben, wie Sie SSIS-Pakete allgemein und lokal bereitstellen. Sie können SSIS-Pakete auch auf folgenden Plattformen bereitstellen:
> - **Die Microsoft Azure-Cloud**. Weitere Informationen finden Sie unter [Migration von SQL Server Integration Services-Workloads in die Cloud per Lift und Shift](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).
> - **Linux**. Weitere Informationen finden Sie unter [Extrahieren, Transformieren und Laden von Daten unter Linux mit SSIS](../../linux/sql-server-linux-migrate-ssis.md).

## <a name="compare-project-deployment-model-and-legacy-package-deployment-model"></a>Vergleich von Projektbereitstellungsmodell und Legacy-Paketbereitstellungsmodells

 Der Typ von Bereitstellungsmodell, den Sie für ein Projekt auswählen, bestimmt, welche Entwicklungs- und Verwaltungsoptionen für dieses Projekt verfügbar sind. In der folgenden Tabelle werden Unterschiede und Ähnlichkeiten der Verwendung des Projektbereitstellungsmodells und des Paketbereitstellungsmodells dargestellt.  
  
|Bei Verwendung des Projektbereitstellungsmodells|Bei Verwendung des Legacy-Paketbereitstellungsmodells|  
|---------------------------------------------|----------------------------------------------------|  
|Ein Projekt stellt die Entwicklungseinheit dar.|Ein Paket ist die Bereitstellungseinheit.|  
|Parameter werden verwendet, um Paketeigenschaften Werte zuzuweisen.|Konfigurationen werden verwendet, um Paketeigenschaften Werte zuzuweisen.|  
|Aus einem Projekt, das Pakete und Parameter enthält, wird eine Projektbereitstellungsdatei (Erweiterung .ISPAC) erstellt.|Pakete (Erweiterung .DTSX) und Konfigurationen (Erweiterung .DTSCONFIG) werden einzeln im Dateisystem gespeichert.|  
|Ein Projekt, das Pakete und Parameter enthält, wird im SSISDB-Katalog auf einer Instanz von SQL Server bereitgestellt.|Pakete und Konfigurationen werden in das Dateisystem auf einem anderen Computer kopiert. Pakete können auch in der MSDB-Datenbank auf einer Instanz von SQL Server gespeichert werden.|  
|Auf der Datenbank-Engine ist die CLR-Integration erforderlich.|Auf der Datenbank-Engine ist die CLR-Integration nicht erforderlich.|  
|Umgebungsspezifische Parameterwerte werden in Umgebungsvariablen gespeichert.|Umgebungsspezifische Konfigurationswerte werden in Konfigurationsdateien gespeichert.|  
|Im Katalog enthaltene Projekte und Pakete können vor der Ausführung auf dem Server überprüft werden. Sie können die Überprüfung mithilfe von SQL Server Management Studio, gespeicherten Prozeduren oder verwaltetem Code ausführen.|Pakete werden unmittelbar vor der Ausführung überprüft. Ein Paket kann auch mit dtExec oder verwaltetem Code überprüft werden.|  
|Pakete werden ausgeführt, indem mit der Datenbank-Engine eine Ausführung gestartet wird. Einer Ausführung werden vor dem Start ein Projektbezeichner, explizite Parameterwerte (optional) und Umgebungsverweise (optional) zugewiesen.<br /><br /> Sie können Pakete mit **dtExec** ausführen.|Pakete werden mit den Ausführungshilfsprogrammen **dtExec** und **DTExecUI** ausgeführt. Anwendbare Konfigurationen werden durch Eingabeaufforderungsargumente (optional) identifiziert.|  
|Während der Ausführung werden Ereignisse, die vom Paket erzeugt werden, automatisch aufgezeichnet und im Katalog gespeichert. Sie können diese Ereignisse mit Transact-SQL-Sichten abfragen.|Während der Ausführung werden Ereignisse, die von einem Paket erzeugt werden, nicht automatisch aufgezeichnet. Dem Paket muss ein Protokollanbieter zum Aufzeichnen von Ereignissen hinzugefügt werden.|  
|Pakete werden in einem separaten Windows-Prozess ausgeführt.|Pakete werden in einem separaten Windows-Prozess ausgeführt.|  
|SQL Server-Agent wird verwendet, um die Paketausführung zu planen.|SQL Server-Agent wird verwendet, um die Paketausführung zu planen.|  


## <a name="features-of-project-deployment-model"></a>Funktionen des Projektbereitstellungsmodells  
 In der folgenden Tabelle sind die Funktionen aufgeführt, die für Projekte, die nur für das Projektbereitstellungsmodell entwickelt wurden, verfügbar sind.  
  
|Funktion|BESCHREIBUNG|  
|-------------|-----------------|  
|Parameter|Ein Parameter gibt die Daten an, die von einem Paket verwendet werden. Sie können die Gültigkeit von Parametern Paketparametern und Projektparametern auf die Paketebene bzw. Projektebene beschränken. Parameter können in Ausdrücken oder Tasks verwendet werden. Wenn das Projekt im Katalog bereitgestellt wird, können Sie einen Literalwert für jeden Parameter zuweisen oder den Standardwert verwenden, der zur Entwurfszeit zugewiesen wurde. Statt eines Literalwerts kann auch auf eine Umgebungsvariable verwiesen werden. Umgebungsvariablenwerte werden zur Laufzeit der Paketausführung aufgelöst.|  
|Umgebungen|Eine Umgebung ist ein Container für Variablen, auf die von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Projekten verwiesen werden kann. Jedes Projekt kann über mehrere Umgebungsverweise verfügen, aber eine einzelne Instanz der Paketausführung kann nur auf Variablen von einer einzelnen Umgebung verweisen. Umgebungen ermöglichen es Ihnen, die Werte zu organisieren, die Sie einem Paket zuweisen. Sie könnten z. B. Umgebungen mit den Namen "Entwicklung", "Test" und "Produktion" definieren.|  
|Umgebungsvariablen|Eine Umgebungsvariable definiert einen Literalwert, der einem Parameter während der Paketausführung zugewiesen werden kann. Um eine Umgebungsvariable zu verwenden, erstellen Sie einen Umgebungsverweis (im Projekt, das der Umgebung entspricht, die über den Parameter verfügt), weisen Sie dem Namen der Umgebungsvariablen einen Parameterwert zu, und geben Sie während der Konfiguration einer Ausführungsinstanz den entsprechenden Umgebungsverweis an.|  
|SSISDB-Katalog|Alle [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Objekte für eine Instanz von SQL Server werden in einer Datenbank, die als SSISDB-Katalog bezeichnet wird, gespeichert und verwaltet. Der Katalog ermöglicht es Ihnen, Projekte und Umgebungen mithilfe von Ordnern zu organisieren. Jede Instanz von SQL Server kann nur einen Katalog besitzen. Jeder Katalog kann 0 (Null) oder mehr Ordner aufweisen. Jeder Ordner kann 0 (Null) oder mehr Projekte und 0 (Null) oder mehr Umgebungen haben. Ein Katalogordner kann auch als Begrenzung für Berechtigungen für [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Objekte verwendet werden.|  
|Im Katalog gespeicherte Prozeduren und Sichten|Eine große Anzahl von gespeicherten Prozeduren und Sichten kann zum Verwalten von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Objekten im Katalog verwendet werden. Zum Beispiel können Sie Werte für Parameter und Umgebungsvariablen angeben, Ausführungen erstellen und starten und Katalogvorgänge überwachen. Sie können sogar vor dem Start der Ausführung genau sehen, welche Werte von einem Paket verwendet werden.|  
  
## <a name="project-deployment"></a>Projektbereitstellung  
 Den Mittelpunkt des Projektbereitstellungsmodells bildet die Projektbereitstellungsdatei (Erweiterung .ISPAC). Die Projektbereitstellungsdatei ist eine in sich geschlossene Bereitstellungseinheit, die nur die wesentlichen Informationen zu den Paketen und Parametern des Projekts umfasst. Die Projektbereitstellungsdatei enthält nicht sämtliche Informationen, die in der Projektdatei von Integration Services (Erweiterung .DTPROJ) enthalten sind. Beispielsweise werden zusätzliche Textdateien, die Sie zum Schreiben von Hinweisen verwenden, nicht in der Projektbereitstellungsdatei gespeichert und daher nicht im Katalog bereitgestellt.  

## <a name="permissions-required-to-deploy-ssis-projects-and-packages"></a>Erforderliche Berechtigungen zum Bereitstellen von SSIS-Projekten und -Paketen

Wenn Sie das SSIS-Standarddienstkonto ändern, müssen Sie möglicherweise zusätzliche Berechtigungen für das vom Standard abweichende Dienstkonto erteilen, bevor Sie erfolgreich Pakete bereitstellen können. Wenn das vom Standard abweichende Dienstkonto nicht über die erforderlichen Berechtigungen verfügt, wird Ihnen möglicherweise folgende Fehlermeldung angezeigt.

`A .NET Framework error occurred during execution of user-defined routine or aggregate "deploy_project_internal":
System.ComponentModel.Win32Exception: A required privilege is not held by the client.`

Dieser Fehler ist in der Regel das Ergebnis fehlender DCOM-Berechtigungen. Gehen Sie folgendermaßen vor, um den Fehler zu beheben:

1.  Öffnen Sie die Konsole **Komponentendienste**, oder führen Sie „Dcomcnfg.exe“ aus.
2.  Erweitern Sie **Komponentendienste** > **Computer** > **Arbeitsplatz** > **DCOM-Konfiguration** in der Konsole **Komponentendienste**.
3.  Suchen Sie in der Liste nach **Microsoft SQL Server Integration Services xx.0** für die Version von SQL Server, die Sie verwenden. SQL Server 2016 ist beispielsweise Version 13.
4.  Klicken Sie mit der rechten Maustaste, und wählen Sie **Eigenschaften** aus.
5.  Klicken Sie im Dialogfeld **Microsoft SQL Server Integration Services 13.0 Properties** (Microsoft SQL Server Integration Services 13.0 – Eigenschaften) auf die Registerkarte **Sicherheit**.
6.  Klicken Sie für jeden der drei Berechtigungssätze (Start und Aktivierung, Zugriff, Konfiguration) auf **Anpassen** und dann auf **Bearbeiten**, um das Dialogfeld **Berechtigung** zu öffnen.
7.  Fügen Sie im Dialogfeld **Berechtigung** das vom Standard abweichende Dienstkonto hinzu, und erteilen Sie bei Bedarf die Berechtigungen **Zulassen**. In der Regel verfügt ein Konto über die Berechtigungen **Lokaler Start** und **Lokale Aktivierung**.
8.  Klicken Sie zweimal auf **OK**, und schließen Sie anschließend die Konsole **Komponentendienste**.

Weitere Informationen zu dem in diesem Abschnitt beschriebenen Fehler und zu den Berechtigungen, die für das SSIS-Dienstkonto erforderlich sind, finden Sie im folgenden Blogbeitrag:
 
- [Fehler beim Bereitstellen des SSIS-Projekts: „System.ComponentModel.Win32Exception: Dem Client fehlt ein erforderliches Recht.“](/archive/blogs/dataaccesstechnologies/system-componentmodel-win32exception-a-required-privilege-is-not-held-by-the-client-while-deploying-ssis-project)

## <a name="deploy-projects-to-integration-services-server"></a>Deploy Projects to Integration Services Server
  In der aktuellen Version von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]können Sie Projekte auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server bereitstellen. Der [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server ermöglicht es Ihnen, Pakete zu verwalten und auszuführen sowie mit Umgebungen Laufzeitwerte für Pakete zu konfigurieren.  
  
> [!NOTE]  
>  Wie in vorherigen Versionen von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]können Sie auch in der aktuellen Version Pakete auf einer SQL Server-Instanz bereitstellen und den [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Dienst zum Ausführen und Verwalten der Pakete verwenden. Sie verwenden das Paketbereitstellungsmodell. Weitere Informationen finden Sie unter [Legacy-Paketbereitstellung &#40;SSIS&#41;](../../integration-services/packages/legacy-package-deployment-ssis.md).  
  
 Um auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server ein Projekt bereitzustellen, führen Sie die folgenden Tasks aus:  
  
1.  Erstellen Sie einen SSISDB-Katalog, falls noch nicht geschehen. Weitere Informationen finden Sie im [SSIS-Katalog](../../integration-services/catalog/ssis-catalog.md).  
  
2.  Konvertieren Sie das Projekt in das Projektbereitstellungsmodell, indem Sie den **Assistenten zum Konvertieren von Integration Services-Projekten** ausführen. Weitere Informationen finden Sie in den folgenden Anweisungen: [So konvertieren Sie ein Projekt in das Projektbereitstellungsmodell](#convert)  
  
    -   Wenn Sie das Projekt in [!INCLUDE[ssISversion12](../../includes/ssisversion12-md.md)] oder höher erstellt haben, verwendet das Projekt standardmäßig das Projektbereitstellungsmodell.  
  
    -   Wenn Sie das Projekt in einer früheren Version von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]erstellt haben, konvertieren Sie das Projekt nach dem Öffnen der Projektdatei in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]in das Projektbereitstellungsmodell.  
  
        > [!NOTE]  
        >  Wenn das Projekt mindestens eine Datenquelle enthält, werden die Datenquellen entfernt, wenn die Projektkonvertierung abgeschlossen wird. Fügen Sie einen Verbindungs-Manager auf Projektebene hinzu, um eine Verbindung mit einer Datenquelle herzustellen, die von den Paketen im Projekt gemeinsam genutzt werden kann. Weitere Informationen finden Sie unter [Hinzufügen, Löschen oder Freigeben eines Verbindungs-Managers in einem Paket](/previous-versions/sql/sql-server-2016/ms140237(v=sql.130))  
  
         Abhängig davon, ob Sie den Assistenten zum Konvertieren von **Integration Services-Projekten** von [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] oder von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]ausführen, führt der Assistent unterschiedliche Konvertierungstasks aus.  
  
        -   Wenn Sie den Assistenten über [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]ausführen, werden die im Projekt enthaltenen Pakete von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 2005, 2008 oder 2008 R2 in das Format konvertiert, das von der aktuellen Version von [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]verwendet wird. Ein Update des ursprünglichen Projekts (.dtproj) und der Paketdateien (.dtsx) wird durchgeführt.  
  
        -   Wenn Sie den Assistenten über [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]ausführen, generiert der Assistent eine Projektbereitstellungsdatei (.ispac) von den Paketen und Konfigurationen, die im Projekt enthalten sind. Ein Update der Originalpaketdateien (.dtsx) wird nicht durchgeführt.  
  
             Sie können eine vorhandene Datei auswählen oder eine neue Datei erstellen (auf der Assistentenseite für das **Auswahlziel**).  
  
             Zur Aktualisierung von Paketdateien bei der Konvertierung eines Projekts führen Sie den **Assistenten für die Konvertierung von Integration Services-Projekten** über [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]aus. Wenn Paketdateien unabhängig von einer Projektkonvertierung aktualisiert werden sollen, führen Sie den Assistenten zum Konvertieren von **Integration Services-Projekten** in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] aus, und führen Sie dann den **SSIS-Paketupgrade-Assistenten** aus. Wenn Sie die Paketdateien getrennt aktualisieren, stellen Sie sicher, dass Sie die Änderungen speichern. Andernfalls werden bei der Konvertierung des Projekts in das Projektbereitstellungsmodell alle nicht gespeicherten Änderungen am Paket nicht konvertiert.  
  
     Weitere Informationen zum Upgraden von Paketen finden Sie unter [Upgraden von Integration Services-Paketen](../../integration-services/install-windows/upgrade-integration-services-packages.md) und [Upgraden von Integration Services-Paketen mit dem SSIS-Paketupgrade-Assistenten](../../integration-services/install-windows/upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard.md).  
  
3.  Stellen Sie das Projekt auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server bereit. Weitere Informationen finden Sie in den folgenden Anweisungen: [So stellen Sie ein Projekt auf dem Integration Services-Server bereit](#deploy)  
  
4.  (Optional) Erstellen Sie eine Umgebung für das bereitgestellte Projekt. 
  
###  <a name="to-convert-a-project-to-the-project-deployment-model"></a><a name="convert"></a> So konvertieren Sie ein Projekt in das Projektbereitstellungsmodell  
  
1.  Öffnen Sie das Projekt in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], und klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt. Klicken Sie anschließend auf **In Projektbereitstellungsmodell konvertieren**.  
  
     - oder -  
  
     Klicken Sie im Objekt-Explorer in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]mit der rechten Maustaste auf den Knoten **Projekte** , und wählen Sie anschließend die Option **Pakete importieren** aus.  
  
2.  Schließen Sie den Assistenten ab.
  
###  <a name="to-deploy-a-project-to-the-integration-services-server"></a><a name="deploy"></a> So stellen Sie ein Projekt auf dem Integration Services-Server bereit  
  
1.  Öffnen Sie das Projekt in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], und wählen Sie dann im Menü **Projekt** die Option **Bereitstellen** aus, um den **Bereitstellungs-Assistent für Integration Services** zu starten.  
  
     oder  
  
     Erweitern Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] den Knoten [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] > **SSISDB** im Objekt-Explorer, und navigieren Sie anschließend zum Projektordner des bereitzustellenden Projekts. Klicken Sie mit der rechten Maustaste auf den Ordner **Projekte** , und klicken Sie anschließend auf **Projekt bereitstellen**.  
  
     oder  
  
     Führen Sie an der Eingabeaufforderung **isdeploymentwizard.exe** unter dem Pfad **%ProgramFiles%\Microsoft SQL Server\130\DTS\Binn** aus. Auf 64-Bit-Computern steht auch eine 32-Bit-Version des Tools unter **%ProgramFiles(x86)%\Microsoft SQL Server\130\DTS\Binn** zur Verfügung.  
  
2.  Klicken Sie auf der Seite **Quelle auswählen** auf **Projektbereitstellungsdatei** , um die Bereitstellungsdatei für das Projekt auszuwählen.  
  
     oder  
  
     Klicken Sie auf **Integration Services-Katalog** , um ein Projekt auszuwählen, das bereits im SSISDB-Katalog bereitgestellt wurde.  
  
3.  Schließen Sie den Assistenten ab. 

## <a name="deploy-packages-to-integration-services-server"></a>Bereitstellen von Paketen auf dem Integration Services-Server
  Mit dem in  [!INCLUDE[ssISversion13](../../includes/ssisversion13-md.md)] eingeführten Feature für inkrementelle Paketbereitstellung können Sie ein oder mehrere Pakete in einem vorhandenen oder neuen Projekt bereitstellen, ohne das gesamte Projekt bereitzustellen.  
  
###  <a name="deploy-packages-by-using-the-integration-services-deployment-wizard"></a><a name="DeployWizard"></a> Bereitstellen von Paketen mit dem Bereitstellungs-Assistenten für Integration Services  
  
1.  Führen Sie an der Eingabeaufforderung **isdeploymentwizard.exe** unter dem Pfad **%ProgramFiles%\Microsoft SQL Server\130\DTS\Binn** aus. Auf 64-Bit-Computern steht auch eine 32-Bit-Version des Tools unter **%ProgramFiles(x86)%\Microsoft SQL Server\130\DTS\Binn** zur Verfügung.  
  
2.  Wechseln Sie auf der Seite **Quelle auswählen** zu **Paketbereitstellungsmodell**. Wählen Sie dann den Ordner aus, der Quellpakete enthält, und konfigurieren Sie die Pakete.  
  
3.  Schließen Sie den Assistenten ab. Führen Sie die restlichen Schritte aus, die in [Paketbereitstellungsmodell](#PackageModel)beschrieben werden.  
  
###  <a name="deploy-packages-by-using-sql-server-management-studio"></a><a name="SSMS"></a> Bereitstellen von Paketen mit SQL Server Management Studio  
  
1.  Erweitern Sie in SQL Server Management Studio im Objekt-Explorer den Knoten **Integration Services-Kataloge** > **SSISDB** .  
  
2.  Klicken Sie mit der rechten Maustaste auf den Ordner **Projekte** , und klicken Sie dann auf **Projekte bereitstellen**.  
  
3.  Wenn die Seite **Einführung** angezeigt wird, klicken Sie auf **Weiter** , um den Vorgang fortzusetzen.  
  
4.  Wechseln Sie auf der Seite **Quelle auswählen** zu **Paketbereitstellungsmodell**. Wählen Sie dann den Ordner aus, der Quellpakete enthält, und konfigurieren Sie die Pakete.  
  
5.  Schließen Sie den Assistenten ab. Führen Sie die restlichen Schritte aus, die in [Paketbereitstellungsmodell](#PackageModel)beschrieben werden.  
  
###  <a name="deploy-packages-by-using-sql-server-data-tools-visual-studio"></a><a name="SSDT"></a> Bereitstellen von Paketen mit SQL Server Data Tools (Visual Studio)  
  
1.  Öffnen Sie in Visual Studio ein Integration Services-Projekt, und wählen Sie ein oder mehrere Pakete aus, die Sie bereitstellen möchten.  
  
2.  Klicken Sie mit der rechten Maustaste, und wählen Sie **Paket bereitstellen** aus. Der Bereitstellung-Assistent wird geöffnet, und in diesem sind die ausgewählten Pakete als Quellpakete konfiguriert.  
  
3.  Schließen Sie den Assistenten ab. Führen Sie die restlichen Schritte aus, die in [Paketbereitstellungsmodell](#PackageModel)beschrieben werden.  
  
###  <a name="deploy-packages-by-using-the-deploy_packages-stored-procedure"></a><a name="StoredProcedure"></a> Bereitstellen von Paketen mithilfe der gespeicherten Prozedur „deploy_packages“  
 Sie können mithilfe der gespeicherten Prozedur **[catalog].[deploy_packages]** ein oder mehrere SSIS-Pakete im SSIS-Katalog bereitstellen. Das folgende Codebeispiel veranschaulicht die Verwendung dieser gespeicherten Prozedur zum Bereitstellen von Paketen auf einem SSIS-Server. Weitere Informationen finden Sie unter [catalog.deploy_packages](../../integration-services/system-stored-procedures/catalog-deploy-packages.md).  
  
```cs
  
private static void Main(string[] args)  
{  
    // Connection string to SSISDB  
    var connectionString = "Data Source=.;Initial Catalog=SSISDB;Integrated Security=True;MultipleActiveResultSets=false";  
  
    using (var sqlConnection = new SqlConnection(connectionString))  
    {  
        sqlConnection.Open();  
  
        var sqlCommand = new SqlCommand  
        {  
            Connection = sqlConnection,  
            CommandType = CommandType.StoredProcedure,  
            CommandText = "[catalog].[deploy_packages]"  
        };  
  
        var packageData = Encoding.UTF8.GetBytes(File.ReadAllText(@"C:\Test\Package.dtsx"));  
  
        // DataTable: name is the package name without extension and package_data is byte array of package.  
        var packageTable = new DataTable();  
        packageTable.Columns.Add("name", typeof(string));  
        packageTable.Columns.Add("package_data", typeof(byte[]));  
        packageTable.Rows.Add("Package", packageData);  
  
        // Set the destination project and folder which is named Folder and Project.  
        sqlCommand.Parameters.Add(new SqlParameter("@folder_name", SqlDbType.NVarChar, ParameterDirection.Input, "Folder", -1));  
        sqlCommand.Parameters.Add(new SqlParameter("@project_name", SqlDbType.NVarChar, ParameterDirection.Input, "Project", -1));  
        sqlCommand.Parameters.Add(new SqlParameter("@packages_table", SqlDbType.Structured, ParameterDirection.Input, packageTable, -1));  
  
        var result = sqlCommand.Parameters.Add("RetVal", SqlDbType.Int);  
        result.Direction = ParameterDirection.ReturnValue;  
  
        sqlCommand.ExecuteNonQuery();  
    }  
}  
  
```  
  
###  <a name="deploy-packages-using-the-management-object-model-api"></a><a name="MOMApi"></a> Bereitstellen von Paketen mit der Management-Object Model-API  
 Das folgende Codebeispiel veranschaulicht, wie mit der Management Object Model-API Pakete auf dem Server bereitgestellt werden.  
  
```cs 
  
static void Main()  
 {  
     // Before deploying packages, make sure the destination project exists in SSISDB.  
     var connectionString = "Data Source=.;Integrated Security=True;MultipleActiveResultSets=false";  
     var catalogName = "SSISDB";  
     var folderName = "Folder";  
     var projectName = "Project";  
  
     // Get the folder instance.  
     var sqlConnection = new SqlConnection(connectionString);  
     var store = new Microsoft.SqlServer.Management.IntegrationServices.IntegrationServices(sqlConnection);  
     var folder = store.Catalogs[catalogName].Folders[folderName];  
  
     // Key is package name without extension and value is package binaries.  
     var packageDict = new Dictionary<string, string>();  
  
     var packageData = File.ReadAllText(@"C:\Folder\Package.dtsx");  
     packageDict.Add("Package", packageData);  
  
     // Deploy package to the destination project.  
     folder.DeployPackages(projectName, packageDict);  
 }  
  
```

## <a name="convert-to-package-deployment-model-dialog-box"></a>In Paketbereitstellungsmodell konvertieren (Dialogfeld)
  Mit dem Befehl **In Paketbereitstellungsmodell konvertieren** können Sie ein Paket auf dem Paketbereitstellungsmodell konvertieren, nachdem das Projekt und jedes Paket im Projekt auf Kompatibilität mit dem Modell überprüft wurden. Wenn ein Paket für ein Projektbereitstellungsmodell eindeutige Funktionen verwendet, z. B. Parameter, dann kann das Paket nicht konvertiert werden.  

 Für das Konvertieren eines Pakets auf ein Paketbereitstellungsmodell sind zwei Schritte erforderlich.  
  
1.  Wenn Sie den Befehl **In Paketbereitstellungsmodell konvertieren** aus dem Menü **Projekt** auswählen, werden das Projekt und jedes Paket auf Kompatibilität mit diesem Modell überprüft. Die Ergebnisse werden in der Tabelle **Ergebnisse** angezeigt.  
  
     Wenn das Projekt oder ein Paket den Kompatibilitätstest nicht besteht, klicken Sie in der Spalte **Ergebnis** auf **Fehler** , um weitere Informationen zu erhalten. Klicken Sie auf **Bericht speichern** , um eine Kopie dieser Informationen in einer Textdatei zu speichern.  
  
2.  Wenn das Projekt und alle Pakete den Kompatibilitätstest bestehen, klicken Sie auf **OK** , um das Paket zu konvertieren.  
  
> [!NOTE]
> Verwenden Sie den **Assistenten für die Konvertierung von Integration Services-Projekten**, um ein Projekt ins Projektbereitstellungsmodell zu konvertieren. Weitere Informationen finden Sie unter [Integration Services Project Conversion Wizard](deploy-integration-services-ssis-projects-and-packages.md).  

## <a name="integration-services-deployment-wizard"></a>Bereitstellungs-Assistent für Integration Services
  Die **Bereitstellungs-Assistent für Integration Services** unterstützt zwei Bereitstellungsmodelle:
   - Projektbereitstellungsmodell
   - Paketbereitstellungsmodell 
   
 Das **Projektbereitstellungsmodell** ermöglicht Ihnen, ein SQL Server Integration Services-Projekt (SSIS) als eine einzelne Einheit im SSIS-Katalog bereitzustellen.
 
 Das **Paketbereitstellungsmodell** ermöglicht Ihnen, aktualisierte Pakete im SSIS-Katalog bereitzustellen, ohne dass Sie das ganze Projekt bereitstellen müssen. 
 
 > [!NOTE]
 > Die Standardbereitstellung für den Assistenten ist das Projektbereitstellungsmodell.  
  
### <a name="launch-the-wizard"></a>Starten des Assistenten
Starten Sie den Assistenten auf eine der folgenden Arten:

 - Eingeben von **SQL Server Bereitstellungs-Assistent** in Windows Search 

 oder

 - Suchen nach der ausführbaren Datei **ISDeploymentWizard.exe** im SQL Server-Installationsordner; Beispiel: „C:\Programme (x86) \Microsoft SQL Server\130\DTS\Binn“. 
 
 > **HINWEIS:** Falls Sie die Seite **Einführung** sehen, klicken Sie auf **Weiter** , um auf die Seite **Quellen auswählen** zu wechseln. 
 
 Die Einstellungen auf dieser Seite unterscheiden sich für jedes Bereitstellungsmodell. Führen Sie die Schritte im Abschnitt [Project Deployment Model](#ProjectModel) oder im Abschnitt [Package Deployment Model](#PackageModel) aus, je nachdem welches Modell Sie auf dieser Seite gewählt haben.  
  
###  <a name="project-deployment-model"></a><a name="ProjectModel"></a> Project Deployment Model  
  
#### <a name="select-source"></a>Auswählen der Quelle

 Um eine von Ihnen erstellte Projektbereitstellungsdatei bereitzustellen, wählen Sie **Projektbereitstellungsdatei** aus, und geben Sie den Pfad für die ISPAC-Datei ein. Um ein Projekt bereitzustellen, das sich im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Katalog befindet, wählen Sie **Integration Services-Katalog** aus und geben dann den Servernamen und den Pfad zum Projekt im Katalog ein. Klicken Sie auf **Weiter** , um die Seite **Ziel auswählen** zu sehen.  
  
#### <a name="select-destination"></a>Ziel auswählen

 Um den Zielordner für das Projekt im [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Katalog auszuwählen, geben Sie die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Instanz ein, oder klicken Sie auf **Durchsuchen** , um aus einer Liste von Servern auszuwählen. Geben Sie den Projektpfad in SSISDB ein, oder klicken Sie auf **Durchsuchen** , um ihn auszuwählen. Klicken Sie auf **Weiter** , um die Seite **Überprüfen** zu sehen.  
  
#### <a name="review-and-deploy"></a>Überprüfen (und Bereitstellen)

 Diese Seite erlaubt Ihnen die Überprüfung der von Ihnen vorgenommenen Einstellungen. Sie können Ihre Auswahl ändern, indem Sie auf **Zurück** klicken oder indem Sie auf einen der Schritte im linken Bereich klicken. Klicken Sie auf **Bereitstellen** , um den Bereitstellungsprozess zu starten.  
  
#### <a name="results"></a>Ergebnisse

 Nachdem der Bereitstellungsvorgang abgeschlossen ist, sollten Sie die Seite **Ergebnisse** sehen. Diese Seite zeigt an, ob die einzelnen Aktionen erfolgreich ausgeführt wurden oder ob Fehler aufgetreten sind. Ist die Aktion fehlerhaft, klicken Sie auf **Fehler** in der Spalte **Ergebnis** , um eine Erklärung über den Fehler anzuzeigen. Klicken Sie auf **Bericht speichern...** , um die Ergebnisse in einer XML-Datei zu speichern, oder klicken Sie auf **Schließen**, um den Assistenten zu beenden.
  
###  <a name="package-deployment-model"></a><a name="PackageModel"></a> Package Deployment Model  
  
#### <a name="select-source"></a>Auswählen der Quelle

 Die Seite **Quelle auswählen** im **Bereitstellungs-Assistenten für Integration Services** zeigt die Einstellungen speziell für das Paketbereitstellungsmodell an, wenn Sie die Option **Paketbereitstellung** als **Bereitstellungsmodell** gewählt haben.  
  
 Klicken Sie zum Auswählen der Quellpakete auf die Schaltfläche **Durchsuchen...**, um den **Ordner** auszuwählen, der die Pakete enthält, oder geben Sie den Ordnerpfad in das Textfeld **Paketordnerpfad** ein, und klicken Sie auf die Schaltfläche **Aktualisieren** am unteren Seitenrand. Jetzt sollten Sie alle Pakete im angegebenen Ordner im Listenfeld sehen. Standardmäßig sind alle Pakete ausgewählt. Klicken Sie das **Kontrollkästchen** in der ersten Spalte, um auszuwählen, welche Pakete an den Server bereitgestellt werden sollen.  
  
 Beziehen Sie sich auf die Spalten **Status** und **Meldung** , um den Status des Pakets zu überprüfen. Falls der Status auf **Bereit** oder **Warnung** steht, würde der Bereitstellungs-Assistent den Bereitstellungsvorgang nicht blockieren. Wenn der Status auf **Fehler** festgelegt ist, setzt der Assistent die Bereitstellung der ausgewählten Pakete nicht fort. Klicken Sie zum Anzeigen der detaillierten Warn- oder Fehlermeldungen auf den Link in der Spalte **Meldung**.  
  
 Falls die sensiblen Daten oder Paketdaten mit einem Kennwort verschlüsselt sind, geben Sie das Kennwort in die Spalte **Kennwort** ein, und klicken Sie auf die Schaltfläche **Aktualisieren** , um zu überprüfen, ob das Kennwort akzeptiert wird. Falls das Kennwort korrekt ist, ändert sich der Status auf **Bereit** , und die Warnmeldung verschwindet. Wenn mehrere Pakete mit dem gleichen Kennwort vorhanden sind, wählen Sie die Pakete mit dem gleichen Verschlüsselungskennwort aus, geben Sie das Kennwort in das Textfeld **Kennwort** ein, und klicken Sie auf die Schaltfläche **Anwenden**. Das Kennwort würde auf die ausgewählten Pakete angewendet werden.  
  
 Falls der Status aller ausgewählten Pakete nicht auf **Fehler** steht, ist die Schaltfläche **Weiter** aktiviert. Sie können also mit dem Paketbereitstellungsvorgang fortfahren.  
  
#### <a name="select-destination"></a>Ziel auswählen

 Nachdem Sie die Paketquellen ausgewählt haben, klicken Sie auf die Schaltfläche **Weiter**, um zur Seite **Ziel auswählen** zu wechseln. Pakete müssen an ein Projekt im SSIS-Katalog (SSISDB) bereitgestellt werden. Stellen Sie vor dem Bereitstellen von Paketen sicher, dass das Zielprojekt bereits im SSIS-Katalog vorhanden ist. Erstellen Sie ein leeres Projekt, falls ein Projekt nicht vorhanden ist. Geben Sie auf der Seite **Ziel auswählen** den Servernamen in das Textfeld **Servername** ein, oder klicken Sie auf die Schaltfläche **Durchsuchen...**, um eine Serverinstanz auszuwählen. Klicken Sie dann auf die Schaltfläche **Durchsuchen...** neben dem Textfeld **Pfad**, um das Zielprojekt anzugeben. Wenn das Projekt nicht vorhanden ist, klicken Sie auf die Schaltfläche **Neues Projekt...**, um ein leeres Projekt als Zielprojekt zu erstellen. Das Projekt muss unter einem Ordner erstellt werden.  
  
#### <a name="review-and-deploy"></a>Überprüfen und bereitstellen

 Klicken Sie auf der Seite **Ziel auswählen** auf **Weiter** , um auf die Seite **Überprüfung** im **Bereitstellungs-Assistenten für Integration Services** zu wechseln. Auf der Seite zur Überprüfung können Sie sich den Bericht zur Bereitstellungsaktion ansehen. Klicken Sie nach der Überprüfung auf die Schaltfläche **Bereitstellen**, um die Bereitstellungsaktion auszuführen.  
  
#### <a name="results"></a>Ergebnisse

 Nachdem die Bereitstellung abgeschlossen ist, sollten Sie die Seite **Ergebnisse** sehen. Überprüfen Sie die Ergebnisse der einzelnen Schritte im Bereitstellungsprozess auf der Seite **Ergebnisse**. Klicken Sie auf **Bericht speichern**, um den Bereitstellungsbericht zu speichern, oder auf **Schließen**, um den Assistenten zu schließen.  

## <a name="create-and-map-a-server-environment"></a>Erstellen und Zuordnen einer Serverumgebung

  Sie erstellen eine Serverumgebung, um Laufzeitwerte für Pakete festzulegen, die in einem auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Server bereitgestellten Projekt enthalten sind. Anschließend können Sie die Umgebungsvariablen Parametern für ein bestimmtes Paket, für Einstiegspunktpakete oder für alle Pakete in einem angegebenen Projekt zuordnen. Ein Einstiegspunktpaket ist in der Regel ein übergeordnetes Paket, von dem ein untergeordnetes Paket ausgeführt wird.  
  
> [!IMPORTANT]  
>  Ein Paket kann jeweils nur mit den Werten ausgeführt werden, die in einer einzelnen Serverumgebung enthalten sind.  
  
 Sie können Sichten nach einer Liste von Serverumgebungen, Umgebungsverweisen und Umgebungsvariablen abfragen. Sie können auch gespeicherte Prozeduren aufrufen, um Umgebungen, Umgebungsverweise und Umgebungsvariablen hinzuzufügen, zu löschen und zu ändern. Weitere Informationen finden Sie im Abschnitt **Serverumgebungen, Servervariablen und Serverumgebungsverweise** im [SSIS-Katalog](../../integration-services/catalog/ssis-catalog.md).  
  
### <a name="to-create-and-use-a-server-environment"></a>So erstellen und verwenden Sie eine Serverumgebung  
  
1.  Erweitern Sie in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] den Knoten [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Kataloge **SSISDB** im Objekt-Explorer, und suchen Sie den Ordner **Umgebungen** des Projekts, für das Sie eine Umgebung erstellen möchten.  
  
2.  Klicken Sie mit der rechten Maustaste auf den Ordner **Umgebungen** , und klicken Sie dann auf **Umgebung erstellen**.  
  
3.  Geben Sie einen Namen für die Umgebung ein, und fügen Sie optional eine Beschreibung hinzu. Klicken Sie auf **OK**.  
  
4.  Klicken Sie mit der rechten Maustaste auf die neue Umgebung, und klicken Sie dann auf **Eigenschaften**.  
  
5.  Gehen Sie auf der Seite **Variablen** wie folgt vor, um eine Variable hinzuzufügen.  
  
    1.  Wählen Sie den **Typ** für die Variable aus. Der Name der Variablen muss nicht mit dem Namen des Projektparameters identisch sein, den Sie der Variablen zuordnen.  
  
    2.  Geben Sie eine optionale **Beschreibung** für die Variable ein.  
  
    3.  Geben Sie den **Wert** für die Umgebungsvariable ein.  
  
         Informationen zu den Benennungsregeln für Umgebungsvariablen finden Sie im Abschnitt **Umgebungsvariable** im [SSIS Catalog](../../integration-services/catalog/ssis-catalog.md).  
  
    4.  Geben Sie an, ob die Variable einen vertraulichen Wert enthält, indem Sie das Kontrollkästchen **Vertraulich** aktivieren oder deaktivieren.  
  
         Bei Auswahl von **Vertraulich** wird der Variablenwert nicht im Feld **Wert** angezeigt.  
  
         Vertrauliche Werte werden im SSISDB-Katalog verschlüsselt. Weitere Informationen zur Verschlüsselung finden Sie unter [SSIS Catalog](../../integration-services/catalog/ssis-catalog.md).  
  
6.  Gehen Sie auf der Seite **Berechtigungen** wie folgt vor, um ausgewählten Benutzern und Rollen Berechtigungen zu erteilen oder zu verweigern.  
  
    1.  Klicken Sie auf **Durchsuchen**, und wählen Sie dann im Dialogfeld **Alle Prinzipale durchsuchen** mindestens einen Benutzer und eine Rolle aus.  
  
    2.  Wählen Sie im Bereich **Anmeldenamen oder Rollen** den Benutzer oder die Rolle aus, dem bzw. der Sie Berechtigungen erteilen oder verweigern möchten.  
  
    3.  Wählen Sie im Bereich **Explizit** neben jeder Berechtigung die Optionen **Erteilen** oder **Verweigern** aus.  
  
7.  Um ein Skript für die Umgebung zu erstellen, klicken Sie auf **Skript**. Das Skript wird standardmäßig in einem neuen Abfrage-Editor-Fenster angezeigt.  
  
    > [!TIP]  
    >  Sie müssen auf **Skript** klicken, nachdem Sie Änderungen an den Umgebungseigenschaften vorgenommen, z. B. eine Variable hinzugefügt, haben und bevor Sie im Dialogfeld **Umgebungseigenschaften** auf **OK** klicken. Andernfalls wird kein Skript generiert.  
  
8.  Klicken Sie auf **OK** , um die Änderungen an den Umgebungseigenschaften zu speichern.  
  
9. Erweitern Sie unter dem Knoten **SSISDB** im Objekt-Explorer den Ordner **Projekte** , klicken Sie mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf **Konfigurieren**.  
  
10. Klicken Sie auf der Seite **Verweise** auf **Hinzufügen** , um eine Umgebung hinzuzufügen, und klicken Sie dann auf **OK** , um den Verweis in der Umgebung zu speichern.  
  
11. Klicken Sie mit der rechten Maustaste erneut auf das Projekt, und klicken Sie dann auf **Konfigurieren**.  
  
12. Gehen Sie folgendermaßen vor, um die Umgebungsvariable einem Parameter zuzuordnen, den Sie dem Paket zur Entwurfszeit hinzugefügt haben, oder einem Parameter, der generiert wurde, als Sie das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Projekt in das Projektbereitstellungsmodell konvertiert haben:
  
    1.  Klicken Sie auf der Seite **Parameter** auf der Registerkarte **Parameter** neben dem Feld **Wert** auf die Schaltfläche zum Durchsuchen.  
  
    2.  Klicken Sie auf **Umgebungsvariable verwenden**, und wählen Sie dann die Umgebungsvariable aus, die Sie erstellt haben.  
  
13. Um die Umgebungsvariable einer Eigenschaft des Verbindungs-Managers zuzuordnen, gehen Sie wie folgt vor. Parameter für die Eigenschaften des Verbindungs-Managers werden automatisch auf dem SSIS-Server generiert.  
  
    1.  Klicken Sie auf der Registerkarte **Verbindungs-Manager** auf der Seite **Parameter** auf die Schaltfläche **Durchsuchen** neben dem Feld **Wert**.  
  
    2.  Klicken Sie auf **Umgebungsvariable verwenden**, und wählen Sie dann die Umgebungsvariable aus, die Sie erstellt haben.  
  
14. Klicken Sie zum Speichern der Änderungen zweimal auf **OK**.  

## <a name="deploy-and-execute-ssis-packages-using-stored-procedures"></a>Bereitstellen und Ausführen von SSIS-Paketen mithilfe von gespeicherten Prozeduren

  Wenn Sie ein [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Projekt für die Verwendung des Projektbereitstellungsmodells konfigurieren, können Sie gespeicherte Prozeduren im [!INCLUDE[ssIS](../../includes/ssis-md.md)] -Katalog verwenden, um das Projekt bereitzustellen und die Pakete auszuführen. Informationen zum Projektbereitstellungsmodell finden Sie unter [Deployment of Projects and Packages](deploy-integration-services-ssis-projects-and-packages.md#compare-project-deployment-model-and-legacy-package-deployment-model).  
  
 Sie können auch [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] zum Bereitstellen des Projekts und zum Ausführen der Pakete verwenden. Weitere Informationen finden Sie in den im Abschnitt **Siehe auch** aufgeführten Themen.  
  
> [!TIP]
>  Die Transact-SQL-Anweisungen für die im nachstehenden Verfahren aufgelisteten gespeicherten Prozeduren – mit Ausnahme von catalog.deploy_project – können problemlos generiert werden, indem Sie folgende Schritte ausführen:
> 
>  1.  Erweitern Sie in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]den Knoten **Integration Services-Kataloge** im Objekt-Explorer und navigieren Sie zu dem Paket, das Sie ausführen möchten.  
> 2.  Klicken Sie mit der rechten Maustaste auf das Paket, und klicken Sie dann auf **Ausführen**.  
> 3.  Legen Sie nach Bedarf Parameterwerte, Verbindungs-Manager-Eigenschaften und Optionen auf der Registerkarte **Erweitert** fest, zum Beispiel den Protokolliergrad.  
> 
>      Weitere Informationen zu Protokolliergraden finden Sie unter [Enable Logging for Package Execution on the SSIS Server](../../integration-services/performance/integration-services-ssis-logging.md#server_logging).  
> 4.  Bevor Sie auf **OK** klicken, um das Paket auszuführen, klicken Sie auf **Skript**. Die Transact-SQL-Anweisung wird in einem Fenster des Abfrage-Editors in [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]angezeigt.  
  
### <a name="to-deploy-and-execute-a-package-using-stored-procedures"></a>So stellen Sie ein Paket mit gespeicherten Prozeduren bereit und führen es aus  
  
1.  Rufen Sie [catalog.deploy_project &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-deploy-project-ssisdb-database.md) auf, um das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Projekt bereitzustellen, das das Paket für den [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Server enthält.  
  
     Verwenden Sie zum Abrufen des binären Inhalts der [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]-Projektbereitstellungsdatei für den _\@project_stream_-Parameter eine SELECT-Anweisung mit der OPENROWSET-Funktion und dem BULK-Rowsetanbieter. Der BULK-Rowsetanbieter ermöglicht es Ihnen, Daten aus einer Datei zu lesen. Das SINGLE_BLOB-Argument für den BULK-Rowsetanbieter gibt den Inhalt der Datendatei als einzeiliges, einspaltiges Rowset vom Typ "varbinary(max)" zurück. Weitere Informationen finden Sie unter [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md).  
  
     Im folgenden Beispiel wird das SSISPackages_ProjectDeployment-Projekt im Ordner „SSIS-Pakete“ auf dem [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Server bereitgestellt. Die Binärdaten werden aus der Projektdatei (SSISPackage_ProjectDeployment.ispac) gelesen und im _\@ProjectBinary-Parameter des Typs „varbinary(max)“ gespeichert. Der Parameterwert _\@ProjectBinary_ wird dem Parameter _\@project_stream_ zugewiesen.  
  
    ```sql
    DECLARE @ProjectBinary as varbinary(max)  
    DECLARE @operation_id as bigint  
    Set @ProjectBinary = (SELECT * FROM OPENROWSET(BULK 'C:\MyProjects\ SSISPackage_ProjectDeployment.ispac', SINGLE_BLOB) as BinaryData)  
  
    Exec catalog.deploy_project @folder_name = 'SSIS Packages', @project_name = 'DeployViaStoredProc_SSIS', @Project_Stream = @ProjectBinary, @operation_id = @operation_id out  
  
    ```  
  
2.  Rufen Sie [catalog.create_execution &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md) auf, um eine Instanz der Paketausführung zu erstellen, und rufen Sie optional [catalog.set_execution_parameter_value &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md) auf, um Laufzeitparameterwerte festzulegen.  
  
     Im folgenden Beispiel erstellt catalog.create_execution eine Ausführungsinstanz für package.dtsx, das im SSISPackage_ProjectDeployment-Projekt enthalten ist. Das Projekt befindet sich im Ordner "SSIS-Pakete". Die von der gespeicherten Prozedur zurückgegebene execution_id wird im Aufruf von catalog.set_execution_parameter_value verwendet. Die zweite gespeicherte Prozedur legt den LOGGING_LEVEL-Parameter auf 3 (ausführliche Protokollierung) und einen Paketparameter namens Parameter1 auf den Wert 1 fest.  
  
     Für Parameter wie LOGGING_LEVEL hat object_type den Wert 50. Für Paketparameter hat object_type den Wert 30.  
  
    ```sql
    Declare @execution_id bigint  
    EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'SSIS Packages', @project_name=N'SSISPackage_ProjectDeployment', @use32bitruntime=False, @reference_id=1  
  
    Select @execution_id  
    DECLARE @var0 smallint = 3  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0  
  
    DECLARE @var1 int = 1  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=30, @parameter_name=N'Parameter1', @parameter_value=@var1  
  
    GO  
  
    ```  
  
3.  Rufen Sie [catalog.start_execution &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md) auf, um das Paket auszuführen.  
  
     Im folgenden Beispiel wird der Transact-SQL-Anweisung ein Aufruf von catalog.start_execution hinzugefügt, um die Paketausführung zu starten. Dabei wird die von der gespeicherten Prozedur catalog.create_execution zurückgegebene execution_id verwendet.  
  
    ```sql
    Declare @execution_id bigint  
    EXEC [SSISDB].[catalog].[create_execution] @package_name=N'Package.dtsx', @execution_id=@execution_id OUTPUT, @folder_name=N'SSIS Packages', @project_name=N'SSISPackage_ProjectDeployment', @use32bitruntime=False, @reference_id=1  
  
    Select @execution_id  
    DECLARE @var0 smallint = 3  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=50, @parameter_name=N'LOGGING_LEVEL', @parameter_value=@var0  
  
    DECLARE @var1 int = 1  
    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @execution_id,  @object_type=30, @parameter_name=N'Parameter1', @parameter_value=@var1  
  
    EXEC [SSISDB].[catalog].[start_execution] @execution_id  
    GO  
  
    ```  
  
### <a name="to-deploy-a-project-from-server-to-server-using-stored-procedures"></a>So stellen Sie ein Projekt mithilfe von gespeicherten Prozeduren zwischen Servern bereit

 Sie können mit den gespeicherten Prozeduren [catalog.get_project &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-get-project-ssisdb-database.md) und [catalog.deploy_project &#40;SSISDB-Datenbank&#41;](../../integration-services/system-stored-procedures/catalog-deploy-project-ssisdb-database.md) ein Projekt von Server zu Server bereitstellen.  
  
 Bevor Sie die gespeicherten Prozeduren ausführen, müssen Sie die folgenden Schritte ausführen.  
  
-   Erstellen Sie ein Verbindungsserverobjekt. Weitere Informationen finden Sie unter [Erstellen von Verbindungsservern &#40;SQL Server-Datenbank-Engine&#41;](../../relational-databases/linked-servers/create-linked-servers-sql-server-database-engine.md).  
  
     Legen Sie auf der Seite **Serveroptionen** des Dialogfelds **Eigenschaften des Verbindungsservers** die Optionen **RPC** und **RPC-Ausgabe** auf **True** fest. Legen Sie außerdem **Höherstufung von verteilten Transaktionen für RPC aktivieren** auf **False** fest.  
  
-   Aktivieren Sie dynamische Parameter für den Anbieter, den Sie für den Verbindungsserver ausgewählt haben, indem Sie im Objekt-Explorer unter **Verbindungsserver** den Knoten **Anbieter** erweitern, mit der rechten Maustaste auf den Anbieter klicken und dann auf **Eigenschaften** klicken. Wählen Sie **Aktivieren** neben **Dynamischer Parameter** aus.  
  
-   Überprüfen Sie, ob der Distributed Transaction Coordinator (DTC) auf beiden Servern gestartet ist.  
  
 Rufen Sie catalog.get_project auf, um die Binärdatei für das Projekt zurückzugeben, und rufen Sie dann catalog.deploy_project auf. Der von catalog.get_project zurückgegebene Wert wird in eine Tabellenvariable des Typs "varbinary(max)" eingefügt. Der Verbindungsserver kann keine Ergebnisse vom Typ „varbinary(max)“ zurückgeben.  
  
 Im folgenden Beispiel gibt catalog.get_project eine Binärdatei für das SSISPackages-Projekt auf dem Verbindungsserver zurück. Das catalog.deploy_project stellt das Projekt auf dem lokalen Server im Ordner DestFolder bereit.  
  
```sql
declare @resultsTableVar table (  
project_binary varbinary(max)  
)  
  
INSERT @resultsTableVar (project_binary)  
EXECUTE [MyLinkedServer].[SSISDB].[catalog].[get_project] 'Packages', 'SSISPackages'  
  
declare @project_binary varbinary(max)  
select @project_binary = project_binary from @resultsTableVar  
  
exec [SSISDB].[CATALOG].[deploy_project] 'DestFolder', 'SSISPackages', @project_binary  
  
```  

## <a name="integration-services-project-conversion-wizard"></a>Assistent für die Konvertierung von Integration Services-Projekten
  Der Assistent für die Konvertierung von **Integration Services-Projekten** konvertiert ein Projekt ins Projektbereitstellungsmodell.  
  
> [!NOTE]  
>  Wenn das Projekt mindestens eine Datenquelle enthält, werden die Datenquellen entfernt, wenn die Projektkonvertierung abgeschlossen wird. Fügen Sie einen Verbindungs-Manager auf Projektebene hinzu, um eine Verbindung mit einer Datenquelle herzustellen, die von den Paketen im Projekt gemeinsam genutzt werden kann. Weitere Informationen finden Sie unter [Hinzufügen, Löschen oder Freigeben eines Verbindungs-Managers in einem Paket](/previous-versions/sql/sql-server-2016/ms140237(v=sql.130))  
  
 **Was möchten Sie tun?**  
  
-   [Öffnen des Assistenten für die Konvertierung von Integration Services-Projekten](#open_dialog)  
  
-   [Festlegen von Optionen auf der Seite "Pakete suchen"](#locate)  
  
-   [Festlegen von Optionen auf der Seite "Pakete auswählen"](#selectPackages)  
  
-   [Festlegen von Optionen auf der Seite "Ziel auswählen"](#destination)  
  
-   [Festlegen von Optionen auf der Seite "Projekteigenschaften angeben"](#projectProperties)  
  
-   [Festlegen von Optionen auf der Seite "Task 'Paket ausführen' aktualisieren"](#executePackage)  
  
-   [Festlegen von Optionen auf der Seite "Konfigurationen auswählen"](#configurations)  
  
-   [Festlegen von Optionen auf der Seite "Parameter erstellen"](#createParameters)  
  
-   [Festlegen von Optionen auf der Seite "Parameter konfigurieren"](#configureParameters)  
  
-   [Festlegen der Optionen auf der Seite zum Überprüfen](#review)  
  
-   [Festlegen der Optionen unter "Konvertierung ausführen"](#conversion)  
  
###  <a name="open-the-integration-services-project-conversion-wizard"></a><a name="open_dialog"></a> Öffnen des Assistenten für die Konvertierung von Integration Services-Projekten  
 Führen Sie einen der folgenden Schritte aus, um den Assistenten zum Konvertieren von **Integration Services-Projekten** zu öffnen.  
  
-   Öffnen Sie das Projekt in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], und klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt. Klicken Sie anschließend auf **In Projektbereitstellungsmodell konvertieren**.  
  
-   Klicken Sie im Objekt-Explorer in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] mit der rechten Maustaste auf den Knoten **Projekte** im **Integration Services-Katalog**, und wählen Sie **Pakete importieren** aus.  
  
 Abhängig davon, ob Sie den Assistenten zum Konvertieren von **Integration Services-Projekten** von [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] oder von [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]ausführen, führt der Assistent unterschiedliche Konvertierungstasks aus.   
  
###  <a name="set-options-on-the-locate-packages-page"></a><a name="locate"></a> Festlegen von Optionen auf der Seite "Pakete suchen"  
  
> [!NOTE]  
>   Die Seite **Pakete suchen** ist nur verfügbar, wenn Sie den Assistenten über [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]ausführen.  
  
 Die folgende Option wird auf der Seite angezeigt, wenn Sie **Dateisystem** in der Dropdownliste **Quelle** auswählen. Wählen Sie diese Option, wenn das Paket im Dateisystem gespeichert ist.  
  
 **Ordner**  
 Geben Sie den Paketpfad ein, oder navigieren Sie zum Paket, indem Sie auf **Durchsuchen** klicken.  
  
 Die folgenden Optionen werden auf der Seite angezeigt, wenn Sie **SSIS-Paketspeicher** in der Dropdownliste **Quelle** auswählen. Weitere Informationen zum Paketspeicher finden Sie unter [Paketverwaltung &#40;SSIS-Dienst&#41;](../../integration-services/service/package-management-ssis-service.md).  
  
 **Server**  
 Geben Sie einen Servernamen ein, oder wählen Sie den Server aus.  
  
 **Ordner**  
 Geben Sie den Paketpfad ein, oder navigieren Sie zum Paket, indem Sie auf **Durchsuchen** klicken.  
  
 Die folgenden Optionen werden auf der Seite angezeigt, wenn Sie **Microsoft SQL Server** in der Dropdownliste **Quelle** auswählen. Wählen Sie diese Option aus, wenn das Paket in Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]gespeichert ist.  
  
 **Server**  
 Geben Sie einen Servernamen ein, oder wählen Sie den Server aus.  
  
 **Windows-Authentifizierung verwenden**  
 Der Microsoft Windows-Authentifizierungsmodus ermöglicht Benutzern das Herstellen einer Verbindung über ein Windows-Benutzerkonto. Wenn Sie die Windows-Authentifizierung verwenden, müssen Sie keinen Benutzernamen und kein Kennwort angeben.  
  
 **SQL Server-Authentifizierung verwenden**  
 Wenn ein Benutzer eine Verbindung mit einem angegebenen Benutzernamen und einem Kennwort von einer nicht vertrauenswürdigen Verbindung herstellt, authentifiziert [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] die Verbindung, indem überprüft wird, ob ein [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Anmeldekonto eingerichtet wurde und ob das angegebene Kennwort mit dem zuvor aufgezeichneten übereinstimmt. Wenn kein Anmeldekonto in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] eingerichtet wurde, schlägt die Authentifizierung fehl, und der Benutzer erhält eine Fehlermeldung.  
  
 **Benutzername**  
 Geben Sie einen Benutzernamen an, wenn Sie die SQL Server-Authentifizierung verwenden.  
  
 **Kennwort**  
 Geben Sie das Kennwort ein, wenn Sie die SQL Server-Authentifizierung verwenden.  
  
 **Ordner**  
 Geben Sie den Paketpfad ein, oder navigieren Sie zum Paket, indem Sie auf **Durchsuchen** klicken.  
  
###  <a name="set-options-on-the-select-packages-page"></a><a name="selectPackages"></a> Festlegen von Optionen auf der Seite "Pakete auswählen"  
 **Paketname**  
 Listet die Paketdatei auf.  
  
 **Status**  
 Gibt an, ob ein Paket bereit ist, in das Projektbereitstellungsmodell konvertiert zu werden.  
  
 **Meldung**  
 Zeigt eine Meldung an, die dem Paket zugeordnet sind.  
  
 **Kennwort**  
 Zeigt ein Kennwort an, das dem Paket zugeordnet sind. Der Kennworttext ist ausgeblendet.  
  
 **Auf Auswahl anwenden**  
 Klicken Sie, um das Kennwort im Textfeld **Kennwort** auf das bzw. die ausgewählten Pakete anzuwenden.  
  
 **Aktualisieren**  
 Aktualisiert die Liste der Pakete.  
  
###  <a name="set-options-on-the-select-destination-page"></a><a name="destination"></a> Festlegen von Optionen auf der Seite "Ziel auswählen"  
 Geben Sie auf dieser Seite den Namen und den Pfad für eine neue Projektbereitstellungsdatei (.ispac) an, oder wählen Sie eine vorhandene Datei aus.  
  
> [!NOTE]  
>   Die Seite **Ziel auswählen** ist nur verfügbar, wenn Sie den Assistenten über [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]ausführen.  
  
 **Ausgabepfad**  
 Geben Sie den Pfad für die Bereitstellungsdatei ein, oder navigieren Sie zur Datei, indem Sie auf **Durchsuchen** klicken.  
  
 **Projektname**  
 Geben Sie den Projektnamen ein.  
  
 **Schutzebene**  
 Wählen Sie die Schutzebene aus. Weitere Informationen finden Sie unter [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
 **Projektbeschreibung**  
 Geben Sie eine optionale Beschreibung für das Projekt ein.  
  
###  <a name="set-options-on-the-specify-project-properties-page"></a><a name="projectProperties"></a> Festlegen von Optionen auf der Seite "Projekteigenschaften angeben"  
  
> [!NOTE]  
>   Die Seite **Projekteigenschaften angeben** ist nur verfügbar, wenn Sie den Assistenten über [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]ausführen.  
  
 **Projektname**  
 Listet den Projektnamen auf.  
  
 **Schutzebene**  
 Wählen Sie eine Schutzebene für die im Projekt enthaltenen Pakete aus. Weitere Informationen zu Schutzebenen finden Sie unter [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
 **Projektbeschreibung**  
 Geben Sie eine optionale Beschreibung für das Projekt ein.  
  
###  <a name="set-options-on-the-update-execute-package-task-page"></a><a name="executePackage"></a> Festlegen von Optionen auf der Seite "Task 'Paket ausführen' aktualisieren"  
 Aktualisieren Sie die in den Paketen enthaltenen Tasks "Paket ausführen", um einen projektbasierten Verweis zu verwenden. Weitere Informationen finden Sie unter [Execute Package Task Editor](../control-flow/execute-package-task.md).  
  
 **Übergeordnetes Paket**  
 Listet den Namen eines Pakets auf, das ein untergeordnetes Paket mithilfe des Tasks "Paket ausführen" ausführt.  
  
 **Aufgabenname**  
 Listet den Namen des Tasks "Paket ausführen" auf.  
  
 **Ursprünglicher Verweis**  
 Listet den aktuellen Pfad des untergeordneten Pakets auf.  
  
 **Verweis zuweisen**  
 Wählen Sie ein untergeordnetes im Projekt gespeichertes Paket aus.  
  
###  <a name="set-options-on-the-select-configurations-page"></a><a name="configurations"></a> Festlegen von Optionen auf der Seite "Konfigurationen auswählen"  
 Wählen Sie die Paketkonfigurationen aus, die Sie durch Parameter ersetzen möchten.  
  
 **Pakete**  
 Listet die Paketdatei auf.  
  
 **Type**  
 Listet den Typ der Konfiguration auf, z. B. eine XML-Konfigurationsdatei.  
  
 **Konfigurationszeichenfolge**  
 Listet den Pfad der Konfigurationsdatei auf.  
  
 **Status**  
 Zeigt eine Statusmeldung für die Konfiguration an. Klicken Sie auf die Meldung, um den gesamten Meldungstext anzuzeigen.  
  
 **Hinzufügen von Konfigurationen**  
 Fügen Sie in anderen Projekten enthaltene Paketkonfigurationen der Liste mit verfügbaren Konfigurationen hinzu, die Sie durch Parameter ersetzen möchten. Sie können in einem Dateisystem oder in SQL Server gespeicherte Konfigurationen auswählen.  
  
 **Aktualisieren**  
 Klicken Sie auf die Option, um die Liste der Konfigurationen zu aktualisieren.  
  
 **Option zum Entfernen der Konfigurationen von allen Paketen nach der Konvertierung**  
 Es wird empfohlen, durch Aktivierung dieser Option alle Konfigurationen vom Projekt zu entfernen.  
  
 Wenn Sie diese Option nicht auswählen, werden nur die Konfigurationen entfernt, die durch Parameter ersetzt werden sollen.  
  
###  <a name="set-options-on-the-create-parameters-page"></a><a name="createParameters"></a> Festlegen von Optionen auf der Seite "Parameter erstellen"  
 Wählen Sie den Parameternamen und den Bereich für jede Konfigurationseigenschaft aus.  
  
 **Pakete**  
 Listet die Paketdatei auf.  
  
 **Parametername**  
 Listet den Namen des Parameters auf.  
  
 **Umfang**  
 Wählen Sie den Bereich des Parameters aus, und zwar entweder Paket oder Projekt.  
  
###  <a name="set-options-on-the-configure-parameters-page"></a><a name="configureParameters"></a> Festlegen von Optionen auf der Seite "Parameter konfigurieren"  
 **Name**  
 Listet den Namen des Parameters auf.  
  
 **Umfang**  
 Listet den Bereich des Parameters auf.  
  
 **Wert**  
 Listet den Parameterwert auf.  
  
 Klicken Sie auf die Auslassungspunkte, die sich neben dem Wertefeld befinden, um die Parametereigenschaften zu konfigurieren.  
  
 Im Dialogfeld **Parameterdetails festlegen** können Sie den Parameterwert bearbeiten. Sie können auch angeben, ob der Parameterwert bereitgestellt werden muss, wenn Sie das Paket ausführen.  
  
 Sie können den Wert auf der Seite **Parameter** des Dialogfelds **Konfigurieren** in [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]ändern, indem Sie neben dem Parameter auf die Schaltfläche zum Durchsuchen klicken. Das Dialogfeld **Parameterwert festlegen** wird angezeigt.  
  
 Das Dialogfeld **Parameterdetails festlegen** listet auch den Datentyp des Parameterwerts und den Ursprung des Parameters auf.  
  
###  <a name="set-the-options-on-the-review-page"></a><a name="review"></a> Festlegen der Optionen auf der Seite zum Überprüfen  
 Verwenden Sie die Seite für die **Überprüfung**, um die Optionen zu bestätigen, die Sie für die Konvertierung des Projekts ausgewählt haben.  
  
 **Previous**  
 Klicken Sie, um eine Option zu ändern.  
  
 **Konvertieren**  
 Klicken Sie, um das Projekt in das Projektbereitstellungsmodell zu konvertieren.  
  
###  <a name="set-the-options-on-the-perform-conversion"></a><a name="conversion"></a> Festlegen der Optionen unter "Konvertierung ausführen"  
 Die Seite "Konvertierung ausführen" zeigt den Status der Projektkonvertierung an.  
  
 **Aktion**  
 Listet einen bestimmten Konvertierungsschritt auf.  
  
 **Ergebnis**  
 Listet die Status sämtlicher Konvertierungsschritte auf. Klicken Sie auf die Statusmeldung, um weitere Informationen zu erhalten.  
  
 Die Projektkonvertierung wird erst gespeichert, wenn das Projekt in [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]gespeichert wird.  
  
 **Bericht speichern**  
 Klicken Sie, um in einer XML-Datei eine Zusammenfassung der Projektkonvertierung zu speichern.