<properties
   pageTitle="Debuggen von Apps in einem lokalen Docker-Container | Microsoft Azure"
   description="Erfahren Sie, wie Sie eine in einem lokalen Docker-Container ausgeführte App ändern, den Container über das Feature zum Bearbeiten und Aktualisieren aktualisieren und Haltepunkte für das Debuggen setzen."
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/22/2016"
   ms.author="mlearned" />

# Debuggen von Apps in einem lokalen Docker-Container

## Übersicht
Die Visual Studio-Tools für Docker bieten eine zuverlässige Möglichkeit der lokalen Entwicklung und Überprüfung Ihrer Anwendung in einem Linux Docker-Container. Sie müssen bei jeder Codeänderung den Container nicht neu starten. In diesem Artikel erfahren Sie, wie Sie das Feature zum Bearbeiten und Aktualisieren nutzen, um eine ASP.NET Core-Web-App in einem lokalen Docker-Container zu starten, die erforderlichen Änderungen vorzunehmen und den Browser dann zu aktualisieren, um die Änderungen anzuzeigen. Ferner wird gezeigt, wie Sie zum Debuggen Haltepunkte festlegen.

> [AZURE.NOTE] Unterstützung von Windows-Container wird in einer künftigen Version geboten.

## Voraussetzungen
Die folgenden Tools müssen installiert werden:

- [Visual Studio 2015 Update 2](https://go.microsoft.com/fwlink/?LinkId=691978)
- Installieren von [Visual Studio 2015 Update 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)

Um Docker-Container lokal ausführen zu können, benötigen Sie einen lokalen Docker-Client. Sie können die veröffentlichte [Docker Toolbox](https://www.docker.com/products/overview#/docker_toolbox) verwenden, wofür Hyper-V deaktiviert werden muss. Sie können auch die [Betaversion von Docker für Windows](https://beta.docker.com) nutzen, die Hyper-V verwendet und Windows 10 erfordert.

Wenn Sie Docker Toolbox verwenden, müssen Sie [den Docker-Client konfigurieren](./vs-azure-tools-docker-setup.md).

## 1\. Erstellen einer Web-App

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## 2\. Hinzufügen der Docker-Unterstützung

[AZURE.INCLUDE [Hinzufügen der Docker-Unterstützung](../includes/vs-azure-tools-docker-add-docker-support.md)]


## 3\. Bearbeiten des Codes und Aktualisieren

Um Änderungen schnell zu durchlaufen, können Sie Ihre Anwendung in einem Container starten, Änderungen weiter vornehmen und wie mithilfe von IIS Express anzeigen.

1. Legen Sie die Projektmappenkonfiguration auf `Debug` fest, und drücken Sie **<STRG+F5>**, um Ihr Docker-Image zu erstellen und lokal auszuführen.

    Sobald das Containerimage erstellt wurde und in einem Docker-Container ausgeführt wird, startet Visual Studio, die Web-App in Ihrem Standardbrowser. Lesen Sie den Abschnitt [Problembehandlung](vs-azure-tools-docker-troubleshooting-docker-errors.md), wenn Sie den Browser Microsoft Edge verwenden oder sonstige Fehlermeldungen erhalten.

1. Wechseln Sie zur Seite „Info“, auf der wir unsere Änderungen vornehmen.

1. Kehren Sie zu Visual Studio zurück, und öffnen Sie `Views\Home\About.cshtml`.

1. Fügen Sie den folgenden HTML-Inhalt am Ende der Datei hinzu, und speichern Sie die Änderungen.

	```
	<h1>Hello from a Docker Container!</h1>
	```

1.	Sobald der .NET-Build fertig gestellt ist, sehen Sie im Ausgabefenster dieses Zeilen. Kehren Sie zu Ihrem Browser zurück, und aktualisieren Sie die Seite „Info“.

    ```
    Now listening on: http://*:80
    Application started. Press Ctrl+C to shut down
    ```

1.	Ihre Änderungen wurden übernommen!

## 4\. Debuggen mit Haltepunkten

Änderungen müssen öfters überprüft werden, wozu die Debugfunktionen von Visual Studio genutzt werden.

1.	Kehren Sie zu Visual Studio zurück, und öffnen Sie `Controllers\HomeController.cs`.

1.  Ersetzen Sie den Inhalt der „About()“-Methode durch Folgendes:

	```
	string message = "Your application description page from wthin a Container";
	ViewData["Message"] = message;
    ````

1.  Setzen Sie links neben die Zeile `string message` einen Haltepunkt.

1.  Drücken Sie **<F5>**, um mit dem Debuggen beginnen.

1.  Navigieren Sie zur Seite „Info“, und den Haltepunkt zu erreichen.

1.  Kehren Sie zu Visual Studio zurück, um den Haltepunkt anzuzeigen, und untersuchen Sie den Wert der Nachricht.

	![][2]

##Zusammenfassung

Die [Visual Studio 2015 Tools für Docker](https://aka.ms/DockerToolsForVS) bieten die Produktivität einer lokalen Umgebung und realitätsnahe Entwicklung innerhalb eines Docker-Containers.

## Problembehandlung

[Problembehandlung bei der Visual Studio Docker-Entwicklung](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## Weitere Informationen zu Docker mit Visual Studio und Azure

- [Docker-Tools für Visual Studio](http://aka.ms/dockertoolsforvs) – Entwicklung von .NET Core-Code in einem Container
- [Docker-Tools für Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) – Erstellen und Bereitstellen von Docker-Containern
- [Docker-Tools für Visual Studio Code](http://aka.ms/dockertoolsforvscode) – Sprachdienste für die Bearbeitung von Docker-Dateien, mit weiteren e2e-Szenarien in Planung
- [Informationen zu Windows-Containern](http://aka.ms/containers) – Informationen zu Windows Server und Nano Server
- [Azure-Containerdienst](https://azure.microsoft.com/services/container-service/) – [Inhalt des Azure-Containerdiensts](http://aka.ms/AzureContainerService)
-    Weitere Beispiele zum Arbeiten mit Docker finden Sie unter [Working with Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) (Arbeiten mit Docker) aus der [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect-[Demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Weitere Schnellstarts aus der Demo „HealthClinic.biz“ finden Sie unter [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts) (Schnellstarts zu Azure-Entwicklungstools).

## Verschiedene Docker-Tools

[Some great docker tools (Blog von Steve Lasker) (Einige großartige Docker-Tools)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## Nützliche Artikel

[Introduction to Microservices from NGINX (Einführung in Microservices von NGINX)](https://www.nginx.com/blog/introduction-to-microservices/)

## Präsentationen

- [Steve Lasker: VS Live Las Vegas 2016 - Docker e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
- [Introduction to ASP.NET Core @ build 2016 - Where You At Demo (Einführung in ASP.NET Core auf der Build 2016)](https://channel9.msdn.com/Events/Build/2016/B810)
- [Developing .NET apps in containers, Channel 9 (Entwickeln von .NET-Apps in Containern)](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png

<!---HONumber=AcomDC_0921_2016-->