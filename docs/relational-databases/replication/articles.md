---
description: Artikel
title: Artikel | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.articles.f1
ms.assetid: 7c743dc6-6c6d-4c92-b711-842e1b0b273e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 96a8558b4a1c233ef94fdd47cae70309f1ce53ec
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467281"
---
# <a name="articles"></a>Artikel
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Auf der Seite **Artikel** können Sie angeben, welche Datenbankobjekte als Artikel in die Veröffentlichung eingeschlossen werden sollen. Wenn Sie ein Datenbankobjekt veröffentlichen, das von mindestens einem weiteren Datenbankobjekt abhängt, müssen Sie alle Objekte veröffentlichen, auf die verwiesen wird. Wenn Sie beispielsweise eine Sicht veröffentlichen, die von einer Tabelle abhängt, muss auch die Tabelle veröffentlicht werden.  
  
 Objekte, die nicht veröffentlicht werden können, sind neben dem Objekt durch ein rotes Symbol gekennzeichnet. Zusätzlich wird im Informationsbereich am unteren Rand der Assistentenseite eine Erläuterung angezeigt. Folgende Objekte können nicht veröffentlicht werden:  
  
-   Verschlüsselte Objekte.  
  
-   Tabellen ohne Primärschlüssel können nicht in Transaktionsveröffentlichungen veröffentlicht werden.  
  
-   Tabellen können nicht gleichzeitig in einer Mergeveröffentlichung und in einer Transaktionsveröffentlichung veröffentlicht werden, für die Abonnements mit verzögertem Update über eine Warteschlange zulässig sind. Weitere Informationen zum Veröffentlichen eines Artikels in mehreren Veröffentlichungen finden Sie im Abschnitt über das „Veröffentlichen von Tabellen in mehreren Veröffentlichungen“ unter [Veröffentlichen von Daten und Datenbankobjekten](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
## <a name="oracle-publishers"></a>Oracle-Verleger  
 Für Oracle-Verleger gelten zusätzliche Festlegungen:  
  
-   Eine Liste der Objekte, die aus Oracle veröffentlicht werden können, finden Sie unter [Design Considerations and Limitations for Oracle Publishers](../../relational-databases/replication/non-sql/design-considerations-and-limitations-for-oracle-publishers.md). Objekte, die nicht veröffentlicht werden können, werden nicht angezeigt.  
  
-   Eine Liste der Datentypen, die veröffentlicht werden können, finden Sie unter [Data Type Mapping for Oracle Publishers](../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md). Spalten mit Datentypen, die nicht veröffentlicht werden können, werden nicht angezeigt.  
  
## <a name="column-filters"></a>Spaltenfilter  
 Die Spalten auf dieser Seite können Sie filtern, indem Sie im Bereich **Zu veröffentlichende Objekte** eine Tabelle erweitern und dann nur die erforderlichen Spalten auswählen (Zeilen können auf der Seite **Tabellenzeilen filtern** dieses Assistenten gefiltert werden). Das Filtern von Spalten kann aus verschiedenen Gründen nützlich sein, darunter aus Sicherheitsgründen (um zu vermeiden, dass vertrauliche Daten ungewollt repliziert werden) oder aus Leistungsgründen (um beispielsweise die Replikation großer BLOB-Spalten zu vermeiden). Weitere Informationen zum Filtern von Spalten, einschließlich einer Liste von Spaltentypen, die nicht gefiltert werden können, finden Sie unter [Filtern von veröffentlichten Daten](../../relational-databases/replication/publish/filter-published-data.md).  
  
## <a name="options"></a>Tastatur  
 Im Bereich **Zu veröffentlichende Objekte** können Sie folgende Schritte ausführen:  
  
-   Anzeigen aller für die Replikation verfügbaren Objekte.  
  
-   Einschließen eines Objektes in eine Veröffentlichung durch Auswählen des Kontrollkästchens neben diesem Objekt.  
  
-   Einschließen aller Objekte eines bestimmten Typs (z. B. einer Tabelle) in die Veröffentlichung durch Aktivieren des Kontrollkästchens neben dem Objekttyp (z. B. **Tabellen**).  
  
-   Erweitern von Tabellenknoten zum Anzeigen der Spalten in der Tabelle.  
  
-   Filtern von Tabellenspalten aus einer Veröffentlichung durch Deaktivieren des Kontrollkästchens neben dieser Spalte.  
  
-   Aufrufen eines Kontextmenüs durch Klicken mit der rechten Maustaste auf ein Objekt.  
  
 **Artikeleigenschaften**  
 Klicken Sie auf **Artikeleigenschaften**, und führen Sie anschließend einen der folgenden Schritte aus:  
  
-   Klicken Sie auf **Eigenschaften des hervorgehobenen \<ObjectType>-Artikels festlegen**, um das Dialogfeld **Artikeleigenschaften – \<ObjectName>** zu öffnen. Die in diesem Dialogfeld vorgenommenen Änderungen werden nur auf das Objekt angewendet, das auf der Seite **Artikel** im Objektbereich markiert ist.  
  
-   Klicken Sie auf **Eigenschaften aller \<ObjectType>-Artikel festlegen**, um das Dialogfeld **Eigenschaften für alle \<ObjectType>-Artikel** zu starten. Die in diesem Dialogfeld vorgenommenen Änderungen werden auf alle Objekte dieses Typs angewendet, die auf der Seite **Artikel** im Objektbereich vorhanden sind, einschließlich Objekte, die noch nicht für die Veröffentlichung ausgewählt wurden.  
  
    > [!NOTE]  
    >  Eigenschaftenänderungen im Dialogfeld **Eigenschaften für alle \<ObjectType>-Artikel** überschreiben alle zuvor im Dialogfeld **Artikeleigenschaften - \<ObjectName>** vorgenommenen Änderungen. Wenn Sie beispielsweise sowohl mehrere Standardwerte für alle Artikel eines Objekttyps als auch bestimmte Eigenschaften für einzelne Objekte festlegen möchten, legen Sie zuerst die Standardwerte für alle Artikel fest. Legen Sie anschließend die Eigenschaften für die einzelnen Objekte fest.  
  
 **Die durch Hervorhebung markierte Tabelle ist nur herunterladbar**  
 Nur für Mergereplikationen zulässig. Nur in [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] und höheren Versionen. Durch Auswahl dieser Option geben Sie an, dass bei Verwendung eines Clientabonnements keine Änderungen auf dem Abonnenten zulässig sind. Da nur herunterladbare Artikel nicht auf dem Abonnenten aktualisiert werden können, wird das Nachverfolgen von Metadaten nicht an die Abonnenten gesendet. Das kann den Speicher auf den Abonnenten entlasten und zu einer höheren Leistung führen, besonders bei einer langsamen Netzwerkverbindung. Diese Option entspricht dem Wert **Nur herunterladbar auf Abonnenten, Abonnentenänderungen nicht zulassen** für die Option **Synchronisierungsrichtung** im Dialogfeld **Artikeleigenschaften** . Weitere Informationen finden Sie unter [Optimieren der Leistung der Mergereplikation durch nur herunterladbare Artikel](../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md).  
  
 **Nur in der Liste aktivierte Objekte anzeigen**  
 Aktivieren Sie dieses Kontrollkästchen, um nur die im Objektbereich ausgewählten Artikel anzuzeigen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Veröffentlichen von Daten und Datenbankobjekten](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Anzeigen und Ändern von Veröffentlichungseigenschaften](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)  
  
  
