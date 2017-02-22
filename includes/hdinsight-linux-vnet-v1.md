> Sie können kein virtuelles Azure-Netzwerk der Version 1 (klassisch) mit Linux-basiertem HDInsight verwenden. Das virtuelle Netzwerk muss Version 2 aufweisen (Azure-Ressourcen-Manager), damit es beim Erstellen des Clusters im Azure-Vorschauportal als Option angezeigt wird bzw. beim Erstellen eines Clusters über die Azure-Befehlszeilenschnittstelle oder Azure PowerShell verwendet werden kann.
> 
> Wenn Ressourcen in einem v1-Netzwerk vorhanden sind und diese Ressourcen über ein virtuelles Netzwerk direkt auf HDInsight zugreifen können sollen, helfen Ihnen die Informationen zum Erstellen einer Verbindung zwischen einem virtuellen v2-Netzwerk und einem virtuellen v1-Netzwerk unter [Herstellen einer Verbindung zwischen klassischen VNets und neuen VNets](../articles/vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) weiter. Nachdem diese Verbindung eingerichtet wurde, können Sie den HDInsight-Cluster im virtuellen v2-Netzwerk erstellen.
> 
> 



<!--HONumber=Jan17_HO3-->


