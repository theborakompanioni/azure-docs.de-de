---
title: "CLI 1.0-Beispiele für den Schnellstart mit Azure Monitor. | Microsoft Docs"
description: "Beispiel-CLI 1.0-Befehle für Azure Monitor-Features. Azure Monitor ist ein Microsoft Azure-Dienst, der das Senden von Warnbenachrichtigungen, das Aufrufen von Web-URLs basierend auf Werten konfigurierter Telemetriedaten und die automatische Skalierung von Clouddiensten, virtuellen Computern und Web-Apps ermöglicht."
author: kamathashwin
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1653aa81-0ee6-4622-9c65-d4801ed9006f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: ashwink
ms.translationtype: HT
ms.sourcegitcommit: 760543dc3880cb0dbe14070055b528b94cffd36b
ms.openlocfilehash: ec4512500dc3c77a40d2ebd1e6b460d5bb005811
ms.contentlocale: de-de
ms.lasthandoff: 08/10/2017

---
# <a name="azure-monitor--cross-platform-cli-10-quick-start-samples"></a>Azure Monitor – Schnellstartbeispiele für die plattformübergreifende CLI 1.0
In diesem Artikel werden Beispielbefehle für die Befehlszeilenschnittstelle (Command Line Interface, CLI) beschrieben, mit denen Sie auf Azure Monitor-Features zugreifen können. Azure Monitor ermöglicht die automatische Skalierung von Clouddiensten, virtuellen Computern und Web-Apps sowie das Senden von Warnbenachrichtigungen oder das Aufrufen von Web-URLs basierend auf Werten konfigurierter Telemetriedaten.

> [!NOTE]
> Azure Monitor ist der neue Name für den Dienst, der bis 25. September 2016 als „Azure Insights“ bezeichnet wurde. Die Namespaces und somit die folgenden Befehle enthalten jedoch weiterhin den Bezeichner „insights“.
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
Wenn Sie die Azure-Befehlszeilenschnittstelle noch nicht installiert haben, finden Sie alle erforderlichen Informationen unter [Installieren der Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md). Falls Sie mit der Azure-Befehlszeilenschnittstelle noch nicht vertraut sind, helfen Ihnen die Informationen unter [Verwenden der plattformübergreifenden Azure-Befehlszeilenschnittstelle mit dem Azure-Ressourcen-Manager](../xplat-cli-azure-resource-manager.md)weiter.

Installieren Sie npm unter Windows über die [Node.js-Website](https://nodejs.org/). Führen Sie nach Abschluss der Installation Folgendes in dem Ordner aus, in dem npm installiert ist, indem Sie „CMD.exe“ mit der Berechtigung „Als Administrator ausführen“ verwenden:

```console
npm install azure-cli --global
```

Navigieren Sie als Nächstes zum gewünschten Ordner bzw. Speicherort, und geben Sie an der Befehlszeile Folgendes ein:

```console
azure help
```

## <a name="log-in-to-azure"></a>Anmelden an Azure
Der erste Schritt besteht darin, sich am Azure-Konto anzumelden.

```console
azure login
```

Nach der Ausführung dieses Befehls müssen Sie sich über die Anweisungen auf dem Bildschirm anmelden. Anschließend werden Ihr Konto, die Mandanten-ID und die standardmäßige Abonnement-ID angezeigt. Alle Befehle funktionieren im Kontext Ihres Standardabonnements.

Verwenden Sie den folgenden Befehl, um die Details Ihres aktuellen Abonnements aufzulisten.

```console
azure account show
```

Verwenden Sie den folgenden Befehl, um den Arbeitskontext in ein anderes Abonnement zu ändern.

```console
azure account set "subscription ID or subscription name"
```

Zum Verwenden von Azure Resource Manager- und Azure Monitor-Befehlen müssen Sie im Azure Resource Manager-Modus arbeiten.

```console
azure config mode arm
```

Führen Sie Folgendes aus, um eine Liste mit allen unterstützten Azure Monitor-Befehlen anzuzeigen.

```console
azure insights
```

## <a name="view-activity-log-for-a-subscription"></a>Anzeigen des Aktivitätsprotokolls für ein Abonnement
Führen Sie die folgenden Schritte aus, um eine Liste mit Aktivitätsprotokollereignissen anzuzeigen.

```console
azure insights logs list [options]
```

Probieren Sie Folgendes aus, um alle verfügbaren Optionen anzuzeigen:

```console
azure insights logs list -help
```

Beispiel für das Auflisten von Protokollen nach resourceGroup

```console
azure insights logs list --resourceGroup "myrg1"
```

Beispiel für das Auflisten von Protokollen nach Aufrufer

```console
azure insights logs list --caller "myname@company.com"
```

Beispiel für das Auflisten von Protokollen nach Aufrufer für einen Ressourcentyp innerhalb eines Zeitraums mit Start- und Enddatum

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Verwenden von Warnungen
Sie können die Informationen in diesem Abschnitt nutzen, um Warnungen zu verwenden.

### <a name="get-alert-rules-in-a-resource-group"></a>Abrufen von Warnungsregeln in einer Ressourcengruppe
```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Erstellen einer metrischen Warnungsregel
```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-webtest-alert-rule"></a>Erstellen einer Webtest-Warnungsregel
```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Löschen einer Warnungsregel
```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Protokollprofile
Verwenden Sie die Informationen in diesem Abschnitt, um Protokollprofile zu verwenden.

### <a name="get-a-log-profile"></a>Abrufen eines Protokollprofils
```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Hinzufügen eines Protokollprofils ohne Aufbewahrung
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Entfernen eines Protokollprofils
```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Hinzufügen eines Protokollprofils mit Aufbewahrung
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Hinzufügen eines Protokollprofils mit Aufbewahrung und Event Hub
```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Diagnose
Verwenden Sie die Informationen in diesem Abschnitt, um Diagnoseeinstellungen zu verwenden.

### <a name="get-a-diagnostic-setting"></a>Abrufen einer Diagnoseeinstellung
```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>Deaktivieren einer Diagnoseeinstellung
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>Aktivieren einer Diagnoseeinstellung ohne Aufbewahrung
```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Autoscale
Nutzen Sie die Informationen in diesem Abschnitt, um Einstellungen für die automatische Skalierung zu verwenden. Sie müssen diese Beispiele ändern.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Abrufen der Einstellungen für die automatische Skalierung für eine Ressourcengruppe
```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Abrufen der Einstellungen für die automatische Skalierung nach Name in einer Ressourcengruppe
```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Festlegen der Einstellungen für die automatische Skalierung
```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```

