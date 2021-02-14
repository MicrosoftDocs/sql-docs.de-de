---
description: Zuordnung von DB2-und SQL Server-Datentypen (DB2ToSQL)
title: Zuordnung von DB2-und SQL Server-Datentypen (DB2ToSQL) | Microsoft-Dokumentation
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: e7e939a8-5e76-4509-beaf-5acd1cab505e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 32cf19b192fe67802ae44eaf9ce6e4ca891d02ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072364"
---
# <a name="mapping-db2-and-sql-server-data-types-db2tosql"></a>Zuordnung von DB2-und SQL Server-Datentypen (DB2ToSQL)
DB2-Datenbanktypen unterscheiden sich von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbanktypen Beim Konvertieren von DB2-Datenbankobjekten in- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Objekte müssen Sie angeben, wie Datentypen aus DB2 zugeordnet werden sollen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Sie können die standardmäßigen Datentyp Zuordnungen akzeptieren, oder Sie können die Zuordnungen anpassen, wie in den folgenden Abschnitten dargestellt.  
  
## <a name="default-mappings"></a>Standardzuordnungen  
SSMA verfügt über einen Standardsatz von Datentyp Zuordnungen. Die Liste der Standard Zuordnungen finden Sie unter [Project Settings &#40;Type Mapping&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-type-mapping-db2tosql.md).  
  
## <a name="type-mapping-inheritance"></a>Vererbung der Typzuordnung  
Sie können Typzuordnungen auf Projektebene, Objekt Kategorieebene (z. b. alle gespeicherten Prozeduren) oder auf Objektebene anpassen. Die Einstellungen werden von der höheren Ebene geerbt, sofern Sie nicht auf einer niedrigeren Ebene überschrieben werden. Wenn Sie z. b. **smallmoney** auf Projektebene zu **Money** zuordnen, verwenden alle Objekte im Projekt diese Zuordnung, es sei denn, Sie passen die Zuordnung auf Objekt-oder Kategorieebene an.  
  
Wenn Sie die Registerkarte **Typzuordnung** in SSMA anzeigen, ist der Hintergrund Farb codiert, um anzuzeigen, welche Typzuordnungen geerbt werden. Der Hintergrund einer Typzuordnung ist für jede geerbte Typzuordnung gelb und weiß für jede Zuordnung, die auf der aktuellen Ebene angegeben wird.  
  
## <a name="customizing-data-type-mappings"></a>Anpassen von Datentyp Zuordnungen  
Im folgenden Verfahren wird gezeigt, wie Datentypen auf Projekt-, Datenbank-oder Objektebene zugeordnet werden:  
  
**So ordnen Sie Datentypen zu**  
  
1.  Um die Datentyp Zuordnung für das gesamte Projekt anzupassen, öffnen Sie das Dialogfeld **Projekteinstellungen** :  
  
    1.  Wählen Sie **im Menü Extras** die Option **Projekteinstellungen** aus.  
  
    2.  Wählen Sie im linken Bereich **Typzuordnung** aus.  
  
        Das Typmapping-Diagramm und die Schaltflächen werden im rechten Bereich angezeigt.  
  
    Um die Datentyp Zuordnung auf der Datenbank-, Tabellen-, Sicht-oder gespeicherten Prozedur Ebene anzupassen, wählen Sie die Datenbank, die Objekt Kategorie oder das Objekt im DB2-metadatenexplorer aus:  
  
    1.  Wählen Sie im DB2-metadatenexplorer den Ordner oder das Objekt zum Anpassen aus.  
  
    2.  Klicken Sie im rechten Bereich auf die Registerkarte **Typzuordnung** .  
  
2.  Gehen Sie folgendermaßen vor, um eine neue Zuordnung hinzuzufügen:  
  
    1.  Klicken Sie auf **Hinzufügen**.  
  
    2.  Wählen Sie unter **Quelltyp** den zuzuordnenden DB2-Datentyp aus.  
  
    3.  Wenn der Typ eine Länge erfordert, geben Sie die minimale Daten Länge für die Zuordnung im Feld **von** und die maximale Daten Länge im Feld **an an** .  
  
        Auf diese Weise können Sie die Datenzuordnung für kleinere und größere Werte desselben Datentyps anpassen.  
  
    4.  Wählen Sie unter **Zieltyp** den Ziel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datentyp aus.  
  
        Einige Typen erfordern eine Länge des Ziel Datentyps. Wenn dies erforderlich ist, geben Sie die neue Daten Länge in das Feld **Ersetzen durch** ein.  
  
    5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
3.  Gehen Sie folgendermaßen vor, um eine Datentyp Zuordnung zu ändern:  
  
    1.  Klicken Sie auf **Bearbeiten**.  
  
    2.  Wählen Sie unter **Quelltyp** den zuzuordnenden DB2-Datentyp aus.  
  
    3.  Wenn der Typ eine Länge erfordert, geben Sie die minimale Daten Länge für die Zuordnung im Feld **von** und die maximale Daten Länge im Feld **an an** .  
  
        Auf diese Weise können Sie die Datenzuordnung für kleinere und größere Werte desselben Datentyps anpassen.  
  
    4.  Wählen Sie unter **Zieltyp** den Ziel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datentyp aus.  
  
        Einige Typen erfordern eine Länge des Ziel Datentyps. Wenn dies erforderlich ist, geben Sie die neue Daten Länge im Feld **Ersetzen durch** ein, und klicken Sie dann auf [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
4.  Gehen Sie folgendermaßen vor, um eine benutzerdefinierte Datentyp Zuordnung zu entfernen:  
  
    1.  Wählen Sie die Zeile in der Liste Typzuordnung aus, die die Datentyp Zuordnung enthält, die Sie entfernen möchten.  
  
    2.  Klicken Sie auf **Entfernen**.  
  
        Geerbte Zuordnungen können nicht entfernt werden. Geerbte Zuordnungen werden jedoch von benutzerdefinierten Zuordnungen für ein bestimmtes Objekt oder eine bestimmte Objekt Kategorie überschrieben.  
  
## <a name="next-steps"></a>Nächste Schritte  
Der nächste Schritt des Migrations Vorgangs besteht darin, entweder die [Bewertung des Berichts &#40;DB2ToSQL&#41;](../../ssma/db2/assessment-report-db2tosql.md) oder [DB2-Schemas &#40;DB2ToSQL&#41;zu ](../../ssma/db2/converting-db2-schemas-db2tosql.md). Wenn Sie einen Bewertungsbericht erstellen, werden DB2-Objekte während der Bewertung automatisch konvertiert.  
  
## <a name="see-also"></a>Weitere Informationen  
[Migrieren von DB2-Datenbanken zu SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
  
