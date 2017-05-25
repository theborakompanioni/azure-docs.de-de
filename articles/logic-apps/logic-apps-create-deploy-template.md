---
title: "Erstellen von Bereitstellungsvorlagen für Azure Logic Apps | Microsoft-Dokumentation"
description: "Erstellen von Azure Resource Manager-Vorlagen für die Bereitstellungs- und Versionsverwaltung von Logik-Apps"
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
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.translationtype: Human Translation
ms.sourcegitcommit: 2c33e75a7d2cb28f8dc6b314e663a530b7b7fdb4
ms.openlocfilehash: e736bf626cd379e1bffe2dbf023e86addb09539f
ms.contentlocale: de-de
ms.lasthandoff: 04/21/2017


---
# <a name="create-templates-for-logic-apps-deployment-and-release-management"></a>Erstellen von Vorlagen für die Bereitstellungs- und Versionsverwaltung von Logik-Apps

Nach dem Erstellen einer Logik-App können Sie diese zum Erstellen einer Azure Resource Manager-Vorlage verwenden.
Auf diese Weise können Sie die Logik-App problemlos in jeder Umgebung oder Ressourcengruppe bereitstellen, in der Sie sie benötigen.
Weitere Informationen zu Resource Manager-Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md) und [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="logic-app-deployment-template"></a>Bereitstellungsvorlage für Logik-Apps

Eine Logik-App besteht aus drei grundlegenden Komponenten:

* **Logik-App-Ressource:** Enthält Informationen zu Tarifen, Speicherort und Workflowdefinition.
* **Workflowdefinition:** Beschreibt die Workflowschritte Ihrer Logik-App und gibt an, wie das Logik-Apps-Modul den Workflow ausführen soll.
Sie können diese Definition im Fenster **Codeansicht** Ihrer Logik-App anzeigen.
In der Logik-App-Ressource finden Sie diese Definition in der `definition`-Eigenschaft.
* **Verbindungen:** Bezieht sich auf separate Ressourcen, um Metadaten für alle Connectorverbindungen wie Verbindungszeichenfolgen und Zugriffstoken sicher zu speichern.
In der Logik-App-Ressource verweist Ihre Logik-App auf diese Ressourcen im Abschnitt `parameters`.

Mithilfe eines Tools wie dem [Azure-Ressourcen-Explorer](http://resources.azure.com) können Sie diese Informationen für vorhandene Logik-Apps anzeigen.

Wenn Sie eine Vorlage für eine Logik-App erstellen möchten, um diese mit Ressourcengruppenbereitstellungen zu verwenden, müssen Sie die Ressourcen definieren und nach Bedarf parametrisieren.
Beispiel: Wenn Sie eine App in einer Entwicklungs-, einer Test- und einer Produktionsumgebung bereitstellen möchten, sollten Sie unterschiedliche Verbindungszeichenfolgen zur SQL-Datenbank in jeder Umgebung verwenden.
Alternativ dazu können Sie die App auch in verschiedenen Abonnements oder Ressourcengruppen bereitstellen.  

## <a name="create-a-logic-app-deployment-template"></a>Erstellen einer Bereitstellungsvorlage für Logik-Apps

Eine gültige Bereitstellungsvorlage für Logik-Apps wird am einfachsten mit den [Visual Studio-Tools für Logic Apps](logic-apps-deploy-from-vs.md) erstellt.
Die Visual Studio-Tools generieren eine gültige Bereitstellungsvorlage, die in allen Abonnements und an allen Standorten eingesetzt werden können.

Beim Erstellen einer Bereitstellungsvorlage für Logik-Apps stehen Ihnen einige weitere Tools zur Verfügung.
Sie können die Vorlage manuell anlegen, indem Sie die bereits erwähnten Ressourcen verwenden, um bei Bedarf Parameter zu erstellen.
Sie können aber auch das PowerShell-Modul [Logic App Template Creator](https://github.com/jeffhollan/LogicAppTemplateCreator) verwenden. Dieses Open Source-Modul evaluiert zunächst die Logik-App sowie die von ihr verwendeten Verbindungen und erstellt dann Vorlagenressourcen mit den Parametern, die für die Bereitstellung erforderlich sind.
Bei einer Logik-App beispielsweise, die eine Meldung von einer Azure Service Bus-Warteschlange erhält und Daten zu Azure SQL-Datenbank hinzufügt, behält das Tool die gesamte Orchestrierungslogik bei und parametrisiert die SQL- und Service Bus-Verbindungszeichenfolgen, damit diese bei der Bereitstellung festgelegt werden können.

> [!NOTE]
> Verbindungen müssen sich innerhalb der gleichen Ressourcengruppe wie die Logik-App befinden.
>
>

### <a name="install-the-logic-app-template-powershell-module"></a>Installieren des PowerShell-Moduls für Logik-App-Vorlagen
Am einfachsten installieren Sie das Modul über den [PowerShell-Katalog](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1) mit dem Befehl `Install-Module -Name LogicAppTemplate`.  

Sie können das PowerShell-Modul auch manuell installieren:

1. Laden Sie die neueste Version von [Logic App Template Creator](https://github.com/jeffhollan/LogicAppTemplateCreator/releases)herunter.  
2. Extrahieren Sie den Ordner in Ihren PowerShell-Modulordner (in der Regel `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Damit das Modul mit jedem Mandanten und Abonnementzugriffstoken funktioniert, empfiehlt es sich, es zusammen mit dem Befehlszeilentool [ARMClient](https://github.com/projectkudu/ARMClient) zu verwenden.  In diesem [Blogbeitrag](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) wird ARMClient ausführlich erläutert.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Generieren einer Logik-App-Vorlage mithilfe von PowerShell
Nach der Installation von PowerShell können Sie mithilfe des folgenden Befehls eine Vorlage generieren:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId` ist die Azure-Abonnement-ID. Diese erste Zeile ruft über ARMClient ein Zugriffstoken ab, übergibt es an das PowerShell-Skript und erstellt dann die Vorlage in einer JSON-Datei.

## <a name="add-parameters-to-a-logic-app-template"></a>Hinzufügen von Parametern zu einer Logik-App-Vorlage
Nach dem Erstellen einer Logik-App-Vorlage können Sie dieser nach Bedarf Parameter hinzufügen oder vorhandene Parameter bearbeiten. Wenn Ihre Definition beispielsweise eine Ressourcen-ID für eine Azure-Funktion oder einen geschachtelten Workflow für eine einzelne Bereitstellung beinhaltet, können Sie Ihrer Vorlage weitere Ressourcen hinzufügen und die IDs nach Bedarf parametrisieren. Dies gilt auch für Verweise auf benutzerdefinierte APIs oder Swagger-Endpunkte, die mit jeder Ressourcengruppe bereitgestellt werden sollen.

## <a name="deploy-a-logic-app-template"></a>Bereitstellen einer Logik-App-Vorlage

Sie können Ihre Vorlage mithilfe beliebiger Tools bereitstellen, wie etwa PowerShell, REST-API, [Visual Studio Team Services Release Management](#team-services) oder Vorlagenbereitstellung über das Azure-Portal.
Es empfiehlt sich auch, eine [Parameterdatei](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) zu erstellen, um die Werte für den Parameter zu speichern.
Erfahren Sie mehr über das [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und PowerShell](../azure-resource-manager/resource-group-template-deploy.md) oder [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-Portal](../azure-resource-manager/resource-group-template-deploy-portal.md).

### <a name="authorize-oauth-connections"></a>Autorisieren von OAuth-Verbindungen

Nach der Bereitstellung funktioniert die Logik-App vollständig mit gültigen Parametern.
Sie müssen OAuth-Verbindungen aber dennoch autorisieren, um ein gültiges Zugriffstoken zu generieren.
Um OAuth-Verbindungen zu autorisieren, öffnen Sie die Logik-App im Designer für Logik-Apps und autorisieren diese Verbindungen. Sie können zur automatischen Bereitstellung auch ein Skript verwenden, um jeder OAuth-Verbindung zuzustimmen.
Ein Beispielskript hierfür finden Sie auf GitHub im Projekt [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) .

<a name="team-services"></a>
## <a name="visual-studio-team-services-release-management"></a>Releaseverwaltung in Visual Studio Team Services

Ein häufiges Szenario für die Bereitstellung und Verwaltung einer Umgebung ist die Verwendung eines Tools wie Releaseverwaltung in Visual Studio Team Services mit einer Bereitstellungsvorlage für Logik-Apps. Visual Studio Team Services enthält eine Aufgabe für die [Bereitstellung von Azure-Ressourcengruppen](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) , die jeder Build- oder Versionspipeline hinzugefügt werden kann. Sie benötigen einen [Dienstprinzipal](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) , um die Bereitstellung zu autorisieren, und können dann die Versionsdefinition generieren.

1. Wählen Sie in der Releaseverwaltung **Leer** aus, damit Sie eine leere Definition erstellen können.

    ![Erstellen einer leeren Definition][1]

2. Wählen Sie alle hierzu benötigten Ressourcen aus. Dabei ist wahrscheinlich die Logik-App-Vorlage enthalten, die manuell oder im Rahmen des Erstellungsprozesses generiert wird.
3. Fügen Sie eine Aufgabe für die **Bereitstellung von Azure-Ressourcengruppen** hinzu.
4. Verwenden Sie für die Konfiguration einen [Dienstprinzipal](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/), und verweisen Sie auf die Vorlagen- und Vorlagenparameterdateien.
5. Erstellen Sie nach Bedarf weitere Schritte im Freigabeprozess für andere Umgebungen, automatisierte Tests oder genehmigende Personen.

<!-- Image References -->
[1]: ./media/logic-apps-create-deploy-template/emptyreleasedefinition.png

