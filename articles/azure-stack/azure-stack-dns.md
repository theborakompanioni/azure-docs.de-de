---
title: DNS in Azure Stack | Microsoft-Dokumentation
description: DNS in Azure Stack
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/27/2017
ms.author: victorh
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: 61a9955c5ee65ccbe9a8231fa394eab01cda584f
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="dns-in-azure-stack"></a>DNS in Azure Stack
Azure Stack enthält die folgenden DNS-Features:
* Unterstützung für DNS-Hostnamenauflösung
* Erstellen und Verwalten von DNS-Zonen und -Einträgen per API

## <a name="support-for-dns-hostname-resolution"></a>Unterstützung für DNS-Hostnamenauflösung
Sie können eine DNS-Domänennamenbezeichnung für eine öffentliche IP-Ressource angeben. Dadurch erstellen Sie für „*Domänennamenbezeichnung.Standort*.cloudapp.azurestack.external“ eine Zuordnung zur öffentlichen IP-Adresse auf den von Azure Stack verwalteten DNS-Servern.  

Wenn Sie beispielsweise eine öffentliche IP-Ressource mit **contoso** als Domänennamenbezeichnung und dem Speicherort „Lokaler Azure Stack“ erstellen, wird der vollqualifizierte Domänenname (FQDN) **contoso.local.cloudapp.azurestack.external** in die öffentliche IP-Adresse der Ressource aufgelöst. Mit diesem FQDN können Sie einen benutzerdefinierten CNAME-Domäneneintrag erstellen, der auf die öffentliche IP-Adresse in Azure Stack verweist.

> [!IMPORTANT]
> Jede erstellte Domänennamenbezeichnung muss für den Azure Stack-Speicherort eindeutig sein.

Wenn Sie die öffentliche IP-Adresse mit dem Portal erstellen, sieht dies wie folgt aus:

![Erstellen einer öffentlichen IP-Adresse](media/azure-stack-whats-new-dns/image01.png)

Diese Konfiguration ist nützlich, wenn Sie eine öffentliche IP-Adresse einer Ressource mit Lastenausgleich zuordnen möchten. Es kann beispielsweise sein, dass Sie über einen Lastenausgleich verfügen, der Anforderungen einer Webanwendung verarbeitet. Im Hintergrund des Lastenausgleichs befindet sich eine Website, die auf einem oder mehreren virtuellen Computern angeordnet ist. Jetzt können Sie auf die Website mit Lastenausgleich per DNS-Name zugreifen, anstatt mit einer IP-Adresse.

## <a name="create-and-manage-dns-zones-and-records-using-api"></a>Erstellen und Verwalten von DNS-Zonen und -Einträgen per API
Sie können DNS-Zonen und -Einträge in Azure Stack erstellen und verwalten.  

Azure Stack stellt einen DNS-Dienst wie unter Azure bereit, und es werden APIs verwendet, die mit den DNS-APIs von Azure konsistent sind.  Durch das Hosten Ihrer Domänen im Azure Stack-DNS stehen Ihnen für die Verwaltung Ihrer DNS-Einträge die gleichen Anmeldeinformationen, APIs und Tools wie für Ihre anderen Azure-Dienste zur Verfügung. Auch Abrechnung und Support erfolgen wie bei diesen Diensten. 

Aus naheliegenden Gründen ist die Azure Stack-DNS-Infrastruktur kompakter als die von Azure. Der Bereich, die Skalierung und die Leistung richten sich daher nach der Skalierung der Azure Stack-Bereitstellung und der dafür verwendeten Umgebung.  Aspekte wie Leistung, Verfügbarkeit, globale Verteilung und Hochverfügbarkeit können daher von Bereitstellung zu Bereitstellung variieren.

## <a name="comparison-with-azure-dns"></a>Vergleich mit dem Azure-DNS
Das DNS in Azure Stack ähnelt dem DNS in Azure, aber es gibt zwei wichtige Ausnahmen:
* **AAAA-Einträge werden nicht unterstützt**

    Azure Stack unterstützt KEINE AAAA-Einträge, weil Azure Stack keine Unterstützung von IPv6-Adressen aufweist.  Dies ist ein wichtiger Unterschied zwischen DNS in Azure und Azure Stack.
* **Nicht mehrinstanzenfähig**

    Im Gegensatz zu Azure ist der DNS-Dienst in Azure Stack nicht mehrinstanzenfähig. Es ist also nicht möglich, dass für jeden Mandanten die gleiche DNS-Zone erstellt wird. Dies ist nur für das erste Abonnement erfolgreich, mit dem versucht wird, die Zone zu erstellen. Nachfolgende Anforderungen schlagen fehl.  Es handelt sich hierbei um ein bekanntes Problem und einen wichtigen Unterschied zwischen Azure und dem Azure Stack-DNS. Dieses Problem wird in einer zukünftigen Version behoben sein.

Außerdem gibt es einige kleinere Unterschiede in Bezug darauf, wie für das Azure Stack-DNS Tags, Metadaten, ETags und Grenzwerte implementiert werden.

Die folgenden Informationen gelten speziell für das Azure Stack-DNS und weichen gegenüber dem Azure-DNS leicht ab. Weitere Informationen zum Azure-DNS finden Sie auf der Website mit der Microsoft Azure-Dokumentation unter [Übersicht über DNS-Zonen und -Einträge](../dns/dns-zones-records.md).

### <a name="tags-metadata-and-etags"></a>Tags, Metadaten und ETags

**Tags**

Das Azure Stack-DNS unterstützt die Verwendung von Azure Resource Manager-Tags in DNS-Zonenressourcen. Tags für DNS-Ressourceneintragssätze werden nicht unterstützt, aber als Alternative werden „Metadaten“ für DNS-Ressourceneintragssätze wie unten beschrieben unterstützt.

**Metadaten**

Das Azure Stack-DNS unterstützt das Hinzufügen von Ressourceneintragssätzen mithilfe von „Metadaten“ als Alternative zu Tags in Ressourceneintragssätzen. Ähnlich wie Tags ermöglichen Metadaten das Zuordnen von Name-Wert-Paare zu jedem Ressourceneintragssatz. Dies kann beispielsweise hilfreich sein, um den Zweck jedes Ressourceneintragssatzes zu dokumentieren. Anders als Tags können Metadaten nicht verwendet werden, um Ihre Azure-Rechnung zu filtern, und sie können nicht in einer Richtlinie von Azure Resource Manager angegeben werden.

**ETags**

Angenommen, zwei Personen oder zwei Prozesse versuchen, einen DNS-Eintrag zur gleichen Zeit zu ändern. Welcher hat Vorrang? Und weiß die Person, die Vorrang erhalten hat, dass sie die Änderungen einer anderen Person überschrieben hat?

Das Azure Stack-DNS verwendet ETags, um gleichzeitige Änderungen an derselben Ressource sicher verarbeiten zu können. ETags unterscheiden sich von Azure Resource Manager-Tags. Jeder DNS-Ressourcen (Zone oder Datensatzgruppe) ist ein ETag zugeordnet. Sobald eine Ressource abgerufen wird, wird auch sein ETag abgerufen. Beim Aktualisieren einer Ressource können Sie auswählen, dass die ETags wieder zurückgegeben werden sollen, damit das Azure Stack-DNS überprüfen kann, ob das ETag auf dem Server übereinstimmt. Da jedes Update einer Ressource dazu führt, dass das ETag erneut generiert wird, weist eine Nichtübereinstimmung des ETags darauf hin, dass eine gleichzeitige Änderung vorgenommen wurde. ETags können auch beim Erstellen einer neuen Ressource verwendet werden, um sicherzustellen, dass die Ressource nicht bereits vorhanden ist.

Standardmäßig verwendet das PowerShell-Modul des Azure Stack-DNS ETags, um gleichzeitige Änderungen an Zonen und Ressourceneintragssätzen zu blockieren. Der optionale Switch *-Overwrite* kann verwendet werden, um ETag-Überprüfungen zu unterdrücken. In diesem Fall werden gleichzeitig vorgenommene Änderungen überschrieben.

Auf Ebene der REST-API des Azure Stack-DNS werden ETags mithilfe von HTTP-Headern angegeben. Ihr Verhalten ist in der folgenden Tabelle angegeben:

| Header | Verhalten|
|--------|---------|
| Keine   | PUT ist immer erfolgreich (keine Etag-Prüfung)|
| If-match| PUT ist nur erfolgreich, wenn die Ressource vorhanden ist und das Etag übereinstimmt.|
| If-match *| PUT ist nur erfolgreich, wenn eine Ressource vorhanden ist.|
| If-none-match *| PUT ist nur erfolgreich, wenn die Ressource nicht vorhanden ist.|

### <a name="limits"></a>Einschränkungen

Bei Verwendung des Azure Stack-DNS gelten folgende Standardgrenzwerte:

| Ressource| Standardlimit|
|---------|--------------|
| Zonen pro Abonnement| 100|
| Datensätze pro Zone| 5.000|
| Datensätze pro Datensatzgruppe| 20|

## <a name="next-steps"></a>Nächste Schritte
[Introducing iDNS for Azure Stack](azure-stack-understanding-dns.md) (Einführung in iDNS für Azure Stack)

