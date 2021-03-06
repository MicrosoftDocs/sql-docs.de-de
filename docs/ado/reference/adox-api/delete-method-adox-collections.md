---
description: Delete-Methode (ADOX-Collections)
title: Delete-Methode (ADOX-Auflistungen) | Microsoft-Dokumentation
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Views::Delete
- Groups::Delete
- Indexes::raw_Delete
- Columns::raw_Delete
- Tables::Delete
- Keys::Delete
- Users::Delete
- Users::raw_Delete
- Keys::raw_Delete
- Procedures::raw_Delete
- Views::raw_Delete
- Indexes::Delete
- Procedures::Delete
- Groups::raw_Delete
- Tables::raw_Delete
- Columns::Delete
helpviewer_keywords:
- delete method [ADOX]
ms.assetid: e6b6e3a4-8952-4d79-81f4-51019c338374
author: rothja
ms.author: jroth
ms.openlocfilehash: 67d50c34c55543d0ddb870ad6ba5a1fd8cbd5f11
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054245"
---
# <a name="delete-method-adox-collections"></a>Delete-Methode (ADOX-Collections)
Entfernt ein-Objekt aus einer Auflistung.  
  
## <a name="syntax"></a>Syntax  
  
```  
  
Collection.Delete Name  
```  
  
#### <a name="parameters"></a>Parameter  
 *Name*  
 Eine **Variante** , die den Namen oder die Ordinalposition (Index) des zu löschenden Objekts angibt.  
  
## <a name="remarks"></a>Bemerkungen  
 Wenn der *Name* in der Auflistung nicht vorhanden ist, tritt ein Fehler auf.  
  
 Bei [Tabellen](./tables-collection-adox.md) -und [Benutzer](./users-collection-adox.md) Sammlungen tritt ein Fehler auf, wenn der Anbieter das Löschen von Tabellen oder Benutzern nicht unterstützt. Für [Prozeduren](./procedures-collection-adox.md) und Ansichts [Auflistungen](./views-collection-adox.md) schlägt **Delete** fehl, wenn der Anbieter das Beibehalten von Befehlen nicht unterstützt.  
  
## <a name="applies-to"></a>Gilt für  

:::row:::
    :::column:::
        [Columns-Collection (ADOX)](./columns-collection-adox.md)  
        [Groups-Collection (ADOX)](./groups-collection-adox.md)  
        [Indexes-Collection (ADOX)](./indexes-collection-adox.md)  
    :::column-end:::
    :::column:::
        [Keys-Collection (ADOX)](./keys-collection-adox.md)  
        [Procedures-Collection (ADOX)](./procedures-collection-adox.md)  
        [Tables-Collection (ADOX)](./tables-collection-adox.md)  
    :::column-end:::
    :::column:::
        [Users-Collection (ADOX)](./users-collection-adox.md)  
        [Views-Collection (ADOX)](./views-collection-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Weitere Informationen  
 [Prozeduren Delete-Methode (Beispiel) (VB)](./procedures-delete-method-example-vb.md)   
 [Delete-Methode für Sichten – Beispiel (VB)](./views-delete-method-example-vb.md)