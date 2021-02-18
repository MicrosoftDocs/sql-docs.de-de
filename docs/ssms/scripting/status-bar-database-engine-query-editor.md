---
title: Statusleiste (Abfrage-Editor der Datenbank-Engine)
description: Informieren Sie sich, wie Sie die Statusleiste eines Fensters im Datenbank-Engine-Abfrage-Editor einfärben, um anzugeben, mit welcher Instanz der Datenbank-Engine das Fenster verbunden ist.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: e7f2d6f4-bb94-4cf5-a096-c34397e679af
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e5ccb8f59a8c343936b163a10983afd9238ce06a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339342"
---
# <a name="status-bar-database-engine-query-editor"></a>Statusleiste (Abfrage-Editor der Datenbank-Engine)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Die Statusleiste des [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor-Fensters kann farblich codiert sein, um so anzuzeigen, mit welcher Instanz des [!INCLUDE[ssDE](../../includes/ssde-md.md)] jedes Fenster verbunden ist.

1. **Vorbereitungen:**  [Statusleistenfarben](#StatusBarColors)  

2. **So legen Sie eine Serverstatusfarbe fest in:**  [Objekt-Explorer](#SetOEServerColor), [Registrierte Server](#SetRegServerColor)  

3. **So verwenden Sie eine Statusfarbe**  [Abfrage-Editor unter Verwendung einer Serverfarbe öffnen](#OpenServerColor), [Abfrage-Editor unter Angabe einer Statusfarbe öffnen](#OpenSpecColor)  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

##  <a name="status-bar-colors"></a><a name="StatusBarColors"></a> Statusleistenfarben

Sie können einem bestimmten Serverknoten in entweder **Objekt-Explorer** oder **Registrierte Server** eine Statusleistenfarbe zuordnen. Die Farben können nur für Serverknoten angegeben werden, die mit einer Instanz des [!INCLUDE[ssDE](../../includes/ssde-md.md)]verbunden sind, nicht aber für Serverknoten für andere SQL Server-Technologien. Ebenso können Sie eine benutzerdefinierte Statusleistenfarbe angeben, wann immer Sie ein neues [!INCLUDE[ssDE](../../includes/ssde-md.md)] -Abfrage-Editor-Fenster mit einer Instanz des [!INCLUDE[ssDE](../../includes/ssde-md.md)]verbinden. Sie können dann entweder mit der für den Serverknoten definierten Statusfarbe ein Abfrage-Editor-Fenster öffnen oder eine eindeutige Farbe für dieses Editorfenster angeben.  

Eine benutzerdefinierte Statusleistenfarbe für einen Serverknoten in Objekt-Explorer muss beim Herstellen der Verbindung festgelegt werden. Um die einem vorhandenen Serverknoten zugeordnete Farbe zu ändern, müssen Sie die Verbindung trennen und dann unter Angabe der neuen Farbe die Verbindung erneut herstellen.  

##  <a name="set-the-status-color-for-a-server-in-object-explorer"></a><a name="SetOEServerColor"></a> Festlegen der Statusfarbe für einen Server in Objekt-Explorer

**So legen Sie eine Serverstatusfarbe in Objekt-Explorer fest**  
  
1.  Wählen Sie im **Objekt-Explorer** die Schaltfläche **Verbinden** und dann **Datenbank-Engine...** aus.  
  
2.  Wählen Sie im Dialogfeld **Verbindung mit Server herstellen** die Option **Optionen >>** .  
  
3.  Aktivieren Sie das Kontrollkästchen **Benutzerdefinierte Farbe verwenden** .  
  
4.  Wählen Sie zur Festlegung der Farbe die Schaltfläche **Auswählen...** aus.  
  
5.  Wählen Sie entweder eine standardmäßige oder benutzerdefinierte Farbe aus und klicken Sie dann auf OK.  
  
6.  Geben Sie die restlichen Verbindungsinformationen ein und wählen Sie dann die Schaltfläche **Verbinden** aus.  
  
##  <a name="set-the-status-color-for-a-registered-server"></a><a name="SetRegServerColor"></a> Festlegen der Statusfarbe für einen Registrierter Server  
 **So legen Sie eine Serverfarbe für einen Registrierten Server fest**  
  
1.  Klicken Sie unter **Registrierte Server** mit der rechten Maustaste auf einen Serverknoten, und wählen Sie dann **Eigenschaften...** aus.  
  
2.  Wählen Sie im Dialogfeld **Serverregistrierungseigenschaften bearbeiten** die Registerkarte **Verbindungseigenschaften** aus.  
  
3.  Aktivieren Sie das Kontrollkästchen **Benutzerdefinierte Farbe verwenden** .  
  
4.  Wählen Sie zur Festlegung der Farbe die Schaltfläche **Auswählen...** aus.  
  
5.  Wählen Sie entweder eine standardmäßige oder benutzerdefinierte Farbe aus und klicken Sie dann auf OK.  
  
6.  Wählen Sie im Dialogfeld **Serverregistrierungseigenschaften bearbeiten** die Schaltfläche **Speichern** aus.  
  
##  <a name="open-an-editor-using-a-server-color"></a><a name="OpenServerColor"></a> Öffnen eines Editors mithilfe einer Serverfarbe  
 **So öffnen Sie ein Editorfenster mithilfe einer Serverfarbe**  
  
-   Klicken Sie mit der rechten Maustaste entweder in **Objekt-Explorer** oder **Registrierte Server** auf einen Serverknoten, und wählen Sie **Neue Abfrage** aus.  
  
-   Markieren Sie alternativ einen Serverknoten und wählen Sie dann die Symbolleistenschaltfläche **Neue Abfrage** aus.  
  
-   Die Statusleiste im Editorfenster verwendet die für den zugeordneten Server definierte Farbe.  
  
##  <a name="open-an-editor-specifying-a-status-color"></a><a name="OpenSpecColor"></a> Öffnen eines Editors unter Angabe einer Statusfarbe  
 **So öffnen Sie ein Editorfenster unter Angabe einer Statusfarbe**  
  
-   Öffnen Sie das Menü **Datei**, wählen Sie **Neu** aus, und wählen Sie dann **Datenbank-Engine-Abfrage** aus.  
  
-   Wählen Sie im Dialogfeld **Verbindung mit Server herstellen** die Option **Optionen >>** .  
  
-   Aktivieren Sie das Kontrollkästchen **Benutzerdefinierte Farbe verwenden** .  
  
-   Wählen Sie zur Festlegung der Farbe die Schaltfläche **Auswählen...** aus.  
  
-   Wählen Sie entweder eine standardmäßige oder benutzerdefinierte Farbe aus und klicken Sie dann auf OK.  
  
-   Geben Sie die restlichen Verbindungsinformationen ein und wählen Sie dann die Schaltfläche **Verbinden** aus.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Abfrage- und Text-Editoren &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)  
  
