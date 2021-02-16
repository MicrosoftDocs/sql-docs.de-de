---
description: Erstellen eines ODBC-Ziels mit der Skriptkomponente
title: Erstellen eines ODBC-Ziels mit der Skriptkomponente | Microsoft-Dokumentation
ms.custom: ''
ms.date: 10/10/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Script component [Integration Services], destination components
- ODBC destination [Integration Services]
- destinations [Integration Services], components
- Script component [Integration Services], examples
ms.assetid: d198c866-78f4-4a50-ae15-333160645815
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 446a4ba272d80d13c8884d3a2a0eb143c44980f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344885"
---
# <a name="creating-an-odbc-destination-with-the-script-component"></a>Erstellen eines ODBC-Ziels mit der Skriptkomponente

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  In [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] werden Daten in der Regel mithilfe eines [!INCLUDE[vstecado](../../includes/vstecado-md.md)]-Ziels und des [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]-Datenanbieters für ODBC in einem ODBC-Ziel gespeichert. Sie können jedoch auch ein Ad-hoc-ODBC-Ziel für die Verwendung in einem einzelnen Paket erstellen. Zur Erstellung dieses Ad-hoc-ODBC-Ziels verwenden Sie die Skriptkomponente, wie in dem folgenden Beispiel dargestellt.  
  
> [!NOTE]  
>  Wenn Sie eine Komponente erstellen möchten, die Sie einfacher in mehreren Datenflusstasks und Paketen wiederverwenden können, empfiehlt es sich, den Code in diesem Skriptkomponentenbeispiel als Ausgangspunkt für eine benutzerdefinierte Datenflusskomponente zu verwenden. Weitere Informationen finden Sie unter [Entwickeln einer benutzerdefinierten Datenflusskomponente](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md).  
  
## <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird veranschaulicht, wie eine Zielkomponente erstellt wird, die einen vorhandenen ODBC-Verbindungs-Manager zum Speichern von Daten aus dem Datenfluss in einer [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Tabelle verwendet.  
  
 Dieses Beispiel ist eine modifizierte Version des benutzerdefinierten [!INCLUDE[vstecado](../../includes/vstecado-md.md)]-Ziels, das im Thema [Erstellen eines Ziels mit der Skriptkomponente](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md) veranschaulicht wurde. In diesem Beispiel wurde das benutzerdefinierte [!INCLUDE[vstecado](../../includes/vstecado-md.md)]-Ziel jedoch so geändert, dass es mit einem ODBC-Verbindungs-Manager funktioniert und Daten in einem ODBC-Ziel speichert. Die Modifizierungen umfassen die folgenden Änderungen:  
  
-   Sie können die **AcquireConnection**-Methode des ODBC-Verbindungs-Managers nicht aus verwaltetem Code aufrufen, da so ein natives Objekt zurückgegeben wird. In diesem Beispiel wird daher die Verbindungszeichenfolge des Verbindungs-Managers verwendet, um eine direkte Verbindung mit der Datenquelle mithilfe des verwalteten ODBC-[!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]-Datenanbieters herzustellen.  
  
-   **OdbcCommand** erwartet Positionsparameter. Die Position von Parametern wird durch die Fragezeichen (?) im Text des Befehls angegeben. (Im Gegensatz dazu erwartet **SqlCommand** benannte Parameter.)  
  
 In diesem Beispiel wird die Tabelle **Person.Address** in der **AdventureWorks**-Beispieldatenbank verwendet. Im Beispiel werden die erste und die vierte Spalte dieser Tabelle, nämlich die Spalten **int _AddressID_** und **nvarchar(30) _City_**, durch den Datenfluss übergeben. Die gleichen Daten werden in den Beispielen für die Quelle, die Transformation und das Ziel im Thema [Developing Specific Types of Script Components](../../integration-services/extending-packages-scripting-data-flow-script-component-types/developing-specific-types-of-script-components.md) (Entwickeln bestimmter Typen von Skriptkomponenten) verwendet.  
  
#### <a name="to-configure-this-script-component-example"></a>So konfigurieren Sie dieses Skriptkomponentenbeispiel  
  
1.  Erstellen Sie einen ODBC-Verbindungs-Manager, der eine Verbindung mit der **AdventureWorks**-Datenbank herstellt.  
  
2.  Erstellen Sie eine Zieltabelle, indem Sie den folgenden Transact-SQL-Befehl in der **AdventureWorks**-Datenbank ausführen:  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
3.  Fügen Sie der Oberfläche des Datenfluss-Designers eine neue Skriptkomponente hinzu, und konfigurieren Sie sie als Ziel.  
  
4.  Verbinden Sie die Ausgabe einer Upstreamquelle oder Transformation mit der Zielkomponente im [!INCLUDE[ssIS](../../includes/ssis-md.md)]-Designer. (Sie können eine Quelle ohne Transformationen direkt mit einem Ziel verbinden.) Um sicherzustellen, dass dieses Beispiel funktioniert, muss die Ausgabe der Upstreamkomponente zumindest die Spalten **AddressID** und **City** aus der **Person.Address**-Tabelle der **AdventureWorks**-Beispieldatenbank enthalten.  
  
5.  Öffnen Sie den **Transformations-Editor für Skripterstellung**. Wählen Sie auf der Seite **Eingabespalten** die Spalten **AddressID** und **City** aus.  
  
6.  Geben Sie der Eingabe auf der Seite **Eingaben und Ausgaben** einen aussagekräftigeren Namen, z.B. **MyAddressInput**.  
  
7.  Fügen Sie auf der Seite **Verbindungs-Manager** den ODBC-Verbindungs-Manager hinzu, oder erstellen Sie diesen, und geben Sie ihm einen aussagekräftigen Namen, z.B. **MyODBCConnectionManager**.  
  
8.  Klicken Sie auf der Seite **Skript** auf **Skript bearbeiten**, und geben Sie das folgende Skript in die **ScriptMain**-Klasse ein.  
  
9. Schließen Sie anschließend die Skriptentwicklungsumgebung und den **Transformations-Editor für Skripterstellung**, und führen Sie das Beispiel aus.  
  
    ```vb  
    Imports System.Data.Odbc  
    ...  
    Public Class ScriptMain  
        Inherits UserComponent  
  
        Dim odbcConn As OdbcConnection  
        Dim odbcCmd As OdbcCommand  
        Dim odbcParam As OdbcParameter  
  
        Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
            Dim connectionString As String  
            connectionString = Me.Connections.MyODBCConnectionManager.ConnectionString  
            odbcConn = New OdbcConnection(connectionString)  
            odbcConn.Open()  
  
        End Sub  
  
        Public Overrides Sub PreExecute()  
  
            odbcCmd = New OdbcCommand("INSERT INTO Person.Address2(AddressID, City) " & _  
                "VALUES(?, ?)", odbcConn)  
            odbcParam = New OdbcParameter("@addressid", OdbcType.Int)  
            odbcCmd.Parameters.Add(odbcParam)  
            odbcParam = New OdbcParameter("@city", OdbcType.NVarChar, 30)  
            odbcCmd.Parameters.Add(odbcParam)  
  
        End Sub  
  
        Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
            With odbcCmd  
                .Parameters("@addressid").Value = Row.AddressID  
                .Parameters("@city").Value = Row.City  
                .ExecuteNonQuery()  
            End With  
  
        End Sub  
  
        Public Overrides Sub ReleaseConnections()  
  
            odbcConn.Close()  
  
        End Sub  
  
    End Class  
    ```  
  
    ```csharp  
    using System.Data.Odbc;  
    ...  
    public class ScriptMain :  
        UserComponent  
    {  
        OdbcConnection odbcConn;  
        OdbcCommand odbcCmd;  
        OdbcParameter odbcParam;  
  
        public override void AcquireConnections(object Transaction)  
        {  
  
            string connectionString;  
            connectionString = this.Connections.MyODBCConnectionManager.ConnectionString;  
            odbcConn = new OdbcConnection(connectionString);  
            odbcConn.Open();  
  
        }  
  
        public override void PreExecute()  
        {  
  
            odbcCmd = new OdbcCommand("INSERT INTO Person.Address2(AddressID, City) " +  
                "VALUES(?, ?)", odbcConn);  
            odbcParam = new OdbcParameter("@addressid", OdbcType.Int);  
            odbcCmd.Parameters.Add(odbcParam);  
            odbcParam = new OdbcParameter("@city", OdbcType.NVarChar, 30);  
            odbcCmd.Parameters.Add(odbcParam);  
  
        }  
  
        public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
        {  
  
            {  
                odbcCmd.Parameters["@addressid"].Value = Row.AddressID;  
                odbcCmd.Parameters["@city"].Value = Row.City;  
                odbcCmd.ExecuteNonQuery();  
            }  
  
        }  
  
        public override void ReleaseConnections()  
        {  
  
            odbcConn.Close();  
  
        }  
    }  
    ```  
  
## <a name="see-also"></a>Weitere Informationen  
 [Creating a Destination with the Script Component (Erstellen eines Ziels mit der Skriptkomponente)](../extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md)  
  
  
