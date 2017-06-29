

![Virtuelle Computer in einem eigenständigen Clouddienst](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

Wenn Sie Ihre virtuellen Computer in einem virtuellen Netzwerk platzieren, können Sie entscheiden, wie viele Clouddienste Sie verwenden möchten, um Lastenausgleich und Verfügbarkeitsgruppen zu nutzen. Darüber hinaus können Sie die virtuellen Computer in Subnetzen auf die gleiche Weise organisieren wie Ihr lokales Netzwerk und das virtuelle Netzwerk mit Ihrem lokalen Netzwerk verbinden. Hier sehen Sie ein Beispiel:

![Virtuelle Computer in einem virtuellen Netzwerk](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

Virtuelle Netzwerke sind das empfohlene Verfahren zum Verbinden virtueller Computern in Azure. Die bewährte Methode ist es, jede Ebene der Anwendung in einem separaten Clouddienst zu konfigurieren. Möglicherweise müssen Sie jedoch einige virtuelle Computer aus verschiedenen Anwendungsebenen im gleichen Clouddienst kombinieren, um die Höchstzahl von 200 Clouddiensten pro Abonnement nicht zu überschreiten. Diese und andere Einschränkungen finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../articles/azure-subscription-service-limits.md).

## <a name="connect-vms-in-a-virtual-network"></a>Virtuelle Computer in einem virtuellen Netzwerk verbinden
So verbinden Sie virtuelle Computer in einem virtuellen Netzwerk:

1. Erstellen Sie das virtuelle Netzwerk im [Azure-Portal](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md), und geben Sie die klassische Bereitstellung an.
2. Erstellen Sie die Gruppe von Clouddiensten für Ihre Bereitstellung entsprechend Ihrem Entwurf für Verfügbarkeitsgruppen und Lastenausgleich. Klicken Sie im Azure-Portal für jeden Clouddienst auf **Neu > Compute > Clouddienst**.

  Wenn Sie die Details für den Clouddienst angeben, wählen Sie die gleiche _Ressourcengruppe_ wie für das virtuelle Netzwerk aus.

3. Klicken Sie zum Erstellen der neuen virtuellen Computer jeweils auf **Neu > Compute**, und wählen Sie dann das entsprechende VM-Image unter **Ausgewählte Apps** aus.

  Wählen Sie auf dem Blatt **Grundlegende Einstellungen** für den virtuellen Computer die gleiche _Ressourcengruppe_ wie für das virtuelle Netzwerk aus.

  ![Blatt „Grundlegende Einstellungen“ für den virtuellen Computer bei Verwendung eines VNet](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_VN.png)

4. Wenn Sie **Einstellungen** für den virtuellen Computer ausfüllen, wählen Sie den richtigen _Clouddienst_ bzw. das richtige _virtuelle Netzwerk_ für den virtuellen Computer aus.

  Azure legt das andere Element basierend auf Ihrer Auswahl fest.

  ![Blatt „Einstellungen“ für den virtuellen Computer bei Verwendung eines VNet](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_VN.png)


## <a name="connect-vms-in-a-standalone-cloud-service"></a>Virtuelle Computer in einem eigenständigen Clouddienst verbinden
So verbinden Sie virtuelle Computer in einem eigenständigen Clouddienst:

1. Erstellen Sie den Clouddienst im [Azure-Portal](http://portal.azure.com). Klicken Sie auf **Neu > Compute > Clouddienst**. Alternativ können Sie beim Erstellen des ersten virtuellen Computers den Clouddienst für Ihre Bereitstellung erstellen.
2. Wenn Sie die virtuellen Computer erstellen, wählen Sie die gleiche Ressourcengruppe wie für den Clouddienst aus.

  ![Hinzufügen eines virtuellen Computers zu einem vorhandenen Clouddienst](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_SA.png)

3.  Wenn Sie die Details für den virtuellen Computer angeben, wählen Sie den Namen des Clouddiensts aus, den Sie im ersten Schritt erstellt haben.

  ![Auswählen eines Clouddiensts für einen virtuellen Computer](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_SA.png)
