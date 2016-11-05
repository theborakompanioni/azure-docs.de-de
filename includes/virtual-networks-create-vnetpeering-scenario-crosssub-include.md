## Abonnementübergreifendes Peering
In diesem Szenario erstellen Sie ein Peering zwischen zwei VNets, die zu unterschiedlichen Abonnements gehören.

![Abonnementübergreifendes Szenario](./media/virtual-networks-create-vnetpeering-scenario-crosssub-include/figure01.PNG)

Beim VNet-Peering wird die rollenbasierte Zugriffssteuerung (RBAC) für die Autorisierung genutzt. Für abonnementübergreifende Szenarien müssen Sie zuerst den Benutzern, die den Peeringlink erstellen sollen, ausreichende Berechtigungen gewähren:

> [!NOTE]
> Wenn derselbe Benutzer über die Berechtigung für beide Abonnements verfügt, können Sie unten die Schritte 1 bis 4 überspringen.
> 
> 

<!---HONumber=AcomDC_0921_2016-->