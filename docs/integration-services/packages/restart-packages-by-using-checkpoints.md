---
description: Neustarten von Paketen mit Prüfpunkten
title: Neustarten von Paketen mit Prüfpunkten | Microsoft-Dokumentation
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- checkpoints [Integration Services]
- restarting packages
- starting packages
ms.assetid: 48f2fbb7-8964-484a-8311-5126cf594bfb
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8279636861b3d4048951d46e725c8d89c4e23fb0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346874"
---
# <a name="restart-packages-by-using-checkpoints"></a>Neustarten von Paketen mit Prüfpunkten

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] können fehlerhafte Pakete an dem Punkt neu gestartet werden, an dem der Fehler aufgetreten ist. Sie brauchen also nicht noch einmal vollständig ausgeführt werden. Wenn ein Paket zum Verwenden von Prüfpunkten konfiguriert ist, werden Informationen zur Ausführung des Pakets in eine Prüfpunktdatei geschrieben. Wenn das fehlerhafte Paket erneut ausgeführt wird, wird die Prüfpunktdatei verwendet, um das Paket von dem Punkt aus, an dem der Fehler aufgetreten ist, auszuführen. Wenn das Paket erfolgreich ausgeführt wird, wird die Prüfpunktdatei gelöscht und beim nächsten Ausführen des Pakets neu erstellt.  
  
 Das Verwenden von Prüfpunkten in einem Paket bietet die folgenden Vorteile:  
  
-   Vermeiden von wiederholten Download- und Uploadvorgängen für umfangreiche Dateien. Beispielsweise kann ein Paket, das mehrere umfangreiche Dateien mithilfe je eines FTP-Tasks herunterlädt, nach dem Herunterladen einer einzigen fehlerhaften Datei neu gestartet werden und anschließend nur diese eine Datei erneut herunterladen.  
  
-   Vermeiden, dass große Datenmengen wiederholt geladen werden müssen. Beispielsweise kann ein Paket, das mithilfe von einzelnen Masseneinfügungstasks Masseneinfügungen in Dimensionstabellen in einem Data Warehouse ausführt, neu gestartet werden, wenn eine Einfügung für eine einzige Dimensionstabelle einen Fehler auslöst. Anschließend braucht nur die fehlerhafte Dimension erneut geladen werden.  
  
-   Vermeiden wiederholter Wertaggregationen. Beispielsweise kann ein Paket, das mithilfe von einzelnen Datenflusstasks zahlreiche verschiedene Aggregate berechnet, wie z. B. Mittelwerte oder Summen, neu gestartet werden, nachdem die Berechnung einer einzigen Aggregation einen Fehler auslöst. Anschließend braucht nur die fehlerhafte Aggregation neu berechnet werden.  
  
 Wenn ein Paket für das Verwenden von Prüfpunkten konfiguriert ist, zeichnet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] den Punkt, an dem neu gestartet werden soll, in der Prüfpunktdatei auf. Der in der Prüfpunktdatei aufgezeichnete Punkt, an dem neu gestartet werden soll, ist vom Typ des fehlerhaften Containers und der Implementierung von Funktionen wie z. B. Transaktionen abhängig. Die aktuellen Werte von Variablen werden ebenfalls in der Prüfpunktdatei erfasst. Die Werte von Variablen mit dem **Object** -Datentyp werden jedoch nicht in Prüfpunktdateien gespeichert.  
  
## <a name="defining-restart-points"></a>Definieren von Prüfpunkten, an denen neu gestartet wird  
 Die kleinste atomare Arbeitseinheit, die neu gestartet werden kann, ist der Taskhostcontainer, der einen einzelnen Task kapselt. Der Foreach-Schleifencontainer und der Transaktionscontainer werden ebenfalls als atomare Arbeitseinheiten behandelt.  
  
 Wenn ein Paket während des Ausführens eines Transaktionscontainers beendet wird, wird auch die Transaktion beendet, und für alle eventuell durch den Container ausgeführten Vorgänge wird ein Rollback ausgeführt. Beim Neustarten des Pakets wird der fehlerhafte Container erneut ausgeführt. Das Abschließen eventuell vorhandener untergeordneter Container in Transaktionscontainern wird nicht in der Prüfpunktdatei aufgezeichnet. Daher werden beim Neustarten des Pakets sowohl der Transaktionscontainer als auch seine untergeordneten Container erneut ausgeführt.  
  
> [!NOTE]  
>  Das Verwenden von Prüfpunkten und Transaktionen im gleichen Paket könnte unerwartete Ergebnisse verursachen. Wenn beispielsweise ein Paket einen Fehler verursacht und von einem Prüfpunkt neu startet, wiederholt das Paket möglicherweise eine Transaktion, die bereits erfolgreich ausgeführt wurde.  
  
 Für For- und Foreach-Schleifencontainer werden keine Prüfpunktdaten gespeichert. Beim Neustart eines Pakets werden sowohl For- und Foreach-Schleifencontainer als auch deren untergeordnete Container erneut ausgeführt. Wenn ein untergeordneter Container einer Schleife erfolgreich ausgeführt wurde, wird er nicht in der Prüfpunktdatei aufgezeichnet, sondern erneut ausgeführt. Weitere Informationen und eine Umgehungslösung finden Sie unter [SSIS-Prüfpunkte werden bei Elementen von For- und Foreach-Schleifencontainern nicht berücksichtigt](/troubleshoot/sql/integration-services/ssis-checkpoints-not-honored-for-loop).  
  
 Beim Neustarten des Pakets werden die Paketkonfigurationen nicht erneut geladen; stattdessen verwendet das Paket die Konfigurationsinformationen der Prüfpunktdatei. Auf diese Weise wird sichergestellt, dass das Paket beim erneuten Ausführen wie beim ursprünglichen (fehlerhaften) Ausführen dieselben Konfigurationen verwendet.  
  
 Ein Paket kann nur auf der Ablaufsteuerungsebene neu gestartet werden. Sie können ein Paket also nicht mitten in einem Datenfluss neu starten. Um zu vermeiden, dass der gesamte Datenfluss erneut ausgeführt werden muss, können Sie beim Entwerfen des Pakets mehrere Datenflüsse planen, die jeweils einen bestimmten Datenflusstask verwenden. Auf diese Weise kann das Paket neu gestartet werden und dabei nur einen Datenflusstask erneut ausführen.  
  
## <a name="configuring-a-package-to-restart"></a>Konfigurieren des Neustarts eines Pakets  
 Die Prüfpunktdatei enthält das Ausführungsergebnis aller abgeschlossenen Container, die aktuellen Werte der benutzerdefinierten und der Systemvariablen, sowie Paketkonfigurationsinformationen. Die Datei enthält außerdem den eindeutigen Bezeichner des Pakets. Der Paketbezeichner in der Prüfpunktdatei muss mit dem des Pakets übereinstimmen, damit das Paket neu gestartet werden kann. Anderenfalls tritt beim Neustarten ein Fehler auf. Auf diese Weise wird vermieden, dass ein Paket eine Prüfpunktdatei verwendet, die von einer anderen Paketversion geschrieben wurde. Wenn das Paket nach dem Neustarten erfolgreich ausgeführt wird, wird die Prüfpunktdatei gelöscht.  
  
 In der folgenden Tabelle sind die Paketeigenschaften aufgeführt, die Sie zum Implementieren von Prüfpunkten festlegen können.  
  
|Eigenschaft|BESCHREIBUNG|  
|--------------|-----------------|  
|CheckpointFileName|Gibt den Namen der Prüfpunktdatei an.|  
|CheckpointUsage|Gibt an, ob Prüfpunkte verwendet werden.|  
|SaveCheckpoints|Gibt an, ob das Paket Prüfpunkte speichert. Diese Eigenschaft muss auf True festgelegt sein, damit ein Paket an dem Punkt neu gestartet wird, an dem ein Fehler aufgetreten ist.|  
  
 Außerdem müssen Sie für alle Container des Pakets, die das Paket nach einem Fehler neu starten sollen, die FailPackageOnFailure-Eigenschaft auf **TRUE** festlegen.  
  
 Mit der ForceExecutionResult-Eigenschaft können Sie die Verwendung der Prüfpunkte eines Pakets testen. Sie können einen Echtzeitfehler imitieren, indem Sie die ForceExecutionResult-Eigenschaft eines Tasks oder eines Containers auf Failure festlegen. Wenn Sie das Paket erneut ausführen, werden der fehlerhafte Task bzw. die fehlerhaften Container erneut ausgeführt.  
  
### <a name="checkpoint-usage"></a>Syntax von Prüfpunkten  
 Die CheckpointUsage-Eigenschaft kann auf die folgenden Werte festgelegt werden:  
  
|Wert|BESCHREIBUNG|  
|-----------|-----------------|  
|**Never**|Gibt an, dass die Prüfpunktdatei nicht verwendet wird und dass das Paket vom Beginn des Paketworkflows aus ausgeführt wird.|  
|**Always**|Gibt an, dass die Prüfpunktdatei immer verwendet wird und dass das Paket von dem Punkt aus neu gestartet wird, an dem bei der letzten Ausführung ein Fehler aufgetreten ist. Wenn die Prüfpunktdatei nicht gefunden wird, schlägt das Paket fehl.|  
|**IfExists**|Gibt an, dass die Prüfpunktdatei verwendet wird, falls sie vorhanden ist. Wenn die Prüfpunktdatei vorhanden ist, wird das Paket an dem Punkt neu gestartet, an dem bei der letzten Ausführung ein Fehler aufgetreten ist; anderenfalls wird das Paket vom Beginn des Paketworkflows aus ausgeführt.|  
  
> [!NOTE]  
>  Das Festlegen der Prüfpunktausführungsoption **/CheckPointing** von dtexec auf „on“ entspricht der **SaveCheckpoints** -Eigenschaft des Pakets **TRUE** oder der **CheckpointUsage** -Eigenschaft „Immer“. Weitere Informationen finden Sie [hier](../../integration-services/packages/dtexec-utility.md).  
  
## <a name="securing-checkpoint-files"></a>Sichern von Prüfpunktdateien  
 Der Schutz auf Paketebene schließt nicht den Schutz von Prüfpunktdateien ein. Daher müssen diese Dateien separat gesichert werden. Prüfpunktdaten können nur im Dateisystem gespeichert werden. Sie sollten daher eine Zugriffssteuerungsliste (ACL, Access Control List) des Betriebssystems verwenden, um den Speicherort der Datei bzw. den Ordner, in dem die Datei gespeichert wird, zu sichern. Prüfpunktdateien sollten unbedingt gesichert werden, da sie Informationen zum Paketstatus enthalten, einschließlich der aktuellen Variablenwerte. Beispielsweise kann eine Variable ein Recordset mit mehreren Zeilen privater Daten, wie z. B. Telefonnummern, enthalten. Weitere Informationen finden Sie unter [Zugriff auf Dateien, die von Paketen verwendet werden](../../integration-services/security/security-overview-integration-services.md#files).  

## <a name="configure-checkpoints-for-restarting-a-failed-package"></a>Konfigurieren von Prüfpunkten zum erneuten Starten eines fehlerhaften Pakets
  Sie können ein [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Paket so konfigurieren, dass nicht das gesamte Paket erneut ausgeführt wird, sondern ab dem Punkt, an dem ein Fehler auftrat. Hierzu legen Sie die Eigenschaften fest, die für Prüfpunkte gelten.  
  
### <a name="to-configure-a-package-to-restart"></a>So konfigurieren Sie den Neustart eines Pakets  
  
1.  Öffnen Sie in [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]das [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] -Projekt mit dem Paket, das Sie konfigurieren möchten.  
  
2.  Doppelklicken Sie im **Projektmappen-Explorer** auf das Paket, um es zu öffnen.  
  
3.  Klicken Sie auf die Registerkarte **Ablaufsteuerung** .  
  
4.  Klicken Sie mit der rechten Maustaste an einer beliebigen Stelle im Hintergrund der Entwurfsoberfläche der Ablaufsteuerung, und klicken Sie anschließend auf **Eigenschaften**.  
  
5.  Legen Sie die SaveCheckpoints-Eigenschaft auf **TRUE** fest.  
  
6.  Geben Sie den Namen der Prüfpunktdatei in die CheckpointFileName-Eigenschaft ein.  
  
7.  Legen Sie die CheckpointUsage-Eigenschaft auf einen der beiden Werte fest:  
  
    -   Wählen Sie **Always** aus, damit das Paket immer am Prüfpunkt neu gestartet wird.  
  
        > [!IMPORTANT]  
        >  Falls die Prüfpunktdatei nicht verfügbar ist, tritt ein Fehler auf.  
  
    -   Wählen Sie **IfExists** aus, damit das Paket nur neu gestartet wird, wenn die Prüfpunktdatei verfügbar ist.  
  
8.  Konfigurieren Sie die Tasks und Container, von denen das Paket neu gestartet werden kann.  
  
    -   Klicken Sie mit der rechten Maustaste auf einen Task oder Container, und klicken Sie anschließend auf **Eigenschaften**.  
  
    -   Legen Sie die FailPackageOnFailure-Eigenschaft auf **TRUE** für alle ausgewählten Tasks und Container fest.  
    
## <a name="external-resources"></a>Externe Ressourcen  
  
-   Technischer Artikel [Automatischer Neustart von SSIS-Paketen nach Failover oder Fehler](https://go.microsoft.com/fwlink/?LinkId=200407)auf social.technet.microsoft.com.  
  
-   Support-Artikel [SSIS-Prüfpunkte werden bei Elementen von For- und Foreach-Schleifencontainern nicht berücksichtigt](/troubleshoot/sql/integration-services/ssis-checkpoints-not-honored-for-loop)auf support.microsoft.com.