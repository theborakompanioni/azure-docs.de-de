---
title: Installieren und Konfigurieren von Azure PowerShell
description: Erfahren Sie, wie Sie Azure PowerShell installieren und konfigurieren.
editor: tysonn
manager: dongill
documentationcenter: ''
services: ''
author: coreyp-at-msft

ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 04/22/2016
ms.author: coreyp

---
# Installieren und Konfigurieren von Azure PowerShell
<div class="dev-center-tutorial-selector sublanding"><a href="/manage/install-and-configure-windows-powershell/" title="PowerShell" class="current">PowerShell</a><a href="/manage/install-and-configure-cli/" title="Azure-Befehlszeilenschnittstelle">Azure-Befehlszeilenschnittstelle</a></div>

## Was ist Azure PowerShell?
Azure PowerShell ist ein Satz von Modulen, die Cmdlets zum Verwalten von Azure mit Windows PowerShell enthält. Sie können diese Cmdlets verwenden, um durch die Azure-Plattform bereitgestellte Lösungen und Dienste zu erstellen, zu testen, bereitzustellen und zu verwalten. In den meisten Fällen können die Cmdlets für die gleichen Aufgaben wie das Azure-Verwaltungsportal verwendet werden, z. B. zum Erstellen und Konfigurieren von Clouddiensten, virtuellen Maschinen, virtuellen Netzwerken und Web-Apps.

<a id="Install"></a>

## Schritt 1: Installieren
Es folgen die beiden Methoden, mit denen Sie Azure PowerShell installieren können. Sie können sie entweder per WebPI oder über den PowerShell-Katalog installieren:

### Installieren von Azure PowerShell per WebPI
Die Installation von Azure PowerShell 1.0 und höher per WebPI ist mit der Installation der Version 0.9.x identisch. Laden Sie [Azure PowerShell](http://aka.ms/webpi-azps) herunter, und starten Sie die Installation. Falls Sie Azure PowerShell 0.9.x installiert haben, wird Version 0.9.x im Rahmen des Upgrades deinstalliert. Wenn Sie Azure PowerShell-Module aus dem PowerShell-Katalog installiert haben, werden die Module vor der Installation automatisch vom Installationsprogramm entfernt, um die Einheitlichkeit der Azure PowerShell-Umgebung sicherzustellen.

> [!NOTE]
> Wenn Sie bereits Azure-Module aus dem PowerShell-Katalog installiert haben, entfernt das Installationsprogramm diese automatisch. Dies vermeidet Verwirrung in Bezug darauf, welche Modulversionen Sie installiert haben und wo sich diese befinden. Module des PowerShell-Katalogs werden normalerweise unter **%Programme%\WindowsPowerShell\Modules** installiert. Im Gegensatz dazu installiert das WebPI-Installationsprogramm die Azure-Module unter „**%ProgramFiles(x86)%\Microsoft SDKs\Azure\PowerShell**“. Wenn während der Installation ein Fehler auftritt, können Sie die Azure*-Ordner aus Ihrem Ordner **%ProgramFiles%\WindowsPowerShell\Modules** entfernen und die Installation erneut durchführen.
> 
> 

Nach Abschluss der Installation sollte Ihre ```$env:PSModulePath```-Einstellung die Verzeichnisse mit den Azure PowerShell-Cmdlets enthalten.

> [!NOTE]
> Es gibt ein bekanntes Problem beim PowerShell-Element **$env:PSModulePath**, das bei der Installation per WebPI auftreten kann. Wenn Ihr Computer aufgrund von Systemupdates oder anderen Installationen neu gestartet werden muss, kann dies dazu führen, dass **$env:PSModulePath**aktualisiert wird und nicht mehr den Pfad enthält, in dem Azure PowerShell installiert ist. In diesem Fall wird beim Versuch, nach der Installation oder Aktualisierung Azure PowerShell-Cmdlets zu verwenden, möglicherweise eine „Cmdlet nicht erkannt“-Meldung angezeigt. Durch einen Neustart des Computers sollte das Problem behoben werden.
> 
> 

Wenn beim Laden oder Ausführen des Cmdlets eine Fehlermeldung wie die folgende angezeigt wird:

```
    PS C:\> Get-AzureRmResource
    Get-AzureRmResource : The term 'Get-AzureRmResource' is not recognized as the name of a cmdlet, function,
    script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is
    correct and try again.
    At line:1 char:1
    + Get-AzureRmResource
    + ~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (get-azurermresourcefork:String) [], CommandNotFoundException
        + FullyQualifiedErrorId : CommandNotFoundException
```

Das Problem kann durch einen Neustart des Computers oder durch Importieren der Cmdlets aus „C:\Programme\WindowsPowerShell\Modules\Azure\XXXX\“ (XXXX ist die installierte PowerShell-Version) behoben werden. Importieren Sie die Cmdlets wie folgt:

```
import-module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\azure.psd1"
import-module "C:\Program Files\WindowsPowerShell\Modules\Azure\XXXX\expressroute\expressroute.psd1"
```

### Installieren von Azure PowerShell aus dem PowerShell-Katalog
Installieren Sie Azure PowerShell 1.3.0 oder höher aus dem PowerShell-Katalog über eine Windows PowerShell- oder PowerShell Integrated Scripting Environment (ISE)-Eingabeaufforderung mit erhöhten Rechten mit den folgenden Befehlen:

    # Install the Azure Resource Manager modules from the PowerShell Gallery
    Install-Module AzureRM

    # Install the Azure Service Management module from the PowerShell Gallery
    Install-Module Azure

#### Weitere Informationen zu diesen Befehlen
* **Install-Module AzureRM** installiert ein Rollup-Modul für die Azure Resource Manager-Cmdlets. Das AzureRM-Modul hängt für jedes Azure Resource Manager-Modul von einem bestimmte Versionsbereich ab. Der enthaltene Versionsbereich stellt sicher, dass keine aktuellen Moduländerungen aufgenommen werden können, wenn AzureRM-Module mit derselben Hauptversion installiert werden. Bei der Installation des AzureRM-Moduls werden alle noch nicht installierten Azure Resource Manager-Module aus dem PowerShell-Katalog heruntergeladen und installiert. Weitere Informationen zur von Azure PowerShell-Modulen verwendeten semantischen Versionsverwaltung finden Sie unter [semver.org](http://semver.org). 
* **Install-Module Azure** installiert das Azure-Modul. Dieses Modul ist das Service Management-Modul aus Azure PowerShell 0.9.x. Hierfür sollten keine größeren Änderungen gelten, und es sollte gegenüber der vorherigen Version des Azure-Moduls austauschbar sein.

## Schritt 2: Starten
Sie können die Cmdlets über die Windows PowerShell-Standardkonsole oder über die PowerShell Integrated Scripting Environment (ISE) ausführen. Die Methode, die Sie zum Öffnen einer der beiden Konsolen verwenden, ist abhängig von der ausgeführten Windows-Version:

* Auf einem Computer, auf dem mindestens Windows 8 oder Windows Server 2012 ausgeführt wird, können Sie die integrierte Suche verwenden. Beginnen Sie auf dem Bildschirm **Start** mit der Eingabe von „power“. Daraufhin wird eine entsprechende Liste von Anwendungen zurückgegeben, die auch Windows PowerShell umfasst. Klicken Sie auf eine dieser beiden Anwendungen, um die Konsole zu öffnen. (Klicken Sie mit der rechten Maustaste auf das Symbol, um die Anwendung an den **Startbildschirm** anzuheften.)
* Verwenden Sie auf einem Computer, auf dem eine niedrigere Version als Windows 8 oder Windows Server 2012 ausgeführt wird, das **Startmenü**. Klicken Sie im **Startmenü** auf **Alle Programme**, **Zubehör** und den **Windows PowerShell**-Ordner, und klicken Sie dann auf **Windows PowerShell**.

Sie können auch die **Windows PowerShell ISE** ausführen, um Menüelemente und Tastenkombinationen zum Durchführen vieler Aufgaben zu nutzen, die Sie auch in der Windows PowerShell-Konsole durchführen würden. Geben Sie zum Verwenden der ISE in der Windows PowerShell-Konsole „Cmd.exe“ ein, oder geben Sie im Feld **Ausführen** den Befehl **powershell\_ise.exe** ein.

### Befehle als Starthilfe
    # To make sure the Azure PowerShell module is available after you install
    Get-Module –ListAvailable 

    # To login to Azure Resource Manager
    Login-AzureRmAccount

    # You can also use a specific Tenant if you would like a faster login experience
    # Login-AzureRmAccount -TenantId xxxx

    # To view all subscriptions for your account
    Get-AzureRmSubscription

    # To select a default subscription for your current session
    Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

    # View your current Azure PowerShell session context
    # This session state is only applicable to the current session and will not affect other sessions
    Get-AzureRmContext

    # To select the default storage context for your current session
    Set-AzureRmCurrentStorageAccount –ResourceGroupName “your resource group” –StorageAccountName “your storage account name”

    # View your current Azure PowerShell session context
    # Note: the CurrentStorageAccount is now set in your session context
    Get-AzureRmContext

    # To list all of the blobs in all of your containers in all of your accounts
    Get-AzureRmStorageAccount | Get-AzureStorageContainer | Get-AzureStorageBlob


## Schritt 3: Verbinden
Für die Cmdlets ist Ihr Abonnement erforderlich, damit Ihre Dienste verwaltet werden können. Sie können ein Azure-Abonnement erwerben, falls Sie noch keines besitzen. Eine Anleitung finden Sie unter [Azure erwerben](http://go.microsoft.com/fwlink/p/?LinkId=320795).

1. Geben Sie **Login-AzureRmAccount** ein.
2. Geben Sie die dem Konto zugeordnete E-Mail-Adresse und das zugehörige Kennwort ein. Die Anmeldeinformationen werden von Azure authentifiziert und gespeichert, dann wird das Fenster geschlossen.

--ODER--

Melden Sie sich mit Ihrem Geschäfts- oder Schulkonto an:

    $cred = Get-Credential
    Login-AzureRmAccount -Credential $cred
> [!NOTE]
> Wenn Sie Ihrem Unternehmenskonto mehrere Mandanten zugeordnet haben, geben Sie den Parameter „TenantId“ ein:
> 
> 

    $loadersubscription = Get-AzureRmSubscription -SubscriptionName $YourSubscriptionName -TenantId $YourAssociatedSubscriptionTenantId


> [!NOTE]
> Diese nicht-interaktive Anmeldemethode funktioniert nur mit einem Geschäfts- oder Schulkonto. Bei einem Geschäfts- oder Schulkonto handelt es sich um ein von Ihrem Unternehmen bzw. Ihrer Bildungseinrichtung verwaltetes Benutzerkonto, das in der entsprechenden Azure Active Directory-Instanz definiert ist. Wenn Sie derzeit kein Geschäfts- oder Schulkonto besitzen und sich mit einem Microsoft-Konto bei Ihrem Azure-Abonnement anmelden, können Sie mit den folgenden Schritten auf einfache Weise ein solches Konto erstellen.
> 
> 1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) an, und klicken Sie auf **Active Directory**.
> 2. Wenn kein Verzeichnis vorhanden ist, wählen Sie **Create your directory** (Verzeichnis erstellen) aus, und stellen Sie die geforderten Informationen bereit.
> 3. Wählen Sie Ihr Verzeichnis aus, und fügen Sie einen neuen Benutzer hinzu. Mit diesem neuen Benutzer können Sie sich über ein Geschäfts- oder Schulkonto anmelden. Während der Erstellung des Benutzers erhalten Sie sowohl eine E-Mail-Adresse für den Benutzer als auch ein temporäres Kennwort. Speichern Sie diese Informationen, da sie in Schritt 5 weiter unten benötigt werden.
> 4. Wählen Sie im klassischen Azure-Portal **Einstellungen** und anschließend **Administratoren**. Wählen Sie **Hinzufügen** aus, und fügen Sie den neuen Benutzer als Co-Administrator hinzu. Dies ermöglicht es dem Geschäfts- oder Schulkonto, Ihr Azure-Abonnement zu verwalten.
> 5. Melden Sie sich schließlich vom klassischen Azure-Portal ab und dann unter Verwendung des Geschäfts- oder Schulkontos erneut an. Wenn Sie sich zu diesem Zeitpunkt zum ersten Mal mit diesem Konto anmelden, werden Sie aufgefordert, das Kennwort zu ändern.
> 
> Weitere Informationen zur Anmeldung bei Microsoft Azure mit einem Geschäfts- oder Schulkonto finden Sie unter [Anmelden bei Microsoft Azure als Organisation](active-directory/sign-up-organization.md).
> 
> Weitere Informationen zur Authentifizierungs- und Abonnemontverwaltung in Azure finden Sie unter [Verwalten von Konten, Abonnements und Administratorrollen](http://go.microsoft.com/fwlink/?LinkId=324796).
> 
> 

### Anzeigen von Konto- und Abonnementinformationen
Sie können über mehrere Konten und Abonnements zur Verwendung durch Azure PowerShell verfügen Sie können mehrere Konten hinzufügen, indem Sie **Add-AzureRmAccount** mehrmals ausführen.

Geben Sie zum Anzeigen der verfügbaren Azure-Konten **Get-AzureAccount** ein.

Um Ihre Azure-Abonnements anzuzeigen, geben Sie **Get-AzureRmSubscription** ein.

## <a id="Help"></a>Hilfe
Diese Ressourcen enthalten Hilfethemen für bestimmte Cmdlets:

* Sie können in der Konsole das integrierte Hilfesystem verwenden. Das Cmdlet **Get-Help** ermöglicht den Zugriff auf dieses System. 
* Hilfe von der Community erhalten Sie in den folgenden beliebten Foren:
  
  * [Azure-Forum auf MSDN](http://go.microsoft.com/fwlink/p/?LinkId=320212)
  * [Stackoverflow](http://go.microsoft.com/fwlink/?LinkId=320213)

## Weitere Informationen
Weitere Informationen zur Verwendung von Cmdlets finden Sie unter den folgenden Ressourcen:

Einfache Anleitungen zur Verwendung von Windows PowerShell finden Sie unter [Verwenden von Windows PowerShell](http://go.microsoft.com/fwlink/p/?LinkId=321939).

Referenzinformationen zu den Cmdlets finden Sie unter [Azure-Cmdlet-Referenz](https://msdn.microsoft.com/library/windowsazure/jj554330.aspx).

Beispielskripts und Anleitungen zur Unterstützung bei der Skripterstellung zum Verwalten von Azure finden Sie im [Script Center](http://go.microsoft.com/fwlink/p/?LinkId=321940).

<!---HONumber=AcomDC_0518_2016-->