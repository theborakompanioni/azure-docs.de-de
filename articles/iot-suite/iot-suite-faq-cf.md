---
title: "Azure IoT Suite Connected Factory – häufig gestellte Fragen | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zur IoT Suite Connected Factory"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/15/2017
ms.author: corywink
ms.translationtype: HT
ms.sourcegitcommit: 646886ad82d47162a62835e343fcaa7dadfaa311
ms.openlocfilehash: 35cf824210a14410d7ea2aedddde0040309901f9
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="frequently-asked-questions-for-iot-suite-connected-factory-preconfigured-solution"></a>Häufig gestellte Fragen zur vorkonfigurierten IoT Suite Connected Factory-Lösung

Informationen finden Sie auch in den allgemeinen [häufig gestellten Fragen](iot-suite-faq.md) zur IoT Suite.

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solution"></a>Wo finde ich den Quellcode für die vorkonfigurierte Lösung?

Der Quellcode ist im folgenden GitHub-Repository gespeichert:

* [Connected factory preconfigured solution](https://github.com/Azure/azure-iot-connected-factory) (Vorkonfigurierte Lösung für eine verbundene Factory)

### <a name="what-is-opc-ua"></a>Was ist OPC UA?

Die OPC Unified Architecture (UA), die 2008 veröffentlicht wurde, ist ein plattformunabhängiger, dienstorientierter Interoperabilitätsstandard. Die OPC UA wird von verschiedenen Industriesystemen und -geräten wie z.B. Industrie-PCs, -PLCs und -sensoren verwendet. Die OPC UA integriert alle Funktionen der klassischen OPC-Spezifikationen in ein erweiterbares Framework mit integrierter Sicherheit. Der Standard wird von der OPC Foundation verwaltet. Die [OPC Foundation](http://opcfoundation.org/) ist eine gemeinnützige Organisation mit über 440 Mitgliedern. Das Ziel der Organisation ist die Verwendung der OPC-Spezifikationen, um eine sichere und zuverlässige Interoperabilität über mehrere Anbieter und Plattformen hinweg zu ermöglichen. Folgende Elemente werden dabei berücksichtigt:

* Infrastruktur
* Spezifikationen
* Technologie
* Prozesse

### <a name="why-did-microsoft-choose-opc-ua-for-the-connected-factory-preconfigured-solution"></a>Warum hat sich Microsoft für OPC UA als vorkonfigurierte Lösung für eine verbundene Factory entschieden?

Microsoft hat OPC UA ausgewählt, da es sich um einen offenen, nicht proprietären, plattformunabhängigen, von der Industrie anerkannten und bewährten Standard handelt. Er stellt eine Voraussetzung für Referenzarchitekturlösungen für die Industrie 4.0 (RAMI4.0) dar und gewährleistet die Interoperabilität zwischen einer Vielzahl von Herstellungsverfahren und -geräten. Microsoft sieht bei seinen Kunden einen Bedarf an der Erstellung von Industrie 4.0-Lösungen. Die Unterstützung für OPC UA verringert die Barrieren für Kunden, ihre Ziele zu erreichen, und bietet sofort Geschäftsvorteile.

### <a name="how-do-i-add-a-public-ip-address-to-the-simulation-vm"></a>Wie füge ich der Simulations-VM eine öffentliche IP-Adresse hinzu?

Zum Hinzufügen der IP-Adresse stehen Ihnen zwei Optionen zur Verfügung:

* Verwenden Sie das PowerShell-Skript `Simulation/Factory/Add-SimulationPublicIp.ps1` im [Repository](https://github.com/Azure/azure-iot-connected-factory). Übergeben Sie den Namen Ihrer Bereitstellung als Parameter. Für eine lokale Bereitstellung verwenden Sie `<your username>ConnFactoryLocal`. Das Skript gibt die IP-Adresse des virtuellen Computers aus.

* Suchen Sie im Azure-Portal die Ressourcengruppe Ihrer Bereitstellung. Außer bei einer lokalen Bereitstellung weist die Ressourcengruppe den Namen auf, den Sie als Lösungs- oder Bereitstellungsnamen angegeben haben. Für eine lokale Bereitstellung mithilfe des Buildskripts lautet der Name der Ressourcengruppe `<your username>ConnFactoryLocal`. Fügen Sie jetzt der Ressourcengruppe eine neue Ressource für die **öffentliche IP-Adresse** hinzu.

> [!NOTE]
> Stellen Sie in allen Fällen sicher, dass Sie die neuesten Patches installieren, indem Sie die Anweisungen auf der [Ubuntu-Website](https://wiki.ubuntu.com/Security/Upgrades) befolgen. Sorgen Sie dafür, dass die Installation aktuell ist, solange Ihr virtueller Computer über eine öffentliche IP-Adresse erreichbar ist.

### <a name="how-do-i-remove-the-public-ip-address-to-the-simulation-vm"></a>Wie entferne ich eine öffentliche IP-Adresse aus der Simulations-VM?

Zum Entfernen der IP-Adresse stehen Ihnen zwei Optionen zur Verfügung:

* Verwenden Sie das PowerShell-Skript Simulation/Factory/Remove-SimulationPublicIp.ps1 im [Repository](https://github.com/Azure/azure-iot-connected-factory). Übergeben Sie den Namen Ihrer Bereitstellung als Parameter. Für eine lokale Bereitstellung verwenden Sie `<your username>ConnFactoryLocal`. Das Skript gibt die IP-Adresse des virtuellen Computers aus.

* Suchen Sie im Azure-Portal die Ressourcengruppe Ihrer Bereitstellung. Außer bei einer lokalen Bereitstellung weist die Ressourcengruppe den Namen auf, den Sie als Lösungs- oder Bereitstellungsnamen angegeben haben. Für eine lokale Bereitstellung mithilfe des Buildskripts lautet der Name der Ressourcengruppe `<your username>ConnFactoryLocal`. Entfernen Sie jetzt die Ressource für die **öffentliche IP-Adresse** aus der Ressourcengruppe.

### <a name="how-do-i-sign-in-to-the-simulation-vm"></a>Wie melde ich mich bei der Simulations-VM an?

Die Anmeldung bei der Simulations-VM wird nur unterstützt, wenn Sie Ihre Lösung mithilfe des PowerShell-Skripts `build.ps1` im [Repository](https://github.com/Azure/azure-iot-connected-factory) bereitgestellt haben.

Wenn Sie die Lösung über www.azureiotsuite.com bereitgestellt haben, können Sie sich nicht bei der VM anmelden. Dies liegt daran, dass das Kennwort zufällig generiert wird und Sie es nicht zurücksetzen können.

1. Fügen Sie der VM eine öffentliche IP-Adresse hinzu. Informationen dazu finden Sie unter [Wie füge ich der Simulations-VM eine öffentliche IP-Adresse hinzu?](#how-do-i-remove-the-public-ip-address-to-the-simulation-vm)
1. Erstellen Sie unter Angabe der IP-Adresse der VM eine SSH-Sitzung mit Ihrer VM.
1. Der zu verwendende Benutzername lautet `docker`.
1. Das Kennwort richtet sich nach der Version, die Sie für die Bereitstellung verwendet haben:
    * Bei Lösungen, die vor dem 1. Juni 2017 mithilfe des Skripts „build.ps1“ erstellt wurden, lautet das Kennwort `Passw0rd`.
    * Bei Lösungen, die nach dem 1. Juni 2017 mithilfe des Skripts „build.ps1“ erstellt wurden, finden Sie das Kennwort in der Datei `<name of your deployment>.config.user`. Das Kennwort ist in der Einstellung **VmAdminPassword** gespeichert. Das Kennwort wird zur Bereitstellungszeit per Zufallsprinzip generiert, sofern Sie es nicht im `build.ps1`-Skriptparameter `-VmAdminPassword` angegeben haben.

### <a name="how-do-i-stop-and-start-all-docker-processes-in-the-simulation-vm"></a>Wie stoppe und starte ich alle Dockerprozesse in der Simulations-VM?

1. Melden Sie sich bei der Simulations-VM an. Informationen dazu finden Sie unter [Wie melde ich mich bei der Simulations-VM an?](#how-do-i-sign-in-to-the-simulation-vm)
1. Um zu prüfen, welche Container aktiv sind, führen Sie `docker ps` aus.
1. Um alle Simulationscontainer anzuhalten, führen Sie `./stopsimulation` aus.
1. Um alle Simulationscontainer zu starten, gehen Sie folgendermaßen vor:
    * Exporten Sie eine Shellvariable mit dem Namen **IOTHUB_CONNECTIONSTRING**. Verwenden Sie den Wert der **IotHubOwnerConnectionString**-Einstellung in der Datei `<name of your deployment>.config.user`. Beispiel:

        ```
        export IOTHUB_CONNECTIONSTRING="HostName={yourdeployment}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={your key}"
        ```

    * Führen Sie `./startsimulation`aus.

### <a name="how-do-i-update-the-simulation-in-the-vm"></a>Wie aktualisiere ich die Simulation in der VM?

Wenn Sie Änderungen an der Simulation vorgenommen haben, können Sie das PowerShell-Skript `build.ps1` im [Repository](https://github.com/Azure/azure-iot-connected-factory) mit dem Befehl `updatedimulation` verwenden. Dieses Skript erstellt alle Simulationskomponenten, hält die Simulation in der VM an, lädt die Komponenten hoch, installiert sie und startet sie.

### <a name="how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution"></a>Wie finde ich die Verbindungszeichenfolge des IoT-Hubs, den meine Lösung verwendet?

Wenn Sie Ihre Lösung mit dem `build.ps1`-Skript im [Repository](https://github.com/Azure/azure-iot-connected-factory) bereitgestellt haben, ist die Verbindungszeichenfolge der Wert von **IotHubOwnerConnectionString** in der Datei `<name of your deployment>.config.user`.

Sie können die Verbindungszeichenfolge auch im Azure-Portal finden. Ermitteln Sie in der IoT Hub-Ressource in der Ressourcengruppe Ihrer Bereitstellung die Einstellung für die Verbindungszeichenfolge.

### <a name="which-iot-hub-devices-does-the-connected-factory-simulation-use"></a>Welche IoT Hub-Geräte werden von der Simulation der verbundenen Factory verwendet?

Die Simulation registriert die folgenden Geräte selbst:

* proxy.beijing.corp.contoso
* proxy.capetown.corp.contoso
* proxy.mumbai.corp.contoso
* proxy.munich0.corp.contoso
* proxy.rio.corp.contoso
* proxy.seattle.corp.contoso
* publisher.beijing.corp.contoso
* publisher.capetown.corp.contoso
* publisher.mumbai.corp.contoso
* publisher.munich0.corp.contoso
* publisher.rio.corp.contoso
* publisher.seattle.corp.contoso

Mithilfe der Tools [Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) oder [IoT Hub Explorer](https://github.com/azure/iothub-explorer) können Sie überprüfen, welche Geräte beim von Ihrer Lösung verwendeten IoT Hub registriert sind. Um diese Tools zu verwenden, benötigen Sie die Verbindungszeichenfolge für den IoT Hub in Ihrer Bereitstellung.

### <a name="how-can-i-get-log-data-from-the-simulation-components"></a>Wie kann ich Protokolldaten aus dem Simulationskomponenten abrufen?

Alle Komponenten in der Simulation schreiben Daten in Protokolldateien. Diese Dateien befinden sich auf dem virtuellen Computer im Ordner `home/docker/Logs`. Um die Protokolle abzurufen, können Sie das PowerShell-Skript `Simulation/Factory/Get-SimulationLogs.ps1` im [Repository](https://github.com/Azure/azure-iot-connected-factory) verwenden.

Dieses Skript muss sich bei der VM anmelden. Möglicherweise müssen Sie Anmeldeinformationen für die Anmeldung angeben. Informationen zum Ermitteln der Anmeldeinformationen finden Sie unter [Wie melde ich mich bei der Simulations-VM an?](#how-do-i-sign-in-to-the-simulation-vm)

Das Skript fügt dem virtuellen Computer eine öffentliche IP-Adresse hinzu, wenn dieser noch keine besitzt, und entfernt diese wieder. Das Skript speichert alle Protokolldateien in ein Archiv und lädt das Archiv auf Ihre Entwicklungsarbeitsstation herunter.

Alternativ dazu können Sie sich über SSH beim virtuellen Computer anmelden und die Protokolldateien zur Laufzeit untersuchen.

### <a name="how-can-i-check-if-the-simulation-is-sending-data-to-the-cloud"></a>Wie kann ich überprüfen, ob die Simulation Daten an die Cloud sendet?

Mit dem Tools [Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) oder [IoT Hub Explorer](https://github.com/azure/iothub-explorer) können Sie die Daten überprüfen, die von bestimmten Geräten an IoT Hub gesendet werden. Um diese Tools zu verwenden, müssen Sie die Verbindungszeichenfolge für den IoT Hub in Ihrer Bereitstellung kennen. Informationen finden Sie unter [Wie finde ich die Verbindungszeichenfolge des IoT-Hubs, den meine Lösung verwendet?](#how-do-i-find-out-the-connection-string-of-the-iot-hub-used-by-my-solution)

Untersuchen Sie die Daten, die von einem der Publisher-Geräte gesendet werden:

* publisher.beijing.corp.contoso
* publisher.capetown.corp.contoso
* publisher.mumbai.corp.contoso
* publisher.munich0.corp.contoso
* publisher.rio.corp.contoso
* publisher.seattle.corp.contoso

Wenn Sie feststellen, dass keine Daten an IoT Hub gesendet werden, liegt ein Problem mit der Simulation vor. Als erstes sollten Sie die Protokolldateien der Simulationskomponenten analysieren. Informationen dazu finden Sie unter [Wie kann ich Protokolldaten aus dem Simulationskomponenten abrufen?](#how-can-i-get-log-data-from-the-simulation-components) Versuchen Sie danach, die Simulation anzuhalten und neu zu starten. Wenn weiterhin keine Daten gesendet werden, aktualisieren Sie die Simulation insgesamt. Informationen dazu finden Sie unter [Wie aktualisiere ich die Simulation in der VM?](#how-do-i-update-the-simulation-in-the-vm)

### <a name="next-steps"></a>Nächste Schritte

Sie können auch einige andere Features und Funktionen der vorkonfigurierten IoT Suite-Lösungen ausprobieren:

* [Übersicht über die vorkonfigurierte Predictive Maintenance-Lösung](iot-suite-predictive-overview.md)
* [Übersicht über die vorkonfigurierte Lösung für eine verbundene Factory](iot-suite-connected-factory-overview.md)
* [Sicherheit im Internet der Dinge von Anfang an](securing-iot-ground-up.md)
