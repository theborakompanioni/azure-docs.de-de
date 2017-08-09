---
title: "Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation"
description: "Konfigurieren Sie die benötigte Verbindungsinfrastruktur, um SAP HANA in Azure (große Instanzen) zu verwenden."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 0425da20f3f0abcfa3ed5c04cec32184210546bb
ms.openlocfilehash: abc3f5ee70130b6be093e63afb495c86b921ba26
ms.contentlocale: de-de
ms.lasthandoff: 07/20/2017

---

# <a name="sap-hana-large-instances-infrastructure-and-connectivity-on-azure"></a>Infrastruktur und Verbindungen mit SAP HANA in Azure (große Instanzen) 

Machen Sie sich mit den hier angegebenen Definitionen vertraut, bevor Sie diese Anleitung lesen. In [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) wurden zwei unterschiedliche Klassen von HANA-Einheiten (große Instanzen) eingeführt:

- S72, S72m, S144, S144m, S192 und S192m, die wir als „Typ I“-Klassen von SKUs bezeichnen.
- S384, S384m, S384xm, S576, S768 und S960, die wir als „Typ II“-Klassen“ von SKUs bezeichnen.

Die Klassenspezifizierer werden in der gesamten Dokumentation zu HANA (große Instanzen) verwendet, um auf unterschiedliche Funktionen und Anforderungen basierend auf SKUs für HANA (große Instanzen) zu verweisen.

Weitere häufig verwendete Definitionen sind:
- **Stapel für große Instanzen:** Ein Hardwareinfrastrukturstapel, der SAP HANA TDI-zertifiziert ist und für die Ausführung von SAP HANA-Instanzen in Azure vorgesehen ist.
- **SAP HANA in Azure (große Instanzen):** Offizieller Name für das Angebot in Azure, HANA-Instanzen auf SAP HANA TDI-zertifizierter Hardware auszuführen, die in Stapeln für große Instanzen in verschiedenen Azure-Regionen bereitgestellt wird. Der zugehörige Begriff **HANA (große Instanzen)** ist die Kurzform für SAP HANA in Azure (große Instanzen) und wird in diesem Handbuch für die technische Bereitstellung gemeinhin verwendet.
 

Nachdem der Kauf von SAP HANA in Azure (große Instanzen) zwischen Ihnen und dem Microsoft Enterprise-Kundenbetreuungsteam abgewickelt ist, benötigt Microsoft die folgenden Informationen, um HANA (große Instanz)-Einheiten bereitzustellen:

- Kundenname
- Geschäftliche Kontaktinformationen (samt E-Mail-Adresse und Telefonnummer)
- Technische Kontaktinformationen (samt E-Mail-Adresse und Telefonnummer)
- Technische Kontaktinformationen zum Netzwerk (samt E-Mail-Adresse und Telefonnummer)
- Region der Azure-Bereitstellung („USA, Westen“, „USA, Osten“, „Australien, Osten“, „Australien, Südosten“, „Europa, Westen“ und „Europa, Norden“ ab Juli) 
- 2017)
- SKU für SAP HANA in Azure (große Instanzen) (Konfiguration)
- Wie bereits im Dokument mit der Übersicht und Architektur für HANA (große Instanzen) ausgeführt, für jede Azure-Region, in der eine Bereitstellung erfolgt:
    - Ein /29-IP-Adressbereich für ER-P2P-Verbindungen, über die Azure-VNets mit HANA (große Instanzen) verbunden werden
    - Ein /24-CIDR-Block, der für den HANA (große Instanzen)-Server-IP-Pool verwendet wird
- Die IP-Adressbereichswerte, die im „VNet-Adressraum“-Attribut jedes Azure-VNets verwendet werden, für das Verbindungen mit HANA (große Instanzen) hergestellt werden
- Daten für jedes HANA (große Instanzen)-System:
  - Der gewünschte Hostname – idealerweise mit vollqualifiziertem Domänennamen
  - Die gewünschte IP-Adresse für die Einheit von HANA (große Instanzen) aus dem Adressbereich des Server-IP-Pools. Beachten Sie, dass die ersten 30 IP-Adressen im Adressbereich des Server-IP-Pools für die interne Verwendung in HANA (große Instanzen) reserviert sind.
  - SAP HANA-SID-Name für die SAP HANA-Instanz (zum Erstellen der benötigten SAP HANA-bezogenen Datenträgervolumes erforderlich). Die HANA-SID wird zum Erstellen der Berechtigungen für <sidadm> auf den NFS-Volumes benötigt, die an die Einheit von HANA (große Instanzen) angefügt werden. Außerdem wird sie als eine der Namenskomponenten der Datenträgervolumes verwendet, die bereitgestellt werden. Wenn Sie auf der Einheit mehr als eine HANA-Instanz ausführen möchten, müssen Sie mehrere HANA-SIDs auflisten. Für jede Instanz wird jeweils ein separater Volumesatz zugewiesen.
  - Die Gruppen-ID (groupid), die der Benutzer „hana-sidadm“ in Linux hat, ist erforderlich, um die notwendigen SAP HANA-bezogenen Datenträgervolumes zu erstellen. Bei einer SAP HANA-Installation wird die Gruppe „sapsys“ in der Regel mit der Gruppen-ID „1001“ erstellt. Der Benutzer „hana-sidadm“ gehört zu dieser Gruppe.
  - Die Benutzer-ID (userid), die der Benutzer „hana-sidadm“ in Linux hat, ist erforderlich, um die notwendigen SAP HANA-bezogenen Datenträgervolumes zu erstellen. Wenn Sie auf der Einheit mehrere HANA-Instanzen ausführen, müssen Sie alle Benutzer vom Typ „<sid>adm“ auflisten. 
- Die ID des Azure-Abonnements, mit dem SAP HANA in Azure HANA (große Instanzen) direkt verbunden wird. Diese Abonnement-ID verweist auf das Azure-Abonnement, für das die HANA-Einheiten (große Instanzen) berechnet werden.

Nachdem Sie die Informationen bereitgestellt haben, stellt Microsoft SAP HANA in Azure (große Instanzen) bereit, und Microsoft gibt die Informationen zurück, die erforderlich sind, damit Sie Ihre Azure-VNets mit HANA (große Instanzen) verbinden und auf die HANA (große Instanz)-Einheiten zugreifen können.

## <a name="connecting-azure-vms-to-hana-large-instances"></a>Verbinden von Azure-VMs mit HANA (große Instanzen)

Wie bereits unter [Übersicht und Architektur von SAP HANA in Azure (große Instanzen)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) beschrieben, sieht die minimale Bereitstellung von HANA (große Instanzen) mit der SAP-Anwendungsebene in Azure wie folgt aus:

![Mit SAP HANA in Azure (große Instanzen) und lokal verbundenes Azure-VNET](./media/hana-overview-architecture/image3-on-premises-infrastructure.png)

Bei genauerer Betrachtung der Azure-VNet-Seite wird klar, dass Folgendes erforderlich ist:

- Die Definition eines Azure-VNets, das verwendet wird, um darin die virtuellen Computer der SAP-Anwendungsschicht bereitzustellen.
- Dies bedeutet automatisch, dass im Azure-VNet ein Standardsubnetz definiert ist, das tatsächlich dasjenige ist, in dem die virtuellen Computer bereitgestellt werden.
- Das Azure-VNet, das erstellt wird, muss mindestens ein VM-Subnetz und ein ExpressRoute-Gatewaysubnetz haben. Diese Subnetze sollten den IP-Adressbereichen so zugewiesen werden, wie dies in den folgenden Abschnitten angegeben und beschrieben ist.

Nun soll ein genauerer Blick auf das Erstellen des Azure-VNets für HANA (große Instanzen) geworfen werden.

### <a name="creating-the-azure-vnet-for-hana-large-instances"></a>Erstellen des Azure-VNets für HANA (großen Instanzen)

>[!Note]
>Das Azure-VNet für HANA (großen Instanzen) muss entsprechend dem Azure Resource Manager-Bereitstellungsmodells erstellt werden. Das ältere Azure-Bereitstellungsmodell, üblicherweise als klassisches Bereitstellungsmodell bezeichnet, wird für die Lösung HANA (großen Instanzen) nicht unterstützt.

Das VNet kann mithilfe des Azure-Portals, mit PowerShell, der Azure-Vorlage oder Azure CLI erstellt werden (siehe [Erstellen eines virtuellen Netzwerks im Azure-Portal](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Im folgenden Beispiel wird ein VNet erläutert, das über das Azure-Portal erstellt wurde.

Bei Betrachtung der Definitionen eines Azure-VNets über das Azure-Portal sollen einige Definitionen untersucht werden. Außerdem soll ermittelt werden, welche Beziehung sie zu den nachstehend aufgelisteten verschiedenen IP-Adressbereichen haben. Mit **Adressraum** ist der Adressraum gemeint, der für das Azure-VNet verwenden werden kann. Dieser Adressraum ist auch der Adressbereich, der im VNet für die BGP-Routenweitergabe verwendet wird. Dieser **Adressraum** ist hier zu sehen:

![Im Azure-Portal angezeigter Adressraum eines Azure-VNets](./media/hana-overview-connectivity/image1-azure-vnet-address-space.png)

Im obigen Fall wurde für das Azure-VNet mit „10.16.0.0/16“ ein relativ großer und umfangreicher verwendbarer IP-Adressbereich bereitgestellt. Dies bedeutet, dass alle IP-Adressbereiche nachfolgender Subnetze in diesem VNet ihre Bereiche in diesem „Adressraum“ haben können. Üblicherweise ist es nicht empfehlenswert, einen solch großen Adressbereich für ein einzelnes VNet in Azure zu haben. Im nächsten Schritt werden nun die Subnetze untersucht, die im Azure-VNet definiert sind:

![Azure-VNet-Subnetze und deren IP-Adressbereiche](./media/hana-overview-connectivity/image2b-vnet-subnets.png)

Wie Sie sehen, geht es um ein VNet mit einem ersten VM-Subnetz (hier als „default“ bezeichnet) und ein Subnetz namens „GatewaySubnet“.
Im folgenden Abschnitt wird auf den IP-Adressbereich des Subnetzes, das in der Abbildung den Namen „default“ hat, mit **IP-Adressbereich des Azure-VM-Subnetzes** verwiesen. In den folgenden Abschnitten wird auf den IP-Adressbereich des Gatewaysubnetzes mit **IP-Adressbereich des VNet-Gatewaysubnetzes** verwiesen. 

Für den Fall, der in den beiden obigen Abbildungen veranschaulicht ist, sehen Sie, dass der **VNet-Adressraum** sowohl den **IP-Adressbereich des Azure-VM-Subnetzes** als auch den **IP-Adressbereich des VNet-Gatewaysubnetzes** abdeckt. 

In anderen Fällen, in denen Sie IP-Adressbereiche sparsam oder spezifisch verwenden müssen, können Sie den **VNet-Adressraum** eines VNets auf die jeweiligen Bereiche einschränken, die für die einzelnen Subnetze verwendet werden. Im vorliegenden Fall können Sie den **VNet-Adressraum** eines VNets als mehrere bestimmte Bereiche definieren:

![Azure-VNet-Adressraum mit zwei Bereichen](./media/hana-overview-connectivity/image3-azure-vnet-address-space_alternate.png)

In diesem Fall hat der **VNet-Adressraum** zwei definierte Bereiche. Diese beiden Adressräume sind mit den IP-Adressbereichen identisch, die für den **IP-Adressbereich des Azure-VM-Subnetzes** und den **IP-Adressbereich des VNet-Gatewaysubnetzes** definiert sind.

Für diese Mandantensubnetze (VM-Subnetze) können Sie einen beliebigen Benennungsstandard verwenden. Es muss aber **immer nur ein einziges Gatewaysubnetz für jedes VNet vorhanden sein**, für das eine Verbindung mit der ExpressRoute-Verbindung von SAP HANA in Azure (große Instanzen) hergestellt wird. Außerdem **muss dieses Gatewaysubnetz immer den Namen „GatewaySubnet“** haben, um dafür zu sorgen, dass das ExpressRoute-Gateway richtig angeordnet ist.

> [!WARNING] 
> Es ist sehr wichtig, dass das Gatewaysubnetz immer „GatewaySubnet“ heißt.

Es können mehrere VM-Subnetze verwendet werden, in denen sogar nicht zusammenhängende Adressbereiche genutzt werden können. Allerdings müssen diese Adressbereiche, wie bereits erwähnt, durch den **VNet-Adressraum** des VNets in aggregierter Form oder in einer Liste der genauen Bereiche der VM-Subnetze und des Gatewaysubnetzes abgedeckt werden.

Es folgt die Zusammenfassung der wichtigen Fakten zu einem Azure-VNet, das mit HANA (große Instanzen) verbunden ist:

- Sie müssen den **VNet-Adressraum** an Microsoft übermitteln, wenn Sie eine Erstbereitstellung von HANA (große Instanzen) durchführen. 
- Der **VNet-Adressraum** kann ein größerer Bereich sein, der den Bereich für die IP-Adressbereiche des Azure-VM-Subnetzes und den IP-Adressbereich des VNet-Gatewaysubnetzes abdeckt.
- Sie können als **VNet-Adressraum** aber auch mehrere Bereiche übermitteln, die die unterschiedlichen IP-Adressbereiche der IP-Adressbereiche des Azure-VM-Subnetzes und den IP-Adressbereich des VNet-Gatewaysubnetzes abdecken.
- Der definierte **VNet-Adressraum** wird zur BGP-Routingweitergabe verwendet.
- Das Gatewaysubnetz muss den Namen **GatewaySubnet** haben.
- Der **VNet-Adressraum** wird auf der HANA (große Instanzen)-Seite als Filter verwendet, um Datenverkehr zu den HANA (große Instanz)-Einheiten von Azure zuzulassen oder zu unterbinden. Wenn die BGP-Routinginformationen des Azure-VNets und die IP-Adressbereiche, die für das Filtern auf der Seite von HANA (große Instanzen) konfiguriert sind, nicht zueinander passen, können Verbindungsprobleme auftreten.
- Es gibt einige Details zu dem Gatewaysubnetz, die weiter unten im Abschnitt „Verbinden eines VNets mit HANA (große Instanzen)-ExpressRoute“ erläutert sind.



### <a name="different-ip-address-ranges-to-be-defined"></a>Andere zu definierende IP-Adressbereiche 

Einige IP-Adressbereiche, die zum Bereitstellen von HANA (große Instanzen) erforderlich sind, wurden bereits in den Abschnitten weiter oben vorgestellt. Es gibt aber noch einige weitere IP-Adressbereiche, die wichtig sind. Dazu folgen nun weitere Details. Die folgenden IP-Adressen, von denen nicht alle an Microsoft zu übermitteln sind, müssen definiert sein, bevor eine Anforderung zur Erstbereitstellung gesendet wird:

- **VNet-Adressraum:** Dies sind, wie bereits oben vorgestellt, der bzw. die IP-Adressbereiche, die Sie dem Adressraum-Parameter in den Azure Virtual Networks (VNets) zugewiesen haben (oder später zuweisen möchten), die mit der Umgebung von SAP HANA (große Instanzen) verbunden werden. Dieser Adressraum-Parameter sollte ein mehrzeiliger Wert sein, der aus den Azure-VM-Subnetzbereichen und dem Azure-Gatewaysubnetzbereich besteht (siehe Abbildungen weiter oben). Dieser Bereich darf weder mit dem Adressbereich Ihres lokalen oder Server-IP-Pools noch mit ER P2P-Adressbereichen überlappen. Wie erhalten Sie diese IP-Adressbereiche? Ihr Unternehmensnetzwerkteam oder Dienstanbieter sollte einen oder mehrere IP-Adressbereiche bereitstellen, die in Ihrem Netzwerk nicht verwendet werden. Beispiel: Wenn Ihr Azure-VM-Subnetz (siehe oben) als „10.0.1.0/24“ und Ihr Azure-Gatewaysubnetz (siehe unten) als „10.0.2.0/28“ angegeben ist, sollte Ihr Azure-VNet-Adressraum aus zwei Zeilen bestehen: „10.0.1.0/24“ und „10.0.2.0/28“. Die Adressraumwerte können zwar aggregiert werden, aber es wird empfohlen, sie an die Subnetzbereiche anzupassen. So wird eine versehentliche spätere Wiederverwendung von ungenutzten IP-Adressbereichen in größeren Adressräumen an einer anderen Stelle Ihres Netzwerks vermieden. **Der VNet-Adressraum ist ein IP-Adressbereich, der an Microsoft übermittelt werden muss, wenn eine Erstbereitstellung angefordert wird.**

- **IP-Adressbereich des Azure-VM-Subnetzes:** Wie bereits weiter oben erläutert, ist dies der IP-Adressbereich, den Sie dem Azure-VNet-Subnetzparameter für das Azure-VNet zugewiesen haben (oder später zuweisen möchten), das mit der Umgebung von SAP HANA (große Instanzen) verbunden wird. Dieser IP-Adressbereich wird dazu verwendet, Ihren Azure-VMs IP-Adressen zuzuweisen. Die IP-Adressen dieses Bereichs können eine Verbindung mit Ihren Servern für SAP HANA (große Instanzen) herstellen. Bei Bedarf können mehrere Azure-VM-Subnetze verwendet werden. Microsoft empfiehlt für jedes Azure-VM-Subnetz einen /24-CIDR-Block. Dieser Adressbereich muss Bestandteil der Werte sein, die im Azure-VNet-Adressraum verwendet werden. Wie erhalten Sie diesen IP-Adressbereich? Ihr Unternehmensnetzwerkteam oder Dienstanbieter sollte einen IP-Adressbereich bereitstellen, der in Ihrem Netzwerk derzeit nicht verwendet wird.

- **IP-Adressbereich des VNet-Gatewaysubnetzes:** Je nachdem, welche Features Sie verwenden möchten, wird die folgende Größe empfohlen:
   - Höchstleistungs-ExpressRoute-Gateway: /26-Adressblock – erforderlich für Typ II-Klasse von SKUs
   - Koexistenz mit VPN und ExpressRoute über ein Hochleistungs-ExpressRoute-Gateway (oder kleiner): /27-Adressblock
   - Alle anderen Fälle: /28-Adressblock. Dieser Adressbereich muss Bestandteil der Werte sein, die in den Werten von „Azure-VNet-Adressraum“ verwendet werden. Dieser Adressbereich muss Bestandteil der Werte sein, die in dem Azure-VNet-Adressraum verwendet werden, den Sie an Microsoft übermitteln müssen. Wie erhalten Sie diesen IP-Adressbereich? Ihr Unternehmensnetzwerkteam oder Dienstanbieter sollte einen IP-Adressbereich bereitstellen, der in Ihrem Netzwerk derzeit nicht verwendet wird. 

- **Adressbereich für ER-P2P-Konnektivität:** Dieser Adressbereich ist der IP-Adressbereich für die ExpressRoute-P2P-Verbindung von SAP HANA (große Instanzen) (ExpressRoute = ER). Dieser Bereich von IP-Adressen muss ein /29-CIDR-IP-Adressbereich sein. Dieser Bereich darf weder mit dem lokalen noch mit anderen Azure-IP-Adressbereichen überlappen. Dieser IP-Adressbereich wird verwendet, um die ER-Konnektivität von Ihrem Azure-VNet ExpressRoute-Gateway zu den Servern von SAP HANA (große Instanzen) einzurichten. Wie erhalten Sie diesen IP-Adressbereich? Ihr Unternehmensnetzwerkteam oder Dienstanbieter sollte einen IP-Adressbereich bereitstellen, der in Ihrem Netzwerk derzeit nicht verwendet wird. **Dieser Bereich ist ein IP-Adressbereich, der an Microsoft übermittelt werden muss, wenn eine Erstbereitstellung angefordert wird.**
  
- **Adressbereich des Server-IP-Pools:** Dieser IP-Adressbereich wird verwendet, um jedem SAP HANA (große Instanzen)-Server seine eigene IP-Adresse zuzuweisen. Als Subnetzgröße wird ein /24-CIDR-Block empfohlen – bei Bedarf kann aber auch ein kleineres Subnetz verwendet werden, mindestens 64 IP-Adressen müssen jedoch bereitgestellt werden. Aus diesem Bereich werden die ersten 30 IP-Adressen für die Verwendung durch Microsoft reserviert. Dieser Punkt ist zu berücksichtigen, wenn Sie die Größe des Bereichs festlegen. Dieser Bereich darf weder mit den lokalen noch mit anderen Azure-IP-Adressen überlappen. Wie erhalten Sie diesen IP-Adressbereich? Ihr Unternehmensnetzwerkteam oder Dienstanbieter sollte einen IP-Adressbereich bereitstellen, der in Ihrem Netzwerk derzeit nicht verwendet wird. Ein eindeutiger /24-CIDR-Block (empfohlen) für die Zuweisung bestimmter IP-Adressen, die für SAP HANA in Azure (große Instanzen) erforderlich sind. **Dieser Bereich ist ein IP-Adressbereich, der an Microsoft übermittelt werden muss, wenn eine Erstbereitstellung angefordert wird.**
 
Sie müssen zwar die oben aufgeführten IP-Adressbereiche planen und definieren, es ist aber nicht erforderlich, dass sie alle dieser Bereich an Microsoft übermitteln. Zusammengefasst ergibt sich, dass Sie Microsoft die folgenden IP-Adressbereiche benennen müssen:

- Azure-VNet-Adressraum oder -Adressräume
- Adressbereich für ER-P2P-Konnektivität
- Adressbereich des Server-IP-Pools

Wenn Sie weitere VNets hinzufügen, die mit SAP HANA (große Instanzen) verbunden werden sollen, müssen Sie Microsoft den neuen Azure-VNet-Adressraum übermitteln, den Sie hinzufügen. 

Es folgen ein Beispiel für die verschiedenen Bereiche sowie einige Beispielbereiche dafür, wie Sie diese konfigurieren und schließlich an Microsoft übermitteln müssen. Wie Sie sehen können, wird der Wert für den Azure-VNet-Adressraum im ersten Beispiel nicht aggregiert, sondern aus den Bereichen des ersten IP-Adressbereichs des Azure-VM-Subnetzes und des IP-Adressbereichs des VNet-Gatewaysubnetzes definiert. Ein Verwenden mehrerer VM-Subnetze im Azure-VNet würde entsprechend funktionieren, indem die weiteren IP-Adressbereiche der zusätzlichen VM-Subnetze als Bestandteil des Azure-VNet-Adressraums konfiguriert und übermittelt werden.

![In einer minimalen Bereitstellung von SAP HANA in Azure (große Instanzen) erforderliche IP-Adressbereiche](./media/hana-overview-connectivity/image4b-ip-addres-ranges-necessary.png)

Sie können die Daten, die Sie an Microsoft übermitteln, auch aggregieren. In diesem Fall umfasst der Adressraum des Azure-VNets nur einen Bereich. Es werden die IP-Adressbereiche aus dem obigen Beispiel verwendet. Dieser aggregierte VNet-Adressraum kann wie folgt aussehen:

![Zweite Möglichkeit für in einer minimalen Bereitstellung von SAP HANA in Azure (große Instanzen) erforderliche IP-Adressbereiche](./media/hana-overview-connectivity/image5b-ip-addres-ranges-necessary-one-value.png)

Wie Sie oben sehen können, wird nun anstelle von zwei kleineren Bereichen, die den Adressraum des Azure-VNets definieren, ein größerer Bereich verwendet, der 4096 IP-Adressen abdeckt. Eine solche große Definition des Adressraums bewirkt, dass einige ziemlich große Bereiche nicht verwendet werden. Da die VNet-Adressraumwerte für die BGP-Routenweitergabe verwendet werden, können Routingprobleme verursacht werden, wenn ungenutzte Bereiche lokal oder an anderer Stelle in Ihrem Netzwerk verwendet werden. Deshalb empfiehlt es sich, den Adressraum eng an dem tatsächlich verwendeten Subnetzadressraum auszurichten. Sie können bei Bedarf später neue Adressraumwerte hinzufügen, ohne dass dies zu Ausfallzeiten im VNet führt.
 
> [!IMPORTANT] 
> Die IP-Adressbereiche für ER-P2P, Server-IP-Pool und Azure-VNet-Adressraum dürfen sich **NICHT** überlappen, und zwar weder untereinander noch mit irgendeinem anderen Bereich, der an anderer Stelle in Ihrem Netzwerk verwendet wird. Für jeden IP-Adressbereich gilt, dass er diskret sein muss und – wie die beiden obigen Abbildungen veranschaulichen – kein Subnetz eines anderen Bereichs sein darf. Gibt es Überlappungen zwischen Bereichen, kann es passieren, dass keine Verbindung zwischen dem Azure-VNet und der ExpressRoute-Leitung hergestellt werden kann.

### <a name="next-steps-after-address-ranges-have-been-defined"></a>Nächste Schritte, nachdem Adressbereiche definiert wurden
Nachdem die IP-Adressbereiche definiert wurden, müssen die folgenden Schritte ausgeführt werden:

1. Übermitteln Sie die IP-Adressbereiche für Azure VNet-Adressraum, den Adressbereich für ER-P2P-Konnektivität und den Adressbereich des Server IP-Pools zusammen mit anderen Daten, die am Anfang des Dokuments aufgelistet sind. Zu diesem Zeitpunkt können Sie auch beginnen, das virtuelle Netzwerk (VNet) und die VM-Subnetze zu erstellen. 
2. Eine ExpressRoute-Leitung wird von Microsoft zwischen Ihrem Azure-Abonnement und dem Stempel für SAP HANA (große Instanzen) erstellt.
3. Microsoft erstellt ein Mandantennetzwerk auf dem Stempel für SAP HANA (große Instanzen).
4. Microsoft konfiguriert die Netzwerkfunktionen in der Infrastruktur von SAP HANA in Azure (große Instanzen), damit IP-Adressen aus Ihrem Azure-VNet-Adressraum akzeptiert werden, über die mit SAP HANA (große Instanzen) kommuniziert werden soll.
5. Je nach der speziellen erworbenen SKU von SAP HANA in Azure (große Instanzen) weist Microsoft eine Computeeinheit in einem Mandantennetzwerk zu, reserviert Speicher, bindet diesen ein und installiert das Betriebssystem (SUSE oder RedHat Linux). IP-Adressen für diese Einheiten werden dem Adressbereich des Server IP-Pools entnommen, den Sie an Microsoft übermittelt haben.

Am Ende des Bereitstellungsprozesses stellt Microsoft Ihnen die folgenden Daten zur Verfügung:
- Informationen, die benötigt werden, um Ihre Azure-VNets mit der ExpressRoute-Leitung zu verbinden, über die Azure-VNets mit SAP HANA (große Instanzen) verbunden werden:
     - Autorisierungsschlüssel
     - ExpressRoute-Peer-ID
- Daten, um auf SAP HANA (große Instanzen) zuzugreifen, nachdem Sie die ExpressRoute-Leitung und das Azure-VNet eingerichtet haben

Die Sequenz für das Verbinden von HANA (große Instanzen) finden Sie auch im Dokument [End to End Setup for SAP HANA Large Instances](https://msdnshared.blob.core.windows.net/media/2017/06/End-to-End-Setup-of-SAP-HANA-Large-Instances.pdf) (End-to-End-Setup für SAP HANA (große Instanzen)). Viele der folgenden Schritte werden in diesem Dokument in einer Beispielbereitstellung dargestellt. 


## <a name="connecting-a-vnet-to-hana-large-instance-expressroute"></a>Verbinden eines VNets mit HANA (große Instanzen)-ExpressRoute

Sobald Sie alle IP-Adressbereiche definiert und die entsprechenden Daten von Microsoft erhalten haben, können Sie beginnen, das von Ihnen zuvor erstellte VNet mit SAP HANA (große Instanzen) zu verbinden. Nachdem das Azure-VNet erstellt wurde, muss ein ExpressRoute-Gateway für das VNet erstellt werden, um es mit der ExpressRoute-Verbindung zu verknüpfen, die sich mit dem Kundenmandanten im „Große Instanz“-Stapel verbindet.

> [!NOTE] 
> Dieser Schritt kann bis zu 30 Minuten dauern, da das neue Gateway im vorgesehenen Azure-Abonnement erstellt und anschließend mit dem angegebenen Azure-VNet verbunden wird.

Wenn bereits ein Gateway vorhanden ist, prüfen Sie, ob es ein ExpressRoute-Gateway ist. Falls nicht, muss das Gateway gelöscht und als ExpressRoute-Gateway neu erstellt werden. Wenn bereits ein ExpressRoute-Gateway eingerichtet ist und weil das Azure-VNet für lokale Konnektivität bereits mit der ExpressRoute-Verbindung verknüpft ist, fahren Sie mit dem folgenden Abschnitt „Verknüpfen von VNets“ fort.

- Verwenden Sie entweder das (neue) [Azure-Portal](https://portal.azure.com/) oder PowerShell zum Erstellen eines ExpressRoute-VPN-Gateways, das mit Ihrem VNet verbunden ist.
  - Wenn Sie das Azure-Portal verwenden, fügen Sie ein neues **virtuelles Netzwerkgateway** hinzu, und wählen Sie dann **ExpressRoute** als Gatewaytyp aus.
  - Wenn Sie PowerShell verwenden, laden Sie zunächst das neueste [Azure PowerShell SDK](https://azure.microsoft.com/downloads/) herunter, um eine optimale Leistung sicherzustellen. Über die folgenden Befehle erstellen Sie ein ExpressRoute-Gateway. Die Texte, denen _$_ vorangestellt ist, bestehen aus benutzerdefinierten Variablen, die mit Ihren jeweiligen Informationen aktualisiert werden müssen.

```PowerShell
# These Values should already exist, update to match your environment
$myAzureRegion = "eastus"
$myGroupName = "SAP-East-Coast"
$myVNetName = "VNet01"

# These values are used to create the gateway, update for how you wish the GW components to be named
$myGWName = "VNet01GW"
$myGWConfig = "VNet01GWConfig"
$myGWPIPName = "VNet01GWPIP"
$myGWSku = "HighPerformance" # Supported values for HANA Large Instances are: HighPerformance or UltraPerformance

# These Commands create the Public IP and ExpressRoute Gateway
$vnet = Get-AzureRmVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
New-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
-Location $myAzureRegion -AllocationMethod Dynamic
$gwpip = Get-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
-PublicIpAddressId $gwpip.Id

New-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
-IpConfigurations $gwipconfig -GatewayType ExpressRoute `
-GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
```

In diesem Beispiel wurde die Gateway-SKU „HighPerformance“ verwendet. Ihre Optionen sind „HighPerformance“ oder „UltraPerformance“ als einzige Gateway-SKUs, die für SAP HANA in Azure (große Instanzen) unterstützt werden.

> [!IMPORTANT]
> Für HANA (große Instanzen) mit den SKU-Typen S384, S384m, S384xm, S576, S768 und S960 (SKUs der Typ II-Klasse) ist die Verwendung der UltraPerformance-Gateway-SKU obligatorisch.

### <a name="linking-vnets"></a>Verknüpfen von VNets

Nun da das Azure-VNet ein ExpressRoute-Gateway hat, verwenden Sie die von Microsoft bereitgestellten Autorisierungsinformationen zum Verknüpfen des ExpressRoute-Gateways mit der ExpressRoute-Verbindung von SAP HANA in Azure (große Instanzen), die für diesen Zweck erstellt wurde. Dieser Schritt kann über das Azure-Portal oder mit PowerShell ausgeführt werden. Die Verwendung des Portals wird empfohlen, aber hier ist der Vollständigkeit halber auch die PowerShell-Anleitung angegeben. 

- Sie führen die folgenden Befehle für jedes VNet-Gateway aus, wobei Sie für jede Verbindung eine andere AuthGUID verwenden. Die ersten beiden unten im Skript gezeigten Einträge stammen aus den Informationen, die von Microsoft bereitgestellt werden. Zudem ist die AuthGUID spezifisch für jedes VNet und sein Gateway. Dies bedeutet Folgendes: Sie benötigen eine weitere AuthID für Ihre ExpressRoute-Leitung, die SAP HANA (große Instanzen) mit Azure verbindet, wenn Sie ein weiteres Azure-VNet hinzufügen möchten. 

```PowerShell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway Information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define the name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between the ER Circuit and your Gateway using the Authorization
$gw = Get-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzureRmVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID
```

Sie müssen diesen Schritt unter Umständen mehrmals ausführen, wenn Sie das Gateway mit mehreren ExpressRoute-Leitungen verbinden möchten, die Ihrem Abonnement zugeordnet sind. Beispielsweise ist die Wahrscheinlichkeit hoch, dass Sie das VNet-Gateway, über das das VNet mit Ihrem lokalen Netzwerk verbunden wird, mit der ExpressRoute-Leitung verbinden.

## <a name="adding-more-ip-addresses-or-subnets"></a>Hinzufügen weiterer IP-Adressen oder Subnetze

Verwenden Sie zum Hinzufügen weiterer IP-Adressen und Subnetze das Azure-Portal, PowerShell oder die CLI.

In diesem Fall empfiehlt es sich, den neuen IP-Adressbereich dem VNet-Adressraum als neuen Bereich hinzuzufügen, anstatt einen neuen aggregierten Bereich zu erstellen. In beiden Fällen müssen Sie diese Änderung an Microsoft übermitteln, damit Verbindungen aus diesem neuen IP-Adressbereich mit den SAP HANA (große Instanzen)-Einheiten in Ihrem Client zugelassen werden. Sie können eine Azure-Supportanfrage öffnen, um den neuen VNet-Adressraum hinzufügen zu lassen. Führen Sie nach Erhalt einer Bestätigung die nächsten Schritte aus.

Wenn Sie im Azure-Portal ein zusätzliches Subnetz erstellen möchten, lesen Sie den Artikel [Erstellen eines virtuellen Netzwerks im Azure-Portal](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Wie diese Schritte mit PowerShell erfolgen, finden Sie unter [Erstellen eines virtuellen Netzwerks mithilfe von PowerShell](../../../virtual-network/virtual-networks-create-vnet-arm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="adding-vnets"></a>Hinzufügen von VNets

Nach dem anfänglichen Herstellen von Verbindungen mit einem oder mehreren Azure VNets möchten Sie ggf. weitere hinzufügen, die auf SAP HANA in Azure (große Instanzen) zugreifen. Senden Sie zunächst eine Azure-Supportanfrage, in der Sie sowohl die speziellen Informationen zur Bestimmung der jeweiligen Azure-Bereitstellung als auch die IP-Adressbereiche des Azure VNet-Adressraums angeben. Das SAP HANA in Azure-Dienstverwaltungsteam stellt Ihnen die erforderlichen Informationen bereit, die Sie zum Verbinden der zusätzlichen VNets und von ExpressRoute benötigen. Für jedes VNet benötigen Sie einen eindeutigen Autorisierungsschlüssel, um eine Verbindung mit der ExpressRoute-Leitung für SAP HANA (große Instanzen) herzustellen.

Schritte zum Hinzufügen eines neuen Azure VNet:

1. Informationen zum ersten Schritt in diesem Prozess finden Sie im Abschnitt _Erstellen eines Azure VNet_.
2. Informationen zum zweiten Schritt in diesem Prozess finden Sie im Abschnitt _Erstellen eines Gatewaysubnetzes_.
3. Öffnen Sie eine Azure-Supportanfrage mit Informationen zum neuen VNet, und fordern Sie einen neuen Autorisierungsschlüssel zum Verknüpfen Ihrer zusätzlichen VNets mit der ExpressRoute-Verbindung von HANA (große Instanzen) an.
4. Nach Erhalt einer Autorisierungsbestätigung verwenden Sie die von Microsoft bereitgestellten Autorisierungsinformationen zum Ausführen des dritten Schritts in diesem Prozess. Siehe dazu _Verknüpfen von VNets_.

## <a name="increasing-expressroute-circuit-bandwidth"></a>Erhöhen der Bandbreite für die ExpressRoute-Verbindung

Besprechen Sie sich mit dem SAP HANA in Azure-Dienstverwaltungsteam. Falls Ihnen geraten wird, die Bandbreite der ExpressRoute-Verbindung für SAP HANA in Azure (große Instanzen) zu erhöhen, erstellen Sie eine Azure-Supportanforderung. (Sie können eine Erhöhung der Bandbreite einer einzelnen Verbindung um maximal 10 Gbit/s anfordern.) Sie erhalten eine Benachrichtigung, sobald der Vorgang abgeschlossen ist. Es sind keine weiteren Aktionen erforderlich, um diese höhere Geschwindigkeit in Azure zu aktivieren.

## <a name="adding-an-additional-expressroute-circuit"></a>Hinzufügen einer zusätzlichen ExpressRoute-Verbindung

Besprechen Sie sich mit dem SAP HANA in Azure-Dienstverwaltungsteam. Falls Ihnen gesagt wird, dass eine weitere ExpressRoute-Verbindung nötig sei, erstellen Sie eine Supportanforderung zum Erstellen einer weiteren ExpressRoute-Verbindung. Rufen Sie außerdem die entsprechenden Autorisierungsinformationen zum Herstellen einer Verbindung damit auf. Der Adressraum, der in den VNets verwendet wird, muss vor dem Übermitteln der Anforderung definiert werden, damit das SAP HANA in Azure-Dienstverwaltungsteam die Autorisierung erteilen kann.

Nachdem die neue Verbindung erstellt wurde und die SAP HANA in Azure-Dienstverwaltungskonfiguration abgeschlossen ist, erhalten Sie die Benachrichtigung mit den Informationen, die Sie zum Fortsetzen des Vorgangs benötigen. Führen Sie die zuvor beschriebenen Schritte zum Erstellen des neuen VNet und zum Herstellen einer Verknüpfung mit dieser zusätzlichen Verbindung durch. Sie können keine Azure-VNets mit dieser zusätzlichen Leitung verbinden, wenn die VNets bereits mit einer anderen ExpressRoute-Leitung von SAP HANA in Azure (große Instanzen) in derselben Azure-Region verbunden sind.

## <a name="deleting-a-subnet"></a>Löschen eines Subnetzes

Zum Entfernen eines VNet-Subnetzes kann das Azure-Portal, PowerShell oder die CLI verwendet werden. Für den Fall, dass Ihr Azure-VNet-IP-Adressbereich/Azure VNet-Adressraum ein aggregierter Bereich ist, besteht keine Notwendigkeit zur Nachsorge mit Microsoft. Die einzige Ausnahme ist, dass das VNet weiterhin BGP-Routenadressraum weitergibt, der das gelöschte Subnetz umfasst. Wenn Sie den Azure-VNet-IP-Adressbereich/Azure-VNet-Adressraum als mehrere IP-Adressbereiche definiert haben, von denen einer dem gelöschten Subnetz zugewiesen war, sollten Sie diesen aus Ihrem VNet-Adressraum löschen und dann die SAP HANA in Azure-Dienstverwaltung informieren, den Bereich aus den Bereichen zu entfernen, mit denen SAP HANA in Azure (große Instanzen) kommunizieren darf.

Es gibt noch keine speziellen Anleitungen für Azure.com zum Entfernen von Subnetzen. Doch der Prozess zum Entfernen von Subnetzen ist die Umkehrung des Hinzufügens von Subnetzen. Der Artikel [Erstellen eines virtuellen Netzwerks im Azure-Portal](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) enthält weitere Informationen zum Erstellen von Subnetzen.

## <a name="deleting-a-vnet"></a>Löschen eines VNet

Sie können ein VNet mithilfe des Azure-Portals, von PowerShell oder der CLI löschen. Das SAP HANA in Azure-Dienstverwaltungsteam entfernt die vorhandenen Autorisierungen für die ExpressRoute-Leitung von SAP HANA in Azure (große Instanzen) und den Azure-VNet-IP-Adressbereich/Azure-VNet-Adressraum für die Kommunikation mit SAP HANA (große Instanzen).

Nachdem das VNet entfernt wurde, beantragen Sie in einer Azure-Supportanfrage das Entfernen der IP-Adressbereiche.

Es gibt noch keine speziellen Anleitungen für Azure.com zum Entfernen von VNets. Doch der Prozess zum Entfernen von VNets ist die Umkehrung des zuvor beschriebenen Hinzufügens von VNets. In den Artikeln [Erstellen eines virtuellen Netzwerks im Azure-Portal](../../../virtual-network/virtual-networks-create-vnet-arm-pportal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) und [Erstellen eines virtuellen Netzwerks mithilfe von PowerShell](../../../virtual-network/virtual-networks-create-vnet-arm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) finden Sie weitere Informationen zum Erstellen von VNets.

Um sicherzustellen, dass alles entfernt wurde, löschen Sie die folgenden Elemente:

- Die ExpressRoute-Verbindung, das VNet-Gateway, die öffentliche IP-Adresse des VNet-Gateways und das VNet

## <a name="deleting-an-expressroute-circuit"></a>Löschen einer ExpressRoute-Verbindung

Um eine weitere ExpressRoute-Verbindung für SAP HANA in Azure (große Instanzen) zu entfernen, bitten Sie das SAP HANA in Azure-Dienstverwaltungsteam in einer Azure-Supportanfrage, die Verbindung zu löschen. Im Azure-Abonnement können Sie das VNet je nach Anforderung löschen oder beibehalten. Allerdings müssen Sie die Verbindung zwischen der SAP HANA (große Instanzen)-ExpressRoute-Leitung und dem verknüpften VNet-Gateway löschen.

Wenn Sie auch ein VNet entfernen möchten, befolgen Sie die Anleitung zum Löschen eines VNet im vorherigen Abschnitt.



