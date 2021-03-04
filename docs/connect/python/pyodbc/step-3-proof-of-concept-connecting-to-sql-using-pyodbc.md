---
title: 'Schritt 3: Herstellen der Verbindung mit SQL mithilfe von pyodbc'
description: Schritt 3 ist ein Proof of Concept, der zeigt, wie Sie mithilfe von Python and pyODBC eine Verbindung mit SQL Server herstellen können. Die grundlegenden Beispiele veranschaulichen das Auswählen und Einfügen von Daten.
ms.custom: ''
ms.date: 03/02/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 4bfd6e52-817d-4f0a-a33d-11466e3f0484
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 808cb17d29362e76fdcc69171b21c9ffffd5c1ed
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837127"
---
# <a name="step-3-proof-of-concept-connecting-to-sql-using-pyodbc"></a>Schritt 3: Proof of Concept für Verbindungen mit SQL mithilfe von pyodbc

Dieses Beispiel ist ein Proof of Concept. Es wurde zur Verdeutlichung vereinfacht und entspricht nicht zwangsläufig den von Microsoft empfohlenen Best Practices.  

Führen Sie als Erstes das folgende Beispielskript aus. Erstellen Sie eine Datei namens „test.py“, und fügen Sie nach und nach alle Codeausschnitte ein.

```
> python test.py
```
  
## <a name="connect"></a>Verbinden  
  
```python
import pyodbc 
# Some other example server values are
# server = 'localhost\sqlexpress' # for a named instance
# server = 'myserver,port' # to specify an alternate port
server = 'tcp:myserver.database.windows.net' 
database = 'mydb' 
username = 'myusername' 
password = 'mypassword' 
cnxn = pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()

```  

## <a name="run-query"></a>Abfrage ausführen  
  
Mit der Funktion „cursor.execute“ können Sie ein Resultset aus einer Abfrage einer SQL-Datenbank abrufen. Diese Funktion akzeptiert eine Abfrage und gibt ein Resultset zurück, das mithilfe von „cursor.fetchone()“ durchlaufen werden kann.

```python
#Sample select query
cursor.execute("SELECT @@version;") 
row = cursor.fetchone() 
while row: 
    print(row[0])
    row = cursor.fetchone()

```  
  
## <a name="insert-a-row"></a>Einfügen einer Zeile  
  
In diesem Beispiel sehen Sie, wie Sie eine [INSERT](../../../t-sql/statements/insert-transact-sql.md)-Anweisung sicher ausführen und Parameter übergeben. Diese Parameter schützen Ihre Anwendung vor einer [Einschleusung von SQL-Befehlen](../../../relational-databases/tables/primary-and-foreign-key-constraints.md).

```python
#Sample insert query
count = cursor.execute("""
INSERT INTO SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) 
VALUES (?,?,?,?,?)""",
'SQL Server Express New 20', 'SQLEXPRESS New 20', 0, 0, CURRENT_TIMESTAMP).rowcount
cnxn.commit()
print('Rows inserted: ' + str(count))
```  

## <a name="azure-active-directory-and-the-connection-string"></a>Azure Active Directory und die Verbindungszeichenfolge

pyODBC verwendet den Microsoft ODBC Driver for SQL Server.
Wenn Sie die ODBC Driver-Version 17.1 oder höher verwenden, können Sie über pyODBC den interaktiven AAD-Modus des Treibers verwenden.
Diese interaktive Option funktioniert, wenn Python und pyODBC dem ODBC Driver gestatten, das Dialogfeld anzuzeigen. Diese Option ist nur unter Windows-Betriebssystemen verfügbar.

### <a name="example-connection-string-for-azure-active-directory-interactive-authentication"></a>Beispiel für eine Verbindungszeichenfolge für die interaktive Azure Active Directory-Authentifizierung

Das folgende Beispiel stellt eine ODBC-Verbindungszeichenfolge bereit, die die interaktive Azure Active Directory-Authentifizierung angibt:

`server=Server;database=Database;UID=UserName;Authentication=ActiveDirectoryInteractive;`

Weitere Informationen zu den Authentifizierungsoptionen, die der ODBC Driver bietet, finden Sie unter [Verwenden von Azure Active Directory mit dem ODBC Driver](../../odbc/using-azure-active-directory.md#new-andor-modified-dsn-and-connection-string-keywords).

## <a name="next-steps"></a>Nächste Schritte
  
Weitere Informationen finden Sie im [Python Developer Center](https://azure.microsoft.com/develop/python/).
