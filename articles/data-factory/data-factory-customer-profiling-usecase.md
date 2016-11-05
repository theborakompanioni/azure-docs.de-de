---
title: Anwendungsfall – Erstellen von Kundenprofilen
description: Erfahren Sie, wie Azure Data Factory zum Erstellen eines datengesteuerten Workflows (Pipeline) verwendet wird, um Profile von Spielekunden zu erstellen.
services: data-factory
documentationcenter: ''
author: spelluru
manager: jhubbard
editor: monicar

ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: spelluru

---
# Anwendungsfall – Erstellen von Kundenprofilen
Azure Data Factory ist einer der zahlreichen Dienste, die zum Implementieren der Cortana Intelligence Suite von Lösungsoptimierungen verwendet werden. Weitere Informationen zu Cortana Intelligence finden Sie unter [Cortana Intelligence Suite](http://www.microsoft.com/cortanaanalytics). In diesem Dokument wird ein einfacher Anwendungsfall beschrieben, der Ihnen dabei helfen soll zu verstehen, wie Sie mit Azure Data Factory verbreitete Analyseprobleme lösen können.

Sie benötigen lediglich ein [Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/), um auf diesen einfachen Anwendungsfall zuzugreifen und ihn zu testen. Sie können ein Beispiel bereitstellen, in dem dieser Anwendungsfall implementiert wird, indem Sie die Schritte im Artikel [Beispiele](data-factory-samples.md) ausführen.

## Szenario
Contoso ist ein Spielehersteller, der Spiele für mehrere Plattformen entwickelt. Hierzu zählen Spielekonsolen, mobile Geräte und PCs. Wenn Benutzer diese Spiele spielen, werden große Mengen an Protokolldaten erzeugt, mit denen Verwendungsmuster, Spielweisen und Einstellungen der Spieler nachverfolgt werden. In Kombination mit demographischen, regionalen und Produktdaten kann Contoso Analysen durchführen, um Hinweise zu geben, wie das Spielerlebnis verbessert werden kann und welche Upgrades und Zusatzfunktionen verfügbar sind.

Das Ziel von Contoso ist das Identifizieren von Up-Selling- bzw. Cross-Selling-Chancen basierend auf dem Spielverlauf der Spieler und das Hinzufügen neuer überzeugender Features, um das Unternehmenswachstum zu fördern und den Kunden eine größere Benutzerfreundlichkeit bereitzustellen. In diesem Fall verwenden wir ein Spiele-Unternehmen als Beispiel für ein Unternehmen. Das Unternehmen möchte seine Spiele auf Basis des Spielerverhaltens optimieren. Diese Prinzipien gelten für alle Unternehmen, die ihre Kunden an ihre Waren und Dienstleistungen binden und das Kundenerlebnis verbessern möchten.

## Herausforderungen
## Lösungsübersicht
Dieser einfache Anwendungsfall kann als Beispiel dafür dienen, wie Sie Azure Data Factory zum Erfassen, Vorbereiten, Transformieren, Analysieren und Veröffentlichen von Daten verwenden können.

![Kompletter Workflow](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

In diesem Beispiel wird dargestellt, wie die Datenpipelines im Azure-Portal angezeigt werden, nachdem sie bereitgestellt wurden.

1. Die **PartitionGameLogsPipeline** liest die Rohdaten der Spielereignisse aus dem Blobspeicher und erstellt Partitionen basierend auf Jahr, Monat und Tag.
2. Die **EnrichGameLogsPipeline** verknüpft partitionierte Spielereignisse mit geografisch codierten Verweisdaten und reichert die Daten an, indem sie die IP-Adressen entsprechenden geografischen Standorten zuordnet.
3. Die **AnalyzeMarketingCampaignPipeline** verarbeitet die angereichten Daten zusammen mit den Werbedaten, um die endgültige Ausgabe zu erstellen, die Aufschluss über die Wirksamkeit der Marketingkampagne gibt.

In diesem Beispiel wird Azure Data Factory verwendet, um die Aktivitäten zu koordinieren, mit denen die Eingabedaten kopiert, die Daten verarbeitet und die endgültigen Daten in einer Azure SQL-Datenbank ausgegeben werden. Über die Benutzeroberfläche können Sie auch das Netzwerk der Datenpipelines visualisieren, sie verwalten und deren Status überwachen.

## Vorteile
Durch Optimieren der Benutzerprofilanalysen und deren Ausrichtung auf die Unternehmensziele kann das Spieleunternehmen schnell Verwendungsmuster sammeln und die Effektivität der Marketingkampagnen analysieren.

<!---HONumber=AcomDC_0907_2016-->