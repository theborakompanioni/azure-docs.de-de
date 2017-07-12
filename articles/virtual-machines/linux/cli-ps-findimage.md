---
title: "Auswählen von Linux-VM-Images mit der Azure-CLI | Microsoft Docs"
description: "Erfahren Sie, wie Sie den Herausgeber, das Angebot und die SKU für Images ermitteln, wenn Sie mit dem Resource Manager-Bereitstellungsmodell einen virtuellen Linux-Computer erstellen."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7a858e38-4f17-4e8e-a28a-c7f801101721
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/15/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 1c4e8b5168d15c480d2aea41ddf7570d310e1252
ms.contentlocale: de-de
ms.lasthandoff: 04/03/2017

---
<a id="how-to-find-linux-vm-images-with-the-azure-cli" class="xliff"></a>

# Suchen von Linux-VM-Images mit der Azure-CLI
In diesem Thema wird beschrieben wie Sie Herausgeber, Angebote, SKUs und Versionen für jeden Ort finden, an dem Sie etwas bereitstellen möchten. 


<a id="use-azure-cli-20" class="xliff"></a>

## Verwenden der Azure CLI 2.0

Verwenden Sie nach dem [Installieren der Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) den Befehl `az vm image list`, um eine zwischengespeicherte Liste mit beliebten VM-Images anzuzeigen. Mit dem Befehl `az vm image list -o table` wird beispielsweise Folgendes angezeigt:

```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
UbuntuServer   Canonical               14.04.4-LTS         Canonical:UbuntuServer:14.04.4-LTS:latest                       UbuntuLTS            latest
CentOS         OpenLogic               7.2                 OpenLogic:CentOS:7.2:latest                                     CentOS               latest
openSUSE       SUSE                    13.2                SUSE:openSUSE:13.2:latest                                       openSUSE             latest
RHEL           RedHat                  7.2                 RedHat:RHEL:7.2:latest                                          RHEL                 latest
SLES           SUSE                    12-SP1              SUSE:SLES:12-SP1:latest                                         SLES                 latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
```

<a id="finding-all-current-images" class="xliff"></a>

### Suchen nach allen aktuellen Images

Verwenden Sie zum Abrufen der aktuellen Liste mit allen Images den Befehl `az vm image list` mit der Option `--all`. Anders als bei den Azure CLI 1.0-Befehlen werden mit dem Befehl `az vm image list --all` standardmäßig alle Images in **westus** zurückgegeben (sofern Sie nicht ein bestimmtes `--location`-Argument angeben). Es dauert also eine Weile, bis die Ausführung des Befehls `--all` abgeschlossen ist. Wenn Sie eine interaktive Untersuchung durchführen möchten, können Sie `az vm image list --all > allImages.json` verwenden. Mit diesem Befehl wird eine Liste mit allen Images zurückgegeben, die in Azure derzeit verfügbar sind, und für die lokale Nutzung als Datei gespeichert. 

Sie können eine von mehreren Optionen angeben, um die Suche bei Bedarf auf einen bestimmten Standort, ein Angebot, einen Herausgeber oder eine SKU zu beschränken. Wenn Sie keinen Standort angeben, werden die Werte für **westus** zurückgegeben.

<a id="find-specific-images" class="xliff"></a>

### Suchen nach bestimmten Images

Verwenden Sie `az vm image list` mit einem Filter, um nach bestimmten Informationen zu suchen. Mit dem folgenden Code werden beispielsweise die **Angebote** angezeigt, die für **Debian** verfügbar sind (zur Erinnerung: Ohne den Switch `--all` wird nur der lokale Cache von allgemeinen Images durchsucht):

```azurecli
az vm image list --offer Debian -o table --all
```

Die Ausgabe sieht etwa wie folgt aus: 
```
Offer   Publisher   Sku   Urn                              Version
------  ---------   ---   -------------------------------  -------------
Debian  credativ    8     credativ:Debian:8:8.0.201701180  8.0.201701180

<list shortened for the example>
```

Sie können ähnliche Filter für die Optionen **--publisher** und **--sku** ausführen. Sie können mit einem Filter auch nach Teilübereinstimmungen suchen. Verwenden Sie z.B. **--offer Deb**, um alle Debian-Images zu finden, oder **--publisher Micr**, um alle von Microsoft veröffentlichten Images zu finden.

Wenn Sie den Ort der Bereitstellung kennen, können Sie die Ergebnisse der Suche nach allgemeinen Images zusammen mit den Befehlen `az vm image list-skus`, `az vm image list-offers` und `az vm image list-publishers` verwenden, um präzise und anhand des Bereitstellungsorts zu suchen. Falls Sie aus dem vorherigen Beispiel wissen, dass `credativ` über ein Debian-Angebot verfügt, können Sie beispielsweise `--location` und andere Optionen verwenden, um genau die gewünschten Daten zu finden. Im folgenden Beispiel wird für **westeurope** nach einem Debian 8-Image gesucht:

```azurecli 
az vm image show -l westeurope -f debian -p credativ --skus 8 --version 8.0.201701180
```

Die Ausgabe lautet:

```json
{
  "dataDiskImages": [],
  "id": "/Subscriptions/<guid>/Providers/Microsoft.Compute/Locations/westeurope/Publishers/credativ/ArtifactTypes/VMImage/Offers/debian/Skus/8/Versions/8.0.201701180",
  "location": "westeurope",
  "name": "8.0.201701180",
  "osDiskImage": {
    "operatingSystem": "Linux"
  },
  "plan": null,
  "tags": null
}
```

<a id="use-azure-cli-10" class="xliff"></a>

## Verwenden der Azure-Befehlszeilenschnittstelle 1.0 

> [!NOTE]
> In diesem Artikel wird beschrieben, wie Sie durch VM-Images navigieren und diese auswählen, indem Sie eine Installation von Azure CLI 1.0 oder Azure PowerShell verwenden, die das Azure Resource Manager-Bereitstellungsmodell unterstützt. Voraussetzung hierfür ist, dass Sie in den Resource Manager-Modus wechseln. Wechseln Sie über die Azure-Befehlszeilenschnittstelle durch Eingabe von `azure config mode arm` in diesen Modus. 
> 

Am schnellsten können Sie ein Image finden, indem Sie den Befehl `azure vm image list` aufrufen und den Standort, den Namen des Herausgebers (Groß-/Kleinschreibung wird nicht beachtet) und ein Angebot, sofern bekannt, übergeben. Zum Beispiel ist die folgende Liste nur ein kurzes Beispiel (einige Listen sind sehr lang), wenn Sie wissen, dass „Canonical“ ein Herausgeber für das „UbuntuServer“-Angebot ist.

```azurecli
azure vm image list westus canonical ubuntuserver
info:    Executing command vm image list
warn:    The parameter --sku if specified will be ignored
+ Getting virtual machine image skus (Publisher:"canonical" Offer:"ubuntuserver" Location:"westus")
data:    Publisher  Offer         Sku                OS     Version          Location  Urn
data:    ---------  ------------  -----------------  -----  ---------------  --------  --------------------------------------------------------
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201604203  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201604203
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201605161  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201605161
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201606100  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606100
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201606270  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606270
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201607210  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201607210
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201608150  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201608150
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607220  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607220
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607230  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607230
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607240  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607240
```

Die **URN**-Spalte ist das Formular, das Sie an `azure vm quick-create` weiterleiten.

Oftmals ist jedoch noch unbekannt, was verfügbar ist. In diesem Fall können Sie durch die Images navigieren, indem Sie den Befehl `azure vm image list-publishers` verwenden und an der Eingabeaufforderung den Standort eines Rechenzentrums angeben. Beispielsweise werden im Folgenden alle Herausgeber von Datenträgerabbildern am Speicherort „West US“ aufgelistet (übergeben Sie das Speicherort-Argument mit Kleinbuchstaben und Entfernen von Leerzeichen von Standardspeicherorten).

```azurecli
azure vm image list-publishers
info:    Executing command vm image list-publishers
Location: westus
+ Getting virtual machine and/or extension image publishers (Location: "westus")
data:    Publisher                                       Location
data:    ----------------------------------------------  --------
data:    a10networks                                     westus  
data:    aiscaler-cache-control-ddos-and-url-rewriting-  westus  
data:    alertlogic                                      westus  
data:    AlertLogic.Extension                            westus  
```

Da diese Listen sehr umfangreich sein können, zeigt die oben dargestellte Beispielliste lediglich einen Ausschnitt. Angenommen, Sie würden bemerken, dass Canonical tatsächlich ein Herausgeber am Speicherort „West US“ ist, dann könnten Sie nun seine Angebote finden, indem Sie `azure vm image list-offers` aufrufen und den Speicherort und den Herausgeber über die Eingabeaufforderung übergeben. Dies ist im folgenden Beispiel dargestellt:

```azurecli
azure vm image list-offers
info:    Executing command vm image list-offers
Location: westus
Publisher: canonical
+ Getting virtual machine image offers (Publisher: "canonical" Location:"westus")
data:    Publisher  Offer                      Location
data:    ---------  -------------------------  --------
data:    canonical  Ubuntu15.04Snappy          westus
data:    canonical  Ubuntu15.04SnappyDocker    westus
data:    canonical  UbunturollingSnappy        westus
data:    canonical  UbuntuServer               westus
data:    canonical  Ubuntu_Snappy_Core         westus
data:    canonical  Ubuntu_Snappy_Core_Docker  westus
info:    vm image list-offers command OK
```

Jetzt wissen wir, dass in der Region „West US“ Canonical das **UbuntuServer** -Angebot in Azure herausgibt. Aber welche SKUs? Rufen Sie `azure vm image list-skus` ab, und antworten Sie auf die Eingabeaufforderung mit dem Standort, Herausgeber und dem von Ihnen entdeckten Angebot, um diese Werte zu erhalten.

```azurecli
azure vm image list-skus
info:    Executing command vm image list-skus
Location: westus
Publisher: canonical
Offer: ubuntuserver
+ Getting virtual machine image skus (Publisher:"canonical" Offer:"ubuntuserver" Location:"westus")
data:    Publisher  Offer         sku                Location
data:    ---------  ------------  -----------------  --------
data:    canonical  ubuntuserver  12.04.2-LTS        westus
data:    canonical  ubuntuserver  12.04.3-LTS        westus
data:    canonical  ubuntuserver  12.04.4-LTS        westus
data:    canonical  ubuntuserver  12.04.5-DAILY-LTS  westus
data:    canonical  ubuntuserver  12.04.5-LTS        westus
data:    canonical  ubuntuserver  12.10              westus
data:    canonical  ubuntuserver  14.04-beta         westus
data:    canonical  ubuntuserver  14.04.0-LTS        westus
data:    canonical  ubuntuserver  14.04.1-LTS        westus
data:    canonical  ubuntuserver  14.04.2-LTS        westus
data:    canonical  ubuntuserver  14.04.3-LTS        westus
data:    canonical  ubuntuserver  14.04.4-DAILY-LTS  westus
data:    canonical  ubuntuserver  14.04.4-LTS        westus
data:    canonical  ubuntuserver  14.04.5-DAILY-LTS  westus
data:    canonical  ubuntuserver  14.04.5-LTS        westus
data:    canonical  ubuntuserver  14.10              westus
data:    canonical  ubuntuserver  14.10-beta         westus
data:    canonical  ubuntuserver  14.10-DAILY        westus
data:    canonical  ubuntuserver  15.04              westus
data:    canonical  ubuntuserver  15.04-beta         westus
data:    canonical  ubuntuserver  15.04-DAILY        westus
data:    canonical  ubuntuserver  15.10              westus
data:    canonical  ubuntuserver  15.10-alpha        westus
data:    canonical  ubuntuserver  15.10-beta         westus
data:    canonical  ubuntuserver  15.10-DAILY        westus
data:    canonical  ubuntuserver  16.04-alpha        westus
data:    canonical  ubuntuserver  16.04-beta         westus
data:    canonical  ubuntuserver  16.04.0-DAILY-LTS  westus
data:    canonical  ubuntuserver  16.04.0-LTS        westus
data:    canonical  ubuntuserver  16.10-DAILY        westus
info:    vm image list-skus command OK
```

Mit diesen Informationen können Sie nun genau das von Ihnen gewünschte Image finden, indem Sie den ursprünglichen Aufruf oben aufrufen.

```azurecli
azure vm image list westus canonical ubuntuserver 16.04.0-LTS
info:    Executing command vm image list
+ Getting virtual machine images (Publisher:"canonical" Offer:"ubuntuserver" Sku: "16.04.0-LTS" Location:"westus")
data:    Publisher  Offer         Sku          OS     Version          Location  Urn
data:    ---------  ------------  -----------  -----  ---------------  --------  --------------------------------------------------
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201604203  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201604203
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201605161  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201605161
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201606100  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606100
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201606270  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606270
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201607210  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201607210
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201608150  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201608150
info:    vm image list command OK
```

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
Jetzt können Sie genau das Datenträgerabbild auswählen, das Sie verwenden möchten. Um schnell mithilfe der von Ihnen soeben gefundenen URN-Informationen einen virtuellen Computer zu erstellen oder eine Vorlage mit diesen URN-Informationen zu verwenden, lesen Sie die Informationen unter [Erstellen einer Linux-VM von Grund auf mit der Azure-Befehlszeilenschnittstelle](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

