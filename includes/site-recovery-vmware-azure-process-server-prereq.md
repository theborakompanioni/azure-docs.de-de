In diesem Artikel wird Folgendes vorausgesetzt:

1. Sie haben zwischen Ihrem lokalen Netzwerk und Azure Virtual Network bereits eine **Site-to-Site-VPN**- oder **ExpressRoute**-Verbindung eingerichtet.
2. Ihr Benutzerkonto verfügt über die erforderlichen Berechtigungen, um einen neuen virtuellen Computer in dem Azure-Abonnement zu erstellen, auf das das Failover der virtuellen Computer erfolgt ist.
3. Ihr Abonnement hat über mindestens vier verfügbare Kerne, um einen neuen virtuellen Prozessservercomputer zu starten.
4. Sie haben die **Passphrase des Konfigurationsservers** zur Hand.

> [!TIP]
> Vergewissern Sie sich, dass Sie über die Azure Virtual Network-Instanz, an die das Failover der virtuellen Computer erfolgt ist, eine Verbindung mit Port 443 des (lokal ausgeführten) Konfigurationsservers herstellen können.
