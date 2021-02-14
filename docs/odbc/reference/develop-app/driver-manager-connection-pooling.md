---
description: Verbindungspooling des Treiber-Managers
title: Treiber-Manager-Verbindungs Pooling | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- connection pooling [ODBC]
- pooled connections [ODBC]
- connecting to driver [ODBC], connection pooling
- connecting to data source [ODBC], connection pooling
ms.assetid: ee95ffdb-5aa1-49a3-beb2-7695b27c3df9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8876696412360275fcf443a5543634c9ac537df6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070335"
---
# <a name="driver-manager-connection-pooling"></a>Verbindungspooling des Treiber-Managers
Das Verbindungspooling ermöglicht einer Anwendung, eine Verbindung aus einem Pool von Verbindungen zu verwenden, die nicht für jede Verwendung wieder hergestellt werden müssen. Nachdem eine Verbindung erstellt und in einen Pool eingefügt wurde, kann eine Anwendung diese Verbindung wieder verwenden, ohne den gesamten Verbindungsprozess auszuführen.  
  
 Die Verwendung einer gepoolten Verbindung kann zu erheblichen Leistungssteigerungen führen, da Anwendungen den Aufwand für die Herstellung einer Verbindung einsparen können. Dies kann besonders für Anwendungen der mittleren Ebene von Bedeutung sein, die über ein Netzwerk verbunden sind, oder für Anwendungen, die wiederholt Verbindungen herstellen und trennen, z. b. Internet Anwendungen.  
  
 Zusätzlich zu den Leistungssteigerungen ermöglicht die Verbindungspooling-Architektur, dass eine Umgebung und die zugehörigen Verbindungen von mehreren Komponenten in einem einzelnen Prozess verwendet werden können. Dies bedeutet, dass eigenständige Komponenten im gleichen Prozess miteinander interagieren können, ohne sich gegenseitig zu kennen. Eine Verbindung in einem Verbindungspool kann von mehreren Komponenten wiederholt verwendet werden.  
  
> [!NOTE]
>  Verbindungspooling kann von einer ODBC-Anwendung verwendet werden, die ODBC 2 ausstellt. *x* -Verhalten, sofern die Anwendung *SQLSetEnvAttr* aufgerufen werden kann. Wenn Sie das Verbindungspooling verwenden, dürfen von der Anwendung keine SQL-Anweisungen ausgeführt werden, die die Datenbank oder den Kontext der Datenbank ändern, z. b. das Ändern von \<*database name*> , wodurch der von einer Datenquelle verwendete Katalog geändert wird.  


 Ein ODBC-Treiber muss vollständig Thread sicher sein, und Verbindungen dürfen keine Thread Affinität aufweisen, um das Verbindungspooling zu unterstützen. Dies bedeutet, dass der Treiber jederzeit einen Aufruf für einen beliebigen Thread verarbeiten kann und eine Verbindung mit einem Thread herstellen kann, die Verbindung in einem anderen Thread verwendet und die Verbindung mit einem dritten Thread getrennt werden kann.  
  
 Der Verbindungspool wird vom Treiber-Manager verwaltet. Verbindungen werden aus dem Pool gezeichnet, wenn die Anwendung **SQLCONNECT** oder **SQLDriverConnect** aufruft und an den Pool zurückgegeben wird, wenn die Anwendung **SQLDisconnect** aufruft. Die Größe des Pools wird basierend auf den angeforderten Ressourcen Zuordnungen dynamisch vergrößert. Die Verkleinerung erfolgt basierend auf dem Inaktivitäts Timeout: Wenn eine Verbindung für einen bestimmten Zeitraum inaktiv ist (Sie wurde nicht in einer Verbindung verwendet), wird Sie aus dem Pool entfernt. Die Größe des Pools wird nur durch Arbeitsspeicher Einschränkungen und-Einschränkungen auf dem Server beschränkt.  
  
 Der Treiber-Manager bestimmt, ob eine bestimmte Verbindung in einem Pool gemäß den in **SQLCONNECT** oder **SQLDriverConnect** weiter gegebenen Argumenten und gemäß den Verbindungs Attributen, die nach dem Zuordnen der Verbindung festgelegt wurden, verwendet werden soll.  
  
 Wenn der Treiber-Manager Verbindungen bündelt, muss er ermitteln können, ob eine Verbindung noch funktioniert, bevor die Verbindung hergestellt wird. Andernfalls übergibt der Treiber-Manager immer dann, wenn ein vorübergehender Netzwerkfehler auftritt, die unzustellbare Verbindung mit der Anwendung. In ODBC 3 *. x*: SQL_ATTR_CONNECTION_DEAD wurde ein neues Verbindungs Attribut definiert. Dabei handelt es sich um ein Schreib geschütztes Verbindungs Attribut, das entweder SQL_CD_TRUE oder SQL_CD_FALSE zurückgibt. Der Wert SQL_CD_TRUE bedeutet, dass die Verbindung verloren gegangen ist, während der Wert SQL_CD_FALSE bedeutet, dass die Verbindung noch aktiv ist. (Treiber, die früheren Versionen von ODBC entsprechen, können dieses Attribut ebenfalls unterstützen.)  
  
 Diese Option muss von einem Treiber effizient implementiert werden, da die Leistung des Verbindungspools beeinträchtigt wird. Ein Aufruf zum Abrufen dieses Verbindungs Attributs sollte insbesondere keinen Roundtrip zum Server auslösen. Stattdessen sollte ein Treiber nur den letzten bekannten Status der Verbindung zurückgeben. Die Verbindung ist nicht erfolgreich, wenn der letzte Trip zum Server fehlgeschlagen ist, und nicht, wenn der letzte Trip erfolgreich war.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn eine Verbindung getrennt wurde (über SQL_ATTR_CONNECTION_DEAD gemeldet), zerstört der ODBC-Treiber-Manager diese Verbindung, indem SQLDisconnect im Treiber aufgerufen wird. Neue Verbindungsanforderungen finden möglicherweise keine verwendbare Verbindung im Pool. Schließlich kann der Treiber-Manager eine neue Verbindung herstellen, vorausgesetzt, der Pool ist leer.  
  
 Um einen Verbindungspool zu verwenden, führt eine Anwendung die folgenden Schritte aus:  
  
1.  Aktiviert das Verbindungspooling durch Aufrufen von **SQLSetEnvAttr** , um das SQL_ATTR_CONNECTION_POOLING-Umgebungs Attribut auf SQL_CP_ONE_PER_DRIVER oder SQL_CP_ONE_PER_HENV festzulegen. Dieser Rückruf muss erfolgen, bevor die Anwendung die freigegebene Umgebung zuordnet, für die das Verbindungspooling aktiviert werden soll. Das Umgebungs Handle im-Befehl von **SQLSetEnvAttr** sollte auf NULL festgelegt werden, wodurch SQL_ATTR_CONNECTION_POOLING ein Attribut auf Prozessebene ist. Wenn das-Attribut auf SQL_CP_ONE_PER_DRIVER festgelegt ist, wird ein einzelner Verbindungspool für jeden Treiber unterstützt. Wenn eine Anwendung mit vielen Treibern und wenigen Umgebungen funktioniert, ist dies möglicherweise effizienter, da möglicherweise weniger Vergleiche erforderlich sind. Wenn SQL_CP_ONE_PER_HENV festgelegt ist, wird für jede Umgebung ein einzelner Verbindungspool unterstützt. Wenn eine Anwendung mit vielen Umgebungen und wenigen Treibern funktioniert, ist dies möglicherweise effizienter, da möglicherweise weniger Vergleiche erforderlich sind. Das Verbindungspooling wird deaktiviert, indem SQL_ATTR_CONNECTION_POOLING auf SQL_CP_OFF festgelegt wird.  
  
2.  Ordnet eine Umgebung zu, indem **sqlzuordchandle** aufgerufen wird, wobei das Argument " *Lenker* " auf SQL_HANDLE_ENV festgelegt ist. Die durch diesen-Befehl zugeordnete Umgebung ist eine implizite freigegebene Umgebung, da Verbindungspooling aktiviert wurde. Die zu verwendende Umgebung wird jedoch erst festgelegt, wenn **sqlzuzuordchandle** mit dem *Typ* "SQL_HANDLE_DBC" für diese Umgebung aufgerufen wird.  
  
3.  Reserviert eine Verbindung durch Aufrufen von **sqlzuordchandle** , wobei *InputHandle* auf SQL_HANDLE_DBC festgelegt ist, und *InputHandle* auf das Umgebungs Handle, das für das Verbindungspooling reserviert ist. Der Treiber-Manager versucht, eine vorhandene Umgebung zu finden, die mit den von der Anwendung festgelegten Umgebungs Attributen übereinstimmt. Wenn keine solche Umgebung vorhanden ist, wird eine solche Umgebung mit einem Verweis Zähler (verwaltet vom Treiber-Manager) von 1 erstellt. Wenn eine übereinstimmende freigegebene Umgebung gefunden wird, wird die Umgebung an die Anwendung zurückgegeben und der Verweis Zähler erhöht. (Die tatsächliche Verbindung, die verwendet werden soll, wird erst vom Treiber-Manager bestimmt, wenn **SQLCONNECT** oder **SQLDriverConnect** aufgerufen wird.)  
  
4.  Ruft **SQLCONNECT** oder **SQLDriverConnect** auf, um die Verbindung herzustellen. Der Treiber-Manager verwendet die Verbindungsoptionen im Befehl **SQLCONNECT** (oder die Verbindungs Schlüsselwörter im Befehl **SQLDriverConnect**) und die Verbindungs Attribute, die nach der Verbindungs Zuweisung festgelegt sind, um zu bestimmen, welche Verbindung im Pool verwendet werden soll.  
  
    > [!NOTE]  
    >  Die Art und Weise, wie eine angeforderte Verbindung mit einer gepoolten Verbindung abgeglichen wird, hängt vom SQL_ATTR_CP_MATCH Environment-Attribut ab Weitere Informationen finden Sie unter [sqlsertenvattr](../../../odbc/reference/syntax/sqlsetenvattr-function.md).  
  
     ODBC-Anwendungen, die das Verbindungspooling verwenden, sollten während der Anwendungs Initialisierung [CoInitializeEx](/windows/win32/api/combaseapi/nf-combaseapi-coinitializeex) aufrufen und bei Schließen der Anwendung [initialisieren](/windows/win32/api/combaseapi/nf-combaseapi-couninitialize) .  
  
5.  Ruft **SQLDisconnect** auf, wenn die Verbindung abgeschlossen ist. Die Verbindung wird an den Verbindungspool zurückgegeben und steht zur erneuten Verwendung zur Verfügung.  
  
 Eine ausführliche Erläuterung finden Sie unter [Pooling in den Microsoft Data Access Components](/previous-versions/ms810829(v=msdn.10)).  
  
## <a name="connection-pooling-considerations"></a>Überlegungen zum Verbindungs Pooling  
 Das Ausführen einer der folgenden Aktionen mithilfe eines SQL-Befehls (anstelle der ODBC-API) kann den Status der Verbindung beeinflussen und unerwartete Probleme verursachen, wenn Verbindungspooling aktiv ist:  
  
-   Öffnen einer Verbindung und Ändern der Standarddatenbank.  
  
-   Verwenden Sie die Set-Anweisung, um konfigurierbare Optionen (einschließlich SET ROWCOUNT, ANSI_NULL, IMPLICIT_TRANSACTIONS, Showplan, Statistics, TEXTSIZE und DATEFORMAT) zu ändern.  
  
-   Erstellen temporärer Tabellen und gespeicherter Prozeduren.  
  
 Wenn eine dieser Aktionen außerhalb der ODBC-API durchgeführt wird, erbt die nächste Person, die die Verbindung verwendet, die vorherigen Einstellungen, Tabellen oder Prozeduren automatisch.  
  
> [!NOTE]  
>  Es ist nicht zu erwarten, dass bestimmte Einstellungen im Verbindungsstatus vorhanden sind. Sie sollten immer den Verbindungsstatus in der Anwendung festlegen und sicherstellen, dass die Anwendung alle nicht verwendeten Verbindungspooling-Einstellungen entfernt.  
  
## <a name="driver-aware-connection-pooling"></a>Treiberfähiges Verbindungspooling  
 Ab Windows 8 kann ein ODBC-Treiber Verbindungen im Pool effizienter nutzen. Weitere Informationen finden Sie unter [Treiber fähiges Verbindungs Pooling](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Herstellen einer Verbindung mit einer Datenquelle oder einem Treiber](../../../odbc/reference/develop-app/connecting-to-a-data-source-or-driver.md)   
 [Entwickeln eines ODBC-Treibers](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Pooling in den Microsoft Data Access-Komponenten](/previous-versions/ms810829(v=msdn.10))