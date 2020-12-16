---
title: Seite „Berechtigungen“ oder „Sicherungsfähige Elemente“ | Microsoft-Dokumentation
description: Verwenden Sie die Seite „Berechtigungen“ oder „Sicherungsfähige Elemente“, um die Berechtigungen für sicherungsfähige Elemente in SQL Server anzuzeigen oder festzulegen.
ms.custom: ''
ms.date: 01/07/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.common.permissions.f1
- sql13.swb.SecurableAndEffectPermissions.f1
- sql13.swb.common.columnperm.f1
- sql13.swb.availabilitygroupproperties.permission.f1
- sql13.swb.SecurableAndEffectivePermission.f1
ms.assetid: b3bf077a-bec2-4161-ac0c-460586199906
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0bc202f99dfc1bcc067b3baf7f7606506f318d3d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479401"
---
# <a name="permissions-or-securables-page"></a>Seite 'Berechtigungen' oder 'Sicherungsfähige Elemente'
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Auf der Seite **Berechtigungen** oder **Sicherungsfähige Elemente** können die Berechtigungen für sicherungsfähige Elemente angezeigt oder festgelegt werden. Diese Seite kann von vielen Orten aus geöffnet werden. Der Inhalt der Seite kann sich geringfügig ändern, je nachdem, wie die Seite geöffnet wird und was sie enthält. Das obere Raster der Seite ist u. U. aufgefüllt, wenn die Seite geöffnet wird, oder es ist leer. Klicken Sie auf **Suchen**, um dem oberen Raster Elemente hinzuzufügen. Wählen Sie im oberen Raster ein Element aus, und legen Sie dann auf der Registerkarte **Explizit** die entsprechenden Berechtigungen fest. Verwenden Sie zum Anzeigen von aggregierten Berechtigungen die Registerkarte **Effektiv** .  
  
 Weitere Informationen zu möglichen Kombinationen aus sicherungsfähigen Elementen und Prinzipalen finden Sie bei den Links zur spezifischen Syntax für sicherungsfähige Elemente im Thema [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md). Weitere Informationen finden Sie unter [Securables](../../relational-databases/security/securables.md).  
  
## <a name="page-header"></a>Seitenheader  
 Der Header der Seite **Berechtigungen** oder **Sicherungsfähige Elemente** variiert je nach sicherungsfähigem Element oder Prinzipal. Er zeigt für das Element relevante Informationen an, z. B. den Namen.  
  
## <a name="upper-grid"></a>Oberes Raster  
 Das obere Raster enthält ein oder mehrere Elemente, für die Berechtigungen festgelegt werden können. Das Dialogfeld enthält die Schaltfläche **Suchen** , über die Sie Objekte oder Prinzipale auswählen können, die Sie dem oberen Raster hinzufügen können. Der Name des Rasters enthält u. U. **Sicherungsfähige Elemente** oder einen oder mehrere Typen an sicherungsfähigen Elementen oder Prinzipalen. Die im oberen Raster angezeigten Spalten variieren je nach Prinzipal oder sicherungsfähigem Element.  
  
 **Name**  
 Die Namen der Prinzipale oder sicherungsfähigen Elemente, die dem Raster hinzugefügt werden.  
  
 **Typ**  
 Beschreibt den Typ jedes Elements.  
  
## <a name="explicit-tab"></a>Registerkarte 'Explizit'  
 Die Registerkarte **Explizit** enthält die möglichen Berechtigungen für die im oberen Raster ausgewählten sicherungsfähigen Elemente. Um die Berechtigungen zu konfigurieren, aktivieren oder deaktivieren Sie die Kontrollkästchen **Erteilen** (oder **Zulassen**), **Mit Erteilung** und **Verweigern** . Für einige explizite Berechtigungen sind nicht alle Optionen verfügbar.  
  
 **Berechtigungen**  
 Der Name der Berechtigung.  
  
 **Grantor**  
 Der Prinzipal, der die Berechtigung erteilt.  
  
 **Erteilen**  
 Aktivieren Sie diese Option, um der Anmeldung diese Berechtigung zu erteilen. Deaktivieren Sie diese Option, um diese Berechtigung aufzuheben.  
  
 **Mit Erteilung**  
 Zeigt den Status der WITH GRANT-Option für die angezeigte Berechtigung an. Dieses Feld ist schreibgeschützt. Verwenden Sie die [GRANT](../../t-sql/statements/grant-transact-sql.md) -Anweisung, um diese Berechtigung anzuwenden.  
  
 **Deny**  
 Aktivieren Sie diese Option, um der Anmeldung diese Berechtigung zu verweigern. Deaktivieren Sie diese Option, um diese Berechtigung aufzuheben.  
  
 **Spaltenberechtigungen**  
 Bei Objekten, die Spalten enthalten (z.B. Tabellen, Sichten oder Tabellenwertfunktionen), wird mit der Schaltfläche **Spaltenberechtigungen** das Dialogfeld **Spaltenberechtigungen** geöffnet. In diesem Dialogfeld können Sie für einzelne Spalten einer Tabelle oder Sicht die Berechtigungen **Erteilen**, **Zulassen** oder **Verweigern** festlegen. Diese Option ist nicht für alle Objekttypen bzw. Berechtigungen verfügbar.  
  
## <a name="effective-tab"></a>Registerkarte 'Effektiv'  
 Die Berechtigungen, die ein Prinzipal einem sicherungsfähigen Element zuordnet, stammen möglicherweise aus Berechtigungen, die für mehrere unterschiedliche Prinzipale festgelegt wurden. Einer Anmeldung könnten beispielsweise einzeln oder als Gruppenmitglied Berechtigungen erteilt werden. Die Registerkarte **Effektiv** zeigt das Ergebnis einer Kombination aus expliziten Berechtigungen und den Berechtigungen, die aus Gruppen- oder Rollenmitgliedschaften empfangen werden. Erteilen-Berechtigungen sind aggregiert. Eine Verweigern-Berechtigung überschreibt alle Erteilen-Berechtigungen.  
  
 **Berechtigungen**  
 Der Name der Berechtigung.  
  
 **Spalte**  
 Die Namen der Spalten, auf die sich die Berechtigung auswirkt.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Rollen auf Datenbankebene](../../relational-databases/security/authentication-access/database-level-roles.md)   
 [Sicherheitscenter für SQL Server-Datenbank-Engine und Azure SQL-Datenbank](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
