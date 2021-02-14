---
description: Anzeigen von Testfallberichten (SybaseToSQL)
title: Anzeigen von Test Fall berichten (sybaseto SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester Component,Test Case Reports
ms.assetid: cb75d281-43ef-4f4a-b754-2c4ee3b62ae7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 833b1e862aa5629a9a6cbea5854e3a8db5a5cab2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080711"
---
# <a name="viewing-test-case-reports-sybasetosql"></a>Anzeigen von Testfallberichten (SybaseToSQL)
Der Bericht "Test Fall" zeigt die Test Überprüfungs Ergebnisse und allgemeine Test Informationen an. Bei einem Test Fehler werden auch Informationen zu nicht übereinstimmenden Daten in verifizierten Objekten angezeigt.  
  
## <a name="report-structure"></a>Berichtsstruktur  
Im oberen Bereich des Berichts werden diese Statistiken angezeigt:  
  
-   Die Gesamtanzahl der getesteten Objekte und die Anzahl von Objekten, für die der Test erfolgreich war.  
  
-   Die Gesamtanzahl der überprüften Tabellen und Fremdschlüssel und die Anzahl der erfolgreich abgeglichen Tabellen und Fremdschlüssel.  
  
-   Die Startzeit, die Endzeit des Testfalls und die Gesamtzeit für die Ausführung.  
  
Im restlichen Bericht werden Informationen in vier Kategorien angezeigt:  
  
**Voraussetzungs Fehler**  
Zeigt alle Fehler an, die im Schritt " **Voraussetzungen** " aufgetreten sind. Normalerweise wird Sie übersprungen.  
  
**Initialisierung**  
Zeigt den Status der Ausführung als **Erfolg** oder **Fehler** an.  
  
**Test Objekte Ergebnis**  
Ein Vergleich der Ergebnisse (Erfolg oder Fehler) und der Konflikte, die der SSMA-Tester bei einem Fehler erkannt hat.  
  
**Abschluss**  
Zeigt den Status der Ausführung als **Erfolg** oder **Fehler** an.  
  
## <a name="see-also"></a>Weitere Informationen  
[Ausführen von Test Fällen &#40;sybaseto SQL-&#41;](../../ssma/sybase/running-test-cases-sybasetosql.md)  
[Testen von migrierten Datenbankobjekten &#40;sybaseto SQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
