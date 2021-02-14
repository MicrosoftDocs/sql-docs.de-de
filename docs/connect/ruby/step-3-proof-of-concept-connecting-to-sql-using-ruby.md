---
title: 'Schritt 3: Machbarkeitsnachweis für Verbindungen mit SQL mithilfe von Ruby'
description: Schritt 3 ist ein Proof of Concept, der zeigt, wie Sie mit Ruby eine Verbindung mit SQL Server herstellen können. Die grundlegenden Beispiele veranschaulichen das Auswählen und Einfügen von Daten.
ms.custom: ''
ms.date: 06/22/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: cac20b18-0a6d-4243-bbda-a5d1b9476441
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 93c40d3c64f2a32cd9f318dc236c4c350cfddc28
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058115"
---
# <a name="step-3-proof-of-concept-connecting-to-sql-using-ruby"></a>Schritt 3: Machbarkeitsnachweis für Verbindungen mit SQL mithilfe von Ruby

Dieses Beispiel ist lediglich als Proof of Concept zu verstehen.  Es wurde zur Verdeutlichung vereinfacht und entspricht nicht zwangsläufig den von Microsoft empfohlenen Best Practices.  
  
## <a name="step-1--connect"></a>Schritt 1:  Verbinden  
  
Die [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) -Funktion dient zum Verbinden mit einer SQL-Datenbank.  
  
```ruby
    require 'tiny_tds'  
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',  
    host: 'yourserver.database.windows.net', port: 1433,  
    database: 'AdventureWorks', azure:true  
```  
  
## <a name="step-2--execute-a-query"></a>Schritt 2:  Ausführen einer Abfrage  
  
Kopieren Sie den folgenden Code, und fügen Sie ihn in eine leere Datei ein. Nennen Sie sie "test.rb". Führen Sie sie anschließend durch Eingeben des folgenden Befehls über die Befehlszeile aus:  
  
```ruby
    ruby test.rb  
```
  
Im Codebeispiel wird die Funktion [TinyTds::Result](https://github.com/rails-sqlserver/tiny_tds) verwendet, um ein Resultset aus einer Abfrage in einer SQL-Datenbank abzurufen. Diese Funktion akzeptiert eine Abfrage und gibt ein Resultset zurück. Das Resultset läuft mithilfe von [result.each do |row|](https://github.com/rails-sqlserver/tiny_tds)durch.  
  
```ruby 
    require 'tiny_tds'    
    print 'test'       
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',  
    host: 'yourserver.database.windows.net', port: 1433,  
    database: 'AdventureWorks', azure:true  
    results = client.execute("SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC")  
    results.each do |row|  
    puts row  
    end  
```  
  
## <a name="step-3--insert-a-row"></a>Schritt 3:  Einfügen einer Zeile  
  
In diesem Beispiel erfahren Sie, wie Sie eine [INSERT](../../t-sql/statements/insert-transact-sql.md)-Anweisung sicher ausführen und Parameter zum Schutz Ihrer Anwendung vor einer [Einschleusung von SQL-Befehlen](../../relational-databases/tables/primary-and-foreign-key-constraints.md) übergeben.    
  
Bei der Verwendung von TinyTDS mit Azure wird empfohlen, dass Sie mehrere `SET` -Anweisungen ausführen, um zu ändern, wie die aktuelle Sitzung bestimmte Informationen behandelt. Empfohlene `SET` -Anweisungen werden im Codebeispiel bereitgestellt. Beispielsweise ermöglicht `SET ANSI_NULL_DFLT_ON` das Erstellen neuer Spalten zum Zulassen von NULL-Werten, selbst wenn der NULL-Zulässigkeitsstatus der Spalte nicht explizit angegeben ist.  
  
Zum Abstimmen mit dem [datetime](../../t-sql/data-types/datetime-transact-sql.md)-Format in Microsoft SQL Server verwenden Sie die [strftime](https://ruby-doc.org/core-2.2.0/Time.html#method-i-strftime)-Funktion, um eine Umwandlung in das entsprechende datetime-Format durchzuführen.  
  
```ruby
    require 'tiny_tds'  
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',  
    host: 'yourserver.database.windows.net', port: 1433,  
    database: 'AdventureWorks', azure:true  
    results = client.execute("SET ANSI_NULLS ON")  
    results = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")  
    results = client.execute("SET ANSI_NULL_DFLT_ON ON")  
    results = client.execute("SET IMPLICIT_TRANSACTIONS OFF")  
    results = client.execute("SET ANSI_PADDING ON")  
    results = client.execute("SET QUOTED_IDENTIFIER ON")  
    results = client.execute("SET ANSI_WARNINGS ON")  
    results = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")  
    require 'date'  
    t = Time.now  
    curr_date = t.strftime("%Y-%m-%d %H:%M:%S.%L")  
    results = client.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate)  
    OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, '#{curr_date}' )")  
    results.each do |row|  
    puts row  
    end  
```
