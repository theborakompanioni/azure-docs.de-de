---
title: "Azure-Glossar – Azure-Wörterbuch | Microsoft-Dokumentation"
description: "Verwenden Sie das Azure-Glossar, um die Cloudterminologie für die Azure Platform zu verstehen. Dieses kompakte Azure-Wörterbuch bietet Definitionen für gängige Cloudbegriffe für Azure."
keywords: Azure dictionary, cloud terminology, Azure glossary, terminology definitions, cloud terms
services: na
documentationcenter: na
author: MonicaRush
manager: jhubbard
editor: 
ms.assetid: d7ac12f7-24b5-4bcd-9e4d-3d76fbd8d297
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: monicar
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: bcfa97c08cb55aba1ab69e67843abb81805a8884


---
# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-the-azure-platform"></a>Microsoft Azure-Glossar: Ein Wörterbuch der Cloudterminologie auf der Azure Platform
Das Microsoft Azure-Glossar ist ein kompaktes Wörterbuch der Cloudterminologie für die Azure Platform.

## <a name="find-service-definitions-and-other-cloud-terms"></a>Auffinden von Dienstdefinitionen und anderen Cloudbegriffen
* Definitionen der Azure-Dienste und ihrer AWS-Entsprechungen finden Sie unter [Microsoft Azure und Amazon Web Services](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).
* Eine Liste allgemeiner Begriffe aus der Cloudbranche finden Sie unter [Cloud Computing-Terminologie](https://azure.microsoft.com/overview/cloud-computing-dictionary/).

Das Azure-Glossar sowie die zwei oben genannten Referenzen bieten eine umfassende Taxonomie für Azure und die Cloudbranche.  

## <a name="azure-glossary-list"></a>Azure-Glossarliste
### <a name="a-nameaccountaaccount"></a><a name="account"></a>Konto
Ein Geschäfts- oder Schulkonto oder ein persönliches Microsoft-Konto, das für den Zugriff und die Verwaltung eines Azure-Abonnements verwendet wird.  
Siehe auch [Beziehung zwischen Azure-Abonnements und Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md)

### <a name="a-nameavailability-setaavailability-set"></a><a name="availability-set"></a>Verfügbarkeitsgruppe
Eine Sammlung virtueller Computer, die gemeinsam verwaltet werden, um Anwendungsredundanz und -zuverlässigkeit zu bieten. Durch die Verwendung einer Verfügbarkeitsgruppe wird sichergestellt, dass während eines geplanten oder ungeplanten Wartungsereignisses mindestens ein virtueller Computer verfügbar ist.  
Siehe auch [Verwalten der Verfügbarkeit virtueller Windows-Computer](virtual-machines/virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) oder [Verwalten der Verfügbarkeit virtueller Linux-Computer](virtual-machines/virtual-machines-linux-manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="a-nameclassic-modelaazure-classic-deployment-model"></a><a name="classic-model"></a>Klassisches Azure-Bereitstellungsmodell
Eines der zwei [Bereitstellungsmodelle](resource-manager-deployment-model.md), die in Azure zum Bereitstellen von Ressourcen verwendet werden. (Das neue Modell ist Azure Resource Manager.) Einige Azure-Ressourcen können entweder in dem einen oder dem anderen Modell bereitgestellt werden, während andere in beiden Modellen bereitgestellt werden können. Richtlinien für einzelne Azure-Ressourcen beschreiben, mit welchem Modell bzw. welchen Modellen eine Ressource bereitgestellt werden kann.

### <a name="a-namecliaazure-command-line-interface-cli"></a><a name="cli"></a>Azure-Befehlszeilenschnittstelle (CLI)
Eine [Befehlszeilenschnittstelle](xplat-cli-install.md) , die zum Verwalten von Azure-Diensten über Windows-, OS X- und Linux-Computer verwendet werden kann.

### <a name="a-namepowershellaazure-powershell"></a><a name="powershell"></a>Azure PowerShell
Eine [Befehlszeilenschnittstelle](powershell-install-configure.md), mit der Azure-Dienste über eine Befehlszeile von einem Windows-PC aus verwaltet werden können. Einige Dienste oder Dienstfeatures können nur über PowerShell oder die CLI verwaltet werden. Richtlinien für einzelne Azure-Ressourcen beschreiben, mit welchem Modell bzw. welchen Modellen eine Ressource bereitgestellt werden kann.   
Siehe auch [Installieren und Konfigurieren von Azure PowerShell](powershell-install-configure.md).

### <a name="a-namearm-modelaazure-resource-manager-deployment-model"></a><a name="arm-model"></a>Azure Resource Manager-Bereitstellungsmodell
Eines der zwei [Bereitstellungsmodelle](resource-manager-deployment-model.md), die in Microsoft Azure zum Bereitstellen von Ressourcen verwendet werden. (Das andere ist das klassische Bereitstellungsmodell.) Einige Azure-Ressourcen können entweder in dem einen oder dem anderen Modell bereitgestellt werden, während andere in beiden Modellen bereitgestellt werden können. Richtlinien für einzelne Azure-Ressourcen beschreiben, mit welchem Modell bzw. welchen Modellen eine Ressource bereitgestellt werden kann.

### <a name="a-namefault-domainafault-domain"></a><a name="fault-domain"></a>Fehlerdomäne
Die Sammlung virtueller Computer in einer Verfügbarkeitsgruppe, die theoretisch zur selben Zeit ausfallen können. Ein Beispiel ist eine Gruppe von Computern in einem Rack, die gemeinsam eine Stromquelle und denselben Netzwerkswitch verwenden. In Azure werden die virtuellen Computer in einer Verfügbarkeitsgruppe automatisch auf mehrere Fehlerdomänen verteilt.  
Siehe auch [Verwalten der Verfügbarkeit virtueller Windows-Computer](virtual-machines/virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) oder [Verwalten der Verfügbarkeit virtueller Linux-Computer](virtual-machines/virtual-machines-linux-manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

### <a name="a-namegeoageo"></a><a name="geo"></a>Geografischer Raum
Eine definierte Grenze für den Speicherort von Daten, der typischerweise zwei oder mehr Regionen umfasst. Die Grenzen können innerhalb nationaler Grenzen liegen oder über diese hinausgehen und verschiedenen steuerlichen Bestimmungen unterliegen. Jeder geografische Raum umfasst mindestens eine Region. Beispiele für geografische Räume sind Asien-Pazifik und Japan. Auch bezeichnet als *Geografie*.  
Siehe auch [Azure-Regionen](best-practices-availability-paired-regions.md)

### <a name="a-namegeo-replicationageo-replication"></a><a name="geo-replication"></a>Georeplikation
Der Vorgang der automatischen Replikation von Inhalten wie beispielsweise Blobs, Tabellen und Warteschlangen innerhalb eines Regionspaars.  
Siehe auch [Aktive Georeplikation für Azure SQL-Datenbank](sql-database/sql-database-geo-replication-overview.md)

### <a name="a-nameimageaimage"></a><a name="image"></a>Image
Eine Datei, die das Betriebssystem und die Anwendungskonfiguration enthält, und die zum Erstellen einer beliebigen Anzahl von virtuellen Computern verwendet werden kann. In Azure können zwei Arten von Images verwendet werden: VM-Images und Betriebssystem-Images. Ein VM-Image enthält ein Betriebssystem und alle Datenträger, die an einen virtuellen Computer angefügt sind, wenn das Image erstellt wird. Ein Betriebssystem-Image enthält nur ein generalisiertes Betriebssystem ohne Datenträgerkonfigurationen.  
Siehe auch [Navigieren zu und Auswählen von Images virtueller Windows-Computer in Azure mithilfe von PowerShell oder der Befehlszeilenschnittstelle](virtual-machines/virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="a-namelimitsalimits"></a><a name="limits"></a>Einschränkungen
Die Anzahl von Ressourcen, die erstellt werden können oder der Leistungsbenchmark, der erzielt werden kann. Grenzwerte werden üblicherweise Abonnements, Diensten und Angeboten zugeordnet.  
Siehe auch [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](azure-subscription-service-limits.md)

### <a name="a-nameload-balanceraload-balancer"></a><a name="load-balancer"></a>Load Balancer
Eine Ressource, die den eingehenden Datenverkehr auf die Computer in einem Netzwerk verteilt. In Azure verteilt ein Load Balancer den Datenverkehr auf virtuelle Computer, die in einer Lastenausgleichsgruppe definiert sind. Ein [Load Balancer](load-balancer/load-balancer-overview.md) kann mit Internetverbindung oder intern eingerichtet werden.  

### <a name="a-nameofferaoffer"></a><a name="offer"></a>Angebot
Die Preise, Gutschriften und Bedingungen, die für ein Azure-Abonnement gelten.  
Siehe [Azure-Angebotsdetails](https://azure.microsoft.com/support/legal/offer-details/)

### <a name="a-nameportalaportal"></a><a name="portal"></a>Portal
Das sichere Webportal, das zum Bereitstellen und Verwalten von Azure-Diensten verwendet wird.  Es gibt zwei Portale: Das [Azure-Portal](http://portal.azure.com/) und das [klassische Portal](http://manage.windowsazure.com/). Einige Dienste sind in beiden Portalen verfügbar, während andere nur in einem der Portale zur Verfügung stehen. Im [Verfügbarkeitsdiagramm der Azure-Portale](https://azure.microsoft.com/features/azure-portal/availability/) wird aufgeführt, welche Dienste in welchem Portal zur Verfügung stehen.  

### <a name="a-nameregionaregion"></a><a name="region"></a>Region
Ein Bereich innerhalb eines geografischen Raums ohne Überschreitung nationaler Grenzen, der mindestens ein Rechenzentrum umfasst. Preise, regionale Dienste und Angebotstypen werden auf Regionsebene zur Verfügung gestellt. Eine Region bildet typischerweise ein Paar mit einer anderen Region. Diese kann sich mehrere Hundert Kilometer weit entfernt befinden. Regionspaare können als Mechanismus zur Notfallwiederherstellung und in Szenarien mit hoher Verfügbarkeit eingesetzt werden. Allgemein auch als *Standort* bezeichnet.  
Siehe auch [Azure-Regionen](best-practices-availability-paired-regions.md)

### <a name="a-nameresourcearesource"></a><a name="resource"></a>Ressource
Ein Element, das Bestandteil Ihrer Azure-Lösung ist. Jeder Azure-Dienst ermöglicht Ihnen das Bereitstellen unterschiedlicher Ressourcentypen, z. B. Datenbanken oder virtuelle Computer.   
Siehe auch [Übersicht über Azure Resource Manager](azure-resource-manager/resource-group-overview.md)

### <a name="a-nameresource-grouparesource-group"></a><a name="resource-group"></a>Ressourcengruppe
Eine Container, der verwandte Ressourcen für eine Anwendung enthält. Die Ressourcengruppe kann alle Ressourcen für eine Anwendung enthalten, oder nur die Ressourcen, die logisch gruppiert sind. Je nachdem, was für Ihre Organisation am sinnvollsten ist, können Sie entscheiden, wie Sie die Ressourcen den Ressourcengruppen zuordnen möchten.  
Siehe auch [Übersicht über Azure Resource Manager](azure-resource-manager/resource-group-overview.md)

### <a name="a-namearm-templatearesource-manager-template"></a><a name="arm-template"></a>Resource Manager-Vorlage
Eine JSON-Datei, die deklarativ eine oder mehrere Azure-Ressourcen definiert, und die Abhängigkeiten zwischen den bereitgestellten Ressourcen definiert. Die Vorlage kann zum konsistenten und wiederholten Bereitstellen der Ressourcen verwendet werden.  
Siehe auch [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md)

### <a name="a-nameresource-provideraresource-provider"></a><a name="resource-provider"></a>Ressourcenanbieter
Ein Dienst, über den Sie die Ressourcen beziehen, die Sie über Resource Manager bereitstellen und verwalten können. Jeder Ressourcenanbieter bietet Vorgänge zum Arbeiten mit den bereitgestellten Ressourcen. Auf Ressourcenanbieter kann über das Azure-Portal, Azure PowerShell und verschiedene SDKs für die Programmierung zugegriffen werden.  
Siehe auch [Übersicht über Azure Resource Manager](azure-resource-manager/resource-group-overview.md)

### <a name="a-namerolearole"></a><a name="role"></a>Rolle
Eine Möglichkeit zum Steuern des Zugriffs, die Benutzern, Gruppen und Diensten zugewiesen werden kann. Rollen können verschiedene Aktionen ausführen und so beispielsweise Azure-Ressourcen erstellen, verwalten und lesen.  
Siehe auch [RBAC: Integrierte Rollen](active-directory/role-based-access-built-in-roles.md)

### <a name="a-nameslaaservice-level-agreement-sla"></a><a name="sla"></a>Vereinbarung zum Servicelevel (SLA)
In dieser Vereinbarung ist die garantierte Verfügbarkeit und Konnektivität beschrieben, die Microsoft zusichert. Jeder Azure-Dienst verfügt über eine bestimmte Vereinbarung zum Servicelevel.  
Siehe auch [Vereinbarungen zum Servicelevel (SLAs)](https://azure.microsoft.com/support/legal/sla/)

### <a name="a-namestorage-accountastorage-account"></a><a name="storage-account"></a>Speicherkonto
Ein Speicherkonto bietet Ihnen Zugriff auf die Azure Blob-, Warteschlangen-, Tabellen- und Dateidienste in Azure Storage. Ihr Speicherkonto stellt den eindeutigen Namespace für Ihre Azure Storage-Datenobjekte bereit.  
Siehe auch [Informationen zu Azure-Speicherkonten](storage/storage-create-storage-account.md)

### <a name="a-namesubscriptionasubscription"></a><a name="subscription"></a>Abonnement
Eine Vereinbarung des Kunden mit Microsoft, die die Inanspruchnahme von Azure-Diensten ermöglicht. Die Abonnementpreise und -bedingungen richten sich nach dem ausgewählten Angebot für das Abonnement. Siehe [Microsoft Online-Abonnementvertrag](https://azure.microsoft.com/support/legal/subscription-agreement/).  
Siehe auch [Beziehung zwischen Azure-Abonnements und Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md)

### <a name="a-nametagatag"></a><a name="tag"></a>Tag
Ein Begriff aus dem Bereich der Indizierung, mit dem Sie Ressourcen gemäß Ihren Anforderungen für die Verwaltung oder Abrechnung kategorisieren können. Die Verwendung von Tags (Markierungen) kann ratsam sein, wenn Sie über eine komplexe Sammlung von Ressourcengruppen und Ressourcen verfügen und diese Ressourcen auf möglichst sinnvolle Weise visualisieren müssen. Beispielsweise können Sie Ressourcen markieren, die in Ihrer Organisation eine ähnliche Funktion haben oder zu derselben Abteilung gehören.  
Siehe auch [Verwenden von Tags zum Organisieren von Ressourcen](resource-group-using-tags.md)

### <a name="a-nameupdate-domainaupdate-domain"></a><a name="update-domain"></a>Updatedomäne
Die Sammlung virtueller Computer in einer Verfügbarkeitsgruppe, die gleichzeitig aktualisiert werden. Virtuelle Computer in derselben Updatedomäne werden während einer geplanten Wartung gemeinsam neu gestartet. Azure startet nie mehr als eine Updatedomäne gleichzeitig. Wird auch als Upgradedomäne bezeichnet.  
Siehe auch [Verwalten der Verfügbarkeit virtueller Windows-Computer](virtual-machines/virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) oder [Verwalten der Verfügbarkeit virtueller Linux-Computer](virtual-machines/virtual-machines-linux-manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

### <a name="a-namevmavirtual-machine"></a><a name="vm"></a>Virtueller Computer
Die Softwareimplementierung eines physischen Computers, der ein Betriebssystem ausführt. Auf derselben Hardware können mehrere virtuelle Computer gleichzeitig ausgeführt werden. In Azure stehen virtuelle Computer in unterschiedlichsten Größen zur Verfügung.  
Siehe auch [Dokumentation zu virtuellen Computern](https://azure.microsoft.com/documentation/services/virtual-machines/)

### <a name="a-namevm-extensionavirtual-machine-extension"></a><a name="vm-extension"></a>VM-Erweiterung
Eine Ressource, die Verhalten oder Features implementiert, die entweder andere Programme bei der Arbeit unterstützen oder Ihnen die Möglichkeit bieten, mit einem ausgeführten Computer zu interagieren. Beispielsweise können Sie die VM-Erweiterung für den Zugriff dazu verwenden, die Werte für den Remotezugriff auf einem virtuellen Azure-Computer zurückzusetzen oder zu ändern.  
Siehe auch [Informationen zu Erweiterungen und Features für virtuelle Computer (Windows)](virtual-machines/virtual-machines-windows-extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) oder [Informationen zu Erweiterungen und Features für virtuelle Computer (Linux)](virtual-machines/virtual-machines-linux-extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="a-namevnetavirtual-network"></a><a name="vnet"></a>Virtuelles Netzwerk
Ein Netzwerk, das für Konnektivität zwischen Ihren Azure-Ressourcen sorgt und von allen anderen Azure-Mandanten isoliert ist. Es kann über [Azure VPN Gateway](vpn-gateway/vpn-gateway-about-vpngateways.md) mit anderen virtuellen Azure-Netzwerken und über [verschiedene Optionen](vpn-gateway/vpn-gateway-plan-design.md) mit Ihrem lokalen Netzwerk verbunden werden. Sie können die IP-Adressblöcke, DNS-Einstellungen, Sicherheitsrichtlinien und Routentabellen in diesem Netzwerk vollständig steuern.  
Siehe auch [Übersicht über Azure Virtual Network](virtual-network/virtual-networks-overview.md)  

### <a name="see-also"></a>**Weitere Informationen**
* [Erste Schritte mit Azure](https://azure.microsoft.com/get-started/)
* [Ressourcen für Ihren Einstieg in die Cloud](https://azure.microsoft.com/resources/)  
* [Azure für Ihre Geschäftsanwendungen](https://azure.microsoft.com/overview/business-apps-on-azure/)
* [Azure in Ihrem Rechenzentrum](https://azure.microsoft.com/overview/business-apps-on-azure/)




<!--HONumber=Nov16_HO3-->


