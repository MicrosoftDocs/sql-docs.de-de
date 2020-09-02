---
description: Der ODBC-Test ist eine ODBC-fähige Anwendung, mit der Sie ODBC-Treiber und den ODBC-Treiber-Manager testen können.
title: ODBC-Test
ms.custom: ''
ms.date: 09/01/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- ODBC test [ODBC]
- ODBC drivers [ODBC], testing
- gtrtst32.dll
- gtrts32w.dll [ODBC]
- odbct32w.exe [ODBC]
- odbcte32.exe [ODBC]
- testing ODBC drivers [ODBC]
ms.assetid: 7f13894c-5697-436c-be3d-fe16e1a02325
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 39869fe1230be75d9a76e13b8ba3938ec31ee5ee
ms.sourcegitcommit: b6ee0d434b3e42384b5d94f1585731fd7d0eff6f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89288245"
---
# <a name="odbc-test"></a>ODBC-Test

## <a name="about"></a>Info

Bei Microsoft® ODBC-Test handelt es sich um eine ODBC-fähige Anwendung, mit der Sie ODBC-Treiber und den ODBC-Treiber-Manager testen können. Der ODBC-Test ist als Teil des [Microsoft Data Access Components (MDAC) 2,8 Software Development Kit](https://www.microsoft.com/download/details.aspx?id=21995)enthalten.

ODBC 3,51 umfasst sowohl ANSI-als auch Unicode-fähige Versionen von ODBC-Test. Die entsprechenden Dateien lauten wie folgt:

- `Odbcte32.exe` und `Gtrtst32.dll` für die ANSI-Version.

- `Odbct32w.exe` und `Gtrts32w.dll` für die Unicode-Version.

Um den ODBC-Test verwenden zu können, müssen Sie sich mit der ODBC-API, der Programmiersprache C und SQL vertraut machen. Weitere Informationen zur ODBC-API finden Sie in der [ODBC Programmer es Reference](../odbc/reference/odbc-programmer-s-reference.md).

Hilfe Themen, die zuvor in diesem Abschnitt der Dokumentation enthalten waren, sind jetzt im ODBC-Test Programm enthalten. Öffnen Sie `Odbcte32.exe` oder `Odbct32w.exe` , öffnen Sie das Menü **Hilfe** , und klicken Sie dann auf **Hilfe Themen**.

Beachten Sie, dass die 64-Bit-Versionen dieser Anwendungen, die für 64-Bit-Microsoft Windows-Betriebssysteme vorgesehen sind, dieselben Namen wie die 32-Bit-Versionen haben, auch wenn es sich um separate Dateien handelt. Dies bedeutet, dass der Name der Unicode-Version der 64-Bit-Version von ODBC-Test ist `odbct32w.exe` .

## <a name="open-source"></a>Open Source

Der ODBC-Test ist Open Source. Um den Code anzuzeigen und die neueste Version des ODBC-Tests selbst zu erstellen, wechseln Sie zum [GitHub-Repository für ODBC-Test](https://github.com/microsoft/ODBCTest).
