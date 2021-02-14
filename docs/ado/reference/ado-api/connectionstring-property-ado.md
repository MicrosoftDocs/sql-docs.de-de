---
description: ConnectionString-Eigenschaft (ADO)
title: ConnectionString-Eigenschaft (ADO) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::ConnectionString
helpviewer_keywords:
- ConnectionString property [ADO]
ms.assetid: 3be75b75-4d36-4479-ab64-9a456869252a
author: rothja
ms.author: jroth
ms.openlocfilehash: 3f8fd13b1f00e7ccebf3b334573948d76aeb7242
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100034640"
---
# <a name="connectionstring-property-ado"></a>ConnectionString-Eigenschaft (ADO)
Gibt die Informationen an, die zum Herstellen einer Verbindung mit einer Datenquelle verwendet werden.  
  
## <a name="settings-and-return-values"></a>Einstellungen und Rückgabewerte  
 Legt einen **Zeichen** folgen Wert fest oder gibt ihn zurück.  
  
## <a name="remarks"></a>Bemerkungen  
 Verwenden Sie die **ConnectionString** -Eigenschaft zum Angeben einer Datenquelle, indem Sie eine ausführliche Verbindungs Zeichenfolge mit einer Reihe von *Argument* *= value* -Anweisungen übergeben, die durch Semikolons getrennt sind.  
  
 ADO unterstützt fünf Argumente für die **ConnectionString** -Eigenschaft. alle anderen Argumente werden direkt an den Anbieter übergeben, ohne dass Sie von ADO verarbeitet werden. Die von ADO unterstützten Argumente lauten wie folgt.  
  
|Argument|BESCHREIBUNG|  
|--------------|-----------------|  
|*Anbieter =*|Gibt den Namen eines Anbieters an, der für die Verbindung verwendet werden soll.|  
|*Dateiname =*|Gibt den Namen einer anbieterspezifischen Datei an (z. b. ein persistente Datenquellen Objekt), die voreingestellte Verbindungsinformationen enthält.|  
|*Remote Anbieter =*|Gibt den Namen eines Anbieters an, der beim Öffnen einer Client seitigen Verbindung verwendet werden soll. (Nur Remote Datendienst.)|  
|*Remote Server =*|Gibt den Pfadnamen des Servers an, der beim Öffnen einer Client seitigen Verbindung verwendet werden soll. (Nur Remote Datendienst.)|  
|*URL =*|Gibt die Verbindungs Zeichenfolge als absolute URL an, die eine Ressource identifiziert, z. b. eine Datei oder ein Verzeichnis.|  
  
 Nachdem Sie die **ConnectionString** -Eigenschaft festgelegt und das [Verbindungs](./connection-object-ado.md) Objekt geöffnet haben, kann der Anbieter den Inhalt der-Eigenschaft ändern, indem Sie z. b. die ADO-definierten Argument Namen ihren Entsprechungen für den jeweiligen Anbieter zuordnet.  
  
 Die **ConnectionString** -Eigenschaft erbt automatisch den Wert, der für das *ConnectionString* -Argument der [Open](./open-method-ado-connection.md) -Methode verwendet wird, sodass Sie die aktuelle **ConnectionString** -Eigenschaft während des **öffnenden** Methoden Aufrufes überschreiben können.  
  
 Da das *File Name* -Argument bewirkt, dass ADO den zugeordneten Anbieter lädt, können Sie nicht die *Anbieter* -und *Dateinamen* Argumente übergeben.  
  
 Die **ConnectionString** -Eigenschaft ist Lese-/Schreibzugriff, wenn die Verbindung geschlossen und schreibgeschützt ist, wenn Sie geöffnet ist.  
  
 Duplikate eines Arguments in der **ConnectionString** -Eigenschaft werden ignoriert. Die letzte Instanz eines beliebigen Arguments wird verwendet.  
  
> [!NOTE]
>  **Verwendung von Remote Datendiensten** Wenn die **ConnectionString** -Eigenschaft für ein Client seitiges **Verbindungs** Objekt verwendet wird, kann Sie nur den *Remote Anbieter* -und den *Remote Server* Parameter enthalten.  
  
 In der folgenden Tabelle sind die Standard-ADO-Anbieter für die einzelnen Windows-Betriebssysteme aufgeführt:  
  
|Standard-ADO-Anbieter|Windows-Betriebssystem|  
|--------------------------|------------------------------|  
|MSDASQL<br /><br /> (Um die Lesbarkeit des Quellcodes zu verbessern, geben Sie den Anbieter Namen explizit in der Verbindungs Zeichenfolge an.)|Windows 2000 (32 Bit)<br /><br /> Windows XP (32-Bit)<br /><br /> Windows 2003 Server (32 Bit)<br /><br /> Windows Vista (32-Bit)<br /><br /> Windows Vista Service Pack 1 oder höher (32-Bit und 64-Bit)<br /><br /> Windows-Versionen nach Windows Vista (32-Bit und 64-Bit)|  
|Keine Standardeinstellung.<br /><br /> Wenn eine ADO-Anwendung unter den folgenden Betriebssystemen ausgeführt wird und den Anbieter nicht explizit angibt, gibt ADO den folgenden Fehler zurück: "ADODB. Verbindung: der Anbieter ist nicht angegeben, und es ist kein festgelegter Standardanbieter vorhanden.|Windows 2000 (64 Bit)<br /><br /> Windows XP (64-Bit)<br /><br /> Windows 2003 Server (64 Bit)<br /><br /> Windows Vista (64-Bit)|  
  
## <a name="applies-to"></a>Gilt für  
 [Connection-Objekt (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Beispiel für ConnectionString, ConnectionTimeout und State Properties (VB)](./connectionstring-connectiontimeout-and-state-properties-example-vb.md)   
 [Beispiel für ConnectionString, ConnectionTimeout und State Properties (VC + +)](./connectionstring-connectiontimeout-and-state-properties-example-vc.md)   
 [Anhang A: Anbieter](../../guide/appendixes/appendix-a-providers.md)