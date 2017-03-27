---
title: "Ausführen umfangreicher paralleler Computinglösungen in der Cloud mit Azure Batch | Microsoft-Dokumentation"
description: "Hier erhalten Sie Informationen über den Azure Batch-Dienst für umfangreiche parallele Workloads und HPC-Workloads."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 93e37d44-7585-495e-8491-312ed584ab79
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 2c9877f84873c825f96b62b492f49d1733e6c64e
ms.openlocfilehash: 8a850f8b1cccda631786f89f2c88647604fba9ed
ms.lasthandoff: 03/15/2017


---
# <a name="run-intrinsically-parallel-workloads-with-batch"></a>Ausführen intrinsisch paralleler Workloads mit Batch

Bei Azure Batch handelt es sich um eine Plattform zum Ausführen umfangreicher paralleler und leistungsstarker Anwendungen (High Performance Computing, HPC) in der Cloud. Azure Batch plant die Ausführung rechenintensiver Aufgaben auf einer verwalteten Sammlung virtueller Computer und kann Computeressourcen automatisch skalieren, um den Anforderungen Ihrer Aufträge gerecht zu werden.

Mit Azure Batch definieren Sie mühelos Azure-Computeressourcen für die parallele und bedarfsorientierte Ausführung Ihrer Anwendungen. Es ist keine manuelle Erstellung, Konfiguration und Verwaltung eines HPC-Clusters, einzelner virtueller Computer, virtueller Netzwerke oder einer komplexen Infrastruktur für die Auftrags- und Aufgabenplanung erforderlich. Azure Batch automatisiert oder vereinfacht diese Aufgaben für Sie.

## <a name="use-cases-for-batch"></a>Anwendungsfälle für Batch
Batch ist ein verwalteter Azure-Dienst für *Batchverarbeitung* bzw. *Batchcomputing*. Dabei werden große Mengen ähnlicher Aufgaben ausgeführt, um das gewünschte Ergebnis zu erzielen. Batchcomputing wird am häufigsten von Unternehmen eingesetzt, die regelmäßig große Mengen an Daten verarbeiten, transformieren und analysieren.

Batch funktioniert gut mit intrinsisch parallelen Anwendungen und Workloads (auch als „hochgradig parallel“ bezeichnet). Intrinsisch parallele Workloads können mühelos in mehrere Aufgaben unterteilt werden, sodass die Arbeitsschritte gleichzeitig auf mehreren Computern ausgeführt werden.

![Parallele Aufgaben][1]<br/>

Beispiele für Workloads, die mit diesem Verfahren häufig verarbeitet werden:

* Risikomodellierung im Finanzbereich
* Datenanalysen für die Bereiche Klima und Hydrologie
* Bildrendering, -analyse und -verarbeitung
* Mediencodierung und-transcodierung
* Analyse genetischer Sequenzen
* Belastungsanalysen für den Maschinenbau
* Testen von Software

Batch kann auch parallele Berechnungen mit einem abschließenden Reduzierungsschritt sowie komplexere HPC-Workloads ausführen, z.B. MPI-Anwendungen ([Message Passing Interface](batch-mpi.md)).

Ein Vergleich zwischen Batch und anderen HPC-Lösungsoptionen in Azure finden Sie unter [Batch und HPC-Lösungen](batch-hpc-solutions.md).

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="scenario-scale-out-a-parallel-workload"></a>Szenario: Horizontales Hochskalieren eines parallelen Workloads
Eine gängige Lösung, bei der die Batch-APIs zum Interagieren mit dem Batch-Dienst verwendet werden, umfasst das horizontale Hochskalieren von intrinsisch parallelen Arbeitsschritten – z. B. das Rendern von Bildern für 3D-Szenen – für einen Pool mit Computeknoten. Dieser Pool mit Computeknoten kann als Ihre „Renderfarm“ dienen, mit der für Ihren Renderauftrag beispielsweise Dutzende, Hunderte oder sogar Tausende von Kernen bereitgestellt werden können.

Im folgenden Diagramm ist ein gängiger Batch-Workflow dargestellt, bei dem eine Clientanwendung oder ein gehosteter Dienst Batch zum Ausführen einer parallelen Workload verwendet.

![Batch-Lösungsworkflow][2]

Bei diesem häufigen Szenario verarbeitet Ihre Anwendung bzw. Ihr Dienst eine Workloadberechnung in Azure Batch, indem die folgenden Schritte ausgeführt werden:

1. Laden Sie die **Eingabedateien** und die **Anwendung**, mit der diese Dateien verarbeitet werden, in Ihr Azure Storage-Konto hoch. Die Eingabedateien können alle Daten sein, die von Ihrer Anwendung verarbeitet werden, z. B. Daten für Finanzmodelle oder zu transcodierende Videodateien. Bei den Anwendungsdateien kann es sich um alle Anwendungen handeln, die zum Verarbeiten der Daten verwendet werden, z. B. eine 3D-Renderanwendung oder einen Medien-Transcoder.
2. Erstellen Sie einen Batch-**Pool** mit Computeknoten in Ihrem Batch-Konto. Diese Knoten sind die virtuellen Computer, mit denen Ihre Aufgaben ausgeführt werden. Sie geben Eigenschaften, z.B. die [Knotengröße](../cloud-services/cloud-services-sizes-specs.md), das Betriebssystem und den Speicherort in Azure Storage, für die Anwendung an, die installiert werden soll, wenn die Knoten dem Pool beitreten (die in Schritt 1 hochgeladene Anwendung). Sie können für den Pool auch eine [automatische Skalierung](batch-automatic-scaling.md) auf der Grundlage der Workload konfigurieren, die von Ihren Aufgaben generiert wird. Bei der automatischen Skalierung wird die Anzahl von Computeknoten im Pool dynamisch angepasst.
3. Erstellen Sie einen Batch- **Auftrag** , um die Workload im Pool mit den Computeknoten auszuführen. Wenn Sie einen Auftrag erstellen, ordnen Sie ihn einem Batch-Pool zu.
4. Fügen Sie dem Auftrag **Aufgaben** hinzu. Wenn Sie einem Auftrag Aufgaben hinzufügen, plant der Batch-Dienst die Aufgaben automatisch für die Ausführung auf den Computeknoten im Pool ein. Jede Aufgabe verwendet die Anwendung, die Sie hochgeladen haben, zum Verarbeiten der Eingabedateien.
   
   * 4a. Bevor eine Aufgabe ausgeführt wird, können die zu verarbeitenden Daten (die Eingabedateien) auf den zugewiesenen Computeknoten heruntergeladen werden. Falls die Anwendung nicht bereits auf dem Knoten installiert wurde (siehe Schritt 2), kann sie stattdessen hier heruntergeladen werden. Nach Abschluss der Downloads werden die Aufgaben auf ihren zugewiesenen Knoten ausgeführt.
5. Während der Ausführung der Aufgaben können Sie Batch abfragen, um den Fortschritt des Auftrags und seiner Aufgaben zu überwachen. Ihre Clientanwendung oder Ihr Dienst kommuniziert mit dem Batch-Dienst über HTTPS. Da Sie unter Umständen Tausende von Aufgaben auf Tausenden von Computeknoten überwachen müssen, empfiehlt es sich, [den Batch-Dienst möglichst effizient abzufragen](batch-efficient-list-queries.md).
6. Nach Abschluss der Aufgaben können die Ergebnisdaten in Azure Storage hochgeladen werden. Sie können Dateien auch direkt aus dem Dateisystem auf einem Computeknoten abrufen.
7. Wenn bei der Überwachung erkannt wird, dass die Aufgaben Ihres Auftrags abgeschlossen wurden, kann Ihre Clientanwendung bzw. der Dienst die Ausgabedaten zur weiteren Verarbeitung oder Auswertung herunterladen.

Bedenken Sie, dass dies nur eine Möglichkeit zur Verwendung von Batch ist und dass in diesem Szenario nur einige der verfügbaren Features beschrieben werden. Beispielsweise können Sie auf jedem Computeknoten [mehrere Aufgaben parallel](batch-parallel-node-tasks.md) ausführen und die [Vorbereitung und den Abschluss von Aufträgen](batch-job-prep-release.md) nutzen, um die Knoten für Ihre Aufträge vorzubereiten. Anschließend können Sie die Bereinigung durchführen.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich hier einen allgemeinen Überblick über den Batch-Dienst verschafft haben, können Sie tiefer einsteigen und sich darüber informieren, wie Sie den Dienst zum Verarbeiten Ihrer rechenintensiven parallelen Workloads nutzen können.

* Sehen Sie sich die [Übersicht über Batch-Features für Entwickler](batch-api-basics.md) an. Diese enthält wichtige Informationen für Entwickler, die sich auf die Verwendung von Batch vorbereiten möchten. Der Artikel enthält ausführliche Informationen zu Batch-Dienstressourcen wie Pools, Knoten, Aufträgen und Aufgaben sowie zu den zahlreichen API-Funktionen, die beim Erstellen der Batch-Anwendung verwendet werden können.
* Informieren Sie sich über die [Batch-APIs und Tools](batch-apis-tools.md), die für die Erstellung von Batch-Lösungen verfügbar sind.
* [Erste Schritte mit der Azure Batch-Bibliothek für .NET](batch-dotnet-get-started.md) erfahren Sie, wie Sie mit C# und der Batch .NET-Bibliothek eine einfache Workload mit einem allgemeinen Batch-Workflow ausführen. Dieser Artikel sollte beim Erlernen des Batch-Diensts einer Ihrer ersten Anlaufpunkte sein. Es ist auch eine [Python-Version](batch-python-tutorial.md) des Tutorials verfügbar.
* Laden Sie die [Codebeispiele unter GitHub][github_samples] herunter, um sich zu informieren, wie C# und Python mit Batch verknüpft werden können, um Beispielworkloads zu planen und zu verarbeiten.
* Sehen Sie sich den [Batch-Lernpfad][learning_path] an, um sich über die Ressourcen zu informieren, die zum Erlernen des Umgangs mit Batch verfügbar sind.


[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png

