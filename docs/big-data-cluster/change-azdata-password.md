---
title: Aktualisieren von AZDATA_PASSWORD
description: '`AZDATA_PASSWORD` manuell aktualisieren'
author: NelGson
ms.author: negust
ms.reviewer: mikeray
ms.date: 03/05/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 71fe901915b533c4af25e98c3573aaaa00b82d53
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247386"
---
# <a name="manually-update-azdata_password"></a>Manuelles Aktualisieren von `AZDATA_PASSWORD`

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Unabhängig davon, ob [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] mit Active Directory-Integration arbeitet oder nicht, wird während der Bereitstellung `AZDATA_PASSWORD` festgelegt. Es ermöglicht eine Standardauthentifizierung bei Clustercontroller und Masterinstanz. In diesem Dokument wird erläutert, wie `AZDATA_PASSWORD` manuell aktualisiert wird.

## <a name="change-azdata_password-for-controller"></a>Ändern von `AZDATA_PASSWORD` für den Controller

Wenn der Cluster nicht im Active Directory-Modus betrieben wird, aktualisieren Sie das Apache Knox Gateway-Kennwort wie folgt:

1. Rufen Sie die [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]-Anmeldeinformationen des Controllers ab, indem Sie die folgenden Befehle ausführen:

   a. Führen Sie diesen Befehl als Kubernetes-Administrator aus:

   ```bash
   kubectl get secret controller-sa-secret -n <cluster name> -o yaml | grep password
   ```

   b. Decodieren Sie das Geheimnis mit Base64:

   ```bash
   echo <password from kubectl command>  | base64 --decode && echo
   ```

1. Machen Sie in einem separaten Befehlsfenster den Port des Datenbankservers des Controllers verfügbar:

   ```bash
   kubectl port-forward controldb-0 1433:1433 --address 0.0.0.0 -n <cluster name>
   ```
 
1. Verwenden Sie das Kennwort des Systemadministrators, das Sie soeben erhalten haben, um sich in einem SQL-Clienttool mit dem Datenbankserver des Controllers zu verbinden.

1. Generieren Sie ein neues komplexes Kennwort für `AZDATA_USERNAME`, um das vorhandene `AZDATA_PASSWORD` zu ersetzen.

   Um das Beispiel zu vereinfachen, wird in den nächsten Schritten „newPassword“ verwendet, da das generierte Kennwort „newPassword“ ist. 

1. Rufen Sie `hexsalt` aus der Tabelle „users“ ab:

   ```sql
   SELECT hexsalt FROM [auth].[users] WHERE username = '<username>'
   ```

   `hexsalt` gibt eine zufällige hexadezimale Zeichenfolge zurück (z. B. `64FC59DF31244FFEE02F457BC0750226`).

1. Verschlüsseln Sie das neue komplexe Kennwort mithilfe von `hexsalt`:

   Zur Vereinfachung stellen wir Ihnen das vorkonfigurierte Tool `pbkdf2` zur Verschlüsselung des Kennworts zur Verfügung. Laden Sie die für die Plattform geeignete .NET Core-App für [`pbkdf2`](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/security/password-hashing/pbkdf2/prebuilt-binaries)herunter.

   Die App ist eigenständig und weist keine Voraussetzungen wie z. B. .NET-Runtimes auf. Führen Sie zum Verschlüsseln des Kennworts Folgendes aus:

   ```bash
   pbkdf2 <password> <hexsalt>
   J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=
   ```

1. Aktualisieren Sie das Kennwort in der Tabelle „users“:

   ```sql
   UPDATE [auth].[users] SET password = 'J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=' WHERE username = '<username>'
   ```

## <a name="change-azdata_password-in-the-sql-server-master-instance"></a>Ändern Sie `AZDATA_PASSWORD` in der SQL Server-Masterinstanz.

1. Verbinden Sie sich über ein beliebiges Administratorkonto mit dem SQL Server-Masterendpunkt.

1. Um das Kennwort für die Anmeldeinformationen zu ändern, die Sie während der Bereitstellung im Parameter `AZDATA_USERNAME` festgelegt haben, führen Sie den folgenden TSQL-Befehl aus:

   ```sql
   ALTER LOGIN <AZDATA_USERNAME> WITH PASSWORD = 'newPassword'
   ```

## <a name="manually-updating-password-for-grafana-and-kibana"></a>Manuelles Aktualisieren des Kennworts für Grafana und Kibana

Nachdem Sie die Schritte zum Aktualisieren von AZDATA_PASSWORD ausgeführt haben, werden Sie sehen, dass [Grafana](app-monitor.md) und [Kibana](cluster-logging-kibana.md) weiterhin das alte Kennwort akzeptieren. Das liegt daran, dass Grafana und Kibana keinen Einblick in das neue Kubernetes-Geheimnis haben. Sie müssen das Kennwort für Grafana und Kibana separat manuell aktualisieren.

## <a name="update-grafana-password"></a>Aktualisieren des Kennworts für Grafana

Befolgen Sie diese Optionen, um das Kennwort für [Grafana](app-monitor.md) manuell zu aktualisieren.

1. Das Hilfsprogramm „htpasswd“ ist erforderlich. Sie können es auf einem beliebigen Clientcomputer installieren.
  
### <a name="for-ubuntu"></a>[Für Ubuntu](#tab/for-ubuntu)
Auf Linux können Sie Folgendes verwenden:
```bash
sudo apt install apache2-utils
```
### <a name="for-rhel"></a>[Für RHEL](#tab/for-rhel)
Auf Red Hat Enterprise Linux können Sie Folgendes verwenden:
```bash
sudo yum install httpd-tools
```
---

2. Generieren Sie das neue Kennwort. 
    
    ```bash
    htpasswd -nbs <username> <password>
    admin:{SHA}<secret>
    ```
    
    Ersetzen Sie z. B. die Werte für /<username/>, /<password/>, /<secret/> wie gewünscht:
    
    ```bash
    htpasswd -nbs admin Test@12345
    admin:{SHA}W/5VKRjIzjusUJ0ih0gHyEPjC/s=
    ```

3. Codieren Sie nun das Kennwort:
    
    ```bash
    echo "admin:{SHA}W/5VKRjIzjusUJ0ih0gHyEPjC/s=" | base64
    ```             
    
    Bewahren Sie die ausgegebene base64-Zeichenfolge für später auf.
    
4. Bearbeiten Sie als Nächstes das mgmtproxy-secret:
    
    ```bash
    kubectl edit secret -n mssql-cluster mgmtproxy-secret
    ```
         
5. Aktualisieren Sie controller-login-htpasswd mit der oben generierten neuen codierten base64 Zeichenfolge für das Kennwort:
    
    ```console
    # Please edit the object below. Lines beginning with a '#' will be ignored,
    # and an empty file will abort the edit. If an error occurs while saving this file will be
    # reopened with the relevant failures.
    #
    apiVersion: v1
    data:
       controller-login-htpasswd: <base64 string from before>
       mssql-internal-controller-password: <password>
       mssql-internal-controller-username: <username>
    ```         

6. Ermitteln und löschen Sie den Pod „mgmtproxy“. 
     
    Ermitteln Sie ggf. den Namen Ihres Pods „mgmtproxy“.
    
    ### <a name="for-windows"></a>[Für Windows](#tab/for-windows)
     Auf einem Windows-Server können Sie Folgendes verwenden:
    ```bash
    kubectl get pods -n <namespace> -l app=mgmtproxy
    ```
    ### <a name="for-linux"></a>[Für Linux](#tab/for-linux)
     Unter Linux können Sie Folgendes verwenden:
    ```bash
    kubectl get pods -n <namespace> | grep 'mgmtproxy'
    ```
    ---

     Entfernen Sie den Pod „mgmtproxy“:
    ```bash
    kubectl delete pod mgmtproxy-xxxxx -n mssql-clutser
    ```

7. Warten Sie darauf, dass der Pod mgmtproxy online geht und das Grafana-Dashboard gestartet wird.  
 
    Die Wartezeit ist nicht signifikant, sodass der Pod binnen weniger Sekunden online sein sollte. Um den Status des Pods zu überprüfen, können Sie denselben Befehl `get pods` wie im vorherigen Schritt verwenden. 

    Wenn Sie sehen, dass der Pod „mgmtproxy“ nicht sofort in den Status „Bereit“ zurückkehrt, verwenden Sie kubectl, um den Pod zu beschreiben: 

    ```bash
    kubectl describe pods mgmtproxy-xxxxx  -n <namespace>
    ```   
    
    Nutzen Sie zur Problembehandlung und weiteren Protokollerfassung den Azure Data-CLI-Befehl [azdata bdc debug copy-logs](../azdata/reference/reference-azdata-bdc-debug.md).   

    
8. Melden Sie sich nun mit dem neuen Kennwort bei Grafana an. 


## <a name="update-the-kibana-password"></a>Aktualisieren des Kennworts für Kibana

Befolgen Sie diese Optionen, um das Kennwort für [Kibana](cluster-logging-kibana.md) manuell zu aktualisieren.

> [!NOTE]
> Der ältere Microsoft Edge-Browser ist nicht mit Kibana kompatibel. Sie müssen den Edge Chromium-basierten Browser verwenden, damit das Dashboard ordnungsgemäß angezeigt wird. Wenn Sie die Dashboards in einem nicht unterstützten Browser laden, wird eine leere Seite angezeigt. Weitere Informationen finden Sie unter [Unterstützte Browser für Kibana](https://www.elastic.co/support/matrix#matrix_browsers).

1. Öffnen Sie die Kibana-URL.
    
    Sie können die Endpunkt-URL des Kibana-Diensts in [Azure Data Studio](manage-with-controller-dashboard.md#controller-dashboard) finden oder den folgenden **azdata**-Befehl verwenden:
    
    ```azurecli
    azdata login
    azdata bdc endpoint list -e logsui -o table
    ```
    
    Beispiel: https://11.111.111.111:30777/kibana/app/kibana#/discover

2. Klicken Sie im linken Bereich auf die Option **Sicherheit**.
    
    ![Screenshot des Menüs im linken Bereich von Kibana, mit ausgewählter Option „Sicherheit“](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-1.jpg)

3. Klicken Sie auf der Seite „Sicherheit“ unter der Überschrift „Authentifizierungs-Back-Ends“ auf **Interne Benutzerdatenbank**.

    ![Screenshot der Seite „Sicherheit“, mit ausgewähltem Feld „Interne Benutzerdatenbank“.](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-2.jpg)

4. Jetzt sehen Sie die Liste der Benutzer unter der Überschrift „Interne Benutzerdatenbank“. Auf dieser Seite können Sie Benutzer für den Zugriff auf Kibana-Endpunkte hinzufügen, ändern und entfernen. Für Benutzer, die ein aktualisiertes Kennwort benötigen, klicken Sie auf der rechten Seite für den jeweiligen Benutzer auf die Schaltfläche **Bearbeiten**.

    ![Screenshot der Seite „Interne Benutzerdatenbank“. In der Benutzerliste ist für den Benutzer KubeAdmin die Schaltfläche „Bearbeiten“ ausgewählt.](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-3.jpg)

5. Geben Sie das neue Kennwort zweimal ein, und klicken Sie auf **Senden**:

    ![Screenshot des Bearbeitungsformulars „Interner Benutzer“. In den Feldern „Kennwort“ und „Kennwort wiederholen“ wurde ein neues Kennwort eingegeben.](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-4.jpg)

6. Schließen Sie den Browser, und verbinden Sie sich mit dem aktualisierten Kennwort erneut mit der Kibana-URL.

> [!Note]
> Wenn Sie nach der Anmeldung mit dem neuen Kennwort leere Seiten in Kibana sehen, melden Sie sich über die Option „Abmelden“ rechts oben manuell ab und dann erneut an.

## <a name="see-also"></a>Weitere Informationen

* [azdata bdc (Azure Data CLI)](../azdata/reference/reference-azdata-bdc.md)  
* [Überwachen von Anwendungen mit azdata und einem Grafana-Dashboard](app-monitor.md)   
* [Auswerten von Clusterprotokollen mit einem Kibana-Dashboard](cluster-logging-kibana.md)   
