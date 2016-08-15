<properties
	pageTitle="Hinzufügen eines Git-Artefaktrepositorys zu einem Lab | Microsoft Azure"
	description="Fügen Sie ein GitHub- oder Visual Studio Team Services-Git-Repository für Ihre benutzerdefinierten Artefakte in DevTest Labs hinzu."
	services="devtest-lab,virtual-machines,visual-studio-online"
	documentationCenter="na"
	authors="tomarcher"
	manager="douge"
	editor=""/>

<tags
	ms.service="devtest-lab"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/01/2016"
	ms.author="tarcher"/>

# Hinzufügen eines Git-Artefaktrepositorys zu einem Lab

> [AZURE.NOTE] Zeigen Sie das Video zu diesem Artikel an: [Hinzufügen eines privaten Artefaktrepositorys zu einem Lab in DevTest Labs](/documentation/videos/how-to-add-your-private-artifacts-repository-in-a-devtest-lab).

## Übersicht

Standardmäßig enthält ein Lab Artefakte aus dem offiziellen Azure DevTest Labs-Artefaktrepository. Sie können Ihrem Lab ein Git-Artefaktrepository hinzufügen, das die von Ihrem Team erstellten Artefakte enthalten soll. Das Repository kann auf [GitHub](https://github.com) oder bei [Visual Studio Team Services (VSTS)](https://visualstudio.com) gehostet werden.

- Anweisungen zum Erstellen eines GitHub-Repositorys finden Sie unter [GitHub Bootcamp](https://help.github.com/categories/bootcamp/) (in englischer Sprache).
- Anweisungen zum Erstellen eines Team Services-Projekts mit einem Git-Repository finden Sie unter [Connect to Visual Studio Team Services](https://www.visualstudio.com/get-started/setup/connect-to-visual-studio-online) (in englischer Sprache).

Der folgende Screenshot zeigt ein mögliches Beispiel für ein Repository mit Artefakten in GitHub: ![Beispiel: GitHub-Artefaktrepository](./media/devtest-lab-add-artifact-repo/devtestlab-github-artifact-repo-home.png)

## Hinzufügen eines GitHub-Artefaktrepositorys zu Ihrem Lab

Um Ihrem Lab ein GitHub-Artefaktrepository hinzuzufügen, rufen Sie zunächst die HTTPS-Klon-URL und das persönliche Zugriffstoken aus dem Artefaktrepository ab und geben diese Informationen dann im Lab ein.

### Abrufen der Klon-URL für das GitHub-Repository und des persönlichen Zugriffstokens

1. Speichern Sie auf der Startseite des GitHub-Repositorys, das die Teamartefakte enthält, die **HTTPS-Klon-URL** zur späteren Verwendung.

1. Wählen Sie das Profilbild in der oberen rechten Ecke aus, und wählen Sie **Settings**.

1. Wählen Sie im Menü **Personal settings** auf der linken Seite auf **Personal access tokens**.

1. Wählen Sie **Generate new token**.

1. Geben Sie auf der Seite **New personal access token** unter **Token description** eine Tokenbeschreibung ein, akzeptieren Sie die Standardelemente unter **Select scopes**, und wählen Sie dann **Generate Token** aus.

1. Speichern Sie das generierte Token, da Sie es später benötigen.

1. Sie können GitHub nun schließen.

###Verbinden Ihres Labs mit dem GitHub-Repository

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.

1. Wählen Sie **Durchsuchen** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.

1. Wählen Sie auf dem Blatt des Labs **Einstellungen**.

1. Wählen Sie auf dem Blatt **Einstellungen** für das Lab **Artifacts Repository**.

1. Gehen Sie auf dem Blatt **Artifacts Repository** folgendermaßen vor:

    1. Geben Sie einen **Namen** für das Repository ein.
    1. Geben Sie die gespeicherte **Git-Klon-URL** ein.
    2. Geben Sie den **Ordnerpfad** im Artefaktrepository ein, der die Artefakte enthält.
    3. Geben Sie das gespeicherte **Persönliche Zugriffstoken** für das Artefaktrepository ein.
    4. Wählen Sie **Speichern** aus.

Die Artefakte im Repository werden jetzt auf dem Blatt **Artefakte hinzufügen** aufgeführt.

## Hinzufügen eines Visual Studio Git-Artefaktrepositorys zu Ihrem Lab

Um Ihrem Lab ein Visual Studio Git-Artefaktrepository hinzuzufügen, rufen Sie zunächst die HTTPS-Klon-URL und das persönliche Zugriffstoken aus dem Artefaktrepository ab und geben diese Informationen dann im Lab ein.

### Visual Studio-Webseite des Artefaktprojekts

1. Öffnen Sie die Startseite Ihrer Teamauflistung (z.B. `https://contoso-web-team.visualstudio.com`), und wählen Sie dann das Artefaktprojekt aus.

2. Wählen Sie auf der Startseite des Projekts **Code**.

1. Um die Klon-URL für das Projekt anzuzeigen, wählen Sie auf der Seite **Code** **Klonen**.

1. Speichern Sie die URL, da Sie sie später in diesem Lernprogramm benötigen.

1. Um ein persönliches Zugriffstoken zu erstellen, wählen Sie im Dropdownmenü Ihres Benutzerkontos **Mein Profil**.

1. Wählen Sie auf der Seite mit den Profilinformationen **Sicherheit**.

1. Wählen Sie auf der Registerkarte **Sicherheit** **Hinzufügen**.

1. Gehen Sie auf der Seite **Persönliches Zugriffstoken erstellen** folgendermaßen vor:

    1. Geben Sie eine **Beschreibung** für das Token ein.
    2. Wählen Sie **180 Tage** in der Liste **Läuft ab in** aus.
    3. Wählen Sie **Alle verfügbaren Konten** in der Liste **Konten** aus.
    4. Wählen Sie die Option **Alle Bereiche** aus.
    5. Wählen Sie **Token erstellen** aus.

1. Nach Abschluss wird das neue Token in der Liste **Persönliche Zugriffstoken** angezeigt. Wählen Sie **Token kopieren**, und speichern Sie dann den Tokenwert, da er in Kürze verwendet wird.

### Im Lab

1. Wählen Sie auf dem Blatt des Labs **Einstellungen**.

    ![Einstellungen auswählen](./media/devtest-lab-add-artifact-repo/devtestlab-add-artifacts-repo-open-dtl-settings.png)

1. Wählen Sie auf dem Blatt **Einstellungen** **Artifacts Repository**.

1. Gehen Sie auf dem Blatt **Artifacts Repository** folgendermaßen vor:

    1. Geben Sie unter **Name** einen Anzeigenamen für das Repository ein.
    1. Geben Sie die gespeicherte **Git-Klon-URL** ein.
    2. Geben Sie den **Ordnerpfad** im Artefaktrepository ein, der die Artefakte enthält.
    3. Geben Sie das gespeicherte **Persönliche Zugriffstoken** für das Artefaktrepository ein.
    4. Wählen Sie **Speichern** aus.

## Verwandte Blogbeiträge
- [How to troubleshoot failing Artifacts in AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs) (Gewusst wie: Problembehandlung für fehlerhafte Artefakte in AzureDevTestLabs)
- [Join a VM to existing AD Domain using ARM template in Azure Dev Test Lab](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs) (Hinzufügen eines virtuellen Computers zu einer vorhandenen AD-Domäne mithilfe einer ARM-Vorlage in Azure Dev Test Lab)

<!---HONumber=AcomDC_0803_2016-->