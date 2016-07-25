<properties
   pageTitle="Red Hat-Updateinfrastruktur (RHUI) | Microsoft Azure"
   description="Hier finden Sie Informationen zur Red Hat-Updateinfrastruktur (RHUI) für On-Demand-Red Hat Enterprise Linux-Instanzen in Microsoft Azure."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="BorisB2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="07/11/2016"
   ms.author="borisb"/>

# Red Hat-Updateinfrastruktur (RHUI) für virtuelle On-Demand-Red Hat Enterprise Linux-Computer in Azure

Virtuelle Computer, die auf der Grundlage der über den Azure Marketplace erhältlichen On-Demand-RHEL-Images (Red Hat Enterprise Linux) erstellt werden, werden für den Zugriff auf die in Azure bereitgestellte Red Hat-Updateinfrastruktur (RHUI) registriert. Die On-Demand-RHEL-Instanzen haben Zugriff auf ein regionales yum-Repository und können inkrementelle Updates empfangen.

Die von RHUI verwaltete yum-Repositoryliste wird während der Bereitstellung in der RHEL-Instanz konfiguriert. Daher ist keine zusätzliche Konfiguration erforderlich. Führen Sie einfach `yum update` aus, wenn die RHEL-Instanz ausgeführt wird, um die neuesten Updates zu erhalten.

## RHUI – Übersicht
[Red Hat Update Infrastructure](https://access.redhat.com/products/red-hat-update-infrastructure) bietet eine hoch skalierbare Lösung zum Verwalten von yum-Repositoryinhalten für Red Hat Enterprise Linux-Cloudinstanzen, die von für Red Hat zertifizierten Cloudanbietern gehostet werden. RHUI basiert auf dem Pulp-Upstreamprojekt und ermöglicht Cloudanbietern, die in Red Hat gehosteten Repositoryinhalte lokal zu spiegeln, benutzerdefinierte Repositorys mit eigenen Inhalten zu erstellen und diese Repositorys über ein Inhaltsübermittlungssystem mit Lastenausgleich einer großen Gruppe von Endbenutzern zur Verfügung zu stellen.

## Regionen, in denen RHUI verfügbar ist
RHUI ist in allen Regionen verfügbar, in denen RHEL-On-Demand-Images verfügbar sind. Dazu zählen derzeit alle öffentlichen Regionen, die auf der Seite [Azure-Status](https://azure.microsoft.com/status/) angegeben sind. Bei virtuellen Computern, die auf der Grundlage von On-Demand-RHEL-Images bereitgestellt werden, ist der RHUI-Zugriff im Preis inbegriffen. Die regionale/nationale Cloudverfügbarkeit wird im Zuge des Ausbaus der RHEL-On-Demand-Verfügbarkeit aktualisiert.

> [AZURE.NOTE] Der Zugriff auf in Azure gehostete RHUI ist auf virtuelle Computer innerhalb der [IP-Bereiche des Microsoft Azure-Datencenters](https://www.microsoft.com/download/details.aspx?id=41653) beschränkt.

## Abrufen von Updates aus einem anderen Updaterepository

Wenn Sie Updates nicht aus der in Azure gehosteten RHUI, sondern aus einem anderen Updaterepository abrufen müssen, müssen Sie die RHUI-Registrierung Ihrer Instanzen aufheben und sie stattdessen bei der gewünschten Updateinfrastruktur registrieren (beispielsweise bei Red Hat Satellite oder Red Hat Customer Portal CDN). Für diese Dienste und die Registrierung für [Red Hat Cloud Access in Azure](https://access.redhat.com/ecosystem/partners/ccsp/microsoft-azure) benötigen Sie geeignete Red Hat-Abonnements.

Gehen Sie wie folgt vor, um die Registrierung von RHUI aufzuheben und RHUI anschließend für Ihre Updateinfrastruktur zu registrieren:

1.	Bearbeiten Sie „/etc/yum.repos.d/rh-cloud.repo“, und ändern Sie alle Vorkommen von `enabled=1` in `enabled=0`. Beispiel:

        # sed -i 's/enabled=1/enabled=0/g' /etc/yum.repos.d/rh-cloud.repo

2.	Bearbeiten Sie „/etc/yum/pluginconf.d/rhnplugin.conf“, und ändern Sie `enabled=0` in `enabled=1`.
3.	Führen Sie anschließend die Registrierung bei der gewünschte Infrastruktur durch (beispielsweise beim Red Hat-Kundenportal). Informationen zum Registrieren eines Systems sowie zum Abonnieren des Red Hat-Kundenportals finden Sie im [Red Hat-Lösungshandbuch](https://access.redhat.com/solutions/253273).

> [AZURE.NOTE] Der Zugriff auf die in Azure-gehostete RHUI ist im Preis für das RHEL-Image mit nutzungsbasierter Bezahlung (Pay-As-You-Go, PAYG) enthalten. Da der virtuelle Computer durch die Aufhebung der Registrierung eines virtuellen PAYG-RHEL-Computers bei der in Azure gehosteten RHUI nicht in einen virtuellen BYOL-Computer (Bring-Your-Own-License) konvertiert wird, fallen unter Umständen doppelte Gebühren an, wenn Sie den gleichen virtuellen Computer bei einer anderen Updatequelle registrieren.
> 
> Wenn Sie anstelle der in Azure gehosteten RHUI durchweg eine andere Infrastruktur verwenden müssen, empfiehlt es sich unter Umständen, eigene (BYOL-)Images zu erstellen und bereitzustellen. Eine entsprechende Beschreibung finden Sie im Artikel [Vorbereiten eines auf Red Hat basierenden virtuellen Computers für Azure](virtual-machines-linux-redhat-create-upload-vhd.md).

## Nächste Schritte
Wenn Sie einen virtuellen Red Hat Enterprise Linux-Computer auf der Grundlage eines Azure Marketplace-Images mit nutzungsbasierter Bezahlung erstellen und die in Azure gehostete RHUI verwenden möchten, besuchen Sie den [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/redhat/). `yum update` kann in der RHEL-Instanz ohne zusätzliche Einrichtung verwendet werden.

<!---HONumber=AcomDC_0713_2016-->