<properties
   pageTitle="Debuggen von Apps in einem lokalen Docker-Container | Microsoft Azure"
   description="Erfahren Sie, wie Sie eine in einem lokalen Docker-Container ausgeführte App ändern, den Container über das Feature zum Bearbeiten und Aktualisieren aktualisieren und Haltepunkte für das Debuggen setzen."
   services="visual-studio-online"
   documentationCenter="na"
   authors="AllenClark"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="05/13/2016"
   ms.author="allclark" />

# Debuggen von Apps in einem lokalen Docker-Container

## Übersicht
Die Visual Studio-Tools für Docker bieten eine zuverlässige Möglichkeit der lokalen Entwicklung und Überprüfung Ihrer Anwendung in einem Linux Docker-Container. Sie müssen bei jeder Codeänderung den Container nicht neu starten. In diesem Artikel erfahren Sie, wie Sie das Feature zum Bearbeiten und Aktualisieren nutzen, um eine ASP.NET Core-Web-App in einem lokalen Docker-Container zu starten, die erforderlichen Änderungen vorzunehmen und den Browser dann zu aktualisieren, um die Änderungen anzuzeigen. Ferner wird gezeigt, wie Sie zum Debuggen Haltepunkte festlegen.

> [AZURE.NOTE] Unterstützung von Windows-Container wird in einer künftigen Version geboten.

## Voraussetzungen
Die folgenden Tools müssen installiert werden:

- [Visual Studio 2015 Update 2](https://go.microsoft.com/fwlink/?LinkId=691978)
- [Microsoft ASP .NET Core RC 2](http://go.microsoft.com/fwlink/?LinkId=798481)
- [Visual Studio 2015-Tools für Docker](https://aka.ms/DockerToolsForVS)

Um Docker-Container lokal ausführen zu können, benötigen Sie einen lokalen Docker-Client. Sie können die veröffentlichte [Docker Toolbox](https://www.docker.com/products/overview#/docker_toolbox) verwenden, wofür Hyper-V deaktiviert werden muss. Alternativ können Sie die [Betaversion von Docker für Windows](https://beta.docker.com) nutzen, die Hyper-V verwendet und Windows 10 erfordert.

Wenn Sie die Docker Toolbox verwenden, müssen Sie [den Docker-Client konfigurieren](./vs-azure-tools-docker-setup.md).

## Bearbeiten einer App, die in einem lokalen Docker-Container ausgeführt wird
Mit den Visual Studio 2015-Tools für Docker können Entwickler von ASP.NET Core RC2-Web-Apps ihre Anwendung in einem Docker-Container testen und ausführen, Änderungen an der Anwendung in Visual Studio vornehmen und den Browser aktualisieren, um die Änderungen anzuzeigen, die auf die im Container ausgeführte App angewendet wurden. Mit .NET Core und der Visual Studio-Tools für Docker-Version 0.20 können Sie auch Haltepunkte im Code setzen, der im Docker-Container ausgeführt wird.

1. Wählen Sie im Visual Studio-Menü **Datei > Neu > Projekt** aus.

1. Wählen Sie im Abschnitt **Vorlagen** des Dialogfelds **Neues Projekt** die Option **Visual C# > Web** aus.

1. Wählen Sie **ASP.NET Core-Webanwendung (.NET Core)**.

1. Weisen Sie Ihrer neuen Anwendung einen Namen zu (oder übernehmen Sie den Standardnamen), und tippen Sie auf **OK**.

1. Wählen Sie unter **ASP.NET Core-Vorlagen** den Eintrag **Webanwendung** aus, und tippen Sie auf **OK**.

1. Deaktivieren Sie **In der Cloud hosten**, da Sie Docker als Bereitstellungslösung verwenden möchten.

1. Klicken Sie im Projektmappen-Explorer von Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen > Docker-Unterstützung** aus.

	![][0]

1. Unterhalb des Projektknotens werden die folgenden Dateien erstellt:

	![][1]

> [AZURE.NOTE] Bei Verwendung der [Betaversion von Docker für Windows](https://beta.docker.com) öffnen Sie „Properties\\Docker.props“. Entfernen Sie den Standardwert, und starten Sie Visual Studio neu, um die Einstellung zu übernehmen. ![][2]

##Bearbeiten und aktualisieren
Um Änderungen schnell zu durchlaufen, können Sie Ihre Anwendung in einem Container starten, Änderungen weiter vornehmen und wie mithilfe von IIS Express anzeigen.

1. Legen Sie die Projektmappenkonfiguration auf `Debug` fest, und drücken Sie **<STRG+F5>**, um Ihr Docker-Image zu erstellen und lokal auszuführen. Siehe das Ausgabefenster

1. Sobald das Containerimage erstellt wurde und in einem Docker-Container ausgeführt wird, versucht Visual Studio, die Web-App in Ihrem Standardbrowser zu starten. Lesen Sie den Abschnitt [Problembehandlung](vs-azure-tools-docker-troubleshooting-docker-errors.md), wenn Sie den Browser Microsoft Edge verwenden oder sonstige Fehlermeldungen erhalten.

1. Kehren Sie zurück zu Visual Studio, und öffnen Sie `Views\Home\About.cshtml`.

1. Fügen Sie den folgenden HTML-Inhalt am Ende der Datei hinzu, und speichern Sie die Änderungen

	```
	<h1>Hello from a Docker Container!</h1>
	```

1.	Sobald der .NET-Build fertig gestellt wurde und `Application started. Press Ctrl+C to shut down` im Ausgabefenster angezeigt wird, kehren Sie zum Browser zurück und aktualisieren die Seite.

1.	Ihre Änderungen sollten übernommen worden sein.

##Debuggen mit Haltepunkten
Änderungen müssen öfters überprüft werden, wozu die Debugfunktionen von Visual Studio genutzt werden.

1.	Kehren Sie zurück zu Visual Studio, und öffnen Sie `Controllers\HomeController.cs`.

1.  Ersetzen Sie den Inhalt der „About()“-Methode durch Folgendes:

	```
	string message = "Your application description page from wthin a Container";
	ViewData["Message"] = message;
    ````

1.  Setzen Sie einen Haltepunkt links neben die Zeile `string message`.

1.  Drücken Sie **<F5>**, um mit dem Debuggen beginnen.

1.  Navigieren Sie zur Seite „Info“, und den Haltepunkt zu erreichen.

1.  Kehren Sie zu Visual Studio zurück, um den Haltepunkt anzuzeigen, und untersuchen Sie den Wert der Nachricht.

	![][3]

##Zusammenfassung
Die [Visual Studio 2015 Tools für Docker](https://aka.ms/DockerToolsForVS) bieten die Produktivität einer lokalen Umgebung und realitätsnahe Entwicklung innerhalb eines Docker-Containers.

## Problembehandlung
[Problembehandlung bei der Visual Studio Docker-Entwicklung](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## Weitere Informationen zu Docker mit Visual Studio und Azure

- [Docker-Tools für Visual Studio](http://aka.ms/dockertoolsforvs) – Entwicklung von .NET Core-Code in einem Container
- [Docker-Tools für Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) – Erstellen und Bereitstellen von Docker-Containern
- [Docker-Tools für Visual Studio Code](http://aka.ms/dockertoolsforvscode) – Sprachdienste für die Bearbeitung von Docker-Dateien, mit weiteren e2e-Szenarien in Planung
- [Informationen zu Windows-Containern](http://aka.ms/containers) – Informationen zu Windows Server und Nano
- [Azure-Containerdienst](https://azure.microsoft.com/services/container-service/) – [Inhalt des Azure-Containerdiensts](http://aka.ms/AzureContainerService)

## Verschiedene Docker-Tools

[Some great docker tools (Blog von Steve Lasker)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/) (Einige großartige Docker-Tools)

## Nützliche Artikel

[Introduction to Microservices from NGINX](https://www.nginx.com/blog/introduction-to-microservices/) (Einführung in Microservices von NGINX)

## Präsentationen

- [Steve Lasker: VS Live Las Vegas 2016 - Docker e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
- [Introduction to ASP.NET Core @ build 2016 - Where You At Demo](https://channel9.msdn.com/Events/Build/2016/B810) (Einführung in ASP.NET Core auf der Build 2016)
- [Developing .NET apps in containers, Channel 9](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/) (Entwickeln von .NET-Apps in Containern)

[0]: ./media/vs-azure-tools-docker-edit-and-refresh/add-docker-support.png
[1]: ./media/vs-azure-tools-docker-edit-and-refresh/docker-files-added.png
[2]: ./media/vs-azure-tools-docker-edit-and-refresh/docker-props.png
[3]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png

<!---HONumber=AcomDC_0518_2016-->