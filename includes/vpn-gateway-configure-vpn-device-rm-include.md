Für Site-to-Site-Verbindungen mit einem lokalen Netzwerk ist ein VPN-Gerät erforderlich. Wir stellen zwar nicht für alle VPN-Geräte Konfigurationsschritte bereit, aber die folgenden Links enthalten hilfreiche Informationen:

- Informationen zu kompatiblen VPN-Geräten finden Sie unter [VPN-Geräte](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md). 
- Links zu Konfigurationseinstellungen für Geräte finden Sie unter [Überprüfte VPN-Geräte](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable). Die Links zur Gerätekonfiguration werden nach bestem Wissen bereitgestellt. Die beste Vorgehensweise besteht immer darin, die aktuellen Informationen zur Konfiguration bei Ihrem Gerätehersteller zu erfragen.
- Informationen zur Bearbeitung von Gerätekonfigurationsbeispielen finden Sie unter [Bearbeiten von Gerätekonfigurationsbeispielen](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#editing).
- Informationen zu IPsec/IKE-Parametern finden Sie unter [Parameter](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec).
- Führen Sie vor dem Konfigurieren Ihres VPN-Geräts eine Überprüfung auf [bekannte Probleme mit der Gerätekompatibilität](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#known) für das VPN-Gerät durch, das Sie verwenden möchten.

Beim Konfigurieren des VPN-Geräts benötigen Sie Folgendes:

- Die öffentliche IP-Adresse Ihres Gateways für virtuelle Netzwerke.

    -  Sie können die öffentliche IP-Adresse mit dem Azure-Portal ermitteln, indem Sie zu **Gateways für virtuelle Netzwerke** navigieren und auf den Namen Ihres Gateways klicken. 
    - Verwenden Sie zum Ermitteln der öffentlichen IP-Adresse Ihres Gateways für virtuelle Netzwerke mit PowerShell das folgende Beispiel, und ersetzen Sie die Werte durch Ihre eigenen Werte.

            Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG
- Einen gemeinsam verwendeten Schlüssel. Dies ist derselbe gemeinsame Schlüssel, den Sie beim Erstellen Ihrer Site-to-Site-VPN-Verbindung angeben. In unseren Beispielen verwenden wir einen sehr einfachen gemeinsamen Schlüssel. Es ist ratsam, einen komplexeren Schlüssel zu generieren.




