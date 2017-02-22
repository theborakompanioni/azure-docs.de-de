---
title: "Anwendungsfall – Erstellen von Kundenprofilen"
description: Erfahren Sie, wie Azure Data Factory zum Erstellen eines datengesteuerten Workflows (Pipeline) verwendet wird, um Profile von Spielekunden zu erstellen.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: e07d55cf-8051-4203-9966-bdfa1035104b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: shlo
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: abc3fa751b3926dbbb9dc040abc40632e316e2a0


---
# <a name="use-case---customer-profiling"></a>Anwendungsfall – Erstellen von Kundenprofilen
Azure Data Factory ist einer der zahlreichen Dienste, die zum Implementieren der Cortana Intelligence Suite von Lösungsoptimierungen verwendet werden.  Weitere Informationen zu Cortana Intelligence finden Sie unter [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics). In diesem Dokument wird ein einfacher Anwendungsfall beschrieben, der Ihnen dabei helfen soll zu verstehen, wie Sie mit Azure Data Factory verbreitete Analyseprobleme lösen können.

## <a name="scenario"></a>Szenario
Contoso ist ein Spielehersteller, der Spiele für mehrere Plattformen entwickelt. Hierzu zählen Spielekonsolen, mobile Geräte und PCs. Wenn Benutzer diese Spiele spielen, werden große Mengen an Protokolldaten erzeugt, mit denen Verwendungsmuster, Spielweisen und Einstellungen der Spieler nachverfolgt werden.  In Kombination mit demographischen, regionalen und Produktdaten kann Contoso Analysen durchführen, um Hinweise zu geben, wie das Spielerlebnis verbessert werden kann und welche Upgrades und Zusatzfunktionen verfügbar sind. 

Das Ziel von Contoso ist das Identifizieren von Up-Selling- bzw. Cross-Selling-Chancen basierend auf dem Spielverlauf der Spieler und das Hinzufügen neuer überzeugender Features, um das Unternehmenswachstum zu fördern und den Kunden eine größere Benutzerfreundlichkeit bereitzustellen. In diesem Fall verwenden wir ein Spiele-Unternehmen als Beispiel für ein Unternehmen. Das Unternehmen möchte seine Spiele auf Basis des Spielerverhaltens optimieren. Diese Prinzipien gelten für alle Unternehmen, die ihre Kunden an ihre Waren und Dienstleistungen binden und das Kundenerlebnis verbessern möchten.

In dieser Lösung möchte Contoso die Wirksamkeit einer kürzlich gestarteten Marketingkampagne untersuchen. Wir beginnen mit den rohen Spielprotokollen, verarbeiten sie, reichern sie mit Geolocation-Daten an, führen sie mit Werbereferenzdaten zusammen und kopieren sie schließlich in eine Azure SQL-Datenbank-Instanz, um die Wirkung der Kampagne zu analysieren.

## <a name="deploy-solution"></a>Bereitstellen der Lösung
Für diesen einfachen Anwendungsfall benötigen Sie lediglich ein [Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/), ein [Azure Blob Storage-Konto](https://docs.microsoft.com/en-us/azure/storage/storage-create-storage-account#create-a-storage-account) und eine [Azure SQL-Datenbank](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started). Die Pipeline zum Erstellen von Kundenprofilen wird über die Kachel **Beispiel-Pipelines** auf der Startseite Ihrer Data Factory bereitgestellt.

1. Erstellen Sie eine Data Factory, oder öffnen Sie eine vorhandene Data Factory. Schritte zum Erstellen einer Data Factory werden unter [Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) beschrieben.
2. Klicken Sie auf dem Blatt **DATA FACTORY** für die Data Factory auf die Kachel **Beispielpipelines**.

    ![Kachel "Beispielpipelines"](./media/data-factory-samples/SamplePipelinesTile.png)
3. Klicken Sie auf dem Blatt **Beispiel-Pipelines** auf die Option vom Typ **Kundenprofile**, die Sie bereitstellen möchten.

    ![Blatt "Beispielpipelines"](./media/data-factory-samples/SampleTile.png)
4. Geben Sie die Konfigurationseinstellungen für das Beispiel an. Hierzu zählen beispielsweise der Name und der Schlüssel Ihres Azure Storage-Kontos, der Azure SQL-Servername, die Datenbank, die Benutzer-ID und das Kennwort.

    ![Blatt "Beispiel"](./media/data-factory-samples/SampleBlade.png)
5. Wenn Sie mit den Konfigurationseinstellungen fertig sind, klicken Sie auf **Erstellen**, um die Beispielpipelines und die von den Pipelines verwendeten verknüpften Dienste und Tabellen zu erstellen bzw. bereitzustellen.
6. Auf dem Blatt **Beispielpipelines** wird auf der Kachel für das Beispiel, auf die Sie zuvor geklickt haben, der Status der Bereitstellung angezeigt.

    ![Bereitstellungsstatus](./media/data-factory-samples/DeploymentStatus.png)
7. Wenn die Meldung **Bereitstellung erfolgreich** auf der Kachel für das Beispiel angezeigt wird, schließen Sie das Blatt **Beispielpipelines**.  
8. Auf dem Blatt **DATA FACTORY** können Sie sehen, dass Ihrer Data Factory verknüpfte Dienste, Datasets und Pipelines hinzugefügt wurden.  

    ![Blatt "Data Factory"](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="solution-overview"></a>Lösungsübersicht
Dieser einfache Anwendungsfall kann als Beispiel dafür dienen, wie Sie Azure Data Factory zum Erfassen, Vorbereiten, Transformieren, Analysieren und Veröffentlichen von Daten verwenden können.

![Kompletter Workflow](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

In diesem Beispiel wird dargestellt, wie die Datenpipelines im Azure-Portal angezeigt werden, nachdem sie bereitgestellt wurden.

1. Die **PartitionGameLogsPipeline** liest die Rohdaten der Spielereignisse aus dem Blobspeicher und erstellt Partitionen basierend auf Jahr, Monat und Tag.
2. Die **EnrichGameLogsPipeline** verknüpft partitionierte Spielereignisse mit geografisch codierten Verweisdaten und reichert die Daten an, indem sie die IP-Adressen entsprechenden geografischen Standorten zuordnet.
3. Die **AnalyzeMarketingCampaignPipeline** verarbeitet die angereichten Daten zusammen mit den Werbedaten, um die endgültige Ausgabe zu erstellen, die Aufschluss über die Wirksamkeit der Marketingkampagne gibt.

In diesem Beispiel wird Azure Data Factory verwendet, um die Aktivitäten zu koordinieren, mit denen die Eingabedaten kopiert, die Daten verarbeitet und die endgültigen Daten in einer Azure SQL-Datenbank ausgegeben werden.  Über die Benutzeroberfläche können Sie auch das Netzwerk der Datenpipelines visualisieren, sie verwalten und deren Status überwachen.

## <a name="benefits"></a>Vorteile
Durch Optimieren der Benutzerprofilanalysen und deren Ausrichtung auf die Unternehmensziele kann das Spieleunternehmen schnell Verwendungsmuster sammeln und die Effektivität der Marketingkampagnen analysieren.




<!--HONumber=Dec16_HO2-->


