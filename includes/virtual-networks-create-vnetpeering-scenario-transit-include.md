## Dienstverkettung – Transit über verknüpftes VNet

Obwohl durch die Verwendung von Systemrouten der Datenverkehr für Ihre Bereitstellung automatisch ermöglicht wird, gibt es Fälle, in denen Sie das Routing von Paketen über ein virtuelles Gerät steuern möchten. Bei diesem Szenario sind in einem Abonnement zwei VNets enthalten, und zwar HubVNet und VNet1. Dies ist unten im Diagramm beschrieben. Sie stellen das virtuelle Netzwerkgerät (Network Virtual Appliance, NVA) im VNet „HubVNet“ bereit. Nach dem Einrichten des VNet-Peerings zwischen HubVNet und VNet1 können Sie benutzerdefinierte Routen einrichten und den nächsten Hop zum virtuellen Netzwerkgerät in HubVNet angeben.

![Transitvorgang für das virtuelle Netzwerkgerät](./media/virtual-networks-create-vnetpeering-scenario-transit-include/figure01.PNG)

> [AZURE.NOTE] Der Einfachheit halber wird hier angenommen, dass sich alle VNets in demselben Abonnement befinden. Der Ansatz funktioniert aber auch für das abonnementübergreifende Szenario.

Die wichtigste Eigenschaft für das Transitrouting ist der Parameter „Allow Forwarded Traffic“ (Weitergeleiteten Datenverkehr zulassen). Er ermöglicht das Akzeptieren und Senden von Datenverkehr vom virtuellen und an das virtuelle Netzwerkgerät im verknüpften VNet.

<!---HONumber=AcomDC_0928_2016-->