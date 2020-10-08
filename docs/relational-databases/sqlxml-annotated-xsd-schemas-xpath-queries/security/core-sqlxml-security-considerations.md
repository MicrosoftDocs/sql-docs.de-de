---
title: Wichtige Sicherheitsüberlegungen zu SQLXML
description: Erfahren Sie mehr über die wichtigsten Sicherheitsrichtlinien für die Verwendung von SQLXML für den Datenzugriff.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- security [SQLXML], about security
ms.assetid: 330cd2ff-d5d5-4c8e-8f93-0869c977be94
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9841e46f97248413f142886193d9c2afedbe4e04
ms.sourcegitcommit: 04cf7905fa32e0a9a44575a6f9641d9a2e5ac0f8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2020
ms.locfileid: "91809792"
---
# <a name="core-sqlxml-security-considerations"></a>Wichtige Sicherheitsüberlegungen zu SQLXML
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Im Folgenden werden Sicherheitsrichtlinien zur Verwendung von SQLXML für den Datenzugriff erläutert.  
  
-   Der SQLXMLOLEDB-Anbieter macht eine **StreamFlags** -Eigenschaft verfügbar, die es Ihnen ermöglicht, Flags festzulegen, die angeben, welche SQLXML-Funktionen für die jeweilige Instanz aktiviert oder deaktiviert werden sollen. Anhand dieser Eigenschaft können Sie die Verwendung von SQLXML anpassen und sicherstellen, dass nur die gewünschten Komponenten aktiviert werden. Weitere Informationen finden Sie unter [SQLXMLOLEDB-Anbieter &#40;SQLXML 4,0&#41;](../data-access-components-provider/sqlxml-4-0-data-access-components-sqlxmloledb-provider.md).  
  
-   Wenn SQLXML-Fehler auftreten und zurückgegeben werden, können sie Informationen über das Datenbankschema wie Tabellennamen, Spaltennamen und Typ umfassen. Gehen Sie bei der Behandlung dieser Fehler vorsichtig vor, sodass Informationen über Ihre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Installation von anderen Benutzern nicht einfach ermittelt werden können, wenn keine Absicht oder Notwendigkeit dazu besteht.  
  
-   Wird SQLXML entweder zum Abfragen oder zum Senden von Updates an [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] verwendet, werden keine Einschränkungen in Bezug auf die Datenmenge, die ausgetauscht werden kann, festgelegt, und die Datenmenge in einer SQLXML-Nutzlast wird vor dem Verarbeitungsversuch nicht überprüft. Wenn Sie Ihre Anwendung mit SQLXML entwickeln, liegt es in Ihrem Verantwortungsbereich sicherzustellen, dass genügend Systemspeicher zum Verarbeiten der Daten vorhanden ist. Beim Abfragen von Daten vom Server sollten Sie beispielsweise überprüfen, ob genügend Speicherplatz auf dem Client vorhanden ist, um die abgefragten Daten zu empfangen. Ebenso müssen Sie beim Laden von Daten auf den Server sicherstellen, dass genügend Speicherplatz auf dem Server zur Verarbeitung der Daten und genügend Festplattenspeicher auf dem Server zum Speichern der Daten zur Verfügung steht.  
  
-   SQLXML generiert [!INCLUDE[tsql](../../../includes/tsql-md.md)]-Abfragen dynamisch und aktualisiert Befehle und sendet sie zur Ausführung an[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Dies ist die einzige Methode, bei der SQLXML den Server abfragt und aktualisiert. Ergebnisse werden entweder als Datenstrom (von XML) oder als Rowset empfangen.  
  
-   Beim Empfang von Abfrageergebnissen führt SQLXML basierend auf dem Inhalt der empfangenen Daten keine Aktionen aus. Es erfolgt keine zusätzliche Verarbeitung auf Grundlage des Typs oder des Inhalts der Daten. Die Daten werden nie als Code behandelt, mit dem Aktionen ausgeführt werden.  
  
-   Beim Ausführen von XML-Vorlagen übersetzt SQLXML die XPath- und DBObject-Abfragen, die in der übermittelten Vorlage enthalten sind, in [!INCLUDE[tsql](../../../includes/tsql-md.md)]-Befehle, die dann für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ausgeführt werden. Diese Befehle beeinflussen nur vorhandene Daten. Von SQLXML generierte Befehle ändern nie die Struktur der Datenbank. Benutzer müssen explizite Befehle ausgeben, um die Datenbankstruktur zu ändern. Dies kann z. b. durch einschließen in einen **SQL: Query** -Block einer Vorlage sein.  
  
-   Beim Ausführen von DBObject-Abfragen und XPath-Anweisungen über Zuordnungsdateien lässt SQLXML die Daten in der Datenbank unverändert.  
  
-   SQLXML kann Formatierungsänderungen für die angegebenen Daten basierend auf den Unterschieden zwischen dem XML- und dem [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Datenmodell vornehmen. Zum Beispiel ist das Format für die Zeitangabe unterschiedlich. SQLXML versucht, diese Unterschiede aufzulösen. Als Folge können Genauigkeitsinformationen verloren gehen.  
  
-   SQLXML legt keine Beschränkungen für die Dauer der Verarbeitung der Daten fest. Die Verarbeitung wird fortgesetzt, bis ein Fehler auftritt oder die Verarbeitung abgeschlossen ist.  
  
-   SQLXML schreibt nicht ins Dateisystem. Wenn Benutzer Daten, die sie aus der Datenbank abrufen, speichern möchten, müssen sie dazu ihren Code verwenden.  
  
-   SQLXML ermöglicht es Benutzern, eine beliebige SQL-Abfrage für die Datenbank auszuführen. Diese Funktion sollte nie einer unsicheren oder unkontrollierten Quelle verfügbar gemacht werden, da es im Grunde eine Öffnung der SQL-Datenbank ohne Bereitstellung für die Benutzer bedeutet.  
  
-   Beim Ausführen von Update grams übersetzt SQLXML die **updg: Sync** -Blöcke in DELETE-, Update-und INSERT-Befehle für die- [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Instanz. Diese Befehle beeinflussen nur vorhandene Daten. Von SQLXML generierte Befehle ändern nie die Datenbank. Benutzer müssen explizite Befehle ausgeben, um die Datenbankstruktur zu ändern. Dies kann z. b. durch einschließen in einen **SQL: Query** -Block einer Vorlage sein.  
  
-   Beim Ausführen von DiffGrams übersetzt SQLXML das DiffGram in DELETE-, UPDATE- und INSERT-Befehle für die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Instanz. Diese Befehle beeinflussen nur vorhandene Daten. Von SQLXML generierte Befehle ändern nie die Datenbank. Benutzer müssen explizite Befehle ausgeben, um die Datenbankstruktur zu ändern. Dies kann z. b. durch einschließen in einen **SQL: Query** -Block einer Vorlage sein.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Sicherheitsüberlegungen zu SQLXML 4.0](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/sqlxml-4-0-security-considerations.md)  
  
