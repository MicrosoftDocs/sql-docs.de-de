---
description: CREATE CERTIFICATE (Transact-SQL)
title: CREATE CERTIFICATE (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 02/06/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CERTIFICATE
- CREATE_CERTIFICATE_TSQL
- sql13.swb.createcertificate.f1
- CERTIFICATE_TSQL
- CREATE CERTIFICATE
- sql13.swb.certificateproperties.f1
dev_langs:
- TSQL
helpviewer_keywords:
- encryption [SQL Server], certificates
- certificates [SQL Server], adding
- certificates [SQL Server]
- adding certificates
- cryptography [SQL Server], certificates
- CREATE CERTIFICATE statement
ms.assetid: a4274b2b-4cb0-446a-a956-1c8e6587515d
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: b670cd34ef9f0fed5fefbc788ac4774458f2c5a2
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421916"
---
# <a name="create-certificate-transact-sql"></a>CREATE CERTIFICATE (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Fügt einer Datenbank in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ein Zertifikat hinzu.  

 Diese Funktion ist inkompatibel mit Datenbankexport über Data-Tier Application Framework (DACFx). Sie müssen alle Zertifikate vor dem Export löschen.  
  
 ![Symbol für Themenlink](../../database-engine/configure-windows/media/topic-link.gif "Symbol für Themenlink") [Transact-SQL-Syntaxkonventionen](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]  
  
## <a name="syntax"></a>Syntax  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
CREATE CERTIFICATE certificate_name [ AUTHORIZATION user_name ]   
    { FROM <existing_keys> | <generate_new_keys> }  
    [ ACTIVE FOR BEGIN_DIALOG = { ON | OFF } ]  
  
<existing_keys> ::=   
    ASSEMBLY assembly_name  
    | {   
        [ EXECUTABLE ] FILE = 'path_to_file'  
        [ WITH PRIVATE KEY ( <private_key_options> ) ]   
      }  
    | {   
        BINARY = asn_encoded_certificate  
        [ WITH PRIVATE KEY ( <private_key_options> ) ]  
      }  
<generate_new_keys> ::=   
    [ ENCRYPTION BY PASSWORD = 'password' ]   
    WITH SUBJECT = 'certificate_subject_name'   
    [ , <date_options> [ ,...n ] ]   
  
<private_key_options> ::=  
      {   
        FILE = 'path_to_private_key'  
         [ , DECRYPTION BY PASSWORD = 'password' ]  
         [ , ENCRYPTION BY PASSWORD = 'password' ]    
      }  
    |  
      {   
        BINARY = private_key_bits  
         [ , DECRYPTION BY PASSWORD = 'password' ]  
         [ , ENCRYPTION BY PASSWORD = 'password' ]    
      }  
  
<date_options> ::=  
    START_DATE = 'datetime' | EXPIRY_DATE = 'datetime'  
```  
  
   
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
CREATE CERTIFICATE certificate_name   
    { <generate_new_keys> | FROM <existing_keys> }  
    [ ; ]  
  
<generate_new_keys> ::=   
    WITH SUBJECT = 'certificate_subject_name'   
    [ , <date_options> [ ,...n ] ]   
  
<existing_keys> ::=   
    {   
      FILE ='path_to_file'  
      WITH PRIVATE KEY   
         (   
           FILE = 'path_to_private_key'  
           , DECRYPTION BY PASSWORD ='password'   
         )  
    }  
  
<date_options> ::=  
    START_DATE ='datetime' | EXPIRY_DATE ='datetime'  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumente
 *certificate_name*  
 Der Name eines Zertifikats in der Datenbank.  
  
 AUTHORIZATION *user_name*  
 Der Name des Benutzers, der das Zertifikat besitzt.  
  
 ASSEMBLY *assembly_name*  
 Gibt eine signierte Assembly an, die bereits in die Datenbank geladen wurde.  
  
 [ EXECUTABLE ] FILE = '*path_to_file*'  
 Gibt den vollständigen Pfad einschließlich des Dateinamens zur DER-codierten Datei an, die das Zertifikat enthält. Falls die EXECUTABLE-Option verwendet wird, handelt es sich bei der Datei um eine DLL-Datei, die mit dem Zertifikat signiert wurde. *path_to_file* kann ein lokaler Pfad oder ein UNC-Pfad zu einem Netzwerkspeicherort sein. Die Datei wird im Sicherheitskontext des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienstkontos ausgeführt. Dieses Konto muss über die erforderlichen Dateisystemberechtigungen verfügen.  

> [!IMPORTANT]
> Das Erstellen eines Zertifikats aus einer Datei oder mithilfe von Dateien mit privaten Schlüsseln wird in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] nicht unterstützt.
  
 BINARY = *asn_encoded_certificate*  
 Mit ASN verschlüsselte Zertifikatbytes, die als binäre Konstante angegeben sind.  
 **Gilt für**:  [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] und höher.  
  
 WITH PRIVATE KEY  
 Gibt an, dass der private Schlüssel des Zertifikats in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] geladen wird. Diese Klausel ist ungültig, wenn das Zertifikat aus einer Assembly erstellt wird. Verwenden Sie [ALTER CERTIFICATE](../../t-sql/statements/alter-certificate-transact-sql.md), um den privaten Schlüssel eines aus einer Assembly erstellten Zertifikats zu laden.  
  
 FILE ='*path_to_private_key*'  
 Gibt den vollständigen Pfad einschließlich des Dateinamens für den privaten Schlüssel an. *path_to_private_key* kann ein lokaler Pfad oder ein UNC-Pfad zu einem Netzwerkspeicherort sein. Die Datei wird im Sicherheitskontext des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Dienstkontos ausgeführt. Dieses Konto muss über die erforderlichen Dateisystemberechtigungen verfügen.  
  
> [!IMPORTANT]  
> Diese Option ist weder in einer eigenständigen Datenbank noch in [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] verfügbar.  
  
 BINARY = *private_key_bits*  
 **Gilt für**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ab [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) und [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
 Private Schlüsselbits, die als binäre Konstante angegeben sind. Diese Bits können in verschlüsselter Form vorhanden sein. Bei Verschlüsselung muss der Benutzer ein Entschlüsselungskennwort bereitstellen. Kennwortrichtlinienüberprüfungen werden für dieses Kennwort nicht ausgeführt. Die privaten Schlüsselbits müssen in einem PVK-Dateiformat vorliegen.  
  
 DECRYPTION BY PASSWORD = '*key_password*'  
 Gibt das Kennwort an, das zum Entschlüsseln eines privaten Schlüssels erforderlich ist, der aus einer Datei abgerufen wird. Diese Klausel ist optional, wenn der private Schlüssel nicht durch ein Kennwort geschützt ist. Das Speichern eines privaten Schlüssels in einer Datei ohne Kennwortschutz wird nicht empfohlen. Falls zwar ein Kennwort erforderlich ist, jedoch keins angegeben wurde, löst die Anweisung einen Fehler aus.  
  
 ENCRYPTION BY PASSWORD = '*password*'  
 Gibt das Kennwort an, mit dem der private Schlüssel verschlüsselt werden soll. Verwenden Sie diese Option nur, wenn Sie das Zertifikat mit einem Kennwort verschlüsseln möchten. Falls diese Klausel ausgelassen wird, wird der private Schlüssel mit dem Datenbank-Hauptschlüssel verschlüsselt. *password* muss den Anforderungen der Windows-Kennwortrichtlinien des Computers entsprechen, auf dem die Instanz von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ausgeführt wird. Weitere Informationen finden Sie unter [Password Policy](../../relational-databases/security/password-policy.md).  
  
 SUBJECT = '*certificate_subject_name*'  
 Der Begriff *subject* (Betreff) bezieht sich auf ein Feld in den Metadaten des Zertifikats, wie es im X.509-Standard definiert ist. Er sollte nicht länger als 64 Zeichen sein. Diese Beschränkung wird für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unter Linux erzwungen. Der Antragsteller kann für [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unter Windows bis zu 128 Zeichen lang sein. Betreffe mit mehr als 128 Zeichen werden abgeschnitten, wenn sie im Katalog gespeichert werden. In den BLOB-Daten (Binary Large Object), die das Zertifikat enthalten, ist jedoch der vollständige Name des Antragstellers gespeichert.  
  
 START_DATE = '*datetime*'  
 Das Datum, an dem das Zertifikat gültig wird. Falls nicht anders angegeben, wird das aktuelle Datum für START_DATE festgelegt. START_DATE ist in UTC-Zeit und kann in jedem Format angegeben werden, das in ein Datum und eine Uhrzeit konvertiert werden kann.  
  
 EXPIRY_DATE = '*datetime*'  
 Das Datum, an dem das Zertifikat abläuft. Falls nicht anders angegeben, wird EXPIRY_DATE auf das Datum ein Jahr nach START_DATE festgelegt. EXPIRY_DATE ist in UTC-Zeit und kann in jedem Format angegeben werden, das in ein Datum und eine Uhrzeit konvertiert werden kann. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Broker überprüft das Ablaufdatum. Bei der Sicherung mit Verschlüsselung unter Verwendung von Zertifikaten wird ebenfalls das Ablaufdatum überprüft, und es wird verhindert, dass eine Sicherung mit einem abgelaufenen Zertifikat erstellt wird. Wiederherstellungen mit einem abgelaufenen Zertifikat sind aber möglich. Der Ablauf wird jedoch nicht erzwungen, wenn das Zertifikat für die die Datenbankverschlüsselung oder die Always Encrypted-Funktion verwendet wird.  
  
 ACTIVE FOR BEGIN_DIALOG = { **ON** | OFF }  
 Stellt das Zertifikat für den Initiator einer [!INCLUDE[ssSB](../../includes/sssb-md.md)]-Dialogkonversation zur Verfügung. Der Standardwert ist ON.  
  
## <a name="remarks"></a>Bemerkungen  
 Ein Zertifikat ist ein sicherungsfähiges Element auf Datenbankebene, das dem X.509-Standard entspricht und X.509 V1-Felder unterstützt. `CREATE CERTIFICATE` kann ein Zertifikat aus einer Datei, einer binären Konstante oder einer Assembly laden. Mit dieser Anweisung kann auch ein Schlüsselpaar generiert und ein selbstsigniertes Zertifikat erstellt werden.  
  
 Der private Schlüssel muss \<= 2500 Byte in einem verschlüsselten Format betragen. Private Schlüssel, die von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generiert werden, umfassen 1024 Bit bis [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] und ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 2048 Bit. Private Schlüssel, die aus einer externen Quelle importiert werden, haben eine minimale Länge von 384 Bits und eine maximale Länge von 4.096 Bits. Die Länge eines importierten privaten Schlüssels muss ein ganzzahliges Produkt von 64 Bits sein. Die für TDE verwendeten Zertifikate sind auf die private Schlüsselgröße von 3456 Bits beschränkt.  
  
 Die vollständige Seriennummer des Zertifikats wird gespeichert, aber nur die ersten 16 Byte werden in der sys.certificates-Katalogsicht angezeigt.  
  
 Das vollständige Feld „Zertifikataussteller“ wird zwar gespeichert, aber nur die ersten 884 Byte werden in der sys.certificates-Katalogsicht angezeigt.  
  
 Der private Schlüssel muss dem öffentlichen Schlüssel entsprechen, der mit *certificate_name* angegeben ist.  
  
 Beim Erstellen eines Zertifikats aus einem Container ist das Laden des privaten Schlüssels optional. Wenn von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] jedoch ein selbstsigniertes Zertifikat generiert wird, wird der private Schlüssel immer erstellt. Standardmäßig ist der private Schlüssel mit dem Datenbank-Hauptschlüssel verschlüsselt. Falls kein Datenbank-Hauptschlüssel vorhanden und kein Kennwort angegeben ist, erzeugt die Anweisung einen Fehler.  
  
 Die `ENCRYPTION BY PASSWORD`-Option ist nicht erforderlich, wenn der private Schlüssel mit dem Datenbankhauptschlüssel verschlüsselt ist. Verwenden Sie diese Option nur, wenn der private Schlüssel mit einem Kennwort verschlüsselt wird. Falls kein Kennwort angegeben ist, wird der private Schlüssel des Zertifikats mit dem Datenbank-Hauptschlüssel verschlüsselt. Wenn der Datenbank-Hauptschlüssel nicht geöffnet werden kann, wird ein Fehler ausgelöst, wenn Sie diese Klausel auslassen.  
  
 Sie müssen kein Entschlüsselungskennwort angeben, wenn der private Schlüssel mit dem Datenbank-Hauptschlüssel verschlüsselt ist.  
  
> [!NOTE]  
> Die Ablaufdaten von Zertifikaten werden von integrierten Funktionen für die Verschlüsselung und Signierung nicht überprüft. Benutzer dieser Funktionen müssen entscheiden, wann die Ablaufdaten der Zertifikate überprüft werden sollen.  
  
 Sie können über die Funktionen [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md) und [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md) eine binäre Beschreibung eines Zertifikats erstellen. Ein Beispiel für die Verwendung von **CERTPRIVATEKEY** und **CERTENCODED** zum Kopieren eines Zertifikats in eine andere Datenbank finden Sie in Beispiel B im Artikel [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md).  

Die Algorithmen MD2, MD4, MD5, SHA und SHA-1 sind ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] veraltet. Bis [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] werden selbstsignierte Zertifikate mit SHA-1 erstellt. Ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] werden selbstsignierte Zertifikate mit SHA-256 erstellt.

## <a name="permissions"></a>Berechtigungen  
 Erfordert die `CREATE CERTIFICATE`-Berechtigung für die Datenbank. Nur Windows-Anmeldenamen, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Anmeldenamen und Anwendungsrollen können Zertifikate besitzen. Gruppen und Rollen können keine Zertifikate besitzen.  
  
## <a name="examples"></a>Beispiele  
  
### <a name="a-creating-a-self-signed-certificate"></a>A. Erstellen eines selbstsignierten Zertifikats  
 Im folgenden Beispiel wird ein Zertifikat namens `Shipping04` erstellt. Der private Schlüssel dieses Zertifikats wird mit einem Kennwort geschützt.  
  
```sql  
CREATE CERTIFICATE Shipping04   
   ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'  
   WITH SUBJECT = 'Sammamish Shipping Records',   
   EXPIRY_DATE = '20201031';  
GO  
```  
  
### <a name="b-creating-a-certificate-from-a-file"></a>B. Erstellen eines Zertifikats aus einer Datei  
 Im folgenden Beispiel wird ein Zertifikat in der Datenbank erstellt, wobei das Schlüsselpaar aus Dateien geladen wird.  
  
```sql  
CREATE CERTIFICATE Shipping11   
    FROM FILE = 'c:\Shipping\Certs\Shipping11.cer'   
    WITH PRIVATE KEY (FILE = 'c:\Shipping\Certs\Shipping11.pvk',   
    DECRYPTION BY PASSWORD = 'sldkflk34et6gs%53#v00');  
GO   
```  

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] bietet keine Unterstützung für das Erstellen eines Zertifikats aus einer Datei.
   
### <a name="c-creating-a-certificate-from-a-signed-executable-file"></a>C. Erstellen eines Zertifikats aus einer signierten ausführbaren Datei  
  
```sql  
CREATE CERTIFICATE Shipping19   
    FROM EXECUTABLE FILE = 'c:\Shipping\Certs\Shipping19.dll';  
GO  
```  
  
 Alternativ können Sie eine Assembly aus der `dll`-Datei erstellen und anschließend ein Zertifikat aus der Assembly erstellen.  
  
```sql  
CREATE ASSEMBLY Shipping19   
    FROM ' c:\Shipping\Certs\Shipping19.dll'   
    WITH PERMISSION_SET = SAFE;  
GO  
CREATE CERTIFICATE Shipping19 FROM ASSEMBLY Shipping19;  
GO  
```  

> [!IMPORTANT]
> [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] bietet keine Unterstützung für das Erstellen eines Zertifikats aus einer Datei.

> [!IMPORTANT]
> Ab [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] verhindert die Serverkonfigurationsoption [„CLR Strict Security“](../../database-engine/configure-windows/clr-strict-security.md) das Laden von Assemblys, ohne diese zuerst deren Sicherheit einzurichten. Laden Sie das Zertifikat, erstellen Sie einen Anmeldenamen, weisen Sie `UNSAFE ASSEMBLY` diesem Anmeldenamen zu, und laden Sie dann die Assembly.

### <a name="d-creating-a-self-signed-certificate"></a>D: Erstellen eines selbstsignierten Zertifikats  
 Das folgende Beispiel erstellt ein Zertifikat mit dem Namen `Shipping04`, ohne ein Verschlüsselungskennwort anzugeben. Dieses Beispiel kann mit [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] verwendet werden.
  
```sql  
CREATE CERTIFICATE Shipping04   
   WITH SUBJECT = 'Sammamish Shipping Records';  
GO  
```  
  
## <a name="see-also"></a>Weitere Informationen  
 [ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)   
 [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md)   
 [Verschlüsselungshierarchie](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md)   
 [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md)  
 [CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md)  
 [CERTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/certproperty-transact-sql.md)  
  
  


