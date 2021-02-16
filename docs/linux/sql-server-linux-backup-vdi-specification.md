---
title: VDI-Sicherungsspezifikation für SQL Server für Linux
description: Lernen Sie die Schnittstellen kennen, die vom SDK des VDI-Clients für SQL Server für Linux bereitgestellt werden.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 0250ba2b-8cdd-450e-9109-bf74f70e1247
ms.openlocfilehash: dcdbd7ba3a00048b63836e52092835fe6f7a618f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343178"
---
# <a name="sql-server-on-linux-vdi-client-sdk-specification"></a>Spezifikation des VDI-Client SDK für SQL Server für Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

In diesem Dokument werden die Schnittstellen beschrieben, die vom VDI-Client SDK (Client SDK für Schnittstellen virtueller Geräte) bereitgestellt werden. Unabhängige Softwarehersteller können mit der API für virtuelle Sicherungsmedien SQL Server in ihre eigenen Produkte integrieren. Das Verhalten von VDI für Linux ähnelt dem von VDI für Windows. Folgende Unterschiede müssen jedoch beachtet werden:

- Anstelle des gemeinsam verwendeten Windows-Speichers wird der gemeinsam verwendete POSIX-Speicher genutzt.
- Anstelle von Windows-Semaphoren werden POSIX-Sempahore verwendet.
- Anstelle von Windows-Typen wie HRESULT und DWORD werden die entsprechenden Integertypen verwendet.
- Die COM-Schnittstellen wurden entfernt und durch C++-Klassen ersetzt.
- SQL Server für Linux unterstützt keine benannten Instanzen. Verweise auf Instanznamen wurden daher entfernt. 
- Die gemeinsam verwendete Bibliothek ist in „libsqlvdi.so“ implementiert und unter /opt/mssql/lib/libsqlvdi.so installiert.

Dieses Dokument ist ein Nachtrag zur Datei **vbackup.chm**, in dem die VDI-Spezifikation für MS SQL Server unter Windows enthalten ist. Laden Sie die [SQL-VDI-Spezifikation für Windows](https://www.microsoft.com/download/details.aspx?id=17282) herunter.

Ein Beispiel für eine VDI-Sicherungslösung finden Sie im [GitHub-Repository für SQL Server-Beispiele](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sqlvdi-linux).

## <a name="user-permissions-setup"></a>Einrichten der Benutzerberechtigungen

Unter Linux sind primitive POSIX-Typen dem Benutzer, der sie erstellt, und ihrer Standardgruppe zugeordnet. Im Fall von Objekten, die von SQL Server erstellt werden, sind diese primitiven Typen standardmäßig dem mssql-Benutzer und der mssql-Gruppe zugeordnet. Damit diese Typen gemeinsam von SQL Server und dem VDI-Client verwendet werden können, empfiehlt sich eine der folgenden Methoden:

1. Führen Sie den VDI-Client als Benutzer „mssql“ aus.
   
   Führen Sie den folgenden Befehl aus, um zum Benutzer „mssql“ zu wechseln:
   
   ```bash
   sudo su mssql
   ```

2. Fügen Sie den Benutzer „mssql“ der Gruppe von„vdiuser“ hinzu. Fügen Sie anschließend diese Gruppe der Gruppe „mssql“ hinzu.
   
   Führen Sie die folgenden Befehle aus:

   ```bash
   sudo useradd vdiuser
   sudo usermod -a -G mssql vdiuser
   sudo usermod -a -G vdiuser mssql
   ```

   Starten Sie den Server neu, damit die neuen Gruppen für SQL Server und „vdiuser“ übernommen werden.

## <a name="client-functions"></a>Clientfunktionen

In diesem Abschnitt werden die einzelnen Clientfunktionen beschrieben. Die Beschreibungen enthalten die folgenden Informationen:

- Zweck der Funktion
- Funktionssyntax
- Parameterliste
- Rückgabewerte
- Bemerkungen

## <a name="clientvirtualdevicesetcreate"></a>ClientVirtualDeviceSet::Create

**Zweck:** Diese Funktion erstellt die Gruppe virtueller Geräte.

**Syntax**
   ```
   int ClientVirtualDeviceSet::Create (
   char *   name,       // name for the set
   VDConfig   * cfg     // configuration for the set
   );
   ```

| Parameter | Argument | Erklärung
| ----- | ----- | ------ |
| | **name** | Mit diesem Argument wird die Gruppe virtueller Geräte identifiziert. Die Benennungsregeln, die von „CreateFileMapping()“ verwendet werden, müssen beachtet werden. Jedes Zeichen außer dem umgekehrten Schrägstrich (\) kann verwendet werden. Bei dem Argument handelt sich um eine Zeichenfolge. Es wird empfohlen, der Zeichenfolge den Produkt- oder Firmennamen des Benutzers und den Datenbanknamen voranzustellen. |
| |**cfg** | Dieses Argument ist die Konfiguration für die Gruppe virtueller Geräte. Zusätzliche Informationen finden Sie weiter unten in diesem Dokument im Konfigurationsabschnitt.

| Rückgabewerte | Argument | Erklärung
| ----- | ----- | ------ |
| |**NOERROR** | Die Funktion wurde erfolgreich ausgeführt. |
| |**VD_E_NOTSUPPORTED** |Mindestens ein Feld in der Konfiguration war ungültig oder wurde nicht unterstützt. |
| |**VD_E_PROTOCOL** | Die Gruppe virtueller Geräte ist bereits vorhanden.

**Hinweise:** Die „Create“-Methode sollte nur einmal pro BACKUP- oder RESTORE-Vorgang ausgeführt werden. Nachdem die „Close“-Methode aufgerufen wurde, kann der Client die Schnittstelle wiederverwenden, um eine weitere Gruppe virtueller Geräte zu erstellen.

## <a name="clientvirtualdevicesetgetconfiguration"></a>ClientVirtualDeviceSet::GetConfiguration

**Zweck:** Diese Funktion wird verwendet, um solange zu warten, bis der Server die Gruppe virtueller Geräte konfiguriert hat.
**Syntax**
   ```
   int ClientVirtualDeviceSet::GetConfiguration (
   time_t       timeout,    // in milliseconds
   VDConfig *       cfg // selected configuration
   );
   ```

| Parameter | Argument | Erklärung
| ----- | ----- | ------ |
| | **timeout** | Mit diesem Argument wird der Timeout in Millisekunden angegeben. Mit INFINITE oder einer negativen ganzen Zahl können Sie einen Timeout verhindern.
| | **cfg** | Wenn die Ausführung erfolgreich ist, enthält dieses Argument die vom Server ausgewählte Konfiguration. Zusätzliche Informationen finden Sie weiter unten in diesem Dokument im Konfigurationsabschnitt.

| Rückgabewerte | Argument | Erklärung
| ----- | ----- | ------ |
| |**NOERROR** | Die Konfiguration wurde zurückgegeben.
| |**VD_E_ABORT** |„SignalAbort“ wurde aufgerufen.
| |**VD_E_TIMEOUT** |Bei der Funktionsausführung ist ein Timeout aufgetreten.

**Hinweise:** Diese Funktion wird in einem Warnzustand blockiert. Nach dem erfolgreichen Aufruf können die Geräte in der Gruppe virtueller Geräte geöffnet werden.


## <a name="clientvirtualdevicesetopendevice"></a>ClientVirtualDeviceSet::OpenDevice
**Zweck:** Diese Funktion öffnet eines der Geräte in der Gruppe virtueller Geräte.
**Syntax**
   ```
   int ClientVirtualDeviceSet::OpenDevice (
   char *           name,       // name for the set
   ClientVirtualDevice **       ppVirtualDevice // returns interface to device
   );
   ```

| Parameter | Argument | Erklärung
| ----- | ----- | ------ |
| | **name** |Mit diesem Argument wird die Gruppe virtueller Geräte identifiziert.
| | **ppVirtualDevice** |Wenn die Funktion erfolgreich ausgeführt wird, wird ein Zeiger auf das virtuelle Gerät zurückgegeben. Dieses Gerät wird für „GetCommand“ und „CompleteCommand“ verwendet.

| Rückgabewerte | Argument | Erklärung
| ----- | ----- | ------ |
| |**NOERROR** |Die Funktion wurde erfolgreich ausgeführt.
| |**VD_E_ABORT** | Ein Abbruch wurde angefordert.
| |**VD_E_OPEN** |  Alle Geräte sind geöffnet.
| |**VD_E_PROTOCOL** |  Die Gruppe befindet sich nicht im Initialisierungszustand, oder das Gerät ist bereits geöffnet.
| |**VD_E_INVALID** |Der Gerätename ist ungültig. Er lässt sich keinem Gerät der Gruppe zuordnen.

**Hinweise:** VD_E_OPEN kann möglicherweise ohne Probleme zurückgegeben werden. Der Client kann „OpenDevice“ mithilfe einer Schleife aufrufen, bis dieser Code zurückgegeben wird.
Wenn mehr als ein Gerät konfiguriert ist (z. B. *n* Geräte), werden von der Gruppe virtueller Geräte *n* eindeutige Geräteschnittstellen zurückgegeben.

Die `GetConfiguration`-Funktion kann verwendet werden, um solange zu warten, bis sich die Geräte öffnen lassen.
Wenn diese Funktion nicht erfolgreich ausgeführt wird, wird ein NULL-Wert über „ppVirtualDevice“ zurückgegeben.
 
## <a name="clientvirtualdevicegetcommand"></a>ClientVirtualDevice::GetCommand

**Zweck:** Mit dieser Funktion wird der nächste Befehl abgerufen, der sich in der Warteschlange des Geräts befindet. Wenn diese Funktion aufgerufen wird, wartet sie auf den nächsten Befehl.

**Syntax**
   ```
   int ClientVirtualDevice::GetCommand (
   time_t       timeout,    // time-out in milliseconds
   VDC_Command**    ppCmd   // returns the next command
   );
   ```

| Parameter | Argument | Erklärung
| ----- | ----- | ------ |
| |**timeout** |Mit diesem Argument wird die Wartezeit in Millisekunden angegeben. Mit INFINTE legen Sie eine unbegrenzte Wartezeit fest. Mit 0 rufen Sie einen Befehl ab. VD_E_TIMEOUT wird zurückgegeben, wenn aktuell kein Befehl verfügbar ist. Wenn der Timeout auftritt, entscheidet der Client, welche Aktion als Nächstes ausgeführt wird.
| |**Timeout** |Mit diesem Argument wird die Wartezeit in Millisekunden angegeben. Mit INFINTE oder einem negativen Wert legen Sie eine unbegrenzte Wartezeit fest. Mit 0 rufen Sie einen Befehl ab. VD_E_TIMEOUT wird zurückgegeben, wen kein Befehl vor Ablauf des Timeouts verfügbar ist. Wenn der Timeout auftritt, entscheidet der Client, welche Aktion als Nächstes ausgeführt wird.
| |**ppCmd** |Wenn ein Befehl erfolgreich zurückgegeben wird, gibt der Parameter die Adresse eines auszuführenden Befehls zurück. Der zurückgegebene Speicher ist schreibgeschützt. Nachdem der Befehl abgeschlossen wurde, wird dieser Zeiger der „CompleteCommand“-Routine übergeben. Ausführliche Informationen zu den einzelnen Befehlen finden Sie weiter unten in diesem Dokument im Befehlsabschnitt.
        
| Rückgabewerte | Argument | Erklärung
| ----- | ----- | ------ |
| |**NOERROR** |Ein Befehl wurde abgerufen.
| |**VD_E_CLOSE** |Das Gerät wurde vom Server geschlossen.
| |**VD_E_TIMEOUT** |Es war kein Befehl verfügbar, und der Timeout ist abgelaufen.
| |**VD_E_ABORT** |Der Client oder Server hat mit „SignalAbort“ das Herunterfahren erzwungen.

**Hinweise:** VD_E_CLOSE wird zurückgegeben, wenn SQL Server das Gerät schließt. Dieser Vorgang findet während des regulären Herunterfahrens statt. Nachdem alle Geräte geschlossen wurden, ruft der Client „ClientVirtualDeviceSet::Close“ auf, um die Gruppe virtueller Geräte zu schließen.
Wenn diese Routine blockiert werden muss, um auf einen Befehl zu warten, wird der Thread in den Warnzustand versetzt.

## <a name="clientvirtualdevicecompletecommand"></a>ClientVirtualDevice::CompleteCommand

**Zweck:** Mit dieser Funktion wird SQL Server benachrichtigt, dass ein Befehl abgeschlossen wurde. Für den Befehl sollten geeignete Abschlussinformationen zurückgegeben werden. Zusätzliche Informationen finden Sie weiter unten in diesem Dokument im Befehlsabschnitt.

**Syntax** 

   ```
   int ClientVirtualDevice::CompleteCommand (
   VDC_Command pCmd,        // the command
   int  completionCode,     // completion code
   unsigned long    bytesTransferred,   // bytes transferred
   int64_t  position        // current position
   );
   ```

| Parameter | Argument | Erklärung
| ----- | ----- | ------ |
| |**pCmd** |Mit diesem Argument wird die Adresse eines Befehls angegeben, der vorher von „ClientVirtualDevice::GetCommand“ zurückgegeben wurde.
| |**completionCode** |Mit diesem Argument wird ein Statuscode angegeben, der auf den Abschlussstatus hinweist. Dieser Parameter muss für alle Befehle zurückgegeben werden. Der zurückgegebene Code sollte für den Befehl geeignet sein, der ausgeführt wird. ERROR_SUCCESS wird immer verwendet, um einen erfolgreich ausgeführten Befehl anzugeben. Eine vollständige Liste der Codes finden Sie in der Datei „vdierror.h“. Eine Liste der typischen Statuscodes für jeden Befehl finden Sie weiter unten in diesem Dokument im Befehlsabschnitt.
| |**bytesTransferred** |Mit diesem Argument wird die Anzahl der erfolgreich übertragenen Bytes angegeben. Dieses Argument wird nur für Lese-und Schreibvorgänge zurückgegeben, die Datenübertragungsbefehle betreffen.
| |**position** |Mit diesem Argument wird ausschließlich eine Antwort für den „GetPosition“-Befehl angegeben.
        
| Rückgabewerte | Argument | Erklärung
| ----- | ----- | ------ |
| |**NOERROR** |Der Abschluss wurde korrekt erfasst.
| |**VD_E_INVALID** |„pCmd“ war kein aktiver Befehl.
| |**VD_E_ABORT** |Ein Abbruch wurde gemeldet.
| |**VD_E_PROTOCOL** |Das Gerät ist nicht geöffnet.

**Hinweise:** Keine.

## <a name="clientvirtualdevicesetsignalabort"></a>ClientVirtualDeviceSet::SignalAbort

**Zweck:** Mit dieser Funktion wird angegeben, dass ein Vorgang unplanmäßig beendet werden soll.

**Syntax** 

   ```
   int ClientVirtualDeviceSet::SignalAbort ();
   ```

| Parameter | Argument | Erklärung
| ----- | ----- | ------ |
| |Keine | Nicht verfügbar
        
| Rückgabewerte | Argument | Erklärung
| ----- | ----- | ------ |
| |**NOERROR**|Die Abbruchbenachrichtigung wurde erfolgreich bereitgestellt.

**Hinweise:** Der Client kann jederzeit den BACKUP- oder RESTORE-Vorgang abbrechen. Mit dieser Routine wird angegeben, dass alle Vorgänge beendet werden sollen. Die gesamte Gruppe virtueller Geräte wird in den Zustand einer unplanmäßigen Beendigung versetzt. Von Geräten werden keine weiteren Befehle zurückgegeben. Alle nicht abgeschlossenen Befehle werden automatisch abgeschlossen. Als Abschlusscode wird ERROR_OPERATION_ABORTED zurückgegeben. Sobald der Client ausstehende Vorgänge zur Verwendung von Clientpuffern sicher beendet, sollte er „ClientVirtualDeviceSet::Close“ aufrufen. Zusätzliche Informationen finden Sie weiter oben in diesem Dokument im Abschnitt zur unplanmäßigen Beendigung.

## <a name="clientvirtualdevicesetclose"></a>ClientVirtualDeviceSet::Close

**Zweck:** Mit dieser Funktion wird die Gruppe virtueller Geräte geschlossen, die durch „ClientVirtualDeviceSet::Create“ erstellt wurde. Dadurch werden alle Ressourcen freigegeben, die der Gruppe virtueller Geräte zugeordnet sind.

**Syntax** 

   ```
   int ClientVirtualDeviceSet::Close ();
   ```

| Parameter | Argument | Erklärung
| ----- | ----- | ------ |
| |Keine |Nicht verfügbar
        
| Rückgabewerte | Argument | Erklärung
| ----- | ----- | ------ |
| |**NOERROR** |Dieses Argument wird zurückgegeben, wenn die Gruppe virtueller Geräte erfolgreich geschlossen wurde.
| |**VD_E_PROTOCOL** |Es wurde keine Aktion ausgeführt, weil die Gruppe virtueller Geräte nicht geöffnet war.
| |**VD_E_OPEN** |Die Geräte waren noch geöffnet.

**Hinweise:** Wenn der Client „Close“ aufruft, wird damit darauf hingewiesen, dass alle Ressourcen, die von der Gruppe virtueller Geräte genutzt werden, freigegeben werden sollten. Der Client muss sicherstellen, dass alle Aktivitäten, die Datenpuffer und virtuelle Geräte betreffen, vor dem Aufrufen von „Close“ beendet werden. Dies liegt daran, dass durch „Close“ sämtliche von „OpenDevice“ zurückgegebene Schnittstellen virtueller Geräte ungültig werden.
Nach der Rückgabe des „Close“-Aufrufs hat der Client die Möglichkeit, einen „Create“-Aufruf an die Schnittstelle für die Gruppe virtueller Geräte zu senden. Durch einen solchen Aufruf wird eine Gruppe virtueller Geräte für einen nachfolgenden BACKUP- oder RESTORE-Vorgang erstellt.
Wenn „Close“ aufgerufen wird und noch mindestens ein virtuelles Gerät geöffnet ist, wird VD_E_OPEN zurückgegeben. In diesem Fall wird „SignalAbort“ intern ausgelöst, um nach Möglichkeit ein ordnungsgemäßes Herunterfahren zu gewährleisten. Dabei werden VDI-Ressourcen freigegeben. Der Client sollte auf jedem Gerät warten, bis die Angabe VD_E_CLOSE übermittelt wird. Erst danach sollte er „ClientVirtualDeviceSet::Close“ aufrufen. Wenn dem Client bekannt ist, dass sich die Gruppe virtueller Geräte bereits im Zustand der unplanmäßigen Beendigung befindet, sollte der Client nicht annehmen, dass die Angabe VD_E_CLOSE von „GetCommand“ übermittelt wird. Der Client hat die Möglichkeit, „ClientVirtualDeviceSet::Close“ aufzurufen, sobald die Aktivität im Zusammenhang mit den gemeinsam verwendeten Puffern beendet wird.
Zusätzliche Informationen finden Sie weiter oben in diesem Dokument im Abschnitt zur unplanmäßigen Beendigung.

## <a name="clientvirtualdevicesetopeninsecondary"></a>ClientVirtualDeviceSet::OpenInSecondary

**Zweck:** Mit dieser Funktion wird die Gruppe virtueller Geräte auf einem sekundären Client geöffnet. Der primäre Client muss vorher bereits „Create“ und „GetConfiguration“ verwendet haben, um die Gruppe virtueller Geräte einzurichten.

**Syntax** 
   
   ```
   int ClientVirtualDeviceSet::OpenInSecondary (
   char *   setName         // name of the set
   );
   ```

| Parameter | Argument | Erklärung
| ----- | ----- | ------ |
| |**setName** |Mit diesem Argument wird die Gruppe identifiziert. Die Groß-/Kleinschreibung dieses Namens wird beachtet. Der Name muss außerdem mit dem Namen übereinstimmen, der vom primären Client beim Aufruf von „ClientVirtualDeviceSet::Create“ verwendet wurde.

| Rückgabewerte | Argument | Erklärung
| ----- | ----- | ------ |
| |**NOERROR** |Die Funktion wurde erfolgreich ausgeführt.
| |**VD_E_PROTOCOL** |Die Gruppe virtueller Geräte wurde noch nicht erstellt, wurde bereits auf diesem Client geöffnet oder akzeptiert keine Öffnungsanforderungen von sekundären Clients.
| |**VD_E_ABORT** |Der Vorgang wird abgebrochen.

**Hinweise:** Wenn ein Modell mit mehreren Prozessen genutzt wird, ist der primäre Client dafür verantwortlich, eine planmäßige und unplanmäßige Beendigung der sekundären Clients zu erkennen.

## <a name="clientvirtualdevicesetgetbufferhandle"></a>ClientVirtualDeviceSet::GetBufferHandle

**Zweck:** Für einige Anwendungen sind möglicherweise mehrere Prozesse erforderlich, damit Puffer verarbeitet werden können, die von „ClientVirtualDevice::GetCommand“ zurückgegeben werden. In solchen Fällen kann der Prozess, der den Befehl empfängt, „GetBufferHandle“ verwenden, um ein prozessunabhängiges Handle abzurufen, das den Puffer identifiziert. Dieses Handle kann dann jedem Prozess übergeben werden, für den dieselbe Gruppe virtueller Geräte geöffnet ist. Anschließend kann der Prozess mit „ClientVirtualDeviceSet::MapBufferHandle“ die Adresse des Puffers abrufen. Bei der Adresse handelt es sich wahrscheinlich um eine andere Adresse als die im zugehörigen Partnerprozess, da jeder Prozess verschiedene Adressen verwenden kann, um Puffer zuzuordnen.

**Syntax** 

   ```
   int ClientVirtualDeviceSet::GetBufferHandle (
   uint8_t*     pBuffer,        // in: buffer address
   unsigned int*        pBufferHandle   // out: buffer handle
   );
   ```

| Parameter | Argument | Erklärung
| ----- | ----- | ------ |
| |**pBuffer** |Mit diesem Argument wird die Adresse eines Puffers angegeben, der von einem Lese-oder Schreibbefehl abgerufen wird.
| |**BufferHandle** |Ein eindeutiger Bezeichner für den Puffer wird zurückgegeben.

| Rückgabewerte | Argument | Erklärung
| ----- | ----- | ------ |
| |**NOERROR** |Die Funktion wurde erfolgreich ausgeführt.
| |**VD_E_PROTOCOL** |Die Gruppe virtueller Geräte ist aktuell nicht geöffnet.
| |**VD_E_INVALID** |„pBuffer“ ist keine gültige Adresse.

Hinweise: Der Prozess, der die „GetBufferHandle“-Funktion aufruft, ist auch dafür verantwortlich, „ClientVirtualDevice::CompleteCommand“ aufzurufen, sobald die Datenübertragung abgeschlossen wird.

## <a name="clientvirtualdevicesetmapbufferhandle"></a>ClientVirtualDeviceSet::MapBufferHandle

**Zweck:** Mit dieser Funktion wird eine Adresse eines gültigen Puffers aus einem Pufferhandle abgerufen, das wiederum aus einem anderen Prozess abgerufen wurde. 

**Syntax** 

   ```
   int ClientVirtualDeviceSet::MapBufferHandle (
   i        nt  dwBuffer,   // in: buffer handle
   uint8_t**    ppBuffer        // out: buffer address
   );
   ```

| Parameter | Argument | Erklärung
| ----- | ----- | ------ |
| |**dwBuffer** |Mit diesem Argument wird das Handle angegeben, das von „ClientVirtualDeviceSet::GetBufferHandle“ zurückgegeben wird.
| |**ppBuffer** |Mit diesem Argument wird die Adresse des Puffers angegeben, der im aktuellen Prozess gültig ist.

| Rückgabewerte | Argument | Erklärung
| ----- | ----- | ------ |
| |**NOERROR** |Die Funktion wurde erfolgreich ausgeführt.
| |**VD_E_PROTOCOL** |Die Gruppe virtueller Geräte ist aktuell nicht geöffnet.
| |**VD_E_INVALID** |„ppBuffer“ ist ein ungültiges Handle.

**Hinweise:** Die Handles müssen unbedingt korrekt übergeben werden, da sie für eine einzelne Gruppe virtueller Geräte nur lokal verfügbar sind. Die Partnerprozesse, die ein Handle gemeinsam nutzen, müssen sicherstellen, dass Pufferhandles nur innerhalb des Bereichs der Gruppe virtueller Geräte verwendet werden, aus dem der Puffer ursprünglich abgerufen wurde.


