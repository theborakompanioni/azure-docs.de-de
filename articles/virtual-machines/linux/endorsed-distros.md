---
title: "Unterstützte Distributionen von Linux | Microsoft Docs"
description: "Hier erfahren Sie mehr über unterstützte Linux-Verteilungen auf Azure, einschließlich Leitfäden für Ubuntu, OpenLogic, Oracle und SUSE."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: 2777a526-c260-4cb9-a31a-bdfe1a55fffc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 3a9edce4b8e7208579ee810ad40a65b22db20cac
ms.lasthandoff: 04/03/2017


---
# <a name="linux-on-azure-endorsed-distributions"></a>Linux auf von Azure unterstützten Verteilungen
> [!NOTE]
> Falls Sie einen Moment Zeit haben, würden wir uns freuen, wenn Sie an dieser [kurzen Umfrage](https://aka.ms/linuxdocsurvey) teilnehmen könnten, um zur Verbesserung der Dokumentation für virtuelle Azure-Computer unter Linux beizutragen. Jede Antwort hilft uns dabei, Sie noch besser bei Ihrer Arbeit zu unterstützen.
> 
> 

Die Linux-Images im Azure-Katalog oder Marketplace werden von verschiedenen Partnern zur Verfügung gestellt. Außerdem arbeiten wir mit verschiedenen Linux-Communitys daran, die Liste der unterstützten Verteilungen noch vielseitiger zu gestalten. In der Zwischenzeit können Sie für Verteilungen, die im Katalog nicht verfügbar sind, Ihre eigenen Linux-Images bereitstellen („Bring Your Own“), indem Sie die Richtlinien [auf dieser Seite](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) befolgen.

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="supported-distributions--versions"></a>Unterstützte Verteilungen und Versionen
In der folgenden Tabelle finden Sie die auf Azure unterstützten Linux-Verteilungen und -Versionen. Weitere Informationen finden Sie unter [Unterstützung für Linux-Images in Microsoft Azure](https://support.microsoft.com/en-us/kb/2941892).

Die Treiber für die Linux-Integrationsdienste (Linux Integration Services, LIS) für Hyper-V und Azure sind Kernelmodule, die Microsoft direkt für den Linux-Upstream-Kernel bereitstellt.  Die LIS-Treiber sind entweder standardmäßig in den Kernel der Verteilung integriert oder stehen für ältere RHEL/CentOS-basierte Verteilungen [hier](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)als separater Download zur Verfügung.  Weitere Informationen zu LIS-Treibern finden Sie [in diesem Artikel](create-upload-generic.md#linux-kernel-requirements) .

Der Azure Linux-Agent ist bereits in den Images im Azure-Katalog vorinstalliert und i. d. R. im Paketrepository der Distribution verfügbar.  Quellcode finden Sie unter [GitHub](https://github.com/azure/walinuxagent).

| Verteilung | Version | Treiber | Agent |
| --- | --- | --- | --- |
| CentOS by OpenLogic |CentOS 6.3+, 7.0+ |CentOS 6.3: [LIS-Download](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4+: Im Kernel |Paket: Im [Open Logic-Repository](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) unter „WALinuxAgent“ <br/>Quellcode: [GitHub](https://github.com/Azure/WALinuxAgent) |
| [CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) |494.4.0+ |Im Kernel |Quellcode: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent) |
| Debian |Debian 7.9+, 8.2+ |Im Kernel |Paket: Im Repository unter „waagent“  <br/>Quellcode: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Oracle Linux |6.4+, 7.0+ |Im Kernel |Paket: Im Repository unter „WALinuxAgent“  <br/>Quellcode: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998) |
| Red Hat Enterprise Linux |RHEL 6.7+, 7.1+ |Im Kernel |Paket: Im Repository unter „WALinuxAgent“  <br/>Quellcode: [GitHub](https://github.com/Azure/WALinuxAgent) |
| SUSE Linux Enterprise |SLES/SLES für SAP<br>11 SP4<br>12 SP1 oder höher|Im Kernel |Paket:<p> Für 11 im [Cloud:Tools](https://build.opensuse.org/project/show/Cloud:Tools)-Repository<br>Für 12 im Modul „Public Cloud“ unter „python-azure-agent“ enthalten<br/>Quellcode: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998) |
| openSUSE |openSUSE Leap 42.1+ |Im Kernel |Paket: Im [Cloud:Tools](https://build.opensuse.org/project/show/Cloud:Tools)-Repository unter „python-azure-agent“ <br/>Quellcode: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Ubuntu |Ubuntu 12.04, 14.04, 16.04, 16.10 |Im Kernel |Paket: Im Repository unter „WALinuxAgent“  <br/>Quellcode: [GitHub](https://github.com/Azure/WALinuxAgent) |

## <a name="partners"></a>Partner
### <a name="openlogic"></a>OpenLogic
[http://www.openlogic.com/azure](http://www.openlogic.com/azure)

OpenLogic ist ein führender Anbieter von Open Source-Lösungen für Unternehmen für die Cloud und das Rechenzentrum. OpenLogic hilft Hunderten führenden Unternehmen in zahlreichen Industriezweigen beim sicheren Erwerb, der Unterstützung sowie Steuerung von Open Source-Software. OpenLogic bietet handelsüblichen technischen Support und Schadenersatzleistungen für 600 von der OpenLogic Expert Community unterstützte Open Source-Pakete, einschließlich professionellem Support für CentOS, und ist zugleich der Launch-Partner für die Bereitstellung von CentOS-Images unter Azure.

### <a name="coreos"></a>CoreOS
[https://coreos.com/docs/running-coreos/cloud-providers/azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

Von der CoreOS-Website:

*CoreOS steht für Sicherheit, Konsistenz und Zuverlässigkeit. CoreOS installiert Paketen nicht über yum oder apt, sondern verwaltet Ihre Dienste mithilfe von Linux-Containern mit einem höheren Maß an Abstraktion. Der Code eines einzelnen Diensts und alle Abhängigkeiten sind in einem Container verpackt, der auf einem oder mehreren CoreOS-Computern ausgeführt werden kann.*

### <a name="credativ"></a>Credativ
[http://www.credativ.de/blog/debian-images-f%C3%BCr-microsoft-azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ ist ein unabhängiges Beratungs- und Dienstleistungsunternehmen, das sich auf die Entwicklung und Implementierung von professionellen Lösungen mithilfe von kostenloser Software spezialisiert hat. Als führender Open Source-Experte verfügt Credative über internationale Anerkennung bei zahlreichen IT-Abteilungen, die den Credative-Support nutzen. Gemeinsam mit Microsoft bereitet Credativ derzeit entsprechende Debian-Images für Debian 8 (Jessie) und Debian vor Version 7 (Wheezy) vor. Diese wurden speziell für die Ausführung auf Azure entwickelt und können einfach über die Plattform verwaltet werden. Über die eigenen Open Source Support Center unterstützt Credativ außerdem die langfristige Wartung und Aktualisierung der Debian-Images für Azure.

### <a name="oracle"></a>Oracle
[http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Die Strategie von Oracle besteht darin, ein weitgefasstes Portfolio von Lösungen für öffentliche und private Clouds anzubieten und den Kunden zugleich Wahlmöglichkeiten und Flexibilität bei der Art und Weise zu geben, wie sie Oracle-Software in Oracle-Clouds und anderen Clouds bereitstellen.  Aufgrund der Partnerschaft zwischen Oracle und Microsoft haben Kunden die Möglichkeit, Oracle-Software in öffentlichen und privaten Clouds von Microsoft bereitzustellen und können dabei auf die Zertifizierung und den Support von Oracle vertrauen.  Das Engagement von Oracle und die Investition in öffentliche und private Cloudlösungen von Oracle bleibt unverändert.

### <a name="red-hat"></a>Red Hat
[http://www.redhat.com/en/partners/strategic-alliance/microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

Als weltweit führender Anbieter von Open Source-Lösungen unterstützt Red Hat mehr als 90 % der Fortune 500-Unternehmen dabei, geschäftliche Probleme zu lösen, ihre IT- und Unternehmensstrategien abzustimmen und sich auf die Zukunft der Technologie vorzubereiten. Red Hat bietet dazu sichere Lösungen über ein offenes Geschäftsmodell und ein kostengünstiges, zuverlässiges Abonnementmodell an.

### <a name="suse"></a>SUSE
[http://www.suse.com/suse-linux-enterprise-server-on-azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server unter Azure ist eine bewährte Plattform, die hervorragende Zuverlässigkeit und Sicherheit für Cloud Computing bietet. Die vielseitige Linux-Plattform SUSE lässt sich nahtlos in Azure-Cloud-Dienste integrieren, um eine einfach zu verwaltende Cloudumgebung bereitzustellen. Mit mehr als 9.200 zertifizierten Anwendungen von über 1.800 unabhängigen Softwareanbietern für SUSE Linux Enterprise Server stellt SUSE zudem sicher, dass vorhandene im Rechenzentrum unterstützte Arbeitsauslastungen vertrauensvoll unter Azure bereitgestellt werden können.

### <a name="canonical"></a>Canonical
[http://www.ubuntu.com/cloud/azure](http://www.ubuntu.com/cloud/azure)

Engineering und Open Community Governance von Canonical steigern den Erfolg von Ubuntu bei Clients, Servern und Cloud Computing, einschließlich persönlicher Clouddienste für Endkunden. Canonicals Vision einer einheitlichen kostenlosen Plattform in Ubuntu, vom Telefon bis hin zur Cloud, mit einer Reihe kohärenter Schnittstellen für Telefon, Tablet, TV und Desktop macht Ubuntu zur ersten Wahl für vielfältige Institutionen – von öffentlichen Cloudanbietern bis hin zu Herstellern von Unterhaltungselektronik – sowie zu einem Favoriten für individuelle Techniker.

Mit Entwicklern und Engineering Centers überall auf der Welt ist Canonical hervorragend positioniert, um Partnerschaften mit Hardwareherstellern, Inhaltsanbietern und Softwareentwicklern einzugehen, um Ubuntu-Lösungen auf den Markt zu bringen – von PCs bis hin zu Servern und portablen Geräten.


