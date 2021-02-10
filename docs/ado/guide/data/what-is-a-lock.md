---
description: Was ist eine Sperre?
title: Was ist eine Sperre? | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- cursors [ADO], locking
- locks [ADO], about locking
ms.assetid: f8989555-28c6-4c17-9bf8-7f44a8a5c407
author: rothja
ms.author: jroth
ms.openlocfilehash: 245a0cd91b9d6c7675df146d9d5808b42bb8c618
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036730"
---
# <a name="what-is-a-lock"></a>Was ist eine Sperre?
Das Sperren ist der Prozess, durch den ein DBMS den Zugriff auf eine Zeile in einer mehr Benutzerumgebung einschränkt. Wenn eine Zeile oder Spalte exklusiv gesperrt ist, dürfen andere Benutzer erst auf die gesperrten Daten zugreifen, wenn die Sperre aufgehoben wurde. Dadurch wird sichergestellt, dass zwei Benutzer nicht gleichzeitig dieselbe Spalte in einer Zeile aktualisieren können.  
  
 Sperren können aus Ressourcen Sicht sehr teuer sein und sollten nur verwendet werden, wenn Sie zum Beibehalten der Datenintegrität benötigt werden. In einer Datenbank, in der Hunderte oder Tausende von Benutzern pro Sekunde versuchen, auf einen Datensatz zuzugreifen (z. b. eine Datenbank, die mit dem Internet verbunden ist), kann eine nicht benötigte Sperre schnell zu einer langsameren Leistung in Ihrer Anwendung führen.  
  
 Sie können steuern, wie die Datenquelle und die ADO-Cursor Bibliothek die Parallelität verwalten, indem Sie die entsprechende Sperroption auswählen.  
  
 Legen Sie die **LockType** -Eigenschaft vor dem Öffnen eines **Recordsets** fest, um anzugeben, welcher Sperrentyp beim Öffnen des Anbieters verwendet werden soll. Lesen Sie die-Eigenschaft, um den in einem geöffneten **Recordset** -Objekt verwendeten Sperrentyp zurückzugeben.  
  
 Anbieter unterstützen möglicherweise nicht alle Sperr Typen. Wenn ein Anbieter die angeforderte **LockType** -Einstellung nicht unterstützt, wird ein anderer Sperrentyp ersetzt. Um die tatsächliche Sperr Funktionalität zu ermitteln, die in einem **Recordset** -Objekt verfügbar ist, verwenden Sie die [unterstützte](../../../ado/reference/ado-api/supports-method.md) Methode mit **adupdate** und **adUpdateBatch**.  
  
 Die **adlockpessimi-** Einstellung wird nicht unterstützt, wenn die [Cursor Location](../../../ado/reference/ado-api/cursorlocation-property-ado.md) -Eigenschaft auf **adUseClient** festgelegt ist. Wenn ein nicht unterstützter Wert festgelegt ist, wird kein Fehler ausgegeben. Stattdessen wird der nächstgelegene unterstützte **LockType** verwendet.  
  
 Die **LockType** -Eigenschaft ist Lese-/Schreibzugriff, wenn das **Recordset** geschlossen ist, und ist schreibgeschützt, wenn es geöffnet ist.  
  
 In diesem Abschnitt werden die folgenden Themen behandelt:  
  
-   [Typen von Sperren](../../../ado/guide/data/types-of-locks.md)
