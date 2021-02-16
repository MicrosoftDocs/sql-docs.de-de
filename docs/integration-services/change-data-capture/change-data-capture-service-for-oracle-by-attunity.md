---
description: Change Data Capture Service für Oracle von Attunity
title: Change Data Capture Service für Oracle von Attunity | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 22ec8a5c-9550-4d38-8a4a-485ec3e53ea8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6b6fafc30e688be35593140cda0c5a59e01ca028
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339239"
---
# <a name="change-data-capture-service-for-oracle-by-attunity"></a>Change Data Capture Service für Oracle von Attunity

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Der CDC Service for Oracle ist ein Windows-Dienst, der Oracle-Transaktionsprotokolle scannt und Änderungen an relevanten Oracle-Tabellen in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Änderungstabellen aufzeichnet. Die SQL-Änderungstabellen, in denen die aus Oracle aufgezeichneten Änderungen gespeichert werden, entsprechen vom Typ her den Änderungstabellen, die in der systemeigenen Change Data Capture-Funktion von [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] verwendet werden. Dies macht das Verarbeiten dieser Änderungen so einfach wie das Verarbeiten von an [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbanken vorgenommenen Änderungen.  
  
## <a name="installation"></a>Installation  

Laden Sie Microsoft Change Data Capture Designer und Service für Oracle von Attunity für die entsprechende SQL Server-Version anhand der folgenden Links herunter:

- [Microsoft SQL Server 2012 Integration Services Attunity Oracle CDC Designer/Service Feature Pack](https://www.microsoft.com/download/details.aspx?id=51606)
- [Microsoft SQL Server 2016 Integration Services Attunity Oracle CDC Designer/Service Feature Pack](https://www.microsoft.com/download/details.aspx?id=55802)
- [Microsoft SQL Server 2017 Integration Services Attunity Oracle CDC Designer/Service Feature Pack](https://www.microsoft.com/download/details.aspx?id=56610)
- [Microsoft SQL Server 2019 Integration Services Feature Pack](https://www.microsoft.com/download/details.aspx?id=100303)
  
 Der CDC Service for Oracle kann auf jedem unterstützten Windows-Computer mit Zugriff auf die aufgezeichneten Oracle-Quelldatenbanken und die [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Zielinstanz installiert werden, auf der sich die CDC-Zieldatenbank befindet. Der CDC Service benötigt keine lokale Installation der Oracle-Datenbank oder der [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbank, nur die unterstützten Clients. Informationen dazu, wo die erforderlichen Datenbankkomponenten installiert werden müssen, finden Sie in diesem Thema unter **Erforderliche Komponenten für die Datenbank** .  
  
 Bei der Installation des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CDC Service for Oracle wird die Benutzeroberfläche der Dienstkonfiguration und das Dienstprogramm am ausgewählten Speicherort abgelegt. Der CDC Service for Oracle wird mithilfe der Oracle CDC Service Configuration Console separat konfiguriert. Weitere Informationen zur Konfiguration des Oracle CDC Service finden Sie unter [Change Data Capture Service für Oracle von Attunity F1 Hilfe](../../integration-services/change-data-capture/change-data-capture-service-for-oracle-by-attunity-f1-help.md).  
  
 Der CDC Service for Oracle kann auf jedem unterstützten Windows-Computer installiert werden, auf dem der [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Native Client installiert ist. Er muss nicht auf dem gleichen Computer installiert sein, auf dem das [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Ziel installiert ist.  
  
## <a name="supported-windows-environments"></a>Unterstützte Windows-Umgebungen  
 Der Change Data Capture Service für Oracle von Attunity kann in den folgenden Windows-Umgebungen ausgeführt werden:  
  
-   Windows 8 und 8.1  
-   Windows 10  
-   Windows Server 2012 und 2012 R2
-   Windows Server 2016
  
## <a name="database-prerequisites"></a>Erforderliche Komponenten für die Datenbank  
 Sie müssen den mit der Oracle-Datenbankversion kompatiblen Oracle-Client installieren, um mit dem CDC-Dienst für Oracle arbeiten zu können. Dies ist eine erforderliche Komponente von Oracle, die vor dem Installieren des Oracle CDC Service vorhanden sein muss. Darüber hinaus müssen Sie den SQL Server-ODBC-Client über das SQL Server-Setup installieren.  
  
 Der CDC Service for Oracle unterstützt die folgenden Versionen:  
  
### <a name="source-oracle-database"></a>Oracle-Quelldatenbank  
  
-   Oracle-Datenbank 10g Release 2
-   Oracle-Datenbank 11g Release 1 und Release 2
-   Oracle Database 12c klassische Installation (Die mehrinstanzenfähige Installation wird nicht unterstützt).  
-   Oracle Database 18c klassische Installation (Die mehrinstanzenfähige Installation wird nicht unterstützt). 
-   Oracle Database 19c klassische Installation (Die mehrinstanzenfähige Installation wird nicht unterstützt). 
  
### <a name="target-sql-server-database"></a>SQL Server-Zieldatenbank  
 Eine Liste der Funktionen, die von den [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]-Editionen unterstützt werden, finden Sie unter [Von den SQL Server-Editionen unterstützte Funktionen](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
## <a name="running-the-installation-program"></a>Ausführen des Installationsprogramms  
 Öffnen Sie zum Installieren des CDC Service for Oracle den Installations-Assistenten für die von Ihnen verwendete Windows-Plattform (32/64 Bit), und befolgen Sie die Anleitung auf dem Bildschirm.  
  
## <a name="uninstalling-change-data-capture-service-for-oracle-by-attunity"></a>Deinstallieren des Change Data Capture Service für Oracle von Attunity  
 Sie deinstallieren den CDC Service for Oracle über die Option Programme und Funktionen in der Systemsteuerung.  
  
 Beim Deinstallieren des CDC Service werden die erstellten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbanken nicht gelöscht. Zum vollständigen Entfernen des Tools müssen Sie die MSXDBCDC-Datenbank und die jeweiligen CDC-Datenbanken entfernen, die auf der verwendeten [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Zielinstanz erstellt wurden.  
  
 Wenn Sie die CDC Service-Software auf einem Computer deinstallieren und auf einem anderen Computer installieren, müssen Sie nur die folgenden Definitionen angeben:  
  
-   Dienstkonto  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Verbindungszeichenfolge und -Anmeldeinformationen  
  
-   Masterkennwort  
  
 Alle anderen Definitionen werden in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] gespeichert und sind über die vorherige Installation auf einem anderen Computer verfügbar.  
  
## <a name="in-this-documentation"></a>In dieser Dokumentation  
  
-   [Change Data Capture Service für Oracle von Attunity System Architecture](../../integration-services/change-data-capture/change-data-capture-service-for-oracle-by-attunity-system-architecture.md)  
  
-   [Oracle CDC Service](../../integration-services/change-data-capture/the-oracle-cdc-service.md)  
  
-   [Change Data Capture Service für Oracle von Attunity – F1-Hilfe](../../integration-services/change-data-capture/change-data-capture-service-for-oracle-by-attunity-f1-help.md)  
  
-   [Change Data Capture Service für Oracle von Attunity – Anleitung](../../integration-services/change-data-capture/change-data-capture-service-for-oracle-by-attunity-how-to-guide.md)  
  
## <a name="see-also"></a>Weitere Informationen  
 [Arbeiten mit dem Oracle CDC Service](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md)  
  
  
