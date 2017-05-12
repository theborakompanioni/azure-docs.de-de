### <a name="to-modify-the-local-network-gateway-gatewayipaddress"></a>So ändern Sie die Angabe für „gatewayIpAddress“ für ein Gateway des lokalen Netzwerks

Wenn für das VPN-Gerät, mit dem Sie eine Verbindung herstellen möchten, die öffentliche IP-Adresse geändert wurde, müssen Sie das Gateway des lokalen Netzwerks entsprechend anpassen. Die Gateway-IP-Adresse kann geändert werden, ohne dass eine vorhandene VPN-Gatewayverbindung (sofern vorhanden) entfernt werden muss. Ersetzen Sie zum Ändern der Gateway-IP-Adresse die Werte „Site2“ und „TestRG1“ durch eigene Werte. Verwenden Sie dazu den Befehl [az network local-gateway update](https://docs.microsoft.com/cli/azure/network/local-gateway#update).

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 --name Site2 --resource-group TestRG1
```

Überprüfen Sie, ob die IP-Adresse in der Ausgabe korrekt ist:

```
"gatewayIpAddress": "23.99.222.170",
```