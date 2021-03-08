---
title: Herstellen einer Verbindung mit sqlcmd
description: Erfahren Sie, wie Sie das sqlcmd-Hilfsprogramm mit dem Microsoft ODBC Driver for SQL Server unter Linux und macOS verwenden.
ms.custom: ''
ms.date: 02/24/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- sqlcmd
ms.assetid: 61a2ec0d-1bcb-4231-bea0-cff866c21463
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 216f78615ca049d3e97134cb14831d9a5e6afd32
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837361"
---
# <a name="connecting-with-sqlcmd"></a>Herstellen einer Verbindung mit sqlcmd
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Das [sqlcmd](../../../tools/sqlcmd-utility.md)-Hilfsprogramm ist mit [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] unter Linux und macOS verfügbar.
  
Die folgenden Befehle zeigen die Verwendung der Windows-Authentifizierung (Kerberos) bzw. der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Authentifizierung:
  
```console
sqlcmd -E -Sxxx.xxx.xxx.xxx  
sqlcmd -Sxxx.xxx.xxx.xxx -Uxxx -Pxxx  
```  
  
## <a name="available-options"></a>Verfügbare Optionen

Im aktuellen Release sind die folgenden Optionen verfügbar:  
  
- -? Zeigt die Verwendung von `sqlcmd` an.  
  
- -a Fordert eine Paketgröße an.  
  
- -b Beendet den Batchauftrag, wenn ein Fehler auftritt.  
  
- -c *Batchabschlusszeichen* Gibt das Batchabschlusszeichen an.  
  
- -C Vertrauenswürdiges Serverzertifikat.  

- -d *Datenbankname* Gibt eine `USE` *Datenbankname*-Anweisung aus, wenn `sqlcmd` gestartet wird.  

- -D Bewirkt, dass der Wert, der an die Option -S von `sqlcmd` übergeben wird, als Datenquellenname (Data Source Name, DSN) interpretiert wird. Weitere Informationen finden Sie unter „DSN-Unterstützung in `sqlcmd` und `bcp`“ am Ende dieses Themas.  
  
- -e Schreibt Eingabeskripts in das Standardausgabegerät (stdout).

- -E Verwendet eine vertrauenswürdige Verbindung (integrierte Authentifizierung). Weitere Informationen zum Herstellen von vertrauenswürdigen Verbindungen, die die integrierte Authentifizierung von einem Linux- oder macOS-Client verwenden, finden Sie unter [Verwenden der integrierten Authentifizierung](using-integrated-authentication.md).

- -f codepage | i:codepage[,o:codepage] | o:codepage[,i:codepage] Gibt die Eingabe- und Ausgabecodepages an. Die Codepagenummer ist ein numerischer Wert, der eine installierte Linux-Codepage angibt.
(verfügbar seit Version 17.5.1.1)

- -G: Dieser Schalter wird vom Client beim Herstellen einer Verbindung mit SQL-Datenbank oder Azure Synapse Analytics verwendet, um anzugeben, dass der Benutzer mithilfe der Azure Active Directory-Authentifizierung authentifiziert werden soll. Durch diese Option wird die sqlcmd -Skriptvariable „SQLCMDUSEAAD = true“ festgelegt. Der Schalter -G erfordert mindestens die sqlcmd-Version 17.6. Führen Sie „sqlcmd -?“ aus, um die von Ihnen verwendete Version zu ermitteln.

> [!IMPORTANT]
> Die Option `-G` gilt nur für Azure SQL-Datenbank und Azure Synapse Analytics.
> Die interaktive AAD-Authentifizierung wird unter Linux oder macOS derzeit nicht unterstützt. Die integrierte AAD-Authentifizierung erfordert [Microsoft ODBC-Treiber 17 für SQL Server](../download-odbc-driver-for-sql-server.md) ab Version 17.6.1 und eine ordnungsgemäß [konfigurierte Kerberos-Umgebung](using-integrated-authentication.md#configure-kerberos).

- -h *number_of_rows* Gibt Sie die Anzahl der Zeilen an, die zwischen den Spaltenüberschriften ausgegeben werden sollen.  
  
- -H Gibt den Namen einer Arbeitsstation an.  
  
- -i *input_file*[,*input_file*[,…]] Identifiziert die Datei, die einen Batch mit SQL-Anweisungen oder gespeicherten Prozeduren enthält.  
  
- -I  Legt die Verbindungsoption `SET QUOTED_IDENTIFIER` auf EIN fest.  
  
- -k Entfernt oder ersetzt Steuerzeichen.  
  
- **-K**_anwendungszweck_  
Deklariert den Arbeitsauslastungstyp der Anwendung beim Herstellen einer Verbindung mit einem Server. Der einzige derzeit unterstützte Wert ist **ReadOnly**. Wenn **-K** nicht angegeben wird, unterstützt `sqlcmd` keine Konnektivität mit einem sekundären Replikat in einer AlwaysOn-Verfügbarkeitsgruppe. Weitere Informationen finden Sie unter [Der ODBC-Treiber unter Linux und macOS für Hochverfügbarkeit und Notfallwiederherstellung](odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md).  
  
> [!NOTE]  
> **-K** wird in der CTP-Version für SUSE Linux nicht unterstützt. Sie können jedoch das Schlüsselwort **ApplicationIntent=ReadOnly** in einer DSN-Datei angeben, die an `sqlcmd` übergeben wird. Weitere Informationen finden Sie unter „DSN-Unterstützung in `sqlcmd` und `bcp`“ am Ende dieses Themas.  
  
- -l *timeout* gibt an, wie viele Sekunden beim Herstellen einer Verbindung mit einem Server verstreichen dürfen, bevor ein Timeout für eine `sqlcmd`-Anmeldung eintritt.

- -m *error_level* Steuert, welche Fehlernachrichten an stdout gesendet werden.  
  
- **-M**_multisubnetz\_failover_  
Geben Sie immer **-M** an, wenn Sie eine Verbindung mit dem Verfügbarkeitsgruppenlistener einer [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]-Verfügbarkeitsgruppe oder einer [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]-Failoverclusterinstanz herstellen. **-M** gewährleistet eine schnellere Erkennung von Failovers und Verbindung mit dem (gerade) aktiven Server. Wenn **-M** nicht angegeben ist, ist **-M** deaktiviert. Weitere Informationen finden Sie zu [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)] unter [ODBC-Treiber unter Linux und macOS für Hochverfügbarkeit und Notfallwiederherstellung](odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md).  
  
> [!NOTE]  
> **-M** wird in der CTP-Version für SUSE Linux nicht unterstützt. Sie können jedoch das Schlüsselwort **MultiSubnetFailover=Yes** in einer DSN-Datei angeben, die an `sqlcmd` übergeben wird. Weitere Informationen finden Sie unter „DSN-Unterstützung in `sqlcmd` und `bcp`“ am Ende dieses Themas.  
  
- -N Verschlüsselt die Verbindung.  
  
- -o *output_file* Identifiziert die Datei, in welche die Ausgabe von `sqlcmd` geschrieben wird.  
  
- -p Gibt Leistungsstatistiken für jedes Resultset aus.  
  
- -P Gibt ein Benutzerkennwort an.  
  
- -q *Befehlszeilenabfrage* Führt eine Abfrage aus, wenn `sqlcmd` gestartet wird, wird jedoch nicht beendet, wenn die Ausführung der Abfrage beendet wird.  

- -Q *Befehlszeilenabfrage* Führt eine Abfrage aus, wenn `sqlcmd` gestartet wird. `sqlcmd` wird beendet, wenn die Abfrage abgeschlossen ist.  

- -r Leitet die Fehlermeldungen an stderr um.

- -R Bewirkt, dass der Treiber die regionalen Einstellungen des Clients verwendet, um Währungs-, Datums- und Uhrzeitdaten in Zeichendaten zu konvertieren. Derzeit werden nur en_US-Formatierungen (US-Englisch) verwendet.
  
- -s *Spaltentrennzeichen* Gibt das Trennzeichen zwischen Spalten an.  

- -S [*Protokoll*:] *Server*[ **,** _Port_]  
Gibt die Instanz von [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] an, mit der eine Verbindung hergestellt werden soll, bzw. bei Verwendung von „-D“ ein DSN. Der ODBC-Treiber unter Linux und macOS erfordert -S. Beachten Sie, dass **tcp** das einzige gültige Protokoll ist.  
  
- -t *query_timeout* Gibt an, wie viele Sekunden verstreichen, bevor für einen Befehl (oder eine SQL-Anweisung) ein Timeout eintritt.  
  
- -u Gibt an, dass output_file unabhängig vom Format von input_file im Unicode-Format gespeichert wird.  
  
- -U *Anmelde-ID* Gibt die Anmelde-ID eines Benutzers an.  
  
- -V *error_severity_level* Steuert den Schweregrad, der zur Festlegung der Variable ERRORLEVEL verwendet wird.  
  
- -w *Spaltenbreite* Gibt die Bildschirmbreite für die Ausgabe an.  
  
- -W Entfernt nachfolgende Leerzeichen aus einer Spalte.  
  
- -x Deaktiviert die Variablenersetzung.  
  
- -X Deaktiviert Befehle, Startskripts und Umgebungsvariablen.  
  
- -y *Bildschirmbreite_mit_variablem_Längentyp* Legt die `sqlcmd`-Skriptvariable `SQLCMDMAXFIXEDTYPEWIDTH` fest.
  
- -Y *Bildschirmbreite_mit_festem_Längentyp* Legt die `sqlcmd`-Skriptvariable `SQLCMDMAXVARTYPEWIDTH` fest.

- -z *Kennwort* Ändert das Kennwort.  
  
- -Z *Kennwort* Ändert das Kennwort und beendet.  


## <a name="available-commands"></a>Verfügbare Befehle

Im aktuellen Release sind die folgenden Befehle verfügbar:  
  
-   [:]!!  
  
-   :Connect  
  
-   :Error  
  
-   [:]EXIT  
  
-   GO [*count*]  
  
-   :Help  
  
-   :List  
  
-   :Listvar  
  
-   :On Error  
  
-   :Out  
  
-   :Perftrace  
  
-   [:]QUIT  
  
-   :r  
  
-   :RESET  
  
-   :setvar  
  
## <a name="unavailable-options"></a>Nicht verfügbare Optionen
Im aktuellen Release sind die folgenden Optionen nicht verfügbar:  

- -A Bewirkt die Anmeldung bei [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] über eine dedizierte Administratorverbindung (Dedicated Administrator Connection, DAC). Weitere Informationen zur Herstellung einer dedizierten Administratorverbindung (DAC) finden Sie unter [Programmierrichtlinien](programming-guidelines.md).  
  
- -L Listet die lokal konfigurierten Servercomputer sowie die Namen der Servercomputer auf, die Broadcastnachrichten über das Netzwerk senden.  
  
- -v Erstellt eine `sqlcmd`-Skriptvariable, die in einem `sqlcmd`-Skript verwendet werden kann.  
  
Sie können die folgende alternative Methode verwenden: Fügen Sie die Parameter in eine einzige Datei ein, die Sie dann an eine andere Daten anhängen können. Dies hilft Ihnen dabei, eine Parameterdatei zu verwenden, um die Werte zu ersetzen. Erstellen Sie beispielsweise eine Datei namens `a.sql` (die Parameterdatei) mit dem folgenden Inhalt:
  
```console
    :setvar ColumnName object_id  
    :setvar TableName sys.objects  
```
  
Erstellen Sie anschließend eine Datei namens `b.sql` mit den Parametern für den Ersatz:  
  
```sql
    select $(ColumnName) from $(TableName)  
```

Kombinieren Sie in der Befehlszeile `a.sql` und `b.sql` mithilfe der folgenden Befehle zu `c.sql`:  
  
```console
    cat a.sql > c.sql 
  
    cat b.sql >> c.sql  
```
  
Führen Sie `sqlcmd` aus, und verwenden Sie `c.sql` als Eingabedatei:  
  
```console
    sqlcmd -S<...> -P<..> -U<..> -I c.sql  
```

## <a name="unavailable-commands"></a>Nicht verfügbare Computer

Im aktuellen Release sind die folgenden Befehle nicht verfügbar:  
  
-   :ED  
  
-   :ServerList  
  
-   :XML  
  
## <a name="dsn-support-in-sqlcmd-and-bcp"></a>DSN-Unterstützung in sqlcmd und bcp

Sie können bei der Option `-S` von **sqlcmd** oder **bcp** (oder im **sqlcmd** :Connect-Befehl) statt eines Servernamens einen Datenquellennamen (Data Source Name, DSN) angeben, wenn Sie `-D` angeben. `-D` sorgt dafür, dass **sqlcmd** oder **bcp** eine Verbindung mit dem Server herstellen kann, der in DSN durch die Option `-S` angegeben ist.  
  
System-DSNs werden in der Datei `odbc.ini` im ODBC-SysConfigDir-Verzeichnis (`/etc/odbc.ini` in Standardinstallationen) gespeichert. Benutzer-DSNs werden in `.odbc.ini` im Basisverzeichnis eines Benutzers (`~/.odbc.ini`) gespeichert.

Auf Windows-Systemen sind System- und Benutzer-DSNs in der Registrierung gespeichert und werden mithilfe von odbcad32 verwaltet. Datei-DSNs werden von bcp und sqlcmd nicht unterstützt.

Eine Liste der Einträge, die vom Treiber unterstützt werden, finden Sie unter [Schlüsselwörter und Attribute von DNS- und Verbindungszeichenfolgen](../dsn-connection-string-attribute.md).

In einem DSN ist nur der Eintrag DRIVER erforderlich. Für die Verbindung mit einem Remoteserver benötigt `sqlcmd` oder `bcp` allerdings einen Wert im SERVER-Element. Wenn das SERVER-Element leer oder nicht im DSN vorhanden ist, versuchen `sqlcmd` und `bcp`, eine Verbindung mit der Standardinstanz auf dem lokalen System herzustellen.

Wenn bcp auf Windows-Systemen verwendet wird, benötigen [!INCLUDE [sssql17-md](../../../includes/sssql17-md.md)] und frühere Versionen den SQL Native Client 11-Treiber (sqlncli11.dll), während für [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] und höher der Microsoft ODBC Driver 17 für SQL Server-Treiber (msodbcsql17.dll) erforderlich ist.  

Wenn diese Option sowohl im DSN als auch in der `sqlcmd`- oder `bcp`-Befehlszeile angegeben ist, überschreibt die Befehlszeilenoption den im DSN verwendeten Wert. Wenn der DSN z.B. einen DATABASE-Eintrag enthält und `sqlcmd`-Befehlszeile **-d** enthält, wird der an **-d** übergebene Wert verwendet. Wenn **Trusted_Connection=yes** im DSN angegeben ist, werden die Kerberos-Authentifizierung sowie der Benutzername ( **–U**) verwendet und das Kennwort ( **–P**), falls vorhanden, ignoriert.

Vorhandene Skripts, die `isql` aufrufen, können geändert werden, um `sqlcmd` zu verwenden, indem der folgende Alias definiert wird: `alias isql="sqlcmd -D"`.  

## <a name="see-also"></a>Weitere Informationen  
[Herstellen einer Verbindung mit **bcp**](connecting-with-bcp.md)  
[Versionsanmerkungen](release-notes-tools.md)
