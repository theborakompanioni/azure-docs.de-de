<properties
   pageTitle="Erstellen von Bereitstellungsvorlagen für Logik-Apps | Microsoft Azure"
   description="Erfahren Sie, wie Bereitstellungsvorlagen für Logik-Apps erstellt und für die Versionsverwaltung verwendet werden können."
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/25/2016"
   ms.author="jehollan"/>
   
# Erstellen von Bereitstellungsvorlagen für Logik-Apps

Wenn Sie eine Logik-App erstellt haben, möchten Sie sie möglicherweise auch als Ressourcenvorlage für Azure erstellen, damit die Bereitstellung der Logik-App in jeder gewünschten Umgebung oder Ressourcengruppe problemlos möglich ist. Eine Einführung zu Resource Manager-Vorlagen erhalten Sie in den Artikeln [Erstellen von Azure Resource Manager-Vorlagen](../resource-group-authoring-templates.md) und [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen](../resource-group-template-deploy.md).

## Übersicht über Bereitstellungsvorlagen für Logik-Apps

Eine Logik-App besteht aus 3 grundlegenden Komponenten:

* **Logik-App-Ressource**: die Ressource, die Informationen zu Tarifen, Speicherort, Workflowdefinition usw. enthält.
* **Workflowdefinition**: wird angezeigt, wenn Sie die **Codeansicht** auswählen. Definiert die Schritte des Workflows und wie diese ausgeführt werden sollen. Dies ist die `definition`-Eigenschaft der Logik-App-Ressource.
* **Verbindungen**: separate Ressourcen, um Metadaten für alle Connectorverbindungen, wie Verbindungszeichenfolgen zu trennen und Zugriffstoken zu speichern. Auf diese wird in einer Logik-App unter `parameters` der Logik-App-Ressource verwiesen.

Mithilfe von Tools wie dem [Azure-Ressourcen-Explorer](http://resources.azure.com) können Sie diese für bestehende Logik-Apps anzeigen.

Wenn Sie eine Vorlage für eine Logik-App erstellen möchten, die mit Ressourcengruppenbereitstellungen funktioniert, müssen Sie die Ressourcen definieren und nach Bedarf parametrisieren. Beispielsweise beim Bereitstellen in einer Entwicklungs-, Test- und Produktionsumgebung sollen unter Umständen unterschiedliche Verbindungszeichenfolgen zur SQL-Datenbank in jeder Umgebung verwendet oder die Bereitstellung in unterschiedlichen Abonnements oder Ressourcengruppen vorgenommen werden.

## Erstellen einer Bereitstellungsvorlage für Logik-Apps

Es gibt einige Tools, die Sie beim Erstellen einer Bereitstellungsvorlage für Logik-Apps unterstützen. Sie können sie manuell erstellen, indem Sie die oben aufgeführten Ressourcen auswählen und die erforderlichen Parameter erstellen. Sie können aber auch das [Logic App Template Creator](https://github.com/jeffhollan/LogicAppTemplateCreator)-PowerShell-Modul verwenden. Dieses Open Source-Modul evaluiert die Logik-App sowie die von ihr verwendeten Verbindungen und erstellt Vorlagenressourcen mit den bereitzustellenden Parametern. Z.B. bei einer Logik-App, die eine Meldung von einer Service Bus-Warteschlange erhalten und Daten zu einer SQL Azure-Datenbank hinzugefügt hat, behält das Tool die Orchestrierungslogik bei und parametrisiert die SQL- und Service Bus-Verbindungszeichenfolgen, damit diese bei der Bereitstellung festgelegt werden können.

>[AZURE.NOTE] Verbindungen müssen sich innerhalb der gleichen Ressourcengruppe wie die Logik-App befinden.

### Installieren des PowerShell-Moduls für Logik-App-Vorlagen

Die einfachste Möglichkeit zur Installation erfolgt über den [PowerShell-Katalog](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1) mit dem Befehl `Install-Module -Name LogicAppTemplate`.

Sie können es aber auch manuell installieren:

1. Laden Sie die neueste Version des [Logic App Template Creator](https://github.com/jeffhollan/LogicAppTemplateCreator/releases) herunter.  
1. Extrahieren Sie den Ordner in Ihren PowerShell-Modulordner (in der Regel `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Damit das Modul mit jedem Mandanten und Abonnementzugriffstoken funktioniert, verwenden Sie es mit dem [armclient](https://github.com/projectkudu/ARMClient)-Befehlszeilentool. Details zu `armclient` finden Sie [hier](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html).

### Generieren einer Logik-App-Vorlage über PowerShell

Nach der Installation können Sie mit dem folgenden Befehl eine Vorlage generieren:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

Dabei ist `$SubscriptionId` die Azure-Abonnement-ID. Bei dieser Zeile wird zuerst über `armclient` ein Zugriffstoken abgerufen und an das PowerShell-Skript übergeben. Anschließend wird die Vorlage in einer `.json`-Datei ausgegeben.

## Hinzufügen von Parametern zu einer Logik-App-Vorlage

Einer bereits erstellten Logik-App-Vorlage können Sie weitere Parameter hinzufügen oder die Parameter bei Bedarf ändern. Wenn Ihre Definition beispielsweise eine Ressourcen-ID für eine Azure-Funktion oder einen geschachtelten Workflow für eine einzelne Bereitstellung beinhaltet, können Sie Ihrer Vorlage weitere Ressourcen hinzufügen und die IDs nach Bedarf parametrisieren. Dies gilt auch für Verweise auf benutzerdefinierte APIs oder Swagger-Endpunkte, die mit jeder Ressourcengruppe bereitgestellt werden sollen.

## Bereitstellen einer Logik-App-Vorlage

Wenn Sie eine Vorlage erstellt haben, können Sie diese mithilfe beliebiger Tools einschließlich PowerShell, REST-API, Visual Studio, Release Management oder der Vorlagenbereitstellung im Azure-Portal bereitstellen. Informationen zur Bereitstellung finden Sie [hier](../resource-group-template-deploy.md). Es wird empfohlen, dass Sie auch eine [Parameterdatei](../resource-group-template-deploy.md#parameter-file) erstellen, um die Werte für den Parameter zu speichern.

### Autorisieren von OAuth-Verbindungen

Nach der Bereitstellung funktioniert die Logik-App End-to-End mit gültigen Parametern, OAuth-Verbindungen müssen jedoch weiterhin autorisiert werden, um ein gültiges Zugriffstoken zu generieren. Öffnen Sie dazu die Logik-App im Designer, und autorisieren Sie Verbindungen. Mit einem Skript können Sie aber auch jeder OAuth-Verbindung automatisch zustimmen. Ein Beispielskript finden Sie in GitHub im Projekt [Connection Auth](https://github.com/logicappsio/LogicAppConnectionAuth).

## Verwenden von Visual Studio Release Management

Umgebungen werden häufig mit Tools wie Visual Studio Release Management mit einer Bereitstellungsvorlage für Logik-Apps bereitgestellt und verwaltet. VSTS enthält eine [Bereitstellungsaufgabe für Azure-Ressourcengruppen](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup), die jedem Build und jeder Versionspipeline hinzugefügt werden kann. Sie benötigen einen [Dienstprinzipal](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/), um die Bereitstellung zu autorisieren, und können dann die Versionsdefinition generieren.

1. Wählen Sie unter **Version** aus, dass Sie eine **neue Definition** mit einer **leeren** Definition beginnen möchten.

    ![][1]

1. Wählen Sie alle dafür benötigten Artefakte. Dies beinhaltet wahrscheinlich die manuell oder als Teil des Buildprozesses generierte Logik-App-Vorlage.
1. Fügen Sie eine **Bereitstellungsaufgabe für Azure-Ressourcengruppen** hinzu.
1. Nehmen Sie die Konfiguration mit einem [Dienstprinzipal](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) vor, und verweisen auf die Dateien **Vorlage** und **Vorlagenparameter**.
1. Erstellen Sie nach Bedarf weitere Schritte im Freigabeprozess für andere Umgebungen, automatisierte Tests oder genehmigende Personen.
    
<!-- Image References -->
[1]: ./media/app-service-logic-create-deploy-template/emptyReleaseDefinition.PNG

<!---HONumber=AcomDC_0601_2016-->