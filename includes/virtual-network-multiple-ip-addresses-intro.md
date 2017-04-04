> [!div class="op_single_selector"]
> * [Portal](../articles/virtual-network/virtual-network-multiple-ip-addresses-portal.md)
> * [PowerShell](../articles/virtual-network/virtual-network-multiple-ip-addresses-powershell.md)
> * [CLI 2.0](../articles/virtual-network/virtual-network-multiple-ip-addresses-cli.md)
> * [CLI 1.0](../articles/virtual-network/virtual-network-multiple-ip-addresses-cli-nodejs.md)
> * [Vorlage](../articles/virtual-network/virtual-network-multiple-ip-addresses-template.md)
>

Ein virtueller Azure-Computer (VM) verfügt über mindestens eine angefügte Netzwerkschnittstelle (NIC). Jede NIC kann eine oder mehrere zugewiesene statische und dynamische öffentliche oder private IP-Adressen besitzen. Durch das Zuweisen von mehreren IP-Adressen zu einem virtuellen Computer bestehen folgende Möglichkeiten:

* Hosten mehrerer Websites oder Dienste mit unterschiedlichen IP-Adressen und SSL-Zertifikaten auf einem einzelnen Server
* Übernehmen der Rolle als virtuelles Netzwerkgerät, z.B. als Firewall oder Load Balancer
* Fähigkeit zum Hinzufügen einer privaten IP-Adresse für eine der NICs zu einem Azure Load Balancer-Back-End-Pool hinzufügen. Bisher konnte nur die primäre IP-Adresse für die primäre NIC einem Back-End-Pool hinzugefügt werden. Weitere Informationen dazu, wie in Konfigurationen mit mehreren IPs ein Lastenausgleich durchgeführt werden kann, finden Sie im Artikel [Load balancing multiple IP configurations](../articles/load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Lastenausgleich bei Konfigurationen mit mehreren IPs).

Jeder NIC, die an einen virtuellen Computer angefügt ist, ist mindestens eine IP-Konfiguration zugeordnet. Jeder Konfiguration wird eine statische oder dynamische private IP-Adresse zugewiesen. Jeder Konfiguration kann auch eine öffentliche IP-Adressressource zugeordnet sein. Einer öffentlichen IP-Adressressource ist entweder eine dynamische oder eine statische IP-Adresse zugewiesen. Weitere Informationen zu IP-Adressen in Azure finden Sie im Artikel [IP-Adressen in Azure](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md). 

Es gilt ein Grenzwert dafür, wie viele private IP-Adressen einer NIC zugewiesen werden können. Außerdem ist begrenzt, wie viele öffentliche IP-Adressen in einem Azure-Abonnement verwendet werden können. Weitere Informationen finden Sie im Artikel [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../articles/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
