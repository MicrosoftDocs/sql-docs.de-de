---
title: Datenbankverweis hinzufügen (Dialogfeld)
description: Hier erfahren Sie mehr über Datenbankverweise und deren Verwendung. Sehen Sie sich die Aktionen an, die Sie im Dialogfeld Datenbankverweis hinzufügen ausführen können.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.adddatabasereference.dialog
- sql.data.tools.newdatabase.dialog
- sql.data.tools.criticalerror.dialog
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: feb7df193ea54f4c7184066bb38cc5cc700f9bf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079661"
---
# <a name="add-database-reference-dialog-box"></a>Datenbankverweis hinzufügen (Dialogfeld)

In diesem Thema werden die Verfahren beschrieben, die Sie im Dialogfeld **Datenbankverweis hinzufügen** ausführen können.  
  
Datenbankverweise ermöglichen folgende Verfahren:  
  
Zugreifen auf Objekte in anderen Datenbanken.  
Ein Projekt kann mithilfe der Auflösung von drei- oder vierteiligen Namen auf eine andere Datenbank auf einem beliebigen Server verweisen. Bei Verwendung eines drei- oder vierteiligen Namens als Verweis können SQLCMD-Variablen verwendet werden, damit Verweise auf mehreren Servern und Datenbanken funktionieren.  
  
Erstellen einer aus mehreren Datenbankprojekten zusammengesetzten Projektmappe.  
In einem zusammengesetzten Projekt wird eine große Datenbank mithilfe von Datenbankverweisen in mehrere separate Projekte aufgeteilt. Sie erstellen einen Verweis ohne Variablen oder Werte für die Datenbank oder den Server (nur mithilfe ein- oder zweiteiliger Namen).  
  
Datenbankverweise können auf ein Datenbankprojekt in der aktuellen Projektmappe oder auf eine DACPAC-Datei verweisen. Durch Hinzufügen eines Datenbankverweises zu einem Projekt werden Projektabhängigkeiten und Buildreihenfolge geändert.  
  
## <a name="selecting-the-database-to-reference"></a>Auswählen der Datenbank für den Verweis

Sie können auf ein anderes Datenbankprojekt in derselben Projektmappe, eine Systemdatenbank oder eine DACPAC-Datei verweisen.  
  
Wenn die Projektmappe mehrere Datenbankprojekte enthält, ist **Datenbankprojekte in der aktuellen Projektmappe** aktiviert. Sie können auf eine andere Datenbank in der Projektmappe verweisen.  
  
Wählen Sie **Systemdatenbank** aus, wenn Sie eine der Systemdatenbanken als Datenbankverweis verwenden möchten.  
  
Wählen Sie **Datenschichtanwendung (DACPAC)** aus, wenn Sie auf eine Datenbank in einer DACPAC-Datei verweisen möchten, und navigieren Sie zu dem Verzeichnis mit der DACPAC-Datei.  
  
## <a name="selecting-the-databases-relative-location"></a>Auswählen des relativen Speicherorts der Datenbank

Nachdem Sie die Datenbank ausgewählt haben, auf die verwiesen werden soll, können Sie den voraussichtlichen Speicherort eines Datenbankobjekts relativ zum verweisenden Projekt angeben.  
  
Verweise können für Objekte an einem der folgenden Orte aufgelöst werden:  
  
- In der Datenbank, die den Verweis enthält.  
  
- In einer anderen Datenbank als der, die den Verweis enthält, die sich aber auf dem gleichen Server befindet.  
  
- In einer anderen Datenbank als der, die den Verweis enthält, die sich aber auf einem anderen Server befindet.  
  
Geben Sie einen Datenbanknamen an. Wenn Sie **Systemdatenbank** ausgewählt haben, sollte Sie den Literal der Systemdatenbank nicht ändern. Wenn Sie **Datenbankprojekte in der aktuellen Projektmappe** auswählen, basiert der Standardname der Datenbank auf dem Namen der Datenbank, die im Projekt enthalten ist.  
  
Wenn Sie **Andere Datenbank, anderer Server** ausgewählt haben, geben Sie einen Servernamen an.  
  
Sie können eine (SQLCMD)-Datenbankvariable verwenden. Wenn Sie mit einer Variablen (anstelle des Literalnamens) auf die Datenbank verweisen möchten, übernehmen Sie die Standardvariable für die Datenbank oder ändern diese. Eine Datenbankvariable ist nützlich, wenn das Datenbankprojekt auf mehreren Servern und Datenbanken veröffentlicht wird. In diesem Fall kann ein Entwickler auf den Eigenschaftenseiten des Projekts **SQLCMD-Variablen** aufrufen und den lokalen Namen der Datenbank angeben. Wenn Sie **Datenbankvariable** leer lassen, kann auf die Datenbank ausschließlich über ihren Literalnamen verwiesen werden. Wenn Sie einen Datenbankvariablennamen angeben, können Sie auf die Datenbank nicht über ihren Literalnamen verweisen.  
  
Wenn Sie **Andere Datenbank, anderer Server** ausgewählt haben, ist eine (SQLCMD-)Servervariable erforderlich. Eine Servervariable ist nützlich, um das Datenbankprojekt auf mehreren Servern und Datenbanken zu veröffentlichen. In diesem Fall kann ein Entwickler auf den Eigenschaftenseiten des Projekts **SQLCMD-Variablen** aufrufen und den lokalen Namen des Servers angeben. Sie können nur über die Variable auf den Server verweisen, nicht über den Servernamen.  
  
> [!IMPORTANT]  
> In bestimmten Situationen können Sie einen Datenbankverweis erstellen, der den gleichen Namen wie ein vorhandener Datenbankverweis besitzt. Wenn zwei Datenbankverweise den gleichen Namen besitzen, kann dies zu unerwartetem Verhalten führen. In diesem Fall löschen Sie beide Datenbankverweise.  
  
## <a name="common-procedures"></a>Allgemeine Verfahren

Im Folgenden sind allgemeine Verfahren aufgeführt:  
  
### <a name="to-create-a-reference-to-a-database-on-the-same-server"></a>So erstellen Sie einen Verweis auf eine Datenbank auf dem gleichen Server  
  
1.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Verweise**, und klicken Sie auf **Datenbankverweis hinzufügen**.  
  
2.  Geben Sie im Dialogfeld **Datenbankverweis hinzufügen** als **Datenbankspeicherort** die Option **Andere Datenbank, derselbe Server** an.  
  
3.  Geben Sie den Namen der Datenbank an.  
  
4.  Überlegen Sie sich, ob Sie eine Datenbankvariable verwenden möchten.  
  
5.  Kopieren Sie das Verwendungsbeispiel, und fügen Sie es in Ihre SQL-Datei ein. Ändern Sie [Schema1] im Verwendungsbeispiel in den Namen Ihres Schemas (z. B. [dbo]). Ändern Sie auch den Namen des Datenbankobjekts in einen für das Projekt geeigneten Namen.  
  
6.  Erstellen Sie die Projektmappe.  
  
### <a name="to-create-a-reference-to-a-database-on-another-server"></a>So erstellen Sie einen Verweis auf eine Datenbank auf einem anderen Server  
  
1.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Verweise**, und klicken Sie auf **Datenbankverweis hinzufügen**.  
  
2.  Geben Sie im Dialogfeld **Datenbankverweis hinzufügen** als **Datenbankspeicherort** die Option **Andere Datenbank, anderer Server** an.  
  
3.  Stellen Sie sicher, dass der Datenbankname richtig ist.  
  
4.  Überlegen Sie sich, ob Sie eine Datenbankvariable verwenden möchten.  
  
5.  Geben Sie den Namen des Servers und die Servervariable an.  
  
6.  Kopieren Sie das Verwendungsbeispiel, und fügen Sie es in Ihre SQL-Datei ein. Ändern Sie [Schema1] im Verwendungsbeispiel in den Namen Ihres Schemas (z. B. [dbo]). Ändern Sie auch den Namen des Datenbankobjekts in einen für das Projekt geeigneten Namen.  
  
7.  Erstellen Sie die Projektmappe.  
  
### <a name="to-create-a-composite-project"></a>So erstellen Sie ein zusammengesetztes Projekt  
  
1.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **Verweise**, und klicken Sie auf **Datenbankverweis hinzufügen**.  
  
2.  Wählen Sie die Quelle der Datenbank aus, auf die Sie verweisen (ein Projekt in der Projektmappe oder eine DACPAC-Datei).  
  
3.  Geben Sie im Dialogfeld **Datenbankverweis hinzufügen** als **Datenbankspeicherort** die Option **Dieselbe Datenbank** an.  
  
4.  Kopieren Sie das Verwendungsbeispiel, und fügen Sie es in Ihre SQL-Datei ein. Ändern Sie [Schema1] im Verwendungsbeispiel in den Namen Ihres Schemas. Ändern Sie auch den Namen des Datenbankobjekts in einen für das Projekt geeigneten Namen.  
  
5.  Erstellen Sie die Projektmappe.  
  
Wenn Sie dieses Projekt veröffentlichen, können Sie zusammengesetzte Projekte in derselben Projektmappe unter einem Ziel bereitstellen:  
  
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Veröffentlichen** aus, um das Dialogfeld **Datenbank veröffentlichen** anzuzeigen.  
  
2.  Klicken Sie im Dialogfeld **Datenbank veröffentlichen** auf **Erweitert**.  
  
3.  Stellen Sie im Dialogfeld **Erweiterte Veröffentlichungseinstellungen** sicher, dass **Zusammengesetzte Objekte einschließen** in der Liste **Erweiterte Bereitstellungsoptionen** aktiviert ist.  
  
## <a name="see-also"></a>Weitere Informationen

[Projektorientierte Offlinedatenbankentwicklung](../ssdt/project-oriented-offline-database-development.md)