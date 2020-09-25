---
description: Gewähren von Benutzerzugriff auf einen Berichtsserver
title: Gewähren von Benutzerzugriff auf einen Berichtsserver | Microsoft-Dokumentation
ms.date: 05/6/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- removing role assignments
- permissions [Reporting Services], granting report server access
- roles [Reporting Services], assignments
- modifying role assignments
- deleting role assignments
ms.assetid: 2144c020-3253-4b47-8cda-e14c928bb471
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0eb38550a0fc9e395b651783af4d449626de1d63
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88373886"
---
# <a name="grant-user-access-to-a-report-server"></a>Gewähren von Benutzerzugriff auf einen Berichtsserver

[!INCLUDE[ssrs-appliesto-sql2016-preview](../../includes/ssrs-appliesto-sql2016-preview.md)]

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] verwendet rollenbasierte Sicherheit, um Benutzerzugriff auf einen Berichtsserver zu gewähren. Bei der Installation eines neuen Berichtsservers haben nur Benutzer, die Mitglieder der lokalen Administratorengruppe sind, Zugriff auf Berichtsserverinhalte und -vorgänge. Damit der Berichtsserver auch von anderen Benutzern verwendet werden kann, müssen Sie Rollenzuweisungen erstellen, mit denen Benutzer- oder Gruppenkonten einer vordefinierten Rolle zugeordnet werden, die eine Auflistung von Tasks festlegt.

 **Berichtsserver im SharePoint-Modus:** Für Berichtsserver, die für den integrierten SharePoint-Modus konfiguriert sind, muss der Zugriff von einer SharePoint-Website mit SharePoint-Berechtigungen konfiguriert werden. Die Berechtigungsebenen der SharePoint-Website legen den Zugriff auf Berichtsserverinhalt und -vorgänge fest. Sie müssen Websiteadministrator sein, um Berechtigungen auf einer SharePoint-Website gewähren zu können. Weitere Informationen finden Sie unter [Erteilen von Berechtigungen für Berichtsserverelemente auf einer SharePoint-Website](../../reporting-services/security/granting-permissions-on-report-server-items-on-a-sharepoint-site.md).

 **Berichtsserver im einheitlichen Modus:** In diesem Artikel liegt der Schwerpunkt auf einem für den einheitlichen Modus konfigurierten Berichtsserver und der Verwendung des Webportals zum Zuweisen von Benutzern zu Rollen. Die folgenden beiden Rollen stehen zur Verfügung:

- Rollen auf Elementebene werden verwendet, um Berichtsserverinhalte, Abonnements, Berichtsverarbeitung und Berichtsverlauf anzuzeigen, hinzuzufügen und zu verwalten. Rollenzuweisungen auf Elementebene werden im Stammknoten (dem Stammordner) oder in bestimmten Ordnern oder Elementen weiter unten in der Hierarchie definiert.

- Rollen auf Systemebene gewähren Zugriff auf siteweite Vorgänge, die an kein bestimmtes Element gebunden sind. Beispiele sind die Verwendung des Berichts-Generators und die Verwendung von freigegebenen Zeitplänen.

    Diese beiden Rollentypen ergänzen sich gegenseitig und sollten zusammen verwendet werden. Das Hinzufügen eines Benutzers zu einem Berichtsserver ist daher ein Vorgang mit zwei Teilvorgängen. Wenn Sie einen Benutzer einer Rolle auf Elementebene zuweisen, sollten Sie ihn ebenfalls einer Rolle auf Systemebene zuweisen. Beim Zuweisen eines Benutzers zu einer Rolle müssen Sie eine bereits definierte Rolle wählen. Verwenden Sie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], um Rollen zu erstellen, zu ändern oder zu löschen. Weitere Informationen finden Sie unter [Erstellen, Löschen oder Ändern einer Rolle &#40;Management Studio&#41;](../../reporting-services/security/role-definitions-create-delete-or-modify.md).

## <a name="before-you-start"></a>Vorbereitung

Überprüfen Sie die folgende Liste vor dem Hinzufügen von Benutzern zu einem Berichtsserver im einheitlichen Modus.

- Sie müssen ein Mitglied der lokalen Administratorengruppe auf dem Berichtsservercomputer sein. Wenn Sie [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] auf [!INCLUDE[wiprlhlong](../../includes/wiprlhlong-md.md)] oder Windows Server 2008 bereitstellen, sind zusätzliche Konfigurationsschritte erforderlich, bevor Sie einen Berichtsserver lokal verwalten können. Weitere Informationen finden Sie unter [Konfigurieren eines Berichtsservers im einheitlichen Modus für die lokale Verwaltung](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).

- Um diese Aufgabe an andere Benutzer zu delegieren, erstellen Sie Rollenzuweisungen, die Benutzerkonten den Rollen Inhalts-Manager und Systemadministrator zuordnen. Benutzer, die über Inhalts-Manager- und Systemadministrator-Berechtigungen verfügen, können einem Berichtsserver Benutzer hinzufügen.

- Sehen Sie sich in [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] die vordefinierten Systemrollen und Benutzerrollen an, damit Ihnen die mit den einzelnen Rollen verbundenen Aufgaben vertraut sind. Im Webportal werden keine Aufgabenbeschreibungen angezeigt, sodass Sie die Rollen kennen sollten, bevor Sie Benutzer hinzufügen.

- Passen Sie die Rollen optional an, oder definieren Sie zusätzliche Rollen, um die Auflistung von benötigten Tasks einzuschließen. Wenn Sie zum Beispiel benutzerdefinierte Sicherheitseinstellungen für einzelne Elemente verwenden möchten, können Sie eine neue Rollendefinition erstellen, die Lesezugriff für Ordner gewährt.

### <a name="to-add-a-user-or-group-to-a-system-role"></a>So fügen Sie einer Systemrolle einen Benutzer oder eine Gruppe hinzu

1. Starten Sie das [Webportal](../web-portal-ssrs-native-mode.md).

2. Wählen Sie in der oberen rechten Ecke das **Zahnradsymbol** aus, und wählen Sie dann im Dropdownmenü **Siteeinstellungen** aus.

    ![Zahnradsymbol und Dropdownmenü im Webportal des Berichtsservers](../../reporting-services/security/media/settings-icon-and-menu.png)

3. Wählen Sie **Sicherheit** aus.

4. Klicken Sie auf **Gruppe oder Benutzer hinzufügen**.

5. Geben Sie unter **Gruppe oder Benutzer** ein Windows-Domänenbenutzerkonto oder -Domänengruppenkonto im folgenden Format an: \<domain>\\<Konto\>.

    > [!NOTE]
    > Wenn Sie die Formularauthentifizierung oder die benutzerdefinierte Sicherheit verwenden, geben Sie das Benutzer- oder Gruppenkonto in dem für Ihre Bereitstellung richtigen Format an.

6. Wählen Sie eine Systemrolle aus, und klicken Sie anschließend auf **OK**.

    Rollen sind kumulativ, das heißt, wenn Sie sowohl die Systemadministrator- als auch die Systembenutzerrolle wählen, kann ein Benutzer oder eine Gruppe die Tasks beider Rollen ausführen.

7. Wiederholen Sie den Vorgang, um weiteren Benutzern oder Gruppen Rollen zuzuweisen.

### <a name="to-add-a-user-or-group-to-an-item-role"></a>So fügen Sie einem Benutzer oder einer Gruppe eine Elementrolle hinzu

1. Starten Sie das **Webportal**, und suchen Sie das Berichtselement, für das Sie einen Benutzer oder eine Gruppe hinzufügen möchten.

2. Klicken Sie auf die Auslassungspunkte (**...**) eines Elements.

3. Klicken Sie im Dropdownmenü auf **Verwalten**.

4. Wählen Sie **Sicherheit** aus.

5. Klicken Sie auf **Gruppe oder Benutzer hinzufügen**.

    > [!NOTE]
    > Falls ein Element aktuell die Sicherheitseinstellungen eines übergeordneten Elements erbt, klicken Sie auf der Symbolleiste auf **Sicherheit anpassen**, um die Sicherheitseinstellungen zu ändern. Klicken Sie dann auf **Gruppe oder Benutzer hinzufügen**.

6. Geben Sie unter **Gruppe oder Benutzer** ein Windows-Domänenbenutzerkonto oder -Domänengruppenkonto im folgenden Format an: \<domain>\\<Konto\>. Wenn Sie die Formularauthentifizierung oder die benutzerdefinierte Sicherheit verwenden, geben Sie das Benutzer- oder Gruppenkonto in dem für Ihre Bereitstellung richtigen Format an.

7. Wählen Sie mindestens eine Rollendefinition aus, die beschreibt, wie der Benutzer oder die Gruppe auf das Element zugreifen soll, und klicken Sie dann auf **OK**.

8. Wiederholen Sie den Vorgang, um weiteren Benutzern oder Gruppen Rollen zuzuweisen.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen und Verwalten von Rollenzuweisungen](../../reporting-services/security/create-and-manage-role-assignments.md)  
[Role Assignments (Rollenzuweisungen)](../../reporting-services/security/role-assignments.md)  
[Rollendefinitionen](../../reporting-services/security/role-definitions.md)  

Haben Sie dazu Fragen? [Stellen Sie eine Frage im Reporting Services-Forum](https://go.microsoft.com/fwlink/?LinkId=620231)
