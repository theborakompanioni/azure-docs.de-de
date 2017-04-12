---
title: "Erstellen eines FQDN für Windows-VMs im Azure-Portal | Microsoft-Dokumentation"
description: "Enthält Informationen zum Erstellen eines vollqualifizierten Domänennamens (FQDN) für einen Resource Manager-basierten virtuellen Computer im Azure-Portal."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a2ae5887-76df-485e-ae19-0efd96df8600
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/14/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 3feec792f906d7d0c6b12d22e1b8baa3f79a5c77
ms.lasthandoff: 03/31/2017


---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-windows-vm"></a>Erstellen eines vollständig qualifizierten Domänennamens im Azure-Portal für eine Windows-VM

Beim Erstellen eines virtuellen Computers (VM, Virtual Machine) im [Azure-Portal](https://portal.azure.com) mit dem Resource Manager-Bereitstellungsmodell wird automatisch eine öffentliche IP als Ressource für den virtuellen Computer erstellt. Mit dieser IP-Adresse greifen Sie per Remotezugriff auf den virtuellen Computer zu. Obwohl das Portal standardmäßig keinen [vollqualifizierten Domänennamen](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)(Fully Qualified Domain Name, FQDN) erstellt, können Sie nach der Erstellung des virtuellen Computers einen solchen erstellen. Dieser Artikel demonstriert die einzelnen Schritte, um einen DNS-Namen oder einen FQDN zu erstellen.

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Sie können jetzt mithilfe dieses DNS-Namens eine Remoteverbindung mit dem virtuellen Computer herstellen, z.B. für RDP (Remote Desktop Protocol, Remotedesktopprotokoll).

## <a name="next-steps"></a>Nächste Schritte
Ihr virtueller Computer verfügt nun über eine öffentliche IP und einen DNS-Namen. Als Nächstes können Sie allgemeine Anwendungsframeworks oder Dienste bereitstellen, z.B. IIS, SQL oder SharePoint.

Außerdem können Sie sich ausführlicher [über die Verwendung von Resource Manager informieren](../../azure-resource-manager/resource-group-overview.md), um Hinweise zum Erstellen Ihrer Azure-Bereitstellungen zu erhalten.


