---
title: Verbindungsbibliotheken für Microsoft SQL Server-Datenbanken | Microsoft-Dokumentation
description: Dieser Artikel bietet Links zum Download von Modulen, die eine Verbindung mit Microsoft SQL Server und Azure SQL-Datenbank mit verschiedenen Clientprogrammiersprachen ermöglichen.
author: David-Engel
ms.prod: sql
ms.technology: connectivity
ms.custom: ''
ms.topic: article
ms.date: 03/06/2020
ms.author: v-daenge
ms.openlocfilehash: b64d5d6d20e7d83385b040a49516cb9728b5f048
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058145"
---
# <a name="connection-modules-for-microsoft-sql-database"></a>Verbindungsmodule für Microsoft SQL-Datenbank

Dieser Artikel bietet Links zum Download von Verbindungsmodulen oder *Treibern*, die Ihre Clientprogramme für die Interaktion mit der Plattform [Microsoft SQL Server](../relational-databases/databases/databases.md) und ihrem Pendant in der Cloud, [Azure SQL-Datenbank](/azure/sql-database/), verwenden können. Treiber sind für verschiedene Programmiersprachen verfügbar und können unter folgenden Betriebssystemen ausgeführt werden:

- Linux
- macOS
- Windows

**Konflikt zwischen objektorientierter Programmierung und relationalem Datenbankformat:**

*Relationales Format*: Clientprogramme, die in einer objektorientierten Programmiersprache (Object-Oriented Programming, OOP) geschrieben sind, verwenden häufig SQL-Treiber, die die abgefragten Daten in einem Format zurückgeben, das eher relational als objektorientiert ist. C# mit ADO.NET ist ein Beispiel hierfür. Der Konflikt zwischen OOP und relationalem Format führt zuweilen dazu, dass OOP-Code schwieriger zu schreiben und zu verstehen ist.

*ORM*: Andere Treiber oder Frameworks geben die abgefragten Daten im OOP-Format zurück, um diesen Konflikt zu vermeiden. Diese Treiber erwarten, dass Klassen so definiert wurden, dass sie den Datenspalten bestimmter SQL-Tabellen entsprechen. Diese Treiber führen dann eine *objektrelationale Zuordnung* (Object-Relational Mapping, ORM) aus, um die abgefragten Daten als Instanz einer Klasse zurückzugeben. Das Microsoft Entity Framework (EF) für C# und Hibernate für Java sind zwei Beispiele hierfür.

Im vorliegenden Artikel sind diesen beiden Arten von Verbindungstreibern zwei separate Abschnitte gewidmet.

<a name="anchor-20-drivers-relational-access" />

## <a name="drivers-for-relational-access"></a>Treiber für den relationalen Zugriff

| Sprache | Download des SQL-Treibers |
| :------- | :---------------------- |
| C# | [ADO.NET](https://www.microsoft.com/net/download/)<br />[Microsoft.Data.SqlClient](https://www.nuget.org/packages/Microsoft.Data.SqlClient/)<br /><br />[.NET Core für: Linux-Ubuntu, macOS, Windows](https://dotnet.microsoft.com/download) |
| C++ | [ODBC](./odbc/download-odbc-driver-for-sql-server.md)<br /><br />[OLE DB](./oledb/download-oledb-driver-for-sql-server.md) |
| Java | [JDBC](./jdbc/download-microsoft-jdbc-driver-for-sql-server.md) |
| Node.js | [Node.js-Treiber, Installationsanweisungen](./node-js/step-1-configure-development-environment-for-node-js-development.md) |
| PHP | [PHP](./php/download-drivers-php-sql-server.md) |
| Python | [pyodbc, Installationsanweisungen](./python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development.md)<br />[Herunterladen des ODBC-Treibers](./odbc/download-odbc-driver-for-sql-server.md) |
| Ruby | [Ruby-Treiber, Installationsanweisungen](./ruby/step-1-configure-development-environment-for-ruby-development.md)<br />[Ruby-Downloadseite](https://rubyinstaller.org/downloads/) |
| &nbsp; | &nbsp; |

<a name="anchor-40-drivers-orm-access" />

## <a name="drivers-for-orm-access"></a>Treiber für den ORM-Zugriff

In der folgenden Tabelle sind Beispiele für ORM-Frameworks aufgelistet, die Clientanwendungen zum Herstellen einer Verbindung mit Microsoft SQL Server-Datenbanken verwenden können.

| Sprache | Download des ORM-Treibers |
| :------- | :------------------ |
| C# | [Entity Framework Core](/ef/core/)<br />[Entity Framework (6.x oder höher)](/ef/) |
| Java | [Hibernate ORM](https://hibernate.org/orm)|
| PHP | [Eloquent ORM, enthalten in der Laravel-Installation](https://laravel.com/docs/) |
| Node.js | [Sequelize ORM](https://sequelize.org/) |
| Python | [Django](https://www.djangoproject.com/) |
| Ruby | [Ruby on Rails](https://rubyonrails.org/) |
| &nbsp; | &nbsp; |

<a name="anchor-60-build-an-app-webpages" />

## <a name="build-an-app-webpages"></a>Webseiten zum Erstellen einer App

Über **[https://aka.ms/sqldev](https://aka.ms/sqldev)** gelangen Sie zu einer Reihe von Webseiten zum *Erstellen einer App*. Diese Webseiten enthalten Informationen zu zahlreichen Kombinationen aus Programmiersprache, Betriebssystem und SQL-Verbindungstreiber. Webseiten zum Erstellen einer App bieten u. a. die folgenden Informationen:

- Ausführliche Informationen zum Einstieg für jede Kombination aus Sprache, Betriebssystem und Treiber.
  - Anweisungen zum Installieren der neuesten SQL-Verbindungstreiber.
- Codebeispiele für jedes der folgenden Elemente:
  - Objektrelationale Codebeispiele.
  - ORM-Codebeispiele
  - Columnstore-Index-Demonstrationen zum Erzielen einer wesentlich schnelleren Leistung.

**Erster Bildschirm einer Webseite zum Erstellen einer App:**  
![Screenshot des ersten Bildschirms einer Webseite zum Erstellen einer App](media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-choose-language-g21.png)

**Menü für Java > Ubuntu auf einer Webseite zum Erstellen einer App**  
![Webseite zum Erstellen einer App, Menü für Java > Ubuntu](media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-java-ubuntu-c31.png)

&nbsp;

## <a name="related-links"></a>Verwandte Links

- [Codebeispiele zum Herstellen einer Verbindung mit Azure SQL-Datenbank in der Cloud mit Java und anderen Sprachen](/azure/sql-database/sql-database-connect-query-java)

<!--
Image references, **obsolete** markdown syntax alternative:

![Build-an-app webpages, first page screenshot][image-ref-163-buildanapp-webpages-first-page]
![Build-an-app webpages, menu Java Ubuntu][image-ref-167-buildanapp-webpages-menu-java-ubuntu]

[image-ref-163-buildanapp-webpages-first-page]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-choose-language-g21.png
[image-ref-167-buildanapp-webpages-menu-java-ubuntu]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-java-ubuntu-c31.png
-->