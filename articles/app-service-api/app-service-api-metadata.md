<properties
	pageTitle="Metadaten in App Service-API-Apps für die API-Ermittlung und Codegenerierung | Microsoft Azure"
	description="Erfahren Sie, wie API-Apps in Azure App Service mithilfe von Swagger-Metadaten die API-Ermittlung und Codegenerierung erleichtern."
	services="app-service\api"
	documentationCenter=".net"
	authors="tdykstra"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service-api"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/02/2016"
	ms.author="rachelap"/>

# Metadaten in App Service-API-Apps für die API-Ermittlung und Codegenerierung 

In App Service-API-Apps ist eine Unterstützung für [Swagger 2.0](http://swagger.io/)-API-Metadaten integriert. Sie müssen Swagger nicht nutzen. Wenn Sie dieses Framework für APIs jedoch verwenden, können von API-Apps-Features profitieren, die die Ermittlung und die Nutzung erleichtern.

## Swagger-Endpunkt

Sie können einen Endpunkt angeben, der Swagger 2.0-JSON-Metadaten für eine API-App in einer Eigenschaft der API-App bereitstellt. Der Endpunkt kann sich auf den Basis-URL der API-App oder auf eine absolute URL beziehen. Absolute URLs können nach außerhalb der API-App verweisen.

Für viele nachgelagerte Clients (z. B. Visual Studio-Codegenerierung und PowerApps-Schritt „API hinzufügen“) muss die URL öffentlich zugänglich sein (ohne Schutz durch Benutzer- oder Dienstauthentifizierung). Dies bedeutet, dass wenn Sie die App Service-Authentifizierung verwenden und die API-Definition innerhalb der App selbst verfügbar machen möchten, Sie eine Authentifizierungsmethode wählen müssen, die anonymen Datenverkehr zu lässt, um Ihre API zu erreichen. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung für App Service-API-Apps](app-service-api-authentication.md).

### Blatt „Portal“

Die Endpunkt-URL wird im [Azure-Portal](https://portal.azure.com/) auf dem Blatt **API-Definition** angezeigt und kann dort geändert werden.

![](./media/app-service-api-metadata/apidefblade.png)

### „Azure-Ressourcen-Manager“-Eigenschaft

Sie können die API-Definitions-URL für eine API-App auch konfigurieren, indem Sie den [Ressourcen-Explorer](https://resources.azure.com/) oder [Azure Resource Manager-Vorlagen](../resource-group-authoring-templates.md) in Befehlszeilentools verwenden, beispielsweise in [Azure PowerShell](../powershell-install-configure.md) und der [Azure-CLI](../xplat-cli-install.md).

Beispiel: Wechseln Sie im **Ressourcen-Explorer** zu **Abonnements > {Ihr Abonnement} > Ressourcengruppen > {Ihre Ressourcengruppe} > Anbieter > Microsoft.Web > Websites > {Ihre Website} > Konfigurieren > Web**. Hier wird die `apiDefinition`-Eigenschaft angezeigt:

		"apiDefinition": {
		  "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
		}

Öffnen Sie die [Datei „azuredeploy.json“ in der Aufgabenlisten-Beispielanwendung](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json), um ein Beispiel für eine Azure Resource Manager-Vorlage zum Festlegen der `apiDefinition`-Eigenschaft anzuzeigen. Suchen Sie nach dem Abschnitt der Vorlage, die wie im oben gezeigten JSON-Beispiel aussieht..

### Standardwert

Wenn Sie eine API-App mit Visual Studio erstellen, wird der API-Definitionsendpunkt automatisch auf die Basis-URL der API-App und `/swagger/docs/v1` festgelegt. Dies ist die Standard-URL, die vom [Swashbuckle](https://www.nuget.org/packages/Swashbuckle)-NuGet-Paket verwendet wird, um ein ASP.NET-Web-API-Projekt mit dynamisch generierten Swagger-Metadaten zu versorgen.

## Codegenerierung

Einer der Vorteile der Integration von Swagger in Azure API-Apps ist die automatische Codegenerierung. Generierte Clientklassen erleichtern es, Code zu schreiben, der eine API-App aufruft.

Sie können Clientcode für eine API-App mit Visual Studio oder über die Befehlszeile generieren. Informationen zum Generieren von Clientklassen für ein ASP.NET-Web-API-Projekt mit Visual Studio finden Sie unter [Erste Schritte mit API-Apps und ASP.NET](app-service-api-dotnet-get-started.md#codegen). Informationen zur Vorgehensweise mit der Befehlszeile für alle unterstützten Sprachen finden Sie auf GitHub.com im Repository [Azure/autorest](https://github.com/azure/autorest) in der Readme-Datei.
 
## Nächste Schritte

Ein Schritt-für-Schritt-Tutorial mit Anleitungen zum Erstellen, Bereitstellen und Nutzen einer API-App finden Sie unter [Erste Schritte mit API-Apps in Azure App Service](app-service-api-dotnet-get-started.md).

Wenn Sie Azure API Management mit API-Apps verwenden, können Sie Ihre API mithilfe von Swagger-Metadaten in API Management importieren. Weitere Informationen finden Sie unter [Importieren einer API-Definition mit Operationen in Azure API Management](../api-management/api-management-howto-import-api.md).

<!---HONumber=AcomDC_0713_2016-->