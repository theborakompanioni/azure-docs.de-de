Die Ansätze für Azure-Endpunkte unterscheiden sich beim klassischen Bereitstellungsmodell und beim Resource Manager-Bereitstellungsmodell leicht. Sie können jetzt flexibel Netzwerkfilter erstellen, mit denen der eingehende und ausgehende Datenverkehrsfluss Ihrer VMs gesteuert wird. Es ist möglich, über einen einfachen Endpunkt hinaus – wie beim klassischen Bereitstellungsmodell – komplexe Netzwerkumgebungen zu erstellen. Dieser Artikel enthält eine Übersicht über die Netzwerksicherheitsgruppen und den Unterschied zur Verwendung von klassischen Endpunkten, die Erstellung von Filterregeln und Beispiele für Bereitstellungsszenarien.

## Übersicht über Resource Manager-Bereitstellungen
Endpunkte im klassischen Bereitstellungsmodell werden durch Netzwerksicherheitsgruppen und Zugriffssteuerungslisten-Regeln (ACL-Regeln) ersetzt. Schritte zum Implementieren von ACL-Regeln für Netzwerksicherheitsgruppen:

* Erstellen einer Netzwerksicherheitsgruppe
* Definieren von ACL-Regeln für Netzwerksicherheitsgruppen zum Zulassen oder Verweigern von Datenverkehr
* Zuweisen der Netzwerksicherheitsgruppe zu einer Netzwerkschnittstelle oder einem Subnetz des virtuellen Netzwerks

Wenn Sie auch die Portweiterleitung durchführen möchten, müssen Sie vor der VM einen Load Balancer (Lastenausgleich) anordnen und NAT-Regeln verwenden. Schritte zum Implementieren eines Load Balancers und von NAT-Regeln:

* Einrichten eines Load Balancers
* Erstellen eines Back-End-Pools und Hinzufügen von VMs zum Pool
* Definieren der NAT-Regeln für die erforderliche Portweiterleitung
* Zuweisen von NAT-Regeln zu den VMs

## Netzwerksicherheitsgruppe – Übersicht
Netzwerksicherheitsgruppen sind ein neues Feature, das Ihnen eine neue Sicherheitsschicht beim Zulassen des Zugriffs auf Ihre VMs für bestimmte Ports und Subnetze bietet. Normalerweise verfügen Sie immer über eine Netzwerksicherheitsgruppe, die diese Sicherheitsschicht zwischen den VMs und der Außenwelt bereitstellt. Netzwerksicherheitsgruppen können auf ein Subnetz des virtuellen Netzwerks oder eine bestimmte Netzwerkschnittstelle für eine VM angewendet werden. Anstatt ACL-Regeln für Endpunkte zu erstellen, erstellen Sie jetzt ACL-Regeln für Netzwerksicherheitsgruppen. Mit diesen ACL-Regeln haben Sie eine viel bessere Kontrolle als bei der einfachen Erstellung eines Endpunkts für die Weiterleitung an einen bestimmten Port. [Hier finden Sie weitere Informationen zu Netzwerksicherheitsgruppen](../articles/virtual-network/virtual-networks-nsg.md).

> [!TIP]
> Sie können Netzwerksicherheitsgruppen mehreren Subnetzen oder Netzwerkschnittstellen zuweisen. Es findet keine 1:1-Zuordnung statt. Dies bedeutet, dass Sie ein Netzwerksicherheitsgruppe mit einer gemeinsamen Gruppe von ACL-Regeln erstellen und diese auf mehrere Subnetze oder Netzwerkschnittstellen anwenden können. Außerdem kann die Netzwerksicherheitsgruppe auf Ressourcen des gesamten Abonnements angewendet werden (basierend auf der [rollenbasierten Zugriffssteuerung](../articles/active-directory/role-based-access-control-what-is.md)).
> 
> 

## Load Balancer – Übersicht
Beim klassischen Bereitstellungsmodell führt Azure die gesamte Netzwerkadressübersetzung (NAT) und Portweiterleitung eines Clouddiensts für Sie durch. Bei der Erstellung eines Endpunkts geben Sie den externen Port an, der verfügbar gemacht werden soll, sowie den internen Port, an den der Datenverkehr geleitet wird. Netzwerksicherheitsgruppen selbst führen diese Art der Netzwerkadressübersetzung und Portweiterleitung nicht durch.

Erstellen Sie in Ihrer Ressourcengruppe einen Azure Load Balancer, der Ihnen die Erstellung von NAT-Regeln für die Portweiterleitung ermöglicht. Der Load Balancer kann bei Bedarf wiederum so präzise eingestellt werden, dass die Einstellung nur für bestimmte VMs gilt. Die NAT-Regeln des Azure Load Balancers in Kombination mit den ACL-Regeln für Netzwerksicherheitsgruppen bieten deutlich mehr Flexibilität und Kontrolle als bei der Verwendung von Clouddienst-Endpunkten. Weitere Informationen finden Sie in der [Übersicht über den Lastenausgleich](../articles/load-balancer/load-balancer-overview.md).

## ACL-Regeln von Netzwerksicherheitsgruppen
Mit ACL-Regeln können Sie definieren, welcher Datenverkehr basierend auf bestimmten Ports, Portbereichen oder Protokollen in die VM bzw. aus der VM fließen kann. Sie können diese Regeln entweder einzelnen VMs oder einem Subnetz zuweisen. Der folgende Screenshot zeigt ein Beispiel für ACL-Regeln für einen gängigen Webserver:

![Liste mit ACL-Regeln von Netzwerksicherheitsgruppen](./media/virtual-machines-common-endpoints-in-resource-manager/example-acl-rules.png)

ACL-Regeln werden basierend auf einer Prioritätsmetrik angewendet, die Sie angeben. Je höher der Wert, desto niedriger die Priorität. Jede Netzwerksicherheitsgruppe verfügt über drei Standardregeln, die dafür ausgelegt sind, den Fluss des Azure-Netzwerkdatenverkehrs zu verarbeiten. Hierbei wird ein explizites `DenyAllInbound`-Element als abschließende Regel verwendet. ACL-Standardregeln erhalten eine niedrige Priorität, damit von Ihnen erstellte Regeln nicht beeinträchtigt werden.

## Zuweisen von Netzwerksicherheitsgruppen
Sie weisen eine Netzwerksicherheitsgruppe einem Subnetz oder einer Netzwerkschnittstelle zu. Bei diesem Ansatz können Sie so präzise wie nötig vorgehen, wenn Sie Ihre ACL-Regeln auf eine bestimmte VM anwenden, oder Sie können sicherstellen, dass auf alle VMs, die Teil eines Subnetzes sind, eine gemeinsame Gruppe von ACL-Regeln angewendet wird:

![Netzwerksicherheitsgruppen auf Netzwerkschnittstellen oder Subnetze anwenden](./media/virtual-machines-common-endpoints-in-resource-manager/apply-nsg-to-resources.png)

Das Verhalten der Netzwerksicherheitsgruppe ändert sich nicht in Abhängigkeit davon, ob sie einem Subnetz oder einer Netzwerkschnittstelle zugewiesen ist. Ein häufiges Bereitstellungsszenario sieht so aus, dass die Netzwerksicherheitsgruppe einem Subnetz zugewiesen ist, um sicherzustellen, dass für alle VMs des Subnetzes für Kompatibilität gesorgt ist. Weitere Informationen finden Sie unter [Anwenden von Netzwerksicherheitsgruppen auf Ressourcen](../articles/virtual-network/virtual-networks-nsg.md#associating-nsgs).

## Standardverhalten von Netzwerksicherheitsgruppen
Je nachdem, wie und wann Sie die Netzwerksicherheitsgruppe erstellen, werden unter Umständen Standardregeln erstellt, um den RDP-Zugriff auf TCP-Port 3389 zuzulassen. Bei Linux-VMs wird der SSH-Zugriff auf TCP-Port 22 zugelassen. Diese automatischen ACL-Regeln werden unter den folgenden Bedingungen erstellt:

* Wenn Sie eine Windows-VM über das Portal anlegen und die Standardaktion zum Erstellen einer Netzwerksicherheitsgruppe akzeptieren, wird eine ACL-Regel zum Zulassen von TCP-Port 3389 (RDP) erstellt.
* Wenn Sie eine Linux-VM über das Portal anlegen und die Standardaktion zum Erstellen einer Netzwerksicherheitsgruppe akzeptieren, wird eine ACL-Regel zum Zulassen von TCP-Port 22 (SSH) erstellt.

Unter allen anderen Bedingungen werden diese ACL-Standardregeln nicht erstellt. Sie können keine Verbindung mit Ihrer VM herstellen, ohne die entsprechenden ACL-Regeln zu erstellen. Hierzu zählen die folgenden allgemeinen Aktionen:

* Erstellen einer Netzwerksicherheitsgruppe über das Portal als separate Aktion zum Erstellen der VM
* Programmgesteuertes Erstellen einer Netzwerksicherheitsgruppe per PowerShell, Azure-Befehlszeilenschnittstelle, Rest-APIs usw.
* Erstellen einer VM und Zuweisen zu einer vorhandenen Netzwerksicherheitsgruppe, für die nicht bereits die richtige ACL-Regel definiert ist

In allen vorherigen Fällen müssen Sie ACL-Regeln für die VM erstellen, um die entsprechenden Verbindungen für die Remoteverwaltung zuzulassen.

## Standardverhalten einer VM ohne Netzwerksicherheitsgruppe
Sie können eine VM anlegen, ohne eine Netzwerksicherheitsgruppe zu erstellen. In diesen Situationen können Sie eine Verbindung mit der VM per RDP oder SSH herstellen, ohne ACL-Regeln zu erstellen. Wenn Sie einen Webdienst unter Port 80 installiert haben, ist dieser Dienst automatisch per Remotezugriff zugänglich. Alle Ports der VM sind geöffnet.

> [!NOTE]
> Einer VM muss aber weiterhin eine öffentliche IP-Adresse zugewiesen sein, damit Remoteverbindungen verwendet werden können. Wenn keine Netzwerksicherheitsgruppe für das Subnetz oder die Netzwerkschnittstelle vorhanden ist, bedeutet dies nicht, dass die VM für jeglichen externen Datenverkehr verfügbar ist. Die Standardaktion beim Erstellen einer VM über das Portal ist das Erstellen einer neuen öffentlichen IP-Adresse. Für alle anderen Formen der Erstellung einer VM, z.B. per PowerShell, Azure-Befehlszeilenschnittstelle oder Resource Manager-Vorlage, wird nicht automatisch eine öffentliche IP-Adresse erstellt, sofern dies nicht explizit angefordert wird. Die Standardaktion über das Portal umfasst auch die Erstellung einer Netzwerksicherheitsgruppe. Es sollte also nicht zu einer Situation mit einer verfügbar gemachten VM kommen, für die keine Netzwerkfilterung vorhanden ist.
> 
> 

## Grundlegendes zu Load Balancern und NAT-Regeln
Beim klassischen Bereitstellungsmodell konnten Sie Endpunkte erstellen, die auch die Portweiterleitung durchgeführt haben. Beim Erstellen einer VM im klassischen Bereitstellungsmodell werden ACL-Regeln für RDP oder SSH automatisch erstellt. TCP-Port 3389 bzw. TCP-Port 22 werden aber nicht für die Außenwelt verfügbar gemacht. Stattdessen wird ein TCP-Port mit hohem Wert verfügbar gemacht, der dem entsprechenden internen Port zugeordnet ist. Auf ähnliche Weise können Sie auch Ihre eigenen ACL-Regeln erstellen, z.B. die Verfügbarmachung eines Webservers über TCP-Port 4280 für die Außenwelt. Diese ACL-Regeln und Portzuordnungen sind im folgenden Screenshot aus dem klassischen Portal dargestellt:

![Portweiterleitung mit klassischen Endpunkten](./media/virtual-machines-common-endpoints-in-resource-manager/classic-endpoints-port-forwarding.png)

Bei Netzwerksicherheitsgruppen wird diese Funktion für die Portweiterleitung von einem Load Balancer übernommen. Weitere Informationen finden Sie unter [Load Balancer in Azure](../articles/load-balancer/load-balancer-overview.md). Ein Beispiel für einen Load Balancer mit einer NAT-Regel zum Durchführen der Portweiterleitung von TCP-Port 4222 zum internen TCP-Port 22 einer VM ist im folgenden Screenshot aus dem Portal dargestellt:

![Load Balancer-NAT-Regeln für Portweiterleitung](./media/virtual-machines-common-endpoints-in-resource-manager/load-balancer-nat-rules.png)

> [!NOTE]
> Wenn Sie einen Load Balancer implementieren, weisen Sie der VM selbst normalerweise keine öffentliche IP-Adresse zu. Stattdessen wird dem Load Balancer eine öffentliche IP-Adresse zugewiesen. Sie müssen trotzdem noch die Netzwerksicherheitsgruppe und ACL-Regeln erstellen, um den ein- und ausgehenden Fluss des Datenverkehrs der VM zu definieren. Mit den NAT-Regeln des Load Balancers wird lediglich definiert, welche Ports über den Load Balancer zugelassen sind und wie sie auf die Back-End-VMs verteilt werden. Sie müssen also eine NAT-Regel für den Fluss des Datenverkehrs durch den Load Balancer und dann eine ACL-Regel für die Netzwerksicherheitsgruppe erstellen, damit der Datenverkehr die VM auch tatsächlich erreicht.
> 
> 

<!---HONumber=AcomDC_0810_2016-->