---
title: Festlegen und Ändern von parametrisierten Zeilenfiltern (Merge)
description: Erfahren Sie, wie Sie einen parametrisierten Zeilenfilter für einen Mergeveröffentlichungsartikel für SQL Server definieren und ändern.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- parameterized filters [SQL Server replication], defining
- parameterized filters [SQL Server replication], modifying
- merge replication [SQL Server replication], dynamic filters
- merge replication parameterized filters [SQL Server replication]
- filters [SQL Server replication], parameterized
- modifying filters, parameterized row
- dynamic filters [SQL Server replication]
ms.assetid: de0482a2-3cc8-4030-8a4a-14364549ac9f
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 40d9ecf88b81263d6e35b5ade9ee6d0f3adeef49
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076738"
---
# <a name="define-and-modify-a-parameterized-row-filter-for-a-merge-article"></a>Definieren und Ändern eines parametrisierten Zeilenfilters für einen Mergeartikel
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  In diesem Thema wird beschrieben, wie in [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mit [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] oder [!INCLUDE[tsql](../../../includes/tsql-md.md)]parametrisierte Zeilenfilter definiert und geändert werden.  
  
 Zum Erstellen von Tabellenartikeln können Sie parametrisierte Zeilenfilter verwenden. In diesen Filtern werden mit einer [WHERE-Klausel](../../../t-sql/queries/where-transact-sql.md) die zu veröffentlichenden Daten ausgewählt. Statt in der Klausel einen Literalwert anzugeben (wie dies bei statischen Zeilenfiltern der Fall wäre), wird mindestens eine der folgenden Systemfunktionen angegeben: [SUSER_SNAME](../../../t-sql/functions/suser-sname-transact-sql.md) und [HOST_NAME](../../../t-sql/functions/host-name-transact-sql.md). Weitere Informationen zu parametrisierten Zeilenfiltern finden Sie unter [Parametrisierte Zeilenfilter](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 **In diesem Thema**  
  
-   **Vorbereitungen:**  
  
     [Einschränkungen](#Restrictions)  
  
-   **So definieren oder ändern Sie einen parametrisierten Zeilenfilter mit:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Vorbereitungen  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Einschränkungen  
  
-   Wenn Sie einen parametrisierten Zeilenfilter hinzufügen, ändern oder löschen, nachdem Abonnements für die Veröffentlichung initialisiert wurden, müssen Sie eine neue Momentaufnahme generieren und alle Abonnements nach vorgenommener Änderung erneut initialisieren. Weitere Informationen zum Ändern von Eigenschaften finden Sie unter [Ändern von Veröffentlichungs- und Artikeleigenschaften](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Empfehlungen  
  
-   Aus Leistungsgründen wird empfohlen, keine Funktionen auf Spaltennamen in parametrisierten Zeilenfilterklauseln (beispielsweise `LEFT([MyColumn]) = SUSER_SNAME()`) anzuwenden. Wenn Sie HOST_NAME in einer Filterklausel verwenden und den HOST_NAME-Wert überschreiben, müssen Datentypen eventuell mit CONVERT konvertiert werden. Weitere Informationen zu bewährten Methoden für diesen Fall finden Sie im Abschnitt über das Überschreiben des HOST_NAME()-Werts im Thema [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Verwenden von SQL Server Management Studio  
 Definieren, ändern und löschen Sie parametrisierte Zeilenfilter auf der Seite **Tabellenzeilen filtern** des Assistenten für neue Veröffentlichung oder auf der Seite **Filterzeilen** des Dialogfelds **Veröffentlichungseigenschaften - \<Publication>** . Weitere Informationen zum Verwenden des Assistenten sowie Zugriff auf das Dialogfeld finden Sie unter [Erstellen einer Veröffentlichung](../../../relational-databases/replication/publish/create-a-publication.md) und [Anzeigen und Ändern von Veröffentlichungseigenschaften](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-define-a-parameterized-row-filter"></a>So definieren Sie einen parametrisierten Zeilenfilter  
  
1.  Klicken Sie auf der Seite **Tabellenzeilen filtern** des Assistenten für neue Veröffentlichung oder auf der Seite **Filterzeilen** der **Veröffentlichungseigenschaften – \<Publication>** auf **Hinzufügen** und dann auf **Filter hinzufügen**.  
  
2.  Wählen Sie in der Dropdownliste im Dialogfeld **Filter hinzufügen** die zu filternde Tabelle aus.  
  
3.  Erstellen Sie im Textfeld **Filteranweisung** eine Filteranweisung. Sie können den Text direkt in den Textbereich eingeben, und Sie können Spalten auch mit Drag und Drop aus dem Listenfeld **Spalten** einfügen.  
  
    -   Der Textbereich **Filteranweisung** enthält den Standardtext im folgenden Format:  
  
        ```  
        SELECT <published_columns> FROM [tableowner].[tablename] WHERE  
        ```  
  
    -   Der Standardtext kann nicht geändert werden. Geben Sie mithilfe der SQL-Standardsyntax im Anschluss an das WHERE-Schlüsselwort die Filterklausel ein. Parametrisierte Filter enthalten einen Aufruf der **HOST_NAME()** - und/oder **SUSER_SNAME()** -Systemfunktion bzw. einer benutzerdefinierten Funktion, die auf eine oder beide dieser Funktionen verweist. Eine vollständige Filterklausel für einen parametrisierten Zeilenfilter kann z. B. wie folgt aussehen:  
  
        ```  
        SELECT <published_columns> FROM [HumanResources].[Employee] WHERE LoginID = SUSER_SNAME()  
        ```  
  
         Verwenden Sie einen zweiteiligen Namen für die WHERE-Klausel, drei- oder vierteilige Namen werden nicht unterstützt.  
  
4.  Wählen Sie die Option aus, mit der angegeben wird, wie Daten für mehrere Abonnenten freigegeben werden:  
  
    -   **Eine Zeile aus dieser Tabelle wird an mehrere Abonnements gesendet**  
  
    -   **Eine Zeile aus dieser Tabelle wird nur an ein Abonnement gesendet**  
  
     Wenn Sie **Eine Zeile aus dieser Tabelle wird nur an ein Abonnement gesendet** auswählen, kann die Mergereplikation die Leistung optimieren, da weniger Metadaten gespeichert und verarbeitet werden. Sie müssen jedoch sicherstellen, dass die Daten so partitioniert werden, dass eine Zeile nicht für mehrere Abonnenten repliziert werden kann. Weitere Informationen finden Sie im Abschnitt zum Festlegen von Partitionsoptionen unter [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
6.  Wenn Sie sich im Dialogfeld **Veröffentlichungseigenschaften - \<Publication>** befinden, klicken Sie auf **OK**, um die Einstellungen zu speichern und das Dialogfeld zu schließen.  
  
#### <a name="to-modify-a-parameterized-row-filter"></a>So ändern Sie einen parametrisierten Zeilenfilter  
  
1.  Wählen Sie auf der Seite **Tabellenzeilen filtern** des Assistenten für neue Veröffentlichung oder auf der Seite **Filterzeilen** von **Veröffentlichungseigenschaften – \<Publication>** einen Filter im Bereich **gefilterte Tabellen**, und klicken Sie dann auf **Bearbeiten**.  
  
2.  Ändern Sie den Filter im Dialogfeld **Filter bearbeiten** .  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-delete-a-parameterized-row-filter"></a>So löschen Sie einen parametrisierten Zeilenfilter  
  
1.  Wählen Sie auf der Seite **Tabellenzeilen filtern** des Assistenten für neue Veröffentlichung oder auf der Seite **Filterzeilen** von **Veröffentlichungseigenschaften - \<Publication>** einen Filter im Bereich **gefilterte Tabellen**, und klicken Sie dann auf **Löschen**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Verwenden von Transact-SQL  
 Parametrisierte Zeilenfilter können mithilfe gespeicherter Replikationsprozeduren programmgesteuert erstellt und geändert werden.  
  
#### <a name="to-define-a-parameterized-row-filter-for-an-article-in-a-merge-publication"></a>So definieren Sie einen parametrisierten Zeilenfilter für einen Artikel in einer Mergeveröffentlichung  
  
1.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) aus. Geben Sie **\@publication** an sowie für **\@article** einen Namen für den Artikel, für **\@source_object** die zu veröffentlichende Tabelle, für **\@subset_filterclause** die WHERE-Klausel zur Definition des parametrisierten Filters (ohne Angabe von `WHERE`) und für **\@partition_options** einen der folgenden Werte zur Beschreibung der Art der Partitionierung, die aus dem parametrisierten Filter resultiert:  
  
    -   **0** &ndash; Das Filtern für den Artikel ist entweder statisch oder ergibt keine eindeutige Teilmenge von Daten für jede Partition (eine "überlappende" Partition).  
  
    -   **1** &ndash; Die resultierenden Partitionen überlappen sich, und beim Abonnenten vorgenommene Updates können nicht zur Änderung der Partition führen, zu der eine Zeile gehört.  
  
    -   **2** &ndash; Der Filtervorgang für den Artikel ergibt nicht überlappende Partitionen. Mehrere Abonnenten können jedoch die gleiche Partition erhalten.  
  
    -   **3** – Der Filtervorgang für den Artikel ergibt sich nicht überlappende Partitionen, die für jedes Abonnement eindeutig sind.  
  
#### <a name="to-change-a-parameterized-row-filter-for-an-article-in-a-merge-publication"></a>So ändern Sie einen parametrisierten Zeilenfilter für einen Artikel in einer Mergeveröffentlichung  
  
1.  Führen Sie auf dem Verleger für die Veröffentlichungsdatenbank [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)aus. Geben Sie **\@publication**, **\@article**, den Wert **subset_filterclause** für **\@property**, den Ausdruck zur Definition des parametrisierten Filters für **\@value** (ohne Angabe von `WHERE`) und den Wert **1** sowohl für **\@force_invalidate_snapshot** als auch für **\@force_reinit_subscription** an.  
  
2.  Wenn diese Änderung zu einem anderem Partitionierungsverhalten führt, dann führen Sie [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) erneut aus. Geben Sie **\@publication**, **\@article**, den Wert **partition_options** für **\@property** und die am besten geeignete Partitionierungsoption für **\@value** an. Folgende Partitionierungsoptionen können angegeben werden:  
  
    -   **0** &ndash; Das Filtern für den Artikel ist entweder statisch oder ergibt keine eindeutige Teilmenge von Daten für jede Partition (eine "überlappende" Partition).  
  
    -   **1** &ndash; Die resultierenden Partitionen überlappen sich, und beim Abonnenten vorgenommene Updates können nicht zur Änderung der Partition führen, zu der eine Zeile gehört.  
  
    -   **2** &ndash; Der Filtervorgang für den Artikel ergibt nicht überlappende Partitionen. Mehrere Abonnenten können jedoch die gleiche Partition erhalten.  
  
    -   **3** – Der Filtervorgang für den Artikel ergibt sich nicht überlappende Partitionen, die für jedes Abonnement eindeutig sind.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Beispiel (Transact-SQL)  
 In diesem Beispiel wird eine Gruppe von Artikeln in einer Mergeveröffentlichung definiert, bei der die Artikel mit einer Reihe von Verknüpfungsfiltern anhand der `Employee` -Tabelle gefiltert werden, die selbst mithilfe eines parametrisierten Zeilenfilters in der **LoginID** -Spalte gefiltert wird. Während der Synchronisierung wird der von der [HOST_NAME](../../../t-sql/functions/host-name-transact-sql.md) -Funktion zurückgegebene Wert überschrieben. Weitere Informationen finden Sie im Abschnitt "Überschreiben des HOST_NAME()-Werts" im Thema [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 [!code-sql[HowTo#sp_MergeDynamicPub1](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-para_1.sql)]  
  
## <a name="see-also"></a>Weitere Informationen  
 [Definieren und Ändern eines Verknüpfungsfilters zwischen Mergeartikeln](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [Ändern von Veröffentlichungs- und Artikeleigenschaften](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Join Filters](../../../relational-databases/replication/merge/join-filters.md)   
 [Parametrisierte Zeilenfilter](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
  
  
