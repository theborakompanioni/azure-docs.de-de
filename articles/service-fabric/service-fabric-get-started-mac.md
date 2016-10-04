<properties
   pageTitle="Einrichten Ihrer Entwicklungsumgebung unter Mac OS X | Microsoft Azure"
   description="Installieren Sie Laufzeit, SDK und Tools, und erstellen Sie einen lokalen Entwicklungscluster. Nach Abschluss des Setups können Sie mit der Erstellung von Clientanwendungen unter Mac OS X beginnen."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="seanmck"/>

# Einrichten Ihrer Entwicklungsumgebung unter Mac OS X

> [AZURE.SELECTOR]
-[ Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

Sie können Service Fabric-Anwendungen für die Ausführung in Linux-Clustern unter Mac OS X erstellen. In diesem Artikel erfahren Sie, wie Sie Ihren Mac für die Entwicklung einrichten.

## Voraussetzungen

Service Fabric wird nicht nativ unter OS X ausgeführt. Für die Verwendung eines lokalen Service Fabric-Clusters stellen wir einen vorkonfigurierten virtuellen Ubuntu-Computer mit Vagrant und VirtualBox bereit. Bevor Sie beginnen, benötigen Sie Folgendes:

- [Vagrant (mindestens Version 1.8.4)](http://wwww.vagrantup.com/downloads)
- [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

## Erstellen des lokalen virtuellen Computers

Führen Sie die folgenden Schritte aus, um den lokalen virtuellen Computer mit einem Service Fabric-Cluster mit fünf Knoten zu erstellen:

1. Klonen Sie das Vagrantfile-Repository:

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```

2. Navigieren Sie zum lokalen Klon des Repositorys:

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```

3. Optional: Ändern Sie die Standardeinstellungen des virtuellen Computers

    Der lokale virtuelle Computer ist standardmäßig wie folgt konfiguriert:

    - 3 GB zugeordneter Arbeitsspeicher
    - Privates Hostnetzwerk (konfiguriert mit der IP-Adresse 192.168.50.50, um Pass-Through-Datenverkehr des Mac-Hosts zu ermöglichen)

    Sie können diese Einstellungen ändern oder die Konfiguration des virtuellen Computers über Vagrantfile ergänzen. Eine Liste mit allen Konfigurationsoptionen finden Sie in der [Vagrant-Dokumentation](http://www.vagrantup.com/docs).

4. Erstellen des virtuellen Computers

    ```bash
    vagrant up
    ```

    In diesem Schritt wird das vorkonfigurierte VM-Image heruntergeladen und lokal gestartet. Anschließend wird auf dem virtuellen Computer ein lokaler Service Fabric-Cluster eingerichtet. Dieser Vorgang dauert ein paar Minuten. Wenn die Einrichtung erfolgreich war, erscheint in der Ausgabe eine Meldung mit dem Hinweis, dass der Cluster gestartet wird:

    ![Start der Clustereinrichtung nach Bereitstellung des virtuellen Computers][cluster-setup-script]

5. Vergewissern Sie sich, dass der Cluster ordnungsgemäß eingerichtet wurde, indem Sie zu Service Fabric Explorer navigieren: http://192.168.50.50:19080/Explorer (vorausgesetzt, Sie haben die standardmäßige IP-Adresse des privaten Netzwerks beibehalten)

    ![Service Fabric Explorer auf dem Host-Mac][sfx-mac]


## Installieren des Service Fabric-Plug-Ins für Eclipse Neon (optional)

Service Fabric bietet ein Plug-In für die Eclipse Neon-IDE, das die Erstellung und Bereitstellung von Java-Diensten vereinfachen kann.

1. Vergewissern Sie sich in Eclipse, dass mindestens die Buildship-Version 1.0.17 installiert ist. Die Version der installierten Komponenten können Sie unter **Hilfe > Installationsdetails** ermitteln. Eine Aktualisierungsanleitung für Buildship finden Sie [hier][buildship-update].

2. Wählen Sie zum Installieren des Service Fabric-Plug-Ins **Hilfe > Install New Software...** (Neue Software installieren...) aus.

3. Geben Sie im Textfeld „Work with“ (Verwenden mit) Folgendes ein: http://dl.windowsazure.com/eclipse/servicefabric.

4. Klicken Sie auf "Hinzufügen".

    ![Eclipse Neon-Plug-In für Service Fabric][sf-eclipse-plugin-install]

5. Wählen Sie das Service Fabric-Plug-In aus, und klicken Sie auf „Weiter“.

6. Fahren Sie mit der Installation fort, und akzeptieren Sie den Endbenutzer-Lizenzvertrag.

## Nächste Schritte

- [Create your first Azure Service Fabric application](service-fabric-create-your-first-linux-application-with-java.md) (Erstellen Ihrer ersten Azure Service Fabric-Anwendung)

<!-- Links -->

- [Erstellen eines Service Fabric-Clusters in Azure über das Azure-Portal](service-fabric-cluster-creation-via-portal.md)
- [Erstellen eines Service Fabric-Clusters in Azure mithilfe von Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
- [Modellieren von Anwendungen in Service Fabric](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!---HONumber=AcomDC_0928_2016-->