---
description: Herstellen einer Verbindung mit DB2 (DB2ToSQL)
title: Herstellen einer Verbindung mit DB2 (DB2ToSQL) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 9d485fd0-ab5d-402a-a59a-e9982a61b7de
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 24ffef4775339baf182b01062f2a06136a04abf1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081571"
---
# <a name="connect-to-db2-db2tosql"></a>Herstellen einer Verbindung mit DB2 (DB2ToSQL)
Verwenden Sie das Dialogfeld **mit DB2 verbinden** , um eine Verbindung mit der DB2-Datenbank herzustellen, die Sie migrieren möchten.  
  
Um auf dieses Dialogfeld zuzugreifen, wählen Sie im Menü **Datei** die Option **mit DB2 verbinden** aus. Wenn Sie bereits eine Verbindung hergestellt haben, stellt der Befehl die **Verbindung mit DB2 wieder** her.  
  
## <a name="options"></a>Tastatur  
**Anbieter**  
Wählen Sie den Datenzugriffs Anbieter für die Verbindung mit der DB2-Datenbank aus. Verfügbare Anbieter sind der DB2-Client Anbieter und der OLE DB-Anbieter. Der Standardwert ist der DB2-Client Anbieter.  
  
**Mode**  
Wählen Sie entweder Standard, TNSNAME oder Verbindungs Zeichen folgen Modus aus.  
  
-   Im Standard Modus geben Sie Werte für den Anbieter, den Servernamen, den Serverport, die DB2-sid, den Benutzernamen und das Kennwort ein bzw. wählen diese aus.  
  
-   Im TNSNAME-Modus geben Sie den Verbindungs Bezeichner (TNS-Alias) der DB2-Datenbank, den Benutzernamen und das Kennwort ein.  
  
-   Im Verbindungs Zeichen folgen Modus geben Sie eine Verbindungs Zeichenfolge an.  
  
    > [!IMPORTANT]  
    > Es wird nicht empfohlen, den Verbindungs Zeichen folgen Modus zu verwenden, da der Text Kenn Wörter enthalten kann, und er wird als Klartext gesendet.  
  
Der Standardmodus ist der Standardmodus.  
  
**Servername**  
Geben Sie den DB2-Servernamen ein. Der Standard Servername ist identisch mit dem Computernamen. Dies ist eine Standard Modus-Option.  
  
**Serverport**  
Wenn Sie für Verbindungen mit DB2 eine andere Portnummer als 1521 (Standard) verwenden, geben Sie die Portnummer ein. Dies ist eine Standard Modus-Option.  
  
**Verbindungs Bezeichner**  
Geben Sie den DB2 Connect-Bezeichner ein. Dies ist der Alias der Datenbank, wie in der lokalen Datei "tnsnames. Ora" definiert.  
  
Dies ist eine Option für den TNSNAME-Modus.  
  
**DB2-sid**  
Geben Sie die sid für die Datenbank ein. Die SID ist ein Bezeichner, der die DB2-Datenbank auf einem Computer unterscheidet. Die Standard-sid für eine Datenbank ist die ersten acht Zeichen des Daten Banknamens.  
  
Dies ist eine Standard Modus-Option.  
  
**Benutzername**  
Geben Sie den Benutzernamen ein, den SSMA für die Verbindung mit der DB2-Datenbank verwendet.  
  
**Kennwort**  
Geben Sie das Kennwort für den Benutzernamen ein.  
  
**Verbindungszeichenfolge**  
> [!IMPORTANT]  
> Es wird nicht empfohlen, den Verbindungs Zeichen folgen Modus zu verwenden, da der Text Kenn Wörter enthalten kann, und er wird als Klartext gesendet.  
  
Wenn Sie den Verbindungs Zeichen folgen Modus verwenden, geben Sie die vollständige Verbindungs Zeichenfolge für die Verbindung mit DB2 ein.  
  
Verbindungs Zeichenfolgen bestehen aus Parameter Name-Wert-Paaren.  
  
-   Informationen OLE DB Verbindungs Zeichenfolgen finden Sie im Artikel [Microsoft OLE DB-Anbieter für DB2](../../ado/guide/appendixes/microsoft-ole-db-provider-for-oracle.md) in der MSDN Library.  
  
Fügen Sie für SSMA-Verbindungs Zeichenfolgen immer den Provider-Parameter ein. Stellen Sie außerdem sicher, dass Sie den Port-Parameter einschließen, wenn Sie eine Verbindung mit DB2 herstellen.  
