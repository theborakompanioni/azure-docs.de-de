---
title: "Verwalten von DNS-Einträgen in Azure DNS mit der Azure CLI 1.0 | Microsoft-Dokumentation"
description: "Verwalten von DNS-Datensatzgruppen und Einträgen in Azure DNS, wenn Sie Ihre Domäne in Azure DNS hosten. Alle Befehle der CLI 1.0 für Vorgänge für Ressourceneintragssätze und Einträge."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 5356a3a5-8dec-44ac-9709-0c2b707f6cb5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/20/2016
ms.author: jonatul
translationtype: Human Translation
ms.sourcegitcommit: 36fa9cd757b27347c08f80657bab8a06789a3c2f
ms.openlocfilehash: 3074bf378f809a9857c7ea72521961368a14772c
ms.lasthandoff: 02/27/2017

---

# <a name="manage-dns-records-in-azure-dns-using-the-azure-cli-10"></a>Verwalten von DNS-Einträgen in Azure DNS mit der Azure CLI 1.0

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

Dieser Artikel zeigt, wie Sie DNS-Einträge für Ihre DNS-Zone mit der plattformübergreifenden Azure-Befehlszeilenschnittstelle (Command Line Interface, CLI) verwalten, die für Windows, Mac und Linux verfügbar ist. Sie können Ihre DNS-Einträge auch mithilfe von [Azure PowerShell](dns-operations-recordsets.md) oder über das [Azure-Portal](dns-operations-recordsets-portal.md) verwalten.

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

* [Azure CLI 1.0:](dns-operations-recordsets-cli-nodejs.md) Unsere Befehlszeilenschnittstelle für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
* [Azure CLI 2.0:](dns-operations-recordsets-cli.md) Unsere Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell

Bei den Beispielen in diesem Artikel wird vorausgesetzt, dass Sie bereits [die Azure CLI 1.0 installiert haben, angemeldet sind und eine DNS-Zone erstellt haben](dns-operations-dnszones-cli-nodejs.md).

## <a name="introduction"></a>Einführung

Bevor Sie DNS-Einträge in Azure DNS erstellen, müssen Sie zunächst verstehen, wie DNS-Einträge von Azure DNS in DNS-Ressourceneintragssätzen organisiert werden.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Weitere Informationen zu DNS-Einträgen in Azure DNS finden Sie unter [DNS-Zonen und -Einträge](dns-zones-records.md).

## <a name="create-a-dns-record"></a>Erstellen eines DNS-Eintrags

Verwenden Sie zum Erstellen eines DNS-Eintrags den Befehl `azure network dns record-set add-record`. Entsprechende Hilfeinformationen finden Sie unter `azure network dns record-set add-record -h`.

Beim Erstellen eines Eintrags müssen Sie den Ressourcengruppennamen, den Zonennamen und den Namen des Eintragssatzes sowie den Eintragstyp und die Details zu dem zu erstellenden Eintrag angeben. Der Name des Eintragssatzes muss ein *relativer* Name sein, also ein Name ohne Zonenname.

Ist der Eintragssatz noch nicht vorhanden, wird er vom Befehl für Sie erstellt. Ist der Eintragssatz bereits vorhanden, fügt dieser Befehl den von Ihnen angegebenen Eintrag zum vorhandenen Eintragssatz hinzu.

Bei der Erstellung eines neuen Eintragssatzes wird ein Standardwert von 3600 für die Gültigkeitsdauer (Time-To-Live, TTL) verwendet. Anleitungen zum Verwenden verschiedener TTL-Werte finden Sie unter [Erstellen eines DNS-Ressourceneintragssatzes](#create-a-dns-record-set).

Im folgenden Beispiel wird in der Ressourcengruppe *MyResourceGroup* in der Zone *contoso.com* ein A-Eintrag namens *www* erstellt. Die IP-Adresse des A-Eintrags lautet *1.2.3.4*.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Zum Erstellen eines Eintrags auf oberster Ebene der Zone (in diesem Fall „contoso.com“) verwenden Sie den Namen des Eintrags "@", einschließlich der Anführungszeichen:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" A -a 1.2.3.4
```

## <a name="create-a-dns-record-set"></a>Erstellen eines DNS-Ressourceneintragssatzes

In den oben gezeigten Beispielen wurde der DNS-Eintrag entweder zu einem vorhandenen Eintragssatz hinzugefügt oder der Eintragssatz wurde *implizit* erstellt. Sie können einen Eintragssatz auch *explizit* erstellen, bevor Sie Datensätze hinzufügen. Azure DNS unterstützt „leere“ Eintragssätze. Diese können als Platzhalter verwendet werden, um vor der Erstellung von DNS-Einträgen einen DNS-Namen zu reservieren. Leere Ressourceneintragssätze sind auf der Steuerungsebene von Azure DNS sichtbar, aber nicht auf den Azure DNS-Namenservern.

Eintragssätze werden mit dem Befehl `azure network dns record-set create` erstellt. Entsprechende Hilfeinformationen finden Sie unter `azure network dns record-set create -h`.

Durch explizites Erstellen eines Eintragssatzes können Sie Eigenschaften für den Eintragssatz angeben, wie z.B. die [Gültigkeitsdauer (TTL)](dns-zones-records.md#time-to-live) oder Metadaten. Mithilfe von [Metadaten für den Eintragssatz](dns-zones-records.md#tags-and-metadata) können den einzelnen Eintragssätzen anwendungsspezifische Daten als Schlüssel-Wert-Paare zugeordnet werden.

Das folgende Beispiel erstellt mithilfe des `--ttl`-Parameters (Kurzform: `-l`) einen leeren Eintragssatz mit einer Gültigkeitsdauer von 60 Sekunden:

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --ttl 60
```

Das folgende Beispiel erstellt mithilfe des `--metadata`-Parameters (Kurzform: `-m`) einen Eintragssatz mit zwei Metadateneinträgen („dept=finance“ und „environment=production“):

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

Nach dem Erstellen eines leeren Eintragssatzes können diesem mithilfe von `azure network dns record-set add-record` Einträge hinzugefügt werden, wie in [Erstellen eines DNS-Eintrags](#create-a-dns-record) beschrieben.

## <a name="create-records-of-other-types"></a>Erstellen anderer Eintragstypen

Nach der ausführlichen Erstellungsanleitung für A-Einträge erfahren Sie in den folgenden Beispielen, wie Sie andere von Azure DNS unterstützte Eintragstypen erstellen.

Die zum Angeben der Eintragsdaten verwendeten Parameter variieren abhängig vom Eintragstyp. Beispiel: Für einen Eintrag vom Typ „A“ geben Sie die IPv4-Adresse mit dem Parameter `-a <IPv4 address>` an. Die Parameter für jeden Eintragstyp können mithilfe von `azure network dns record-set add-record -h` aufgelistet werden.

Für jeden Fall wird gezeigt, wie Sie einen einzelnen Eintrag erstellen. Der Eintrag wird dem vorhandenen Eintragssatz hinzugefügt, oder es wird implizit ein Eintragssatz erstellt. Weitere Informationen zum Erstellen von Eintragssätzen und zum expliziten Definieren von Eintragssatzparametern finden Sie unter [Erstellen eines DNS-Ressourceneintragssatzes](#create-a-dns-record-set).

Die Erstellung eines SOA-Ressourceneintragssatzes wird hier nicht gezeigt, da SOAs zusammen mit der jeweiligen DNS-Zone erstellt und gelöscht werden und nicht separat erstellt oder gelöscht werden können. Der SOA-Eintrag kann jedoch geändert werden. Dies wird in einem [späteren Beispiel](#to-modify-an-SOA-record) gezeigt.

### <a name="create-an-aaaa-record"></a>Erstellen eines AAAA-Eintrags

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-aaaa AAAA --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-a-cname-record"></a>Erstellen eines CNAME-Eintrags

> [!NOTE]
> Aufgrund der DNS-Standards sind auf der obersten Ebene einer Zone (`-Name "@"`) keine CNAME-Einträge und keine Ressourceneintragssätze mit mehreren Einträgen zulässig.
> 
> Weitere Informationen finden Sie unter [CNAME-Einträge](dns-zones-records.md#cname-records).

```azurecli
azure network dns record-set add-record  MyResourceGroup contoso.com  test-cname CNAME --cname www.contoso.com
```

### <a name="create-an-mx-record"></a>Erstellen eines MX-Eintrags

In diesem Beispiel verwenden wir den Namen des Datensatzes „@“ zum Erstellen des MX-Eintrags auf oberster Ebene der Zone (in diesem Fall „contoso.com“).

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "@" MX --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a>Erstellen eines NS-Eintrags

```azurecli
azure network dns record-set add-record MyResourceGroup  contoso.com  test-ns NS --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a>Erstellen eines PTR-Eintrags

In diesem Fall ist „my-arpa-zone.com“ die ARPA-Zone, die Ihren IP-Adressbereich darstellt. Jeder PTR-Eintragssatz in dieser Zone entspricht einer IP-Adresse innerhalb dieses IP-Adressbereichs.  Der Eintragsname „10“ ist das letzte Oktett der IP-Adresse innerhalb dieses IP-Adressbereichs, der durch diesen Eintrag dargestellt wird.

```azurecli
azure network dns record-set add-record MyResourceGroup my-arpa-zone.com "10" PTR --ptrdname "myservice.contoso.com"
```

### <a name="create-an-srv-record"></a>Erstellen eines SRV-Eintrags

Geben Sie beim Erstellen eines [SRV-Ressourceneintragssatzes](dns-zones-records.md#srv-records) den *\_Dienst* und das *\_Protokoll* im Namen des Ressourceneintragssatzes an. Wenn Sie einen SRV-Ressourceneintragssatz auf der obersten Ebene der Zone erstellen, muss "@" nicht in den Namen des Ressourceneintragssatzes eingeschlossen werden.

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "_sip._tls" SRV --priority 10 --weight 5 --port 8080 --target "sip.contoso.com"
```

### <a name="create-a-txt-record"></a>Erstellen eines TXT-Eintrags

Das folgende Beispiel zeigt die Erstellung eines TXT-Eintrags. Weitere Informationen zur maximal unterstützten Zeichenfolgenlänge in TXT-Einträgen finden Sie unter [TXT-Einträge](dns-zones-records.md#txt-records).

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-txt TXT --text "This is a TXT record"
```

## <a name="get-a-record-set"></a>Abrufen einer Datensatzgruppe

Verwenden Sie `azure network dns record-set show`zum Abrufen eines vorhandenen Ressourceneintragssatzes. Entsprechende Hilfeinformationen finden Sie unter `azure network dns record-set show -h`.

Wie beim Erstellen eines Eintrags oder Eintragssatzes muss der Name des Eintragssatzes ein *relativer* Name sein, also ein Name ohne Zonenname. Außerdem müssen Sie den Eintragstyp, die Zone, die den Eintragssatz enthält, und die Ressourcengruppe, die die Zone enthält, angeben.

Das folgende Beispiel ruft den Eintrag *www* vom Typ A aus der Zone *contoso.com* in der Ressourcengruppe *MyResourceGroup* ab:

```azurecli
azure network dns record-set show MyResourceGroup contoso.com www A
```

## <a name="list-record-sets"></a>Auflisten von Datensatzgruppen

Sie können alle Eintragssätze in einer DNS-Zone mit dem Befehl `azure network dns record-set list` auflisten. Entsprechende Hilfeinformationen finden Sie unter `azure network dns record-set list -h`.

Dieses Beispiel gibt alle Eintragssätze in der Zone *contoso.com* in der Ressourcengruppe *MyResourceGroup* zurück, unabhängig vom Namen oder Typ:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```

Dieses Beispiel gibt alle Eintragssätze zurück, die dem angegebenen Eintragstyp (in diesem Fall „A“-Einträge) entsprechen:

```azurecli
azure network dns record-set list MyResourceGroup contoso.com --type A
```

## <a name="add-a-record-to-an-existing-record-set"></a>Hinzufügen eines Eintrags zu einem vorhandenen Ressourceneintragssatz

Sie können `azure network dns record-set add-record` sowohl zum Erstellen eines Eintrags in einem neuen Eintragssatz als auch zum Hinzufügen eines Eintrags zu einem vorhandenen Eintragssatz verwenden.

Weitere Informationen finden Sie unter [Erstellen eines DNS-Eintrags](#create-a-dns-record) und [Erstellen anderer Eintragstypen](#create-records-of-other-types) weiter oben.

## <a name="remove-a-record-from-an-existing-record-set"></a>Entfernen eines Eintrags aus einem vorhandenen Eintragssatz

Um einen DNS-Eintrag aus einem vorhandenen Eintragssatz zu entfernen, verwenden Sie `azure network dns record-set delete-record`. Entsprechende Hilfeinformationen finden Sie unter `azure network dns record-set delete-record -h`.

Dieser Befehl löscht einen DNS-Eintrag aus einem Eintragssatz. Wenn der letzte Eintrag in einem Eintragssatz gelöscht wird, wird der Eintragssatz selbst **nicht** gelöscht. Der Eintragssatz bleibt leer. Informationen zum Löschen des Eintragssatzes finden Sie unter [Löschen eines Eintragssatzes](#delete-a-record-set).

Sie müssen den zu löschenden Eintrag und die Zone angeben, aus der der Eintrag gelöscht werden soll. Verwenden Sie dazu die gleichen Parameter wie beim Erstellen eines Eintrags mithilfe von `azure network dns record-set add-record`. Diese Parameter werden unter [Erstellen eines DNS-Eintrags](#create-a-dns-record) und [Erstellen anderer Eintragstypen](#create-records-of-other-types) weiter oben beschrieben.

Dieser Befehl fordert Sie zur Bestätigung auf. Diese Aufforderung kann mit der Option `--quiet` (Kurzform: `-q`) unterdrückt werden.

Das folgende Beispiel löscht den A-Eintrag mit dem Wert „1.2.3.4“ aus dem Eintragssatz *www* in der Zone *contoso.com* der Ressourcengruppe *MyResourceGroup*. Die Bestätigungsaufforderung wird unterdrückt.

```azurecli
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4 --quiet
```

## <a name="modify-an-existing-record-set"></a>Ändern eines vorhandenen Ressourceneintragssatzes

Jeder Eintragssatz enthält eine [Gültigkeitsdauer (TTL)](dns-zones-records.md#time-to-live), [Metadaten](dns-zones-records.md#tags-and-metadata) und DNS-Einträge. In den folgenden Abschnitten wird erläutert, wie diese Eigenschaften geändert werden können.

### <a name="to-modify-an-a-aaaa-mx-ns-ptr-srv-or-txt-record"></a>So ändern Sie einen Eintrag vom Typ A, AAAA, MX, NS, PTR, SRV oder TXT

Um einen vorhandenen Eintrag vom Typ A, AAAA, MX, NS, PTR, SRV oder TXT zu ändern, sollten Sie zunächst einen neuen Eintrag erstellen und den vorhandenen Eintrag löschen. Ausführliche Anweisungen zum Löschen und Hinzufügen von Einträgen finden Sie in den Abschnitten weiter oben in diesem Artikel.

Das folgende Beispiel zeigt, wie Sie einen A-Eintrag von der IP-Adresse 1.2.3.4 in die IP-Adresse 5.6.7.8 ändern:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 5.6.7.8
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

Sie können im automatisch erstellten NS-Ressourceneintragssatz auf der obersten Ebene der Zone (`-Name "@"`, einschließlich Anführungszeichen) keine Einträge hinzufügen, entfernen oder ändern. Für diesen Eintragssatz können Sie nur die Gültigkeitsdauer (Time-to-Live, TTL) und die Metadaten ändern.

### <a name="to-modify-a-cname-record"></a>Ändern eines CNAME-Eintrags

Um einen CNAME-Eintrag zu ändern, verwenden Sie `azure network dns record-set add-record` zum Hinzufügen eines neuen Eintragswerts. Im Gegensatz zu anderen Eintragstypen kann ein CNAME-Eintrag nur einen einzelnen Eintrag enthalten. Daher wird der vorhandene Eintrag *ersetzt*, wenn ein neuer Eintrag hinzugefügt wird, und muss nicht separat gelöscht werden.  Sie werden aufgefordert, die Ersetzung zu akzeptieren.

Diese Beispiel ändert den CNAME-Eintragssatz *www* in der Zone *contoso.com* der Ressourcengruppe *MyResourceGroup* so, dass er auf „www.fabrikam.net“ anstatt auf den vorhandenen Wert zeigt:

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www CNAME --cname www.fabrikam.net
``` 

### <a name="to-modify-an-soa-record"></a>So ändern Sie einen SOA-Eintrag

Verwenden Sie `azure network dns record-set set-soa-record`, um den SOA-Eintrag für eine bestimmte DNS-Zone zu ändern. Entsprechende Hilfeinformationen finden Sie unter `azure network dns record-set set-soa-record -h`.

Das folgende Beispiel zeigt, wie Sie die Eigenschaft „email“ des SOA-Eintrags für die Zone *contoso.com* in der Ressourcengruppe *MyResourceGroup* festlegen:

```azurecli
azure network dns record-set set-soa-record rg1 contoso.com --email admin.contoso.com
```

### <a name="to-modify-the-ttl-of-an-existing-record-set"></a>So ändern Sie die Gültigkeitsdauer eines vorhandenen Eintragssatzes

Um die Gültigkeitsdauer eines vorhandenen Eintragssatzes zu ändern, verwenden Sie `azure network dns record-set set`. Entsprechende Hilfeinformationen finden Sie unter `azure network dns record-set set -h`.

Das folgende Beispiel zeigt, wie Sie die Gültigkeitsdauer eines Eintragssatzes ändern, in diesem Fall in 60 Sekunden:

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --ttl 60
```

### <a name="to-modify-the-metadata-of-an-existing-record-set"></a>So ändern Sie die Metadaten eines vorhandenen Eintragssatzes

Mithilfe von [Metadaten für den Eintragssatz](dns-zones-records.md#tags-and-metadata) können den einzelnen Eintragssätzen anwendungsspezifische Daten als Schlüssel-Wert-Paare zugeordnet werden. Um die Metadaten eines vorhandenen Eintragssatzes zu ändern, verwenden Sie `azure network dns record-set set`. Entsprechende Hilfeinformationen finden Sie unter `azure network dns record-set set -h`.

Das folgende Beispiel zeigt, wie Sie mithilfe des `--metadata`-Parameters (Kurzform: `-m`) einen Eintragssatz mit zwei Metadateneinträgen („dept=finance“ und „environment=production“) ändern. Beachten Sie, dass jegliche vorhandenen Metadaten durch die angegebenen Werte *ersetzt* werden.

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

## <a name="delete-a-record-set"></a>Löschen eines Eintragssatzes

Eintragssätze können mit dem Befehl `azure network dns record-set delete` gelöscht werden. Entsprechende Hilfeinformationen finden Sie unter `azure network dns record-set delete -h`. Beim Löschen eines Ressourceneintragssatzes werden auch alle darin enthaltenen Einträge gelöscht.

> [!NOTE]
> Die SOA- und NS-Ressourceneintragssätze auf der obersten Ebene der Zone (`-Name "@"`) können nicht gelöscht werden.  Diese werden automatisch erstellt, wenn die Zone erstellt wird, und automatisch gelöscht, wenn die Zone gelöscht wird.

Das folgende Beispiel löscht den Eintragssatz namens *www* vom Typ A aus der Zone *contoso.com* in der Ressourcengruppe *MyResourceGroup*:

```azurecli
azure network dns record-set delete MyResourceGroup contoso.com www A
```

Sie werden aufgefordert, den Löschvorgang zu bestätigen. Um diesen Aufforderung zu unterdrücken, verwenden Sie die Option `--quiet` (Kurzform: `-q`).

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich ausführlicher über [Zonen und Einträge in Azure DNS](dns-zones-records.md).
<br>
Lernen Sie, wie Sie bei Verwendung von Azure DNS [Ihre Zonen und Einträge schützen](dns-protect-zones-recordsets.md).

