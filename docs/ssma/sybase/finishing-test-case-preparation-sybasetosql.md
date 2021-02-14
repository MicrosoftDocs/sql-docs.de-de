---
description: Beenden der Vorbereitung für Testfälle (SybaseToSQL)
title: Fertigstellung der Test Fall Vorbereitung (sybaseto SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester Component,Test Case Settings
ms.assetid: 8b2a49b0-4296-4f3f-9e56-323aa6a6fa8e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 0c1f34f19034042d542cba2d81a55bf32288058e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067518"
---
# <a name="finishing-test-case-preparation-sybasetosql"></a>Beenden der Vorbereitung für Testfälle (SybaseToSQL)
Auf der letzten Seite des Assistenten werden die Test Fallbeschreibung und Informationen zu den am Test beteiligten Objekten angezeigt. Außerdem können Sie auf dieser Seite die Test Ausführungs Optionen festlegen.  
  
Der Abschnitt " **Test Fall Informationen** " zeigt den Namen und die Beschreibung des Testfalls an.  
  
Der Abschnitt " **Test Objekte** " enthält die benannte Liste getesteter Objekte, die nach Objekttyp gruppiert sind.  
  
Im Abschnitt **Betroffene Objekte, die analysiert werden** sollen, wird die benannte Liste der Objekte angezeigt, die nach der Ausführung getesteter Objekte mit Datenänderungen verglichen werden sollen.  
  
## <a name="test-case-settings"></a>Test Fall Einstellungen  
Im Abschnitt **Test Fall Einstellungen** können Sie die folgenden Ausführungs Test Optionen festlegen:  
  
### <a name="stop-test-execution-after-first-failure"></a>Testausführung nach dem ersten Fehler beendet  
Gibt an, dass der Test unterbricht, wenn während der Testausführung ein Fehler auftritt.  
  
-   Wenn Sie **Ja** auswählen, wird die Testausführung unterbrochen, wenn ein Fehler auftritt.  
  
-   Wenn Sie **Nein** auswählen, wird die Testausführung nach einem Fehler fortgesetzt.  
  
### <a name="perform-data-rollback"></a>Ausführen eines Daten Rollbacks  
Aktivieren des automatischen Daten Rollbacks nach der Testausführung.  
  
-   Wenn Sie **Ja** auswählen, gehen Datenänderungen nach der Ausführung des Tests verloren.  
  
-   Wenn Sie **Nein** auswählen, werden alle Änderungen an der Test Ausführungsdaten gespeichert.  
  
### <a name="auxiliary-tables-saving-mode"></a>Speicher Modus für Hilfstabellen  
Definiert den Speicher Modus für die während der Testausführung erstellten Hilfstabellen. Weitere Informationen finden Sie in der Beschreibung der Erweiterungs Tabellen im Thema [Ausführen von Test Fällen &#40;sybaseto SQL&#41;](../../ssma/sybase/running-test-cases-sybasetosql.md) .  
  
-   Wenn Sie **immer speichern** auswählen, werden die Daten der zusätzlichen Tabelle immer zur späteren Verwendung gespeichert.  
  
-   Wenn Sie speichern auswählen, **Wenn beim Tabellenvergleich** ein Fehler aufgetreten ist, werden die Daten der zusätzlichen Tabelle nur dann gespeichert, wenn ein Fehler auftritt.  
  
-   Wenn Sie **immer löschen** auswählen, werden zusätzliche Tabellen nach der Ausführung des Tests immer gelöscht.  
  
-   Wenn Sie bei **fehlgeschlagenen Tabellen vergleichen die Option Benutzer anfordern** auswählen, kann der Benutzer die erforderliche Aktion auswählen, wenn ein Fehler auftritt.  
  
Klicken Sie auf die Schaltfläche **Fertig** stellen, um den vorbereiteten Testfall in [mithilfe von Test Depots &#40;Sybase&#41;](../../ssma/sybase/using-test-repositories-sybasetosql.md)zu speichern.  
  
## <a name="see-also"></a>Weitere Informationen  
[Verwenden von Test Depots &#40;Sybase&#41;](../../ssma/sybase/using-test-repositories-sybasetosql.md)  
[Ausführen von Test Fällen &#40;sybaseto SQL-&#41;](../../ssma/sybase/running-test-cases-sybasetosql.md)  
[Testen von migrierten Datenbankobjekten &#40;sybaseto SQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
