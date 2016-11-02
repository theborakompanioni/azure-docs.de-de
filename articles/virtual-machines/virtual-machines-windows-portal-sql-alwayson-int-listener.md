<properties
   pageTitle="Erstellen eines Listeners für eine AlwaysOn-Verfügbarkeitsgruppe für SQL Server auf virtuellen Azure-Computern"
   description="Eine schrittweise Anleitung zum Erstellen eines Listeners für eine AlwaysOn-Verfügbarkeitsgruppe für SQL Server auf virtuellen Azure-Computern."
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="07/12/2016"
   ms.author="MikeRayMSFT"/>

# Konfigurieren eines internen Load Balancers für eine AlwaysOn-Verfügbarkeitsgruppe in Azure

In diesem Thema erfahren Sie, wie Sie einen internen Load Balancer für eine SQL Server-AlwaysOn-Verfügbarkeitsgruppe auf virtuellen Azure-Computern erstellen, die unter dem Resource Manager-Modell ausgeführt werden. Eine AlwaysOn-Verfügbarkeitsgruppe benötigt einen Load Balancer, wenn sich die SQL Server-Instanzen auf virtuellen Azure-Computern befinden. Der Load-Balancer speichert die IP-Adresse für den Verfügbarkeitsgruppenlistener. Wenn sich eine Verfügbarkeitsgruppe über mehrere Regionen erstreckt, benötigt jede Region einen Load Balancer.

Für diese Aufgabe benötigen Sie eine SQL Server-AlwaysOn-Verfügbarkeitsgruppe, die auf virtuellen Azure-Computern unter dem Resource Manager-Modell bereitgestellt wird. Beide virtuellen SQL Server-Computer müssen der gleichen Verfügbarkeitsgruppe angehören. Mithilfe der [Microsoft-Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) können Sie die AlwaysOn-Verfügbarkeitsgruppe in Azure Resource Manager automatisch erstellen. Diese Vorlage nimmt Ihnen die Erstellung des internen Load Balancers ab.

Alternativ können Sie aber auch [eine AlwaysOn-Verfügbarkeitsgruppe manuell konfigurieren](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Dieses Thema setzt voraus, dass Ihre Verfügbarkeitsgruppen bereits konfiguriert sind.

Verwandte Themen:

 - [Konfigurieren von AlwaysOn-Verfügbarkeitsgruppen in einem virtuellen Azure-Computer (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
 
 - [Konfigurieren einer VNet-zu-VNet-Verbindung mit Azure Resource Manager und PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## Schritte

Im Rahmen dieses Dokuments wird mithilfe des Azure-Portals Schritt für Schritt ein Load Balancer erstellt und konfiguriert. Anschließend wird der Cluster so konfiguriert, dass er die IP-Adresse aus dem Load Balancer für den AlwaysOn-Verfügbarkeitsgruppenlistener verwendet.

## Erstellen und Konfigurieren des Load Balancers im Azure-Portal

In diesem Teil führen Sie die folgenden Schritte im Azure-Portal aus:

1. Erstellen des Load Balancers und Konfigurieren der IP-Adresse

1. Konfigurieren des Back-End-Pools

1. Erstellen des Tests

1. Festlegen der Lastenausgleichsregeln

>[AZURE.NOTE] Falls sich die SQL Server in unterschiedlichen Ressourcengruppen und Regionen befinden, müssen alle diese Schritte zweimal (je einmal pro Ressourcengruppe) ausgeführt werden.

## 1\. Erstellen des Load Balancers und Konfigurieren der IP-Adresse

Im ersten Schritt wird zunächst der Load Balancer erstellt. Öffnen Sie im Azure-Portal die Ressourcengruppe mit den virtuellen SQL Server-Computern. Klicken Sie in der Ressourcengruppe auf **Hinzufügen**.

- Suchen Sie nach **Load Balancer**. Wählen Sie in den Suchergebnissen den (von **Microsoft** veröffentlichten) **Load Balancer** aus.

- Klicken Sie auf dem Blatt **Load Balancer** auf **Erstellen**.

- Konfigurieren Sie den Load Balancer unter **Load Balancer erstellen** wie folgt:

| Einstellung | Wert |
| ----- | ----- |
| **Name** | Namenstext für den Load Balancer. Beispiel: **sqlLB**. |
| **Schema** | **Intern** |
| **Virtuelles Netzwerk** | Wählen Sie das virtuelle Netzwerk aus, in dem sich die SQL Server befinden. |
| **Subnetz** | Wählen Sie das Subnetz aus, in dem sich die SQL Server befinden. |
| **Abonnement** | Dieses Feld wird unter Umständen angezeigt, wenn Sie über mehrere Abonnements verfügen. Wählen Sie das Abonnement aus, mit dem diese Ressource verknüpft werden soll. Hierbei handelt es sich üblicherweise um das gleiche Abonnement wie bei allen anderen Ressourcen für die Verfügbarkeitsgruppe. |
| **Ressourcengruppe** | Wählen Sie die Ressourcengruppe aus, in der sich die SQL Server befinden. | 
| **Standort** | Wählen Sie den Azure-Standort aus, an dem sich die SQL Server befinden. |

- Klicken Sie auf **Erstellen**.

Azure erstellt den Load Balancer, den Sie anhand der obigen Angaben konfiguriert haben. Der Load Balancer gehört zu einem bestimmten Netzwerk, Subnetz, einer bestimmten Ressourcengruppe und einem bestimmten Standort. Überprüfen Sie nach Abschluss des Vorgangs die Einstellungen für den Load Balancer in Azure.

Konfigurieren Sie nun die IP-Adresse des Load Balancers.

- Klicken Sie auf dem Blatt **Einstellungen** des Load Balancers auf **IP-Adresse**. Auf dem Blatt **IP-Adresse** sehen Sie, dass es sich um einen privaten Load Balancer handelt, der sich im gleichen virtuellen Netzwerk befindet wie Ihre SQL Server.

- Legen Sie folgende Einstellungen fest:

| Einstellung | Wert |
| ----- | ----- |
| **Subnetz** | Wählen Sie das Subnetz aus, in dem sich die SQL Server befinden. |
| **Zuweisung** | **Statisch** |
| **IP-Adresse** | Geben Sie eine nicht verwendete virtuelle IP-Adresse aus dem Subnetz ein. |

- Speichern Sie die Einstellungen.

Der Load Balancer besitzt nun eine IP-Adresse. Notieren Sie sich diese IP-Adresse. Sie wird bei der Listener-Erstellung für den Cluster benötigt. Die Adresse wird später in einem PowerShell-Skript für die `$ILBIP`-Variable verwendet.



## 2\. Konfigurieren des Back-End-Pools

Als Nächstes muss ein Back-End-Adresspool erstellt werden. Im Kontext von Azure wird der Back-End-Adresspool als *Back-End-Pool* bezeichnet. Im vorliegenden Fall enthält der Back-End-Pool die Adressen der beiden SQL Server in Ihrer Verfügbarkeitsgruppe.

- Klicken Sie in Ihrer Ressourcengruppe auf den zuvor erstellten Load Balancer.

- Klicken Sie unter **Einstellungen** auf **Back-End-Pools**.

- Klicken Sie im Bereich **Back-End-Adresspools** auf **Hinzufügen**, um einen Back-End-Adresspool zu erstellen.

- Geben Sie im Bereich **Back-End-Pool hinzufügen** unter **Name** einen Namen für den Back-End-Pool ein.

- Klicken Sie unter **Virtuelle Computer** auf **+ Virtuellen Computer hinzufügen**.

- Klicken Sie unter **Choose virtual machines** (Virtuelle Computer auswählen) auf **Choose an availability set** (Verfügbarkeitsgruppe auswählen), und geben Sie die Verfügbarkeitsgruppe an, zu der die virtuellen SQL Server-Computer gehören.

- Klicken Sie nach dem Auswählen der Verfügbarkeitsgruppe auf **Choose the virtual machines** (Virtuelle Computer auswählen). Klicken Sie auf die beiden virtuellen Computer, die die SQL Server-Instanzen in der Verfügbarkeitsgruppe hosten. Klicken Sie auf **Auswählen**.

- Klicken Sie auf **OK**, um die Blätter zum **Auswählen der virtuellen Computer** bzw. zum **Hinzufügen des Back-End-Pools** zu schließen.

Azure aktualisiert die Einstellungen für den Back-End-Adresspool. Die Verfügbarkeitsgruppe verfügt nun über einen Pool mit zwei SQL Server-Instanzen.

## 3\. Erstellen eines Tests

Im nächsten Schritt wird ein Test erstellt. Mit dem Test wird definiert, wie Azure überprüft, welcher der SQL Server gerade für den Verfügbarkeitsgruppenlistener zuständig ist. Azure testet den Dienst auf der Grundlage der IP-Adresse an einem Port, den Sie beim Erstellen des Tests definieren.

- Klicken Sie auf dem Blatt **Einstellungen** des Load Balancers auf **Tests**.

- Klicken Sie auf dem Blatt **Tests** auf **Hinzufügen**.

- Konfigurieren Sie den Test auf dem Blatt **Test hinzufügen**. Verwenden Sie dabei die folgenden Werte:

| Einstellung | Wert |
| ----- | ----- |
| **Name** | Namenstext für den Test. Beispiel: **SQLAlwaysOnEndPointProbe**. |
| **Protokoll** | **TCP** |
| **Port** | Ein beliebiger verfügbarer Port. Beispiel: *59999*. |
| **Intervall** | *5* | 
| **Fehlerhafter Schwellenwert** | *2* | 

- Klicken Sie auf **OK**.

>[AZURE.NOTE] Vergewissern Sie sich, dass der angegebene Port in der Firewall beider SQL Server geöffnet ist. Beide Server benötigen eine eingehende Regel für den TCP-Port, den Sie verwenden möchten. Weitere Informationen finden Sie unter [Hinzufügen oder Bearbeiten einer Firewallregel](http://technet.microsoft.com/library/cc753558.aspx).

Azure erstellt den Test. Mithilfe des Tests wird ermittelt, welcher SQL Server über den Listener für die Verfügbarkeitsgruppe verfügt.

## 4\. Festlegen der Lastenausgleichsregeln

Legen Sie die Lastenausgleichsregeln fest. Mit den Lastenausgleichsregeln wird konfiguriert, wir der Load Balancer Datenverkehr an die SQL Server weiterleitet. Für diesen Load Balancer wird „Direct Server Return“ aktiviert, da immer nur einer der beiden SQL Server für die Verfügbarkeitsgruppenlistener-Ressource zuständig ist.

- Klicken Sie auf dem Blatt **Einstellungen** des Load Balancers auf **Lastenausgleichsregeln**.

- Klicken Sie auf dem Blatt **Lastenausgleichsregeln** auf **Hinzufügen**.

- Konfigurieren Sie auf dem Blatt **Lastenausgleichsregeln hinzufügen** die Lastenausgleichsregel. Verwenden Sie folgende Einstellungen:

| Einstellung | Wert |
| ----- | ----- |
| **Name** | Namenstext für die Lastenausgleichsregeln. Beispiel: **SQLAlwaysOnEndPointListener**. |
| **Protokoll** | **TCP** |
| **Port** | *1433* |
| **Back-End-Port** | *1433*. Wird deaktiviert, da diese Regel **Floating IP (Direct Server Return)** verwendet. |
| **Test** | Verwenden Sie den Namen des Tests, den Sie für diesen Load Balancer erstellt haben. |
| **Session persistence (Sitzungspersistenz)** | **Keine** | 
| **Leerlaufzeitüberschreitung (Minuten)** | *4* | 
| **Floating IP (Direct Server Return)** | **Aktiviert** | 

 >[AZURE.NOTE] Möglicherweise müssen Sie auf dem Blatt einen Bildlauf nach unten durchführen, um alle Einstellungen anzuzeigen.

- Klicken Sie auf **OK**.

- Azure konfiguriert die Lastenausgleichsregel. Damit ist die Lastenausgleichsregel konfiguriert, die Datenverkehr an den SQL Server weiterleitet, der den Listener für die Verfügbarkeitsgruppe hostet.

Die Ressourcengruppe verfügt nun also über einen mit beiden SQL Server-Computern verbundenen Load Balancer. Der Load Balancer verfügt zudem über eine IP-Adresse für den SQL Server-AlwaysOn-Verfügbarkeitsgruppenlistener, sodass beide Computer auf Anforderungen für die Verfügbarkeitsgruppen reagieren können.

>[AZURE.NOTE] Falls sich Ihre SQL Server in unterschiedlichen Regionen befinden, wiederholen Sie die Schritte in der anderen Region. Jede Region benötigt einen Load Balancer.

## Konfigurieren des Clusters, von dem die IP-Adresse des Load Balancers verwendet werden soll 

Als Nächstes wird der Listener für den Cluster konfiguriert und online geschaltet. Führen Sie hierzu die folgenden Schritte aus:

1. Erstellen des Verfügbarkeitsgruppenlisteners für den Failovercluster

1. Onlineschalten des Listeners

## 1\. Erstellen des Verfügbarkeitsgruppenlisteners für den Failovercluster

In diesem Schritt erstellen Sie manuell den Verfügbarkeitsgruppenlistener im Failovercluster-Manager und in SQL Server Management Studio (SSMS).

- Stellen Sie eine RDP-Verbindung mit dem virtuellen Azure-Computer her, der das primäre Replikat hostet.

- Öffnen Sie den Failovercluster-Manager.

- Wählen Sie den Knoten **Netzwerke** aus, und notieren Sie den Netzwerknamen des Clusters. Dieser Name wird im PowerShell-Skript in der `$ClusterNetworkName`-Variablen verwendet.

- Erweitern Sie den Clusternamen, und klicken Sie dann auf **Rollen**.

- Klicken Sie im Bereich **Rollen** mit der rechten Maustaste auf den Verfügbarkeitsgruppennamen, und wählen Sie dann **Ressource hinzufügen** > **Clientzugriffspunkt** aus.

- Erstellen Sie im Feld **Name** einen Namen für diesen neuen Listener, klicken Sie dann zweimal auf **Weiter**, und klicken Sie anschließend auf **Fertig stellen**. Schalten Sie den Listener oder die Ressource jetzt noch nicht online.

 >[AZURE.NOTE] Der Name für den neuen Listener ist der Netzwerkname, den Anwendungen beim Herstellen einer Verbindung mit Datenbanken in der SQL Server-Verfügbarkeitsgruppe verwenden.

- Klicken Sie auf die Registerkarte **Ressourcen**, und erweitern Sie dann den Clientzugriffspunkt, den Sie gerade erstellt haben. Klicken Sie mit der rechten Maustaste auf die IP-Ressource, und klicken Sie auf „Eigenschaften“. Notieren Sie sich den Namen der IP-Adresse. Dieser Name wird im PowerShell-Skript in der `$IPResourceName`-Variablen verwendet.

- Klicken Sie unter **IP-Adresse** auf **Statische IP-Adresse**, und legen Sie die statische IP-Adresse auf die Adresse fest, die Sie auch beim Festlegen der Load Balancer-IP-Adresse im Azure-Portal verwendet haben. Aktivieren Sie NetBIOS für diese Adresse, und klicken Sie auf **OK**. Wiederholen Sie diesen Schritt für jede IP-Ressource, wenn die Lösung mehrere Azure-VNets umfasst.

- Öffnen Sie auf dem Clusterknoten, der gerade das primäre Replikat hostet, eine PowerShell-ISE, und fügen Sie die folgenden Befehle in ein neues Skript ein:

        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    
        Import-Module FailoverClusters
    
        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

- Aktualisieren Sie die Variablen, und führen Sie das PowerShell-Skript aus, um die IP-Adresse und den Port für den neuen Listener zu konfigurieren.

 >[AZURE.NOTE] Falls sich Ihre SQL Server in unterschiedlichen Regionen befinden, muss das PowerShell-Skript zweimal ausgeführt werden. Verwenden Sie beim ersten Mal den Clusternetzwerknamen, den Cluster-IP-Ressourcennamen und die Load Balancer-IP-Adresse aus der ersten Ressourcengruppe. Verwenden Sie beim zweiten Mal den Clusternetzwerknamen, den Cluster-IP-Ressourcennamen und die Load Balancer-IP-Adresse aus der zweiten Ressourcengruppe.

Der Cluster verfügt nun über eine Verfügbarkeitsgruppenlistener-Ressource.

## 2\. Onlineschalten des Listeners

Nach dem Konfigurieren der Verfügbarkeitsgruppenlistener-Ressource können Sie den Listener online schalten, damit Clientanwendungen eine Verbindung mit Datenbanken in der Verfügbarkeitsgruppe mit dem Listener herstellen können.

- Wechseln Sie wieder zum Failovercluster-Manager. Erweitern Sie **Rollen**, und markieren Sie dann die Verfügbarkeitsgruppe. Klicken Sie auf der Registerkarte **Ressourcen** mit der rechten Maustaste auf den Listenernamen, und klicken Sie anschließend auf **Eigenschaften**.

- Klicken Sie auf die Registerkarte **Abhängigkeiten**. Wenn mehrere Ressourcen aufgeführt sind, überprüfen Sie, ob die IP-Adressen OR-Abhängigkeiten aufweisen (statt AND-Abhängigkeiten). Klicken Sie auf **OK**.

- Klicken Sie mit der rechten Maustaste auf den Listenernamen, und klicken Sie auf **Online schalten**.


- Sobald der Listener online ist, klicken Sie auf der Registerkarte **Ressourcen** mit der rechten Maustaste auf die Verfügbarkeitsgruppe, und klicken Sie auf **Eigenschaften**.

- Erstellen Sie eine Abhängigkeit für die Ressource des Listenernamens (nicht den Ressourcennamen der IP-Adresse). Klicken Sie auf **OK**.


- Starten Sie SQL Server Management Studio, und stellen Sie eine Verbindung mit dem primären Replikat her.


- Navigieren Sie zu **Hohe Verfügbarkeit mit AlwaysOn** > **Verfügbarkeitsgruppen** > **Verfügbarkeitsgruppenlistener**.


- Jetzt sollte der Listenername angezeigt werden, den Sie im Failovercluster-Manager erstellt haben. Klicken Sie mit der rechten Maustaste auf den Listenernamen, und klicken Sie auf **Eigenschaften**.


- Geben Sie im Feld **Port** die Portnummer für den Verfügbarkeitsgruppenlistener an. Verwenden Sie dabei den zuvor verwendeten Wert für „$EndpointPort“ (Standardwert: 1433). Klicken Sie anschließend auf **OK**.

Sie verfügen nun über eine SQL Server-AlwaysOn-Verfügbarkeitsgruppe auf virtuellen Azure-Computern im Azure Resource Manager-Modus.

## Testen der Verbindung mit dem Listener

Gehen Sie wie folgt vor, um die Verbindung zu testen:

1. Stellen Sie eine RDP-Verbindung mit einem SQL Server her, der sich im gleichen virtuellen Netzwerk befindet, aber nicht für das Replikat zuständig ist. Hierbei kann es sich um den anderen SQL Server im Cluster handeln.

1. Testen Sie die Verbindung mithilfe des Hilfsprogramms **sqlcmd**. Das folgende Skript stellt beispielsweise über den Listener eine **sqlcmd**-Verbindung mit Windows-Authentifizierung mit dem primären Replikat her:

        sqlcmd -S <listenerName> -E

Die sqlcmd-Verbindung wird automatisch mit der SQL Server-Instanz hergestellt, die das primäre Replikat hostet.

## Richtlinien und Einschränkungen

Für Verfügbarkeitsgruppenlistener in Azure mit internem Load Balancer gelten folgenden Richtlinien:

- Da der Listener für den Load Balancer konfiguriert wird und nur ein interner Load Balancer vorhanden ist, wird pro Clouddienst maximal ein Verfügbarkeitsgruppenlistener unterstützt. Es können jedoch mehrere externe Listener erstellt werden.

- Bei Verwendung eines internen Load Balancers erfolgt der Zugriff auf den Listener nur innerhalb des gleichen virtuellen Netzwerks.
 

<!---HONumber=AcomDC_0720_2016-->