---
title: Einrichten eines Service Fabric-Clusters mithilfe von Visual Studio | Microsoft Docs
description: Beschreibt das Einrichten eines Service Fabric-Clusters mithilfe einer Azure Resource Manager-Vorlage, die anhand eines Azure-Ressourcengruppenprojekts in Visual Studio erstellt wurde
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: adegeo
editor: 
ms.assetid: bd2c0511-36c9-4828-8dc3-69e4b6a70567
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/21/2017
ms.author: mikhegn@microsoft.com
translationtype: Human Translation
ms.sourcegitcommit: 0eefa64d6292dec14842c031ebad80fa9094436d
ms.openlocfilehash: 535e33eec22ed1c488cace9442328084b416b7a8


---
# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a>Einrichten eines Service Fabric-Clusters mit Visual Studio
In diesem Artikel wird das Einrichten eines Azure Service Fabric-Clusters mit Visual Studio und einer Azure Resource Manager-Vorlage beschrieben. Wir verwenden ein Azure-Ressourcengruppenprojekt in Visual Studio zum Erstellen der Vorlage. Nachdem die Vorlage erstellt wurde, kann sie in Visual Studio direkt in Azure bereitgestellt werden. Sie kann auch aus einem Skript oder als Teil einer CI-Einrichtung (Continuous Integration) verwendet werden.

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a>Erstellen einer Service Fabric-Clustervorlage mittels eines Azure-Ressourcengruppenprojekts
Öffnen Sie zunächst Visual Studio, und erstellen Sie ein Azure-Ressourcengruppenprojekt (das unter dem Ordner **Cloud** verfügbar ist):

![Dialogfeld „Neues Projekt“ mit ausgewähltem Projekt „Azure-Ressourcengruppe“][1]

Sie können eine neue Visual Studio-Projektmappe für dieses Projekt erstellen oder es einer vorhandenen Projektmappe hinzufügen.

> [!NOTE]
> Wenn das Azure-Ressourcengruppenprojekt nicht unter dem Knoten „Cloud“ angezeigt wird, ist das Azure SDK nicht installiert. Starten Sie den Webplattform-Installer ([installieren Sie ihn jetzt](http://www.microsoft.com/web/downloads/platform.aspx) , wenn dies nicht bereits geschehen ist), suchen Sie nach „Azure SDK für .NET“, und installieren Sie die Version, die mit Ihrer Version von Visual Studio kompatibel ist.
> 
> 

Nachdem Sie auf die Schaltfläche „OK“ geklickt haben, fordert Visual Studio Sie auf, die Ressourcen-Manager-Vorlage auszuwählen, die Sie erstellen möchten:

![Dialogfeld „Azure-Vorlage auswählen“ mit ausgewählter Service Fabric-Clustervorlage][2]

Wählen Sie die Vorlage „Service-Fabric-Cluster“ aus, und klicken Sie erneut auf „OK“. Das Projekt und die Ressourcen-Manager-Vorlage sind jetzt erstellt.

## <a name="prepare-the-template-for-deployment"></a>Vorbereiten der Vorlage für die Bereitstellung
Bevor die Vorlage bereitgestellt wird, um den Cluster zu erstellen, müssen Sie Werte für die erforderlichen Vorlagenparameter angeben. Diese Parameterwerte werden aus der Datei `ServiceFabricCluster.parameters.json` gelesen, die sich im Ordner `Templates` des Ressourcengruppenprojekts befindet. Öffnen Sie die Datei, und geben Sie die folgenden Werte an:

| Parametername | Beschreibung |
| --- | --- |
| adminUserName |Der Name des Administratorkontos für Service Fabric-Computer (Knoten). |
| certificateThumbprint |Der Fingerabdruck des Zertifikats für den Schutz des Clusters. |
| sourceVaultResourceId |Die *Ressourcen-ID* des Schlüsseltresors, in dem das Zertifikat zum Schutz des Clusters gespeichert ist. |
| certificateUrlValue |Die URL des Sicherheitszertifikats des Clusters. |

Die Visual Studio Service Fabric-Ressourcen-Manager-Vorlage erstellt einen sicheren Cluster, der durch ein Zertifikat geschützt ist. Dieses Zertifikat wird durch die letzten drei Vorlagenparameter (`certificateThumbprint`, `sourceVaultValue` und `certificateUrlValue`) identifiziert und muss in einer **Azure Key Vault**-Instanz vorhanden sein. Weitere Informationen zum Erstellen des Sicherheitszertifikats des Clusters finden Sie unter [Szenarien für die Clustersicherheit in Service Fabric](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) .

## <a name="optional-change-the-cluster-name"></a>Optional: Ändern des Clusternamens
Jeder Service Fabric-Cluster verfügt über einen Namen. Wenn ein Fabric-Cluster in Azure erstellt wird, bestimmt der Clustername (zusammen mit der Azure-Region) den DNS-Namen (Domain Name System) für den Cluster. Beispiel: Wenn Sie Ihren Cluster `myBigCluster` nennen und ihn in einer Ressourcengruppe am Standort (bzw. in der Azure-Region) „USA, Osten“ hosten, lautet der DNS-Name des Clusters `myBigCluster.eastus.cloudapp.azure.com`.

Standardmäßig wird der Name des Clusters automatisch generiert und durch Anfügen eines zufälligen Suffixes an das Präfix „cluster“ eindeutig festgelegt. So kann die Vorlage sehr einfach als Teil eines **Continuous Integration** -Systems (CI) verwendet werden. Wenn Sie einen bestimmten (für Sie aussagekräftigen) Namen für den Cluster verwenden möchten, legen Sie den Wert der `clusterName`-Variablen in der Resource Manager-Vorlagendatei (`ServiceFabricCluster.json`) auf Ihren ausgewählten Namen fest. Diese Variable ist die erste Variable, die in der Datei definiert ist.

## <a name="optional-add-public-application-ports"></a>Optional: Hinzufügen öffentlicher Anwendungsports
Vielleicht möchten Sie auch die öffentlichen Anwendungsports für den Cluster vor seiner Bereitstellung ändern. Standardmäßig öffnet die Vorlage nur zwei öffentliche TCP-Ports (80 und 8081). Wenn Sie für Ihre Anwendungen mehr benötigen, ändern Sie die Azure-Load Balancer-Definition in der Vorlage. Die Definition ist in der Hauptvorlagendatei (`ServiceFabricCluster.json`) gespeichert. Öffnen Sie diese Datei, und suchen Sie nach `loadBalancedAppPort`. Jeder Port ist drei Artefakten zugeordnet:

1. Einer Vorlagenvariablen, die den Wert für den TCP-Port definiert:
   
    ```json
    "loadBalancedAppPort1": "80"
    ```
2. Einem *Test*, der bestimmt, wie oft und wie lange der Azure Load Balancer versucht, einen bestimmten Service Fabric-Knoten zu verwenden, bevor ein Failover auf einen anderen erfolgt. Die Tests sind Teil der Load Balancer-Ressource. Hier sehen Sie die Testdefinition für den ersten Standardport der Anwendung:
   
    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```
3. Eine *Lastenausgleichsregel* , die den Port und den Test miteinander verbindet und so einen Lastenausgleich über eine Gruppe von Service Fabric-Clusterknoten hinweg ermöglicht:
   
    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
   Wenn die Anwendungen, die Sie im Cluster bereitstellen möchten, mehr Ports benötigen, können Sie diese hinzufügen, indem Sie zusätzliche Regeldefinitionen für Test und Lastenausgleich erstellen. Weitere Informationen zum Arbeiten mit Azure Load Balancer über Resource Manager-Vorlagen finden Sie unter [Erste Schritte zum Erstellen eines internen Load Balancers mithilfe einer Vorlage](../load-balancer/load-balancer-get-started-ilb-arm-template.md).

## <a name="deploy-the-template-by-using-visual-studio"></a>Bereitstellen der Vorlage mit Visual Studio
Nachdem Sie alle erforderlichen Parameterwerte in der Datei`ServiceFabricCluster.param.dev.json` gespeichert haben, können Sie die Vorlage bereitstellen und Ihren Service Fabric-Cluster erstellen. Klicken Sie im Visual Studio-Projektmappen-Explorer mit der rechten Maustaste auf das Ressourcengruppenprojekt, und wählen Sie **Bereitstellen | Neue Bereitstellung...**aus. Bei Bedarf zeigt Visual Studio das Dialogfeld **Für Ressourcengruppe bereitstellen** an und fordert Sie auf, sich bei Azure zu authentifizieren:

![Dialogfeld "In Ressourcengruppe bereitstellen"][3]

Im Dialogfeld können Sie eine vorhandene Ressourcen-Manager-Ressourcengruppe für den Cluster auswählen und nach Wunsch eine neue erstellen. In der Regel ist es sinnvoll, eine eigene Ressourcengruppe für einen Service Fabric-Cluster zu verwenden.

Nach Klicken auf die Schaltfläche „Bereitstellen“ werden Sie von Visual Studio aufgefordert, die Parameterwerte der Vorlage zu bestätigen. Klicken Sie auf die Schaltfläche **Speichern** . Ein Parameter hat keinen persistenten Wert: das Administratorkonto-Kennwort für den Cluster. Sie müssen einen Kennwortwert angeben, wenn Visual Studio Sie dazu auffordert.

> [!NOTE]
> Ab Azure SDK 2.9 unterstützt Visual Studio das Lesen von Kennwörtern aus **Azure Key Vault** während der Bereitstellung. Beachten Sie im Dialogfeld für die Vorlagenparameter, dass das Parametertextfeld `adminPassword` auf der rechten Seite ein kleines Schlüsselsymbol aufweist. Über dieses Symbol können Sie ein vorhandenes Schlüsseltresorgeheimnis als Administratorkennwort für den Cluster auswählen. Sie müssen unbedingt als Erstes den Zugriff von Azure Resource Manager auf die Vorlagenbereitstellung in den erweiterten Zugriffsrichtlinien Ihres Schlüsseltresors aktivieren. 
> 
> 

Sie können den Status des Bereitstellungsprozesses im Ausgabefenster von Visual Studio überwachen. Nach Abschluss der Bereitstellung der Vorlage ist Ihr neuer Cluster einsatzbereit!

> [!NOTE]
> Wenn PowerShell noch nicht zum Verwalten von Azure auf dem aktuell genutzten Computer verwendet wurde, sind einige einfache Wartungsaufgaben nötig.
> 
> 1. Aktivieren Sie die Verwendung von PowerShell-Skripts durch Ausführen des Befehls [`Set-ExecutionPolicy`](https://technet.microsoft.com/library/hh849812.aspx) . Für Entwicklungscomputer ist die Richtlinie „Unrestricted“ in der Regel akzeptabel.
> 2. Entscheiden Sie, ob die Sammlung von Diagnosedaten mit Azure PowerShell-Befehlen zugelassen werden soll, und führen Sie nach Bedarf [`Enable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619303.aspx) oder [`Disable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619236.aspx) aus. Dadurch lassen sich unnötige Eingabeaufforderungen während der Bereitstellung der Vorlage vermeiden.
> 
> 

Wenn Fehler auftreten, wechseln Sie zum [Azure-Portal](https://portal.azure.com/) , und öffnen Sie die Ressourcengruppe, in der Sie die Bereitstellung durchgeführt haben. Klicken Sie auf **Alle Einstellungen**, und klicken Sie dann auf dem Blatt „Einstellungen“ auf **Bereitstellungen**. Falls die Bereitstellung einer Ressourcengruppe keinen Erfolg hatte, finden Sie dort ausführliche Diagnoseinformationen.

> [!NOTE]
> Um die Verfügbarkeit zu gewährleisten und den Zustand beizubehalten, muss eine bestimmte Anzahl von Knoten in einem Service Fabric-Cluster in Betrieb sein. Dies wird auch als „Aufrechterhalten eines Quorums“ bezeichnet. Aus Sicherheitsgründen sollten nicht alle Computer innerhalb des Clusters heruntergefahren werden, sofern Sie zuvor keine [vollständige Sicherung des Zustands](service-fabric-reliable-services-backup-restore.md) durchgeführt haben.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* [Informationen zum Einrichten eines Service Fabric-Clusters mit dem Azure-Portal](service-fabric-cluster-creation-via-portal.md)
* [Informationen zum Verwalten und Bereitstellen von Service Fabric-Anwendungen mit Visual Studio](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png



<!--HONumber=Feb17_HO4-->


