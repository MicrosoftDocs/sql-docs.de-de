---
description: Beenden der Vorbereitung für Testfälle (OracleToSQL)
title: Fertigstellung der Test Fall Vorbereitung (oracleto SQL) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 32f38713-7ae4-48d3-980d-74cadc8545a0
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: f1d0f99987a58a8d942c9d7601124c3cfc39b09d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014621"
---
# <a name="finishing-test-case-preparation-oracletosql"></a>Beenden der Vorbereitung für Testfälle (OracleToSQL)
Auf der letzten Seite des Assistenten werden die Test Fallbeschreibung und Informationen zu den am Test beteiligten Objekten angezeigt. Außerdem können Sie auf dieser Seite die Test Ausführungs Optionen festlegen.  
  
Der Abschnitt " **Test Fall Informationen** " zeigt den Namen und die Beschreibung des Testfalls an.  
  
Der Abschnitt **zu testende Objekte** enthält die benannte Liste getesteter Objekte, gruppiert nach Objekttyp.  
  
Der Abschnitt **von Tests Betroffene Objekte, die analysiert werden,** zeigt die benannte Liste der Objekte an, die nach der Ausführung getesteter Objekte mit Datenänderungen verglichen werden sollen.  
  
## <a name="test-case-settings"></a>Test Fall Einstellungen  
Im Abschnitt **Test Fall Einstellungen** können Sie die folgenden Ausführungs Test Optionen festlegen:  
  
### <a name="stop-test-execution-after-first-failure"></a>Testausführung nach dem ersten Fehler beendet  
Gibt an, dass der Test unterbricht, wenn während der Testausführung ein Fehler auftritt.  
  
-   Wenn Sie **Ja** auswählen, wird die Testausführung unterbrochen, wenn ein Fehler auftritt.  
  
-   Wenn Sie **Nein** auswählen, wird die Testausführung nach einem Fehler fortgesetzt.  
  
### <a name="perform-data-rollback"></a>Ausführen eines Daten Rollbacks  
Aktiviert das automatische Zurücksetzen von Daten nach der Testausführung.  
  
-   Wenn Sie **Ja** auswählen, gehen Datenänderungen nach der Ausführung des Tests verloren.  
  
-   Wenn Sie **Nein** auswählen, werden alle Änderungen an der Test Ausführungsdaten gespeichert.  
  
### <a name="auxiliary-tables-saving-mode"></a>Speicher Modus für Hilfstabellen  
Definiert den Speicher Modus für die während der Testausführung erstellten Hilfstabellen. Weitere Informationen finden Sie in der Beschreibung der Erweiterungs Tabellen im Thema [Ausführen von Test Fällen &#40;oracleto SQL&#41;](../../ssma/oracle/running-test-cases-oracletosql.md) .  
  
-   Wenn Sie **immer speichern** auswählen, werden die Daten der zusätzlichen Tabelle immer zur späteren Verwendung gespeichert.  
  
-   Wenn Sie speichern auswählen, **Wenn beim Tabellenvergleich** ein Fehler aufgetreten ist, werden die Daten der zusätzlichen Tabelle nur dann gespeichert, wenn ein Fehler auftritt.  
  
-   Wenn Sie **immer löschen** auswählen, werden zusätzliche Tabellen nach der Ausführung des Tests immer gelöscht.  
  
-   Wenn Sie bei **fehlgeschlagenen Tabellen vergleichen die Option Benutzer anfordern** auswählen, kann der Benutzer die erforderliche Aktion auswählen, wenn ein Fehler auftritt.  
  
Klicken Sie auf die Schaltfläche **Fertig** stellen, um den vorbereiteten Testfall in [mithilfe von Test Depots (oracleto SQL)](./using-test-repositories-oracletosql.md)zu speichern.  
  
## <a name="see-also"></a>Weitere Informationen  
[Verwenden von Test Depots &#40;oracleto SQL&#41;](../../ssma/oracle/using-test-repositories-oracletosql.md)  
[Ausführen von Test Fällen &#40;oracleto SQL&#41;](../../ssma/oracle/running-test-cases-oracletosql.md)  
[Testen von migrierten Datenbankobjekten &#40;oracleto SQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
