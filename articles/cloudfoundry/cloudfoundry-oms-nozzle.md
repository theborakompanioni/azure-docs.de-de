---
title: "Bereitstellen von Azure Log Analytics Nozzle zur Überwachung von Cloud Foundry | Microsoft-Dokumentation"
description: "Schritt-für-Schritt-Anleitung zum Bereitstellen von Cloud Foundry Loggregator Nozzle für Azure Log Analytics, zum Konfigurieren von Azure Log Analytics und der OMS-Konsole sowie zum Überwachen der Integritäts- und Leistungsmetriken des Cloud Foundry-Systems mithilfe dieser Tools."
services: virtual-machines-linux
documentationcenter: 
author: ningk
manager: timlt
editor: 
tags: Cloud-Foundry
ms.assetid: 00c76c49-3738-494b-b70d-344d8efc0853
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/22/2017
ms.author: ningk
ms.translationtype: HT
ms.sourcegitcommit: 12c20264b14a477643a4bbc1469a8d1c0941c6e6
ms.openlocfilehash: f704a2638a4b6b57c014d502dd87303a55334672
ms.contentlocale: de-de
ms.lasthandoff: 09/07/2017

---

# <a name="deploy-azure-log-analytics-nozzle-for-cloud-foundry-system-monitoring"></a>Bereitstellen von Azure Log Analytics Nozzle zur Überwachung des Cloud Foundry-Systems

## <a name="background"></a>Hintergrund

[Azure Log Analytics](https://azure.microsoft.com/services/log-analytics/) ist ein Dienst aus der [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/) (OMS) von Microsoft. Er dient zum Erfassen und Analysieren von Daten, die von Ihren cloudbasierten und lokalen Umgebungen generiert werden.

Microsoft Azure Log Analytics Nozzle (die Nozzle-Komponente) ist eine Cloud Foundry-Komponente, die Metriken aus der Firehose-Komponente von [Cloud Foundry Loggregator](https://docs.cloudfoundry.org/loggregator/architecture.html) an Azure Log Analytics weiterleitet. Mithilfe der Nozzle-Komponente können Sie Integritäts- und Leistungsmetriken für Ihr Cloud Foundry-System über mehrere Bereitstellungen hinweg sammeln, anzeigen, analysieren.

In diesem Dokument erfahren Sie, wie Sie die Nozzle-Komponente in Ihrer Cloud Foundry-Umgebung bereitstellen und über die Azure Log Analytics-OMS-Konsole auf die Daten zugreifen.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="1-deploy-a-cf-or-pcf-environment-in-azure"></a>1. Bereitstellen einer CF- oder PCF-Umgebung in Azure

Sie können die Nozzle-Komponente entweder mit einer Open Source-CF-Bereitstellung (Cloud Foundry) oder mit einer PCF-Bereitstellung (Pivotal Cloud Foundry) verwenden.

* [Deploy Cloud Foundry on Azure](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md) (Bereitstellen von Cloud Foundry in Azure)

* [Installing Pivotal Cloud Foundry on Azure](https://docs.pivotal.io/pivotalcf/1-11/customizing/azure.html) (Installieren von Pivotal Cloud Foundry in Azure)

### <a name="2-install-the-cf-command-line-tools-for-deploying-the-nozzle"></a>2. Installieren der CF-Befehlszeilentools zum Bereitstellen der Nozzle-Komponente

Die Nozzle-Komponente wird als Anwendung in der CF-Umgebung ausgeführt. Zum Bereitstellen der Anwendung benötigen Sie die CF-Befehlszeilenschnittstelle. Außerdem sind Zugriffsberechtigungen für die Loggregator-Firehose-Komponente und den Cloudcontroller erforderlich, und Sie benötigen den UAA-Befehlszeilenclient, um den Benutzer zu erstellen und zu konfigurieren.

* [Installing the cf CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html) (Installieren der CF-Befehlszeilenschnittstelle)

* [CloudFoundry UAA Command Line Client](https://github.com/cloudfoundry/cf-uaac/blob/master/README.md) (Cloud Foundry-UAA-Befehlszeilenclient)

Vergewissern Sie sich vor dem Einrichten des UAA-Befehlszeilenclients, dass Rubygems installiert ist.

### <a name="3-create-an-oms-workspace-in-azure"></a>3. Erstellen eines OMS-Arbeitsbereichs in Azure

#### <a name="create-the-oms-workspace-manually"></a>Manuelles Erstellen des OMS-Arbeitsbereichs

Sie können den OMS-Arbeitsbereich manuell erstellen und die vorkonfigurierten OMS-Ansichten und -Warnungen nach Abschluss der Nozzle-Bereitstellung laden.

1. Suchen Sie im Azure-Portal in der Marketplace-Dienstliste nach „Log Analytics“, und wählen Sie „Log Analytics“ aus.
2. Klicken Sie auf „Erstellen“, und wählen Sie anschließend Optionen für die folgenden Elemente aus:

* OMS-Arbeitsbereich: Geben Sie einen Namen für Ihren Arbeitsbereich ein.
* Abonnement: Falls Sie über mehrere Abonnements verfügen, wählen Sie das Abonnement mit Ihrer CF-Bereitstellung aus.
* Ressourcengruppe: Sie können eine neue Ressourcengruppe erstellen oder die gleiche wie für Ihre CF-Bereitstellung verwenden.
* Standort
* Tarif: Klicken Sie auf „OK“, um den Vorgang abzuschließen.
> Weitere Informationen finden Sie unter [Erste Schritte mit Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started).

#### <a name="create-the-oms-workspace-through-the-oms-template"></a>Erstellen des OMS-Arbeitsbereichs mithilfe der OMS-Vorlage

Mithilfe der [Azure OMS Log Analytics-Lösung für Cloud Foundry](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-cloudfoundry-solution) können Sie den OMS-Arbeitsbereich erstellen und die vorkonfigurierten OMS-Ansichten und -Benachrichtigungen laden.

## <a name="deploy-the-nozzle"></a>Bereitstellen der Nozzle-Komponente

### <a name="deploy-the-nozzle-as-a-pcf-ops-manager-tile"></a>Bereitstellen der Nozzle-Komponente als PCF Operations Manager-Kachel

Wenn Sie PCF über Operations Manager bereitgestellt haben, gehen Sie wie unter [Installing and Configuring Microsoft Azure Log Analytics Nozzle for PCF](http://docs.pivotal.io/partners/azure-log-analytics-nozzle/installing.html) (Installieren und Konfigurieren von Microsoft Azure Log Analytics Nozzle für PCF) beschrieben vor. Die Nozzle-Komponente wird als Kachel mit Operations Manager installiert.

### <a name="deploy-the-nozzle-as-an-application-to-cloud-foundry"></a>Bereitstellen der Nozzle-Komponente als Anwendung in Cloud Foundry

Wenn Sie nicht PCF Operations Manager verwenden, müssen Sie die Nozzle-Komponente als Anwendung mithilfe von Push übertragen.

#### <a name="log-in-to-your-cf-deployment-as-an-admin-through-cf-cli"></a>Melden Sie sich über die CF-Befehlszeilenschnittstelle als Administrator bei Ihrer CF-Bereitstellung an.

Führen Sie in Ihrer Dev Box den folgenden Befehl aus:
```
cf login -a https://api.${SYSTEM_DOMAIN} -u ${CF_USER} --skip-ssl-validation
```

> „SYSTEM_DOMAIN“ steht für Ihren CF-Domänennamen. Diesen können Sie ermitteln, indem Sie in Ihrer CF-Bereitstellungsmanifestdatei nach „SYSTEM_DOMAIN“ suchen. 
> „CF_User“ ist der CF-Administratorname. Zum Ermitteln des Namens und Kennworts können Sie den Abschnitt „scim“ in Ihrer CF-Bereitstellungsmanifestdatei nach dem Namen und nach „cf_admin_password“ durchsuchen.

#### <a name="create-a-cf-user-and-grant-required-privileges"></a>Erstellen Sie einen CF-Benutzer, und gewähren Sie die erforderlichen Berechtigungen.

Führen Sie in Ihrer Dev Box die folgenden Befehle aus:
```
uaac target https://uaa.${SYSTEM_DOMAIN} --skip-ssl-validation
uaac token client get admin
cf create-user ${FIREHOSE_USER} ${FIREHOSE_USER_PASSWORD}
uaac member add cloud_controller.admin ${FIREHOSE_USER}
uaac member add doppler.firehose ${FIREHOSE_USER}
```

> „SYSTEM_DOMAIN“ steht für Ihren CF-Domänennamen. Diesen können Sie ermitteln, indem Sie in Ihrer CF-Bereitstellungsmanifestdatei nach „SYSTEM_DOMAIN“ suchen.

#### <a name="download-the-latest-azure-log-analytics-nozzle-release"></a>Laden Sie die neueste Version von Azure Log Analytics Nozzle herunter.

Führen Sie in Ihrer Dev Box den folgenden Befehl aus:
```
git clone https://github.com/Azure/oms-log-analytics-firehose-nozzle.git
cd oms-log-analytics-firehose-nozzle
```

#### <a name="set-environment-variables-in-manifestyml-in-your-current-directory"></a>Legen Sie in Ihrem aktuellen Verzeichnis in „manifest.yml“ Umgebungsvariablen fest.

Hierbei handelt es sich um das App-Manifest für die Nozzle-Komponente. Ersetzen Sie den Wert durch die spezifischen Informationen für Ihren OMS-Arbeitsbereich.

```
OMS_WORKSPACE             : OMS workspace ID: open OMS portal from your OMS workspace, click "Settings", click "connected sources"
OMS_KEY                   : OMS key: open OMS portal from your OMS workspace, click "Settings", click "connected sources"
OMS_POST_TIMEOUT          : HTTP post timeout for sending events to OMS Log Analytics, default is 10s.
OMS_BATCH_TIME            : Interval for posting a batch to OMS Log Analytics, default is 10s.
OMS_MAX_MSG_NUM_PER_BATCH : The max number of messages in a batch to OMS Log Analytics, default is 1000.
API_ADDR                  : The api URL of the CF environment, refer to "Push the Nozzle as an App to Cloud Foundry" section step 1 on how to retrive your <CF_SYSTEM_DOMAIN>
DOPPLER_ADDR              : Loggregator's traffic controller URL, refer to "Deploy the Nozzle as an App to Cloud Foundry" section step 1 on how to retrive your <CF_SYSTEM_DOMAIN>
FIREHOSE_USER             : CF user you created in "Push the Nozzle as an App to Cloud Foundry" section, who has firehose and Cloud Controller admin access.
FIREHOSE_USER_PASSWORD    : Password of the CF user above.
EVENT_FILTER              : Event types to be filtered out. The format is a comma separated list, valid event types are METRIC,LOG,HTTP
SKIP_SSL_VALIDATION       : If true, allows insecure connections to the UAA and the Trafficcontroller
CF_ENVIRONMENT            : Enter any string value for identifying logs and metrics from different CF environments
IDLE_TIMEOUT              : Keep Alive duration for the firehose consumer, default is 60s.
LOG_LEVEL                 : Logging level of the nozzle, valid levels: DEBUG, INFO, ERROR
LOG_EVENT_COUNT           : If true, the total count of events that the nozzle has received and sent will be logged to OMS Log Analytics as CounterEvents
LOG_EVENT_COUNT_INTERVAL  : The time interval of logging event count to OMS Log Analytics, default is 60s.
```

### <a name="push-the-application-from-your-dev-box"></a>Übertragen Sie die Anwendung mithilfe von Push über Ihre Dev Box.

Vergewissern Sie sich, dass Sie sich im Ordner „oms-log-analytics-firehose-nozzle“ befinden, und führen Sie dann Folgendes aus:
```
cf push
```

## <a name="validate-the-nozzle-installation"></a>Überprüfen der Nozzle-Installation

### <a name="from-apps-manager-for-pcf"></a>Über Apps Manager (für PCF)

1. Melden Sie sich bei Operations Manager an, und vergewissern Sie sich, dass die Kachel auf dem Installationsdashboard angezeigt wird.
2. Melden Sie bei Apps Manager an, und vergewissern Sie sich, dass der für die Nozzle-Komponente erstellte Bereich im Nutzungsbericht aufgeführt wird und der Status normal ist.

### <a name="from-dev-box"></a>Über die Dev Box

Geben Sie im Fenster der CF-Befehlszeilenschnittstelle Ihrer Dev Box Folgendes ein:
```
cf apps
```
Vergewissern Sie sich, dass die OMS-Nozzle-Anwendung ausgeführt wird.

## <a name="view-the-data-in-oms-portal"></a>Anzeigen der Daten im OMS-Portal

### <a name="1-import-oms-view"></a>1. Importieren der OMS-Ansicht

Navigieren Sie im OMS-Portal zu **Ansicht-Designer** -> **Importieren** -> **Durchsuchen**, wählen Sie eine der OMSVIEW-Dateien aus (beispielsweise *Cloud Foundry.omsview*), und speichern Sie die Ansicht. Daraufhin wird auf der OMS-Hauptübersichtsseite eine **Kachel** angezeigt. Klicken Sie auf die **Kachel**. Sie zeigt visualisierte Metriken an.

Bediener können diese Ansichten über den **Ansicht-Designer** anpassen oder neue Ansichten erstellen.

*Cloud Foundry.omsview* ist eine Vorschauversion der Cloud Foundry-OMS-Ansichtsvorlage. Eine vollständig konfigurierte Standardvorlage ist in Arbeit. Vorschläge und Feedback können Sie im [Problemabschnitt](https://github.com/Azure/oms-log-analytics-firehose-nozzle/issues) übermitteln.

### <a name="2-create-alert-rules"></a>2. Erstellen von Warnungsregeln

Bediener können [die Warnungen erstellen](https://docs.microsoft.com/azure/log-analytics/log-analytics-alerts). Abfragen und Schwellenwerte können nach Bedarf angepasst werden. Im Anschluss finden Sie eine Reihe empfohlener Warnungen:

| Suchabfrage                                                                  | Warnung generieren basierend auf | Beschreibung                                                                       |
| ----------------------------------------------------------------------------- | ----------------------- | --------------------------------------------------------------------------------- |
| Type=CF_ValueMetric_CL Origin_s=bbs Name_s="Domain.cf-apps"                   | Anzahl von Ergebnissen < 1   | **bbs.Domain.cf-apps** gibt an, ob die Domäne „cf-apps“ auf dem neuesten Stand ist, was bedeutet, dass CF-App-Anforderungen des Cloudcontrollers mit „bbs.LRPsDesired“ (von Diego gewünschte KIs) für die Ausführung synchronisiert sind. Wenn keine Daten empfangen werden, bedeutet das, dass die Domäne „cf-apps“ im angegebenen Zeitraum nicht auf dem neuesten Stand ist. |
| Type=CF_ValueMetric_CL Origin_s=rep Name_s=UnhealthyCell Value_d>1            | Anzahl von Ergebnissen > 0   | Für Diego-Zellen bedeutet der Wert 0 fehlerfrei und der Wert 1 fehlerhaft. Legen Sie die Warnung fest, wenn im angegebenen Zeitfenster mehrere **fehlerhafte Diego-Zellen** erkannt werden. |
| Type=CF_ValueMetric_CL Origin_s="bosh-hm-forwarder" Name_s="system.healthy" Value_d=0 | Anzahl von Ergebnissen > 0 | Bei 1 ist das System fehlerfrei, bei 0 nicht. |
| Type=CF_ValueMetric_CL Origin_s=route_emitter Name_s=ConsulDownMode Value_d>0 | Anzahl von Ergebnissen > 0   | Consul gibt in regelmäßigen Abständen seinen Integritätsstatus aus. Bei 0 ist das System fehlerfrei. Bei 1 hat der Routen-Emitter den **Ausfall von Consul** festgestellt. |
| Type=CF_CounterEvent_CL Origin_s=DopplerServer (Name_s="TruncatingBuffer.DroppedMessages" or Name_s="doppler.shedEnvelopes") Delta_d>0 | Anzahl von Ergebnissen > 0 | Die Deltaanzahl von Nachrichten, die von Doppler aufgrund eines Rückstaus vorsätzlich **gelöscht** wurden. |
| Type=CF_LogMessage_CL SourceType_s=LGR MessageType_s=ERR                      | Anzahl von Ergebnissen > 0   | Loggregator gibt **LGR** aus, um auf Probleme mit dem Protokollierungsprozess hinzuweisen (beispielsweise, wenn zu viele Protokollmeldungen ausgegeben werden). |
| Type=CF_ValueMetric_CL Name_s=slowConsumerAlert                               | Anzahl von Ergebnissen > 0   | Wenn die Nozzle-Komponente von Loggregator eine Warnung wegen eines langsamen Consumers empfängt, sendet sie „**SlowConsumerAlert** ValueMetric“ an OMS. |
| Type=CF_CounterEvent_CL Job_s=nozzle Name_s=eventsLost Delta_d>0              | Anzahl von Ergebnissen > 0   | Wenn die Deltaanzahl **verloren gegangener Ereignisse** einen Schwellenwert erreicht, liegt unter Umständen ein Problem mit der Nozzle-Ausführung vor. |

## <a name="scale"></a>Skalieren

### <a name="scale-the-nozzle"></a>Skalieren der Nozzle-Komponente

Es empfiehlt sich für Bediener, mit mindestens zwei Nozzle-Instanzen zu beginnen. Die Firehose-Komponente verteilt die Workload auf alle Instanzen der Nozzle-Komponente.
Um sicherzustellen, dass die Nozzle-Komponente mit dem Datenverkehr aus der Firehose-Komponente Schritt halten kann, sollten Bediener die Warnung **slowConsumerAlert** aus dem Abschnitt „Erstellen von Warnungsregeln“ einrichten. Orientieren Sie sich im Falle einer Warnung am [Leitfaden für eine langsame Nozzle-Komponente](https://docs.pivotal.io/pivotalcf/1-11/loggregator/log-ops-guide.html#slow-noz), um bestimmen, ob eine Skalierung erforderlich ist.
Verwenden Sie zum Skalieren der Nozzle-Komponente entweder Apps Manager oder die CF-Befehlszeilenschnittstelle, um die Instanzanzahl oder die Arbeitsspeicher-/Datenträgerressourcen für die Nozzle-Komponente zu erhöhen.

### <a name="scale-the-loggregator"></a>Skalieren von Loggregator

Loggregator sendet die Protokollmeldung **LGR**, um auf Probleme mit dem Protokollierungsprozess hinzuweisen. Der Bediener kann die Warnung überwachen, um zu ermitteln, ob Loggregator zentral hochskaliert werden muss.
Um Loggregator zentral hochzuskalieren, kann der Bediener entweder die Doppler-Puffergröße erhöhen oder zusätzliche Doppler-Serverinstanzen im CF-Manifest hinzufügen. Ausführliche Informationen finden Sie im [Leitfaden zum Skalieren von Loggregator](https://docs.cloudfoundry.org/running/managing-cf/logging-config.html#scaling).

## <a name="update"></a>Aktualisieren

Wenn Sie die Nozzle-Komponente mit einer neueren Version aktualisieren möchten, laden Sie die neue Nozzle-Version herunter, führen Sie die Schritte im Bereitstellungsabschnitt aus, und übermitteln Sie die Anwendung erneut mithilfe von Push.

Wenn Sie die Nozzle-Komponente entfernen möchten, gehen Sie wie folgt vor:

### <a name="from-the-ops-manager"></a>Über Operations Manager

1. Melden Sie sich bei Operations Manager an.
2. Suchen Sie nach der Kachel „Microsoft Azure Log Analytics Nozzle für PCF“.
3. Klicken Sie auf das Papierkorbsymbol, und bestätigen Sie den Löschvorgang.

### <a name="from-the-dev-box"></a>Über die Dev Box

Geben Sie im Fenster der CF-Befehlszeilenschnittstelle Folgendes ein:
```
cf delete <App Name> -r
```

Wenn Sie die Nozzle-Komponente entfernen, werden die Daten im OMS-Portal nicht automatisch entfernt, sondern laufen gemäß der Aufbewahrungseinstellung für OMS Log Analytics ab.

## <a name="support-and-feedback"></a>Support und Feedback

Die Azure Log Analytics-Nozzle ist eine Open Source-Lösung. Fragen und Feedback können Sie im [GitHub-Abschnitt](https://github.com/Azure/oms-log-analytics-firehose-nozzle/issues) übermitteln. Wenn Sie eine Azure-Supportanfrage erstellen möchten, verwenden Sie „Virtueller Computer mit Cloud Foundry“ als Dienstkategorie. 

## <a name="next-step"></a>Nächster Schritt

Neben den durch die Nozzle-Komponente abgedeckten Cloud Foundry-Metriken können Sie auch den OMS-Agent verwenden, um Informationen zu Betriebsdaten auf der Ebene des virtuellen Computers (Syslog, Leistung, Warnungen, Bestand) zu erhalten. Der Client wird als BOSH-Add-On auf Ihren virtuellen CF-Computern installiert.
> Ausführliche Informationen finden Sie unter [Deploy OMS agent to your Cloud Foundry deployment](https://github.com/Azure/oms-agent-for-linux-boshrelease) (Bereitstellen des OMS-Agents für Ihre Cloud Foundry-Bereitstellung).
