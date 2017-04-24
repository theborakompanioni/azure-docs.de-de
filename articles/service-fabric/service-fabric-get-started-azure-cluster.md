---
title: Einrichten eines Azure Service Fabric-Clusters | Microsoft-Dokumentation
description: "Schnellstart: Es wird beschrieben, wie Sie in Azure einen Service Fabric-Cluster für Windows oder Linux erstellen."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/17/2017
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 813217567c7e1fa5fc9ac11492933ad0c34553d1
ms.lasthandoff: 04/18/2017


---

# <a name="create-your-first-service-fabric-cluster-on-azure"></a>Erstellen Ihres ersten Service Fabric-Clusters in Azure
Ein [Service Fabric-Cluster](service-fabric-deploy-anywhere.md) enthält eine per Netzwerk verbundene Gruppe von virtuellen oder physischen Computern, auf denen Ihre Microservices bereitgestellt und verwaltet werden. Mithilfe dieses Schnellstarts können Sie über das [Azure-Portal](http://portal.azure.com) in wenigen Minuten einen Cluster mit fünf Knoten für Windows oder Linux erstellen.  

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure
Melden Sie sich unter [http://portal.azure.com](http://portal.azure.com) am Azure-Portal an.

## <a name="create-the-cluster"></a>Erstellen des Clusters

1. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.
2. Wählen Sie auf dem Blatt **Neu** die Option **Compute** und dann auf dem Blatt **Compute** die Option **Service Fabric-Cluster**.
3. Füllen Sie das Formular mit den **Grundeinstellungen** für Service Fabric aus. Wählen Sie für **Betriebssystem** die Windows- oder Linux-Version aus, die für die Clusterknoten ausgeführt werden soll. Der hier eingegebene Benutzername und das Kennwort werden zum Anmelden am virtuellen Computer verwendet. Erstellen Sie für **Ressourcengruppe** eine neue Ressourcengruppe. Eine Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen erstellt und kollektiv verwaltet werden. Klicken Sie zum Abschluss auf **OK**.

    ![Ausgabe bei der Clustereinrichtung][cluster-setup-basics]

4. Füllen Sie das Formular **Clusterkonfiguration** aus.  Geben Sie für **Anzahl von Knotentypen** den Wert „1“ ein, und legen Sie [Dauerhaftigkeitsstufe](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) auf „Bronze“ fest.

5. Wählen Sie **Jeden Knotentyp konfigurieren**, und füllen Sie das Formular **Knotentypkonfiguration** aus. Anhand von Knotentypen werden die VM-Größe, die Anzahl von VMs, benutzerdefinierte Endpunkte und andere Einstellungen für die VMs des jeweiligen Typs festgelegt. Jeder definierte Knotentyp wird als separate VM-Skalierungsgruppe eingerichtet, die zum Bereitstellen und Verwalten von virtuellen Computern als Gruppe verwendet wird. Jeder Knotentyp kann einzeln zentral hoch- oder herunterskaliert werden, bei jedem Typ können unterschiedliche Portgruppen geöffnet sein, und die Typen können verschiedene Kapazitätsmetriken aufweisen.  Der erste bzw. primäre Knotentyp dient zum Hosten von Service Fabric-Systemdiensten und muss über mindestens fünf VMs verfügen.

    Die [Kapazitätsplanung](service-fabric-cluster-capacity.md) ist ein wichtiger Schritt bei jeder Produktionsbereitstellung.  Für diesen Schnellstart führen Sie aber keine Anwendungen aus, sodass Sie die VM-Größe *DS1_v2 Standard* wählen können.  Wählen Sie als [Zuverlässigkeitsstufe](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) die Einstellung „Silber“, und geben Sie als Anfangskapazität für die VM-Skalierungsgruppe den Wert „5“ an.  

    Für benutzerdefinierte Endpunkte werden Ports im Azure Load Balancer geöffnet, damit Sie Verbindungen mit Anwendungen herstellen können, die im Cluster ausgeführt werden.  Geben Sie „80, 8172“ ein, um die Ports 80 und 8172 zu öffnen.

    Achten Sie darauf, dass Sie das Kontrollkästchen **Erweiterte Einstellungen konfigurieren** nicht aktivieren. Es wird zum Anpassen von TCP/HTTP-Verwaltungsendpunkten, Anwendungsportbereichen, [Platzierungseinschränkungen](service-fabric-cluster-resource-manager-configure-services.md#placement-constraints) und [Kapazitätseigenschaften](service-fabric-cluster-resource-manager-metrics.md) verwendet.    

    Klicken Sie auf **OK**.

6. Legen Sie im Formular **Clusterkonfiguration** die Option **Diagnose** auf **Ein** fest.  Für diesen Schnellstart müssen Sie keine Eigenschaften für [Fabric-Einstellung](service-fabric-cluster-fabric-settings.md) eingeben.  Wählen Sie unter **Fabricversion** den Upgrademodus **Automatisch** aus, damit die Version des im Cluster ausgeführten Fabric-Codes von Microsoft automatisch aktualisiert wird.  Legen Sie den Modus auf **Manuell** fest, wenn Sie selbst eine [unterstützte Version](service-fabric-cluster-upgrade.md) für das Upgrade auswählen möchten. 

    ![Knotentypkonfiguration][node-type-config]

    Klicken Sie auf **OK**.

7. Füllen Sie das Formular **Sicherheit** aus.  Wählen Sie für diesen Schnellstart die Option **Unsicher**.  Es wird dringend empfohlen, für Produktionsworkloads einen sicheren Cluster zu erstellen. Der Grund hierfür ist, dass mit einem unsicheren Cluster anonyme Verbindungen hergestellt und Verwaltungsvorgänge durchgeführt werden können.  

    Zertifikate werden in Service Fabric zur Authentifizierung und Verschlüsselung verwendet, um verschiedene Aspekte eines Clusters und der zugehörigen Anwendungen zu sichern. Weitere Informationen zur Verwendung von Zertifikaten in Service Fabric finden Sie unter [Szenarien für die Clustersicherheit in Service Fabric](service-fabric-cluster-security.md).  [Erstellen Sie einen Cluster mit einer Resource Manager-Vorlage](service-fabric-cluster-creation-via-arm.md), um die Benutzerauthentifizierung per Azure Active Directory zu ermöglichen oder Zertifikate für die Anwendungssicherheit einzurichten.

    Klicken Sie auf **OK**.

8. Überprüfen Sie die Zusammenfassung.  Wählen Sie die Option **Vorlage und Parameter herunterladen**, wenn Sie eine Resource Manager-Vorlage herunterladen möchten, die mit den von Ihnen eingegebenen Einstellungen erstellt wurde.  Wählen Sie **Erstellen**, um den Cluster zu erstellen.

    Sie können den Verlauf der Erstellung in den Benachrichtigungen finden. (Klicken Sie auf das Glockensymbol in der Nähe der Statusleiste am oberen rechten Bildschirmrand.) Wenn Sie beim Erstellen des Clusters auf **An Startmenü anheften** geklickt haben, wird im Menü **Start** der Hinweis **Deploying Service Fabric Cluster** (Service Fabric-Cluster wird bereitgestellt) angezeigt.

## <a name="view-cluster-status"></a>Anzeigen des Clusterstatus
Sobald der Cluster erstellt wurde, können Sie Ihren Cluster im Portal auf dem Blatt **Übersicht** überprüfen. Jetzt sehen Sie die Details Ihres Clusters im Dashboard, einschließlich des öffentlichen Endpunkts des Clusters und eines Links zu Service Fabric Explorer.

![Clusterstatus][cluster-status]

## <a name="visualize-the-cluster-using-service-fabric-explorer"></a>Visualisieren des Clusters mit Service Fabric Explorer
Mit [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) können Sie Ihren Cluster visualisieren und Anwendungen verwalten.  Service Fabric Explorer ist ein Dienst, der im Cluster ausgeführt wird.  Greifen Sie mit einem Webbrowser darauf zu, indem Sie im Portal auf der Seite **Übersicht** des Clusters auf den Link **Service Fabric Explorer** klicken.  Sie können die Adresse auch direkt in den Browser eingeben: [http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer](http://quickstartcluster.westus.cloudapp.azure.com:19080/Explorer)

Das Clusterdashboard bietet eine Übersicht über Ihren Cluster mit einer Zusammenfassung der Anwendungs- und Knotenintegrität. Die Knotenansicht zeigt das physische Layout des Clusters. Für einen Knoten können Sie überprüfen, für welche Anwendungen Code auf dem Knoten bereitgestellt wurde.

![Service Fabric Explorer][service-fabric-explorer]

## <a name="connect-to-the-cluster-using-powershell"></a>Herstellen einer Verbindung mit dem Cluster über PowerShell
Stellen Sie sicher, dass der Cluster ausgeführt wird, indem Sie mit PowerShell eine Verbindung herstellen.  Das Service Fabric-PowerShell-Modul wird zusammen mit dem [Service Fabric-SDK](service-fabric-get-started.md) installiert.  Das Cmdlet [Connect-ServiceFabricCluster](/powershell/module/ServiceFabric/Connect-ServiceFabricCluster) stellt eine Verbindung mit dem Cluster her.   

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint localhost:19000
```
Weitere Beispiele für die Herstellung einer Clusterverbindung finden Sie unter [Herstellen einer Verbindung mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md). Verwenden Sie nach dem Herstellen einer Verbindung mit dem Cluster das Cmdlet [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode), um eine Liste mit den Knoten des Clusters sowie Statusinformationen für die einzelnen Knoten anzuzeigen. **HealthState** muss für jeden Knoten *OK* lauten.

```powershell
PS C:\> Get-ServiceFabricNode |Format-Table

NodeDeactivationInfo NodeName     IpAddressOrFQDN NodeType  CodeVersion ConfigVersion NodeStatus NodeUpTime NodeDownTime HealthState
-------------------- --------     --------------- --------  ----------- ------------- ---------- ---------- ------------ -----------
                     _nodetype1_2 10.0.0.6        nodetype1 5.5.216.0   1                     Up 00:59:04   00:00:00              Ok
                     _nodetype1_1 10.0.0.5        nodetype1 5.5.216.0   1                     Up 00:59:04   00:00:00              Ok
                     _nodetype1_0 10.0.0.4        nodetype1 5.5.216.0   1                     Up 00:59:04   00:00:00              Ok
                     _nodetype1_4 10.0.0.8        nodetype1 5.5.216.0   1                     Up 00:59:04   00:00:00              Ok
                     _nodetype1_3 10.0.0.7        nodetype1 5.5.216.0   1                     Up 00:59:04   00:00:00              Ok
```

## <a name="remove-the-cluster"></a>Entfernen des Clusters
Ein Service Fabric-Cluster besteht, zusätzlich zu der Clusterressource selbst, aus weiteren Azure-Ressourcen. Daher müssen Sie alle Ressourcen löschen, aus denen der Cluster besteht, um einen Service Fabric-Cluster zu löschen. Die einfachste Möglichkeit zum Löschen des Clusters und aller dazugehörigen Ressourcen besteht darin, die Ressourcengruppe zu löschen. Andere Möglichkeiten zum Löschen eines Clusters oder nur einiger Ressourcen einer Ressourcengruppe finden Sie unter [Löschen eines Clusters](service-fabric-cluster-delete.md).

Löschen Sie eine Ressourcengruppe im Azure-Portal:
1. Navigieren Sie zu dem Service Fabric-Cluster, den Sie löschen möchten.
2. Klicken Sie auf der Clusterseite „Essentials“ (Zusammenfassung) auf den Namen der **Ressourcengruppe**.
3. Klicken Sie auf der Seite **Essentials** (Zusammenfassung) der Ressourcengruppe auf **Löschen**, und befolgen Sie die Anleitung auf der Seite, um das Löschen der Ressourcengruppe durchzuführen.
    ![Löschen der Ressourcengruppe][cluster-delete]

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun einen eigenständigen Entwicklungscluster eingerichtet haben, können Sie Folgendes ausprobieren:
* [Erstellen eines sicheren Clusters im Portal](service-fabric-cluster-creation-via-portal.md)
* [Erstellen eines Clusters aus einer Vorlage](service-fabric-cluster-creation-via-arm.md) 
* [Bereitstellen von Apps mit PowerShell](service-fabric-deploy-remove-applications.md)


[cluster-setup-basics]: ./media/service-fabric-get-started-azure-cluster/basics.png
[node-type-config]: ./media/service-fabric-get-started-azure-cluster/nodetypeconfig.png
[cluster-status]: ./media/service-fabric-get-started-azure-cluster/clusterstatus.png
[service-fabric-explorer]: ./media/service-fabric-get-started-azure-cluster/sfx.png
[cluster-delete]: ./media/service-fabric-get-started-azure-cluster/delete.png
