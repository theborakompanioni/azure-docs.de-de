<properties
	pageTitle="Grundlagen des Azure Batch-Diensts | Microsoft Azure"
	description="Hier erhalten Sie Informationen über den Azure Batch-Dienst für umfangreiche parallele Workloads und HPC-Workloads."
	services="batch"
	documentationCenter=""
	authors="mmacy"
	manager="timlt"
	editor=""/>

<tags
	ms.service="batch"
	ms.workload="big-compute"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="07/06/2016"
	ms.author="marsma"/>

# Grundlagen von Azure Batch

Azure Batch ermöglicht Ihnen die effiziente Ausführung von größeren parallelen und leistungsstarken Anwendungen (High Performance Computing, HPC) in der Cloud. Es handelt sich um einen Plattformdienst zur Planung rechenintensiver Aufgaben für die Ausführung auf einer verwalteten Sammlung von virtuellen Maschinen, und die Computeressourcen können automatisch skaliert werden, um die Anforderungen Ihrer Aufträge zu erfüllen.

Mit dem Batch-Dienst definieren Sie Azure-Computeressourcen, um Ihre Anwendungen parallel und bedarfsorientiert auszuführen. Sie können bedarfsgesteuerte oder geplante Aufträge ausführen. Es ist nicht erforderlich, einen HPC-Cluster, einzelne virtuelle Computer, virtuelle Netzwerke oder eine komplexe Infrastruktur für die Auftrags- und Aufgabenplanung manuell zu erstellen, zu konfigurieren und zu verwalten.

## Anwendungsfälle für Batch

Batch ist ein verwalteter Azure-Dienst, der für die *Batchverarbeitung* bzw. das *Batchcomputing* verwendet wird. Es werden große Mengen an ähnlichen Aufgaben ausgeführt, um das gewünschte Ergebnis zu erzielen. Batchcomputing wird am häufigsten von Unternehmen eingesetzt, die regelmäßig große Mengen an Daten verarbeiten, transformieren und analysieren.

Batch funktioniert gut mit intrinsisch parallelen Anwendungen und Workloads (auch als „hochgradig parallel“ bezeichnet). Intrinsisch parallele Workloads können leicht in mehrere Aufgaben unterteilt werden, mit denen die Arbeitsschritte gleichzeitig auf mehreren Computern ausgeführt werden.

![Parallele Aufgaben][1]<br/>

Beispiele für Workloads, die mit diesem Verfahren häufig verarbeitet werden:

* Risikomodellierung im Finanzbereich
* Datenanalysen für die Bereiche Klima und Hydrologie
* Bildrendering, -analyse und -verarbeitung
* Mediencodierung und-transcodierung
* Analyse genetischer Sequenzen
* Belastungsanalysen für den Maschinenbau
* Testen von Software

Batch kann auch parallele Berechnungen mit einem abschließenden Reduzierungsschritt sowie komplexere HPC-Workloads ausführen, z. B. [MPI](batch-mpi.md)-Anwendungen (Message Passing Interface).

Ein Vergleich zwischen Batch und anderen HPC-Lösungsoptionen in Azure finden Sie unter [Batch und HPC-Lösungen](batch-hpc-solutions.md).

## Entwickeln mit Batch

Die Verarbeitung von parallelen Workloads mit Batch wird normalerweise programmgesteuert mit einer der [Batch-APIs](#batch-development-apis) durchgeführt. Mit den Batch-APIs erstellen und verwalten Sie Pools mit Computeknoten (virtuellen Maschinen) und planen Aufträge und Aufgaben, die auf diesen Knoten ausgeführt werden. Für eine Clientanwendung oder einen Dienst, die bzw. den Sie erstellen, werden die Batch-APIs zum Kommunizieren mit dem Batch-Dienst verwendet.

Sie können größere Workloads für Ihr Unternehmen auf effiziente Weise verarbeiten oder ein Dienst-Front-End für Ihre Kunden bereitstellen, damit sie Aufträge und Aufgaben – bedarfsgesteuert oder nach Zeitplan – auf einem, Hunderten oder Tausenden von Knoten ausführen können. Sie können Batch auch als Teil eines größeren Workflows nutzen, der mit Tools verwaltet wird, z.B. [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md).

> [AZURE.TIP] Wenn Sie ein besseres Verständnis der Batch-API-Funktionen entwickeln möchten, können Sie sich [Übersicht über Batch-Features für Entwickler](batch-api-basics.md) durchlesen.

### Benötigte Azure-Konten

Beim Entwickeln von Batch-Lösungen verwenden Sie in Microsoft Azure die folgenden Konten:

- **Azure-Konto und -Abonnement:** Wenn Sie noch kein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnentenleistungen][msdn_benefits] in Anspruch nehmen oder sich für ein [kostenloses Azure-Konto][free_account] registrieren. Beim Erstellen eines Kontos wird für Sie ein Standardabonnement erstellt.

- **Batch-Konto:** Wenn Ihre Anwendungen mit dem Batch-Dienst interagieren, werden der Kontoname, die URL des Kontos und ein Zugriffsschlüssel als Anmeldeinformationen verwendet. Ihre Batch-Ressourcen, z. B. Pools, Computeknoten, Aufträge und Aufgaben, sind einem Batch-Konto zugeordnet. Sie können [ein Batch-Konto im Azure-Portal erstellen und verwalten](batch-account-create-portal.md).

- **Speicherkonto:** Batch enthält integrierte Unterstützung zum Verwenden von Dateien in [Azure Storage][azure_storage]. Für nahezu jedes Batch-Szenario wird Azure Storage verwendet: für das Staging der Programme, die von Ihren Aufgaben ausgeführt werden, und für die dabei verarbeiteten Daten sowie für die Speicherung der generierten Ausgabedaten. Informationen zum Erstellen eines Speicherkontos finden Sie unter [Informationen zu Azure-Speicherkonten](./../storage/storage-create-storage-account.md).

### APIs für die Batch-Entwicklung

Ihre Anwendungen und Dienste können direkte REST-API-Aufrufe ausgeben und/oder eine oder mehrere der folgenden Clientbibliotheken verwenden, um mit dem Batch-Dienst Computeressourcen zu verwalten und parallele Workloads bedarfsgerecht auszuführen.

| API | API-Referenz | Herunterladen | Codebeispiele |
| ----------------- | ------------- | -------- | ------------ |
| **Batch REST (in englischer Sprache)** | [MSDN][batch_rest] | – | [MSDN][batch_rest] |
| **Batch .NET** | [MSDN][api_net] | [NuGet][api_net_nuget] | [GitHub][api_sample_net] |
| **Batch Python** | [readthedocs.io][api_python] | [PyPI][api_python_pypi] |[GitHub][api_sample_python] |
| **Batch Node.js** | [github.io][api_nodejs] | [npm][api_nodejs_npm] | - | | **Batch Java** (Vorschau) | [github.io][api_java] | [Repository mit Maven-Momentaufnahmen][api_java_jar] | [GitHub][api_sample_java] |

### Batch-Ressourcenverwaltung

Zusätzlich zu den Client-APIs können Sie auch Folgendes verwenden, um Ressourcen im Batch-Konto zu verwalten.

- [Batch PowerShell-Cmdlets:][batch_ps] Mit den Azure Batch-Cmdlets im [Azure PowerShell](../powershell-install-configure.md)-Modul können Sie Batch-Ressourcen mit PowerShell verwalten.

- [Azure-CLI:](../xplat-cli-install.md) Die Azure-Befehlszeilenschnittstelle (Azure-CLI) ist ein plattformübergreifendes Toolset mit Shellbefehlen für die Interaktion mit vielen Azure-Diensten, z.B. Batch.

- [Batch Management .NET](batch-management-dotnet.md)-Clientbibliothek: Ist auch über [NuGet][api_net_mgmt_nuget] verfügbar. Sie können die Batch Management .NET-Clientbibliothek nutzen, um Batch-Konten, -Kontingente und -Anwendungspakete programmgesteuert zu verwalten. Den Verweis auf die Verwaltungsbibliothek finden Sie auf der [MSDN-Website][api_net_mgmt].

## Szenario: Horizontales Hochskalieren eines parallelen Workloads

Eine gängige Lösung, bei der die Batch-APIs zum Interagieren mit dem Batch-Dienst verwendet werden, umfasst das horizontale Hochskalieren von intrinsisch parallelen Arbeitsschritten – z. B. das Rendern von Bildern für 3D-Szenen – für einen Pool mit Computeknoten. Dieser Pool mit Computeknoten kann als Ihre „Renderfarm“ dienen, mit der für Ihren Renderauftrag beispielsweise Dutzende, Hunderte oder sogar Tausende von Kernen bereitgestellt werden können.

Im folgenden Diagramm ist ein gängiger Batch-Workflow dargestellt, bei dem eine Clientanwendung oder ein gehosteter Dienst Batch zum Ausführen einer parallelen Workload verwendet.

![Batch-Lösungsworkflow][2]

Bei diesem häufigen Szenario verarbeitet Ihre Anwendung bzw. Ihr Dienst eine Workloadberechnung in Azure Batch, indem die folgenden Schritte ausgeführt werden:

1. Laden Sie die **Eingabedateien** und die **Anwendung**, mit der diese Dateien verarbeitet werden, in Ihr Azure Storage-Konto hoch. Die Eingabedateien können alle Daten sein, die von Ihrer Anwendung verarbeitet werden, z. B. Daten für Finanzmodelle oder zu transcodierende Videodateien. Bei den Anwendungsdateien kann es sich um alle Anwendungen handeln, die zum Verarbeiten der Daten verwendet werden, z. B. eine 3D-Renderanwendung oder einen Medien-Transcoder.

2. Erstellen Sie einen Batch-**Pool** mit Computeknoten in Ihrem Batch-Konto. Dies sind die virtuellen Computer, mit denen Ihre Aufgaben ausgeführt werden. Sie geben Eigenschaften, z.B. die [Knotengröße](./../cloud-services/cloud-services-sizes-specs.md), das Betriebssystem und den Speicherort in Azure Storage für die Anwendung an, die installiert werden soll, wenn die Knoten dem Pool beitreten (die in Schritt 1 hochgeladene Anwendung). Sie können für den Pool auch eine [automatische Skalierung](batch-automatic-scaling.md) konfigurieren. In diesem Fall wird die Anzahl von Computeknoten im Pool entsprechend der Workload, die von Ihren Aufgaben generiert wird, automatisch angepasst.

3. Erstellen Sie einen Batch-**Auftrag**, um die Workload im Pool mit den Computeknoten auszuführen. Wenn Sie einen Auftrag erstellen, ordnen Sie ihn einem Batch-Pool zu.

4. Fügen Sie dem Auftrag **Aufgaben** hinzu. Wenn Sie einem Auftrag Aufgaben hinzufügen, plant der Batch-Dienst die Aufgaben automatisch für die Ausführung auf den Computeknoten im Pool ein. Jede Aufgabe verwendet die Anwendung, die Sie hochgeladen haben, zum Verarbeiten der Eingabedateien.

    - 4a. Bevor eine Aufgabe ausgeführt wird, können die zu verarbeitenden Daten (die Eingabedateien) auf den zugewiesenen Computeknoten heruntergeladen werden. Falls die Anwendung nicht bereits auf dem Knoten installiert wurde (siehe Schritt 2), kann sie stattdessen hier heruntergeladen werden. Nach Abschluss der Downloads werden die Aufgaben auf ihren zugewiesenen Knoten ausgeführt.

5. Während der Ausführung der Aufgaben können Sie Batch abfragen, um den Fortschritt des Auftrags und seiner Aufgaben zu überwachen. Die Clientanwendung bzw. der Dienst kommuniziert mit dem Batch-Dienst per HTTPS. Da Sie unter Umständen Tausende von Aufgaben auf Tausenden von Computeknoten überwachen müssen, sollten Sie für ein [effizientes Abfragen des Batch-Diensts](batch-efficient-list-queries.md) sorgen.

6. Nach Abschluss der Aufgaben können die Ergebnisdaten in Azure Storage hochgeladen werden. Sie können Dateien auch direkt von Computeknoten abrufen.

7. Wenn bei der Überwachung erkannt wird, dass die Aufgaben Ihres Auftrags abgeschlossen wurden, kann Ihre Clientanwendung bzw. der Dienst die Ausgabedaten zur weiteren Verarbeitung oder Auswertung herunterladen.

Bedenken Sie, dass dies nur eine Möglichkeit zur Verwendung von Batch ist und dass in diesem Szenario nur einige der verfügbaren Features beschrieben werden. Beispielsweise können Sie auf jedem Computeknoten [mehrere Aufgaben parallel](batch-parallel-node-tasks.md) ausführen und die [Vorbereitung und den Abschluss von Aufträgen](batch-job-prep-release.md) nutzen, um die Knoten für Ihre Aufträge vorzubereiten. Anschließend können Sie die Bereinigung durchführen.

## Nächste Schritte

Nachdem Sie sich hier einen allgemeinen Überblick über den Batch-Dienst verschafft haben, können Sie tiefer einsteigen und sich darüber informieren, wie Sie den Dienst zum Verarbeiten Ihrer rechenintensiven parallelen Workloads nutzen können.

- Lesen Sie die [Übersicht über Batch-Features für Entwickler](batch-api-basics.md), um ausführlichere Informationen zu den API-Features zu erhalten, die von Batch für die Verarbeitung Ihrer Workloads bereitgestellt werden. Die ist für die Vorbereitung auf die Verwendung von Batch eine wichtige Lektüre.

- Unter [Erste Schritte mit der Azure Batch-Bibliothek für .NET](batch-dotnet-get-started.md) erfahren Sie, wie Sie C# und die Batch-Bibliothek für .NET verwenden, um eine einfache Workload mit einem allgemeinen Batch-Workflow auszuführen. Dies sollte beim Erlernen des Batch-Diensts einer Ihrer ersten Anlaufpunkte sein. Es ist auch eine [Python-Version](batch-python-tutorial.md) des Tutorials verfügbar.

- Laden Sie die [Codebeispiele unter GitHub][github_samples] herunter, um sich zu informieren, wie C# und Python mit Batch verknüpft werden können, um Beispielworkloads zu planen und zu verarbeiten.

- Sehen Sie sich den [Batch-Lernpfad][learning_path] an, um sich über die Ressourcen zu informieren, die zum Erlernen des Umgangs mit Batch verfügbar sind.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://adxsnapshots.azurewebsites.net/?dir=com%5cmicrosoft%5cazure%5cazure-batch
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png

<!---HONumber=AcomDC_0706_2016-->