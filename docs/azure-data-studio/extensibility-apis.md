---
title: Erweiterbarkeits-APIs
description: Hier erfahren Sie mehr über die Erweiterbarkeits-APIs von Azure Data Studio, die die Interaktion von Erweiterungen mit anderen Komponenten von Azure Data Studio (z. B. dem Objekt-Explorer) ermöglichen.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: d795d96d47fa7f07744c492d8639cdecdc2e23fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052156"
---
# <a name="azure-data-studio-extensibility-apis"></a>Erweiterbarkeits-APIs für Azure Data Studio

Azure Data Studio bietet eine API, mit der Erweiterungen mit anderen Teilen von Azure Data Studio interagieren können, z. B. dem Objekt-Explorer. Diese APIs sind in der [`src/sql/azdata.d.ts`](https://github.com/Microsoft/azuredatastudio/blob/main/src/sql/azdata.d.ts)-Datei verfügbar und werden nachfolgend beschrieben.

## <a name="connection-management"></a>Verbindungsverwaltung
`azdata.connection`

### <a name="top-level-functions"></a>Funktionen der obersten Ebene

- `getCurrentConnection(): Thenable<azdata.connection.Connection>` Ruft die aktuelle Verbindung auf Grundlage des aktiven Editors oder der Objekt-Explorer-Auswahl ab.

- `getActiveConnections(): Thenable<azdata.connection.Connection[]>` Ruft eine Liste aller aktiven Verbindungen des Benutzers ab. Gibt eine leere Liste zurück, wenn keine solchen Verbindungen vorhanden sind.

- `getCredentials(connectionId: string): Thenable<{ [name: string]: string }>` Ruft ein Wörterbuch ab, das die einer Verbindung zugeordneten Anmeldeinformationen enthält. Diese würden andernfalls als Teil des Optionswörterbuchs unter einem `azdata.connection.Connection`-Objekt zurückgegeben, aber aus diesem Objekt entfernt werden. 

### `Connection`
- `options: { [name: string]: string }` Das Wörterbuch der Verbindungsoptionen
- `providerName: string` Der Name des Verbindungsanbieters (z.B. „MSSQL“)
- `connectionId: string` Der eindeutige Bezeichner für die Verbindung

### <a name="example-code"></a>Beispielcode
```
> let connection = azdata.connection.getCurrentConnection();
connection: {
    providerName: 'MSSQL',
    connectionId: 'd97bb63a-466e-4ef0-ab6f-00cd44721dcc',
    options: {
        server: 'mairvine-sql-server',
        user: 'sa',
        authenticationType: 'sqlLogin',
        ...
    },
    ...
}
> let credentials = azdata.connection.getCredentials(connection.connectionId);
credentials: {
    password: 'abc123'
}

```

## <a name="object-explorer"></a>Objekt-Explorer

`azdata.objectexplorer`


### <a name="top-level-functions"></a>Funktionen der obersten Ebene
- `getNode(connectionId: string, nodePath?: string): Thenable<azdata.objectexplorer.ObjectExplorerNode>` Abrufen eines Objekt-Explorer-Knotens, der der angegebenen Verbindung und dem angegebenen Pfad entspricht. Wenn kein Pfad angegeben wird, wird der Knoten der obersten Ebene für die angegebene Verbindung zurückgegeben. Wenn im angegebenen Pfad kein Knoten vorhanden ist, wird `undefined` zurückgegeben. Hinweis: Der `nodePath` für ein Objekt wird vom SQL Tools Service-Back-End generiert und ist schwer manuell zu konstruieren. Zukünftige API-Verbesserungen werden Ihnen ermöglichen, Knoten basierend auf Metadaten abzurufen, die Sie über den Knoten angeben, z.B. Name, Typ und Schema.

- `getActiveConnectionNodes(): Thenable<azdata.objectexplorer.ObjectExplorerNode>` Abrufen aller aktiven Objekt-Explorer-Verbindungsknoten.

- `findNodes(connectionId: string, type: string, schema: string, name: string, database: string, parentObjectNames: string[]): Thenable<azdata.objectexplorer.ObjectExplorerNode[]>` Suchen aller Objekt-Explorer-Knoten, die den angegebenen Metadaten entsprechen. Die Argumente `schema`, `database` und `parentObjectNames` sollten `undefined` sein, wenn sie nicht anwendbar sind. `parentObjectNames` ist eine Liste im Objekt-Explorer von übergeordneten Nicht-Datenbank-Objekten, unter denen sich das gewünschte Objekt befindet, von der höchsten bis zur niedrigsten Ebene. Wenn Sie z.B. nach der Spalte „column1“ suchen, die zur Tabelle „schema1.table1“ und der Datenbank „database1“ mit Verbindungs-ID `connectionId` gehört, rufen Sie `findNodes(connectionId, 'Column', undefined, 'column1', 'database1', ['schema1.table1'])` auf. Beachten Sie auch die [Liste der Typen, die Azure Data Studio standardmäßig für diesen API-Aufruf unterstützt](https://github.com/Microsoft/azuredatastudio/wiki/Object-Explorer-types-supported-by-FindNodes-API).

### <a name="objectexplorernode"></a>ObjectExplorerNode
- `connectionId: string` Die ID der Verbindung, unter der der Knoten vorhanden ist.

- `nodePath: string` Der Pfad des Knotens, der für einen Aufruf der `getNode`-Funktion verwendet wird.

- `nodeType: string` Eine Zeichenfolge, die den Typ des Knotens darstellt.

- `nodeSubType: string` Eine Zeichenfolge, die den Untertyp des Knotens darstellt.

- `nodeStatus: string` Eine Zeichenfolge, die den Status des Knotens darstellt.

- `label: string` Die Bezeichnung für den Knoten, die im Objekt-Explorer angezeigt wird.

- `isLeaf: boolean` Gibt an, ob der Knoten ein Blattknoten ist und daher keine untergeordneten Elemente aufweist.

- `metadata: azdata.ObjectMetadata` Metadaten, die das von diesem Knoten dargestellte Objekt beschreiben.

- `errorMessage: string` Meldung, die angezeigt wird, wenn sich der Knoten in einem Fehlerzustand befindet.

- `isExpanded(): Thenable<boolean>` Gibt an, ob der Knoten derzeit im Objekt-Explorer erweitert ist.

- `setExpandedState(expandedState: vscode.TreeItemCollapsibleState): Thenable<void>` Festlegen, ob der Knoten erweitert oder reduziert wird. Wenn der Status auf „None“ (Keine) festgelegt ist, wird der Knoten nicht geändert.

- `setSelected(selected: boolean, clearOtherSelections?: boolean): Thenable<void>` Festlegen, ob der Knoten ausgewählt ist. Wenn `clearOtherSelections` „true“ ist, löschen Sie jede andere Auswahl, wenn Sie die neue Auswahl treffen. Wenn der Wert „false“ ist, behalten Sie jede vorhandene Auswahl bei. `clearOtherSelections` hat den Standardwert „true“, wenn `selected` „true“ ist, und „false“, wenn `selected` „false“ ist.

- `getChildren(): Thenable<azdata.objectexplorer.ObjectExplorerNode[]>` Abrufen aller untergeordneten Knoten dieses Knotens. Gibt eine leere Liste zurück, wenn keine untergeordneten Knoten vorhanden sind.

- `getParent(): Thenable<azdata.objectexplorer.ObjectExplorerNode>` Abrufen des übergeordneten Knotens dieses Knotens. Gibt „undefined“ (undefiniert) zurück, wenn kein übergeordneter Knoten vorhanden ist.

### <a name="example-code"></a>Beispielcode

```cs
private async interactWithOENode(selectedNode: azdata.objectexplorer.ObjectExplorerNode): Promise<void> {
    let choices = ['Expand', 'Collapse', 'Select', 'Select (multi)', 'Deselect', 'Deselect (multi)'];
    if (selectedNode.isLeaf) {
        choices[0] += ' (is leaf)';
        choices[1] += ' (is leaf)';
    } else {
        let expanded = await selectedNode.isExpanded();
        if (expanded) {
            choices[0] += ' (is expanded)';
        } else {
            choices[1] += ' (is collapsed)';
        }
    }
    let parent = await selectedNode.getParent();
    if (parent) {
        choices.push('Get Parent');
    }
    let children = await selectedNode.getChildren();
    children.forEach(child => choices.push(child.label));
    let choice = await vscode.window.showQuickPick(choices);
    let nextNode: azdata.objectexplorer.ObjectExplorerNode = undefined;
    if (choice === choices[0]) {
        selectedNode.setExpandedState(vscode.TreeItemCollapsibleState.Expanded);
    } else if (choice === choices[1]) {
        selectedNode.setExpandedState(vscode.TreeItemCollapsibleState.Collapsed);
    } else if (choice === choices[2]) {
        selectedNode.setSelected(true);
    } else if (choice === choices[3]) {
        selectedNode.setSelected(true, false);
    } else if (choice === choices[4]) {
        selectedNode.setSelected(false);
    } else if (choice === choices[5]) {
        selectedNode.setSelected(false, true);
    } else if (choice === 'Get Parent') {
        nextNode = parent;
    } else {
        let childNode = children.find(child => child.label === choice);
        nextNode = childNode;
    }
    if (nextNode) {
        let updatedNode = await azdata.objectexplorer.getNode(nextNode.connectionId, nextNode.nodePath);
        this.interactWithOENode(updatedNode);
    }
}

vscode.commands.registerCommand('mssql.objectexplorer.interact', () => {
    azdata.objectexplorer.getActiveConnectionNodes().then(activeConnections => {
        vscode.window.showQuickPick(activeConnections.map(connection => connection.label + ' ' + connection.connectionId)).then(selection => {
            let selectedNode = activeConnections.find(connection => connection.label + ' ' + connection.connectionId === selection);
            this.interactWithOENode(selectedNode);
        });
    });
});
```

## <a name="proposed-apis"></a>Vorgeschlagene APIs

Wir haben vorgeschlagene APIs hinzugefügt, um Erweiterungen neben anderen Funktionen die Anzeige benutzerdefinierter Benutzeroberflächen in Dialogfeldern, Assistenten und Dokumentregisterkarten zu ermöglichen. Weitere Dokumentation finden Sie in der [Datei der vorgeschlagenen API-Typen](https://github.com/Microsoft/azuredatastudio/blob/main/src/sql/azdata.proposed.d.ts). Beachten Sie jedoch, dass diese APIs jederzeit geändert werden können. Beispiele für die Verwendung einiger dieser APIs finden Sie in der [Beispielerweiterung „sqlservices“](https://github.com/Microsoft/azuredatastudio/tree/main/samples/sqlservices).


