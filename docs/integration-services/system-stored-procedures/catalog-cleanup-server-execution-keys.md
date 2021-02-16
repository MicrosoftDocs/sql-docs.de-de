---
description: catalog.cleanup_server_execution_keys
title: catalog.cleanup_server_execution_keys | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: a79f1006-54e8-4cbf-96f8-5ed143ebb830
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 02872a9142ecb9b010846925f87d9b852a4d4d0b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340426"
---
# <a name="catalogcleanup_server_execution_keys"></a>catalog.cleanup_server_execution_keys 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Löscht Zertifikate und symmetrische Schlüssel aus der SSISDB-Datenbank.  
  
## <a name="syntax"></a>Syntax  
  
```sql
catalog.cleanup_server_execution_keys [ @cleanup_flag = ] cleanup_flag ,  
[ @delete_batch_size = ] delete_batch_size  
```  
  
## <a name="arguments"></a>Argumente  
 [ @cleanup_flag = ] *cleanup_flag*  
 Gibt an, ob Zertifikate und symmetrische Schlüssel auf Ausführungsebene (1) oder Projektebene (2) gelöscht werden sollen.  
  
 Verwenden Sie die Ausführungsebene (1) nur, wenn SERVER_OPERATION_ENCRYPTION_LEVEL auf PER_EXECUTION (1) festgelegt ist.  
  
 Verwenden Sie die Projektebene (2) nur, wenn SERVER_OPERATION_ENCRYPTION_LEVEL auf PER_PROJECT (2) festgelegt ist. Zertifikate und symmetrische Schlüssel werden nur für Projekte gelöscht, die gelöscht und für die die Vorgangsprotokolle bereinigt wurden.  
  
 [ @delete_batch_size = ] *delete_batch_size*  
 Die Anzahl der zu löschenden Schlüssel und Zertifikate. Der Standardwert lautet „1000“.  
  
## <a name="return-code-values"></a>Rückgabecodewerte  
 0 für „erfolgreich“ und 1 für „fehlerhaft“.  
  
## <a name="result-sets"></a>Resultsets  
 Keine.  
  
## <a name="permissions"></a>Berechtigungen  
 Diese gespeicherte Prozedur erfordert eine der folgenden Berechtigungen:  
  
-   READ- und EXECUTE-Berechtigungen für das Projekt und ggf. READ-Berechtigungen für die Umgebung, auf die verwiesen wird.  
  
-   Mitgliedschaft in der Datenbankrolle **ssis_admin**.  
  
-   Mitgliedschaft in der Serverrolle **sysadmin**  
  
## <a name="errors-and-warnings"></a>Fehler und Warnungen  
 Diese gespeicherte Prozedur löst Fehler in den folgenden Szenarien aus:  
  
-   Es gibt mindestens einen aktiven Vorgang in SSISDB.  
  
-   Die SSIS-Datenbank befindet sich nicht im Einzelbenutzermodus.  
  
## <a name="remarks"></a>Bemerkungen  
 In SQL Server 2012 Service Pack 2 wird die Eigenschaft SERVER_OPERATION_ENCRYPTION_LEVEL der Tabelle **internal.catalog_properties** hinzugefügt. Diese Eigenschaft verfügt über zwei mögliche Werte:  
  
-   **PER_EXECUTION (1):** Das Zertifikat und der symmetrische Schlüssel, die zum Schutz von sensiblen Ausführungsparametern und -protokollen verwendet werden, werden für jede Ausführung erstellt. Dies ist der Standardwert. Möglicherweise entstehen Probleme mit der Leistung (Deadlocks, fehlerhafte Wartungsaufträge usw.) in einer Produktionsumgebung, da für jede Ausführung Zertifikate bzw. Schlüssel generiert werden. Trotzdem bietet diese Einstellung ein höheres Maß an Sicherheit als der andere Wert (2).  
  
-   **PER_PROJECT (2):** Das Zertifikat und der symmetrische Schlüssel, die zum Schutz von sensiblen Parametern verwendet werden, werden für jedes Projekt erstellt. Diese Einstellung bietet eine bessere Leistung als die PER_EXECUTION-Ebene, da der Schlüssel und das Zertifikat nur einmal pro Projekt anstatt für jede Ausführung generiert werden.  
  
 Sie müssen die gespeicherte Prozedur [catalog.cleanup_server_log](../../integration-services/system-stored-procedures/catalog-cleanup-server-log.md) ausführen, bevor Sie SERVER_OPERATION_ENCRYPTION_LEVEL von 1 in 2 oder von 2 in 1 ändern können. Führen Sie die folgenden Schritte aus, bevor Sie diese gespeicherte Prozedur ausführen:  
  
1.  Stellen Sie sicher, dass der Wert der Eigenschaft OPERATION_CLEANUP_ENABLED in der Tabelle [catalog.catalog_properties &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md) auf TRUE festgelegt ist.  
  
2.  Legen Sie die Integration Services-Datenbank (SSISDB) auf den Einzelbenutzermodus fest. Starten Sie in SQL Server Management Studio das Dialogfeld „Datenbankeigenschaften“ für SSISDB, wechseln Sie zur Registerkarte „Optionen“, und legen Sie die Eigenschaft „Zugriff beschränken“ auf den Einzelbenutzermodus fest (SINGLE_USER). Nachdem Sie die gespeicherte Prozedur „cleanup_server_log“ ausgeführt haben, setzen Sie den Eigenschaftswert auf den ursprünglichen Wert zurück.  
  
3.  Führen Sie die gespeicherte Prozedur [catalog.cleanup_server_log](../../integration-services/system-stored-procedures/catalog-cleanup-server-log.md) aus.  
  
4.  Nun können Sie den Wert für die Eigenschaft SERVER_OPERATION_ENCRYPTION_LEVEL in der Tabelle [catalog.catalog_properties &#40;SSISDB-Datenbank&#41;](../../integration-services/system-views/catalog-catalog-properties-ssisdb-database.md) ändern.  
  
5.  Führen Sie die gespeicherte [catalog.cleanup_server_execution_keys](../../integration-services/system-stored-procedures/catalog-cleanup-server-execution-keys.md)-Prozedur aus, um Zertifikatschlüssel aus der SSIS-Datenbank zu bereinigen. Das Löschen von Zertifikaten und Schlüssel aus der SSIS-Datenbank nimmt möglicherweise einige Zeit in Anspruch. Aus diesem Grund sollten Sie diesen Vorgang regelmäßig außerhalb der Spitzenzeiten ausführen.  
  
     Sie können auch den Bereich oder die Ebene (Ausführung bzw. Projekt) angeben und festlegen, wie viele Schlüssel gelöscht werden sollen. Die Standardbatchgröße zum Löschen beträgt 1000. Wenn Sie die Ebene auf 2 festlegen, werden die Schlüssel und Zertifikate nur gelöscht, wenn zugehörige Projekte gelöscht wurden.  
  
 Weitere Informationen finden Sie in den folgenden Wissensdatenbankartikeln: [FIX: Leistungsprobleme bei der Verwendung von SSISDB als Bereitstellungsspeicher in SQL Server 2012](https://support.microsoft.com/kb/2972285)  
  
## <a name="example"></a>Beispiel  
 Mithilfe des folgenden Beispiels wird die gespeicherte Prozedur „cleanup_server_execution_keys“ aufgerufen.  
  
```sql  
USE [SSISDB]  
GO  
  
DECLARE@return_value int  
  
EXEC@return_value = [internal].[cleanup_server_execution_keys]  
@cleanup_flag = 1,  
@delete_batch_size = 500  
  
SELECT'Return Value' = @return_value  
  
GO  
```  
  
  
