---
title: 'Bereitstellen im Active Directory-Modus: Voraussetzungen'
titleSuffix: SQL Server Big Data Cluster
description: Konfigurieren von Active Directory für SQL Server Big Data-Cluster
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3424863924338a0d3f02102609052d6c159e1043
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048010"
---
# <a name="deploy-big-data-clusters-2019-in-active-directory-mode-prerequisites"></a>Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] im Active Directory-Modus: Voraussetzungen

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

In diesem Artikel wird erläutert, wie Sie die Bereitstellung eines Big Data-Clusters für SQL Server im Active Directory-Authentifizierungsmodus vorbereiten. Der Cluster verwendet eine vorhandene AD-Domäne für die Authentifizierung.

>[!Note]
>Vor dem Release von SQL Server 2019 CU5 gab es eine Einschränkung für Big Data-Cluster, durch die nur ein Cluster für eine Active Directory-Domäne bereitgestellt werden konnte. Diese Einschränkung wurde mit dem CU5-Release entfernt. Weitere Informationen zu den neuen Funktionen finden Sie unter [Konzept: Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] im Active Directory-Modus](active-directory-deployment-background.md). Die in diesem Artikel gezeigten Beispiele sind an beide Bereitstellungsszenarios angepasst.

## <a name="background"></a>Hintergrund

Um die Active Directory-Authentifizierung (AD) zu aktivieren, erstellt der BDC automatisch alle Benutzer-, Gruppen- und Computerkonten sowie Dienstprinzipalnamen, die von den verschiedenen Diensten im Cluster benötigt werden. Es wird empfohlen, dass Sie eine Organisationseinheit (OE) vor der Clusterbereitstellung erstellen, um eine gewisse Eigenständigkeit dieser Konten sowie bereichsbezogene Berechtigungen zu ermöglichen. Alle BDC-bezogenen AD-Objekte werden während der Bereitstellung erstellt. 

## <a name="pre-requisites"></a>Voraussetzungen

### <a name="organizational-unit-ou"></a>Organisationseinheit (OE)
Eine Organisationseinheit (OE) ist eine Unterteilung innerhalb einer Active Directory-Instanz, in der Benutzer, Gruppen und sogar andere Organisationseinheiten platziert werden. Große Organisationseinheiten können verwendet werden, um die funktionale Struktur oder Geschäftsstruktur einer Organisation widerzuspiegeln. In diesem Artikel wird eine Organisationseinheit namens `bdc` als Beispiel erstellt. 

>[!NOTE]
>Die Organisationseinheit (OE) stellt Verwaltungsgrenzen dar und ermöglicht es Kunden, den Gültigkeitsbereich von Datenadministratoren zu steuern. 

Sie können [OE-Entwurfsprinzipien](/windows-server/identity/ad-ds/plan/reviewing-ou-design-concepts) befolgen, um die für Sie am besten geeignete Struktur bei der Arbeit mit Organisationseinheiten in Ihrer Organisation zu ermitteln.

### <a name="ad-account-for-bdc-domain-service-account"></a>AD-Konto für BDC-Domänendienstkonto

Damit alle erforderlichen Objekte automatisch in Active Directory erstellt werden können, benötigt der BDC ein AD-Konto, das über bestimmte Berechtigungen zum Erstellen von Benutzern, Gruppen und Computerkonten in der angegebenen Organisationseinheit verfügt. In diesem Artikel wird erläutert, wie die Berechtigung dieses AD-Kontos konfiguriert wird. Wir verwenden einen AD-Konto namens `bdcDSA` als Beispiel in diesem Artikel.

### <a name="auto-generated-active-directory-objects"></a>Automatisch generierte Active Directory-Objekte
Der BDC generiert automatisch Konten- und Gruppennamen. Jedes der Konten stellt einen Dienst im BDC dar und wird während der gesamten Lebensdauer, in der ein BDC verwendet wird, vom BDC verwaltet. Diese Konten besitzen die Dienstprinzipalnamen (SPNs), die für jeden Dienst erforderlich sind.  Eine vollständige Liste der automatisch generierten AD-Konten, Gruppen und Dienste, die von ihnen verwaltet werden, finden Sie unter [Automatisch generierte Active Directory-Objekte](active-directory-objects.md).

>[!IMPORTANT]
>Abhängig von der im Domänencontroller festgelegten Richtlinie zum Kennwortablauf, können die Kennwörter für diese Konten ablaufen. Die Standardablaufrichtlinie gibt 42 Tage vor. Es gibt keinen Mechanismus zum Rotieren von Anmeldeinformationen für alle Konten im BDC, sodass der Cluster nicht mehr funktionsfähig ist, sobald das Ablaufdatum erreicht ist. Um dieses Problem zu umgehen, aktualisieren Sie die Ablaufrichtlinie für die BDC-Dienstkonten im Domänencontroller in „Kennwort läuft nie ab“. Diese Aktion kann vor oder nach der Ablaufzeit durchgeführt werden. Im letzteren Fall aktiviert Active Directory die abgelaufenen Kennwörter erneut.
>
>In der folgenden Abbildung wird gezeigt, wo diese Eigenschaft unter „Active Directory-Benutzer und -Computer“ festgelegt wird.
>
>:::image type="content" source="media/deploy-active-directory/image25.png" alt-text="Festlegen einer Kennwortablaufrichtlinie":::

In den folgenden Schritten wird vorausgesetzt, dass Sie bereits über einen Active Directory-Domänencontroller verfügen. Wenn Sie noch keinen Domänencontroller eingerichtet haben, finden Sie in dieser [Anleitung](https://social.technet.microsoft.com/wiki/contents/articles/37528.create-and-configure-active-directory-domain-controller-in-azure-windows-server.aspx) nützliche Schritte.

## <a name="create-ad-objects"></a>Erstellen von AD-Objekten

Führen Sie die folgenden Schritte aus, bevor Sie einen BDC mit AD-Integration bereitstellen:

1. Erstellen Sie eine Organisationseinheit (OE), in der alle AD-Objekte für den BDC gespeichert werden. Alternativ können Sie bei der Bereitstellung eine vorhandene Organisationseinheit auswählen.
1. Erstellen Sie ein AD-Konto für den BDC, oder verwenden Sie ein vorhandenes Konto, und weisen Sie diesem BDC-AD-Konto die geeigneten Berechtigungen innerhalb der bereitgestellten Organisationseinheit (OE) zu.

### <a name="create-a-user-in-ad-for-bdc-domain-service-account"></a>Erstellen eines Benutzers in AD für das BDC-Domänendienstkonto

Der Big Data-Cluster benötigt ein Konto mit bestimmten Berechtigungen. Bevor Sie fortfahren, müssen Sie entweder über ein vorhandenes AD-Konto verfügen oder ein neues AD-Konto erstellen, das der Big Data-Cluster zum Einrichten der erforderlichen Objekte verwenden kann.

Um in AD einen neuen Benutzer zu erstellen, können Sie mit der rechten Maustaste auf die Domäne oder die OE klicken und **Neu** > **Benutzer** auswählen:

![Active Directory-Benutzerdialogfeld](./media/deploy-active-directory/image12.png)

Dieses Benutzerkonto wird im vorliegenden Artikel als *BDC-Domänendienstkonto* bezeichnet.

### <a name="create-an-ou"></a>Erstellen einer Organisationseinheit

Öffnen Sie auf dem Domänencontroller **Active Directory-Benutzer und -Computer**. Klicken Sie im linken Bereich mit der rechten Maustaste auf das Verzeichnis, in dem Sie Ihre OE erstellen möchten, klicken Sie dann auf **Neu** \> **Organisationseinheit**, und folgen Sie den Anweisungen des Assistenten, um die OE zu erstellen. Alternativ können Sie eine OE mithilfe von PowerShell erstellen:

```powershell
New-ADOrganizationalUnit -Name "<name>" -Path "<Distinguished name of the directory you wish to create the OU in>"
```

In den Beispielen in diesem Artikel wird `bdc` als Name der OE verwendet.

![Active Directory-Organisationseinheit](./media/deploy-active-directory/image13.png)

![Neues Objekt – Organisationseinheit](./media/deploy-active-directory/image14.png)

### <a name="set-permissions-for-an-ad-account"></a>Festlegen von Berechtigungen für ein AD-Konto

Unabhängig davon, ob Sie einen neuen AD-Benutzer erstellt haben oder einen vorhandenen AD-Benutzer verwenden, benötigt der Benutzer bestimmte Berechtigungen. Dieses Konto ist das Benutzerkonto, das der BDC-Controller verwendet, um den Cluster zu AD hinzuzufügen.

Das BDC-Domänendienstkonto muss in der Lage sein, Benutzer-, Gruppen- und Computerkonten in der OE zu erstellen. In den folgenden Schritten wurde das BDC-Domänendienstkonto als `bdcDSA` benannt. Sie können einen beliebigen Namen für dieses Konto auswählen.

1. Öffnen Sie auf dem Domänencontroller **Active Directory-Benutzer und -Computer**.

1. Navigieren Sie im linken Bereich zu Ihrer Domäne und dann zu der OE, die für `bdc` verwendet wird.

1. Klicken Sie mit der rechten Maustaste auf die OE, und wählen Sie **Eigenschaften** aus.

1. Wechseln Sie zur Registerkarte „Sicherheit“. Stellen Sie hierbei sicher, dass die Einstellung **Erweiterte Funktionen** aktiviert wurde, indem Sie mit der rechten Maustaste auf die OE klicken und dann **Anzeigen** auswählen.

    ![BDC-Objekteigenschaften](./media/deploy-active-directory/image15.png)

1. Klicken Sie auf **Hinzufügen…** , und fügen Sie den Benutzer **bdcDSA** hinzu.

    ![Hinzufügen von BDC-Objekteigenschaften](./media/deploy-active-directory/image16.png)

    ![Objekt auswählen](./media/deploy-active-directory/image17.png)

1. Wählen Sie den Benutzer **bdcDSA** aus, und deaktivieren Sie alle Berechtigungen. Klicken Sie anschließend auf **Erweitert**.

1. Klicken Sie auf **Hinzufügen**.

    ![Klicken Sie auf „Hinzufügen“.](./media/deploy-active-directory/image18.png)

    - Klicken Sie auf **Prinzipal auswählen**, fügen Sie **bdcDSA** ein, und klicken Sie auf „OK“.

    - Legen Sie den **Typ** auf **Zulassen** fest.

    - Legen Sie **Anwenden auf** auf **Dieses und alle untergeordneten Objekte** fest.

        ![„Zulassen“ für Eigenschaften festlegen](./media/deploy-active-directory/image19.png)

    - Scrollen Sie bis ganz nach unten, und klicken Sie auf **Alle deaktivieren**.

    - Scrollen Sie zurück nach oben, und aktivieren Sie die folgenden Berechtigungen:
       - **Alle Eigenschaften lesen**
       - **Alle Eigenschaften schreiben**
       - **Computerobjekte erstellen**
       - **Computerobjekte löschen**
       - **Gruppenobjekte erstellen**
       - **Gruppenobjekte löschen**
       - **Benutzerobjekte erstellen**
       - **Benutzerobjekte löschen**

    - Klicken Sie auf **OK**

- Klicken Sie auf **Hinzufügen**.

    - Klicken Sie auf **Prinzipal auswählen**, fügen Sie **bdcDSA** ein, und klicken Sie auf „OK“.

    - Legen Sie den **Typ** auf **Zulassen** fest.

    - Legen Sie **Anwenden auf** auf **Untergeordnete Computerobjekte** fest.

    - Scrollen Sie bis ganz nach unten, und klicken Sie auf **Alle deaktivieren**.

    - Scrollen Sie zurück nach oben, und wählen Sie **Kennwort zurücksetzen** aus.

    - Klicken Sie auf **OK**

- Klicken Sie auf **Hinzufügen**.

    - Klicken Sie auf **Prinzipal auswählen**, fügen Sie **bdcDSA** ein, und klicken Sie auf „OK“.

    - Legen Sie den **Typ** auf **Zulassen** fest.

    - Legen Sie **Anwenden auf** auf **Untergeordnete Benutzerobjekte** fest.

    - Scrollen Sie bis ganz nach unten, und klicken Sie auf **Alle deaktivieren**.

    - Scrollen Sie zurück nach oben, und wählen Sie **Kennwort zurücksetzen** aus.

    - Klicken Sie auf **OK**

- Klicken Sie zweimal auf **OK**, um die offenen Dialogfelder zu schließen.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] im Active Directory-Modus](active-directory-deploy.md)

[Behandeln von Problemen mit der Integration von Active Directory in SQL Server-Big Data-Clustern](troubleshoot-active-directory.md)

[Konzept: Bereitstellen von [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] im Active Directory-Modus](active-directory-deployment-background.md)
