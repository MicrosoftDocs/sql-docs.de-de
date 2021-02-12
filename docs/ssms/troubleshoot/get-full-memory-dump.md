---
description: Erhalten eines vollständigen Speicherabbilds
title: Abrufen eines vollständigen Speicherabbilds zum Troubleshooting in SSMS
Description: Erfassen Sie Diagnoseinformationen vom SQL Server Management Studio (SSMS), sodass Sie einen Absturz oder ein nicht reagierendes System beheben können.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier, sstein
ms.custom: seo-lt-2019
ms.date: 05/17/2019
ms.openlocfilehash: 38e711e9857aea6601878f712c9c5de8cf908e03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017395"
---
# <a name="get-full-memory-dump"></a>Erhalten eines vollständigen Speicherabbilds

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

In diesem Artikel erfahren Sie, wie Sie Diagnoseinformationen erfassen, um eine Problembehandlung durchzuführen, wenn SQL Server Management Studio (SSMS) nicht antwortet oder abgestürzt ist.

Führen Sie zum Erfassen von Diagnoseinformationen für die Problembehandlung die folgenden Schritte aus.

1. Laden Sie [ProcDump](/sysinternals/downloads/procdump) herunter.

2. Entpacken Sie den Download in einen Ordner.

3. Öffnen Sie eine Eingabeaufforderung (wie `cmd.exe`), und führen Sie den folgenden Befehl aus:

    ```console
    <PathToProcDumpFolder>\procdump.exe -e -h -ma -w ssms.exe
    ```

    Sie sollten aufgefordert werden, einem Lizenzvertrag zuzustimmen. Wählen Sie also **Stimme zu** aus.

4. Starten Sie SQL Server Management Studio (SSMS), wenn nicht schon geschehen.

5. Reproduzieren Sie Ihr Problem.

6. In der Eingabeaufforderung sollte Text über das Schreiben der Abbilddatei angezeigt werden. Warten Sie bis dieser Vorgang abgeschlossen ist.

7. Erstellen Sie einen neuen Ordner, und kopieren Sie die geschriebene DMP-Ausgabedatei in diesen Ordner.

8. Kopieren Sie die folgenden Dateien in denselben Ordner.

    * "C:\Windows\Microsoft.NET\Framework\v4.0.30319\mscordacwks.dll"
    * "C:\Windows\Microsoft.NET\Framework\v4.0.30319\SOS.dll"
    * "C:\Windows\Microsoft.NET\Framework\v4.0.30319\clr.dll"

9. Komprimieren Sie den Ordner in eine ZIP-Datei.

## <a name="outofmemoryexception"></a>OutOfMemoryException

Sie können das vollständige Speicherabbild von SSMS auch abrufen, wenn eine „OutOfMemoryException“ ausgelöst wird (es kann sich um jede verwaltete Ausnahme handeln).

Führen Sie zum Erfassen von Diagnoseinformationen für die Problembehandlung einer „OutOfMemoryException“-Ausnahme in SSMS die folgenden Schritte aus.

1. Laden Sie [ProcDump](/sysinternals/downloads/procdump) herunter.

2. Entpacken Sie den Download in einen Ordner.

3. Öffnen Sie eine Eingabeaufforderung, und führen Sie den folgenden Befehl aus.

    ```cmd
    <PathToProcDumpFolder>\procdump.exe -e 1 -f System.OutOfMemoryException -ma -w ssms.exe
    ```

    Sie sollten aufgefordert werden, einem Lizenzvertrag zuzustimmen. Wählen Sie also **Stimme zu** aus.

4. Starten Sie SQL Server Management Studio, wenn nicht schon geschehen.

5. Reproduzieren Sie das Problem.

6. In der Eingabeaufforderung sollte Text über das Schreiben der Abbilddatei angezeigt werden. Warten Sie bis dieser Vorgang abgeschlossen ist.

7. Erstellen Sie einen neuen Ordner, und kopieren Sie die geschriebene DMP-Ausgabedatei in diesen Ordner.

8. Kopieren Sie die folgenden Dateien in denselben Ordner.

    * "C:\Windows\Microsoft.NET\Framework\v4.0.30319\mscordacwks.dll"
    * "C:\Windows\Microsoft.NET\Framework\v4.0.30319\SOS.dll"
    * "C:\Windows\Microsoft.NET\Framework\v4.0.30319\clr.dll"

9. Komprimieren Sie den Ordner in eine ZIP-Datei.

## <a name="share-the-information"></a>Freigeben der Informationen

1. Zum Freigeben der Informationen für das SSMS-Team protokollieren Sie das Problem auf https://aka.ms/sqlfeedback.

2. Geben Sie dann die erfasste Speicherabbilddatei auf OneDrive (oder Ähnlichem) frei, von wo die Datei abgerufen werden kann.

    > [!Important]
    > Speicherabbilddateien können vertrauliche Informationen enthalten.

## <a name="next-steps"></a>Nächste Schritte

[SQL Server Management Studio](../sql-server-management-studio-ssms.md)