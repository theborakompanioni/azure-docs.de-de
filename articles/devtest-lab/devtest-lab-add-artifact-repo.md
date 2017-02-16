---
title: "Hinzufügen eines Git-Artefaktrepositorys zu einem Lab in Azure DevTest Labs | Microsoft Docs"
description: "Fügen Sie ein GitHub- oder Visual Studio Team Services-Git-Repository für Ihre benutzerdefinierte Artefaktquelle in Azure DevTest Labs hinzu."
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 01b459f7-eaf2-45a8-b4b5-2c0a821b33c8
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: f7656382500682898cd3ed6372630afa3c3f6350
ms.openlocfilehash: 5c595bd2175f1cbc650bd1a757eeacc80cf39e23


---
# <a name="add-a-git-artifact-repository-to-a-lab-in-azure-devtest-labs"></a>Hinzufügen eines Git-Artefaktrepositorys zu einem Lab in Azure DevTest Labs
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-add-your-private-Artifacts-Repository-in-a-DevTest-Lab/player]
> 
> 

In Azure DevTest Labs sind Artefakte *Aktionen*, die beim Erstellen eines virtuellen Computers ausgeführt werden, z.B. das Installieren von Software oder das Ausführen von Skripts und Befehlen. Standardmäßig enthält ein Lab Artefakte aus dem offiziellen Azure DevTest Labs-Artefaktrepository. Sie können Ihrem Lab ein Git-Artefaktrepository hinzufügen, das die von Ihrem Team erstellten Artefakte enthalten soll. Das Repository kann auf [GitHub](https://github.com) oder bei [Visual Studio Team Services (VSTS)](https://visualstudio.com) gehostet werden.

* Anweisungen zum Erstellen eines GitHub-Repositorys finden Sie unter [GitHub Bootcamp](https://help.github.com/categories/bootcamp/)(in englischer Sprache).
* Anweisungen zum Erstellen eines Team Services-Projekts mit einem Git-Repository finden Sie unter [Connect to Visual Studio Team Services](https://www.visualstudio.com/get-started/setup/connect-to-visual-studio-online)(in englischer Sprache).

Der folgende Screenshot zeigt ein mögliches Beispiel für ein Repository mit Artefakten in GitHub:   
![Beispiel: GitHub-Artefaktrepository](./media/devtest-lab-add-artifact-repo/devtestlab-github-artifact-repo-home.png)

## <a name="get-the-repository-information-and-credentials"></a>Abrufen von Repository- und Anmeldeinformationen
Bevor Sie Ihrem Lab ein Artefaktrepository hinzufügen können, müssen Sie zunächst bestimmte Informationen aus dem Repository abrufen. In den folgenden Abschnitten wird beschrieben, wie Sie diese Informationen für Artefaktrepositorys abrufen, die auf GitHub und in Visual Studio Team Services gehostet sind.

### <a name="get-the-github-repository-clone-url-and-personal-access-token"></a>Abrufen der Klon-URL und des persönlichen Zugriffstokens für das GitHub-Repository
Führen Sie folgende Schritte aus, um die Klon-URL und das persönliche Zugriffstoken für das GitHub-Repository abzurufen:

1. Navigieren Sie zur Startseite des GitHub-Repositorys, das die Artefaktdefinitionen enthält.
2. Wählen Sie **Clone or download**aus.
3. Klicken Sie auf die Schaltfläche zum Kopieren der **HTTPS-Klon-URL** , und speichern Sie die URL zur späteren Verwendung.
4. Wählen Sie das Profilbild in der oberen rechten Ecke von GitHub aus, und wählen Sie **Settings**.
5. Wählen Sie im Menü **Personal settings** auf der linken Seite auf **Personal access tokens**.
6. Wählen Sie **Generate new token**.
7. Geben Sie auf der Seite **New personal access token** unter **Token description** eine Tokenbeschreibung ein, akzeptieren Sie die Standardelemente unter **Select scopes**, und wählen Sie dann **Generate Token** aus.
8. Speichern Sie das generierte Token, da Sie es später benötigen.
9. Sie können GitHub nun schließen.   
10. Fahren Sie mit dem Abschnitt [Verbinden des Labs mit dem Artefaktrepository](#connect-your-lab-to-the-artifact-repository) fort.

### <a name="get-the-visual-studio-team-services-repository-clone-url-and-personal-access-token"></a>Abrufen der Klon-URL und des persönlichen Zugriffstokens für das Visual Studio Team Services-Repository
Führen Sie folgende Schritte aus, um die Klon-URL und das persönlichen Zugriffstoken für das Visual Studio Team Services-Repository abzurufen:

1. Öffnen Sie die Startseite Ihrer Teamauflistung (z.B. `https://contoso-web-team.visualstudio.com`), und wählen Sie dann das Artefaktprojekt aus.
2. Wählen Sie auf der Startseite des Projekts **Code**.
3. Um die Klon-URL für das Projekt anzuzeigen, wählen Sie auf der Seite **Code** die Option **Klonen** aus.
4. Speichern Sie die URL, da Sie sie später in diesem Tutorial benötigen.
5. Um ein persönliches Zugriffstoken zu erstellen, wählen Sie im Dropdownmenü Ihres Benutzerkontos **Mein Profil** .
6. Wählen Sie auf der Seite mit den Profilinformationen **Sicherheit**.
7. Wählen Sie auf der Registerkarte **Sicherheit** die Option **Hinzufügen** aus.
8. Gehen Sie auf der Seite **Persönliches Zugriffstoken erstellen** folgendermaßen vor:
   
   * Geben Sie eine **Beschreibung** für das Token ein.
   * Wählen Sie **180 Tage** in der Liste **Läuft ab in** aus.
   * Wählen Sie **Alle verfügbaren Konten** in der Liste **Konten** aus.
   * Wählen Sie die Option **Alle Bereiche** aus.
   * Wählen Sie **Token erstellen**aus.
9. Nach Abschluss wird das neue Token in der Liste **Persönliche Zugriffstoken** angezeigt. Wählen Sie **Token kopieren**, und speichern Sie den Tokenwert für die spätere Verwendung.
10. Fahren Sie mit dem Abschnitt [Verbinden des Labs mit dem Artefaktrepository](#connect-your-lab-to-the-artifact-repository) fort.

## <a name="connect-your-lab-to-the-artifact-repository"></a>Verbinden des Labs mit dem Artefaktrepository
1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
2. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs** aus.
3. Wählen Sie in der Liste der Labs das gewünschte Lab aus.   
4. Wählen Sie auf dem Blatt des Labs **Konfiguration**aus.
5. Wählen Sie auf dem Blatt **Configuration** (Konfiguration) für das Lab die Option **Artifacts Repositories** (Artefaktrepositorys) aus.
6. Wählen Sie auf dem Blatt **Artifacts Repositories** (Artefaktrepositorys) die Option **+ Add** (+Hinzufügen).
   
    ![Schaltfläche zum Hinzufügen eines Artefaktrepositorys](./media/devtest-lab-add-artifact-repo/add-artifact-repo.png)
7. Geben Sie auf dem zweiten Blatt **Artifacts Repositories** (Artefaktrepositorys) Folgendes an:
   
   * **Name** : Geben Sie einen Namen für das Repository ein.
   * **Git Clone Url** (Git-Klon-URL): Geben Sie die Git-HTTPS-Klon-URL ein, die Sie zuvor aus GitHub oder Visual Studio Team Services kopiert haben. 
   * **Folder Path** (Ordnerpfad): Geben Sie den auf die Klon-URL bezogenen Pfad zu dem Ordner an, der die Artefaktdefinitionen enthält.
   * **Branch** (Verzweigung): Geben Sie die Verzweigung an, um zu Ihren Artefaktdefinitionen zu gelangen.
   * **Personal Access Token** (Persönliches Zugriffstoken): Geben Sie das persönliche Zugriffstoken ein, das sie zuvor aus GitHub oder Visual Studio Team Services abgerufen haben. 
     
     ![Blatt „Artefaktrepository“](./media/devtest-lab-add-artifact-repo/artifact-repo-blade.png)
8. Wählen Sie **Speichern**aus.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Verwandte Blogbeiträge
* [How to troubleshoot failing Artifacts in AzureDevTestLabs (Gewusst wie: Problembehandlung für fehlerhafte Artefakte in AzureDevTestLabs)](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs)
* [Join a VM to existing AD Domain using ARM template in Azure Dev Test Lab (Hinzufügen eines virtuellen Computers zu einer vorhandenen AD-Domäne mithilfe einer ARM-Vorlage in Azure Dev Test Lab)](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs)




<!--HONumber=Jan17_HO2-->


