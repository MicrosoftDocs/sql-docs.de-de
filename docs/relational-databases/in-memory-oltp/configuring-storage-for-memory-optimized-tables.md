---
title: Konfigurieren von Speicher für speicheroptimierte Tabellen | Microsoft Dokumentation
description: Erfahren Sie, wie Sie Speicherkapazität und E/A-Vorgänge pro Sekunde (IOPS) für speicheroptimierte Tabellen in SQL Server konfigurieren.
ms.custom: ''
ms.date: 1/15/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 6e005de0-3a77-4b91-b497-14cc0f9f6605
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 892ce217acbc07bdd98ca7166aeb0a9211b2adea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350671"
---
# <a name="configuring-storage-for-memory-optimized-tables"></a>Konfigurieren von Speicher für speicheroptimierte Tabellen
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Sie müssen die Speicherkapazität und die E/A-Vorgänge pro Sekunde (IOPS) konfigurieren.  
  
## <a name="storage-capacity"></a>Speicherkapazität  

Verwenden Sie die Informationen unter [Schätzen der Arbeitsspeicheranforderungen speicheroptimierter Tabellen](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md) , um für die dauerhaften speicheroptimierten Tabellen der Datenbank deren Größe im Arbeitsspeicher zu schätzen. Da die Indizes der speicheroptimierten Tabellen nicht beibehalten werden, muss die Größe der Indizes nicht berücksichtigt werden. 
 
Nachdem Sie die Größe bestimmt haben, müssen Sie eine ausreichende Menge an Speicherplatz für die Prüfpunktdateien bereitstellen, die zum Speichern neu geänderter Daten verwendet werden. Die gespeicherten Daten enthalten nicht nur die Inhalte neuer Zeilen, die den speicheroptimierten Tabellen hinzugefügt werden, sondern auch neue Versionen vorhandener Zeilen. Dieser Speicher wächst an, wenn Zeilen eingefügt oder aktualisiert werden. Zeilenversionen werden zusammengeführt, und Speicher wird freigegeben, wenn eine Protokollkürzung erfolgt. Falls sich die Protokollkürzung aus irgendeinem Grund verzögert, wächst der In-Memory-OLTP-Speicher an.

Ein guter Ausgangspunkt zur Dimensionierung des Speichers für diesen Bereich ist die vierfache Größe der dauerhaften In-Memory-Tabellen. Überwachen Sie die Speichernutzung, und bereiten Sie sich darauf vor, den verfügbaren Speicher bei Bedarf zu erweitern.
  
## <a name="storage-iops"></a>Speicher-IOPS  
 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] kann den Arbeitsauslastungsdurchsatz erheblich erhöhen. Daher ist es wichtig sicherzustellen, dass die E/A-Vorgänge keinen Engpass darstellen.  
  
-   Wenn Sie datenträgerbasierte Tabellen zu speicheroptimierten Tabellen migrieren, stellen Sie sicher, dass sich das Transaktionsprotokoll auf einem Speichermedium befindet, das die erhöhte Transaktionsprotokollaktivität verarbeiten kann. Wenn das Speichermedium z. B. Transaktionsprotokollvorgänge bei 100 MB/s unterstützt und durch speicheroptimierte Tabellen eine fünfmal höhere Leistung erzielt wird, muss das Speichermedium des Transaktionsprotokolls in der Lage sein, die fünfmal höhere Leistung auch zu unterstützen, damit die Transaktionsprotokollaktivitäten nicht zu einem Leistungsengpass führen.  
  
-   Speicheroptimierte Tabellen werden in Prüfpunktdateien beibehalten, die über einen oder mehrere Container verteilt sind. Jeder Container sollte in der Regel einem eigenen Speichergerät zugeordnet sein. Er wird sowohl für eine höhere Speicherkapazität als auch für einen höheren IOPS-Wert verwendet. Sie müssen sicherstellen, dass durch sequenziellen IOPS des Speichermediums der dreifache Wert des Transaktionsprotokolldurchsatzes unterstützt werden kann. Schreibvorgänge in Prüfpunktdateien entsprechen 256 KB für Datendateien und 4 KB für Änderungsdateien.
  
     - Wenn durch speicheroptimierte Tabellen beispielsweise fortlaufend Aktivitäten mit 500 MB/s im Transaktionsprotokoll generiert werden, muss der Speicher für speicheroptimierte Tabellen einen IOPS-Wert von 1,5 GB/s unterstützen. Die Notwendigkeit zur Unterstützung des dreifachen dauerhaften Transaktionsprotokolldurchsatzes ergibt sich daraus, dass zuerst die Daten-/Änderungsdateipaare mit den anfänglichen Daten geschrieben und diese anschließend als Teil eines Zusammenführungsvorgangs gelesen und erneut geschrieben werden müssen.  
  
- Ein weiterer Faktor bei der Schätzung des IOPS-Werts für den Speicher ist die Wiederherstellungszeit für speicheroptimierte Tabellen. Daten aus dauerhaften Tabellen müssen in den Speicher gelesen werden, bevor die Datenbank für Anwendungen zur Verfügung steht. Normalerweise können Daten mit dem maximalen IOPS-Wert in speicheroptimierte Tabellen geladen werden. Wenn der gesamte Speicherplatz für dauerhafte speicheroptimierte Tabellen 60 GB beträgt und Sie diese Daten innerhalb von 1 Minute laden möchten, muss der Speicher IOPS mit 1 GB/s unterstützen.  
  
-   Prüfpunktdateien werden in der Regel gleichmäßig in allen Containern verteilt, wenn genügend Speicherplatz vorhanden ist. Bei SQL Server 2014 müssen Sie eine ungerade Anzahl von Containern bereitstellen, um eine gleichmäßige Verteilung zu erzielen. Ab 2016 kann eine gleichmäßige Verteilung durch eine ungerade oder gerade Anzahl von Containern erzielt werden.
  
## <a name="encryption"></a>Verschlüsselung  
 Ab [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] wird der Speicher für speicheroptimierte Tabellen im Ruhezustand verschlüsselt, um TDE (Transparent Data Encryption) für die Datenbank zu ermöglichen. Weitere Informationen finden Sie unter [TDE (Transparent Data Encryption)](../../relational-databases/security/encryption/transparent-data-encryption.md). In [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] werden Prüfpunktdateien nicht verschlüsselt, auch wenn TDE für die Datenbank aktiviert ist.

 Daten in [nicht dauerhaften](../../relational-databases/in-memory-oltp/defining-durability-for-memory-optimized-objects.md) speicheroptimierten Tabellen (SCHEMA_ONLY) werden nie auf den Datenträger geschrieben. Daher gilt TDE nicht für solche Tabellen.
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen und Verwalten von Speicher für speicheroptimierte Objekte](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)  
  
  
