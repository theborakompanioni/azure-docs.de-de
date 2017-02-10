---
title: Problembehandlung beim Azure IoT Intel Edison Starter Kit | Microsoft Docs
description: "Problembehandlungsseite für die C-Umgebung des Intel Edison"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Problembehandlung für Arduino"
ms.assetid: fe20f2fe-490c-4910-82e1-578ed504ae86
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/8/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: f45b3bf00d619376ac07418f0c02eca5f3241939
ms.openlocfilehash: c4282c89ad0e65c930d51c3456722afd5ebbc905


---
# <a name="troubleshooting"></a>Problembehandlung
## <a name="hardware-issues"></a>Hardwareprobleme
Informationen zum Beheben von gängigen Problemen auf dem Intel Edison finden Sie auf der [offiziellen Problembehandlungsseite](https://software.intel.com/en-us/node/637974).

## <a name="nodejs-package-issues"></a>Probleme mit Node.js-Paketen
### <a name="no-response-during-gulp-tasks"></a>Keine Antwort während Gulp-Tasks
Wenn Sie während der Ausführung von Gulp-Tasks auf Probleme stoßen, können Sie die Option `--verbose` für das Debuggen hinzufügen. Versuchen Sie, laufende Gulp-Tasks mit `Ctrl + C` zu beenden, und führen Sie dann den folgenden Befehl in Ihrem Konsolenfenster aus, um Debugmeldungen anzuzeigen. In der Konsolenausgabe werden unter Umständen detaillierte Fehlermeldungen angezeigt. 

```bash
gulp --verbose
```

### <a name="npm-issues"></a>NPM-Probleme
Führen Sie den folgenden Befehl aus, um das NPM-Paket zu aktualisieren:

```bash
npm install -g npm
```

Wenn das Problem weiterhin besteht, schreiben Sie am Ende dieses Artikels einen Kommentar, oder erstellen Sie in unserem [Beispielrepository][sample-repository] ein GitHub-Problem.

## <a name="azure-cli-issues"></a>Probleme mit der Azure-Befehlszeilenschnittstelle
Die Azure-Befehlszeilenschnittstelle (Azure CLI) ist ein Vorschaubuild. Sie können im [Installationshandbuch für Vorabversionen](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) nach Lösungen suchen. Aktualisieren Sie die Azure-Befehlszeilenschnittstelle auf die neueste Version, falls die Befehle nicht wie erwartet funktionieren.

Wenn mit dem Tool Fehler auftreten, legen Sie einen [Eintrag](https://github.com/Azure/azure-cli/issues) im Bereich **Probleme** des GitHub Repositorys an.

Für Unterstützung bei der Behebung von gängigen Problemen, lesen Sie [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).

Wenn in einem Fehler angezeigt wird, dass keine Version gefunden werden konnte, die die Anforderung erfüllt, führen Sie den folgenden Befehl aus, um pip auf die neueste Version zu aktualisieren.

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a>Probleme bei der Installation von Python
### <a name="legacy-installation-issues-macos"></a>Probleme bei der Vorgänger-Installation (MacOS)
Bei der Installation von **pip** tritt ein Berechtigungsfehler auf, wenn frühere Pakete vorhanden sind, die mit **su**-Berechtigungen installiert wurden. Diese Situation tritt auf, wenn eine frühere Installation von Python mithilfe von brew (MacOS) nicht vollständig deinstalliert wurde. Einige **pip**-Pakete aus einer früheren Installation wurden durch root erstellt, was den Berechtigungsfehler verursacht. Die Lösung ist, die Pakete zu entfernen, die durch root erstellt wurden. Führen Sie die folgenden Schritte aus, um dies abzuschließen:

1. Wechseln Sie zu „/usr/local/lib/python2.7/site-packages“.
2. Listen Sie die Pakete auf, die durch root erstellt wurden: `ls -l | grep root`
3. Deinstallieren Sie die Pakete aus Schritt 2: `sudo rm -rf {package name}`
4. Installieren Sie Python neu.

## <a name="azure-iot-hub-issues"></a>Probleme mit Azure IoT Hub
Wenn Sie Azure IoT Hub erfolgreich per `azure-cli` bereitgestellt haben und ein Tool benötigen, um die Geräte zu verwalten, die eine Verbindung mit IoT Hub herstellen, können Sie die unten angegebenen Tools ausprobieren:

### <a name="device-explorer"></a>Geräte-Explorer
[Geräte-Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) wird auf Ihrem lokalen Windows-Computer ausgeführt und stellt eine Verbindung mit IoT-Hub in Azure her. Er kommuniziert mit den folgenden [IoT Hub-Endpunkten](iot-hub-devguide.md):

- _Geräteidentitätsverwaltung_ zum Bereitstellen und Verwalten von Geräten, die bei IoT Hub registriert sind.
- _Empfangen von Gerät-zu-Cloud_, damit Sie von Ihrem Gerät an IoT Hub gesendete Nachrichten überwachen können.
- _Senden von Cloud-zu-Gerät_ ermöglicht es Ihnen, von Ihren Geräten Nachrichten an IoT Hub zu senden.

Konfigurieren Sie `IoT hub connection string` innerhalb dieses Tools, um all seine Eigenschaften zu verwenden.

### <a name="iot-hub-explorer"></a>ioT Hub-Explorer
[iothub-explorer](https://github.com/Azure/iothub-explorer) ist ein Beispiel für CLI-Tools für mehrere Plattformen, um Geräteclients zu verwalten. Mit dem Tool können Sie Geräte im Identitätsregister verwalten, Gerät-zu-Cloud-Nachrichten überwachen und Cloud-zu-Gerät-Befehle senden.

Um die aktuellste Version (Vorabversion) des iothub-explorer-Tools zu installieren, führen Sie den folgenden Befehl in Ihrer Befehlszeilenumgebung aus:

```bash
npm install -g iothub-explorer@latest
```

Sie können den folgenden Befehl verwenden, um weitere Hilfe zu allen iothub-explorer-Befehlen und ihren Parametern zu erhalten:

```bash
iothub-explorer help
```

### <a name="azure-portal"></a>Azure-Portal
Sie können eine vollständige CLI-Oberfläche nutzen, um alle Azure-Ressourcen zu erstellen und zu verwalten. Sie können auch das [Azure-Portal](../azure-portal-overview.md) nutzen, um das Bereitstellen, Verwalten, und Debuggen Ihrer Azure-Ressourcen zu unterstützen.

## <a name="azure-storage-issues"></a>Probleme mit Azure Storage
[Microsoft Azure-Speicher-Explorer (Vorschau)](http://storageexplorer.com) ist eine eigenständige App von Microsoft, mit der Sie unter Windows, macOS und Linux mit [Azure Storage](https://azure.microsoft.com/en-us/services/storage/)-Daten arbeiten können. Mit diesem Tool können Sie eine Verbindung mit Ihrer Tabelle herstellen und die Daten darin anzeigen. Mithilfe dieses Tools können Sie Probleme mit Azure Storage beheben.

## <a name="next-steps"></a>Nächste Schritte
Diese Seite enthält nur die am häufigsten auftretenden Probleme mit dem Intel Edison Kit. Sie können zudem unten einen Kommentar schreiben, um Probleme zur weiteren Problembehandlung zu melden.

Zurück zu [Erste Schritte mit Intel Edison (C)](iot-hub-intel-edison-kit-c-get-started.md)

<!-- Images and links -->

[sample-repository]: https://github.com/Azure-Samples/iot-hub-c-edison-getting-started


<!--HONumber=Dec16_HO2-->


