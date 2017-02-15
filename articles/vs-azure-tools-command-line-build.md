---
title: "Build per Befehlszeile für Azure | Microsoft Docs"
description: "Build per Befehlszeile für Azure"
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 94b35d0d-0d35-48b6-b48b-3641377867fd
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e189bf8241266443bee4dbbb5faf1cffa983bdb4


---
# <a name="command-line-build-for-azure"></a>Build per Befehlszeile für Azure
## <a name="overview"></a>Übersicht
Sie können ein Paket für die Azure-Bereitstellung durch Ausführen von "MSBuild" an der Eingabeaufforderung erstellen. Sie können neben der Automatisierung eines Teils des Buildprozesses Builds für das Debugging, Staging und die Produktion konfigurieren und definieren.

## <a name="microsoft-build-engine-msbuild"></a>Microsoft-Buildmodul (MSBuild)
Mithilfe des Microsoft-Buildmoduls (MSBuild) können Sie Produkte in Testumgebungen für Builds erstellen, in denen Visual Studio nicht installiert ist. Von MSBuild wird ein XML-Format für Projektdateien verwendet, das erweiterbar ist und von Microsoft vollständig unterstützt wird. In diesem Dateiformat können Sie beschreiben, welche Elemente für eine oder mehrere Plattformen und Konfigurationen erstellt werden müssen.

Sie können MSBuild auch an der Eingabeaufforderung ausführen. Diese Methode wird in diesem Thema beschrieben. Durch Festlegen von Eigenschaften an der Eingabeaufforderung können Sie bestimmte Konfigurationen für ein Projekt erstellen. Entsprechend können Sie die Ziele definieren, die vom MSBuild-Befehl erstellt werden. Weitere Informationen zu Befehlszeilenparametern und MSBuild finden Sie in der [MSBuild-Befehlszeilenreferenz](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="installation"></a>Installation
Wie im folgenden Verfahren beschrieben, müssen Software und Tools auf dem Buildserver installiert werden, bevor Sie ein Azure-Paket mithilfe von MSBuild erstellen können:

1. Installieren Sie .NET Framework 4 oder höher. MSBuild ist darin enthalten.
2. Installieren Sie die [Azure Authoring Tools](http://go.microsoft.com/fwlink/?LinkId=394615) (suchen Sie nach "MicrosoftAzureAuthoringTools-x64.msi" oder "MicrosoftAzureAuthoringTools-x86.msi").
3. Installieren Sie die [Azure-Bibliotheken für .NET](http://go.microsoft.com/fwlink/?LinkId=394616) (suchen Sie nach "MicrosoftAzureLibsForNet-x64.msi" oder "MicrosoftAzureLibsForNet-x86.msi").
4. Kopieren Sie die Datei "Microsoft.WebApplication.targets" aus einer Visual Studio-Installation auf einem anderen Computer.
   
    Die Datei befindet sich im Verzeichnis "C:\Programme (x86)\MSBuild\Microsoft\Visual Studio\v12.0\WebApplications (v11.0 für Visual Studio 2012)". Kopieren Sie die Datei in dasselbe Verzeichnis auf dem Buildserver.
5. Installieren Sie die [Azure Tools für Visual Studio](http://go.microsoft.com/fwlink/?LinkId=394616).
   
    Suchen Sie nach "WindowsAzureTools.vs120.exe", um Visual Studio 2013-Projekte zu erstellen.

## <a name="msbuild-parameters"></a>MSBuild-Parameter
Die einfachste Möglichkeit zur Erstellung eines Pakets besteht im Ausführen von MSBuild mit der Option `/t:Publish`. Standardmäßig erstellt dieser Befehl ein Verzeichnis unter dem Stammordner des Projekts, z.B. \bin\\app.publish\. Beim Erstellen eines Azure-Projekts werden zwei Dateien generiert, die Paketdatei selbst und die zugehörige Konfigurationsdatei:

* Project.cspkg
* ServiceConfiguration.Zielprofil.cscfg

In der Standardeinstellung enthält jedes Azure-Projekt eine Dienstkonfigurationsdatei für lokale (Debugging-) Builds und eine für Cloudbuilds (Staging oder Produktion), aber Sie können Dienstkonfigurationsdateien nach Bedarf hinzufügen oder entfernen. Beim Erstellen eines Pakets in Visual Studio werden Sie gefragt, welche Dienstkonfigurationsdatei zusammen mit dem Paket enthalten sein soll. Beim Erstellen eines Pakets mithilfe von MSBuild ist standardmäßig die lokale Dienstkonfigurationsdatei enthalten. Damit eine andere Dienstkonfigurationsdatei enthalten ist, legen Sie die `TargetProfile`-Eigenschaft des MSBuild-Befehls folgendermaßen fest: `MSBuild /t:Publish /p:TargetProfile=ProfileName`.

Wenn Sie ein alternatives Verzeichnis zur Speicherung des Pakets und der Konfigurationsdateien verwenden möchten, legen Sie den Pfad mithilfe der Option `/p:PublishDir=Directory\` fest, einschließlich des nachgestellten umgekehrten Schrägstrichs als Trennzeichen.

## <a name="deployment"></a>Bereitstellung
Nach dem Erstellen des Pakets können Sie es in Azure bereitstellen. Ein Tutorial, das diesen Prozess veranschaulicht, finden Sie auf der Azure-Website. Weitere Informationen zur Automatisierung dieses Prozesses finden Sie unter [Kontinuierliche Bereitstellung für Clouddienste in Azure](cloud-services/cloud-services-dotnet-continuous-delivery.md).




<!--HONumber=Nov16_HO3-->


