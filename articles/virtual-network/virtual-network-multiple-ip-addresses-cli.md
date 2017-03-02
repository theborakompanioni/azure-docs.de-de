---
title: "Mehrere IP-Adressen für virtuelle Computer: Azure CLI | Microsoft-Dokumentation"
description: Erfahren Sie mehr zum Zuweisen von mehreren IP-Adressen zu einem virtuellen Computer mit Azure CLI. | Resource Manager
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
ms.sourcegitcommit: 7e99731f6826e563109da734a80fcccfff85676a
ms.openlocfilehash: 3deb0e1668aae15a7d9abe00791f4524ed49d77b
ms.lasthandoff: 02/21/2017


---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-azure-cli"></a>Zuweisen von mehreren IP-Adressen zu virtuellen Computern mit Azure CLI

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

In diesem Artikel wird beschrieben, wie Sie über das Azure Resource Manager-Bereitstellungsmodell mithilfe der Azure CLI einen virtuellen Computer erstellen. Ressourcen, die mit dem klassischen Bereitstellungsmodell erstellt wurden, können nicht mehrere IP-Adressen zugewiesen werden. Weitere Informationen zu den Azure-Bereitstellungsmodellen finden Sie im Artikel zum Thema [Understand deployment models (Bereitstellungsmodelle verstehen)](../resource-manager-deployment-model.md).

[!INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name="a-name--createacreate-a-vm-with-multiple-ip-addresses"></a><a name = "create"></a>Erstellen eines virtuellen Computers mit mehreren IP-Adressen

In den folgenden Schritten wird beschrieben, wie gemäß dem Szenario beispielhaft ein virtueller Computer mit mehreren IP-Adressen erstellt werden kann. Sie können die Variablennamen und IP-Adresstypen an Ihren Implementierungsbedarf anpassen.

1. Installieren und konfigurieren Sie die Azure CLI anhand der Schritte im Artikel [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), und melden Sie sich dann an Ihrem Azure-Konto an.

2. Registrieren Sie sich für die Vorschau, indem Sie in PowerShell die folgenden Befehle ausführen, nachdem Sie sich angemeldet und das entsprechende Abonnement ausgewählt haben:
    ```
    Register-AzureRmProviderFeature -FeatureName AllowMultipleIpConfigurationsPerNic -ProviderNamespace Microsoft.Network

    Register-AzureRmProviderFeature -FeatureName AllowLoadBalancingonSecondaryIpconfigs -ProviderNamespace Microsoft.Network
    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ```
    Führen Sie die restlichen Schritte erst aus, nachdem nach Ausführen des Befehls ```Get-AzureRmProviderFeature``` die folgende Ausgabe angezeigt wird:
        
    ```powershell
    FeatureName                            ProviderName      RegistrationState
    -----------                            ------------      -----------------      
    AllowLoadBalancingOnSecondaryIpConfigs Microsoft.Network Registered       
    AllowMultipleIpConfigurationsPerNic    Microsoft.Network Registered       
    ```
        
    >[!NOTE] 
    >Dies kann einige Minuten dauern.

3. [Erstellen Sie eine Ressourcengruppe](../virtual-machines/virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-resource-groups-and-choose-deployment-locations) und anschließend ein [virtuelles Netzwerk und ein Subnetz](../virtual-machines/virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-virtual-network-and-subnet). Ändern Sie die Felder ``` --address-prefixes ``` und ```--address-prefix``` auf die folgenden Werte, um das in diesem Artikel skizzierte Szenario exakt einzuhalten:

    ```azurecli
    --address-prefixes 10.0.0.0/16
    --address-prefix 10.0.0.0/24
    ```
    >[!NOTE] 
    >Im oben referenzierten Artikel wird Europa, Westen als Standort für die Ressourcenerstellung gewählt, aber in diesem Artikel verwenden wir die Region „USA, Westen-Mitte“. Führen Sie bitte die entsprechenden Änderungen durch.

4. [Erstellen Sie ein Speicherkonto](../virtual-machines/virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-storage-account) für Ihren virtuellen Computer.

5. Erstellen Sie NIC und die IP-Konfigurationen, die Sie der NIC zuweisen möchten. Sie können Konfigurationen je nach Bedarf hinzufügen, entfernen oder ändern. Die folgenden Konfigurationen werden in dem Szenario beschrieben:

    **IPConfig-1**

    Geben Sie die folgenden Befehle ein, um die folgenden Elemente zu erstellen:

    - eine öffentliche IP-Adressressource mit einer statischen öffentlichen IP-Adresse
    - eine IP-Konfiguration mit der öffentlichen IP-Adressressource und einer dynamischen privaten IP-Adresse

    ```azurecli
    azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP --domain-name-label mypublicdns --allocation-method Static
    ```
    > [!NOTE]
    > Für öffentliche IP-Adressen fällt eine geringe Gebühr an. Weitere Informationen zu den Preisen finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses) . Die Anzahl der öffentlichen IP-Adressen, die in einem Abonnement verwendet werden können, ist beschränkt. Weitere Informationen über die Einschränkungen finden Sie im Artikel zu den [Azure-Einschränkungen](../azure-subscription-service-limits.md#networking-limits) .

    ```azurecli
    azure network nic create --resource-group myResourceGroup --location westcentralus --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 --public-ip-name myPublicIP
    ```

    **IPConfig-2**

     Geben Sie die folgenden Befehle ein, um eine neue öffentliche IP-Adressressource und eine neue IP-Konfiguration mit einer statischen öffentlichen IP-Adresse und einer statischen privaten IP-Adresse zu erstellen:
    
    ```azurecli
    azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP2 --domain-name-label mypublicdns2 --allocation-method Static

    azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-2 --private-ip-address 10.0.0.5 --public-ip-name myPublicIP2
    ```

    **IPConfig-3**

    Geben Sie die folgenden Befehle ein, um eine IP-Konfiguration mit einer dynamischen privaten IP-Adresse und ohne öffentliche IP-Adressen zu erstellen:

    ```azurecli
    azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3
    ```

    >[!NOTE] 
    >Obwohl in diesem Artikel alle IP-Konfigurationen einer einzigen NIC zugewiesen werden, ist es auch möglich, jeder NIC in einem virtuellen Computer mehrere IP-Konfigurationen zuzuweisen. Erfahren Sie im Artikel „Erstellen eines virtuellen Computers mit mehreren Netzwerkschnittstellenkarten (NICs)“, wie Sie einen virtuellen Computer mit mehreren NICs erstellen.

6. Artikel: [Erstellen der Linux-VMs](../virtual-machines/virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-the-linux-vms). Achten Sie darauf, die Eigenschaft ```  --availset-name myAvailabilitySet \ ``` zu entfernen, da sie in diesem Szenario nicht benötigt wird. Verwenden Sie basierend auf Ihrem Szenario den richtigen Standort. 

    >[!WARNING] 
    > Schritt 6 im Artikel „Erstellen eines virtuellen Computers“ wird scheitern, wenn die Größe des virtuellen Computers an dem von Ihnen gewählten Standort nicht unterstützt wird. Führen Sie den folgenden Befehl aus, um beispielsweise eine vollständige Liste der virtuellen Computer in der Region „USA, Westen-Mitte“ zu erhalten: `azure vm sizes --location westcentralus` Dieser Standortname kann Ihrem Szenario entsprechend geändert werden.

    Um z.B. die Größe des virtuellen Computers auf Standard DS2 v2 zu ändern, fügen Sie einfach die Eigenschaft ```  --vm-size Standard_DS3_v2``` zu dem Befehl ``` azure vm create ``` in Schritt 6 hinzu.

7. Geben Sie den folgenden Befehl ein, um die NIC und die zugeordneten IP-Konfigurationen anzuzeigen:

    ```azurecli
    azure network nic show --resource-group myResourceGroup --name myNic1
    ```
8. Fügen Sie die privaten IP-Adressen dem Betriebssystem des virtuellen Computers hinzu. Führen Sie dazu die Schritte für Ihr Betriebssystem im Abschnitt [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](#os-config) in diesem Artikel aus.

## <a name="a-nameaddaadd-ip-addresses-to-a-vm"></a><a name="add"></a>Hinzufügen von IP-Adressen zu einem virtuellen Computer

Sie können zu einer vorhandenen NIC zusätzliche private und öffentliche IP-Adressen hinzufügen. Führen Sie dazu die folgenden Schritte aus. Die Beispiele bauen auf dem in diesem Artikel beschriebenen [Szenario](#Scenario) auf.

1. Öffnen Sie Azure CLI, und führen Sie die in diesem Abschnitt verbleibenden Schritte innerhalb einer einzigen CLI-Sitzung aus. Wenn Azure CLI nicht schon installiert und konfiguriert ist, installieren und konfigurieren Sie Azure CLI anhand der Schritte im Artikel [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md), und melden Sie sich dann bei Ihrem Azure-Konto an.

2. Registrieren Sie sich für die öffentliche Vorschau, indem Sie Schritt 2 im Abschnitt **Erstellen eines virtuellen Computers mit mehreren IP-Adressen** ausführen.

3. Führen Sie gemäß Ihren Anforderungen die Schritte eines der folgenden Abschnitte aus:

    **Hinzufügen einer privaten IP-Adresse**
    
    Um eine private IP-Adresse zu einer NIC hinzuzufügen, müssen Sie mit dem folgenden Befehl eine IP-Konfiguration erstellen.  Wenn Sie eine dynamische private IP-Adresse hinzufügen möchten, entfernen Sie vor der Eingabe des Befehls ```-PrivateIpAddress 10.0.0.7```. Wenn Sie eine statische IP-Adresse angeben, darf es sich dabei nicht um eine für das Subnetz verwendete Adresse handeln.

    ```azurecli
    azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 --private-ip-address 10.0.0.7 --name IPConfig-4
    ```
    Sie können so viele Konfigurationen wie benötigt erstellen. Verwenden Sie dabei eindeutige Namen für die Konfigurationen und privaten IP-Adressen (bei Konfigurationen mit statischen IP-Adressen).

    **Hinzufügen einer öffentlichen IP-Adresse**
    
    Eine öffentliche IP-Adresse wird hinzugefügt, indem entweder eine neue oder eine vorhandene IP-Konfiguration zugeordnet wird. Führen Sie gemäß Ihren Anforderungen die Schritte des entsprechenden Abschnitts durch.

    > [!NOTE]
    > Für öffentliche IP-Adressen fällt eine geringe Gebühr an. Weitere Informationen zu den Preisen finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses) . Die Anzahl der öffentlichen IP-Adressen, die in einem Abonnement verwendet werden können, ist beschränkt. Weitere Informationen über die Einschränkungen finden Sie im Artikel zu den [Azure-Einschränkungen](../azure-subscription-service-limits.md#networking-limits) .
    >

    **Zuordnen der Ressource zu einer neuen IP-Konfiguration**
    
    Immer, wenn Sie in einer neuen IP-Konfiguration eine öffentliche IP-Adresse hinzufügen, müssen Sie auch eine private IP-Adresse hinzufügen, denn in jeder IP-Konfiguration muss es eine private IP-Adresse geben. Sie können entweder eine vorhandene öffentliche IP-Adressressource hinzufügen oder eine neue erstellen. Verwenden Sie den folgenden Befehl, um eine neue zu erstellen:
    
    ```azurecli
      azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP3 --domain-name-label mypublicdns3
    ```

     Um eine neue IP-Konfiguration mit einer dynamischen privaten IP-Adresse und der zugeordneten öffentlichen IP-Adressressource *myPublicIP3* zu erstellen, geben Sie den folgenden Befehl ein:

    ```azurecli
    azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic --name IPConfig-4 --public-ip-name myPublicIP3
    ```

    **Zuordnen der Ressource zu einer vorhandenen IP-Konfiguration**
    Eine öffentliche IP-Adressressource kann nur einer IP-Konfiguration zugeordnet werden, der noch keine öffentliche IP-Adressressource zugeordnet ist. Ob einer IP-Konfiguration eine öffentliche IP-Adresse zugeordnet ist, können Sie mithilfe des folgenden Befehls ermitteln:

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```

    Suchen Sie nach einer Zeile, die der folgenden Rückgabezeile ähnelt:
    
        Name               Provisioning state  Primary  Private IP allocation  Private IP version  Private IP address  Subnet    Public IP
        -----------------  ------------------  -------  ---------------------  ------------------  ------------------  --------  -----------
        default-ip-config  Succeeded           true     Dynamic                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                 IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Dynamic                IPv4                10.0.0.6            mySubnet
     
    Da die Spalte **Public IP** für *IpConfig-3* leer ist, ist derzeit keine öffentliche IP-Adressressource zugeordnet. Sie können IpConfig-3 eine vorhandene öffentliche IP-Adressressource zuordnen oder mit dem folgenden Befehl eine erstellen:

    ```azurecli
    azure network public-ip create --resource-group  myResourceGroup --location westcentralus --name myPublicIP3 --domain-name-label mypublicdns3 --allocation-method Static
    ```
    
    Geben Sie den folgenden Befehl ein, um die öffentliche IP-Adressressource der vorhandenen IP-Konfiguration namens *IPConfig-3* zuzuordnen:
    
    ```azurecli
    azure network nic ip-config set --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3 --public-ip-name myPublicIP3
    ```

7. Geben Sie den folgenden Befehl ein, um die privaten IP-Adressen und die der NIC zugewiesene öffentliche IP-Adressressource anzuzeigen:

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```
    Eine Ausgabe ähnlich der folgenden sollte angezeigt werden: 
    
        Name               Provisioning state  Primary  Private IP allocation  Private IP version  Private IP address  Subnet    Public IP
        -----------------  ------------------  -------  ---------------------  ------------------  ------------------  --------  -----------
        default-ip-config  Succeeded           true     Dynamic                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                 IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Dynamic                IPv4                10.0.0.6            mySubnet  myPublicIP3
     
9. Fügen Sie die privaten IP-Adressen hinzu, die Sie der NIC im Betriebssystem des virtuellen Computers hinzugefügt haben. Befolgen Sie dazu die Anweisungen im Abschnitt [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](#os-config) in diesem Artikel. Fügen Sie dem Betriebssystem nicht die öffentlichen IP-Adressen hinzu.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]

