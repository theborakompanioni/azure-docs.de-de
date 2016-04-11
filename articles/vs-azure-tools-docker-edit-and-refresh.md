<properties
   pageTitle="Debuggen von Apps in einem lokalen Docker-Container mithilfe des Features zum Bearbeiten und Aktualisieren | Microsoft Azure"
   description="Erfahren Sie, wie Sie eine in einem lokalen Docker-Container ausgeführte App ändern und den Container über das Feature zum Bearbeiten und Aktualisieren aktualisieren"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="03/25/2016"
   ms.author="tarcher" />

# Debuggen von Apps in einem lokalen Docker-Container mithilfe des Features zum Bearbeiten und Aktualisieren

## Übersicht
Die benutzerfreundlichen Visual Studio-Tools für Docker eignen sich hervorragend, um Ihre Anwendung lokal in einem Docker-Container zu entwickeln und zu testen, ohne den Container bei jeder Codeänderung neu starten zu müssen. In diesem Artikel erfahren Sie, wie Sie das Feature zum Bearbeiten und Aktualisieren nutzen, um eine ASP.NET 5-Web-App in einem lokalen Docker-Container zu starten, die erforderlichen Änderungen vorzunehmen und den Browser dann zu aktualisieren, um die Änderungen anzuzeigen.

## Voraussetzungen
Die folgenden Tools müssen installiert werden:

- [Visual Studio 2015 Update 1](https://go.microsoft.com/fwlink/?LinkId=691979)
- [Microsoft ASP.NET und Webtools 2015 (RC)](https://go.microsoft.com/fwlink/?LinkId=627627)
- [Docker Toolbox](https://www.docker.com/products/overview#/docker_toolbox)
- [Visual Studio 2015-Tools für Docker](https://aka.ms/DockerToolsForVS)
- [Konfigurieren des Docker-Clients](./vs-azure-tools-docker-setup.md)

> [AZURE.NOTE] Wenn auf Ihrem System bereits eine frühere Version der Visual Studio 2015-Tools für Docker installiert ist, muss diese über die Systemsteuerung deinstalliert werden, bevor Sie die aktuelle Version installieren.

## Bearbeiten einer App, die in einem lokalen Docker-Container ausgeführt wird
Mit den Visual Studio 2015-Tools für Docker können Entwickler von ASP.NET 5-Web-Apps ihre Anwendung in einem Docker-Container testen und ausführen, Änderungen an der Anwendung in Visual Studio vornehmen und den Browser aktualisieren, um die Änderungen anzuzeigen, die auf die im Container ausgeführte App angewendet wurden.

1. Wählen Sie im Visual Studio-Menü **Datei > Neu > Projekt** aus. 

1. Wählen Sie im Abschnitt **Vorlagen** des Dialogfelds **Neues Projekt** die Option **Visual C# > Web** aus.

1. Wählen Sie **ASP.NET-Webanwendung** aus.

1. Weisen Sie Ihrer neuen Anwendung einen Namen zu (oder übernehmen Sie den Standardnamen).

1. Tippen Sie auf **OK**.

1. Wählen Sie unter **ASP.NET 5-Vorlagen** die Option **ASP.NET-Webanwendung** aus.

1. Tippen Sie auf **OK**.

1. Klicken Sie im Projektmappen-Explorer von Visual Studio mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen > Docker-Unterstützung** aus.

	![][0]
 
1. Unterhalb des Projektknotens werden die folgenden Dateien erstellt:

	![][1]

1. Legen Sie die Projektmappenkonfiguration auf `Debug` fest, und drücken Sie **&lt;F5>**, um mit dem lokalen Testen Ihrer Anwendung zu beginnen.

1. Sobald das Containerimage erstellt wurde und in einem Docker-Container ausgeführt wird, versucht eine PowerShell-Konsole, die Web-App in Ihrem Standardbrowser zu starten. Lesen Sie den Abschnitt [Problembehandlung](#troubleshooting), wenn Sie den Microsoft Edge-Browser verwenden.

1. Wechseln Sie erneut zu Visual Studio, und öffnen Sie `Views\Home\Index.cshtml`.

1. Fügen Sie den folgenden HTML-Inhalt am Ende der Datei hinzu, und speichern Sie die Änderungen

		<div>
			<h1>Hello from Docker Container!</h1>
		</div>

1.	Wechseln Sie erneut zu Ihrem Browser, und aktualisieren Sie ihn.

1.	Scrollen Sie zum Ende der Startseite. Ihre Änderungen sollten dort bereits übernommen worden sein. Beachten Sie, dass die erneute Kompilierung der Website einige Sekunden dauern kann. Aktualisieren Sie Ihren Browser erneut, wenn die Änderungen noch nicht sichtbar sind.

##Problembehandlung 

- **Das Ausführen der App hat zur Folge, dass PowerShell geöffnet, eine Fehlermeldung angezeigt und PowerShell wieder geschlossen wird. Die Browserseite wird nicht geöffnet.**

	Dabei könnte es sich um einen Fehler während des folgenden Vorgangs handeln: `docker-compose-up` Führen Sie die folgenden Schritte aus, um den Fehler anzuzeigen:

	1. Öffnen Sie die Datei `Properties\launchSettings.json`.
	
	1. Wechseln Sie zum Docker-Eintrag.
	
	1. Wechseln Sie zur Zeile, die wie folgt beginnt:

			"commandLineArgs": "-ExecutionPolicy RemoteSigned …”
	
	1. Fügen Sie den `-noexit`-Parameter hinzu. Die Zeile sollte nun wie folgt aussehen: Durch diese Änderung bleibt PowerShell geöffnet, sodass Sie den Fehler anzeigen können.

			"commandLineArgs": "-noexit -ExecutionPolicy RemoteSigned …”

- **Erstellung: Fehler beim Erstellen des Images, Fehler beim Überprüfen der TLS-Verbindung: Der Host wird nicht ausgeführt**

	Überprüfen Sie, ob der Docker-Standardhost in Betrieb ist. Siehe dazu den Artikel zum [Konfigurieren des Docker-Clients](./vs-azure-tools-docker-setup.md).

- **Volumezuordnung kann nicht gefunden werden**

	VirtualBox gibt `C:\Users` standardmäßig als `c:/Users` frei. Wenn das Projekt nicht unter `c:\Users` angezeigt wird, fügen Sie es den [freigegebenen Ordnern](https://www.virtualbox.org/manual/ch04.html#sharedfolders) von VirtualBox manuell hinzu.

- **Verwenden von Microsoft Edge als Standardbrowser**

	Wenn Sie den Microsoft Edge-Browser verwenden, wird die Website möglicherweise nicht geöffnet, weil Edge die IP-Adresse als ungesichert einstuft. Führen Sie die folgenden Schritte aus, um dieses Problem zu beheben:
	1. Geben Sie Windows-Feld „Ausführen“ Folgendes ein: `Internet Options`
	2. Tippen Sie auf **Internetoptionen**, wenn diese Option angezeigt wird. 
	2. Tippen Sie auf die Registerkarte **Sicherheit**.
	3. Wählen Sie die Zone **Lokales Intranet** aus.
	4. Tippen Sie auf **Sites**. 
	5. Fügen Sie die IP-Adresse Ihres virtuellen Computers (in diesem Fall der Docker-Host) in der Liste hinzu. 
	6. Aktualisieren Sie die Seite in Edge. Die Website sollte nun verfügbar sein und angezeigt werden. 
	7. Weitere Informationen zu diesem Problem finden Sie in Scott Hanselmans Blogbeitrag [Microsoft Edge can't see or open VirtualBox-hosted local web sites](http://www.hanselman.com/blog/FixedMicrosoftEdgeCantSeeOrOpenVirtualBoxhostedLocalWebSites.aspx) (Über VirtualBox gehostete lokale Websites können in Microsoft Edge nicht angezeigt oder geöffnet werden).

[0]: ./media/vs-azure-tools-docker-edit-and-refresh/add-docker-support.png
[1]: ./media/vs-azure-tools-docker-edit-and-refresh/docker-files-added.png

<!---HONumber=AcomDC_0330_2016-->