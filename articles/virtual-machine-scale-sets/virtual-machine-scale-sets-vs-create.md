---
title: Bereitstellen einer VM-Skalierungsgruppe in Visual Studio | Microsoft Docs
description: Bereitstellen von VM-Skalierungsgruppen mit Visual Studio und einer Resource Manager-Vorlage
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ed0786b8-34b2-49a8-85b5-2a628128ead6
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: guybo
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 038259ad4e1c7c6623426f9476ea96d02043fbf9


---
# <a name="deploy-virtual-machine-scale-set-using-visual-studio"></a>Bereitstellen einer VM-Skalierungsgruppe in Visual Studio
In diesem Artikel erfahren Sie, wie eine Azure-VM-Skalierungsgruppe über eine Visual Studio-Ressourcengruppenbereitstellung bereitgestellt wird.

[Azure-VM-Skalierungsgruppen](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) sind eine Azure Compute-Ressource zur Bereitstellung und Verwaltung einer Auflistung ähnlicher virtueller Computer mit einfach integrierten Optionen für automatische Skalierung und Lastenausgleich. Sie können VM-Skalierungsgruppen über [ARM-Vorlagen (Azure-Ressourcen-Manager)](https://github.com/Azure/azure-quickstart-templates)bereitstellen. ARM-Vorlagen können mithilfe der Azure-Befehlszeilenschnittstelle, mit PowerShell, REST und auch direkt über Visual Studio bereitgestellt werden. Visual Studio bietet eine Reihe von Beispielvorlagen, die als Teil eines Azure-Projekts zur Ressourcengruppenbereitstellung bereitgestellt werden können.

Azure-Ressourcengruppenbereitstellungen bieten eine Möglichkeit, mehrere zusammengehörige Azure-Ressourcen in einem einzelnen Bereitstellungsvorgang zusammenzufassen und zu veröffentlichen. Informationen dazu finden Sie hier: [Erstellen und Bereitstellen von Azure-Ressourcengruppen mit Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)

## <a name="pre-requisites"></a>Voraussetzungen
Um mit der Bereitstellung von VM-Skalierungsgruppen in Visual Studio zu beginnen, benötigen Sie Folgendes:

* Visual Studio 2013 oder 2015
* Azure SDK 2.7, 2.8 oder 2.9

Hinweis: In diesen Anweisungen wird davon ausgegangen, dass Sie Visual Studio 2015 mit [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/)verwenden.

## <a name="creating-a-project"></a>Erstellen eines Projekts
1. Erstellen Sie ein neues Projekt in Visual Studio 2015 durch Auswählen von **Datei | Neu | Projekt**.
   
    ![Datei - Neu][file_new]
2. Wählen Sie unter **Visual C# | Cloud** die Option **Azure Resource Manager** aus, um ein Projekt zur Bereitstellung einer ARM-Vorlage zu erstellen.
   
    ![Projekt erstellen][create_project]
3. Wählen Sie aus der Liste der Vorlagen die VM-Skalierungsgruppenvorlage für Linux oder Windows aus.
   
   ![Vorlage auswählen][select_Template]
4. Nachdem das Projekt erstellt wurde, werden PowerShell-Bereitstellungsskripts, eine Azure-Ressourcen-Manager-Vorlage und eine Parameterdatei für die VM-Skalierungsgruppe angezeigt.
   
    ![Projektmappen-Explorer][solution_explorer]

## <a name="customize-your-project"></a>Anpassen Ihres Projekts
Jetzt können Sie die Vorlage an die Anforderungen Ihrer Anwendung anpassen, beispielsweise VM-Erweiterungseigenschaften hinzufügen oder Lastenausgleichsregeln bearbeiten. Die VM-Skalierungsgruppenvorlagen sind standardmäßig so konfiguriert, dass die AzureDiagnostics-Erweiterung bereitgestellt wird. Auf diese Weise können ganz einfach Regeln zur automatischen Skalierung hinzugefügt werden. Zudem wird ein Lastenausgleichsmodul mit einer öffentlichen IP-Adresse bereitgestellt, das mit eingehenden NAT-Regeln konfiguriert ist, die Ihnen eine Verbindung der VM-Instanzen mit SSH (Linux) oder RDP (Windows) erlauben. Der Front-End-Portbereich beginnt bei 50000, wodurch Sie im Fall von Linux bei einer Verbindung per SSH mit Port 50000 der öffentlichen IP-Adresse (oder des Domänennamens) an Port 22 des ersten virtuellen Computers in der Skalierungsgruppe weitergeleitet werden. Durch Herstellen einer Verbindung mit Port 50001 werden Sie an Port 22 des zweiten virtuellen Computers weitergeleitet usw.

 Eine gute Möglichkeit zum Bearbeiten Ihrer Vorlagen mit Visual Studio besteht darin, die JSON-Gliederung zum Organisieren der Parameter, Variablen und Ressourcen einzusetzen. Mit Kenntnis des Schemas kann Visual Studio Fehler in der Vorlage aufzeigen, bevor Sie sie bereitstellen.

![JSON-Explorer][json_explorer]

## <a name="deploy-the-project"></a>Bereitstellen des Projekts
1. Stellen Sie die ARM-Vorlage in Azure bereit, um die VM-Skalierungsgruppenressource zu erstellen. Klicken Sie mit der rechten Maustaste auf den Projektknoten, und wählen Sie **Bereitstellen | Neue Bereitstellung**aus.
   
    ![Vorlage bereitstellen][5deploy_Template]
2. Wählen Sie Ihr Abonnement im Dialogfeld „Für Ressourcengruppe bereitstellen“.
   
    ![Vorlage bereitstellen][6deploy_Template]
3. Von hier aus können Sie auch eine neue Azure-Ressourcengruppe zum Bereitstellen Ihrer Vorlage erstellen.
   
    ![Neue Ressourcengruppe][new_resource]
4. Klicken Sie als Nächstes auf die Schaltfläche **Parameter bearbeiten**, um Parameter einzugeben, die an die Vorlage übergeben werden. Bestimmte Werte, wie z.B. der Benutzername und das Kennwort für das Betriebssystem, sind zum Erstellen der Bereitstellung erforderlich. Falls die PowerShell-Tools für Visual Studio nicht installiert sind, empfiehlt es sich, das Kontrollkästchen „Kennwörter speichern“ zu aktivieren, um eine ausgeblendete PowerShell-Eingabeaufforderung zu vermeiden. Alternativ können Sie auch die [Key Vault-Unterstützung](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/) verwenden.
   
    ![Parameter bearbeiten][edit_parameters]
5. Klicken Sie jetzt auf **Bereitstellen**. Das Fenster **Ausgabe** zeigt den Fortschritt der Bereitstellung. Beachten Sie, dass bei der Aktion das Skript **Deploy-AzureResourceGroup.ps1** ausgeführt wird.
   
   ![Ausgabefenster][output_window]

## <a name="exploring-your-vm-scale-set"></a>Untersuchen der VM-Skalierungsgruppe
Nachdem die Bereitstellung abgeschlossen ist, sehen Sie die neue VM-Skalierungsgruppe im **Cloud-Explorer** von Visual Studio (aktualisieren Sie die Liste). Mit dem Cloud-Explorer können Sie Azure-Ressourcen in Visual Studio verwalten und gleichzeitig Anwendungen entwickeln. Sie können außerdem Ihre VM-Skalierungsgruppe im [Azure-Portal](https://portal.azure.com) und im [Azure-Ressourcen-Explorer](https://resources.azure.com/) anzeigen.

![Cloud-Explorer][cloud_explorer]

 Das Portal bietet die beste Möglichkeit, Ihre Azure-Infrastruktur mit einem Webbrowser visuell zu verwalten, während der Azure-Ressourcen-Explorer eine einfache Möglichkeit zum Untersuchen und Debuggen von Azure-Ressourcen bietet. Er bietet Einblicke in die „Instanzsicht“ und führt PowerShell-Befehle für die Ressourcen auf, die Sie gerade anzeigen. Während sich VM-Skalierungsgruppen in der Vorschau befinden, zeigt der Ressourcen-Explorer die meisten Details für Ihre VM-Skalierungsgruppen an.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie VM-Skalierungsgruppen erfolgreich über Visual Studio bereitgestellt haben, können Sie Ihr Projekt den Anforderungen der Anwendung entsprechend weiter anpassen. Beispielsweise können Sie die automatische Skalierung einrichten, indem Sie eine Insights-Ressource hinzufügen, Ihrer Vorlage eine Infrastruktur wie z. B. eigenständige VMs hinzufügen oder Anwendungen über die benutzerdefinierte Skripterweiterung bereitstellen. Eine gute Quelle für Beispielvorlagen ist das GitHub-Repository für [Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates) (suchen Sie nach „vmss“).

[file_new]: ./media/virtual-machine-scale-sets-vs-create/1-FileNew.png
[create_project]: ./media/virtual-machine-scale-sets-vs-create/2-CreateProject.png
[select_Template]: ./media/virtual-machine-scale-sets-vs-create/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machine-scale-sets-vs-create/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machine-scale-sets-vs-create/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/6-DeployTemplate.png
[new_resource]: ./media/virtual-machine-scale-sets-vs-create/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machine-scale-sets-vs-create/8-EditParameter.png
[output_window]: ./media/virtual-machine-scale-sets-vs-create/9-Output.png
[cloud_explorer]: ./media/virtual-machine-scale-sets-vs-create/12-CloudExplorer.png



<!--HONumber=Feb17_HO3-->


