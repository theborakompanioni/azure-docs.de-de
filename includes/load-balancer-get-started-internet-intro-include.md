Ein Azure Load Balancer ist ein Layer-4-Load Balancer (TCP, UDP). Der Load Balancer sorgt für hohe Verfügbarkeit, indem er eingehenden Datenverkehr zwischen funktionierenden Dienstinstanzen in Clouddiensten oder auf virtuelle Computer verteilt, die in einer Gruppe für den Lastenausgleich definiert wurden. Der Azure Load Balancer kann diese Dienste auch auf mehreren Ports, mehreren IP-Adressen oder beidem leisten.

Hierfür können Sie einen Load Balancer konfigurieren:

* einen Lastenausgleich des auf den virtuellen Computern (VMs) eingehenden Internetdatenverkehrs herzustellen. Wir bezeichnen einen Load Balancer in diesem Szenario als einen [Lastenausgleich mit Internetzugriff](../articles/load-balancer/load-balancer-internet-overview.md).
* Lastenausgleich für Datenverkehr zwischen VMs in einem virtuellen Netzwerk (VNet), zwischen virtuellen Computern in Clouddiensten oder zwischen lokalen Computern und VMs in einem standortübergreifenden virtuellen Netzwerk. Wir bezeichnen einwn Load Balancer in diesem Szenario als einen [internen Load Balancer (ILB)](../articles/load-balancer/load-balancer-internal-overview.md).
* Weiterleiten von externem Datenverkehr an eine bestimmte VM-Instanz.
