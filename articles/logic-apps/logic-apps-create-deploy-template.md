---
title: "Erstellen einer Bereitstellungsvorlage für Logik-Apps | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine Bereitstellungsvorlage für Logik-Apps erstellen und für die Versionsverwaltung verwenden."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 85928ec6-d7cb-488e-926e-2e5db89508ee
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: dc8c9eac941f133bcb3a9807334075bfba15de46
ms.openlocfilehash: 4e1de756696bec4d3d0ff443e643d735523b851b


---
# <a name="create-a-logic-app-deployment-template"></a>Erstellen einer Bereitstellungsvorlage für Logik-Apps
Nach dem Erstellen einer Logik-App können Sie diese zum Erstellen einer Azure Resource Manager-Vorlage verwenden. Auf diese Weise können Sie die Logik-App problemlos in jeder Umgebung oder Ressourcengruppe bereitstellen, in der Sie sie benötigen. Eine Einführung zu Resource Manager-Vorlagen erhalten Sie in den Artikeln [Erstellen von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md) und [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="logic-app-deployment-template"></a>Bereitstellungsvorlage für Logik-Apps
Eine Logik-App besteht aus drei grundlegenden Komponenten:

* **Logik-App-Ressource**. Diese Ressource enthält Informationen zu Tarifen, Speicherort, Workflowdefinition usw.
* **Workflowdefinition**. Die Anzeige der Informationen in der Codeansicht. Umfasst die Definition der Workflowschritte und Informationen zur Ausführung des Moduls. Dies ist die `definition` -Eigenschaft der Logik-App-Ressource.
* **Verbindungen**. Dies sind separate Ressourcen, um Metadaten für alle Connectorverbindungen, wie Verbindungszeichenfolgen und Zugriffstoken, sicher zu speichern. Auf diese wird in einer Logik-App im Abschnitt `parameters` der Logik-App-Ressource verwiesen.

Mithilfe eines Tools wie dem [Azure-Ressourcen-Explorer](http://resources.azure.com)können Sie diese Informationen für vorhandene Logik-Apps anzeigen.

Wenn Sie eine Vorlage für eine Logik-App erstellen möchten, um diese mit Ressourcengruppenbereitstellungen zu verwenden, müssen Sie die Ressourcen definieren und nach Bedarf parametrisieren. Beispiel: Wenn Sie eine App sowohl in einer Entwicklungs- als auch einer Test- und einer Produktionsumgebung bereitstellen möchten, sollten Sie unterschiedliche Verbindungszeichenfolgen zur SQL-Datenbank in jeder Umgebung verwenden. Alternativ dazu können Sie die App auch in verschiedenen Abonnements oder Ressourcengruppen bereitstellen.  

## <a name="create-a-logic-app-deployment-template"></a>Erstellen einer Bereitstellungsvorlage für Logik-Apps
Eine gültige Bereitstellungsvorlage für Logik-Apps wird am einfachsten mit den [Visual Studio-Tools für Logic Apps](logic-apps-deploy-from-vs.md) erstellt.  Die Visual Studio-Tools generieren eine gültige Bereitstellungsvorlage, die in allen Abonnements und an allen Standorten eingesetzt werden können.

Beim Erstellen einer Bereitstellungsvorlage für Logik-Apps stehen Ihnen einige weitere Tools zur Verfügung. Sie können die Vorlage manuell anlegen, indem Sie die bereits erwähnten Ressourcen verwenden, um bei Bedarf Parameter zu erstellen. Sie können aber auch das PowerShell-Modul [Logic App Template Creator](https://github.com/jeffhollan/LogicAppTemplateCreator) verwenden. Dieses Open Source-Modul evaluiert zunächst die Logik-App sowie die von ihr verwendeten Verbindungen und erstellt dann Vorlagenressourcen mit den Parametern, die für die Bereitstellung erforderlich sind. Bei einer Logik-App beispielsweise, die eine Meldung von einer Azure Service Bus-Warteschlange erhält und Daten zu einer Azure-SQL Datenbank hinzufügt, behält das Tool die gesamte Orchestrierungslogik bei und parametrisiert die SQL- und Service Bus-Verbindungszeichenfolgen, damit diese bei der Bereitstellung festgelegt werden können.

> [!NOTE]
> Verbindungen müssen sich innerhalb der gleichen Ressourcengruppe wie die Logik-App befinden.
>
>

### <a name="install-the-logic-app-template-powershell-module"></a>Installieren des PowerShell-Moduls für Logik-App-Vorlagen
Am einfachsten installieren Sie das Modul über den [PowerShell-Katalog](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1) mit dem Befehl `Install-Module -Name LogicAppTemplate`.  

Sie können das PowerShell-Modul auch manuell installieren:

1. Laden Sie die neueste Version von [Logic App Template Creator](https://github.com/jeffhollan/LogicAppTemplateCreator/releases)herunter.  
2. Extrahieren Sie den Ordner in Ihren PowerShell-Modulordner (in der Regel `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Damit das Modul mit jedem Mandanten und Abonnementzugriffstoken funktioniert, empfiehlt es sich, es zusammen mit dem Befehlszeilentool [ARMClient](https://github.com/projectkudu/ARMClient) zu verwenden.  In diesem [Blogbeitrag ](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) wird ARMClient ausführlich erläutert.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Generieren einer Logik-App-Vorlage mithilfe von PowerShell
Nach der Installation von PowerShell können Sie mithilfe des folgenden Befehls eine Vorlage generieren:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId` ist die Azure-Abonnement-ID. Diese erste Zeile ruft über ARMClient ein Zugriffstoken ab, übergibt es an das PowerShell-Skript und erstellt dann die Vorlage in einer JSON-Datei.

## <a name="add-parameters-to-a-logic-app-template"></a>Hinzufügen von Parametern zu einer Logik-App-Vorlage
Nach dem Erstellen einer Logik-App-Vorlage können Sie dieser nach Bedarf Parameter hinzufügen oder vorhandene Parameter bearbeiten. Wenn Ihre Definition beispielsweise eine Ressourcen-ID für eine Azure-Funktion oder einen geschachtelten Workflow für eine einzelne Bereitstellung beinhaltet, können Sie Ihrer Vorlage weitere Ressourcen hinzufügen und die IDs nach Bedarf parametrisieren. Dies gilt auch für Verweise auf benutzerdefinierte APIs oder Swagger-Endpunkte, die mit jeder Ressourcengruppe bereitgestellt werden sollen.

## <a name="deploy-a-logic-app-template"></a>Bereitstellen einer Logik-App-Vorlage
Sie können Ihre Vorlage mithilfe beliebiger Tools bereitstellen, einschließlich PowerShell, REST-API, Visual Studio Release Management oder der Vorlagenbereitstellung im Azure-Portal. Weitere Informationen finden Sie im Artikel [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-template-deploy.md) . Es empfiehlt sich auch, eine [Parameterdatei](../azure-resource-manager/resource-group-template-deploy.md#parameters) zu erstellen, um die Werte für den Parameter zu speichern.

### <a name="authorize-oauth-connections"></a>Autorisieren von OAuth-Verbindungen
Nach der Bereitstellung funktioniert die Logik-App vollständig mit gültigen Parametern. Dennoch müssen OAuth-Verbindungen autorisiert werden, um ein gültiges Zugriffstoken zu generieren. Dazu können Sie die Logik-App im Designer öffnen und dort Verbindungen autorisieren. Wenn Sie diesen Vorgang automatisch durchführen lassen möchten, können Sie ein Skript verwenden, um jeder OAuth-Verbindung zuzustimmen. Ein Beispielskript hierfür finden Sie auf GitHub im Projekt [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) .

## <a name="visual-studio-release-management"></a>Visual Studio Release Management
Ein häufiges Szenario für die Bereitstellung und Verwaltung einer Umgebung ist die Verwendung eines Tools wie Visual Studio Release Management mit einer Bereitstellungsvorlage für Logik-Apps. Visual Studio Team Services enthält eine Aufgabe für die [Bereitstellung von Azure-Ressourcengruppen](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) , die jeder Build- oder Versionspipeline hinzugefügt werden kann. Sie benötigen einen [Dienstprinzipal](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) , um die Bereitstellung zu autorisieren, und können dann die Versionsdefinition generieren.

1. Zum Erstellen einer neuen Definition in Release Management wählen Sie **Leer**, um mit einer leeren Definition zu beginnen.

    ![Erstellen einer neuen, leeren Definition][1]   
2. Wählen Sie alle dafür benötigten Ressourcen. Dies beinhaltet wahrscheinlich die manuell oder als Teil des Buildprozesses generierte Logik-App-Vorlage.
3. Fügen Sie eine Aufgabe für die **Bereitstellung von Azure-Ressourcengruppen** hinzu.
4. Verwenden Sie für die Konfiguration einen [Dienstprinzipal](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/), und verweisen Sie auf die Vorlagen- und Vorlagenparameterdateien.
5. Erstellen Sie nach Bedarf weitere Schritte im Freigabeprozess für andere Umgebungen, automatisierte Tests oder genehmigende Personen.

<!-- Image References -->
[1]: ./media/logic-apps-create-deploy-template/emptyreleasedefinition.png



<!--HONumber=Jan17_HO3-->


