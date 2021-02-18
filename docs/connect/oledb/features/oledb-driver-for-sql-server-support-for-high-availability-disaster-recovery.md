---
title: OLE DB-Treiber für SQL Server-Unterstützung für Hochverfügbarkeit, Notfallwiederherstellung
description: Erfahren Sie, auf welche Weise der OLE DB-Treiber für SQL Server Hochverfügbarkeit und Notfallwiederherstellung unterstützt, wenn eine Verbindung mit Datenbanken hergestellt wird, für die diese Funktionen konfiguriert wurden.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d5adce5bcda14dd275791f055359654bc120e79d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347898"
---
# <a name="ole-db-driver-for-sql-server-support-for-high-availability-disaster-recovery"></a>OLE DB-Treiber für SQL Server-Unterstützung für Hochverfügbarkeit, Notfallwiederherstellung
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  In diesem Artikel wird die *OLE DB-Treiber für SQL Server*-Unterstützung für [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] erläutert. Weitere Informationen zu [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] finden Sie unter [Verfügbarkeitsgruppenlistener, Clientkonnektivität und Anwendungsfailover &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md), [Erstellung und Konfiguration von Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md), [Failoverclustering und AlwaysOn-Verfügbarkeitsgruppen &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md) und [Aktive sekundäre Replikate: Lesbare sekundäre Replikate &#40;AlwaysOn-Verfügbarkeitsgruppen&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
 Sie können den Verfügbarkeitsgruppenlistener einer bestimmten Verfügbarkeitsgruppe in der Verbindungszeichenfolge angeben. Wenn ein OLE DB-Treiber für SQL Server-Anwendung mit einer Datenbank in einer Verfügbarkeitsgruppe verbunden ist, die ein Failover ausführt, wird die ursprüngliche Verbindung unterbrochen, und die Anwendung muss eine neue Verbindung herstellen, um die Arbeit nach dem Failover fortzusetzen.  
  
 Wenn Sie keine Verbindung zu einem verfügbaren Gruppenlistener herstellen und mehrere IP-Adressen einem Hostnamen zugeordnet sind, durchläuft der OLE DB-Treiber für SQL Server nacheinander alle IP-Adressen, die dem DNS-Eintrag zugeordnet sind. Dies kann zeitaufwändig sein, wenn die erste vom DNS-Server zurückgegebene IP-Adresse an keine Netzwerkschnittstellenkarte (NIC) gebunden ist. Beim Herstellen einer Verbindung zu einem verfügbaren Gruppenlistener versucht der OLE DB-Treiber für SQL Server, Verbindungen zu allen IP-Adressen parallel herzustellen, und wenn ein Verbindungsversuch erfolgreich ist, verwirft der Treiber alle ausstehenden Verbindungsversuche.  
  
> [!NOTE]  
> Das Erhöhen des Verbindungstimeouts sowie die Implementierung von Verbindungswiederholungslogik erhöhen die Wahrscheinlichkeit, dass eine Anwendung eine Verbindung zu einer Verfügbarkeitsgruppe herstellt. Da zudem eine Verbindung aufgrund eines Verfügbarkeitsgruppenfailovers fehlschlagen kann, empfiehlt sich die Implementierung von Verbindungswiederholungslogik, wodurch im Fall einer fehlgeschlagenen Verbindung bis zur erneuten Verbindung Wiederholungsversuche erfolgen.  
  
## <a name="connecting-with-multisubnetfailover"></a>Verbinden mit MultiSubnetFailover  
 Geben Sie immer **MultiSubnetFailover=Yes** an, wenn Sie eine Verbindung mit einem SQL Server Always On-Verfügbarkeitsgruppenlistener oder einer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Failoverclusterinstanz herstellen. **MultiSubnetFailover** ermöglicht ein schnelleres Failover für alle Always On-Verfügbarkeitsgruppen und die Failoverclusterinstanz in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] und reduziert die Failoverzeit für einzelne und Multisubnetz-Always On-Topologien erheblich. Während eines Multisubnetzfailovers versucht der Client Verbindungen parallel. Während eines Subnetzfailovers versucht der OLE DB-Treiber für SQL Server die TCP-Verbindung wiederherzustellen.  
  
 Die **MultiSubnetFailover**-Verbindungseigenschaft gibt an, dass die Anwendung in einer Verfügbarkeitsgruppe oder einer Failoverclusterinstanz bereitgestellt wird und der OLE DB-Treiber für SQL Server versucht, eine Verbindung mit der Datenbank auf der primären [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz herzustellen, indem mit allen IP-Adressen der Verfügbarkeitsgruppe Verbindungsversuche unternommen werden. Wenn **MultiSubnetFailover=Yes** für eine Verbindung angegeben wird, wiederholt der Client TCP-Verbindungsversuche schneller als dies bei den standardmäßigen TCP-Neuübertragungsintervallen des Betriebssystems der Fall ist. Auf diese Weise kann die Verbindung nach einem Failover einer Always On-Verfügbarkeitsgruppe oder einer Failoverclusterinstanz schneller wiederhergestellt werden. Diese Einstellung gilt sowohl für Einzelsubnetz- als auch Multisubnetz-Verfügbarkeitsgruppen und -Failoverclusterinstanzen.  
  
 Weitere Informationen zu Verbindungszeichenfolgen-Schlüsselwörtern finden Sie unter [Using Connection String Keywords with SQL Server Native Client (Verwenden von Verbindungszeichenfolgen-Schlüsselwörtern mit dem OLE DB-Treiber für SQL Server)](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md).  
  
 Das Angeben von **MultiSubnetFailover=Yes** für ein anderes Verbindungsziel als einen Verfügbarkeitsgruppenlistener oder eine Failoverclusterinstanz kann die Leistung beeinträchtigen und wird nicht unterstützt.  
  
 Befolgen Sie beim Herstellen einer Verbindung mit einem Server in einer Verfügbarkeitsgruppe oder einer Failoverclusterinstanz die folgenden Richtlinien:  
  
-   Verwenden Sie die **MultiSubnetFailover** -Verbindungseigenschaft, wenn Sie eine Verbindung mit einem Einzelsubnetz oder Multisubnetz herstellen. Dadurch wird die Leistung für beide verbessert.  
  
-   Um eine Verbindung mit einer Verfügbarkeitsgruppe herzustellen, geben Sie in der Verbindungszeichenfolge den Verfügbarkeitsgruppenlistener der Verfügbarkeitsgruppe als Server an.  
  
-   Ein Verbindungsversuch mit einer mit mehr als 64 IP-Adressen konfigurierten [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Instanz verursacht einen Verbindungsfehler.  
  
-   Das Verhalten einer Anwendung, die die **MultiSubnetFailover** -Verbindungseigenschaft verwendet, wird nicht vom Authentifizierungstyp beeinflusst: [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Authentifizierung, Kerberos-Authentifizierung oder Windows-Authentifizierung.  
  
-   Sie können den Wert von **loginTimeout** erhöhen, um die Failoverzeit zu berücksichtigen und Wiederholungsversuche für Anwendungsverbindungen zu reduzieren.  
  
-   Verteilte Transaktionen werden nicht unterstützt.  
  
Wenn das schreibgeschützte Routing nicht aktiviert ist, scheitert das Herstellen der Verbindung mit einem sekundären Replikatspeicherort in einer Verfügbarkeitsgruppe in den folgenden Situationen:  
  
1.  Wenn der sekundäre Replikatspeicherort nicht zum Akzeptieren von Verbindungen konfiguriert ist.  
  
2.  Wenn eine Anwendung **ApplicationIntent=ReadWrite** verwendet (weiter unten erläutert), und der sekundäre Replikatspeicherort für schreibgeschützten Zugriff konfiguriert ist.  
  
Es kann keine Verbindung hergestellt werden, wenn ein primäres Replikat so konfiguriert ist, dass schreibgeschützte Arbeitslasten abgelehnt werden, und die Verbindungszeichenfolge **ApplicationIntent=ReadOnly** enthält.  
  
## <a name="upgrading-to-use-multi-subnet-clusters-from-database-mirroring"></a>Aktualisieren zur Verwendung von Multisubnetzclustern aus Datenbankspiegelung  
Ein Verbindungsfehler tritt auf, wenn die Verbindungsschlüsselwörter **MultiSubnetFailover** und **Failover_Partner** in der Verbindungszeichenfolge vorhanden sind. Es tritt auch ein Fehler auf, wenn **MultiSubnetFailover** verwendet wird und [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] eine Failoverpartnerantwort zurückgibt, die angibt, dass es Teil eines Datenbankspiegelungspaars ist.  
  
Wenn Sie eine OLE DB-Treiber für SQL Server-Anwendung aktualisieren, die derzeit Datenbankspiegelung in einem Multisubnetzszenario verwendet, müssen Sie die **Failover_Partner**-Verbindungseigenschaft entfernen und durch **MultiSubnetFailover**, festgelegt auf **Yes**, ersetzen, sowie den Servernamen in der Verbindungszeichenfolge durch einen Verfügbarkeitsgruppenlistener ersetzen. Wenn eine Verbindungszeichenfolge **Failover_Partner** und **MultiSubnetFailover=Yes** verwendet, generiert der Treiber einen Fehler. Wenn eine Verbindungszeichenfolge jedoch **Failover_Partner** und **MultiSubnetFailover=No** (oder **ApplicationIntent=ReadWrite**) verwendet, verwendet die Anwendung Datenbankspiegelung.  
  
Der Treiber gibt einen Fehler zurück, wenn die Datenbankspiegelung in der primären Datenbank in der Verfügbarkeitsgruppe verwendet wird, und wenn **MultiSubnetFailover=Yes** in der Verbindungszeichenfolge verwendet wird, die statt mit einem Verfügbarkeitsgruppenlistener eine Verbindung mit einer primären Datenbank herstellt.  


[!INCLUDE[specify-application-intent_read-only-routing](~/includes/paragraph-content/specify-application-intent-read-only-routing.md)]


## <a name="ole-db"></a>OLE DB  
Der OLE DB-Treiber für SQL Server unterstützt die beiden Schlüsselwörter **ApplicationIntent** und **MultiSubnetFailover**.   
  
Die zwei OLE DB-Verbindungszeichenfolgenschlüsselwörter wurden hinzugefügt, um [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] im OLE DB-Treiber für SQL Server zu unterstützen:  
  
-   **ApplicationIntent** 
-   **MultiSubnetFailover**  
  
 Weitere Informationen zu Verbindungszeichenfolgen-Schlüsselwörtern im OLE DB-Treiber für SQL Server finden Sie unter [Verwenden von Verbindungszeichenfolgen-Schlüsselwörtern mit dem OLE DB-Treiber für SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md).  

### <a name="application-intent"></a>Application Intent 

Die entsprechenden Verbindungseigenschaften sind:  
  
-   **SSPROP_INIT_APPLICATIONINTENT**  
  
-   **DBPROP_INIT_PROVIDERSTRING**  
  
Eine OLE DB-Treiber für SQL Server-Anwendung kann eine der Methoden verwenden, um die Anwendungsabsicht anzugeben:  
  
 -   **IDBInitialize::Initialize**  
 **IDBInitialize::Initialize** verwendet den zuvor konfigurierten Satz von Eigenschaften, um die Datenquelle zu initialisieren und das Datenquellenobjekt zu erstellen. Geben Sie die Anwendungsabsicht als Anbietereigenschaft oder als einen Teil der erweiterten Eigenschaftenzeichenfolge an.  
  
 -   **IDataInitialize::GetDataSource**  
 **IDataInitialize::GetDataSource** verwendet eine Eingabeverbindungszeichenfolge, die das **Application Intent** -Schlüsselwort enthalten kann.  
  
 -   **IDBProperties::SetProperties**  
 Um den **ApplicationIntent** -Eigenschaftswert festzulegen, rufen Sie **IDBProperties::SetProperties** auf, indem Sie die **SSPROP_INIT_APPLICATIONINTENT** -Eigenschaft mit dem Wert "**ReadWrite**" oder "**ReadOnly**" oder die **DBPROP_INIT_PROVIDERSTRING** -Eigenschaft mit dem Wert angeben, der "**ApplicationIntent=ReadOnly**" oder "**ApplicationIntent=ReadWrite**" enthält.  
  
Sie können die Anwendungsabsicht im Feld für die Eigenschaften der Anwendungsabsicht auf der Registerkarte Alle im Dialogfeld **Datenverknüpfungseigenschaften** angeben.  
  
Wenn implizite Verbindungen hergestellt werden, verwendet die implizite Verbindung die Einstellung der Anwendungsabsicht der übergeordneten Verbindung. Auf ähnliche Weise erben mehrere aus der gleichen Datenquelle erstellte Sitzungen die Einstellung für die Anwendungsabsicht der Datenquelle.  
  
### <a name="multisubnetfailover"></a>MultiSubnetFailover

Die entsprechenden Verbindungseigenschaften sind:  
  
-   **SSPROP_INIT_MULTISUBNETFAILOVER**  
  
-   **DBPROP_INIT_PROVIDERSTRING**  

Eine OLE DB-Treiber für SQL Server-Anwendung kann eine der folgenden Methoden verwenden, um die Option „MultiSubnetFailover“ festzulegen:  

 -   **IDBInitialize::Initialize**  
 **IDBInitialize::Initialize** verwendet den zuvor konfigurierten Satz von Eigenschaften, um die Datenquelle zu initialisieren und das Datenquellenobjekt zu erstellen. Geben Sie die Anwendungsabsicht als Anbietereigenschaft oder als einen Teil der erweiterten Eigenschaftenzeichenfolge an.  
  
 -   **IDataInitialize::GetDataSource**  
 **IDataInitialize::GetDataSource** verwendet eine Eingabeverbindungszeichenfolge, die das **MultiSubnetFailover**-Schlüsselwort enthalten kann.  

-   **IDBProperties::SetProperties**  
Rufen Sie **IDBProperties::SetProperties** auf, und übergeben Sie dabei die Eigenschaft **SSPROP_INIT_MULTISUBNETFAILOVER** mit dem Wert **VARIANT_TRUE** oder **VARIANT_FALSE** oder die Eigenschaft **DBPROP_INIT_PROVIDERSTRING** mit einem Wert, der „**MultiSubnetFailover=Yes**“ oder „**MultiSubnetFailover=No**“ enthält, um den Eigenschaftswert **MultiSubnetFailover** festzulegen.

#### <a name="example"></a>Beispiel

```
DBPROP rgPropMultisubnet;

rgPropMultisubnet.dwPropertyID = SSPROP_INIT_MULTISUBNETFAILOVER;
rgPropMultisubnet.dwOptions = DBPROPOPTIONS_REQUIRED;
rgPropMultisubnet.dwStatus = DBPROPSTATUS_OK;
rgPropMultisubnet.colid = DB_NULLID;
V_VT(&(rgPropMultisubnet.vValue)) = VT_BOOL;
V_BOOL(&(rgPropMultisubnet.vValue)) = VARIANT_TRUE;

DBPROPSET PropSet;

PropSet.rgProperties = &rgPropMultisubnet;
PropSet.cProperties = 1;
PropSet.guidPropertySet = DBPROPSET_SQLSERVERDBINIT;
IDBProperties* pIDBProperties = NULL;
hr = pIDBInitialize->QueryInterface(IID_IDBProperties, (void **)&pIDBProperties);
pIDBProperties->SetProperties(1, &PropSet);
```

## <a name="see-also"></a>Weitere Informationen  
 [OLE DB-Treiber für SQL Server-Features](../../oledb/features/oledb-driver-for-sql-server-features.md)    
 [Verwenden von Verbindungszeichenfolgen-Schlüsselwörtern mit dem OLE DB-Treiber für SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)  
  
  
