---
title: SQL Server-Verschlüsselung | Microsoft-Dokumentation
description: Mithilfe dieser Informationen können Sie besser verstehen, wie SQL Server die Verschlüsselung verwendet, um die Sicherheit für Ihre Datenbank zu erhöhen.
ms.custom: ''
ms.date: 05/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], about encryption
- security [SQL Server], encryption
- cryptography [SQL Server], about cryptography
ms.assetid: ead0150e-4943-4ad5-84c8-36f85c7278f4
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fb11e565e4f1623ab0e25f47ee1b3c34bd38059a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475561"
---
# <a name="sql-server-encryption"></a>SQL Server-Verschlüsselung
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Als Verschlüsselung wird der Vorgang bezeichnet, Daten mithilfe eines Schlüssels oder eines Kennworts unlesbar zu machen. Die Daten werden dadurch ohne den entsprechenden Entschlüsselungsschlüssel oder das Kennwort unbrauchbar. Durch Verschlüsselung werden keine Probleme der Zugriffssteuerung gelöst. Sie erhöht jedoch die Sicherheit, indem sie den Datenverlust auch dann einschränkt, wenn die Zugriffssteuerung umgangen wird. Wenn der Hostcomputer einer Datenbank beispielsweise falsch konfiguriert wurde und ein Hacker Zugriff auf vertrauliche Daten erlangt, sind diese gestohlenen Daten sehr wahrscheinlich nutzlos, wenn sie verschlüsselt wurden.  
  

> [!IMPORTANT]  
>  Zwar ist die Verschlüsselung ein wertvolles Mittel, Sicherheit zu gewährleisten, sollte jedoch nicht für alle Daten oder Verbindungen verwendet werden. Bevor Sie sich dafür entscheiden, Verschlüsselung zu verwenden, prüfen Sie, wie Benutzer auf die Daten zugreifen. Wenn Benutzer über ein öffentliches Netzwerk auf Daten zugreifen, könnte die Datenverschlüsselung die Sicherheit erhöhen. Wenn sich aber der gesamte Zugriff innerhalb einer sicheren Intranetkonfiguration abspielt, ist eine Verschlüsselung möglicherweise nicht erforderlich. Jede Verwendung der Verschlüsselung sollte auch eine Wartungsstrategie für Kennwörter, Schlüssel und Zertifikate einschließen.  
  
> [!NOTE]  
>  Die neuesten Informationen zu Transport Layer Security (TLS 1.2) stehen unter [Unterstützung für Microsoft SQL Server TLS 1.2](https://support.microsoft.com/kb/3135244) zur Verfügung.  

Sie können in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Verbindungen, Daten und gespeicherte Prozeduren verschlüsseln. Die folgenden Themen enthalten weitere Informationen zur Verschlüsselung in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  

 [Verschlüsselungshierarchie](../../../relational-databases/security/encryption/encryption-hierarchy.md)  
 Informationen zur Verschlüsselungshierarchie in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 [Auswählen eines Verschlüsselungsalgorithmus](../../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)  
 Informationen zur Auswahl eines wirksamen Verschlüsselungsalgorithmus.  
  
 [Transparente Datenverschlüsselung &#40;TDE&#41;](../../../relational-databases/security/encryption/transparent-data-encryption.md)  
 Allgemeine Informationen über das transparente Verschlüsseln von Daten.  
  
 [Verschlüsselungsschlüssel für SQL Server und Datenbank &#40;Datenbank-Engine&#41;](../../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)  
 Die Verschlüsselungsschlüssel in [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]bestehen aus einer Kombination von öffentlichen, privaten und symmetrischen Schlüsseln, die zum Schutz sensibler Daten verwendet werden. Dieser Abschnitt erklärt, wie Verschlüsselungsschlüssel implementiert und verwaltet werden.  
  
 [Always Encrypted &#40;Datenbank-Engine&#41;](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)  
 Sicherstellen, dass lokale Datenbankadministratoren, Betreiber von Clouddatenbanken oder andere nicht autorisierte Benutzer mit hohen Berechtigungen nicht auf die verschlüsselten Daten zugreifen können.  
  
 [Dynamische Datenmaskierung](../../../relational-databases/security/dynamic-data-masking.md)  
 Einschränken der Offenlegung vertraulicher Daten, indem sie für nicht berechtigte Benutzer maskiert werden.  
  
 [SQL Server-Zertifikate und asymmetrische Schlüssel](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)  
 Informationen zur Verwendung von Kryptografie mit öffentlichem Schlüssel.  
  
## <a name="related-content"></a>Verwandte Inhalte  
 [Sichern von SQL Server](../../../relational-databases/security/securing-sql-server.md)  
 Überblick über die Absicherung der [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Plattform und die Arbeit mit Benutzern und sicherungsfähigen Objekten.  

[Überblick über die Sicherheitsfunktionen der Azure SQL-Datenbank](/azure/sql-database/sql-database-security-overview)
</br>Überblick über die Sicherheit der Azure SQL Datenbank zum Schutz von Daten, zur Steuerung des Zugriffs und zur proaktiven Überwachung.
  
 [Kryptografiefunktionen &#40;Transact-SQL&#41;](../../../t-sql/functions/cryptographic-functions-transact-sql.md)  
 Informationen dazu, wie Kryptografiefunktionen implementiert werden.  
  
 [ENCRYPTBYPASSPHRASE &#40;Transact-SQL&#41;](../../../t-sql/functions/encryptbypassphrase-transact-sql.md)  
 Informationen dazu, wie ein Kennwort zur Verschlüsselung von Daten verwendet wird.  
  
 [ENCRYPTBYKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/encryptbykey-transact-sql.md)  
 Informationen dazu, wie ein symmetrischer Schlüssel zur Verschlüsselung von Daten verwendet wird.  
  
 [ENCRYPTBYASYMKEY &#40;Transact-SQL&#41;](../../../t-sql/functions/encryptbyasymkey-transact-sql.md)  
 Informationen dazu, wie ein asymmetrischer Schlüssel zur Verschlüsselung von Daten verwendet wird.  
  
 [ENCRYPTBYCERT &#40;Transact-SQL&#41;](../../../t-sql/functions/encryptbycert-transact-sql.md)  
 Informationen dazu, wie ein Zertifikat zur Verschlüsselung von Daten verwendet wird.  
  
## <a name="external-resources"></a>Externe Ressourcen  
 [Microsoft TechNet: SQL Server TechCenter: SQL Server 2012, Sicherheit und Schutz](https://download.microsoft.com/download/8/F/A/8FABACD7-803E-40FC-ADF8-355E7D218F4C/SQL_Server_2012_Security_Best_Practice_Whitepaper_Apr2012.docx)  
 Aktuelle Informationen über die [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] -Sicherheit.  
  
## <a name="see-also"></a>Weitere Informationen  
 [sys.key_encryptions &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-key-encryptions-transact-sql.md)   
 [Verschlüsselungsschlüssel für SQL Server und Datenbank &#40;Datenbank-Engine&#41;](../../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
 [Sichern und Wiederherstellen von Reporting Services-Verschlüsselungsschlüsseln](../../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)     
 [Aktivieren von verschlüsselten Verbindungen mit der Datenbank-Engine](../../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)    
  
