---
title: Automatisieren der Bereitstellung der Azure-App mit Befehlszeilentools | Microsoft Docs
description: "Ermitteln von Informationen zum Bereitstellen der Azure-App über die Befehlszeile"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 8b65980c-eb75-44a2-8e0f-f9eb9e617d16
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 283b1cfda82b4f96ad5148c522a4c9833cb4c381
ms.openlocfilehash: e0e2e65557911bcac06d4dc355f47e9331934f8a


---
# <a name="automate-deployment-of-your-azure-app-with-command-line-tools"></a>Automatisieren der Bereitstellung der Azure-App mit Befehlszeilentools
Sie können die Bereitstellung von Azure-Apps mit Befehlszeilentools automatisieren. Dieser Artikel führt die verfügbaren Tools sowie nützliche Links auf, die zeigen, wie sie diese in Ihrem Bereitstellungsworkflow verwenden. 

## <a name="a-namemsbuildaautomate-deployment-with-msbuild"></a><a name="msbuild"></a>Automatisieren der Bereitstellung mit MSBuild
Wenn Sie die [Visual Studio IDE](#vs) für die Entwicklung verwenden, können Sie [MSBuild](http://msbuildbook.com/) für die Automatisierung sämtlicher Aufgaben nutzen, die Sie in Ihrer IDE ausführen können. Sie können MSBuild für die Verwendung von [Web Deploy](#webdeploy) oder [FTP/FTPS](#ftp) zum Kopieren der Dateien konfigurieren. Mit Web Deploy können auch viele andere Aufgaben, die mit der Bereitstellung in Zusammenhang stehen, automatisiert werden, wie beispielsweise das Bereitstellen von Datenbanken.

Weitere Informationen zur Befehlszeilen-Bereitstellung mithilfe von MSBuild finden Sie in den folgenden Ressourcen:

* [ASP.NET-Webbereitstellung mithilfe von Visual Studio: Befehlszeilenbereitstellung](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). Der zehnte Teil in einer Lernprogrammserie, in dem die Bereitstellung in Azure mithilfe von Visual Studio erläutert wird. In diesem Lernprogramm wird gezeigt, wie die Befehlszeile für die Bereitstellung verwendet wird, nachdem Veröffentlichungsprofile in Visual Studio eingerichtet wurden.
* [Inside the Microsoft Build Engine: Using MSBuild and Team Foundation Build (In der Microsoft Build Engine: Verwenden von MSBuild und Team Foundation Build,  in englischer Sprache)](http://msbuildbook.com/). Buch, in dem Kapitel zur Verwendung von MSBuild für die Bereitstellung enthalten sind.

## <a name="a-namepowershellaautomate-deployment-with-windows-powershell"></a><a name="powershell"></a>Automatisieren der Bereitstellung mit Windows PowerShell
Sie können MSBuild- oder FTP-Bereitstellungsfunktionen mithilfe von [Windows PowerShell](http://msdn.microsoft.com/library/dd835506.aspx)ausführen. Wenn dies der Fall ist, können Sie auch eine Sammlung von Windows PowerShell-Cmdlets nutzen, mit denen die Azure-REST-Verwaltungs-API leicht aufgerufen werden kann.

Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Bereitstellen einer Web-App in einem GitHub-Repository](app-service-web-arm-from-github-provision.md)
* [Bereitstellen einer Web-App mit einer SQL-Datenbank](app-service-web-arm-with-sql-database-provision.md)
* [Vorhersagbares Bereitstellen von Microservices in Azure](app-service-deploy-complex-application-predictably.md)
* [Building Real-World Cloud Apps with Azure - Automate Everything (Erstellen von Cloud-Anwendungen mit Azure – Automatisierung, in englischer Sprache)](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything). In diesem E-Book-Kapitel wird erklärt, wie die im E-Book dargestellte Beispielanwendung Windows PowerShell-Skripte nutzt, um eine Azure-Testumgebung zu erstellen und wie die Bereitstellung für diese Umgebung erfolgt. Im Abschnitt [Ressourcen](http://asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything#resources) finden Sie Links für die weiterführende Azure PowerShell-Dokumentation.
* [Veröffentlichung in Entwicklungs- und Testumgebungen mithilfe von Windows PowerShell-Skripts](../vs-azure-tools-publishing-using-powershell-scripts.md). Hier erfahren Sie, wie Sie Windows PowerShell-Bereitstellungsskripts verwenden, die von Visual Studio generiert werden.

## <a name="a-nameapiaautomate-deployment-with-net-management-api"></a><a name="api"></a>Automatisieren der Bereitstellung mit .NET Verwaltungs-API
Sie können C#-Code schreiben, um die MSBuild- oder FTP-Funktionen für die Bereitstellung auszuführen. Wenn Sie dies tun, können Sie auf die Azure-REST-Verwaltungs-API zugreifen, um die Website-Verwaltungsfunktionen auszuführen.

Weitere Informationen finden Sie in der folgenden Ressource:

* [Automating everything with the Azure Management Libraries and .NET (Automatisierung mithilfe der Azure-Verwaltungsbibliotheken und .NET, in englischer Sprache)](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Einführung in die .NET-Verwaltungs-API und Links für die weiterführende Dokumentation.

## <a name="a-namecliadeploy-from-azure-command-line-interface-azure-cli"></a><a name="cli"></a>Bereitstellen über die Azure-Befehlszeilenschnittstelle (Azure-CLI)
Über die Befehlszeile können Sie auf Windows-, Mac- oder Linux-Computern FTP-basierte Bereitstellungen durchführen. In diesem Fall können Sie über die Azure-Befehlszeilenschnittstelle auch auf die Azure-REST-Verwaltungs-API zugreifen.

Weitere Informationen finden Sie in der folgenden Ressource:

* [Azure-Befehlszeilentools](https://azure.microsoft.com/downloads/). Portalseite auf Azure.com mit Informationen zu Befehlszeilentools.

## <a name="a-namewebdeployadeploy-from-web-deploy-command-line"></a><a name="webdeploy"></a>Bereitstellen über die Web Deploy-Befehlszeile
Bei [Web Deploy](http://www.iis.net/downloads/microsoft/web-deploy) handelt es sich um eine Microsoft-Software für die Bereitstellung in IIS, wobei neben den Funktionen für die intelligente Dateisynchronisierung auch viele andere Aufgaben, die mit der Bereitstellung in Zusammenhang stehen und bei der Verwendung von FTP nicht automatisierbar sind, ausgeführt und koordiniert werden können. Mithilfe von Web Deploy können beispielsweise neue Datenbanken oder Datenbank-Updates in Ihrer Web-App bereitgestellt werden. Auch die Zeit, die zum Aktualisieren einer vorhandenen Website erforderlich ist, kann mit Web Deploy verringert werden, da dieses Programm auf intelligente Weise lediglich geänderte Dateien kopiert. Microsoft Visual Studio und Team Foundation Server verfügen über eine integrierte Unterstützung für Web Deploy. Sie können Web Deploy aber auch direkt über die Befehlszeile verwenden, um die Bereitstellung zu automatisieren. Die Web Deploy-Befehle sind äußerst leistungsstark, stellen aber einen gewaltigen Lernprozess dar.

## <a name="more-resources"></a>Weitere Ressourcen
Eine weitere Möglichkeit der Bereitstellung mit Automatisierung über die Befehlszeile ist die Nutzung eines cloudbasierten Diensts, z.B. [Octopus Deploy](http://en.wikipedia.org/wiki/Octopus_Deploy). Weitere Informationen finden Sie unter [Bereitstellen von ASP.NET-Anwendungen für Azure-Websites](https://octopusdeploy.com/blog/deploy-aspnet-applications-to-azure-websites).

Weitere Informationen zu Befehlszeilentools finden Sie in der folgenden Ressource:

* [Simple Web Apps: Deployment (Einfache Web-Apps: Bereitstellung, in englischer Sprache)](https://azure.microsoft.com/blog/2014/07/28/simple-azure-websites-deployment/). Blog von David Ebbo über ein von ihm geschriebenes Tool zur einfacheren Verwendung von Web Deploy.
* [Webbereitstellungstool](http://technet.microsoft.com/library/dd568996). Offizielle Dokumentation auf der Microsoft TechNet-Website. Der Inhalt ist nicht mehr auf dem neuesten Stand, aber für den Anfang gut geeignet.
* [Verwenden von Web Deploy](http://www.iis.net/learn/publish/using-web-deploy). Offizielle Dokumentation auf der Microsoft IIS.NET-Website. Der Inhalt ist auch nicht mehr auf dem neuesten Stand, aber für den Anfang gut geeignet.
* [ASP.NET-Webbereitstellung mithilfe von Visual Studio: Befehlszeilenbereitstellung](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/command-line-deployment). Bei MSBuild handelt es sich um das von Visual Studio verwendete Buildmodul, das auch über die Befehlszeile zum Bereitstellen von Webanwendungen in Web-Apps genutzt werden kann. Dieses Lernprogramm ist Bestandteil einer Serie, in der es in erster Linie um die Visual Studio-Bereitstellung geht.




<!--HONumber=Jan17_HO1-->


