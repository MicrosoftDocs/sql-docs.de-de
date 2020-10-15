---
description: Rollendefinitionen
title: Rollendefinitionen | Microsoft-Dokumentation
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- roles [Reporting Services], creating
- roles [Reporting Services], security
- security [Reporting Services], role definitions
- role-based security [Reporting Services], role definitions
ms.assetid: d1b8dbf0-4462-402e-92dd-0e4835002b6e
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d8dbfe6d890c84bee9a66141971a554c410d47dd
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "91987382"
---
# <a name="role-definitions"></a>Rollendefinitionen
  In [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ist eine *Rollendefinition* eine benannte Auflistung von Aufgaben, die die auf einem Berichtsserver verfügbaren Vorgänge definieren. Rollendefinitionen stellen die Regeln bereit, mit denen der Berichtsserver die Sicherheit erzwingt. Wenn ein Benutzer versucht, eine Aufgabe auszuführen, z. B. das Veröffentlichen eines Berichts in einem Ordner, überprüft der Berichtsserver die Rollenzuweisung des Benutzers, um zu bestimmen, ob die Aufgabe in der Rollendefinition enthalten ist. Falls die Aufgabe in der Rollendefinition enthalten ist, wird die Anforderung gesendet.  
  
## <a name="using-roles-to-authorize-access-to-a-report-server"></a>Autorisieren des Zugriffs auf einen Berichtsserver mithilfe von Rollen  
 Eine Rolle wird erst wirksam, wenn sie in einer Rollenzuweisung verwendet wird. Weitere Informationen dazu, wie Rollen die Sicherheit erhöhen, finden Sie unter [Rollenzuweisungen](../../reporting-services/security/role-assignments.md).  
  
## <a name="types-of-role-definitions"></a>Typen von Rollendefinitionen  
 Rollendefinitionen sind entweder Definitionen auf Element- oder auf Systemebene. Eine *Rollendefinition auf Elementebene* beschreibt die Aufgaben, die sich auf die auf einem Berichtsserver gespeicherten und verwalteten Elemente beziehen, z.B. Berichte, Ordner und Modelle. Berichte verwalten, Ordner anzeigen und Einzelne Abonnements verwalten sind nur einige der Aufgaben, die Sie in eine Rollendefinition auf Elementebene integrieren können. Eine *Systemrollendefinition* enthält Aufgaben, die für die ganze Site gelten. Berichtsservereigenschaften anzeigen ist ein Beispiel für eine Aufgabe, die Sie in eine Systemrolle integrieren können.  
  
## <a name="predefined-roles"></a>Vordefinierte Rollen  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] enthält vordefinierte Rollen, die den verschiedenen Ebenen der Benutzerinteraktion entsprechen. Die folgende Liste enthält die vordefinierten Rollen, die Sie verwenden können:  
  
-   Inhalts-Manager, Verleger, Browser, Berichts-Generator und Meine Berichte sind Rollendefinitionen auf Elementebene, die Sie beim Erstellen von Rollenzuweisungen für den Zugriff auf Berichtsserverinhalt verwenden können.  
  
-   Systemadministrator und Systembenutzer sind Rollendefinitionen auf Systemebene, die Sie zum Autorisieren des Zugriffs auf Sitevorgänge verwenden können.  
  
 Weitere Informationen finden Sie unter [Vordefinierte Rollen](../../reporting-services/security/role-definitions-predefined-roles.md).  
  
## <a name="creating-a-role-definition"></a>Erstellen einer Rollendefinition  
 Um eine Rolle zu erstellen, geben Sie in Management Studio einen Namen und in ihr integrierte Aufgaben an. Sie müssen separate Rollendefinition für Element- und Systemaufgaben erstellen. Rollen können Aufgaben auf Element- oder auf Systemebene enthalten, aber keine Kombination aus beidem. Beim Erstellen einer Rolledefinition muss ein Name angegeben und eine Gruppe von Aufgaben für die Definition ausgewählt werden. Zum Erstellen einer Rollendefinition benötigen Sie die entsprechende Berechtigung. Diese Berechtigungen werden mit der Aufgabe "Die Sicherheit für einzelne Elemente festlegen" gewährt. Standardmäßig können Administratoren und Benutzer, die der vordefinierten **Inhalts-Manager** -Rolle zugewiesen sind, diese Aufgabe ausführen.  
  
 Für eine Rolle ist ein eindeutiger Name erforderlich. Eine gültige Rollendefinition muss mindestens eine Aufgabe enthalten. Weitere Informationen finden Sie unter [Tasks and Permissions](../../reporting-services/security/tasks-and-permissions.md).  
  
 Verwenden Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] zum Erstellen einer Rollendefinition. Weitere Informationen finden Sie unter [Erstellen, Löschen oder Ändern einer Rolle &#40;Management Studio&#41;](../../reporting-services/security/role-definitions-create-delete-or-modify.md).  
  
 Nachdem Sie eine Rollendefinition erstellt haben, können Sie diese verwenden, indem Sie sie in einer Rollenzuweisung auswählen. Weitere Informationen finden Sie unter [Gewähren von Benutzerzugriff auf einen Berichtsserver &#40;Berichts-Manager&#41;](./grant-user-access-to-a-report-server.md).  
  
## <a name="customize-or-delete-a-role-definition"></a>Anpassen oder Löschen einer Rollendefinition  
 Vordefinierte Rollen können geändert oder durch benutzerdefinierte Rollen ersetzt werden. Um eine Rolle zu ändern, fügen Sie der Rollendefinition Aufgaben hinzu bzw. entfernen Aufgaben daraus. Sie können keine Rolle umbenennen. Vorgenommene Änderungen werden sofort auf alle Rollenzuweisungen angewendet, die diese Rollendefinition enthalten.  
  
 Sie können eine nicht mehr verwendete Rollendefinition löschen. Es ist nicht möglich, die Rollendefinition, die für die Funktion Meine Berichte ausgewählt ist, zu löschen, solange diese Funktion aktiviert ist. Zum Löschen der für Meine Berichte verwendeten Rollendefinition müssen Sie diese Funktion zunächst deaktivieren oder eine andere Rollendefinition dafür auswählen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Aufgaben und Berechtigungen](../../reporting-services/security/tasks-and-permissions.md)   
 [Erteilen von Berechtigungen für einen Berichtsserver im einheitlichen Modus](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)   
 [Erstellen, Löschen oder Ändern einer Rolle &#40;Management Studio&#41;](../../reporting-services/security/role-definitions-create-delete-or-modify.md)   
 [Gewähren von Benutzerzugriff auf einen Berichtsserver &#40;Berichts-Manager&#41;](./grant-user-access-to-a-report-server.md)   
 [Ändern oder Löschen einer Rollenzuweisung (Berichts-Manager)](../../reporting-services/security/role-assignments-modify-or-delete.md)   
 [Festlegen von Berechtigungen für Berichtsserverelemente auf einer SharePoint-Website &#40;Reporting Services im integrierten SharePoint-Modus&#41;](../../reporting-services/security/set-permissions-for-report-server-items-on-a-sharepoint-site.md)  
  
