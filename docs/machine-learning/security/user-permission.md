---
title: Erteilen von Berechtigungen zum Ausführen von Python- und R-Skripts
description: In diesem Artikel erfahren Sie, wie Sie Benutzern die Berechtigung zum Ausführen externer Python- und R-Skripts in SQL Server Machine Learning Services erteilen und wie Sie Datenbanken Lese-, Schreib-oder DDL-Berechtigungen (Data Definition Language, Datendefinitionssprache) erteilen.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019, contperf-fy20q4
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 9214a27e0f52108f7e22ebf2108bfc06a3d6fbaf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340363"
---
# <a name="grant-users-permission-to-execute-python-and-r-scripts-with-sql-server-machine-learning-services"></a>Erteilen Sie Benutzern die Berechtigung zum Ausführen von Python- und R-Skripts mit SQL Server Machine Learning Services.
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

In diesem Artikel erfahren Sie, wie Sie Benutzern die Berechtigung zum Ausführen externer Python- und R-Skripts in [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) erteilen und wie Sie Datenbanken Lese-, Schreib-oder DDL-Berechtigungen (Data Definition Language, Datendefinitionssprache) erteilen.

Weitere Informationen finden Sie im Abschnitt „Berechtigungen“ unter [Sicherheitsübersicht für das Erweiterbarkeitsframework](../../machine-learning/concepts/security.md#permissions).

<a name="permissions-external-script"></a>

## <a name="permission-to-run-scripts"></a>Berechtigung zum Ausführen von Skripts

Jedem Benutzer, der Python- oder R-Skripts mit SQL Server Machine Learning Services ausführt und kein Administrator ist, müssen Sie die Berechtigung zum Ausführen externer Skripts in jeder Datenbank erteilen, in der die betreffende Sprache verwendet wird.

Führen Sie das folgende Skript aus, um die Berechtigung zum Ausführen externer Skripts zu erteilen:

```sql
USE <database_name>
GO
GRANT EXECUTE ANY EXTERNAL SCRIPT TO [UserName]
```

> [!NOTE]
> Berechtigungen sind nicht für die unterstützte Skriptsprache spezifisch. Es gibt also keine separaten Berechtigungsebenen für R-Skripts und Python-Skripts.

<a name="permissions-db"></a>

## <a name="grant-databases-permissions"></a>Erteilen von Datenbankberechtigungen

Wenn ein Benutzer Skripts ausführt, benötigt dieser möglicherweise Lesezugriff auf Daten in anderen Datenbanken. Möglicherweise muss der Benutzer auch neue Tabellen erstellen, um Ergebnisse zu speichern, und Daten in Tabellen schreiben.

Stellen Sie für alle Windows-Benutzerkonten und SQL-Anmeldungen, die R- oder Python-Skripts ausführen, sicher, dass diese über die richtigen Berechtigungen für die jeweilige Datenbank verfügen: 

+ `db_datareader` zum Lesen von Daten
+ `db_datawriter` zum Speichern von Objekten in der Datenbank
+ `db_ddladmin` zum Erstellen von Objekten wie gespeicherten Prozeduren oder Tabellen, die trainierte und serialisierte Daten enthalten

Die folgende [!INCLUDE[tsql](../../includes/tsql-md.md)]-Anweisung erteilt beispielsweise dem SQL-Konto *MySQLLogin* die Rechte zum Ausführen von T-SQL-Abfragen in der Datenbank *ML_Samples*. Um diese Anweisung auszuführen, muss die SQL-Anmeldung bereits im Sicherheitskontext des Servers vorhanden sein.

```sql
USE ML_Samples
GO
EXEC sp_addrolemember 'db_datareader', 'MySQLLogin'
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Berechtigungen der einzelnen Rollen finden Sie unter [Rollen auf Datenbankebene](../../relational-databases/security/authentication-access/database-level-roles.md).
