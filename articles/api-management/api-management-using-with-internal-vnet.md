---
title: Verwenden von Azure API Management mit internen virtuellen Netzwerken | Microsoft-Dokumentation
description: Erfahren Sie, wie Azure API Management in einem internen virtuellen Netzwerk eingerichtet und konfiguriert wird.
services: api-management
documentationcenter: 
author: vladvino
manager: kjoshi
editor: 
ms.assetid: dac28ccf-2550-45a5-89cf-192d87369bc3
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.translationtype: HT
ms.sourcegitcommit: 07e5e15f4f4c4281a93c8c3267c0225b1d79af45
ms.openlocfilehash: a4c2bda1226ca05c775d011fba7bc59d4dab8998
ms.contentlocale: de-de
ms.lasthandoff: 08/31/2017

---
# <a name="using-azure-api-management-service-with-internal-virtual-network"></a>Verwenden von Azure API Management mit internen virtuellen Netzwerken
Mit virtuellen Azure-Netzwerken (VNETs) kann API Management APIs verwalten, auf die nicht über das Internet zugegriffen werden kann. Verschiedene VPN-Technologien stehen zum Herstellen der Verbindung zur Verfügung, und API Management kann in zwei Hauptmodi im virtuellen Netzwerk bereitgestellt werden:
* Extern
* Intern

## <a name="overview"> </a>Übersicht
Wenn API Management im Modus eines internen virtuellen Netzwerks bereitgestellt wird, sind alle Dienstendpunkte (Gateway, Entwicklerportal, Herausgeberportal, direkte Verwaltung und Git) nur in einem virtuellen Netzwerk sichtbar, auf das Sie den Zugriff steuern. Keiner der Dienstendpunkte ist auf dem öffentlichen DNS-Server registriert.

Mit API Management im internen Modus können Sie die folgenden Szenarien erreichen
* APIs, die in Ihrem privaten Datencenter gehostet werden, können externen Dritten über Site-to-Site- oder ExpressRoute-VPN-Verbindungen sicher zugänglich gemacht werden.
* Durch Verfügbarmachen von cloudbasierten und lokalen APIs über ein gemeinsames Gateway können Hybrid Cloud-Szenarien ermöglicht werden.
* APIs, die in mehreren geografischen Standorten gehostet werden, können mit einem einzelnen Gatewayendpunkt verwaltet werden. 

## <a name="enable-vpn"> </a>Einrichten von API Management in einem internen virtuellen Netzwerk
Der API Management-Dienst in einem internen virtuellen Netzwerk wird hinter einem internen Lastenausgleich (Internal Load Balancer, ILB) gehostet. Die IP-Adresse des ILB liegt im [RFC1918](http://www.faqs.org/rfcs/rfc1918.html)-Bereich.  

### <a name="enable-vnet-connection-using-azure-portal"></a>Aktivieren der VNET-Verbindung über das Azure-Portal
Erstellen Sie zunächst den API Management-Dienst, indem Sie die Schritte zum [Erstellen des API Management-Diensts][Create API Management service] ausführen. Richten Sie dann API Management so ein, dass der Dienst in einem virtuellen Netzwerk bereitgestellt wird.

![Menü für die Einrichtung von APIM in einem internen virtuellen Netzwerk][api-management-using-internal-vnet-menu]

Nach erfolgreicher Bereitstellung sollte die interne virtuelle IP-Adresse des Diensts auf dem Dashboard angezeigt werden.

![API Management-Dashboard mit konfiguriertem internen VNET][api-management-internal-vnet-dashboard]

### <a name="enable-vnet-connection-using-powershell-cmdlets"></a>Aktivieren der VNET-Verbindung mithilfe von PowerShell-Cmdlets
Sie können VNET-Verbindungen auch mithilfe der PowerShell-Cmdlets aktivieren.

* **Erstellen eines API Management-Diensts in einem VNET**: Verwenden Sie das Cmdlet [New-AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement), um einen Azure API Management-Dienst in einem VNET zu erstellen und so zu konfigurieren, dass als Typ ein internes VNET verwendet wird.

* **Bereitstellen eines vorhandenen API Management-Diensts in einem VNET**: Verwenden Sie das Cmdlet [Update-AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment), um einen vorhandenen Azure API Management-Dienst in ein virtuelles Netzwerk zu verschieben und so zu konfigurieren, dass als Typ ein internes VNET verwendet wird.

## <a name="apim-dns-configuration"></a>DNS-Konfiguration
Bei Verwendung von API Management im Modus eines externen virtuellen Netzwerks wird DNS von Azure verwaltet. Beim Modus eines internen virtuellen Netzwerks müssen Sie DNS selbst verwalten.

> [!NOTE]
> Der API Management-Dienst lauscht nicht auf Anforderungen für IP-Adressen. Er reagiert nur auf Anforderungen für den Hostnamen, der auf den Dienstendpunkten konfiguriert ist (einschließlich Gateway, Entwicklerportal, Herausgeberportal, Endpunkt für die direkte Verwaltung und Git).

### <a name="access-on-default-host-names"></a>Zugriff auf Standardhostnamen:
Wenn Sie z.B. einen API Management-Dienst in einer öffentlichen Azure-Cloud mit dem Namen „Contoso“ erstellen, werden standardmäßig die folgenden Dienstendpunkte konfiguriert.

>   Gateway/Proxy: contoso.azure-api.net

> Herausgeberportal und Entwicklerportal: contoso.portal.azure-api.net

> Endpunkt für die direkte Verwaltung: contoso.management.azure-api.net

>   Git: contoso.scm.azure-api.net

Für den Zugriff auf diese API Management-Dienstendpunkte können Sie einen virtuellen Computer in einem Subnetz erstellen, das mit dem virtuellen Netzwerk verbunden ist, in dem API Management bereitgestellt wurde. Wenn die interne virtuelle IP-Adresse für Ihren Dienst 10.0.0.5 ist, können Sie die folgende Zuordnung in der Datei „hosts“ (% SystemDrive%\drivers\etc\hosts) vornehmen:

> 10.0.0.5    contoso.azure-api.net

> 10.0.0.5    contoso.portal.azure-api.net

> 10.0.0.5    contoso.management.azure-api.net

> 10.0.0.5    contoso.scm.azure-api.net

Sie können dann über den virtuellen Computer, den Sie erstellt haben, auf alle Dienstendpunkte zugreifen. Wenn Sie einen benutzerdefinierten DNS-Server in einem virtuellen Netzwerk verwenden, können Sie auch DNS-A-Datensätze erstellen und auf diese Endpunkte von überall in Ihrem virtuellen Netzwerk zugreifen. 

### <a name="access-on-custom-domain-names"></a>Zugriff auf benutzerdefinierte Domänennamen:
Wenn Sie auf den API Management-Dienst nicht mit den Standardhostnamen zugreifen möchten, können Sie benutzerdefinierte Domänennamen für alle Dienstendpunkte einrichten, wie unten dargestellt.

![Einrichten einer benutzerdefinierten Domäne für API Management][api-management-custom-domain-name]

Anschließend können Sie A-Datensätze in Ihrem DNS-Server erstellen, um auf diese Endpunkte zuzugreifen, auf die nur aus dem virtuellen Netzwerk zugegriffen werden kann.

## <a name="related-content"> </a>Verwandte Inhalte
* [Häufige Probleme mit der Netzwerkkonfiguration beim Einrichten von APIM in einem VNET][Common Network Configuration Issues]
* [Häufig gestellte Fragen zu virtuellen Netzwerken](../virtual-network/virtual-networks-faq.md)
* [Erstellen eines A-Datensatzes in DNS](https://msdn.microsoft.com/en-us/library/bb727018.aspx)

[api-management-using-internal-vnet-menu]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-menu.png
[api-management-internal-vnet-dashboard]: ./media/api-management-using-with-internal-vnet/api-management-internal-vnet-dashboard.png
[api-management-custom-domain-name]: ./media/api-management-using-with-internal-vnet/api-management-custom-domain-name.png

[Create API Management service]: api-management-get-started.md#create-service-instance
[Common Network Configuration Issues]: api-management-using-with-vnet.md#network-configuration-issues

