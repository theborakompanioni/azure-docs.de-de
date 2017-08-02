---
title: "Azure Service Fabric-Anwendung für die Patchorchestrierung | Microsoft-Dokumentation"
description: Anwendung zum Automatisieren von Betriebssystempatches in einem Service Fabric-Cluster.
services: service-fabric
documentationcenter: .net
author: novino
manager: timlt
editor: 
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/9/2017
ms.author: nachandr
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: db6e654de074fc6651fd0d7479ee52038f944745
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---

# <a name="patch-the-windows-operating-system-in-your-service-fabric-cluster"></a>Patchen des Windows-Betriebssystem in Ihrem Service Fabric-Cluster

Die Anwendung für die Patchorchestrierung ist eine Service Fabric-Anwendung, mit der das Aufspielen von Betriebssystempatches in einem Service Fabric-Cluster in Azure ohne Ausfallzeiten automatisiert werden kann.

Die App für die Patchorchestrierung bietet Folgendes:

- **Automatische Installation von Betriebssystemupdates**. Betriebssystemupdates werden automatisch heruntergeladen und installiert. Clusterknoten werden bei Bedarf ohne Ausfall des Clusters neu gestartet.

- **Clusterfähiges Patchen und Integration von Integrität**. Beim Anwenden von Updates überwacht die App für die Patchorchestrierung die Integrität der Clusterknoten. Knoten im Cluster werden einzeln oder in Upgradedomänen aktualisiert. Wenn die Integrität des Clusters aufgrund des Patchingprozesses abnimmt, wird das Patchen angehalten, um eine Verschärfung des Problems zu verhindern.

## <a name="internal-details-of-the-app"></a>Interne Details der App

Die App für die Patchorchestrierung besteht aus den folgenden Teilkomponenten:

- **Koordinatordienst**: Dieser zustandsbehaftete Dienst ist für Folgendes zuständig:
    - Koordinieren der Windows Update-Aufträge im gesamten Cluster.
    - Speichern der Ergebnisse der abgeschlossenen Windows Update-Vorgänge.
- **Knoten-Agent-Dienst**: Dieser zustandslose Dienst wird auf allen Service Fabric-Clusterknoten ausgeführt. Der Dienst ist für Folgendes zuständig:
    - Bootstrapping des Knoten-Agent-NT-Diensts.
    - Überwachen des Knoten-Agent-NT-Diensts.
- **Knoten-Agent-NT-Dienst**: Dieser Windows NT-Dienst wird mit Berechtigungen einer höheren Ebene ausgeführt (SYSTEM). Im Gegensatz dazu werden der Knoten-Agent-Dienst und der Koordinatordienst mit niedrigeren Berechtigungen ausgeführt (NETZWERKDIENST). Der Dienst ist für die Ausführung der folgenden Windows Update-Aufträge auf allen Clusterknoten zuständig:
    - Deaktivieren der automatischen Windows Update-Funktion auf dem Knoten.
    - Herunterladen und Installieren von Windows Update entsprechend der vom Benutzer bereitgestellten Richtlinie.
    - Neustarten des Computers nach der Windows Update-Installation.
    - Hochladen der Ergebnisse von Windows-Updates in den Koordinatordienst.
    - Senden von Integritätsberichten, falls der Vorgang nach allen Wiederholungsversuchen fehlschlägt.

> [!NOTE]
> Die App für die Patchorchestrierung verwendet den Reparatur-Manager-Systemdienst in Service Fabric, um die Knoten zu aktivieren und zu deaktivieren und um Integritätsprüfungen durchzuführen. Der von der App für die Patchorchestrierung erstellte Reparaturtask verfolgt den Windows Update-Fortschritt für jeden Knoten nach.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="minimum-supported-service-fabric-runtime-version"></a>Mindestens unterstützte Service Fabric-Laufzeitversion

#### <a name="azure-clusters"></a>Azure-Cluster
Die App für die Patchorchestrierung muss auf Azure-Clustern mit Service Fabric-Laufzeitversion 5.5 oder höher ausgeführt werden.

#### <a name="standalone-on-premises-clusters"></a>Eigenständige lokale Cluster
Die App für die Patchorchestrierung muss auf eigenständigen Clustern mit Service Fabric-Laufzeitversion 5.6 oder höher ausgeführt werden.

### <a name="enable-the-repair-manager-service-if-its-not-running-already"></a>Aktivieren des Reparatur-Manager-Diensts (falls dieser nicht bereits ausgeführt wird)

Für die App für die Patchorchestrierung muss der Reparatur-Manager-Systemdienst im Cluster aktiviert sein.

#### <a name="azure-clusters"></a>Azure-Cluster

In Azure-Clustern auf Dauerhaftigkeitsstufe „Silver“ ist der Reparatur-Manager-Dienst standardmäßig aktiviert. Bei Azure-Clustern mit der Dauerhaftigkeitsstufe „Gold“ ist der Reparatur-Manager-Dienst möglicherweise aktiviert, abhängig davon, wann diese Cluster erstellt wurden. In Azure-Clustern auf Dauerhaftigkeitsstufe „Bronze“ ist der Reparatur-Manager-Dienst standardmäßig nicht aktiviert. Wenn der Dienst bereits aktiviert ist, wird er im Service Fabric Explorer im Abschnitt mit den Systemdiensten aufgeführt.

Sie können den Reparatur-Manager-Dienst über die [Azure Resource Manager-Vorlage](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) für neue und vorhandene Service Fabric-Cluster aktivieren. Rufen Sie die Vorlage für den Cluster ab, den Sie bereitstellen möchten. Sie können entweder die Beispielvorlagen verwenden oder eine benutzerdefinierte Resource Manager-Vorlage erstellen. 

So aktivieren Sie den Reparatur-Manager-Dienst

1. Überprüfen Sie zunächst, ob `apiversion` für die `Microsoft.ServiceFabric/clusters`-Ressource auf `2017-07-01-preview` festgelegt ist, wie im folgenden Codeausschnitt gezeigt. Liegt eine andere Einstellung vor, müssen Sie `apiVersion` auf den Wert `2017-07-01-preview` aktualisieren:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Aktivieren Sie dann den Reparatur-Manager-Dienst, indem Sie folgenden `addonFeatures`-Abschnitt nach dem Abschnitt `fabricSettings` hinzufügen:

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. Nachdem Sie die Clustervorlage mit diesen Änderungen aktualisiert haben, wenden Sie die Änderungen an, und warten Sie, bis das Upgrade abgeschlossen wurde. Sie sehen nun, dass der Reparatur-Manager-Systemdienst im Cluster ausgeführt wird. Der Name im Abschnitt für Systemdienste im Service Fabric Explorer lautet`fabric:/System/RepairManagerService`. 

### <a name="standalone-on-premises-clusters"></a>Eigenständige lokale Cluster

Sie können über die [Konfigurationseinstellungen für eigenständige Windows-Cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) den Reparatur-Manager-Dienst auf neuen und vorhandenen Service Fabric-Clustern aktivieren.

So aktivieren Sie den Reparatur-Manager-Dienst

1. Überprüfen Sie zunächst, dass `apiversion` unter [General cluster configurations](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) (Allgemeine Clusterkonfigurationen) auf `04-2017` oder höher festgelegt ist:

    ```json
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "04-2017",
        ...
    }
    ```

2. Aktivieren Sie dann den Reparatur-Manager-Dienst, indem Sie folgenden `addonFeaturres`-Abschnitt nach dem Abschnitt `fabricSettings` hinzufügen, wie unten gezeigt:

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. Aktualisieren Sie das Clustermanifest mit diesen Änderungen, und verwenden Sie dabei das aktualisierte Clustermanifest zum [Erstellen eines neuen Clusters](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) oder zum [Aktualisieren der Clusterkonfiguration](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server#Upgrade-the-cluster-configuration). Sobald der Cluster mit dem aktualisierten Clustermanifest ausgeführt wird, wird der in Ihrem Cluster ausgeführte Reparatur-Manager-Systemdienst mit dem Namen `fabric:/System/RepairManagerService` im Service Fabric Explorer im Abschnitt „Systemdienste“ angezeigt.

### <a name="disable-automatic-windows-update-on-all-nodes"></a>Deaktivieren der automatischen Windows Update-Funktion auf allen Knoten

Automatische Windows-Updates können zu einer Verringerung der Verfügbarkeit führen, da mehrere Clusterknoten gleichzeitig neu gestartet werden können. Die App für die Patchorchestrierung versucht standardmäßig, die automatische Windows Update-Funktion auf jedem Clusterknoten zu deaktivieren. Wenn die Einstellungen jedoch von einem Administrator bzw. durch eine Gruppenrichtlinie verwaltet werden, empfiehlt es sich, die Windows Update-Richtlinie explizit auf „Bei Download benachrichtigen“ festzulegen.

### <a name="optional-enable-azure-diagnostics"></a>Optional: Aktivieren der Azure-Diagnose

Protokolle für die App für die Patchorchestrierung werden lokal auf jedem Clusterknoten erfasst. Außerdem werden für Cluster mit Service Fabric-Laufzeitversion `5.6.220.9494` und höher Protokolle im Rahmen der Service Fabric-Protokolle erfasst.

Bei Clustern mit einer Service Fabric-Laufzeitversion unter `5.6.220.9494` sollten Sie die Azure-Diagnose konfigurieren, um Protokolle von allen Knoten an einem zentralen Ort hochzuladen.

Weitere Informationen zum Aktivieren der Azure-Diagnose finden Sie unter [Sammeln von Protokollen mit der Azure-Diagnose](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-how-to-setup-wad).

Die Protokolle der App für die Patchorchestrierung werden auf folgenden festen Anbieter-IDs generiert:

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

Wechseln Sie in der Resource Manager-Vorlage zum Abschnitt `EtwEventSourceProviderConfiguration` unter `WadCfg`, und fügen Sie die folgenden Einträge hinzu:

```json
  {
    "provider": "e39b723c-590c-4090-abb0-11e3e6616346",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
      "eventDestination": "PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "fc0028ff-bfdc-499f-80dc-ed922c52c5e9",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "24afa313-0d3b-4c7c-b485-1047fd964b60",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "05dc046c-60e9-4ef7-965e-91660adffa68",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  }
```

> [!NOTE]
> Wenn Ihr Service Fabric-Cluster mehrere Knotentypen umfasst, muss der vorherige Abschnitt für alle `WadCfg`-Abschnitte hinzugefügt werden.

## <a name="download-the-app-package"></a>Herunterladen des App-Pakets

Laden Sie die Anwendung über den [Downloadlink](https://go.microsoft.com/fwlink/P/?linkid=849590) herunter.

## <a name="configure-the-app"></a>Konfigurieren der App

Das Verhalten der App für die Patchorchestrierung kann Ihren Anforderungen entsprechend konfiguriert werden. Überschreiben Sie die Standardwerte, indem Sie während der Erstellung bzw. Aktualisierung einer Anwendung den Anwendungsparameter übergeben. Anwendungsparameter können durch Angeben von `ApplicationParameter` in den Cmdlets `Start-ServiceFabricApplicationUpgrade` oder `New-ServiceFabricApplication` festgelegt werden.

|**Parameter**        |**Typ**                          | **Details**|
|:-|-|-|
|MaxResultsToCache    |Long                              | Maximale Anzahl von Windows Update-Ergebnissen, die zwischengespeichert werden sollen. <br>Der Standardwert ist 3000, wobei Folgendes angenommen wird: <br> – Es sind 20 Knoten vorhanden. <br> – Jeden Monat können fünf Updates auf einem Knoten erfolgen. <br> – Pro Vorgang können zehn Ergebnisse vorliegen. <br> – Es sollen die Ergebnisse für die letzten drei Monaten gespeichert werden. |
|TaskApprovalPolicy   |Enum <br> { NodeWise, UpgradeDomainWise }                          |TaskApprovalPolicy gibt die Richtlinie an, die vom Koordinatordienst zum Installieren von Windows-Updates auf den Service Fabric-Clusterknoten verwendet werden soll.<br>                         Zulässige Werte sind: <br>                                                           <b>NodeWise</b>. Windows Update wird immer nur auf jeweils einem Knoten installiert. <br>                                                           <b>UpgradeDomainWise</b>. Windows Update wird immer nur in jeweils einer Upgradedomäne installiert. (Höchstens alle Knoten in einer Upgradedomäne können Windows Update verwenden.)
|LogsDiskQuotaInMB   |Long  <br> (Standardwert: 1024)               |Maximale Größe der Protokolle für die App für die Patchorchestrierung in MB, die lokal auf jedem Knoten beibehalten werden können.
| WUQuery               | string<br>(Standardwert: "IsInstalled=0")                | Abfrage zum Abrufen von Windows-Updates. Weitere Informationen finden Sie unter [WuQuery](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx).
| InstallWindowsOSOnlyUpdates | Bool <br> (Standardwert: True)                 | Dieses Flag ermöglicht die Installation von Windows-Betriebssystemupdates.            |
| WUOperationTimeOutInMinutes | int <br>(Standardwert: 90)                   | Gibt den Timeoutwert für jeden Windows Update-Vorgang an (Suchen/Herunterladen/Installieren). Wenn der Vorgang nicht innerhalb des angegebenen Timeoutzeitraums abgeschlossen ist, wird er abgebrochen.       |
| WURescheduleCount     | int <br> (Standardwert: 5)                  | Gibt an, wie oft der Dienst das Windows Update maximal erneut plant, falls bei dem Vorgang wiederholt ein Fehler auftritt.          |
| WURescheduleTimeInMinutes | int <br>(Standardwert: 30) | Das Intervall, nach dem der Dienst das Windows-Update erneut plant, falls der Fehler weiterhin besteht. |
| WUFrequency           | Durch Trennzeichen getrennte Zeichenfolge (Standard: „Wöchentlich, Mittwoch, 7:00:00“)     | Die Häufigkeit, mit der Windows-Updates installiert werden sollen. Folgende Formate und Werte sind möglich: <br>– Monatlich, TT,HH:MM:SS, z.B. Monatlich, 5,12:22:32. <br> – Wöchentlich, TAG,HH:MM:SS, z.B. Wöchentlich, Dienstag, 12:22:32.  <br> – Täglich, HH:MM:SS, z.B. Täglich, 12:22:32.  <br> – Keine: Gibt an, dass keine Windows-Updates durchgeführt werden sollen.  <br><br> Alle Uhrzeiten sind in UTC angegeben.|
| AcceptWindowsUpdateEula | Bool <br>(Standardwert: true) | Wenn Sie dieses Flag festlegen, akzeptiert die Anwendung den Endbenutzer-Lizenzvertrag für Windows Update für den Besitzer des Computers.              |

> [!TIP]
> Wenn Windows Update sofort ausgeführt werden soll, legen Sie `WUFrequency` relativ zum Zeitpunkt der Anwendungsbereitstellung fest. Angenommen, Sie haben einen Testcluster mit fünf Knoten und möchten die App ca. um 17:00 Uhr (UTC) bereitstellen. Wenn Sie davon ausgehen, dass das Upgrade oder die Bereitstellung der Anwendung maximal 30 Minuten dauert, legen Sie WUFrequency auf „Täglich, 17:30:00“ fest.

## <a name="deploy-the-app"></a>Bereitstellen der App

1. Führen Sie alle erforderlichen Schritte zur Vorbereitung des Clusters aus.
2. Stellen Sie die App für die Patchorchestrierung wie jede andere Service Fabric-App bereit. Sie können die App mit PowerShell bereitstellen. Führen Sie die Schritte unter [Bereitstellen und Entfernen von Anwendungen mit PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) aus.
3. Übergeben Sie zum Konfigurieren der Anwendung zum Zeitpunkt der Bereitstellung `ApplicationParamater` an das `New-ServiceFabricApplication`-Cmdlet. Zur Vereinfachung wurde das Skript „Deploy.ps1“ zusammen mit der Anwendung bereitgestellt. So verwenden Sie das Skript

    - Stellen Sie mit `Connect-ServiceFabricCluster` eine Verbindung mit einem Service Fabric-Cluster her.
    - Führen Sie das PowerShell-Skript „Deploy.ps1“ mit einem geeigneten `ApplicationParameter`-Wert aus.

> [!NOTE]
> Speichern Sie das Skript und den Anwendungsordner „PatchOrchestrationApplication“ im gleichen Verzeichnis.

## <a name="upgrade-the-app"></a>Aktualisieren der App

Um eine vorhandene App für die Patchorchestrierung mithilfe von PowerShell zu aktualisieren, führen Sie die Schritte unter [Service Fabric-Anwendungsupgrade mithilfe von PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell) aus.

## <a name="remove-the-app"></a>Entfernen der App

Führen Sie die Schritte unter [Bereitstellen und Entfernen von Anwendungen mit PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) aus, um die Anwendung zu entfernen.

Zur Vereinfachung wurde das Skript „Undeploy.ps1“ zusammen mit der Anwendung bereitgestellt. So verwenden Sie das Skript

  - Stellen Sie mit ```Connect-ServiceFabricCluster``` eine Verbindung mit einem Service Fabric-Cluster her.

  - Führen Sie das PowerShell-Skript „Undeploy.ps1“ aus.

> [!NOTE]
> Speichern Sie das Skript und den Anwendungsordner „PatchOrchestrationApplication“ im gleichen Verzeichnis.

## <a name="view-the-windows-update-results"></a>Anzeigen der Ergebnisse von Windows Update

Die App für die Patchorchestrierung macht eine REST-API verfügbar, um dem Benutzer die Verlaufsergebnisse anzuzeigen. Beispiel für die JSON-Ausgabe für das Ergebnis:
```json
[
  {
    "NodeName": "_stg1vm_1",
    "WindowsUpdateOperationResults": [
      {
        "OperationResult": 0,
        "NodeName": "_stg1vm_1",
        "OperationTime": "2017-05-21T11:46:52.1953713Z",
        "UpdateDetails": [
          {
            "UpdateId": "7392acaf-6a85-427c-8a8d-058c25beb0d6",
            "Title": "Cumulative Security Update for Internet Explorer 11 for Windows Server 2012 R2 (KB3185319)",
            "Description": "A security issue has been identified in a Microsoft software product that could affect your system. You can help protect your system by installing this update from Microsoft. For a complete listing of the issues that are included in this update, see the associated Microsoft Knowledge Base article. After you install this update, you may have to restart your system.",
            "ResultCode": 0
          }
        ],
        "OperationType": 1,
        "WindowsUpdateQuery": "IsInstalled=0",
        "WindowsUpdateFrequency": "Daily,10:00:00",
        "RebootRequired": false
      }
    ]
  },
  ...
]
```
Wenn noch keine Aktualisierung geplant ist, ist die JSON-Ausgabe leer.

Melden Sie sich beim Cluster an, um Windows Update-Ergebnisse abzufragen. Ermitteln Sie dann die Replikatadresse des primären Knotens für den Koordinatordienst, und öffnen Sie diese URL im Browser: http://&lt;REPLIKAT-IP&gt;:&lt;Anwendungsport&gt;/PatchOrchestrationApplication/v1/GetWindowsUpdateResults.

Der REST-Endpunkt für den Koordinatordienst verfügt über einen dynamischen Port. Die genaue URL finden Sie im Service Fabric Explorer. Die Ergebnisse sind beispielsweise unter `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults` verfügbar.

![Abbildung des REST-Endpunkts](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)


Wenn der Reverseproxy auf dem Cluster aktiviert ist, können Benutzer auch von außerhalb des Clusters auf die URL zugreifen.
Aufzurufender Endpunkt: http://&lt;SERVER-URL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.

Führen Sie zum Aktivieren des Reverseproxys auf dem Cluster die Schritte unter [Reverseproxy in Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) aus. 

> 
> [!WARNING]
> Sobald der Reverseproxy konfiguriert wurde, können alle Microservices im Cluster, die einen HTTP-Endpunkt verfügbar machen, außerhalb des Clusters aufgerufen werden.

## <a name="diagnosticshealth-events"></a>Diagnose/Integritätsereignisse

### <a name="collect-patch-orchestration-app-logs"></a>Erfassen von Protokollen für die App für die Patchorchestrierung

Protokolle für die App für die Patchorchestrierung werden als Teil der Service Fabric-Protokolle aus Laufzeitversion `5.6.220.9494` und höher erfasst.
Für Cluster, auf denen eine Service Fabric-Laufzeitversion vor `5.6.220.9494` ausgeführt wird, können Protokolle mithilfe eines der folgenden Verfahren erfasst werden.

#### <a name="locally-on-each-node"></a>Lokal auf jedem Knoten

Protokolle werden lokal auf jedem Service Fabric-Clusterknoten gesammelt. Der Speicherort für den Zugriff auf die Protokolle lautet: \[Installationslaufwerk\_für\_Service Fabric\]:\\PatchOrchestrationApplication\\logs.

Wenn Service Fabric z.B. auf dem Laufwerk „D“ installiert ist, lautet der Pfad folgendermaßen: D:\\PatchOrchestrationApplication\\logs

#### <a name="central-location"></a>Zentraler Speicherort

Wenn die Azure-Diagnose im Rahmen der vorbereitenden Schritte konfiguriert wurde, sind die Protokolle der App für die Patchorchestrierung in Azure Storage verfügbar.

### <a name="health-reports"></a>Integritätsberichte

Die App für die Patchorchestrierung veröffentlicht in folgenden Fällen auch Integritätsberichte für den Koordinatordienst oder den Knoten-Agent-Dienst:

#### <a name="a-windows-update-operation-failed"></a>Fehler bei einem Windows Update-Vorgang

Wenn beim Windows Update-Vorgang auf einem Knoten ein Fehler auftritt, wird ein Integritätsbericht für den Knoten-Agent-Dienst generiert. Der Integritätsbericht enthält den Namen des problematischen Knotens.

Der Bericht wird automatisch gelöscht, sobald das Patchen des problematischen Knotens erfolgreich abgeschlossen wurde.

#### <a name="the-node-agent-ntservice-is-down"></a>Ausfall des Knoten-Agent-NT-Diensts

Wenn der Knoten-Agent-NT-Dienst auf einem Knoten ausgefallen ist, wird für den Knoten-Agent-Dienst ein Integritätsbericht mit Warnstufe generiert.

#### <a name="the-repair-manager-service-is-not-enabled"></a>Nicht aktivierter Reparatur-Manager-Dienst

Wenn der Reparatur-Manager-Dienst im Cluster nicht gefunden werden kann, wird für den Koordinatordienst ein Integritätsbericht mit Warnstufe generiert.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

F: **Warum befindet sich mein Cluster im Status „Fehler“, wenn die App für die Patchorchestrierung ausgeführt wird?**

A: Während des Installationsprozesses deaktiviert die App für die Patchorchestrierung Knoten und/oder startet diese neu. Dies kann dazu führen, dass die Integrität des Clusters vorübergehend sinkt.

Basierend auf der Richtlinie für die Anwendung kann während eines Patchvorgangs ein Knoten *oder* eine ganze Upgradedomäne gleichzeitig heruntergefahren werden.

Nach Abschluss der Windows Update-Installation werden die Knoten neu gestartet und erneut aktiviert.

Im folgenden Beispiel ist der Cluster vorübergehend in einen Fehlerzustand gewechselt, da zwei Knoten ausgefallen waren und die Richtlinie „MaxPercentageUnhealthNodes“ verletzt wurde. Dies ist ein temporärer Fehler, der nur auftritt, solange der Patchvorgang ausgeführt wird.

![Abbildung eines Clusters mit Fehler](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Sollte das Problem dauerhaft auftreten, lesen Sie die Informationen im Abschnitt zur Problembehandlung.

F: **Die App für die Patchorchestrierung befindet sich im Status „Warnung“.**

A: Überprüfen Sie, ob der für die Anwendung gesendete Integritätsbericht Informationen zur Ursache enthält. Üblicherweise enthält die Warnung Details zum Problem. Wenn das Problem vorübergehend ist, ist zu erwarten, dass die Anwendung automatisch wiederhergestellt wird.

F: **Was kann ich tun, wenn mein Cluster einen Fehler aufweist und ich ein dringendes Betriebssystemupdate ausführen muss?**

A: Die App für die Patchorchestrierung installiert keine Updates, solange der Cluster sich in einem fehlerhaften Zustand befindet. Versuchen Sie, Ihren Cluster in einen fehlerfreien Zustand zu versetzen, um den Workflow der App für die Patchorchestrierung fortzuführen.

F: **Warum dauert das Patchen für den gesamten Cluster so lange?**

A: Die Ausführungsdauer der App für die Patchorchestrierung ist größtenteils von den folgenden Faktoren abhängig:

- Richtlinie des Koordinatordiensts. 
  - Die Standardrichtlinie `NodeWise` führt dazu, dass nur jeweils ein Knoten gepatcht wird. Besonders bei größeren Clustern sollten Sie die Richtlinie `UpgradeDomainWise` verwenden, um ein schnelleres Patchen bei mehreren Clustern zu erreichen.
- Anzahl der zum Herunterladen und Installieren verfügbaren Updates. 
- Durchschnittliche Zeit zum Herunterladen und Installieren eines Updates. Dies sollte nicht länger als einige Stunden dauern.
- Leistung des virtuellen Computers und Netzwerkbandbreite.

## <a name="disclaimers"></a>Haftungsausschlüsse

- Die App für die Patchorchestrierung akzeptiert den Endbenutzer-Lizenzvertrag von Windows Update im Namen des Benutzers. Diese Einstellung kann optional in der Konfiguration der Anwendung deaktiviert werden.

- Die App für die Patchorchestrierung sammelt Telemetriedaten zum Nachverfolgen von Nutzung und Leistung. Die Telemetrieeinstellung der Anwendung folgt der Telemetrieeinstellung der Service Fabric-Laufzeit (standardmäßig ist die Einstellung aktiviert).

## <a name="troubleshooting"></a>Problembehandlung

### <a name="a-node-is-not-coming-back-to-up-state"></a>Ein Knoten wird nicht wieder in den betriebsbereiten Zustand versetzt.

**Der Knoten kann sich aus folgenden Gründen im deaktivierten Zustand befinden:**

Eine Sicherheitsprüfung steht aus. Um in dieser Situation Abhilfe zu schaffen, stellen Sie sicher, dass genügend Knoten in fehlerfreiem Zustand verfügbar sind.

**Der deaktivierte Zustand des Knotens kann aus folgenden Gründen nicht aufgehoben werden**:

- Der Knoten wurde manuell deaktiviert.
- Der Knoten wurde aufgrund eines laufenden Azure-Infrastrukturauftrags deaktiviert.
- Der Knoten wurde von der App für die Patchorchestrierung vorübergehend deaktiviert, um Patches auf den Knoten aufzuspielen.

**Der Ausfall des Knoten kann aus folgenden Gründen nicht aufgehoben werden**:

- Der Knoten wurde manuell in den Zustand „Ausgefallen“ versetzt.
- Der Knoten wird gerade neu gestartet (dies kann auch durch die App für die Patchorchestrierung ausgelöst worden sein).
- Der Knoten ist aufgrund eines fehlerhaften virtuellen Computers oder aufgrund von Netzwerkverbindungsproblemen ausgefallen.

### <a name="updates-were-skipped-on-some-nodes"></a>Updates wurden auf einigen Knoten übersprungen

Die App für die Patchorchestrierung versucht, Windows-Updates gemäß der Neuplanungsrichtlinie zu installieren. Der Dienst versucht gemäß der Anwendungsrichtlinie, den Knoten wiederherzustellen und das Update zu überspringen.

In einem solchen Fall wird ein Integritätsbericht mit Warnstufe für den Knoten-Agent-Dienst generiert. Das Ergebnis der Windows Update-Funktion enthält ebenfalls mögliche Fehlerursachen.

### <a name="the-health-of-the-cluster-goes-to-error-while-the-update-installs"></a>Während der Installation eines Updates wird der Cluster in einen Fehlerzustand versetzt.

Ein fehlerhaftes Windows Update kann die Integrität einer Anwendung oder eines Clusters auf einem bestimmten Knoten oder in einer Upgradedomäne verringern. Die App für die Patchorchestrierung reagiert nicht mehr auf folgende Windows Update-Vorgänge, bis der Cluster wieder fehlerfrei ist.

Ein Administrator muss eingreifen und ermitteln, weshalb die Integrität der Anwendung oder des Clusters aufgrund von Windows Update beeinträchtigt wurde.

