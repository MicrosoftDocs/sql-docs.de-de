---
description: Flexible Dateiquelle
title: Flexible Dateiquelle | Microsoft-Dokumentation
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpextfilesrc.f1
- sql14.dts.designer.afpextfilesrc.f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 26f960ea45509a2016d38824474a18ab2ffb88e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339745"
---
# <a name="flexible-file-source"></a>Flexible Dateiquelle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Die Komponente **Flexible Dateiquelle** ermöglicht einem SSIS-Paket das Lesen von Daten aus verschiedenen unterstützten Speicherdiensten.
Zurzeit unterstützte Speicherdienste:

- [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake Storage Gen2](/azure/storage/blobs/data-lake-storage-introduction)
  
Um den Editor für die flexible Dateiquelle aufzurufen, ziehen Sie die **flexible Dateiquelle** auf den Datenfluss-Designer, und doppelklicken Sie darauf, um den Editor zu öffnen.
  
Die **flexible Dateiquelle** ist eine Komponente des [SQL Server Integration Services Feature Pack (SSIS) für Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).  
  
Die folgenden Eigenschaften stehen im **Editor für die flexible Dateiquelle** zur Verfügung.

- **Typ des Verbindungs-Managers:** Gibt den Typ des Quellverbindungs-Managers an. Wählen Sie einen vorhandenen Manager des angegebenen Typs aus, oder erstellen Sie einen neuen.
- **Ordnerpfad:** Gibt den Pfad des Quellordners an.
- **Dateiname:** Gibt den Namen der Quelldatei an.
- **Dateiformat:** Gibt das Format der Quelldatei an. Unterstützte Formate sind **Text**, **Avro**, **ORC**, **Parquet**. Java ist für ORC/Parquet erforderlich. Ausführliche Informationen finden Sie [hier](../../integration-services/azure-feature-pack-for-integration-services-ssis.md#dependency-on-java).
- **Spaltentrennzeichen:** Gibt das als Trennzeichen für Spalten verwendete Zeichen an (Trennzeichen, die aus mehreren Zeichen bestehen, werden nicht unterstützt).
- **Erste Zeile als Spaltenname:** Gibt an, ob die erste Zeile als Spaltenname behandelt werden soll.
- **Datei dekomprimieren:** Gibt an, ob die Quelldatei dekomprimiert werden soll.
- **Komprimierungstyp:** Gibt das Komprimierungsformat der Quelldatei an. Unterstützte Formate sind **GZIP**, **DEFLATE**, **BZIP2**.
  
Die folgenden Eigenschaften stehen im **Erweiterten Editor** zur Verfügung.

- **rowDelimiter:** Das Zeichen, das zum Trennen von Zeilen in einer Datei verwendet wird. Es ist nur ein Zeichen zulässig. Der **Standardwert** ist „\r\n“.
- **escapeChar:** Das Sonderzeichen, mit dem ein Spaltentrennzeichen im Inhalt der Eingabedatei mit Escapezeichen versehen werden kann. Sie können nicht gleichzeitig „escapeChar“ und „quoteChar“ für eine Tabelle angeben. Es ist nur ein Zeichen zulässig. Für dieses Feld gibt es keinen Standardwert.
- **quoteChar:** Das Zeichen, mit dem ein Zeichenfolgenwert in Anführungszeichen gesetzt wird. Die Spalten- und Zeilentrennzeichen innerhalb der Anführungszeichen werden als Teil des Zeichenfolgenwerts behandelt. Diese Eigenschaft gilt sowohl für Eingabe- als auch Ausgabedatasets. Sie können nicht gleichzeitig „escapeChar“ und „quoteChar“ für eine Tabelle angeben. Es ist nur ein Zeichen zulässig. Für dieses Feld gibt es keinen Standardwert.
- **nullValue:** Ein oder mehrere Zeichen, mit denen ein NULL-Wert dargestellt wird. Der **Standardwert** ist „\N“.
- **encodingName:** Geben Sie den Codierungsnamen an. Siehe Eigenschaft [Encoding.EncodingName](/dotnet/api/system.text.encoding).
- **skipLineCount:**  Gibt die Anzahl der nicht leeren Zeilen an, die beim Lesen von Daten aus Eingabedateien übersprungen werden sollen. Wenn „skipLineCount“ und „firstRowAsHeader“ gleichzeitig angegeben sind, werden die Zeilen zuerst übersprungen, und anschließend werden die Kopfzeileninformationen aus der Eingabedatei gelesen.
- **treatEmptyAsNull:** Gibt an, ob Null- oder leere Zeichenfolgen beim Lesen von Daten aus einer Eingabedatei als NULL-Werte behandelt werden sollen. Der Standardwert ist **True**.

Nachdem Sie die Verbindungsinformationen angegeben haben, wechseln Sie zur Seite **Spalten**, um den Zielspalten für den SSIS-Datenfluss Quellspalten zuzuordnen.

**Hinweise zur Konfiguration der Dienstprinzipalberechtigung**

Damit die **Testverbindung** funktioniert (Blob Storage oder Data Lake Storage Gen2), müssen Sie dem Dienstprinzipal mindestens die Rolle **Storage-Blobdatenleser** zuweisen.
Dies erfolgt mit der [RBAC](/azure/storage/common/storage-auth-aad-rbac-portal#assign-rbac-roles-using-the-azure-portal).

Für Blob Storage wird die Leseberechtigung gewährt, indem mindestens die Rolle **Storage-Blobdatenleser** zugewiesen wird.

Für Data Lake Storage Gen2 wird die Berechtigung durch die RBAC und [ACLs](/azure/storage/blobs/data-lake-storage-how-to-set-permissions-storage-explorer) bestimmt.
Beachten Sie, dass ACLs wie [hier](/azure/storage/blobs/data-lake-storage-access-control#how-do-i-set-acls-correctly-for-a-service-principal) beschrieben mithilfe der Objekt-ID (OID) des Dienstprinzipals für die App-Registrierung konfiguriert werden.
Dies unterscheidet sich von der Anwendungs-ID (Client-ID), die mit der RBAC-Konfiguration verwendet wird.
Wenn ein Sicherheitsprinzipal durch eine integrierte Rolle oder eine benutzerdefinierte Rolle RBAC-Datenberechtigungen erhält, werden diese Berechtigungen vor der Autorisierung einer Anforderung zunächst ausgewertet.
Wenn der Anforderungsvorgang von den RBAC-Zuweisungen des Sicherheitsprinzipals autorisiert wurde, wird die Autorisierung sofort aufgelöst, und es werden keine weiteren ACL-Prüfungen durchgeführt.
Wenn der Sicherheitsprinzipal über keine RBAC-Zuweisung verfügt oder der Vorgang der Anforderung nicht mit der zugewiesenen Berechtigung übereinstimmt, werden alternativ ACL-Prüfungen durchgeführt, um zu bestimmen, ob der Sicherheitsprinzipal für die Durchführung des angeforderten Vorgangs autorisiert ist.
Für die Leseberechtigung müssen Sie mindestens die Berechtigung **Execute** (Ausführen) ab dem Quelldateisystem sowie die Berechtigung **Read** (Lesen) für die zu lesenden Dateien gewähren.
Gewähren Sie alternativ mindestens die Rolle **Storage-Blobdatenleser** mit der RBAC.
Weitere Informationen finden Sie in [diesem Artikel](/azure/storage/blobs/data-lake-storage-access-control).