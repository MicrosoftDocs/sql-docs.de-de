---
title: Active Directory- und Kubernetes-DNS-Abstimmung in Big Data-Cluster-Bereitstellungen
description: Konfigurieren des DNS-Abgleichs für SQL Server Big Data-Cluster im Active Directory-Modus
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a20afc81637eba30427b7b1062738b2d81ae67a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047880"
---
# <a name="active-directory-and-kubernetes-dns-reconciliation-in-big-data-clusters-deployments"></a>Active Directory- und Kubernetes-DNS-Abstimmung in Big Data-Cluster-Bereitstellungen

In diesem Artikel werden einige der Herausforderungen beim und Lösungen für das Implementieren einer Active Directory-Integration beim Bereitstellen von Big Data-Cluster (BDC) beschrieben.

## <a name="overview"></a>Übersicht

Wenn der Big Data-Cluster nicht mit Active Directory-Integration bereitgestellt wird, wird der Dienst [Kubernetes CoreDNS](https://kubernetes.io/docs/tasks/administer-cluster/coredns/) für interne DNS-Auflösungen verwendet. Kubernetes verwendet eine interne Domäne, z. B. `<namespace>.svc.cluster.local`. Es werden A-Einträge (Forward-Lookup) und PTR-Einträge (Reverse-Lookup) auf dem DNS-Server mit Namen in dieser Domäne erstellt.

Wenn der Active Directory-Modus aktiviert ist, wird jedoch eine neue Domäne mit eigenen DNS-Servern verwendet. Bei der internen Namensauflösung kann dies zu Verwirrung in Bezug darauf führen, welche DNS-Server für Forward- und Reverse-Lookups zu verwenden sind.

## <a name="challenges"></a>Herausforderungen

* Wenn neue Kubernetes-Pods bereitgestellt werden, müssen in beiden Gruppen von DNS-Servern DNS-Einträge hinzugefügt werden. Kubernetes übernimmt das Speichern der Einträge in Kubernetes CoreDNS, das Hinzufügen der erforderlichen Einträge auf den DNS-Servern der Active Directory-Domänencontroller erfolgt jedoch im BDC-Bereitstellungsworkflow. Beim Löschen eines BDC-Clusters muss der Workflow wiederum sicherstellen, dass diese Einträge entfernt werden.
* Active Directory-DNS-Server sind nicht Teil des Kubernetes-Clusters. BDC verfügt jedoch über einen eigenen IP-Adressraum in Kubernetes, für den BDC keine Einträge auf einem externen DNS-Server erstellen kann, da er außerhalb der Clustergrenzen nicht sichtbar ist.
* Wenn im Kubernetes-Cluster Failoverereignisse auftreten, müssen auch die Einträge auf den AD-DNS-Servern aktualisiert werden.
* Neben den Podnamen müssen auch die Namen von Kubernetes-Diensten über AD-Domänennamenlookups adressierbar sein. Dies stellt eine weitere Herausforderung im Active Directory-DNS dar, da ein einzelner Dienstname mehreren Pod-IP-Adressen zugeordnet sein kann.
* Es kann zu erheblichen Verzögerungen bei der Weiterleitung von Updates und der Replizierung auf den Active Directory-DNS-Servern der Organisation kommen, auf die die BDC-Verwaltungsworkflows keinen Einfluss haben. Dies kann sich direkt bei der Bereitstellung und beim Failover auf die BDC-Funktionalität auswirken. Im Gegensatz dazu ist Kubernetes CoreDNS aufgrund des Orts schneller und effizienter.

## <a name="solution"></a>Lösung

Zur Umgehung der oben erwähnten Herausforderungen umfasst die in BDC implementierte Lösung den neuen internen CoreDNS-Dienst, der innerhalb des BDC-Namespace verwaltet wird. Dies ist der einzige DNS-Dienst, den die Pods im BDC-Namespace für die Namensauflösung verwenden. Die Komplexität mehrerer Domänen wird hinter dem neuen CoreDNS-Dienst verborgen.

Zum Beispiel im folgenden Diagramm verwenden die Pods den BDC-CoreDNS-Server für die Namensauflösung. Die Pods interagieren weder mit dem Kubernetes-CoreDNS-Server noch mit dem AD-DNS-Server direkt. 

:::image type="content" source="media/active-directory-dns-reconciliation/bdc-ad-dns-reconciliation.png" alt-text="Pods stellen eine Verbindung mit dem CoreDNS-Server im eigenen Namespace her":::

Im Folgenden sind einige der Implementierungsdetails aufgeführt, die die Funktionsweise dieses Aufbaus in BDC verdeutlichen:

### <a name="centralized-management-of-multiple-domains"></a>Zentrale Verwaltung mehrerer Domänen

Die Komplexität der Vorgänge bei Namensabfragen bleibt zentral hinter dem internen DNS-Dienst verborgen. Dadurch wird vermieden, dass einzelne Pods mehrere Domänen verwalten müssen, und die Struktur wird vereinfacht.

### <a name="no-records-for-internal-pods-in-external-dns-servers"></a>Keine Einträge für interne Pods auf externen DNS-Servern

Aufgrund dieses Strukturprinzips muss BDC keine A- und PTR-Einträge für Pods im Kubernetes-IP-Adressraum auf externen DNS-Servern erstellen und verwalten.

### <a name="no-duplication-of-records"></a>Keine Duplizierung von Einträgen

Interne DNS-Einträge an mehreren Orten. Der einzige Speicher für diese Einträge ist Kubernetes CoreDNS. Die BDC-interne Version von CoreDNS führt eine computerseitige Umschreibung und Weiterleitung von DNS-Abfragen an Kubernetes CoreDNS durch.

### <a name="computational-rewriting"></a>Computerseitige Umschreibung

Da BDC keine Einträge speichert, ist BDC für die Übersetzung eingehender Forward-Lookup-Abfragen mit Namen in der AD-Domäne in die Namen in der Kubernetes-Domäne sowie die Weiterleitung dieser Abfragen an Kubernetes CoreDNS zuständig.
Beispielsweise würde eine eingehende Abfrage für `compute-0-0.contoso.local` in `compute-0-0.compute-0-svc.contoso.svc.cluster.local` umgeschrieben und diese Abfrage dann an Kubernetes CoreDNS weitergeleitet.
Bei Reverse-Lookups wird die Abfrage mit den internen IP-Adressen aus Kubernetes CoreDNS weitergeleitet und die Antwort in den AD-Domänennamen umgeschrieben, bevor sie an den Client gesendet wird.

### <a name="simplicity-in-pod-configurations"></a>Einfachheit bei Podkonfigurationen

Da bei allen BDC-Pods in /etc/resolv.conf nur auf die BDC-interne Version von CoreDNS verwiesen wird, vereinfacht dies die Netzwerkansicht aus Sicht der Pods. Die Komplexität wird stattdessen in der internen Version von CoreDNS verborgen.

### <a name="static-and-reliable-ip-address-for-dns-service"></a>Statische und zuverlässige IP-Adresse für den DNS-Dienst

Der von BDC bereitgestellte CoreDNS-Dienst verfügt über eine registrierte statische interne IP-Adresse, auf die von allen Pods aus zugegriffen werden kann. Dadurch wird sichergestellt, dass die Werte in /etc/resolv.conf nicht aktualisiert werden müssen.

### <a name="service-load-balance-management-is-retained-by-kubernetes"></a>Die Verwaltung des Lastenausgleichs für den Dienst wird weiterhin von Kubernetes übernommen.

Wenn Lookups für Dienste anstelle einzelner Pods durchgeführt werden, werden sie weiterhin an Kubernetes CoreDNS weitergeleitet, sodass BDC nicht für die Implementierung eines Lastenausgleichs speziell für die AD-Domäne verantwortlich ist.

Wenn beispielsweise eine Forward-Lookup-Abfrage für `compute-0-svc.contoso.local` erfolgt, wird diese in ` compute-0-svc.contoso.svc.cluster`.local umgeschrieben. Diese Abfrage wird an Kubernetes CoreDNS weitergeleitet, wo der Lastenausgleich vorgenommen wird. Die Antwort ist eine IP-Adresse für eine der verschiedenen Computepoolinstanzen (Podreplikate).

### <a name="scalability"></a>Skalierbarkeit

Da BDC keine Einträge speichert, kann die BDC-interne Version von CoreDNS ohne Zustandsbeibehaltung und Eintragsreplikation replikateübergreifend skaliert werden. Wenn die DNS-Einträge in BDC gespeichert werden, muss BDC auch den Status in alle Pods replizieren.

### <a name="externally-visible-service-entries-stay-in-ad-dns"></a>Extern sichtbare Diensteinträge bleiben im AD-DNS

Für Dienstendpunkte, auf die Clients außerhalb des Kubernetes-Clusters zugreifen können müssen, werden bei der Bereitstellung von BDC DNS-Einträge auf dem AD-DNS-Server erstellt. Der Benutzer gibt die zu registrierenden DNS-Namen über die Bereitstellungskonfigurationsprofile an.

### <a name="self-deprovisioning"></a>Eigenes Aufheben der Bereitstellung

Wenn BDC gelöscht wurde, ist keine zusätzliche dynamische Arbeit zum Löschen von DNS-Einträgen erforderlich, wenn die Bereitstellung des Clusters aufgehoben wird. Die einzigen Einträge im Remote-Active Directory-DNS, die bereinigt werden müssen, sind für externe Dienste, und die Anzahl dieser Einträge ist statisch. Die internen DNS-Einträge werden automatisch zusammen mit dem Cluster entfernt.

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen von Big Data-Cluster für SQL Server im Active Directory-Modus](active-directory-deploy.md)
- [Verwalten des Zugriffs auf Big Data-Cluster im Active Directory-Modus](active-directory-objects.md)
- [Bereitstellen von mehreren Big Data-Clustern für SQL Server in derselben Active Directory-Domäne](active-directory-deployment-background.md)
