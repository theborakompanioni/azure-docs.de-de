| Ressource | Standardlimit | Maximales Limit |
| --- | --- | --- |
| VMs pro [Abonnement](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> pro Region |10.000 pro Region |
| Gesamte Kerne pro [Abonnement](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> pro Region | Support kontaktieren |
| VM-Kerne pro Serie (Dv2, F usw.) pro [Abonnement](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> pro Region | Support kontaktieren |
| [Co-Administratoren](../articles/billing-add-change-azure-subscription-administrator.md) pro Abonnement |Unbegrenzt |Unbegrenzt |
| [Speicherkonten](../articles/storage/storage-create-storage-account.md) pro Abonnement |200 |200<sup>2</sup> |
| [Ressourcengruppen](../articles/azure-resource-manager/resource-group-overview.md) pro Abonnement |800 |800 |
| [Verfügbarkeitsgruppen](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) pro Abonnement |2.000 pro Region |2.000 pro Region |
| Ressourcen-Manager-API-Lesevorgänge |15.000 pro Stunde |15.000 pro Stunde |
| Ressourcen-Manager-API-Schreibvorgänge |1.200 pro Stunde |1.200 pro Stunde |
| Ressourcen-Manager-API-Anforderungsgröße |4.194.304 Bytes |4.194.304 Bytes |
| Tags pro Abonnement<sup>3</sup> |Unbegrenzt |Unbegrenzt |
| Eindeutige Tag-Berechnungen pro Abonnement<sup>3</sup> | 10.000 | 10.000 |
| [Clouddienste](../articles/cloud-services/cloud-services-choose-me.md) pro Abonnement |Nicht anwendbar<sup>4</sup> |Nicht anwendbar<sup>4</sup> |
| [Affinitätsgruppen](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) pro Abonnement |Nicht anwendbar<sup>4</sup> |Nicht anwendbar<sup>4</sup> |

<sup>1</sup>Standardgrenzwerte variieren nach angebotenem Kategorietyp, z.B. kostenlose Testversion, nutzungsbasierte Bezahlung und Serie wie Dv2, F, G usw.

<sup>2</sup>Dies umfasst sowohl Standard- als auch Premium-Speicherkonten. Wenn Sie mehr als 200 Speicherkonten benötigen, stellen Sie eine Anfrage an den [Azure-Support](https://azure.microsoft.com/support/faq/). Das Azure Storage-Team überprüft Ihr Geschäftsszenario und kann bis zu 250 Speicherkonten genehmigen.

<sup>3</sup>Sie können eine unbegrenzte Anzahl von Tags pro Abonnement anwenden. Die Anzahl von Tags pro Ressource oder Ressourcengruppe ist auf 15 beschränkt. Resource Manager gibt nur eine [Liste mit eindeutigen Tagnamen und Werten](/rest/api/resources/tags#Tags_List) im Abonnement zurück, wenn die Anzahl der Tags 10.000 oder weniger beträgt. Sie können eine Ressource jedoch auch anhand des Tags finden, wenn die Anzahl 10.000 überschreitet.  

<sup>4</sup> Diese Features sind für Azure-Ressourcengruppen und Azure Resource Manager nicht mehr erforderlich.

> [!NOTE]
> Es ist wichtig zu betonen, dass für Kerne von virtuellen Computern ein regionaler Gesamtgrenzwert und ein regionaler Grenzwert pro Größenserie (Dv2, F usw.) gilt, die separat voneinander erzwungen werden.  Angenommen, Sie verwenden ein Abonnement mit einem Kerngesamtgrenzwert von 30 für VMs in der Region „USA, Osten“, einem Kerngrenzwert von 30 für die A-Serie und einem Kerngrenzwert von 30 für die D-Serie.  Für dieses Abonnement dürfen dann 30 virtuelle A1-Computer bzw. 30 virtuelle D1-Computer bereitgestellt werden, oder eine Kombination daraus, bei der die Gesamtzahl von 30 Kernen nicht überschritten wird (z.B. 10 virtuelle A1-Computer und 20 virtuelle D1-Computer).  
> <!-- -->
> 
> 

