# <a name="regions-and-availability-for-virtual-machines-in-azure"></a>Regionen und Verfügbarkeit für virtuelle Computer in Azure
Es ist wichtig zu verstehen, wie und wo Ihre virtuellen Computer (VMs) in Azure betrieben werden. Dies gilt auch für die Optionen zum Verbessern der Leistung, Verfügbarkeit und Redundanz. Azure wird in mehreren Rechenzentren weltweit betrieben. Diese Rechenzentren sind in geografische Regionen unterteilt, sodass Sie auswählen können, wo Sie Ihre Anwendungen erstellen möchten. Dieser Artikel enthält eine Übersicht über die Verfügbarkeits- und Redundanzfunktionen von Azure.

## <a name="what-are-azure-regions"></a>Was sind Azure-Regionen?
Mit Azure können Sie Ressourcen wie virtuelle Computer in definierten geografischen Regionen wie „USA, Westen“, „Europa, Norden“ oder „Asien, Südosten“ erstellen. Es gibt derzeit weltweit 30 Azure-Regionen. Sie können die [Liste der Regionen und Standorte](https://azure.microsoft.com/regions/)überprüfen. Innerhalb jeder Region sind mehrere Datencenter vorhanden, um Redundanz und Verfügbarkeit bereitzustellen. Dieser Ansatz gibt Ihnen beim Erstellen von Anwendungen die Flexibilität, VMs möglichst nah an Ihren Benutzern zu erstellen, um rechtliche oder steuerliche Anforderungen zu erfüllen und Compliance zu wahren.

## <a name="special-azure-regions"></a>Spezielle Azure-Regionen
Für Compliance oder rechtliche Zwecke gibt es einige spezielle Azure-Regionen, die Sie verwenden könnten, wenn Sie Ihre Anwendung erstellen. Es gibt die folgenden speziellen Regionen:

* **USA Gov Virginia** und **USA Gov Iowa**
  * Eine physische und logische vom Netzwerk isolierte Instanz von Azure für US-Regierungsbehörden und Partner, die von überprüften Personen aus den USA betreiben wird. Sie umfasst zusätzliche Compliancezertifizierungen, z.B. [FedRAMP](https://www.microsoft.com/en-us/TrustCenter/Compliance/FedRAMP) und [DISA](https://www.microsoft.com/en-us/TrustCenter/Compliance/DISA). Erfahren Sie mehr über [Azure Government](https://azure.microsoft.com/features/gov/).
* **Indien, Mitte**, **Indien, Süden** und **Indien, Westen**
  * Diese Regionen sind zurzeit für Kunden mit Volumenlizenzierung und Partner mit lokaler Registrierung in Indien verfügbar. Im Jahr 2016 können Benutzer auf diese zugreifen, wenn sie direkte Onlineabonnements erworben haben.
* **China, Osten** und **China, Norden**
  * Diese Regionen sind über eine einzigartige Partnerschaft zwischen Microsoft und 21Vianet verfügbar, wobei Microsoft die Rechenzentren nicht direkt betreut. Weitere Informationen finden Sie unter [Microsoft Azure in China](http://www.windowsazure.cn/).
* **Deutschland, Mitte** und **Deutschland, Nordosten**
  * Diese Regionen werden derzeit über ein Treuhändermodell für Daten bereitgestellt, bei dem Kundendaten in Deutschland verbleiben und von T-Systems als Treuhänder für deutsche Daten kontrolliert werden. (T-Systems ist ein Unternehmen von Deutsche Telekom.)

## <a name="region-pairs"></a>Regionspaare
Jede Azure-Region ist mit einer anderen Region innerhalb desselben Gebiets (z.B. USA, Europa oder Asien) kombiniert. Dieser Ansatz ermöglicht die Replikation von Ressourcen wie VM-Speicher innerhalb eines Gebiets. Dadurch sollte die Wahrscheinlichkeit verringert werden, dass Naturkatastrophen, Unruhen, Stromausfälle oder physische Netzwerkausfälle beide Regionen gleichzeitig betreffen. Weitere Vorteile von Regionspaaren:

* Bei einem größeren Azure-Ausfall erhält eine Region in jedem Paar Priorität, um die Zeit für die Wiederherstellung von Anwendungen zu verkürzen. 
* Geplante Azure-Updates werden nacheinander in den Regionen eines Paars eingeführt, um Ausfallzeiten und das Risiko von Anwendungsausfällen zu minimieren.
* Daten befinden sich immer innerhalb desselben Gebiets wie ihr Paar (mit Ausnahme von Brasilien, Süden), um steuerliche und rechtliche Anforderungen zu erfüllen.

Beispiele für Regionspaare:

| Primär | Sekundär |
|:--- |:--- |
| USA (Westen) |USA (Ost) |
| Nordeuropa |Westeuropa |
| Südostasien |Asien (Osten) |

Sie können die vollständige [Liste von Regionspaaren hier](../articles/best-practices-availability-paired-regions.md#what-are-paired-regions)anzeigen.

## <a name="feature-availability"></a>Verfügbarkeit von Funktionen
Einige Dienste oder VM-Funktionen stehen nur in bestimmten Regionen zur Verfügung, z.B. bestimmte VM-Größen oder Speichertypen. Es gibt auch einige globale Azure-Dienste, für die Sie keine bestimmte Region auswählen müssen, beispielsweise [Azure Active Directory](../articles/active-directory/active-directory-whatis.md), [Traffic Manager](../articles/traffic-manager/traffic-manager-overview.md) oder [Azure DNS](../articles/dns/dns-overview.md). Als Hilfe beim Entwerfen Ihrer Anwendungsumgebung können Sie die [Verfügbarkeit von Azure-Diensten in jeder Region](https://azure.microsoft.com/regions/#services) überprüfen. 

## <a name="storage-availability"></a>Speicherverfügbarkeit
Beim Betrachten der verfügbaren Optionen für die Speicherreplikation sind grundlegende Informationen zu Azure-Regionen und -Gebiete erforderlich. Je nach Speichertyp stehen Ihnen verschiedene Replikationsoptionen zur Verfügung.

**Azure Managed Disks**
* Lokal redundanter Speicher (LRS)
  * Repliziert Ihre Daten dreimal innerhalb der Region, in der Sie das Speicherkonto erstellt haben.

**Speicherkontobasierte Datenträger**
* Lokal redundanter Speicher (LRS)
  * Repliziert Ihre Daten dreimal innerhalb der Region, in der Sie das Speicherkonto erstellt haben.
* Zonenredundanter Speicher (ZRS)
  * Repliziert Ihre Daten dreimal über zwei oder drei Standorte hinweg, entweder innerhalb einer einzelnen Region oder über zwei Regionen hinweg.
* Georedundanter Speicher (GRS)
  * Repliziert Ihre Daten in eine sekundäre Region, die Hunderte von Kilometern von der primären Region entfernt ist.
* Georedundanter Speicher mit Lesezugriff (RA-GRS)
  * Repliziert die Daten in eine sekundäre Region (wie bei GRS), bietet dann aber auch schreibgeschützten Zugriff auf die Daten am sekundären Standort.

Die folgende Tabelle bietet einen schnellen Überblick über die Unterschiede zwischen Speicherreplikationstypen:

| Replikationsstrategie | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Daten werden in mehreren Datencentern repliziert. |Nein |Ja |Ja |Ja |
| Daten können vom sekundären Standort und vom primären Standort aus gelesen werden. |Nein |Nein |Nein |Ja |
| Anzahl von Datenkopien, die auf separaten Knoten aufbewahrt werden. |3 |3 |6 |6 |

Sie erhalten weitere Informationen über [Azure Storage-Replikationsoptionen hier](../articles/storage/storage-redundancy.md). Weitere Informationen zu verwalteten Datenträgern finden Sie in der [Übersicht über Managed Disks](../articles/storage/storage-managed-disks-overview.md).

### <a name="storage-costs"></a>Speicherkosten
Die Preise hängen vom gewählten Speichertyp und der gewünschten Verfügbarkeit ab.

**Azure Managed Disks**
* Verwaltete Premium-Datenträger werden durch Solid-State-Laufwerke (SSD) gesichert, und verwaltete Standarddatenträger werden durch reguläre rotierende Festplatten gesichert. Sowohl verwaltete Premium- als auch Standard-Datenträger werden basierend auf der bereitgestellten Kapazität für den Datenträger in Rechnung gestellt.

**Nicht verwaltete Datenträger**
* Storage Premium wird durch Solid State Drives (SSDs) gesichert und wird basierend auf der Kapazität des Datenträgers berechnet.
* Standardspeicher wird durch reguläre rotierende Festplatten gesichert und wird basierend auf der verwendeten Kapazität und der gewünschten Speicherverfügbarkeit berechnet.
  * Für RA-GRS fallen zusätzliche Gebühren für Datenübertragungen zur Georeplikation für die Bandbreite für die Replikation dieser Daten in eine andere Azure-Region an.

Informationen zu den Preisen für die verschiedenen Speichertypen und Verfügbarkeitsoptionen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/) .

## <a name="azure-images"></a>Azure-Images
In Azure werden virtuelle Computer aus einem Image erstellt. Die Images stammen normalerweise vom [Azure Marketplace](https://azure.microsoft.com/marketplace/), wo Partner vorkonfigurierte vollständige Betriebssystem- oder Anwendungsimages anbieten können.

Wenn Sie einen virtuellen Computer aus einem Image im Azure Marketplace erstellen, arbeiten Sie tatsächlich mit Vorlagen. Azure Resource Manager-Vorlagen sind deklarative JSON-Dateien (JavaScript Object Notation), die zum Erstellen von komplexen Anwendungsumgebungen mit virtuellen Computern, Speicher, virtuellen Netzwerken usw. verwendet werden können. Erfahren Sie mehr über die Verwendung von [Azure Resource Manager-Vorlagen](../articles/azure-resource-manager/resource-group-overview.md) und das [Erstellen Ihrer eigenen Vorlagen](../articles/resource-group-authoring-templates.md).

Sie können auch eigene benutzerdefinierte Images erstellen und mit der [Azure-Befehlszeilenschnittstelle](../articles/virtual-machines/virtual-machines-linux-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder [Azure PowerShell](../articles/virtual-machines/virtual-machines-windows-upload-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hochladen, um schnell benutzerdefinierte virtuelle Computer für Ihre speziellen Anforderungen zu erstellen.

## <a name="availability-sets"></a>Verfügbarkeitsgruppen
Eine Verfügbarkeitsgruppe ist eine logische Gruppierung von virtuellen Computern, die es Azure ermöglicht zu verstehen, wie Ihre Anwendung erstellt ist, um Redundanz und Verfügbarkeit zu bieten. Es wird empfohlen, dass zwei oder mehr virtuelle Computer in einer Verfügbarkeitsgruppe erstellt werden, um eine Anwendung mit hoher Verfügbarkeit bereitzustellen und um die [Azure-SLA von&99;,95%](https://azure.microsoft.com/support/legal/sla/virtual-machines/) zu erfüllen. Wenn ein einzelner virtuelle Computer [Azure Storage Premium](../articles/storage/storage-premium-storage.md) verwendet, gilt die Azure-SLA für ungeplante Wartungsereignisse. Eine Verfügbarkeitsgruppe besteht aus zwei zusätzlichen Gruppierungen, die vor Hardwareausfällen schützen und die sichere Anwendung von Updates ermöglichen: Fehlerdomänen (FDs) und Updatedomänen (UDs).

![Schematische Darstellung der Konfiguration mit Updatedomäne und Fehlerdomäne](./media/virtual-machines-common-regions-and-availability/ud-fd-configuration.png)

Erfahren Sie mehr über das Verwalten der Verfügbarkeit von [virtuellen Linux-Computern](../articles/virtual-machines/virtual-machines-linux-manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder [virtuellen Windows-Computern](../articles/virtual-machines/virtual-machines-linux-manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="fault-domains"></a>Fehlerdomänen
Eine Fehlerdomäne ist eine logische Gruppe von zugrunde liegender Hardware mit einer gemeinsamen Stromquelle und einem Netzwerkswitch, ähnlich einem Rack in einem lokalen Rechenzentrum. Wenn Sie VMs in einer Verfügbarkeitsgruppe erstellen, werden Ihre VMs von der Azure-Plattform automatisch auf diese Fehlerdomänen verteilt. Bei diesem Ansatz werden die Auswirkungen von potenziellen Hardwareausfällen, Netzwerkausfällen oder Stromausfällen beschränkt.

#### <a name="managed-disk-fault-domains-and-availability-sets"></a>Fehlerdomänen und Verfügbarkeitsgruppen für verwaltete Datenträger
Virtuelle Computer mit [Azure Managed Disks](../articles/storage/storage-faq-for-disks.md) werden bei Verwendung einer verwalteten Verfügbarkeitsgruppe auf Fehlerdomänen für verwaltete Datenträger ausgerichtet. Diese Ausrichtung stellt sicher, dass sich alle verwalteten, an einen virtuellen Computer angefügten Datenträger innerhalb der gleichen Fehlerdomäne für verwaltete Datenträger befinden. Nur virtuelle Computer mit verwalteten Datenträgern können in einer verwalteten Verfügbarkeitsgruppe erstellt werden. Die Anzahl der Fehlerdomänen für verwaltete Datenträger variiert je nach Region – pro Region sind entweder zwei oder drei Fehlerdomänen für verwaltete Datenträger vorhanden.

### <a name="update-domains"></a>Updatedomänen
Eine Updatedomäne ist eine logische Gruppe von zugrunde liegender Hardware, die zur gleichen Zeit gewartet oder neu gestartet werden kann. Wenn Sie VMs in einer Verfügbarkeitsgruppe erstellen, werden Ihre VMs von der Azure-Plattform automatisch auf diese Updatedomänen verteilt. Mit diesem Ansatz wird sichergestellt, dass mindestens eine Instanz Ihrer Anwendung immer ausgeführt wird, wenn für die Azure-Plattform die regelmäßige Wartung durchgeführt wird. Während einer geplanten Wartung werden die Updatedomänen unter Umständen nicht der Reihe nach neu gestartet, sondern es wird jeweils nur eine Updatedomäne neu gestartet.

## <a name="next-steps"></a>Nächste Schritte
Sie können diese Verfügbarkeits- und Redundanzfunktionen jetzt nutzen, um Ihre Azure-Umgebung zu erstellen. Weitere Informationen zu bewährten Methoden finden Sie unter [Checkliste für die Verfügbarkeit](../articles/best-practices-availability-checklist.md).



<!--HONumber=Feb17_HO2-->


