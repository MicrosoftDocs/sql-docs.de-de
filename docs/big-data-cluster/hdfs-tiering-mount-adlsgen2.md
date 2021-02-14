---
title: Einbinden von ADLS Gen2 für HDFS-Tiering
titleSuffix: How to mount ADLS Gen2
description: Dieser Artikel enthält ein Beispiel für die Konfiguration von HDFS-Tiering mit einer Azure Data Lake Storage Gen2-Datenquelle.
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 06/29/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b41ad6a56da30fb30677d781630e30ad5e07d333
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046550"
---
# <a name="how-to-mount-adls-gen2-for-hdfs-tiering-in-a-big-data-cluster"></a>Einbinden von ADLS Gen2 für HDFS-Tiering in einen Big Data-Cluster

Die folgenden Abschnitte zeigen ein Beispiel für die Konfiguration von HDFS-Tiering mit einer Azure Data Lake Storage Gen2-Datenquelle.

## <a name="prerequisites"></a>Voraussetzungen

- [Bereitgestellte Big Data-Cluster](deployment-guidance.md)
- [Big-Data-Tools](deploy-big-data-tools.md)
  - **azdata**
  - **kubectl**

## <a name="load-data-into-azure-data-lake-storage"></a><a id="load"></a> Laden von Daten in Azure Data Lake Storage Gen2

Im folgenden Abschnitt wird beschrieben, wie Sie Azure Data Lake Storage Gen2 zum Testen des HDFS-Tierings einrichten. Wenn Sie bereits Daten in Azure Data Lake Storage gespeichert haben, können Sie diesen Abschnitt überspringen, um Ihre eigenen Daten zu verwenden.

1. [Erstellen eines Azure Data Lake Storage Gen2-Speicherkontos](/azure/storage/blobs/data-lake-storage-quickstart-create-account).

1. [Erstellen Sie ein Dateisystem](/azure/storage/blobs/data-lake-storage-explorer) in diesem Speicherkonto für Ihre Daten.

1. Laden Sie eine CSV- oder Parquet-Datei in den Container hoch. Dies sind die externen HDFS-Daten, die im Big Data-Cluster in HDFS eingebunden werden.

## <a name="credentials-for-mounting"></a>Anmeldeinformationen für die Einbindung

### <a name="use-oauth-credentials-to-mount"></a>Verwenden von OAuth-Anmeldeinformationen zum Einbinden

Um OAuth-Anmeldeinformationen zum Einbinden zu verwenden, müssen Sie die folgenden Schritte ausführen:

1. [Navigieren Sie zum Azure-Portal](https://portal.azure.com).
1. Rufen Sie Azure Active Directory auf. Dieser Dienst sollte Ihnen in der linken Navigationsleiste angezeigt werden.
1. Klicken Sie in der rechten Navigationsleiste auf „App-Registrierungen“, und erstellen Sie eine neue Registrierung.
1. Erstellen Sie eine Web-App, und befolgen Sie die Anweisungen des Assistenten. **Merken Sie sich den Namen der App, die Sie hier erstellen**. Sie müssen diesen Namen Ihrem ADLS-Konto als autorisierter Benutzer hinzufügen. Notieren Sie sich auch die Anwendungsclient-ID, die in der Übersicht angezeigt wird, wenn Sie die App auswählen.
1. Navigieren Sie zu „Certificates&secrets“ (Zertifikate und Geheimnisse), sobald die neue Webanwendung erstellt wurde, erstellen Sie einen **neuen geheimen Clientschlüssel**, und wählen Sie eine Schlüsseldauer aus. **Fügen Sie das Geheimnis hinzu**.
1. Navigieren Sie zurück zur Seite App-Registrierungen, und klicken Sie oben auf „Endpunkte“. **Notieren Sie sich die URL des „OAuth-Tokenendpunkts (v2)“** .
1. Für OAuth sollten Sie nun folgende Punkte notiert haben:

    - Die „Anwendungsclient-ID“ der Webanwendung
    - Den geheimen Clientschlüssel
    - Den Tokenendpunkt

### <a name="adding-the-service-principal-to-your-adls-account"></a>Hinzufügen des Dienstprinzipals zu Ihrem ADLS-Konto

1. Rufen Sie erneut das Portal auf, navigieren Sie zum Dateisystem Ihres ADLS-Speicherkontos, und klicken Sie im linken Menü auf „Zugriffssteuerung (IAM)“.
1. Klicken Sie auf „Rollenzuweisung hinzufügen“. 
1. Wählen Sie die Rolle „Mitwirkender an Storage-Blobdaten“ aus.
1. Suchen Sie nach dem Namen, den Sie oben erstellt haben (beachten Sie, dass er nicht in der Liste angezeigt wird, aber Sie finden ihn, wenn Sie nach dem vollständigen Namen suchen).
1. Speichern Sie die Rolle.

Warten Sie 5-10 Minuten, bevor Sie die Anmeldeinformationen für die Einbindung verwenden.

### <a name="set-environment-variable-for-oauth-credentials"></a>Festlegen der Umgebungsvariablen für OAuth-Anmeldeinformationen

Öffnen Sie eine Eingabeaufforderung auf einem Clientcomputer, der auf Ihren Big Data-Cluster zugreifen kann. Legen Sie eine Umgebungsvariable gemäß dem folgenden Format fest. Die Anmeldeinformationen müssen sich in einer durch Kommas getrennten Liste befinden. Der „Set“-Befehl wird unter Windows verwendet. Wenn Sie Linux verwenden, verwenden Sie stattdessen „export“.

**Hinweis:** Sie müssen alle Zeilenumbrüche oder Leerzeichen zwischen den Kommas („,“) entfernen, wenn Sie die Anmeldeinformationen eingeben. Die folgende Formatierung dient lediglich der Lesbarkeit.

```console
   set MOUNT_CREDENTIALS=fs.azure.account.auth.type=OAuth,
   fs.azure.account.oauth.provider.type=org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider,
   fs.azure.account.oauth2.client.endpoint=[token endpoint],
   fs.azure.account.oauth2.client.id=[Application client ID],
   fs.azure.account.oauth2.client.secret=[client secret]
```

## <a name="use-access-keys-to-mount"></a>Verwenden von Zugriffsschlüsseln zum Einbinden

Sie können die Einbindung auch mithilfe von Zugriffsschlüsseln durchführen, die Sie für Ihr ADLS-Konto im Azure-Portal abrufen können.

 > [!TIP]
   > Weitere Informationen zum Suchen des Zugriffsschlüssels (`<storage-account-access-key>`) für Ihr Speicherkonto finden Sie unter [Anzeigen von Kontoschlüsseln und Verbindungszeichenfolgen](/azure/storage/common/storage-account-keys-manage#view-access-keys-and-connection-string).

### <a name="set-environment-variable-for-access-key-credentials"></a>Festlegen der Umgebungsvariablen für Zugriffsschlüssel-Anmeldeinformationen

1. Öffnen Sie eine Eingabeaufforderung auf einem Clientcomputer, der auf Ihren Big Data-Cluster zugreifen kann.

1. Öffnen Sie eine Eingabeaufforderung auf einem Clientcomputer, der auf Ihren Big Data-Cluster zugreifen kann. Legen Sie eine Umgebungsvariable im folgenden Format fest. Die Anmeldeinformationen müssen sich in einer durch Kommas getrennten Liste befinden müssen. Der „Set“-Befehl wird unter Windows verwendet. Wenn Sie Linux verwenden, verwenden Sie stattdessen „export“.

**Hinweis:** Sie müssen alle Zeilenumbrüche oder Leerzeichen zwischen den Kommas („,“) entfernen, wenn Sie die Anmeldeinformationen eingeben. Die folgende Formatierung dient lediglich der Lesbarkeit.

```console
set MOUNT_CREDENTIALS=fs.azure.abfs.account.name=<your-storage-account-name>.dfs.core.windows.net,
fs.azure.account.key.<your-storage-account-name>.dfs.core.windows.net=<storage-account-access-key>
```

## <a name="mount-the-remote-hdfs-storage"></a><a id="mount"></a> Einbinden des HDFS-Remotespeichers

Nachdem Sie die MOUNT_CREDENTIALS-Umgebungsvariable für Zugriffsschlüssel oder die Verwendung von OAuth festgelegt haben, können Sie mit der Einbindung beginnen. In den folgenden Schritten wird der HDFS-Remotespeicher in Azure Data Lake in den lokalen HDFS-Speicher Ihres Big Data-Clusters eingebunden.

1. Suchen Sie mit **kubectl** die IP-Adresse für den Endpunkt **controller-svc-external**-Dienst in Ihrem Big Data-Cluster. Suchen Sie nach der **externen IP-Adresse**.

   ```bash
   kubectl get svc controller-svc-external -n <your-big-data-cluster-name>
   ```

1. Melden Sie sich mit **azdata** über die externe IP-Adresse des Controllerendpunkts mit Ihrem Benutzernamen und -kennwort für den Cluster an:

   ```bash
   azdata login -e https://<IP-of-controller-svc-external>:30080
   ```
1. Legen Sie die Umgebungsvariable MOUNT_CREDENTIALS fest (scrollen Sie für die Anweisungen nach oben).

1. Binden Sie den HDFS-Remotespeicher mit dem Befehl **azdata bdc hdfs mount create** in Azure ein. Ersetzen Sie die Platzhalterwerte, bevor Sie den folgenden Befehl ausführen:

   ```bash
   azdata bdc hdfs mount create --remote-uri abfs://<blob-container-name>@<storage-account-name>.dfs.core.windows.net/ --mount-path /mounts/<mount-name>
   ```

   > [!NOTE]
   > Der „mount create“-Befehl ist asynchron. Zurzeit gibt keine Meldung an, ob die Einbindung erfolgreich war. Weitere Informationen zum Überprüfen des Status Ihrer Einbindungen finden Sie im [Statusabschnitt](#status).

Wenn die Einbindung erfolgreich war, sollten Sie in der Lage sein, die HDFS-Daten abzufragen und Spark-Aufträge dafür auszuführen. Sie wird im HDFS für Ihren Big Data-Cluster an dem Speicherort angezeigt, der durch `--mount-path` angegeben wird.

## <a name="get-the-status-of-mounts"></a><a id="status"></a> Abrufen des Status von Einbindungen

Verwenden Sie den folgenden Befehl, um die Status aller Einbindungen in Ihrem Big Data-Cluster aufzulisten:

```bash
azdata bdc hdfs mount status
```

Verwenden Sie den folgenden Befehl, um den Status einer Einbindung in einem bestimmten Pfad im HDFS aufzulisten:

```bash
azdata bdc hdfs mount status --mount-path <mount-path-in-hdfs>
```

## <a name="refresh-a-mount"></a>Aktualisieren einer Einbindung

Im folgenden Beispiel wird die Einbindung aktualisiert. Durch diese Aktualisierung wird auch der Einbindungscache bereinigt.

```bash
azdata bdc hdfs mount refresh --mount-path <mount-path-in-hdfs>
```

## <a name="delete-the-mount"></a><a id="delete"></a> Löschen der Einbindung

Verwenden Sie zum Löschen den Einbindung den Befehl **azdata bdc hdfs mount delete**, und geben Sie den Einbindungspfad in HDFS an:

```bash
azdata bdc hdfs mount delete --mount-path <mount-path-in-hdfs>
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] finden Sie unter [Was sind [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).
