---
description: Sichern von freigegebenen Datasetelementen
title: Sichern von freigegebenen Datasetelementen | Microsoft-Dokumentation
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
ms.assetid: 08e6d8b5-d88c-4ed2-9c05-55c757e00014
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 80cfeba2128e82c6254ba962e91c6c51461ff955
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2020
ms.locfileid: "88373216"
---
# <a name="secure-shared-dataset-items"></a>Sichern von freigegebenen Datasetelementen
  Auf einem Berichtsserver können freigegebene Datasetelemente von mehreren Berichten verwendet werden. Sie können freigegebene Datasets sichern, um die Zugriffsebene der Benutzer zu steuern. Standardmäßig können nur Benutzer, die Mitglieder der integrierten Gruppe **Administratoren** sind, freigegebene Datasets anzeigen, Eigenschaften ändern, das Zwischenspeichern aktivieren, Cacheaktualisierungspläne erstellen und die Elemente löschen. Für alle anderen Benutzer müssen Rollenzuweisungen erstellt werden, die den Zugriff auf ein freigegebenes Dataset zulassen.  
  
 Zum Festlegen der Sicherheit erstellen Sie eine Rollenzuweisung, die angibt, welches Benutzer- oder Gruppenkonto Zugriff auf das freigegebene Dataset hat.  
  
## <a name="role-based-access-to-shared-datasets"></a>Rollenbasierter Zugriff auf freigegebene Datasets  
 Wenn Sie den Zugriff auf freigegebene Datasets gewähren möchten, können Sie es den Benutzern ermöglichen, vorhandene Rollenzuweisungen von einem übergeordneten Ordner zu erben oder eine neue Rollenzuweisung für das Element selbst zu erstellen.  
  
 Die Standardrollenzuweisungen, mit denen Sie Elementeigenschaften hinzufügen, löschen und bearbeiten sowie verknüpfte Berichte und freigegebene Datenquellen für freigegebene Datasets anzeigen können, lauten Inhalts-Manager, Meine Berichte und Verleger. Um die Definition eines freigegebenen Datasets zu bearbeiten, verwenden Sie die Standardrollenzuweisungen Berichts-Generator oder Inhalts-Manager.  
  
 Da Rollenzuweisungen normalerweise von einem übergeordneten Knoten geerbt werden, übergibt ein Ordner, für den der Task **Berichte anzeigen** aktiviert ist, diese Berechtigung an freigegebene Datasets und Berichte im Ordner.  
  
 Um mehr Kontrolle über freigegebene Datasets und deren Abfrageergebnisse zu erhalten, konfigurieren Sie die Sicherheit für das freigegebene Datasetelement auf Elementebene, oder speichern Sie freigegebene Datasets in einem Ordner, und konfigurieren Sie die Sicherheit für den Ordner auf Elementebene.  
  
## <a name="shared-dataset-parameters"></a>Parameter für freigegebene Datasets  
 Parameter für freigegebene Datasets können nicht verwendet werden, um Daten für bestimmte Benutzer einzuschränken. Sie bieten vielmehr eine Möglichkeit, diejenigen Daten anzugeben, die in die Verarbeitung des freigegebenen Datasets eingeschlossen werden sollen. Die Werte von Parametern für freigegebene Datasets können auf dem Berichtsserver nicht gesichert werden.  
  
 In der Definition eines freigegebenen Datasets können Sie Parameter als schreibgeschützt kennzeichnen und Standardwerte angeben. Als schreibgeschützt gekennzeichnete Parameter können auf dem Server nicht überschrieben werden. In einem Cacheaktualisierungsplan für ein freigegebenes Dataset können Sie z. B. keine Werte für schreibgeschützte Parameter angeben. Wenn die Parameter für freigegebene Datasets Ausdrücke enthalten, die sich auf die globale User-Auflistung beziehen, oder andere Benutzerabhängigkeiten aufweisen, können Sie keinen Cacheaktualisierungsplan für das freigegebene Dataset erstellen.  
  
## <a name="tasks-access-to-items-and-default-roles"></a>Tasks, Zugriff auf Elemente und Standardrollen  
 Für freigegebene Datasets gilt das gleiche Sicherheitsmodell wie für Berichte. Um einem Benutzer Berechtigungen für bestimmte Aktionen zu gewähren, wählen Sie eine Rolle mit einem Task aus, der die entsprechenden Berechtigungen enthält. In der folgenden Tabelle sind Tasks und die darin enthaltenen Aktionen aufgeführt.  
  
|Verwendete Aufgabe|Berechtigt zu folgender Aktion|Standardrollen, in denen der Task enthalten ist|  
|----------------------|---------------------------------|-----------------------------------------|  
|Berichte anzeigen|Anzeigen des freigegebenen Datasetelements in der Ordnerhierarchie. Ohne diesen Task ist das Element für Benutzer nicht sichtbar, und sie bemerken möglicherweise nicht, dass das Dataset verfügbar ist.|Browser<br /><br /> Inhalts-Manager<br /><br /> Report Builder<br /><br /> Meine Berichte|  
|Berichte verwalten|Anzeigen von Eigenschaften, die den Namen, eine Beschreibung und Verbindungsinformationen angeben. Dieser Task wird auch verwendet, um ein freigegebenes Datasetelement in der Ordnerhierarchie anzuzeigen. Wenn Sie diesen Task auswählen, können Sie den Task Berichte anzeigen auslassen.|Inhalts-Manager<br /><br /> Herausgeber<br /><br /> Meine Berichte|  
|Berichte lesen|Anzeigen der Definition des freigegebenen Datasets.|Inhalts-Manager<br /><br /> Report Builder|  
|Die Sicherheit für einzelne Elemente festlegen|Erstellen und Ändern von Rollenzuweisungen, die den Zugriff auf das freigegebene Dataset steuern. Dieser Task muss zusammen mit dem Task Berichte anzeigen oder Berichte verwalten verwendet werden. Andernfalls zeigt sie keine Wirkung, weil der Benutzer das Element nicht auswählen kann.|Inhalts-Manager|  
  
 Weitere Informationen finden Sie unter [Aufgaben auf Elementebene](../../reporting-services/security/tasks-and-permissions-item-level-tasks.md) und [Vordefinierte Rollen](../../reporting-services/security/role-definitions-predefined-roles.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Verwalten von freigegebenen Datasets](../../reporting-services/report-data/manage-shared-datasets.md)   
 [Sichere Ordner](../../reporting-services/security/secure-folders.md)   
 [Sichere Berichte und Ressourcen](../../reporting-services/security/secure-reports-and-resources.md)   
 [Erteilen von Berechtigungen für einen Berichtsserver im einheitlichen Modus](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)   
 [Granting Permissions on a Native Mode Report Server (Erteilen von Berechtigungen für einen Berichtsserver im einheitlichen Modus)](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)  
  
  
