---
title: Was sind erweiterte Sicherheitsupdates?
description: Erfahren Sie, wie Sie die SQL Server-Registrierung verwenden, um erweiterte Sicherheitsupdates für Ihre SQL Server-Produkte, deren Support und Lebensdauer ausläuft, wie z. B. SQL Server 2008 und SQL Server 2008 R2, zu erhalten.
ms.custom: ''
ms.date: 11/24/2020
ms.prod: sql
ms.technology: install
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.reviewer: pmasl
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 0f97fcf001db6fa561d54b3a9df6642f2aa148ea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346177"
---
# <a name="what-are-extended-security-updates-for-sql-server"></a>Was sind erweiterte Sicherheitsupdates für SQL Server?
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Dieser Artikel bietet Informationen zur Verwendung des SQL Server-Registrierungsdiensts, um erweiterte Sicherheitsupdates für [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] und [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] zu erhalten. Weitere Informationen zu anderen Optionen bei Ende des Supports finden Sie unter [Optionen bei Ende des Supports](sql-server-end-of-life-overview.md). 

## <a name="overview"></a>Übersicht
Sobald [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] das Ende seines Supportlebenszyklus erreicht hat, haben Sie die Möglichkeit, ein Abonnement für ein erweitertes Sicherheitsupdate (ESU) für Ihre Server abzuschließen und bis zu drei Jahre lang geschützt zu bleiben, bis Sie bereit sind, auf eine neuere Version von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu aktualisieren oder zu [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] zu migrieren. Dieses Abonnement ist auf zwei Arten verfügbar:
-  Sie können es für lokale oder gehostete Umgebungsserver kaufen.
-  Sie erhalten es standardmäßig kostenlos (und aktiviert), wenn Sie lokale Server in Azure Virtual Machines migrieren. Sie können anschließend mithilfe des **SQL Server-Registrierungsdiensts** im Azure-Portal Ihre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz mit abgelaufenem Support registrieren und Updates herunterladen, sobald diese verfügbar sind. 

Microsoft empfiehlt, ESU-Patches anzuwenden, sobald sie verfügbar sind, um Ihre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz zu schützen. Detaillierte Informationen zu ESUs finden Sie auf der [Seite mit häufig gestellten Fragen zu ESUs](https://www.microsoft.com/cloud-platform/extended-security-updates).

> [!IMPORTANT]
> [Der erweiterte Support für SQL Server 2008 und 2008 R2 wurde am 10. Juli 2019 eingestellt](https://www.microsoft.com/cloud-platform/windows-sql-server-2008). Erwägen Sie für diese Versionen den Einsatz der in diesem Artikel beschriebenen erweiterten Sicherheitsupdates oder andere Migrationsoptionen. Weitere Informationen finden Sie unter [Optionen bei Ende des Supports](sql-server-end-of-life-overview.md).

## <a name="what-are-extended-security-updates"></a>Was sind erweiterte Sicherheitsupdates?
Erweiterte Sicherheitsupdates (ESUs) für [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] und [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] umfassen die Bereitstellung von Sicherheitsupdates für Kunden, die ein Abonnement für erweiterte Supportupdates erworben haben.

ESUs werden **bei Bedarf** zur Verfügung gestellt, sobald ein Sicherheitsrisiko entdeckt und vom [Microsoft Security Response Center (MSRC)](https://portal.msrc.microsoft.com) als **Kritisch** eingestuft wird. Daher gibt es kein regelmäßiges Veröffentlichungsintervall für ESUs für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

ESUs bieten Folgendes nicht:
- Neue Funktionen
- Funktionale Verbesserungen
- Von Kunden angeforderte Korrekturen

### <a name="support"></a>Support
ESUs sehen keinen technischen Support vor. Sie können jedoch einen aktiven Supportvertrag wie [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?activetab=software-assurance-default-pivot%3aprimaryr3) oder Premier/Unified Support für [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] / [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] nutzen, um technischen Support für die von ESUs abgedeckten Workloads zu erhalten, wenn Sie sich dafür entscheiden, weiterhin lokal zu arbeiten. Bei Hosting in Azure können Sie alternativ einen Azure-Supportplan nutzen, um technischen Support zu erhalten. 

  > [!NOTE]
  > Microsoft kann ohne ESU-Abonnement (sowohl in lokalen als auch in Hostingumgebungen) keinen technischen Support für [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]- und [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)]-Instanzen leisten. 

## <a name="esu-availability-and-deployment"></a>Verfügbarkeit und Bereitstellung von ESUs
ESUs sind für Kunden verfügbar, die ihre Workload in Azure, lokalen oder gehosteten Umgebungen ausführen.

### <a name="azure-virtual-machines"></a>Azure Virtual Machines
Wenn Sie Ihre Workloads zu Azure Virtual Machines (IaaS) migrieren, haben Sie bis zu drei Jahre nach Ende des Supports Zugriff auf erweiterte Sicherheitsupdates für [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] und [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)], und zwar **ohne zusätzliche Kosten** über die für den Betrieb des virtuellen Computers hinaus. Kunden benötigen nicht Software Assurance, um erweiterte Sicherheitsupdates in Azure zu erhalten. 

Azure-VMs auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unter **Windows Server 2008 R2 und höher** erhält automatisch ESUs über die vorhandenen Updatekanäle von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], wenn der virtuelle Computer für [automatisierte Patches](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching) konfiguriert ist.

Für Azure-VMs auf [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unter **Windows Server 2008** oder VMs, die **_nicht_ für [automatisierte Patches](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)** konfiguriert wurden, müssen Sie ESU-Patches wie im Abschnitt [Lokale oder gehostete Umgebungen](#on-premises-or-hosted-environments) beschrieben manuell herunterladen und bereitstellen.

### <a name="on-premises-or-hosted-environments"></a>Lokale oder gehostete Umgebungen
Wenn Sie über Software Assurance verfügen, können Sie im Rahmen eines Enterprise Agreement (EA), Enterprise Subscription Agreement (EAS), Server & Cloud Enrollment (SCE) oder Enrollment for Education Solutions (EES) ein ESU-Abonnement (Erweiterte Sicherheitsupdates) für bis zu drei Jahre nach Ablauf des Supportzeitraums erwerben. Sie können ESUs nur für die Server erwerben, die Sie schützen müssen. ESUs können direkt bei Microsoft oder einem Microsoft-Lizenzierungspartner erworben werden. 

Kunden mit ESU-Vereinbarungen müssen folgende Schritte durchführen, um einen ESU-Patch herunterzuladen und bereitzustellen:
-  [Registrieren Sie zulässige Instanzen](#register-instances-for-esus) mit der **[SQL Server-Registrierung](#create-sql-server-registry)** . 
-  Wenn nach der Registrierung ESU-Patches veröffentlicht werden, können Sie das Paket über einen Downloadlink im Azure-Portal herunterladen. 
-  Das heruntergeladene Paket kann in Ihrer lokalen oder gehosteten Umgebung manuell oder über eine beliebige Lösung für die Updateorchestrierung in Ihrer Organisation bereitgestellt werden, z. B. Microsoft Endpoint Configuration Manager (ehemals System Center Configuration Manager). 

> [!NOTE]
> Dies ist auch die Vorgehensweise, die Kunden für Azure Stack und Azure Virtual Machines befolgen müssen, die nicht den Empfang automatischer Updates konfiguriert sind.

Weitere Informationen finden Sie unter [Häufig gestellte Fragen zu erweiterten Sicherheitsupdates](https://www.microsoft.com/cloud-platform/extended-security-updates). 

## <a name="create-sql-server-registry"></a>Erstellen der SQL Server-Registrierung
Um Ihre ESU-fähigen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanzen zu registrieren, müssen Sie zunächst im Azure-Portal die SQL Server-Registrierung erstellen. 

> [!IMPORTANT]
> Es ist nicht notwendig, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanzen für ESUs zu registrieren, wenn eine Azure-VM ausgeführt wird, die für [automatische Updates](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching) konfiguriert ist. 

Führen Sie die folgenden Schritte aus, um die SQL Server-Registrierung zu erstellen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
1. Aktivieren Sie die Option zum **Erstellen einer Ressource**. 
1. Geben Sie im Suchfeld als Suchbegriff `SQL Server registry` ein.  
1. Wählen Sie die von [!INCLUDE[msCoName](../../includes/msconame-md.md)] veröffentlichte Option **SQL Server-Registrierung** und dann **Erstellen** aus. 

   ![Screenshot des Azure-Portals, in dem gezeigt wird, wie eine SQL Server Registrierung erstellt wird](media/sql-server-extended-security-updates/sql-server-registry-service.png)

1. Wählen Sie unter **Projektdetails** in der Dropdownliste Ihr Abonnement aus. Wählen Sie dann entweder eine vorhandene **Ressourcengruppe** oder **Neu erstellen**, um für Ihren neuen SQL Server-Registrierungsdienst eine neue Ressourcengruppe zu erstellen. 
1. Geben Sie unter **Dienstdetails** einen Namen und eine Region für Ihre neue Ressource des Typs **SQL Server-Registrierung** an: 

   ![Screenshot der SQL Server-Registrierung mit der Registerkarte „Grundeinstellungen“](media/sql-server-extended-security-updates/create-new-sql-server-registry.png)

1. Wählen Sie **Überprüfen und erstellen** aus, um die Details Ihrer **SQL Server-Registrierung** zu überprüfen. Wählen Sie nach erfolgreicher Überprüfung **Erstellen** aus. 

## <a name="register-instances-for-esus"></a>Registrieren von Instanzen für ESUs

Nach der Bereitstellung der Ressource **SQL Server-Registrierung** können Sie wählen, ob Sie eine [einzelne](#single-sql-server-instance) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz oder mehrere Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in einem [Massenvorgang](#multiple-sql-server-instances-in-bulk) registrieren möchten. Es ist erforderlich, dass mindestens eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz im Geltungsbereich Ihrer SQL Server-Registrierung registriert ist, um ESU-Pakete herunterzuladen. 

### <a name="single-sql-server-instance"></a>Einzelne SQL Server-Instanz

Führen Sie die folgenden Schritte aus, um eine einzelne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz zu registrieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
1. Wechseln Sie zu Ihrer Ressource **SQL Server-Registrierung**. 
1. Wählen Sie im Bereich **Übersicht** die Option **+ Registrieren** aus: 

   ![Wählen von „Registrieren“ zum Registrieren einer einzelnen Instanz von SQL Server](media/sql-server-extended-security-updates/register-single-sql-server-instance.png)

1. Geben Sie die gemäß dieser Tabelle erforderlichen Informationen ein, und wählen Sie dann **Registrieren** aus: 

   |**Wert**| **Beschreibung**|
   | :-------| :------------- |
   | **Instanz** | Geben Sie die Ausgabe des Befehls `SELECT @@SERVERNAME`ein, z. B. `MyServer\Instance01`. | 
   | **SQL Server-Version** | Wählen Sie in der Dropdownliste „2008“ oder „2008 R2“ aus. | 
   | **Edition** | Wählen Sie in der Dropdownliste die gewünschte Edition aus: Datacenter, Developer (bei Kauf von ESUs kostenlos bereitstellbar), Enterprise, Standard, Web, Workgroup. | 
   | **Kerne** | Geben Sie die Anzahl der Kerne für diese Instanz ein. | 
   | **Hosttyp** | Wählen Sie in der Dropdownliste den gewünschten Hosttyp aus: Virtueller Computer (lokal), physischer Server (lokal), Azure Virtual Machine, Amazon EC2, Google Compute Engine, Anderer. |
   | **Abonnement-ID**<sup>1</sup> | Geben Sie die ID des Abonnements ein, in dem die VM erstellt wird.  |
   | **Ressourcengruppe**<sup>1</sup> | Geben Sie die Ressourcengruppe ein, in der die VM erstellt wird.  | 
   | **Name der Azure-VM**<sup>1</sup>  | Geben Sie den Ressourcennamen der VM ein.  | 
   | **Azure-VM-Betriebssystem**<sup>1</sup> | Wählen Sie in der Dropdownliste die gewünschte Version des Windows Server-Betriebssystems aus. | 

   <sup>1</sup> Nur für virtuelle Azure-Computer erforderlich. 

Die neu registrierte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz ist jetzt im Abschnitt **SQL Server-Instanzen registrieren** des Fensters **Übersicht** zu sehen: 

![Registrierte SQL Server-Instanzen](media/sql-server-extended-security-updates/registered-sql-instance.png)

Nachdem eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz registriert wurde, wird der Abschnitt **Sicherheitsupdates** verfügbar. Alle verfügbaren ESUs werden hier bereitgestellt. 

### <a name="multiple-sql-server-instances-in-bulk"></a>Mehrere SQL Server-Instanzen in einem Massenvorgang

Mehrere [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanzen können in einem Massenvorgang registriert werden, indem eine CSV-Datei hochgeladen wird. Nachdem Ihre [CSV-Datei ordnungsgemäß formatiert wurde](#formatting-requirements-for-csv-file), können Sie die folgenden Schritte ausführen, um Ihre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanzen bei der Ressource „SQL Server-Registrierung“ in einem Massenvorgang zu registrieren: 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
1. Wechseln Sie zu Ihrer Ressource **SQL Server-Registrierung**. 
1. Wählen Sie im Bereich **Übersicht** die Option **Massenregistrierung** aus:  

   ![Wählen von „Massenregistrierung“ zum Registrieren mehrerer Instanzen von SQL Server](media/sql-server-extended-security-updates/bulk-register-sql-server-instances.png)

1. Wählen Sie das Dateisymbol aus, um zum Speicherort Ihrer CSV-Datei zu navigieren. Wählen Sie die CSV-Datei aus. Wählen Sie dann **Registrieren** aus, um die Datei hochzuladen und mehrere Instanzen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] zu registrieren. 

   ![Hochladen einer CSV-Datei zur Registrierung mehrerer Instanzen von SQL Server](media/sql-server-extended-security-updates/upload-csv-file-for-bulk-registration.png)

### <a name="formatting-requirements-for-csv-file"></a>Formatierungsanforderungen für die CSV-Datei
- Werte sind durch Kommas getrennt.
- Werte sind nicht in einfache oder doppelte Anführungszeichen gesetzt.
- Spaltennamen müssen Groß-/Kleinschreibung nicht beachten, aber wie folgt **benannt** sein: 
  - name
  - version
  - Edition
  - cores
  - hostType
  - subscriptionID<sup>1</sup>
  - resourceGroup<sup>1</sup>
  - azureVmName<sup>1</sup>
  - AzureVmOS<sup>1</sup>

<sup>1</sup> Nur für Azure Virtual Machines erforderlich. 

#### <a name="csv-example-1---on-premises"></a>CSV-Beispiel 1: lokal

Für lokale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanzen muss die CSV-Datei wie folgt aussehen: 

```csv
name,version,edition,cores,hostType
Server1\SQL2008,2008,Enterprise,12,Physical Server
Server1\SQL2008R2,2008 R2,Enterprise,12,Physical Server
Server2\SQL2008R2,2008 R2,Enterprise,24,Physical Server
Server3\SQL2008R2,2008 R2,Enterprise,12,Virtual Machine
Server4\SQL2008,2008,Developer,8,Physical Server  
```

Ein Beispiel einer CSV-Datei finden Sie unter [MyPhysicalServers.csv](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts/MyPhysicalServers.csv).

#### <a name="csv-example-2---azure-vm"></a>CSV-Beispiel 2: Azure-VM

Für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanzen in Azure Virtual Machines muss die CSV-Datei wie folgt aussehen: 

```csv
name,version,edition,cores,hostType,subscriptionId,resourceGroup,azureVmName,azureVmOS    
ProdServerUS1\SQL01,2008 R2,Enterprise,12,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM1,2012    
ProdServerUS1\SQL02,2008 R2,Enterprise,24,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM1,2012    
ServerUS2\SQL01,2008,Enterprise,12,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM2,2012 R2    
ServerUS2\SQL02,2008,Enterprise,8,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM2,2012 R2    
SalesServer\SQLProdSales,2008 R2,Developer,8,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM3,2008 R2  
```

Unter [MyAzureVMs.csv](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts/MyAzureVMs.csv) finden Sie ein Beispiel einer CSV-Datei für Azure-VMs. 


> [!TIP]
> Unter [Beispiele für ESU-Registrierungsskripts](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts.md) finden Sie [!INCLUDE[tsql](../../includes/tsql-md.md)]- und PowerShell-Beispielskripts, die die erforderlichen Registrierungsinformationen für eine [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz in einer CSV-Datei generieren können. 

## <a name="download-esus"></a>Herunterladen von ESUs

Nachdem Ihre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanzen beim SQL Server-Registrierungsdienst registriert wurden, können Sie die Pakete für erweiterte Sicherheitsupdates über den Link im Azure-Portal herunterladen, sobald sie verfügbar sind. 

Gehen Sie zum Herunterladen von ESUs wie folgt vor: 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
1. Wechseln Sie zu Ihrer Ressource **SQL Server-Registrierung**. 
1. Wählen Sie im Navigationsbereich **Sicherheitsupdates** aus. 

   ![Überprüfen des Bereichs „Sicherheitsupdates“ auf verfügbare Updates](media/sql-server-extended-security-updates/security-updates-sql-registry.png)

1. Laden Sie Sicherheitsupdates hier herunter, sobald sie verfügbar sind. 

## <a name="supported-regions-and-data-residency"></a>Unterstützte Regionen und Datenresidenz

Der Dienst **SQL Server-Registrierung** (Vorschau) ist in bestimmten Azure-Regionen verfügbar. In der folgenden Tabelle werden die unterstützten Regionen und zugehörigen Typen von Datenresidenz gezeigt.

| **Region** | **Datenresidenz** |
|:--|:--|
|Australien (Osten)|geografischer Raum|
|Australien, Südosten|geografischer Raum|
|Kanada, Mitte|geografischer Raum|
|Frankreich, Mitte|geografischer Raum|
|Japan, Osten|geografischer Raum|
|Japan, Westen|geografischer Raum|
|Korea, Mitte|geografischer Raum|
|Korea, Süden|geografischer Raum|
|USA Nord Mitte|geografischer Raum|
|Nordeuropa|geografischer Raum|
|USA Süd Mitte|geografischer Raum|
|Asien, Südosten|Eine Region|
|Indien (Süden)|geografischer Raum|
|Südafrika, Norden|geografischer Raum|
|UK, Süden|geografischer Raum|
|UK, Westen|geografischer Raum|
|USA (Westen)|geografischer Raum|
|East US|geografischer Raum|
|USA (Mitte)|geografischer Raum|
|Asien, Osten|geografischer Raum|
|Europa, Westen|geografischer Raum|
|USA, Westen-Mitte|geografischer Raum|
|USA, Westen 2|geografischer Raum|
|USA (Ost) 2|geografischer Raum|

In den Regionen mit geografischer Residenz verwaltet der SQL-Registrierungsdienst Datensicherungen in einem georedundanten Speicherkonto (GRS).  In den Regionen mit Residenz in einer einzelnen Region verwaltet der SQL-Registrierungsdienst Datensicherungen in einem zonenredundanten Speicherkonto (GRS). Weitere Informationen finden Sie im [Trust Center](https://azuredatacentermap.azurewebsites.net/).

## <a name="configure-regional-redundancy"></a>Konfigurieren der regionalen Redundanz 

Kunden, die regionale Redundanz für ihre **SQL Server-Registrierung** benötigen, können Registrierungsdaten in zwei verschiedenen Regionen erstellen. Dann können die Kunden Sicherheitsupdates aus beiden Regionen basierend auf der Verfügbarkeit des **SQL Server-Registrierungsdiensts** herunterladen. 

Um regionale Redundanz herzustellen, muss der Dienst **SQL Server-Registrierung** in zwei verschiedenen Regionen erstellt werden, und der SQL Server-Bestand muss zwischen diesen beiden Diensten aufgeteilt werden. Auf diese Weise wird die Hälfte der SQL Server-Instanzen mit dem Registrierungsdienst in einer Region und die andere Hälfte mit dem Registrierungsdienst in der anderen Region registriert. 

Zum Konfigurieren der regionalen Redundanz führen Sie die folgenden Schritte aus:

1. Teilen Sie Ihren Bestand an SQL Server 2008- oder 2008 R2-Instanzen in zwei Dateien auf, z. B. „upload1.csv“ und „upload2.csv“. 
  
   :::image type="content" source="media/sql-server-extended-security-updates/two-upload-files-for-regional-redundancy.png" alt-text="Beispieldateien für den Upload":::

1. Erstellen Sie den ersten **SQL Server-Registrierungsdienst** in einer Region, und führen Sie dann eine Massenregistrierung der Instanzen in einer der CSV-Dateien in dieser Region durch. Beispiel: Erstellen Sie den ersten **SQL Server-Registrierungsdienst** in der Region **USA, Westen**, und führen Sie eine Massenregistrierung Ihrer SQL Server-Instanzen mit der Datei „upload1.csv“ durch. 
1. Erstellen Sie den zweiten **SQL Server-Registrierungsdienst** in der zweiten Region, und führen Sie dann eine Massenregistrierung der Instanzen in der anderen CSV-Dateien in dieser Region durch. Beispiel: Erstellen Sie den zweiten **SQL Server-Registrierungsdienst** in der Region **USA, Osten**, und führen Sie eine Massenregistrierung Ihrer SQL Server-Instanzen mit der Datei „upload2.csv“ durch. 


Sobald die Daten bei den beiden unterschiedlichen Ressourcen für die **SQL Server-Registrierung** registriert sind, können Sie je nach Dienstverfügbarkeit Sicherheitsupdates aus beiden Regionen herunterladen. 


## <a name="faq"></a>Häufig gestellte Fragen

Allgemeine häufig gestellte Fragen zu erweiterten Sicherheitsupdates finden Sie in den [häufig gestellten Fragen zu erweiterten Sicherheitsupdates](https://www.microsoft.com/cloud-platform/extended-security-updates). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-spezifische häufig gestellte Fragen sind nachstehend aufgeführt. 

**Wann wurde der Support für SQL Server 2008 und 2008 R2 eingestellt?**

Der Support für [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] und [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] endete am 9. Juli 2019. 

**Was bedeutet das Ende des Supports?**

Die Microsoft-Lebenszyklusrichtlinie sieht für Unternehmens- und Entwicklerprodukte (z. B. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und Windows Server) 10 Jahre Support vor (5 Jahre grundlegenden Support und 5 Jahre erweiterten Support). Gemäß der Richtlinie gibt es nach Ablauf des erweiterten Supportzeitraums keine Patches oder Sicherheitsupdates mehr. Dies kann zu Sicherheits- und Konformitätsproblemen führen und die Anwendungen und das Geschäft von Kunden ernsthaften Sicherheitsrisiken aussetzen.

**Welche Editionen von SQL Server kommen für erweiterte Sicherheitsupdates in Frage?**

Die Editionen Enterprise, Datacenter, Standard, Web und Workgroup von [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] und [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] sind sowohl in der x86- als auch der x64-Version für erweiterte Sicherheitsupdates berechtigt. 

**Wann sind die angebotenen erweiterten Sicherheitsupdates verfügbar?**

Erweiterte Sicherheitsupdates sind jetzt käuflich erwerbbar und können bei [!INCLUDE[msCoName](../../includes/msconame-md.md)]- oder [!INCLUDE[msCoName](../../includes/msconame-md.md)]-Lizenzierungspartnern bestellt werden. Die Bereitstellung erweiterter Sicherheitsupdates beginnt nach dem Enddatum des Supports gemäß Verfügbarkeit. Kunden, die sich für eine Migration zu Azure interessieren, können dies sofort tun. 

**Welche Aufgabe haben erweiterte Sicherheitsupdates?** 

Erweiterte Sicherheitsupdates umfassen die Bereitstellung von Sicherheitsupdates und -bulletins, die vom [Microsoft Security Response Center (MSRC)](https://portal.msrc.microsoft.com/) als **kritisch** eingestuft werden, für maximal drei Jahre nach dem 9. Juli 2019. Erweiterte Sicherheitsupdates werden gemäß Verfügbarkeit verteilt. Erweiterte Sicherheitsupdates bieten keinen technischen Support. Sie können jedoch andere [!INCLUDE[msCoName](../../includes/msconame-md.md)]-Supportpläne nutzen, um Unterstützung bei Ihren auf [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] und [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] bezogenen Fragen zu den von erweiterten Sicherheitsupdates abgedeckten Workloads zu erhalten. Erweiterte Sicherheitsupdates bieten keine neuen Funktionen, funktionellen Verbesserungen oder von Kunden gewünschte Korrekturen. Allerdings kann [!INCLUDE[msCoName](../../includes/msconame-md.md)] auch nicht sicherheitsrelevante Korrekturen bieten, sofern als notwendig erachtet.

**Warum bieten erweiterte Sicherheitsupdates für SQL Server 2008 und 2008 R2 nur „kritische“ Updates?**

Beim Ende des Supports in der Vergangenheit hat [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nur Sicherheitsupdates des Typs „Kritisch“ zur Verfügung gestellt, was die Konformitätskriterien unserer Unternehmenskunden erfüllt. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stellt kein allgemeines monatliches Sicherheitsupdate zur Verfügung. [!INCLUDE[msCoName](../../includes/msconame-md.md)] stellt nur auf Nachfrage [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Sicherheitsupdates (allgemeine Vertriebsversionen) für MSRC-Bulletins zur Verfügung, wenn [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als betroffenes Produkt ermittelt wird.
Wenn Situationen vorliegen, in denen neue wichtige [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Updates nicht zur Verfügung gestellt werden und die vom Kunden, aber nicht von MSRC als kritisch erachtet werden, arbeiten wir mit dem Kunden fallweise zusammen, um angemessene Abhilfemaßnahmen vorzuschlagen.

**Welche Lizenzierungsprogramme kommen für erweiterte Sicherheitsupdates in Frage?**

Software Assurance-Kunden können im Rahmen eines Enterprise Agreement (EA), Enterprise Subscription Agreement (EAS), Server & Cloud Enrollment (SCE) oder Enrollment for Education Solutions (EES) Sicherheitsupdates lokal erwerben. Software Assurance muss nicht zur gleichen Registrierung gehören.

**Müssen SQL Server-Kunden das aktuellste Service Pack ausführen, um von erweiterten Sicherheitsupdates zu profitieren?**

Ja, Kunden müssen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] oder Windows Server 2008 und 2008 R2 mit dem neuesten Service Pack ausführen, um erweiterte Sicherheitsupdates anwenden zu können. [!INCLUDE[msCoName](../../includes/msconame-md.md)] generiert nur Updates, die auf das neueste Service Pack angewendet werden können.

**Welche Optionen haben SQL Server-Kunden ohne Software Assurance?** 

Für Kunden ohne Software Assurance besteht alternativ in der Migration zu Azure die Möglichkeit des Zugriffs auf erweiterte Sicherheitsupdates. Bei variablen Workloads empfehlen wir Kunden die Migration zu Azure über das Modell der nutzungsbasierten Zahlung, was ein jederzeit ein zentrales Hoch- oder Herunterskalieren ermöglicht. Für vorhersehbare Workloads empfehlen wir Kunden die Migration zu Azure über Serverabonnement und reservierte Instanzen.
  
**Gilt dieses Angebot auch für SQL Server 2005?**

Nein. Für diese älteren Versionen empfehlen wir ein Upgrade auf die neuesten Versionen. Kunden können jedoch ein Upgrade auf die Version [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] oder [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] vornehmen, um in den Genuss dieses Angebots zu kommen.

**Kann ich eine ganz neue SQL Server 2008- oder 2008 R2-Instanz in Azure implementieren und dennoch erweiterte Sicherheitsupdates erhalten?**

Ja, Kunden können eine neue [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]- oder [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)]-Instanz auf einer Azure-VM starten und haben anschließend Zugriff auf erweiterte Sicherheitsupdates.

**Kann ich nach dem Enddatum des Supports lokalen technischen Support für SQL Server 2008 oder 2008 R2 erhalten, ohne erweiterte Sicherheitsupdates zu erwerben?**

Nein. Wenn ein Kunde [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] oder [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] hat und sich dafür entscheidet, während einer Migration ohne erweiterte Sicherheitsupdates lokal weiter zu arbeiten, kann er kein Supportticket protokollieren, selbst wenn er einen Supportplan hat. Bei einer Migration zu Azure kann er jedoch Support im Rahmen seines Azure-Supportplans erhalten.

**Wenn ein SQL Server 2008- und 2008 R2-Kunde seine eigene Lizenz (BYOL) einbringen möchte, muss er dann über Software Assurance-Abdeckung verfügen?**

Ja, Kunden benötigen Software Assurance, um die Vorteile des BYOL-Programms für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf Azure-VMs als Teil des License Mobility-Programms nutzen zu können. Kunden ohne Software Assurance empfehlen wir den Umstieg auf Azure SQL Managed Instance für ihre [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]-Umgebungen. Kunden können auch bei nutzungsbasierter Zahlung zu Azure Virtual Machines migrieren. Software Assurance-Kunden, die SQL Server kernbezogen lizenzieren, haben auch die Möglichkeit, mithilfe des Azure-Hybridvorteils zu Azure zu migrieren.

Azure SQL Managed Instance ist ein Dienst in Azure, der nahezu 100 %-ige Kompatibilität mit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (lokal) bietet. Die verwaltete Instanz bietet integrierte Funktionen für Hochverfügbarkeit und Notfallwiederherstellung sowie intelligente Leistungsmerkmale und die Möglichkeit zur dynamischen Skalierung. Die verwaltete Instanz bietet auch eine versionslose Lösung, die manuelle Sicherheitspatches und Upgrades überflüssig macht. Weitere Informationen zum BYOL-Programm finden Sie auf der Seite mit den Azure-Tarifen.

**Welche Möglichkeiten haben Kunden, SQL Server in Azure auszuführen?**

Kunden können ältere [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Umgebungen zu Azure SQL Managed Instance migrieren. Dabei handelt es sich um einen vollständig verwalteten Datenplattformdienst (PaaS), der in Anbetracht möglicher Bedenken hinsichtlich des Enddatums des Supports eine „versionsfreie“ Option bietet. Sie können ältere Umgebungen aber auch zu Azure Virtual Machines migrieren, um Zugriff auf Sicherheitsupdates zu erhalten. Die migrierten Datenbanken bleiben mit dem bestehenden System kompatibel. Weitere Informationen finden Sie unter [Kompatibilitätszertifizierung](../../database-engine/install-windows/compatibility-certification.md).

Erweiterte Sicherheitsupdates werden für [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] und [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] in Azure Virtual Machines nach dem Enddatum des Supports am 9. Juli 2019 für die nächsten drei Jahre verfügbar sein. Für Kunden, die ein Upgrade von [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] und [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] vornehmen möchten, werden alle nachfolgenden Versionen von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unterstützt. Für [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] bis [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] müssen Kunden das neueste unterstützte Service Pack verwenden. Ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] wird Kunden empfohlen, das neueste kumulative Update zu verwenden. Beachten Sie, dass Service Packs nicht ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] verfügbar sind, sondern nur kumulative Updates und allgemeine Vertriebsversionen (GDRs).

Azure SQL Managed Instance ist eine instanzbezogene Bereitstellungsoption in [!INCLUDE[ssSDS](../../includes/sssds-md.md)] und bietet weitreichende Kompatibilität mit der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Engine sowie native Unterstützung für virtuelle Netzwerke (VNET). Daher können Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbanken ohne Änderungen von Apps zu Azure SQL Managed Instance migrieren. Sie verknüpft die umfangreiche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Benutzeroberfläche mit den betrieblichen und finanziellen Vorteilen eines intelligenten, vollständig verwalteten Diensts. Nutzen Sie den neuen Azure Database Migration Service, um [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] und [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] mit wenigen (oder gar keinen) Änderungen am Anwendungscode zu Azure SQL Managed Instance zu verschieben.

**Können Kunden den Azure-Hybridvorteil für die Versionen SQL Server 2008 und 2008 R2 nutzen?**

Ja, Kunden mit aktiver Software Assurance oder entsprechenden Serverabonnements können den Azure-Hybridvorteil nutzen, indem sie bereits getätigte Investitionen in lokale Lizenzen zu ermäßigten Preisen für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] für die Ausführung in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] und Azure Virtual Machines nutzen.

**Können Kunden kostenlose erweiterte Sicherheitsupdates in Azure Government-Regionen erhalten?**

Ja, erweiterte Sicherheitsupdates stehen in Azure Virtual Machines in Azure Government-Regionen zur Verfügung.

**Können Kunden erweiterte Sicherheitsupdates für Azure Stack kostenlos erhalten?**

Ja, Kunden können [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] und Windows Server 2008 und [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] zu Azure Stack migrieren und nach dem Enddatum des Supports erweiterte Sicherheitsupdates ohne zusätzliche Kosten erhalten.

**Welche Richtlinien gelten für Kunden mit einem SQL Server 2008- und -2008 R2-Cluster, die gemeinsam genutzten Speicher verwenden, für die Migration zu Azure?**

Azure unterstützt derzeit kein Clustering mit gemeinsam genutztem Speicher. Hinweise zur Konfiguration einer hochverfügbaren [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Instanz in Azure finden Sie unter [Hochverfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr).

**Können Kunden erweiterte Sicherheitsupdates für SQL Server mit einem externen Hoster nutzen?**

Kunden können erweiterte Sicherheitsupdates nicht nutzen, wenn sie ihre [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]-Umgebung in eine PaaS-Implementierung anderer Cloudanbieter verlagern. Wenn Kunden den Umstieg auf virtuelle Computer (IaaS) planen, können sie License Mobility für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] über Software Assurance nutzen und erweiterte Sicherheitsupdates von [!INCLUDE[msCoName](../../includes/msconame-md.md)] erwerben. Damit können sie Patches auf die [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]-Instanzen manuell anwenden, die auf einer VM (IaaS) auf dem Server eines autorisierten SPLA-Hosters ausgeführt werden. Kostenlose Updates in Azure sind jedoch das attraktivere Angebot.

**Was sind die besten Vorgehensweisen zur Verbesserung der Leistung von SQL Server in virtuellen Azure-Computern?** 

Hinweise zur Optimierung der Leistung für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] in virtuellen Azure-Computern finden Sie in der [ Anleitung zur SQL Server-Optimierung](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance). 

## <a name="see-also"></a>Weitere Informationen

- [Seite zum Lebenszyklus von SQL Server 2008/2008 R2](https://support.microsoft.com/lifecycle/search?alpha=sql%20server%202008)
- [Seite zum Ende des Supports von SQL Server 2008/2008 R2](./sql-server-end-of-life-overview.md?WT.mc_id=akamseos)
- [Häufig gestellte Fragen zu erweiterten Sicherheitsupdates](https://aka.ms/sqleosfaq)
- [Microsoft Security Response Center (MSRC)](https://portal.msrc.microsoft.com/security-guidance/summary)
- [Verwalten von Windows-Updates mithilfe von Azure Automation](/azure/automation/update-management/overview)
- [Automatisierte Patches für SQL Server-VMs](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)
- [Anleitung zur Migration von Microsoft-Datenbanken](https://datamigration.microsoft.com/)
- [Azure-Migration: Optionen für die Migration per Lift & Shift Ihres aktuellen SQL Server 2008/2008 R2-Systems in eine Azure-VM](https://azure.microsoft.com/services/azure-migrate/)
- [Framework für die Cloudeinführung (Cloud Adoption Framework): Migration von SQL Server](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)
- [ESU-bezogene Skripts auf GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/sql-server-extended-security-updates/scripts)