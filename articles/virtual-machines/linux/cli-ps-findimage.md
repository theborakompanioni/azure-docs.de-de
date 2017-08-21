---
title: "Auswählen von Linux-VM-Images mit der Azure CLI | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die Verwendung der Azure CLI, um den Herausgeber, das Angebot, die SKU und die Version für Marketplace-VM-Images zu ermitteln."
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
ms.date: 07/11/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 818f7756189ed4ceefdac9114a0b89ef9ee8fb7a
ms.openlocfilehash: 72c5c2efe2c8a60f13d18b595062448e6a0d1816
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---
# <a name="how-to-find-linux-vm-images-in-the-azure-marketplace-with-the-azure-cli"></a>Vorgehensweise zum Suchen nach Linux-VM-Images im Azure Marketplace mit der Azure CLI
Dieses Thema beschreibt, wie Sie mit Azure CLI 2.0 nach VM-Images im Azure Marketplace suchen. Verwenden Sie diese Informationen, um bei der Erstellung einer Linux-VM ein Marketplace-Image anzugeben.

Stellen Sie sicher, dass Sie die aktuelle Version von [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert haben und bei einem Azure-Konto (`az login`) angemeldet sind.

## <a name="list-popular-images"></a>Liste von beliebten Images

Führen Sie den Befehl [az vm image list](/cli/azure/vm/image#list) ohne die Option `--all` aus, um eine Liste beliebter VM-Images im Azure Marketplace anzuzeigen. Führen Sie beispielsweise den folgenden Befehl aus, um eine zwischengespeicherte Liste beliebter Images im Tabellenformat anzuzeigen:

```azurecli
az vm image list --output table
```

Die Ausgabe umfasst den URN (den Wert in der Spalte *URN*), der folgendem Muster folgt: *Herausgeber*:*Angebot*:*SKU*:*Version*. Verwenden Sie diesen Wert, um bei der Erstellung einer VM ein Image mit `az vm create` anzugeben. Beim Erstellen einer VM mit einem der beliebten VM-Images können Sie alternativ den URN-Alias angeben, z.B. *UbuntuLTS*.

```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
WindowsServer  MicrosoftWindowsServer  2016-Datacenter     MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest     Win2016Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
```

## <a name="list-all-current-images"></a>Liste aller aktuellen Images

Verwenden Sie zum Abrufen der aktuellen Liste aller VM-Images im Marketplace den Befehl `az vm image list` mit der Option `--all`. Diese Version des Befehls nimmt einige Zeit in Anspruch:


```azurecli
az vm image list --all
```

Wenn Sie keinen bestimmten Standort mit der Option `--location` angeben, werden standardmäßig die Werte für `westus` zurückgegeben. (Legen Sie einen anderen Standardstandort fest, indem Sie `az configure --defaults location=<location>` ausführen.)


Wenn Sie eine interaktive Untersuchung beabsichtigen, leiten Sie die Ausgabe an eine lokale Datei weiter. Beispiel:

```azurecli
az vm image list --all > allImages.json
```




## <a name="find-specific-images"></a>Suchen nach bestimmten Images

Verwenden Sie `az vm image list` mit zusätzlichen Optionen, um Ihre Suche auf einen bestimmten Standort, ein Angebot, einen Herausgeber oder eine SKU zu beschränken. Mit dem folgenden Code werden beispielsweise alle Debian-Angebote angezeigt (zur Erinnerung: Ohne den Switch `--all` wird nur der lokale Cache von allgemeinen Images durchsucht):

```azurecli
az vm image list --offer Debian --all --output table 
```

Da die Ausgabe eine lange Liste sein kann, wird sie hier abgeschnitten: 
```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  --------------
Debian   credativ     7                  credativ:Debian:7:7.0.201602010                  7.0.201602010
Debian   credativ     7                  credativ:Debian:7:7.0.201603020                  7.0.201603020
Debian   credativ     7                  credativ:Debian:7:7.0.201604050                  7.0.201604050
Debian   credativ     7                  credativ:Debian:7:7.0.201604200                  7.0.201604200
Debian   credativ     7                  credativ:Debian:7:7.0.201606280                  7.0.201606280
Debian   credativ     7                  credativ:Debian:7:7.0.201609120                  7.0.201609120
Debian   credativ     7                  credativ:Debian:7:7.0.201611020                  7.0.201611020
...
```


Wenden Sie ähnliche Filter mit den Optionen `--location`, `--publisher` und `--sku` an. Sie können sogar Teilübereinstimmungen für einen Filter durchführen, z.B. bei der Suche nach `--offer Deb`, um alle Debian-Images zu finden.

Beispielsweise listet der folgende Befehl listet alle Debian 8-SKUs in `westeurope` auf:

```azurecli
az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
```

Ausgabe:

```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  -------------
Debian   credativ     8                  credativ:Debian:8:8.0.201602010                  8.0.201602010
Debian   credativ     8                  credativ:Debian:8:8.0.201603020                  8.0.201603020
Debian   credativ     8                  credativ:Debian:8:8.0.201604050                  8.0.201604050
Debian   credativ     8                  credativ:Debian:8:8.0.201604200                  8.0.201604200
Debian   credativ     8                  credativ:Debian:8:8.0.201606280                  8.0.201606280
Debian   credativ     8                  credativ:Debian:8:8.0.201609120                  8.0.201609120
Debian   credativ     8                  credativ:Debian:8:8.0.201611020                  8.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201701180                  8.0.201701180
Debian   credativ     8                  credativ:Debian:8:8.0.201703150                  8.0.201703150
Debian   credativ     8                  credativ:Debian:8:8.0.201704110                  8.0.201704110
Debian   credativ     8                  credativ:Debian:8:8.0.201704180                  8.0.201704180
Debian   credativ     8                  credativ:Debian:8:8.0.201706190                  8.0.201706190
Debian   credativ     8                  credativ:Debian:8:8.0.201706210                  8.0.201706210
...
```


## <a name="navigate-the-images"></a>Navigieren zu den Images 
Eine weitere Möglichkeit für die Suche nach einem Image an einem Standort besteht darin, die Befehle [az vm image list-publishers](/cli/azure/vm/image#list-publishers), [az vm image list-offers](/cli/azure/vm/image#list-offers) und [az vm image list-skus](/cli/azure/vm/image#list-skus) nacheinander auszuführen. Mit diesen Befehlen ermitteln Sie folgende Werte:

1. Auflistung der Herausgeber von Images
2. Auflistung der Angebote eines bestimmten Anbieters
3. Auflistung der SKUs eines bestimmten Angebots


Beispielsweise listet der folgende Befehl die Herausgeber von Images für den Standort „USA, Westen“ auf:

```azurecli
az vm image list-publishers --location westus --output table
```

Ausgabe:

```
Location    Name
----------  ----------------------------------------------------
westus      4psa
westus      7isolutions
westus      a10networks
westus      abiquo
westus      accellion
westus      Acronis
westus      Acronis.Backup
westus      actian_matrix
westus      actifio
westus      activeeon
westus      adatao
...
```
Diese Listen können lang sein, daher stellt diese Beispielausgabe nur einen Ausschnitt dar. Verwenden Sie diese Informationen, um nach Angeboten eines bestimmten Herausgebers zu suchen. Wenn beispielsweise „Canonical“ als Herausgeber von Images für den Standort „USA, Westen“ festgelegt ist, suchen Sie nach dessen Angeboten, indem Sie `azure vm image list-offers` ausführen. Übergeben Sie den Standort und den Herausgeber, wie im folgenden Beispiel gezeigt wird:

```azurecli
az vm image list-offers --location westus --publisher Canonical --output table
```

Ausgabe:

```
Location    Name
----------  -------------------------
westus      Ubuntu15.04Snappy
westus      Ubuntu15.04SnappyDocker
westus      UbunturollingSnappy
westus      UbuntuServer
westus      Ubuntu_Core
westus      Ubuntu_Snappy_Core
westus      Ubuntu_Snappy_Core_Docker
```
Sie sehen, dass in der Region „USA, Westen“ Canonical das **UbuntuServer**-Angebot in Azure herausgibt. Aber welche SKUs? Um diese Werte zu erhalten, führen Sie `azure vm image list-skus` aus, und legen Sie den Standort, Herausgeber und das von Ihnen entdeckte Angebot fest:

```azurecli
az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
```

Ausgabe:

```
Location    Name
----------  -----------------
westus      12.04.3-LTS
westus      12.04.4-LTS
westus      12.04.5-DAILY-LTS
westus      12.04.5-LTS
westus      12.10
westus      14.04.0-LTS
westus      14.04.1-LTS
westus      14.04.2-LTS
westus      14.04.3-LTS
westus      14.04.4-LTS
westus      14.04.5-DAILY-LTS
westus      14.04.5-LTS
westus      16.04-beta
westus      16.04-DAILY-LTS
westus      16.04-LTS
westus      16.04.0-LTS
westus      16.10
westus      16.10-DAILY
westus      17.04
westus      17.04-DAILY
westus      17.10-DAILY
```

Verwenden Sie abschließend den Befehl `az vm image list`, um nach einer bestimmten gewünschten Version der SKU zu suchen, z.B. **14.04-LTS**:

```azurecli
az vm image list --location westus --publisher Canonical --offer UbuntuServer --sku 16.04-LTS --all --output table
```

Ausgabe:

```
Offer         Publisher    Sku        Urn                                               Version
------------  -----------  ---------  ------------------------------------------------  ---------------
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201611220  16.04.201611220
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201611300  16.04.201611300
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612050  16.04.201612050
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612140  16.04.201612140
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612210  16.04.201612210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201701130  16.04.201701130
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702020  16.04.201702020
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702200  16.04.201702200
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702210  16.04.201702210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702240  16.04.201702240
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703020  16.04.201703020
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703030  16.04.201703030
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703070  16.04.201703070
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703270  16.04.201703270
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703280  16.04.201703280
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703300  16.04.201703300
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201705080  16.04.201705080
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201705160  16.04.201705160
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201706100  16.04.201706100
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201706191  16.04.201706191
```
## <a name="next-steps"></a>Nächste Schritte
Jetzt können Sie genau das Image auswählen, das Sie verwenden möchten. Notieren Sie sich hierzu den Wert des URN. Wenn Sie das Image angeben, können Sie optional die Versionsnummer im URN durch „latest“ ersetzen. Diese Version ist immer die neueste Version der Verteilung. Um schnell mithilfe der von Ihnen soeben gefundenen URN-Informationen einen virtuellen Computer zu erstellen, lesen Sie die Informationen unter [Erstellen einer Linux-VM mit der Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

