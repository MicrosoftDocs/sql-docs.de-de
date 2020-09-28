---
title: Grundlegendes zu den Datentypen des JDBC-Treibers
description: Erfahren Sie mehr über JDBC-Datentypen und darüber, wie diese Typen vom Microsoft JDBC-Treiber für SQL Server in Datenbanktypen konvertiert werden.
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 7802328d-4d23-4775-9573-4169b127d258
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 48090db2e671fdfb1fb076645b06c9315b32bdd5
ms.sourcegitcommit: 620a868e623134ad6ced6728ce9d03d7d0038fe0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87410916"
---
# <a name="understanding-the-jdbc-driver-data-types"></a>Grundlegendes zu den Datentypen des JDBC-Treibers

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

[!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] unterstützt die Verwendung von einfachen und erweiterten JDBC-Datentypen in einer Java-Anwendung, die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] als Datenbank verwendet.  
  
Das JDBC-Typsystem vermittelt die Konvertierung zwischen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Datentypen und Java-Typen und -Objekten. Die JDBC-Typen beruhen auf den SQL-92- und SQL-99-Typen. Der JDBC-Treiber befolgt die JDBC-Spezifikation und stellt eine optimale Balance zwischen Vorhersehbarkeit und Flexibilität bereit.  
  
Die Themen in diesem Abschnitt beschreiben die Verwendung der grundlegenden und erweiterten Datentypen sowie das Konvertieren der Datentypen in andere Datentypen.  
  
## <a name="in-this-section"></a>In diesem Abschnitt  
  
| Thema                                                                                                                                            | BESCHREIBUNG                                                                                                                                                                                                                                                          |
| ------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Verwenden von Standarddatentypen](../../connect/jdbc/using-basic-data-types.md)                                                                           | Beschreibt die grundlegenden JDBC-Datentypen. Umfasst Beispiele zum Arbeiten mit den Datentypen mithilfe von Resultsets, parametrisierten Abfragen und gespeicherten Prozeduren.                                                                                                        |
| [Konfigurieren der Art und Weise, wie java.sql.Time-Werte an den Server gesendet werden](../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md) | Beschreibt die Generierung von Datumsangaben durch den JDBC-Treiber.                                                                                                                                                                                                                       |
| [Verwenden von erweiterten Datentypen](../../connect/jdbc/using-advanced-data-types.md)                                                                     | Beschreibt die erweiterten JDBC-Datentypen.                                                                                                                                                                                                                              |
| [Grundlegendes zu den Unterschieden von Datentypen](../../connect/jdbc/understanding-data-type-differences.md)                                                 | Beschreibt Unterschiede zwischen den verschiedenen Datentypen des JDBC-Treibers.                                                                                                                                                                                                    |
| [Grundlegendes zu Datentypkonvertierungen](../../connect/jdbc/understanding-data-type-conversions.md)                                                 | Beschreibt, wie die Datentypkonvertierung bei der Verwendung von Methoden zum Festlegen und Abrufen behandelt wird.                                                                                                                                                                                  |
| [Unterstützung für nationale Zeichensätze](../../connect/jdbc/national-character-set-support.md)                                                           | Beschreibt die Unterstützung der Typen für nationale Zeichensätze.                                                                                                                                                                                                          |
| [Unterstützen von XML-Daten](../../connect/jdbc/supporting-xml-data.md)                                                                                 | Beschreibt die SQLXML-Schnittstelle. Beschreibt außerdem, wie Sie XML-Daten mit dem **SQLXML**-Java-Datentyp in bzw. aus einer relationalen Datenbank schreiben und lesen.                                                                                                             |
| [Wrapper und Schnittstellen](../../connect/jdbc/wrappers-and-interfaces.md)                                                                         | Behandelt die Schnittstellen, die über die [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]-spezifischen Methoden und Konstanten verfügen und es einem Anwendungsserver ermöglichen, einen Proxy der Klasse zu erstellen. Behandelt außerdem Unterstützungen für die `java.sql.Wrapper`-Schnittstelle. |
  
## <a name="see-also"></a>Weitere Informationen

[Übersicht über den JDBC-Treiber](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
