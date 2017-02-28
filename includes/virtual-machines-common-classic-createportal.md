

Ein *benutzerdefinierter* virtueller Computer ist einfach ein virtueller Computer, den Sie mit einer **ausgewählten App** im **Marketplace** erstellen, weil Ihnen damit viele Arbeitsschritte abgenommen werden. Sie können aber trotzdem Konfigurationen mit den folgenden Elementen auswählen:

* Verbinden des virtuellen Computers mit einem virtuellen Netzwerk.
* Installieren des virtuellen Azure-Computer-Agents und der -Erweiterungen, z. B. für Antischadsoftware.
* Hinzufügen des virtuellen Computers zu vorhandenen Clouddiensten.
* Hinzufügen des virtuellen Computers zu einem vorhandenen Speicherkonto.
* Hinzufügen des virtuellen Computers zu einer Verfügbarkeitsgruppe.

<!--
> [!IMPORTANT]
> If you want your virtual machine to use a virtual network so you can connect to it directly by host name or set up cross-premises connections, make sure that you specify the virtual network when you create the virtual machine. A virtual machine can be configured to join a virtual network only when you create the virtual machine. For details on virtual networks, see [Azure Virtual Network overview](../articles/virtual-network/virtual-networks-overview.md).
>
>
 -->

> [!IMPORTANT]
> Wenn Ihr virtueller Computer ein virtuelles Netzwerk verwenden soll, geben Sie das virtuelle Netzwerk beim Erstellen des virtuellen Computers an.

> * Zwei Vorteile der Nutzung eines virtuellen Netzwerks sind die direkte Verbindung mit dem virtuellen Computer und die Einrichtung von lokalen Verbindungen.

> * Ein virtueller Computer kann so konfiguriert werden, dass er nur zu dem Zeitpunkt Teil eines virtuellen Netzwerks werden kann, zu dem er erstellt wird. Ausführliche Informationen über virtuelle Netzwerke erhalten Sie unter [Übersicht über Azure Virtual Network](../articles/virtual-network/virtual-networks-overview.md).
>
>

## <a name="to-create-the-virtual-machine"></a>So erstellen Sie den virtuellen Computer
