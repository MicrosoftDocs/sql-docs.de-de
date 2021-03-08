---
title: Verwenden des JDBC-Treibers | Microsoft-Dokumentation
description: Dieser Abschnitt bietet eine Schnellstartanleitung, um unter Verwendung des Microsoft JDBC-Treibers für SQL Server eine einfache Verbindung mit einer SQL Server-Datenbank herzustellen.
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 6faaf05b-8b70-4ed2-9b44-eee5897f1cd0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e74135523d2ba4cbde27e588eee999b3450def37
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837517"
---
# <a name="using-the-jdbc-driver"></a>Verwenden des JDBC-Treibers

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Dieser Abschnitt enthält Anleitungen, um in kurzer Zeit mit [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] eine einfache Verbindung mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank herzustellen. Vor dem Herstellen einer Verbindung mit einer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datenbank müssen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auf dem lokalen Computer oder einem Server sowie der JDBC-Treiber auf dem lokalen Computer installiert werden.  
  
## <a name="choosing-the-right-jar-file"></a>Auswählen der richtigen JAR-Datei

Der Microsoft JDBC-Treiber bietet verschiedene JAR-Dateien, die in Verbindung mit Ihren bevorzugten Einstellungen für die Java Runtime Environment (JRE) verwendet werden können:

Der Microsoft JDBC-Treiber 9.2 für SQL Server stellt die Klassenbibliotheksdateien **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** und **mssql-jdbc-9.2.1.jre15.jar** bereit.

Der Microsoft JDBC-Treiber 8.4 für SQL Server stellt die Klassenbibliotheksdateien **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** und **mssql-jdbc-8.4.1.jre14.jar** bereit.

Der Microsoft JDBC-Treiber 8.2 für SQL Server bietet die Klassenbibliotheksdateien **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** und **mssql-jdbc-8.2.2.jre13.jar**.

Der Microsoft JDBC-Treiber 7.4 für SQL Server bietet die Klassenbibliotheksdateien **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** und **mssql-jdbc-7.4.1.jre12.jar**.

Der Microsoft JDBC-Treiber 7.2 für SQL Server bietet die Klassenbibliotheksdateien **mssql-jdbc-7.2.2.jre8.jar** und **mssql-jdbc-7.2.2.jre11.jar**.

Der Microsoft JDBC-Treiber 7.0 für SQL Server bietet die Klassenbibliotheksdateien **mssql-jdbc-7.0.0.jre8.jar** und **mssql-jdbc-7.0.0.jre10.jar**.

Der Microsoft JDBC-Treiber 6.4 für SQL Server bietet die Klassenbibliotheksdateien **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** und **mssql-jdbc-6.4.0.jre9.jar**.

Der Microsoft JDBC-Treiber 6.2 für SQL Server bietet die Klassenbibliotheksdateien **mssql-jdbc-6.2.2.jre7.jar** und **mssql-jdbc-6.2.2.jre8.jar**.
  
Die Microsoft JDBC-Treiber 6.0 und 4.2 für SQL Server bieten die Klassenbibliotheksdateien **sqljdbc41.jar** und **sqljdbc42.jar**.
  
Der Microsoft JDBC-Treiber 4.1 für SQL Server bietet die Klassenbibliotheksdatei **sqljdbc41.jar**.

Ihre Auswahl entscheidet auch über die verfügbaren Funktionen. Weitere Informationen zum Auswählen der richtigen JAR-Datei finden Sie unter [Systemanforderungen für den JDBC-Treiber](../../connect/jdbc/system-requirements-for-the-jdbc-driver.md).  
  
## <a name="setting-the-classpath"></a>Festlegen des Klassenpfads

Die JAR-Dateien des Microsoft JDBC-Treibers sind nicht Bestandteil des Java SDK und müssen im Klassenpfad der Benutzeranwendung enthalten sein.

Wenn Sie JDBC-Treiber 4.1 oder 4.2 verwenden, legen Sie fest, dass der Klassenpfad die Datei **sqljdbc41.jar** oder **sqljdbc42.jar** aus dem entsprechenden Treiberdownload enthält.

Wenn Sie JDBC-Treiber 6.2 verwenden, legen Sie fest, dass der Klassenpfad die Datei **mssql-jdbc-6.2.2.jre7.jar** oder **mssql-jdbc-6.2.2.jre8.jar** enthält.

Wenn Sie den JDBC-Treiber 6.4 verwenden, legen Sie fest, dass der Klassenpfad die Datei **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** oder **mssql-jdbc-6.4.0.jre9.jar** enthält.

Wenn Sie JDBC-Treiber 7.0 verwenden, legen Sie fest, dass der Klassenpfad die Datei **mssql-jdbc-7.0.0.jre8.jar** oder **mssql-jdbc-7.0.0.jre10.jar** enthält.

Wenn Sie JDBC-Treiber 7.2 verwenden, legen Sie fest, dass der Klassenpfad die Datei **mssql-jdbc-7.2.2.jre8.jar** oder **mssql-jdbc-7.2.2.jre11.jar** enthält.

Wenn Sie den JDBC-Treiber 7.4 verwenden, legen Sie fest, dass der Klassenpfad die Datei **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** oder **mssql-jdbc-7.4.1.jre12.jar** enthält.

Wenn Sie den JDBC-Treiber 8.2 verwenden, legen Sie fest, dass der Klassenpfad die Datei **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** oder **mssql-jdbc-8.2.2.jre13.jar** enthält.

Wenn Sie den JDBC-Treiber 8.4 verwenden, legen Sie fest, dass der Klassenpfad die Datei **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** oder **mssql-jdbc-8.4.1.jre14.jar** enthält.

Wenn Sie den JDBC-Treiber 9.2 verwenden, nehmen Sie die Datei **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** oder **mssql-jdbc-9.2.1.jre15.jar** in den Klassenpfad auf.

Wenn im Klassenpfad kein Eintrag für die richtige JAR-Datei vorhanden ist, löst eine Anwendung die allgemeine Ausnahme `Class not found` aus.  

### <a name="for-microsoft-jdbc-driver-92"></a>Für den Microsoft JDBC-Treiber 9.2

Die Dateien **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** oder **mssql-jdbc-9.2.1.jre15.jar** werden an den folgenden Speicherorten installiert:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-9.2.1.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-9.2.1.jre11.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-9.2.1.jre15.jar
```

Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Windows-Anwendung verwendet wird:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 9.2 for SQL Server\sqljdbc_9.2\enu\mssql-jdbc-9.2.1.jre11.jar`

Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Linux/Unix-Anwendung verwendet wird:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_9.2/enu/mssql-jdbc-9.2.1.jre11.jar`

Stellen Sie sicher, dass die CLASSPATH-Anweisung nur eine [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]-Datei enthält, also entweder **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** oder **mssql-jdbc-9.2.1.jre15.jar**.


### <a name="for-microsoft-jdbc-driver-84"></a>Für Microsoft JDBC-Treiber 8.4

Die Dateien **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** oder **mssql-jdbc-8.4.1.jre14.jar** werden in den folgenden Verzeichnissen installiert:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.4.1.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.4.1.jre11.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.4.1.jre14.jar
```

Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Windows-Anwendung verwendet wird:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 8.4 for SQL Server\sqljdbc_8.4\enu\mssql-jdbc-8.4.1.jre11.jar`

Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Linux/Unix-Anwendung verwendet wird:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_8.4/enu/mssql-jdbc-8.4.1.jre11.jar`

Stellen Sie sicher, dass die CLASSPATH-Anweisung nur eine [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]-Datei enthält, also entweder **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** oder **mssql-jdbc-8.4.1.jre14.jar**.


### <a name="for-microsoft-jdbc-driver-82"></a>Für Microsoft JDBC-Treiber 8.2

Die Dateien **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** und **mssql-jdbc-8.2.2.jre13.jar** werden unter den folgenden Verzeichnissen installiert:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.2.2.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.2.2.jre11.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.2.2.jre13.jar
```

Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Windows-Anwendung verwendet wird:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 8.2 for SQL Server\sqljdbc_8.2\enu\mssql-jdbc-8.2.2.jre11.jar`

Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Linux/Unix-Anwendung verwendet wird:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_8.2/enu/mssql-jdbc-8.2.2.jre11.jar`

Stellen Sie sicher, dass die CLASSPATH-Anweisung nur eine [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]-Datei enthält, z. B. entweder **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** oder **mssql-jdbc-8.2.2.jre13.jar**.

### <a name="for-microsoft-jdbc-driver-74"></a>Für Microsoft JDBC-Treiber 7.4

Die Datei **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** oder **mssql-jdbc-7.4.1.jre12.jar** wird im folgenden Verzeichnis installiert:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.4.1.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.4.1.jre11.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.4.1.jre12.jar
```

Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Windows-Anwendung verwendet wird:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 7.4 for SQL Server\sqljdbc_7.4\enu\mssql-jdbc-7.4.1.jre11.jar`

Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Linux/Unix-Anwendung verwendet wird:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_7.4/enu/mssql-jdbc-7.4.1.jre11.jar`

Stellen Sie sicher, dass die CLASSPATH-Anweisung nur eine [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]-Datei enthält, z.B. entweder **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** oder **mssql-jdbc-7.4.1.jre12.jar**.

### <a name="for-microsoft-jdbc-driver-72"></a>Für Microsoft JDBC-Treiber 7.2

Die Dateien **mssql-jdbc-7.2.2.jre8.jar** und **mssql-jdbc-7.2.2.jre11.jar** werden in den folgenden Verzeichnissen installiert:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.2.2.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.2.2.jre11.jar
```

Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Windows-Anwendung verwendet wird:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 7.2 for SQL Server\sqljdbc_7.2\enu\mssql-jdbc-7.2.2.jre11.jar`

Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Linux/Unix-Anwendung verwendet wird:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_7.2/enu/mssql-jdbc-7.2.2.jre11.jar`

Stellen Sie sicher, dass die CLASSPATH-Anweisung nur eine [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]-Datei enthält, z. B. entweder **mssql-jdbc-7.2.2.jre8.jar** oder **mssql-jdbc-7.2.2.jre11.jar**.
  
### <a name="for-microsoft-jdbc-driver-70"></a>Für Microsoft JDBC-Treiber 7.0

Die Dateien **mssql-jdbc-7.0.0.jre8.jar** und **mssql-jdbc-7.0.0.jre10.jar** werden in den folgenden Verzeichnissen installiert:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.0.0.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.0.0.jre10.jar
```

Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Windows-Anwendung verwendet wird:  

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 7.0 for SQL Server\sqljdbc_7.0\enu\mssql-jdbc-7.0.0.jre10.jar`  
  
Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Linux/Unix-Anwendung verwendet wird:  
  
`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_7.0/enu/mssql-jdbc-7.0.0.jre10.jar`  
  
Stellen Sie sicher, dass die CLASSPATH-Anweisung nur eine [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]-Datei enthält, z. B. entweder **mssql-jdbc-7.0.0.jre8.jar** oder **mssql-jdbc-7.0.0.jre10.jar**.

### <a name="for-microsoft-jdbc-driver-64"></a>Für Microsoft JDBC-Treiber 6.4

Die Datei **mssql-jdbc-6.4.0.jre7.jar**, „**mssql-jdbc-6.4.0.jre8.jar“ oder **mssql-jdbc-6.4.0.jre9.jar** wird im folgenden Verzeichnis installiert:  

```bash  
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.4.0.jre7.jar
  
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.4.0.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.4.0.jre9.jar
```

Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Windows-Anwendung verwendet wird:  
  
`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 6.4 for SQL Server\sqljdbc_6.4\enu\mssql-jdbc-6.4.0.jre9.jar`  
  
Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Linux/Unix-Anwendung verwendet wird:  
  
`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_6.4/enu/mssql-jdbc-6.4.0.jre9.jar`  
  
Stellen Sie sicher, dass die CLASSPATH-Anweisung nur eine [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]-Datei enthält, z. B. entweder **mssql-jdbc-6.4.0.jre7.jar**, „**mssql-jdbc-6.4.0.jre8.jar“ oder **mssql-jdbc-6.4.0.jre9.jar**.

### <a name="for-microsoft-jdbc-driver-62"></a>Für Microsoft JDBC-Treiber 6.2

Die Dateien **mssql-jdbc-6.2.2.jre7.jar** und **mssql-jdbc-6.2.2.jre8.jar** werden in den folgenden Verzeichnissen installiert:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.2.2.jre7.jar
  
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.2.2.jre8.jar
```

Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Windows-Anwendung verwendet wird:  
  
`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 6.2 for SQL Server\sqljdbc_6.2\enu\mssql-jdbc-6.2.2.jre8.jar`  
  
Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Linux/Unix-Anwendung verwendet wird:  
  
`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_6.2/enu/mssql-jdbc-6.2.2.jre8.jar`  
  
Stellen Sie sicher, dass die CLASSPATH-Anweisung nur eine [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]-Datei enthält, z. B. entweder „mssql-jdbc-6.2.2.jre7.jar“ oder „mssql-jdbc-6.2.2.jre8.jar“.  

### <a name="for-microsoft-jdbc-driver-41-42-and-60"></a>Für Microsoft JDBC-Treiber 4.1, 4.2 und 6.0

Die JAR-Datei sqljdbc.jar file, sqljdbc4.jar file, sqljdbc41.jar oder sqljdbc42.jar wird an folgendem Speicherorten installiert.  

```bash
\<installation directory>\sqljdbc_<version>\<language>\sqljdbc.jar  
  
\<installation directory>\sqljdbc_<version>\<language>\sqljdbc4.jar  
  
\<installation directory>\sqljdbc_<version>\<language>\sqljdbc41.jar  
  
\<installation directory>\sqljdbc_<version>\<language>\sqljdbc42.jar  
```

Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Windows-Anwendung verwendet wird:  
  
`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 6.0 for SQL Server\sqljdbc_4.2\enu\sqljdbc42.jar`  
  
Der folgende Codeausschnitt stellt ein Beispiel für die CLASSPATH-Anweisung dar, die für eine Linux/Unix-Anwendung verwendet wird:  

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_4.2/enu/sqljdbc42.jar`  
  
Stellen Sie sicher, dass die CLASSPATH-Anweisung nur eine [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]-Datei enthält, z.B. „sqljdbc4.jar“, „sqljdbc41.jar“ oder „sqljdbc42.jar“.  
  
> [!NOTE]  
> Auf Windows-Systemen können Verzeichnisnamen, die länger als die 8.3-Namenskonvention sind, oder Ordnernamen mit Leerzeichen zu Problemen bei Klassenpfaden führen. Wenn Sie ein derartiges Problem vermuten, sollten Sie die Datei „sqljdbc.jar“, „sqljdbc4.jar“ oder „sqljdbc41.jar“ vorübergehend in ein Verzeichnis mit einem einfachen Namen wie `C:\Temp` verschieben, den Klassenpfad ändern und prüfen, ob das Problem dadurch behoben wurde.  
  
### <a name="applications-that-are-run-directly-at-the-command-prompt"></a>Anwendungen, die direkt an der Eingabeaufforderung ausgeführt werden

Der Klassenpfad wird im Betriebssystem konfiguriert. Hängen Sie "sqljdbc.jar", "sqljdbc4.jar" oder "sqljdbc41.jar" an den Klassenpfad des Systems an. Alternativ können Sie den Klassenpfad in der Java-Befehlszeile, mit der die Anwendung ausgeführt wird, mit der Option `java -classpath` angeben.  
  
### <a name="applications-that-run-in-an-ide"></a>Anwendungen, die in einer IDE ausgeführt werden  

Jeder IDE-Hersteller verwendet eine andere Methode, um den Klassenpfad in der IDE festzulegen. Es reicht nicht aus, den Klassenpfad lediglich im Betriebssystem festzulegen. Sie müssen "sqljdbc.jar", "sqljdbc4.jar" oder "sqljdbc41.jar" an den IDE-Klassenpfad anhängen.  
  
### <a name="servlets-and-jsps"></a>Servlets und JSPs  

Servlets und JSPs werden in einer Servlet-/JSP-Engine wie Tomcat ausgeführt. Der Klassenpfad muss wie in der Dokumentation für die Servlet-/JSP-Engine angegeben festgelegt werden. Es reicht nicht aus, den Klassenpfad lediglich im Betriebssystem festzulegen. Einige Servlet-/JSP-Engines verfügen über Setupfenster, in denen Sie den Klassenpfad der Engine festlegen können. In dieser Situation müssen Sie die richtige JAR-Datei für den JDBC-Treiber an den vorhandenen Klassenpfad der Engine anhängen und die Engine neu starten. In anderen Situationen können Sie den Treiber bereitstellen, indem Sie "sqljdbc.jar", "sqljdbc4.jar" oder "sqljdbc41.jar" bei der Engine-Installation in ein bestimmtes Verzeichnis kopieren, z. B. "lib". Der Klassenpfad für den Engine-Treiber kann auch in einer Engine-spezifischen Konfigurationsdatei angegeben werden.  
  
### <a name="enterprise-java-beans"></a>Enterprise Java Beans  

Enterprise Java Beans (EJB) werden in einem EJB-Container ausgeführt. EJB-Container sind von verschiedenen Herstellern erhältlich. Java-Applets werden in einem Browser ausgeführt, aber von einem Webserver heruntergeladen. Kopieren Sie „sqljdbc.jar“ „sqljdbc4.jar“ oder „sqljdbc41.jar“ in das Stammverzeichnis des Webservers, und geben Sie den Namen der JAR-Datei auf der HTML-Archiv-Registerkarte des Applets an, z.B. `<applet ... archive=mssql-jdbc-***.jar>`.  
  
## <a name="making-a-simple-connection-to-a-database"></a>Herstellen einer einfachen Verbindung mit einer Datenbank

Wenn Sie die sqljdbc.jar-Klassenbibliothek verwenden, müssen Sie zuerst den Treiber wie folgt registrieren:  
  
`Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");`  

Nachdem der Treiber geladen wurde, können Sie über eine Verbindungs-URL und die getConnection-Methode der DriverManager-Klasse eine Verbindung herstellen:

```java
String connectionUrl = "jdbc:sqlserver://localhost:1433;" +  
   "databaseName=AdventureWorks;user=MyUserName;password=*****;";  
Connection con = DriverManager.getConnection(connectionUrl);  
```

Ab der JDBC-API 4.0 wurde die `DriverManager.getConnection()`-Methode so erweitert, dass JDBC-Treiber automatisch geladen werden. Daher müssen Anwendungen die `Class.forName`-Methode bei Verwendung von JAR-Treiberbibliotheken nicht aufrufen, um den Treiber zu registrieren oder zu laden.  
  
Wenn die getConnection-Methode der DriverManager-Klasse aufgerufen wird, wird aus der Gruppe der registrierten JDBC-Treiber ein geeigneter Treiber ausgewählt. „sqljdbc4.jar“, „sqljdbc41.jar“ und „sqljdbc42.jar“ enthalten die Datei „META-INF/services/java.sql.Driver“, die den **com.microsoft.sqlserver.jdbc.SQLServerDriver** als registrierten Treiber enthält. Vorhandene Anwendungen, die die Treiber derzeit mit der Methode Class.forName laden, funktionieren auch weiterhin ohne Änderungen.  
  
> [!NOTE]  
> Die Klassenbibliothek „sqljdbc4.jar“, „sqljdbc41.jar“ oder „sqljdbc42.jar“ kann nicht mit älteren Versionen von Java Runtime Environment (JRE) verwendet werden. Die Liste der vom [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] unterstützten JRE-Versionen finden Sie unter [Systemanforderungen für den JDBC-Treiber](../../connect/jdbc/system-requirements-for-the-jdbc-driver.md).  

Weitere Informationen zum Herstellen einer Verbindung mit Datenquellen und zum Verwenden einer Verbindungs-URL finden Sie unter [Erstellen der Verbindungs-URL](../../connect/jdbc/building-the-connection-url.md) und [Festlegen der Verbindungseigenschaften](../../connect/jdbc/setting-the-connection-properties.md).  
  
## <a name="see-also"></a>Weitere Informationen  

[Übersicht über den JDBC-Treiber](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
