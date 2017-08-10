---
title: "Verwenden von Azure Batch-APIs und -Tools zum Entwickeln von größeren parallelen Verarbeitungslösungen | Microsoft-Dokumentation"
description: "Es werden die APIs und Tools beschrieben, die zum Entwickeln von Lösungen mit dem Azure Batch-Dienst verfügbar sind."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: tamram
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: c11f0c0a6cdc89449b7aa515319bbe6b6bdf1a4a
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---


# <a name="overview-of-batch-apis-and-tools"></a>Übersicht über Batch-APIs und -Tools

Die Verarbeitung paralleler Workloads mit Azure Batch wird üblicherweise programmgesteuert mit einer der [Batch-APIs](#batch-development-apis) durchgeführt. Ihre Clientanwendung oder Ihr Dienst kann mithilfe der Batch-APIs mit dem Batch-Dienst kommunizieren. Mithilfe der Batch-APIs können Sie Pools mit Computeknoten (virtuelle Computer oder Clouddienste) erstellen und verwalten. Anschließend können Sie die Ausführung von Aufträgen und Aufgaben auf diesen Knoten planen. 

Sie können größere Workloads für Ihr Unternehmen auf effiziente Weise verarbeiten oder ein Dienst-Front-End für Ihre Kunden bereitstellen, damit sie Aufträge und Aufgaben – bedarfsgesteuert oder nach Zeitplan – auf einem, Hunderten oder Tausenden von Knoten ausführen können. Sie können Azure Batch auch als Teil eines größeren Workflows nutzen und mithilfe von Tools wie [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md?toc=%2fazure%2fbatch%2ftoc.json) verwalten.

> [!TIP]
> Wenn Sie ein besseres Verständnis der Batch-API-Funktionen entwickeln möchten, können Sie sich [Übersicht über Batch-Features für Entwickler](batch-api-basics.md)durchlesen.
> 
> 

## <a name="azure-accounts-for-batch-development"></a>Azure-Konten für die Batch Entwicklung
Beim Entwickeln von Batch-Lösungen verwenden Sie in Microsoft Azure die folgenden Konten:

* **Azure-Konto und -Abonnement:** Wenn Sie noch kein Azure-Abonnement haben, können Sie Ihre [MSDN-Abonnentenleistungen][msdn_benefits] in Anspruch nehmen oder sich für ein [kostenloses Azure-Konto][free_account] registrieren. Beim Erstellen eines Kontos wird für Sie ein Standardabonnement erstellt.
* **Batch-Konto:** Azure Batch-Ressourcen (einschließlich Pools, Computeknoten, Aufträge und Aufgaben) werden einem Azure Batch-Konto zugeordnet. Wenn Ihre Anwendung eine Anforderung an den Batch-Dienst richtet, wird die Anforderung anhand des Azure Batch-Kontonamens, der URL des Kontos und eines Zugriffsschlüssels authentifiziert. Sie können im Azure-Portal ein [Batch-Konto erstellen](batch-account-create-portal.md).
* **Speicherkonto:** Batch enthält integrierte Unterstützung zum Verwenden von Dateien in [Azure Storage][azure_storage]. In nahezu jedem Batch-Szenario wird Azure Blob Storage für das Staging der Programme, die von Ihren Aufgaben ausgeführt werden, und für die dabei verarbeiteten Daten sowie für die Speicherung der generierten Ausgabedaten verwendet. Informationen zum Erstellen eines Speicherkontos finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md).

## <a name="batch-service-apis"></a>Batch-Dienst-APIs

Ihre Anwendungen und Dienste können direkte REST-API-Aufrufe ausgeben oder Ihre Azure Batch-Workloads unter Verwendung einer oder mehrerer der folgenden Clientbibliotheken ausführen und verwalten:

| API | API-Referenz | Download | Lernprogramm: | Codebeispiele | Weitere Informationen |
| --- | --- | --- | --- | --- | --- |
| **Batch REST (in englischer Sprache)** |[MSDN][batch_rest] |N/V |- |- | [Unterstützte Versionen](https://docs.microsoft.com/rest/api/batchservice/batch-service-rest-api-versioning) |
| **Batch .NET** |[docs.microsoft.com][api_net] |[NuGet ][api_net_nuget] |[Tutorial](batch-dotnet-get-started.md) |[GitHub][api_sample_net] | [Anmerkungen zu dieser Version](http://aka.ms/batch-net-dataplane-changelog) |
| **Batch Python** |[readthedocs.io][api_python] |[PyPI][api_python_pypi] |[Tutorial](batch-python-tutorial.md)|[GitHub][api_sample_python] | [Infodatei](https://github.com/Azure/azure-sdk-for-python/blob/master/doc/batch.rst) |
| **Batch Node.js** |[github.io][api_nodejs] |[npm][api_nodejs_npm] |- |- | [Infodatei](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/batch) |
| **Batch Java** |[github.io][api_java] |[Maven][api_java_jar] |- |[Infodatei][api_sample_java] | [Infodatei](https://github.com/Azure/azure-batch-sdk-for-java)|

## <a name="batch-management-apis"></a>Batch Management-APIs

Die Azure Resource Manager-APIs für Batch ermöglichen den programmgesteuerten Zugriff auf Batch-Konten. Mithilfe dieser APIs können Sie Batch-Konten, Kontingente und Anwendungspakete programmgesteuert verwalten.  

| API | API-Referenz | Download | Lernprogramm: | Codebeispiele |
| --- | --- | --- | --- | --- |
| **Batch Resource Manager REST** |[docs.microsoft.com][api_rest_mgmt] |– |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **Batch Resource Manager .NET** |[docs.microsoft.com][api_net_mgmt] |[NuGet ][api_net_mgmt_nuget] | [Tutorial](batch-management-dotnet.md) |[GitHub][api_sample_net] |

## <a name="batch-command-line-tools"></a>Batch-Befehlszeilentools

Diese Befehlszeilentools bieten die gleiche Funktionalität wie die Batch-Dienst- und Batch Management-APIs: 

* [Batch PowerShell-Cmdlets:][batch_ps] Mit den Azure Batch-Cmdlets im [Azure PowerShell](/powershell/azure/overview)-Modul können Sie Batch-Ressourcen mit PowerShell verwalten.
* [Azure CLI](/cli/azure/overview): Die Azure-Befehlszeilenschnittstelle (Azure CLI) ist ein plattformübergreifendes Toolset mit Shellbefehlen für die Interaktion mit vielen Azure-Diensten, z.B. dem Batch-Dienst und dem Batch Management-Dienst. Weitere Informationen zur Verwendung der Azure CLI mit Batch finden Sie unter [Verwalten von Batch-Ressourcen mit der Azure CLI](batch-cli-get-started.md).

## <a name="other-tools-for-application-development"></a>Weitere Tools für die Anwendungsentwicklung

Folgende weitere Tools können beim Entwickeln und Debuggen Ihrer Batch-Anwendungen und -Dienste hilfreich sein:

* [Azure-Portal:][portal] Sie können Batch-Pools, -Aufträge und -Aufgaben auf den Batch-Blättern des Azure-Portals erstellen, überwachen und löschen. Sie können die Statusinformationen für diese und andere Ressourcen beim Ausführen der Aufträge anzeigen und sogar Dateien von den Computeknoten in Ihre Pools herunterladen. Beispielsweise können Sie bei der Problembehandlung die Datei `stderr.txt` einer fehlerhaften Aufgabe herunterladen. Außerdem können Sie RDP-Dateien (Remotedesktop) herunterladen, die sich zum Anmelden an Computeknoten eignen.
* [Azure Batch-Explorer:][batch_explorer] Der Batch-Explorer stellt ähnliche Funktionen für die Batch-Ressourcenverwaltung wie das Azure-Portal bereit, nutzt dafür aber eine eigenständige WPF-Clientanwendung (Windows Presentation Foundation). Dies ist eine der Batch .NET-Beispielanwendungen auf [GitHub][github_samples]. Sie können sie mit Visual Studio 2015 oder höher erstellen und zum Durchsuchen und Verwalten der Ressourcen in Ihrem Batch-Konto verwenden, während Sie Batch-Lösungen entwickeln und debuggen. Zeigen Sie Details zu Aufträgen, Pools und Aufgaben an, laden Sie Dateien von Computeknoten herunter, und stellen Sie mit RDP-Dateien (Remotedesktop) eine Remoteverbindung mit Knoten her. Sie können diese Dateien mit Batch-Explorer herunterladen.
* [Microsoft Azure-Speicher-Explorer:][storage_explorer] Dies ist zwar kein Azure Batch-Tool, aber der Speicher-Explorer ist ein weiteres wertvolles Tool, das Sie beim Entwickeln und Debuggen Ihrer Batch-Lösungen einsetzen können.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- Weitere Informationen zum Protokollieren von Ereignissen in der Batch-Anwendung finden Sie unter [Protokollereignisse für die Diagnoseauswertung und -überwachung von Batch-Lösungen](batch-diagnostics.md). Eine Referenz zu Ereignissen, die vom Batch-Dienst ausgelöst werden, finden Sie unter [Batch-Analysen](batch-analytics.md).
- Informationen zu Umgebungsvariablen für Serverknoten finden Sie unter [Umgebungsvariablen für Azure Batch-Computeknoten](batch-compute-node-environment-variables.md).

## <a name="next-steps"></a>Nächste Schritte

* Sehen Sie sich die [Übersicht über Batch-Features für Entwickler](batch-api-basics.md) an. Diese enthält wichtige Informationen für Entwickler, die sich auf die Verwendung von Batch vorbereiten möchten. Der Artikel enthält ausführliche Informationen zu Batch-Dienstressourcen wie Pools, Knoten, Aufträgen und Aufgaben sowie zu den zahlreichen API-Funktionen, die beim Erstellen der Batch-Anwendung verwendet werden können.
* [Erste Schritte mit der Azure Batch-Bibliothek für .NET](batch-dotnet-get-started.md) erfahren Sie, wie Sie mit C# und der Batch .NET-Bibliothek eine einfache Workload mit einem allgemeinen Batch-Workflow ausführen. Dieser Artikel sollte beim Erlernen des Batch-Diensts einer Ihrer ersten Anlaufpunkte sein. Es ist auch eine [Python-Version](batch-python-tutorial.md) des Tutorials verfügbar.
* Laden Sie die [Codebeispiele unter GitHub][github_samples] herunter, um sich zu informieren, wie C# und Python mit Batch verknüpft werden können, um Beispielworkloads zu planen und zu verarbeiten.
* Sehen Sie sich den [Batch-Lernpfad][learning_path] an, um sich über die Ressourcen zu informieren, die zum Erlernen des Umgangs mit Batch verfügbar sind.


[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_rest_mgmt]: https://docs.microsoft.com/\rest/api/batchmanagement/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: /powershell/resourcemanager/azurerm.batch/v2.7.0/azurerm.batch
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

