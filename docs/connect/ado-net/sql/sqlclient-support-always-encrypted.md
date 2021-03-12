---
title: Verwenden von Always Encrypted mit SqlClient
description: Erfahren Sie, wie Sie Anwendung mit Microsoft.Data.SqlClient und Always Encrypted entwickeln, um Ihre Daten zu schützen.
ms.date: 03/03/2021
ms.assetid: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: d5b2e79205e3c82e22f723536e594fffd06ee99f
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464876"
---
# <a name="using-always-encrypted-with-the-microsoft-net-data-provider-for-sql-server"></a>Verwenden von Always Encrypted mit dem Microsoft .NET-Datenanbieter für SQL Server

[!INCLUDE[appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

Dieser Artikel bietet Informationen zum Entwickeln von .NET-Anwendungen mithilfe von [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) oder [Always Encrypted mit Secure Enclaves](../../../relational-databases/security/encryption/always-encrypted-enclaves.md) und dem [**Microsoft .NET-Datenanbieter für SQL Server**](../microsoft-ado-net-sql-server.md).

Always Encrypted ermöglicht Clientanwendungen das Verschlüsseln von vertraulichen Daten in einer Weise, dass weder die Daten noch die Verschlüsselungsschlüssel zu irgendeinem Zeitpunkt gegenüber SQL Server oder Azure SQL-Datenbank offengelegt werden. Ein für Always Encrypted aktivierter Treiber (z. B. **Microsoft .NET-Datenanbieter für SQL Server**) bietet diese Sicherheit durch die transparente Ver- und Entschlüsselung sensibler Daten in der Clientanwendung. Der Treiber ermittelt automatisch, welche Abfrageparameter vertraulichen Datenbankspalten (mit Always Encrypted geschützt) entsprechen. Die Werte dieser Parameter werden dann vor der Übergabe an SQL Server oder Azure SQL-Datenbank verschlüsselt. Auf ähnliche Weise entschlüsselt der Treiber die Daten transparent, die von verschlüsselten Datenbankspalten in Abfrageergebnissen empfangen werden. Weitere Informationen finden Sie unter [Always Encrypted (Cliententwicklung)](../../../relational-databases/security/encryption/always-encrypted-client-development.md) und [Entwickeln von Anwendungen mithilfe von Always Encrypted mit Secure Enclaves](../../../relational-databases/security/encryption/always-encrypted-enclaves-client-development.md).

## <a name="prerequisites"></a>Voraussetzungen

- Konfigurieren Sie Always Encrypted in Ihrer Datenbank. Dieser Vorgang beinhaltet die Bereitstellung von Always Encrypted-Schlüsseln und die Einrichtung der Verschlüsselung für ausgewählte Datenbankspalten. Wenn Sie nicht bereits über eine Datenbank verfügen, für die Always Encrypted konfiguriert ist, befolgen Sie die Anweisungen in [Erste Schritte mit Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted).
- Weitere Voraussetzungen für die Verwendung von Always Encrypted mit Secure Enclaves finden Sie unter [Entwickeln von Anwendungen mithilfe von Always Encrypted mit Secure Enclaves](../../../relational-databases/security/encryption/always-encrypted-enclaves-client-development.md).
- Stellen Sie sicher, dass auf dem Entwicklungscomputer die erforderliche .NET-Plattform installiert ist. Mit [Microsoft.Data.SqlClient](../microsoft-ado-net-sql-server.md) wird das Feature Always Encrypted sowohl für .NET Framework als auch für .NET Core unterstützt. Stellen Sie sicher, dass in Ihrer Entwicklungsumgebung mindestens [.NET Framework 4.6](/dotnet/framework/) bzw. [.NET Core 2.1](/dotnet/core/) als .NET-Zielplattformversion konfiguriert ist. Ab Microsoft.Data.SqlClient-Version 2.1.0 wird das Feature Always Encrypted auch von [.NET Standard 2.0](/dotnet/standard/net-standard) unterstützt. Damit Always Encrypted mit Secure Enclaves verwendet werden kann, ist [.NET Standard 2.1](/dotnet/standard/net-standard) erforderlich. Wenn Sie Visual Studio verwenden, lesen Sie [Übersicht über Frameworkziele](/visualstudio/ide/visual-studio-multi-targeting-overview).

Die nachfolgende Tabelle enthält eine Übersicht über die erforderlichen .NET-Plattformen für die Nutzung von Always Encrypted mit **Microsoft.Data.SqlClient**.

| Unterstützung von Always Encrypted | Verwenden von Always Encrypted mit Secure Enclaves  | Zielframework | Microsoft.Data.SqlClient-Version | Betriebssystem |
|:--|:--|:--|:--:|:--:|
| Ja | Ja | .NET Framework 4.6+ | Ab 1.1.0 | Windows |
| Ja | Ja | .NET Core 2.1 oder höher | Ab 2.1.0<sup>1</sup> | Windows, Linux, macOS |
| Ja | Nein  | .NET-Standard 2.0 | Ab 2.1.0 | Windows, Linux, macOS |
| Ja | Ja | Ab .NET Standard 2.1 | Ab 2.1.0 | Windows, Linux, macOS |

> [!NOTE]
> <sup>1</sup> Vor Microsoft.Data.SqlClient-Version 2.1.0 wird Always Encrypted nur unter Windows unterstützt.

## <a name="enabling-always-encrypted-for-application-queries"></a>Aktivieren von Always Encrypted für Anwendungsabfragen

Die einfachste Möglichkeit, die Verschlüsselung von Parametern und die Entschlüsselung von Abfrageergebnissen, die auf verschlüsselte Spalten abzielen, zu aktivieren, besteht darin, den Wert des Schlüsselworts der Verbindungszeichenfolge `Column Encryption Setting` auf **enabled** festzulegen.

Im Folgenden wird ein Beispiel einer Verbindungszeichenfolge verwendet, die Always Encrypted aktiviert:

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
SqlConnection connection = new SqlConnection(connectionString);
```

Im folgenden Codeausschnitt finden Sie auch ein gleichwertiges Beispiel, das die SqlConnectionStringBuilder.ColumnEncryptionSetting-Eigenschaft verwendet.

```cs
SqlConnectionStringBuilder builder = new SqlConnectionStringBuilder();
builder.DataSource = "server63";
builder.InitialCatalog = "Clinic";
builder.IntegratedSecurity = true;
builder.ColumnEncryptionSetting = SqlConnectionColumnEncryptionSetting.Enabled;
SqlConnection connection = new SqlConnection(builder.ConnectionString);
connection.Open();
```

Always Encrypted kann auch für einzelne Abfragen aktiviert werden. Weitere Informationen finden Sie im Abschnitt **Kontrollieren der Auswirkungen von Always Encrypted auf die Leistung**.
Die Aktivierung von Always Encrypted ist für eine erfolgreiche Verschlüsselung und Entschlüsselung nicht ausreichend. Sie müssen auch Folgendes sicherstellen:

- Die Anwendung verfügt über die Datenbankberechtigungen *VIEW ANY COLUMN MASTER KEY DEFINITION* und *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* , die für den Zugriff auf die Metadaten in der Datenbank über Always Encrypted-Schlüssel erforderlich sind. Weitere Informationen finden Sie im [Abschnitt „Datenbankberechtigungen“ in Always Encrypted (Datenbank-Engine)](../../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).
- Die Anwendung kann auf den Hauptschlüssel der Spalte zugreifen, der die Spaltenverschlüsselungsschlüssel schützt, mit denen die abgefragten Datenbankspalten verschlüsselt werden.

## <a name="enabling-always-encrypted-with-secure-enclaves"></a>Aktivieren von Always Encrypted mit Secure Enclaves

Ab Microsoft.Data.SqlClient-Version 1.1.0 unterstützt der Treiber [Always Encrypted mit Secure Enclaves](../../../relational-databases/security/encryption/always-encrypted-enclaves.md).

Allgemeine Informationen zum Entwickeln mit Enclaves finden Sie unter [Entwickeln von Anwendungen mithilfe von Always Encrypted mit Secure Enclaves](../../../relational-databases/security/encryption/always-encrypted-enclaves-client-development.md).

Um Enclaveberechnungen für eine Datenbankverbindung zu ermöglichen, müssen Sie zusätzlich zur Aktivierung von Always Encrypted die folgenden Schlüsselwörter für die Verbindungszeichenfolge festlegen (wie im vorherigen Abschnitt erläutert):

- `Attestation Protocol`: gibt ein Nachweisprotokoll an.
  - Wenn Sie [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] und den Host-Überwachungsdienst (Host Guardian Service, HGS) verwenden, sollte der Wert dieses Schlüsselworts `HGS` lauten.
  - Wenn Sie [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] und Microsoft Azure Attestation verwenden, sollte der Wert dieses Schlüsselworts `AAS` lauten.

- `Enclave Attestation URL`: gibt eine Nachweis-URL (einen Endpunkt für den Nachweisdienst) an. Sie benötigen für Ihre Umgebung eine Nachweis-URL von dem Dienstadministrator, der für Nachweise zuständig ist.

  - Wenn Sie [!INCLUDE[ssnoversion-md](../../../includes/ssnoversion-md.md)] und den Host-Überwachungsdienst verwenden, finden Sie weitere Informationen unter [Ermitteln und Freigeben der HGS-Nachweis-URL](../../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Wenn Sie [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] und Microsoft Azure Attestation verwenden, finden Sie weitere Informationen unter [Ermitteln der Nachweis-URL für Ihre Nachweisrichtlinie](../../../relational-databases/security/encryption/always-encrypted-enclaves.md#secure-enclave-attestation).

Ein Schritt-für-Schritt-Tutorial finden Sie unter [Tutorial: Entwickeln einer .NET-Anwendung mithilfe von Always Encrypted mit Secure Enclaves](tutorial-always-encrypted-enclaves-develop-net-apps.md).

## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Abrufen und Ändern von Daten in verschlüsselten Spalten

Nachdem Sie Always Encrypted für Anwendungsabfragen aktiviert haben, können Sie mithilfe der standardmäßigen SqlClient-APIs (siehe [Abrufen und Ändern von Daten in ADO.NET](/dotnet/framework/data/adonet/retrieving-and-modifying-data)) oder der APIs für [**Microsoft .NET-Datenanbieter für SQL Server**](index.md), die im Namespace [Microsoft.Data.SqlClient](/dotnet/api/microsoft.data.sqlclient) definiert sind, die Daten in verschlüsselten Datenbankspalten abrufen oder ändern. Wenn Ihre Anwendung über die erforderlichen Datenbankberechtigungen verfügt und auf den Hauptschlüssel für die Spalte zugreifen kann, verschlüsselt **Microsoft .NET-Datenanbieter für SQL Server** alle Abfrageparameter, die auf verschlüsselte Spalten abzielen, und entschlüsselt die aus verschlüsselten Spalten abgerufenen Daten. Zurückgegeben werden Klartextwerte von .NET-Typen, die den SQL Server-Datentypen entsprechen, die für die Spalten im Datenbankschema festgelegt wurden.
Wenn Always Encrypted nicht aktiviert ist, tritt bei Abfragen mit Parametern, die verschlüsselte Spalten anzielen, ein Fehler auf. Abfragen können weiterhin Daten aus verschlüsselten Spalten abrufen, solange die Abfrage keine Parameter für verschlüsselte Spalten enthält. Die **Microsoft .NET-Datenanbieter für SQL Server** versuchen jedoch nicht, die aus verschlüsselten Spalten abgerufenen Werte zu entschlüsseln, daher erhält die Anwendung binär verschlüsselte Daten (als Bytearrays).

Die folgende Tabelle fasst das Verhalten von Abfragen in Abhängigkeit davon zusammen, ob Always Encrypted aktiviert ist:

|Abfragemerkmal | Always Encrypted ist aktiviert, und die Anwendung kann auf die Schlüssel und Schlüsselmetadaten zugreifen.|Always Encrypted ist aktiviert, und die Anwendung kann nicht auf die Schlüssel oder Schlüsselmetadaten zugreifen. | Always Encrypted ist deaktiviert|
|:---|:---|:---|:---|
| Abfragen mit Parametern, die auf verschlüsselte Spalten ausgerichtet sind. | Parameterwerte werden transparent verschlüsselt. | Fehler | Fehler |
| Abfragen, bei denen Daten von verschlüsselten Spalten ohne Parameter abgerufen werden, die auf verschlüsselte Spalten ausgerichtet sind. | Ergebnisse von verschlüsselten Spalten werden transparent entschlüsselt. Die Anwendung empfängt Klartextwerte der .NET-Datentypen, die den SQL Server-Datentypen entsprechen, die für die verschlüsselten Spalten konfiguriert wurden. | Fehler | Ergebnisse von verschlüsselten Spalten werden nicht entschlüsselt. Die Anwendung erhält verschlüsselte Werte als Bytearrays (byte[]). |

Die folgenden Beispiele veranschaulichen das Abrufen und Ändern von Daten in verschlüsselten Spalten. Die Beispiele gehen von der Zieltabelle mit folgendem Schema aus. Die Spalten `SSN` und `BirthDate` sind verschlüsselt.

```sql
CREATE TABLE [dbo].[Patients]([PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY])
 GO
```

### <a name="inserting-data-example"></a>Beispiel zum Einfügen von Daten

In diesem Beispiel wird eine Zeile in die Tabelle „Patients“ eingefügt. Beachten Sie Folgendes:

- Es erfolgt keine spezielle Verschlüsselung im Beispielcode. Der **Microsoft .NET-Datenanbieter für SQL Server** erkennt und verschlüsselt die Parameter `paramSSN` und `paramBirthdate` automatisch, die auf verschlüsselte Spalten ausgerichtet sind. Durch dieses Verhalten wird die Verschlüsselung für die Anwendung transparent.
- Die in die Datenbankspalten eingefügten Werte, einschließlich der verschlüsselten Spalten, werden als [SqlParameter](/dotnet/api/microsoft.data.sqlclient.sqlparameter) -Objekte übergeben. Während die Verwendung von **SqlParameter** optional ist, wenn Werte an nicht verschlüsselte Spalten gesendet werden (obwohl sie dringend empfohlen wird, da sie dabei hilft, eine SQL-Einschleusung zu verhindern), ist sie für Werte erforderlich, die auf verschlüsselte Spalten ausgerichtet sind. Wenn die in die Spalten `SSN` oder `BirthDate` eingefügten Werte als Literale übergeben würden, die in die Abfrageanweisung eingebettet sind, würde bei der Abfrage ein Fehler auftreten, da der **Microsoft .NET-Datenanbieter für SQL Server** die Werte in den verschlüsselten Zielspalten nicht ermittelt könnte. Daher würde er die Werte nicht verschlüsseln. Daher würde der Server sie zurückweisen, da sie mit den verschlüsselten Spalten inkompatibel sind.
- Der auf die Spalte `SSN` ausgerichtete Datentyp des Parameters wird auf eine ANSI-Zeichenfolge (Nicht-Unicode) festgelegt, der dem SQL Server-Datentyp „char/varchar“ zuordnet wird. Wenn der Typ des Parameters eine Unicode-Zeichenfolge (String) ist, die „nchar/nvarchar“ zugeordnet wird, würde bei der Abfrage ein Fehler auftreten, da Always Encrypted keine Konvertierungen von verschlüsselten „nchar/nvarchar“-Werten in verschlüsselte „char/narchar“-Werte unterstützt. Weitere Informationen zu den Datentypzuordnungen finden Sie unter [SQL Server-Datentypzuordnungen](/dotnet/framework/data/adonet/sql-server-data-type-mappings) .
- Der Datentyp des in die Spalte `BirthDate` eingefügten Parameters wird mithilfe der [SqlParameter.SqlDbType](/dotnet/api/microsoft.data.sqlclient.sqlparameter.sqldbtype)-Eigenschaft explizit auf den SQL Server-Datentyp festgelegt, anstatt auf die implizite Zuordnung von .NET-Typen zu vertrauen, die bei der Verwendung der [SqlParameter.DbType](/dotnet/api/microsoft.data.sqlclient.sqlparameter.dbtype)-Eigenschaft angewendet werden. Standardmäßig wird die [DateTime-Struktur](/dotnet/api/system.datetime) dem SQL Server-Datentyp „datetime“ zugeordnet. Da der Datentyp der Spalte `BirthDate` dem Wert „date“ entspricht und Always Encrypted keine Konvertierung von verschlüsselten „datetime“-Werten in verschlüsselte „date“-Werte unterstützt, würde die Verwendung der Standardzuordnung zu einem Fehler führen.

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";

using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
using (SqlCommand cmd = connection.CreateCommand())
{
    connection.Open();
    cmd.CommandText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

    SqlParameter paramSSN = cmd.CreateParameter();
    paramSSN.ParameterName = @"@SSN";
    paramSSN.DbType = DbType.AnsiStringFixedLength;
    paramSSN.Direction = ParameterDirection.Input;
    paramSSN.Value = "795-73-9838";
    paramSSN.Size = 11;
    cmd.Parameters.Add(paramSSN);

    SqlParameter paramFirstName = cmd.CreateParameter();
    paramFirstName.ParameterName = @"@FirstName";
    paramFirstName.DbType = DbType.String;
    paramFirstName.Direction = ParameterDirection.Input;
    paramFirstName.Value = "Catherine";
    paramFirstName.Size = 50;
    cmd.Parameters.Add(paramFirstName);

    SqlParameter paramLastName = cmd.CreateParameter();
    paramLastName.ParameterName = @"@LastName";
    paramLastName.DbType = DbType.String;
    paramLastName.Direction = ParameterDirection.Input;
    paramLastName.Value = "Abel";
    paramLastName.Size = 50;
    cmd.Parameters.Add(paramLastName);

    SqlParameter paramBirthdate = cmd.CreateParameter();
    paramBirthdate.ParameterName = @"@BirthDate";
    paramBirthdate.SqlDbType = SqlDbType.Date;
    paramBirthdate.Direction = ParameterDirection.Input;
    paramBirthdate.Value = new DateTime(1996, 09, 10);
    cmd.Parameters.Add(paramBirthdate);

    cmd.ExecuteNonQuery();
}
```

### <a name="retrieving-plaintext-data-example"></a>Beispiel zum Abrufen von Klartextdaten

Im folgenden Beispiel wird das Filtern von Daten auf Basis verschlüsselter Werte und das Abrufen von Klartextdaten aus verschlüsselten Spalten veranschaulicht.

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true; Column Encryption Setting=enabled";
using (SqlConnection connection = new SqlConnection(builder.ConnectionString))
using (SqlCommand cmd = connection.CreateCommand())
{
    connection.Open();
    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN=@SSN";

    SqlParameter paramSSN = cmd.CreateParameter();
    paramSSN.ParameterName = @"@SSN";
    paramSSN.DbType = DbType.AnsiStringFixedLength;
    paramSSN.Direction = ParameterDirection.Input;
    paramSSN.Value = "795-73-9838";
    paramSSN.Size = 11;
    cmd.Parameters.Add(paramSSN);
    using (SqlDataReader reader = cmd.ExecuteReader())
    {
        if (reader.HasRows)
        {
            while (reader.Read())
            {
                Console.WriteLine(@"{0}, {1}, {2}, {3}", reader[0], reader[1], reader[2], ((DateTime)reader[3]).ToShortDateString());
            }
        }
    }
}
```

> [!NOTE]
>
> - Der in der WHERE-Klausel zum Filtern der Spalte `SSN` verwendete Wert muss mithilfe von SqlParameter übergeben werden, damit ihn der **Microsoft .NET-Datenanbieter für SQL Server** vor dem Senden an die Datenbank transparent verschlüsseln kann.
>
> - Alle Werte werden vom Programm als Klartext ausgegeben, da der **Microsoft .NET-Datenanbieter für SQL** Server die aus den Spalten `SSN` und `BirthDate` abgerufenen Daten transparent entschlüsselt.
>
> - Abfragen können auf Spalten Übereinstimmungsvergleiche ausführen, wenn sie mittels deterministischer Verschlüsselung verschlüsselt sind. Weitere Informationen finden Sie im Abschnitt [Auswählen der deterministischen oder zufälligen Verschlüsselung](../../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

### <a name="retrieving-encrypted-data-example"></a>Beispiel zum Abrufen von verschlüsselten Daten

Wenn Always Encrypted nicht aktiviert ist, können Abfragen weiterhin Daten aus verschlüsselten Spalten abrufen, so lange die Abfrage keine Parameter für verschlüsselte Spalten enthält.

Im folgenden Beispiel wird das Abrufen von binär verschlüsselten Daten aus verschlüsselten Spalten veranschaulicht.

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";

using (SqlConnection connection = new SqlConnection(connectionString))
using (SqlCommand cmd = connection.CreateCommand())
{
    connection.Open();
    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName]=@LastName";

    SqlParameter paramLastName = cmd.CreateParameter();
    paramLastName.ParameterName = @"@LastName";
    paramLastName.DbType = DbType.String;
    paramLastName.Direction = ParameterDirection.Input;
    paramLastName.Value = "Abel";
    paramLastName.Size = 50;
    cmd.Parameters.Add(paramLastName);
    using (SqlDataReader reader = cmd.ExecuteReader())
    {
        if (reader.HasRows)
        {
            while (reader.Read())
            {
                Console.WriteLine(@"{0}, {1}, {2}, {3}", BitConverter.ToString((byte[])reader[0]), reader[1], reader[2], BitConverter.ToString((byte[])reader[3]));
            }
        }
    }
}
```

> [!NOTE]
> - Da Always Encrypted in der Verbindungszeichenfolge nicht aktiviert ist, gibt die Abfrage verschlüsselte Werte von `SSN` und `BirthDate` als Bytearrays zurück (das Programm konvertiert die Werte in Zeichenfolgen).
>
> - Eine Abfrage, die Daten aus verschlüsselten Spalten mit deaktiviertem Always Encrypted abruft, kann Parameter aufweisen, so lange keiner der Parameter auf eine verschlüsselte Spalte ausgerichtet ist. Die obige Abfrage filtert nach der Spalte „LastName“, die in der Datenbank nicht verschlüsselt ist. Wenn die Abfrage nach `SSN` oder `BirthDate` filtert, würde ein Fehler auftreten.

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Vermeiden allgemeiner Probleme beim Abfragen von verschlüsselten Spalten

Dieser Abschnitt beschreibt die allgemeinen Kategorien von Fehlern bei der Abfrage verschlüsselter Spalten über .NET-Anwendungen sowie einige Grundsätze zum Vermeiden dieser Fehler.

### <a name="unsupported-data-type-conversion-errors"></a>Konvertierungsfehler durch nicht unterstützte Datentypen

Always Encrypted unterstützt einige Konvertierungen für verschlüsselte Datentypen. Eine ausführliche Liste der unterstützten Typkonvertierungen finden Sie unter [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md). Gehen Sie wie folgt vor, um Fehler bei Konvertierung des Datentyps zu vermeiden:

- Legen Sie die Typen der auf die verschlüsselten Spalten ausgerichteten Parameter so fest, dass der SQL Server-Datentyp des Parameters genau dem Typ der Zielspalte entspricht oder dass eine Konvertierung des SQL Server-Datentyps des Parameters in den Zieltyp der Spalte unterstützt wird. Sie können die gewünschte Zuordnung von .NET-Datentypen zu bestimmten SQL Server-Datentypen mithilfe der SqlParameter.SqlDbType-Eigenschaft erzwingen.
- Überprüfen Sie, ob die Genauigkeit und Dezimalstellenanzahl von Parametern, die auf Spalten der SQL Server-Datentypen „decimal“ und „numeric“ ausgerichtet sind, mit der für die Zielspalte konfigurierten Genauigkeit und Dezimalstellenanzahl identisch sind.  
- Überprüfen Sie, ob die Genauigkeit von Parametern, die auf Spalten der SQL Server-Datentypen „datetime2“, „datetimeoffset“ oder „time“ ausgerichtet sind, in Abfragen, in denen Werte der Zielspalte geändert werden, nicht größer als die Genauigkeit für die Zielspalte ist.  

### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Fehler aufgrund der Übergabe von Klartext anstelle von verschlüsselten Werten

Jeder Wert, der auf eine verschlüsselte Spalte ausgerichtet ist, muss in der Anwendung verschlüsselt werden. Der Versuch, einen Klartextwert einzufügen bzw. zu ändern oder nach einem Klartextwert für eine verschlüsselte Spalte zu filtern, führt zu einem Fehler wie dem folgenden:

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): Operand type clash: varchar is incompatible with varchar(8000) encrypted with (encryption_type = 'DETERMINISTIC', encryption_algorithm_name = 'AEAD_AES_256_CBC_HMAC_SHA_256', column_encryption_key_name = 'CEK_Auto1', column_encryption_key_database_name = 'Clinic') collation_name = 'SQL_Latin1_General_CP1_CI_AS'
```

Stellen Sie Folgendes sicher, um solche Fehler zu vermeiden:

- Always Encrypted ist für Anwendungsabfragen aktiviert, die auf verschlüsselte Spalten ausgerichtet sind (für die Verbindungszeichenfolge oder im [SqlCommand](/dotnet/api/microsoft.data.sqlclient.sqlcommand) -Objekt für eine bestimmte Abfrage).
- Sie verwenden „SqlParameter“ zum Senden von Daten, die auf verschlüsselte Spalten ausgerichtet sind. Das folgende Beispiel zeigt eine Abfrage, die falsch nach einem Literal bzw. einer Konstante einer verschlüsselten Spalte (SSN) filtert (anstatt das Literal innerhalb eines SqlParameter-Objekts zu übergeben).

```cs
using (SqlCommand cmd = connection.CreateCommand())
{
    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN = '795-73-9838'";
    cmd.ExecuteNonQuery();
}
```

## <a name="working-with-column-master-key-stores"></a>Arbeiten mit Spaltenhauptschlüsselspeichern

Der **Microsoft .NET-Datenanbieter für SQL Server** muss zum Verschlüsseln eines Parameterwerts oder zum Entschlüsseln von Daten in Abfrageergebnissen einen Spaltenverschlüsselungsschlüssel erhalten, der für die Zielspalte konfiguriert ist. Spaltenverschlüsselungsschlüssel werden in der verschlüsselten Form in den Datenbankmetadaten gespeichert. Jeder Spaltenverschlüsselungsschlüssel weist einen entsprechenden Spaltenhauptschlüssel auf, mit dem der Spaltenverschlüsselungsschlüssel verschlüsselt wurde. Die Spaltenhauptschlüssel werden nicht von den Datenbankmetadaten gespeichert. Sie enthalten lediglich die Informationen zu einem Schlüsselspeicher, der einen bestimmten Spaltenhauptschlüssel und die Position des Schlüssels im Schlüsselspeicher enthält.

Um einen Klartextwert eines Spaltenverschlüsselungsschlüssels zu erhalten, ruft der **Microsoft.NET-Datenanbieter für SQL Server** zunächst die Metadaten sowohl zum Spaltenverschlüsselungsschlüssel als auch zum entsprechenden Spaltenhauptschlüssel ab. Anschließend werden die Informationen in den Metadaten verwendet, um den Schlüsselspeicher zu kontaktieren, der den Spaltenhauptschlüssel enthält, und um den verschlüsselten Spaltenverschlüsselungsschlüssel zu entschlüsseln. Der **Microsoft .NET-Datenanbieter für SQL Server** kommuniziert mit einem Schlüsselspeicher, wobei ein Spaltenhauptschlüssel-Speicheranbieter verwendet wird, der eine Instanz einer Klasse darstellt, die von der Klasse [SqlColumnEncryptionKeyStoreProvider](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptionkeystoreprovider) abgeleitet wird.

Vorgang zum Abrufen eines Spaltenverschlüsselungsschlüssels:

1. Wenn Always Encrypted für eine Abfrage aktiviert ist, ruft der **Microsoft .NET-Datenanbieter für SQL Server** transparent **sys.sp_describe_parameter_encryption** auf, um Verschlüsselungsmetadaten für Parameter abzurufen, die verschlüsselte Spalten zum Ziel haben, falls die Abfrage Parameter aufweist. Für verschlüsselte Daten, die in den Ergebnissen einer Abfrage enthalten sind, fügt SQL Server automatisch Verschlüsselungsmetadaten an. Die Informationen über den Spaltenhauptschlüssel umfassen:
    - Den Namen eines Schlüsselspeicheranbieters, in dem ein Schlüsselspeicher verkapselt ist, der den Spaltenhauptschlüssel enthält.
    - Den Schlüsselpfad, der den Speicherort des Spaltenhauptschlüssels im Schlüsselspeicher angibt.

    Die Informationen über den Spaltenverschlüsselungsschlüssel umfassen:

    - Den verschlüsselten Wert eines Spaltenverschlüsselungsschlüssels.
    - Der Name des Algorithmus, der verwendet wurde, um den CEK zu verschlüsseln.
2. Der **Microsoft .NET-Datenanbieter für SQL Server** verwendet den Namen des Speicheranbieters des Spaltenhauptschlüssels, um das Anbieterobjekt (eine Instanz einer von der SqlColumnEncryptionKeyStoreProvider-Klasse abgeleiteten Klasse) in einer internen Datenstruktur nachzuschlagen.
3. Um den Spaltenverschlüsselungsschlüssel zu entschlüsseln, ruft der **Microsoft .NET-Datenanbieter für SQL Server** die Methode `SqlColumnEncryptionKeyStoreProvider.DecryptColumnEncryptionKey()` auf und übergibt den Spaltenhauptschlüsselpfad, den verschlüsselten Wert des Spaltenverschlüsselungsschlüssels und den Namen des Verschlüsselungsalgorithmus, der zum Erstellen des verschlüsselten Spaltenverschlüsselungsschlüssels verwendet wurde.

### <a name="using-built-in-column-master-key-store-providers"></a>Verwenden integrierter Spaltenhauptschlüssel-Speicheranbieter

Der **Microsoft .NET-Datenanbieter für SQL Server** enthält die folgenden integrierten Spaltenhauptschlüssel-Speicheranbieter, die mit den bestimmten Anbieternamen vorab registriert wurden (um den Anbieter zu suchen). Diese integrierten Schlüsselspeicheranbieter werden nur unter Windows unterstützt.

| Klasse | BESCHREIBUNG | Anbietername (Suche) | Plattform |
|:---|:---|:---|:---|
|[SqlColumnEncryptionCertificateStoreProvider-Klasse](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider) | Ein Anbieter für den Windows-Zertifikatspeicher. | MSSQL_CERTIFICATE_STORE | Windows |
|[SqlColumnEncryptionCngProvider-Klasse](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncngprovider) | Ein Anbieter für einen Schlüsselspeicher, der die [Microsoft Cryptography API unterstützt: Next Generation (CNG) API](/windows/win32/seccng/cng-portal). Üblicherweise handelt es sich bei einem solchen Speicher um ein Hardwaresicherheitsmodul – ein physisches Gerät, das digitale Schlüssel schützt und verwaltet und die kryptografische Verarbeitung bereitstellt. | MSSQL_CNG_STORE | Windows |
| [SqlColumnEncryptionCspProvider-Klasse](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncspprovider) | Ein Anbieter für einen Schlüsselspeicher, der die [Microsoft Cryptography API (CAPI)](/windows/win32/seccrypto/cryptographic-service-providers)unterstützt. Üblicherweise handelt es sich bei einem solchen Speicher um ein Hardwaresicherheitsmodul – ein physisches Gerät, das digitale Schlüssel schützt und verwaltet und die kryptografische Verarbeitung bereitstellt. | MSSQL_CSP_PROVIDER | Windows |

Sie müssen keine Änderungen am Anwendungscode vornehmen, um diese Anbieter zu verwenden. Beachten Sie aber Folgendes:

- Sie (oder der Datenbankadministrator) müssen sicherstellen, dass der in den Metadaten des Spaltenhauptschlüssels konfigurierte Anbietername richtig ist und der Pfad des Spaltenhauptschlüssels dem Schlüsselpfadformat entspricht, das für einen angegebenen Anbieter gültig ist. Es wird empfohlen, dass Sie die Schlüssel mithilfe von Tools wie SQL Server Management Studio konfigurieren, die die gültigen Anbieternamen und Schlüsselpfade automatisch generieren, wenn die Anweisung [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) ausgegeben wird. Weitere Informationen finden Sie unter [Konfigurieren von Always Encrypted mithilfe von SQL Server Management Studio](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md) und [Konfigurieren von Always Encrypted mithilfe von PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md).
- Stellen Sie sicher, dass die Anwendung auf den Schlüssel im Schlüsselspeicher zugreifen kann. Dieser Vorgang kann das Gewähren des Zugriffs auf den Schlüssel und/oder Schlüsselspeicher für die Anwendung (abhängig vom Schlüsselspeicher) oder das Ausführen anderer wichtiger speicherspezifischer Konfigurationsschritte beinhalten. Für den Zugriff auf einen Schlüsselspeicher beispielsweise, der eine CNG oder CAPI (z. B. ein Hardwaresicherheitsmodul) implementiert, müssen Sie sicherstellen, dass eine Bibliothek auf dem Anwendungscomputer installiert ist, die eine CNG oder CAPI für den Speicher implementiert. Ausführliche Informationen finden Sie unter [Erstellen und Speichern von Spaltenhauptschlüsseln für Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

### <a name="using-the-azure-key-vault-provider"></a>Verwenden des Azure Key Vault-Anbieters

Azure Key Vault ist eine praktische Möglichkeit zum Speichern von Spaltenhauptschlüsseln für Always Encrypted (insbesondere, wenn Ihre Anwendungen in Azure gehostet werden). Der **Microsoft .NET Framework-Datenprovider für SQL Server** umfasst keinen integrierten Spaltenhauptschlüssel-Speicheranbieter für Azure Key Vault, aber er steht als NuGet-Paket ([Microsoft.Data.SqLClient.AlwaysEncrypted.AzureKeyVaultProvider](https://www.nuget.org/packages/Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider)) bereit, das Sie problemlos mit Ihrer Anwendung integrieren können. Weitere Informationen finden Sie unter [Always Encrypted – Schützen vertraulicher Daten in SQL-Datenbank mit Datenverschlüsselung und Speichern von Verschlüsselungsschlüsseln in Azure Key Vault](/azure/azure-sql/database/always-encrypted-azure-key-vault-configure).

| Klasse | BESCHREIBUNG | Anbietername (Suche) | Plattform |
|:---|:---|:---|:---|
|[SqlColumnEncryptionAzureKeyVaultProvider Class](/dotnet/api/microsoft.data.sqlclient.alwaysencrypted.azurekeyvaultprovider.sqlcolumnencryptionazurekeyvaultprovider) | Anbieter für Azure Key Vault. | AZURE_KEY_VAULT | Windows, Linux, macOS |

#### <a name="net-supportability"></a>.NET-Unterstützung

| Version | Version von Microsoft.Data.SqlClient | .NET-Plattformen | 
| ------- | ----------- | ------- |
| 2.0.0 | Ab 1.1.3 <br/> Ab 2.1.0 | .NET Framework 4.6.1 und höher, .NET Core 2.1 und höher <br/>.NET Standard 2.0 und höher |
| 1.2.0 | Ab 1.0.19269.1 <br/> Ab 2.1.0 | .NET Framework 4.6 und höher, .NET Core 2.1 und höher <br/>.NET Standard 2.0 und höher |
| 1.1.0 | Ab 1.0.19269.1 | .NET Framework 4.6 und höher, .NET Core 2.1 und höher |
| 1.0.0 | Ab 1.0.19269.1 | .NET Framework 4.6 und höher, .NET Core 2.1 und höher |
|||

Ab Version **2.0.0** unterstützt`Microsoft.Data.SqLClient.AlwaysEncrypted.AzureKeyVaultProvider` die neuen Azure.Core- und Azure.Identity-APIs für die Authentifizierung mit Azure Key Vault. Eine Instanz der [`TokenCredential`](/dotnet/api/azure.core.tokencredential)-Implementierung kann jetzt an [`SqlColumnEncryptionAzureKeyVaultProvider`](/dotnet/api/microsoft.data.sqlclient.alwaysencrypted.azurekeyvaultprovider.sqlcolumnencryptionazurekeyvaultprovider)-Konstruktoren übergeben werden, um das Azure Key Vault-Anbieterobjekt zu initialisieren.

Beispiele zur Veranschaulichung der Ver- und Entschlüsselung mit Azure Key Vault finden Sie unter [Azure Key Vault mit Always Encrypted](azure-key-vault-example.md) und [Azure Key Vault mit Always Encrypted mit Secure Enclaves](azure-key-vault-enclave-example.md).

### <a name="implementing-a-custom-column-master-key-store-provider"></a>Implementieren eines benutzerdefinierten Speicheranbieters für den Spaltenhauptschlüssel

Wenn Sie Spaltenhauptschlüssel in einem Schlüsselspeicher speichern möchten, der nicht von einem vorhandenen Anbieter unterstützt wird, können Sie einen benutzerdefinierten Anbieter implementieren, indem Sie die [SqlColumnEncryptionKeyStoreProvider](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptionkeystoreprovider) -Klasse erweitern und den Anbieter mithilfe der [SqlConnection.RegisterColumnEncryptionKeyStoreProviders](/dotnet/api/microsoft.data.sqlclient.sqlconnection.registercolumnencryptionkeystoreproviders)-Methode registrieren.

```cs
public class MyCustomKeyStoreProvider : SqlColumnEncryptionKeyStoreProvider
{
    public const string ProviderName = "MY_CUSTOM_STORE";

    public override byte[] EncryptColumnEncryptionKey(string masterKeyPath, string encryptionAlgorithm, byte[] columnEncryptionKey)
    {
        // Logic for encrypting a column encrypted key.
    }
    public override byte[] DecryptColumnEncryptionKey(string masterKeyPath, string encryptionAlgorithm, byte[] EncryptedColumnEncryptionKey)
    {
        // Logic for decrypting a column encrypted key.
    }
}  
class Program
{
    static void Main(string[] args)
    {
        Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
            new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();
        providers.Add(MyCustomKeyStoreProvider.ProviderName, new MyCustomKeyStoreProvider());
        SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
        // ...
    }
}
```

### <a name="using-column-master-key-store-providers-for-programmatic-key-provisioning"></a>Verwenden von Spaltenhauptschlüssel-Speicheranbietern für die programmgesteuerte Schlüsselbereitstellung

Beim Zugriff auf verschlüsselte Spalten sucht der **Microsoft .NET-Datenanbieter für SQL Server** den richtigen Speicheranbieter für Spaltenhauptschlüssel transparent und ruft ihn anschließend auf, um die Spaltenverschlüsselungsschlüssel zu entschlüsseln. In der Regel ruft der normale Anwendungscode die Speicheranbieter für Spaltenhauptschlüssel nicht direkt auf. Sie können einen Anbieter jedoch explizit instanziieren und aufrufen, um Always Encrypted-Schlüssel programmgesteuert zum Generieren eines verschlüsselten Spaltenverschlüsselungsschlüssels und Entschlüsseln eines Spaltenverschlüsselungsschlüssels (z. B. im Rahmen einer Spaltenhauptschlüsselrotation) bereitzustellen und zu verwalten. Weitere Informationen finden Sie unter [Übersicht über die Schlüsselverwaltung für Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).
Die Implementierung eigener Schlüsselverwaltungstools ist möglicherweise nur erforderlich, wenn Sie einen benutzerdefinierten Schlüsselspeicheranbieter verwenden. Bei der Verwendung von Schlüsseln, die in Schlüsselspeichern (für die integrierte Anbieter vorhanden sind) und/oder in Azure Key Vault gespeichert werden, können Sie vorhandene Tools wie SQL Server Management Studio oder PowerShell verwenden, um Schlüssel zu verwalten und bereitzustellen.
Im folgenden Beispiel wird das Generieren eines Spaltenverschlüsselungsschlüssels und das Verwenden der [SqlColumnEncryptionCertificateStoreProvider](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider)-Klasse zum Verschlüsseln des Schlüssels mit einem Zertifikat veranschaulicht.

```cs
using System.Security.Cryptography;
static void Main(string[] args)
{
    byte[] EncryptedColumnEncryptionKey = GetEncryptedColumnEncryptonKey();
    Console.WriteLine("0x" + BitConverter.ToString(EncryptedColumnEncryptionKey).Replace("-", ""));
    Console.ReadKey();
}

static byte[]  GetEncryptedColumnEncryptonKey()
{
    int cekLength = 32;
    String certificateStoreLocation = "CurrentUser";
    String certificateThumbprint = "698C7F8E21B2158E9AED4978ADB147CF66574180";
    // Generate the plaintext column encryption key.
    byte[] columnEncryptionKey = new byte[cekLength];
    RNGCryptoServiceProvider rngCsp = new RNGCryptoServiceProvider();
    rngCsp.GetBytes(columnEncryptionKey);

    // Encrypt the column encryption key with a certificate.
    string keyPath = String.Format(@"{0}/My/{1}", certificateStoreLocation, certificateThumbprint);
    SqlColumnEncryptionCertificateStoreProvider provider = new SqlColumnEncryptionCertificateStoreProvider();
    return provider.EncryptColumnEncryptionKey(keyPath, @"RSA_OAEP", columnEncryptionKey);
}
```

## <a name="controlling-the-performance-impact-of-always-encrypted"></a>Kontrollieren der Auswirkungen von Always Encrypted auf die Leistung

Da Always Encrypted eine clientseitige Verschlüsselungstechnologie ist, werden die meisten Leistungseinbußen auf Clientseite und nicht in der Datenbank beobachtet. Neben den Kosten für Verschlüsselungs- und Entschlüsselungsvorgänge sorgen folgende andere Quellen für Leistungseinbußen auf Clientseite:

- Zusätzliche Roundtrips zur Datenbank zum Abrufen von Metadaten für Abfrageparameter.
- Aufrufe an einen Spaltenhauptschlüsselspeicher für den Zugriff auf einen Spaltenhauptschlüssel.

In diesem Abschnitt werden die integrierten Leistungsoptimierungen im **Microsoft .NET-Datenanbieter für SQL Server** und die Steuerung der Auswirkung der beiden oben genannten Faktoren auf die Leistung beschrieben.

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>Kontrollieren von Roundtrips zum Abrufen von Metadaten für Abfrageparameter

Wenn Always Encrypted für eine Verbindung aktiviert ist, ruft der **Microsoft .NET-Datenanbieter für SQL Server** standardmäßig [sys.sp_describe_parameter_encryption](../../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) für jede parametrisierte Abfrage auf, wobei die Abfrageanweisung (ohne Parameterwerte) an SQL Server übergeben wird. Die Abfrageanweisung wird von **sys.sp_describe_parameter_encryption** analysiert, um zu ermitteln, ob Parameter verschlüsselt werden müssen. In diesem Fall gibt sie die verschlüsselungsbezogenen Informationen zurück, die dem **Microsoft .NET-Datenanbieter für SQL Server** das Verschlüsseln von Parameterwerten ermöglichen. Das oben beschriebene Verhalten stellt ein hohes Maß an Transparenz für die Clientanwendung sicher. Die Anwendung (und der Anwendungsentwickler) muss nicht beachten, welche Abfragen Zugriff auf verschlüsselte Spalten haben, solange die auf verschlüsselte Spalten ausgerichteten Werte in SqlParameter-Objekten an den **Microsoft .NET-Datenanbieter für SQL Server** übergeben werden.

### <a name="query-metadata-caching"></a>Zwischenspeichern von Abfragemetadaten

Der **Microsoft .NET-Datenanbieter für SQL Server** speichert die Ergebnisse von **sys.sp_describe_parameter_encryption** für jede Abfrageanweisung zwischen. Daher ruft der Treiber **sys.sp_describe_parameter_encryption** nur einmal auf, wenn die gleiche Abfrageanweisung mehrfach ausgeführt wird. Die Zwischenspeicherung von Verschlüsselungsmetadaten für Abfrageanweisungen verringert die Leistungskosten beim Abrufen von Metadaten bei der Datenbank erheblich. Die Zwischenspeicherung ist standardmäßig aktiviert. Sie können die Zwischenspeicherung von Parametermetadaten deaktivieren, indem Sie die Eigenschaft [SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptionquerymetadatacacheenabled) auf FALSE festlegen. Dies wird jedoch mit Ausnahme seltener Fälle, wie dem unten beschriebenen, nicht empfohlen:

Betrachten Sie eine Datenbank mit zwei verschiedenen Schemas: `s1` und `s2`. Jedes Schema enthält eine Tabelle mit dem gleichen Namen: `t`. Die Definitionen der Tabellen `s1.t` und `s2.t` sind mit Ausnahme der verschlüsselungsbezogenen Eigenschaften identisch: Eine Spalte mit dem Namen `c` ist in `s1.t` nicht verschlüsselt, in `s2.t` hingegen schon. Die Datenbank weist zwei Benutzer auf, `u1` und `u2`. Das Standardschema für Benutzer von `u1` ist `s1`. Das Standardschema für `u2` ist `s2`. Eine .NET-Anwendung öffnet zwei Verbindungen mit der Datenbank, mit der Identität des Benutzers `u1` in der einen Verbindung und der des Benutzers `u2` in der anderen Verbindung. Die Anwendung sendet eine Abfrage mit einem Parameter, der auf die Spalte `c` abzielt, über die Verbindung für Benutzer `u1` (das Schema wird von der Abfrage nicht angegeben, sodass vom Standardbenutzerschema ausgegangen wird). Anschließend sendet die Anwendung die gleiche Abfrage über die Verbindung für den Benutzer `u2`. Wenn die Zwischenspeicherung von Metadaten aktiviert ist, ist der Cache nach der ersten Abfrage mit Metadaten aufgefüllt, die angeben, dass die Spalte `c`, auf die die Abfrageparameter verweisen, nicht verschlüsselt ist. Da die zweite Abfrage die gleiche Abfrageanweisung aufweist, werden die im Cache gespeicherten Informationen verwendet. Daher sendet der Treiber die Abfrage, ohne den Parameter zu verschlüsseln (was falsch ist, da die Zielspalte `s2.t.c` verschlüsselt ist), und legt so den Klartextwert des Parameters dem Server gegenüber offen. Der Server wird diese Inkompatibilität erkennen und den Treiber anweisen, den Cache zu aktualisieren, sodass die Anwendung die Abfrage transparent mit ordnungsgemäß verschlüsseltem Parameterwert erneut sendet. In einem solchen Fall sollte die Zwischenspeicherung deaktiviert werden, um die Offenlegung vertraulicher Werte gegenüber dem Server zu verhindern.

### <a name="setting-always-encrypted-at-the-query-level"></a>Festlegen von Always Encrypted auf Abfrageebene

Sie können Always Encrypted für einzelne Abfragen aktivieren, anstatt es für die Verbindung einzurichten, um beim Abrufen von Verschlüsselungsmetadaten für parametrisierte Abfragen die Auswirkung auf die Leistung zu steuern. Auf diese Weise können Sie sicherstellen, dass **sys.sp_describe_parameter_encryption** nur für Abfragen aufgerufen wird, bei denen Ihnen bekannt ist, dass sie über Parameter verfügen, die auf verschlüsselte Spalten ausgerichtet sind. Beachten Sie jedoch, dass Sie auf diese Weise die Transparenz der Verschlüsselung reduzieren: Wenn Sie die Verschlüsselungseigenschaften der Datenbankspalten ändern, müssen Sie möglicherweise den Code der Anwendung ändern, um ihn mit den Schemaänderungen auszurichten.

> [!NOTE]
> Die Festlegung von Always Encrypted auf Abfrageebene mindert den Leistungsvorteil der Zwischenspeicherung von Metadaten zur Parameterverschlüsselung.

Sie müssen diesen Konstruktor von [SqlCommand](/dotnet/api/microsoft.data.sqlclient.sqlcommand) und [SqlCommandColumnEncryptionSetting](/dotnet/api/microsoft.data.sqlclient.sqlcommandcolumnencryptionsetting)verwenden, um das Verhalten von Always Encrypted für einzelne Abfragen zu steuern. Hier sind einige nützliche Richtlinien:

- Die meisten Abfragen, die eine Clientanwendung ausführt, greifen auf verschlüsselte Spalten zu:
  - Legen Sie für das Verbindungszeichenfolgen-Kennwort für **Spaltenverschlüsselungseinstellung** den Wert **Aktiviert** fest.
  - Legen Sie für einzelne Abfragen, die nicht auf verschlüsselte Spalten zugreifen, **SqlCommandColumnEncryptionSetting** auf **Disabled** fest. Durch diese Einstellung werden sowohl der Aufruf von **sys.sp_describe_parameter_encryption** als auch der Versuch, Werte im Resultset zu entschlüsseln, deaktiviert.
  - Legen Sie für einzelne Abfragen, die keine zu verschlüsselnden Parameter aufweisen, aber Daten aus verschlüsselten Spalten abrufen, **SqlCommandColumnEncryptionSetting** auf **ResultSetOnly** fest. Dadurch werden der Aufruf von **sys.sp_describe_parameter_encryption** und die Parameterverschlüsselung deaktiviert. Die Abfrage ist in der Lage, die Ergebnisse von Spaltenverschlüsselungen zu entschlüsseln.
- Die meisten Abfragen, die eine Clientanwendung ausführt, greifen nicht auf verschlüsselte Spalten zu:
  - Legen Sie für das Verbindungszeichenfolgen-Kennwort für **Spaltenverschlüsselungseinstellung** den Wert **Deaktiviert** fest.
  - Legen Sie für einzelne Abfragen, die zu verschlüsselnde Parameter aufweisen, **SqlCommandColumnEncryptionSetting** auf **Enabled** fest. Dadurch werden sowohl der Aufruf von **sys.sp_describe_parameter_encryption** als auch die Entschlüsselung von aus verschlüsselten Spalten abgerufenen Abfrageergebnissen aktiviert.
  - Legen Sie für Abfragen, die keine zu verschlüsselnden Parameter aufweisen, aber Daten aus verschlüsselten Spalten abrufen, **SqlCommandColumnEncryptionSetting** auf **ResultSetOnly** fest. Durch diese Einstellung werden der Aufruf von **sys.sp_describe_parameter_encryption** und die Parameterverschlüsselung deaktiviert. Die Abfrage ist in der Lage, die Ergebnisse von Spaltenverschlüsselungen zu entschlüsseln.

In dem folgenden Beispiel ist Always Encrypted für die Datenbankverbindung deaktiviert. Die von der Anwendung ausgestellte Abfrage weist einen Parameter auf, der auf die nicht verschlüsselte Spalte „LastName“ ausgerichtet ist. Die Abfrage ruft Daten aus den Spalten `SSN` und `BirthDate` ab, die beide verschlüsselt sind. In diesem Fall ist der Aufruf von **sys.sp_describe_parameter_encryption** nicht erforderlich, um Verschlüsselungsmetadaten abzurufen. Die Entschlüsselung der Abfrageergebnisse muss jedoch aktiviert sein, damit die Anwendung Klartextwerte aus den beiden verschlüsselten Spalten erhalten kann. Mit der Einstellung von **SqlCommandColumnEncryptionSetting** auf **ResultSetOnly** wird dies sichergestellt.

```cs
string connectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
using (SqlConnection connection = new SqlConnection(connectionString))
using (SqlCommand cmd = new SqlCommand(@"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName]=@LastName",
connection, null, SqlCommandColumnEncryptionSetting.ResultSetOnly))
{
    connection.Open();
    SqlParameter paramLastName = cmd.CreateParameter();
    paramLastName.ParameterName = @"@LastName";
    paramLastName.DbType = DbType.String;
    paramLastName.Direction = ParameterDirection.Input;
    paramLastName.Value = "Abel";
    paramLastName.Size = 50;
    cmd.Parameters.Add(paramLastName);
    using (SqlDataReader reader = cmd.ExecuteReader())
    {
        if (reader.HasRows)
        {
            while (reader.Read())
            {
                Console.WriteLine(@"{0}, {1}, {2}, {3}", reader[0], reader[1], reader[2], ((DateTime)reader[3]).ToShortDateString());
            }
        }
    }
}
```

### <a name="column-encryption-key-caching"></a>Zwischenspeicherung von Spaltenverschlüsselungsschlüsseln

Der **Microsoft .NET-Datenanbieter für SQL Server** speichert die Spaltenverschlüsselungsschlüssel im Klartext im Speicher zwischen, um die Anzahl der Aufrufe an einen Spaltenhauptschlüsselspeicher zu verringern. Nach dem Erhalt des Verschlüsselungsschlüsselwerts für die verschlüsselte Spalte von den Datenbankmetadaten versucht der Treiber zunächst, den Spaltenverschlüsselungsschlüssel im Klartext zu finden, der dem verschlüsselten Schlüsselwert entspricht. Der Treiber ruft den Schlüsselspeicher, der den Spaltenhauptschlüssel enthält, nur dann auf, wenn er den verschlüsselten Spaltenverschlüsselungsschlüssel im Cache nicht finden kann.

Die Cacheeinträge werden nach einer konfigurierbaren Gültigkeitsdauer aus Sicherheitsgründen zwangsweise entfernt. Der Standardwert für die Gültigkeitsdauer beträgt 2 Stunden. Wenn Sie strengere Anforderungen an die Dauer der Speicherung von unverschlüsselten Spaltenverschlüsselungsschlüsseln in der Anwendung haben, können Sie den Wert mithilfe der Eigenschaft [SqlConnection.ColumnEncryptionKeyCacheTtl](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptionkeycachettl) ändern.

## <a name="enabling-extra-protection-for-a-compromised-sql-server"></a>Aktivieren von zusätzlichem Schutz für einen kompromittierten SQL Server

Standardmäßig verlässt sich der **Microsoft .NET-Datenanbieter für SQL Server** beim Bereitstellen von Metadaten über die in der Datenbank zu verschlüsselnden Spalten und die Art der Verschlüsselung auf das Datenbanksystem (SQL Server oder Azure SQL Database). Die Verschlüsselungsmetadaten ermöglichen dem **Microsoft .NET-Datenanbieter für SQL Server** das Verschlüsseln von Abfrageparametern und das Entschlüsseln von Abfrageergebnissen ohne irgendwelche Eingaben von der Anwendung, wodurch sich die Anzahl der in der Anwendung erforderlichen Änderungen erheblich reduziert. Wenn der SQL Server-Prozess jedoch gefährdet ist und ein Angreifer die Metadaten manipuliert, die von SQL Server an den **Microsoft .NET-Datenanbieter für SQL Server** gesendet werden, kann der Angreifer unter Umständen imstande sein, vertrauliche Informationen zu stehlen. In diesem Abschnitt werden APIs beschrieben, die das Bereitstellen einer zusätzlichen Schutzebene gegen diese Art von Angriff, jedoch auf Kosten geringerer Transparenz, unterstützen.

### <a name="forcing-parameter-encryption"></a>Erzwingen von Parameterverschlüsselung

Bevor der **Microsoft .NET-Datenanbieter für SQL Server** eine parametrisierte Abfrage an SQL Server sendet, bittet er SQL Server (durch Aufrufen von [sys.sp_describe_parameter_encryption](../../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md)), die Abfrageanweisung zu analysieren und Informationen über die in der Abfrage zu verschlüsselnden Parameter bereitzustellen. Eine gefährdete SQL Server-Instanz könnte den **Microsoft .NET-Datenanbieter für SQL Server** in die Irre führen, indem er Metadaten sendet, dass ein Parameter auf eine nicht verschlüsselte Spalte gerichtet ist, obwohl die Spalte in der Datenbank verschlüsselt ist. Der **Microsoft .NET-Datenanbieter für SQL Server** würde den Parameterwert daher nicht verschlüsseln und ihn im Klartext an die gefährdete SQL Server-Instanz senden.

Um einen derartigen Angriff zu verhindern, kann eine Anwendung die [SqlParameter.ForceColumnEncryption](/dotnet/api/microsoft.data.sqlclient.sqlparameter.forcecolumnencryption) -Eigenschaft für den Parameter auf WAHR festlegen. Bei dieser Einstellung löst **Microsoft .NET-Datenanbieter für SQL Server** eine Ausnahme aus, wenn aus den vom Server empfangenen Metadaten hervorgeht, dass der Parameter nicht verschlüsselt werden muss.

Die Eigenschaft **SqlParameter.ForceColumnEncryption** hilft zwar beim Verbessern der Sicherheit, jedoch verringert sie zugleich die Transparenz der Verschlüsselung für die Clientanwendung. Wenn Sie das Datenbankschema aktualisieren, um die Menge der verschlüsselten Spalten zu ändern, müssen Sie möglicherweise auch an der Anwendung Änderungen vornehmen.

Das folgende Codebeispiel veranschaulicht die Verwendung der **SqlParameter.ForceColumnEncryption**-Eigenschaft, um zu verhindern, dass Sozialversicherungsnummern in Klartext an die Datenbank gesendet werden.

```cs
using (SqlCommand cmd = _sqlconn.CreateCommand())
{
    // Use parameterized queries to access Always Encrypted data.

    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = @SSN;";

    SqlParameter paramSSN = cmd.CreateParameter();
    paramSSN.ParameterName = @"@SSN";
    paramSSN.DbType = DbType.AnsiStringFixedLength;
    paramSSN.Direction = ParameterDirection.Input;
    paramSSN.Value = ssn;
    paramSSN.Size = 11;
    paramSSN.ForceColumnEncryption = true;
    cmd.Parameters.Add(paramSSN);

    using (SqlDataReader reader = cmd.ExecuteReader())
    {
        // Do something.
    }
}
```

### <a name="configuring-trusted-column-master-key-paths"></a>Konfigurieren von vertrauenswürdigen Spaltenhauptschlüsselpfaden

Die Verschlüsselungsmetadaten, die SQL Server für Abfrageparameter mit verschlüsselten Spalten als Ziel und für die aus verschlüsselten Spalten abgerufenen Ergebnisse zurückgibt, beinhalten den Schlüsselpfad des Spaltenhauptschlüssels, der den Schlüsselspeicher und den Speicherort des Schlüssels im Schlüsselspeicher angibt. Wenn die SQL Server-Instanz gefährdet ist, könnte sie den Schlüsselpfad, der den **Microsoft .NET-Datenanbieter für SQL Server** anweist, an den von einem Angreifer kontrollierten Speicherort senden. Dieser Vorgang kann zur Preisgabe der Anmeldeinformationen des Schlüsselspeichers führen, falls der Schlüsselspeicher die Authentifizierung der Anwendung vorschreibt.

Um solche Angriffe zu verhindern, kann die Anwendung die Liste der vertrauenswürdigen Schlüsselpfade für einen bestimmten Server mithilfe der Eigenschaft [SqlConnection.ColumnEncryptionTrustedMasterKeyPaths](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptiontrustedmasterkeypaths) festlegen. Wenn der .**Microsoft .NET-Datenanbieter für SQL Server** einen Schlüsselpfad empfängt, der nicht in der Liste der vertrauenswürdigen Schlüsselpfade aufgeführt ist, löst er eine Ausnahme aus.

Durch das Festlegen vertrauenswürdiger Schlüsselpfade verbessert sich zwar die Sicherheit Ihrer Anwendung, Sie müssen jedoch bei jeder turnusmäßigen Änderung des Spaltenhauptschlüssels (d. h. bei jedem Wechsel des Spaltenhauptschlüsselpfads) Änderungen am Code und/oder an der Konfiguration der Anwendung vornehmen.

Das folgende Beispiel zeigt die Konfiguration von vertrauenswürdigen Spaltenhauptschlüsselpfaden:

```cs
// Configure trusted key paths to protect against fake key paths sent by a compromised SQL Server instance
// First, create a list of trusted key paths for your server
List<string> trustedKeyPathList = new List<string>();
trustedKeyPathList.Add("CurrentUser/my/425CFBB9DDDD081BB0061534CE6AB06CB5283F5Ea");

// Register the trusted key path list for your server
SqlConnection.ColumnEncryptionTrustedMasterKeyPaths.Add(serverName, trustedKeyPathList);

```

## <a name="copying-encrypted-data-using-sqlbulkcopy"></a>Kopieren von verschlüsselten Daten mithilfe von „SqlBulkCopy“

Mit „SqlBulkCopy“ können Sie Daten, die bereits verschlüsselt sind und in einer Tabelle gespeichert werden, in eine andere Tabelle kopieren, ohne die Daten zu entschlüsseln. Gehen Sie dafür folgendermaßen vor:

- Stellen Sie sicher, dass die Verschlüsselungskonfiguration der Zieltabelle mit der Konfiguration der Quelltabelle identisch ist. Insbesondere müssen für beide Tabellen dieselben Spalten verschlüsselt sein. Zudem müssen die Spalten mithilfe derselben Verschlüsselungstypen und mit denselben Verschlüsselungsschlüsseln verschlüsselt werden. Wenn eine der Zielspalten anders als die entsprechende Quellspalte verschlüsselt wurde, können Sie die Daten in der Zieltabelle nach dem Kopiervorgang nicht entschlüsseln. Die Daten werden beschädigt.
- Konfigurieren Sie beide Datenbankverbindungen, für die Quelltabelle und für die Zieltabelle, ohne Always Encrypted zu aktivieren.
- Legen Sie die Option `AllowEncryptedValueModifications` fest (siehe [SqlBulkCopyOptions](/dotnet/api/microsoft.data.sqlclient.sqlbulkcopyoptions)).

> [!NOTE]
> Lassen Sie bei der Angabe von `AllowEncryptedValueModifications` Vorsicht walten. Diese Einstellung kann zu einer Beschädigung der Datenbank führen, da **Microsoft .NET-Datenanbieter für SQL Server** nicht überprüft, ob die Daten tatsächlich verschlüsselt sind bzw. mit demselben Verschlüsselungstyp, Algorithmus und Schlüssel wie die Zielspalte ordnungsgemäß verschlüsselt wurden.

Hier folgt ein Beispiel, in dem Daten aus einer Tabelle in eine andere kopiert werden. Es wird angenommen, dass die Spalten `SSN` und `BirthDate` verschlüsselt sind.

```cs
static public void CopyTablesUsingBulk(string sourceTable, string targetTable)
{
    string sourceConnectionString = "Data Source=server63; Initial Catalog=Clinic; Integrated Security=true";
    string targetConnectionString = "Data Source=server64; Initial Catalog=Clinic; Integrated Security=true";
    using (SqlConnection connSource = new SqlConnection(sourceConnectionString))
    {
        connSource.Open();
        using (SqlCommand cmd = new SqlCommand(string.Format("SELECT [PatientID], [SSN], [FirstName], [LastName], [BirthDate] FROM {0}", sourceTable), connSource))
        {
            using (SqlDataReader reader = cmd.ExecuteReader())
            using (SqlBulkCopy copy = new SqlBulkCopy(targetConnectionString, SqlBulkCopyOptions.KeepIdentity | SqlBulkCopyOptions.AllowEncryptedValueModifications))
            {
                copy.EnableStreaming = true;
                copy.DestinationTableName = targetTable;
                copy.WriteToServer(reader);
            }
        }
    }
}
```

## <a name="always-encrypted-api-reference"></a>„Immer verschlüsselt“ – API-Referenz

**Namespace:** [Microsoft.Data.SqlClient](/dotnet/api/microsoft.data.sqlclient)

**Assembly:** Microsoft.Data.SqlClient.dll

|Name|BESCHREIBUNG|
|:---|:---|
|[SqlColumnEncryptionCertificateStoreProvider-Klasse](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncertificatestoreprovider)|Ein Schlüsselspeicheranbieter für den Windows-Zertifikatspeicher.|
|[SqlColumnEncryptionCngProvider-Klasse](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncngprovider)|Eine Schlüsselspeicheranbieter für die Microsoft Cryptography-API: Next Generation (CNG).|
|[SqlColumnEncryptionCspProvider-Klasse](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptioncspprovider)|Eine Schlüsselspeicheranbieter für die auf der Microsoft CAPI basierenden Kryptografiedienstanbieter.|
|[SqlColumnEncryptionKeyStoreProvider](/dotnet/api/microsoft.data.sqlclient.sqlcolumnencryptionkeystoreprovider)|Die Basisklasse der Schlüsselspeicheranbieter.|
|[SqlCommandColumnEncryptionSetting-Enumeration](/dotnet/api/microsoft.data.sqlclient.sqlcommandcolumnencryptionsetting)|Einstellungen zum Steuern des Verhaltens von „Always Encrypted“ für einzelne Abfragen.|
|[SqlConnectionAttestationProtocol-Enumeration](/dotnet/api/microsoft.data.sqlclient.sqlconnectionattestationprotocol)|Gibt einen Wert für das Nachweisprotokoll an, wenn Always Encrypted mit Secure Enclaves verwendet wird|
|[SqlConnectionColumnEncryptionSetting-Enumeration](/dotnet/api/microsoft.data.sqlclient.sqlconnectioncolumnencryptionsetting)|Einstellungen zum Aktivieren der Verschlüsselung und Entschlüsselung für eine Datenbankverbindung.|
|[SqlConnectionStringBuilder.ColumnEncryptionSetting-Eigenschaft](/dotnet/api/microsoft.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting)|Ruft Always Encrypted in der Verbindungszeichenfolge ab und legt es fest.|
|[SqlConnection.ColumnEncryptionQueryMetadataCacheEnabled](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptionquerymetadatacacheenabled)|Aktiviert und deaktiviert das Zwischenspeichern von Verschlüsselungsabfrage-Metadaten.|
|[SqlConnection.ColumnEncryptionKeyCacheTtl](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptionkeycachettl)|Ruft die Gültigkeitsdauer für Einträge im Cache für den Spaltenverschlüsselungsschlüssel ab und legt diese fest.|
|[SqlConnection.ColumnEncryptionTrustedMasterKeyPaths](/dotnet/api/microsoft.data.sqlclient.sqlconnection.columnencryptiontrustedmasterkeypaths)|Ermöglicht Ihnen, eine Liste von vertrauenswürdigen Schlüsselpfaden für einen Datenbankserver festzulegen. Wenn der Treiber während der Verarbeitung einer Anwendungsabfrage einen Schlüsselpfad empfängt, der nicht in der Liste enthalten ist, schlägt die Abfrage fehl. Diese Eigenschaft bietet zusätzlichen Schutz vor Angriffen, bei denen ein kompromittierter SQL Server gefälschte Schlüsselpfade bereitstellt, was zur Preisgabe der Anmeldeinformationen des Schlüsselspeichers führen kann.|
|[SqlConnection.RegisterColumnEncryptionKeyStoreProviders-Methode](/dotnet/api/microsoft.data.sqlclient.sqlconnection.registercolumnencryptionkeystoreproviders)|Ermöglicht Ihnen, benutzerdefinierte Schlüsselspeicheranbieter zu registrieren. Dies ist ein Wörterbuch, das Anbieternamen von Schlüsselspeichern Implementierungen von Schlüsselspeicheranbietern zuordnet.|
|[SqlCommand-Konstruktor (String, SqlConnection, SqlTransaction, SqlCommandColumnEncryptionSetting)](/dotnet/api/microsoft.data.sqlclient.sqlcommand.-ctor?view=sqlclient-dotnet-core-1.0&preserve-view=true#Microsoft_Data_SqlClient_SqlCommand__ctor_System_String_Microsoft_Data_SqlClient_SqlConnection_Microsoft_Data_SqlClient_SqlTransaction_Microsoft_Data_SqlClient_SqlCommandColumnEncryptionSetting_)|Ermöglicht das Steuern des Verhaltens von „Always Encrypted“ für einzelne Abfragen.|
|[SqlParameter.ForceColumnEncryption](/dotnet/api/microsoft.data.sqlclient.sqlparameter.forcecolumnencryption)|Erzwingt die Verschlüsselung eines Parameters. Wenn SQL Server den Treiber informiert, dass der Parameter nicht verschlüsselt werden muss, tritt bei der Abfrage, die diesen Parameter verwendet, ein Fehler auf. Diese Eigenschaft bietet zusätzlichen Schutz vor Angriffen, bei denen ein kompromittierter SQL Server dem Client falsche Verschlüsselungsmetadaten bereitstellt, was zur Offenlegung von Daten führen kann.|
|Schlüsselwort für [Verbindungszeichenfolgen](/dotnet/api/microsoft.data.sqlclient.sqlconnection.connectionstring): `Column Encryption Setting=enabled`|Aktiviert oder deaktiviert die „Always Encrypted“-Funktionalität für die Verbindung.|

## <a name="see-also"></a>Weitere Informationen

- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Always Encrypted mit Secure Enclaves](../../../relational-databases/security/encryption/always-encrypted-enclaves.md)
- [Tutorial zu SQL-Datenbank: Schützen vertraulicher Daten mit Always Encrypted](/azure/azure-sql/database/always-encrypted-certificate-store-configure)
- [Tutorial: Entwickeln einer .NET-Anwendung mithilfe von Always Encrypted mit Secure Enclaves](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Beispiel: Azure Key Vault mit Always Encrypted](azure-key-vault-example.md)
- [Beispiel: Azure Key Vault mit Always Encrypted mit Secure Enclaves](azure-key-vault-enclave-example.md)
