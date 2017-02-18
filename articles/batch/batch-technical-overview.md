---
title: "Übersicht über den Azure Batch-Dienst | Microsoft-Dokumentation"
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
ms.date: 01/23/2017
ms.author: tamram
translationtype: Human Translation
ms.sourcegitcommit: bf22cd3426e936c8d74377f59443e5e1a6834286
ms.openlocfilehash: 3a01764b6a0e777e947cf8648606cdd568d118ff


---
# <a name="basics-of-azure-batch"></a>Grundlagen von Azure Batch
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

## <a name="developing-with-batch"></a>Entwickeln mit Batch
Die Verarbeitung paralleler Workloads mit Azure Batch wird üblicherweise programmgesteuert mit einer der [Batch-APIs](#batch-development-apis) durchgeführt. Ihre Clientanwendung oder Ihr Dienst kann mithilfe der Batch-APIs mit dem Batch-Dienst kommunizieren. Mithilfe der Batch-APIs können Sie Pools mit Computeknoten (virtuelle Computer oder Clouddienste) erstellen und verwalten. Anschließend können Sie die Ausführung von Aufträgen und Aufgaben auf diesen Knoten planen. 

Sie können größere Workloads für Ihr Unternehmen auf effiziente Weise verarbeiten oder ein Dienst-Front-End für Ihre Kunden bereitstellen, damit sie Aufträge und Aufgaben – bedarfsgesteuert oder nach Zeitplan – auf einem, Hunderten oder Tausenden von Knoten ausführen können. Sie können Azure Batch auch als Teil eines größeren Workflows nutzen und mithilfe von Tools wie [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md) verwalten.

> [!TIP]
> Wenn Sie ein besseres Verständnis der Batch-API-Funktionen entwickeln möchten, können Sie sich [Übersicht über Batch-Features für Entwickler](batch-api-basics.md)durchlesen.
> 
> 

### <a name="azure-accounts-youll-need"></a>Benötigte Azure-Konten
Beim Entwickeln von Batch-Lösungen verwenden Sie in Microsoft Azure die folgenden Konten:

* **Azure-Konto und -Abonnement:** Wenn Sie noch kein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnentenleistungen][msdn_benefits] in Anspruch nehmen oder sich für ein [kostenloses Azure-Konto][free_account] registrieren. Beim Erstellen eines Kontos wird für Sie ein Standardabonnement erstellt.
* **Batch-Konto:** Azure Batch-Ressourcen (einschließlich Pools, Computeknoten, Aufträge und Aufgaben) werden einem Azure Batch-Konto zugeordnet. Wenn Ihre Anwendung eine Anforderung an den Batch-Dienst richtet, wird die Anforderung anhand des Azure Batch-Kontonamens, der URL des Kontos und eines Zugriffsschlüssels authentifiziert. Sie können im Azure-Portal ein [Batch-Konto erstellen](batch-account-create-portal.md).
* **Speicherkonto:** Batch enthält integrierte Unterstützung zum Verwenden von Dateien in [Azure Storage][azure_storage]. In nahezu jedem Batch-Szenario wird Azure Blob Storage für das Staging der Programme, die von Ihren Aufgaben ausgeführt werden, und für die dabei verarbeiteten Daten sowie für die Speicherung der generierten Ausgabedaten verwendet. Informationen zum Erstellen eines Speicherkontos finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md).

### <a name="batch-development-apis"></a>APIs für die Batch-Entwicklung
Ihre Anwendungen und Dienste können direkte REST-API-Aufrufe ausgeben oder Ihre Azure Batch-Workloads unter Verwendung einer oder mehrerer der folgenden Clientbibliotheken ausführen und verwalten:

| API | API-Referenz | Download | Lernprogramm: | Codebeispiele |
| --- | --- | --- | --- | --- |
| **Batch REST (in englischer Sprache)** |[MSDN][batch_rest] |N/V |- |- |
| **Batch .NET** |[docs.microsoft.com][api_net] |[NuGet ][api_net_nuget] |[Tutorial](batch-dotnet-get-started.md) |[GitHub][api_sample_net] |
| **Batch Python** |[readthedocs.io][api_python] |[PyPI][api_python_pypi] |[Tutorial](batch-python-tutorial.md)|[GitHub][api_sample_python] |
| **Batch Node.js** |[github.io][api_nodejs] |[npm][api_nodejs_npm] |- |- |
| **Batch Java** (Vorschau) |[github.io][api_java] |[Maven][api_java_jar] |- |[GitHub][api_sample_java] |

### <a name="batch-command-line-tools"></a>Batch-Befehlszeilentools

Die von den Entwicklungs-APIs bereitgestellten Funktionen stehen auch über Befehlszeilentools zur Verfügung: 

* [Batch PowerShell-Cmdlets:][batch_ps] Mit den Azure Batch-Cmdlets im [Azure PowerShell](/powershell/azureps-cmdlets-docs)-Modul können Sie Batch-Ressourcen mit PowerShell verwalten.
* [Azure-CLI](../xplat-cli-install.md): Die Azure-Befehlszeilenschnittstelle (Azure-CLI) ist ein plattformübergreifendes Toolset mit Shellbefehlen für die Interaktion mit vielen Azure-Diensten, z.B. Batch.

### <a name="batch-resource-management"></a>Batch-Ressourcenverwaltung

Die Azure Resource Manager-APIs für Batch ermöglichen den programmgesteuerten Zugriff auf Batch-Konten. Mithilfe dieser APIs können Sie Batch-Konten, Kontingente und Anwendungspakete programmgesteuert verwalten.  

| API | API-Referenz | Download | Lernprogramm: | Codebeispiele |
| --- | --- | --- | --- | --- |
| **Batch Resource Manager REST** |[docs.microsoft.com][api_rest_mgmt] |– |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **Batch Resource Manager .NET** |[docs.microsoft.com][api_net_mgmt] |[NuGet ][api_net_mgmt_nuget] | [Tutorial](batch-management-dotnet.md) |[GitHub][api_sample_net] |


### <a name="batch-tools"></a>Batch-Tools
Sie sind zum Erstellen von Lösungen mit Batch zwar nicht erforderlich, aber hier sind einige sehr nützliche Tools angegeben, die Sie zum Erstellen und Debuggen Ihrer Batch-Anwendungen und -Dienste verwenden können.

* [Azure-Portal:][portal] Sie können Batch-Pools, -Aufträge und -Aufgaben auf den Batch-Blättern des Azure-Portals erstellen, überwachen und löschen. Sie können die Statusinformationen für diese und andere Ressourcen beim Ausführen der Aufträge anzeigen und sogar Dateien von den Computeknoten in Ihren Pools herunterladen (z.B. Download der Datei `stderr.txt` einer fehlgeschlagenen Aufgabe bei der Problembehandlung). Außerdem können Sie RDP-Dateien (Remotedesktop) herunterladen, die sich zum Anmelden an Computeknoten eignen.
* [Azure Batch-Explorer:][batch_explorer] Der Batch-Explorer stellt ähnliche Funktionen für die Batch-Ressourcenverwaltung wie das Azure-Portal bereit, nutzt dafür aber eine eigenständige WPF-Clientanwendung (Windows Presentation Foundation). Dies ist eine der Batch .NET-Beispielanwendungen auf [GitHub][github_samples]. Sie können sie mit Visual Studio 2015 oder höher erstellen und zum Durchsuchen und Verwalten der Ressourcen in Ihrem Batch-Konto verwenden, während Sie Batch-Lösungen entwickeln und debuggen. Zeigen Sie Details zu Aufträgen, Pools und Aufgaben an, laden Sie Dateien von Computeknoten herunter, und stellen Sie mit RDP-Dateien (Remotedesktop) eine Remoteverbindung mit Knoten her. Sie können diese Dateien mit Batch-Explorer herunterladen.
* [Microsoft Azure-Speicher-Explorer:][storage_explorer] Dies ist zwar kein Azure Batch-Tool, aber der Speicher-Explorer ist ein weiteres wertvolles Tool, das Sie beim Entwickeln und Debuggen Ihrer Batch-Lösungen einsetzen können.

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
* [Erste Schritte mit der Azure Batch-Bibliothek für .NET](batch-dotnet-get-started.md) erfahren Sie, wie Sie mit C# und der Batch .NET-Bibliothek eine einfache Workload mit einem allgemeinen Batch-Workflow ausführen. Dieser Artikel sollte beim Erlernen des Batch-Diensts einer Ihrer ersten Anlaufpunkte sein. Es ist auch eine [Python-Version](batch-python-tutorial.md) des Tutorials verfügbar.
* Laden Sie die [Codebeispiele unter GitHub][github_samples] herunter, um sich zu informieren, wie C# und Python mit Batch verknüpft werden können, um Beispielworkloads zu planen und zu verarbeiten.
* Sehen Sie sich den [Batch-Lernpfad][learning_path] an, um sich über die Ressourcen zu informieren, die zum Erlernen des Umgangs mit Batch verfügbar sind.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_rest_mgmt]: https://docs.microsoft.com/en-us/rest/api/batchmanagement/
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
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png



<!--HONumber=Jan17_HO4-->


