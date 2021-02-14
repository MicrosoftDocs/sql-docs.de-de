---
title: Erstellen einer Java-JAR-Datei aus Klassendateien
description: Packen Sie Klassendateien in eine JAR-Datei, wenn Sie SQL Server-Spracherweiterungen für die Ausführung von Java-Code verwenden.
author: dphansen
ms.author: davidph
ms.date: 11/05/2019
ms.topic: how-to
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: d5ae0b8975e47f64f6ecac8a5b8e2977b7dce93f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081521"
---
# <a name="create-a-java-jar-file-from-class-files"></a>Erstellen einer Java-JAR-Datei aus Klassendateien
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Hier erfahren Sie, wie Sie Klassendateien in eine JAR-Datei packen können, wenn Sie [SQL Server-Spracherweiterungen](../language-extensions-overview.md) für die Ausführung von Java-Code verwenden. Wir empfehlen, die Dateien zu packen.

## <a name="create-a-jar-file"></a>Erstellen einer JAR-Datei

Um eine JAR-Datei aus Klassendateien zu erstellen, navigieren zu dem Ordner, der Ihre Klassendatei enthält, und führen Sie den folgenden Befehl aus:

```cmd
jar -cf <MyJar.jar> *.class
```

Stellen Sie sicher, dass der Pfad zu `jar.exe` Teil der Systempfadvariable ist. Alternativ dazu können Sie auch den vollständigen Pfad zur JAR-Datei angeben, den Sie im JDK-Ordner unter `/bin` finden. Beispiel:

```cmd
C:\Users\MyUser\Desktop\jdk1.8.0_201\bin\jar -cf <MyJar.jar> *.class
```

## <a name="next-steps"></a>Nächste Schritte

+ [Aufrufen der Java-Runtime in SQL Server-Spracherweiterungen](../how-to/call-java-from-sql.md)