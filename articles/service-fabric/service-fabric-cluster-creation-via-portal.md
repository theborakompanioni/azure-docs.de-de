<properties
   pageTitle="Erstellen eines Service Fabric-Clusters im Azure-Portal | Microsoft Azure"
   description="Erstellen Sie einen Service Fabric-Cluster im Azure-Portal."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="03/28/2016"
   ms.author="chackdan"/>


# Erstellen eines Service Fabric-Clusters im Azure-Portal

Auf dieser Seite erhalten Sie Informationen zum Einrichten eines Service Fabric-Clusters. Ihr Abonnement muss ausreichend Kerne zum Bereitstellen der virtuellen IaaS-Computer umfassen, aus denen dieser Cluster bestehen soll.


## Suchen nach der Service Fabric-Clusterressource

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.

2. Klicken Sie auf **+ Neu**, um eine neue Ressourcenvorlage hinzuzufügen. Suchen Sie Ihre Vorlage im **Marketplace** unter **Alles**. Sie heißt **Service Fabric Cluster**.

    a. Klicken Sie in der obersten Ebene auf **Marketplace**.

    b. Geben Sie unter **Alles** „Fabric“ ein, und drücken Sie die EINGABETASTE. Manchmal funktioniert der automatische Filter nicht. Drücken Sie daher in jedem Fall die EINGABETASTE. ![Screenshot der Suche nach der Service Fabric-Clustervorlage im Azure-Portal.][SearchforServiceFabricClusterTemplate]

3. Wählen Sie **Service Fabric-Cluster** aus der Liste aus.

4. Wechseln Sie zum Blatt **Service Fabric-Cluster**, und klicken Sie auf **Erstellen**.

5. Ihnen wird nun ein Blatt **Service Fabric-Cluster erstellen** angezeigt, auf dem 4 Schritte aufgeführt sind.

## Schritt 1: Grundeinstellungen

Auf dem Blatt mit den Grundeinstellungen müssen die grundlegenden Details für Ihren Cluster angegeben werden.

1. Geben Sie den Namen Ihres Clusters ein.

2. Wählen Sie einen **Benutzernamen** und **Kennwort** für den VM-Remotedesktop.

3. Stellen Sie sicher, dass Sie das gewünschte **Abonnement** auswählen, für das Ihr Cluster bereitgestellt werden soll. Dies ist besonders wichtig, wenn Sie über mehrere Abonnements verfügen.

4. Erstellen Sie eine **neue Ressourcengruppe** mit demselben Namen wie der Cluster, damit Sie sie später einfach finden können. Das ist besonders hilfreich, wenn Sie Änderungen an Ihrer Bereitstellung vornehmen oder Ihren Cluster löschen.

    >[AZURE.NOTE] Auch wenn Sie eine vorhandene Ressourcengruppe verwenden können, empfiehlt es sich, eine neue Ressourcengruppe zu erstellen. So können Sie leichter die Cluster löschen, die Sie nicht mehr benötigen.

 	![Screenshot der Erstellung einer neuen Ressourcengruppe.][CreateRG]


5. Wählen Sie einen **Standort** aus der Dropdownliste aus. Der Standardwert ist **USA, Westen**. Klicken Sie auf "OK".

## Schritt 2: Konfigurieren des Clusters

10. Zunächst soll erläutert werden, was ein **Knotentyp** ist. Der Knotentyp kann als Äquivalent zu Rollen in Clouddiensten betrachtet werden. Knotentypen definieren die Größe, die Anzahl und die Eigenschaften der virtuellen Computer. Der Cluster kann über mehrere Knotentypen verfügen. Der primäre Knotentyp (der erste, den Sie im Portal definieren), muss jedoch mindestens fünf VMs aufweisen. Dies ist der Knotentyp, auf dem Service Fabric-Systemdienste platziert werden. Berücksichtigen Sie Folgendes, wenn Sie Ihre Anforderungen im Hinblick auf mehrere Knotentypen ermitteln.

	* Die Anwendung, die bereitgestellt werden soll, umfasst einen Front-End-Dienst und einen Back-End-Dienst. Der Front-End-Dienst soll auf kleineren VMs (VM-Größen wie D2) ausgeführt werden, und die VMs weisen Ports auf, die für das Internet geöffnet sind. Der rechenintensive Back-End-Dienst soll auf größeren VMs (VM-Größen wie D4, D6, D15 usw.) platziert werden, die nicht über Internetzugriff verfügen.

	* Auch wenn beide Dienste auf einem Knotentyp verwendet werden können, wird empfohlen, sie in einen Cluster mit zwei Knotentypen zu platzieren. Jeder Knotentyp kann unterschiedliche Eigenschaften aufweisen, wie Internetkonnektivität, VM-Größe und Anzahl der virtuellen Computer, die unabhängig voneinander skaliert werden können.

	* Definieren Sie zuerst einen Knotentyp, der mindestens fünf virtuelle Computer umfasst. Die anderen Knotentypen können über mindestens einen virtuellen Computer verfügen.

13.  So konfigurieren Sie Ihren Knotentyp:

	a. Wählen Sie einen Namen für Ihren Knotentyp aus (ein bis zwölf Zeichen, nur Buchstaben und Zahlen).

	b. Die Mindestgröße von VMs für den primären Knotentyp hängt von der Dauerhaftigkeitsstufe ab, die Sie für den Cluster wählen. Der Standardwert für die Dauerhaftigkeitsstufe ist „Bronze“. Weitere Informationen zum [Auswählen der Zuverlässigkeits- und Dauerhaftigkeitsmerkmale für Service Fabric-Cluster](service-fabric-cluster-capacity.md).

	b. Wählen Sie die VM-Größe und den Tarif aus. Standardmäßig ist D4 Standard ausgewählt. Wenn Sie diesen Cluster jedoch nur zum Testen Ihrer Anwendung verwenden, können Sie auch D2 oder kleinere virtuelle Computer auswählen.

	c. Die Mindestanzahl von VMs für den primären Knotentyp hängt von der Zuverlässigkeitsstufe ab, die Sie für den Cluster wählen. Der Standardwert für die Zuverlässigkeitsstufe ist „Silber“. Weitere Informationen zum [Auswählen der Zuverlässigkeits- und Dauerhaftigkeitsmerkmale für Service Fabric-Cluster](service-fabric-cluster-reliability-and-durability.md).

	c. Wählen Sie die Anzahl von VMs für den Knotentyp aus. Sie können die Anzahl von VMs auf einem Knotentyp später zentral hoch- oder herunterskalieren, beim primären Knotentyp wird die Mindestanzahl jedoch durch die gewählte Zuverlässigkeitsstufe bestimmt. Die anderen Knotentypen können über mindestens einen virtuellen Computer verfügen.


  	![Screenshot der Erstellung eines Knotentyps.][CreateNodeType]

9. Wenn Sie Ihre Anwendungen sofort im Cluster bereitstellen möchten, fügen Sie Ports, die Sie für Ihre Anwendungen öffnen möchten, auf einem Knotentyp **Anwendungsports** (oder auf den von Ihnen erstellten Knotentypen) hinzu. Sie können dem Knotentyp später Ports hinzufügen, indem Sie den Lastenausgleich ändern, der diesem Knoten zugeordnet ist. (Fügen Sie einen Test hinzu, und fügen Sie den Test dann den Lastenausgleichsregeln hinzu.) Das gestaltet sich nun es etwas einfacher, da die erforderlichen Tests und Regeln dem Lastenausgleich durch die Portalautomatisierung hinzugefügt werden.

	a. Sie finden die Anwendungsports in Ihren Dienstmanifesten, die im Anwendungspaket enthalten sind. Rufen Sie Ihre Anwendungen auf, öffnen Sie die Dienstmanifeste, und notieren Sie alle Eingabeendpunkte, die Ihre Anwendung zum Kommunizieren mit der Außenwelt benötigt.

	b. Fügen Sie im Feld **Anwendungseingabe-Endpunkte** alle Ports (durch Kommas getrennt) hinzu. Der Endpunkt der TCP-Client-Verbindung ist standardmäßig 19000, sodass Sie keinen angegeben müssen. Für die Beispielanwendung „WordCount“ muss beispielsweise Port 83 geöffnet sein. Sie finden dies im Anwendungspaket in der Datei „servicemanifest.xml“. (Es gibt möglicherweise mehrere Dateien des Namens „servicemanifest.xml“.)

    c. Die meisten Beispielanwendungen verwenden Port 80 und 8081. Fügen Sie diese also hinzu, wenn Sie Beispiele in diesem Cluster bereitstellen möchten. ![Ports][Ports]

10. Sie müssen keine **Platzierungseigenschaften** konfigurieren, da vom System eine standardmäßige Platzierungseigenschaft „NodeTypeName“ hinzugefügt wird. Wenn die Anwendung dies erfordert, können Sie weitere hinzufügen.

11. Sie müssen keine **Kapazitätseigenschaften** konfigurieren. Dies wird jedoch empfohlen, da Sie diese in Ihren Anwendungen verwenden können, um Informationen zur Last an das System zu übermitteln. So kann Einfluss auf die Vorgänge zur Platzierung und zum Ressourcenausgleich genommen werden, die das System im Service Fabric-Cluster ausführt. Weitere Informationen zum Service Fabric-Ressourcenausgleich finden Sie in [diesem Dokument](service-fabric-cluster-resource-manager-architecture.md).

12. Führen Sie die oben beschriebenen Schritte für alle Knotentypen aus.

14. Konfigurieren der **Diagnose** für Cluster. Die Diagnose ist standardmäßig in Ihrem Cluster aktiviert, um die Behebung von Problemen in Ihrem Cluster zu vereinfachen. Zum Deaktivieren der Diagnose ändern Sie den **Status** in **Aus**. Das Ausschalten der Diagnose wird **nicht** empfohlen.

15. Optional: Festlegen der **Service Fabric-Clustereinstellungen** Mit dieser fortgeschrittenen Option können Sie die Standardeinstellungen für den Service Fabric-Cluster ändern. Es wird empfohlen, die Standardeinstellungen unverändert zu lassen, wenn Ihre Anwendung oder Ihr Cluster nicht unbedingt eine Änderung erfordert.



## Schritt 3: Konfigurieren der Sicherheitseinstellungen

Zu diesem Zeitpunkt unterstützt Service Fabric die Sicherung von Clustern nur über ein X.509-Zertifikat. Bevor Sie diesen Vorgang starten, müssen Sie Ihr Zertifikat in Key Vault hochladen. Weitere Informationen zur Vorgehensweise finden Sie unter [Service Fabric-Clustersicherheit](service-fabric-cluster-security.md).

Die Cluster-Sicherung ist optional, wird jedoch dringend empfohlen. Wenn Sie Ihren Cluster nicht schützen möchten, wählen Sie für **Sicherheitsmodus** die Option **Unsicher** auswählen. Hinweis: Ein unsicherer Cluster kann zu einem späteren Zeitpunkt **nicht** in einen sicheren Cluster aktualisiert werden.

Sicherheitsaspekte und Anweisungen finden Sie unter [Service Fabric cluster security (Service Fabric-Clustersicherheit)](service-fabric-cluster-security.md).

![Screenshot der Sicherheitskonfigurationen im Azure-Portal.][SecurityConfigs]


## Schritt 4: Abschließen der Clustererstellung

Um die Erstellung des Clusters abzuschließen, klicken Sie auf **Zusammenfassung**, um die bereitgestellten Konfigurationen anzuzeigen, oder laden Sie die Azure Resource Manager-Vorlage herunter, die zur Bereitstellung des Clusters verwendet werden soll. Nachdem Sie die Pflichteinstellungen angegeben haben, wird die Schaltfläche **OK** aktiviert. Klicken Sie auf diese Schaltfläche, um den Vorgang zur Clustererstellung zu starten.

Sie können den Verlauf der Erstellung in den Benachrichtigungen finden. (Klicken Sie auf das Glockensymbol in der Nähe der Statusleiste am oberen rechten Bildschirmrand.) Wenn Sie beim Erstellen des Clusters auf **An Startmenü anheften** geklickt haben, wird im **Startmenü** der Hinweis **Service Fabric-Cluster wird bereitgestellt** angezeigt.

![Screenshot des Startmenüs mit der Anzeige „Service Fabric-Cluster wird bereitgestellt“.][Notifications]

## Anzeigen des Clusterstatus

Sobald der Cluster erstellt wurde, können Sie Ihren Cluster im Portal überprüfen.

1. Wechseln Sie zu **Durchsuchen**, und klicken Sie auf **Service Fabric-Cluster**.

2. Suchen Sie Ihren Cluster, und klicken Sie darauf. 

![Screenshot der Suche nach dem Cluster im Portal.][BrowseCluster]
3. Sie können jetzt die Details Ihres Clusters, einschließlich der öffentlichen IP-Adresse, im Dashboard anzeigen. Wenn Sie die Maus über **Öffentliche IP-Adresse des Clusters** bewegen, wird eine Zwischenablage angezeigt, in die Sie die Adresse kopieren können, indem Sie hinein klicken. 

![Screenshot der Clusterdetails im Dashboard.][ClusterDashboard]
  Der Abschnitt **Knotenmonitor** auf dem Dashboardblatt des Clusters gibt die Anzahl von virtuellen Computern an, die fehlerfrei bzw. fehlerhaft sind. Weitere Informationen zur Clusterintegrität finden Sie unter [Service Fabric health model introduction (Einführung in die Service Fabric-Integritätsüberwachung)](service-fabric-health-introduction.md).

>[AZURE.NOTE] Um Verfügbarkeit sicherzustellen und den Zustand beizubehalten, muss eine bestimmte Anzahl von Knoten in einem Service Fabric-Cluster stets in Betrieb sein. Dies wird auch als „Aufrechterhalten eines Quorums“ bezeichnet. Daher ist es üblicherweise nicht sicher, alle Computer innerhalb des Clusters herunterzufahren, sofern Sie nicht zunächst eine [vollständige Sicherung des Zustands](service-fabric-reliable-services-backup-restore.md) durchgeführt haben.

## Herstellen einer Verbindung mit dem Cluster und Bereitstellen einer Anwendung

Nach Abschluss der Clustereinrichtung können Sie jetzt eine Verbindung herstellen und mit dem Bereitstellen von Anwendungen beginnen. Starten Sie Windows PowerShell auf einem Computer, auf dem Sie das Service Fabric SDK installiert haben. Führen Sie zum Herstellen einer Verbindung mit dem Cluster einen der folgenden PowerShell-Befehle aus, je nachdem, ob sie einen sicheren oder einen unsicheren Cluster erstellt haben:

### Herstellen einer Verbindung mit einem unsicheren Cluster

    ```powershell
    Connect-serviceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 -KeepAliveIntervalInSec 10
    ```

### Herstellen einer Verbindung mit einem sicheren Cluster

    1. Führen Sie die folgenden Befehle aus, um das Zertifikat auf dem Computer einzurichten, den Sie zum Ausführen des PowerShell-Befehls „Connect-serviceFabricCluster“ verwenden möchten.

        ```powershell
        Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
                -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
                -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
        ```

    2. Führen Sie den folgenden PS aus, um die Verbindung zu einem sicheren Cluster auszuführen. Die Details des Zertifikats entsprechen Ihren Angaben im Portal.

        ```powershell
        Connect-serviceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
                  -KeepAliveIntervalInSec 10 `
                  -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `
                  -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `
                  -StoreLocation CurrentUser -StoreName My
        ```

Beispielsweise sollte der PS-Befehls oben wie der Folgende aussehen.

        ```powershell
        Connect-serviceFabricCluster -ConnectionEndpoint sfcluster4doc.westus.cloudapp.azure.com:19000 `
                  -KeepAliveIntervalInSec 10 `
                  -X509Credential -ServerCertThumbprint C179E609BBF0B227844342535142306F3913D6ED `
                  -FindType FindByThumbprint -FindValue C179E609BBF0B227844342535142306F3913D6ED `
                  -StoreLocation CurrentUser -StoreName My
        ```

### Bereitstellen Ihrer App
Nachdem Sie die Verbindung hergestellt haben, führen Sie die folgenden Befehle zum Bereitstellen der Anwendung aus. Ersetzen Sie dabei die angezeigten Pfade durch die entsprechenden Pfade auf Ihrem Computer. Im folgenden Beispiel wird die Beispielanwendung zur Wortzählung bereitgestellt:

1. Kopieren Sie das Paket in den Cluster, mit dem Sie zuvor eine Verbindung hergestellt haben.

    ```powershell
    $applicationPath = "C:\VS2015\WordCount\WordCount\pkg\Debug"
    ```

    ```powershell
    Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $applicationPath -ApplicationPackagePathInImageStore "WordCount" -ImageStoreConnectionString fabric:ImageStore
    ```
2. Registrieren Sie ihren Anwendungstyp bei Service Fabric.

    ```powershell
    Register-ServiceFabricApplicationType -ApplicationPathInImageStore "WordCount"
    ```

3. Erstellen Sie eine neue Instanz auf dem soeben registrierten Anwendungstyp.

    ```powershell
    New-ServiceFabricApplication -ApplicationName fabric:/WordCount -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0.0
    ```

4. Öffnen Sie jetzt den gewünschten Browser, und stellen Sie eine Verbindung mit dem Endpunkt her, auf dem die Anwendung lauscht. Bei der Beispielanwendung WordCount sieht die URL wie folgt aus:

    http://sfcluster4doc.westus.cloudapp.azure.com:31000

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## Herstellen einer Remoteverbindung mit einer Instanz der VM-Skalierungsgruppe oder einem Clusterknoten

Für jeden Knotentyp, den Sie in Ihrem Cluster angeben, wird jeweils eine VM-Skalierungsgruppe eingerichtet. Ausführliche Informationen finden Sie unter [How to RDP into your VMSS instance (Herstellen einer Verbindung mit Ihrer VMSS-Instanz per RDP)](service-fabric-cluster-nodetypes.md).

## Nächste Schritte

Machen Sie sich nach dem Erstellen eines Clusters mit dem Schützen des Clusters und der Bereitstellung von Apps vertraut:
- [Verwalten von Service Fabric-Anwendungen in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
- [Service Fabric-Clustersicherheit](service-fabric-cluster-security.md)
- [Einführung in das Service Fabric-Integritätsmodell](service-fabric-health-introduction.md)


<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[Ports]: ./media/service-fabric-cluster-creation-via-portal/ports.png
[SFConfigurations]: ./media/service-fabric-cluster-creation-via-portal/SFConfigurations.png
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[BrowseCluster]: ./media/service-fabric-cluster-creation-via-portal/browse.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[SecureConnection]: ./media/service-fabric-cluster-creation-via-portal/SecureConnection.png

<!---HONumber=AcomDC_0330_2016-->