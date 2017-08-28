---
title: "Build per Befehlszeile für Azure | Microsoft Docs"
description: "Build per Befehlszeile für Azure"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 94b35d0d-0d35-48b6-b48b-3641377867fd
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/05/2017
ms.author: kraigb
ms.translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: 5add8703b7b16ba8d9dc49f42f5e71b195c46653
ms.contentlocale: de-de
ms.lasthandoff: 03/27/2017

---
# <a name="building-azure-projects-from-the-command-line"></a>Erstellen von Azure-Projekten über die Befehlszeile
Mithilfe des Microsoft-Buildmoduls (MSBuild) können Sie Produkte in Build-Laborumgebungen erstellen, in denen Visual Studio nicht installiert ist. Von MSBuild wird ein XML-Format für Projektdateien verwendet, das erweiterbar ist und von Microsoft vollständig unterstützt wird. Mit dem MSBuild-Dateiformat können Sie beschreiben, welche Elemente für Plattformen und Konfigurationen erstellt werden müssen.

Sie können MSBuild auch über die Befehlszeile ausführen. Diese Methode wird in diesem Thema beschrieben. Durch Festlegen von Eigenschaften an der Befehlszeile können Sie bestimmte Konfigurationen für ein Projekt erstellen. Entsprechend können Sie die Ziele definieren, die von MSBuild erstellt werden. Weitere Informationen zu Befehlszeilenparametern und MSBuild finden Sie unter [MSBuild-Befehlszeilenreferenz](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="msbuild-parameters"></a>MSBuild-Parameter
Die einfachste Möglichkeit zur Erstellung eines Pakets besteht im Ausführen von MSBuild mit der Option `/t:Publish`. Standardmäßig erstellt dieser Befehl ein Verzeichnis unter dem Stammordner des Projekts, z.B. `<ProjectDirectory>\bin\Configuration\app.publish\`. Beim Erstellen eines Azure-Projekts werden zwei Dateien generiert, die Paketdatei selbst und die zugehörige Konfigurationsdatei:

* Paketdatei (`project.cspkg`)
* Konfigurationsdatei (`ServiceConfiguration.TargetProfile.cscfg`)

In der Standardeinstellung enthält jedes Azure-Projekt eine Dienstkonfigurationsdatei für lokale Builds (Debugging) und eine für Cloudbuilds (Staging oder Produktion). Sie können Dienstkonfigurationsdateien jedoch nach Bedarf hinzufügen oder entfernen. Wenn Sie ein Paket in Visual Studio erstellen, werden Sie gefragt, welche Dienstkonfigurationsdatei für das Paket eingeschlossen werden soll. Beim Erstellen eines Pakets mithilfe von MSBuild ist standardmäßig die lokale Dienstkonfigurationsdatei enthalten. Damit eine andere Dienstkonfigurationsdatei enthalten ist, legen Sie die `TargetProfile`-Eigenschaft des MSBuild-Befehls folgendermaßen fest: `MSBuild /t:Publish /p:TargetProfile=ProfileName`.

Wenn Sie ein alternatives Verzeichnis zur Speicherung des Pakets und der Konfigurationsdateien verwenden möchten, legen Sie den Pfad mithilfe der Option `/p:PublishDir=Directory\` fest, einschließlich des nachgestellten umgekehrten Schrägstrichs als Trennzeichen.

## <a name="next-steps"></a>Nächste Schritte
Nach dem Erstellen des Pakets können Sie es in Azure bereitstellen. Ein Tutorial zur Automatisierung dieses Prozesses finden Sie unter [Kontinuierliche Bereitstellung für Clouddienste in Azure](./cloud-services/cloud-services-dotnet-continuous-delivery.md).


