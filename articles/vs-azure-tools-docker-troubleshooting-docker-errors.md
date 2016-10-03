<properties
   pageTitle="Problembehandlung bei Docker-Clientfehlern unter Windows mit Visual Studio | Microsoft Azure"
   description="Beheben Sie Probleme, die bei der Verwendung von Visual Studio unter Windows zum Erstellen und Bereitstellen von Web-Apps in Docker auftreten können."
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
   ms.date="06/08/2016"
   ms.author="allclark" />

# Problembehandlung bei der Visual Studio Docker-Entwicklung

Bei der Arbeit mit der Preview-Version von Visual Studio-Tools für Docker können für solche Versionen typische Probleme auftreten. Es folgen einige häufig auftretende Probleme und entsprechende Lösungen.


## Volumezuordnung kann nicht überprüft werden
Die Volumezuordnung wird benötigt, um den Quellcode und die Binärdateien Ihrer Anwendung für den App-Ordner im Container freizugeben. Spezifische Volumezuordnungen sind in den Dateien „docker-compose.dev.debug.yml“ und „docker-compose.dev.release.yml“ enthalten. Wenn Dateien auf dem Hostcomputer geändert werden, werden diese Änderungen in den Containern in einer ähnlichen Ordnerstruktur berücksichtigt.

Öffnen Sie zum Aktivieren der Volumezuordnung über das Wal-Taskleistensymbol von Docker für Windows die Option **Einstellungen...**, und wählen Sie dann die Registerkarte **Shared Drives** (Freigegebene Laufwerke) aus. Vergewissern Sie sich, dass der Buchstabe des Laufwerks, das als Host für Ihr Projekt fungiert, und der Buchstabe des Laufwerks, auf dem sich % USERPROFILE% befindet, freigegeben sind, und klicken Sie dann auf **Übernehmen**.

Führen Sie zum Testen der Volumezuordnung nach der Freigabe der Laufwerke entweder eine Neuerstellung in Visual Studio durch, und drücken Sie F5, oder führen Sie an einer Eingabeaufforderung Folgendes aus:

*Windows-Eingabeaufforderung:*

*[Hinweis: Hierbei wird davon ausgegangen, dass sich Ihr Benutzerordner auf dem Laufwerk „C“ befindet und freigegeben wurde. Sollten Sie ein anderes Laufwerk freigegeben haben, aktualisieren Sie den Befehl ggf. entsprechend.]*
```
docker run -it -v /c/Users/Public:/wormhole busybox
```

*Linux-Container:*

```
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
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

**Hinweis:** *Bei der Arbeit mit Linux-VMs muss im Containerdateisystem die Groß-/Kleinschreibung beachtet werden.*

##Build: Unerwarteter Fehler bei der PrepareForBuild-Aufgabe.

Microsoft.DotNet.Docker.CommandLine.ClientException: Fehler beim Herstellen der Verbindung:

Überprüfen Sie, ob der Docker-Standardhost in Betrieb ist. Öffnen Sie eine Eingabeaufforderung, und führen Sie Folgendes aus:

```
docker info
```

Wird hierbei ein Fehler zurückgegeben, versuchen Sie, die Desktop-App **Docker für Windows** zu starten. Wenn die Desktop-App ausgeführt wird, sollte auf der Taskleiste das **Walsymbol** angezeigt werden. Klicken Sie mit der rechten Maustaste auf das Taskleistensymbol, und öffnen Sie **Einstellungen**. Klicken Sie auf die Registerkarte **Zurücksetzen**, und starten Sie Docker neu.

##Manuelles Upgraden von Version 0.31 auf 0.40


1. Sichern Sie das Projekt.
1. Löschen Sie die folgenden Dateien im Projekt:

    ```
      Dockerfile
      Dockerfile.debug
      DockerTask.ps1
      docker-compose-yml
      docker-compose.debug.yml
      .dockerignore
      Properties\Docker.props
      Properties\Docker.targets
    ```

1. Schließen Sie die Projektmappe, und entfernen Sie die folgenden Zeilen aus der XPROJ-Datei:

    ```
      <DockerToolsMinVersion>0.xx</DockerToolsMinVersion>
      <Import Project="Properties\Docker.props" />
      <Import Project="Properties\Docker.targets" />
    ```

1. Öffnen Sie die Projektmappe wieder.
1. Entfernen Sie die folgenden Zeilen aus „Properties\\launchSettings.json“:

    ```
      "Docker": {
        "executablePath": "%WINDIR%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
        "commandLineArgs": "-ExecutionPolicy RemoteSigned .\\DockerTask.ps1 -Run -Environment $(Configuration) -Machine '$(DockerMachineName)'"
      }
    ```

1. Entfernen Sie die folgenden Docker-bezogenen Dateien aus „project.json“ (unter „publishOptions“):

    ```
    "publishOptions": {
      "include": [
        ...
        "docker-compose.yml",
        "docker-compose.debug.yml",
        "Dockerfile.debug",
        "Dockerfile",
        ".dockerignore"
      ]
    },
    ```

1. Deinstallieren Sie die frühere Version, installieren Sie Docker Tools 0.40, und klicken Sie dann im Kontextmenü für Ihre ASP.Net Core-Webanwendung oder -Konsolenanwendung erneut auf **Hinzufügen > Docker Support** (Docker-Unterstützung). Dadurch werden die neuen erforderlichen Docker-Artefakte wieder Ihrem Projekt hinzugefügt.

## Fehler beim Verwenden von **Hinzufügen > Docker Support** (Docker-Unterstützung) oder beim Debuggen (F5) einer ASP.NET Core-Anwendung in einem Container

Gelegentlich hat das Deinstallieren und Installieren von Erweiterungen eine Beschädigung des MEF-Caches (Managed Extensibility Framework) von Visual Studio zur Folge. In diesem Fall können beim Hinzufügen der Docker-Unterstützung und/oder beim Ausführen oder Debuggen (F5) Ihrer ASP.NET Core-Anwendung verschiedene Fehlerdialogfelder angezeigt werden. Führen Sie als vorübergehende Problemumgehung die folgenden Schritte aus, um den MEF-Cache zu löschen und neu zu generieren:

1. Schließen Sie alle Instanzen von Visual Studio.
1. Öffnen Sie „%USERPROFILE%\\AppData\\Local\\Microsoft\\VisualStudio\\14.0\\“.
1. Löschen Sie die folgenden Ordner:
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Öffnen Sie Visual Studio.
1. Wiederholen Sie das Szenario.

<!---HONumber=AcomDC_0921_2016-->