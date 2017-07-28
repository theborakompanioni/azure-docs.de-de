---
title: "Beibehalten von Ergebnissen oder Protokollen von abgeschlossenen Aufträgen und Tasks in einem Datenspeicher – Azure Batch | Microsoft-Dokumentation"
description: "Lernen Sie verschiedene Optionen zum Beibehalten von Ausgabedaten von Batch-Tasks und -Aufträgen kennen. Sie können Daten in Azure Storage oder in einem anderen Datenspeicher beibehalten."
services: batch
author: tamram
manager: timlt
editor: 
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 3ca93e823f02b1483ed290cf89de191937d1e2c3
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017


---
# <a name="persist-job-and-task-output"></a>Persistente Aufträge und Aufgabenausgabe

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

Einige gängige Beispiele der Taskausgabe sind unter anderem:

- Erstellte Dateien, wenn die Aufgabe Eingabedaten verarbeitet
- Protokolldateien, die mit der Taskausführung in Verbindung stehen 

Dieser Artikel beschreibt die verschiedenen Optionen zur Beibehaltung der Taskausgabe und die Szenarios, für die die einzelnen Optionen am besten geeignet sind.   

## <a name="about-the-batch-file-conventions-standard"></a>Informationen zum Batch-Dateikonventionenstandard

Batch definiert einen optionalen Satz von Konventionen zum Benennen von Taskausgabedateien in Azure Storage. Der [Batch-Dateikonventionenstandard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) beschreibt diese Konventionen. Der Dateikonventionenstandard bestimmt die Namen des Zielcontainers und den Blobpfad in Azure Storage für eine gegebene Ausgabedatei, die auf den Namen des Auftrags und der Aufgabe basiert.

Es liegt an Ihnen, ob Sie Dateikonventionenstandard für die Benennung Ihrer Ausgabedatendateien verwenden. Sie können auch den Zielcontainer und -blob benennen, wie Sie möchten. Wenn sie den Dateikonventionenstandard zum Benennen von Ausgabedateien verwenden, sind Ihre Ausgabedateien zur Anzeige im [Azure-Portal][portal] verfügbar.

Es gibt unterschiedliche Möglichkeiten, wie Sie den Dateikonventionenstandard verwenden können:

- Wenn Sie die Batch-Dienst-API verwenden, um Ausgabedateien persistent zu speichern, können Sie auswählen, Zielcontainer und Blobs gemäß dem Dateikonventionenstandard zu benennen. Mithilfe der Batch-Dienst-API können sie Ausgabedateien von Clientcode beibehalten, ohne Ihre Taskanwendung zu bearbeiten.
- Wenn Sie mit .NET entwickeln, können Sie die [Bibliothek für Azure Batch-Dateikonventionen für .NET][nuget_package] verwenden. Ein Vorteil der Verwendung dieser Bibliothek ist, dass Ihre Ausgabedateien gemäß ihrer ID bzw. dem Zweck Abfragen unterstützt. Die integrierte Abfragefunktion erleichtert den Zugriff auf Ausgabedateien von einer Clientanwendung oder anderen Aufgaben aus. Ihre Taskanwendung muss jedoch geändert werden, um Bibliotheken für Dateikonventionen aufzurufen. Weitere Informationen finden Sie in der Referenz für die [Bibliothek für Dateikonventionen für .NET](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.aspx).
- Wenn Sie mit einer anderen Sprache als .NET entwickeln, können Sie den Dateikonventionenstandard in Ihrer Anwendung implementieren.

## <a name="design-considerations-for-persisting-output"></a>Entwurfsüberlegungen zum Beibehalten der Ausgabe 

Berücksichtigen Sie beim Entwerfen Ihrer Batch-Lösung folgende Faktoren, die die Ausgabe von Aufträgen und Tasks betreffen.

* **Lebensdauer der Computeknoten**: Computeknoten sind häufig vorübergehender Natur, insbesondere in Pools mit aktivierter automatischer Skalierung. Die Ausgabe einer Aufgabe, die auf einem Knoten ausgeführt wird, ist nur verfügbar, solange der Knoten vorhanden ist, und nur für die Dateiaufbewahrungszeit, die Sie für den Task festgelegt haben. Wenn eine Aufgabe eine Ausgabe produziert, die möglicherweise nach Abschluss der Aufgabe erforderlich ist, muss die Aufgabe diese Ausgabedateien in einen permanenten Speicher hochladen, z.B. Azure Storage.

* **Ausgabespeicher**: Azure Storage wird als Datenspeicher für die Taskausgabe empfohlen, aber Sie können auch jeden anderen persistenten Speicher verwenden. Das Schreiben der Taskausgabe in Azure Storage ist in der Batch-Dienst-API enthalten. Wenn Sie eine andere Form des permanenten Speichers verwenden, müssen Sie die Anwendungslogik selbst schreiben, um die Taskausgabe beizubehalten.   

* **Abrufen der Ausgabe**: Sie können die Taskausgabe direkt aus den Computeknoten im Pool oder von Azure Storage einem anderen Datenspeicher beibehalten, wenn Sie über persistente Taskausgabe verfügen. Um die Ausgabe eines Tasks direkt von einem Computeknoten abzurufen, benötigen Sie den Dateinamen und den Ausgabespeicherort auf dem Knoten. Wenn Sie die Taskausgabe in Azure Storage beibehalten, benötigen Sie den vollständigen Pfad zur Datei in Azure Storage, um die Ausgabedateien mit dem Azure Storage SDK herunterzuladen.

* **Anzeigen der Ausgabe**: Wenn Sie im Azure-Portal zu einem Batch-Task navigieren und **Dateien auf Knoten** auswählen, werden Ihnen alle Dateien angezeigt, die mit dem Task verknüpft sind (nicht nur die Ausgabedateien, die Sie interessieren). Wie bereits erwähnt, sind Dateien auf Computeknoten nur verfügbar, solange der Knoten vorhanden ist, und nur für die Dateiaufbewahrungszeit, die Sie für den Task festgelegt haben. Um die Taskausgabe anzuzeigen, die Sie in Azure Storage beibehalten haben, können Sie das Azure-Portal oder eine Azure Storage-Clientanwendung wie z.B. [Azure Storage Explorer][storage_explorer] verwenden. Um Ausgabedaten in Azure Storage mit dem Portal oder einem anderen Tool anzuzeigen, müssen Sie den Speicherort der Datei kennen und direkt dorthin navigieren.

## <a name="options-for-persisting-output"></a>Optionen für das Beibehalten der Ausgabe

Je nach Szenario gibt es ein paar unterschiedliche Ansätze, die Sie ergreifen können, um die Taskausgabe beizubehalten:

- Verwenden Sie die Batch-Dienst-API.  
- Verwenden Sie die Bibliothek für Batch-Dateikonventionen für .NET.  
- Implementieren Sie den Batch-Dateikonventionenstandard in Ihrer Anwendung.
- Implementieren Sie eine benutzerdefinierte Dateibewegungslösung.

In den folgenden Abschnitten wird jeder Ansatz ausführlicher beschrieben.

### <a name="use-the-batch-service-api"></a>Verwenden der Batch-Dienst-API

Mit der Version vom 1.5.2017 fügt der Batch-Dienst Unterstützung für die Angabe von Ausgabedateien in Azure Storage für Taskdaten hinzu, wenn Sie [einen Task einem Auftrag hinzufügen](https://docs.microsoft.com/rest/api/batchservice/add-a-task-to-a-job) oder [eine Sammlung von Tasks einem Auftrag hinzufügen](https://docs.microsoft.com/rest/api/batchservice/add-a-collection-of-tasks-to-a-job).

Die Batch-API unterstützt die Beibehaltung von Taskdaten zu einem Azure Storage-Konto von Pools, die mit den Konfigurationen des virtuellen Computers erstellt wurden. Mit der Batch-Dienst-API können Sie Daten für den Task beibehalten, ohne die Anwendungen zu ändern, die von Ihrem Task ausgeführt werden. Sie können optional den [Batch-Dateikonventionenstandard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) für die Benennung der Dateien befolgen, die Sie in Azure Storage beibehalten möchten. 

Verwenden Sie die Batch-Dienst-API, um die Taskausgabe beizubehalten, wenn:

- Sie Daten aus Batch-Tasks und Auftrags-Manager-Aufgaben in Pools beibehalten möchten, die mit den Konfigurationen des virtuellen Computers erstellt wurden.
- Sie Daten in einem Azure Storage-Container mit einem beliebigen Namen beibehalten möchten.
- Sie Daten in einem Azure Storage-Container beibehalten möchten, der auf Grundlage des [Batch-Dateikonventionenstandards](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) benannt wurde. 

> [!NOTE]
> Die Batch-Dienst-API unterstützt keine persistenten Daten aus Tasks, die in Pools ausgeführt werden, die mit der Clouddienstkonfiguration erstellt wurden. Informationen zum Beibehalten von Taskausgaben aus Pools, die in Clouddienstkonfigurationen ausgeführt werden, finden Sie unter [Persist job and task data to Azure Storage with the Batch File Conventions library for .NET to persist (Beibehalten von Auftrags- und Taskdaten in Azure Storage mit der Batch-Dateikonventionenbibliothek für .NET zur Beibehaltung)](batch-task-output-file-conventions.md).
> 
> 

Weitere Informationen zum Beibehalten der Taskausgabe mit der Batch-Dienst-API, finden Sie unter [Persist task data to Azure Storage with the Batch service API (Beibehalten von Taskdaten in Azure Storage mit der Batch-Dienst-API)](batch-task-output-files.md). Sehen Sie sich auch das Beispielprojekt [PersistOutputs][github_persistoutputs] auf GitHub an, das zeigt, wie die Batch-Clientbibliothek für .NET verwendet wird, um die Taskausgabe in dauerhaftem Speicher beizubehalten.

### <a name="use-the-batch-file-conventions-library-for-net"></a>Verwenden der Batch-Dateikonventionenbibliothek für .NET

Entwickler, die Batch-Lösungen mit C# und .NET erstellen, können die [Dateikonventionenbibiothek für .NET][nuget_package] verwenden, um Taskdaten in einem Azure Storage-Konto auf Grundlage des [Batch-Dateikonventionenstandards](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) beizubehalten. Die Dateikonventionenbibliothek behandelt das Verschieben von Ausgabedateien zu Azure Storage und das Benennen von Zielcontainern und Blobs auf bekannte Weise.

Die Dateikonventionenbibliothek unterstützt die Abfrage von Ausgabedateien durch die ID bzw. dem Zweck, ohne dass die vollständigen Datei-URIs erforderlich sind. 

Verwenden Sie die Konventionen für die Batch-Bibliothek für .NET, um die Taskausgabe beizubehalten, wenn:

- Sie Dateien in Azure Storage streamen möchten, während der Task noch ausgeführt wird.
- Sie Daten aus Pools beibehalten möchten, die entweder über die Clouddienstkonfiguration oder die Konfiguration des virtuellen Computers erstellt wurden.
- Ihre Clientanwendung oder andere Taks im Auftrag Taskausgabedateien nach ID oder Zweck gefunden und heruntergeladen werden müssen. 
- Sie Prüfungspunkte oder einen frühen Upload erster Ergebnisse ausführen möchten.
- Sie die Taskausgabe im Azure-Portal anzeigen möchten.

Informationen zum Beibehalten von Taskausgaben mit der Dateikonventionenbibliothek für .NET finden Sie unter [Persist job and task data to Azure Storage with the Batch File Conventions library for .NET to persist (Beibehalten von Auftrags- und Taskdaten in Azure Storage mit der Batch-Dateikonventionenbibliothek für .NET zur Beibehaltung)](batch-task-output-file-conventions.md). Sehen Sie sich auch das Beispielprojekt [PersistOutputs][github_persistoutputs] auf GitHub an, das zeigt, wie die Dateikonventionenbibliothek für .NET verwendet wird, um die Taskausgabe in dauerhaftem Speicher beizubehalten.

Das Beispielprojekt [PersistOutputs][github_persistoutputs] auf GitHub veranschaulicht, wie die Batch-Clientbibliothek für .NET verwendet wird, um die Taskausgabe in dauerhaftem Speicher beizubehalten.

### <a name="implement-the-batch-file-conventions-standard"></a>Implementieren des Batch-Konventionenstandards

Wenn Sie eine andere Sprache als .NET verwenden, können Sie den [Batch-Dateikonventionenstandard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) in Ihrer eigenen Anwendung implementieren. 

Möglicherweise möchten Sie den Dateikonventionen-Benennungsstandard selbst implementieren, wenn Sie ein bewährtes Benennungsschema wollen oder die Taskausgabe im Azure-Portal anzeigen möchten.

### <a name="implement-a-custom-file-movement-solution"></a>Implementieren einer benutzerdefinierten Lösung zur Dateiverschiebung

Sie können auch Ihre eigene vollständige Lösung zur Dateiverschiebung implementieren. Nutzen Sie dieses Verfahren, wenn:

- Sie Taskdaten in einem Datenspeicher beibehalten möchten, der nicht Azure Storage ist. Um Dateien in einen Datenspeicher wie Azure SQL oder Azure Data Lake hochzuladen, können Sie ein benutzerdefiniertes Skript oder eine ausführbare Datei erstellen, um Dateien in diesen Speicherort zu laden. Sie können sie dann über die Befehlszeile aufrufen, nachdem Sie Ihre erste ausführbare Datei ausgeführt haben. Sie können z.B. auf einem Windows-Knoten möglicherweise diese beiden Befehle aufrufen: `doMyWork.exe && uploadMyFilesToSql.exe`
- Sie Prüfungspunkte oder einen frühen Upload erster Ergebnisse ausführen möchten.
- Sie präzise Kontrolle über die Fehlerbehandlung beibehalten möchten. Sie möchten z.B. Ihre eigene Lösung implementieren, wenn sie Taskabhängigkeitsaktionen verwenden wollen, um bestimmte Uploadaktionen auf Grundlage bestimmter Exitcodes für Tasks auszuführen. Weitere Informationen zu Taskabhängigkeitsaktionen finden Sie unter [Erstellen von Taskabhängigkeiten zum Ausführen von Tasks, die von anderen Tasks abhängen](batch-task-dependencies.md). 

## <a name="next-steps"></a>Nächste Schritte

- Untersuchen Sie mithilfe der neuen Funktionen in der Batch-Dienst-API das Beibehalten von Daten unter [Persist task data to Azure Storage with the Batch service API (Beibehalten von Taskdaten in Azure Storage mit der Batch-Dienst-API)](batch-task-output-files.md).
- Informationen zur Verwendung der Batch-Dateikonventionenbibliothek für .NET finden Sie unter [Persist job and task data to Azure Storage with the Batch File Conventions library for .NET to persist (Beibehalten von Auftrags- und Taskdaten in Azure Storage mit der Batch-Dateikonventionenbibliothek für .NET zur Beibehaltung)](batch-task-output-file-conventions.md).
- Sehen Sie sich das Beispielprojekt [PersistOutputs][github_persistoutputs] auf GitHub an, das zeigt, wie die Batch-Clientbibliothek für .NET und die Dateikonventionenbibliothek für .NET verwendet werden, um die Taskausgabe in dauerhaftem Speicher beizubehalten.

[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

