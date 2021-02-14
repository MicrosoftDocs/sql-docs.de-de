---
description: Herstellen einer Verbindung mit Azure SQL-Datenbank (mysqlto SQL)
title: Herstellen einer Verbindung mit Azure SQL-Datenbank (mysqlto SQL) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connecting to Azure SQL Database, SQL Azure permissions
- Connecting to Azure SQL Database, synchronization
ms.assetid: d0b6f16a-1880-459d-a0c7-28b7ef15c56a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: ad03f4f1dda77ddae21138af8581dfba4c8f8bf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068936"
---
# <a name="connecting-to-azure-sql-database-mysqltosql"></a>Herstellen einer Verbindung mit Azure SQL-Datenbank (mysqlto SQL)

Um MySQL-Datenbanken zu zu migrieren [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , müssen Sie eine Verbindung mit der-Ziel Instanz von herstellen [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] . Wenn Sie eine Verbindung herstellen, erhält SSMA Metadaten zu allen Datenbanken in der Instanz von [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] und zeigt Daten Bank Metadaten im **metadatenexplorer von Azure SQL-Datenbank** an. SSMA speichert Informationen der Instanz von [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , mit der Sie verbunden sind, speichert aber keine Kenn Wörter.

Die Verbindung mit [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] bleibt aktiv, bis Sie das Projekt schließen. Wenn Sie das Projekt erneut öffnen, müssen Sie erneut eine Verbindung mit herstellen, [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] Wenn Sie eine aktive Verbindung mit dem Server herstellen möchten. Sie können offline arbeiten, bis Sie Datenbankobjekte in Laden [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] und Daten migrieren.

Metadaten über die Instanz von werden [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] nicht automatisch synchronisiert. Stattdessen müssen Sie die Metadaten manuell aktualisieren, um die Metadaten im **metadatenexplorer von Azure SQL-Datenbank** zu aktualisieren [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] . Weitere Informationen finden Sie im Abschnitt "Synchronisieren von Metadaten von Azure SQL-Datenbank" weiter unten in diesem Thema.

## <a name="required-azure-sql-database-permissions"></a>Erforderliche Azure SQL-Daten Bank Berechtigungen

Das Konto, das verwendet wird, um eine Verbindung mit herzustellen, [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] erfordert abhängig von den Aktionen, die das Konto ausführt, unterschiedliche Berechtigungen:

- [!INCLUDE[tsql](../../includes/tsql-md.md)]Das Konto muss über die Berechtigung verfügen, sich bei der Instanz von anzumelden, um MySQL-Objekte in die-Syntax zu konvertieren, um Metadaten aus zu aktualisieren [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] oder um konvertierte Syntax in Skripts zu speichern [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

- [!INCLUDE[ssAzure](../../includes/ssazure_md.md)]Das Konto muss ein Mitglied der **db_ddladmin** Daten Bank Rolle sein, um Datenbankobjekte in laden zu können.

- Zum Migrieren von Daten zu [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] muss das Konto Mitglied der **db_owner** Daten Bank Rolle sein.

## <a name="establishing-an-azure-sql-database-connection"></a>Einrichten einer Azure SQL-Datenbankverbindung

Bevor Sie MySQL-Datenbankobjekte in [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] Syntax konvertieren, müssen Sie eine Verbindung mit der Instanz von herstellen, [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] in der Sie die MySQL-Datenbank oder die Datenbanken migrieren möchten.

Wenn Sie die Verbindungs Eigenschaften definieren, geben Sie auch die Datenbank an, in die Objekte und Daten migriert werden. Sie können diese Zuordnung auf der MySQL-Schema Ebene anpassen, nachdem Sie eine Verbindung mit hergestellt haben [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] . Weitere Informationen finden Sie unter [Mapping MySQL-Datenbanken zu SQL Server Schemas &#40;mysqldesql&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md).

> [!IMPORTANT]
> Bevor Sie versuchen, eine Verbindung mit herzustellen [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] , stellen Sie sicher, dass Ihre IP-Adresse über die Firewall zugelassen wird [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

So stellen Sie eine Verbindung mit dem [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] her

1. Wählen Sie im Menü **Datei** die Option **mit Azure SQL-Datenbank verbinden** aus. (diese Option wird nach der Projekt Erstellung aktiviert.)
   Wenn Sie zuvor eine Verbindung mit [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] hergestellt haben, wird der Befehls Name **erneut mit der Azure SQL-Datenbank** verbunden.

2. Geben Sie im Dialogfeld Verbindung den Servernamen ein, oder wählen Sie ihn aus [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

3. Geben Sie den Datenbanknamen ein, oder **Suchen** Sie ihn.

4. Geben Sie **username** ein, oder wählen Sie

5. Geben Sie das **Kennwort ein**.

6. SSMA empfiehlt eine verschlüsselte Verbindung mit [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

7. Klicken Sie auf **Verbinden**.
  
## <a name="synchronizing-azure-sql-database-metadata"></a>Synchronisieren von Metadaten von Azure SQL-Datenbank

Metadaten zu Datenbanken in werden [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] nicht automatisch aktualisiert. Bei den Metadaten im **metadatenexplorer von Azure SQL-Datenbank** handelt es sich um eine Momentaufnahme der Metadaten, wenn Sie zum ersten Mal eine Verbindung mit hergestellt [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] haben oder wenn Sie die Metadaten zuletzt manuell aktualisiert Sie können Metadaten für alle Datenbanken oder für eine einzelne Datenbank oder ein Datenbankobjekt manuell aktualisieren. So synchronisieren Sie Metadaten:

1. Stellen Sie sicher, dass Sie mit verbunden sind [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] .

2. Aktivieren Sie im **metadatenexplorer von Azure SQL-Datenbank** das Kontrollkästchen neben der Datenbank oder dem Datenbankschema, das Sie aktualisieren möchten.
   Wenn Sie z. b. die Metadaten für alle Datenbanken aktualisieren möchten, aktivieren Sie das Kontrollkästchen neben **Datenbanken**.

3. Klicken Sie mit der rechten Maustaste auf **Datenbanken** oder die einzelne Datenbank oder das Datenbankschema, und wählen Sie dann **mit Datenbank synchronisieren** aus.

## <a name="next-step"></a>Nächster Schritt

Der nächste Schritt der Migration hängt von Ihren Projektanforderungen ab:

- Informationen zum Anpassen der Zuordnung zwischen MySQL-Schemas und finden Sie unter [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] [Mapping von MySQL-Datenbanken zu SQL Server Schemas &#40;mysqlto SQL&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md).
- Informationen zum Anpassen der Konfigurationsoptionen für die Projekte finden Sie unter [Festlegen von Projektoptionen &#40;mysqlto SQL&#41;](../../ssma/mysql/setting-project-options-mysqltosql.md).
- Informationen zum Anpassen der Zuordnung von Quell-und Ziel Datentypen finden Sie unter [Mapping MySQL and SQL Server Data Types &#40;mysqltosql&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md).
- Wenn Sie diese Aufgaben nicht ausführen müssen, können Sie die MySQL-Datenbankobjekt Definitionen in [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] Objekt Definitionen konvertieren. Weitere Informationen finden Sie unter [umstellen von MySQL-Datenbanken &#40;mysqldesql&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md).

## <a name="see-also"></a>Weitere Informationen

[Migrieren von MySQL-Datenbanken zu SQL Server Azure SQL-Datenbank &#40;mysqlto SQL&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)
