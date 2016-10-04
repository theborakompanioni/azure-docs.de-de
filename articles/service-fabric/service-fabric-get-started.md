<properties
   pageTitle="Einrichten der Entwicklungsumgebung | Microsoft Azure"
   description="Installieren Sie Laufzeit, SDK und Tools, und erstellen Sie einen lokalen Entwicklungscluster. Nach Abschluss des Setups können Sie mit der Erstellung von Clientanwendungen beginnen."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/13/2016"
   ms.author="ryanwi"/>

# Vorbereiten Ihrer Entwicklungsumgebung

> [AZURE.SELECTOR]
-[ Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Zum Entwickeln und Ausführen von [Azure Service Fabric-Anwendungen][1] auf dem Entwicklungscomputer müssen Sie die Laufzeit, das SDK und Tools installieren. Sie müssen auch die Ausführung der im SDK enthaltenen Windows PowerShell-Skripts aktivieren.

## Voraussetzungen
### Unterstützte Betriebssystemversionen
Die folgenden Betriebssystemversionen werden bei der Entwicklung unterstützt:

- Windows 7
- Windows 8/Windows 8.1
- Windows Server 2012 R2
- Windows 10

>[AZURE.NOTE] Windows 7 enthält standardmäßig nur Windows PowerShell 2.0. Für Service Fabric PowerShell-Cmdlets ist PowerShell 3.0 oder höher erforderlich. Sie können [Windows PowerShell 5.0][powershell5-download] aus dem Microsoft Download Center herunterladen.

## Installieren von Laufzeit, SDK und Tools

Der Webplattform-Installer bietet zwei Konfigurationen für die Service Fabric-Entwicklung:

- [Installieren der Service Fabric-Laufzeit, des SDK und der Tools für Visual Studio 2015 (Visual Studio 2015 Update 2 oder höher erforderlich)][full-bundle-vs2015]
- [Installieren der Service Fabric-Laufzeit und des SDKs (keine Visual Studio-Tools)][core-sdk]

## Aktivieren der PowerShell-Skriptausführung

Service Fabric verwendet Windows PowerShell-Skripts zum Erstellen eines lokalen Entwicklungsclusters und zum Bereitstellen von Anwendungen aus Visual Studio. Die Ausführung dieser Skripts wird von Windows standardmäßig blockiert. Um die Skripts zu aktivieren, müssen Sie die PowerShell-Ausführungsrichtlinie ändern. Öffnen Sie PowerShell als Administrator, und geben Sie folgenden Befehl ein:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## Nächste Schritte
Nachdem Sie die Entwicklungsumgebung eingerichtet haben, können Sie nun mit dem Erstellen und Ausführen von Apps beginnen.

- [Erstellen Ihrer ersten Service Fabric-Anwendung in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
- [Weitere Informationen zum Bereitstellen und Verwalten von Anwendungen in Ihrem lokalen Cluster](service-fabric-get-started-with-a-local-cluster.md)
- [Weitere Informationen zu Programmiermodellen: Reliable Services und Reliable Actors](service-fabric-choose-framework.md)
- [Service Fabric-Codebeispiele auf GitHub](https://aka.ms/servicefabricsamples)
- [Visualisieren des Clusters mit Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)
- [Folgen Sie dem Service Fabric-Lernpfad, um eine umfassende Einführung in die Plattform zu erhalten.](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/campaigns/service-fabric/ "Service Fabric-Kampagnenseite"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]: http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "WebPI-Link für VS 2015"
[full-bundle-dev15]: http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "WebPI-Link für Dev15"
[core-sdk]: http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "WebPI-Link für Core SDK"
[powershell5-download]: https://www.microsoft.com/de-DE/download/details.aspx?id=50395

<!---HONumber=AcomDC_0928_2016-->