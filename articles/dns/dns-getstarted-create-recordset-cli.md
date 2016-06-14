<properties
   pageTitle="Erstellen einer Datensatzgruppe und von Einträgen für eine DNS-Zone mithilfe der Befehlszeilenschnittstelle (CLI) | Microsoft Azure"
   description="Erstellen von Hosteinträgen für Azure DNS. Einrichten von Datensatzgruppen und Einträgen mithilfe der Befehlszeilenschnittstelle (CLI)"
   services="dns"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/06/2016"
   ms.author="cherylmc"/>

# Erstellen von DNS-Ressourceneintragssätzen und Einträgen über die CLI

> [AZURE.SELECTOR]
- [Azure-Portal](dns-getstarted-create-recordset-portal.md)
- [PowerShell](dns-getstarted-create-recordset.md)
- [Azure-Befehlszeilenschnittstelle](dns-getstarted-create-recordset-cli.md)


Dieser Artikel beschreibt das Erstellen von Einträgen und Eintragssätzen mithilfe der CLI. Nach dem Erstellen der DNS-Zone müssen Sie die DNS-Einträge für Ihre Domäne hinzufügen. Zu diesem Zweck müssen Sie zunächst Grundlegendes zu DNS-Einträgen und Datensatzgruppen verstehen.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## Erstellen eines Eintragssatzes und eines Eintrags

Dieser Abschnitt zeigt, wie Sie einen Eintragssatz und Einträge erstellen. In diesem Beispiel erstellen Sie einen Eintragssatz mit dem relativen Namen „www“ in der DNS-Zone „contoso.com“. Der vollqualifizierte Name der Einträge ist „www.contoso.com“. Der Eintragstyp ist „A“, und die Gültigkeitsdauer beträgt 60 Sekunden. Wenn Sie diesen Schritt ausgeführt haben, haben Sie eine leere Datensatzgruppe erstellt.

Zum Erstellen eines Eintragssatzes auf oberster Ebene der Zone (in diesem Fall „contoso.com“) verwenden Sie den Namen des Eintrags „@“, einschließlich der Anführungszeichen. Dies ist eine allgemeine DNS-Konvention.

### 1\. Erstellen einer Datensatzgruppe

Verwenden Sie `azure network dns record-set create` zum Erstellen eines Eintragssatzes. Geben Sie die Ressourcengruppe, den Zonennamen, den relativen Namen des Eintragssatzes, den Eintragstyp und die Gültigkeitsdauer an. Wenn der Parameter `--ttl` nicht definiert ist, liegt der Standardwert bei vier (in Sekunden). Wenn Sie diesen Schritt ausgeführt haben, verfügen Sie über eine leere „www“-Datensatzgruppe.

*Verwendung: network dns record-set create <resource-group> <dns-zone-name> <name> <type> <ttl>*

	azure network dns record-set create myresourcegroup  contoso.com  www A  60

### 2\. Hinzufügen von Datensätzen

Damit Sie den neu erstellten Eintragssatz „www“ verwenden können, müssen Sie Einträge hinzufügen. Einträge werden Eintragssätzen mithilfe von `azure network dns record-set add-record` hinzugefügt.

Die Parameter zum Hinzufügen von Einträgen zu einer Datensatzgruppe variieren je nach Typ der Datensatzgruppe. Wenn Sie beispielsweise einen Eintragssatz vom Typ „A“ verwenden, können Sie nur Einträge mit dem Parameter `-a <IPv4 address>` angeben.

Mit dem folgenden Befehl können Sie dem Eintragssatz „www“ IPv4-*A*-Einträge hinzufügen:

*Verwendung: network dns record-set add-record <resource-group> <dns-zone-name> <record-set-name> <type>*

	azure network dns record-set add-record myresourcegroup contoso.com  www A  -a 134.170.185.46

## Zusätzliche Beispiele für Datensatztypen

Die folgenden Beispiele zeigen, wie Sie einen Eintragssatz jedes Eintragstyps erstellen. Jeder Eintragssatz enthält einen einzelnen Eintrag.

[AZURE.INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]

## Nächste Schritte

Informationen zur Verwaltung Ihrer Eintragssätze und Einträge finden Sie unter [Verwalten von DNS-Einträgen und -Eintragssätzen über die CLI](dns-operations-recordsets-portal.md).

Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md).

<!---HONumber=AcomDC_0608_2016-->