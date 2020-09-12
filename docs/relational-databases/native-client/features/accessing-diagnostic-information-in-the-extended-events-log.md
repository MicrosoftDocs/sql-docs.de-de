---
description: Zugreifen auf SQL Server Native Client Diagnoseinformationen im Protokoll für erweiterte Ereignisse
title: Diagnoseinformationen im Protokoll für erweiterte Ereignisse
ms.date: 03/14/2017
ms.reviewer: ''
ms.prod: sql
ms.technology: native-client
ms.topic: reference
ms.assetid: aaa180c2-5e1a-4534-a125-507c647186ab
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 51dc4f1def0f612c0eb074ae1111a177e27c1413
ms.sourcegitcommit: f7c9e562d6048f89d203d71685ba86f127d8d241
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2020
ms.locfileid: "90042761"
---
# <a name="accessing-sql-server-native-client-diagnostic-information-in-the-extended-events-log"></a>Zugreifen auf SQL Server Native Client Diagnoseinformationen im Protokoll für erweiterte Ereignisse
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Ab [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]wurden der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client und die Datenzugriffs-Ablaufverfolgung ([Datenzugriffs-Ablaufverfolgung](https://go.microsoft.com/fwlink/?LinkId=125805)) aktualisiert, um das Abrufen von Diagnoseinformationen über Verbindungsfehler vom Verbindungsringpuffer sowie von Informationen zur Anwendungsleistung aus dem Protokoll für erweiterte Ereignisse zu erleichtern.  
  
 Informationen dazu, wie Sie das Protokoll für erweiterte Ereignisse lesen, finden Sie unter [View Event Session Data](https://msdn.microsoft.com/library/ac742a01-2a95-42c7-b65e-ad565020dc49).  
  
> [!NOTE]  
>  Diese Funktion ist nur für Problembehandlung und Diagnosezwecke vorgesehen und ist möglicherweise nicht geeignet zu Überwachungs oder Sicherheitszwecken.  
  
## <a name="remarks"></a>Bemerkungen  
 Für Verbindungsvorgänge sendet der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client eine Clientverbindungs-ID. Wenn die Verbindung nicht hergestellt werden kann, können Sie auf den Konnektivitätsringpuffer zugreifen ([Behandlung von Konnektivitätsproblemen in SQL Server 2008 mit dem Konnektivitätsringpuffer](https://techcommunity.microsoft.com/t5/sql-server/connectivity-troubleshooting-in-sql-server-2008-with-the/ba-p/383393)), das Feld **ClientConnectionID** suchen und Diagnoseinformationen zum Verbindungsfehler abrufen. Clientverbindungs-IDs werden nur im Ringpuffer protokolliert, wenn ein Fehler auftritt. (Wenn vor dem Senden des prelogin-Pakets keine Verbindung hergestellt werden kann, wird keine Clientverbindungs-ID generiert.) Die Clientverbindungs-ID ist eine 16-Byte-GUID. Sie können auch die Clientverbindungs-ID im erweiterten Ereignisse-Ausgabeziel suchen, wenn Ereignissen in einer Sitzung für erweiterte Ereignisse die **client_connection_id** -Aktion hinzugefügt wird. Sie können die Datenzugriffs-Ablaufverfolgung aktivieren, den Verbindungsbefehl erneut ausführen und das **ClientConnectionID** -Feld in der Datenzugriffs-Ablaufverfolgung für einen fehlgeschlagenen Vorgang beobachten, wenn Sie weitere Hilfe bei der Diagnose benötigen.  
  
 Wenn Sie ODBC in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client verwenden und eine Verbindung erfolgreich ist, können Sie die Clientverbindungs-ID mithilfe des **SQL_COPT_SS_CLIENT_CONNECTION_ID** -Attributs mit [SQLGetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md).  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client sendet auch eine Thread-spezifische Aktivitäts-ID. Die Aktivitäts-ID wird in den Sitzungen für erweiterte Ereignisse aufgezeichnet, wenn die Sitzungen bei aktivierter TRACK_CAUSAILITY-Option gestartet werden. Bei Leistungsproblemen mit einer aktiven Verbindung können Sie die Aktivitäts-ID aus der Datenzugriffs-Ablaufverfolgung des Clients abrufen (**ActivityID** -Feld) und dann die Aktivitäts-ID in der Ausgabe für die erweiterten Ereignisse suchen. Die Aktivitäts-ID in den erweiterten Ereignissen ist eine 16-Byte-GUID (nicht dieselbe wie die GUID für die Clientverbindungs-ID), der eine Vier-Byte-Sequenznummer angefügt wurde. Die Sequenznummer steht für die Reihenfolge einer Anforderung innerhalb eines Threads und gibt die relative Reihenfolge des Batches und der RPC-Anweisungen für den Thread an. Der **ActivityID** wird optional für SQL-Batchanweisungen und RPC-Anforderungen gesendet, wenn die Datenzugriffsablaufverfolgung aktiviert und das 18. Bit im Datenzugriffs-Ablaufverfolgungs-Konfigurationswort auf ON gestellt wird.  
  
 Im folgenden Beispiel wird [!INCLUDE[tsql](../../../includes/tsql-md.md)] zum Starten einer Sitzung für erweiterte Ereignisse verwendet, die in einem Ringpuffer gespeichert werden und die von einem Client in RPC- und Batch-Vorgängen gesendete Aktivitäts-ID aufzeichnen.  
  
```  
create event session MySession on server   
add event connectivity_ring_buffer_recorded,   
add event sql_statement_starting (action (client_connection_id)),   
add event sql_statement_completed (action (client_connection_id)),   
add event rpc_starting (action (client_connection_id)),   
add event rpc_completed (action (client_connection_id))  
add target ring_buffer with (track_causality=on)  
  
```  
  
## <a name="control-file"></a>Steuerelementdatei  
 In [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]ist der Inhalt der SQL Server Native Client-Steuerelementdatei (ctrl.guid.snac11):  
  
```  
{8B98D3F2-3CC6-0B9C-6651-9649CCE5C752}  0x00000000  0   MSDADIAG.ETW  
{2DA81B52-908E-7DB6-EF81-76856BB47C4F}  0xFFFFFFFF  0   SQLNCLI11.1  
```  
  
## <a name="mof-file"></a>MOF-Datei  
 In [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]ist der Inhalt der SQL Server Native Client-MOF-Datei:  
  
```  
#pragma classflags("forceupdate")  
#pragma namespace ("\\\\.\\Root\\WMI")  
  
/////////////////////////////////////////////////////////////////////////////  
//  
//  MSDADIAG.ETW  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW"),  
 Guid("{8B98D3F2-3CC6-0B9C-6651-9649CCE5C752}"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW : EventTrace  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW"),  
 Guid("{8B98D3F3-3CC6-0B9C-6651-9649CCE5C752}"),  
 DisplayName("msdadiag"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW_Trace : Bid2Etw_MSDADIAG_ETW  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW formatted output (A)"),  
 EventType(17),  
 EventTypeName("TextA"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW_Trace_TextA : Bid2Etw_MSDADIAG_ETW_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringA"),  
     extension("RString"),  
     read  
    ]  
    object msgStr;  
};  
  
[  
 dynamic: ToInstance,  
 Description("MSDADIAG.ETW formatted output (W)"),  
 EventType(18),  
 EventTypeName("TextW"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_MSDADIAG_ETW_Trace_TextW : Bid2Etw_MSDADIAG_ETW_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringW"),  
     extension("RWString"),  
     read  
    ]  
    object msgStr;  
};  
  
/////////////////////////////////////////////////////////////////////////////  
//  
//  SQLNCLI11.1  
  
[  
 dynamic: ToInstance,  
 Description("SQLNCLI11.1"),  
 Guid("{2DA81B52-908E-7DB6-EF81-76856BB47C4F}"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_SQLNCLI11_1 : EventTrace  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("SQLNCLI11.1"),  
 Guid("{2DA81B53-908E-7DB6-EF81-76856BB47C4F}"),  
 DisplayName("SQLNCLI11.1"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_SQLNCLI11_1_Trace : Bid2Etw_SQLNCLI11_1  
{  
};  
  
[  
 dynamic: ToInstance,  
 Description("SQLNCLI11.1 formatted output (A)"),  
 EventType(17),  
 EventTypeName("TextA"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_SQLNCLI11_1_Trace_TextA : Bid2Etw_SQLNCLI11_1_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringA"),  
     extension("RString"),  
     read  
    ]  
    object msgStr;  
};  
  
[  
 dynamic: ToInstance,  
 Description("SQLNCLI11.1 formatted output (W)"),  
 EventType(18),  
 EventTypeName("TextW"),  
 locale("MS\\0x409")  
]  
class Bid2Etw_SQLNCLI11_1_Trace_TextW : Bid2Etw_SQLNCLI11_1_Trace  
{  
    [  
     WmiDataId(1),  
     Description("Module ID"),  
     read  
    ]  
    uint32 ModID;  
  
    [  
     WmiDataId(2),  
     Description("Text StringW"),  
     extension("RWString"),  
     read  
    ]  
    object msgStr;  
};  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Behandlung von Fehlern und Meldungen](../../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
  
  
