---
title: "Einrichten einer Entwicklungsumgebung für Azure-Microservices | Microsoft-Dokumentation"
description: "Installieren Sie Laufzeit, SDK und Tools, und erstellen Sie einen lokalen Entwicklungscluster. Nach Abschluss des Setups können Sie mit der Erstellung von Clientanwendungen beginnen."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/07/2017
ms.author: ryanwi, mikhegn
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: 935b0ba8725b083878a0f9213e4fbd31bd27ab2c
ms.lasthandoff: 03/08/2017


---
# <a name="prepare-your-development-environment"></a>Vorbereiten Ihrer Entwicklungsumgebung
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

 Zum Entwickeln und Ausführen von [Azure Service Fabric-Anwendungen][1] müssen Sie auf dem Entwicklungscomputer die Laufzeit, das SDK und die Tools installieren. Sie müssen auch die Ausführung der im SDK enthaltenen Windows PowerShell-Skripts aktivieren.

## <a name="prerequisites"></a>Voraussetzungen
### <a name="supported-operating-system-versions"></a>Unterstützte Betriebssystemversionen
Die folgenden Betriebssystemversionen werden bei der Entwicklung unterstützt:

* Windows 7
* Windows 8/Windows 8.1
* Windows Server 2012 R2
* Windows Server 2016
* Windows 10

> [!NOTE]
> Windows 7 enthält standardmäßig nur Windows PowerShell 2.0. Für Service Fabric PowerShell-Cmdlets ist PowerShell 3.0 oder höher erforderlich. Sie können [Windows PowerShell 5.0][powershell5-download] aus dem Microsoft Download Center herunterladen.
> 
> 

## <a name="install-the-sdk-and-tools"></a>Installieren des SDKs und der Tools
### <a name="to-use-visual-studio-2017"></a>So verwenden Sie Visual Studio 2017
Service Fabric-Tools sind Bestandteil der Workload für die Azure-Entwicklung und -Verwaltung in Visual Studio 2017. Aktivieren Sie diese Workload im Rahmen der Visual Studio-Installation.
Zudem müssen Sie das Microsoft Azure Service Fabric SDK mithilfe des Webplattform-Installers installieren.

* [Installieren des Microsoft Azure Service Fabric-SDKs][core-sdk]

### <a name="to-use-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a>So verwenden Sie Visual Studio 2015 (erfordert mindestens Visual Studio 2015 Update 2)
Für Visual Studio 2015 werden Service Fabric-Tools zusammen mit dem SDK mithilfe des Webplattform-Installers installiert:

* [Installieren des Microsoft Azure Service Fabric-SDKs und der Tools][full-bundle-vs2015]

### <a name="sdk-installation-only"></a>Nur SDK-Installation
Wenn Sie nur das SDK benötigen, können Sie dieses Paket installieren:
* [Installieren des Microsoft Azure Service Fabric-SDKs][core-sdk]

> [!WARNING]
> Kunden haben uns darauf hingewiesen, dass bei Verwendung dieser Startlinks Installationsfehler auftreten können (insbesondere bei Verwendung des Chrome-Browsers). Diese Fehler des Webplattform-Installers sind bekannt und werden momentan behoben.  Probieren Sie die folgenden Problemumgehungen aus:
>- Verwenden Sie die obigen Links in Internet Explorer oder Edge. Oder:
>- Starten Sie den Webplattform-Installer über das Startmenü, suchen Sie nach „Service Fabric“, und installieren Sie das SDK.
> 
> Wir entschuldigen uns für die Unannehmlichkeiten. 

Aktuelle Versionen:
* Service Fabric SDK: 2.4.164
* Service Fabric-Runtime: 5.4.164
* Visual Studio 2015-Tools: 1.4.50124

Eine Liste mit unterstützten Versionen finden Sie unter [Service Fabric-Unterstützung](service-fabric-support.md).

## <a name="enable-powershell-script-execution"></a>Aktivieren der PowerShell-Skriptausführung
Service Fabric verwendet Windows PowerShell-Skripts zum Erstellen eines lokalen Entwicklungsclusters und zum Bereitstellen von Anwendungen aus Visual Studio. Die Ausführung dieser Skripts wird von Windows standardmäßig blockiert. Um die Skripts zu aktivieren, müssen Sie die PowerShell-Ausführungsrichtlinie ändern. Öffnen Sie PowerShell als Administrator, und geben Sie folgenden Befehl ein:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie die Entwicklungsumgebung eingerichtet haben, können Sie nun mit dem Erstellen und Ausführen von Apps beginnen.

* [Erstellen Ihrer ersten Service Fabric-Anwendung in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
* [Weitere Informationen zum Bereitstellen und Verwalten von Anwendungen in Ihrem lokalen Cluster](service-fabric-get-started-with-a-local-cluster.md)
* [Weitere Informationen zu Programmiermodellen: Reliable Services und Reliable Actors](service-fabric-choose-framework.md)
* [Service Fabric-Codebeispiele auf GitHub](https://aka.ms/servicefabricsamples)
* [Visualisieren des Clusters mit Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)
* [Folgen Sie dem Service Fabric-Lernpfad, um eine umfassende Einführung in die Plattform zu erhalten.](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* Informieren Sie sich über [Service Fabric-Supportoptionen](service-fabric-support.md).

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Service Fabric-Kampagnenseite"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "WebPI-Link für VS 2015"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "WebPI-Link für Dev15"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "WebPI-Link für Core SDK"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395

