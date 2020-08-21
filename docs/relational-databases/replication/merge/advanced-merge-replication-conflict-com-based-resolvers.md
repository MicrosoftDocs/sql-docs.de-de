---
description: Erweiterte Konflikte bei der Mergereplikation – COM-basierte Konfliktlöser
title: Microsoft COM-basierte Konfliktlöser | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- COM-based resolvers [SQL Server replication]
- custom resolvers [SQL Server replication]
ms.assetid: a6637e4b-4e6b-40aa-bee6-39d98cc507c8
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 20b7e91ad9b6ce87cbfecc266d1eae1e70d1780d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88465247"
---
# <a name="advanced-merge-replication-conflict---com-based-resolvers"></a>Erweiterte Konflikte bei der Mergereplikation – COM-basierte Konfliktlöser
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Alle COM-basierten Konfliktlöser in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] behandeln Aktualisierungskonflikte und gegebenenfalls auch Einfügungs- und Löschkonflikte. Sie alle behandeln das Protokollieren auf Spaltenebene und größtenteils auch das Protokollieren auf Zeilenebene. Diese und alle anderen COM-basierten Konfliktlöser deklarieren die Konflikttypen, die sie behandeln können; der Merge-Agent verwendet den Standardkonfliktlöser für alle anderen Konflikttypen.  
  
 Die Konfliktlöser werden während des Installationsprozesses für [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]installiert. Führen Sie die gespeicherte Prozedur **sp_enumcustomresolvers** aus, um alle auf dem Computer registrierten Konfliktlöser anzuzeigen. Durch das Ausführen der Prozedur wird die Beschreibung und der global eindeutige Bezeichner (Globally Unique Identifier, GUID) für jeden Konfliktlöser in einem separaten Resultset angezeigt.  
  
 Informationen zum Angeben eines Konfliktlösers finden Sie unter [Specify a Merge Article Resolver](../../../relational-databases/replication/publish/specify-a-merge-article-resolver.md).  
  
 In der folgenden Tabelle werden die Attribute der bestimmten Konfliktlöser beschrieben.  
  
|Name|Erforderliche Eingabe|BESCHREIBUNG|Kommentare|  
|----------|--------------------|-----------------|--------------|  
|[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Konfliktlöser „Zusatz“|Name der zu summierenden Spalte. Sie muss einen arithmetischen Datentyp (wie z. B. **int**, **smallint**, **numeric**usw.) aufweisen.|Der Gewinner des Konflikts wird anhand des priority-Wertes ermittelt. Angegebene Spaltenwerte werden auf die Summe der Quelle und der Zielspaltenwerte festgelegt. Wenn ein Wert auf NULL festgelegt ist, werden sie auf den Wert der anderen Spalte festgelegt.|Unterstützt Updatekonflikte, nur Spaltenprotokollierung.|  
|[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Konfliktlöser „Mittelwerterstellung“|Der Name der Spalte, deren Mittelwert ermittelt werden soll. Sie muss einen arithmetischen Datentyp (wie z. B. **int**, **smallint**, **numeric**usw.) aufweisen.|Der Gewinner des Konflikts wird anhand des priority-Wertes ermittelt. Resultierende Spaltenwerte werden auf den Mittelwert der Quelle und der Zielspaltenwerte festgelegt. Wenn ein Wert auf NULL festgelegt ist, werden sie auf den Wert der anderen Spalte festgelegt.|Unterstützt Updatekonflikte, nur Spaltenprotokollierung.|  
|[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Konfliktlöser „DATETIME (früher gewinnt)“|Name der Spalte, die zum Bestimmen des Konfliktgewinners verwendet wird. Sie muss einen **datetime** -Datentyp aufweisen.|Die Spalte mit dem früheren **datetime** -Wert bestimmt den Konfliktgewinner. Wenn für einen Wert NULL festgelegt ist, ist die Zeile mit dem anderen Wert der Gewinner.|Unterstützt Updatekonflikte, Zeilen- und Spaltenprotokollierung. Die Spaltenwerte werden direkt verglichen; eine Anpassung für verschiedene Zeitzonen wird nicht vorgenommen.|  
|[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Konfliktlöser „DATETIME (später gewinnt)“|Name der Spalte, die zum Bestimmen des Konfliktgewinners verwendet wird. Sie muss einen **datetime** -Datentyp aufweisen.|Die Spalte mit dem späteren **datetime** -Wert bestimmt den Konfliktgewinner. Wenn für einen Wert NULL festgelegt ist, ist die Zeile mit dem anderen Wert der Gewinner.|Unterstützt Updatekonflikte, Zeilen- und Spaltenprotokollierung.|  
|[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Konfliktlöser „Maximum“|Name der Spalte, die zum Bestimmen des Konfliktgewinners verwendet wird. Sie muss einen arithmetischen Datentyp (wie z. B. **int**, **smallint**, **numeric**usw.) aufweisen.|Die Spalte mit dem höheren nummerischen Wert bestimmt den Konfliktgewinner. Wenn für einen Wert NULL festgelegt ist, ist die Zeile mit dem anderen Wert der Gewinner.|Unterstützt Zeilen- und Spaltenprotokollierung.|  
|[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Konfliktlöser „Minimum“|Name der Spalte, die zum Bestimmen des Konfliktgewinners verwendet wird. Sie muss einen arithmetischen Datentyp (wie z. B. **int**, **smallint**, **numeric**usw.) aufweisen.|Die Spalte mit dem niedrigeren nummerischen Wert bestimmt den Konfliktgewinner. Wenn für einen Wert NULL festgelegt ist, ist die Zeile mit dem anderen Wert der Gewinner.|Unterstützt Updatekonflikte, Zeilen- und Spaltenprotokollierung.|  
|[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Konfliktlöser „Textspalten zusammenführen“|Name der Textspalte und Trennzeichen, z. B. `@resolver_info = '[col1][===]'`.|Der Gewinner des Konflikts wird anhand des priority-Wertes ermittelt. Konflikt verursachende Textspalten werden auf einen zusammengeführten Wert festgelegt, der aus einem gemeinsamen Präfix gefolgt von einem eindeutigen Teil des Verlegers, dem Trennzeichen und einem eindeutigen Teil des Abonnenten besteht.|Unterstützt Updatekonflikte, nur Spaltenprotokollierung.|  
|[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Konfliktlöser „Abonnent gewinnt immer“|Keine Eingaben.|Der Abonnent ist der Gewinner, unabhängig davon, ob er Quelle oder Ziel ist.|Unterstützt alle Konflikttypen.|  
|[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Konfliktlöser „Priorität“|Name der Spalte, die zum Bestimmen des Konfliktgewinners verwendet wird. Sie muss einen arithmetischen Datentyp (wie z. B. **int**, **smallint**, **numeric**usw.) aufweisen.|Die Spalte mit dem höheren nummerischen Wert bestimmt den Konfliktgewinner. Wenn für einen Wert NULL festgelegt ist, ist die Zeile mit dem anderen Wert der Gewinner.|Unterstützt Updatekonflikte, Zeilen- und Spaltenprotokollierung.|  
|[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Konfliktlöser „Nur hochladen“|Keine Eingaben.|Für den Verleger hochgeladene Änderungen werden akzeptiert; Änderungen werden nicht auf den Abonnenten heruntergeladen.|Unterstützt alle Konflikttypen.|  
|[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]-Konfliktlöser „Nur Herunterladen“|Keine Eingaben.|Für den Verleger hochgeladene Änderungen werden abgelehnt; Änderungen werden auf den Abonnenten heruntergeladen.|Unterstützt alle Konflikttypen.|  
|[!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLServer-Konfliktlöser für gespeicherte Prozeduren|Name der gespeicherten Prozedur, die der Konfliktlöser zur Problembehandlung aufrufen soll.|Die Konfliktlösung hängt von der Logik in der von Ihnen angegebenen Prozedur ab.|Updatekonflikte werden unterstützt. Weitere Informationen finden Sie unter [Implementieren eines benutzerdefinierten Konfliktlösers für einen Mergeartikel](../../../relational-databases/replication/implement-a-custom-conflict-resolver-for-a-merge-article.md)|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erweiterte Konflikterkennung und -lösung der Mergereplikation](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)   
 [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md)  
  
  
