---
title: Problembehandlung bei Docker-Clientfehlern unter Windows mit Visual Studio | Microsoft-Dokumentation
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
ms.sourcegitcommit: 649cc1a78f3a9f343533cb18fb7d763e4f9ea196
ms.openlocfilehash: 89fa04a1107b6abb49aefd68066443717ac9b731


---

# <a name="troubleshoot-visual-studio-docker-development"></a>Problembehandlung bei der Docker-Entwicklung in Visual Studio

Bei der Arbeit mit Visual Studio-Tools für Docker Preview können Probleme auftreten, die für Previews typisch sind.
Im Folgenden werden einige häufig auftretende Probleme und entsprechende Lösungen erörtert.  

## <a name="visual-studio-2017-rc"></a>Visual Studio 2017 RC

### <a name="linux-containers"></a>**Linux-Container**

####  <a name="build-errors-occur-when-debugging-a-net-core-web-or-console-application"></a>Buildfehler treten beim Debuggen einer .NET Core-Web- oder Konsolenanwendung auf.  

Dies kann damit zusammenhängen, dass das Laufwerk nicht freigegeben wird, wenn das Projekt unter Docker für Windows vorhanden ist.  Möglicherweise erhalten Sie etwa folgende Fehlermeldung:

```
The "PrepareForLaunch" task failed unexpectedly.
Microsoft.DotNet.Docker.CommandLineClientException: Creating network "webapplication13628050196_default" with the default driver
Building webapplication1
Creating webapplication13628050196_webapplication1_1
ERROR: for webapplication1  Cannot create container for service webapplication1: C: drive is not shared. Please share it in Docker for Windows Settings
```
So lösen Sie dieses Problem:

1. Klicken Sie im Benachrichtigungsbereich mit der rechten Maustaste auf **Docker für Windows**, und wählen Sie dann **Einstellungen**.  
2. Wählen Sie **Freigegebene Laufwerke**, und geben Sie das Laufwerk an, auf dem sich das Projekt befindet.

### <a name="windows-containers"></a>**Windows-Container**

Die folgenden Probleme sind spezifische Probleme des Debuggens von .NET Framework-Web- und -Konsolenanwendungen in Windows-Containern.

#### <a name="prerequisites"></a>Voraussetzungen

1. Visual Studio 2017 RC (oder höher) mit installierter .NET Core- und Docker Preview-Workload.
2. Windows 10 Anniversary Update mit aktuellen Windows-Updatepatches. Insbesondere [KB3194798](https://support.microsoft.com/en-us/help/3194798/cumulative-update-for-windows-10-version-1607-and-windows-server-2016-october-11,-2016) muss installiert sein. 
3. [Docker für Windows](https://docs.docker.com/docker-for-windows/) (Build 1.13.0 oder höher) muss installiert sein.
4. **Zu Windows-Containern wechseln** muss ausgewählt werden. Klicken Sie im Benachrichtigungsbereich auf **Docker für Windows**, und wählen Sie dann **Zu Windows-Containern wechseln**. Stellen Sie nach dem Neustart des Computers sicher, dass diese Einstellung beibehalten wird.

#### <a name="console-output-does-not-appear-in-visual-studios-output-window-while-debugging-a-console-application"></a>Die Konsolenausgabe wird im Ausgabefenster von Visual Studio nicht angezeigt, während für eine Konsolenanwendung das Debuggen durchgeführt wird.

Dies ist ein bekanntes Problem des Visual Studio Debugger (msvsmon.exe), der für diesen Fall derzeit nicht ausgelegt ist. Ein zukünftiges Release könnte dieses Szenario unterstützen. Verwenden Sie die Option **Docker: Projekt starten**, die **Starten ohne Debugging** entspricht, um die Ausgabe der Konsolenanwendung in Visual Studio anzuzeigen.

#### <a name="debugging-web-applications-with-the-release-configuration-fails-with-403-forbidden-error"></a>Beim Debuggen von Webanwendungen mit der Releasekonfiguration tritt der Fehler „(403) Verboten“ auf.

Öffnen Sie zum Umgehen dieses Problems die Datei „web.release.config“ in der Projektmappe, und kommentieren Sie die folgenden Zeilen aus, bzw. löschen Sie sie:

```
<compilation xdt:Transform="RemoveAttributes(debug)" />
```

## <a name="visual-studio-2015"></a>Visual Studio 2015

### <a name="linux-containers"></a>**Linux-Container**

#### <a name="unable-to-validate-volume-mapping"></a>Volumezuordnung kann nicht überprüft werden
Die Volumezuordnung wird benötigt, um den Quellcode und die Binärdateien Ihrer Anwendung für den App-Ordner im Container freizugeben.  Spezifische Volumezuordnungen sind in den Dateien „docker-compose.dev.debug.yml“ und „docker-compose.dev.release.yml“ enthalten. Wenn Dateien auf dem Hostcomputer geändert werden, werden diese Änderungen in den Containern in einer ähnlichen Ordnerstruktur berücksichtigt.

So aktivieren Sie die Volumezuordnung:

1. Klicken Sie im Infobereich auf **Moby**, und wählen Sie **Einstellungen**.
2. Wählen Sie **Freigegebene Laufwerke**.
3. Wählen Sie das Laufwerk, das Ihr Projekt hostet, und das Laufwerk, auf dem % USERPROFILE% gespeichert ist.
4. Klicken Sie auf **Übernehmen**.

Führen Sie zum Testen der Volumezuordnung nach der Freigabe von mindestens einem Laufwerk eine Neuerstellung in Visual Studio durch, und drücken Sie F5, oder führen Sie an einer Eingabeaufforderung den folgenden Code aus.

> [!NOTE]
> Dieses Beispiel setzt voraus, dass sich Ihr Benutzerordner auf dem Laufwerk C: befindet und freigegeben wurde.
> Sollten Sie ein anderes Laufwerk freigegeben haben, ändern Sie den Befehl ggf. entsprechend.

```
docker run -it -v /c/Users/Public:/wormhole busybox
```

Führen Sie den folgenden Code im Linux-Container aus.

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

> [!NOTE]
> Wenn Sie mit virtuellen Linux-Computern arbeiten, müssen Sie im Containerdateisystem die Groß-/Kleinschreibung beachten.

## <a name="build-prepareforbuild-task-failed-unexpectedly"></a>Build: Unerwarteter Fehler bei der PrepareForBuild-Aufgabe.

Microsoft.DotNet.Docker.CommandLine.ClientException: Fehler beim Herstellen der Verbindung.

Überprüfen Sie, ob der Docker-Standardhost in Betrieb ist. Öffnen Sie eine Eingabeaufforderung, und führen Sie Folgendes aus:

```
docker info
```

Wird hierbei ein Fehler zurückgegeben, versuchen Sie, die Desktop-App **Docker für Windows** zu starten. Wenn die Desktop-App ausgeführt wird, sollte im Benachrichtigungsbereich das **Walsymbol** angezeigt werden. Klicken Sie mit der rechten Maustaste auf das **Walsymbol**, und öffnen Sie **Einstellungen**. Klicken Sie auf **Zurücksetzen**, und starten Sie Docker neu.

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Bei dem Versuch, einer ASP.NET Core-Anwendung in einem Container Docker-Unterstützung hinzuzufügen oder zu debuggen (F5), wird ein Fehlerdialogfeld angezeigt.

Nach dem Deinstallieren und Installieren von Erweiterungen kann der MEF-Cache (Managed Extensibility Framework) in Visual Studio beschädigt werden. In diesem Fall können beim Hinzufügen der Docker-Unterstützung und/oder beim Ausführen oder Debuggen (F5) Ihrer ASP.NET Core-Anwendung verschiedene Fehlermeldungen angezeigt werden. Führen Sie als vorübergehende Problemumgehung die folgenden Schritte aus, um den MEF-Cache zu löschen und neu zu generieren.

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



<!--HONumber=Feb17_HO1-->


