In der folgenden Tabelle sind die Anforderungen für PolicyBased- und RouteBased-VPN-Gateways aufgeführt. Diese Tabelle betrifft sowohl das Resource Manager-Bereitstellungsmodell als auch das klassische Bereitstellungsmodell. Im klassischen Modell sind PolicyBased-VPN-Gateways das Gleiche wie statische Gateways, und RouteBased-Gateways entsprechen dynamischen Gateways.


| | **PolicyBased-VPN-Gateway, Basic** | **RouteBased-VPN-Gateway, Basic** | **RouteBased-VPN-Gateway, Standard** | **RouteBased-VPN-Gateway, High Performance** |
|---|---------------------------------------|---------------------------------------|----------------------------|----------------------------------|
| **Site-to-Site-Konnektivität (S2S)** | PolicyBased-VPN-Konfiguration | RouteBased-VPN-Konfiguration | RouteBased-VPN-Konfiguration | RouteBased-VPN-Konfiguration |
| **Punkt-zu-Standort-Konnektivität (P2S)** | Nicht unterstützt | Unterstützt (gemeinsame Verwendung mit S2S möglich) | Unterstützt (gemeinsame Verwendung mit S2S möglich) | Unterstützt (gemeinsame Verwendung mit S2S möglich) |
| **Authentifizierungsmethode** | Vorinstallierter Schlüssel | Vorinstallierter Schlüssel für S2S-Konnektivität, Zertifikate für P2S-Konnektivität | Vorinstallierter Schlüssel für S2S-Konnektivität, Zertifikate für P2S-Konnektivität | Vorinstallierter Schlüssel für S2S-Konnektivität, Zertifikate für P2S-Konnektivität |
| **Maximale Anzahl von S2S-Verbindungen** | 1 | 10 | 10 | 30 |
| **Maximale Anzahl von P2S-Verbindungen** | Nicht unterstützt | 128 | 128 | 128 |
|**Aktive Routingunterstützung (BGP)** | Nicht unterstützt | Nicht unterstützt | Unterstützt | Unterstützt |
 

<!---HONumber=AcomDC_0921_2016-->