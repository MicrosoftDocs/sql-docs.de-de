---
title: Datenermittlung und -klassifizierung in SqlClient
description: In diesem Artikel wird erläutert, wie Sie überprüfen, ob eine SQL Server-Datenbank die Datenklassifizierung unterstützt. Außerdem erfahren Sie, wie Sie über das SqlDataReader-Objekt auf Datenklassifizierungsinformationen zugreifen.
ms.date: 11/23/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: bac8c09b862e43549ade3a6d98e34fdc97596131
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835767"
---
# <a name="data-discovery-and-classification-in-sqlclient"></a>Datenermittlung und -klassifizierung in SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

Bei der [Datenermittlung und -klassifizierung](../../../relational-databases/security/sql-data-discovery-and-classification.md) handelt es sich um mehrere erweiterte Dienste für die Ermittlung, Klassifizierung, Bezeichnung und Berichterstellung für vertrauliche Daten in Ihren Datenbanken. SqlClient stellt eine API bereit, die schreibgeschützte Datenermittlungs- und Datenklassifizierungsinformationen verfügbar macht, wenn die zugrunde liegende Quelle das Feature unterstützt. Der Zugriff auf diese Informationen erfolgt über SqlDataReader.

Version 2.1.0 von Microsoft.Data.SqlClient bietet jetzt Unterstützung von Informationen zu `Sensitivity Rank` zur Datenklassifizierung. `Sensitivity Rank` ist ein Bezeichner, der auf vordefinierten Werten basiert, die die Vertraulichkeitsbewertung bestimmen. Er kann von anderen Diensten wie Advanced Threat Protection verwendet werden, um Anomalien basierend auf ihrer Bewertung zu erkennen. Die folgenden Datenklassifizierungs-APIs sind jetzt im Namespace Microsoft.Data.SqlClient.DataClassification verfügbar:

```csharp
// New in Microsoft.Data.SqlClient v2.1.0
public enum SensitivityRank
{
    NOT_DEFINED = -1,
    NONE = 0,
    LOW = 10,
    MEDIUM = 20,
    HIGH = 30,
    CRITICAL = 40
}

public sealed class SensitivityClassification
{
  // Returns the sensitivity rank for the query associated with the active 'SqlDataReader'.
  // New in Microsoft.Data.SqlClient v2.1.0
  public SensitivityRank SensitivityRank;

  // Returns the labels collection for this 'SensitivityClassification' Object
  public ReadOnlyCollection<Label> Labels;

  // Returns the information types collection for this 'SensitivityClassification' Object
  public ReadOnlyCollection<InformationType> InformationTypes;

  // Returns the column sensitivity for this 'SensitivityClassification' Object
  public ReadOnlyCollection<ColumnSensitivity> ColumnSensitivities;
}

public sealed class SensitivityProperty
{
  // Returns the sensitivity rank for this 'SensitivityProperty' Object
  // New in Microsoft.Data.SqlClient v2.1.0
  public SensitivityRank SensitivityRank;

  // Returns the label for this 'SensitivityProperty' Object
  public Label Label;

  // Returns the information type for this 'SensitivityProperty' Object
  public InformationType InformationType;
}

public sealed class Label
{
  // Gets the name for this 'Label' object
  public string Name;

  // Gets the ID for this 'Label' object
  public string Id;
}

public sealed class InformationType
{
  // Gets the name for this 'InformationType' object
  public string Name;

  // Gets the ID for this 'InformationType' object
  public string Id;
}

public sealed class ColumnSensitivity
{
  // Returns the list of sensitivity properties as received from Server for this 'ColumnSensitivity' information      
  public ReadOnlyCollection<SensitivityProperty> SensitivityProperties;
}
```

> [!NOTE]
> Microsoft.Data.SqlClient liest die Informationen von `Sensitivity Rank` nur, wenn SQL Server die Datenklassifizierung mit Bewertung unterstützt. Für Server, die die alte Version der Datenklassifizierung ohne Bewertung verwenden, lautet der Wert für Abfragen NICHT DEFINIERT.

Diese Beispielanwendung zeigt, wie auf die Datenklassifizierungseigenschaften von SqlDataReader zugegriffen wird.

[!code-csharp [SqlDataReader_DataDiscoveryAndClassification#1](~/../sqlclient/doc/samples/SqlDataReader_DataDiscoveryAndClassification.cs#1)]


**Weitere Informationen**  

 - [SQL Server-Features und ADO.NET](sql-server-features-adonet.md)
 - [sys.sensitivity_classifications (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md)
 - [ADD SENSITIVITY CLASSIFICATION](../../../t-sql/statements/add-sensitivity-classification-transact-sql.md)