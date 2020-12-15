---
title: Richtlinien zur Unterstützung
description: Erfahren Sie mehr über SQL Server Native Client unterstützte SQL Server Versionen, Betriebssysteme und Support Richtlinien für ADO, bcp, ODBC und OLE DB.
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.custom: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: 09c80cf4-23e6-4027-a24f-cdb9c87af811
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1bdc9812b46869a46879adfe10efda8b35128bf9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469251"
---
# <a name="support-policies-for-sql-server-native-client"></a>Richtlinien zur Unterstützung für SQL Server Native Client
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  In diesem Thema wird die Verwendung verschiedener Datenzugriffskomponenten mit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client erläutert.  
  
## <a name="server-support"></a>Serverunterstützung  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 11,0 unterstützt Verbindungen mit, [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] ,, [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] , [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] und [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] .  
  
## <a name="supported-operating-system-versions"></a>Unterstützte Betriebssystemversionen  
 In der folgenden Tabelle werden die Betriebssysteme, die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client unterstützen, aufgeführt.  
  
|SQL Server Native Client-Version|Unterstützte Betriebssysteme|  
|--------------------------------------|---------------------------------|  
|SQL Server Native Client (SQL Server 2005)|Microsoft Windows 2000 Service Pack 4 oder höher<br /><br /> Microsoft Windows Server 2003 oder höher<br /><br /> Microsoft Windows XP Service Pack 1 oder höher<br /><br /> Microsoft Windows Vista (erfordert [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Service Pack 2 oder höher)<br /><br /> Microsoft Windows Server 2008 R2 (erfordert [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Service Pack 2 oder höher)|  
|SQL Server Native Client 10,0 ( [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] )|Microsoft Windows Server 2003 Service Pack 2 oder höher<br /><br /> Microsoft Windows XP Service Pack 2 oder höher<br /><br /> Microsoft Windows Vista<br /><br /> Microsoft Windows Server 2008 R2|  
|SQL Server Native Client 10,5 ( [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] )|Microsoft Windows Server 2003 Service Pack 2 oder höher<br /><br /> Microsoft Windows XP Service Pack 2 oder höher<br /><br /> Microsoft Windows Vista<br /><br /> Microsoft Windows Server 2008 R2<br /><br /> Microsoft Windows 7|  
|SQL Server Native Client 11.0 ([!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] und [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)])|Microsoft Windows Vista<br /><br /> Microsoft Windows Server 2008 R2<br /><br /> Microsoft Windows 7<br /><br /> Microsoft Windows 8<br /><br /> Microsoft Windows Server 2012|  
  
## <a name="ado-support-policies"></a>Richtlinien zur ADO-Unterstützung  
 ADO-Anwendungen können den zum Lieferumfang von Windows gehörenden SQLOLEDB OLE DB-Anbieter verwenden, sofern sie keine Funktionen von [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] oder höher benötigen.  
  
 ADO-Anwendungen können die Version von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client verwenden, die in enthalten ist [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] . ADO-Anwendungen können auch [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 11.0 (im Lieferumfang von [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] enthalten) verwenden, müssen in diesem Fall allerdings in den Verbindungszeichenfolgen `DataTypeCompatibility=80` angeben. Wenn `DataTypeCompatibility=80` in den Verbindungszeichenfolgen angegeben wird, sind nur die Funktionen von [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] verfügbar.  
  
## <a name="bcp-support-policies"></a>Richtlinien zur BCP-Unterstützung  
 Ab [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] unterstützt bcp.exe Datendateien, die nicht mehr als drei [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Versionen älter sind als die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Version, mit der bcp.exe ausgeliefert wurde.  
  
## <a name="odbc-support-policies"></a>Richtlinien zur ODBC-Unterstützung  
 Anwendungen sollten den im Betriebssystem Windows enthaltenen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-ODBC-Treiber verwenden. Sie können den ODBC-Treiber von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client verwenden, wenn die Anwendung für die Verwendung einer bestimmten Version von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client zertifiziert wurde.  
  
## <a name="ole-db-support-policies"></a>Richtlinien zur OLE DB-Unterstützung  
 Anwendungen sollten den im Betriebssystem Windows enthaltenen [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] OLE DB-Anbieter verwenden. Sie können den OLE DB-Treiber von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client verwenden, wenn die Anwendung für die Verwendung einer bestimmten Version von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client zertifiziert wurde.  
  
 OLE DB-Anwendungen, die nicht für die Verwendung mit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client zertifiziert wurden, können [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client einsetzen, wenn in den Verbindungszeichenfolgen `DataTypeCompatibility=80` angegeben wird.  
  
 OLE DB-Anwendungen, die OLE DB-Dienstkomponenten einsetzen, können nur dann [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client verwenden, wenn in den Verbindungszeichenfolgen `DataTypeCompatibility=80` angegeben wird. In diesem Fall sind jedoch keine Features verfügbar, die hinzugefügt werden [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] .  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen von Anwendungen mit SQL Server Native Client](../../../relational-databases/native-client/applications/building-applications-with-sql-server-native-client.md)  
  
  
