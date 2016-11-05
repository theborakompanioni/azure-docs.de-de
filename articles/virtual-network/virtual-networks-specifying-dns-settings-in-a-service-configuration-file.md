---
title: Angeben von DNS-Einstellungen in einer Dienstkonfigurationsdatei | Microsoft Docs
description: Angeben benutzerdefinierter DNS-Einstellungen über die Dienstkonfigurationsdatei für ein virtuelles Netzwerk
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn

ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2016
ms.author: jdial

---
# Angeben von DNS-Einstellungen in einer Dienstkonfigurationsdatei
## DNS-Elemente
Eine Dienstkonfigurationsdatei kann ein DnsServers-Element mit einer Liste von IPv4-Adressen für die DNS-Server (Domain Name System) enthalten, die der Dienst verwendet. Einstellungen in der Dienstkonfigurationsdatei überschreiben die Einstellungen in der Netzwerkkonfigurationsdatei. Weitere Informationen finden Sie unter [Azure-Dienstkonfigurationsschema (.cscfg-Datei)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration-Element**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> Das **name**-Attribut im **DnsServer**-Element wird nur als Verweisname verwendet. Es stellt nicht den Hostnamen für den DNS-Server dar. Jeder **DnsServer**-Attributwert muss im gesamten Microsoft Azure-Abonnement eindeutig sein.
> 
> 

## Weitere Informationen
[Azure-Dienstkonfigurationsschema (CSCFG)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Konfigurationsschema für virtuelle Azure-Netzwerke](http://go.microsoft.com/fwlink/?LinkId=248093)

[Konfigurieren eines virtuellen Netzwerks mithilfe einer Netzwerkkonfigurationsdatei](http://go.microsoft.com/fwlink/?LinkId=248094)

[Einstellungen für virtuelle Netzwerke im Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkId=248092)

<!---HONumber=AcomDC_0810_2016-->