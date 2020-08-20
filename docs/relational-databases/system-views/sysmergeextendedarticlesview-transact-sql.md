---
description: sysmergeextendedarticlesview (Transact-SQL)
title: sysmergeextendedarticlesview (Transact-SQL) | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergeextendedarticlesview
- sysmergeextendedarticlesview_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergeextendedarticlesview view
ms.assetid: bd5c8414-5292-41fd-80aa-b55a50ced7e2
author: stevestein
ms.author: sstein
ms.openlocfilehash: f5f400a8e08d242427ca5101461ccb9dacd58310
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88485453"
---
# <a name="sysmergeextendedarticlesview-transact-sql"></a>sysmergeextendedarticlesview (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Die **sysmergeextendedarticlesview** -Sicht macht Artikel Informationen verfügbar. Diese Sicht wird in der Veröffentlichungsdatenbank auf dem Verleger und in der Abonnementdatenbank auf dem Abonnenten gespeichert.  
  
|Spaltenname|Datentyp|BESCHREIBUNG|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Der Name des Artikels.|  
|**type**|**tinyint**|Gibt den Artikeltyp an, der einen der folgenden Werte aufweisen kann:<br /><br /> **10** = Tabelle.<br /><br /> **32** = nur proc-Schema.<br /><br /> **64** = nur Schema anzeigen oder Schema der indizierten Sicht.<br /><br /> **128** = nur Funktions Schema.<br /><br /> **160** = nur Synonym Schema.|  
|**objid**|**int**|Der Bezeichner des Verlegerobjekts.|  
|**sync_objid**|**int**|Der Bezeichner der Sicht, die das synchronisierte Dataset darstellt.|  
|**view_type**|**tinyint**|Der Typ der Sicht:<br /><br /> **0** = keine Ansicht; Verwenden Sie das gesamte Basisobjekt.<br /><br /> **1** = permanente Ansicht.<br /><br /> **2** = temporäre Ansicht.|  
|**artid**|**uniqueidentifier**|Die eindeutige ID des angegebenen Artikels.|  
|**Beschreibung**|**nvarchar(255)**|Eine kurze Beschreibung des Artikels.|  
|**pre_creation_command**|**tinyint**|Die Standardaktion, die durchgeführt wird, wenn der Artikel in der Abonnementdatenbank erstellt wird:<br /><br /> **0** = None: Wenn die Tabelle bereits auf dem Abonnenten vorhanden ist, wird keine Aktion ausgeführt.<br /><br /> **1** = Drop-löscht die Tabelle, bevor Sie neu erstellt wird.<br /><br /> **2** = DELETE: gibt einen Löschvorgang basierend auf der WHERE-Klausel im Teilmengen Filter aus.<br /><br /> **3** = Abschneiden-identisch mit 2, löscht jedoch Seiten anstelle von Zeilen. Eine WHERE-Klausel wird jedoch nicht verwendet.|  
|**pubid**|**uniqueidentifier**|Die ID der Veröffentlichung, zu der der aktuelle Artikel gehört.|  
|**Namen**|**int**|Die Spitznamenzuordnung zur Identifikation des Artikels.|  
|**column_tracking**|**int**|Zeigt an, ob die Spaltenprotokollierung für den Artikel implementiert wurde.|  
|**status**|**tinyint**|Zeigt den Status des Artikels an. Die folgenden Werte sind möglich:<br /><br /> **1** = nicht synchronisiert: das Anfangs Verarbeitungs Skript zum Veröffentlichen der Tabelle wird bei der nächsten Ausführung des Momentaufnahmen-Agent ausgeführt.<br /><br /> **2** = aktiv: das Anfangs Verarbeitungs Skript zum Veröffentlichen der Tabelle wurde ausgeführt.<br /><br /> **5** = New_inactive hinzuzufügen.<br /><br /> **6** = New_active hinzuzufügen.|  
|**conflict_table**|**sysname**|Der Name der lokalen Tabelle, die die Konflikt verursachenden Datensätze für den aktuellen Artikel enthält. Diese Tabelle dient nur zu Informationszwecken; ihr Inhalt kann mit benutzerdefinierten Konfliktlösungsroutinen oder direkt vom Administrator geändert oder gelöscht werden.|  
|**creation_script**|**nvarchar(255)**|Das Erstellungsskript für diesen Artikel.|  
|**conflict_script**|**nvarchar(255)**|Das Konfliktskript für diesen Artikel.|  
|**article_resolver**|**nvarchar(255)**|Der benutzerdefinierte Konfliktlöser auf Zeilenebene für diesen Artikel.|  
|**ins_conflict_proc**|**sysname**|Die Prozedur, die zum Schreiben von Konflikten in **conflict_table**verwendet wird.|  
|**insert_proc**|**sysname**|Die Prozedur, die vom Standardkonfliktlöser zum Einfügen von Zeilen während der Synchronisierung verwendet wird.|  
|**update_proc**|**sysname**|Die Prozedur, die vom Standardkonfliktlöser zum Aktualisieren von Zeilen während der Synchronisierung verwendet wird.|  
|**select_proc**|**sysname**|Der Name einer automatisch generierten gespeicherten Prozedur, die der Merge-Agent verwendet, um Sperren einzurichten bzw. um Spalten und Zeilen für einen Artikel zu finden.|  
|**schema_option**|**Binär (8)**|Die unterstützten Werte *schema_option*finden Sie unter [sp_addmergearticle &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
|**destination_object**|**sysname**|Der Name der auf dem Abonnenten erstellten Tabelle.|  
|**resolver_clsid**|**nvarchar(50)**|Die ID des benutzerdefinierten Konfliktlösers.|  
|**subset_filterclause**|**nvarchar (1000)**|Die Filterklausel für diesen Artikel.|  
|**missing_col_count**|**int**|Die Anzahl der fehlenden Spalten.|  
|**missing_cols**|**varbinary(128)**|Das Bitmap der fehlenden Spalten.|  
|**Spalten**|**varbinary(128)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**resolver_info**|**nvarchar(255)**|Speicherplatz für zusätzliche vom benutzerdefinierten Konfliktlöser benötigte Informationen.|  
|**view_sel_proc**|**nvarchar (290)**|Der Name einer gespeicherten Prozedur, die der Merge-Agent zum ersten Auffüllen eines Artikels in einer dynamisch gefilterten Veröffentlichung und zum Auflisten von geänderten Zeilen in einer beliebigen gefilterten Veröffentlichung verwendet.|  
|**gen_cur**|**int**|Die Generierungsnummer für lokale Änderungen an der Basistabelle eines Artikels.|  
|**excluded_cols**|**varbinary(128)**|Das Bitmap der Spalten, die vom Artikel ausgeschlossen werden, wenn dieser an den Abonnenten gesendet wird.|  
|**excluded_col_count**|**int**|Die Anzahl der ausgeschlossenen Spalten.|  
|**vertical_partition**|**int**|Gibt an, ob die Spaltenfilterung für einen Tabellenartikel aktiviert ist. **0** gibt an, dass keine vertikale Filterung vorhanden ist und alle Spalten veröffentlicht werden.|  
|**identity_support**|**int**|Gibt an, ob die automatische Verarbeitung der Identitätsbereiche aktiviert ist. **1** bedeutet, dass die Identitäts Bereichs Behandlung aktiviert ist, und **0** bedeutet, dass keine Identitäts Bereichs Unterstützung vorhanden ist.|  
|**destination_owner**|**sysname**|Der Name des Besitzers des Zielobjekts.|  
|**before_image_objid**|**int**|Die Objekt-ID der Nachverfolgungstabelle. Die Nachverfolgungstabelle enthält bestimmte Schlüsselspaltenwerte, wenn eine Veröffentlichung so konfiguriert ist, dass Optimierungen von Partitionsänderungen aktiviert sind.|  
|**before_view_objid**|**int**|Die Objekt-ID einer Sichttabelle. Die Sicht ist für eine Tabelle festgelegt, die überwacht, ob eine Zeile zu einem bestimmten Abonnenten gehört hat, bevor sie gelöscht oder aktualisiert wurde. Gilt nur, wenn eine Veröffentlichung mit * \@ keep_partition_changes*  =  **true**erstellt wird.|  
|**verify_resolver_signature**|**int**|Gibt an, ob eine digitale Signatur überprüft wird, bevor ein Konfliktlöser in einer Mergereplikation verwendet wird:<br /><br /> **0** = Signatur wird nicht überprüft.<br /><br /> **1** = Signatur wird überprüft, um festzustellen, ob Sie von einer vertrauenswürdigen Quelle ist.|  
|**allow_interactive_resolver**|**bit**|Gibt an, ob die Verwendung des interaktiven Konfliktlösers für einen Artikel aktiviert ist. der Wert **1** gibt an, dass der interaktive Konflikt Löser für den Artikel verwendet wird.|  
|**fast_multicol_updateproc**|**bit**|Gibt an, ob der Merge-Agent aktiviert wurde, um in einer UPDATE-Anweisung Änderungen auf mehrere Spalten in derselben Zeile anzuwenden.<br /><br /> **0** = gibt ein separates Update für jede geänderte Spalte aus.<br /><br /> **1** = wird bei der Update-Anweisung ausgegeben, wodurch Aktualisierungen in mehreren Spalten in einer Anweisung auftreten.|  
|**check_permissions**|**int**|Die Bitmap der Berechtigungen auf Tabellenebene, die überprüft werden, wenn der Merge-Agent Änderungen auf den Verleger anwendet. *check_permissions* kann einen der folgenden Werte aufweisen:<br /><br /> **0x00** = Berechtigungen werden nicht geprüft.<br /><br /> **0x10** = überprüft Berechtigungen auf dem Verleger, bevor auf einem Abonnenten vorgenommene Einfügungen hochgeladen werden können.<br /><br /> **0x20** = überprüft Berechtigungen auf dem Verleger, bevor auf einem Abonnenten vorgenommene Aktualisierungen hochgeladen werden können.<br /><br /> **0x40** = überprüft Berechtigungen auf dem Verleger, bevor auf einem Abonnenten ausgeführte Löschvorgänge hochgeladen werden können.|  
|**maxversion_at_cleanup**|**int**|Die höchste Generierung, für die ein Cleanup der Metadaten ausgeführt wird.|  
|**processing_order**|**int**|Gibt die Verarbeitungsreihenfolge von Artikeln in einer Mergeveröffentlichung an. der Wert **0** gibt an, dass der Artikel nicht sortiert ist, und die Artikel werden in der Reihenfolge vom niedrigsten zum höchsten Wert verarbeitet. Wenn zwei Artikel denselben Wert haben, werden sie gleichzeitig verarbeitet. Weitere Informationen finden Sie unter [Specify Merge Replication properties (Angeben von Mergereplikationseigenschaften)](../../relational-databases/replication/merge/specify-merge-replication-properties.md).|  
|**published_in_tran_pub**|**bit**|Gibt an, dass ein Artikel in einer Mergeveröffentlichung auch in einer Transaktionsveröffentlichung veröffentlicht wird.<br /><br /> **0** = der Artikel wird nicht in einem transaktionalen Artikel veröffentlicht.<br /><br /> **1** = der Artikel wird auch in einem transaktionalen Artikel veröffentlicht.|  
|**upload_options**|**tinyiny**|Definiert, ob Änderungen auf dem Abonnenten vorgenommen oder von diesem hochgeladen werden können. Die folgenden Werte sind möglich.<br /><br /> **0** = es gibt keine Einschränkungen für Updates, die auf dem Abonnenten vorgenommen wurden. Alle Änderungen werden auf den Verleger hochgeladen.<br /><br /> **1** = Änderungen sind auf dem Abonnenten zulässig, werden jedoch nicht auf den Verleger hochgeladen.<br /><br /> **2** = Änderungen sind auf dem Abonnenten nicht zulässig.|  
|**Gewicht**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**delete_proc**|**sysname**|Die Prozedur, die vom Standardkonfliktlöser zum Löschen von Zeilen während der Synchronisierung verwendet wird.|  
|**before_upd_view_objid**|**int**|Die ID der Sicht einer Tabelle vor Updates.|  
|**delete_tracking**|**bit**|Zeigt an, ob Löschvorgänge repliziert werden.<br /><br /> **0** = Löschvorgänge werden nicht repliziert.<br /><br /> **1** = Löschvorgänge werden repliziert. Dies ist das Standardverhalten für die Mergereplikation.<br /><br /> Wenn der Wert von *delete_tracking* **0**ist, müssen auf dem Abonnenten gelöschte Zeilen manuell auf dem Verleger entfernt werden, und auf dem Verleger gelöschte Zeilen müssen manuell auf dem Abonnenten entfernt werden.<br /><br /> Hinweis: der Wert **0** führt zu einer nicht Konvergenz.|  
|**compensate_for_errors**|**bit**|Zeigt an, ob kompensierende Aktionen ausgeführt werden, wenn während der Synchronisierung Fehler auftreten.<br /><br /> **0** = kompensierende Aktionen sind deaktiviert.<br /><br /> **1** = Änderungen, die nicht auf einem Abonnenten oder Verleger angewendet werden können, führen immer zu kompensierenden Aktionen, um diese Änderungen rückgängig zu machen. Dies ist das Standardverhalten der Mergereplikation.<br /><br /> Hinweis: der Wert **0** führt zu einer nicht Konvergenz.|  
|**pub_range**|**bigint**|Die Größe des Identitätsbereichs für den Verleger.|  
|**range**|**bigint**|Die Bereichsgröße der aufeinander folgenden Identitätswerte, die Abonnenten bei einer Anpassung zugewiesen würden.|  
|**threshold**|**int**|Als Prozentsatz angegebener Schwellenwert für den Identitätsbereich.|  
|**metadata_select_proc**|**sysname**|Der Name der automatisch generierten gespeicherten Prozedur, mit der in den Systemtabellen der Mergereplikation auf Metadaten zugegriffen wird.|  
|**stream_blob_columns**|**bit**|Gibt an, ob eine Datenstrom Optimierung beim Replizieren Binary Large Object Spalten verwendet wird. **1** bedeutet, dass die Optimierung versucht wird.|  
|**preserve_rowguidcol**|**bit**|Zeigt an, ob die Replikation eine vorhandene rowguid-Spalte verwendet. Der Wert **1** bedeutet, dass eine vorhandene ROWGUIDCOL-Spalte verwendet wird. **0** bedeutet, dass die Replikation die ROWGUIDCOL-Spalte hinzugefügt hat.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Replikations Tabellen &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Replikations Sichten &#40;Transact-SQL-&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addmergearticle &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)   
 [sp_changemergearticle &#40;Transact-SQL-&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)   
 [sp_helpmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)   
 [sysmergearticles &#40;Transact-SQL-&#41;](../../relational-databases/system-tables/sysmergearticles-transact-sql.md)  
  
  
