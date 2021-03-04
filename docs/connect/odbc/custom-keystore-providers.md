---
title: Benutzerdefinierte Keystore-Anbieter
description: Erfahren Sie, wie Sie einen benutzerdefinierten Keystore-Anbieter für die Verwendung mit dem ODBC-Treiber für SQL Server und das Always Encrypted-Feature implementieren.
ms.custom: ''
ms.date: 07/12/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: a6166d7d-ef34-4f87-bd1b-838d3ca59ae7
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: f1f075c7ea2e80ec1760012c33ed9f719837c036
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837429"
---
# <a name="custom-keystore-providers"></a>Benutzerdefinierte Keystore-Anbieter
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

## <a name="overview"></a>Übersicht

Die Spaltenverschlüsselungsfunktion von SQL Server 2016 erfordert, dass die auf dem Server gespeicherten verschlüsselten Spaltenverschlüsselungsschlüssel (Encrypted Column Encryption Keys, ECEKs) vom Client abgerufen und anschließend in Spaltenverschlüsselungsschlüsseln (Column Encryption Keys, CEKs) entschlüsselt werden, um auf die in verschlüsselten Spalten gespeicherten Daten zuzugreifen. ECEKs werden mithilfe von Spaltenhauptschlüsseln (Column Master Keys, CMKs) verschlüsselt, und die Sicherheit des CMK ist für die Sicherheit der Spaltenverschlüsselung wichtig. Daher sollte der CMK an einem sicheren Speicherort gespeichert werden. Ein Spaltenverschlüsselungsschlüssel-Speicheranbieter hat den Zweck, eine Schnittstelle bereitzustellen, die dem ODBC-Treiber ermöglicht, auf diese sicher gespeicherten CMKs zuzugreifen. Für Benutzer mit einem eigenen sicheren Speicher bietet die Schnittstelle für benutzerdefinierte Keystore-Anbieter ein Framework für die Implementierung des Zugriffs auf den sicheren Speicher des CMK für den ODBC-Treiber, mit dem dann CEK-Verschlüsselung und -Entschlüsselung durchgeführt werden kann.

Jeder Keystore-Anbieter enthält und verwaltet einen oder mehrere CMKs, die durch Schlüsselpfade identifiziert werden – Zeichenfolgen in einem vom Anbieter definierten Format. Dieser CMK kann – zusammen mit dem Verschlüsselungsalgorithmus, auch eine vom Anbieter definierte Zeichenfolge – verwendet werden, um die Verschlüsselung eines CEK und die Entschlüsselung eines ECEK auszuführen. Der Algorithmus wird zusammen mit dem ECEK und dem Namen des Anbieters in den Verschlüsselungsmetadaten der Datenbank gespeichert. Weitere Informationen finden Sie unter [CREATE COLUMN MASTER KEY](../../t-sql/statements/create-column-master-key-transact-sql.md) und [CREATE COLUMN ENCRYPTION KEY](../../t-sql/statements/create-column-encryption-key-transact-sql.md). Die beiden grundlegenden Vorgänge der Schlüsselverwaltung sind folglich:

```
CEK = DecryptViaCEKeystoreProvider(CEKeystoreProvider_name, Key_path, Key_algorithm, ECEK)

-and-

ECEK = EncryptViaCEKeystoreProvider(CEKeyStoreProvider_name, Key_path, Key_algorithm, CEK)
```

Dabei wird der `CEKeystoreProvider_name` verwendet, um den spezifischen Spaltenverschlüsselungs-Keystore-Anbieter (Column Encryption Keystore Provider, CEKeystoreProvider) und die anderen vom CEKeystoreProvider zum Verschlüsseln/Entschlüsseln des (E)CEK verwendeten Argumente zu identifizieren. Der Name und der Schlüsselpfad werden von den CMK-Metadaten bereitgestellt, während der Algorithmus und der ECEK-Wert von den CEK-Metadaten bereitgestellt werden. Neben den integrierten Standardanbietern können mehrere Keystore-Anbieter vorhanden sein. Wenn Sie einen Vorgang ausführen, der den CEK erfordert, verwendet der Treiber die CMK-Metadaten, um den passenden Keystore-Anbieter anhand des Namens zu suchen, und führt den Entschlüsselungsvorgang aus, der wie folgt ausgedrückt werden kann:

```
CEK = CEKeyStoreProvider_specific_decrypt(Key_path, Key_algorithm, ECEK)
```

Der Treiber muss zwar keine CEKs verschlüsseln, aber ein Schlüsselverwaltungstool muss dies möglicherweise tun, um Vorgänge wie die CMK-Erstellung und -Rotation zu implementieren. Für diese Aktionen muss der umgekehrte Vorgang durchgeführt werden:

```
ECEK = CEKeyStoreProvider_specific_encrypt(Key_path, Key_algorithm, CEK)
```

### <a name="cekeystoreprovider-interface"></a>CEKeyStoreProvider-Schnittstelle

In diesem Dokument wird die CEKeyStoreProvider-Schnittstelle ausführlich beschrieben. Ein Keystore-Anbieter, der diese Schnittstelle implementiert, kann vom Microsoft ODBC Driver for SQL Server verwendet werden. CEKeyStoreProvider-Implementierungen können dieses Handbuch verwenden, um benutzerdefinierte Keystore-Anbieter zu entwickeln, die vom Treiber verwendet werden können.

Eine Keystore-Anbieterbibliothek („Anbieterbibliothek“) ist eine Dynamic Link Library, die vom ODBC-Treiber geladen werden kann und mindestens einen Keystore-Anbieter enthält. Das Symbol `CEKeystoreProvider` muss von einer Anbieterbibliothek exportiert werden und die Adresse eines mit NULL endenden Arrays von Zeigern auf `CEKeystoreProvider`-Strukturen sein, eine für jeden Keystore-Anbieter innerhalb der Bibliothek.

Eine `CEKeystoreProvider`-Struktur definiert die Einstiegspunkte eines einzelnen Keystore-Anbieters:

```
typedef struct CEKeystoreProvider {
    wchar_t *Name;
    int (*Init)(CEKEYSTORECONTEXT *ctx, errFunc *onError);
    int (*Read)(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int *len);
    int (*Write)(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int len);
    int (*DecryptCEK)(  CEKEYSTORECONTEXT *ctx,
                        errFunc *onError,
                        const wchar_t *keyPath,
                        const wchar_t *alg,
                        unsigned char *ecek,
                        unsigned short ecekLen,
                        unsigned char **cekOut,
                        unsigned short *cekLen);
    int (*EncryptCEK)(  CEKEYSTORECONTEXT *ctx,
                        errFunc *onError,
                        const wchar_t *keyPath,
                        const wchar_t *alg,
                        unsigned char *cek,
                        unsigned short cekLen,
                        unsigned char **ecekOut,
                        unsigned short *ecekLen);
    void (*Free)();
} CEKEYSTOREPROVIDER;
```

|Feldname|BESCHREIBUNG|
|:--|:--|
|`Name`|Der Name des Keystore-Anbieters. Er darf nicht mit einem anderen Keystore-Anbieter identisch sein, der zuvor vom Treiber geladen wurde oder in dieser Bibliothek vorhanden war. Auf NULL endende Zeichenfolge für breite* Zeichen.|
|`Init`|Initialisierungsfunktion. Wenn keine Initialisierungsfunktion erforderlich ist, ist dieses Feld möglicherweise NULL.|
|`Read`|Anbieterlesefunktion. Kann NULL sein, falls nicht erforderlich.|
|`Write`|Anbieterschreibfunktion. Erforderlich, wenn „Read“ nicht NULL ist. Kann NULL sein, falls nicht erforderlich.|
|`DecryptCEK`|ECEK-Entschlüsselungsfunktion. Diese Funktion ist der Grund für das Vorhandensein eines Keystore-Anbieters und darf nicht NULL sein.|
|`EncryptCEK`|CEK-Verschlüsselungsfunktion. Der Treiber ruft diese Funktion nicht auf, wird jedoch bereitgestellt, um den programmgesteuerten Zugriff auf die ECEK-Erstellung durch Schlüsselverwaltungstools zuzulassen. Kann NULL sein, falls nicht erforderlich.|
|`Free`|Beendigungsfunktion. Kann NULL sein, falls nicht erforderlich.|

Mit Ausnahme von „Free“ verfügen die Funktionen in dieser Schnittstelle über ein Parameterpaar, **ctx** und **onError**. Der erste identifiziert den Kontext, in dem die Funktion aufgerufen wird, der zweite wird zum Melden von Fehlern verwendet. Weitere Informationen finden Sie unter [Kontexte](#context-association) und [Fehlerbehandlung](#error-handling) weiter unten.

```
int Init(CEKEYSTORECONTEXT *ctx, errFunc onError);
```
Platzhaltername für eine anbieterdefinierte Initialisierungsfunktion. Der Treiber ruft diese Funktion einmal auf, nachdem ein Anbieter geladen wurde, aber bevor er sie zum ersten Mal benötigt, um ECEK-Entschlüsselungs- oder Read()-/Write()-Anforderungen auszuführen. Verwenden Sie diese Funktion, um jede erforderliche Initialisierung auszuführen.

|Argument|BESCHREIBUNG|
|:--|:--|
|`ctx`|[Eingabe] Vorgangskontext.|
|`onError`|[Eingabe] Fehlermeldefunktion.|
|`Return Value`|Gibt einen Wert ungleich 0 (null) zurück, um Erfolg anzuzeigen, oder 0 (null), um einen Fehler anzuzeigen.|

```
int Read(CEKEYSTORECONTEXT *ctx, errFunc onError, void *data, unsigned int *len);
```

Platzhaltername für eine anbieterdefinierte Kommunikationsfunktion. Der Treiber ruft diese Funktion auf, wenn die Anwendung das Lesen von Daten von einem Anbieter (auf den zuvor geschrieben wurde) mit dem SQL_COPT_SS_CEKEYSTOREDATA-Verbindungsattribut anfordert, sodass die Anwendung beliebige Daten vom Anbieter lesen kann. Weitere Informationen finden Sie unter [Kommunizieren mit Schlüsselspeicheranbietern](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#communicating-with-keystore-providers).

|Argument|BESCHREIBUNG|
|:--|:--|
|`ctx`|[Eingabe] Vorgangskontext.|
|`onError`|[Eingabe] Fehlermeldefunktion.|
|`data`|[Ausgabe] Zeiger auf einen Puffer, in den der Anbieter Daten schreibt, die von der Anwendung gelesen werden sollen. Dieser Puffer entspricht dem data-Feld der CEKEYSTOREDATA-Struktur.|
|`len`|[InOut] Zeiger auf einen Längenwert; bei der Eingabe ist dies die maximale Länge des Datenpuffers, und der Anbieter darf nicht mehr als „*len“ Bytes hineinschreiben. Bei der Rückgabe sollte der Anbieter „*len“ mit der Anzahl der geschriebenen Bytes aktualisieren.|
|`Return Value`|Gibt einen Wert ungleich 0 (null) zurück, um Erfolg anzuzeigen, oder 0 (null), um einen Fehler anzuzeigen.|

```
int Write(CEKEYSTORECONTEXT *ctx, errFunc onError, void *data, unsigned int len);
```
Platzhaltername für eine anbieterdefinierte Kommunikationsfunktion. Der Treiber ruft diese Funktion auf, wenn die Anwendung das Schreiben von Daten auf einen Anbieter mit dem SQL_COPT_SS_CEKEYSTOREDATA-Verbindungsattribut anfordert, sodass die Anwendung beliebige Daten auf den Anbieter schreiben kann. Weitere Informationen finden Sie unter [Kommunizieren mit Schlüsselspeicheranbietern](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#communicating-with-keystore-providers).

|Argument|BESCHREIBUNG|
|:--|:--|
|`ctx`|[Eingabe] Vorgangskontext.|
|`onError`|[Eingabe] Fehlermeldefunktion.|
|`data`|[Eingabe] Zeiger auf einen Puffer, der die Daten für den zu lesenden Anbieter enthält. Dieser Puffer entspricht dem data-Feld der CEKEYSTOREDATA-Struktur. Der Anbieter darf nicht mehr als „len“ Bytes aus diesem Puffer lesen.|
|`len`|[Eingabe] Die Anzahl von Bytes, die in den Daten verfügbar sind. Dies entspricht dem dataSize-Feld der CEKEYSTOREDATA-Struktur.|
|`Return Value`|Gibt einen Wert ungleich 0 (null) zurück, um Erfolg anzuzeigen, oder 0 (null), um einen Fehler anzuzeigen.|

```
int (*DecryptCEK)( CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg, unsigned char *ecek, unsigned short ecekLen, unsigned char **cekOut, unsigned short *cekLen);
```
Platzhaltername für eine anbieterdefinierte ECEK-Entschlüsselungsfunktion. Der Treiber ruft diese Funktion auf, um einen ECEK zu entschlüsseln, der durch einen CMK verschlüsselt ist, der mit diesem Anbieter in einem CEK verknüpft ist.

|Argument|BESCHREIBUNG|
|:--|:--|
|`ctx`|[Eingabe] Vorgangskontext.|
|`onError`|[Eingabe] Fehlermeldefunktion.|
|`keyPath`|[Eingabe] Der Wert des [KEY_PATH](../../t-sql/statements/create-column-master-key-transact-sql.md)-Metadatenattributs für den CMK, auf den der angegebene ECEK verweist. Auf NULL endende Zeichenfolge für breite* Zeichen. Dieser Wert dient zur Identifizierung eines von diesem Anbieter behandelten CMK.|
|`alg`|[Eingabe] Der Wert des [ALGORITHM](../../t-sql/statements/create-column-encryption-key-transact-sql.md)-Metadatenattributs für den angegebenen ECEK. Auf NULL endende Zeichenfolge für breite* Zeichen. Dieser Wert dient zur Identifizierung des Verschlüsselungsalgorithmus, der zum Verschlüsseln des gegebenen ECEK verwendet wird.|
|`ecek`|[Eingabe] Zeiger auf den zu entschlüsselnden ECEK.|
|`ecekLen`|[Eingabe] Länge des ECEK.|
|`cekOut`|[Ausgabe] Der Anbieter muss Speicher für den entschlüsselten ECEK zuweisen und seine Adresse in den Zeiger schreiben, auf den cekOut zeigt. Es muss möglich sein, diesen Speicherblock mithilfe der Funktion [LocalFree](/windows/desktop/api/winbase/nf-winbase-localfree) (Windows) oder „free“ (Linux/macOS) freizugeben. Wenn aufgrund eines Fehlers oder aus anderen Gründen kein Arbeitsspeicher zugeordnet wurde, muss der Anbieter *cekOut auf einen NULL-Zeiger festlegen.|
|`cekLen`|[Ausgabe] Der Anbieter muss in die Adresse, auf die cekLen verweist, die Länge des entschlüsselten ECEK schreiben, den er in **cekOut geschrieben hat.|
|`Return Value`|Gibt einen Wert ungleich 0 (null) zurück, um Erfolg anzuzeigen, oder 0 (null), um einen Fehler anzuzeigen.|

```
int (*EncryptCEK)( CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg, unsigned char *cek,unsigned short cekLen, unsigned char **ecekOut, unsigned short *ecekLen);
```
Platzhaltername für eine anbieterdefinierte CEK-Verschlüsselungsfunktion. Der Treiber ruft diese Funktion weder auf, noch macht er ihre Funktionalität über die ODBC-Schnittstelle verfügbar, doch sie wird bereitgestellt, um den programmgesteuerten Zugriff auf die ECEK-Erstellung durch Schlüsselverwaltungstools zuzulassen.

|Argument|BESCHREIBUNG|
|:--|:--|
|`ctx`|[Eingabe] Vorgangskontext.|
|`onError`|[Eingabe] Fehlermeldefunktion.|
|`keyPath`|[Eingabe] Der Wert des [KEY_PATH](../../t-sql/statements/create-column-master-key-transact-sql.md)-Metadatenattributs für den CMK, auf den der angegebene ECEK verweist. Auf NULL endende Zeichenfolge für breite* Zeichen. Dies dient zur Identifizierung eines von diesem Anbieter behandelten CMK.|
|`alg`|[Eingabe] Der Wert des [ALGORITHM](../../t-sql/statements/create-column-encryption-key-transact-sql.md)-Metadatenattributs für den angegebenen ECEK. Auf NULL endende Zeichenfolge für breite* Zeichen. Dieser Wert dient zur Identifizierung des Verschlüsselungsalgorithmus, der zum Verschlüsseln des gegebenen ECEK verwendet wird.|
|`cek`|[Eingabe] Zeiger auf den zu verschlüsselnden CEK.|
|`cekLen`|[Eingabe] Länge des CEK.|
|`ecekOut`|[Ausgabe] Der Anbieter muss Speicher für den verschlüsselten CEK zuweisen und seine Adresse in den Zeiger schreiben, auf den ecekOut zeigt. Es muss möglich sein, diesen Speicherblock mithilfe der Funktion [LocalFree](/windows/desktop/api/winbase/nf-winbase-localfree) (Windows) oder „free“ (Linux/macOS) freizugeben. Wenn aufgrund eines Fehlers oder aus anderen Gründen kein Arbeitsspeicher zugeordnet wurde, muss der Anbieter *ecekOut auf einen NULL-Zeiger festlegen.|
|`ecekLen`|[Ausgabe] Der Anbieter muss in die Adresse, auf die ecekLen verweist, die Länge des verschlüsselten CEK schreiben, den er in **ecekOut geschrieben hat.|
|`Return Value`|Gibt einen Wert ungleich 0 (null) zurück, um Erfolg anzuzeigen, oder 0 (null), um einen Fehler anzuzeigen.|

```
void (*Free)();
```
Platzhaltername für eine anbieterdefinierte Beendigungssfunktion. Der Treiber kann diese Funktion bei normaler Beendigung des Prozesses aufrufen.

> [!NOTE]
> *Breitzeichen-Zeichenfolgen sind 2-Byte-Zeichen (UTF-16) aufgrund der Art, in der sie von SQL Server gespeichert werden.*


### <a name="error-handling"></a>Fehlerbehandlung

Da während der Verarbeitung eines Anbieters Fehler auftreten können, wird ein Mechanismus bereitgestellt, der ermöglicht, Fehler detaillierter an den Treiber zurückzumelden, als wenn der Erfolg bzw. ein Fehler mit einem booleschen Wert gemeldet wird. Viele der Funktionen verfügen über ein Parameterpaar, **ctx** und **onError**, das zusätzlich zum Erfolg-/Fehler-Rückgabewert für diesen Zweck verwendet wird.

Der **ctx**-Parameter identifiziert den Kontext, in dem ein Anbietervorgang auftritt.

Der **onError**-Parameter verweist auf eine Fehlermeldefunktion mit folgendem Prototyp:

`typedef void errFunc(CEKEYSTORECONTEXT *ctx, const wchar_t *msg, ...);`

|Argument|BESCHREIBUNG|
|:--|:--|
|`ctx`|[Eingabe] Der Kontext, für den der Fehler gemeldet werden soll.|
|`msg`|[Eingabe] Die zu berichtende Fehlermeldung. Auf NULL endende Zeichenfolge für breite Zeichen. Damit parametrisierte Informationen vorhanden sein können, enthält diese Zeichenfolge möglicherweise Einfügeformatierungsequenzen der Form, die von der [FormatMessage](/windows/desktop/api/winbase/nf-winbase-formatmessage)-Funktion akzeptiert wird. Erweiterte Funktionalität kann mithilfe dieses Parameters wie unten beschrieben angegeben werden.|
|...|[Eingabe] Zusätzliche variadische Parameter, um ggf. Formatbezeichner in der Meldung anzupassen.|

Um zu melden, dass ein Fehler aufgetreten ist, ruft der Anbieter onError auf und gibt dabei den Kontextparameter an, der vom Treiber an die Anbieterfunktion übergeben wurde, und eine Fehlermeldung mit optionalen zusätzlichen Parametern, die formatiert werden sollen. Der Anbieter kann diese Funktion mehrmals aufrufen, um mehrere Fehlermeldungen in einem Aufruf der Anbieterfunktion nacheinander zu senden. Beispiel:

```
    if (!doSomething(...))
    {
        onError(ctx, L"An error occurred in doSomething.");
        onError(ctx, L"Additional error message with more details.");
        return 0;
    }
```


Der `msg`-Parameter ist normalerweise eine Breitzeichen-Zeichenfolge, es sind jedoch zusätzliche Erweiterungen verfügbar:

Wenn Sie einen der speziellen vordefinierten Werte mit dem IDS_MSG-Makro verwenden, können generische Fehlermeldungen, die bereits in einem lokalisierten Formular im Treiber vorhanden sind, verwendet werden. Wenn ein Anbieter z. B. keinen Arbeitsspeicher belegen kann, kann die Meldung `IDS_S1_001` „Fehler beim Belegen von Speicher“ verwendet werden:

`onError(ctx, IDS_MSG(IDS_S1_001));`

Damit der Fehler vom Treiber erkannt wird, muss die Anbieterfunktion einen Fehler zurückgeben. Wenn im Kontext eines ODBC-Vorgangs ein Fehler auftritt, wird der Zugriff auf die veröffentlichten Fehler mit dem Verbindungs- oder Anweisungshandle über den standardmäßigen ODBC-Diagnosemechanismus (`SQLError`, `SQLGetDiagRec` und `SQLGetDiagField`) möglich.


### <a name="context-association"></a>Kontextverknüpfung

Die `CEKEYSTORECONTEXT`-Struktur kann zusätzlich zur Bereitstellung des Kontexts für den Fehlerrückruf auch verwendet werden, um den ODBC-Kontext zu ermitteln, in dem ein Anbietervorgang ausgeführt wird. Dieser Kontext ermöglicht einem Anbieter, den einzelnen Kontexten Daten zuzuordnen, z. B. um die Konfiguration pro Verbindung zu implementieren. Zu diesem Zweck enthält die Struktur drei nicht transparente Zeiger, die dem Umgebungs-, Verbindungs- und Anweisungskontext entsprechen:

```
typedef struct CEKeystoreContext
{
void *envCtx;
void *dbcCtx;
void *stmtCtx;
} CEKEYSTORECONTEXT;
```

|Feld|BESCHREIBUNG|
|:--|:--|
|`envCtx`|Umgebungskontext.|
|`dbcCtx`|Verbindungskontext.|
|`stmtCtx`|Anweisungskontext.|

Jeder dieser Kontexte ist ein nicht transparenter Wert, der zwar nicht dasselbe ist wie das entsprechende ODBC-Handle, aber als eindeutiger Bezeichner für das Handle verwendet werden kann: Wenn Handle *X* dem Kontextwert *Y* zugeordnet ist, haben keine anderen Umgebungs-, Verbindungs- oder Anweisungshandle, die gleichzeitig mit *X* vorhanden sind, den Kontextwert *Y*, und keine anderen Kontextwerte sind dem Handle *X* zugeordnet. Wenn dem durchgeführten Anbietervorgang ein bestimmter Handlekontext fehlt (z. B. ruft SQLSetConnectAttr zum Laden und Konfigurieren von Anbietern auf, in denen kein Anweisungshandle vorhanden ist), ist der entsprechende Kontextwert in der Struktur NULL.


## <a name="example"></a>Beispiel

### <a name="keystore-provider"></a>Keystore-Anbieter

Der folgende Code ist ein Beispiel für eine minimale Implementierung des Keystore-Anbieters.

```
/* Custom Keystore Provider Example

Windows:   compile with cl MyKSP.c /LD /MD /link /out:MyKSP.dll
Linux/macOS: compile with gcc -fshort-wchar -fPIC -o MyKSP.so -shared MyKSP.c

 */

#ifdef _WIN32
#include <windows.h>
#else
#define __stdcall
#endif

#include <stdlib.h>
#include <sqltypes.h>
#include "msodbcsql.h"
#include <sql.h>
#include <sqlext.h>

int __stdcall KeystoreInit(CEKEYSTORECONTEXT *ctx, errFunc *onError) {
    printf("KSP Init() function called\n");
    return 1;
}

static unsigned char *g_encryptKey;
static unsigned int g_encryptKeyLen;

int __stdcall KeystoreWrite(CEKEYSTORECONTEXT *ctx, errFunc *onError, void *data, unsigned int len) {
    printf("KSP Write() function called (%d bytes)\n", len);
    if (len) {
        if (g_encryptKey)
            free(g_encryptKey);
        g_encryptKey = malloc(len);
        if (!g_encryptKey) {
            onError(ctx, L"Memory Allocation Error");
            return 0;
        }
        memcpy(g_encryptKey, data, len);
        g_encryptKeyLen = len;
    }
    return 1;
}

// Very simple "encryption" scheme - rotating XOR with the key
int __stdcall KeystoreDecrypt(CEKEYSTORECONTEXT *ctx, errFunc *onError, const wchar_t *keyPath, const wchar_t *alg,
    unsigned char *ecek, unsigned short ecekLen, unsigned char **cekOut, unsigned short *cekLen) {
    unsigned int i;
    printf("KSP Decrypt() function called (keypath=%S alg=%S ecekLen=%u)\n", keyPath, alg, ecekLen);
    if (wcscmp(keyPath, L"TheOneAndOnlyKey")) {
        onError(ctx, L"Invalid key path");
        return 0;
    }
    if (wcscmp(alg, L"none")) {
        onError(ctx, L"Invalid algorithm");
        return 0;
    }
    if (!g_encryptKey) {
        onError(ctx, L"Keystore provider not initialized with key");
        return 0;
    }
#ifndef _WIN32
    *cekOut = malloc(ecekLen);
#else
    *cekOut = LocalAlloc(LMEM_FIXED, ecekLen);
#endif
    if (!*cekOut) {
        onError(ctx, L"Memory Allocation Error");
        return 0;
    }
    *cekLen = ecekLen;
    for (i = 0; i < ecekLen; i++)
        (*cekOut)[i] = ecek[i] ^ g_encryptKey[i % g_encryptKeyLen];
    return 1;
}

// Note that in the provider interface, this function would be referenced via the CEKEYSTOREPROVIDER
// structure. However, that does not preclude keystore providers from exporting their own functions,
// as illustrated by this example where the encryption is performed via a separate function (with a
// different prototype than the one in the KSP interface.)
#ifdef _WIN32
__declspec(dllexport)
#endif
int KeystoreEncrypt(CEKEYSTORECONTEXT *ctx, errFunc *onError,
    unsigned char *cek, unsigned short cekLen,
    unsigned char **ecekOut, unsigned short *ecekLen) {
    unsigned int i;
    printf("KSP Encrypt() function called (cekLen=%u)\n", cekLen);
    if (!g_encryptKey) {
        onError(ctx, L"Keystore provider not initialized with key");
        return 0;
    }
    *ecekOut = malloc(cekLen);
    if (!*ecekOut) {
        onError(ctx, L"Memory Allocation Error");
        return 0;
    }
    *ecekLen = cekLen;
    for (i = 0; i < cekLen; i++)
        (*ecekOut)[i] = cek[i] ^ g_encryptKey[i % g_encryptKeyLen];
    return 1;
}

CEKEYSTOREPROVIDER MyCustomKSPName_desc = {
    L"MyCustomKSPName",
    KeystoreInit,
    0,
    KeystoreWrite,
    KeystoreDecrypt,
    0
};

#ifdef _WIN32
__declspec(dllexport)
#endif
CEKEYSTOREPROVIDER *CEKeystoreProvider[] = {
    &MyCustomKSPName_desc,
    0
};
```

### <a name="odbc-application"></a>ODBC-Anwendung

Der folgende Code ist eine Demoanwendung, die den oben genannten Keystore-Anbieter verwendet. Stellen Sie bei der Ausführung sicher, dass sich die Anbieterbibliothek im selben Verzeichnis wie die Anwendungsbinärdatei befindet, und dass die Verbindungszeichenfolge die `ColumnEncryption=Enabled`-Einstellung angibt (oder einen DSN, der sie enthält).

```
/*
 Example application for demonstration of custom keystore provider usage

Windows:   compile with cl /MD kspapp.c /link odbc32.lib
Linux/macOS: compile with gcc -o kspapp -fshort-wchar kspapp.c -lodbc -ldl
 
 usage: kspapp connstr

 */

#define KSPNAME L"MyCustomKSPName"
#define PROV_ENCRYPT_KEY "JHKCWYT06N3RG98J0MBLG4E3"

#include <stdio.h>
#include <stdlib.h>
#ifdef _WIN32
#include <windows.h>
#else
#define __stdcall
#include <dlfcn.h>
#endif
#include <sql.h>
#include <sqlext.h>
#include "msodbcsql.h"

/* Convenience functions */

int checkRC(SQLRETURN rc, char *msg, int ret, SQLHANDLE h, SQLSMALLINT ht) {
    if (rc == SQL_ERROR) {
        fprintf(stderr, "Error occurred upon %s\n", msg);
        if (h) {
            SQLSMALLINT i = 0;
            SQLSMALLINT outlen = 0;
            char errmsg[1024];
            while ((rc = SQLGetDiagField(
                ht, h, ++i, SQL_DIAG_MESSAGE_TEXT, errmsg, sizeof(errmsg), &outlen)) == SQL_SUCCESS
                || rc == SQL_SUCCESS_WITH_INFO) {
                fprintf(stderr, "Err#%d: %s\n", i, errmsg);
            }
        }
        if (ret)
            exit(ret);
        return 0;
    }
    else if (rc == SQL_SUCCESS_WITH_INFO && h) {
        SQLSMALLINT i = 0;
        SQLSMALLINT outlen = 0;
        char errmsg[1024];
        printf("Success with info for %s:\n", msg);
        while ((rc = SQLGetDiagField(
            ht, h, ++i, SQL_DIAG_MESSAGE_TEXT, errmsg, sizeof(errmsg), &outlen)) == SQL_SUCCESS
            || rc == SQL_SUCCESS_WITH_INFO) {
            fprintf(stderr, "Msg#%d: %s\n", i, errmsg);
        }
    }
    return 1;
}

void postKspError(CEKEYSTORECONTEXT *ctx, const wchar_t *msg, ...) {
    if (msg > (wchar_t*)65535)
        wprintf(L"Provider emitted message: %s\n", msg);
    else
        wprintf(L"Provider emitted message ID %d\n", msg);
}

int main(int argc, char **argv) {
    char sqlbuf[1024];
    SQLHENV env;
    SQLHDBC dbc;
    SQLHSTMT stmt;
    SQLRETURN rc;
    unsigned char CEK[32];
    unsigned char *ECEK;
    unsigned short ECEKlen;
#ifdef _WIN32
    HMODULE hProvLib;
#else
    void *hProvLib;
#endif
    CEKEYSTORECONTEXT ctx = {0};
    CEKEYSTOREPROVIDER **ppKsp, *pKsp;
    int(__stdcall *pEncryptCEK)(CEKEYSTORECONTEXT *, errFunc *, unsigned char *, unsigned short, unsigned char **, unsigned short *);
    int i;
    if (argc < 2) {
        fprintf(stderr, "usage: kspapp connstr\n");
        return 1;
    }

    /* Load the provider library */
#ifdef _WIN32
    if (!(hProvLib = LoadLibrary("MyKSP.dll"))) {
#else
    if (!(hProvLib = dlopen("./MyKSP.so", RTLD_NOW))) {
#endif
        fprintf(stderr, "Error loading KSP library\n");
        return 2;
    }
#ifdef _WIN32
    if (!(ppKsp = (CEKEYSTOREPROVIDER**)GetProcAddress(hProvLib, "CEKeystoreProvider"))) {
#else
    if (!(ppKsp = (CEKEYSTOREPROVIDER**)dlsym(hProvLib, "CEKeystoreProvider"))) {
#endif
        fprintf(stderr, "The export CEKeystoreProvider was not found in the KSP library\n");
        return 3;
    }
    while (pKsp = *ppKsp++) {
        if (!memcmp(KSPNAME, pKsp->Name, sizeof(KSPNAME)))
            goto FoundProv;
    }
    fprintf(stderr, "Could not find provider in the library\n");
    return 4;
FoundProv:
    if (pKsp->Init && !pKsp->Init(&ctx, postKspError)) {
        fprintf(stderr, "Could not initialize provider\n");
        return 5;
    }
#ifdef _WIN32
    if (!(pEncryptCEK = (LPVOID)GetProcAddress(hProvLib, "KeystoreEncrypt"))) {
#else
    if (!(pEncryptCEK = dlsym(hProvLib, "KeystoreEncrypt"))) {
#endif
        fprintf(stderr, "The export KeystoreEncrypt was not found in the KSP library\n");
        return 6;
    }
    if (!pKsp->Write) {
        fprintf(stderr, "Provider does not support configuration\n");
        return 7;
    }

    /* Configure the provider with the key */
    if (!pKsp->Write(&ctx, postKspError, PROV_ENCRYPT_KEY, strlen(PROV_ENCRYPT_KEY))) {
        fprintf(stderr, "Error writing to KSP\n");
        return 8;
    }

    /* Generate a CEK and encrypt it with the provider */
    srand(time(0) ^ getpid());
    for (i = 0; i < sizeof(CEK); i++)
        CEK[i] = rand();

    if (!pEncryptCEK(&ctx, postKspError, CEK, sizeof(CEK), &ECEK, &ECEKlen)) {
        fprintf(stderr, "Error encrypting CEK\n");
        return 9;
    }

    /* Connect to Server */
    rc = SQLAllocHandle(SQL_HANDLE_ENV, NULL, &env);
    checkRC(rc, "allocating environment handle", 2, 0, 0);
    rc = SQLSetEnvAttr(env, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, 0);
    checkRC(rc, "setting ODBC version to 3.0", 3, env, SQL_HANDLE_ENV);
    rc = SQLAllocHandle(SQL_HANDLE_DBC, env, &dbc);
    checkRC(rc, "allocating connection handle", 4, env, SQL_HANDLE_ENV);
    rc = SQLDriverConnect(dbc, 0, argv[1], strlen(argv[1]), NULL, 0, NULL, SQL_DRIVER_NOPROMPT);
    checkRC(rc, "connecting to data source", 5, dbc, SQL_HANDLE_DBC);
    rc = SQLAllocHandle(SQL_HANDLE_STMT, dbc, &stmt);
    checkRC(rc, "allocating statement handle", 6, dbc, SQL_HANDLE_DBC);

    /* Create a CMK definition on the server */
    {
        static char cmkSql[] = "CREATE COLUMN MASTER KEY CustomCMK WITH ("
            "KEY_STORE_PROVIDER_NAME = 'MyCustomKSPName',"
            "KEY_PATH = 'TheOneAndOnlyKey')";
        printf("Create CMK: %s\n", cmkSql);
        SQLExecDirect(stmt, cmkSql, SQL_NTS);
    }

    /* Create a CEK definition on the server */
    {
        const char cekSqlBefore[] = "CREATE COLUMN ENCRYPTION KEY CustomCEK WITH VALUES ("
            "COLUMN_MASTER_KEY = CustomCMK,"
            "ALGORITHM = 'none',"
            "ENCRYPTED_VALUE = 0x";
        char *cekSql = malloc(sizeof(cekSqlBefore) + 2 * ECEKlen + 2); /* 1 for ')', 1 for null terminator */
        strcpy(cekSql, cekSqlBefore);
        for (i = 0; i < ECEKlen; i++)
            sprintf(cekSql + sizeof(cekSqlBefore) - 1 + 2 * i, "%02x", ECEK[i]);
        strcat(cekSql, ")");
        printf("Create CEK: %s\n", cekSql);
        SQLExecDirect(stmt, cekSql, SQL_NTS);
        free(cekSql);
#ifdef _WIN32
        LocalFree(ECEK);
#else
        free(ECEK);
#endif
    }

#ifdef _WIN32
    FreeLibrary(hProvLib);
#else
    dlclose(hProvLib);
#endif

    /* Create a table with encrypted columns */
    {
        static char *tableSql = "CREATE TABLE CustomKSPTestTable ("
            "c1 int,"
            "c2 varchar(255) COLLATE Latin1_General_BIN2 ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = CustomCEK, ENCRYPTION_TYPE = DETERMINISTIC, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'))";
        printf("Create table: %s\n", tableSql);
        SQLExecDirect(stmt, tableSql, SQL_NTS);
    }

    /* Load provider into the ODBC Driver and configure it */
    {
        unsigned char ksd[sizeof(CEKEYSTOREDATA) + sizeof(PROV_ENCRYPT_KEY) - 1];
        CEKEYSTOREDATA *pKsd = (CEKEYSTOREDATA*)ksd;
        pKsd->name = L"MyCustomKSPName";
        pKsd->dataSize = sizeof(PROV_ENCRYPT_KEY) - 1;
        memcpy(pKsd->data, PROV_ENCRYPT_KEY, sizeof(PROV_ENCRYPT_KEY) - 1);
#ifdef _WIN32
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREPROVIDER, "MyKSP.dll", SQL_NTS);
#else
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREPROVIDER, "./MyKSP.so", SQL_NTS);
#endif
        checkRC(rc, "Loading KSP into ODBC Driver", 7, dbc, SQL_HANDLE_DBC);
        rc = SQLSetConnectAttr(dbc, SQL_COPT_SS_CEKEYSTOREDATA, (SQLPOINTER)pKsd, SQL_IS_POINTER);
        checkRC(rc, "Configuring the KSP", 7, dbc, SQL_HANDLE_DBC);
    }

    /* Insert some data */
    {
        int c1;
        char c2[256];
        rc = SQLBindParameter(stmt, 1, SQL_PARAM_INPUT, SQL_C_LONG, SQL_INTEGER, 0, 0, &c1, 0, 0);
        checkRC(rc, "Binding parameters for insert", 9, stmt, SQL_HANDLE_STMT);
        rc = SQLBindParameter(stmt, 2, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_VARCHAR, 255, 0, c2, 255, 0);
        checkRC(rc, "Binding parameters for insert", 9, stmt, SQL_HANDLE_STMT);
        for (i = 0; i < 10; i++) {
            c1 = i * 10 + i + 1;
            sprintf(c2, "Sample data %d for column 2", i);
            rc = SQLExecDirect(stmt, "INSERT INTO CustomKSPTestTable (c1, c2) values (?, ?)", SQL_NTS);
            checkRC(rc, "Inserting rows query", 10, stmt, SQL_HANDLE_STMT);
        }
        printf("(Encrypted) data has been inserted into the [CustomKSPTestTable]. You may inspect the data now.\n"
            "Press Enter to continue...\n");
        getchar();
    }

    /* Retrieve the data */
    {
        int c1;
        char c2[256];
        rc = SQLBindCol(stmt, 1, SQL_C_LONG, &c1, 0, 0);
        checkRC(rc, "Binding columns for select", 11, stmt, SQL_HANDLE_STMT);
        rc = SQLBindCol(stmt, 2, SQL_C_CHAR, c2, sizeof(c2), 0);
        checkRC(rc, "Binding columns for select", 11, stmt, SQL_HANDLE_STMT);
        rc = SQLExecDirect(stmt, "SELECT c1, c2 FROM CustomKSPTestTable", SQL_NTS);
        checkRC(rc, "Retrieving rows query", 12, stmt, SQL_HANDLE_STMT);
        while (SQL_SUCCESS == (rc = SQLFetch(stmt)))
            printf("Retrieved data: c1=%d c2=%s\n", c1, c2);
        SQLFreeStmt(stmt, SQL_CLOSE);
        printf("Press Enter to clean up and exit...\n");
        getchar();
    }

    /* Clean up */
    {
        SQLExecDirect(stmt, "DROP TABLE CustomKSPTestTable", SQL_NTS);
        SQLExecDirect(stmt, "DROP COLUMN ENCRYPTION KEY CustomCEK", SQL_NTS);
        SQLExecDirect(stmt, "DROP COLUMN MASTER KEY CustomCMK", SQL_NTS);
        printf("Removed table, CEK, and CMK\n");
    }
    SQLDisconnect(dbc);
    SQLFreeHandle(SQL_HANDLE_DBC, dbc);
    SQLFreeHandle(SQL_HANDLE_ENV, env);
    return 0;
}

```

## <a name="see-also"></a>Weitere Informationen

[Verwenden von Always Encrypted mit dem ODBC-Treiber](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)
