---
description: Artikelprobleme
title: Probleme mit Artikeln | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.articleissues.f1
ms.assetid: bde57da2-dd47-412f-9df7-9224968b2448
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 53df3848ffac7b7408d90b48d74a27410cd2a155
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467271"
---
# <a name="article-issues"></a>Artikelprobleme
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
   Auf der Seite **Artikelprobleme** werden die für Artikel ermittelten Bedingungen aufgeführt sowie alle Änderungen, die als Folge dieser Bedingungen erforderlich sind. In der folgenden Tabelle werden mögliche Probleme und Aktionen aufgeführt, die erforderlich sind, um die richtige Funktionsweise von Replikation und vorhandenen Anwendungen sicherzustellen.  
  
|Artikelproblem|Details|Erforderliche Aktion|  
|-------------------|-------------|---------------------|  
|Den Tabellen werden Uniqueidentifier-Spalten hinzugefügt.|Bei der Replikation wird für alle Artikel einer Merge- oder Transaktionsveröffentlichung, die aktualisierbare Abonnements zulassen, eine Spalte vom Datentyp **Uniqueidentifier** benötigt.|Veröffentlichten Tabellen, die beim Generieren der ersten Momentaufnahme keine Spalte vom Datentyp **Uniqueidentifier** besitzen, wird diese Spalte bei der Replikation automatisch hinzugefügt. Sie sollten sicherstellen, dass in den Anweisungen INSERT und UPDATE, die auf diese Tabellen verweisen, Spaltenlisten verwendet werden. Darüber hinaus sollten Sie sicherstellen, dass auf dem Datenträger genügend Speicherplatz für die zusätzliche Spalte verfügbar ist.|  
|IDENTITY-Spalten benötigen die Option NOT FOR REPLICATION.|Bei der Replikation müssen alle IDENTITY-Spalten die Option NOT FOR REPLICATION verwenden. Wenn eine veröffentlichte IDENTITY-Spalte diese Option nicht verwendet, werden INSERT-Befehle gegebenenfalls nicht richtig repliziert.|Gilt für Veröffentlichungen, die auf Verlegern mit [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] oder früher erstellt wurden. Geben Sie für alle IDENTITY-Spalten die Eigenschaft NOT FOR REPLICATION an.|  
|IDENTITY-Eigenschaft wird nicht an Abonnenten übertragen.|Diese Veröffentlichung lässt keine Updates von Abonnenten zu. Beim Übertragen der IDENTITY-Spalten an den Abonnenten wird die IDENTITY-Eigenschaft nicht mit übertragen. Beispielsweise wird eine Spalte, die auf dem Verleger als INT IDENTITY definiert wurde, auf dem Abonnenten als INT definiert.|Gilt für Veröffentlichungen, die auf Verlegern mit [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] oder einer früheren Version erstellt wurden. Es ist keine Aktion erforderlich.|  
|Tabellen, auf die von Sichten verwiesen wird, sind notwendig.|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erfordert, dass alle Tabellen, auf die von Sichten und indizierten Sichten verwiesen wird, auf dem Abonnenten verfügbar sind. Wenn die Tabellen, auf die verwiesen wird, nicht als Artikel in dieser Veröffentlichung veröffentlicht sind, müssen sie manuell auf dem Abonnenten erstellt werden.|Mithilfe der Schaltfläche **Zurück** können Sie zur Seite **Artikel** navigieren. Fügen Sie alle benötigten Objekte hinzu.|  
|Objekte, auf die von gespeicherten Prozeduren verwiesen wird, sind notwendig.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erfordert, dass alle Objekte, auf die von veröffentlichten gespeicherten Prozeduren, z. B. Tabellen und benutzerdefinierten Funktionen, verwiesen wird, auf dem Abonnenten verfügbar sind. Wenn die Objekte, auf die verwiesen wird, nicht als Artikel in dieser Veröffentlichung veröffentlicht sind, müssen sie manuell auf dem Abonnenten erstellt werden.|Mithilfe der Schaltfläche **Zurück** können Sie zur Seite **Artikel** navigieren. Fügen Sie alle benötigten Objekte hinzu.|  
  
## <a name="see-also"></a>Weitere Informationen  
 [Veröffentlichen von Daten und Datenbankobjekten](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Erstellen einer Veröffentlichung](../../relational-databases/replication/publish/create-a-publication.md)  
  
  
