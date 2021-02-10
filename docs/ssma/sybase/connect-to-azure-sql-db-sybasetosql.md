---
description: Herstellen einer Verbindung mit Azure SQL-Datenbank (sybaseto SQL)
title: Herstellen einer Verbindung mit Azure SQL-Datenbank (sybaseto SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 96538007-1099-40c8-9902-edd07c5620ee
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 93d1901cd5701cbdea537b947aa93de64fe59661
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056715"
---
# <a name="connect-to-azure-sql-database--sybasetosql"></a>Herstellen einer Verbindung mit Azure SQL-Datenbank (sybaseto SQL)
Verwenden Sie das Dialogfeld Verbindung mit Azure SQL-Datenbank herstellen, um eine Verbindung mit der Azure SQL-DatenbankDatenbank herzustellen, die Sie migrieren möchten.  
  
Um auf dieses Dialogfeld zuzugreifen, wählen Sie im Menü **Datei** die Option **mit Azure SQL-Datenbank verbinden** aus. Wenn Sie bereits eine Verbindung hergestellt haben, stellt der Befehl **erneut eine Verbindung mit der Azure SQL-Datenbank her.**  
  
## <a name="options"></a>Tastatur  
**Servername**  
  
Geben Sie den Server Namen für die Verbindung mit der Azure SQL-Datenbank ein  
  
**Datenbank**  
  
Wählen **Sie den Daten** Banknamen aus, oder geben Sie ihn ein.  
  
> [!IMPORTANT]  
> SSMA für Sybase unterstützt keine Verbindung mit der Master-Datenbank in Azure SQL-Datenbank.  
  
**Benutzername**  
  
Geben Sie den Benutzernamen ein, den SSMA zum Herstellen einer Verbindung mit der Azure SQL-DatenbankDatenbank verwendet.  
  
**Kennwort**  
  
Geben Sie das Kennwort für den Benutzernamen ein.  
  
**Encrypt**  
  
SSMA empfiehlt eine verschlüsselte Verbindung mit Azure SQL-Datenbank.  
  
## <a name="create-azure-database"></a>Azure-Datenbank erstellen  
Wenn sich keine Datenbanken im Azure SQL-Daten Bankkonto befinden, können Sie die erste Datenbank erstellen.  
  
Führen Sie die folgenden Schritte aus, um eine neue Datenbank zum ersten Mal zu erstellen:  
  
1.  Klicken Sie auf die Schaltfläche Durchsuchen, die im Dialogfeld Verbindung mit Azure SQL-Datenbank herstellen vorhanden ist.  
  
2.  Wenn keine Datenbanken vorhanden sind, werden die folgenden zwei Menü Elemente angezeigt.  
  
    1.  **(keine Datenbanken gefunden)** , die deaktiviert und ständig ausgegraut sind  
  
    2.  **Erstellen Sie eine neue Datenbank** , die nur aktiviert ist, wenn keine Datenbanken für das Azure SQL-Daten Bankkonto vorhanden sind. Wenn Sie auf dieses Menü Element klicken, ist das Dialogfeld Azure-Datenbank erstellen mit Datenbankname und-Größe vorhanden.  
  
3.  Zum Zeitpunkt der Erstellung der Datenbank werden die folgenden beiden Parameter als Eingabe angegeben:  
  
    1.  **Datenbankname:** Geben Sie den Datenbanknamen ein.  
  
    2.  **Datenbankgröße:** Wählen Sie die Datenbankgröße aus, die Sie im Azure SQL-Daten Bankkonto erstellen müssen.  
  
