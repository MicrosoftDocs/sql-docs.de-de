---
title: Hinzufügen zusätzlicher Funktionen durch Erweiterbarkeit
description: Erfahren Sie mehr über das Erweiterbarkeitsmodell und die wichtigsten Erweiterbarkeitsbereiche zum Erweitern der Funktionalität von Azure Data Studio
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 881b913e3d36c98650b46445eeee7d2e99313a11
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048610"
---
# <a name="azure-data-studio-extensibility"></a>Erweiterbarkeit für Azure Data Studio

Azure Data Studio verfügt über mehrere Erweiterbarkeitsmechanismen, um die Benutzeroberfläche anzupassen und diese Anpassungen der gesamten Benutzercommunity zur Verfügung zu stellen. Die Kern der Azure Data Studio-Plattform basiert auf Visual Studio Code, sodass die meisten Erweiterbarkeits-APIs von Visual Studio Code verfügbar sind. Außerdem haben wir zusätzliche Erweiterbarkeitspunkte für Aktivitäten bereitgestellt, die für die Datenverwaltung spezifisch sind.

Zu den wichtigsten Erweiterbarkeitspunkten gehören:

- Visual Studio Code-Erweiterbarkeits-APIs
- Tools zur Erstellung von Erweiterungen für Azure Data Studio
- Verwalten von Beiträgen zum Dashboard-Registerkartenbereich
- Erkenntnisse mit Actions-Erfahrungen
- Erweiterbarkeits-APIs für Azure Data Studio
- Benutzerdefinierte Datenanbieter-APIs

## <a name="visual-studio-code-extensibility-apis"></a>Visual Studio Code-Erweiterbarkeits-APIs

Da die Kernplattform von Azure Data Studio auf Visual Studio Code basiert, finden Sie ausführliche Informationen zu den Erweiterbarkeits-APIs von Visual Studio Code in der Dokumentation zu [Erweiterungserstellung](https://code.visualstudio.com/docs/extensions/overview) und [Erweiterungs-APIs](https://code.visualstudio.com/docs/extensionAPI/overview) auf der Visual Studio Code-Website.

> [!NOTE]
>  Die Releases von Azure Data Studio sind an einer aktuellen Version von VS Code ausgerichtet, doch die enthaltene VS Code-Engine entspricht möglicherweise nicht dem aktuellen VS Code-Release. Beispielsweise entspricht im November 2020 die VS Code-Engine in Azure Data Studio 1.48, die aktuelle VS Code-Version ist jedoch 1.51.  Die Anzeige der Fehlermeldung „Die Erweiterung '<name>' kann nicht installiert werden, weil sie nicht mit VS Code <version> kompatibel ist." bei der Installation einer Erweiterung wird durch eine Erweiterung verursacht, die eine spätere Version der VS Code-Engine enthält als im Paketmanifest (`package.json`) definiert. Sie können die Version der VS Code-Engine in Ihrer Azure Data Studio-Instanz über das Menü **Hilfe** unter **Info** überprüfen.

## <a name="manage-dashboard-tab-panel-contributions"></a>Verwalten von Beiträgen zum Dashboard-Registerkartenbereich

Weitere Informationen finden Sie unter [Beitragspunkte](#contribution-points) und [Kontextvariablen](#context-variables).

## <a name="azure-data-studio-extensibility-apis"></a>Erweiterbarkeits-APIs für Azure Data Studio

Weitere Informationen finden Sie unter [Azure Data Studio-Erweiterbarkeits-APIs](extensibility-apis.md).


## <a name="contribution-points"></a>Beitragspunkte

In diesem Abschnitt werden die verschiedenen Beitragspunkte behandelt, die im package.json-Erweiterungsmanifest definiert sind.

IntelliSense wird in azuredatastudio unterstützt.

### <a name="dashboard-contribution-points"></a>Dashboardbeitragspunkte

Tragen Sie eine Registerkarte, einen Container und/oder ein Erkenntnis-Widget zum Dashboard bei.

![Dashboard](media/extensibility/dashboard-page.png)

`dashboard.tabs`

„Dashboard.tabs“ erstellt die Registerkartenabschnitte innerhalb der Dashboardseite. Es wird ein Objekt oder ein Array von Objekten erwartet.  

```json
"dashboard.tabs": [
{
    "id": "test-tab1",
    "title": "Test 1",
    "description": "The test 1 displays a list of widgets.",
    "when": "connectionProvider == 'MSSQL' && !mssql:iscloud",
    "alwaysShow": true,
    "container": {
        ...
    }
}
]
```

`dashboard.containers`

Anstatt den Dashboardcontainer inline anzugeben (innerhalb von dashboard.tab), können Sie Container mithilfe von „dashboard.containers“ registrieren. Es wird ein Objekt oder ein Array des Objekts akzeptiert.

```json
"dashboard.containers": [
{
    "id": "innerTab1",
    "container": {
        ...
    }
},
{
    "id": "innerTab2",
    "container": {
       ...
    }
}
]
```

Um auf registrierte Container zu verweisen, geben Sie die ID des Containers an.

```json
"dashboard.tabs": [
{
    ...
    "container": {
        "innerTab1": {             
        }
    }
}
]

```

`dashboard.insights`

Sie können Erkenntnisse mithilfe von „dashboard.insights“ registrieren. Dies ähnelt [Tutorial: Erstellen eines benutzerdefinierten Erkenntniswidgets](./tutorial-build-custom-insight-sql-server.md)

```json
"dashboard.insights": {
"id": "my-widget"
"type": {
    "count": {
        "dataDirection": "vertical",
        "dataType": "number",
        "legendPosition": "none",
           "labelFirstColumn": false,
        "columnsAsLabels": false
       }
   },
   "queryFile": "{your file folder}/activeSession.sql"
}
```


### <a name="dashboard-container-types"></a>Typen von Dashboardcontainern

Zurzeit werden vier Containertypen unterstützt:

1. `widgets-container`

    ![Widgetscontainer](media/extensibility/widgets-container.png)

    Die Liste der Widgets, die im Container angezeigt werden. Es ist ein Flusslayout. Es akzeptiert die Liste der Widgets.

    ```json
    "container": {
        "widgets-container": [
        {
            "widget": {
                "query-data-store-db-insight": {
                }
            }
        },
        {
            "widget": {
                "explorer-widget": {
                }
            }
        }
      ]
    }
    ```

2. `webview-container`

    ![WebView-Container](media/extensibility/webview-container.png)

    Die WebView wird im gesamten Container angezeigt. Es wird erwartet, dass die WebView-ID mit der Registerkarten-ID identisch ist.

    ```json
    "container": {
        "webview-container": null
    }
    ```

3. `grid-container`

   ![Rastercontainer](media/extensibility/grid-container.png)

   Die Liste der Widgets oder WebViews, die im Rasterlayout angezeigt werden.

    ```json
    "container": {
        "grid-container": [
        {
            "name": "widget 1",
            "widget": {
                "explorer-widget": {
                }
            },
            "row":0,
            "col":0
        },
        {
            "name": "widget 2",
            "widget": {
                "tasks-widget": {
                    "backup", 
                    "restore",
                    "configureDashboard",
                    "newQuery"
                }
            },
            "row":0,
            "col":1
        },
        {
            "name": "Webview 1",
            "webview": {
                "id": "google"
            },
            "row":1,
            "col":0,
            "colspan":2
        },
        {
            "name": "widget 3",
            "widget": {
                "explorer-widget": {}
            },
            "row":0,
            "col":3,
            "rowspan":2
        },
    ]
    ```

4.  `nav-section`

    ![Navigationsabschnitt](media/extensibility/nav-section.png)

    Der Navigationsabschnitt wird im Container angezeigt.

    ```json
    "container": {
        "nav-section": [
            {
                "id": "innerTab1",
                "title": "inner-tab1",
                "icon": {
                    "light": "./icons/tab1Icon.svg",
                    "dark": "./icons/tab1Icon_dark.svg"
                },
                "container": {
                    ...
                }
            },
            {
                "id": "innerTab2",
                "title": "inner-tab2",
                "icon": {
                    "light": "./icons/tab2Icon.svg",
                    "dark": "./icons/tab2Icon_dark.svg"
                },
                "container": {
                    ...
                }
            }
        ]
    }
    ```



## <a name="context-variables"></a>Kontextvariablen

Allgemeine Informationen zum Kontext in Visual Studio Code und anschließend Azure Data Studio finden Sie unter [Erweiterbarkeit](https://code.visualstudio.com/docs/extensionAPI/extension-points#_example).

In Azure Data Studio gibt es einen spezifischen Kontext um Datenbankverbindungen, die für Erweiterungen verfügbar sind.

### <a name="dashboard"></a>Dashboard

Im Dashboard stellen wir die folgenden Kontextvariablen bereit:

|Kontextvariable| description|
|:---|:---|
|`connectionProvider` | Eine Zeichenfolge des Bezeichners für den Anbieter der aktuellen Verbindung. Ex. `connectionProvider == 'MSSQL'`.|
|`serverName`|Eine Zeichenfolge des Servernamens der aktuellen Verbindung. Ex. `serverName == 'localhost'`.|
|`databaseName` | Eine Zeichenfolge des Datenbanknamens der aktuellen Verbindung. Ex. `databaseName == 'master'`.|
|`connection` | Das vollständige Verbindungsprofilobjekt für die aktuelle Verbindung (IConnectionProfile)|
|`dashboardContext` | Eine Zeichenfolge des Kontexts der Seite des Dashboards ist derzeit aktiv. Entweder „database“ oder „server“. Ex. `dashboardContext == 'database'`|