---
title: Erstellen einer CI/CD-Pipeline in Azure mit Team Services | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie eine Visual Studio Team Services-Pipeline für Continuous Integration und Delivery erstellen, die eine Web-App in IIS auf einem virtuellen Windows-Computer bereitstellt."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: afa23b1395b8275e72048bd47fffcf38f9dcd334
ms.openlocfilehash: b4074c67c7a8214f8e103385d5aea75f019d2da6
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017

---

# <a name="create-a-continuous-integration-pipeline-with-visual-studio-team-services-and-iis"></a>Erstellen einer Continuous Integration-Pipeline mit Visual Studio Team Services und IIS
Sie können zum Automatisieren der Erstellungs-, Test- und Bereitstellungphasen der Anwendungsentwicklung eine Pipeline für Continuous Integration und Deployment (CI/CD) verwenden. In diesem Tutorial erstellen Sie eine CI/CD-Pipeline mit Visual Studio Team Services sowie einen virtuellen Windows-Computer in Azure, auf dem IIS ausgeführt wird. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Veröffentlichen einer ASP.NET-Webanwendung in einem Team Services-Projekt
> * Erstellen einer Builddefinition, die durch Codecommits ausgelöst wird
> * Installieren und Konfigurieren von IIS auf einem virtuellen Computer in Azure
> * Hinzufügen der IIS-Instanz zu einer Bereitstellungsgruppe in Team Services
> * Erstellen einer Releasedefinition zum Veröffentlichen neuer Webbereitstellungspakete in IIS
> * Testen der CI/CD-Pipeline

Für dieses Tutorial ist das Azure PowerShell-Modul Version 3.6 oder höher erforderlich. Führen Sie `Get-Module -ListAvailable AzureRM` aus, um die Version zu finden. Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Installieren und Konfigurieren von Azure PowerShell) Informationen dazu.


## <a name="create-project-in-team-services"></a>Erstellen eines Projekts in Team Services
Visual Studio Team Services ermöglicht eine einfache Zusammenarbeit und Entwicklung ohne Wartung einer lokalen Codeverwaltungslösung. Team Services ermöglicht Cloudcodetests und bietet Erkenntnisse zu Builds und Anwendungen. Sie können das Repository für die Versionskontrolle und eine IDE auswählen, die sich am besten für die Codeentwicklung eignen. Für dieses Tutorial können Sie ein kostenloses Konto nutzen, um eine grundlegende ASP.NET-Web-App und CI/CD-Pipeline zu erstellen. Falls Sie noch kein Team Services-Konto besitzen, [erstellen Sie eins](http://go.microsoft.com/fwlink/?LinkId=307137).

Erstellen Sie zum Verwalten des Codecommitprozesses, der Builddefinitionen und der Releasedefinitionen wie folgt ein Projekt in Team Services:

1. Öffnen Sie Ihr Team Services-Dashboard in einem Webbrowser, und wählen Sie **Neues Projekt**.
2. Geben Sie für **Projektname** den Namen *myWebApp* ein. Übernehmen Sie alle anderen Standardwerte, um die *Git*-Versionskontrolle und den Arbeitselementprozess *Agile* zu verwenden.
3. Wählen Sie die Option für die Freigabe für**** *Teammitglieder*, und wählen Sie anschließend **Erstellen**.
5. Nachdem das Projekt erstellt wurde, wählen Sie die Option **Initialize with a README or gitignore** (Mit einer INFODATEI oder gitignore initialisieren) und anschließend **Initialisieren**.
6. Wählen Sie im neuen Projekt im oberen Bereich **Dashboards** und anschließend **In Visual Studio öffnen**.


## <a name="create-aspnet-web-application"></a>Erstellen einer ASP.NET-Webanwendung
Im vorherigen Schritt haben Sie ein Projekt in Team Services erstellt. Im letzten Schritt wird Ihr neues Projekt in Visual Studio geöffnet. Sie verwalten Ihre Codecommits im **Team Explorer**-Fenster. Erstellen Sie eine lokale Kopie des neuen Projekts und anschließend wie folgt eine ASP.NET-Webanwendung aus einer Vorlage:

1. Wählen Sie **Klonen**, um ein lokales Git-Repository Ihres Team Services-Projekts zu erstellen.
    
    ![Klonen eines Repositorys aus dem Team Services-Projekt](media/tutorial-vsts-iis-cicd/clone_repo.png)

2. Wählen Sie unter **Projektmappen** die Option **Neu**.

    ![Erstellen der Projektmappe für die Webanwendung](media/tutorial-vsts-iis-cicd/new_solution.png)

3. Wählen Sie die ****Webvorlagen und dann die Vorlage für die **ASP.NET-Webanwendung** aus.
    1. Geben Sie einen Namen für Ihre Anwendung ein, z.B. *myWebApp*, und deaktivieren Sie das Kontrollkästchen für **Projektmappenverzeichnis erstellen**.
    2. Wenn die Option verfügbar ist, deaktivieren Sie das Kontrollkästchen **Application Insights zum Projekt hinzufügen**. Application Insights setzt voraus, dass Sie Ihre Webanwendung mit Azure Application Insights autorisieren. Überspringen Sie zur Vereinfachung dieses Tutorials diesen Prozess.
    3. Klicken Sie auf **OK**.
4. Wählen Sie in der Vorlagenliste **MVC** aus.
    1. Wählen Sie nacheinander **Authentifizierung ändern**, **Keine Authentifizierung** und **OK**.
    2. Wählen Sie **OK**, um die Projektmappe zu erstellen.
5. Wählen Sie im **Team Explorer**-Fenster die Option **Änderungen**.

    ![Übernehmen lokaler Änderungen am Team Services-Git-Repository](media/tutorial-vsts-iis-cicd/commit_changes.png)

6. Geben Sie in das Textfeld für den Commit eine Nachricht ein, beispielsweise *Commit initialisieren*. Wählen Sie im Dropdownmenü die Option **Commit für alle und Sync**.


## <a name="create-build-definition"></a>Erstellen einer Builddefinition
In Team Services geben Sie mithilfe einer Builddefinition an, wie Ihre Anwendung erstellt werden soll. In diesem Tutorial erstellen wir eine grundlegende Definition, die den Quellcode verwendet und die Projektmappe und dann das Webbereitstellungspaket erstellt, mit dem wir die Web-App auf einem IIS-Server ausführen können.

1. Wählen Sie in Ihrem Team Services-Projekt im oberen Bereich **Build und Release** und anschließend **Builds**.
3. Wählen Sie **+ Neue Definition**.
4. Wählen Sie die Vorlage **ASP.NET (VORSCHAU)** und anschließend **Übernehmen**.
5. Übernehmen Sie alle Standardaufgabenwerte. Vergewissern Sie sich unter **Get sources** (Quellen abrufen), dass das Repository *myWebApp* und die *Hauptverzweigung* ausgewählt sind.

    ![Erstellen einer Builddefinition im Team Services-Projekt](media/tutorial-vsts-iis-cicd/create_build_definition.png)

6. Schieben Sie auf der Registerkarte **Trigger** den Schieberegler für **Diesen Auslöser aktivieren** auf *Aktiviert*.
7. Speichern Sie die Builddefinition, und stellen Sie einen neuen Build in die Warteschlange, indem Sie **Speichern und in Warteschlange einreihen** und dann erneut **Speichern und in Warteschlange einreihen** auswählen. Übernehmen Sie die Standardwerte, und wählen Sie **Warteschlange**.

Sie können zusehen, wie der Build auf einem gehosteten Agent geplant und dann erstellt wird. Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

![Erfolgreiche Erstellung des Team Services-Projekts](media/tutorial-vsts-iis-cicd/successful_build.png)


## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers
Um eine Plattform für die Ausführung Ihrer ASP.NET-Web-App zur Verfügung zu stellen, benötigen Sie einen virtuellen Windows-Computer mit IIS. Team Services verwendet einen Agent für die Interaktion mit der IIS-Instanz, wenn Sie für Ihren Code einen Commit ausführen und Builds ausgelöst werden.

Erstellen Sie einen virtuellen Computer unter Windows Server 2016 mithilfe [dieses Skriptbeispiels](../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json). Es dauert einige Minuten, bis das Skript ausgeführt und der virtuelle Computer erstellt wird. Öffnen Sie nach der Erstellung des virtuellen Computers mithilfe von [Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermresourcegroup) Port 80 folgendermaßen für Webdatenverkehr:

```powershell
Get-AzureRmNetworkSecurityGroup `
  -ResourceGroupName $resourceGroup `
  -Name "myNetworkSecurityGroup" | `
Add-AzureRmNetworkSecurityRuleConfig `
  -Name "myNetworkSecurityGroupRuleWeb" `
  -Protocol "Tcp" `
  -Direction "Inbound" `
  -Priority "1001" `
  -SourceAddressPrefix "*" `
  -SourcePortRange "*" `
  -DestinationAddressPrefix "*" `
  -DestinationPortRange "80" `
  -Access "Allow" | `
Set-AzureRmNetworkSecurityGroup
```

Rufen Sie zum Herstellen einer Verbindung mit dem virtuellen Computer mithilfe von [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) wie folgt die öffentliche IP-Adresse ab:

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup | Select IpAddress
```

Erstellen Sie eine Remotedesktopsitzung für Ihren virtuellen Computer:

```cmd
mstsc /v:<publicIpAddress>
```

Öffnen Sie auf dem virtuellen Computer eine **PowerShell-Administratoreingabeaufforderung**. Installieren Sie wie folgt IIS und die erforderlichen .NET-Features:

```powershell
Install-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features
```


## <a name="create-deployment-group"></a>Erstellen einer Bereitstellungsgruppe
Zum Übertragen des Webbereitstellungspaket an den IIS-Server per Pushvorgang definieren Sie eine Bereitstellungsgruppe in Team Services. Mit dieser Gruppe können Sie angeben, welche Server als Ziel der neuen Builds verwendet werden, wenn Sie für Ihren Code einen Commit in Team Services ausführen und Builds abgeschlossen werden.

1. Wählen Sie in Team Services **Build und Release**, und wählen Sie dann **Bereitstellungsgruppen**.
2. Wählen Sie **Bereitstellungsgruppe hinzufügen**.
3. Geben Sie einen Namen für die Gruppe an, z.B. *myIIS*, und wählen Sie dann **Erstellen**.
4. Vergewissern Sie sich im Abschnitt **Computer registrieren**, dass *Windows* ausgewählt ist, und aktivieren Sie das Kästchen **Use a personal access token in the script for authentication** (Persönliches Zugriffstoken im Skript zur Authentifizierung verwenden).
5. Wählen Sie **Copy script to clipboard** (Skript in Zwischenablage kopieren).


### <a name="add-iis-vm-to-the-deployment-group"></a>Hinzufügen des virtuellen IIS-Computers zur Bereitstellungsgruppe
Fügen Sie nach Erstellung der Bereitstellungsgruppe die einzelnen IIS-Instanzen zur Gruppe hinzu. Team Services generiert ein Skript, das auf dem virtuellen Computer einen Agent herunterlädt und konfiguriert, der wiederum neue Webbereitstellungspakete empfängt und dann auf IIS anwendet.

1. Fügen Sie zurück in der **PowerShell-Administratorsitzung** auf dem virtuellen Computer das aus Team Services kopierte Skript ein, und führen Sie es aus.
2. Wenn Sie aufgefordert werden, Tags für den Agent zu konfigurieren, wählen Sie *J*, und geben Sie *Web* ein.
3. Wenn Sie zur Eingabe des Benutzerkontos aufgefordert werden, drücken Sie die EINGABETASTE**, um die Standardwerte zu übernehmen.
4. Warten Sie, bis die Ausführung des Skripts mit der Meldung *Service vstsagent.account.computername started successfully* (Dienst „vstsagent.account.computername“ wurde gestartet.) abgeschlossen wird.
5. Öffnen Sie im Menü **Build und Release** auf der Seite **Bereitstellungsgruppen** die Bereitstellungsgruppe *myIIS*. Überprüfen Sie auf der Registerkarte **Computer**, ob Ihr virtueller Computer aufgeführt ist.

    ![Der virtuelle Computer wurde zur Team Services-Bereitstellungsgruppe hinzugefügt.](media/tutorial-vsts-iis-cicd/deployment_group.png)


## <a name="create-release-definition"></a>Erstellen einer Releasedefinition
Erstellen Sie zum Veröffentlichen Ihrer Builds eine Releasedefinition in Team Services. Diese Definition wird bei der erfolgreichen Erstellung Ihrer Anwendung automatisch ausgelöst. Sie wählen die Bereitstellungsgruppe aus, an die Ihr Webbereitstellungspaket per Push übertragen werden soll, und definieren die entsprechenden IIS-Einstellungen.

1. Wählen Sie **Build und Release** und dann **Builds**. Wählen Sie die Builddefinition, die Sie in einem vorherigen Schritt erstellt haben.
2. Wählen Sie unter **Kürzlich abgeschlossen** den zuletzt abgeschlossenen Build aus, und wählen Sie dann **Release**.
3. Wählen Sie **Ja**, um eine Releasedefinition zu erstellen.
4. Wählen Sie die Vorlage **Leer** und dann **Weiter**.
5. Überprüfen Sie, ob das Projekt und die Quellbuilddefinition mit Ihrem Projekt ausgefüllt werden.
6. Aktivieren Sie das Kontrollkästchen **Continuous Deployment**, und wählen Sie dann **Erstellen**.
7. Wählen Sie das Dropdownfeld neben **+ Aufgaben hinzufügen** und dann *Bereitstellungsgruppenphase hinzufügen*.
    
    ![Hinzufügen einer Aufgabe zu einer Releasedefinition in Team Services](media/tutorial-vsts-iis-cicd/add_release_task.png)

8. Wählen Sie neben **IIS Web App Deploy(Preview)** (IIS-Web-App-Bereitstellung (Vorschau)) die Option **Hinzufügen** und danach **Schließen**.
9. Wählen Sie die übergeordnete Aufgabe **Für Bereitstellungsgruppe ausführen**.
    1. Wählen Sie für **Bereitstellungsgruppe** die zuvor erstellte Bereitstellungsgruppe, z.B. *myIIS*.
    2. Wählen Sie im Feld **Computertags** die Option **Hinzufügen** und dann das Tag *Web*.
    
    ![Bereitstellungsgruppe der Releasedefinition für IIS](media/tutorial-vsts-iis-cicd/release_definition_iis.png)
 
11. Wählen Sie die Aufgabe **Deploy: IIS Web App Deploy** (Bereitstellen: IIS-Web-App-Bereitstellung), um die Einstellungen der IIS-Instanz wie folgt zu konfigurieren:
    1. Geben Sie unter **Websitename** den Namen *Standardwebsite* ein.
    2. Übernehmen Sie alle anderen Standardeinstellungen.
12. Wählen Sie **Speichern** und anschließend zweimal **OK**.


## <a name="create-a-release-and-publish"></a>Erstellen und Veröffentlichen eines Release
Sie können nun Ihr Webbereitstellungspaket als neues Release übertragen. In diesem Schritt erfolgt die Kommunikation mit dem Agent der einzelnen Instanzen, die Teil der Bereitstellungsgruppe sind. Darüber hinaus wird das Webbereitstellungspaket übertragen und IIS zur Ausführung der aktualisierten Webanwendung konfiguriert.

1. Wählen Sie in Ihrer Releasedefinition **+ Release** und anschließend **Release erstellen**.
2. Stellen Sie sicher, dass in der Dropdownliste der aktuelle Build und **Automatisierte Bereitstellung: nach der Releaseerstellung** ausgewählt sind. Klicken Sie auf **Erstellen**.
3. Im oberen Bereich der Releasedefinition wird ein kleines Banner angezeigt, etwa *Release „Release-1“ wurde erstellt*. Wählen Sie den Releaselink aus.
4. Öffnen Sie die Registerkarte **Protokolle**, um den Releasestatus anzuzeigen.
    
    ![Erfolgreiche Übertragung des Team Services-Release und -Webbereitstellungspakets](media/tutorial-vsts-iis-cicd/successful_release.png)

5. Öffnen Sie nach Abschluss des Release einen Webbrowser, und geben Sie die öffentliche IP-Adresse Ihres virtuellen Computers ein. Ihre ASP.NET-Webanwendung wird ausgeführt.

    ![Auf dem virtuellen IIS-Computer ausgeführte ASP.NET-Web-App](media/tutorial-vsts-iis-cicd/running_web_app.png)


## <a name="test-the-whole-cicd-pipeline"></a>Testen der gesamten CI/CD-Pipeline
Ihre Webanwendung wird unter IIS ausgeführt. Testen Sie nun die gesamte CI/CD-Pipeline. Nachdem Sie eine Änderung in Visual Studio vorgenommen und für Ihren Code einen Commit ausgeführt haben, wird ein Build ausgelöst, was wiederum eine Veröffentlichung Ihres aktualisierten Webbereitstellungspakets in IIS auslöst:

1. Öffnen Sie in Visual Studio das Fenster **Projektmappen-Explorer**.
2. Navigieren Sie zu *myWebApp | Ansichten | Start | Index.cshtml*, und öffnen Sie die Datei.
3. Bearbeiten Sie die Zeile 6 wie folgt:

    `<h1>ASP.NET with VSTS and CI/CD!</h1>`

4. Speichern Sie die Datei.
5. Öffnen Sie das **Team Explorer**-Fenster, wählen Sie das Projekt *myWebApp* aus, und wählen Sie dann **Änderungen**.
6. Geben Sie eine Commitnachricht ein, z.B. *Testen der CI/CD-Pipeline*, und wählen Sie dann im Dropdownmenü **Commit für alle und Sync**.
7. In einem Team Services-Arbeitsbereich wird durch den Codecommit ein neuer Build ausgelöst. 
    - Wählen Sie **Build und Release** und dann **Builds**. 
    - Wählen Sie Ihre Builddefinition aus, und wählen Sie dann den Build **Queued & running** (In der Warteschlange und ausgeführt), um den Status des Builds zu überwachen.
9. Nachdem der Build erfolgreich erstellt wurde, wird ein neues Release ausgelöst.
    - Wählen Sie **Build und Release** und dann **Releases**, um das Webbereitstellungspaket per Push an den virtuellen IIS-Computer zu übertragen. 
    - Wählen Sie das Symbol **Aktualisieren**, um den Status zu aktualisieren. Wenn in der Spalte *Umgebungen* ein grünes Häkchen angezeigt wird, wurde das Release erfolgreich in IIS bereitgestellt.
11. Aktualisieren Sie die IIS-Website in einem Browser, um die Änderungen zu übernehmen.

    ![ASP.NET-Web-App, die über die CI/CD-Pipeline auf dem virtuellen IIS-Computer ausgeführt wird](media/tutorial-vsts-iis-cicd/running_web_app_cicd.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine ASP.NET-Webanwendung in Team Services erstellt und Build- und Releasedefinitionen konfiguriert, um mit jedem Codecommit neue Webbereitstellungspakete in IIS bereitzustellen. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Veröffentlichen einer ASP.NET-Webanwendung in einem Team Services-Projekt
> * Erstellen einer Builddefinition, die durch Codecommits ausgelöst wird
> * Installieren und Konfigurieren von IIS auf einem virtuellen Computer in Azure
> * Hinzufügen der IIS-Instanz zu einer Bereitstellungsgruppe in Team Services
> * Erstellen einer Releasedefinition zum Veröffentlichen neuer Webbereitstellungspakete in IIS
> * Testen der CI/CD-Pipeline

Folgen Sie diesem Link, um vordefinierte Skriptbeispiele für virtuelle Computer anzusehen.

> [!div class="nextstepaction"]
> [Virtueller Azure-Computer – PowerShell-Beispiele](./powershell-samples.md)
