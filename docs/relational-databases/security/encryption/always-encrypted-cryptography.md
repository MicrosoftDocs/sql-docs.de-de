---
title: Always Encrypted-Kryptografie | Microsoft-Dokumentation
description: Hier erfahren Sie mehr über Verschlüsselungsalgorithmen und -mechanismen, die Kryptografiematerial im Always Encrypted-Feature in SQL Server und Azure SQL-Datenbank ableiten.
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, cryptography system
ms.assetid: ae8226ff-0853-4716-be7b-673ce77dd370
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0da1e6ba00c86bbc391d098ab7cf8b9b218d60e1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344386"
---
# <a name="always-encrypted-cryptography"></a>Always Encrypted-Kryptografie
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

  Dieses Dokument beschreibt Verschlüsselungsalgorithmen und -mechanismen zum Ableiten von kryptografischem Material, das in der Funktion [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] und [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)]verwendet wird.  
  
## <a name="keys-key-stores-and-key-encryption-algorithms"></a>Schlüssel, Schlüsselspeicher und Algorithmen für die Schlüsselverschlüsselung
 Always Encrypted nutzt zwei Arten von Schlüsseln: Spaltenhauptschlüssel und Spaltenverschlüsselungsschlüssel.  
  
 Ein Spaltenhauptschlüssel (Column Master Key; CMK) ist ein Schlüsselverschlüsselungsschlüssel (d. h. ein Schlüssel zum Verschlüsseln anderer Schlüssel), der sich immer unter der Kontrolle eines Clients befindet und in einem externen Schlüsselspeicher gespeichert ist. Ein Clienttreiber, der für Always Encrypted aktiviert ist, interagiert mit dem Schlüsselspeicher über einen CMK-Speicheranbieter, der entweder Teil der Treiberbibliothek (ein [!INCLUDE[msCoName](../../../includes/msconame-md.md)]-/Systemanbieter) oder Teil der Clientanwendung (ein benutzerdefinierter Anbieter) ist. Clienttreiberbibliotheken umfassen derzeit [!INCLUDE[msCoName](../../../includes/msconame-md.md)] -Schlüsselspeicheranbieter für [Windows-Zertifikatspeicher](/windows/desktop/SecCrypto/using-certificate-stores) und Hardwaresicherheitsmodule (HSMs). Eine aktuelle Liste der Anbieter finden Sie unter [CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-column-master-key-transact-sql.md). Ein Anwendungsentwickler kann einen benutzerdefinierten Anbieter für einen beliebigen Speicher angeben.  
  
 Ein Spaltenverschlüsselungsschlüssel (column encryption key; CEK) ist ein Inhaltsverschlüsselungsschlüssel (d.h. ein Schlüssel zum Schützen von Daten), der durch einen CMK geschützt ist.  
  
 Alle [!INCLUDE[msCoName](../../../includes/msconame-md.md)]-CMK-Speicheranbieter verschlüsseln CEKs, indem sie RSA-OAEP (RSA mit optimalem asymmetrischen Verschlüsselungspadding) verwenden. Der Schlüsselspeicheranbieter, der die Microsoft Cryptography API unterstützt: Next Generation (CNG) in .NET Framework ([SqlColumnEncryptionCngProvider-Klasse](/dotnet/api/system.data.sqlclient.sqlcolumnencryptioncngprovider)) verwendet dich von RFC 8017 in Abschnitt A.2.1 angegeben Standardparameter Diese Standardparameter verwenden eine Hashfunktion von SHA-1 und eine Maskengenerierungsfunktion von MGF1 mit SHA-1. Alle anderen Schlüsselspeicheranbieter verwenden SHA-256. 

Always Encrypted verwendet intern überprüfte FPS 140-2-Kryptografiemodule. 

## <a name="data-encryption-algorithm"></a>Datenverschlüsselungsalgorithmus  
 Always Encrypted verwendet den Algorithmus **AEAD_AES_256_CBC_HMAC_SHA_256** zum Verschlüsseln von Daten in der Datenbank.  
  
 **AEAD_AES_256_CBC_HMAC_SHA_256** wird vom Spezifikationsentwurf unter [https://tools.ietf.org/html/draft-mcgrew-aead-aes-cbc-hmac-sha2-05](https://tools.ietf.org/html/draft-mcgrew-aead-aes-cbc-hmac-sha2-05) abgeleitet. Er verwendet ein authentifiziertes Verschlüsselungsschema mit zugeordneten Daten nach einem Encrypt-then-MAC-Ansatz. D.h. zunächst wird der Klartext verschlüsselt, und anschließend wird der MAC basierend auf dem resultierenden Chiffretext erstellt.  
  
 Um Muster zu verbergen, verwendet **AEAD_AES_256_CBC_HMAC_SHA_256** den Betriebsmodus der Blockchiffreverkettung (Cipher Block Chaining, CBC), in dem ein Anfangswert in das System eingegeben wird, der als Initialisierungsvektor (IV) bezeichnet wird. Eine vollständige Beschreibung des CBC-Modus finden Sie unter [https://csrc.nist.gov/publications/nistpubs/800-38a/sp800-38a.pdf](https://csrc.nist.gov/publications/nistpubs/800-38a/sp800-38a.pdf).  
  
 **AEAD_AES_256_CBC_HMAC_SHA_256** berechnet einen Chiffretextwert für einen angegebenen Klartextwert mithilfe der folgenden Schritte.  
  
### <a name="step-1-generating-the-initialization-vector-iv"></a>Schritt 1: Generieren des Initialisierungsvektors (IV)  
 Always Encrypted unterstützt zwei Variationen von **AEAD_AES_256_CBC_HMAC_SHA_256**:  
  
-   Zufällig  
  
-   Deterministisch  
  
 Für die zufällige Verschlüsselung wird der IV zufällig generiert. Daher wird jedes Mal, wenn der gleiche Klartext verschlüsselt wird, ein anderer Chiffretext generiert, was jegliche Offenlegung von Informationen verhindert.  
  
```  
When using randomized encryption: IV = Generate cryptographicaly random 128bits  
```  
  
 Bei der deterministischen Verschlüsselung wird der IV nicht zufällig generiert, sondern mithilfe des folgenden Algorithmus aus dem Klartextwert abgeleitet:  
  
```  
When using deterministic encryption: IV = HMAC-SHA-256( iv_key, cell_data ) truncated to 128 bits.  
```  
  
 Wobei iv_key vom CEK folgendermaßen abgeleitet wird:  
  
```  
iv_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell IV key" + algorithm + CEK_length)  
```  
  
 Das Abschneiden des HMAC-Werts wird ausgeführt, um einen Block von Daten wie für den IV benötigt anzupassen.
Daher erzeugt die deterministische Verschlüsselung immer den gleichen Chiffretext für einen angegebenen Klartextwert. Dadurch kann aus einem Vergleich der entsprechenden Chiffretextwerte abgeleitet werden, ob zwei Klartexte identisch sind. Diese eingeschränkte Offenlegung von Informationen ermöglicht dem Datenbanksystem das Unterstützen der Gleichheitsüberprüfung von verschlüsselten Datenwerten.  
  
 Die deterministische Verschlüsselung ist im Vergleich zu Alternativen, wie der Verwendung von vordefinierten IV-Werten, effektiver im Verdecken von Mustern.  
  
### <a name="step-2-computing-aes_256_cbc-ciphertext"></a>Schritt 2: Berechnen des Chiffretexts „AES_256_CBC“  
 Nach dem Berechnen des IV-Werts wird der Chiffretext **AES_256_CBC** generiert:  
  
```  
aes_256_cbc_ciphertext = AES-CBC-256(enc_key, IV, cell_data) with PKCS7 padding.  
```  
  
 Wobei der Verschlüsselungsschlüssel (enc_key) vom CEK folgendermaßen abgeleitet wird.  
  
```  
enc_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell encryption key" + algorithm + CEK_length )  
```  
  
### <a name="step-3-computing-mac"></a>Schritt 3: Berechnen des MAC  
 Anschließend wird der MAC mithilfe des folgenden Algorithmus berechnet:  
  
```  
MAC = HMAC-SHA-256(mac_key, versionbyte + IV + Ciphertext + versionbyte_length)  
```  
  
 Hierbei gilt:  
  
```  
versionbyte = 0x01 and versionbyte_length = 1
mac_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell MAC key" + algorithm + CEK_length)  
```  
  
### <a name="step-4-concatenation"></a>Schritt 4: Verkettung  
 Schließlich wird der verschlüsselte Wert erzeugt, indem das Algorithmusversionsbyte, der MAC, der IV und der Chiffretext „AES_256_CBC“ verkettet werden:  
  
```  
aead_aes_256_cbc_hmac_sha_256 = versionbyte + MAC + IV + aes_256_cbc_ciphertext  
```  
  
## <a name="ciphertext-length"></a>Chiffretextlänge  
 Die Längen (in Bytes) bestimmter Komponenten des Chiffretexts **AEAD_AES_256_CBC_HMAC_SHA_256** lauten wie folgt:  
  
-   Versionsbyte: 1  
  
-   MAC: 32  
  
-   IV: 16  
  
-   aes_256_cbc_ciphertext: `(FLOOR (DATALENGTH(cell_data)/ block_size) + 1)* block_size`, wobei:  
  
    -   block_size 16 Byte beträgt  
  
    -   cell_data ein Klartextwert ist  
  
     Daher ist die minimale Größe des aes_256_cbc_ciphertext 1 Block, der 16 Byte groß ist.  
  
 Daher kann die Länge des Chiffretexts, die sich aus der Verschlüsselung bestimmter Klartextwerte (cell_data) ergibt, mithilfe der folgenden Formel berechnet werden:  
  
```  
1 + 32 + 16 + (FLOOR(DATALENGTH(cell_data)/16) + 1) * 16  
```  
  
 Beispiel:  
  
-   Ein 4 Bytes langer **int** -Klartextwert wird nach der Verschlüsselung zu einem 65 Bytes langen Binärwert.  
  
-   Ein 2000 Bytes langer **nchar(1000)** -Klartextwert wird nach der Verschlüsselung zu einem 2065 Bytes langen Binärwert.  
  
 Die folgende Tabelle enthält eine vollständige Liste der Datentypen und Längen der Chiffretexte für jeden Typ.  
  
|Datentyp|Chiffretextlänge [Bytes]|  
|---------------|---------------------------------|  
|**bigint**|65|  
|**binary**|Verschiedene Ursachen. Verwenden Sie die oben stehende Formel.|  
|**bit**|65|  
|**char**|Verschiedene Ursachen. Verwenden Sie die oben stehende Formel.|  
|**date**|65|  
|**datetime**|65|  
|**datetime2**|65|  
|**datetimeoffset**|65|  
|**decimal**|81|  
|**float**|65|  
|**geography**|N/V (nicht unterstützt)|  
|**geometry**|N/V (nicht unterstützt)|  
|**hierarchyid**|N/V (nicht unterstützt)|  
|**image**|N/V (nicht unterstützt)|  
|**int**|65|  
|**money**|65|  
|**nchar**|Verschiedene Ursachen. Verwenden Sie die oben stehende Formel.|  
|**ntext**|N/V (nicht unterstützt)|  
|**numeric**|81|  
|**nvarchar**|Verschiedene Ursachen. Verwenden Sie die oben stehende Formel.|  
|**real**|65|  
|**smalldatetime**|65|  
|**smallint**|65|  
|**smallmoney**|65|  
|**sql_variant**|N/V (nicht unterstützt)|  
|**sysname**|N/V (nicht unterstützt)|  
|**text**|N/V (nicht unterstützt)|  
|**time**|65|  
|**timestamp**<br /><br /> (**rowversion**)|N/V (nicht unterstützt)|  
|**tinyint**|65|  
|**uniqueidentifier**|81|  
|**varbinary**|Verschiedene Ursachen. Verwenden Sie die oben stehende Formel.|  
|**varchar**|Verschiedene Ursachen. Verwenden Sie die oben stehende Formel.|  
|**xml**|N/V (nicht unterstützt)|  
  
## <a name="net-reference"></a>.NET-Referenz  
 Weitere Informationen zu den in diesem Dokument beschriebenen Algorithmen finden Sie in der [.NET-Referenz](https://referencesource.microsoft.com/) in den Dateien **SqlAeadAes256CbcHmac256Algorithm.cs**, **SqlColumnEncryptionCertificateStoreProvider.cs** und **SqlColumnEncryptionCertificateStoreProvider.cs**.  
  
## <a name="see-also"></a>Weitere Informationen  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 - [Entwickeln von Anwendungen mit Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-client-development.md)  
  
