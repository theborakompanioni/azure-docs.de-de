<properties
   pageTitle="Problembehandlung bei Docker-Clientfehlern unter Windows mit Visual Studio | Microsoft Azure"
   description="Beheben Sie Probleme, die bei der Verwendung von Visual Studio unter Windows zum Erstellen und Bereitstellen von Web-Apps in Docker auftreten können."
   services="azure-container-service"
   documentationCenter="na"
   authors="allclark"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="allclark" />

# Problembehandlung bei der Visual Studio Docker-Entwicklung

Bei der Arbeit mit der Preview-Version von Visual Studio-Tools für Docker können für solche Versionen typische Probleme auftreten. Es folgen einige häufig auftretende Probleme und entsprechende Lösungen.

##Fehler beim Konfigurieren der Datei „Program.cs“ zur Docker-Unterstützung

Beim Hinzufügen von Docker-Unterstützung muss `.UseUrls(Environment.GetEnvironmentVariable("ASPNETCORE_SERVER.URLS"))` zu „WebHostBuilder()“ hinzugefügt werden. Wenn die Datei „Program.cs“, die Funktion `Main()` oder eine neue WebHostBuilder-Klasse nicht gefunden wurde, wird eine Warnung angezeigt. `.UseUrls()` ist erforderlich, damit Kestrel den eingehenden Datenverkehr über „localhost“ hinaus überwachen kann, wenn die Ausführung in einem Docker-Container erfolgt. Nach Fertigstellung sieht der typische Code wie folgt aus:

```
public class Program
{
    public static void Main(string[] args)
    {
        var host = new WebHostBuilder()
            .UseUrls(Environment.GetEnvironmentVariable("ASPNETCORE_URLS") ?? String.Empty)
            .UseKestrel()
            .UseContentRoot(Directory.GetCurrentDirectory() ?? "")
            .UseIISIntegration()
            .UseStartup<Startup>()
            .Build();

        host.Run();
    }
}
```

„UseUrls()“ hat „WebHost“ so konfiguriert, dass eingehender URL-Datenverkehr überwacht wird. Die [Docker-Tools für Visual Studio](http://aka.ms/DockerToolsForVS) konfigurieren die Umgebungsvariable im Modus „dockerfile.debug“/release“ wie folgt:

```
# Configure the listening port to 80
ENV ASPNETCORE_SERVER.URLS http://*:80
```

## Volumezuordnung funktioniert nicht
Zum Aktivieren der Funktionen zum Bearbeiten und Aktualisieren ist die Volumezuordnung so konfiguriert, dass der Quellcode Ihres Projekts mit dem Ordner „.app“ im Container gemeinsam genutzt wird. Bei Änderungen von Dateien auf Ihrem Hostcomputer verwendet das Verzeichnis „/app“ der Container dasselbe Verzeichnis. In „docker-compose.debug.yml“ ermöglicht die folgende Konfiguration die Volumezuordnung:

```
volumes:
    - ..:/app
```

Zum Überprüfen, ob die Volumezuordnung funktioniert, probieren Sie den folgenden Befehl:

**Windows**

```
a
/ # ls
```

Im Ordner „Users/Public“ sollte eine Verzeichnisliste angezeigt werden. Wenn keine Dateien angezeigt werden und Ihr Ordner „/c/Users/Public“ nicht leer ist, ist die Volumezuordnung nicht ordnungsgemäß konfiguriert.

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Wechseln Sie zum Verzeichnis „wormhole“, um den Inhalt des Verzeichnisses `/c/Users/Public` anzuzeigen:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     a.txt
Documents        Libraries        Pictures         desktop.ini
/wormhole #
```

**Hinweis:** *Bei der Arbeit mit Linux-VMs muss im Containerdateisystem die Groß-/Kleinschreibung beachtet werden.*

Wenn Sie den Inhalt nicht anzeigen können, probieren Sie Folgendes:

**Betaversion von Docker für Windows**
- Überprüfen Sie, ob die Docker für Windows-Desktop-App ausgeführt wird, indem Sie auf der Taskleiste nach dem Walsymbol suchen, das weiß und funktionsbereit angezeigt werden muss.
- Überprüfen Sie, ob die Volumezuordnung konfiguriert ist, indem Sie mit der rechten Maustaste auf der Taskleiste auf das Walsymbol klicken, „Einstellungen“ auswählen und dann auf **Freigegebene Laufwerke verwalten** klicken.

**Docker Toolbox mit VirtualBox**

VirtualBox gibt `C:\Users` standardmäßig als `c:/Users` frei. Falls möglich, verschieben Sie Ihr Projekt unter dieses Verzeichnis. Fügen Sie es andernfalls den [freigegebenen Ordnern](https://www.virtualbox.org/manual/ch04.html#sharedfolders) von VirtualBox manuell hinzu.
	
##Erstellung: Fehler beim Erstellen des Images, Fehler beim Überprüfen der TLS-Verbindung: Der Host wird nicht ausgeführt

- Überprüfen Sie, ob der Docker-Standardhost in Betrieb ist. Siehe dazu den Artikel zum [Konfigurieren des Docker-Clients](./vs-azure-tools-docker-setup.md).

##Verwenden von Microsoft Edge als Standardbrowser

Wenn Sie den Microsoft Edge-Browser verwenden, wird die Website möglicherweise nicht geöffnet, weil Edge die IP-Adresse als ungesichert einstuft. Führen Sie die folgenden Schritte aus, um dieses Problem zu beheben:

1. Wechseln Sie zu **Internetoptionen**.
    - Unter Windows 10 können Sie `Internet Options` in das Windows-Feld „Ausführen“ eingeben.
    - In Internet Explorer können Sie das Menü **Einstellungen** aufrufen und **Internetoptionen** auswählen. 
1. Klicken Sie auf **Internetoptionen**, sobald diese Option angezeigt wird. 
1. Klicken Sie auf die Registerkarte **Sicherheit**.
1. Wählen Sie die Zone **Lokales Intranet** aus.
1. Klicken Sie auf **Sites**. 
1. Fügen Sie die IP-Adresse Ihres virtuellen Computers (in diesem Fall der Docker-Host) in der Liste hinzu. 
1. Aktualisieren Sie die Seite in Edge. Die Website sollte nun verfügbar sein und angezeigt werden. 
1. Weitere Informationen zu diesem Problem finden Sie in Scott Hanselmans Blogbeitrag [Microsoft Edge can't see or open VirtualBox-hosted local web sites](http://www.hanselman.com/blog/FixedMicrosoftEdgeCantSeeOrOpenVirtualBoxhostedLocalWebSites.aspx) (Über VirtualBox gehostete lokale Websites können in Microsoft Edge nicht angezeigt oder geöffnet werden). 

##Problembehandlung bei Versionen bis 0.15


###Das Ausführen der App hat zur Folge, dass PowerShell geöffnet, eine Fehlermeldung angezeigt und PowerShell wieder geschlossen wird. Die Browserseite wird nicht geöffnet.

Dabei könnte es sich um einen Fehler während des folgenden Vorgangs handeln: `docker-compose-up`. Führen Sie die folgenden Schritte aus, um den Fehler anzuzeigen:

1. Öffnen Sie die Datei `Properties\launchSettings.json`.
1. Wechseln Sie zum Docker-Eintrag.
1. Wechseln Sie zur Zeile, die wie folgt beginnt:

    ```
    "commandLineArgs": "-ExecutionPolicy RemoteSigned …”
    ```
	
1. Fügen Sie den `-noexit`-Parameter hinzu. Die Zeile sollte nun wie folgt aussehen: Durch diese Änderung bleibt PowerShell geöffnet, sodass Sie den Fehler anzeigen können.

    ```
	"commandLineArgs": "-noexit -ExecutionPolicy RemoteSigned …”
    ```

<!---HONumber=AcomDC_0622_2016-->