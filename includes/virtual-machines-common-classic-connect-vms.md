

![Virtuelle Computer in einem eigenständigen Clouddienst](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

Wenn Sie Ihre virtuellen Computer in einem virtuellen Netzwerk platzieren, können Sie entscheiden, wie viele Clouddienste Sie verwenden möchten, um Lastenausgleich und Verfügbarkeitsgruppen zu nutzen. Darüber hinaus können Sie die virtuellen Computer in Subnetzen auf die gleiche Weise organisieren wie Ihr lokales Netzwerk und das virtuelle Netzwerk mit Ihrem lokalen Netzwerk verbinden. Hier sehen Sie ein Beispiel:

![Virtuelle Computer in einem virtuellen Netzwerk](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

Virtuelle Netzwerke sind das empfohlene Verfahren zum Verbinden virtueller Computern in Azure. Die bewährte Methode ist es, jede Ebene der Anwendung in einem separaten Clouddienst zu konfigurieren. Möglicherweise müssen Sie jedoch einige virtuelle Computer aus verschiedenen Anwendungsebenen im gleichen Clouddienst kombinieren, um die Höchstzahl von 200 Clouddiensten pro Abonnement nicht zu überschreiten. Diese und andere Einschränkungen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../articles/azure-subscription-service-limits.md).

## <a name="connect-vms-in-a-virtual-network"></a>Virtuelle Computer in einem virtuellen Netzwerk verbinden
So verbinden Sie virtuelle Computer in einem virtuellen Netzwerk:

1. Erstellen Sie das virtuelle Netzwerk im [Azure-Portal](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md).
2. Erstellen Sie die Gruppe von Clouddiensten für Ihre Bereitstellung entsprechend Ihrem Entwurf für Verfügbarkeitsgruppen und Lastenausgleich. Klicken Sie im klassischen Azure-Portal für jeden Clouddienst auf **Neu > Compute > Cloud-Dienst > Benutzerdefiniert erstellen**.
3. Klicken Sie zum Erstellen der einzelnen neuen virtuellen Computer auf **Neu > Compute > Virtueller Computer > Aus Katalog**. Wählen Sie den richtigen Clouddienst und das richtige virtuelle Netzwerk für den virtuellen Computer aus. Wurde der Clouddienst bereits mit einem virtuellen Netzwerk verbunden, ist der Name dieses Diensts bereits für Sie ausgewählt.

![Auswählen eines Clouddiensts für einen virtuellen Computer](./media/virtual-machines-common-classic-connect-vms/VMConfig1.png)

## <a name="connect-vms-in-a-standalone-cloud-service"></a>Virtuelle Computer in einem eigenständigen Clouddienst verbinden
So verbinden Sie virtuelle Computer in einem eigenständigen Clouddienst:

1. Erstellen Sie den Clouddienst im [klassischen Azure-Portal](http://manage.windowsazure.com). Klicken Sie auf **Neu > Compute > Clouddienst > Benutzerdefiniert erstellen**. Alternativ können Sie beim Erstellen des ersten virtuellen Computers den Clouddienst für Ihre Bereitstellung erstellen.
2. Wählen Sie beim Erstellen der virtuellen Computer den Namen des im vorherigen Schritts erstellten Clouddiensts aus.
   
   ![Hinzufügen eines virtuellen Computers zu einem vorhandenen Clouddienst](./media/virtual-machines-common-classic-connect-vms/Connect-VM-to-CS.png)

