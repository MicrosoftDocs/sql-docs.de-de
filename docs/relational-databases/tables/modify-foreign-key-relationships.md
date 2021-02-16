---
description: Ändern von Fremdschlüsselbeziehungen
title: Ändern von Fremdschlüsselbeziehungen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 07/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- vdtsql.chm:65538
- vdt.ppg.relationships
helpviewer_keywords:
- foreign keys [SQL Server], modifying
- modifying foreign keys
ms.assetid: 0c9ca80d-d79b-44c4-a21e-0fce39c398ec
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 457f4db2126b314237ccc09678718e14ffc25069
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354126"
---
# <a name="modify-foreign-key-relationships"></a>Ändern von Fremdschlüsselbeziehungen

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Sie können die Fremdschlüsselseite einer Beziehung in [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../includes/tsql-md.md)]ändern. Wenn Sie den Fremdschlüssel einer Tabelle ändern, wird entsprechend angepasst, welche Spalten mit den Spalten in der Primärschlüsseltabelle verknüpft sind.  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Einschränkungen](#Restrictions)  
  
     [Security](#Security)  
  
-   **So ändern Sie einen Fremdschlüssel mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
 Für die neue Fremdschlüsselspalte und die verknüpfte Primärschlüsselspalte müssen Datentyp und Größe übereinstimmen, mit diesen Ausnahmen:  
  
-   Eine Spalte vom Typ **char** oder **sysname** kann mit einer Spalte vom Typ **varchar** verknüpft werden.  
  
-   Eine Spalte vom Typ **binary** kann mit einer Spalte vom Typ **varbinary** verknüpft werden.  
  
-   Ein Aliasdatentyp kann mit seinem Basistyp verknüpft werden.  
  
###  <a name="security"></a><a name="Security"></a> Sicherheit  
  
####  <a name="permissions"></a><a name="Permissions"></a> Berechtigungen  
 Erfordert die ALTER-Berechtigung für die Tabelle.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
  
#### <a name="to-modify-a-foreign-key"></a>So ändern Sie einen Fremdschlüssel  
  
1.  Erweitern Sie im **Objekt-Explorer** die Tabelle mit dem Fremdschlüssel, und erweitern Sie dann die Option **Schlüssel**.  
  
2.  Klicken Sie mit der rechten Maustaste auf den zu ändernden Fremdschlüssel, und wählen Sie die Option **Ändern**.  
  
3.  Im Dialogfeld **Fremdschlüsselbeziehungen** können Sie die folgenden Änderungen vornehmen.  
  
     **Ausgew. Beziehung**  
     Listet bestehende Beziehungen auf. Wählen Sie eine Beziehung aus, um ihre Eigenschaften im Datenblatt rechts anzuzeigen. Wenn die Liste leer ist, wurden bisher keine Beziehungen für die Tabelle definiert.  
  
     **Add (Hinzufügen)**  
     Erstellt eine neue Beziehung. Die **Tabellen- und Spaltenspezifikation** muss festgelegt werden, bevor die Beziehung gültig wird.  
  
     **Löschen**  
     Löscht die in der Liste **ausgewählte Beziehung** ausgewählte Beziehung. Verwenden Sie diese Schaltfläche zum Entfernen der Beziehung, um das Hinzufügen einer Beziehung abzubrechen.  
  
     **Kategorie Allgemein**  
     Wenn die Kategorie erweitert ist, werden **Vorhandene Daten bei Erstellung oder Reaktivierung überprüfen** und **Tabellen- und Spaltenspezifikation** angezeigt.  
  
     **Vorhandene Daten bei Erstellung oder Reaktivierung überprüfen**  
     Überprüft alle Daten, die vor der Erstellung oder Reaktivierung der Einschränkung in der Tabelle vorhandenen sind, auf die Einschränkung hin.  
  
     **Kategorie Tabellen- und Spaltenspezifikation**  
     Wenn die Kategorie erweitert ist, wird angezeigt, welche Spalten aus welchen Tabellen als Fremdschlüssel, Primärschlüssel oder eindeutiger Schlüssel in der Beziehung fungieren. Klicken Sie rechts neben dem Eigenschaftenfeld auf die Schaltfläche mit den Auslassungspunkten ( **...** ), um diese Werte zu bearbeiten oder zu definieren.  
  
     **Fremdschlüssel-Basistabelle**  
     Zeigt an, welche Tabelle die Spalte enthält, die in der ausgewählten Beziehung als Fremdschlüssel fungiert.  
  
     **Fremdschlüsselspalten**  
     Zeigt an, welche Spalte in der ausgewählten Beziehung als Fremdschlüssel fungiert.  
  
     **Primary/Unique Schlüsselbasistabelle**  
     Zeigt an, welche Tabelle die Spalte enthält, die in der ausgewählten Beziehung als Primärschlüssel oder eindeutiger Schlüssel fungiert.  
  
     **Primary/Unique Schlüsselspalten**  
     Zeigt an, welche Spalte in der ausgewählten Beziehung als Primärschlüssel oder eindeutiger Schlüssel fungiert.  
  
     **Kategorie Identität**  
     Wenn die Kategorie erweitert ist, werden die Eigenschaftenfelder für **Name** und **Beschreibung** angezeigt.  
  
     **Name**  
     Zeigt den Namen der Beziehung an. Wenn eine neue Beziehung erstellt wird, erhält sie einen Standardnamen, der auf der Tabelle im aktiven Fenster im **Tabellen-Designer** basiert. Sie können den Namen jederzeit ändern.  
  
     **Beschreibung**  
     Beschreibt die Beziehung. Um eine detailliertere Beschreibung zu erstellen, klicken Sie auf **Beschreibung** , und klicken Sie anschließend auf die Auslassungspunkte **(...)** rechts neben dem Eigenschaftenfeld. Dadurch wird ein größerer Bereich verfügbar, in den Sie Text eingeben können.  
  
     **Kategorie Tabellen-Designer**  
     Wenn die Kategorie erweitert ist, werden Informationen über **Vorhandene Daten bei Erstellung oder Reaktivierung überprüfen** und **Für Replikation erzwingen** angezeigt.  
  
     **Für Replikation erzwingen**  
     Gibt an, ob die Einschränkung erzwungen wird, wenn durch den Replikations-Agent in der Tabelle eine INSERT-, ein UPDATE- oder DELETE-Anweisung ausgeführt wird.  
  
     **Fremdschlüsseleinschränkung erzwingen**  
     Gibt an, ob Änderungen der Daten in den Spalten der Beziehung zulässig sind, wenn die Integrität der Fremdschlüsselbeziehung durch diese Änderungen aufgehoben werden. Wählen Sie **Ja** aus, um solche Änderungen nicht zuzulassen, und wählen Sie **Nein** aus, um sie zuzulassen.  
  
     **Kategorie INSERT- und UPDATE-Spezifikation**  
     Wenn die Kategorie erweitert ist, werden Informationen über **Regel löschen** und **Regel aktualisieren** für die Beziehung angezeigt.  
  
     **Regel löschen**  
     Gibt an, was geschehen soll, wenn ein Benutzer versucht, eine Zeile mit Daten zu löschen, die Teil einer Fremdschlüsselbeziehung ist.  
  
    -   **Keine Aktion** Eine Fehlermeldung teilt dem Benutzer mit, dass der Löschvorgang unzulässig ist und ein Rollback für die DELETE-Anweisung durchgeführt wurde.  
  
    -   **Löschweitergabe** Löscht alle Zeilen, die Daten enthalten, die Teil der Fremdschlüsselbeziehung sind. Geben Sie CASCADE nicht an, wenn die Tabelle in eine Mergeveröffentlichung einbezogen werden soll, bei der logische Datensätze verwendet werden.  
  
    -   **NULL festlegen** Legt den Wert auf NULL fest, wenn alle Fremdschlüsselspalten der Tabelle NULL-Werte annehmen können.  
  
    -   **Standard festlegen** Legt den Wert auf den für die Spalte definierten Standardwert fest, wenn für alle Fremdschlüsselspalten der Tabelle Standardwerte definiert sind.  
  
     **Regel aktualisieren**  
     Geben Sie an, was geschieht, wenn ein Benutzer versucht, eine Zeile mit Daten zu aktualisieren, die an einer Fremdschlüsselbeziehung beteiligt sind:  
  
    -   **Keine Aktion** In einer Fehlermeldung wird dem Benutzer mitgeteilt, dass das Update unzulässig ist und ein Rollback für die UPDATE-Anweisung ausgeführt wird.  
  
    -   **Überlappend** Aktualisiert alle Zeilen, die Daten enthalten, die an der Fremdschlüsselbeziehung beteiligt sind. Geben Sie CASCADE nicht an, wenn die Tabelle in eine Mergeveröffentlichung einbezogen werden soll, bei der logische Datensätze verwendet werden.  
  
    -   **NULL festlegen** Legt den Wert auf NULL fest, wenn alle Fremdschlüsselspalten der Tabelle NULL-Werte annehmen können.  
  
    -   **Standard festlegen** Legt den Wert auf den für die Spalte definierten Standardwert fest, wenn für alle Fremdschlüsselspalten der Tabelle Standardwerte definiert sind.  
  
4.  Klicken Sie im Menü **Datei** auf _Tabellenname_**speichern**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 **So ändern Sie einen Fremdschlüssel**  
  
 Um eine FOREIGN KEY-Einschränkung mit Transact-SQL zu ändern, müssen Sie zuerst die vorhandene FOREIGN KEY-Einschränkung löschen und sie dann mit der neuen Definition neu erstellen. Weitere Informationen finden Sie unter [Delete Foreign Key Relationships](../../relational-databases/tables/delete-foreign-key-relationships.md) und [Create Foreign Key Relationships](../../relational-databases/tables/create-foreign-key-relationships.md).  
  
###  <a name="TsqlExample"></a>  
