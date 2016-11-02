<properties
	pageTitle="Azure Resource Manager-basierte PowerShell-Befehle zum Verwalten von Azure-Web-Apps | Microsoft Azure"
	description="Erfahren Sie, wie die neuen Azure Resource Manager-basierten PowerShell-Befehle zum Verwalten Ihrer Azure-Web-Apps verwendet werden."
	services="app-service\web"
	documentationCenter=""
	authors="ahmedelnably"
	manager="stefsch"
	editor=""/>

<tags
	ms.service="app-service-web"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/14/2016"
	ms.author="aelnably"/>

# Verwenden von auf Azure Resource Manager basierenden PowerShell-Befehlen zum Verwalten von Azure-Web-Apps#

Der Version 1.0.0 von Microsoft Azure PowerShell wurden neue Befehle hinzugefügt, die dem Benutzer die Möglichkeit bieten, zum Verwalten von Web-Apps auf Azure Resource Manager basierende PowerShell-Befehle zu nutzen.

Informationen zum Verwalten von Ressourcengruppen finden Sie unter [Verwenden von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md).

Die vollständige Liste von Parametern und Optionen für die PowerShell-Cmdlets von Azure Resource Manager für Web-Apps finden Sie in der [vollständigen Cmdlet-Referenz für die Azure Resource Manager-basierten PowerShell-Cmdlets für Web-Apps](https://msdn.microsoft.com/library/mt619237.aspx).

## Verwalten von App Service-Plänen ##

### Erstellen eines App Service-Plans ###
Zum Erstellen eines neuen App Service-Plans verwenden Sie das Cmdlet **New-AzureRmAppServicePlan**.

Es folgen Beschreibungen der verschiedenen Parameter:

- 	**Name**: Name des App Service-Plans.
- 	**Location**: Region des App Service-Plans.
- 	**ResourceGroupName**: Ressourcengruppe, die den neu erstellten App Service-Plan enthält.
- 	**Tier**: Der gewünschten Tarif (Standardeinstellung ist Free, andere Optionen sind Shared, Basic, Standard und Premium).
- 	**WorkerSize**: Die Größe der Worker (Standard ist „Small“, wenn der Parameter „Tier“ als Basic, Standard oder Premium angegeben wurde. Andere Optionen sind „Medium“ und „Large“.)
- 	**NumberofWorkers**: Anzahl der Worker im App Service-Plan (Standardwert 1). 

Beispiel für die Verwendung dieses Cmdlets:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### Erstellen eines App Service-Plans in einer App Service-Umgebung ###
Zum Erstellen eines neuen App Service-Plans in einer App Service-Umgebung kann der Befehl **New-AzureRmAppServicePlan** mit zusätzlichen Parametern zum Angeben des Namens der App Service-Umgebung und der Ressourcengruppe verwendet werden, zu der die App Service-Umgebung gehört.

Beispiel für die Verwendung dieses Cmdlets:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Informationen zur App Service-Umgebung finden Sie unter [Einführung in die App Service-Umgebung](app-service-app-service-environment-intro.md).

### Auflisten vorhandener App Service-Pläne ###

Verwenden Sie zum Auflisten vorhandener App Service-Pläne das Cmdlet **Get-AzureRmAppServicePlan**.

Verwenden Sie Folgendes zum Auflisten aller App Service-Pläne in Ihrem Abonnement:

    Get-AzureRmAppServicePlan

Verwenden Sie zum Auflisten aller App Service-Pläne in einer bestimmten Ressourcengruppe Folgendes:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

Verwenden Sie zum Abrufen eines bestimmten App Service-Plans Folgendes:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### Konfigurieren eines vorhandenen App Service-Plans ###

Verwenden Sie zum Ändern der Einstellungen eines vorhandenen App Service-Plans das Cmdlet **Set-AzureRmAppServicePlan**. Sie können den Tarif, die Workergröße und Anzahl der Worker ändern.

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### Skalieren eines App Service-Plans ####

Zum Skalieren eines vorhandenen App Service-Plans verwenden Sie Folgendes:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### Ändern der Workergröße eines App Service-Plans ####

Verwenden Sie zum Ändern der Größe der Worker in einem vorhandenen App Service-Plan Folgendes:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### Ändern des Tarifs eines App Service-Plans ####

Verwenden Sie zum Ändern des Tarifs eines vorhandenen App Service-Plans Folgendes:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### Löschen eines vorhandenen App Service-Plans ###

Zum Löschen eines vorhandenen App Service-Plans müssen alle zugewiesenen Web-Apps zuerst verschoben oder gelöscht werden. Anschließend können Sie mit dem Cmdlet **Remove-AzureRmAppServicePlan** den App Service-Plan löschen.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## Verwalten von Azure App Service-Web-Apps ##

### Erstellen einer neuen Web-App ###

Verwenden Sie zum Erstellen einer neuen Web-App das Cmdlet**New-AzureRmWebApp**.

Es folgen Beschreibungen der verschiedenen Parameter:

- **Name**: Name der Web-App.
- **AppServicePlan**: Name des Service-Plans, der zum Hosten der Web-App verwendet wird.
- **ResourceGroupName**: Ressourcengruppe, die den App Service-Plan hostet.
- **Location**: Der Speicherort der Web-App.

Beispiel für die Verwendung dieses Cmdlets:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### Erstellen einer neuen Web-App in einer App Service-Umgebung ###

Zum Erstellen einer neuen Web-App in einer App Service-Umgebung kann der Befehl **New-AzureRmWebApp** mit zusätzlichen Parametern zum Angeben des Namens der App Service-Umgebung und der Ressourcengruppe verwendet werden, zu der die App Service-Umgebung gehört.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Informationen zur App Service-Umgebung finden Sie unter [Einführung in die App Service-Umgebung](app-service-app-service-environment-intro.md).

### Löschen einer vorhandenen Web-App ###

Zum Löschen einer vorhandenen Web-App können Sie das Cmdlet **Remove-AzureRmWebApp** verwenden. Sie müssen den Namen der Web-App und der Ressourcengruppe angeben.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### Auflisten vorhandener Web-Apps ###

Verwenden Sie zum Auflisten vorhandener Web-Apps das Cmdlet **Get-AzureRmWebApp**.

Verwenden Sie zum Auflisten aller Web-Apps in Ihrem Abonnement Folgendes:

    Get-AzureRmWebApp

Verwenden Sie zum Auflisten aller Web-Apps in einer bestimmten Ressourcengruppe Folgendes:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

Verwenden Sie zum Abrufen einer bestimmten Web-App Folgendes:

    Get-AzureRmWebApp -Name ContosoWebApp

### Konfigurieren einer vorhandenen Web-App ###

Verwenden Sie zum Ändern der Einstellungen und Konfigurationen einer vorhandenen Web-App das Cmdlet **Set-AzureRmWebApp**. Klicken Sie auf den [Link zur Cmdlet-Referenz](https://msdn.microsoft.com/library/mt652487.aspx), um eine vollständige Liste der Parameter anzuzeigen.

Beispiel 1: Verwenden Sie dieses Cmdlet zum Ändern von Verbindungszeichenfolgen.

	$connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
	Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

Beispiel 2: Beispiel für das Hinzufügen von App-Einstellungen

	$appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
	Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


Beispiel 3: Festlegen der Web-App auf die Ausführung im 64-Bit-Modus

	Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### Ändern des Status einer vorhandenen Web-App ###

#### Neustarten einer Web-App ####

Um eine Web-App neu zu starten, müssen Sie den Namen und die Ressourcengruppe der Web-App angeben.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### Beenden einer Web-App ####

Um eine Web-App zu beenden, müssen Sie den Namen und die Ressourcengruppe der Web-App angeben.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### Starten einer Web-App ####

Um eine Web-App zu starten, müssen Sie den Namen und die Ressourcengruppe der Web-App angeben.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### Verwalten von Web-App-Veröffentlichungsprofilen ###

Jede Web-App hat ein Veröffentlichungsprofil, das verwendet werden kann, um Ihre Apps zu veröffentlichen. Eine Reihe von Vorgängen kann auf Veröffentlichungsprofile angewendet werden.

#### Abrufen des Veröffentlichungsprofils ####

Um das Veröffentlichungsprofil für eine Web-App abzurufen, verwenden Sie Folgendes:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Beachten Sie, dass dadurch das Veröffentlichungsprofil an der Befehlszeile wiedergegeben und in einer Textdatei ausgegeben wird.

#### Zurücksetzen des Veröffentlichungsprofils ####

Um sowohl das Veröffentlichungskennwort für die FTP- und Webbereitstellung einer Web-App zurückzusetzen, verwenden Sie Folgendes:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### Verwalten von Web-App-Zertifikaten ###

Informationen zum Verwalten von Web-App-Zertifikaten finden Sie unter [SSL-Zertifikatbindung mithilfe von PowerShell](app-service-web-app-powershell-ssl-binding.md).



### Nächste Schritte ###
- Weitere Informationen zur PowerShell-Unterstützung von Azure Resource Manager finden Sie unter [Verwenden von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md).
- Informationen zu App Service-Umgebungen finden Sie in der [Einführung in die App Service-Umgebung](app-service-app-service-environment-intro.md).
- Informationen zum Verwalten von App Service-SSL-Zertifikaten mit PowerShell finden Sie unter [SSL-Zertifikatbindung mithilfe von PowerShell](app-service-web-app-powershell-ssl-binding.md).
- Die vollständige Liste von Azure Resource Manager-basierten PowerShell-Cmdlets für Azure-Web-Apps finden Sie in der [Azure-Cmdlet-Referenz für die Azure Resource Manager-basierten PowerShell-Cmdlets für Web-Apps](https://msdn.microsoft.com/library/mt619237.aspx).

<!---HONumber=AcomDC_0615_2016-->