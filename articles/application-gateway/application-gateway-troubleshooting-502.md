<properties
   pageTitle="Behandeln von Fehlern aufgrund eines ungültigen Gateways (502) in Application Gateway | Microsoft Azure"
   description="Hier erfahren Sie, wie Sie Application Gateway-Fehler vom Typ 502 behandeln."
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/02/2016"
   ms.author="amitsriva" />

# Behandeln von Fehlern aufgrund eines ungültigen Gateways in Application Gateway

## Übersicht

Nach dem Konfigurieren einer Azure Application Gateway-Instanz tritt bei Benutzern unter Umständen folgender Fehler auf: „Serverfehler: 502 - Webserver hat als Gateway oder Proxyserver eine ungültige Antwort erhalten.“ Dies kann folgende Hauptursachen haben:

- Der Back-End-Pool von Azure Application Gateway ist nicht konfiguriert oder leer.
- Die virtuellen Computer oder Instanzen in der VM-Skalierungsgruppe befinden sich nicht in einem fehlerfreien Zustand.
- Virtuelle Back-End-Computer oder Instanzen der VM-Skalierungsgruppe reagieren nicht auf die standardmäßige Integritätsüberprüfung.
- Benutzerdefinierte Integritätsüberprüfungen sind ungültig oder nicht korrekt konfiguriert.
- Bei der Anforderung tritt ein Timeout auf, oder es liegen Verbindungsprobleme bei Benutzeranforderungen vor.

## Leerer Back-End-Adresspool

### Ursache

Falls im Back-End-Adresspool für Application Gateway keine virtuellen Computer konfiguriert sind oder keine VM-Skalierungsgruppe konfiguriert ist, können Kundenanforderungen nicht weitergeleitet werden, und es tritt ein Fehler mit dem Hinweis auf ein ungültiges Gateway auf.

### Lösung

Vergewissern Sie sich, dass der Back-End-Adresspool nicht leer ist. Hierzu können Sie entweder PowerShell, die Befehlszeilenschnittstelle oder das Portal verwenden.

	Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"

Die Ausgabe des vorherigen Cmdlets muss nicht leere Back-End-Adresspools enthalten. Im folgenden Beispiel werden zwei Pools zurückgegeben, die mit FQDN oder IP-Adressen für virtuelle Back-End-Computer konfiguriert sind. Der Bereitstellungszustand des Back-End-Adresspools muss „Succeeded“ lauten.
	
	BackendAddressPoolsText: 
			[{
				"BackendAddresses": [{
					"ipAddress": "10.0.0.10",
					"ipAddress": "10.0.0.11"
				}],
				"BackendIpConfigurations": [],
				"ProvisioningState": "Succeeded",
				"Name": "Pool01",
				"Etag": "W/"00000000-0000-0000-0000-000000000000"",
				"Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
			}, {
				"BackendAddresses": [{
					"Fqdn": "xyx.cloudapp.net",
					"Fqdn": "abc.cloudapp.net"
				}],
				"BackendIpConfigurations": [],
				"ProvisioningState": "Succeeded",
				"Name": "Pool02",
				"Etag": "W/"00000000-0000-0000-0000-000000000000"",
				"Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
			}]


## Fehlerhafte Instanzen im Back-End-Adresspool

### Ursache

Wenn alle Instanzen des Back-End-Adresspools fehlerhaft sind, steht Application Gateway kein Back-End zur Weiterleitung von Benutzeranforderungen zur Verfügung. Dies kann auch der Fall sein, wenn zwar fehlerfreie Back-End-Instanzen vorhanden sind, diese aber nicht über die erforderliche Anwendung verfügen.

### Lösung

Stellen Sie sicher, dass die Instanzen fehlerfrei sind und die Anwendung ordnungsgemäß konfiguriert ist. Prüfen Sie, ob die Back-End-Instanzen auf ein Ping von einem anderen virtuellen Computer im gleichen virtuellen Netzwerk reagieren. Vergewissern Sie sich bei einer Konfiguration mit öffentlichem Endpunkt, dass eine an die Webanwendung gerichtete Browseranforderung verarbeitet werden kann.

## Probleme mit der standardmäßigen Integritätsüberprüfung

### Ursache

502-Fehler sind auch oftmals darauf zurückzuführen, dass virtuelle Back-End-Computer für die standardmäßige Integritätsüberprüfung nicht erreichbar sind. Beim Bereitstellen einer Application Gateway-Instanz wird unter Verwendung von Eigenschaften der Back-End-HTTP-Einstellung automatisch für jeden Back-End-Adresspool eine standardmäßige Integritätsüberprüfung konfiguriert. Zum Festlegen dieser Überprüfung ist keinerlei Benutzereingabe erforderlich. Wenn eine Regel für den Lastenausgleich konfiguriert wird, erfolgt eine Zuordnung zwischen einer Back-End-HTTP-Einstellung und dem Back-End-Adresspool. Für diese Zuordnungen wird jeweils eine standardmäßige Integritätsüberprüfung konfiguriert, und Application Gateway stellt über den im BackendHttpSetting-Element angegebenen Port in regelmäßigen Abständen eine Verbindung mit den einzelnen Instanzen im Back-End-Adresspool her, um die Integrität zu überprüfen. Die folgende Tabelle enthält die Werte der standardmäßigen Integritätsüberprüfung:


|Überprüfungseigenschaft | Wert | Beschreibung|
|---|---|---|
| Überprüfungs-URL| http://127.0.0.1/ | URL-Pfad |
| Intervall | 30 | Überprüfungsintervall in Sekunden |
| Zeitüberschreitung | 30 | Zeitüberschreitung der Überprüfung in Sekunden |
| Fehlerhafter Schwellenwert | 3 | Anzahl der Wiederholungsversuche der Überprüfung Der Back-End-Server wird als außer Betrieb markiert, nachdem die Anzahl der aufeinanderfolgenden fehlgeschlagenen Überprüfungen den fehlerhaften Schwellenwert erreicht. |

### Lösung

- Stellen Sie sicher, dass die Standardwebsite konfiguriert ist und an 127.0.0.1 lauscht.
- Falls im BackendHttpSetting-Element nicht der Port 80 angegeben ist, muss die Standardwebsite so konfiguriert werden, dass sie am angegebenen Port lauscht.
- Der Aufruf von http://127.0.0.1:port muss den HTTP-Ergebniscode 200 zurückgeben. Dieser muss innerhalb des Timeoutzeitraums von 30 Sekunden zurückgegeben werden.
- Vergewissern Sie sich, dass der konfigurierte Port geöffnet ist und dass eingehender oder ausgehender Datenverkehr am konfigurierten Port nicht durch Firewallregeln oder Azure-Netzwerksicherheitsgruppen blockiert wird.
- Stellen Sie bei Verwendung klassischer virtueller Azure-Computer sowie bei Verwendung des Clouddiensts mit FQDN oder öffentlicher IP-Adresse außerdem sicher, dass der entsprechende [Endpunkt](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md) geöffnet ist.
- Falls der virtuelle Computer über Azure Resource Manager konfiguriert wurde und sich außerhalb des virtuellen Netzwerks befindet, in dem Application Gateway bereitgestellt ist, muss für den Zugriff auf den gewünschten Port eine [Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) konfiguriert werden.


## Probleme mit einer benutzerdefinierten Integritätsüberprüfung

### Ursache

Benutzerdefinierte Integritätsüberprüfungen sorgen für zusätzliche Flexibilität. Bei Verwendung von benutzerdefinierten Überprüfungen können Benutzer das Überprüfungsintervall, die URL und den zu überprüfenden Pfad konfigurieren und festlegen, wie viele fehlerhafte Antworten akzeptiert werden sollen, bevor die Back-End-Pool-Instanz als fehlerhaft gekennzeichnet wird. Folgende Zusatzeigenschaften werden hinzugefügt:

|Überprüfungseigenschaft| Beschreibung|
|---|---|
| Name | Name der Überprüfung. Dieser Name wird verwendet, um in den Back-End-HTTP-Einstellungen auf die Überprüfung zu verweisen. |
| Protocol | Das zum Senden der Überprüfung verwendete Protokoll. HTTP ist das einzige gültige Protokoll. |
| Host | Hostname zum Senden der Überprüfung Nur relevant, wenn in Application Gateway mehrere Standorte konfiguriert sind. Entspricht nicht dem VM-Hostnamen. |
| Pfad | Relativer Pfad der Überprüfung. Der gültige Pfad beginnt mit „/“. Die Überprüfung wird an „<Protokoll>://<Host>:<Port><Pfad>“ gesendet. |
| Intervall | Überprüfungsintervall in Sekunden Dies ist das Zeitintervall zwischen zwei aufeinanderfolgenden Überprüfungen.|
| Zeitüberschreitung | Zeitüberschreitung der Überprüfung in Sekunden. Die Überprüfung wird als fehlerhaft markiert, wenn innerhalb des Zeitraums für die Zeitüberschreitung keine gültige Antwort empfangen wird. |
| Fehlerhafter Schwellenwert | Anzahl der Wiederholungsversuche der Überprüfung Der Back-End-Server wird als außer Betrieb markiert, nachdem die Anzahl der aufeinanderfolgenden fehlgeschlagenen Überprüfungen den fehlerhaften Schwellenwert erreicht. |


### Lösung

Vergewissern Sie sich anhand der Tabelle weiter oben, dass die benutzerdefinierte Integritätsüberprüfung ordnungsgemäß konfiguriert ist. Stellen Sie zusätzlich zu den oben aufgeführten Problembehandlungsschritten auch Folgendes sicher:

- Stellen Sie sicher, dass das Protokoll nur auf HTTP festgelegt ist. HTTPS wird derzeit nicht unterstützt.
- Stellen Sie anhand der [Anleitung](application-gateway-create-probe-ps.md) sicher, dass die Überprüfung ordnungsgemäß angegeben ist.
- Falls Application Gateway für einen einzelnen Standort konfiguriert ist, muss der Hostname standardmäßig als „127.0.0.1“ angegeben werden, sofern in der benutzerdefinierten Überprüfung nichts anderes konfiguriert ist.
- Vergewissern Sie sich, dass ein Aufruf von „http://\<Host>:<Port><Pfad>“ den HTTP-Ergebniscode 200 zurückgibt.
- Stellen Sie sicher, dass „Intervall“, „Zeitüberschreitung“ und „Fehlerhafter Schwellenwert“ innerhalb des zulässigen Bereichs liegen.

## Anforderungstimeout

### Ursache

Wenn eine Benutzeranforderung empfangen wird, wendet Application Gateway die konfigurierten Regeln auf die Anforderung an und leitet sie an eine Back-End-Poolinstanz weiter. Danach wartet Application Gateway eine bestimmte Zeit auf eine Antwort der Back-End-Instanz. Dieses Intervall ist standardmäßig auf **30 Sekunden** festgelegt. Wenn Application Gateway innerhalb dieses Intervalls keine Antwort von der Back-End-Anwendung erhält, wird dem Benutzer der Fehler 502 angezeigt.

### Lösung

Mit Application Gateway können Benutzer diese Einstellung über das BackendHttpSetting-Element konfigurieren und auf verschiedene Pools anwenden. Unterschiedliche Back-End-Adresspools können unterschiedliche Back-End-HTTP-Einstellungen und somit unterschiedliche Anforderungstimeouts besitzen.

	New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60

## Nächste Schritte

Sollte sich das Problem mit den oben genannten Schritten nicht beheben lassen, erstellen Sie ein [Supportticket](https://azure.microsoft.com/support/options/).

<!---HONumber=AcomDC_0907_2016-->