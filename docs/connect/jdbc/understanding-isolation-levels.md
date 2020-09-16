---
description: Grundlegendes zu Isolationsstufen
title: Transaktionsisolationsstufen | Microsoft-Dokumentation
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 2c41e23a-da6c-4650-b5fc-b5fe53ba65c3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 758012112df2bce1b0a7624834bfde3c3c4ece10
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88488043"
---
# <a name="understanding-isolation-levels"></a>Grundlegendes zu Isolationsstufen

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Transaktionen geben eine Isolationsstufe an, mit der definiert wird, bis zu welchem Ausmaß eine Transaktion von Ressourcen- oder Datenänderungen isoliert sein muss, die von anderen Transaktionen durchgeführt werden. Die einzelnen Isolationsstufen werden dahin gehend beschrieben, welche Parallelitätsnebeneffekte (wie z. B. Dirty Reads oder Phantomlesezugriffe) zulässig sind.  
  
Transaktionsisolationsstufen steuern Folgendes:  
  
- Ob beim Lesen von Daten Sperren aktiviert werden können, und welcher Sperrentyp angefordert wird.  
  
- Wie lange die Lesesperren aufrechterhalten werden.  
  
- Ob ein Lesevorgang, der auf Zeilen verweist, die durch eine andere Transaktion geändert wurden:  
  
  - blockiert wird, bis die exklusive Sperre für die Zeile aufgehoben wird.  
  
  - die durch einen Commit bestätigte Version der Zeile abruft, die zum Zeitpunkt des Anweisungs- oder Transaktionsstarts vorhanden war.  
  
  - die Datenänderung liest, für die noch kein Commit ausgeführt wurde.  

Das Auswählen einer Transaktionsisolationsstufe hat keine Auswirkungen auf die Sperren, die zum Schutz von Datenänderungen eingerichtet werden. Eine Transaktion erhält immer eine exklusive Sperre für alle von ihr geänderten Daten und hält diese Sperre bis zum Abschluss der Transaktion aufrecht, und zwar unabhängig davon, welche Isolationsstufe für diese Transaktion festgelegt wurde. Für Lesevorgänge wird durch die Transaktionsisolationsstufen in erster Linie der Grad des Schutzes vor den Auswirkungen der Änderungen definiert, die durch andere Transaktionen vorgenommen werden.  
  
Eine niedrigere Isolationsstufe erhöht einerseits die Möglichkeit, dass viele Benutzer gleichzeitig auf Daten zugreifen können, führt aber auch dazu, dass die Benutzer mit einem Anstieg der Parallelitätseffekte (Dirty Reads oder verlorene Updates) rechnen müssen. Umgekehrt schränkt eine höhere Isolationsstufe zwar die Typen der Parallelitätseffekte ein, mit denen Benutzer rechnen müssen, gleichzeitig werden dafür aber mehr Systemressourcen beansprucht, und die Wahrscheinlichkeit steigt, dass sich die Transaktionen gegenseitig blockieren. So muss bei jeder Auswahl der geeigneten Isolationsstufe eine Abwägung zwischen den Datenintegritätsanforderungen der Anwendungen einerseits und dem mit jeder Isolationsstufe verbundenen Aufwand andererseits erfolgen. Die höchste Isolationsstufe (Serializable) garantiert, dass eine Transaktion jedes Mal, wenn sie einen Lesevorgang wiederholt, genau dieselben Daten liest. Dies wird jedoch durch ein Ausmaß an Sperren erreicht, das in Systemen mit mehreren Benutzern wahrscheinlich zu negativen Auswirkungen für andere Benutzer führt. Mit der niedrigsten Isolationsstufe (Read Uncommitted) können Daten abgerufen werden, die von anderen Transaktionen geändert wurden, für die jedoch noch kein Commit ausgeführt wurde. In der Isolationsstufe „Read Uncommitted“ können sämtliche denkbaren Parallelitätsnebeneffekte auftreten, dagegen werden keine Lesesperren und keine Versionsverwaltung verwendet, wodurch der Aufwand minimiert wird.  

## <a name="remarks"></a>Bemerkungen

 Die folgende Tabelle veranschaulicht, welche Parallelitätsnebeneffekte in den einzelnen Isolationsstufen zulässig sind.  
  
| Isolationsstufe  | Dirty Read | Non Repeatable Read | Phantom |
| ---------------- | ---------- | ------------------- | ------- |
| Read Uncommitted | Ja        | Ja                 | Ja     |
| Read Committed   | Nein         | Ja                 | Ja     |
| Repeatable Read  | Nein         | Nein                  | Ja     |
| Momentaufnahme         | Nein         | Nein                  | Nein      |
| Serialisierbar     | Nein         | Nein                  | Nein      |
  
Transaktionen müssen mindestens auf der Isolationsstufe „Repeatable Read“ ausgeführt werden, um den Verlust von Updates zu verhindern. Dies kann auftreten, wenn zwei Transaktionen jeweils die gleiche Zeile abrufen und sie später auf Grundlage der zunächst abgerufenen Werte aktualisieren. Wenn die beiden Transaktionen Zeilen mit einer einzigen UPDATE-Anweisung aktualisieren und die Aktualisierung nicht auf Grundlage der zuvor abgerufenen Werte erfolgt, können bei der Standardisolationsstufe „Read Committed“ keine verlorenen Aktualisierungen auftreten.  

Zum Festlegen der Isolationsstufe für eine Transaktion können Sie die [setTransactionIsolation](../../connect/jdbc/reference/settransactionisolation-method-sqlserverconnection.md)-Methode der [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md)-Klasse verwenden. Diese Methode nimmt einen **ganzzahligen** Wert als Argument an. Dieser basiert wie im Folgenden dargestellt auf den Verbindungskonstanten:  

```java
con.setTransactionIsolation(Connection.TRANSACTION_READ_COMMITTED);  
```

Zum Verwenden der neuen Snapshotisolationsstufe von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] können Sie eine der `SQLServerConnection`-Konstanten verwenden:  

```java
con.setTransactionIsolation(SQLServerConnection.TRANSACTION_SNAPSHOT);  
```

oder Sie können Folgendes verwenden:  

```java
con.setTransactionIsolation(Connection.TRANSACTION_READ_COMMITTED + 4094);  
```

Weitere Informationen zu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Isolationsstufen finden Sie unter „Isolationsstufen in [!INCLUDE[ssDE](../../includes/ssde_md.md)]“ in der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Onlinedokumentation.  

## <a name="see-also"></a>Weitere Informationen

[Ausführen von Transaktionen mit dem JDBC-Treiber](../../connect/jdbc/performing-transactions-with-the-jdbc-driver.md)  
