---
title: Mapping von Oracle-und SQL Server-Datentypen (oracletosql) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie SSMA für Oracle-Zuordnungen zwischen Oracle-Datentypen und SQL Server anpassen oder die Standardeinstellung übernehmen.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Type Mapping Inheritance
ms.assetid: 05da1495-63b9-47b7-86e2-6746394a2d8a
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 8058123020571ad43e3142fccb920aa22a4b5af5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080781"
---
# <a name="mapping-oracle-and-sql-server-data-types-oracletosql"></a>Zuordnen von Oracle- und SQL Server-Datentypen (OracleToSQL)
Oracle-Datenbanktypen unterscheiden sich von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datenbanktypen Beim Konvertieren von Oracle-Datenbankobjekten in- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Objekte müssen Sie angeben, wie Datentypen aus Oracle zugeordnet werden sollen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Sie können die standardmäßigen Datentyp Zuordnungen akzeptieren, oder Sie können die Zuordnungen anpassen, wie in den folgenden Abschnitten dargestellt.  
  
## <a name="default-mappings"></a>Standardzuordnungen  
SSMA verfügt über einen Standardsatz von Datentyp Zuordnungen. Die Liste der Standard Zuordnungen finden Sie unter [Project Settings &#40;Type Mapping&#41; &#40;oracleto SQL&#41;](../../ssma/oracle/project-settings-type-mapping-oracletosql.md).  
  
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
  
    Wenn Sie die Datentyp Zuordnung auf Datenbank-, Tabellen-, Sicht-oder gespeicherter Prozedur anpassen möchten, wählen Sie im Oracle Metadata Explorer die Datenbank, die Objekt Kategorie oder das Objekt aus:  
  
    1.  Wählen Sie im Oracle Metadata Explorer den Ordner oder das Objekt aus, der angepasst werden soll.  
  
    2.  Klicken Sie im rechten Bereich auf die Registerkarte **Typzuordnung** .  
  
2.  Gehen Sie folgendermaßen vor, um eine neue Zuordnung hinzuzufügen:  
  
    1.  Klicken Sie auf **Hinzufügen**.  
  
    2.  Wählen Sie unter **Quelltyp** den Oracle-Datentyp aus, der zugeordnet werden soll.  
  
    3.  Wenn der Typ eine Länge erfordert, geben Sie die minimale Daten Länge für die Zuordnung im Feld **von** und die maximale Daten Länge im Feld **an an** .  
  
        Auf diese Weise können Sie die Datenzuordnung für kleinere und größere Werte desselben Datentyps anpassen.  
  
    4.  Wählen Sie unter **Zieltyp** den Ziel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datentyp aus.  
  
        Einige Typen erfordern eine Länge des Ziel Datentyps. Wenn dies erforderlich ist, geben Sie die neue Daten Länge in das Feld **Ersetzen durch** ein.  
  
    5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
3.  Gehen Sie folgendermaßen vor, um eine Datentyp Zuordnung zu ändern:  
  
    1.  Klicken Sie auf **Bearbeiten**.  
  
    2.  Wählen Sie unter **Quelltyp** den Oracle-Datentyp aus, der zugeordnet werden soll.  
  
    3.  Wenn der Typ eine Länge erfordert, geben Sie die minimale Daten Länge für die Zuordnung im Feld **von** und die maximale Daten Länge im Feld **an an** .  
  
        Auf diese Weise können Sie die Datenzuordnung für kleinere und größere Werte desselben Datentyps anpassen.  
  
    4.  Wählen Sie unter **Zieltyp** den Ziel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Datentyp aus.  
  
        Einige Typen erfordern eine Länge des Ziel Datentyps. Wenn dies erforderlich ist, geben Sie die neue Daten Länge im Feld **Ersetzen durch** ein, und klicken Sie dann auf [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
4.  Gehen Sie folgendermaßen vor, um eine benutzerdefinierte Datentyp Zuordnung zu entfernen:  
  
    1.  Wählen Sie die Zeile in der Liste Typzuordnung aus, die die Datentyp Zuordnung enthält, die Sie entfernen möchten.  
  
    2.  Klicken Sie auf **Entfernen**.  
  
        Geerbte Zuordnungen können nicht entfernt werden. Geerbte Zuordnungen werden jedoch von benutzerdefinierten Zuordnungen für ein bestimmtes Objekt oder eine bestimmte Objekt Kategorie überschrieben.  
  
## <a name="next-steps"></a>Nächste Schritte  
Der nächste Schritt des Migrations Vorgangs besteht darin, entweder [einen Bewertungsbericht zu erstellen](assessing-oracle-schemas-for-conversion-oracletosql.md) oder [Oracle-Datenbankobjekte in SQL Server-Syntax zu konvertieren](converting-oracle-schemas-oracletosql.md). Wenn Sie einen Bewertungsbericht erstellen, werden Oracle-Objekte während der Bewertung automatisch konvertiert.  
  
## <a name="see-also"></a>Weitere Informationen  
[Migrieren von Oracle-Datenbanken zu SQL Server &#40;oracleto SQL-&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
