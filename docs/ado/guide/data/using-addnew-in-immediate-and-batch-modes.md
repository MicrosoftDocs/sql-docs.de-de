---
title: Verwenden von AddNew im unmittelbaren und im Batch Modus | Microsoft-Dokumentation
description: Erläutert, wie AddNew im unmittelbaren und im Batch Modus verwendet wird.
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- AddNew method [ADO]
- ADO, editing data
- ADO, adding data
- editing data [ADO], AddNew method
ms.assetid: ed314bb9-e188-4658-a68c-a2abc49610be
author: rothja
ms.author: jroth
ms.openlocfilehash: 689b8fbc6c8bb9446adfeb9fec98d53d59b28917
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88979051"
---
# <a name="using-addnew-in-immediate-and-batch-modes"></a>Verwenden von AddNew im unmittelbaren und im Batchmodus
Das Verhalten der **AddNew** -Methode hängt vom Aktualisierungs Modus des **Recordset** -Objekts und davon ab, ob die Argumente *FieldList* und *Values* übergeben werden.  
  
 Im sofortigen Update Modus (in dem der Anbieter Änderungen in die zugrunde liegende Datenquelle schreibt, nachdem Sie die **Update** -Methode aufgerufen haben), wird durch Aufrufen der **AddNew** -Methode ohne Argumente die **EditMode** -Eigenschaft auf **adEditAdd** festgelegt. Der Anbieter speichert alle Feldwert Änderungen lokal zwischen. Durch Aufrufen der **Update** -Methode wird der neue Datensatz in der Datenbank bereitgestellt, und die **EditMode** -Eigenschaft wird auf " **adEditNone** " zurückgesetzt. Wenn Sie die Argumente *FieldList* und *Values* übergeben, sendet ADO den neuen Datensatz sofort an die Datenbank (kein **Update** -Befehl erforderlich); der **EditMode** -Eigenschafts Wert ändert sich nicht (**adEditNone**).  
  
 Im Batch Update Modus wird durch Aufrufen der **AddNew** -Methode ohne Argumente die **EditMode** -Eigenschaft auf **adEditAdd**festgelegt. Der Anbieter speichert alle Feldwert Änderungen lokal zwischen. Wenn Sie die **Update** -Methode aufrufen, wird der neue Datensatz dem aktuellen **Recordset** hinzugefügt, und die **EditMode** -Eigenschaft wird auf " **adEditNone**" zurückgesetzt, aber der Anbieter sendet die Änderungen nicht an die zugrunde liegende Datenbank, bis Sie die **UpdateBatch** -Methode aufrufen. Wenn Sie die Argumente *FieldList* und *Values* übergeben, sendet ADO den neuen Datensatz zum Speichern in einem Cache an den Anbieter. Sie müssen die **UpdateBatch** -Methode aufrufen, um den neuen Datensatz in der zugrunde liegenden Datenbank zu veröffentlichen. Weitere Informationen zu **Update** und Update **Batch**finden Sie unter [aktualisieren und](../../../ado/guide/data/updating-and-persisting-data.md)beibehalten von Daten.
