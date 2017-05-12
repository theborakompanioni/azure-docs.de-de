Mit dem Befehl [az network vpn-connection show](/cli/azure/network/vpn-connection#show) können Sie überprüfen, ob die Verbindung erfolgreich hergestellt wurde. Konfigurieren Sie die Werte so, dass sie Ihren eigenen Werten entsprechen. In dem Beispiel verweist „-Name“ auf den Namen der Verbindung, die Sie erstellt haben und testen möchten.

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```

Während der Verbindungsherstellung wird als Verbindungsstatus „Connecting“ (Wird verbunden...) angezeigt. Sobald die Verbindung hergestellt wurde, ändert sich der Status in „Connected“ (Verbunden), und Sie sehen die Eingangs- und Ausgangsbytes.