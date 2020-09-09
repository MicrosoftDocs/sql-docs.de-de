---
description: sys.dm_broker_connections (Transact-SQL)
title: sys. dm_broker_connections (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/08/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_broker_connections
- dm_broker_connections
- sys.dm_broker_connections_TSQL
- dm_broker_connections_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_broker_connections dynamic management view
ms.assetid: d9e20433-67fe-4fcc-80e3-b94335b2daef
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8ce810ed6014710b6d4a9a3cb61da9fe4e0605cd
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89537683"
---
# <a name="sysdm_broker_connections-transact-sql"></a>sys.dm_broker_connections (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Gibt eine Zeile für jede [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Netzwerkverbindung zurück. Die folgende Tabelle enthält weitere Informationen:  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**connection_id**|**uniqueidentifier**|Bezeichner der Verbindung. Lässt NULL-Werte zu.|  
|**transport_stream_id**|**uniqueidentifier**|Der Bezeichner der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Netzwerkschnittstellen Verbindung (SNI), die von dieser Verbindung für die TCP/IP-Kommunikation verwendet wird. Lässt NULL-Werte zu.|  
|**state**|**smallint**|Aktueller Verbindungsstatus. Lässt NULL-Werte zu. Mögliche Werte:<br /><br /> 1 = NEW<br /><br /> 2 = CONNECTING<br /><br /> 3 = CONNECTED<br /><br /> 4 = LOGGED_IN<br /><br /> 5 = geschlossen|  
|**state_desc**|**nvarchar(60)**|Aktueller Verbindungsstatus. Lässt NULL-Werte zu. Mögliche Werte:<br /><br /> NEW<br /><br /> CONNECTING<br /><br /> CONNECTED<br /><br /> LOGGED_IN<br /><br /> CLOSED|  
|**connect_time**|**datetime**|Datum und Uhrzeit der Verbindungseröffnung. Lässt NULL-Werte zu.|  
|**login_time**|**datetime**|Datum und Uhrzeit der erfolgreichen Verbindungsanmeldung. Lässt NULL-Werte zu.|  
|**authentication_method**|**nvarchar(128)**|Name der Windows-Authentifizierungsmethode, z. B. NTLM oder KERBEROS. Dieser Wert stammt von Windows. Lässt NULL-Werte zu.|  
|**principal_name**|**nvarchar(128)**|Anmeldename, der auf Verbindungsberechtigungen überprüft wurde. Bei der Windows-Authentifizierung handelt es sich bei diesem Wert um den Remotebenutzernamen. Bei der Zertifikatsauthentifizierung handelt es sich bei diesem Wert um den Besitzer des Zertifikats. Lässt NULL-Werte zu.|  
|**remote_user_name**|**nvarchar(128)**|Name des Peer-Benutzers aus der anderen Datenbank, die von der Windows-Authentifizierung verwendet wird. Lässt NULL-Werte zu.|  
|**last_activity_time**|**datetime**|Datum und Uhrzeit für die letzte Verwendung der Verbindung zum Senden oder Empfangen von Informationen. Lässt NULL-Werte zu.|  
|**is_accept**|**bit**|Gibt an, ob die Verbindung ursprünglich von der Remoteseite stammt. Lässt NULL-Werte zu.<br /><br /> 1 = Die Verbindung ist eine Anforderung, die von der Remoteinstanz angenommen wurde.<br /><br /> 0 = Die Verbindung wurde von der lokalen Instanz gestartet.|  
|**login_state**|**smallint**|Status des Anmeldeprozesses für diese Verbindung. Mögliche Werte:<br /><br /> 0 = INITIAL<br /><br /> 1 = WAIT LOGIN NEGOTIATE<br /><br /> 2 = ONE ISC<br /><br /> 3 = ONE ASC<br /><br /> 4 = TWO ISC<br /><br /> 5 = TWO ASC<br /><br /> 6 = WAIT ISC Confirm<br /><br /> 7 = WAIT ASC Confirm<br /><br /> 8 = WAIT REJECT<br /><br /> 9 = WAIT PRE-MASTER SECRET<br /><br /> 10 = WAIT VALIDATION<br /><br /> 11 = WAIT ARBITRATION<br /><br /> 12 = online<br /><br /> 13 = ERROR|  
|**login_state_desc**|**nvarchar(60)**|Aktueller Anmeldestatus des Remotecomputers. Mögliche Werte:<br /><br /> Verbindungshandshake wird initialisiert.<br /><br /> Verbindungshandshake wartet auf Anmeldungsaushandlungs-Meldung.<br /><br /> Verbindungshandshake hat Sicherheitskontext zur Authentifizierung initialisiert und gesendet.<br /><br /> Verbindungshandshake hat Sicherheitskontext zur Authentifizierung empfangen und akzeptiert.<br /><br /> Verbindungshandshake hat Sicherheitskontext zur Authentifizierung initialisiert und gesendet. Ein optionaler Mechanismus ist für das Authentifizieren der Peers verfügbar.<br /><br /> Verbindungshandshake hat Sicherheitskontext zur Authentifizierung empfangen und gesendet. Ein optionaler Mechanismus ist für das Authentifizieren der Peers verfügbar.<br /><br /> Verbindungshandshake wartet auf Meldung zur Bestätigung der Sicherheitskontextinitialisierung.<br /><br /> Verbindungshandshake wartet auf Meldung zur Bestätigung der Sicherheitskontextannahme.<br /><br /> Verbindungshandshake wartet auf SSPI-Ablehnungsmeldung zur fehlgeschlagenen Authentifizierung.<br /><br /> Verbindungshandshake wartet auf Meldung für Vorstufe des geheimen Hauptschlüssels.<br /><br /> Verbindungshandshake wartet auf Überprüfungsmeldung.<br /><br /> Verbindungshandshake wartet auf Vermittlungsmeldung.<br /><br /> Verbindungshandshake wurde abgeschlossen und ist online (bereit) für Nachrichtenaustausch.<br /><br /> Verbindungsfehler.|  
|**peer_certificate_id**|**int**|ID des lokalen Objekts für das Zertifikat, das von der Remoteinstanz zur Authentifizierung verwendet wird. Der Besitzer dieses Zertifikats muss über CONNECT-Berechtigungen für den [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Endpunkt verfügen. Lässt NULL-Werte zu.|  
|**encryption_algorithm**|**smallint**|Der für diese Verbindung verwendete Verschlüsselungsalgorithmus. Lässt NULL-Werte zu. Mögliche Werte:<br /><br /> **Wert &#124; Beschreibung &#124; entsprechende DDL-Option**<br /><br /> 0 &#124; keine &#124; deaktiviert.<br /><br /> 1 &#124; nur Signierung<br /><br /> 2 &#124; AES, RC4 &#124; erforderlich &#124; erforderlicher Algorithmus RC4}<br /><br /> 3 &#124; AES-&#124;erforderlicher Algorithmus AES<br /><br /> **Hinweis:** Der RC4-Algorithmus wird nur aus Gründen der Abwärtskompatibilität unterstützt. Neues Material kann nur mit RC4 oder RC4_128 verschlüsselt werden, wenn die Datenbank den Kompatibilitätsgrad 90 oder 100 besitzt. (Nicht empfohlen.) Verwenden Sie stattdessen einen neueren Algorithmus, z. B. einen der AES-Algorithmen. In [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höheren Versionen kann mit RC4 oder RC4_128 verschlüsseltes Material in jedem Kompatibilitätsgrad entschlüsselt werden.|  
|**encryption_algorithm_desc**|**nvarchar(60)**|Textdarstellung des Verschlüsselungsalgorithmus. Lässt NULL-Werte zu. Mögliche Werte:<br /><br /> **Beschreibung &#124; entsprechende DDL-Option**<br /><br /> Keine &#124; deaktiviert.<br /><br /> RC4-&#124; {Required &#124; erforderlicher Algorithmus RC4}<br /><br /> AES-&#124; erforderlicher Algorithmus AES<br /><br /> Keine, RC4 &#124; {unterstützt &#124; unterstützter Algorithmus RC4}<br /><br /> Keine, AES-&#124; unterstützter Algorithmus RC4<br /><br /> RC4, AES-&#124; erforderlicher Algorithmus RC4 AES<br /><br /> AES, RC4 &#124; erforderlichen Algorithmus AES RC4<br /><br /> None, RC4, AES &#124; unterstützter Algorithmus RC4 AES<br /><br /> None, AES, RC4 &#124; unterstützter Algorithmus AES RC4|  
|**receives_posted**|**smallint**|Die Anzahl asynchroner Netzwerkempfangsvorgänge, die für diese Verbindung noch nicht abgeschlossen wurden. Lässt NULL-Werte zu.|  
|**is_receive_flow_controlled**|**bit**|Angabe, ob Netzwerkempfangsvorgänge aus Gründen der Datenflusskontrolle verschoben wurden, da das Netzwerk ausgelastet ist. Lässt NULL-Werte zu.<br /><br /> 1 = True|  
|**sends_posted**|**smallint**|Die Anzahl asynchroner Netzwerksendevorgänge, die für diese Verbindung noch nicht abgeschlossen wurden. Lässt NULL-Werte zu.|  
|**is_send_flow_controlled**|**bit**|Angabe, ob Netzwerksendevorgänge aus Gründen der Datenflusskontrolle verschoben wurden, da das Netzwerk ausgelastet ist. Lässt NULL-Werte zu.<br /><br /> 1 = True|  
|**total_bytes_sent**|**bigint**|Gesamtanzahl der von dieser Verbindung gesendeten Bytes. Lässt NULL-Werte zu.|  
|**total_bytes_received**|**bigint**|Die Gesamtanzahl der von dieser Verbindung empfangenen Bytes. Lässt NULL-Werte zu.|  
|**total_fragments_sent**|**bigint**|Die Gesamtanzahl der von dieser Verbindung gesendeten [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Nachrichtenfragmente. Lässt NULL-Werte zu.|  
|**total_fragments_received**|**bigint**|Die Gesamtanzahl der von dieser Verbindung empfangenen [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Nachrichtenfragmente. Lässt NULL-Werte zu.|  
|**total_sends**|**bigint**|Die Gesamtanzahl der von dieser Verbindung ausgegebenen Netzwerksendeanforderungen. Lässt NULL-Werte zu.|  
|**total_receives**|**bigint**|Die Gesamtanzahl der von dieser Verbindung ausgegebenen Netzwerk Empfangs Anforderungen. Lässt NULL-Werte zu.|  
|**peer_arbitration_id**|**uniqueidentifier**|Interner Bezeichner für den Endpunkt. Lässt NULL-Werte zu.|  
  
## <a name="permissions"></a>Berechtigungen  
 Erfordert die VIEW SERVER STATE-Berechtigung auf dem Server.  
  
## <a name="physical-joins"></a>Physische Joins  
 ![Joins für sys.dm_broker_connections](../../relational-databases/system-dynamic-management-views/media/join-dm-broker-connections-1.gif "Joins für sys.dm_broker_connections")  
  
## <a name="relationship-cardinalities"></a>Kardinalität der Beziehungen  
  
|From|To|Beziehung|  
|----------|--------|------------------|  
|**dm_broker_connections.connection_id**|**dm_exec_connections.connection_id**|1:1|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Dynamische Verwaltungssichten und -funktionen &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Dynamische Verwaltungssichten in Verbindung mit Service Broker &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/service-broker-related-dynamic-management-views-transact-sql.md)  
  
  

