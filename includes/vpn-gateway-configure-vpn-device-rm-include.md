Für Site-to-Site-Verbindungen mit einem lokalen Netzwerk ist ein VPN-Gerät erforderlich. Es gibt viele verschiedene VPN-Geräte, die mit Azure verwendet werden können. Weitere Informationen zu VPN-Geräten und Konfigurationseinstellungen finden Sie unter [VPN-Geräte](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md). Führen Sie vor dem Konfigurieren Ihres VPN-Geräts eine Überprüfung auf [bekannte Probleme mit der Gerätekompatibilität](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#known) für das VPN-Gerät durch, das Sie verwenden möchten. Genaue Informationen zur Konfiguration des VPN-Geräts erhalten Sie beim Gerätehersteller.

Beim Konfigurieren des VPN-Geräts benötigen Sie Folgendes:

- Die **öffentliche IP-Adresse** Ihres Gateways für virtuelle Netzwerke.

    -  Sie können die öffentliche IP-Adresse mit dem Azure-Portal ermitteln, indem Sie zu **Gateways für virtuelle Netzwerke** navigieren und auf den Namen Ihres Gateways klicken. 

    - Verwenden Sie zum Ermitteln der öffentlichen IP-Adresse Ihres Gateways für virtuelle Netzwerke mit PowerShell das folgende Beispiel, und ersetzen Sie die Werte durch Ihre eigenen Werte.

            Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG
- Einen **gemeinsam verwendeten Schlüssel**. Dies ist derselbe gemeinsame Schlüssel, den Sie beim Erstellen Ihrer Site-to-Site-VPN-Verbindung angeben. In unseren Beispielen verwenden wir einen sehr einfachen gemeinsamen Schlüssel. Es ist ratsam, einen komplexeren Schlüssel zu generieren.




