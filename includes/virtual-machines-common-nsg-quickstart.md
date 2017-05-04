In Azure öffnen Sie einen Port oder erstellen einen Endpunkt für einen virtuellen Computer (VM), indem Sie einen Netzwerkfilter in einem Subnetz oder einer VM-Netzwerkschnittstelle erstellen. Sie platzieren diese Filter, mit denen sowohl eingehender als auch ausgehender Datenverkehr gesteuert werden kann, in einer Netzwerksicherheitsgruppe, die an die Ressource angefügt ist, die den Datenverkehr empfängt.

Wir verwenden ein gängiges Beispiel für Webdatenverkehr über Port 80. Sobald Sie über eine VM verfügen, die für die Bereitstellung von Webanforderungen über den standardmäßigen TCP-Port 80 konfiguriert ist (denken Sie auch an das Starten der richtigen Dienste und das Öffnen der erforderlichen Betriebssystem-Firewallregeln auf der VM), führen Sie die folgenden Schritte aus:

1. Erstellen einer Netzwerksicherheitsgruppe
2. Erstellen einer Datenverkehr-Eingangsregel mit diesen Angaben:
   * Zielportbereich „80“
   * Quellportbereich „*“ (beliebiger Quellport)
   * Prioritätswert von weniger als 65.500 (um eine höhere Priorität als mit der standardmäßigen Catch-All-Verweigerungsregel für eingehenden Datenverkehr zu erzielen)
3. Zuordnen der Netzwerksicherheitsgruppe zur VM-Netzwerkschnittstelle oder zum Subnetz

Sie können komplexe Netzwerkkonfigurationen zur Sicherung Ihrer Umgebung mithilfe von Netzwerksicherheitsgruppen und Regeln erstellen. In unserem Beispiel werden nur ein oder zwei Regeln verwendet, die HTTP-Datenverkehr oder Remoteverwaltung zulassen. Weitere Informationen finden Sie im folgenden Abschnitt [Weitere Informationen](#more-information-on-network-security-groups) oder unter [Was ist eine Netzwerksicherheitsgruppe?](../articles/virtual-network/virtual-networks-nsg.md)

