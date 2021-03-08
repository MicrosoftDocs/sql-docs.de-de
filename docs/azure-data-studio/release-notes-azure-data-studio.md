---
title: Azure Data Studio-Versionshinweise
description: Dieser Artikel enthält Versionshinweise für Azure Data Studio-Releases von November 2017 bis heute. Für viele der zusammengefassten Issues werden Links zu weiteren Informationen bereitgestellt.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan
ms.custom: seodec18
ms.date: 2/18/2021
ms.openlocfilehash: c666ae644200c1b2beefb7f73c8a819294831985
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836027"
---
# <a name="release-notes-for-azure-data-studio"></a>Versionshinweise für Azure Data Studio

**[Neuestes Release herunterladen und installieren](./download-azure-data-studio.md)**

## <a name="february-2021"></a>Februar 2021

18. Februar 2021 &nbsp; / &nbsp; Version: 1.26.0

&nbsp;

| Change | Details |
| ------ | ------- |
| Fehlerbehebungen | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22February+2021+Release%22+is%3Aclosed). |
| Update für Erweiterung(en) | [Dacpac](extensions/sql-server-dacpac-extension.md) <br/> [Kusto (KQL)](extensions/kusto-extension.md) </br> [MachineLearning](extensions/machine-learning-extension.md) </br> [Profiler](extensions/sql-server-profiler-extension.md) </br> [SchemaCompare](extensions/schema-compare-extension.md) </br> [SQLDatabaseProjects](extensions/sql-database-project-extension.md) |
| Neue Azure Arc-Features | Mehrere Datencontroller werden jetzt unterstützt <br/> Neue Verbindungsdialogoptionen wie Kube-Konfigurationsdatei <br/> Verbesserungen am Postgres-Dashboard |
| Neue Notebookfeatures | Um 50 % verbesserte Startzeit des Jupyter-Servers unter Windows <br/> Unterstützung der Bearbeitung von Jupyter Books durch Klicken mit der rechten Maustaste hinzugefügt <br/> Unterstützung für die Parametrisierung von URI-Notebooks und [Dokumentation zur Parametrisierung von Notebooks hinzugefügt](https://docs.microsoft.com/sql/azure-data-studio/notebooks/notebooks-parameterization) |

## <a name="december-2020-hotfix"></a>Dezember 2020 (Hotfix)

10. Februar 2021 &nbsp; / &nbsp; Version: 1.25.3

| Change | Details |
| ------ | ------- |
| Behebung des Fehlers [#13899](https://github.com/microsoft/azuredatastudio/issues/13899) | Scrollen zu den entsprechenden Querverweislinks in Notebooks |
| Upgraden von Electron zum Einschließen wichtiger Fehlerbehebungen| – |

## <a name="december-2020"></a>Dezember 2020

9\. Dezember 2020 &nbsp; / &nbsp; Version: 1.25.0

&nbsp;

| Change | Details |
| ------ | ------- |
| Fehlerbehebungen | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22December+2020+Release%22). |
| Update für die Erweiterung „Database Projects“ | Arbeitsbereiche hinzugefügt und Seitenleiste verbessert |

## <a name="november-2020"></a>November 2020

12. November 2020 &nbsp; / &nbsp; Version: 1.24.0

&nbsp;

| Change | Details |
| ------ | ------- |
| Fehlerbehebungen | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22November+2020+Release%22+is%3Aclosed). |
| Verbindungsdialogfeld | Neue Suchregisterkarte zum Verbindungsdialogfeld hinzugefügt. |
| Update für Erweiterung(en) | Update für Postgres-Erweiterung veröffentlicht. |
| Neue Notebookfeatures | Neue Features für SQL zur Notebookunterstützung hinzugefügt. <br/> Neue Features zur Unterstützung für Notebookparameter hinzugefügt. <br/>  Neue Features zum Ergebnisstreaming für SQL-Notebooks hinzugefügt. |
| Python-Installation | PROSE-Paket wurde aus der Python-Standardinstallation entfernt. |

### <a name="known-issues-1240"></a>Bekannte Probleme (1.24.0)

| Neues Element | Details | Problemumgehung |
|----------|---------|------------|
| Azure Arc-Erweiterung | [Bekanntes Problem:](https://github.com/microsoft/azuredatastudio/issues/13319) Die Schaltfläche „Skript in Notebook schreiben“ für Arc MIAA & PG-Bereitstellungen führt vor der Skripterstellung für das Notebook keine Feldüberprüfung aus. Das bedeutet Folgendes: Wenn ein Benutzer ein falsches Kennwort in das Kennwortbestätigungsfeld eingibt, entsteht möglicherweise ein Notebook mit einem falschen Kennwortwert.| Die Schaltfläche „Bereitstellen“ funktioniert erwartungsgemäß. Daher sollten Benutzer diese Schaltfläche verwenden. |
| Objekt-Explorer | ADS-Releases vor 1.24.0 weisen aufgrund von Änderungen der Engine im Zusammenhang mit [serverlosen SQL-Pools bei Azure Synapse Analytics](/azure/synapse-analytics/sql/on-demand-workspace-overview) einen Breaking Change im Objekt-Explorer auf. | Um den Objekt-Explorer in Azure Data Studio weiterhin mit serverlosen SQL-Pools in Azure Synapse Analytics verwenden zu können, müssen Sie Azure Data Studio 1.24.0 oder höher verwenden. |

Unter [Azure Data Studio-Feedback](https://github.com/microsoft/azuredatastudio) können Sie Lösungen für andere bekannte Probleme finden und Feedback an das Produktteam übermitteln.

## <a name="october-2020"></a>Oktober 2020

14. Oktober 2020 &nbsp; / &nbsp;, Version: 1.23.0

&nbsp;

| Change | Details |
| ------ | ------- |
| Azure SQL Edge | Unterstützung für Azure SQL Edge-Objekte. |
| Fehlerbehebungen | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is:issue+milestone:%22October+2020+Release%22+is:closed). |
| Datenbanken| Unterstützung für Verweise auf die gleiche Datenbank. |
| Updates für Erweiterungen | [Azure Arc](extensions/azure-arc-extension.md)</br>[azdata](../azdata/install/deploy-install-azdata.md)</br>[Machine Learning](extensions/machine-learning-extension.md)</br>[Kusto (KQL)](extensions/kusto-extension.md)</br>[Schemavergleich](extensions/schema-compare-extension.md)</br>SQL Assessment</br>[SQL-Datenbankprojekte](extensions/sql-database-project-extension.md)</br>[SQL Server-Import](extensions/sql-server-import-extension.md) |
| Neue Bereitstellungsfunktionen | Azure SQL-Datenbank und VM-Bereitstellungen wurden hinzugefügt. |
| PowerShell | Unterstützung für das Streaming von PowerShell-Kernelergebnissen wurde hinzugefügt. |

## <a name="september-2020-hotfix"></a>September 2020 (Hotfix)

30. September 2020 &nbsp; / &nbsp; Version: 1.22.1

&nbsp;

| Change | Details |
| ------ | ------- |
| Fehler und Issues wurden behoben. | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/releases/tag/1.22.1). |

## <a name="september-2020"></a>September 2020

22. September 2020 &nbsp; / &nbsp; Version: 1.22.0

&nbsp;

| Change | Details |
| ------ | ------- |
| Neue Notebookfeatures | <br/> &bull; &nbsp; Unterstützung für brandneue Bearbeitungsfeatures für Textzellen, die auf Rich-Text-Formatierung und einer nahtlosen Umwandlung in Markdown basieren. Diese werden auch als What-You-See-Is-What-You-Get-Symbolleiste bezeichnet. <br/> &bull; &nbsp; Unterstützung für den Kusto-Kernel <br/> &bull; &nbsp; Unterstützung für das Anheften von Notebooks <br/> &bull; &nbsp; Unterstützung für eine neue Version von Jupyter Book-Instanzen <br/> &bull; &nbsp; Verbesserte Jupyter-Tastenkombinationen <br/> &bull; &nbsp; Verbesserungen für das Laden von perf |
| SQL Database Projects-Erweiterung | Die Erweiterung „SQL Database Projects“ (SQL-Datenbank-Projekte) führt die projektbasierte Datenbankentwicklung in Azure Data Studio ein. In diesem Vorschaurelease können SQL-Projekte erstellt und über Azure Data Studio veröffentlicht werden. |
| Kusto-Erweiterung (KQL) | Macht native Kusto-Features zum Durchsuchen von Daten und zur Analyse einer riesigen Menge von Echtzeitstreamingdaten, die in Azure Data Explorer gespeichert sind, in Azure Data Studio verfügbar. Dieses Vorschaurelease unterstützt das Verbinden und Durchsuchen von Azure Data Explorer-Clustern, das Schreiben von KQL-Abfragen sowie das Erstellen von Notebooks mit dem Kusto-Kernel. |
| Azure Arc-Erweiterung | Benutzer können die Public Preview von Azure Arc über Azure Data Studio ausprobieren. Dies schließt Folgendes ein: <br/> &bull; &nbsp; Bereitstellen eines Datencontrollers <br/> &bull; &nbsp; Bereitstellen von Postgres <br/> &bull; &nbsp; Bereitstellen von Managed Instance für Azure Arc <br/> &bull; &nbsp; Verbinden mit dem Datencontroller <br/> &bull; &nbsp; Zugreifen auf Dashboards von Datendiensten <br/> &bull; &nbsp; Jupyter Book für Azure Arc |
| Bereitstellungsoptionen | <br/> &bull; &nbsp; Azure SQL Database Edge <br/> (Edge erfordert eine Azure SQL Edge-Bereitstellungserweiterung) |
| Allgemeine Verfügbarkeit der Erweiterung „SQL Server Import“ | Ankündigung der allgemeinen Verfügbarkeit der Erweiterung „SQL Server Import“, Features befinden sich nicht mehr in der Vorschau. Diese Erweiterung vereinfacht das Importieren von CSV-/TXT-Dateien. Weitere Informationen zur Erweiterung finden Sie [in diesem Artikel](./extensions/sql-server-import-extension.md). |
| Fehler und Issues wurden behoben. | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22September+2020+Release%22+is%3Aclosed). |

## <a name="august-2020"></a>August 2020

12. August 2020 &nbsp; / &nbsp; Version: 1.21.0

&nbsp;

| Change | Details |
| :----- | :------ |
| Neue Notebookfeatures | &bull; &nbsp; Zellenspeicherorte verschieben <br/> &bull; &nbsp; Zellen in Textzellen oder Codezellen konvertieren
| Auswahl von Jupyter Books | Benutzer können jetzt Jupyter Book-Instanzen aus GitHub-Releases auswählen und nahtlos in Azure Data Studio öffnen. |
| Suche zum Notebooks-Viewlet hinzugefügt | Benutzer können ganz einfach in ihren Notebooks und Jupyter Books nach Inhalten suchen. |
| Fehler und Issues wurden behoben. | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22August+2020+Release%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="july-2020-hotfix"></a>Juli 2020 (Hotfix)

17. Juli 2020 &nbsp; / &nbsp; Version: 1.20.1

&nbsp;

| Change | Details |
| :----- | :------ |
| Korrektur von Fehler #11372 Objekt-Explorer Drag & Drop-Tabelle führt fälschlicherweise Umbrüche von Tabellennamen durch. | [#11372](https://github.com/microsoft/azuredatastudio/issues/11372) |
| Korrektur von Fehler #11356 Dunkles Design ist nun das Standarddesign. | [#11356](https://github.com/microsoft/azuredatastudio/issues/11356) |
| &nbsp; | &nbsp; |

### <a name="known-issue"></a>Bekanntes Problem

- Einige Benutzer haben Verbindungsfehler von dem neuen Microsoft.DataData.SqlClient v2.0.0 gemeldet, der in diesem Release enthalten ist. Benutzer haben festgestellt, dass [die Befolgung dieser Anweisungen](https://github.com/microsoft/azuredatastudio/issues/11367#issuecomment-659614111) zu einer erfolgreichen Verbindung führt.

## <a name="july-2020"></a>Juli 2020

15. Juli 2020 &nbsp; / &nbsp; Version: 1.20.0

&nbsp;

| Change | Details |
| :----- | :------ |
| Neue Featuretour hinzugefügt | Über die Homepage und die Befehlspalette können Benutzer jetzt eine Featuretour starten, um eine exemplarische Vorgehensweise für häufig verwendete Features wie die Viewlets „Verbindungen“ und „Notebooks“ oder den Marketplace für Erweiterungen zu erhalten. |
| Neue Notebookfeatures | &bull; &nbsp; Headerunterstützung in der Markdown-Symbolleiste<br/> &bull; &nbsp; Parallele Markdownvorschau in Textzellen
| Ziehen und Ablegen von Spalten und Tabellen im Abfrage-Editor | Benutzer können jetzt Spalten und Tabellen per Drag & Drop direkt aus dem Viewlet „Verbindungen“ in den Abfrage-Editor ziehen. |
| Azure-Kontosymbol wurde zur Aktivitätsleiste hinzugefügt | Benutzer können jetzt leicht erkennen, wo sie sich bei Azure anmelden können. |
| Fehler und Issues wurden behoben. | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22July+2020+Release%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="june-2020"></a>Juni 2020

15. Juni 2020 &nbsp; / &nbsp; Version: 1.19.0

&nbsp;

| Change | Details |
| :----- | :------ |
| Hinzufügen von Azure Data Studio zur Azure-Portal-Integration | Benutzer können das Azure-Portal nun direkt über eine Azure SQL-Datenbank-Verbindung, über Azure Postgres und mehr aufrufen. |
| Neue Notebookfeatures | &bull; &nbsp; Neue Notebook-Symbolleiste <br/> &bull; &nbsp; Neue Symbolleiste „Edit Cell“ (Zelle bearbeiten) <br/> &bull; &nbsp; Updates für die Benutzeroberfläche des Assistenten für Python-Abhängigkeiten <br/> &bull; &nbsp; Notebookübergreifende Verbesserung der Abstände |
| Ankündigung API-Erweiterung für die SQL-Bewertung | Diese Erweiterung fügt SQL Server-Bewertungen im Einklang mit Best Practices zu ADS hinzu. Sie stellt die zuvor nur für die Verwendung im PowerShell-Modul SqlServer und in SMO verfügbare API für die SQL-Bewertung bereit, damit Sie Ihre SQL Server-Instanzen bewerten und vom SQL Server-Team Empfehlungen für sie erhalten können. Mehr über die API für die SQL-Bewertung und deren Funktionen erfahren Sie [in diesem Artikel](../tools/sql-assessment-api/sql-assessment-api-overview.md). |
| [Verbesserungen an der Machine-Learning-Erweiterung](./extensions/machine-learning-extension.md) | Unterstützt jetzt Azure SQL Managed Instance |
| Verbesserungen an der Datenvirtualisierungserweiterung | Unterstützt jetzt MongoDB und Teradata |
| Fehlerbehebungen für die Postgres-Erweiterung | Behebung von Problemen mit Azure MFA |
| Fehler und Issues wurden behoben. | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22June+2020+Release%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="may-2020-hotfix"></a>Mai 2020 (Hotfix)

27. Mai 2020 &nbsp; / &nbsp; Version: 1.18.1

&nbsp;

| Change | Details |
| :----- | :------ |
| Behebung des Fehlers 10538: „Run Current Query“ keybind no longer behaving as expected (Verhalten bei Tastenzuordnung für „Aktuelle Abfrage ausführen“ nicht mehr wie erwartet) | [10538](https://github.com/microsoft/azuredatastudio/issues/10538)  |
| Behebung des Fehlers 10537: Unable to open new or existing sql files on v1.18 (Öffnen von neuen oder vorhandenen SQL-Dateien in Version 1.18 nicht möglich) | [10537](https://github.com/microsoft/azuredatastudio/issues/10537)  |
| &nbsp; | &nbsp; |

## <a name="may-2020"></a>Mai 2020

20. Mai 2020 &nbsp; / &nbsp; Version: 1.18.0

&nbsp;

| Change | Details |
| :----- | :------ |
| Ankündigung der Redgate SQL Prompt-Erweiterung | Mit dieser Erweiterung können Sie Formatierungsstile direkt in Azure Data Studio verwalten. So können Sie Ihre Stile erstellen und bearbeiten, ohne die IDE zu verlassen. |
| Ankündigung der Machine-Learning-Erweiterung | Diese Erweiterung ermöglicht Ihnen Folgendes: <br/> &bull; &nbsp; Verwalten von Python- und R-Paketen mit SQL Server Machine Learning Services in Azure Data Studio<br/> &bull; &nbsp; Verwenden eines ONNX-Modells für Vorhersagen in Azure SQL Edge<br/> &bull; &nbsp; Anzeigen von ONNX-Modellen in einer Azure SQL Edge-Datenbank <br/> &bull; &nbsp; Importieren von ONNX-Modellen aus einer Datei oder Azure Machine Learning in eine Azure SQL Edge-Datenbank <br/> &bull; &nbsp; Erstellen eines Notebooks zum Ausführen von Experimenten |
| Neue Notebookfeatures | &bull; &nbsp; Hinzufügung eines neuen Assistenten für Python-Abhängigkeiten, um die Installation von Python-Abhängigkeiten zu vereinfachen <br/> &bull; &nbsp; Hinzufügung von Unterstreichungsunterstützung für die Markdown-Symbolleiste |
| Parametrisierung für Always Encrypted | Ermöglicht das Ausführen von Abfragen, die verschlüsselte Datenbankspalten einfügen, aktualisieren oder danach filtern|
| Fehler und Issues wurden behoben. | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22May+2020+Release%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="april-2020-hotfix"></a>April 2020 (Hotfix)

30. April 2020 &nbsp; / &nbsp; Version: 1.17.1

&nbsp;

| Change | Details |
| :----- | :------ |
| Fehlerbehebung #10197 Verbindung per MFA kann nicht hergestellt werden | [#10197](https://github.com/microsoft/azuredatastudio/issues/10197)  |
| &nbsp; | &nbsp; |

## <a name="april-2020"></a>April 2020

27. April 2020 &nbsp; / &nbsp; Version: 1.17.0

&nbsp;

| Change | Details |
| :----- | :------ |
| Verbesserte Startseite | Durch ein Update der Benutzeroberfläche ist es einfacher, häufig verwendete Aktionen zu finden, und Erweiterungen wurden hervorgehoben. |
| Neue Notebookfeatures | &bull; &nbsp; Eine neue Markdown-Symbolleiste wurde beim Bearbeiten von Textzellen hinzugefügt, um beim Schreiben von Markdown zu helfen. <br/> &bull; &nbsp; Das Jupyter-Books-Viewlet wurde in ein Notebooks-Viewlet umgestalten, über das Sie Jupyter-Books und Notebooks gemeinsam verwalten können. <br/>&bull; &nbsp; Die Unterstützung für das Speichern von Diagrammen beim Speichern eines Notebooks wurde hinzugefügt. <br/> &bull; &nbsp; Die Unterstützung für KQL Magic in Python-Notebooks wurde hinzugefügt.|
| Verbesserte Dashboards | Die Dashboards in Azure Data Studio wurden mit den neuesten Entwurfsmustern und einer Aktionssymbolleiste aktualisiert. Dies gilt auch für viele Erweiterungen. |
| Die Cloud Shell-Integration wurde zur Azure-Ansicht hinzugefügt. | |
| Unterstützung für Always Encrypted und Always Encrypted mit Secure Enclaves | |
| Fehler und Issues wurden behoben. | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22April+2020+Release%22). |
| &nbsp; | &nbsp; |
| Fehler und Issues wurden behoben. | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22April+2020+Release%22). |
| &nbsp; | &nbsp; |

## <a name="march-2020"></a>März 2020

18. März 2020 &nbsp; / &nbsp; Version: 1.16.0 

&nbsp;

| Change | Details |
| :----- | :------ |
| Erweiterte Diagrammunterstützung auf SQL Server-Notebooks | Benutzer können bei der Ausführung einer SQL-Abfrage in einer Codezelle Diagramme erstellen und speichern. |
| Weitere Funktionen für das Erstellen von Jupyter-Books | Benutzer können nun mithilfe eines Notebooks eigene Jupyter-Books erstellen. |
| Azure AD-Unterstützung für Postgres-Erweiterung hinzugefügt | |
| Es wurden viele Zugriffsfehler behoben | [Liste der Zugriffsfehler](https://github.com/microsoft/azuredatastudio/issues?page=1&q=is%3Aissue+is%3Aclosed+milestone%3A%22S360+-+Accessibility%22+label%3AA11y_AzureDataStudio) |
| Merge von VS Code zu 1.42 | Dieses Release enthält Updates für VS Code aus den drei vorherigen VS Code-Releases. Weitere Informationen finden Sie in den [Versionshinweisen](https://code.visualstudio.com/updates/v1_42). |
| Fehler und Issues wurden behoben. | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22March+2020%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="february-hotfix"></a>Februar (Hotfix)

19. Februar 2020 &nbsp; / &nbsp; Version: 1.15.1

&nbsp;

| Change | Details |
| :----- | :------ |
| Fehlerbehebung #9149 „Aktive Verbindungen anzeigen“ | [Issue #9149](https://github.com/microsoft/azuredatastudio/issues/9149)  |
| Fehlerbehebung #9061 Beim Bearbeiten des Datenrasters wird die Größe nicht ordnungsgemäß angepasst, wenn der SQL-Bereich angezeigt oder ausgeblendet wird | [Issue #9061](https://github.com/microsoft/azuredatastudio/issues/9061)  |
| &nbsp; | &nbsp; |

## <a name="february-2020"></a>Februar 2020

13. Februar 2020 &nbsp; / &nbsp; Version: 1.15.0

&nbsp;

| Change | Details |
| :----- | :------ |
| Neue Verbesserung der Azure-Anmeldung | Eine Funktion wurde zur Azure-Anmeldung hinzugefügt, einschließlich Entfernung des Kopier-/Einfügevorgangs von Gerätecode für eine nahtlosere Funktion. |
| Suche in Notebooks | Benutzer können STRG+F innerhalb eines Notebooks verwenden. Die Suche in der Notebook-Unterstützung durchsucht alle Zeilen in Code- und Textzellen. |
| VS Code-Zusammenführung der Versionen von 1.38 bis 1.42 | Dieses Release enthält Updates für VS Code aus den drei vorherigen VS Code-Releases. Weitere Informationen finden Sie in den [Versionshinweisen](https://code.visualstudio.com/updates/v1_42). |
| Fehlerbehebung für das oft gemeldete Problem mit [„leeren Anzeigen“](https://github.com/microsoft/azuredatastudio/issues/8775). | |
| Fehler und Issues wurden behoben. | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22February+2020%22). |
| &nbsp; | &nbsp; |

### <a name="known-issue"></a>Bekanntes Problem

- Benutzer unter macOS Catalina müssen mit der rechten Maustaste auf Azure Data Studio klicken, und anschließend „Öffnen“ auswählen.

## <a name="december-2019-hotfix"></a>Dezember 2019 (Hotfix)

26. Dezember 2019 &nbsp; / &nbsp; Version: 1.14.1

&nbsp;

| Change | Details |
| :----- | :------ |
| Fehler #8747 „OE Expansion fails“ (Fehler bei OE-Erweiterung.) wurde behoben. | [#8747](https://github.com/microsoft/azuredatastudio/issues/8747)  |
| &nbsp; | &nbsp; |

## <a name="december-2019"></a>Dezember 2019

19. Dezember 2019 &nbsp; / &nbsp; Version: 1.14.0 

&nbsp;

| Change | Details |
| :----- | :------ |
| Das Dropdownmenü zum Anfügen an eine Verbindung in Notebooks wurde geändert, sodass nur die derzeit aktive Verbindung angezeigt wird. | [#8129](https://github.com/microsoft/azuredatastudio/issues/8129) |
| Die Einstellung „bigdatacluster.ignoreSslVerification“ wurde hinzugefügt, sodass TLS/SSL-Überprüfungsfehler bei der Verbindung mit einem BDC-Dienst ignoriert werden können. | [#8582](https://github.com/microsoft/azuredatastudio/pull/8582) |
| Das Ändern der Standardsprachvariante für Offlineabfrage-Editoren ist nun zulässig. | [#8419](https://github.com/microsoft/azuredatastudio/pull/8419) |
| Big Data Cluster-/SQL 2019-Features sind allgemein verfügbar. | [#8269](https://github.com/microsoft/azuredatastudio/issues/8269) |
| Fehler und Issues wurden behoben. | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/milestone/44?closed=1). |
| &nbsp; | &nbsp; |


## <a name="november-2019-hotfix"></a>November 2019 (Hotfix)

15. November 2019 &nbsp; / &nbsp; Version: 1.13.1

&nbsp;

| Change | Details |
| :----- | :------ |
| Fehler #8210 Ergebnisse nach Kopieren/Einfügen nicht in richtiger Reihenfolge |  |
| &nbsp; | &nbsp; |

## <a name="november-2019"></a>November 2019

4\. November 2019 &nbsp; / &nbsp; Version: 1.13.0 

&nbsp;

| Change | Details |
| :----- | :------ |
| Neue Features für SQL Server 2019 werden unterstützt. | &bull; &nbsp; Bereitstellen des Big Data-Clusters für SQL Server 2019 mit dem BDC-Bereitstellungs-Assistenten <br/>&bull; &nbsp; Verwalten der Clusterintegrität mit dem Controllerdashboard <br/>&bull; &nbsp; Verwalten von HDFS-Zugriffssteuerungslisten mit dem Dialogfeld „Security ACLs“ (Sicherheits-ACLs) <br/> &bull; &nbsp; Hinzufügen von Bereitstellungen mit dem Dialogfeld „HDFS Tiering“ (HDFS-Tiering) <br/> &bull; &nbsp; Problembehandlung mithilfe des integrierten Jupyter-Buchs „Leitfaden zu SQL Server 2019“ <br/> &bull; &nbsp; Umbenennung in die SQL vNext-Erweiterung „Datenvirtualisierungserweiterung“ <br/> &bull; &nbsp; Hinzufügen der Unterstützung von Teradata und Mongo im Assistenten für externe Tabellen|
| Neue Notebookfeatures | &bull; &nbsp; Ankündigung von PowerShell-Notebooks <br/> &bull; &nbsp; Ankündigung von reduzierbaren Codezellen <br/>&bull; &nbsp; Leistungsverbesserungen in Notebooks <br/> &bull; &nbsp; Weitere Informationen finden Sie in der [vollständigen Liste der Verbesserungen](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22November+2019+Release%22+is%3Aclosed+label%3A%22Area+-+Notebooks%22). |
| Ankündigung von Jupyter-Büchern  | Jupyter-Bücher sind eine Sammlung von Notebooks und Markdowndateien, die in einem Inhaltsverzeichnis organisiert sind. |
| Neuer SQL Server-Bereitstellungsassistent  | Bietet jetzt Unterstützung für die Bereitstellung von <br/> &bull; &nbsp; SQL Server 2019 unter Windows <br/> &bull; &nbsp; SQL Server 2017 unter Windows <br/> &bull; &nbsp; SQL Server 2019 unter Docker <br/> &bull; &nbsp; SQL Server 2017 unter Docker |
| Ankündigung der allgemeinen Verfügbarkeit der Schemavergleichserweiterung| &bull; &nbsp; SQLCMD-Modus <br/> &bull; &nbsp; Lokalisierungsunterstützung <br/> &bull; &nbsp; Problembehebungen bei der Barrierefreiheit <br/> &bull; &nbsp; Sicherheitsfehler  |
| Ankündigung der allgemeinen Verfügbarkeit der DACPAC-Erweiterung für SQL Server| <br/> &bull; &nbsp; Lokalisierungsunterstützung <br/> &bull; &nbsp; Problembehebungen bei der Barrierefreiheit <br/> &bull; &nbsp; Sicherheitsfehler |
| Ankündigung der IntelliCode-Erweiterung für Visual Studio | Visual Studio IntelliCode unterstützt jetzt SQL, wodurch intelligentere Vorschläge für reservierte Schlüsselwörter bereitgestellt werden. |
| Fehler und Issues wurden behoben. | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22November+2019+Release%22+is%3Aclosed). |
| &nbsp; | &nbsp; |

## <a name="october-2019-hotfix-2"></a>Oktober 2019 (Hotfix 2)

11. Oktober 2019 &nbsp; / &nbsp; Version: 1.12.2

&nbsp;

| Change | Details |
| :----- | :------ |
| Der automatische Start von EH im Überprüfungsmodus wird deaktiviert. |  |
| &nbsp; | &nbsp; |

## <a name="october-2019-hotfix"></a>Oktober 2019 (Hotfix)

8\. Oktober 2019 &nbsp; / &nbsp; Version: 1.12.1

&nbsp;

| Change | Details |
| :----- | :------ |
| Das Problem, dass Anführungszeichen und umgekehrte Schrägstriche in Notebooks nicht richtig als Escapezeichen umgesetzt werden konnten, wurde behoben. |  |
| &nbsp; | &nbsp; |

## <a name="october-2019"></a>Oktober 2019

2\. Oktober 2019 &nbsp; / &nbsp; Version: 1.12.0

&nbsp;

| Change | Details |
| :----- | :------ |
| Release der Erweiterung für den Abfrageverlauf | Die Erweiterung für den SQL-Verlauf speichert alle früheren Abfragen, die in einer Azure Data Studio-Sitzung ausgeführt wurden, und listet diese in Ausführungsreihenfolge auf. Benutzern werden die Optionen „Abfrage öffnen“, „Abfrage ausführen“, „Abfrage löschen“, „Abfrageverlauf anhalten“ oder „Alle Einträge im Abfrageverlauf löschen“ angezeigt. |
| Neue Kopieren/Einfügen-Optionen für Ergebnisse | Wir haben weitere Möglichkeiten zum Kopieren und Einfügen von Ergebnissen aus dem Ergebnisraster hinzugefügt. |
| Update für PowerShell-Erweiterung |  |
| Fehler und Issues wurden behoben. | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/milestone/42?closed=1). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Bekannte Probleme

- Notebooks
  - [7080](https://github.com/microsoft/azuredatastudio/issues/7080) In seltenen Fällen, in denen das Notebook falsch serialisiert wurde

## <a name="september-2019"></a>September 2019

10. September 2019 &nbsp; / &nbsp; Version: 1.11.0 

&nbsp;

| Change | Details |
| :----- | :------ |
| Aktivieren des SQLCMD-Modus | Der Abfrage-Editor unterstützt jetzt das Umschalten des SQLCMD-Modus zum Schreiben und Bearbeiten von Abfragen als SQLCMD-Skripts. |
| Communityerweiterung: Query Editor Boost | Query Editor Boost ist eine Open-Source-Erweiterung zur Verbesserung des Abfrage-Editors von Azure Data Studio für Benutzer, die häufig Abfragen erstellen. &bull; &nbsp; Speichern der aktuellen Abfrage als Ausschnitt <br/>&bull; &nbsp; Wechseln zwischen Datenbanken mithilfe von STRG+U <br/> &bull; &nbsp; Neue Abfrage aus Vorlage <br/> &bull; &nbsp; Weitere Informationen finden Sie in der [vollständigen Liste der Verbesserungen](https://github.com/dzsquared/query-editor-boost). |
| Verbesserungen an Notebooks wurden vorgenommen. | &bull; &nbsp; Leistungsverbesserungen bei der Unterstützung größerer Notebookdateien <br/> &bull; &nbsp; Weitere Informationen finden Sie in der [vollständigen Liste der Verbesserungen](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22September+2019+Release%22+label%3A%22Area%3A+Notebooks%22+is%3Aclosed). |
| Das August-Release von Visual Studio Code (Merge 1.38) ist verfügbar. | Weitere Informationen finden Sie im [Artikel mit den aktuellen Verbesserungen](https://code.visualstudio.com/updates/v1_38). |
| Fehler und Issues wurden behoben. | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/milestone/39?closed=1). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Bekannte Probleme

- Notebooks
  - [7080](https://github.com/microsoft/azuredatastudio/issues/7080) In seltenen Fällen, in denen das Notebook falsch serialisiert wurde

## <a name="august-2019"></a>August 2019

15. August 2019 &nbsp; / &nbsp; Version: 1.10.0 

&nbsp;

| Change | Details |
| :----- | :------ |
| Freigabe der SandDance 1.3.1-Erweiterung | &bull; &nbsp; Intelligente Diagrammerkennung <br/>&bull; &nbsp; 3D-Visualisierungen <br/> &bull; &nbsp; Datenfilterung |
| Verbesserungen an Notebooks wurden vorgenommen. | &bull; &nbsp; Hinzufügen von Code- oder Textzelle in Zeilen <br/>&bull; &nbsp; Es wurde die Funktion hinzugefügt, das Ergebnis beim Klicken mit der rechten Maustaste auf das SQL-Ergebnisraster als CSV- oder JSON-Datei usw. zu speichern. <br/> &bull; &nbsp; Verbesserung der Notebookladeleistung zum schnelleren Laden von JSON-Code <br/> &bull; &nbsp; Weitere Informationen finden Sie in der [vollständigen Liste der Verbesserungen](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+label%3A%22Area%3A+Notebooks%22+milestone%3A%22August+2019+Release%22+is%3Aclosed). |
| Neue Features für SQL Server 2019 werden unterstützt. |  Mit diesem Release werden zusätzliche Features für Big-Data-Cluster in SQL Server 2019 unterstützt: <br/> &bull; &nbsp; Die Zeit zum Laden von Tabellen- und Spalteninformationen auf der Objektzuordnungsseite wurde reduziert. <br/> &bull; &nbsp; Es wurde ein Fehler in Verbindung mit dem Laden vorhandener datenbankweit gültiger Anmeldeinformationen auf der Seite „Verbindungsdetails“ behoben. <br/> &bull; &nbsp; Die Standard-Stichprobengröße, die für die PROSE-Analyse verwendet wird, wurde erweitert. | 
| DACPAC-Erweiterung unterstützt jetzt Azure AD | 
| Das Juli-Release von Visual Studio Code (Merge 1.37) ist verfügbar. | Weitere Informationen finden Sie im [Artikel mit den aktuellen Verbesserungen](https://code.visualstudio.com/updates/v1_37). |
| Fehler und Issues wurden behoben. | Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/milestone/39?closed=1). |
| &nbsp; | &nbsp; |

## <a name="july-2019"></a>Juli 2019

11. Juli 2019 &nbsp; / &nbsp; Version: 1.9.0 

&nbsp;

| Change | Details |
| :----- | :------ |
| Die Erweiterung SentryOne Plan Explorer wurde veröffentlicht. | Der geschätzte Microsoft-Partner SentryOne stellt ab sofort die Erweiterung [SentryOne Plan Explorer für Azure Data Studio](https://www.sentryone.com/products/sentryone-plan-explorer-extension-azure-data-studio) zur Verfügung. <br> Dabei handelt es sich um eine kostenlose Erweiterung, die erweiterte Ausführungsplandiagramme für Abfragen in Azure Data Studio bereitstellt. Mit den optimierten Layoutalgorithmen und der intuitiven Farbcodierung lassen sich schnell Operatoren identifizieren, die die Abfrageleistung besonders stark beeinträchtigen. Weitere Informationen zur Erweiterung finden Sie im [Blogbeitrag von SentryOne](https://sqlperformance.com/2019/07/sentryone/plan-explorer-extension-azure-data-studio). |
| Neue Features für den Schemavergleich sind verfügbar. | &bull; &nbsp; Schemavergleichsdateien (SCMP-Dateien) werden unterstützt. <br/>&bull; &nbsp; Der Schemavergleichsabbruch wird unterstützt. <br/>&bull; &nbsp; Weitere Informationen finden Sie in der [vollständigen Liste aller Änderungen](https://github.com/microsoft/azuredatastudio/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A%22July+2019+Release%22+label%3A%22Area%3A+Schema+Compare%22+is%3Aclosed+).|
| Verbesserungen an Notebooks wurden vorgenommen. | &bull; &nbsp; Unterstützung für Plotly Python <br/>&bull; &nbsp; Öffnen von Notebook im Browser <br/> &bull; &nbsp; Dialogfeld für die Python-Paketverwaltung <br/> &bull; &nbsp; Leistungs- und Markdownoptimierungen <br/> &bull; &nbsp; Update der Tastenkombinationen <br/>  &bull; &nbsp; Weitere Informationen finden Sie in der [Liste der Fehlerbehebungen und Featureverbesserungen](https://github.com/microsoft/azuredatastudio/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A%22July+2019+Release%22+is%3Aclosed+label%3A%22Area%3A+Notebooks%22+). |
| Neue Features für SQL Server 2019 werden unterstützt. |  Mit diesem Release werden zusätzliche Features für Big-Data-Cluster in SQL Server 2019 unterstützt: <br/> &bull; &nbsp; Tabelle mit Dienstendpunkten im Verwaltungsdashboard mit allen wichtigen Diensten im Cluster <br/> &bull; &nbsp; Notebook für Clusterstatus, das zeigt, wie Sie den Clusterstatus für alle Dienste und Pods abfragen und eine Problembehandlung durchführen| 
| Aktualisierte Sprachpakete sind verfügbar.| Im Marketplace für Erweiterungen sind jetzt 10 Sprachpakete verfügbar. Sie können einfach eine Sprache suchen und diese über den Marketplace installieren. Anschließend fordert Azure Data Studio Sie auf, einen Neustart mit der ausgewählten Sprache durchzuführen. |
| SQL Server Profiler wurde aktualisiert. | Die SQL Server Profiler-Erweiterung wurde um neue Features ergänzt: <br/> &bull; &nbsp; Filtern nach Datenbanknamen <br/> &bull; &nbsp; Unterstützung für Kopieren und Einfügen <br/> &bull; &nbsp; Speichern und Laden von Filtern <br/>Weitere Informationen finden Sie in der [vollständigen Liste der Verbesserungen für die SQL Server Profiler-Erweiterung](https://github.com/microsoft/azuredatastudio/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aclosed+milestone%3A%22July+2019+Release%22+label%3A%22Area%3A+SQL+Profiler%22+).  |
| Das Mai-Release von Visual Studio Code (Merge 1.35) ist verfügbar. | Weitere Informationen finden Sie im [Artikel mit den aktuellen Verbesserungen](https://code.visualstudio.com/updates/v1_35). |
| Fehler und Issues wurden behoben. | Wenn in früheren Releases von Azure Data Studio beim Herstellen einer Verbindung über das Dialogfeld „Verbindung“ eine Benutzerdatenbank ausgewählt wurde, war der resultierende Objekt-Explorer-Eintrag auf diese einzelne Datenbank beschränkt. Dieses Verhalten wurde im aktuellen Release so angepasst, dass Eigenschaften auf Serverebene auch im Objekt-Explorer angezeigt werden. <br/> Eine vollständige [Liste der Fehlerbehebungen und Issues finden Sie auf GitHub](https://github.com/microsoft/azuredatastudio/milestone/35?closed=1). |
| &nbsp; | &nbsp; |

## <a name="june-2019"></a>Juni 2019

6\. Juni 2019 &nbsp; / &nbsp; Version: 1.8.0

&nbsp;

| Change | Details |
| :----- | :------ |
| Das Release der CMS-Erweiterung (Central Management Servers; zentrale Verwaltungsserver) ist verfügbar. | Zentrale Verwaltungsserver speichern eine Liste von SQL Server-Instanzen, die in ein oder mehrere Gruppen zentraler Verwaltungsserver unterteilt sind. Benutzer können eine Verbindung mit ihren eigenen CMS-Servern herstellen und diese verwalten. Beispielsweise können sie Server hinzufügen oder entfernen. [Weitere Informationen](../relational-databases/administer-multiple-servers-using-central-management-servers.md) |
| Das Release der Datenbankverwaltungstool-Erweiterungen für Windows ist verfügbar. | Mit dieser Erweiterung werden in Azure Data Studio zwei Features eingeführt, die in SQL Server Management Studio besonders häufig verwendet werden. Benutzer können mit der rechten Maustaste auf verschiedene Objekte (z. B. Datenbanken, Tabellen, Spalten und Sichten) und dann auf „Eigenschaften“ klicken, um das Dialogfeld für SSMS-Eigenschaften für dieses Objekt anzuzeigen. Zusätzlich können Benutzer mit der rechten Maustaste auf eine Datenbank und dann auf „Skripts generieren“ klicken, um den bekannten Assistenten zum Generieren von Skripts zu starten. 
| Verbesserungen am Schemavergleich wurden vorgenommen. | &bull; &nbsp; Die Optionen „Ausschließen“ und „Einschließen“ wurden hinzugefügt. <br/>&bull; &nbsp; „Skript generieren“ öffnet ein Skript nach dessen Erstellung. <br/>&bull; &nbsp; Doppelte Scrollleisten wurden entfernt.  <br/>&bull; &nbsp; Verbesserungen an der Formatierung und am Layout wurden vorgenommen. <br/>&bull; &nbsp; Weitere Informationen finden Sie in der [vollständigen Liste aller Änderungen](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22June+2019+Release%22+label%3A%22Area%3A+Schema+Compare%22+is%3Aclosed).|
| Der Abschnitt „Meldungen“ wurde auf eine separate Registerkarte verschoben. | Wenn Benutzer SQL-Abfragen ausführten, befanden sich die Ergebnisse und Meldungen bisher auf StackPanel-Elementen. Nun sind sie ebenso wie in SSMS auf eigenständigen Registerkarten in einem Panel aufgeführt. |
| Verbesserungen an SQL-Notebooks wurden vorgenommen. | &bull; &nbsp; Benutzer können nun eigene Installationen von Python 3 oder Anaconda in Notebooks verwenden. <br/>&bull; &nbsp; Mehrere Stabilitätsprobleme und letzte Fehler wurden behoben. <br/> &bull; &nbsp; Weitere Informationen finden Sie in der [vollständigen Liste der Verbesserungen](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22June+2019+Release%22+is%3Aclosed+label%3A%22Area%3A+Notebooks%22).|
| Das April-Release von Visual Studio Code (Merge 1.34) ist verfügbar. | Weitere Informationen finden Sie im [Artikel mit den aktuellen Verbesserungen](https://code.visualstudio.com/updates/v1_34). |
| Fehler und Issues wurden behoben. | Weitere Informationen finden Sie in der [Liste der Fehler und Issues auf GitHub](https://github.com/microsoft/azuredatastudio/milestone/32?closed=1). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Bekannte Probleme

- Datenbankverwaltungstool-Erweiterungen für Windows
    - „Eigenschaften“ kann nicht über den getrennten Serverknoten aufgerufen werden.
    - „Eigenschaften“ kann nicht für Azure-Server aufgerufen werden.
    - Nicht alle Objekte verfügen über Dialogfelder für Eigenschaften.
    - Der Aufruf von Dialogfeldern nimmt sehr viel Zeit in Anspruch.
    - Fehler beim Starten von Servern mit bestimmten Verbindungstypen (z. B. Azure AD)
- Notebooks
    - [#5838:](https://github.com/microsoft/azuredatastudio/issues/5838) Benutzer sollten die Systemversion von Python für Notebooks verwenden können.
- Schemavergleich
    - [#5804:](https://github.com/microsoft/azuredatastudio/issues/5804) Für Schemavergleichsaufgaben wird das Standardkontextmenü „Cancel“ (Abbrechen) angezeigt. Wenn darauf geklickt wird, werden allerdings keine Funktionen ausgeführt.

## <a name="may-2019"></a>Mai 2019

8\. Mai 2019 &nbsp; / &nbsp; Version: 1.7.0 

&nbsp;

| Change | Details |
| :----- | :------ |
| Das Release der Schemavergleichserweiterung ist verfügbar. | Der Schemavergleich ist ein bekanntes Feature in SQL Server Data Tools (SSDT). Der wichtigste Anwendungsfall besteht darin, die Unterschiede zwischen Datenbanken und DAPAC-Dateien zu ermitteln und zu visualisieren sowie Aktionen auszuführen, mit denen diese Unterschiede beseitigt werden. |
| Die Aufgabenansicht wurde in das Ausgabefenster verschoben. | Benutzer können sich nun den Status zeitintensiver Aufgaben wie „Sichern“, „Wiederherstellen“ und „Schemavergleich“ in der Aufgabenansicht im Ausgabefenster ansehen.
| Eine Startseite wurde hinzugefügt. | &bull; &nbsp; Links zu häufig ausgeführten Aktionen wie „Neue Abfrage“, „Neue Datei“ und „Neues Notebook“ wurden bereitgestellt. <br/>&bull; &nbsp; Links zur Dokumentation und zu GitHub wurden bereitgestellt. |
| Verbesserungen an SQL-Notebooks wurden vorgenommen. | &bull; &nbsp; Verbesserungen beim Rendering von Markdown wurden vorgenommen. Dadurch werden z. B. Hinweise und Tabellen besser unterstützt. <br/>&bull; &nbsp; Die Symbolleiste ist nun benutzerfreundlicher. <br/>&bull; &nbsp; Markdownlinks für vertrauenswürdige Notebooks müssen nicht mehr durch CMD bzw. STRG und einen Klick aufgerufen werden. Stattdessen können sie einfach angeklickt werden. <br/>&bull; &nbsp; Die Bereinigung der Jupyter-Prozesse nach dem Schließen von Notebooks wurde verbessert. Außerdem wurden Fehler beim gleichzeitigen Starten mehrerer Notebooks behoben. <br/>&bull; &nbsp; SQL-Notebookverbindungen wurden so angepasst, dass keine Fehler mehr auftreten, wenn zwei Notebooks für dieselbe Datenbank ausgeführt werden. <br/>&bull; &nbsp; Das automatische Scrollen in Notebooks zur aktuell ausgeführten Zelle wurde verbessert. Er wird gestartet, wenn in der Symbolleiste auf „Zellen ausführen“ geklickt wird. <br/>&bull; &nbsp; Allgemeine Stabilitäts- und Leistungsverbesserungen wurden vorgenommen. |
| Fehler und Issues wurden behoben. | Weitere Informationen finden Sie in der [Liste der Fehler und Issues auf GitHub](https://github.com/microsoft/azuredatastudio/milestone/31?closed=1). |
| &nbsp; | &nbsp; |

## <a name="april-2019"></a>April 2019

18. April 2019 &nbsp; / &nbsp; Version: 1.6.0 

&nbsp;

| Change | Details |
| :----- | :------ |
| Die Registerkarte **Server** wurde in **Verbindungen** umbenannt. | |
| Der Azure-Ressourcen-Explorer wurde als Azure-Viewlet nach „Verbindungen“ verschoben. | Benutzer können sich nun Azure SQL-Instanzen über das Azure-Viewlet in der Ansicht „Verbindungen“ anzeigen lassen. Außerdem können sie diese erweitern, sodass Ansichtsobjekte unter jedem Server oder jeder Datenbank angezeigt werden.|
| Verbesserungen an SQL-Notebooks wurden vorgenommen. | &bull; &nbsp; Der Symbolleiste wurde eine Schaltfläche hinzugefügt, mit der sich die Ausgabe für alle Zellen löschen lässt. <br/>&bull; &nbsp; Der Symbolleiste wurde eine Schaltfläche hinzugefügt, mit der sich alle Zellen ausführen lassen. <br/>&bull; &nbsp; Ein Problem wurde behoben, durch das der Servername (sofern festgelegt) anstelle des Verbindungsnamens im Dropdownmenü „Anfügen an“ angezeigt wurde. <br/>&bull; &nbsp; Ein Problem wurde behoben, durch das Bilder in Markdown nicht gerendert wurden, wenn für diese relative Pfade angegeben wurden. <br/>&bull; &nbsp; Die Funktionen für Notebookraster wurden verbessert. Durch einen Doppelklick auf eine Spalte wird nun deren Größe automatisch geändert. Außerdem wird das Mausrad besser unterstützt. <br/>&bull; &nbsp; Die Fehlerbehandlung für die Installation von Python mithilfe von Notebooks wurde verbessert. Außerdem wurde die Installationsresilienz erhöht. <br/>&bull; &nbsp; Das Feature „Alle auswählen“ funktioniert nun besser bei der Auswahl von Notebookzellen. <br/>&bull; &nbsp; Notebookverbindungen wurden so angepasst, dass Notebooks nicht geschlossen werden und dadurch nicht die Verbindung mit dem Objekt-Explorer beeinträchtigen. <br/>&bull; &nbsp; In Notebooks wird dem Benutzer nun eine Meldung angezeigt, wenn eine Notebookverbindung getrennt wird, diese jedoch erforderlich ist, um Zellen auszuführen.<br/>&bull; &nbsp; Die wiederholte Aktivierung nicht gespeicherter Notebooks in ADS nach einem Neustart von ADS wird nun besser unterstützt. |
| Fehler und Issues wurden behoben. | Weitere Informationen finden Sie in der [Liste der Fehler und Issues auf GitHub](https://github.com/Microsoft/azuredatastudio/milestone/26?closed=1). |
| &nbsp; | &nbsp; |

## <a name="march-2019-hotfix"></a>März 2019 (Hotfix)

22. März 2019 &nbsp; / &nbsp; Version: 1.5.2 &nbsp; / &nbsp;Hotfixrelease

&nbsp;

| Change | Details |
| :----- | :------ |
| Mehrere in Version 1.5.1 entdeckte Probleme wurden behoben. | Weitere Informationen finden Sie im [März-Hotfixrelease auf GitHub](https://github.com/Microsoft/azuredatastudio/milestone/28).<br/> <br/>&bull; &nbsp; Ein Problem wurde behoben, durch das Benutzer ein Notebook, das über die Aufgabe „Notebook öffnen“ im Dashboard geöffnet wurde, nicht schließen konnten. <br/>&bull; &nbsp; Ein Problem wurde behoben, durch das Notebook-JSON-Code nach dem Speichern um eine zusätzliche geschweifte Klammer („}“) ergänzt wurde. <br/>&bull; &nbsp; Ein Problem wurde behoben, durch das Notebookraster nicht auf Designänderungen reagierten. <br/>&bull; &nbsp; Ein Problem wurde behoben, durch das der vollständige Notebookpfad in der Tabellenkopfzeile angezeigt wurde. Nun wird nur noch der Dateiname angezeigt. |
| &nbsp; | &nbsp; |

## <a name="march-2019"></a>März 2019

18. März 2019 &nbsp; / &nbsp; Version: 1.5.1

&nbsp;

| Change | Details |
| :----- | :------ |
| Azure Data Studio wurde eine [PostgreSQL-Erweiterung](./extensions/postgres-extension.md) hinzugefügt. | Unterstützte Features: <br/>&bull; &nbsp; Dialogfeld „Verbindung“ <br/>&bull; &nbsp; Objekt-Explorer <br/>&bull; &nbsp; Abfrage-Editor <br/>&bull; &nbsp; Diagramme <br/>&bull; &nbsp; Dashboards <br/>&bull; &nbsp; Codeausschnitte <br/>&bull; &nbsp; Bearbeiten von Daten <br/>&bull; &nbsp; Notebooks |
| SQL-Notebooks wurden hinzugefügt. | Der SQL-Kernel wird nun für den integrierten Notebook-Viewer unterstützt: <br/>&bull; &nbsp; T-SQL wird unterstützt. <br/>&bull; &nbsp; PGSQL wird unterstützt. |
| Eine PowerShell-Erweiterung wurde hinzugefügt. | Mit dieser Änderung wird die [PowerShell-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell) zur Verfügung gestellt, die bisher nur für VS Code verfügbar war.  |
| Die DACPAC-Erweiterung für SQL Server wurde hinzugefügt.  | Der DAC-Assistent wurde aus der Erweiterung SQL Server-Import entfernt und in eine neue Erweiterung integriert.  |
| Die Communityerweiterung „QueryPlan.show“ wurde hinzugefügt. | Die Integration zur Visualisierung von Abfrageplänen wird nun unterstützt.  |
| Die SQL Server 2019-Erweiterung (Vorschauversion) wurde aktualisiert. | &bull; &nbsp; Jupyter Notebook wird nun einschließlich der Kernel für Python 3 und Spark im Azure Data Studio-Haupttool unterstützt. <br/>&bull; &nbsp; Fehler im Assistenten für externe Daten wurden behoben. |
| Fehler und Issues wurden behoben. | Weitere Informationen finden Sie in der [Liste der Fehler und Issues auf GitHub](https://github.com/Microsoft/azuredatastudio/milestone/25?closed=1). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Bekannte Probleme
- [#4427:](https://github.com/Microsoft/azuredatastudio/issues/4427) Wenn eine Zelle ausgewählt und auf „Ausführen“ geklickt wird, bevor die Kernel für Spark bereit sind, führt dies zu einem schwerwiegenden Fehler. **Problemumgehung:** Warten Sie, bis die Kernel geladen sind, und führen Sie erst danach Zellen aus.
- [#4493:](https://github.com/Microsoft/azuredatastudio/issues/4493) Wenn ADS über SSMS gestartet und dabei die SQL-Authentifizierung eingesetzt wird, wird der Benutzer aufgefordert, sein Kennwort einzugeben. **Problemumgehung:** Verwenden Sie bis auf Weiteres die Windows-Authentifizierung. 
- [#4494:](https://github.com/Microsoft/azuredatastudio/issues/4494) Das SQL-Notebookfeature kann nicht installiert werden. <br/>
**Problemumgehung:** Führen Sie die auf [GitHub beschriebenen Schritte](https://github.com/Microsoft/azuredatastudio/issues/4494#issuecomment-473043832) aus. 
- [#4503:](https://github.com/Microsoft/azuredatastudio/issues/4503) Azure Data Studio kann nicht direkt über den Ordner „Downloads“ (Mac) geöffnet werden. <br />
**Problemumgehung:** Entzippen Sie die App, und starten Sie anschließend den Computer neu. Das Problem wird untersucht. 
- [#4539:](https://github.com/Microsoft/azuredatastudio/issues/4539)  Wenn für ein Notebook die Aktion „Speichern unter“ ausgeführt wird, geht der Verbindungskontext verloren. <br />
**Problemumgehung:** Das Problem wird im nächsten Release behoben. 
- [#4458:](https://github.com/Microsoft/azuredatastudio/issues/4458) Der DACPAC-Extraktionsvorgang führt dazu, dass SQL Tools Service abstürzt, wenn eine ungültige Version verwendet wird. <br/>
**Problemumgehung:** Starten Sie Azure Data Studio neu, und stellen Sie sicher, dass die richtige Version verwendet wird.
- Die Symbole für „Neues Notebook“ und „Notebook öffnen“ werden nicht mehr angezeigt. <br/>
**Problemumgehung:** Der Legacyverbindungstyp ist veraltet. Empfohlen wird, eine Verbindung mit dem SQL Server-Endpunkt herzustellen. Anschließend werden alle Aktionen („Neues Notebook“ und „Spark Job“ (Spark-Auftrag)) wie erwartet angezeigt. 

## <a name="february-2019"></a>Februar 2019

13. Februar 2019 &nbsp; / &nbsp; Version: 1.4.5

&nbsp;

| Change | Details |
| :----- | :------ |
| Das Erweiterungspaket **Admin pack for SQL Server** (Administratorpaket für SQL Server) wurde hinzugefügt. | Dieses Paket vereinfacht die Installation von SQL Server-Administratorerweiterungen. Dies schließt Folgendes ein:<br/>&bull; &nbsp; [SQL Server-Agent](./extensions/sql-server-agent-extension.md)<br/>&bull; &nbsp; [SQL Server Profiler](./extensions/sql-server-profiler-extension.md)<br/>&bull; &nbsp; [SQL Server-Import](./extensions/sql-server-import-extension.md) |
| Die Ereignisunterstützung in der Profilerweiterung wurde um eine Filterfunktion ergänzt. | &nbsp; |
| Das Feature „Save as XML“ (Als XML speichern) wurde hinzugefügt. Damit können T-SQL-Ergebnisse im XML-Format gespeichert werden. | &nbsp; |
| Verbesserungen am DAC-Assistenten wurden vorgenommen. | &bull; &nbsp; Die Schaltfläche „Skript generieren“ wurde hinzugefügt.<br/>&bull; &nbsp; Eine Ansicht wurde hinzugefügt, in der Warnungen vor möglichem Datenverlust während der Bereitstellung angezeigt werden. |
| Die SQL Server 2019-Erweiterung (Vorschauversion) wurde aktualisiert. | Weitere Informationen finden Sie unter [Datenvirtualisierungserweiterung](./extensions/data-virtualization-extension.md). |
| Das Ergebnisstreaming wurde für zeitintensive Abfragen standardmäßig aktiviert. | &nbsp; |
| Fehler und Issues wurden behoben. | Weitere Informationen finden Sie in der [Liste der Fehler und Issues auf GitHub](https://github.com/Microsoft/azuredatastudio/milestone/23?closed=1). |
| &nbsp; | &nbsp; |

## <a name="january-2019-hotfix"></a>Januar 2019 (Hotfix)

16. Januar 2019 &nbsp; / &nbsp; Version: 1.3.9 &nbsp; / &nbsp; Hotfixrelease

&nbsp;

| Change | Details |
| :----- | :------ |
| Mehrere in Version 1.3.8 entdeckte Probleme wurden behoben. | Weitere Informationen finden Sie im [Januar-Hotfixrelease auf GitHub](https://github.com/Microsoft/azuredatastudio/milestone/24?closed=1).<br/><br/>Ausführliche Informationen finden Sie unter:<br/>&bull; &nbsp; [Änderungsprotokoll auf GitHub](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md).<br/>&bull; &nbsp; [Releases auf GitHub](https://github.com/Microsoft/azuredatastudio/releases). |
| &nbsp; | &nbsp; |

## <a name="january-2019"></a>Januar 2019

9\. Januar 2019 &nbsp; / &nbsp; Version: 1.3.8

&nbsp;

| Change | Details |
| :----- | :------ |
| Ein neues Benutzerinstallationsprogramm für Windows wurde hinzugefügt. | Für das neue Benutzerinstallationsprogramm sind anders als beim vorhandenen Systeminstallationsprogramm keine Administratorberechtigungen erforderlich. Dadurch können auch Nichtadministratoren leichter Upgrades durchführen. |
| Die Azure Active Directory-Authentifizierung wird nun unterstützt. | &nbsp; |
| Ankündigung von Idera SQL DM Performance Insights (Vorschau) | &nbsp; |
| Der DAC-Assistent wird in der Erweiterung SQL Server-Import unterstützt. | &nbsp; |
| Die SQL Server 2019-Erweiterung (Vorschauversion) wurde aktualisiert. | Weitere Informationen finden Sie unter [Datenvirtualisierungserweiterung](./extensions/data-virtualization-extension.md). |
| Verbesserungen am SQL Server Profiler wurden vorgenommen. | &nbsp; |
| Das Ergebnisstreaming für große Abfragen (Vorschauversion) ist verfügbar. | &nbsp; |
| Die Communityerweiterungen „sp_executesql to SQL“ und „New Database“ sind verfügbar. | &nbsp; |
| Fehler und Issues wurden behoben. | Weitere Informationen finden Sie in der [Liste der Fehler und Issues auf GitHub](https://github.com/Microsoft/azuredatastudio/milestone/19?closed=1). |
| &nbsp; | &nbsp; |

## <a name="november-2018"></a>November 2018

6\. November 2018 &nbsp; / &nbsp; Version: 1.2.4

&nbsp;

| Change | Details |
| :----- | :------ |
| Die SQL Server 2019-Erweiterung (Vorschauversion) wurde aktualisiert. | Weitere Informationen finden Sie unter [Datenvirtualisierungserweiterung](./extensions/data-virtualization-extension.md). |
| Die neue Erweiterung Paste the Plan ist verfügbar. | &nbsp; |
| Die neue Erweiterung High Color Queries einschließlich des SSMS-Editor-Designs ist verfügbar. | &nbsp; |
| Fehlerbehebungen in den Erweiterungen SQL Server-Agent, SQL Server Profiler und SQL Server-Import wurden vorgenommen. | &nbsp; |
| Ein Keep-Alive-Problem bei .NET Core-Sockets wurde behoben, durch das inaktive Verbindungen unter macOS getrennt wurden. | &nbsp; |
| Upgrade von SQL Tools Service auf .NET Core 2.2 Preview 3 (für zukünftige Unterstützung von Azure AD) | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-november-2018"></a>Fehlerbehebungen im November 2018

- Behebung von [Issue #2933](https://github.com/Microsoft/azuredatastudio/issues/2933): Verbindung mit Azure SQL-Datenbank wird getrennt
- Behebung von [Issue #2914](https://github.com/Microsoft/azuredatastudio/issues/2914): Die Ausnahme „Invalid argument“ (Ungültiges Argument) tritt beim Erweitern von OE-Datenbankknoten auf.
- Behebung von [Issue #2935](https://github.com/Microsoft/azuredatastudio/pull/2935): Mehrzeilige Nachrichten sollten in Abfrageergebnissen korrekt angezeigt werden.
- Behebung von [Issue #2906](https://github.com/Microsoft/azuredatastudio/pull/2906): Fehlerbehebung für „Daten bearbeiten“ bei Dokumentnamen, wenn der Tabellenname Sonderzeichen enthält.
- Behebung von [Issue #2929](https://github.com/Microsoft/azuredatastudio/issues/2929): Änderungsprotokoll für integrierte Erweiterung fordert dazu auf, die Versionshinweise für VS Code auf Änderungen zu überprüfen.
- Behebung von [Issue #2719](https://github.com/Microsoft/azuredatastudio/issues/2719): Das Design mit hohem Kontrast verdoppelt/verdreifacht Symbole.
- Behebung von [Issue #3047](https://github.com/Microsoft/azuredatastudio/pull/3047): Eine Befehlszeilenschnittstelle zum Herstellen einer Verbindung mit SQL Server sollte hinzugefügt werden.
- Behebung von [Issue #3031](https://github.com/Microsoft/azuredatastudio/pull/3031): Ein Abfrageplandesign sollte unterstützt werden.

## <a name="october-2018"></a>Oktober 2018

29. Oktober 2018 &nbsp; / &nbsp; Version: 1.1.4

&nbsp;

| Change | Details |
| :----- | :------ |
| Einführung des Azure-Ressourcen-Explorers zum Durchsuchen von Azure SQL-Datenbank | &nbsp; |
| Objekt-Explorer- und Abfrage-Editor-Verbindungen sind nun stabiler. | &nbsp; |
| SQL Server Agent-Erweiterungen wurden verbessert. | &nbsp; |
| Die SQL Server 2019-Erweiterung (Vorschauversion) wurde aktualisiert. | Weitere Informationen finden Sie unter [Datenvirtualisierungserweiterung](./extensions/data-virtualization-extension.md). |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-october-2018"></a>Fehlerbehebungen im Oktober 2018

- Behebung von [Issue #2717](https://github.com/Microsoft/azuredatastudio/issues/2717): Die Formatierung beim Klicken auf XML-Spaltenergebnisse ist fehlerhaft.
- Behebung von [Issue #2993](https://github.com/Microsoft/azuredatastudio/issues/2993): Die Breite des Ergebnisfensters wird nicht angepasst.
- Behebung von [Issue #2999](https://github.com/Microsoft/azuredatastudio/issues/2999): Die Datei „System.Diagnostics.Tracing“ kann unter macOS beim Herstellen einer Verbindung mit der Datenbank nicht geladen werden.
- Behebung von [Issue #2851](https://github.com/Microsoft/azuredatastudio/issues/2851): Das TimeSeries-Diagramm wird nicht korrekt gerendert.
- Behebung von [Issue #2996](https://github.com/Microsoft/azuredatastudio/issues/2996): Temporäre Tabellen werden bei einer plötzlichen Sitzungsänderung gelöscht.

Ausführliche Informationen finden Sie im [Änderungsprotokoll](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md)und [in den Releases](https://github.com/Microsoft/azuredatastudio/releases).

## <a name="september-2018-ga-release"></a>September 2018 (allgemein verfügbares Release)

24. September 2018 &nbsp; / &nbsp; Version: 1.0 &nbsp; / &nbsp; allgemein verfügbares Release

Allgemein verfügbares Release von Azure Data Studio (früher SQL Operations Studio)

&nbsp;

| Change | Details |
| :----- | :------ |
| Leistungsverbesserungen für Abfrageergebnisraster und Änderungen für eine benutzerfreundlichere Anzeige bei einer großen Zahl von Resultsets wurden vorgenommen. | &nbsp; |
| Durch die Aktualisierung des VS Code-Quellcodes von Version 1.23 auf Version 1.26.1 werden ein Rasterlayout und ein verbesserter Einstellungs-Editor (Vorschauversion) bereitgestellt. | &nbsp; |
| Verbesserungen bei der Barrierefreiheit für die Sprachausgabe, Tastaturnavigation und für hohe Kontraste wurden vorgenommen. | &nbsp; |
| Die Option `Connection name` wurde hinzugefügt. Durch diese wird ein alternativer Anzeigename im Serverviewlet bereitgestellt. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="announcing-the-sql-server-2019-preview-extension"></a>Ankündigung der SQL Server 2019-Erweiterung (Vorschauversion)

&nbsp;

| Change | Details |
| :----- | :------ |
| Für SQL Server 2019 werden Previewfunktionen wie [Big-Data-Cluster](../big-data-cluster/big-data-cluster-overview.md) unterstützt. | Sie können nun eine Verbindung mit dem HDFS/Spark-Gateway herstellen, das in der Vorschauversion von SQL Server 2019 enthalten ist.<br/><br/>Sie können nun HDFS durchsuchen, Dateien hochladen und speichern sowie nützliche Aktionen wie „Analyze in Notebook“ (In Notebook analysieren) für CSV-Dateien ausführen.<br/><br/>Sie können nun Spark-Aufträge über das Dashboard übermitteln oder mit der rechten Maustaste auf eine HDFS/Spark-Verbindung im Objekt-Explorer klicken. |
| Azure Data Studio-Notebooks. | Sie können nun Notebooks mithilfe eines integrierten Notebook-Viewers erstellen oder öffnen. In diesem Release unterstützt der Notebook-Viewer ausschließlich das Herstellen von Verbindungen mit lokalen Kernels und dem Big Data-Cluster von SQL Server 2019.<br/><br/>Sie können nun die Bibliotheken von PROSE Code Accelerator in Ihrem Notebook verwenden, um Dateiformate und die Datentypen für die schnelle Datenaufbereitung zu ermitteln. |
| Azure-Ressourcen-Explorer. | Mithilfe der Azure-Ressourcen-Explorer-Ansicht können Sie nach datenbezogenen Endpunkten für Ihre Azure-Konten suchen und im Objekt-Explorer Verbindungen mit diesen Endpunkten herstellen. Ab diesem Release wird Azure SQL-Datenbank unterstützt. |
| Assistent zum Erstellen externer Tabellen für PolyBase in SQL Server. | Sie können nun mit einem benutzerfreundlichen Assistenten eine externe Tabelle und zugehörige Metadatenstrukturen erstellen. In diesem Release werden Remoteserver unter SQL Server und Oracle unterstützt. |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-september-2018"></a>Fehlerbehebungen im September 2018

- Behebung von [Issue #2647](https://github.com/Microsoft/azuredatastudio/issues/143): Erhebliche Rückschritte bei Diagrammen
- Behebung von [Issue #2648](https://github.com/Microsoft/azuredatastudio/issues/143): SELECT gibt JSON-Hyperlinks für die gesamte Spalte zurück.

Ausführliche Informationen finden Sie im [Änderungsprotokoll](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md)und [in den Releases](https://github.com/Microsoft/azuredatastudio/releases).

## <a name="august-2018"></a>August 2018

30. August 2018 &nbsp; / &nbsp; Version: 0.32.8 &nbsp; / &nbsp; Public Preview

In der *Public Preview vom August* wurde der Schwerpunkt auf Fehlerbehebungen, Ergänzungen vorhandener Szenarios und auf die Produktstabilisierung gelegt.

_Version 0.32.8 enthält Fehlerbehebungen für einige Regressionen, die in Version 0.32.7 aufgetreten sind ([#1971](https://github.com/Microsoft/azuredatastudio/issues/1971), [#2372](https://github.com/Microsoft/azuredatastudio/issues/2372))_

&nbsp;

| Change | Details |
| :----- | :------ |
| Ankündigung der Erweiterung SQL Server-Import. | &nbsp; |
| Sitzungsverwaltung für SQL Server Profiler. | &nbsp; |
| Sitzungsvorlagen für SQL Server Profiler werden unterstützt. | &nbsp; |
| Verbesserungen am SQL Server-Agent wurden vorgenommen. | &nbsp; |
| Neue Communityerweiterung: First Responder Kit. | &nbsp; |
| Verbesserungen bei der Benutzerfreundlichkeit: Verbindungszeichenfolgen | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-august-2018"></a>Fehlerbehebungen im August 2018

- SQL-Analyse im Abfrage-Editor-Fenster mithilfe des `Parse Syntax`-Befehls.
- Behebung von [Issue #143](https://github.com/Microsoft/azuredatastudio/issues/143): Das Zeichen „@“ wird bei einem Doppelklick nicht im Variablennamen ausgewählt.
- Behebung von [Issue #387](https://github.com/Microsoft/azuredatastudio/issues/387): Das Datenbanksymbol auf einer SQL-Registerkarte ist rot.
- Behebung von [Issue #825](https://github.com/Microsoft/azuredatastudio/issues/825): Anforderung: Automatisches Herstellen einer Verbindung mit dem aktuellen Server nach Ausführung von „Skripterstellung als“. 
- Behebung von [Issue #1278](https://github.com/Microsoft/azuredatastudio/issues/1278): sqlops.desktop [Desktop-Eintrag]: redundanter Wert für Name und Kommentar.
- Behebung von [Issue #1285](https://github.com/Microsoft/azuredatastudio/issues/1285): Eine Aktualisierung führt dazu, dass das Anwendungssymbol unter Windows entfernt bzw. ersetzt wird.
- Behebung von [Issue #1317](https://github.com/Microsoft/azuredatastudio/issues/1317): Das Dezimaltrennzeichen sollte korrigiert werden.
- Behebung von [Issue #1474](https://github.com/Microsoft/azuredatastudio/issues/1474): Beim Abbrechen des Vorgangs „Verbindung ändern“ wird die aktuelle Verbindung getrennt.
- Behebung von [Issue #1497](https://github.com/Microsoft/azuredatastudio/issues/1497): Die Optionen für „View as Chart“ (Als Diagramm anzeigen) sind im unteren Bildschirmbereich nicht vollständig zu sehen.
- Behebung von [Issue #1524](https://github.com/Microsoft/azuredatastudio/issues/1524): Shell/Dashboard: Die Symbole des Hauptviewlets lassen sich verschieben, was zu einem Absturz der App führen kann.
- Behebung von [Issue #1578](https://github.com/Microsoft/azuredatastudio/issues/1578): Der Ordner im Remotedateibrowser kann nicht durch einen Klick auf den Ordnernamen erweitert oder reduziert werden.
- Behebung von [Issue #1620](https://github.com/Microsoft/azuredatastudio/issues/1620): Featurevorschlag: Abruf einer Verbindungszeichenfolge für eine bestehende Verbindung.
- Behebung von [Issue #1624](https://github.com/Microsoft/azuredatastudio/issues/1624): Die Farbe des Dropdownmenüs ändert sich nicht, wenn es deaktiviert wird.
- Behebung von [Issue #1728](https://github.com/Microsoft/azuredatastudio/issues/1728): „Speichern unter“ erzeugt keine Dateien im JSON-, Excel- oder CSV-Format.
- Behebung von [Issue #1744](https://github.com/Microsoft/azuredatastudio/issues/1744): Die Scrollpositionen im Ergebnisbereich werden beim Wechseln zwischen Registerkarten nicht beibehalten.
- Behebung von [Issue #1748](https://github.com/Microsoft/azuredatastudio/issues/1748): Nach dem ersten Speichern einer Excel-Datei tritt bei darauffolgenden Speichervorgängen eine Fehlermeldung auf.
- Behebung von [Issue #1782](https://github.com/Microsoft/azuredatastudio/issues/1782): „Daten bearbeiten“: Beim Drücken der ESC-TASTE wird der ursprüngliche Wert der Zelle nicht wiederhergestellt.
- Behebung von [Issue #1836](https://github.com/Microsoft/azuredatastudio/issues/1836): SQL-Dateien werden nicht SQL Operations Studio zugeordnet.
- Behebung von [Issue #1850](https://github.com/Microsoft/azuredatastudio/issues/1850): Wenn „N''“ eingegeben wird, wird durch die Autovervollständigung „N'''“ angezeigt.
- Behebung von [Issue #1985](https://github.com/Microsoft/azuredatastudio/issues/1985): Copy from query results grid is off by one column. (Beim Kopieren eines Werts aus dem Ergebnisraster einer Abfrage wird der Wert aus der darauffolgenden Spalte kopiert.)
- Behebung von [Issue #1998](https://github.com/Microsoft/azuredatastudio/pull/1998): Ergänzung der VS Code-Version im Dialogfeld „Info“.
- Behebung von [Issue #2042](https://github.com/Microsoft/azuredatastudio/pull/2042): Mitarbeiter: Die Schaltfläche zum Importieren von Abfragen aus SQL-Dateien wurde aktiviert.
- Behebung von [Issue #2091](https://github.com/Microsoft/azuredatastudio/issues/2091): Die Tastenkombination STRG+C kann nicht zum Kopieren von Werten aus dem Ergebnisbereich verwendet werden.
- Behebung von [Issue #2099](https://github.com/Microsoft/azuredatastudio/pull/2099): Weitere Optionen für „saveAsCsv“ wurden hinzugefügt.
- Behebung von [Issue #2107](https://github.com/Microsoft/azuredatastudio/issues/2107): Für Dashboard- und Profilerdokumente sollten entsprechende Symbole verwendet werden.
- Behebung von [Issue #2129](https://github.com/Microsoft/azuredatastudio/pull/2129): Die Scrollposition in „Daten bearbeiten“ sollte beim Wechseln von Registerkarten gespeichert werden.
- Behebung von [Issue #2152](https://github.com/Microsoft/azuredatastudio/issues/2152): Angezeigte Zeilennummer für Ergebnisraster beginnt bei null.

### <a name="known-issues-august-2018"></a>Bekannte Probleme im August 2018

- [Problem #2371:](https://github.com/Microsoft/azuredatastudio/issues/2371) „Save As Excel“ (Speichern als Excel-Datei) speichert nur erste Zeile der Daten.
- [Issue #2150](https://github.com/Microsoft/azuredatastudio/issues/2150): Unter Ubuntu 16.04 kann in einem Container keine Verbindung mit SQL hergestellt werden.

## <a name="july-2018"></a>Juli 2018

19. Juli 2018 &nbsp; / &nbsp; Version: 0.31.4 &nbsp; / &nbsp; Public Preview

In der *Public Preview, die im Juli veröffentlicht wurde*, wurde der Schwerpunkt auf folgende Punkte gelegt:

- Erstrelease der SQL Server-Agent-Konfigurationsszenarios
- Verbesserungen an Sitzungs- und Ansichtsvorlagen von SQL Server Profiler
- Weitere Fehlerbehebungen für GitHub-Issues, die von Kunden gemeldet wurden

&nbsp;

| Change | Details |
| :----- | :------ |
| Verbesserungen an der [SQL Server-Agent-Erweiterung für SQL Operations Studio](./extensions/sql-server-agent-extension.md) wurden vorgenommen. | Ansicht für Warnungen, Operatoren und Proxys sowie Symbole im linken Bereich wurde hinzugefügt.<br/><br/>Dialogfelder für „Neuer Auftrag“, „Neuer Auftragsschritt“, „Neue Warnung“ und „Neuer Operator“ wurden hinzugefügt.<br/><br/>„Auftrag löschen“, „Warnung löschen“ und „Delete Operator“ (Operator löschen) wurden hinzugefügt und sind über einen Rechtsklick verfügbar.<br/><br/>Visualisierung für „Previous Runs“ (Vorherige Ausführungen) wurde hinzugefügt.<br/><br/>Den einzelnen Spaltennamen wurden Filter hinzugefügt. |
| Verbesserungen an der [SQL Server Profiler-Erweiterung für SQL Operations Studio](./extensions/sql-server-profiler-extension.md) wurden vorgenommen. | Fünf Standardvorlagen zum Anzeigen erweiterter Ereignisse wurden hinzugefügt.<br/><br/>Der Name des Servers/der Datenbankverbindung wurde hinzugefügt.<br/><br/>Azure SQL-Datenbank-Instanzen werden nun unterstützt.<br/><br/>Wenn die Registerkarte geschlossen und der Profiler immer noch ausgeführt wird, wird nun vorgeschlagen, den Profiler zu beenden. |
| Die Combine Scripts-Erweiterung wurde veröffentlicht. | &nbsp; |
| Erweiterbarkeitspunkte für Assistenten und Dialogfelder wurden für Ersteller von Erweiterungen hinzugefügt. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-july-2018"></a>Fehlerbehebungen im Juli 2018

- Behebung von [Issue #728](https://github.com/Microsoft/azuredatastudio/issues/728): „Verbindung hinzufügen“ reagiert nicht unter macOS.
- Behebung von [Issue #1612](https://github.com/Microsoft/azuredatastudio/issues/1612): Der im Ergebnisraster angezeigte Text wird aufgrund internationaler Zeichen falsch dargestellt.
- Behebung von [Issue #1693](https://github.com/Microsoft/azuredatastudio/issues/1693): Dialogfeld „Sicherung“: Die Benutzeroberfläche des Dateibrowsers wird nicht richtig dargestellt.
- Behebung von [Issue #1713](https://github.com/Microsoft/azuredatastudio/issues/1713): Anzahl betroffener Zeilen.
- Behebung von [Issue #1718](https://github.com/Microsoft/azuredatastudio/issues/1718): Es kann keine Verbindung mit einer Datenquelle hergestellt werden.
- Behebung von [Issue #1719](https://github.com/Microsoft/azuredatastudio/issues/1719): Beim Herstellen einer Verbindung mit dem Server tritt ein Typfehler (TypeError) auf.
- Behebung von [Issue #1724](https://github.com/Microsoft/azuredatastudio/issues/1724): Die Erweiterungsdialogfelder funktionieren nicht mehr.
- Behebung von [Issue #1749](https://github.com/Microsoft/azuredatastudio/issues/1749): Fehler: HTML-Daten in einer Spalte werden ausgewertet.
- Behebung von [Issue #1789](https://github.com/Microsoft/azuredatastudio/issues/1789): Erweiterbarkeit: Ein hinzugefügter Verbindungsanbieter wird bei der Deinstallation nicht aus der Liste entfernt.
- Behebung von [Issue #1791](https://github.com/Microsoft/azuredatastudio/issues/1791): Erweiterungen für SQL Operations Studio: „queryeditor.connect()“ stellt eine Verbindung mit der Zieldatenbank her; die Verbindung wird allerdings nicht auf der Benutzeroberfläche angezeigt.
- Behebung von [Issue #1799](https://github.com/Microsoft/azuredatastudio/issues/1799): Das Diagramm „Top 10 DB Size“ (Größe der zehn am häufigsten verwendeten Datenbanken) wird nicht für Instanzen angezeigt, bei denen die Groß-/Kleinschreibung beachtet wird.
- Behebung von [Issue #1814](https://github.com/Microsoft/azuredatastudio/issues/1814): Tippfehler in „sqlops.d.ts“ verursacht implizite „any“-Typdefinition.
- Behebung von [Issue #1817](https://github.com/Microsoft/azuredatastudio/issues/1817): Rechtschreibfehler.
- Behebung von [Issue #1830](https://github.com/Microsoft/azuredatastudio/issues/1830): Wenn nach dem Aufruf von „component()“ „iconPath“ in „ButtonComponent“ festgelegt wird, wird das Symbol nicht angepasst.
- Behebung von [Issue #1843](https://github.com/Microsoft/azuredatastudio/issues/1843): Verbesserte Tabellenstrukturierung.

## <a name="june-2018"></a>Juni 2018

20. Juni 2018 &nbsp; / &nbsp; Version: 0.30.6 &nbsp; / &nbsp; Public Preview

&nbsp;

| Change | Details |
| :----- | :------ |
| Das Erstrelease der Erweiterung **SQL Server Profiler für SQL Operations Studio (_Vorschauversion_)** wurde veröffentlicht. | &nbsp; |
| Die neue **SQL Data Warehouse**-Erweiterung enthält umfangreiche und anpassbare Dashboardwidgets, die Erkenntnisse zu Ihrem Data Warehouse enthalten. | Aus diesen Informationen können Sie wesentliche Verwaltungs- und Optimierungsszenarios für Ihr Data Warehouse ableiten, die dauerhaft dessen Leistungsfähigkeit sicherstellen. |
| **Filter- und Sortierfunktionen für „Daten bearbeiten“** werden nun unterstützt. | &nbsp; |
| Verbesserungen an den Ansichten „Aufträge“ und „Auftragsverlauf“ in der Erweiterung **SQL Server-Agent für SQL Operations Studio (_Vorschauversion_)** wurden vorgenommen. | &nbsp; |
| Die Erweiterbarkeits-APIs für das **Framework des Benutzeroberflächen-Generators zur Erstellung von Assistenten und Dialogfeldern** wurden verbessert. | &nbsp; |
| Der Quellcode von VS Code wurde verbessert. | Die folgenden Releases wurden integriert:<br/>&bull; &nbsp; [März 2018 (1.22)](https://code.visualstudio.com/updates/v1_22)<br/>&bull; &nbsp; [April 2018 (1.23)](https://code.visualstudio.com/updates/v1_23) |
| &nbsp; | &nbsp; |

### <a name="github-issues-fixes-june-2018"></a>Behebungen von GitHub-Issues im Juni 2018

- Featurevorschlag ([Issue #1204](https://github.com/Microsoft/azuredatastudio/issues/1204)): Im Ergebnisraster sollte die Spaltenbreite automatisch je nach Datenmenge angepasst werden. Außerdem sollten manuelle Änderungen beibehalten werden, wenn dieselbe Abfrage mehrfach ausgeführt wird.
- Behebung von [Issue #1398](https://github.com/Microsoft/azuredatastudio/issues/1398): Wenn das verknüpfte Konto leer ist, sollten eine Meldung und eine Schaltfläche zum Hinzufügen eines Kontos angezeigt werden.
- Behebung von [Issue #1399](https://github.com/Microsoft/azuredatastudio/issues/1399): Wenn die Ansicht reduziert wird, wird die Registerkarte für das verknüpfte Konto nicht mehr richtig angezeigt.
- Behebung von [Issue #1374](https://github.com/Microsoft/azuredatastudio/issues/1374): SQL Tools Service stürzt ab, wenn die SQL-Datei auf dem Datenträger geöffnet wird.
- Behebung von [Issue #1372](https://github.com/Microsoft/azuredatastudio/issues/1372): Das SQL-Schlüsselwort BETWEEN fehlt.
- Behebung von [Issue #1395](https://github.com/Microsoft/azuredatastudio/issues/1395): SQL Tools Service stürzt ab, wenn das Schlüsselwort MATCH verwendet wird.
- Behebung von [Issue #1496](https://github.com/Microsoft/azuredatastudio/issues/1496): Bei einem Klick auf die Kontextmenüoption „New Profiler“ (Neuer Profiler) wird keine Aktion ausgeführt.
- Behebung von [Issue #1495](https://github.com/Microsoft/azuredatastudio/issues/1495): Der Abfrageplan „Erläutern“ im Abfrage-Editor funktioniert nicht.

## <a name="may-2018"></a>Mai 2018

7\. Mai 2018 &nbsp; / &nbsp; Version: 0.29.3 &nbsp; / &nbsp; Public Preview

In der *Public Preview, die im Mai veröffentlicht wurde*, wurden die Schwerpunkte auf Stabilität und auf Fehlerbehebungen gelegt:

&nbsp;

| Change | Details |
| :----- | :------ |
| Ankündigung: Die Erweiterung Redgate SQL Search ist im Erweiterungs-Manager verfügbar. | &nbsp; |
| Von der Community lokalisierte Inhalte sind in zehn Sprachen verfügbar. | Deutsch, Spanisch, Französisch, Italienisch, Japanisch, Koreanisch, Portugiesisch, Russisch, Chinesisch (vereinfacht) und Chinesisch (traditionell). |
| Änderungen an der Telemetriedatenerfassung wurden vorgenommen. | &bull; &nbsp; Weniger Telemetriedaten werden erfasst.<br/>&bull; &nbsp; Die Widerrufsfunktion wurde verbessert.<br/>&bull; &nbsp; Produktinterne Links zu Datenschutzbestimmungen wurden hinzugefügt. |
| Der Marketplace wurde mithilfe des Erweiterungs-Managers verbessert. | Communityerweiterungen lassen sich leichter finden. |
| SQL Server-Agent-Erweiterung. | &bull; &nbsp; Aufträge<br/>&bull; &nbsp; Verbesserung an der Ansicht „Auftragsverlauf“ |
| Updates für die Erweiterungen whoisactive und Server Reports sind verfügbar. | &nbsp; |
| Die Scrollfunktion für „Manage Dashboard Properties“ (Dashboardeigenschaften verwalten) wurde verbessert. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="fix-github-issues"></a>Behebungen von GitHub-Issues

- Behebung von [Issue #703](https://github.com/Microsoft/azuredatastudio/issues/703): Wenn Texte mit HTML-Auszeichnungen in „Daten bearbeiten“ eingegeben werden, werden die Werte bis zur nächsten Aktualisierung falsch dargestellt.
- Behebung von [Issue #821](https://github.com/Microsoft/azuredatastudio/issues/821): „azuredatastudio.deb“-Paketabhängigkeit.
- Behebung von [Issue #1260](https://github.com/Microsoft/azuredatastudio/issues/1260): Das Schlüsselwort „distinct“ wird nicht hervorgehoben.
- Behebung von [Issue #1332](https://github.com/Microsoft/azuredatastudio/issues/1332): Zeilen können in „Daten bearbeiten“ nicht wiederhergestellt werden.
- Behebung von [Issue #1215](https://github.com/Microsoft/azuredatastudio/issues/1215): SQL Server-Agent-Erweiterung und Statusleiste.
- Behebung von [Issue #1316](https://github.com/Microsoft/azuredatastudio/issues/1316): Die Größe der SQL Server-Agent-Ansicht wird nicht geändert, wenn sich die Fenstergröße ändert.

## <a name="april-2018"></a>April 2018

25. April 2018 &nbsp; / &nbsp; Version: 0.28.6 &nbsp; / &nbsp; Public Preview

Die im *April veröffentlichte Public Preview* enthält Fehlerbehebungen und Verbesserungen.

&nbsp;

| Change | Details |
| :----- | :------ |
| Verbesserungen an der SQL Server-Agent-Erweiterung (Vorschauversion): | &nbsp; |
| &nbsp; &nbsp; &nbsp; Die Dateiunterstützung wurde verbessert. | &bull; &nbsp; Große Dateien werden unterstützt.<br/>&bull; &nbsp; Von Administratoren geschützte Dateien können nun gespeichert werden.<br/>&bull; &nbsp; Das Speichern von Dateien \>256 MB wird in SQL Operations Studio unterstützt. |
| &nbsp; &nbsp; &nbsp; Die Verwendung mehrerer integrierter Terminals ist nun möglich. | Sie können nun gleichzeitig mit mehreren geöffneten Terminals arbeiten. |
| &nbsp; &nbsp; &nbsp; Schnellere Installationen und kürzere Startzeiten sind nun möglich. | Geringerer Installationsumfang, da weniger Dateien auf dem Datenträger vorhanden sind. |
| &nbsp; | &nbsp; |

### <a name="fix-github-issues-april-2018"></a>Behebungen von GitHub-Issues im April 2018

- Behebung von [Issue #37](https://github.com/Microsoft/azuredatastudio/issues/37): Wenn der Diagramm-Viewer einen Fehler auslöst, tritt unerwartetes Verhalten auf.
- Behebung von [Issue #462](https://github.com/Microsoft/azuredatastudio/issues/462): Featurevorschlag: „Servergruppen“ sollte mithilfe einer zusätzlichen Option standardmäßig erweiterbar sein.
- Behebung von [Issue #606](https://github.com/Microsoft/azuredatastudio/issues/606): Falscher IntelliSense-Vorschlag für UPDATE-Befehl.
- Behebung von [Issue #967](https://github.com/Microsoft/azuredatastudio/issues/967): Wenn der XML-Showplan im Ergebnisraster ausgewählt wird, sollte der Abfrageplan geöffnet werden.
- Behebung von [Issue #1023](https://github.com/Microsoft/azuredatastudio/issues/1023): Dem „ms_foreachdb“-Aufruf des Beitragenden„flyfishingdba“ sollten eckige Klammern hinzufügt werden.
- Behebung von [Issue #1048](https://github.com/Microsoft/azuredatastudio/issues/1048): Fehler bei SSL/TLS-Handshake vor der Anmeldung.
- Behebung von [Issue #1050](https://github.com/Microsoft/azuredatastudio/issues/1050): Ansicht für Erkenntnisse sollte zurückgesetzt werden, bevor ein Fehler angezeigt wird.
- Behebung von [Issue #1057](https://github.com/Microsoft/azuredatastudio/issues/1057): Die Aktionen „Wiederherstellen“ und „Neue Abfrage“ des Explorer-Widgets funktionieren nicht.
- Behebung von [Issue #1068](https://github.com/Microsoft/azuredatastudio/issues/1068): Das Dashboardausgabefenster wird geöffnet und enthält eine Fehlermeldung, die Azure SQL-Datenbank betrifft.
- Behebung von [Issue #1069](https://github.com/Microsoft/azuredatastudio/issues/1069): Kurz nach der Anzeige des Dialogfelds „Verbindung“ wird die Fehlermeldung „Server is required.“ (Der Server muss angegeben werden.) angezeigt.
- Behebung von [Issue #1070](https://github.com/Microsoft/azuredatastudio/issues/1070): „Servergruppen“ lässt sich nur noch durch einen Doppelklick erweitern.
- Behebung von [Issue #1072](https://github.com/Microsoft/azuredatastudio/issues/1072): Der Hintergrund des Dropdownmenüs ist transparent.
- Behebung von [Issue #1115](https://github.com/Microsoft/azuredatastudio/issues/1115): Behebung aller Probleme mit Bedienungshilfen im Zusammenhang mit hohen Kontrasten in SQL Operations Studio.
- Behebung von [Issue #1101](https://github.com/Microsoft/azuredatastudio/issues/1101): Der Link „Download Manually“ (Manuell herunterladen), der beim Fehlschlagen eines Erweiterungsupgrades angezeigt wird, verweist auf den falschen Speicherort.
- Behebung von [Issue #1103](https://github.com/Microsoft/azuredatastudio/issues/1103): Auf der Registerkarte „Home“ (Startseite) kann nicht nach oben oder unten gescrollt werden.
- Behebung von [Issue #1104](https://github.com/Microsoft/azuredatastudio/issues/1104): Die Registerkarten für SQL-Erweiterungen können nicht mehr verwendet werden.

### <a name="visual-studio-code-121-platform"></a>Visual Studio Code 1.21

Besonders an der im April veröffentlichten Public Preview hervorzuheben ist die Aktualisierung des Quellcodes für Visual Studio Code 1.21. Neu im Vergleich zu Version 1.19 sind mehrere Updates des Kern-Editors und der Workbench. Einige Beispiele für Änderungen sind in der folgenden Liste aufgeführt:

&nbsp;

| Change | Details |
| :----- | :------ |
| [Neue Benutzeroberfläche für Benachrichtigungen](https://code.visualstudio.com/updates/v1_21#_new-notifications-ui). | Sie können nun Benachrichtigungen in SQL Operations Studio einfach verwalten und analysieren. |
| [Verwendung mehrerer integrierter Terminals](https://code.visualstudio.com/updates/v1_21#_split-terminals). | Sie können nun mit mehreren Terminals gleichzeitig arbeiten. |
| [Speichern von großen und geschützten Dateien](https://code.visualstudio.com/updates/v1_20#_save-files-that-need-admin-privileges). | Sie können nun Dateien, die von Administratoren geschützt wurden und 256 MB überschreiten, in SQL Operations Studio speichern. |
| [Verbesserte Unterstützung großer Dateien](https://code.visualstudio.com/updates/v1_21#_text-buffer-improvements). | Der Textpuffer wurde für große Dateien optimiert. |
| [Verbesserte Suche nach Einstellungen](https://code.visualstudio.com/updates/v1_20#_settings-search). | Sie können nun mithilfe natürlicher Sprache leicht die richtigen Einstellungen finden. |
| [Globale Codeausschnitte](https://code.visualstudio.com/updates/v1_20#_global-snippets). | Sie können nun Codeausschnitte für alle Dateitypen erstellen. |
| [Mehrfachauswahl im Explorer](https://code.visualstudio.com/updates/v1_20#_multi-select-in-the-explorer). | Sie können nun Aktionen für mehrere Dateien gleichzeitig ausführen. |
| [Fehler und Warnungen im Explorer](https://code.visualstudio.com/updates/v1_20#_error-indicators-in-the-explorer). | Sie können nun schnell zu Fehlern in der Codebasis navigieren. |
| [Fensterübergreifendes Drag & Drop sowie Kopieren und Einfügen](https://code.visualstudio.com/updates/v1_21#_better-drag-and-drop-support). | Sie können nun Dateien zwischen geöffneten Fenstern in SQL Operations Studio verschieben. |
| [Unterstützung von Git-Submodulen](https://code.visualstudio.com/updates/v1_20#_git-submodules). | Sie können nun Git-Vorgänge für geschachtelte Git-Repositorys ausführen. |
| [Unterstützung der Sprachausgabe für Terminals](https://code.visualstudio.com/updates/v1_20#_screen-reader-support). | Für integrierte Terminals ist nun ein Modus verfügbar, der für **Sprachausgaben optimiert** ist. |
| [Zentriertes Editor-Layout](https://code.visualstudio.com/updates/v1_21#_centered-editor-layout). | Sie können nun die Codeansicht maximieren. |
| [Horizontale Anzeige von Suchergebnissen (Vorschauversion)](https://code.visualstudio.com/updates/v1_21#_horizontal-search). | Sie können sich nun Suchergebnisse in einem horizontal angeordneten Bereich anzeigen lassen. |
| &nbsp; | &nbsp; |

Zusätzliche Details finden Sie in den [Versionshinweisen vom Februar](https://code.visualstudio.com/updates/v1_21) sowie in den [Versionshinweisen zu Visual Studio Code vom Januar](https://code.visualstudio.com/updates/v1_20).

Weitere Informationen sind im [Änderungsprotokoll](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md) verfügbar.

## <a name="march-2018"></a>März 2018

28. März 2018 &nbsp; / &nbsp; Version: 0.27.3 &nbsp; / &nbsp; Public Preview

In der *Public Preview, die im März veröffentlicht wurde*, wurden erneut die wichtigsten GitHub-Issues behoben. Außerdem wurde die Erweiterbarkeit verbessert. Im Vordergrund standen die Aktivierung des Erweiterungs-Managers, die Verbesserung der Dashboardverwaltung und die Bereitstellung der SQL Server-Agent-Erweiterung sowie der Erweiterung für Erkenntnisse. Dieses Release enthält die folgenden Erweiterungen und Verbesserungen:

&nbsp;

| Change | Details |
| :----- | :------ |
| Das Dashboarderweiterbarkeitsmodell wurde zur Unterstützung von Bereichen im Registerkartenformat erweitert, die Informationen zu Erkenntnissen und Konfigurationen enthalten. | Der Erweiterungs-Manager vereinfacht die Suche nach und den Zugriff auf Erweiterungen.<br/><br/>Dashboarderweiterungen für „sp\_whoisactive“ auf [whoisactive.com](http://www.whoisactive.com).<br/><br/>Weitere Informationen finden Sie unter [Erweitern der Funktionalität von SQL Operations Studio](./extensions/add-extensions.md). |
| Zusätzliche [Erweiterbarkeits-APIs für die Verbindungsverwaltung und für den Objekt-Explorer](https://github.com/Microsoft/azuredatastudio/wiki/Extensibility-API) sind verfügbar. | &nbsp; |
| Es wurden erneut wichtige [GitHub-Issues](https://github.com/Microsoft/azuredatastudio/issues) behoben, die die Arbeit von Kunden stark beeinträchtigten. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="february-2018"></a>Februar 2018

15 Februar 2018 &nbsp; / &nbsp; Version: 0.26.7 &nbsp; / &nbsp; Public Preview

Die im *Februar veröffentlichte Public Preview* enthält mehrere Featurevorschläge und Behebungen von Fehlern mit hoher Priorität. Dieses Release enthält die folgenden Erweiterungen und Verbesserungen:

&nbsp;

| Change | Details |
| :----- | :------ |
| Das Feature für die automatische Installation von Updates wurde eingeführt. Dieses zeigt eine Benachrichtigung an, wenn ein neues Release heruntergeladen werden kann. | &nbsp; |
| Das Feld **Database** (Datenbank) im Dialogfeld „Verbindung“ ist nun eine Dropdownliste. Diese wird dynamisch mit den Namen der Datenbanken aufgefüllt, die sich auf dem angegebenen Server befinden. | &nbsp; |
| Einführung der Erweiterbarkeits-API für Verbindungen. | &nbsp; |
| Integration des VS Code-Editors (Version 1.19). | &nbsp; |
| Die html-query-plan-Komponente des Beitragenden „JustinPealing“ wurde aktualisiert und enthält nun mehrere Verbesserungen, die am Abfrageplan-Viewer vorgenommen wurden. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="fixed-issues-february-2018"></a>Behobene Issues im Februar 2018

- Behebung von [Issue #6](https://github.com/Microsoft/azuredatastudio/issues/6): Die Verbindungsdetails und die ausgewählte Datenbank sollten auf neuen Abfrageregisterkarten übernommen werden.
- Behebung von [Issue #22](https://github.com/Microsoft/azuredatastudio/issues/22): Können die Textfelder „Servername“ und „Datenbankname“ in Dropdownfelder umgewandelt werden?
- Behebung von [Issue #549](https://github.com/Microsoft/azuredatastudio/issues/549): Wenn die Installation mit den Optionen SILENT/VERYSILENT durchgeführt wird, öffnet sich die Anwendung nach der Installation automatisch.
- Behebung von [Issue #481](https://github.com/Microsoft/azuredatastudio/issues/481): Eine Option mit der Bezeichnung „Nach Updates suchen“ sollte hinzugefügt werden.
- Fehlerbehebungen für farbliche Hervorhebungen und Autovervollständigung im SQL-Editor:
  - Behebung von [Issue #584](https://github.com/Microsoft/azuredatastudio/issues/584): Das Schlüsselwort FULL wird von IntelliSense nicht hervorgehoben.
  - Behebung von [Issue #345](https://github.com/Microsoft/azuredatastudio/issues/345): SQL-Funktionen im Editor sollten farblich hervorgehoben werden.
  - Behebung von [Issue #300](https://github.com/Microsoft/azuredatastudio/issues/300): [#tempData] – letzte eckige Klammer „]“ wird grün angezeigt.
  - Behebung von [Issue #225](https://github.com/Microsoft/azuredatastudio/issues/225): Bestimmte Schlüsselwörter werden farblich falsch hervorgehoben.
  - Behebung von [Issue #60](https://github.com/Microsoft/azuredatastudio/issues/60): Fehlerhafte farbliche Hervorhebung für SQL-Syntax bei der Verwendung temporärer Tabellen in FROM-Klauseln.

## <a name="january-2018"></a>Januar 2018

17. Januar 2018 &nbsp; / &nbsp; Version: 0.25.4 &nbsp; / &nbsp; Public Preview

Die im *Januar veröffentlichte Public Preview* enthält mehrere Featurevorschläge und Behebungen von Fehlern mit hoher Priorität. Dieses Release enthält die folgenden Erweiterungen und Verbesserungen:

&nbsp;

| Change | Details |
| :----- | :------ |
| Gespeicherte Serververbindungen sind im Dialogfeld „Verbindung“ verfügbar. | &nbsp; |
| Aktivierung von Hot Exit. Hot Exit ist standardmäßig deaktiviert. Informationen zur Aktivierung finden Sie unter [Hot Exit](settings.md#hot-exit). | &nbsp; |
| Anpassung der Registerkartenfarben je nach Servergruppe. Die Anpassung der Registerkartenfarben ist standardmäßig deaktiviert. Informationen zur Aktivierung finden Sie unter [Registerkartenfarbe](settings.md#tab-color). | &nbsp; |
| *Servername* wurde im Dialogfeld „Verbindung“ in *Server* geändert. | &nbsp; |
| Fehlerbehebung für Befehl *Run Current Query* (Aktuelle Abfrage ausführen). | &nbsp; |
| Behebung von Skriptfehler, der zur Deaktivierung von Drag & Drop führte. | &nbsp; |
| Fehlerbehebung für falsch angeheftetes Startmenüsymbol. | &nbsp; |
| Fehlerbehebung für fehlendes Brandingsymbol bei Azure-Konto. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="december-2017"></a>Dezember 2017

19. Dezember 2017 &nbsp; / &nbsp; Version: 0.24.1 &nbsp; / &nbsp; Public Preview

Die im *Dezember veröffentlichte Public Preview* enthält mehrere Fehlerbehebungen für alle Features sowie die folgenden Verbesserungen und Erweiterungen:

&nbsp;

| Change | Details |
| :----- | :------ |
| Das Dialogfeld „Firewallregel erstellen“ unterstützt nun den Benutzer dabei, eine Verbindung mit Azure SQL-Datenbank und Azure Synapse Analytics herzustellen. | &nbsp; |
| Ein Windows Setup sowie DEB- und RPM-Installationspakete für Linux wurden hinzugefügt. | &nbsp; |
| Layout-Editor für „Manage Dashboard“ (Dashboard verwalten). | &nbsp; |
| Befehle *Script As Alter* (ALTER-Skript) und *Script As Execute* (EXECUTE-Skript). | &nbsp; |
| Befehl *Run Current Query with Actual Plan* (Aktuelle Abfrage mit tatsächlichem Plan ausführen). | &nbsp; |
| Integration des VS Code-Editors (Version 1.18.1). | &nbsp; |
| Aktivierung des Querladens für VSIX-Erweiterungsdateien. | &nbsp; |
| Unterstützung der Batchiterationssyntax „GO N“. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="november-2017"></a>November 2017

15. November 2017 &nbsp; / &nbsp; Version: 0.23.6

- Erstes Release von Azure Data Studio

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Schnellstarts finden Sie Hinweise zu den ersten Schritten:

- [Herstellen einer Verbindung mit und Abfragen von SQL Server](quickstart-sql-server.md)
- [Herstellen einer Verbindung mit und Abfragen von Azure SQL-Datenbank](quickstart-sql-database.md)
- [Herstellen einer Verbindung mit und Abfragen von Azure Synapse Analytics](quickstart-sql-dw.md)

Wirken Sie an Azure Data Studio mit:

- [https://github.com/Microsoft/azuredatastudio](https://github.com/Microsoft/azuredatastudio)
