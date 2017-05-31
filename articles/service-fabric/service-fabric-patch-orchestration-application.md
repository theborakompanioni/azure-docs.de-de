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
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: a3554881e7db894c602e73feb385b5b361837409
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---

# <a name="application-to-patch-windows-os-in-your-service-fabric-cluster"></a>Anwendung zum Aufspielen von Windows-Betriebssystempatches in Ihrem Service Fabric-Cluster

Die Anwendung für die Patchorchestrierung ist eine Service Fabric-Anwendung, mit der Sie das Aufspielen von Betriebssystempatches in einem Service Fabric-Cluster in Azure oder lokal ohne Ausfallzeiten automatisieren können.

Die Anwendung für die Patchorchestrierung umfasst die folgenden hervorragenden Funktionen:

1. **Automatische Installation von Betriebssystemupdates**: Die Anwendung für die Patchorchestrierung stellt sicher, dass Updates automatisch heruntergeladen und installiert werden. Die Knoten werden dabei gegebenenfalls neu gestartet.
    Dies erfolgt ohne Ausfallzeiten auf allen Clusterknoten.

1. **Clusterfähiges Patchen und Überwachen der Integrität**: Die Anwendung für die Patchorchestrierung überwacht während des Anwendens der Updates die Integrität des Clusters. Es wird jeweils nur ein Knoten bzw. eine Upgradedomäne gleichzeitig aktualisiert. 
    Wenn festgestellt wird, dass die Integrität des Clusters aufgrund des Patchprozesses sinkt, wird der Prozess angehalten, um zu verhindern, dass sich das Problem verschlimmert.

1. **Unterstützung für alle Service Fabric-Cluster**: Die Anwendung ist so generisch, dass sie sowohl in Azure Service Fabric-Clustern als auch in eigenständigen Clustern funktioniert.
    > [!NOTE]
    > Unterstützung für eigenständige Cluster wird in Kürze verfügbar sein.

## <a name="link-to-download-the-application-package"></a>Link zum Herunterladen des Anwendungspakets

Laden Sie die Anwendung über den [Downloadlink](https://go.microsoft.com/fwlink/P/?linkid=849590) herunter.

## <a name="internal-details-of-the-application"></a>Interne Details der Anwendung

Die Anwendung für die Patchorchestrierung besteht aus den folgenden Unterkomponenten:

- **Koordinatordienst**: ein zustandsbehafteter Dienst. Der Dienst ist für Folgendes zuständig:
    - Koordinieren der Windows Update-Aufträge im gesamten Cluster.
    - Speichern der Ergebnisse der abgeschlossenen Windows Update-Vorgänge.
- **Knoten-Agent-Dienst**: ein zustandsloser Dienst, der auf allen Service Fabric-Clusterknoten ausgeführt wird. Der Dienst ist für Folgendes zuständig:
    - Bootstrapping des Knoten-Agent-NT-Diensts.
    - Überwachen des Knoten-Agent-NT-Diensts.
- **Knoten-Agent-NT-Dienst**: ein Windows NT-Dienst. Der Knoten-Agent-NT-Dienst wird mit höheren Berechtigungen ausgeführt (SYSTEM). Im Gegensatz dazu werden der Knoten-Agent-Dienst und der Koordinatordienst mit niedrigeren Berechtigungen ausgeführt (NETZWERKDIENST). Der Dienst ist für die Ausführung der folgenden Windows Update-Aufträge auf allen Clusterknoten zuständig.
    - Deaktivieren der automatischen Windows Update-Funktion auf dem Knoten.
    - Herunterladen und Installieren von Windows-Updates gemäß der vom Benutzer bereitgestellten Richtlinie.
    - Neustarten des Computers nach Installation der Windows-Updates.
    - Hochladen des Ergebnisses der Windows Update-Funktion in den Koordinatordienst.
    - Senden eines Integritätsberichts, falls der Vorgang nach allen Wiederholungsversuchen fehlschlägt.

> [!NOTE]
> Die Anwendung für die Patchorchestrierung verwendet den **Reparatur-Manager** des Service Fabric-Systemdiensts, um die Knoten zu aktivieren und zu deaktivieren und um Integritätsprüfungen durchzuführen. Der von der Anwendung für die Patchorchestrierung erstellte Reparaturtask verfolgt den Windows Update-Fortschritt für jeden Knoten nach.

## <a name="prerequisites-for-using-the-application"></a>Voraussetzungen für das Verwenden der Anwendung

### <a name="ensure-service-fabric-version-is-55-or-above"></a>Stellen Sie sicher, dass Service Fabric in Version 5.5 oder höher vorliegt.

Die Anwendung für die Patchorchestrierung kann in Clustern mit der Service Fabric Runtime-Version v5.5 oder höher ausgeführt werden.

### <a name="enable-repair-manager-service-if-not-running-already"></a>Aktivieren Sie den Reparatur-Manager-Dienst (falls dieser nicht bereits ausgeführt wird).

Für die Anwendung für die Patchorchestrierung muss der Reparatur-Manager-Systemdienst im Cluster aktiviert sein.

#### <a name="service-fabric-clusters-on-azure"></a>Service Fabric-Cluster in Azure

In Azure-Clustern auf Dauerhaftigkeitsstufe „Silver“ ist der Reparatur-Manager-Dienst in der Regel standardmäßig aktiviert. In Azure-Clustern auf Dauerhaftigkeitsstufe „Gold“ kann der Reparatur-Manager-Dienst aktiviert sein, je nachdem, wann diese Cluster erstellt wurden. In Azure-Clustern auf Dauerhaftigkeitsstufe „Bronze“ ist der Reparatur-Manager-Dienst standardmäßig nicht aktiviert. 

Falls der Reparatur-Manager-Dienst nicht bereits im Abschnitt „Systemdienste“ des Service Fabric Explorers ausgeführt wird, können Sie die [Azure Resource Manager-Vorlage](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) verwenden, um den Dienst in neuen oder vorhandenen Service Fabric-Clustern zu aktivieren.

Rufen Sie zunächst die Vorlage für den Cluster ab, den Sie bereitstellen möchten. Sie können entweder die Beispielvorlagen verwenden oder eine benutzerdefinierte Resource Manager-Vorlage erstellen. Anschließend können Sie den Reparatur-Manager mit folgenden Schritten aktivieren:

1. Überprüfen Sie zunächst, ob `apiversion` für die `Microsoft.ServiceFabric/clusters`-Ressource auf `2017-07-01-preview` festgelegt ist, wie im folgenden Codeausschnitt gezeigt. Liegt eine andere Einstellung vor, müssen Sie `apiVersion` auf den Wert `2017-07-01-preview` aktualisieren.

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Jetzt aktivieren Sie den Reparatur-Manager-Dienst, indem Sie folgenden `addonFeaturres`-Abschnitt nach dem Abschnitt `fabricSettings` hinzufügen, wie unten gezeigt.

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. Nachdem Sie die Clustervorlage mit diesen Änderungen aktualisiert haben, wenden Sie die Änderungen an, und lassen Sie das Upgrade fortfahren. Sobald der Vorgang abgeschlossen ist, wird der in Ihrem Cluster ausgeführte Reparatur-Manager-Systemdienst mit dem Namen `fabric:/System/RepairManagerService` im Service Fabric Explorer im Abschnitt „Systemdienste“ angezeigt. 

#### <a name="standalone-on-premise-clusters"></a>Eigenständige lokale Cluster

> [!NOTE]
> Unterstützung für eigenständige Cluster wird in Kürze verfügbar sein.

### <a name="disable-automatic-windows-update-on-all-nodes"></a>Deaktivieren Sie die automatische Windows Update-Funktion auf allen Knoten.

Die Aktivierung der automatischen Windows Update-Funktion in einem Service Fabric-Cluster kann zu Verfügbarkeitsverlusten führen, da möglicherweise mehrere Knoten gleichzeitig neu gestartet werden, um ein Update abzuschließen.

Die Anwendung für die Patchorchestrierung versucht standardmäßig, die automatische Windows Update-Funktion auf jedem Clusterknoten zu deaktivieren.

Wenn jedoch die Einstellungen von einem Administrator bzw. durch eine Gruppenrichtlinie verwaltet werden, empfiehlt es sich, die Windows Update-Richtlinie explizit auf „Vor dem Download benachrichtigen“ festzulegen.


### <a name="optional-enable-windows-azure-diagnostics"></a>Optional: Aktivieren der Microsoft Azure-Diagnose

In naher Zukunft werden Protokolle der Anwendung für die Patchorchestrierung als Teil der Service Fabric-Protokolle selbst gesammelt. Bis dahin werden die Protokolle jedoch lokal auf jedem einzelnen Clusterknoten gesammelt. Es empfiehlt sich, die Microsoft Azure-Diagnose so zu konfigurieren, dass die Protokolle von allen Knoten in einen zentralen Speicherort hochgeladen werden.

Die einzelnen Schritte zum Aktivieren der Microsoft Azure-Diagnose finden Sie [hier.](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-how-to-setup-wad)

Die Protokolle der Anwendung für die Patchorchestrierung werden auf folgenden festen Anbieter-IDs generiert.

- e39b723c-590c-4090-abb0-11e3e6616346
- fc0028ff-bfdc-499f-80dc-ed922c52c5e9
- 24afa313-0d3b-4c7c-b485-1047fd964b60
- 05dc046c-60e9-4ef7-965e-91660adffa68

Fügen Sie in der Azure Resource Manager-Vorlage im Abschnitt „WadCfg“ folgenden Abschnitt ein: 

```json
"PatchOrchestrationApplication": \[
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
  },
\]
```

> [!NOTE]
> Falls Ihr Service Fabric-Cluster aus mehreren Knotentypen besteht, muss der obige Abschnitt in alle WadCfg-Abschnitte eingefügt werden.

## <a name="configuring-the-application"></a>Konfigurieren der Anwendung

Folgende Konfigurationen können vom Benutzer festgelegt werden, um das Verhalten der Anwendung für die Patchorchestrierung an ihre Anforderungen anzupassen.

Ein Benutzer kann die Standardwerte außer Kraft setzen, indem er während der Erstellung bzw. Aktualisierung einer Anwendung den Anwendungsparameter übergibt. Anwendungsparameter können durch Angeben von `ApplicationParameter` in den Cmdlets `Start-ServiceFabricApplicationUpgrade` oder `New-ServiceFabricApplication` festgelegt werden.

|**Parameter**        |**Typ**                          | **Details**|
|:-|-|-|
|MaxResultsToCache    |Long                              | Maximale Anzahl von Windows Update-Ergebnissen im Verlauf, die zwischengespeichert werden sollen. <br>Der Standardwert ist 3000, wobei Folgendes angenommen wird: <br> – Es sind 20 Knoten vorhanden. <br> – Jeden Monat können 5 Updates auf einem Knoten erfolgen. <br> – Pro Vorgang können 10 Ergebnisse vorliegen. <br> – Es sollen die Ergebnisse der letzten 3 Monate gespeichert werden. |
|TaskApprovalPolicy   |Enum <br> { NodeWise, UpgradeDomainWise }                          |TaskApprovalPolicy gibt die Richtlinie an, die vom Koordinatordienst beim Installieren von Windows-Updates auf den Service Fabric-Clusterknoten verwendet werden soll.<br>                         Zulässige Werte sind: <br><br>                                                           <b>NodeWise</b>:-Windows-Updates werden immer nur auf einem Knoten gleichzeitig installiert. <br>                                                           <b>UpgradeDomainWise</b>:-Windows-Updates werden nur in einer Upgradedomäne gleichzeitig installiert (Windows-Updates können nur auf den Knoten installiert, die zu einer Upgradedomäne gehören).
|LogsDiskQuotaInMB   |Long  <br> (Standardwert: 1024)               |Maximale Größe der Protokolle der Anwendung für die Patchorchestrierung in MB, die lokal auf jedem Knoten beibehalten werden können.
| WUQuery               | string<br>(Standardwert: "IsInstalled=0")                | Abfrage zum Abrufen von Windows-Updates. Weitere Informationen finden Sie unter [WuQuery](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx).
| InstallWindowsOSOnlyUpdates | Bool <br> (Standardwert: True)                 | Dieses Flag ermöglicht die Installation von Windows-Betriebssystemupdates.            |
| WUOperationTimeOutInMinutes | int <br>(Standardwert: 90)                   | Gibt den Timeoutwert für jeden Windows Update-Vorgang an (Suchen/Herunterladen/Installieren). Wenn der Vorgang nicht innerhalb des angegebene Timeoutzeitraums abgeschlossen ist, wird er abgebrochen.       |
| WURescheduleCount     | int <br> (Standardwert: 5)                  | Diese Konfiguration legt fest, wie oft der Dienst das Windows Update erneut plant, falls bei dem Vorgang wiederholt ein Fehler auftritt.          |
| WURescheduleTimeInMinutes | int <br>(Standardwert: 30) | Diese Konfiguration legt das Intervall fest, nach dem der Dienst das Windows Update erneut plant, falls der Fehler weiterhin besteht. |
| WUFrequency           | Durch Trennzeichen getrennte Zeichenfolge (Standard: „Wöchentlich, Mittwoch, 7:00:00“)     | Die Häufigkeit, mit der Windows-Updates installiert werden sollen. Folgende Formate und Werte sind möglich: <br>– Monatlich,TT,HH:MM:SS – Beispiel: Monatlich,5,12:22:32 <br> – Wöchentlich,TAG,HH:MM:SS – Beispiel: Wöchentlich, Dienstag, 12:22:32  <br> – Täglich, HH:MM:SS – Beispiel: Täglich, 12:22:32  <br> – Keine: Gibt an, dass keine Windows-Updates durchgeführt werden sollen  <br><br> HINWEIS: Alle Uhrzeiten sind UTC.|
| AcceptWindowsUpdateEula | Bool <br>(Standardwert: true) | Wenn Sie dieses Flag festlegen, akzeptiert die Anwendung den Endbenutzer-Lizenzvertrag von Windows Update im Namen des Besitzers des Computers.              |


## <a name="steps-to-deploy-the-application"></a>Schritte zur Bereitstellung der Anwendung

1. Beenden Sie alle erforderlichen Schritte zur Vorbereitung des Clusters.
1. Stellen Sie die Anwendung bereit. Hierzu können Sie die [hier](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) beschriebenen Schritte in PowerShell ausführen.
1. Um die Anwendung zum Zeitpunkt der Bereitstellung zu konfigurieren, verwenden Sie `ApplicationParamater` im Cmdlet `New-ServiceFabricApplication`.

    Zum Vereinfachen des Prozesses stellen wir das Skript „Deploy.ps1“ zusammen mit der Anwendung bereit. Gehen Sie zur Verwendung wie folgt vor:

    - Stellen Sie über `Connect-ServiceFabricCluster` eine Verbindung mit dem Service Fabric-Cluster her.
    - Führen Sie das PowerShell-Skript „Deploy.ps1“ mit einem geeigneten `ApplicationParameter`-Wert aus.

> [!NOTE]
> Speichern Sie das Skript und den Anwendungsordner PatchOrchestrationApplication im gleichen Verzeichnis.

## <a name="steps-to-upgrade-the-application"></a>Schritte zum Upgrade der Anwendung

Um mithilfe von PowerShell ein Upgrade für eine vorhandene Anwendung für die Patchorchestrierung durchzuführen, lesen Sie die [hier](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell) beschriebenen Schritte zum Durchführen von Anwendungsupgrades.

Wenn Sie nur die Anwendungsparameter der Anwendung ändern möchten, geben Sie `ApplicationParamater` im Cmdlet `Start-ServiceFabricApplicationUpgrade` mit der vorhandenen Anwendungsversion an.

## <a name="steps-to-remove-the-application"></a>Schritte zum Entfernen der Anwendung

Um die Anwendung zu entfernen, lesen Sie die [hier](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications) beschriebenen Schritte.

Zum Vereinfachen des Prozesses stellen wir das Skript „Undeploy.ps1“ zusammen mit der Anwendung bereit. Gehen Sie zur Verwendung wie folgt vor:
    - Stellen Sie über ```Connect-ServiceFabricCluster``` eine Verbindung mit dem Service Fabric-Cluster her.
    - Führen Sie das PowerShell-Skript „Undeploy.ps1“ aus.

> [!NOTE]
> Speichern Sie das Skript und den Anwendungsordner PatchOrchestrationApplication im gleichen Verzeichnis.

## <a name="viewing-the-windows-update-results"></a>Anzeigen der Ergebnisse von Windows Update

Die Anwendung für die Patchorchestrierung macht eine REST-API verfügbar, um dem Benutzer die Verlaufsergebnisse anzuzeigen.

Sie können Windows Update-Ergebnisse abfragen, indem Sie sich beim Cluster anmelden, die Replikatadresse des primären Knotens für den Koordinatordienst suchen und diese URL im Browser öffnen.
http://&lt;REPLICA-IP&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1/GetWindowsUpdateResults: Der REST-Endpunkt für den Koordinatordienst verfügt über einen dynamischen Port. Die genaue URL finden Sie im Service Fabric Explorer.
Beispiel: Für das unten stehende Beispiel wären die Ergebnisse hier verfügbar: `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`
![Abbildung des REST-Endpunkts](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)


Benutzer können auch von außerhalb des Clusters auf die URL zugreifen, falls im Cluster ein Reverseproxy aktiviert ist.
Aufzurufender Endpunkt: http://&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults. Der Reverseproxy kann mithilfe der [hier](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) beschriebenen Schritte im Cluster aktiviert werden. 

> 
> [!WARNING]
> Sobald der Reverseproxy konfiguriert wurde, können alle Microservices im Cluster, die einen HTTP-Endpunkt verfügbar machen, von außerhalb des Clusters aufgerufen werden.

## <a name="diagnostics--health-events"></a>Diagnose-/Integritätsereignisse

### <a name="collecting-patch-orchestration-application-logs"></a>Sammeln von Protokollen der Anwendung für die Patchorchestrierung

In naher Zukunft werden Protokolle der Anwendung für die Patchorchestrierung als Teil der Service Fabric-Protokolle gesammelt.
Bis dahin können die Protokolle mithilfe einer der folgenden Methoden gesammelt werden.

#### <a name="locally-on-each-node"></a>Lokal auf jedem Knoten

Protokolle werden lokal auf jedem Service Fabric-Clusterknoten gesammelt. Der Speicherort für den Zugriff auf die Protokolle lautet: \[Installationslaufwerk\_für\_Service Fabric\]:\\PatchOrchestrationApplication\\logs.

Wenn Service Fabric z.B. auf dem Laufwerk „D“ installiert ist, lautet der Pfad folgendermaßen: D:\\PatchOrchestrationApplication\\logs

#### <a name="central-location"></a>Zentraler Speicherort

Wenn die Microsoft Azure-Diagnose im Rahmen der vorbereitenden Schritte konfiguriert wurde, sind die Protokolle der Anwendung für die Patchorchestrierung in Azure Storage verfügbar.

### <a name="health-reports"></a>Integritätsberichte

Die Anwendung für die Patchorchestrierung veröffentlicht in folgenden Fällen auch Integritätsberichte für den Koordinatordienst oder den Knoten-Agent-Dienst.

#### <a name="windows-update-operation-failed"></a>Fehler beim Windows Update-Vorgang

Wenn beim Windows Update-Vorgang auf einem Knoten ein Fehler auftritt, wird ein Integritätsbericht für den Knoten-Agent-Dienst generiert.
Der Integritätsbericht enthält den Namen des problematischen Knotens.

Der Bericht wird automatisch gelöscht, sobald das Patchen des problematischen Knotens erfolgreich abgeschlossen wurde.

#### <a name="node-agent-ntservice-is-down"></a>Ausfall des Knoten-Agent-NT-Diensts

Wenn der Knoten-Agent-NT-Dienst auf einem Knoten ausgefallen ist, wird für den Knoten-Agent-Dienst ein Integritätsbericht mit Warnstufe generiert.

#### <a name="repair-manager-is-not-enabled"></a>Nicht aktivierter Reparatur-Manager

Wenn der Reparatur-Manager-Dienst im Cluster nicht gefunden werden kann, wird für den Koordinatordienst ein Integritätsbericht mit Warnstufe generiert.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen:

F: **Ich sehe, dass mein Cluster sich im Status „Fehler“ befindet, wenn die Anwendung für die Patchorchestrierung ausgeführt wird**.

A: Während des Installationsprozesses deaktiviert die Anwendung für die Patchorchestrierung Knoten und/oder startet diese neu. Dies kann dazu führen, dass die Integrität des Clusters vorübergehend sinkt.

Basierend auf der Richtlinie für die Anwendung kann während eines Patchvorgangs ein Knoten ODER eine ganze Upgradedomäne gleichzeitig heruntergefahren werden.

Beachten Sie, dass nach Abschluss der Windows Update-Installation die Knoten neu gestartet und erneut aktiviert werden.

Beispiel: Im unten stehenden Fall wurde der Cluster vorübergehend in einen Fehlerstatus versetzt, da zwei Knoten ausgefallen waren und die MaxPercentageUnhealthNodes-Richtlinie verletzt wurde. Dies ist ein temporärer Fehler, der nur auftritt, solange der Patchvorgang ausgeführt wird.

![Abbildung eines Clusters mit Fehler](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

Sollte das Problem dauerhaft auftreten, lesen Sie die Informationen im Abschnitt zur Problembehandlung.

F: **Kann ich die Anwendung für die Patchorchestrierung für eigenständige Cluster verwenden?**

A: Noch nicht. Unterstützung für eigenständige Cluster wird in Kürze verfügbar sein.

F: **Die Anwendung für die Patchorchestrierung befindet sich im Status „Warnung“.**

A: Überprüfen Sie, ob der für die Anwendung gesendete Integritätsbericht Informationen zur Ursache enthält. Üblicherweise enthält die Warnung Details zum Problem.
Wenn das Problem vorübergehend ist, ist zu erwarten, dass die Anwendung automatisch wiederhergestellt wird.

F: **Mein Cluster ist fehlerhaft. Ich muss jedoch dringend ein Betriebssystemupdate ausführen.**

A: Die Anwendung für die Patchorchestrierung installiert keine Updates, solange der Cluster sich in einem fehlerhaften Zustand befindet.
Versuchen Sie, Ihren Cluster in einen fehlerfreien Zustand zu versetzen, um den Workflow der Anwendung für die Patchorchestrierung fortzuführen.

F: **Warum dauert das Patchen für den gesamten Cluster so lange?**

A: Die Ausführungsdauer der Anwendung für die Patchorchestrierung ist größtenteils von den folgenden Faktoren abhängig:

- Richtlinie des Koordinatordiensts: Die Standardrichtlinie `NodeWise` führt dazu, dass immer nur ein Knoten gleichzeitig gepatcht wird. Insbesondere bei größeren Clustern empfiehlt es sich, die Richtlinie `UpgradeDomainWise` zu verwenden, um den Patchprozess für den gesamten Cluster zu beschleunigen.
- Anzahl der zum Herunterladen und Installieren verfügbaren Updates: Die durchschnittlich benötigte Zeit für das Herunterladen und Installieren eines Updates sollte einige Stunden nicht überschreiten.
- Leistung des virtuellen Computers und Netzwerkbandbreite.

## <a name="disclaimers"></a>Haftungsausschlüsse

- Die Anwendung für die Patchorchestrierung akzeptiert den Endbenutzer-Lizenzvertrag von Windows Update im Namen des Benutzers. Diese Einstellung kann optional in der Konfiguration der Anwendung deaktiviert werden.

- Die Anwendung für die Patchorchestrierung sammelt Telemetriedaten zum Nachverfolgen von Nutzung und Leistung.
    Die Telemetrieeinstellung der Anwendung folgt der Telemetrieeinstellung der Service Fabric Runtime (standardmäßig ist die Einstellung aktiviert).

## <a name="troubleshooting-help"></a>Hilfe zur Problembehandlung

### <a name="node-not-coming-back-to-up-state"></a>Knoten wird nicht wieder in betriebsbereiten Zustand versetzt

**Ein Knoten kann im Zustand „Deaktiviert“ verbleiben** Dies kann passieren, wenn ein für die Ausführung geplanter Knoten aufgrund einer ausstehenden Sicherheitsüberprüfung nicht deaktiviert werden kann. Um in dieser Situation Abhilfe zu schaffen, stellen Sie sicher, dass genügend Knoten in fehlerfreiem Zustand verfügbar sind.

**Ein Knoten kann aus folgenden Gründen im Zustand „Deaktiviert“ verbleiben**

- Der Knoten wurde manuell deaktiviert.
- Der Knoten wurde aufgrund eines laufenden Azure-Infrastrukturauftrags deaktiviert.
- Der Knoten wurde von der Anwendung für die Patchorchestrierung vorübergehend deaktiviert, um Patches auf den Knoten aufzuspielen.

**Ein Knoten kann aus folgenden Gründen im Zustand „Ausgefallen“ verbleiben**

- Der Knoten wurde manuell in den Zustand „Ausgefallen“ versetzt.
- Der Knoten wird gerade neu gestartet (möglicherweise durch die Anwendung für die Patchorchestrierung ausgelöst).
- Der Knoten ist aufgrund eines fehlerhaften (virtuellen) Computers oder aufgrund von Netzwerkverbindungsproblemen ausgefallen.

### <a name="updates-were-skipped-on-some-nodes"></a>Updates wurden auf einigen Knoten übersprungen

Die Anwendung für die Patchorchestrierung versucht, Windows-Updates gemäß der Neuplanungsrichtlinie zu installieren. Der Dienst versucht gemäß Anwendungsrichtlinie, den Knoten wiederherzustellen und das Update zu überspringen.

In einem solchen Fall wird ein Integritätsbericht mit Warnstufe für den Knoten-Agent-Dienst generiert.
Das Ergebnis der Windows Update-Funktion enthält ebenfalls mögliche Fehlerursachen.

### <a name="health-of-the-cluster-goes-to-error-while-update-was-getting-installed"></a>Während der Installation eines Updates wird der Cluster in einen Fehlerzustand versetzt

Falls ein fehlerhaftes Windows-Update die Integrität einer Anwendung oder eines Clusters auf einem bestimmten Knoten oder in einer bestimmten Upgradedomäne beschädigt, fährt die Anwendung für die Patchorchestrierung erst dann mit weiteren Windows-Updates fort, wenn der Cluster wieder einen fehlerfreien Zustand aufweist.

Der Administrator muss eingreifen und sich darüber informieren, warum ein Windows-Update die Integrität der Anwendung bzw. des Clusters beschädigt.

