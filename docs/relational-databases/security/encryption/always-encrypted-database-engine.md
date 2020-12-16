---
title: Always Encrypted | Microsoft-Dokumentation
description: Übersicht über Always Encrypted, das transparente clientseitige Verschlüsselung und vertrauliches Computing in SQL Server und Azure SQL Database unterstützt.
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], Always Encrypted
- Always Encrypted
- TCE Always Encrypted
- Always Encrypted, about
- SQL13.SWB.COLUMNMASTERKEY.CLEANUP.F1
ms.assetid: 54757c91-615b-468f-814b-87e5376a960f
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1f097d500c1d1b0a035f2bb0e737214d65803414
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477721"
---
# <a name="always-encrypted"></a>Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

  ![Always Encrypted](../../../relational-databases/security/encryption/media/always-encrypted.png "Always Encrypted")  
  
 Always Encrypted ist eine Funktion zum Schutz sensibler Daten, wie Kreditkartennummern oder Personalausweisnummern (z.B. US-Sozialversicherungsnummern), die in [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)]- oder [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Datenbanken gespeichert sind. Always Encrypted ermöglicht es Clients, sensible Daten in Clientanwendungen zu verschlüsseln und die Verschlüsselungsschlüssel niemals an [!INCLUDE[ssDE](../../../includes/ssde-md.md)] ([!INCLUDE[ssSDS](../../../includes/sssds-md.md)] oder [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]) weiterzugeben. Daher bietet Always Encrypted eine Trennung zwischen denjenigen, die die Daten besitzen und sie ansehen können, und denjenigen, die die Daten verwalten, aber keinen Zugriff haben sollten. Always Encrypted stellt sicher, dass Administratoren lokaler Datenbanken, Clouddatenbank-Operatoren oder andere nicht autorisierte Benutzer mit umfangreichen Rechten nicht auf die verschlüsselten Daten zugreifen können. So können Kunden ihre vertraulichen Daten ruhigen Gewissens außerhalb ihres direkten Kontrollbereichs speichern. Dies ermöglicht Organisationen, Daten in Azure zu speichern. Daraufhin können die lokale Datenbankverwaltung an Drittanbieter delegiert oder die Anforderungen an die Sicherheitsfreigaben der eigenen Datenbankadministrationsmitarbeiter reduziert werden.

 Always Encrypted bietet Funktionen für vertrauliches Computing, indem es dem [!INCLUDE[ssDE](../../../includes/ssde-md.md)] ermöglicht, einige Abfragen zu verschlüsselten Daten zu verarbeiten, während die Vertraulichkeit der Daten gewährleistet ist und die oben genannten Sicherheitsmaßnahmen umgesetzt werden. In [!INCLUDE[ssSQL15](../../../includes/sssql15-md.md)], [!INCLUDE[sssSQLv14](../../../includes/sssqlv14-md.md)] und [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)] unterstützt Always Encrypted einen Gleichstellungsabgleich durch deterministische Verschlüsselung. Weitere Informationen finden Sie unter [Auswählen der deterministischen oder zufälligen Verschlüsselung](#selecting--deterministic-or-randomized-encryption). 

  > [!NOTE] 
  > In [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] ermöglicht Secure Enclaves eine erhebliche Erweiterung des vertraulichen Computings von Always Encrypted mit Musterabgleich, anderen Vergleichsoperatoren und direkter Verschlüsselung. Weitere Informationen finden Sie unter [Always Encrypted mit Secure Enclaves](always-encrypted-enclaves.md).

 Always Encrypted macht die Verschlüsselung den Anwendungen gegenüber transparent. Ein auf dem Clientcomputer installierter Treiber, bei dem Always Encrypted aktiviert ist, erreicht dies durch die automatische Ver- und Entschlüsselung von sensiblen Daten in der Clientanwendung. Der Treiber verschlüsselt die Daten in vertraulichen Spalten, bevor er sie an [!INCLUDE[ssDE](../../../includes/ssde-md.md)]weitergibt, und schreibt Abfragen automatisch neu, sodass die Semantik der Anwendung beibehalten wird. Analog dazu werden in verschlüsselten Datenbankspalten gespeicherte Daten aus Abfrageergebnissen durch den Treiber transparent entschlüsselt.  
  
 Always Encrypted ist in allen Editionen von [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)] verfügbar, beginnend mit [!INCLUDE[ssSQL15](../../../includes/sssql15-md.md)] und allen Dienstebenen von [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]. (Vor [!INCLUDE[ssSQL15_md](../../../includes/sssql15-md.md)] SP1 war Always Encrypted auf die Enterprise Edition beschränkt.) Eine Channel 9-Präsentation zu Always Encrypted finden Sie unter [Keeping Sensitive Data Secure with Always Encrypted](https://channel9.msdn.com/events/DataDriven/SQLServer2016/AlwaysEncrypted).  

  
## <a name="typical-scenarios"></a>Typische Szenarien  
  
### <a name="client-and-data-on-premises"></a>Lokaler Client und lokale Daten  
 Ein Kunde verfügt über eine Clientanwendung und [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , die beide lokal, an ihrem Unternehmensstandort ausgeführt werden. Der Kunde möchte einen externen Anbieter einstellen, um [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]zu verwalten. Um die sensiblen, in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]gespeicherten Daten zu schützen, verwendet der Kunde Always Encrypted, um die Trennung der Pflichten von Datenbankadministratoren und Anwendungsadministratoren sicherzustellen. Der Kunde speichert Klartextwerte von Always Encrypted-Schlüsseln in einem vertrauenswürdigen Schlüsselspeicher, auf den die Clientanwendung zugreifen kann. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Administratoren haben keinen Zugriff auf die Schlüssel und sind daher nicht in der Lage, die in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]gespeicherten sensiblen Daten zu entschlüsseln.  
  
### <a name="client-on-premises-with-data-in-azure"></a>Lokaler Client mit Daten in Azure  
 Ein Kunde verfügt über eine lokale Clientanwendung an seinem Unternehmensstandort. Die Anwendung arbeitet mit sensiblen Daten, die in einer von Azure gehosteten Datenbank gespeichert sind ([!INCLUDE[ssSDS](../../../includes/sssds-md.md)] oder [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] auf einem virtuellen Computer in Microsoft Azure ausgeführt). Der Kunde verwendet Always Encrypted und speichert Always Encrypted-Schlüssel in einem vertrauenswürdigen, lokal gehosteten Schlüsselspeicher, damit [!INCLUDE[msCoName](../../../includes/msconame-md.md)] -Cloudadministratoren keinen Zugriff auf sensible Daten haben.  
  
### <a name="client-and-data-in-azure"></a>Client und Daten in Azure  
 Ein Kunde hat eine Clientanwendung, die in Microsoft Azure (z.B. in einer Worker- oder einer Webrolle) gehostet wird. Sie operiert mit sensiblen Daten, die auch in Microsoft Azure gespeichert sind (SQL-Datenbank oder SQL Server werden auf einem virtuellen Computer in Microsoft Azure ausgeführt). Obwohl Always Encrypted keine vollständige Isolation der Daten von Cloudadministratoren bietet, da sowohl die Daten als auch die Schlüssel den Cloudadministratoren der Plattform, die die Clientebene hostet, verfügbar gemacht werden, profitiert der Kunde dennoch von der kleineren Angriffsfläche für Sicherheitsangriffe (die Daten werden in der Datenbank immer verschlüsselt).  
 
## <a name="how-it-works"></a>Funktionsweise

Sie können Always Encrypted für individuelle Datenbankspalten konfigurieren, die Ihre sensiblen Daten enthalten. Beim Einrichten der Verschlüsselung für eine Spalte geben Sie die Informationen über den Verschlüsselungsalgorithmus an sowie kryptografische Schlüssel für den Schutz der Daten in der Spalte. Always Encrypted verwendet zwei Schlüsseltypen: Spaltenverschlüsselungsschlüssel und Spaltenhauptschlüssel. Ein Spaltenverschlüsselungsschlüssel wird verwendet, um Daten in einer verschlüsselten Spalte zu verschlüsseln. Ein Spaltenhauptschlüssel ist ein Schlüssel zum Schutz von Schlüsseln, der einen oder mehrere Spaltenverschlüsselungsschlüssel verschlüsselt. 

Die Datenbank-Engine speichert die Verschlüsselungskonfiguration für jede Spalte in den Datenbankmetadaten. Beachten Sie, dass die Datenbank-Engine jedoch nie Schlüssel jedes Typs als Klartext speichert. Es speichert nur verschlüsselte Werte von Spaltenverschlüsselungsschlüsseln sowie die Information über den Speicherort der Spaltenhauptschlüssel, die in externen vertrauenswürdigen Schlüsselspeichern gespeichert werden, z.B. in Azure Key Vault, im Windows-Zertifikatspeicher, auf einem Clientcomputer oder einem Hardwaresicherheitsmodul.

Eine Anwendung muss einen Always Encrypted-fähigen Clienttreiber verwenden, um auf Daten zuzugreifen, die in einer verschlüsselten Spalte als Klartext gespeichert sind. Wenn eine Anwendung eine parametrisierte Abfrage ausgibt, arbeitet der Treiber transparent mit der Datenbank-Engine zusammen, um zu ermitteln, welche Parameter verschlüsselte Spalten anvisieren und deshalb verschlüsselt werden müssen. Für jeden Parameter, der verschlüsselt werden muss, ruft der Treiber die Informationen über den Verschlüsselungsalgorithmus und den verschlüsselten Wert des Spaltenverschlüsselungsschlüssels für die Spalte, die Parameterziele sowie den Speicherort des entsprechenden Spaltenhauptschlüssels ab.

Als nächstes kontaktiert der Treiber den Schlüsselspeicher, der den Spaltenhauptschlüssel enthält, um den verschlüsselten Wert des Spaltenverschlüsselungsschlüssels zu entschlüsseln und anschließend verwendet er den Klartext-Verschlüsselungsschlüssel zum Entschlüsseln der Parameter. Der sich ergebende Klartext-Spaltenverschlüsselungsschlüssel wird zwischengespeichert, um die Anzahl der Roundtrips zum Schlüsselspeicher für die nachfolgende Verwendung des gleichen Spaltenverschlüsselungsschlüssels zu verringern. Der Treiber ersetzt die Klartextwerte der Parameter, die verschlüsselte Spalten mit den verschlüsselten Werten anvisieren und übermittelt die Abfrage an den Server zur Verarbeitung.

Der Server berechnet das Resultset, und für jede verschlüsselte Spalte im Resultset fügt der Treiber die Verschlüsselungsmetadaten für die Spalte an, einschließlich der Informationen zum Verschlüsselungsalgorithmus und zu den entsprechenden Schlüsseln. Der Treiber versucht zuerst, den Klartext-Verschlüsselungsschlüssel im lokalen Cache zu finden und fragt nur dann den Spaltenhauptschlüssel ab, wenn er den Schlüssel nicht im Cache finden kann. Als Nächstes entschlüsselt der Treiber die Ergebnisse und übermittelt Klartextwerte an die Anwendung.

 Ein Clienttreiber interagiert mit einem Schlüsselspeicher, der einen Spaltenhauptschlüssel enthält, indem er einen Speicheranbieter für den Spaltenhauptschlüssel verwendet. Dies ist eine clientseitige Softwarekomponente, die einen Schlüsselspeicher einkapselt, der den Spaltenhauptschlüssel enthält. Anbieter der gängigen Schlüsselspeicherarten sind in clientseitigen Treiberbibliotheken von Microsoft oder als eigenständige Downloads verfügbar. Sie können auch einen eigenen Anbieter implementieren. Die Funktionen von Always Encrypted, einschließlich integrierte Speicheranbieter für Spaltenhauptschlüssel, variieren je nach Treiberbibliothek und deren Version. 

Weitere Informationen zur Entwicklung von Anwendungen mit Always Encrypted mit bestimmten Clienttreibern finden Sie unter [Always Encrypted (Cliententwicklung)](always-encrypted-client-development.md).

## <a name="remarks"></a>Bemerkungen

Die Verschlüsselung und Entschlüsselung erfolgt über den Clienttreiber. Das bedeutet, dass einige Aktionen, die nur serverseitig auftreten, nicht funktionieren, wenn Always Encrypted verwendet wird. Beispiele hierfür sind das Kopieren von Daten aus einer Spalte in eine andere über die Anweisungen UPDATE, BULK INSERT(T-SQL), SELECT INTO, INSERT..SELECT. 

Hier sehen Sie ein Beispiel einer UPDATE-Anweisung, die versucht, Daten von einer verschlüsselten Spalte in eine nicht verschlüsselte Spalte ohne die Rückgabe eines Resultsets an den Client zu verschieben: 

```sql
update dbo.Patients set testssn = SSN
```

Wenn die US-Sozialversicherungsnummer (SSN) eine Spalte darstellt, die über Always Encrypted verschlüsselt wird, schlägt die oben dargestellte Updateanweisung mit einem Fehler wie dem folgenden fehl:

```
Msg 206, Level 16, State 2, Line 89
Operand type clash: char(11) encrypted with (encryption_type = 'DETERMINISTIC', encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_1', column_encryption_key_database_name = 'ssn') collation_name = 'Latin1_General_BIN2' is incompatible with char
```

Führen Sie folgende Schritte durch, um die Spalte erfolgreich zu aktualisieren:

1. Wählen Sie über die Anweisung SELECT die Datei aus der SSN-Spalte aus, und speichern Sie diese in einem Resultset in der Anwendung. Dadurch kann die Anwendung (*Clienttreiber*) die Spalte entschlüsseln.
2. Fügen Sie über INSERT die Daten aus dem Resultset in SQL Server ein. 

 >[!IMPORTANT]
 > In diesem Szenario werden die Daten entschlüsselt, sobald sie zurück an den Server gesendet werden, da die Zielspalte einen regulären Varchar-Typ darstellt, der verschlüsselte Daten nicht akzeptiert. 
  
## <a name="selecting-deterministic-or-randomized-encryption"></a><a name="selecting--deterministic-or-randomized-encryption"></a> Auswählen der deterministischen oder zufälligen Verschlüsselung  
 Die Datenbank-Engine wird nie auf Grundlage von Klartextdaten ausgeführt, die in verschlüsselten Spalten gespeichert sind, doch es unterstützt einige Abfragen für verschlüsselte Daten je nach Verschlüsselungstyp für die Spalte. Always Encrypted unterstützt zwei Arten von Verschlüsselung: die zufällige und die deterministische Verschlüsselung.  
  
- Die deterministische Verschlüsselung generiert immer denselben verschlüsselten Wert für jeden angegebenen Klartextwert. Die deterministische Verschlüsselung ermöglicht die Punktsuche, Gleichheitsverknüpfung, Gruppierung und Indizierung in verschlüsselten Spalten. Sie erlaubt jedoch auch, dass nicht autorisierte Benutzer Informationen zu verschlüsselten Werten erraten, indem sie die Muster in der verschlüsselten Spalte untersucht, insbesondere wenn es eine kleine Anzahl von möglichen verschlüsselten Werten wie TRUE/FALSE oder die Region Norden/Süden/Osten/Westen gibt. Die deterministische Verschlüsselung muss eine Spaltensortierung mit einer binary2-Sortierreihenfolge für Zeichenspalten verwenden.

- Die zufällige Verschlüsselung verwendet eine Methode, die Daten in einer weniger vorhersagbaren Weise verschlüsselt. Die zufällige Verschlüsselung ist sicherer, verhindert aber die Suche, Gruppierung, Indizierung und Verknüpfung für verschlüsselte Spalten.

Verwenden Sie die deterministische Verschlüsselung für Spalten, die als Such- oder Gruppierungsparameter verwendet werden, wie z. B. eine Personalausweisnummer. Verwenden Sie die zufällige Verschlüsselung für Daten wie beispielsweise Kommentare zu vertraulichen Untersuchungen, die nicht mit anderen Datensätzen gruppiert oder für die Verknüpfung mit anderen Tabellen verwendet werden.
Informationen zu kryptografischen Algorithmen von Always Encrypted finden Sie unter [Always Encrypted-Kryptografie](../../../relational-databases/security/encryption/always-encrypted-cryptography.md).

## <a name="configuring-always-encrypted"></a>Konfigurieren von Always Encrypted

 Die Erstinstallation von Always Encrypted in einer Datenbank umfasst die Generierung von Always Encrypted-Schlüsseln, das Erstellen von Schlüsselmetadaten, die Konfigurierung von Verschlüsselungseigenschaften von ausgewählten Datenbankspalten, und/oder das Verschlüsseln von Daten, die möglicherweise bereits in Spalten vorhanden sind, die verschlüsselt werden müssen. Bitte beachten Sie, dass einige dieser Aufgaben nicht in Transact-SQL unterstützt werden und die Verwendung von clientseitigen Tools erfordern. Da Always Encrypted-Schlüssel und geschützte sensible Daten nie dem Server in Klartext offengelegt werden, kann die Datenbank-Engine nicht an der Schlüsselbereitstellung oder der Ausführung von Vorgängen zur Datenverschlüsselung oder -entschlüsselung beteiligt werden. Sie können SQL Server Management Studio oder PowerShell verwenden, um solche Aufgaben auszuführen. 

|Aufgabe|SSMS|PowerShell|T-SQL|
|:---|:---|:---|:---
|Bereitstellung von Spaltenhauptschlüsseln, Spaltenverschlüsselungsschlüsseln und Verschlüsselungsschlüsseln für verschlüsselte Spalten mit dem entsprechenden Spaltenhauptschlüssel|Ja|Ja|Nein |
|Erstellen von Schlüsselmetadaten in der Datenbank|Ja|Ja|Ja|
|Erstellen neuer Tabellen mit verschlüsselten Spalten|Ja|Ja|Ja|
|Verschlüsseln von vorhandenen Daten in ausgewählten Datenbankspalten|Ja|Ja|Nein |

> [!NOTE]
> [Always Encrypted mit Secure Enclaves](always-encrypted-enclaves.md), eingeführt in [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)], unterstützt die Verschlüsselung vorhandener Daten mit Transact-SQL. Außerdem entfällt die Notwendigkeit, die Daten für kryptografische Vorgänge außerhalb der Daten zu verschieben.

> [!NOTE]
> Stellen Sie sicher, dass Sie Schlüsselbereitstellungs- oder Datenverschlüsselungstools in einer sicheren Umgebung auf einem Computer ausführen, auf dem nicht Ihre Datenbank gehostet wird. Andernfalls könnten vertrauliche Daten oder die Schlüssel in der Serverumgebung zugänglich werden, was die Vorteile der Verwendung von Always Encrypted verringern würde.  

Weitere Informationen zum Konfigurieren von Always Encrypted finden Sie unter:
- [Konfigurieren von Always Encrypted mithilfe von SSMS](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)
- [Konfigurieren von Always Encrypted mithilfe von PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)

## <a name="getting-started-with-always-encrypted"></a>Erste Schritte mit Always Encrypted

Verwenden Sie den [Always Encrypted-Assistenten](../../../relational-databases/security/encryption/always-encrypted-wizard.md) , um schnell in Always Encrypted einzusteigen. Der Assistent stellt die benötigten Schlüssel bereit und konfiguriert die Verschlüsselung für ausgewählte Spalten. Wenn die Spalten, für die Sie die Verschlüsselung festlegen, schon Daten enthalten, verschlüsselt der Assistent die Daten. Im folgenden Beispiel wird veranschaulicht, wie eine Spalte verschlüsselt wird.

> [!NOTE]  
>  Ein Video, das die Verwendung des Assistenten erläutert, finden Sie unter [Getting Started with Always Encrypted with SSMS](https://channel9.msdn.com/Shows/Data-Exposed/Getting-Started-with-Always-Encrypted-with-SSMS)(Video in englischer Sprache).

1.  Stellen Sie eine Verbindung zu einer vorhandenen Datenbank her, die Tabellen mit Spalten enthält, die Sie mithilfe des **Objekt-Explorers** oder Management Studio verschlüsseln möchten, oder erstellen Sie eine neue Datenbank und anschließend eine Tabelle oder mehrere Tabellen mit Spalten, die verschlüsselt werden müssen, und stellen Sie eine Verbindung dazu her.
2.  Klicken Sie mit der rechten Maustaste auf Ihre Datenbank, zeigen Sie auf **Aufgaben**, und klicken Sie auf **Spalten verschlüsseln**, um den **Assistenten für Always Encrypted** zu öffnen.
3.  Lesen Sie die Seite **Einführung** , und klicken Sie dann auf **Weiter**.
4.  Erweitern Sie die Tabellen auf der **Spaltenauswahl** -Seite, und wählen Sie die Spalten aus, die Sie verschlüsseln möchten.
5.  Legen Sie für jede zu verschlüsselnde Spalte den **Verschlüsselungstyp** fest: entweder *deterministisch* oder *zufällig*.
6.  Wählen Sie für jede zu verschlüsselnde Spalte einen **Verschlüsselungsschlüssel** aus. Falls Sie noch keinen Verschlüsselungsschlüssel für diese Datenbank erstellt haben, wählen Sie die Standardoption (neuer, automatisch generierter Schlüssel) aus, und klicken Sie anschließend auf **Weiter**.
7.  Wählen Sie auf der Seite **Konfiguration des Hauptschlüssels** einen Speicherort für den neuen Schlüssel aus, wählen Sie eine Hauptschlüsselquelle aus, und klicken Sie anschließend auf **Weiter**.
8.  Wählen Sie auf der Seite **Überprüfung** aus, ob das Skript sofort ausgeführt oder ob ein PowerShell-Skript erstellt werden soll, und klicken Sie dann auf **Weiter**.
9.  Überprüfen Sie die ausgewählten Optionen auf der Seite **Zusammenfassung**, und klicken Sie dann auf **Fertig stellen**. Schließen Sie den Assistenten nach Abschluss.

  
## <a name="feature-details"></a>Details zur Funktion  
  
-   Abfragen können in deterministisch verschlüsselten Spalten Gleichheitsvergleiche durchführen. Andere Vorgänge (z.B. „größer/kleiner als“, Mustervergleiche mittels LIKE-Operator oder arithmetische Operationen) können nicht durchgeführt werden.  
  
-   Abfragen zu Spalten, die mithilfe der zufälligen Verschlüsselung verschlüsselt wurden, können für keine dieser Spalten Vorgänge durchführen. Das Indizieren von zufällig verschlüsselten Spalten wird nicht unterstützt.  
 > [!NOTE] 
 > [Always Encrypted mit Secure Enclaves](always-encrypted-enclaves.md), eingeführt in [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)], behebt die obige Einschränkung, indem ein Musterabgleich, Vergleichsoperatoren und Indexierung für Spalten mit zufälliger Verschlüsselung ermöglicht werden.

-   Ein Spaltenverschlüsselungsschlüssel kann bis zu zwei verschiedene verschlüsselte Werte aufweisen. Diese sind jeweils mit einem anderen Spaltenhauptschlüssel verschlüsselt. Dies erleichtert die Rotation der Spaltenhauptschlüssel.  
  
-   Die deterministische Verschlüsselung erfordert eine der [*binary2* - Sortierungen](../../../relational-databases/collations/collation-and-unicode-support.md)in der Spalte.  

-   Führen Sie nach der Änderung der Definition [sp_refresh_parameter_encryption](../../../relational-databases/system-stored-procedures/sp-refresh-parameter-encryption-transact-sql.md) aus, um die Always Encrypted-Metadaten für das Objekt zu aktualisieren.
  
Always Encrypted wird für die Spalten mit den folgenden Merkmalen nicht unterstützt. Wenn beispielsweise eine der folgenden Bedingungen auf die Spalte zutrifft, kann die `ENCRYPTED WITH`-Klausel in `CREATE TABLE/ALTER TABLE` für eine Spalte nicht verwendet werden:  
  
-   Spalten, die einen der folgenden Datentypen verwenden: `xml`, `timestamp`/`rowversion`, `image`, `ntext`, `text`, `sql_variant`, `hierarchyid`, `geography`, `geometry`, Alias, benutzerdefinierte Typen  
- `FILESTREAM`-Spalten  
- Spalten mit der `IDENTITY`-Eigenschaft  
- Spalten mit der `ROWGUIDCOL`-Eigenschaft  
- Zeichenfolgenspalten (`varchar`, `char` usw.) mit Nicht-BIN2-Sortierungen  
- Spalten, die bei Verwendung der zufälligen Verschlüsselung Schlüssel für gruppierte und nicht gruppierte Indizes darstellen (deterministische Verschlüsselung wird unterstützt)
- In Volltextindizes eingeschlossene Spalten (Always Encrypted unterstützt die [Volltextsuche](../../../relational-databases/search/full-text-search.md) nicht)  
- Berechnete Spalten.
- Spalten, auf die berechnete Spalten verweisen (wenn der Ausdruck nicht unterstützte Vorgänge für Always Encrypted ausführt)  
- Sparsespalten  
- Spalten, auf die bei Verwendung der zufälligen Verschlüsselung von Statistiken verwiesen wird (deterministische Verschlüsselung wird unterstützt)  
- Spalten, die Aliastypen verwenden  
- Partitionierungsspalten  
- Spalten mit standardmäßigen Einschränkungen  
- Spalten, auf die Unique-Einschränkungen verweisen, wenn die zufällige Verschlüsselung verwendet wird (deterministische Verschlüsselung wird unterstützt)  
- Primärschlüsselspalten, wenn die zufällige Verschlüsselung verwendet wird (deterministische Verschlüsselung wird unterstützt)  
- Verweisende Spalten in Fremdschlüsseleinschränkungen bei Verwendung der zufälligen Verschlüsselung oder bei Verwendung der deterministischen Verschlüsselung, falls die referenzierten und die verweisenden Spalten verschiedene Schlüssel oder Algorithmen verwenden  
- Spalten, auf die Check-Einschränkungen verweisen  
- Spalten, die mithilfe von Change Data Capture erfasst oder nachverfolgt werden  
- Primärschlüsselspalten in Tabellen mit Änderungsnachverfolgung  
- Spalten, die maskiert werden (mithilfe der dynamischen Datenmaskierung)  
- Spalten in Stretch-aktivierten Tabellen (Tabellen mit Spalten, die mit Always Encrypted verschlüsselt sind, können für Stretch aktiviert werden.)  
- Spalten in externen (PolyBase)-Tabellen (Hinweis: Die Verwendung von externen Tabellen und Tabellen mit verschlüsselten Spalten in der gleichen Abfrage wird unterstützt)  
- Tabellenwertparameter, die auf verschlüsselte Spalten ausgerichtet sind, werden nicht unterstützt  

Die folgenden Klauseln dürfen in verschlüsselten Spalten nicht verwendet werden:

- `FOR XML`
- `FOR JSON PATH`

Die folgenden Funktionen funktionieren nicht bei verschlüsselten Spalten:

- Transaktions- oder Mergereplikation
- Verteilte Abfragen (Verbindungsserver, `OPENROWSET`(T-SQL), `OPENDATASOURCE`(T-SQL))

Anforderungen an Tools

- SQL Server Management Studio-Version 18 oder höher wird empfohlen, um Abfragen auszuführen, die die aus verschlüsselten Spalten abgerufenen Ergebnisse entschlüsseln oder verschlüsselte Spalten einfügen, aktualisieren oder filtern. 
- Erfordert `sqlcmd`-Version 13.1 oder höher, die im [Download Center](https://go.microsoft.com/fwlink/?LinkID=825643) verfügbar ist.

  
## <a name="database-permissions"></a>Datenbankberechtigungen  
 Es gibt vier Berechtigungen für Always Encrypted:  
  
*  `ALTER ANY COLUMN MASTER KEY` (Zum Erstellen und Löschen eines Spaltenhauptschlüssels erforderlich.)  
  
*  `ALTER ANY COLUMN ENCRYPTION KEY` (Zum Erstellen und Löschen eines Spaltenverschlüsselungsschlüssels erforderlich.) 
  
*  `VIEW ANY COLUMN MASTER KEY DEFINITION` (Zum Lesen der Metadaten der Spaltenhauptschlüssel sowie für den Zugriff erforderlich, um Schlüssel zu verwalten oder verschlüsselte Spalten abzufragen.)  
  
*  `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` (Zum Lesen der Metadaten des Spaltenverschlüsselungsschlüssels sowie für den Zugriff erforderlich, um Schlüssel zu verwalten oder verschlüsselte Spalten abzufragen.)  
  
 In der folgenden Tabelle werden die erforderlichen Berechtigungen für häufig verwendete Aktionen zusammengefasst.  
  
|Szenario|<code>ALTER ANY COLUMN MASTER KEY</code>|<code>ALTER ANY COLUMN ENCRYPTION KEY</code>|<code>VIEW ANY COLUMN MASTER KEY DEFINITION</code>|<code>VIEW ANY COLUMN ENCRYPTION KEY DEFINITION</code>|  
|--------------|-----------------------------------|---------------------------------------|---------------------------------------------|-------------------------------------------------|  
|Schlüsselverwaltung (Erstellen/Ändern/Überprüfen von Schlüsselmetadaten in der Datenbank)|X|X|X|X|  
|Abfragen von verschlüsselten Spalten|||X|X|  
  
 **Wichtige Hinweise:**  
  
-   Die Berechtigungen gelten für Aktionen, die [!INCLUDE[tsql](../../../includes/tsql-md.md)], [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] (Dialogfelder und Assistent) oder PowerShell verwenden.  
  
-   Die beiden *VIEW*-Berechtigungen werden benötigt, wenn verschlüsselte Spalten ausgewählt werden, auch wenn der Benutzer über keine Berechtigung für die Entschlüsselung der Spalten verfügt.  
  
-   In [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], werden beide *VIEW* -Berechtigungen standardmäßig an die feste Datenbankrolle `public` erteilt. Ein Datenbankadministrator kann die *VIEW* -Berechtigungen an die `public` -Rolle aufheben (oder verweigern) und sie bestimmten Rollen oder Benutzern gewähren, um mehr eingeschränkte Kontrolle zu implementieren.  
  
-   In [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] werden beide *VIEW*-Berechtigungen standardmäßig nicht an die feste Datenbankrolle `public` erteilt. Auf diese Weise können bestimmte vorhandene Legacytools (die ältere Versionen von DacFx verwenden) ordnungsgemäß funktionieren. Demzufolge muss ein Datenbankadministrator explizit die beiden *VIEW* -Berechtigungen erteilen, um mit verschlüsselten Spalten zu arbeiten (auch wenn sie nicht entschlüsselt werden).  

  
## <a name="example"></a>Beispiel  
 Der folgende [!INCLUDE[tsql](../../../includes/tsql-md.md)] -Befehl erstellt Spaltenhauptschlüssel-Metadaten, Spaltenverschlüsselungsschlüssel-Metadaten und eine Tabelle mit verschlüsselten Spalten. Informationen zum Erstellen der Schlüssel, auf die in den Metadaten verwiesen wird, finden Sie unter:
- [Konfigurieren von Always Encrypted mithilfe von SSMS](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)
- [Konfigurieren von Always Encrypted mithilfe von PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)

  
```sql  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
     KEY_STORE_PROVIDER_NAME = 'MSSQL_CERTIFICATE_STORE',   
     KEY_PATH = 'Current User/Personal/f2260f28d909d21c642a3d8e0b45a830e79a1420'  
   );  
---------------------------------------------  
CREATE COLUMN ENCRYPTION KEY MyCEK   
WITH VALUES  
(  
    COLUMN_MASTER_KEY = MyCMK,   
    ALGORITHM = 'RSA_OAEP',   
    ENCRYPTED_VALUE = 0x01700000016C006F00630061006C006D0061006300680069006E0065002F006D0079002F003200660061006600640038003100320031003400340034006500620031006100320065003000360039003300340038006100350064003400300032003300380065006600620063006300610031006300284FC4316518CF3328A6D9304F65DD2CE387B79D95D077B4156E9ED8683FC0E09FA848275C685373228762B02DF2522AFF6D661782607B4A2275F2F922A5324B392C9D498E4ECFC61B79F0553EE8FB2E5A8635C4DBC0224D5A7F1B136C182DCDE32A00451F1A7AC6B4492067FD0FAC7D3D6F4AB7FC0E86614455DBB2AB37013E0A5B8B5089B180CA36D8B06CDB15E95A7D06E25AACB645D42C85B0B7EA2962BD3080B9A7CDB805C6279FE7DD6941E7EA4C2139E0D4101D8D7891076E70D433A214E82D9030CF1F40C503103075DEEB3D64537D15D244F503C2750CF940B71967F51095BFA51A85D2F764C78704CAB6F015EA87753355367C5C9F66E465C0C66BADEDFDF76FB7E5C21A0D89A2FCCA8595471F8918B1387E055FA0B816E74201CD5C50129D29C015895CD073925B6EA87CAF4A4FAF018C06A3856F5DFB724F42807543F777D82B809232B465D983E6F19DFB572BEA7B61C50154605452A891190FB5A0C4E464862CF5EFAD5E7D91F7D65AA1A78F688E69A1EB098AB42E95C674E234173CD7E0925541AD5AE7CED9A3D12FDFE6EB8EA4F8AAD2629D4F5A18BA3DDCC9CF7F352A892D4BEBDC4A1303F9C683DACD51A237E34B045EBE579A381E26B40DCFBF49EFFA6F65D17F37C6DBA54AA99A65D5573D4EB5BA038E024910A4D36B79A1D4E3C70349DADFF08FD8B4DEE77FDB57F01CB276ED5E676F1EC973154F86  
);  
---------------------------------------------  
CREATE TABLE Customers (  
    CustName nvarchar(60)   
        COLLATE  Latin1_General_BIN2 ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = MyCEK,  
        ENCRYPTION_TYPE = RANDOMIZED,  
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'),   
    SSN varchar(11)   
        COLLATE  Latin1_General_BIN2 ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = MyCEK,  
        ENCRYPTION_TYPE = DETERMINISTIC ,  
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256'),   
    Age int NULL  
);  
GO  
  
```  
## <a name="see-also"></a>Weitere Informationen  
- [Konfigurieren von Always Encrypted mithilfe von SSMS](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)   
- [Konfigurieren von Always Encrypted mithilfe von PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)   
- [Entwickeln von Anwendungen mit Always Encrypted](always-encrypted-client-development.md) 
- [Konfigurieren der Spaltenverschlüsselung mit dem Always Encrypted-Assistenten](always-encrypted-wizard.md)
- [Always Encrypted-Kryptografie](../../../relational-databases/security/encryption/always-encrypted-cryptography.md)   
- [CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-column-master-key-transact-sql.md)   
- [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)   
- [CREATE TABLE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-table-transact-sql.md)   
- [column_definition &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-table-column-definition-transact-sql.md)   
- [sys.column_encryption_keys  &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)  
- [sys.column_encryption_key_values &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-column-encryption-key-values-transact-sql.md)   
- [sys.column_master_keys &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)   
- [sys.columns &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
- [sp_refresh_parameter_encryption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-refresh-parameter-encryption-transact-sql.md)   
  
  
