---
title: Erstellen, Ändern und Löschen von freigegebenen Datenquellen | Microsoft-Dokumentation
description: In diesem Artikel erfahren Sie, wie Sie Datenquellen verwalten, auf die von mehreren Berichten, Modellen und datengesteuerten Abonnements verwiesen werden kann, die auf einem Berichtsserver in den Reporting Services ausgeführt werden.
ms.date: 05/24/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
helpviewer_keywords:
- modifying data source properties
- shared data sources [Reporting Services]
- removing shared data sources
- roles [Reporting Services], shared data sources
- data sources [Reporting Services], shared
- data sources [Reporting Services], modifying properties
- deleting shared data sources
ms.assetid: 1e58c1c2-5ecf-4ce6-9d04-0a8acfba17be
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 287347c10600b0038cf3489a0c8df8fae8a00661
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91934694"
---
# <a name="create-modify-and-delete-shared-data-sources-ssrs"></a>Erstellen, Ändern und Löschen von freigegebenen Datenquellen (SSRS)
  Eine freigegebene Datenquelle besteht aus einem Satz von Datenquellen-Verbindungseigenschaften, auf die von mehreren auf einem [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] -Berichtsserver ausgeführten Berichten, Modellen und datengesteuerten Abonnements verwiesen werden kann.  Freigegebene Datenquellen bieten eine einfache Möglichkeit, Datenquelleneigenschaften zu verwalten, die sich im Laufe der Zeit häufig ändern. Wenn sich ein Benutzerkonto oder Kennwort ändert oder Sie die Datenbank auf einen anderen Server verschieben, können Sie die Verbindungsinformationen zentral aktualisieren.  
  
 Freigegebene Datenquellen sind für Berichte und datengesteuerte Abonnements optional, für Berichtsmodelle jedoch erforderlich. Wenn Sie Berichtsmodelle für die Ad-hoc-Berichterstellung verwenden möchten, müssen Sie ein freigegebenes Datenquellenelement erstellen und verwalten, um Verbindungsinformationen für das Modell bereitzustellen.  
  
 Eine freigegebene Datenquelle setzt sich aus folgenden Komponenten zusammen:  
  
|Teil|BESCHREIBUNG|  
|----------|-----------------|  
|Name|Ein Name, der das Element innerhalb der Ordnerhierarchie des Berichtsservers identifiziert.|  
|BESCHREIBUNG|Eine Beschreibung, die mit dem Element im Webportal angezeigt wird, wenn Sie den Inhalt des Ordners anzeigen.|  
|Verbindungstyp|Die für die Datenquelle verwendete Datenverarbeitungserweiterung. Sie können nur Datenverarbeitungserweiterungen verwenden, die auf dem Berichtsserver bereitgestellt werden. Weitere Informationen zu in [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] enthaltenen Datenverarbeitungserweiterungen finden Sie unter [Von Reporting Services unterstützte Datenquellen (SSRS)](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).|  
|Verbindungszeichenfolge|Die Verbindungszeichenfolge für die Datenbank. Weitere Informationen und Beispiele für Verbindungszeichenfolgen bis hin zu häufig verwendeten Datenquellen finden Sie unter [Datenverbindungen, Datenquellen und Verbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).|  
|Anmeldeinformationstyp|Gibt an, wie Anmeldeinformationen für die Verbindung abgerufen werden und ob sie nach dem Herstellen der Verbindung verwendet werden sollen. Weitere Informationen finden Sie unter [Angeben der Anmeldeinformationen und Verbindungsinformationen für Berichtsdatenquellen](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md).|  
  
 Eine freigegebene Datenquelle enthält keine Abfrageinformationen für das Abrufen von Daten. Die Abfrage ist stets Bestandteil einer Berichtsdefinition.  
  
## <a name="creating-and-modifying-shared-data-sources"></a>Erstellen und Ändern freigegebener Datenquellen  
 Zum Erstellen einer freigegebenen Datenquelle oder zum Bearbeiten ihrer Eigenschaften ist die Berechtigung **Datenquellen verwalten** für den Berichtsserver erforderlich. Wenn der Berichtsserver im einheitlichen Modus ausgeführt wird, können Sie die freigegebene Datenquelle im Webportal erstellen und konfigurieren. Wird der Berichtsserver im integrierten SharePoint-Modus ausgeführt, können Sie die Anwendungsseiten einer SharePoint-Website verwenden. Unabhängig vom jeweiligen Modus können Sie für jeden Berichtsserver eine freigegebene Datenquelle im Berichts-Designer erstellen und sie anschließend auf einem Zielserver veröffentlichen.  
  
 Nachdem Sie eine freigegebene Datenquelle auf dem Berichtsserver erstellt haben, können Sie Rollenzuweisungen zur Steuerung des Zugriffs erstellen, die Datenquelle an einen anderen Speicherort verschieben, sie umbenennen sowie den Offlinemodus aktivieren, um die Berichtsverarbeitung zu verhindern, während Wartungsvorgänge für die externe Datenquelle ausgeführt werden. Falls Sie ein freigegebenes Datenquellenelement umbenennen oder in einen anderen Speicherort in der Ordnerhierarchie des Berichtsservers verschieben, werden die Pfadangaben in allen Berichten oder Abonnements, die auf die freigegebene Datenquelle verweisen, entsprechend aktualisiert. Wenn Sie die freigegebene Datenquelle offline schalten, werden alle Berichte, Modelle und Abonnements erst ausgeführt, wenn Sie die Datenquelle erneut aktivieren.  
  
 Weitere Informationen zum Steuern des Zugriffs auf freigegebene Datenquellen in der Ordnerhierarchie des Berichtsservers finden Sie unter [Sichern freigegebener Datenquellenelemente](../../reporting-services/security/secure-shared-data-source-items.md).  
  
 **So erstellen Sie eine freigegebene Datenquelle im Berichts-Designer**  
  
1.  Klicken Sie im Berichtsdatenbereich auf der Symbolleiste auf **Neu** , und klicken Sie dann auf **Datenquelle**. Das Dialogfeld **Datenquelleneigenschaften** wird angezeigt.  
  
    > [!NOTE]  
    >  Zum Anzeigen des Berichtsdatenbereichs klicken Sie im Menü **Ansicht** auf **Berichtsdaten** .  
  
2.  Geben Sie im Textfeld **Name** einen Namen für die Datenquelle ein, oder übernehmen Sie den Standardnamen. Der Name der Datenquelle wird intern für den Bericht verwendet. Zur Verdeutlichung sollte der Name der Datenquelle den Namen der Datenbank enthalten, die in der Verbindungszeichenfolge angegeben ist.  
  
3.  Vergewissern Sie sich, dass die Option **Freigegebenen Datenquellenverweis verwenden** ausgewählt ist, und machen Sie anschließend Folgendes.  
  
    1.  Klicken Sie auf **Neu**. Führen Sie im Eigenschaftendialogfeld **Freigegebene Datenquelle** die Schritte 2 und 3 aus, um eine neue Datenquelle zu erstellen.  
  
    2.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
         Die neue freigegebene Datenquelle wird im Projektmappen-Explorer im Ordner Freigegebene Datenquellen angezeigt.  
  
4.  Klicken Sie auf **Anmeldeinformationen**.  
  
     Geben Sie die Anmeldeinformationen für diese Datenquelle an. Der Besitzer der Datenquelle wählt den Typ von Anmeldeinformationen aus, die unterstützt werden.  
  
 **So erstellen Sie eine freigegebene Datenquelle im Webportal**  
  
1.  Wählen Sie im Webportal **Neu** > **Datenquelle** aus. 
  
4.  Geben Sie einen Namen für das Element ein. Ein Name muss mindestens ein Zeichen enthalten und muss mit einem Buchstaben beginnen. Er kann auch Sonderzeichen enthalten, er darf jedoch keine Leerzeichen und folgende Zeichen nicht enthalten: ; ? : \@ & = + , $ / * < > | " /.  
  
5.  Optional können Sie auch eine Beschreibung eingeben, um Benutzern Informationen zur Verbindung bereitzustellen.  
  
6.  Geben Sie in der Liste **Datenquellentyp** die Datenverarbeitungserweiterung an, die zum Verarbeiten von Daten aus der Datenquelle verwendet wird.  
  
7.  Geben Sie in das Feld **Verbindungszeichenfolge**die Verbindungszeichenfolge an, die vom Berichtsserver zum Herstellen der Verbindung zur Datenquelle verwendet wird. Es wird empfohlen, keine Anmeldeinformationen in der Verbindungszeichenfolge anzugeben.  
  
     Das folgende Beispiel zeigt eine Verbindungszeichenfolge, mit der eine Verbindung zur lokalen [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] AdventureWorks2016-Datenbank hergestellt wird:  
  
    ```  
    data source=<localservername>; initial catalog=AdventureWorks2016 
    ```  
  
8.  Geben Sie für **Verbindung herstellen über**an, wie die Anmeldeinformationen bei Ausführung des Berichts abgerufen werden:  
  
    -   Wenn der Benutzer zur Eingabe eines Anmeldenamens und eines Kennworts aufgefordert werden soll, klicken Sie auf **Bereitgestellte Anmeldeinformationen vom Benutzer, der den Bericht ausführt**. Klicken Sie auf **Beim Herstellen einer Verbindung mit der Datenquelle als Windows-Anmeldeinformationen verwenden**, um die vom Benutzer eingegebenen Anmeldeinformationen als Windows-Anmeldeinformationen zu verwenden. Wenn der Benutzername und das Kennwort Datenbank-Anmeldeinformationen darstellen, sollten Sie diese Option nicht aktivieren.  
  
    -   Wenn Sie die Datenquelle als freigegebene Datenquelle mit gespeicherten, vom Besitzer der Datenquelle verwalteten Anmeldeinformationen verwenden möchten bzw. für Berichte, die Abonnements oder andere geplante Vorgänge (z.B. automatisierte Berichtsverlaufgenerierung) unterstützen, klicken Sie auf **Anmeldeinformationen, die sicher auf dem Berichtsserver gespeichert sind**. Wenn der Datenbankserver Identitätswechsel oder Delegierung unterstützt, können Sie die Option **Die Identität des authentifizierten Benutzers annehmen, nachdem eine Verbindung zur Datenquelle hergestellt wurde**auswählen.  
  
    -   Wenn der Berichtsserver die Anmeldeinformationen des auf den Bericht zugreifenden Benutzers an den Server übergeben soll, der die externe Datenquelle hostet, klicken Sie auf **Integrierte Sicherheit von Windows**. In diesem Fall wird der Benutzer nicht aufgefordert, einen Benutzernamen oder ein Kennwort einzugeben.  
  
    -   Klicken Sie auf **Anmeldeinformationen sind nicht erforderlich**, wenn Sie eine Datenquelle verwenden, die nicht mit Anmeldeinformationen arbeitet (z.B. wenn es sich bei der Datenquelle um eine XML-Datei handelt, auf die vom Dateisystem zugegriffen wird). Diesen Typ Anmeldeinformationen sollten Sie nur dann angeben, wenn er von der Datenquelle unterstützt wird. Wenn Sie diese Option für eine Datenquelle aktivieren, die Authentifizierung erfordert, schlägt die Verbindungsherstellung fehl. Vergewissern Sie sich bei der Auswahl dieser Option, dass Sie das unbeaufsichtigte Ausführungskonto konfigurieren, mit dem der Berichtsserver eine Verbindung zu anderen Computern herstellen kann, um Daten oder Dateien abzurufen, wenn keine Anmeldeinformationen zur Verfügung stehen.  
  
         Weitere Informationen zum Konfigurieren von Anmeldeinformationen finden Sie unter [Angeben der Anmeldeinformationen und Verbindungsinformationen für Berichtsdatenquellen](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md). Weitere Informationen zum Konto für die unbeaufsichtigte Ausführung finden Sie unter [Konfigurieren des unbeaufsichtigten Ausführungskontos &#40;Berichtsserver-Konfigurations-Manager&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
9. Klicken Sie auf die Schaltfläche **Verbindung testen** , um die Datenquellenkonfiguration zu überprüfen.  
  
    > [!NOTE]  
    >  Die Schaltfläche zum Testen der Verbindung wird für den XML-Datenquellentyp nicht unterstützt.  
  
10. Klicken Sie auf **OK**  
  
 **So ändern Sie eine freigegebene Datenquelle im Webportal**  
  
1.  Navigieren Sie im Webportal zur freigegebenen Datenquelle.  
  
2.  Klicken Sie auf die Auslassungspunkte (...) in der oberen rechten Ecke der freigegebenen Datenquelle und anschließend auf **Verwalten**.   

    Die Seite **Eigenschaften** wird geöffnet.
  
3.  Ändern Sie die Datenquelle, und klicken Sie anschließend auf **Anwenden**.  
  
## <a name="deleting-shared-data-sources"></a>Löschen von freigegebenen Datenquellen  
 Sie können eine freigegebene Datenquelle auf die gleiche Weise löschen wie ein Element auf dem Berichtsserver.  
  
 **So löschen Sie eine freigegebene Datenquelle**  
  
1. Navigieren Sie im Webportal zur freigegebenen Datenquelle.  
  
2.  Klicken Sie auf die Auslassungspunkte (...) in der oberen rechten Ecke der freigegebenen Datenquelle und anschließend auf **Verwalten**.    
    Die Seite **Eigenschaften** wird geöffnet.
  
3. Klicken Sie auf **Löschen**, und klicken Sie anschließend auf **OK**.  
  
Das Löschen der freigegebenen Datenquelle deaktiviert alle Berichte, Modelle und datengesteuerten Abonnements, die sie verwenden. Ohne die Verbindungsinformationen für die Datenquelle werden die Elemente nicht mehr ausgeführt. Zum Aktivieren dieser Elemente müssen Sie jedes Element einzeln öffnen und wie folgt vorgehen:  
  
-   Für Berichte und datengesteuerte Abonnements, die auf die freigegebene Datenquelle verweisen, können Sie die Verbindungsinformationen für die Datenquelle in Berichtseigenschaften oder Abonnements angeben, oder Sie können eine neue freigegebene Datenquelle auswählen, die die Werte, die Sie verwenden möchten, enthält.  
  
-   Für Modelle und Berichts-Generator-Berichte, die dieses Modell verwenden, müssen Sie eine neue freigegebene Datenquelle angeben. Modelle erhalten Verbindungsinformationen für die Datenquelle nur über freigegebene Datenquellen.  
  
 Das Löschen einer freigegebenen Datenquelle lässt sich nicht rückgängig machen. Wenn Sie versehentlich eine freigegebene Datenquelle löschen, können Sie eine neue Datenquelle mit denselben Eigenschaftswerten erstellen. Sie müssen jeden einzelnen Bericht, jedes Modell und jedes datengesteuerte Abonnement öffnen, um die freigegebene Datenquelle erneut mit dem Element zu verbinden, das die Datenquelle verwendet. Solange die Datenquelleneigenschaften gleich bleiben, funktionieren die Berichte, Modelle und Abonnements jedoch weiterhin wie zuvor.  
  
## <a name="importing-shared-data-sources"></a>Importieren von freigegebenen Datenquellen  

**So importieren Sie eine vorhandene Datenquelle in Berichts-Designer**  
  
1.  Klicken Sie im Projektmappen-Explorer im Berichtsserverprojekt mit der rechten Maustaste auf den Ordner **Freigegebene Datenquellen** , und klicken Sie anschließend auf **Vorhandenes Element hinzufügen**. Das Dialogfeld **Vorhandenes Element hinzufügen** wird geöffnet.  
  
2.  Navigieren Sie zu einer vorhandenen RDS-Datenquelldatei (Report Definition Shared), und klicken Sie auf **Öffnen**.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="shared-data-sources-in-sharepoint"></a>Freigegebene Datenquellen in SharePoint  
 Beim Ausführen eines Berichts aus einer SharePoint-Bibliothek können die Verbindungsinformationen innerhalb des Berichts oder in einer externen Datei definiert werden, die mit dem Bericht verknüpft ist. Falls die Verbindungsinformationen in den Bericht eingebettet sind, wird die Datenquelle als benutzerdefinierte Datenquelle bezeichnet. Sind die Verbindungsinformationen in einer externen Datei definiert, wird sie als freigegebene Datenquelle bezeichnet. Bei der externen Datei kann es sich um eine RSDS-Datei (Report Server Data Source, Berichtsserver-Datenquellendatei) oder um eine Office Data Connection-Datei (ODC-Datei) handeln.  
  
 Eine RSDS-Datei ist mit einer RDS-Datei vergleichbar, aber sie weist ein anderes Schema auf. Zum Erstellen einer RSDS-Datei können Sie eine RDS-Datei aus dem Berichts-Designer oder dem Modell-Designer in einer SharePoint-Bibliothek veröffentlichen (aus der ursprünglichen RDS-Datei wird eine neue RSDS-Datei erstellt). Alternativ können Sie eine neue Datei in einer Bibliothek auf einer SharePoint-Website erstellen.  
  
 Nachdem Sie eine freigegebene Datenquelle erstellt oder veröffentlicht haben, können Sie die Verbindungseigenschaften bearbeiten oder die Datei löschen, wenn sie nicht mehr verwendet wird. Bevor Sie eine freigegebene Datenquelle löschen, sollten Sie bestimmen, ob sie von Berichten oder Berichtsmodellen verwendet wird. Zeigen Sie dazu abhängige Elemente an, die auf die freigegebene Datenquelle verweisen.  
  
 Obwohl Sie in der Liste der abhängigen Elemente Informationen erhalten, ob auf die freigegebene Datenquelle verwiesen wird, erfahren Sie nicht, ob sie aktiv verwendet wird. Um zu bestimmen, ob die freigegebene Datenquelle oder das Modell aktiv verwendet wird, können Sie die Protokolldateien auf dem Berichtsservercomputer überprüfen. Wenn Sie keinen Zugriff auf die Protokolldateien haben oder wenn die Dateien die gewünschten Informationen nicht enthalten, können Sie den Bericht während der Statusbestimmung in einen Ordner verschieben, auf den kein Zugriff besteht.  
  
 **So erstellen Sie eine freigegebene Datenquellendatei (RSDS-Datei) in Sharepoint 2010**  
  
1.  Klicken Sie auf dem Bibliothekmenüband auf die Registerkarte **Dokumente** .  
  
2.  Klicken Sie im Menü **Neues Dokument** auf **Berichtsdatenquelle**.  
  
    > [!NOTE]  
    >  Wenn das Element **Berichtsdatenquelle** nicht im Menü angezeigt wird, wurde der Inhaltstyp für Berichtsdatenquellen noch nicht aktiviert. Weitere Informationen finden Sie unter [Hinzufügen von Reporting Services-Inhaltstypen zu einer SharePoint-Bibliothek](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md).  
  
3.  Geben Sie unter **Name**einen beschreibenden Namen für die RSDS-Datei ein.  
  
4.  Wählen Sie unter **Datenquellentyp**den Datenquellentyp aus der Liste aus. Weitere Informationen finden Sie unter [Von Reporting Services unterstützte Datenquellen &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
5.  Geben Sie unter **Connection String**einen Zeiger auf die Datenquelle und alle anderen Einstellungen an, die zum Herstellen einer Verbindung mit der externen Datenquelle erforderlich sind. Die Syntax der Verbindungszeichenfolge wird durch den von Ihnen verwendeten Datenquellentyp bestimmt. Weitere Informationen und Beispiele finden Sie unter [Datenverbindungen, Datenquellen und Verbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
6.  Geben Sie in **Anmeldeinformationen**an, wie der Berichtsserver Anmeldeinformationen für den Zugriff auf die externe Datenquelle erhält. Anmeldeinformationen können für die unbeaufsichtigte Berichtsverarbeitung gespeichert, angefordert, integriert oder konfiguriert werden.  
  
    -   Wählen Sie **Windows-Authentifizierung (integriert)** aus, wenn der Zugriff auf die Daten mit den Anmeldeinformationen des Benutzers erfolgen soll, der den Bericht geöffnet hat. Wählen Sie diese Option nicht aus, wenn für die SharePoint-Website oder -Farm die Formularauthentifizierung verwendet oder über ein vertrauenswürdiges Konto eine Verbindung mit dem Berichtsserver hergestellt wird. Wählen Sie diese Option nicht aus, wenn Sie eine Abonnement- oder Datenverarbeitung für diesen Bericht planen möchten. Diese Option kann am besten verwendet werden, wenn die Kerberos-Authentifizierung für die Domäne aktiviert ist oder wenn sich die Datenquelle auf demselben Computer wie der Berichtsserver befindet. Wenn die Kerberos-Authentifizierung nicht aktiviert ist, können die Windows-Anmeldeinformationen nur an einen anderen Computer weitergegeben werden. Dies bedeutet, dass statt der erwarteten Daten ein Fehler angezeigt wird, wenn sich die externe Datenquelle auf einem anderen Computer befindet, für den eine zusätzliche Verbindung erforderlich ist.  
  
    -   Wählen Sie **Zur Eingabe der Anmeldeinformationen auffordern** aus, wenn der Benutzer die Anmeldeinformationen bei jeder Ausführung des Berichts eingeben soll. Wählen Sie diese Option nicht aus, wenn Sie eine Abonnement- oder Datenverarbeitung für diesen Bericht planen möchten.  
  
    -   Wählen Sie **Gespeicherte Anmeldeinformationen** aus, wenn der Zugriff auf die Daten mit einem einzigen Satz Anmeldeinformationen erfolgen soll. Die Anmeldeinformationen werden vor der Speicherung verschlüsselt. Sie können Optionen auswählen, die bestimmen, wie die gespeicherten Anmeldeinformationen authentifiziert werden. Wählen Sie Windows-Anmeldeinformationen verwenden aus, wenn die gespeicherten Anmeldeinformationen zu einem Windows-Benutzerkonto gehören. Wählen Sie **Ausführungskontext für dieses Konto festlegen** aus, wenn Sie den Ausführungskontext auf dem Datenbankserver festlegen möchten. Bei [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -Datenbanken kann mit dieser Option die SETUSER-Funktion festgelegt werden. Weitere Informationen finden Sie unter [SETUSER (Transact-SQL)](../../t-sql/statements/setuser-transact-sql.md).  
  
    -   Wählen Sie **Anmeldeinformationen sind nicht erforderlich** aus, wenn Sie Anmeldeinformationen in der Verbindungszeichenfolge angeben möchten oder wenn Sie den Bericht mithilfe eines Kontos mit Minimalprivilegien ausführen möchten, das auf dem Berichtsserver konfiguriert ist. Ist das Konto nicht auf dem Berichtsserver konfiguriert, werden die Anmeldeinformationen von den Benutzern angefordert, und geplante Vorgänge, die Sie für den Bericht definieren, werden nicht ausgeführt.  
  
7.  Wählen Sie **Diese Datenquelle aktivieren** aus, wenn die Datenquelle aktiviert sein soll. Wenn die Datenquelle konfiguriert aber nicht aktiv ist, wird Benutzern beim Versuch, einen Bericht auf Grundlage der Datenquelle zu verwenden, eine Fehlermeldung angezeigt.  
  
8.  Klicken Sie auf die Schaltfläche **Verbindung testen** , um die Datenquellenkonfiguration zu überprüfen.  
  
    > [!NOTE]  
    >  Die Schaltfläche zum Testen der Verbindung wird für den XML-Datenquellentyp nicht unterstützt.  
  
9. Klicken Sie auf **OK** , um die freigegebene Datenquelle zu speichern.  
  
 **So löschen Sie eine freigegebene Datenquellendatei (RSDS-Datei)**  
  
1.  Öffnen Sie die Bibliothek, die die RSDS-Datei enthält.  
  
2.  Zeigen Sie auf die freigegebene Datenquelle.  
  
3.  Klicken Sie, um einen Pfeil nach unten anzuzeigen, und klicken Sie auf **Löschen**.  
  
 Wenn Sie versehentlich eine freigegebene Datenquelle löschen, die beibehalten werden sollte, können Sie eine neue Datenquelle mit denselben Verbindungsinformationen erstellen. Nachdem Sie die freigegebene Datenquelle erneut erstellt haben, müssen Sie jeden Bericht und jedes Berichtsmodell, von dem diese Datenquelle verwendet wurde, öffnen und die freigegebene Datenquelle auswählen. Der Name, die Anmeldeinformationen und die Syntax der Verbindungszeichenfolge des neuen freigegebenen Datenquellenelements müssen nicht mit denen des gelöschten übereinstimmen. Solange die Verbindung zur selben Datenquelle aufgelöst wird, können Datenquelleneigenschaften von den ursprünglichen Werten abweichen.  
  
 Gehen Sie beim Löschen eines Berichtsmodells umsichtig vor. Wenn Sie ein Modell löschen, können Sie die Berichte, die auf diesem Modell im Berichts-Generator basieren, nicht mehr öffnen und ändern. Wenn Sie versehentlich ein Modell löschen, das von vorhandenen Berichten verwendet wird, müssen Sie das Modell erneut generieren, alle Berichte, von denen das Modell verwendet wird, erneut erstellen und speichern und die zu verwendende Modellelementsicherheit erneut angeben. Es ist nicht möglich, das Modell einfach neu zu generieren und es anschließend einem vorhandenen Bericht anzufügen.  
  
## <a name="dependent-items"></a>Abhängige Elemente  
 Um eine Liste der Berichte und Modelle anzuzeigen, die die Datenquelle verwenden, öffnen Sie die Seite Abhängige Elemente für die freigegebene Datenquelle. Um auf diese Seite zuzugreifen, öffnen Sie die Datenquelle im Webportal oder auf einer SharePoint-Anwendungsseite. Beachten Sie, dass die Seite Abhängige Elemente keine datengesteuerten Abonnements anzeigt. Wenn eine freigegebene Datenquelle von einem Abonnement verwendet wird, wird das Abonnement nicht in der Liste der abhängigen Elemente aufgeführt.  
  
 **So zeigen Sie abhängige Elemente in SharePoint an**  
  
1.  Öffnen Sie die Bibliothek, die die RSDS-Datei enthält.  
  
2.  Zeigen Sie auf die freigegebene Datenquelle.  
  
3.  Klicken Sie, um einen Pfeil nach unten anzuzeigen, und wählen Sie **Abhängige Elemente anzeigen**aus.  
  
     Für Berichtsmodelle enthält die Liste der abhängigen Elemente die Berichte, die im Berichts-Generator erstellt wurden. Bei freigegebenen Datenquellen kann die Liste der abhängigen Elemente sowohl Berichte als auch Berichtsmodelle enthalten.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Erstellen von Datenverbindungszeichenfolgen (Berichts-Generator und SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)   
 [Verwalten von Berichtsdatenquellen](../../reporting-services/report-data/manage-report-data-sources.md)   
 [Konfigurieren von Datenquelleneigenschaften für einen paginierten Bericht](../../reporting-services/report-data/configure-data-source-properties-for-a-report-report-manager.md)  
  
  
