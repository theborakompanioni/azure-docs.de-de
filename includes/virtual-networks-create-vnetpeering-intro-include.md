VNet-Peering ist ein Mechanismus zum Verbinden von zwei Virtual Networks in derselben Region über das Azure-Backbonenetzwerk. Nach dem Peering werden die beiden Virtual Networks für alle Verbindungszwecke als ein Virtual Network angezeigt. Lesen Sie sich [Übersicht über VNet-Peering](../articles/virtual-network/virtual-network-peering-overview.md) durch, wenn Sie mit dem VNet-Peering noch nicht vertraut sind.

Das VNet-Peering ist als öffentliche Vorschauversion verfügbar. Um es nutzen zu können, müssen Sie sich mit dem folgenden Befehl registrieren:

> [AZURE.NOTE] Register-AzureRmProviderFeature -FeatureName AllowVnetPeering -ProviderNamespace Microsoft.Network –force
 

<!---HONumber=AcomDC_0803_2016-->