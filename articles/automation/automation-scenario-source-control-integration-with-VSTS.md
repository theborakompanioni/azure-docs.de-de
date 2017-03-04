---
title: Integrieren von Azure Automation mit Visual Studio Team Services-Quellcodeverwaltung | Microsoft-Dokumentation
description: "In diesem Szenario wird das Einrichten der Integration mit einem Azure Automation-Konto und der Visual Stuido Team Services-Quellcodeverwaltung erläutert."
services: automation
documentationcenter: 
author: eamono
manager: 
editor: 
keywords: Azure PowerShell, VSTS, Quellcodeverwaltung, Automation
ms.assetid: a43b395a-e740-41a3-ae62-40eac9d0ec00
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
translationtype: Human Translation
ms.sourcegitcommit: f68563587d375dde22fd1ca45591fd49042533d0
ms.openlocfilehash: 01f9c01c9e04e02dbb548b68cf99684ba6ddd57e
ms.lasthandoff: 02/21/2017

---
# <a name="azure-automation-scenario---automation-source-control-integration-with-visual-studio-team-services"></a>Azure Automation-Szenario – Integration der Automation-Quellcodeverwaltung mit Visual Studio Team Services

In diesem Szenario verwenden Sie ein Visual Studio Team Services-Projekt, mit dem Sie Azure Automation-Runbooks oder DSC-Konfigurationen unter Quellcodeverwaltung verwalten.
Dieser Artikel beschreibt, wie VSTS in Ihre Azure Automation-Umgebung integriert wird, sodass bei jedem Einchecken Continuous Integration erfolgt.

## <a name="getting-the-scenario"></a>Abrufen des Szenarios

Dieses Szenario basiert auf zwei PowerShell-Runbooks, die Sie direkt aus dem [Runbookkatalog](automation-runbook-gallery.md) des Azure-Portals importieren oder aus dem [PowerShell-Katalog](https://www.powershellgallery.com) herunterladen können.

### <a name="runbooks"></a>Runbooks

Runbook | Beschreibung| 
--------|------------|
Sync-VSTS | Importieren Sie Runbooks oder Konfigurationen aus der VSTS-Quellcodeverwaltung, wenn ein Eincheckvorgang ausgeführt wird. Bei der manuellen Ausführung werden alle Runbooks oder Konfigurationen in das Automation-Konto importiert und veröffentlicht.| 
Sync-VSTSGit | Importieren Sie Runbooks oder Konfigurationen aus VSTS unter Git-Quellcodeverwaltung, wenn ein Eincheckvorgang ausgeführt wird. Bei der manuellen Ausführung werden alle Runbooks oder Konfigurationen in das Automation-Konto importiert und veröffentlicht.|

### <a name="variables"></a>Variablen

Variable | Beschreibung|
-----------|------------|
VSToken | Sichern Sie das erstellte Variablenasset, das das persönliche VSTS-Zugriffstoken enthält. Sie finden Informationen zum Erstellen eines persönlichen VSTS-Zugriffstokens auf der [VSTS-Authentifizierungsseite](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview). 
## <a name="installing-and-configuring-this-scenario"></a>Installieren und Konfigurieren dieses Szenarios

Erstellen Sie ein [persönliches Zugriffstoken](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview) in VSTS, mit dem Sie die Runbooks oder Konfigurationen in Ihrem Automation-Konto synchronisieren.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

Erstellen Sie eine [sichere Variable](automation-variables.md) in Ihrem Automation-Konto, die das persönliche Zugriffstoken aufnimmt, damit das Runbook bei VSTS authentifiziert werden kann und die Runbooks oder Konfigurationen im Automation-Konto synchronisiert werden können. Sie können es VSToken nennen. 

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

Importieren Sie das Runbook, das Ihre Runbooks oder Konfigurationen im Automation-Konto synchronisiert. Sie können das [VSTS-Beispielrunbook](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) oder das [VSTS-mit-Git-Beispielrunbook] (https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) von „PowerShellGallery.com“ verwenden. Dies hängt davon ab, ob Sie die VSTS-Quellcodeverwaltung oder VSTS mit Git verwenden. Stellen Sie das Runbook in Ihrem Automation-Konto bereit.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

Sie können dieses Runbook jetzt [veröffentlichen](automation-creating-importing-runbook.md#publishing-a-runbook), sodass Sie einen Webhook erstellen können. 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

Erstellen Sie einen [Webhook](automation-webhooks.md) für dieses Sync-VSTS-Runbook, und geben Sie die Parameter an, wie unten dargestellt. Sie müssen die Webhook-URL kopieren, da Sie sie für einen Service Hook in VSTS benötigen. VSAccessTokenVariableName ist der Name (VSToken) der sicheren Variablen, die Sie zuvor für das persönliche Zugriffstoken erstellt haben. 

Für das Integrieren in VSTS (Sync-VSTS.ps1) werden die folgenden Parameter verwendet.
### <a name="sync-vsts-parameters"></a>Sync-VSTS-Parameter

Parameter | Beschreibung| 
--------|------------|
WebhookData | Dieser Parameter enthält die vom VSTS Service Hook gesendeten Informationen über das Einchecken. Sie sollten diesen Parameter leer lassen.| 
ResourceGroup | Dies ist der Name der Ressourcengruppe, in der sich das Automation-Konto befindet.|
AutomationAccountName | Der Name des Automation-Kontos, das mit VSTS synchronisiert wird.|
VSFolder | Der Name des Ordners in VSTS, in dem sich die Runbooks und Konfigurationen befinden.|
VSAccount | Der Name des Visual Studio Team Services-Kontos.| 
VSAccessTokenVariableName | Der Name der sicheren Variablen (VSToken), die das persönliche VSTS-Zugriffstoken enthält.| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

Wenn Sie VSTS mit Git (Sync-VSTSGit.ps1) verwenden, werden die folgenden Parameter verwendet.

Parameter | Beschreibung|
--------|------------|
WebhookData | Dieser Parameter enthält die vom VSTS Service Hook gesendeten Informationen über das Einchecken. Sie sollten diesen Parameter leer lassen.| ResourceGroup | Dies ist der Name der Ressourcengruppe, in der sich das Automation-Konto befindet.|
AutomationAccountName | Der Name des Automation-Kontos, das mit VSTS synchronisiert wird.|
VSAccount | Der Name des Visual Studio Team Services-Kontos.|
VSProject | Der Name des Projekts in VSTS, in dem sich die Runbooks und Konfigurationen befinden.|
GitRepo | Der Name des Git-Repositorys.|
GitBranch | Der Name der Verzweigung im VSTS-Git-Repository.|
Ordner | Der Name des Ordners in der VSTS-Git-Verzweigung.|
VSAccessTokenVariableName | Der Name der sicheren Variablen (VSToken), die das persönliche VSTS-Zugriffstoken enthält.|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Erstellen Sie einen Service Hook in VSTS für Eincheckvorgänge in den Ordner, der diesen Webhook beim Einchecken von Code auslöst. Wählen Sie Webhooks als Dienst für die Integration aus, wenn Sie ein neues Abonnement erstellen. Weitere Informationen zu Service Hooks finden Sie in der [VSTS Service Hooks-Dokumentation](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

Sie sollten jetzt in der Lage sein, alle Eincheckvorgänge Ihrer Runbooks und Konfigurationen in VSTS durchzuführen und diese automatisch in Ihrem Automation-Konto zu synchronisieren.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Wenn Sie dieses Runbook manuell ausführen, ohne dass es von VSTS ausgelöst wird, können Sie den webhookdata-Parameter leer lassen. Dann wird eine vollständige Synchronisierung aus dem angegebenen VSTS-Ordner durchgeführt.

Wenn Sie das Szenario deinstallieren möchten, entfernen Sie den Service Hook aus VSTS, löschen Sie das Runbook und die VSToken-Variable.

