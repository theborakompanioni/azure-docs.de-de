---
title: "Importieren und Exportieren einer Domänenzonendatei in Azure DNS mit der Befehlszeilenschnittstelle | Microsoft Docs"
description: Informationen zum Importieren und Exportieren einer DNS-Zonendatei in Azure DNS mithilfe der Azure-CLI
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: f5797782-3005-4663-a488-ac0089809010
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 765a30f360cf8d3f8bde08aa94b20eba0d4537c9
ms.lasthandoff: 04/18/2017

---

# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli"></a>Importieren und Exportieren einer DNS-Zonendatei mit der Azure-Befehlszeilenschnittstelle

In diesem Artikel wird das Importieren und Exportieren von DNS-Zonendateien für Azure DNS mithilfe der Azure-Befehlszeilenschnittstelle erläutert.

## <a name="introduction-to-dns-zone-migration"></a>Einführung in die DNS-Zonenmigration

Eine DNS-Zonendatei ist eine Textdatei, die Details aller Domain Name System-Einträge (DNS) in der Zone enthält. Sie hat ein Standardformat, weshalb sie sich für das Übertragen von DNS-Einträgen zwischen DNS-Systemen eignet. Das Verwenden einer Zonendatei ist eine schnelle, zuverlässige und praktische Möglichkeit, eine DNS-Zone in und aus Azure DNS zu übertragen.

Für Azure DNS wird das Importieren und Exportieren von Zonendateien mit der Azure-Befehlszeilenschnittstelle (CLI) unterstützt. Das Importieren von Zonendateien mithilfe von Azure PowerShell oder dem Azure-Portal wird derzeit **nicht** unterstützt.

Die Azure-CLI (Command Line Interface, Befehlszeilenschnittstelle) ist ein plattformübergreifendes Befehlszeilentool zum Verwalten von Azure-Diensten. Sie steht auf der [Azure-Downloadseite](https://azure.microsoft.com/downloads/)für Windows-, Mac- und Linux-Plattformen zur Verfügung. Die plattformübergreifende Unterstützung ist besonders wichtig für das Importieren und Exportieren von Zonendateien, da die am häufigsten verwendete Namenserversoftware ( [BIND](https://www.isc.org/downloads/bind/)) meist unter Linux ausgeführt wird.

> [!NOTE]
> Es gibt derzeit zwei Versionen der Azure-CLI. CLI 1.0 basiert auf Node.js, und die Befehle beginnen mit „azure“.
> CLI 2.0 basiert auf Python, und die Befehle beginnen mit „az“. In beiden Versionen wird der Import von Zonendateien unterstützt, es wird jedoch empfohlen, die Befehle von CLI 1.0 zu verwenden, wie auf dieser Seite beschrieben.

## <a name="obtain-your-existing-dns-zone-file"></a>Abrufen Ihrer vorhandenen DNS-Zonendatei

Bevor Sie eine DNS-Zonendatei in Azure DNS importieren, müssen Sie eine Kopie der Zonendatei abrufen. Der Speicherort dieser Datei hängt davon ab, wo die DNS-Zone derzeit gehostet wird.

* Wenn Ihre DNS-Zone von einem Partnerdienst (z. B. einer Domänenregistrierungsstelle, einem dedizierten DNS-Hostinganbieter oder anderen Cloudanbieter) gehostet wird, sollte der Dienst die Möglichkeit zum Herunterladen der DNS-Zonendatei bieten.
* Falls Ihre DNS-Zone unter einem Windows-DNS gehostet wird, lautet der Standardordner für die Zonendateien **%systemroot%\system32\dns**. Der vollständige Pfad zu jeder Zonendatei wird auch in der DNS-Dienstverwaltungskonsole auf der Registerkarte **Allgemein** angezeigt.
* Wenn Ihre DNS-Zone mithilfe von BIND gehostet wird, wird der Speicherort der Zonendatei für jede Zone in der BIND-Konfigurationsdatei **named.conf**angegeben.

> [!NOTE]
> Das Format von Zonendateien, die von GoDaddy heruntergeladen werden, weicht leicht vom Standardformat ab. Sie müssen dies korrigieren, bevor Sie diese Zonendateien in Azure DNS importieren.
>
> DNS-Namen in den RDATA eines DNS-Eintrags werden in Form von vollqualifizierten Namen angegeben, weisen am Ende aber keinen Punkt („.“) auf. Dies bedeutet, dass sie von anderen DNS-Systemen als relative Namen interpretiert werden. Sie müssen die Zonendatei bearbeiten, indem Sie den abschließenden „.“ an ihren Namen anfügen, bevor Sie sie in Azure DNS importieren.
>
> Beispielsweise sollte der CNAME-Eintrag „www 3600 IN CNAME contoso.com“ in „www 3600 IN CNAME contoso.com.“
> (mit dem abschließenden „.“) geändert werden.

## <a name="import-a-dns-zone-file-into-azure-dns"></a>Importieren einer DNS-Zonendatei in Azure DNS

Beim Importieren einer Zonendatei wird eine neue Zone in Azure DNS erstellt, wenn noch keine vorhanden ist. Wenn die Zone bereits vorhanden ist, werden die Ressourceneintragssätze in der Zonendatei mit den vorhandenen Ressourceneintragssätzen zusammengeführt.

### <a name="merge-behavior"></a>Zusammenführungsverhalten

* Standardmäßig werden vorhandene und neue Ressourceneintragssätze zusammengeführt. Identische Datensätze innerhalb eines zusammengeführten Ressourceneintragssatzes werden dedupliziert.
* Alternativ ersetzt der Importvorgang bei Angabe der Option `--force` vorhandene Ressourceneintragssätze durch neue Ressourceneintragssätze. Vorhandene Ressourceneintragssätze, die keinen entsprechenden Ressourceneintragssatz in der importierten Zonendatei haben, werden nicht entfernt.
* Wenn Ressourceneintragssätze zusammengeführt werden, wird die Gültigkeitsdauer (Time to live, TTL) von bereits vorhandenen Ressourceneintragssätzen verwendet. Bei Verwendung von `--force` wird die Gültigkeitsdauer des neuen Ressourceneintragssatzes verwendet.
* SOA-Parameter (Start of Authority, Autoritätsursprung) werden – mit Ausnahme von `host` – immer aus der importierten Zonendatei verwendet. Dies gilt unabhängig davon, ob `--force` verwendet wird. Auch für den Namenserver-Ressourceneintragssatz an der Zonenspitze wird die Gültigkeitsdauer stets der importierten Zonendatei entnommen.
* Ein importierter CNAME-Eintrag ersetzt keinen vorhandenen CNAME-Eintrag mit dem gleichen Namen, es sei denn, der Parameter `--force` wurde angegeben.
* Wenn ein Konflikt zwischen einem CNAME-Eintrag und einem weiteren Eintrag mit demselben Namen – aber einem anderen Typ – entsteht (unabhängig davon, welcher vorhanden oder neu ist), bleibt der vorhandene Eintrag erhalten. Dies gilt unabhängig von der Verwendung von `--force`.

### <a name="additional-information-about-importing"></a>Weitere Informationen zum Importieren

Die folgenden Hinweise liefern weitere technische Details zum Zonenimportprozess.

* Die Direktive `$TTL` ist optional und wird unterstützt. Wird die Direktive `$TTL` nicht angegeben, werden Einträge ohne explizite Gültigkeitsdauer mit dem Standardwert der Gültigkeitsdauer von 3.600 Sekunden importiert. Wenn bei zwei Einträgen in demselben Ressourceneintragssatz eine unterschiedliche Gültigkeitsdauer angegeben ist, wird der niedrigere Wert verwendet.
* Die Direktive `$ORIGIN` ist optional und wird unterstützt. Wird `$ORIGIN` nicht festgelegt, ist der verwendete Standardwert der Zonenname gemäß Angabe in der Befehlszeile (plus abschließendem „.“).
* Die Direktiven `$INCLUDE` und `$GENERATE` werden nicht unterstützt.
* Die folgenden Eintragstypen werden unterstützt: A, AAAA, CNAME, MX, NS, SOA, SRV und TXT.
* Der SOA-Eintrag wird von Azure DNS automatisch erstellt, wenn eine Zone erstellt wird. Wenn Sie eine Zonendatei importieren, werden alle SOA-Parameter aus der Zonendatei entnommen, *mit Ausnahme* des Parameters `host`. Für diesen Parameter wird der Wert verwendet, der von Azure DNS bereitgestellt wird. Der Grund ist, dass dieser Parameter auf den von Azure DNS bereitgestellten primären Namenserver verweisen muss.
* Der Namenserver-Ressourceneintragssatz an der Zonenspitze wird beim Erstellen der Zone auch automatisch von Azure DNS erstellt. Nur die Gültigkeitsdauer dieses Ressourceneintragssatzes wird importiert. Diese Einträge enthalten die von Azure DNS bereitgestellten Namen der Namenserver. Die Eintragsdaten werden von den Werten in der importierten Zonendatei nicht überschrieben.
* Während der öffentlichen Vorschauphase unterstützt Azure DNS nur TXT-Einträge mit einer einzelnen Zeichenfolge. TXT-Einträge mit mehreren Zeichenfolgen werden verkettet und auf 255 Zeichen verkürzt.

### <a name="cli-format-and-values"></a>Format und Werte der Befehlszeilenschnittstelle

Das Format des Befehls der Azure-Befehlszeilenschnittstelle zum Importieren einer DNS-Zone lautet:

```azurecli
azure network dns zone import [options] <resource group> <zone name> <zone file name>
```

Werte:

* `<resource group>` ist der Name der Ressourcengruppe für die Zone in Azure DNS.
* `<zone name>` ist der Name der Zone.
* `<zone file name>` ist der Pfad/Name der zu importierenden Zonendatei.

Wenn Sie eine Zone mit diesem Namen in der Ressourcengruppe nicht vorhanden ist, wird sie für Sie erstellt. Wenn die Zone bereits vorhanden ist, werden die importierten Ressourceneintragssätze mit vorhandenen Ressourceneintragssätzen zusammengeführt. Verwenden Sie die Option `--force` , um die vorhandenen Ressourceneintragssätze zu überschreiben.

Um das Format einer Zonendatei zu überprüfen, ohne sie tatsächlich zu importieren, verwenden Sie die Option `--parse-only`.

### <a name="step-1-import-a-zone-file"></a>Schritt 1: Importieren einer Zonendatei

So importieren Sie eine Zonendatei für die Zone **contoso.com**

1. Melden Sie sich bei Ihrem Azure-Abonnement an, indem Sie die Azure-Befehlszeilenschnittstelle verwenden.

    ```azurecli
    azure login
    ```

2. Wählen Sie das Abonnement aus, in dem Sie die neue DNS-Zone erstellen möchten.

    ```azurecli
    azure account set <subscription name>
    ```

3. Für Azure DNS kann nur der Azure-Ressourcen-Manager verwendet werden. Sie müssen für die Azure-Befehlszeilenschnittstelle also auf den Ressourcen-Manager-Modus umschalten.

    ```azurecli
    azure config mode arm
    ```

4. Bevor Sie den Azure DNS-Dienst nutzen können, müssen Sie Ihr Abonnement für die Verwendung des Microsoft.Network-Ressourcenanbieters registrieren. (Dieser Schritt muss einmal für jedes Abonnement ausgeführt werden.)

    ```azurecli
    azure provider register Microsoft.Network
    ```

5. Falls noch nicht vorhanden, müssen Sie auch eine Ressourcen-Manager-Ressourcengruppe erstellen.

    ```azurecli
    azure group create myresourcegroup westeurope
    ```

6. Um die Zone **contoso.com** aus der Datei **contoso.com.txt** in eine neue DNS-Zone in der Ressourcengruppe **myresourcegroup** zu importieren, führen Sie den Befehl `azure network dns zone import` aus.<BR>Mit diesem Befehl wird die Zonendatei geladen und analysiert. Der Befehl führt eine Reihe von Befehlen für den Azure DNS-Dienst aus, um die Zone und alle Ressourceneintragssätze in der Zone zu erstellen. Außerdem werden mit dem Befehl im Konsolenfenster der Status sowie Fehler und Warnungen angegeben. Da Ressourceneintragssätze in Reihe erstellt werden, dauert der Import einer großen Zonendatei einige Minuten.

    ```azurecli
    azure network dns zone import myresourcegroup contoso.com contoso.com.txt
    ```

### <a name="step-2-verify-the-zone"></a>Schritt 2: Überprüfen der Zone

Um die DNS-Zone zu überprüfen, nachdem Sie die Datei importiert haben, können Sie eine der folgenden Methoden verwenden:

* Sie können die Einträge mit dem unten angegebenen Azure-CLI-Befehl auflisten.

    ```azurecli
    azure network dns record-set list myresourcegroup contoso.com
    ```

* Sie können die Einträge mithilfe des PowerShell-Cmdlets `Get-AzureRmDnsRecordSet`auflisten.
* Sie können `nslookup` verwenden, um die Namensauflösung für die Einträge zu überprüfen. Da die Zone nicht noch delegiert ist, müssen Sie die richtigen Azure DNS-Namenserver explizit angeben. Im Beispiel unten wird gezeigt, wie Sie die Namen der Namenserver abrufen, die der Zone zugewiesen sind. Außerdem wird veranschaulicht, wie Sie den Eintrag „www“ mit `nslookup`abfragen.

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/.../resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a>Schritt 3: Aktualisieren der DNS-Delegierung

Nachdem Sie sichergestellt haben, dass die Zone richtig importiert wurde, müssen Sie die DNS-Delegierung so aktualisieren, dass auf die Azure DNS-Namenserver verwiesen wird. Weitere Informationen finden Sie im Artikel [Delegieren von Domänen an Azure DNS](dns-domain-delegation.md).

## <a name="export-a-dns-zone-file-from-azure-dns"></a>Exportieren einer DNS-Zonendatei aus Azure DNS

Das Format des Befehls der Azure-Befehlszeilenschnittstelle zum Importieren einer DNS-Zone lautet:

    ```azurecli
    azure network dns zone export [options] <resource group> <zone name> <zone file name>
    ```

Werte:

* `<resource group>` ist der Name der Ressourcengruppe für die Zone in Azure DNS.
* `<zone name>` ist der Name der Zone.
* `<zone file name>` ist der Pfad/Name der zu exportierenden Zonendatei.

Sie müssen sich – wie beim Zonenimport – zunächst anmelden, Ihr Abonnement auswählen und die Azure-Befehlszeilenschnittstelle so konfigurieren, dass der Ressourcen-Manager-Modus verwendet wird.

### <a name="to-export-a-zone-file"></a>So exportieren Sie eine Zonendatei

1. Melden Sie sich bei Ihrem Azure-Abonnement an, indem Sie die Azure-Befehlszeilenschnittstelle verwenden.

    ```azurecli
    azure login
    ```

2. Wählen Sie das Abonnement aus, in dem Sie die neue DNS-Zone erstellen möchten.

    ```azurecli
    azure account set <subscription name>
    ```

3. Azure DNS ist ein nur über Azure-Ressourcen-Manager verfügbarer Dienst. Für die Azure-Befehlszeilenschnittstelle muss in den Ressourcen-Manager-Modus gewechselt werden.

    ```azurecli
    azure config mode arm
    ```

4. Um die vorhandene Azure-DNS-Zone **contoso.com** in die Ressourcengruppe **myresourcegroup** in der Datei **contoso.com.txt** (im aktuellen Ordner) zu exportieren, führen Sie `azure network dns zone export` aus. Mit diesem Befehl wird der Azure DNS-Dienst aufgefordert, die Ressourceneintragssätze in der Zone aufzuzählen und die Ergebnisse in eine mit BIND kompatible Zonendatei zu exportieren.

    ```azurecli
    azure network dns zone export myresourcegroup contoso.com contoso.com.txt
    ```

