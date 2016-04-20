<properties
   pageTitle="Erstellen eines Ressourceneintragssatzes und von Einträgen für eine DNS-Zone mit dem Azure-Portal | Microsoft Azure"
   description="Erstellen von Hosteinträgen für Azure DNS und Erstellen von Ressourceneintragssätzen und Einträgen mit dem Azure-Portal"
   services="dns"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="cherylmc"/>


# Erstellen von DNS-Einträgen und -Ressourceneintragssätzen mit dem Azure-Portal


> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-recordset-portal.md)
- [PowerShell](dns-getstarted-create-recordset.md)
- [Azure-Befehlszeilenschnittstelle](dns-getstarted-create-recordset-cli.md)


Nach dem Erstellen der DNS-Zone müssen Sie die DNS-Einträge für Ihre Domäne hinzufügen. Zu diesem Zweck müssen Sie zunächst Grundlegendes zu DNS-Einträgen und Datensatzgruppen verstehen.

## Grundlegendes zu Datensatzgruppen und Einträgen

### Informationen zu Einträgen

Jeder DNS-Eintrag hat einen Namen und einen Typ.

Ein „vollqualifizierter“ Domänenname (FQDN) beinhaltet den Zonennamen, ein „relativer“ Name hingegen nicht. Der relative Eintragsname „www“ in der Zone „contoso.com“ gibt beispielsweise den vollqualifizierten Eintragsnamen „www.contoso.com“ an.

>[AZURE.NOTE] Einträge in Azure DNS werden mit relativen Namen angegeben.

Es gibt verschiedene Eintragstypen, je nach den darin enthaltenen Daten. Der häufigste Typ ist ein „**A**“-Eintrag, der einen Namen einer IPv4-Adresse zuordnet. Ein weiterer Typ ist ein „**MX**“-Eintrag, der einen Namen einem Mailserver zuordnet.

Azure DNS unterstützt alle allgemeine DNS-Eintragstypen: A, AAAA, CNAME, MX, NS, SOA, SRV und TXT. Beachten Sie, dass SPF-Einträge mit dem TXT-Eintragstyp erstellt werden sollten. Weitere Informationen finden Sie auf [dieser Seite](http://tools.ietf.org/html/rfc7208#section-3.1).


### Informationen zu Ressourceneintragssätzen

In einigen Fällen müssen Sie mehrere DNS-Einträge mit einem bestimmten Namen und Typ erstellen. Angenommen, die Website "www.contoso.com" wird auf zwei verschiedenen IP-Adressen gehostet. Dazu sind zwei verschiedene A-Einträge erforderlich, einer für jede IP-Adresse. Dies ist ein Beispiel für eine Datensatzgruppe.

	www.contoso.com.		3600	IN	A	134.170.185.46
	www.contoso.com.		3600	IN	A	134.170.188.221

Azure DNS verwaltet die DNS-Einträge mithilfe von Datensatzgruppen. Eine Datensatzgruppe ist die Auflistung von DNS-Einträgen in einer Zone mit dem gleichen Namen und dem gleichen Typ. Die meisten Datensatzgruppen enthalten einen einzelnen Eintrag, aber Beispiele wie das obige, in dem eine Datensatzgruppe mehr als einen Eintrag enthält, sind nicht ungewöhnlich.

Ressourceneintragssätze vom Typ SOA und CNAME stellen eine Ausnahme dar; die DNS-Standards lassen für diese Typen keine Mehrfacheinträge mit gleichem Namen zu.

Die Gültigkeitsdauer (TTL oder Time-to-Live) gibt an, wie lange jeder Eintrag von den Clients zwischengespeichert wird, bevor er erneut abgefragt wird. Im obigen Beispiel ist der TTL-Wert 3600 Sekunden oder 1 Stunde. Der TTL-Wert wird für die Datensatzgruppe, nicht für jeden Eintrag angegeben, dadurch wird der gleiche Wert für alle Einträge in dieser Datensatzgruppe verwendet.

#### Platzhalter-Ressourceneintragssätze

Azure DNS unterstützt [Platzhalterdatensätze](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Diese werden für alle Abfragen mit einem übereinstimmenden Namen zurückgegeben (es sei denn, es gibt eine genauere Übereinstimmung aus einer Datensatzgruppe ohne Platzhalter). Datensatzgruppen mit Platzhaltern werden für alle Datensatztypen mit Ausnahme von NS und SOA unterstützt.

Um eine Datensatzgruppe mit Platzhaltern zu erstellen, verwenden Sie den Namen der Datensatzgruppe „\*“ oder einen Namen, dessen Bezeichnung mit „\*“ beginnt, z. B. „\*.foo“.

#### CNAME-Ressourceneintragssätze

CNAME-Datensatzgruppen können nicht gleichzeitig neben anderen Datensatzgruppen mit dem gleichen Namen vorhanden sein. Sie können einen CNAME z.B. nicht mit dem relativen Namen „www“, und gleichzeitig einen A-Eintrag mit dem relativen Namen „www“ erstellen. Da die Zonenspitze (Name = " @") immer die NS- und SOA-Datensatzgruppen enthält, die beim Erstellen der Zone erstellt wurden, bedeutet dies, dass Sie keine CNAME-Datensatzgruppe an der Zonenspitze erstellen können. Diese Einschränkungen ergeben sich aus den DNS-Standards; sie sind keine Einschränkungen von Azure DNS.


## Erstellen eines neuen Ressourceneintragssatzes und eines Eintrags

Im folgende Beispiel wird die Erstellung eines Ressourceneintragssatzes und von Einträgen mit dem Azure-Portal schrittweise erläutert. Wir verwenden den DNS-Eintragstyp „A“.

1. Melden Sie sich beim Azure-Portal an.

2. Navigieren Sie zu dem **DNS-Zone**-Blatt, in dem Sie Ihre Ressourceneintragssätze erstellen möchten.

3. Klicken Sie im Blatt „DNS-Zone“ oben auf **Record set** (Ressourceneintragssatz), um das Blatt **Ressourceneintragssatz hinzufügen** zu öffnen.
 
	![Neuer Ressourceneintragssatz](./media/dns-getstarted-create-recordset-portal/newrecordset500.png)

4. Benennen Sie im Blatt **Ressourceneintragssatz hinzufügen** Ihren Ressourceneintragssatz. Sie können dem Ressourceneintragssatz z.B. den Namen „**www**“.
  
	![Hinzufügen eines Ressourceneintragssatzes](./media/dns-getstarted-create-recordset-portal/addrecordset500.png)

5. Wählen Sie bei „Typ“ den gewünschten Eintragstyp. Zum Beispiel **A**.

6. Legen Sie die **TTL** fest. Die Standardeinstellung für das Portal ist eine Stunde.

7. Fügen Sie die IP-Adressen hinzu, eine IP-Adresse pro Zeile. Durch Verwenden des vorgeschlagenen Namens für den Ressourceneintragssatz und den oben angegebenen Eintragstyp wird die IPv4-IP-Adresse dem A-Eintrag für den www-Ressourceneintragssatz hinzugefügt.

8. Wenn die IP-Adresse hinzugefügt wurde, klicken Sie unten auf dem Blatt auf **OK**. Der DNS-Ressourceneintragssatz wird erstellt.

## Testen Sie Ihre DNS-Zone mit DNS-Tools.


Wenn Sie Ihre Domäne noch nicht delegiert haben, um die neue Zone in Azure DNS zu verwenden, müssen Sie die DNS-Abfrage direkt auf einen der Namenserver für die Zone leiten. Die Namenserver Ihrer Zone werden im Bereich „Essentials“ im Blatt Ihrer DNS-Zone angezeigt. Weitere Informationen finden Sie unter [Delegieren Ihrer Domäne an Azure DNS](dns-domain-delegation.md).

Sie können Ihre DNS-Zone mit DNS-Tools wie nslookup, dig oder mit dem [PowerShell-Cmdlet Resolve-DnsName](https://technet.microsoft.com/library/jj590781.aspx) testen.


## Nächste Schritte

Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md). Weitere Informationen zum Automatisieren von DNS-Einträgen finden Sie unter [Erstellen von DNS-Zonen und -Datensatzgruppen mithilfe des .NET SDK](dns-sdk.md).

<!------HONumber=AcomDC_0406_2016-->