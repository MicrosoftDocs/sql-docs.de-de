---
description: LocalDBUnshareInstance-Funktion
title: Localdbunshareinstance-Funktion | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
apiname:
- LocalDBUnshareInstance
apilocation:
- sqluserinstance.dll
apitype: DLLExport
ms.assetid: 54012ccb-eded-43f7-8ea5-da5ce79224c6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b0b990898d034cc970036f208672ec48a342b69c
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/08/2020
ms.locfileid: "89543988"
---
# <a name="localdbunshareinstance-function"></a>LocalDBUnshareInstance-Funktion
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Beendet die Freigabe der angegebenen SQL Server Express LocalDB-Instanz.  
  
 **Headerdatei:** sqlncli.h  
  
## <a name="syntax"></a>Syntax  
  
```  
HRESULT LocalDBUnShareInstance(  
           PCWSTR pInstanceSharedName,   
           DWORD dwFlags   
);  
```  
  
## <a name="parameters"></a>Parameter  
 *pinstancesharedname*  
 [Eingabe] Der Freigabename für die LocalDB-Instanz, deren Freigabe aufgehoben werden soll.  
  
 *dwFlags*  
 [Eingabe] Zur künftigen Verwendung reserviert. Muss derzeit auf 0 festgelegt sein.  
  
## <a name="returns"></a>Gibt zurück  
 S_OK  
 Die Funktion wurde erfolgreich ausgeführt.  
  
 [LOCALDB_ERROR_NOT_INSTALLED](../../relational-databases/express-localdb-error-messages/localdb-error-not-installed.md)  
 SQL Server Express LocalDB ist nicht auf dem Computer installiert.  
  
 [LOCALDB_ERROR_INVALID_PARAMETER](../../relational-databases/express-localdb-error-messages/localdb-error-invalid-parameter.md)  
 Mindestens ein angegebener Eingabeparameter ist ungültig.  
  
 [LOCALDB_ERROR_INVALID_INSTANCE_NAME](../../relational-databases/express-localdb-error-messages/localdb-error-invalid-instance-name.md)  
 Der angegebene Instanzname ist ungültig.  
  
 [LOCALDB_ERROR_UNKNOWN_INSTANCE](../../relational-databases/express-localdb-error-messages/localdb-error-unknown-instance.md)  
 Die angegebene Instanz ist nicht vorhanden.  
  
 [LOCALDB_ERROR_ADMIN_RIGHTS_REQUIRED](../../relational-databases/express-localdb-error-messages/localdb-error-admin-rights-required.md)  
 Administratorberechtigungen sind erforderlich, um diesen Vorgang auszuführen.  
  
 [LOCALDB_ERROR_INTERNAL_ERROR](../../relational-databases/express-localdb-error-messages/localdb-error-internal-error.md)  
 Ein unerwarteter Fehler ist aufgetreten. Weitere Informationen finden Sie im Ereignisprotokoll.  
  
## <a name="remarks"></a>Hinweise  
 Ein Codebeispiel, in dem die LocalDB-API verwendet wird, finden Sie unter [SQL Server Express LocalDB Reference](../../relational-databases/sql-server-express-localdb-reference.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [SQL Server Express LocalDB-Header und Versionsinformationen](../../relational-databases/express-localdb-instance-apis/sql-server-express-localdb-header-and-version-information.md)  
  
  
