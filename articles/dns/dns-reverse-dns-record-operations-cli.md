---
title: Mit der Azure-Befehlszeilenschnittstelle im Ressourcen-Manager Ihre Reverse-DNS-Einträge für Ihre Dienste verwalten | Microsoft Docs
description: Mit der Azure-Befehlszeilenschnittstelle im Ressourcen-Manager Ihre Reverse-DNS-Einträge oder PTR-Einträge für Ihre Dienste verwalten
services: DNS
documentationcenter: na
author: s-malone
manager: carmonm
editor: ''
tags: azure-resource-manager

ms.service: DNS
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/05/2016
ms.author: smalone

---
# Mit der Azure-Befehlszeilenschnittstelle Ihre Reverse-DNS-Einträge für Ihre Dienste verwalten
[!INCLUDE [DNS-reverse-dns-record-operations-arm-selectors-include.md](../../includes/dns-reverse-dns-record-operations-arm-selectors-include.md)]

<BR>
[!INCLUDE [DNS-reverse-dns-record-operations-intro-include.md](../../includes/dns-reverse-dns-record-operations-intro-include.md)]
<BR>
[!INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]
[classic deployment model](dns-reverse-dns-record-operations-classic-ps.md)

## Überprüfung der Reverse-DNS-Einträge
Um sicherzustellen, dass kein Dritter Reverse-DNS-Einträge erstellen kann, die Ihren Domänen zugeordnet sind, erlaubt Azure die Erstellung von Reverse-DNS-Einträgen nur, wenn Folgendes zutrifft:

* Der ReverseFqdn-Wert entspricht dem vollqualifizierte Domänennamen (FQDN) der öffentlichen IP-Adressressource, für die er angegeben wurde, oder er entspricht dem FQDN einer beliebigen öffentlichen IP-Adresse innerhalb des gleichen Abonnements (lautet also beispielsweise „contosoapp1.northus.cloudapp.azure.com.“).
* Der ReverseFqdn-Wert wird vorwärts zum Namen oder zur IP-Adresse der öffentlichen IP-Adresse aufgelöst, für die er angegeben wurde – oder zu einem FQDN einer beliebigen öffentlichen IP-Adresse bzw. zu einer IP-Adresse innerhalb des gleichen Abonnements. „ReverseFqdn“ lautet also beispielsweise „app1.contoso.com.“ (ein CName-Alias für „contosoapp1.northus.cloudapp.azure.com.“).

Überprüfungen werden nur durchgeführt, wenn die Reverse-DNS-Eigenschaft für eine öffentliche IP-Adresse eingerichtet oder geändert wird. Es finden keine regelmäßigen Neuüberprüfungen statt.

## Hinzufügen von Reverse-DNS zu vorhandenen öffentlichen IP-Adressen
Sie können mit „azure network public-ip set“ einer vorhandenen öffentlichen IP-Adresse Reverse-DNS hinzufügen:

    azure network public-ip set -n PublicIp -g NRP-DemoRG-PS -f contosoapp1.westus.cloudapp.azure.com.

Wenn Sie Reverse-DNS einer vorhandenen öffentlichen IP-Adresse hinzufügen möchten, die noch keinen DNS-Namen besitzt, müssen Sie auch einen DNS-Namen angeben. Hierzu können Sie „azure network public-ip set“ verwenden:

    azure network public-ip set -n PublicIp -g NRP-DemoRG-PS -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.

## Erstellen einer öffentlichen IP-Adresse mit Reverse-DNS
Sie können eine neue öffentliche IP-Adresse mit der angegebenen Reverse-DNS-Eigenschaft hinzufügen, indem Sie „azure network public-ip create“ ausführen:

    azure network public-ip create -n PublicIp3 -g NRP-DemoRG-PS -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.

## Anzeigen von Reverse-DNS für vorhandene öffentliche IP-Adressen
Mithilfe von „azure network public-ip show“ können Sie den konfigurierten Wert für eine vorhandene öffentliche IP-Adresse anzeigen:

    azure network public-ip show -n PublicIp3 -g NRP-DemoRG-PS

## Entfernen Sie eine Reverse-DNS aus vorhandenen öffentlichen IP-Adressen.
Mithilfe von „azure network public-ip set“ können Sie eine Reverse-DNS-Eigenschaft aus einer öffentlichen IP-Adresse entfernen. Dies erfolgt, indem der ReverseFqdn-Eigenschaftswert auf „leer“ gesetzt wird:

    azure network public-ip set -n PublicIp3 -g NRP-DemoRG-PS –f “”

[!INCLUDE [HÄUFIG GESTELLTE FRAGEN](../../includes/dns-reverse-dns-record-operations-faq-arm-include.md)]

<!---HONumber=AcomDC_0907_2016-->