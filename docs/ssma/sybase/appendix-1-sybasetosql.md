---
description: Anhang 1 (SybaseToSQL)
title: Anhang-1 (sybaseto SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Sybase Console,Appendix
ms.assetid: 6dcfd6d5-772c-4876-aa94-a7f43c4b9d59
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 63088f95aa36ca183a282615a25e6f9a8b29a0dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075068"
---
# <a name="appendix---1-sybasetosql"></a>Anhang 1 (SybaseToSQL)
Schnellansicht der Befehlszeilenoptionen der SSMA-Konsole:  
  
|SL. Nein.|Schalter|Erforderlich?|Switch-Argument|Zulässige Werte|  
|-----------|----------|-------------|-------------------|--------------------|  
|1|-s/Skript|Ja|scriptfile|Gültiger XML-Dateiname.<br /><br />Konsolen Skript-Definitionsdatei.|  
|2|-v/Variable|Nein|variablevaluefile|Gültiger XML-Dateiname.<br /><br />Wenn Variablen in der Skriptdatei verwendet werden, muss diese Datei angegeben werden.|  
|3|-c/Server Connection|Nein|serverconnectionfile|Gültiger XML-Dateiname.<br /><br />Diese Datei enthält Server Verbindungsinformationen.|  
|4|-x/xmloutput|Nein|xmloutputfile|Diese Option gibt die Konsolenausgabe im XML-Format an. Wenn diese Option nicht angegeben wird, wird die Standardausgabe im Text Format angegeben.<br /><br />Wenn xmloutputfile nicht angegeben wird, wird die XML-Ausgabe an stdout weitergeleitet.<br /><br />Xmloutputfile ist der Name der Datei, in die die Konsolenausgabe im XML-Format geschrieben wird.|  
|5|-l/Protokoll|Nein|logfile|Gültiger Dateiname.|  
|6|-e/projectenvironment|Nein|projectenvironmentfolder|Gültiger Ordnername, der SSMA-Projekt Umgebungs Dateien enthält.|  
|7|-p/SecurePassword|Nein|-a/Add {<server_id> [,... n] &#124; alle}-c&#124;Server Connection <Server-Verbindungs Datei> [-v&#124;Variable <Variable-Wert-Datei>] [-o/Überschreibung]<br /><br />oder<br /><br />-a/Add {<server_id> [,... n] &#124; alle}-s&#124;Skript <Skriptdatei> [-v&#124;Variable <Variable-Wert-Datei>] [-o/Überschreibung]<br /><br />-r/Remove {<server_id> [,... n] &#124; alle}<br /><br />-l/Liste<br /><br />-e/Export {<Server-ID> [,... n] &#124; alle} <verschlüsselte Kenn Wort Datei><br /><br />-i/Import {<Server-ID> [,... n] &#124; alle} <verschlüsselte Kenn Wort Datei>|Wenn diese Option angegeben wird, darf Sie nicht mit anderen Optionen kombiniert werden.<br /><br />Server-ID: eine eindeutige ID, die für einen Server {String} angegeben ist.<br /><br />Server-Connection-file: Server Definitionsdatei (serverconnectionfile oder scriptfile).<br /><br />Variable-Wert-file: Dies ist eine Variablen Definitionsdatei und wird in der Server Connection-Datei verwendet.<br /><br />verschlüsselte Kenn Wort Datei: Es handelt sich um eine Datei mit Server Kennwörtern, die mit einem vom Benutzer angegebenen Passphrase verschlüsselt wurde.|  
|8|-?|Nein|Nicht zutreffend|Nicht zutreffend|  
  
## <a name="see-also"></a>Weitere Informationen  
[Ausführen der SSMA-Konsole (Sybase)](./executing-the-ssma-console-sybasetosql.md)  
