---
title: Problembehandlung bei Docker-Clientfehlern unter Windows mit Visual Studio | Microsoft Docs
description: "Beheben Sie Probleme, die bei der Verwendung von Visual Studio unter Windows zum Erstellen und Bereitstellen von Web-Apps in Docker auftreten können."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 346f70b9-7b52-4688-a8e8-8f53869618d3
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
translationtype: Human Translation
ms.sourcegitcommit: 01c10d04606e15082c8842ad87f617703a00c903
ms.openlocfilehash: cbb376dae88d39e965838de74d90158691b59f90


---

# <a name="troubleshooting-visual-studio-docker-development"></a>Problembehandlung bei der Visual Studio Docker-Entwicklung

Bei der Arbeit mit der Preview-Version von Visual Studio-Tools für Docker können für solche Versionen typische Probleme auftreten.
Es folgen einige häufig auftretende Probleme und entsprechende Lösungen.  

## <a name="visual-studio-2017-rc"></a>Visual Studio 2017 RC

### <a name="linux-containers"></a>**Linux-Container**

###  <a name="build-errors-occur-when-debugging-a-net-core-web-or-console-application"></a>Buildfehler treten beim Debuggen einer .NET Core-Web- oder Konsolenanwendung auf.  

Dies kann damit zusammenhängen, dass das Laufwerk nicht freigegeben wird, wenn das Projekt unter Docker für Windows vorhanden ist.  Unter Umständen erhalten Sie beispielsweise den folgenden Fehler:

```
The "PrepareForLaunch" task failed unexpectedly.
Microsoft.DotNet.Docker.CommandLineClientException: Creating network "webapplication13628050196_default" with the default driver
Building webapplication1
Creating webapplication13628050196_webapplication1_1
ERROR: for webapplication1  Cannot create container for service webapplication1: C: drive is not shared. Please share it in Docker for Windows Settings
```
Klicken Sie in der Taskleiste mit der rechten Maustaste auf das Docker für Windows-Symbol, und wählen Sie „Einstellungen...“, um das Problem zu beheben.  Klicken Sie auf die Registerkarte „Shared Drives“ (Freigegebene Laufwerke), und geben Sie den Buchstaben des Laufwerks an, auf dem sich das Projekt befindet.

### <a name="windows-containers"></a>**Windows Containers (in englischer Sprache)**

Die folgenden Informationen gelten für die Behandlung von Fehlern, wenn das Debuggen für .NET Framework-Web- und -Konsolenanwendungen in Windows-Containern durchgeführt wird.

### <a name="pre-requisites"></a>Voraussetzungen

1. Visual Studio 2017 RC (oder höher) mit installierter .NET Core- und Docker-Workload (Vorschau).
2. Windows 10 Anniversary Update mit installierten aktuellen Windows-Updatepatches.
3. Docker für Windows (Beta) – https://docs.docker.com/docker-for-windows/ (Version 1.12.2-beta28 7813 oder höher).
4. Wählen Sie in der Taskleiste das Docker für Windows-Symbol und dann „Switch to Windows containers“ (Zu Windows-Containern wechseln).  Stellen Sie nach dem Neustart des Computers sicher, dass diese Einstellung beibehalten wurde.

### <a name="clicking-docker-debug-project-results-in-the-error--client-version-122-is-too-old--minimum-supported-api-version-is-124-please-upgrade-your-client-to-a-newer-version"></a>Wenn Sie auf **Docker: Projekt debuggen** klicken, wird der folgende Fehler angezeigt: „client version 1.22 is too old.  Minimum supported API version is 1.24, please upgrade your client to a newer version.“ (Clientversion 1.22 ist zu alt. Die unterstützte API-Mindestversion ist 1.24. Aktualisieren Sie den Client auf eine neuere Version.)

Für Version 1.13-RC2-beta31 (9123) oder höher von Docker für Windows ist die Verwendung von Version 2.1 von Docker Compose erforderlich.   Ändern Sie zum Beheben dieses Fehlers alle Vorkommen von Version „2“ in den Dateien „docker-compose*.yml“ des Projekts. Die Standardvorlagen, die dem Projekt von Visual Studio hinzugefügt werden, werden in einer zukünftigen Version der Tools aktualisiert. 

### <a name="console-output-does-not-appear-in-visual-studios-output-window-while-debugging-a-console-application"></a>Die Konsolenausgabe wird im Ausgabefenster von Visual Studio nicht angezeigt, während für eine Konsolenanwendung das Debuggen durchgeführt wird.

Dies ist ein bekanntes Problem des Visual Studio Debugger (msvsmon.exe), der für diesen Fall derzeit nicht ausgelegt ist.  Wir arbeiten daran, dies in einer zukünftigen Version zu unterstützen.  Verwenden Sie die Option „Docker: Projekt starten“, die „Starten ohne Debugging“ entspricht, um die Ausgabe der Konsolenanwendung in Visual Studio anzuzeigen.

### <a name="debugging-web-applications-with-the-release-configuration-fails-with-403-forbidden-error"></a>Beim Debuggen von Webanwendungen mit der Versionskonfiguration tritt der Fehler „(403) Verboten“ auf.

Öffnen Sie zum Umgehen dieses Problems die Datei „web.release.config“ in der Projektmappe, und kommentieren Sie die folgenden Zeilen aus bzw. löschen Sie sie:

```
<compilation xdt:Transform="RemoveAttributes(debug)" />
```

### <a name="when-switching-to-windows-containers-you-could-potentially-see-an-error-dialog-stating-error-response-from-daemon-io-timeout"></a>Beim Wechseln zu Windows-Containern wird ggf. ein Fehlerdialogfeld mit der Meldung „Error response from daemon: i/o timeout“ (Fehlerantwort von Daemon: E/A-Timeout) angezeigt.

Sie können dieses Problem in Docker für Windows unter „https://github.com/docker/for-win/issues/178“ nachverfolgen.


## <a name="visual-studio-2015"></a>Visual Studio 2015

### <a name="linux-containers"></a>**Linux-Container**

### <a name="unable-to-validate-volume-mapping"></a>Volumezuordnung kann nicht überprüft werden
Die Volumezuordnung wird benötigt, um den Quellcode und die Binärdateien Ihrer Anwendung für den App-Ordner im Container freizugeben.  Spezifische Volumezuordnungen sind in den Dateien „docker-compose.dev.debug.yml“ und „docker-compose.dev.release.yml“ enthalten. Wenn Dateien auf dem Hostcomputer geändert werden, werden diese Änderungen in den Containern in einer ähnlichen Ordnerstruktur berücksichtigt.

Öffnen Sie zum Aktivieren der Volumezuordnung über das Wal-Taskleistensymbol von Docker für Windows die Option **Einstellungen...**, und wählen Sie dann die Registerkarte **Shared Drives** (Freigegebene Laufwerke) aus.  Vergewissern Sie sich, dass der Buchstabe des Laufwerks, das als Host für Ihr Projekt fungiert, und der Buchstabe des Laufwerks, auf dem sich %USERPROFILE% befindet, freigegeben sind, und klicken Sie dann auf **Übernehmen**.

Führen Sie zum Testen der Volumezuordnung nach der Freigabe von mindestens einem Laufwerk entweder eine Neuerstellung in Visual Studio durch, und drücken Sie F5, oder führen Sie an einer Eingabeaufforderung Folgendes aus:

*Windows-Eingabeaufforderung:*

> [!Note]
> In diesem Beispiel wird davon ausgegangen, dass sich Ihr Benutzerordner auf dem Laufwerk „C“ befindet und freigegeben wurde.
> Sollten Sie ein anderes Laufwerk freigegeben haben, aktualisieren Sie den Befehl ggf. entsprechend.

```
docker run -it -v /c/Users/Public:/wormhole busybox
```

*Linux-Container:*

```
/ # ls
```

Im Ordner „Users/Public“ sollte eine Verzeichnisliste angezeigt werden.
Wenn keine Dateien angezeigt werden und Ihr Ordner „/c/Users/Public“ nicht leer ist, ist die Volumezuordnung nicht ordnungsgemäß konfiguriert.

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

> [!Note]
> Bei der Arbeit mit virtuellen Linux-Computern muss im Containerdateisystem die Groß-/Kleinschreibung beachtet werden.

##<a name="build--prepareforbuild-task-failed-unexpectedly"></a>Build: Unerwarteter Fehler bei der PrepareForBuild-Aufgabe.

Microsoft.DotNet.Docker.CommandLine.ClientException: Fehler beim Herstellen der Verbindung:

Überprüfen Sie, ob der Docker-Standardhost in Betrieb ist. Öffnen Sie eine Eingabeaufforderung, und führen Sie Folgendes aus:

```
docker info
```

Wird hierbei ein Fehler zurückgegeben, versuchen Sie, die Desktop-App **Docker für Windows** zu starten.  Wenn die Desktop-App ausgeführt wird, sollte auf der Taskleiste das **Walsymbol** angezeigt werden. Klicken Sie mit der rechten Maustaste auf das Taskleistensymbol, und öffnen Sie **Einstellungen**.  Klicken Sie auf die Registerkarte **Zurücksetzen**, und starten Sie Docker neu.****

##<a name="manually-upgrading-from-version-031-to-040"></a>Manuelles Upgraden von Version 0.31 auf 0.40


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
1. Entfernen Sie die folgenden Zeilen aus „Properties\launchSettings.json“:

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

1. Deinstallieren Sie die frühere Version, installieren Sie Docker Tools 0.40, und klicken Sie dann im Kontextmenü für Ihre ASP.Net Core-Webanwendung oder -Konsolenanwendung erneut auf **Hinzufügen > Docker Support** (Docker-Unterstützung). Hierdurch werden die neuen erforderlichen Docker-Artefakte wieder Ihrem Projekt hinzugefügt.

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Fehler beim Verwenden von **Hinzufügen > Docker Support** (Docker-Unterstützung) oder beim Debuggen (F5) einer ASP.NET Core-Anwendung in einem Container

Gelegentlich hat das Deinstallieren und Installieren von Erweiterungen eine Beschädigung des MEF-Caches (Managed Extensibility Framework) von Visual Studio zur Folge. In diesem Fall können beim Hinzufügen der Docker-Unterstützung und/oder beim Ausführen oder Debuggen (F5) Ihrer ASP.NET Core-Anwendung verschiedene Fehlerdialogfelder angezeigt werden. Führen Sie als vorübergehende Problemumgehung die folgenden Schritte aus, um den MEF-Cache zu löschen und neu zu generieren:

1. Schließen Sie alle Instanzen von Visual Studio.
1. Öffnen Sie „%USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\“.
1. Löschen Sie die folgenden Ordner:
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Öffnen Sie Visual Studio.
1. Wiederholen Sie das Szenario.



<!--HONumber=Dec16_HO2-->


