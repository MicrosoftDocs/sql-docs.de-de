---
description: MSSQLSERVER_6602
title: MSSQLSERVER_6602
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 6602 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 594898082fb21f712cfb99abfb535aea46b5164d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348725"
---
# <a name="mssqlserver_6602"></a>MSSQLSERVER_6602
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Details

|attribute|Wert|
|---|---|
|Produktname|SQL Server|
|Ereignis-ID|6602|
|Ereignisquelle|MSSQLSERVER|
|Komponente|SQLEngine|
|Symbolischer Name|XMLERR_PARSEERR2|
|Meldungstext|Die Fehlerbeschreibung lautet '%.*ls'.|
||

## <a name="explanation"></a>Erklärung

Dieser Fehler tritt auf, wenn Sie versuchen, eine gespeicherte `sp_xml_preparedocument`-Prozedur, bei der der Inhalt des Parameters `xmltext` ein komplexes XML-Dokument ist, in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] auszuführen. Dem Benutzer wird eine Fehlermeldung angezeigt, die der folgenden ähnelt:

> XML-Analysefehler 0x80004005 bei Zeilennummer 1 in der Nähe des XML-Texts „\<XML document sample>“  
Meldung 6602, Ebene 16, Status 2, Prozedur sp_xml_preparedocument, Zeile 1  
Die Fehlerbeschreibung ist „Nicht angegebener Fehler“.

## <a name="cause"></a>Ursache

Dieses Problem tritt aufgrund einer Entwurfsgrenzwerts des MSXML-Parsers (Msxmlsql.dll) auf, der von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet wird.

Das Problem hängt nicht unbedingt mit der Größe des XML-Dokuments zusammen, sondern mit seiner komplexen Struktur. Eine Kombination aus der Strukturtiefe des XML-Elements, der Anzahl und Größe der Attribute und der Anzahl der Entitäten innerhalb der Attribute kann dieses Problem verursachen. Der Komplexitätsgrad, der dazu führt, dass dieser Grenzwert erreicht wird, tritt jedoch in XML-Dokumenten auf, die mehrere Megabyte groß sind.

## <a name="user-action"></a>Benutzeraktion

Um dieses Problem zu umgehen, versuchen Sie, die Komplexität des XML-Dokuments zu verringern.

> [!NOTE]
> Versuchen Sie, sehr große Attribute mit einzelnen Zeichenfolgen zu vermeiden, die eine große Menge an XML\Entitäten enthalten.
