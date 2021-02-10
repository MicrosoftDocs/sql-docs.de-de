---
description: Verwalten von Kennwörtern (MySqlToSql)
title: Verwalten von Kenn Wörtern (mysqlto SQL) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 07/06/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Password management, importing or exporting encrypted password
- Password management, securing password
ms.assetid: 4ffbc587-ea3f-49ad-bc42-a654f672325e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e5986144465beac5b9358869bbbd5f7510d70fbe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017477"
---
# <a name="managing-passwords-mysqltosql"></a>Verwalten von Kennwörtern (MySqlToSql)
In diesem Artikel geht es um das Sichern von Daten Bank Kennwörtern und das Verfahren zum Importieren oder Exportieren von Datenbankservern.
  
## <a name="securing-password"></a>Sichern des Kennworts  
SSMA ermöglicht Ihnen das Sichern Ihres Kennworts für eine Datenbank.  
  
Verwenden Sie das folgende Verfahren, um eine sichere Verbindung zu implementieren:  
  
Geben Sie ein gültiges Kennwort an, indem Sie eine der folgenden drei Methoden verwenden:  
  
1.  **Klartext löschen:** Geben Sie das Daten Bank Kennwort in das Attribut Wert des Knotens "Password" ein. Sie befindet sich unter dem Knoten Server Definition im Abschnitt Server der Skriptdatei oder der Server Verbindungs Datei.  
  
    Kenn Wörter im Klartext sind nicht sicher. Aus diesem Grund wird in der Konsolenausgabe folgende Warnmeldung angezeigt: *"Server &lt; Server-ID &gt; Kennwort wird in unsicherem Klartext bereitgestellt. die SSMA-Konsolenanwendung bietet eine Option zum Schützen des Kennworts durch Verschlüsselung. Weitere Informationen finden Sie unter der Option"-SecurePassword "in der SSMA-Hilfedatei.*  
  
    **Verschlüsselte Kenn Wörter:** Das angegebene Kennwort wird in diesem Fall in verschlüsselter Form auf dem lokalen Computer in ProtectedStorage. SSMA gespeichert.  
  
    -   **Sichern von Kenn Wörtern**  
  
        -   Führen `SSMAforMySQLConsole.exe` Sie das mit dem `-securepassword` und den Switch-Schalter in der Befehlszeile aus, und übergeben Sie die Server Verbindung oder die Skriptdatei mit dem Kenn Wort Knoten im Abschnitt Server Definition.  
  
        -   Bei der Eingabeaufforderung wird der Benutzer aufgefordert, das Daten Bank Kennwort einzugeben und zu bestätigen.  
  
            Die Server Definitions-IDs und die zugehörigen verschlüsselten Kenn Wörter werden in einer Datei auf dem lokalen Computer gespeichert.  
            
            **Beispiel 1:**
            
            1. Kennwort angeben
            
            2. `C:\SSMA\SSMAforMySQLConsole.EXE -securepassword -add all -s "D:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\AssessmentReportGenerationSample.xml" -v "D:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ VariableValueFileSample.xml"`
            
            3. Kennwort für server_id ' XXX_1 ' eingeben: XXXXXXX
            
            4. Kennwort für server_id ' XXX_1 ' erneut eingeben: XXXXXXX
            
            **Beispiel 2:**
            
            1. `C:\SSMA\SSMAforMySQLConsole.EXE -securepassword -add "source_1,target_1" -c "D:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ServersConnectionFileSample.xml" - v "D:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ VariableValueFileSample.xml" -o`
            
            2. Kennwort für server_id ' source_1 ' eingeben: XXXXXXX
            
            3. Kennwort für server_id ' source_1 ' erneut eingeben: XXXXXXX
            
            4. Kennwort für server_id ' target_1 ' eingeben: XXXXXXX
            
            5. Kennwort für server_id "Ziel _1" erneut eingeben: XXXXXXX
            
    -   **Entfernen verschlüsselter Kenn Wörter**  
  
        Führen `SSMAforMySQLConsole.exe` Sie das mit `-securepassword` dem `-remove` aus, und wechseln Sie in der Befehlszeile zum übergeben der Server-IDs, um die verschlüsselten Kenn Wörter aus der geschützten Speicherdatei auf dem lokalen Computer zu entfernen.  
  
        Beispiel:  

        ```console
        C:\SSMA\SSMAforMySQLConsole.EXE -securepassword -remove all
        C:\SSMA\SSMAforMySQLConsole.EXE -securepassword -remove "source_1,target_1"  
        ```
  
    -   **Auflisten von Server-IDs, deren Kenn Wörter verschlüsselt sind**  
  
        Führen `SSMAforMySQLConsole.exe` Sie mit dem `-securepassword` aus `-list` , und wechseln Sie in der Befehlszeile, um alle Server-IDs aufzulisten, deren Kenn Wörter verschlüsselt wurden.  
  
        Beispiel:  
        
        ```console
        C:\SSMA\SSMAforMySQLConsole.EXE -securepassword -list  
        ```
  
    > [!NOTE]  
    > 1.  Das Kennwort im Klartext, das in der Skript-oder Server Verbindungs Datei angegeben ist, hat Vorrang vor dem verschlüsselten Kennwort in der gesicherten Datei.  
    > 2.  Wenn im Server Abschnitt der Server Verbindungs Datei oder der Skriptdatei kein Kennwort vorhanden ist oder wenn es nicht auf dem lokalen Computer gesichert wurde, werden Sie von der-Konsole aufgefordert, das Kennwort einzugeben.  
  
## <a name="exporting-or-importing-encrypted-passwords"></a>Exportieren oder importieren verschlüsselter Kenn Wörter  
Mit der SSMA-Konsolenanwendung können Sie verschlüsselte Daten Bank Kennwörter, die in einer Datei auf dem lokalen Computer vorhanden sind, in eine gesicherte Datei exportieren und umgekehrt. Dies hilft bei der unabhängigen Erstellung der verschlüsselten Kenn Wörter.

Die Export Funktion liest die Server-ID und das Kennwort aus dem lokalen geschützten Speicher und speichert die Informationen in einer verschlüsselten Datei. Der Benutzer wird aufgefordert, das Kennwort für die gesicherte Datei einzugeben. Stellen Sie sicher, dass das eingegebene Kennwort mindestens 8 Zeichen lang ist. Diese gesicherte Datei ist auf verschiedenen Computern portabel.

Die Import Funktion liest die Server-ID und die Kenn Wort Informationen aus der gesicherten Datei. Der Benutzer wird aufgefordert, das Kennwort für die gesicherte Datei einzugeben, und fügt die Informationen an den lokalen geschützten Speicher an.  
  
### <a name="export-example"></a>Beispiel für den Export:  

1. Kennwort exportieren

2. Kennwort zum Schutz der exportierten Datei eingeben

3. C:\SSMA\SSMAforMySQLConsole.EXE-SecurePassword-Export alle "machine1passwords. File"

4. Geben Sie das Kennwort zum Schutz der exportierten Datei ein: xxxxxxxx

5. Bestätigen Sie das Kennwort: xxxxxxxx

6. C:\SSMA\SSMAforMySQLConsole.EXE-p-e "MySQLDB_1_1 Sql_1" "machine2passwords. File"

7. Geben Sie das Kennwort zum Schutz der exportierten Datei ein: xxxxxxxx

8. Bestätigen Sie das Kennwort: xxxxxxxx  
  
### <a name="import-example"></a>Beispiel für den Import:  

1. Importieren eines verschlüsselten Kennworts

2. Kennwort zum Schutz der importierten Datei eingeben

3. C:\SSMA\SSMAforMySQLConsole.EXE-SecurePassword-alle "machine1passwords. File" Importieren

4. Kennwort eingeben, um die Server aus der verschlüsselten Datei zu importieren: xxxxxxxx

5. Bestätigen Sie das Kennwort: xxxxxxxx

6. C:\SSMA\SSMAforMySQLConsole.EXE-p-i "MySQLDB_1 Sql_1" "machine2passwords. File"

7. Kennwort eingeben, um die Server aus der verschlüsselten Datei zu importieren: xxxxxxxx

8. Bestätigen Sie das Kennwort: xxxxxxxx  
  
## <a name="see-also"></a>Weitere Informationen  
[Ausführen der SSMA-Konsole (MySQL)](./executing-the-ssma-console-mysqltosql.md)  
