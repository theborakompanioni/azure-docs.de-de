## <a name="a-namex-subapeering-across-subscriptions"></a><a name="x-sub"></a>Abonnementübergreifendes Peering
In diesem Szenario erstellen Sie ein Peering zwischen zwei VNets in unterschiedlichen Abonnements.

![Abonnementübergreifendes Szenario](./media/virtual-networks-create-vnetpeering-scenario-crosssub-include/figure01.PNG)

Beim VNET-Peering wird die rollenbasierte Zugriffssteuerung (RBAC) für die Autorisierung genutzt. Für abonnementübergreifende Szenarien müssen Sie zuerst den Benutzern, die den Peeringlink erstellen sollen, ausreichende Berechtigungen gewähren.

> [!NOTE]
> Falls ein Benutzer über die Berechtigung für beide Abonnements verfügt, können Sie die nachfolgenden Schritte 1 bis 4 überspringen.
> 
> 



<!--HONumber=Feb17_HO1-->


