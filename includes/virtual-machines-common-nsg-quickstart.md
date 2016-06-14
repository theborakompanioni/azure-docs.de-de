In Azure öffnen Sie einen Port oder erstellen einen Endpunkt, indem Sie einen Netzwerkfilter erstellen, mit dem Datenverkehr an den von Ihnen ausgewählten Port in einem Subnetz oder die Netzwerkschnittstelle auf einem virtuellen Computer (VM) geleitet werden kann. Diese Filter, mit denen sowohl eingehender als auch ausgehender Datenverkehr gesteuert werden kann, sind in einer Netzwerksicherheitsgruppe angeordnet und an die Ressource angefügt, die den Datenverkehr empfängt.

Wir verwenden ein gängiges Beispiel für Webdatenverkehr über Port 80. Nachdem Sie über eine VM verfügen, die für die Bereitstellung von Webanforderungen über den standardmäßigen TCP-Port 80 konfiguriert ist (denken Sie auch an das Starten der richtigen Dienste und das Öffnen der erforderlichen Betriebssystem-Firewallregeln auf der VM), führen Sie die folgenden Schritte aus:

1. Erstellen einer Netzwerksicherheitsgruppe
2. Erstellen einer Datenverkehr-Eingangsregel mit diesen Angaben:
  - Zielportbereich „80“
  - Quellportbereich „*“ (beliebiger Quellport)
  - Prioritätswert von weniger als 65.500 (um eine höhere Priorität als mit der standardmäßigen Catch-All-Verweigerungsregel für eingehenden Datenverkehr zu erzielen)
3. Zuordnen der Netzwerksicherheitsgruppe zur VM-Netzwerkschnittstelle oder zum Subnetz
    
Sie können sehr komplexe Netzwerkkonfigurationen erstellen, um Ihre Umgebung mit Netzwerksicherheitsgruppen und Regeln zu schützen, anstatt nur mit ein oder zwei Regeln, die HTTP-Datenverkehr oder die Remoteverwaltung ermöglichen. Weitere Informationen finden Sie unten im Abschnitt [Weitere Informationen](#more-information-on-network-security-groups) oder unter [Was ist eine Netzwerksicherheitsgruppe?](../articles/virtual-network/virtual-networks-nsg.md).

<!---HONumber=AcomDC_0601_2016-->