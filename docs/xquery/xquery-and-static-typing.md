---
title: XQuery und statische Typisierung | Microsoft-Dokumentation
description: Erfahren Sie mehr über die statische Typrückschluss und die statische Typüberprüfung in XQuery.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- XQuery, static typing
- static typing
- checking static types
- inference [XQuery]
ms.assetid: d599c791-200d-46f8-b758-97e761a1a5c0
author: rothja
ms.author: jroth
ms.openlocfilehash: 6dcaa7bbbd5674c6db3b6d8ec5185cc31e890707
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352311"
---
# <a name="xquery-and-static-typing"></a>XQuery und statische Typisierung
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  XQuery ist in [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] eine statisch typisierte Sprache. Sie gibt bei der Abfragekompilierung einen Typfehler aus, wenn ein Ausdruck einen Wert zurückliefert, dessen Typ oder Kardinalität von einer bestimmten Funktion oder einem Operator nicht angenommen wird. Darüber hinaus kann eine Überprüfung des statischen Typs auch erkennen, ob ein Pfadausdruck eines typisierten XML-Dokuments falsch typisiert ist. Der XQuery-Compiler realisiert zuerst die Normalisierungsphase, in der die impliziten Vorgänge, wie die Atomisierung, hinzugefügt werden. Anschließend erfolgen die Inferenz und die Überprüfung des statischen Typs.  
  
## <a name="static-type-inference"></a>Inferenz des statischen Typs  
 Die Inferenz des statischen Typs ermittelt den Rückgabetyp eines Ausdrucks. Hierbei wird aus den statischen Typen der Eingabeparameter und aus der statischen Semantik des Vorgangs der statische Typ des Ergebnisses abgeleitet. Beispiel: Der statische Typ des Ausdrucks 1 + 2,3 wird wie folgt abgeleitet:  
  
-   Der statische Typ von 1 ist **xs: Integer** , und der statische Typ 2,3 ist **xs: Decimal**. Basierend auf der dynamischen Semantik konvertiert die statische Semantik des **+** Vorgangs die Ganzzahl in eine Dezimalzahl und gibt dann ein Dezimaltrennzeichen zurück. Der abgeleitet static-Typ wäre dann **xs: Decimal**.  
  
 Für nicht typisierte XML-Instanzen gibt es spezielle Typen, mit deren Hilfe angegeben wird, dass die Daten nicht typisiert sind. Diese Information wird bei der Überprüfung des statischen Typs und zur Durchführung bestimmter impliziter Datentypkonvertierungen verwendet.  
  
 Bei typisierten Daten wird der Eingabetyp aus der XML-Schemaauflistung abgeleitet, die die XML-Datentypinstanz einschränkt. Wenn das Schema z. b. nur Elemente vom Typ **xs: Integer** zulässt, sind die Ergebnisse eines Pfad Ausdrucks, der dieses Element verwendet, 0 (null) oder mehr Elemente vom Typ **xs: Integer**. Dies wird zurzeit mithilfe eines Ausdrucks ausgedrückt, z `element(age,xs:integer)*` . b. wenn das Sternchen ( \* ) die Kardinalität des resultierenden Typs angibt. In diesem Beispiel kann der Ausdruck NULL oder mehr Elemente mit dem Namen "Age" und dem Typ " **xs: Integer**" ergeben. Andere Kardinalitäten sind genau 1 und werden durch die Verwendung des Typnamens allein, NULL oder eins ausgedrückt und mit einem Fragezeichen (**?**) und 1 oder mehr und mit einem Pluszeichen ( **+** ) ausgedrückt.  
  
 In manchen Fällen kann die Inferenz des statischen Typs ableiten, dass ein Ausdruck immer eine leere Zeichenfolge zurückliefert. Wenn ein Pfad Ausdruck für einen typisierten XML-Datentyp z. b. nach einem- \<name> Element in einem- \<customer> Element (/Customer/Name) sucht, das Schema jedoch keinen innerhalb von zulässt \<name> \<customer> , wird durch den statischen Typrückschluss abgeleitet, dass das Ergebnis leer ist. Dies wird zum erkennen falscher Abfragen verwendet und als statischer Fehler gemeldet, es sei denn, der Ausdruck war () oder **Data (())**.  
  
 Die detaillierten Inferenzregeln werden in der formalen Semantik der XQuery-Spezifikation angegeben. Microsoft hat diese nur geringfügig für typisierte XML-Datentypinstanzen angepasst. Die wichtigste Änderung zum Standard besteht darin, dass der implizite Dokumentknoten den Typ der XML-Datentypinstanz kennt. Aus diesem Grund wird ein Pfadausdruck der Form /age auf der Grundlage dieser Information exakt typisiert.  
  
 Wenn Sie [SQL Server Profiler Vorlagen und Berechtigungen](../tools/sql-server-profiler/sql-server-profiler-templates-and-permissions.md)verwenden, können Sie die statischen Typen sehen, die als Teil der Abfrage Kompilierungen zurückgegeben werden. Dazu muss die Ablaufverfolgung das XQuery Static Type-Ereignis in der TSQL-Ereigniskategorie aufweisen.  
  
## <a name="static-type-checking"></a>Überprüfung des statischen Typs  
 Die Überprüfung des statischen Typs stellt sicher, dass bei der Ausführung zur Laufzeit nur Werte des für den Vorgang passenden Typs empfangen werden. Da die Typen nicht zur Laufzeit geprüft werden müssen, können potenzielle Fehler zu einem frühen Zeitpunkt der Kompilierung festgestellt werden. Dies verbessert die Leistung. Die statische Typisierung setzt jedoch voraus, dass der Verfasser der Abfrage diese sorgfältiger formuliert.  
  
 Folgende Typen können verwendet werden:  
  
-   Typen, die durch eine Funktion oder einen Vorgang explizit zugelassen werden.  
  
-   Ein Untertyp eines explizit zugelassenen Typs.  
  
 Untertypen werden auf der Grundlage der Untertypisierungsregeln definiert, um Ableitungen durch Beschränkung oder Erweiterung des XML-Schemas zu verwenden. Ein Typ S ist beispielsweise ein Untertyp von T, wenn alle Werte, die den Typ S besitzen, auch Instanzen des Typs T sind.  
  
 Darüber hinaus sind alle Ganzzahlwerte auf der Grundlage der Typhierarchie des XML-Schemas auch Dezimalwerte. Nicht alle Dezimalwerte sind allerdings ganze Zahlen. Aus diesem Grund ist eine ganze Zahl ein Untertyp einer Dezimalzahl, aber nicht umgekehrt. Der- **+** Vorgang lässt z. b. nur Werte bestimmter Typen zu, z. b. die numerischen Typen **xs: Integer**, **xs: Decimal**, **xs: float** und **xs: Double**. Wenn Werte anderer Typen, wie z. b. **xs: String**, übermittelt werden, löst der Vorgang einen Typfehler aus. Dies wird als strenge Typisierung bezeichnet. Werte anderer Typen, wie der Typ atomic, der untypisiertes XML kennzeichnet, können implizit in einen Wert eines Typs konvertiert werden, den die Operation zulässt. Dies wird als schwache Typisierung bezeichnet.  
  
 Wenn eine implizite Konvertierung notwendig ist, stellt eine anschließende Überprüfung des statischen Typs sicher, dass an eine Operation nur Werte des zugelassenen Typs mit der richtigen Kardinalität übergeben werden. Bei "String" + 1 wird erkannt, dass der statische Typ von "String" **xs: String** ist. Da es sich hierbei nicht um einen zulässigen Typ für den **+** Vorgang handelt, wird ein Typfehler ausgelöst.  
  
 Wenn das Ergebnis eines beliebigen Ausdrucks E1 zu einem beliebigen Ausdruck E2 addiert wird (E1 + E2), bestimmt die Inferenz des statischen Typs zuerst die statischen Typen von E1 und E2 und prüft diese dann gegen die für den Vorgang zulässigen statischen Typen. Wenn der statische Typ von E1 z. b. entweder **xs: String** oder **xs: Integer** sein kann, löst die Überprüfung des statischen Typs einen Typfehler aus, auch wenn einige Werte zur Laufzeit ganze Zahlen sein könnten. Dasselbe wäre der Fall, wenn der statische Typ von E1 **xs: Integer&#42;** wäre. Da der **+** Vorgang nur genau einen ganzzahligen Wert akzeptiert und E1 NULL oder mehr als 1 zurückgeben kann, löst die Überprüfung des statischen Typs einen Fehler aus.  
  
 Wie zuvor erwähnt, ermittelt die Typinferenz häufig einen Typ, der breiter angelegt ist, als dies dem Benutzer hinsichtlich des zu übergebenden Datentyps bekannt ist. In diesen Fällen muss der Benutzer die Abfrage neu schreiben. Zu den typischen Fällen gehören die folgenden:  
  
-   Der Typ folgert einen allgemeineren Typ, wie einen Untertyp oder eine Typvereinigung. Wenn der Typ atomic ist, müssen Sie den Datentypkonvertierungsausdruck oder die Konstruktorfunktion verwenden, um den tatsächlichen statischen Typ anzugeben. Wenn z. b. der herausgestellte Typ des Ausdrucks E1 eine Wahl zwischen **xs: String** oder **xs: Integer** ist und die Addition **xs: Integer** erfordert, sollten Sie `xs:integer(E1) + E2` anstelle von schreiben `E1+E2` . Bei diesem Ausdruck kann zur Laufzeit ein Fehler auftreten, wenn ein Zeichen folgen Wert gefunden wird, der nicht in **xs: Integer** umgewandelt werden kann. Der Ausdruck wird jetzt jedoch die Überprüfung des statischen Typs durchlaufen. Dieser Ausdruck wird der leeren Sequenz zugeordnet.  
  
-   Aus dem Typ wird eine höhere Kardinalität abgeleitet, als die tatsächlich in den Daten enthaltene. Dies tritt häufig auf, da der **XML** -Datentyp mehr als ein Element der obersten Ebene enthalten kann, und eine XML-Schema Auflistung dies nicht einschränken kann. Um den statischen Typ zu reduzieren und sicherzustellen, dass stattdessen maximal ein Wert übergeben wird, müssen Sie das Positionsprädikat `[1]` verwenden. Beispiel: Um 1 zum Wert des Attributs `c` des Elements `b` unter dem Element der obersten Ebene zu addieren, müssen Sie `write (/a/b/@c)[1]+1` angeben. Zusätzlich können Sie mit einer XML-Schemaauflistung das Schlüsselwort DOCUMENT verwenden.  
  
-   Bei einigen Vorgängen geht bei der Inferenz die Typinformation verloren. Wenn z. b. der Typ eines Knotens nicht bestimmt werden kann, wird er **anyType**. Dieser wird nicht implizit in irgendeinen anderen Typ umgewandelt. Das tritt am deutlichsten während der Navigation mithilfe der übergeordneten Achse auf. Sie sollten die Verwendung solcher Vorgänge vermeiden und die Abfrage neu schreiben, falls der Ausdruck einen statischen Typfehler erzeugt.  
  
## <a name="type-checking-of-union-types"></a>Typüberprüfung von Union-Typen  
 Union-Typen erfordern aufgrund der Typüberprüfung eine sorgfältige Handhabung. In den folgenden Beispielen sind zwei der Probleme dargestellt.  
  
### <a name="example-function-over-union-type"></a>Beispiel: Funktion für Union-Typ  
 Stellen Sie eine Element Definition für <`r`> eines Union-Typs in Erwägung:  
  
```  
<xs:element name="r">  
<xs:simpleType>  
   <xs:union memberTypes="xs:int xs:float xs:double"/>  
</xs:simpleType>  
</xs:element>  
```  
  
 Im XQuery-Kontext gibt die "Average"-Funktion `fn:avg (//r)` einen statischen Fehler zurück, da der XQuery-Compiler keine Werte verschiedener Typen (**xs: int**, **xs: float** oder **xs: Double**) für die <`r`> Elemente im-Argument von **FN: AVG ()** hinzufügen kann. Um dieses Problem zu beheben, müssen Sie den Funktionsaufruf als `fn:avg(for $r in //r return $r cast as xs:double ?)` umschreiben.  
  
### <a name="example-operator-over-union-type"></a>Beispiel: Operator für Union-Typ  
 Die Additionsoperation ('+') erfordert präzise Typen der Operanden. Folglich gibt der Ausdruck `(//r)[1] + 1` einen statischen Fehler zurück, der die zuvor beschriebene Typdefinition für Element <> aufweist `r` . Eine mögliche Lösung besteht im Umschreiben des Ausdrucks als as `(//r)[1] cast as xs:int? +1`, wobei das "?" das Auftreten von 0 oder 1 anzeigt. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] erfordert "cast as" mit "?", weil jede Umwandlung die leere Sequenz als ein Ergebnis von Laufzeitfehlern verursachen kann.  
  
## <a name="see-also"></a>Weitere Informationen  
 [XQuery-Sprachreferenz &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)  
  
  
