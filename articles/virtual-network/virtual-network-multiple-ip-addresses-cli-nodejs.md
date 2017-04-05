---
title: "Virtueller Computer mit mehreren IP-Adressen – Azure CLI 1.0 | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie mit der Azure CLI 1.0 und Resource Manager mehrere IP-Adressen zu einem virtuellen Computer zuweisen.
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: annahar
translationtype: Human Translation
ms.sourcegitcommit: 6e0ad6b5bec11c5197dd7bded64168a1b8cc2fdd
ms.openlocfilehash: 9f085dfa1fe4db36d58cb976bb550a46bf241ac7
ms.lasthandoff: 03/28/2017


---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-azure-cli-10"></a>Zuweisen von mehreren IP-Adressen zu virtuellen Computern mit der Azure CLI 1.0

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

In diesem Artikel wird beschrieben, wie Sie über das Azure Resource Manager-Bereitstellungsmodell mit der Azure CLI 1.0 einen virtuellen Computer (VM) erstellen. Ressourcen, die mit dem klassischen Bereitstellungsmodell erstellt wurden, können nicht mehrere IP-Adressen zugewiesen werden. Weitere Informationen zu den Azure-Bereitstellungsmodellen finden Sie im Artikel zum Thema [Understand deployment models (Bereitstellungsmodelle verstehen)](../resource-manager-deployment-model.md).

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Erstellen eines virtuellen Computers mit mehreren IP-Adressen

Sie können diese Aufgabe mithilfe der Azure CLI 1.0 (dieser Artikel) oder mit der [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md) ausführen. In den folgenden Schritten wird beschrieben, wie gemäß dem Szenario beispielhaft ein virtueller Computer mit mehreren IP-Adressen erstellt werden kann. Sie können die Variablennamen und IP-Adresstypen an Ihren Implementierungsbedarf anpassen.

1. Installieren und konfigurieren Sie die Azure CLI 1.0 anhand der Schritte im Artikel [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), und melden Sie sich dann mit dem Befehl `azure-login` bei Ihrem Azure-Konto an.

2. Erstellen Sie eine Ressourcengruppe:
    
    ```azurecli
    RgName=myResourceGroup
    Location=westcentralus
    azure group create --name $RgName --location $Location
    ```
3. Erstellen Sie ein virtuelles Netzwerk:

    ```azurecli
    azure network vnet create --resource-group $RgName --location $Location --name myVNet \
    --address-prefixes 10.0.0.0/16
    ```
4. Erstellen Sie ein Subnetz im virtuellen Netzwerk:

    ```azurecli
    azure network vnet subnet create --name mySubnet --resource-group $RgName --vnet-name myVNet \
    --address-prefix 10.0.0.0/24
    ```
    
5. Erstellen Sie ein Speicherkonto für den virtuellen Computer. Ersetzen Sie *mystorageaccount* vor dem Ausführen des folgenden Befehls durch einen eindeutigen Namen. Der Name muss innerhalb von Azure eindeutig sein:

    ```azurecli
    az storage account create --resource-group $RgName --location $Location --name mystorageaccount \
    --kind Storage --sku Standard_LRS
    ```

6. Erstellen Sie die IP-Konfigurationen und eine NIC, und weisen Sie der NIC die IP-Konfigurationen zu. Sie können Konfigurationen je nach Bedarf hinzufügen, entfernen oder ändern. Die folgenden Konfigurationen werden in dem Szenario beschrieben:

    **IPConfig-1**

    Geben Sie die folgenden Befehle ein, um die folgenden Elemente zu erstellen:

    - eine öffentliche IP-Adressressource mit einer statischen öffentlichen IP-Adresse
    - eine NIC, der die öffentliche IP-Adresse und eine statische private IP-Adresse zugewiesen wird
    
    Ersetzen Sie *mypublicdns* durch einen Namen, der innerhalb des Azure-Standorts eindeutig ist.

      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP1 \
      --domain-name-label mypublicdns --allocation-method Static
        
      azure network nic create --resource-group $RgName --location $Location --name myNic1 \
      --private-ip-address 10.0.0.4 --subnet-name mySubnet --subnet-vnet-name myVNet \
      --subnet-name mySubnet --public-ip-name myPublicIP1
      ```

      > [!NOTE]
      > Für öffentliche IP-Adressen fällt eine geringe Gebühr an. Weitere Informationen zu den Preisen finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses) . Die Anzahl der öffentlichen IP-Adressen, die in einem Abonnement verwendet werden können, ist beschränkt. Weitere Informationen über die Einschränkungen finden Sie im Artikel zu den [Azure-Einschränkungen](../azure-subscription-service-limits.md#networking-limits) .

    **IPConfig-2**

     Geben Sie die folgenden Befehle ein, um eine neue öffentliche IP-Adressressource und eine neue IP-Konfiguration mit einer statischen öffentlichen IP-Adresse und einer statischen privaten IP-Adresse zu erstellen:
    
      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP2
      --domain-name-label mypublicdns2 --allocation-method Static

      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --name IPConfig-2
      --private-ip-address 10.0.0.5 --public-ip-name myPublicIP2
      ```

    **IPConfig-3**

    Geben Sie die folgenden Befehle ein, um eine IP-Konfiguration mit einer statischen privaten IP-Adresse und ohne öffentliche IP-Adresse zu erstellen:

      ```azurecli
      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --private-ip-address 10.0.0.6 \
      --name IPConfig-3
      ```

    >[!NOTE] 
    >Obwohl in diesem Artikel alle IP-Konfigurationen einer einzigen NIC zugewiesen werden, ist es auch möglich, jeder NIC in einem virtuellen Computer mehrere IP-Konfigurationen zuzuweisen. Erfahren Sie im Artikel „Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellenkarten (NICs)“, wie Sie einen virtuellen Computer mit mehreren NICs erstellen.

7. Erstellen eines virtuellen Linux-Computers 

    ```azurecli
    az vm create --resource-group $RgName --name myVM1 --location $Location --nics myNic1 \
    --image UbuntuLTS --ssh-key-value ~/.ssh/id_rsa.pub --admin-username azureuser
    ```

    Um z.B. die Größe des virtuellen Computers auf Standard DS2 v2 zu ändern, fügen Sie einfach die Eigenschaft `--vm-size Standard_DS3_v2` zu dem Befehl `azure vm create` in Schritt 6 hinzu.

8. Geben Sie den folgenden Befehl ein, um die NIC und die zugeordneten IP-Konfigurationen anzuzeigen:

    ```azurecli
    azure network nic show --resource-group $RgName    --name myNic1
    ```
9. Fügen Sie die privaten IP-Adressen dem Betriebssystem des virtuellen Computers hinzu. Führen Sie dazu die Schritte für Ihr Betriebssystem im Abschnitt [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](#os-config) in diesem Artikel aus.

## <a name="add"></a>Hinzufügen von IP-Adressen zu einem virtuellen Computer

Sie können zu einer vorhandenen NIC zusätzliche private und öffentliche IP-Adressen hinzufügen. Führen Sie dazu die folgenden Schritte aus. Die Beispiele bauen auf dem in diesem Artikel beschriebenen [Szenario](#Scenario) auf.

1. Öffnen Sie Azure CLI, und führen Sie die in diesem Abschnitt verbleibenden Schritte innerhalb einer einzigen CLI-Sitzung aus. Wenn Azure CLI nicht schon installiert und konfiguriert ist, installieren und konfigurieren Sie Azure CLI anhand der Schritte im Artikel [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), und melden Sie sich dann bei Ihrem Azure-Konto an.

2. Führen Sie gemäß Ihren Anforderungen die Schritte eines der folgenden Abschnitte aus:

    - **Hinzufügen einer privaten IP-Adresse**
    
        Um eine private IP-Adresse zu einer NIC hinzuzufügen, müssen Sie mit dem folgenden Befehl eine IP-Konfiguration erstellen. Bei der statischen Adresse darf es sich nicht um eine für das Subnetz verwendete Adresse handeln.

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 \
        --private-ip-address 10.0.0.7 --name IPConfig-4
        ```
        Sie können so viele Konfigurationen wie benötigt erstellen. Verwenden Sie dabei eindeutige Namen für die Konfigurationen und privaten IP-Adressen (bei Konfigurationen mit statischen IP-Adressen).

    - **Hinzufügen einer öffentlichen IP-Adresse**
    
        Eine öffentliche IP-Adresse wird hinzugefügt, indem entweder eine neue oder eine vorhandene IP-Konfiguration zugeordnet wird. Führen Sie gemäß Ihren Anforderungen die Schritte des entsprechenden Abschnitts durch.

        > [!NOTE]
        > Für öffentliche IP-Adressen fällt eine geringe Gebühr an. Weitere Informationen zu den Preisen finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses) . Die Anzahl der öffentlichen IP-Adressen, die in einem Abonnement verwendet werden können, ist beschränkt. Weitere Informationen über die Einschränkungen finden Sie im Artikel zu den [Azure-Einschränkungen](../azure-subscription-service-limits.md#networking-limits) .
        >

        **Zuordnen der Ressource zu einer neuen IP-Konfiguration**
    
        Immer, wenn Sie in einer neuen IP-Konfiguration eine öffentliche IP-Adresse hinzufügen, müssen Sie auch eine private IP-Adresse hinzufügen, denn in jeder IP-Konfiguration muss es eine private IP-Adresse geben. Sie können entweder eine vorhandene öffentliche IP-Adressressource hinzufügen oder eine neue erstellen. Verwenden Sie den folgenden Befehl, um eine neue zu erstellen:

        ```azurecli
        azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP3 \
        --domain-name-label mypublicdns3
        ```

         Um eine neue IP-Konfiguration mit einer statischen privaten IP-Adresse und der zugeordneten öffentlichen IP-Adressressource *myPublicIP3* zu erstellen, geben Sie den folgenden Befehl ein:

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic --name IPConfig-4 \
        --private-ip-address 10.0.0.8 --public-ip-name myPublicIP3
        ```

        **Zuordnen der Ressource zu einer vorhandenen IP-Konfiguration**

        Eine öffentliche IP-Adressressource kann nur einer IP-Konfiguration zugeordnet werden, der noch keine öffentliche IP-Adressressource zugeordnet ist. Ob einer IP-Konfiguration eine öffentliche IP-Adresse zugeordnet ist, können Sie mithilfe des folgenden Befehls ermitteln:

        ```azurecli
        azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
        ```

        Suchen Sie nach einer Zeile, die der folgenden Rückgabezeile für IPConfig-3 ähnelt:

        ```            
        Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet
        ```
          
        Da die Spalte **Public IP** für *IpConfig-3* leer ist, ist derzeit keine öffentliche IP-Adressressource zugeordnet. Sie können IpConfig-3 eine vorhandene öffentliche IP-Adressressource zuordnen oder mit dem folgenden Befehl eine erstellen:

        ```azurecli
        azure network public-ip create --resource-group  myResourceGroup --location westcentralus \
        --name myPublicIP3 --domain-name-label mypublicdns3 --allocation-method Static
        ```

        Geben Sie den folgenden Befehl ein, um die öffentliche IP-Adressressource der vorhandenen IP-Konfiguration namens *IPConfig-3* zuzuordnen:
        ```azurecli
        azure network nic ip-config set --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3 \
        --public-ip-name myPublicIP3
        ```

3. Geben Sie den folgenden Befehl ein, um die privaten IP-Adressen und die der NIC zugewiesene öffentliche IP-Adressressource anzuzeigen:

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```

      Die Rückgabe sieht in etwa wie folgt aus:
      ```
      Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        
      default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
      IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
      IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet  myPublicIP3
      ```
4. Fügen Sie die privaten IP-Adressen hinzu, die Sie der NIC im Betriebssystem des virtuellen Computers hinzugefügt haben. Befolgen Sie dazu die Anweisungen im Abschnitt [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](#os-config) in diesem Artikel. Fügen Sie dem Betriebssystem nicht die öffentlichen IP-Adressen hinzu.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]

