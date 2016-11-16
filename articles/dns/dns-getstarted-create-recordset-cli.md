---
title: "Erstellen einer Datensatzgruppe und von Einträgen für eine DNS-Zone mithilfe der Befehlszeilenschnittstelle (CLI) | Microsoft Docs"
description: "Erstellen von Hosteinträgen für Azure DNS. Einrichten von Datensatzgruppen und Einträgen mithilfe der Befehlszeilenschnittstelle (CLI)"
services: dns
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: 
ms.assetid: 02b897d3-e83b-4257-b96d-5c29aa59e843
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 40accee35eca32eefd4afc2315c70d51e0edcdcd


---
# <a name="create-dns-record-sets-and-records-by-using-cli"></a>Erstellen von DNS-Ressourceneintragssätzen und Einträgen über die CLI
> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-create-recordset-portal.md)
> * [PowerShell](dns-getstarted-create-recordset.md)
> * [Azure-Befehlszeilenschnittstelle](dns-getstarted-create-recordset-cli.md)
> 
> 

Dieser Artikel beschreibt das Erstellen von Einträgen und Eintragssätzen mithilfe der CLI. Nach dem Erstellen der DNS-Zone müssen Sie die DNS-Einträge für Ihre Domäne hinzufügen. Zu diesem Zweck müssen Sie zunächst Grundlegendes zu DNS-Einträgen und Datensatzgruppen verstehen.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## <a name="create-a-record-set-and-record"></a>Erstellen eines Eintragssatzes und eines Eintrags
Dieser Abschnitt zeigt, wie Sie einen Eintragssatz und Einträge erstellen. In diesem Beispiel erstellen Sie einen Eintragssatz mit dem relativen Namen „www“ in der DNS-Zone „contoso.com“. Der vollqualifizierte Name der Einträge ist „www.contoso.com“. Der Eintragstyp ist „A“, und die Gültigkeitsdauer beträgt 60 Sekunden. Wenn Sie diesen Schritt ausgeführt haben, haben Sie eine leere Datensatzgruppe erstellt.

Zum Erstellen eines Eintragssatzes auf oberster Ebene der Zone (in diesem Fall „contoso.com“) verwenden Sie den Namen des Eintrags „"@",“, einschließlich der Anführungszeichen. Dies ist eine allgemeine DNS-Konvention.

### <a name="1-create-a-record-set"></a>1. Erstellen einer Datensatzgruppe
Verwenden Sie `azure network dns record-set create` zum Erstellen eines Eintragssatzes. Geben Sie die Ressourcengruppe, den Zonennamen, den relativen Namen des Eintragssatzes, den Eintragstyp und die Gültigkeitsdauer an. Wenn der Parameter `--ttl` nicht definiert ist, liegt der Standardwert bei vier (in Sekunden). Wenn Sie diesen Schritt ausgeführt haben, verfügen Sie über eine leere „www“-Datensatzgruppe.

*<name>Syntax: network dns record-set create <Ressourcengruppe> <Name der DNS-Zone> <Name> <Typ> <type> <ttl>*

    azure network dns record-set create myresourcegroup  contoso.com  www A  60

### <a name="2-add-records"></a>2. Hinzufügen von Datensätzen
Damit Sie den neu erstellten Eintragssatz „www“ verwenden können, müssen Sie Einträge hinzufügen. Einträge werden Eintragssätzen mithilfe von `azure network dns record-set add-record`hinzugefügt.

Die Parameter zum Hinzufügen von Einträgen zu einer Datensatzgruppe variieren je nach Typ der Datensatzgruppe. Wenn Sie beispielsweise einen Eintragssatz vom Typ „A“ verwenden, können Sie nur Einträge mit dem Parameter `-a <IPv4 address>`angeben.

Mit dem folgenden Befehl können Sie dem Eintragssatz „www“ IPv4-Einträge vom Typ *A* hinzufügen:

*Syntax: network dns record-set add-record <Ressourcengruppe> <Name der DNS-Zone> <Name des Eintragssatzes><type>*

    azure network dns record-set add-record myresourcegroup contoso.com  www A  -a 134.170.185.46

## <a name="additional-record-type-examples"></a>Zusätzliche Beispiele für Datensatztypen
Die folgenden Beispiele zeigen, wie Sie einen Eintragssatz jedes Eintragstyps erstellen. Jeder Eintragssatz enthält einen einzelnen Eintrag.

[!INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]

## <a name="next-steps"></a>Nächste Schritte
Informationen zur Verwaltung Ihrer Eintragssätze und Einträge finden Sie unter [Verwalten von DNS-Ressourceneinträgen und DNS-Ressourceneintragssätzen über die Befehlszeilenschnittstelle](dns-operations-recordsets-portal.md).

Weitere Informationen zu Azure DNS finden Sie unter [Azure DNS – Übersicht](dns-overview.md).




<!--HONumber=Nov16_HO2-->


