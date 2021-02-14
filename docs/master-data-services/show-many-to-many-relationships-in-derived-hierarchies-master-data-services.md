---
description: Anzeigen von m:n-Beziehungen in abgeleiteten Hierarchien (Master Data Services)
title: Anzeigen von m:n-Beziehungen in abgeleiteten Hierarchien
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 8b2a9c43-40e0-48f7-a6a9-325beb9f27da
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f550a109d5271a216614d0566ed6f3a4d87be056
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336292"
---
# <a name="show-many-to-many-relationships-in-derived-hierarchies-master-data-services"></a>Anzeigen von m:n-Beziehungen in abgeleiteten Hierarchien (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Abgeleitete Hierarchien zeigen (DH) 1:n-Beziehungen an und können jetzt auch m:n-Beziehungen anzeigen.  
  
## <a name="many-to-many-m2m-relationships"></a>m:n-Beziehungen  
 Eine m:n-Beziehung zwischen zwei Entitäten kann mithilfe einer dritten Entität modelliert werden, die eine Zuordnung zwischen ihnen bietet.  
  
 ![mds_hierarchies_manytomany](../master-data-services/media/mds-hierarchies-manytomany.png "mds_hierarchies_manytomany")  
  
 Im obigen Beispiel besteht eine m:n-Beziehung zwischen den Entitäten **Employee** (Mitarbeiter) und **TrainingClass** (Trainingskurs), die von der Zuordnungsentität **ClassRegistration**(Kursregistrierung) bereitgestellt wird. Ein Mitarbeiter kann als Teilnehmer in mehreren Kursen registriert sein, und jeder Kurs kann mehrere Teilnehmer enthalten.  
  
 Abgeleitete Hierarchien konnte zuvor keine m:n-Beziehungen modellieren. Ab [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]können Sie jetzt eine abgeleitete Hierarchie erstellen, die z.B. Teilnehmer nach Kursen anzeigt, oder die Beziehung umkehren und nach Teilnehmern gruppierte Kurse anzeigen.  
  
 Wechseln Sie zunächst zur Verwaltungsseite „Abgeleitete Hierarchie“, und erstellen Sie eine neue abgeleitete Hierarchie:  
  
 ![mds_hierarchies_add_derived_hierarchy](../master-data-services/media/mds-hierarchies-add-derived-hierarchy.png "mds_hierarchies_add_derived_hierarchy")  
  
 Fügen Sie dann zur neuen abgeleiteten Hierarchie Ebenen hinzu, wobei Sie von unten beginnen. In diesem Beispiel möchten wir nach Kursen gruppierte Teilnehmer (Mitarbeiter) anzeigen. Die Entität **Employee** ist daher die Blattebene der Hierarchie und wird zuerst hinzugefügt:  
  
 ![mds_hierarchies_edit_derived_hierarchy_one](../master-data-services/media/mds-hierarchies-edit-derived-hierarchy-one.PNG "mds_hierarchies_edit_derived_hierarchy_one")  
  
 Beachten Sie im obigen Screenshot, dass die Entität **Employee** unter **Aktuelle Ebenen** in der Mitte als einzige Ebene angezeigt wird. Die abgeleitete Hierarchie **Preview** auf der rechten Seite zeigt einfach eine Liste aller Elemente der Entität **Employee** an. Der Abschnitt **Verfügbare Ebenen** auf der linken Seite zeigt an, welche Ebenen oberhalb der aktuell obersten Ebene hinzugefügt werden können (**Employee**). Bei den meisten handelt es sich um domänenbasierte Attribute (DBAs) für die Entität **Employee** . Hierzu zählt auch das domänenbasierte Attribut **Department** .  
  
 Ab [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]gibt es eine neue Art von Ebene, die m:n-Beziehungen modelliert, z.B. **Class (zugeordnet über „ClassRegistration.Student“)**. Der Ebenenname ist ausführlicher als die anderen, um die zusätzliche Information widerzuspiegeln, die zum eindeutigen Beschreiben der Zuordnungsbeziehung erforderlich ist. Fügen Sie diese Ebene per Drag &amp; Drop zur Ebene **Employee** im Abschnitt **Aktuelle Ebenen** hinzu:  
  
 ![mds_hierarchies_edit_derived_hierarchy_two](../master-data-services/media/mds-hierarchies-edit-derived-hierarchy-two.PNG "mds_hierarchies_edit_derived_hierarchy_two")  
  
 In der Vorschau werden jetzt die nach Schulungskurs gruppierten Mitarbeiter angezeigt, für die diese registriert sind. Da dies eine m:n-Beziehung ist, kann jedes untergeordnete Element über mehrere übergeordnete Elemente verfügen. Im obigen Beispiel ist der Mitarbeiter **6 {Hillman, Reinout N}** als Teilnehmer für zwei Kurse registriert, **1 {Master Data Services 101}** und **4 {Career-Limiting Moves}**(nicht karriereförderliches Handeln).  
  
 Diese Zuordnungsbeziehung kann auch umgekehrt angezeigt werden, wobei die Kurse nach Teilnehmern gruppiert werden:  
  
 ![mds_hierarchies_available_entities_and_hierarchies](../master-data-services/media/mds-hierarchies-available-entities-and-hierarchies.PNG "mds_hierarchies_available_entities_and_hierarchies")  
  
 Dies ist ein weiteres Beispiel dafür, dass ein untergeordnetes Element unter mehreren übergeordneten Elementen angezeigt werden kann: Schulungskurs **1 {Master Data Services 101}** wird sowohl unter **6 {Hillman, Reinout N}** als auch unter **40 {Ford, Jeffrey L}** angezeigt.  
  
 Die Elemente der Zuordnungsentität **ClassRegistration** werden nicht innerhalb der abgeleiteten Hierarchie angezeigt. Sie werden lediglich zum Definieren der Beziehungen zwischen übergeordneten und untergeordneten Elementen in der Hierarchie verwendet.  
  
 Sie können die m:n-Beziehung bearbeiten, indem Sie die Elemente der Zuordnungsentität mithilfe einer der folgenden Methoden ändern. Die m:n-Beziehung ist auf der Seite des **Explorers für abgeleitete Hierarchien** schreibgeschützt.  
  
-   Ändern Sie die Elemente der Zuordnungsentität auf der Seite **Entitäts-Explorer** mithilfe des Master Data Services-Add-Ins für Excel oder über das Staging von Daten.  
  
-   Verschieben Sie untergeordnete Knoten per Drag &amp; Drop auf der Seite des **Explorers für abgeleitete Hierarchien** zwischen übergeordneten Elementen.  
  
     Diese Methode ändert nach Möglichkeit vorhandene Elemente und fügt neue Elemente bei Bedarf hinzu. Vorhandene Elemente werden nicht gelöscht.  
  
     Mit der Zuordnungsentität „ClassRegistration“ wird z. B. beim Verschieben eines Teilnehmers auf den nicht verwendeten Knoten der Kursattributwert des entsprechenden Zuordnungsentitätselements in null geändert, und das Element wird nicht gelöscht. Umgekehrt wird beim Verschieben eines Teilnehmers aus dem nicht verwendeten Knoten in einen Kurs das Element geändert, wenn ein vorhandenes Zuordnungselement vorhanden ist, das dem Teilnehmer entspricht, bei dem der Kurs null ist, indem für den Kurs der Wert von null in das neue übergeordnete Element geändert wird. Wenn dieses Element nicht vorhanden ist, wird eines hinzugefügt.  
  
     Dieser Prozess verhindert das Löschen von Elementen, um das ungewollte Löschen von anderen Benutzerdaten zu vermeiden, wenn z. B. die Zuordnungsentität andere Attribute als die beiden enthält, die die Beziehung zwischen über- und untergeordnetem Element definieren. Benutzer müssen die Löschvorgänge explizit und direkt für die Zuordnungsentität vornehmen.  
  
 Die neue m:n-Ebene kann überall innerhalb einer abgeleiteten Hierarchie angezeigt werden, in der eine domänenbasierte Attributebene zulässig ist. Eine m:n-Ebene kann sich wie in den obigen Beispielen auf der obersten Ebene befinden. Sie kann über und/oder unter einer domänenbasierten Attributebene angeordnet sein, einschließlich rekursiver Ebenen. Sie kann unter der Abschlussebene einer expliziten Hierarchie (veraltet) angeordnet sein. Mehrere m:n-Beziehungen können in derselben abgeleiteten Hierarchie miteinander verkettet werden.  
  
 m:n-Ebenen können genau wie andere abgeleitete Hierarchieebenen ausgeblendet werden.  
   
### <a name="m2m-relationship-in-sample-model"></a><a name="M2MSample"></a> M2M-Beziehung im Beispielmodell  
Eine Vorführung einer m:n-Beziehung finden Sie in der abgeleiteten Hierarchie „Region Climate“ (regionales Klima) im Beispielmodell „Customer“, das in [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]enthalten ist.   
  
Wie in der folgenden Abbildung gezeigt, lautet der Ebenenname, der diese Beziehung modelliert, ![mds_Number1](../master-data-services/media/mds-number1.png)**Climate (mapped via RegionClimate.Region)**(Klima (über RegionClimate.Region zugeordnet)). ![mds_Number2](../master-data-services/media/mds-number2.png)**Preview** (Vorschau) zeigt die Regionen gruppiert nach den Klimatypen, denen sie zugeordnet sind. Hierbei handelt es sich um eine m:n-Beziehung, weil einige Regionen (untergeordnete Elemente) mehreren Klimata (übergeordneten Elementen) zugeordnet sind. So ist ![mds_Number3](../master-data-services/media/mds-number3.png)**APCR {Asia Pacific}** (Asien/Pazifik) z.B. ![mds_Number4](../master-data-services/media/mds-number4.png)**A {Tropical}** (Tropisch) und ![mds_Number5](../master-data-services/media/mds-number5.png)**B {Dry}**(Trocken) zugeordnet.  
  
![mds_M2MRelationship_Example_CustomerModel](../master-data-services/media/mds-m2mrelationship-example-customermodel.png)  
  
Eine Anleitung zum Bereitstellen des Customer-Beispielmodells und weitere in [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)]enthaltene Beispielmodelle finden Sie unter [Bereitstellen von Beispielmodelle und Daten](~/master-data-services/sql-server-samples-model-deployment-packages-mds.md).   
  
## <a name="one-many-relationship"></a>1:n-Beziehung  
 Ein Element einer abgeleiteten Hierarchie kann das übergeordnete Element vieler untergeordneter Elemente sein, aber es kann in der Regel nicht mehr als ein übergeordnetes Element aufweisen (Ausnahmen finden Sie unter [Elementsicherheit](#bkmk_member_security)). Angenommen, es gibt zwei Entitäten: „Employee“ und „Department“, wobei jeder „Employee“ zu einem einzelnen „Department“ gehört. Diese Beziehung wird durch Hinzufügen eines domänenbasierten Attributs zur Entität „Employee“ modelliert, das auf die Entität „Department“ verweist:  
  
 ![mds_hierarchies_onetomany](../master-data-services/media/mds-hierarchies-onetomany.png "mds_hierarchies_onetomany")  
  
 Dies ist eine 1:n-Beziehung, da jeder „Employee“ nur zu einem „Department“ gehört, aber jedes „Department“ kann mehrere „Employee“-Entitäten enthalten. Es kann eine abgeleitete Hierarchie erstellt werden, in der „Employee“ nach „Department“ gruppiert angezeigt wird:  
  
 ![mds_hierarchies_dh_screenshot](../master-data-services/media/mds-hierarchies-dh-screenshot.png "mds_hierarchies_dh_screenshot")  
  
##  <a name="member-security"></a><a name="bkmk_member_security"></a> Element Sicherheit  
 Eine Hierarchie, die das Kopieren von Elementen ermöglicht (dadurch kann ein Element mehrere übergeordnete Elemente aufweisen), kann nicht dazu verwendet werden, um Elementsicherheitsberechtigungen zuzuweisen. Beispiel:  
  
-   Eine rekursive abgeleitete Hierarchie, die keine Null-Rekursionen verankert (jedes Element der rekursiven Ebene wird sowohl unter ROOT als auch unter seinem rekursiven übergeordneten Element angezeigt).  
  
-   Eine rekursive abgeleitete Hierarchie mit einer Ebene über der rekursiven Ebene (jedes Element der rekursiven Ebene wird sowohl unter seinem nicht rekursiven als auch unter seinem rekursiven übergeordneten Element angezeigt).  
  
-   Eine abgeleitete Hierarchie mit einer m:n-Ebene (ein untergeordnetes Element kann vielen übergeordneten Elementen zugeordnet werden).  
  
## <a name="collections"></a>Sammlungen  
 Sammlungen und explizite Hierarchien sind veraltet. Die gespeicherte Konvertierungsprozedur (udpConvertCollectionAndConsolidatedMembersToLeaf) konvertiert Sammlungselemente in Blattelemente und erstellt abgeleitete m:n-Hierarchien, um die Informationen zur Sammlungsmitgliedschaft zu erfassen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Abgeleitete Hierarchien &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)  
  
  
