<properties
  pageTitle="Verwenden von Azure DNS mit anderen Azure-Diensten | Microsoft Azure"
  description="Grundlegendes zum Verwenden von Azure DNS zum Auflösen von Namen für andere Azure-Dienste"
  services="dns"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure dns"
/>
<tags
  ms.service="dns"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="09/21/2016"
  ms.author="sewhee"
/>

# Verwenden von Azure DNS mit anderen Azure-Diensten

Azure DNS ist ein gehosteter Dienst für die DNS-Verwaltung und -Namensauflösung. Der Dienst ermöglicht Ihnen das Erstellen öffentlicher DNS-Namen für andere Anwendungen und Dienste, die Sie in Azure bereitgestellt haben. Das Erstellen eines Namens für einen Azure-Dienst in Ihrer benutzerdefinierten Domäne ist so einfach wie das Hinzufügen eines Datensatzes mit dem richtigen Typ für Ihren Dienst.

* Für dynamisch zugewiesene IP-Adressen müssen Sie einen DNS CNAME-Eintrag erstellen, der dem DNS-Namen zugeordnet ist, den Azure für Ihren Dienst erstellt hat. DNS-Standards hindern Sie am Verwenden eines CNAME-Eintrags für die oberste Ebene der Zone.
* Für statisch zugewiesene IP-Adressen können Sie einen DNS A-Eintrag mit einem beliebigen Namen erstellen, einschließlich eines _Domänennamens ohne „www“_ auf der obersten Ebene der Zone.

Die folgende Tabelle enthält die unterstützten Eintragstypen, die für verschiedene Azure-Dienste verwendet werden können. Wie Sie in dieser Tabelle erkennen können, unterstützt Azure DNS nur DNS-Einträge für Netzwerkressourcen mit Internetzugriff. Azure DNS kann nicht für die Namensauflösung interner, privater Adressen verwendet werden.

| Azure-Dienst | Netzwerkschnittstelle | Beschreibung |
|---------------|-------------------|-------------|
| Application Gateway | Öffentliche IP-Adresse des Front-Ends | Sie können einen DNS A- oder CNAME-Eintrag erstellen. |
| Lastenausgleichsmodul | Öffentliche IP-Adresse des Front-Ends | Sie können einen DNS A- oder CNAME-Eintrag erstellen. Der Load Balancer kann eine öffentliche IPv6-Adresse haben, die dynamisch zugewiesen wird. Daher müssen Sie einen CNAME-Eintrag für eine IPv6-Adresse erstellen. |
| Traffic Manager | Öffentlicher Name | Sie können nur einen CNAME-Eintrag erstellen, der dem „trafficmanager.net“-Namen des Traffic Manager-Profils zugewiesen ist. Weitere Informationen finden Sie unter [Funktionsweise von Traffic Manager](../traffic-manager/traffic-manager-how-traffic-manager-works.md#traffic-manager-example). |
| Clouddienst | Öffentliche IP-Adresse | Für statisch zugewiesene IP-Adressen können Sie einen DNS A-Eintrag erstellen. Für dynamisch zugewiesenen IP-Adressen müssen Sie einen CNAME-Eintrag erstellen, der dem _cloudapp.net_-Namen zugeordnet ist. Diese Regel gilt für virtuelle Computer, die im klassischen Portal erstellt werden, da sie als Clouddienst bereitgestellt werden. Weitere Informationen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens in Cloud Services](../cloud-services/cloud-services-custom-domain-name-portal.md). |
| App Service | Externe IP-Adresse | Für externe IP-Adressen können Sie einen DNS A-Eintrag erstellen. Andernfalls müssen Sie einen CNAME-Eintrag erstellen, der dem „azurewebsites.net“-Namen zugeordnet ist. Weitere Informationen finden Sie unter [Zuordnen eines benutzerdefinierten Domänennamens zu einer Azure-App](../app-service-web/web-sites-custom-domain-name.md). |
| Resource Manager-VMs | Öffentliche IP-Adresse | Resource Manager-VMs können öffentliche IP-Adressen haben. Eine VM mit einer öffentlichen IP-Adresse befindet sich möglicherweise hinter einem Load Balancer. Sie können einen DNS A- oder CNAME-Eintrag für die öffentliche IP-Adresse erstellen. Dieser benutzerdefinierte Name kann verwendet werden, um die VIP-Adresse des Lastenausgleichs zu umgehen. |
| Klassische virtuelle Computer | Öffentliche IP-Adresse | Mithilfe von PowerShell oder der CLI erstellte klassische VMs können mit einer dynamischen oder statischen (reservierten) virtuellen IP-Adresse konfiguriert werden. Sie können einen DNS-Eintrag des Typs „CNAME“ oder „A“ erstellen. |

<!---HONumber=AcomDC_0928_2016-->