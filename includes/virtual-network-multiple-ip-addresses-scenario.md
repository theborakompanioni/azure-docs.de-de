## <a name="scenario"></a>Szenario
Ein virtueller Computer mit einer einzelnen NIC wird erstellt und mit einem virtuellen Netzwerk verbunden. Der virtuelle Computer benötigt drei verschiedene *private* IP-Adressen und zwei *öffentliche* IP-Adressen. Die IP-Adressen werden den folgenden IP-Konfigurationen zugewiesen:

* **IPConfig-1:** Weist eine *statische* private IP-Adresse und eine *statische* öffentliche IP-Adresse zu.
* **IPConfig-2:** Weist eine *statische* private IP-Adresse und eine *statische* öffentliche IP-Adresse zu.
* **IPConfig-3:** Weist eine *statische* private IP-Adresse und keine öffentliche IP-Adresse zu.
  
    ![Mehrere IP-Adressen](./media/virtual-network-multiple-ip-addresses-scenario/multiple-ipconfigs.png)

Die IP-Konfigurationen werden der NIC zugeordnet, wenn diese erstellt wird, und die NIC wird an den virtuellen Computer angefügt, wenn dieser erstellt wird. Die für das Szenario verwendeten IP-Adresstypen dienen lediglich zur Veranschaulichung. Sie können beliebige IP-Adressen und Zuweisungstypen zuweisen.

> [!NOTE]
> In den Schritten dieses Artikels werden zwar alle IP-Konfigurationen einer einzelnen NIC zugewiesen, es ist jedoch auch möglich, jeder NIC in einem virtuellen Computer mit mehreren NICs mehrere IP-Konfigurationen zuzuweisen. Wie Sie einen virtuellen Computer mit mehreren NICs erstellen, erfahren Sie im Artikel [Erstellen einer VM mit mehreren Netzwerkkarten (NICs) mithilfe von PowerShell](../articles/virtual-network/virtual-network-deploy-multinic-arm-ps.md).