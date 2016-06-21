<properties
   pageTitle="Erstellen eines FQDN für einen virtuellen Computer im Azure-Portal | Microsoft Azure"
   description="Enthält Informationen zum Erstellen eines vollqualifizierten Domänennamens (FQDN) für einen Ressourcen-Manager-basierten virtuellen Computer im Azure-Portal."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="06/07/2016"
   ms.author="iainfou"/>

# Erstellen eines vollständig qualifizierten Domänennamens im Azure-Portal
Beim Erstellen eines virtuellen Computers (VM, Virtual Machine) im [Azure-Portal](https://portal.azure.com) mit dem Resource Manager-Bereitstellungsmodell wird automatisch eine öffentliche IP als Ressource für den virtuellen Computer erstellt. Mit dieser IP-Adresse können Sie per Remotezugriff auf den virtuellen Computer zugreifen. Obwohl das Portal standardmäßig keinen [vollqualifizierten Domänennamen](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (Fully Qualified Domain Name, FQDN) erstellt, ist es sehr einfach, nach der Erstellung des virtuellen Computers einen solchen zu erzeugen. Dieser Artikel demonstriert die einzelnen Schritte, um einen DNS-Namen oder einen FQDN zu erstellen.

[AZURE.INCLUDE [virtual-machines-common-portal-create-fqdn](../../includes/virtual-machines-common-portal-create-fqdn.md)]

Sie können jetzt mithilfe dieses DNS-Namens eine Remoteverbindung mit dem virtuellen Computer herstellen, z.B. mit `ssh adminuser@testdnslabel.centralus.cloudapp.azure.com`.

## Nächste Schritte
Ihr virtueller Computer verfügt nun über eine öffentliche IP und einen DNS-Namen. Als Nächstes können Sie allgemeine Anwendungsframeworks oder Dienste bereitstellen, z.B. nginx, MongoDB, Docker usw.

Außerdem können Sie sich ausführlicher [über die Verwendung von Resource Manager informieren](../resource-group-overview.md), um Hinweise zum Erstellen Ihrer Azure-Bereitstellungen zu erhalten.

<!---HONumber=AcomDC_0608_2016-->