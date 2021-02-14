---
description: Auswählen und Konfigurieren von zu testenden Objekten (OracleToSQL)
title: Auswählen und Konfigurieren von zu testenden Objekten (oracleto SQL) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Selection of Objects to Test,Parameter Comparison Settings
ms.assetid: 29fb6542-5c1f-4b14-a822-87700beb7623
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 2cf0aab82f56740dfe73476af89899fd9fd135f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067612"
---
# <a name="selecting-and-configuring-objects-to-test-oracletosql"></a>Auswählen und Konfigurieren von zu testenden Objekten (OracleToSQL)
In diesem Schritt wählen Sie die zu testenden Objekte aus und konfigurieren die Einstellungen für den Vergleich der Ausgabeparameter der Prozeduren und Funktionen sowie die Rückgabewerte von Funktionen.  
  
## <a name="selection-of-objects-to-test"></a>Auswahl der zu testenden Objekte  
Überprüfen Sie in der Oracle-Objektstruktur, die sich auf der linken Seite des Fensters befindet, die Objekte, die Sie während des Testprozesses aufrufen möchten. Weitere Informationen finden Sie in der vollständigen Liste mit Test fähigen Objekten im Thema [Testen von migrierten Datenbankobjekten &#40;oracleto SQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md) .  
  
Wenn SSMA Tester keines der für den Test ausgewählten Objekte unterstützt, wird der Link mit der Bezeichnung **einige ausgewählte Objekte enthält** unter der Struktur Objekte Fehler angezeigt. Klicken Sie auf diesen Link, um die Gründe anzuzeigen, warum diese Objekte nicht getestet werden können, und um die Auswahl falscher Objekte zu löschen.  
  
Auf der rechten Seite können Sie mehrere Seiten anzeigen. die **SQL** -Seite zeigt die Definition des aktuellen Objekts an. Die Seite **Parameter** listet die Parameter auf, wenn es sich bei dem Objekt um eine gespeicherte Prozedur oder eine Funktion handelt. Auf der Seite **Eigenschaften** werden zusätzliche Eigenschaften des Objekts angezeigt. Weitere Informationen finden Sie in der Beschreibung der Seiten **Parameter Vergleiche** und **Aufrufen von Werten** .  
  
## <a name="parameter-comparison-settings"></a>Parameter Vergleichs Einstellungen  
Legen Sie die Vergleichs Regeln für Ausgabeparameter und Rückgabewerte auf der Seite **Parameter Vergleiche** fest. Sie können die folgenden Einstellungen vornehmen.  
  
### <a name="use-during-test-comparisons"></a>Verwendung während der Test Vergleiche  
Aktiviert die Verwendung des ausgewählten Parameters im Vergleich mit Testergebnissen.  
  
-   Wenn Sie **true** auswählen, vergleicht SSMA den Ausgabewert dieses Parameters nach dem Ausführen der Prozedur in Oracle mit dem entsprechenden Wert auf SQL Server.
  
-   Wenn Sie **false** auswählen, wird der Parameter von der Ergebnis Überprüfung ausgeschlossen.  
  
### <a name="use-custom-scale"></a>Benutzerdefinierte Skalierung verwenden  
Für Parameter des numerischen Datentyps können Sie eine benutzerdefinierte Skala für den Vergleich festlegen.  
  
-   Wenn Sie **true** auswählen, werden numerische Werte entsprechend dem **Vergleichs Skalierungs** Wert gerundet, bevor Sie verglichen werden.  
  
-   Wenn Sie **false** auswählen, ist der numerische Vergleich genau.  
  
### <a name="comparing-scale"></a>Vergleichen der Skala  
Nur verfügbar, wenn die Option **benutzerdefinierte Skalierung verwenden** auf **true** festgelegt ist. Dies ist die Genauigkeit für den numerischen Vergleich.  
  
### <a name="date-time-comparing"></a>Vergleichen von Datum und Uhrzeit  
Definiert, wie Datums-/Uhrzeitwerte verglichen werden.  
  
-   Wenn Sie die Option **gesamtes Datum vergleichen** auswählen, wird ein vollständiger Vergleich der Werte beider Plattformen ausgeführt.  
  
-   Wenn Sie **Datum nur vergleichen** auswählen, wird der Uhrzeit Teil ignoriert.  
  
-   Wenn Sie **nur die Zeit vergleichen** auswählen, wird der Datums Teil ignoriert.  
  
-   Wenn Sie die Option **Millisekunden ignorieren** auswählen, werden die Ergebnisse bis zu Sekunden verglichen.  
  
-   Wenn Sie **Datum und Millisekunden ignorieren** auswählen, wird das Ergebnis nur nach Zeit Teil verglichen, und die Sekundenbruchteile werden ignoriert.  
  
### <a name="ignore-strings-case"></a>Zeichen folgen ignorieren  
Steuert die Groß-/Kleinschreibung des Vergleichs.  
  
-   Wenn Sie **true** auswählen, wird beim Vergleich die Groß-/Kleinschreibung nicht beachtet.  
  
-   Wenn Sie **false** auswählen, wird beim Vergleich die Groß-/Kleinschreibung beachtet.  
  
### <a name="ignore-trailing-spaces"></a>Nachfolgende Leerzeichen ignorieren  
Steuert, wie nachfolgende Leerzeichen während des Vergleichs behandelt werden.  
  
-   Wenn Sie **true** auswählen, werden die verglichenen Zeichen folgen vor dem Vergleich mit der rechten Seite abgeschnitten.  
  
-   Wenn Sie **false** auswählen, werden nachfolgende Leerzeichen in den verglichenen Zeichen folgen beibehalten.  
  
## <a name="specify-input-values-for-procedures-and-functions-call-values"></a>Angeben von Eingabe Werten für Prozeduren und Funktionen (aufrufwerte)  
Sie können die Eingabeparameter Werte auf der Seite " **Werte aufrufen** " angeben. Mit der Schaltfläche " **aufrufen** " wird ein neuer-Befehl mit leeren Parameterwerten hinzugefügt. Die Schaltfläche **Aufrufe entfernen** entfernt den aktuellen Aufruf.  
  
## <a name="next-step"></a>Nächster Schritt  
[Auswählen und konfigurieren betroffener Objekte &#40;oracleto SQL&#41;](../../ssma/oracle/selecting-and-configuring-affected-objects-oracletosql.md)  
  
## <a name="see-also"></a>Weitere Informationen  
[Testen von migrierten Datenbankobjekten &#40;oracleto SQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  
