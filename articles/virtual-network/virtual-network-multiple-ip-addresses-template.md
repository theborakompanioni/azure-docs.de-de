---
title: "Mehrere IP-Adressen für virtuelle Azure Computer – Vorlage | Microsoft Docs"
description: "Erfahren Sie, wie Sie einem virtuellen Azure-Computer mehrere IP-Adressen mit einer Azure Resource Manager-Vorlage zuordnen können."
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/08/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 3eda8b459b5f095a40c6ea1ed355472daf23a6e3
ms.openlocfilehash: ae5c430e702b561ddf156aa29016cfec6a0a8153


---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-an-azure-resource-manager-template"></a>Zuweisen von mehreren IP-Adressen zu virtuellen Computern mit einer Azure Resource Manager-Vorlage

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

In diesem Artikel wird beschrieben, wie Sie über das Azure Resource Manager-Bereitstellungsmodell mithilfe einer Resource Manager-Vorlage einen virtuellen Computer erstellen. Es ist nicht möglich, einer Netzwerkkarte mehrere öffentliche und private IP-Adressen zuzuweisen, wenn ein virtueller Computer über das klassische Bereitstellungsmodell bereitgestellt wird. Weitere Informationen zu den Azure-Bereitstellungsmodellen finden Sie im Artikel zum Thema [Understand deployment models (Bereitstellungsmodelle verstehen)](../resource-manager-deployment-model.md).

[!INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name="template-description"></a>Beschreibung der Vorlage

Das Bereitstellen einer Vorlage ermöglicht es Ihnen, schnell und konsistent Azure-Ressourcen mit unterschiedlichen Konfigurationswerten zu erstellen. Wenn Sie nicht mit Azure Resource Manager-Vorlagen vertraut sind, sollten Sie den Artikel [Resource Manager-Vorlage – Exemplarische Vorgehensweise](../azure-resource-manager/resource-manager-template-walkthrough.md) lesen. In diesem Artikel wird die Vorlage [Deploy a VM with multiple IPs](https://azure.microsoft.com/resources/templates/101-vm-multiple-ipconfig) verwendet.

<a name="resources"></a>Durch Bereitstellen der Vorlage werden die folgenden Ressourcen erstellt:

|Ressource|Name|Beschreibung|
|---|---|---|
|Netzwerkschnittstelle|*myNic1*|Die drei IP-Konfigurationen, die im Szenarioabschnitt dieses Artikels beschrieben sind, werden erstellt und dieser Netzwerkkarte (Network Interface Card, NIC) zugeordnet.|
|Öffentliche IP-Adressressource|Es werden&2; erstellt: *myPublicIP* und *myPublicIP2*|Diesen Ressourcen sind statische öffentliche IP-Adressen zugewiesen, und sie sind den IP-Konfigurationen *IPConfig-1* und *IPConfig-2* zugewiesen, die im Szenario beschrieben sind.|
|VM|*myVM1*|Ein standardmäßiger virtueller DS3-Computer.|
|Virtuelles Netzwerk|*myVNet1*|Ein virtuelles Netzwerk mit einem Subnetz namens *mySubnet*.|
|Speicherkonto|Einzig für die Bereitstellung|Ein Speicherkonto.|

<a name="parameters"></a>Wenn Sie die Vorlage bereitstellen, müssen Sie Werte für die folgenden Parameter angeben:

|Name|Beschreibung|
|---|---|
|adminUsername|Benutzername des Administrators. Der Benutzername muss den [Azure-Anforderungen für Benutzernamen](../virtual-machines/virtual-machines-windows-faq.md) entsprechen.|
|adminPassword|Kennwort des Administrators. Das Kennwort muss den [Azure-Anforderungen für Kennwörter](../virtual-machines/virtual-machines-windows-faq.md#what-are-the-password-requirements-when-creating-a-vm) entsprechen.|
|dnsLabelPrefix|DNS-Name für PublicIPAddressName1. Der DNS-Name wird zu einer der öffentlichen IP-Adressen aufgelöst, die dem virtuellen Computer zugewiesen sind. Der Name muss innerhalb der Azure-Region (Standort), in der Sie den virtuellen Computer erstellen, eindeutig sein.|
|dnsLabelPrefix1|DNS-Name für PublicIPAddressName2. Der DNS-Name wird zu einer der öffentlichen IP-Adressen aufgelöst, die dem virtuellen Computer zugewiesen sind. Der Name muss innerhalb der Azure-Region (Standort), in der Sie den virtuellen Computer erstellen, eindeutig sein.|
|OSVersion|Die Windows/Linux-Version für den virtuellen Computer. Das Betriebssystem ist ein vollständig gepatchtes Image der Windows/Linux-Version, die ausgewählt ist.|
|imagePublisher|Der Windows/Linux-Image-Herausgeber (Publisher) für den ausgewählten virtuellen Computer.|
|imageOffer|Das Windows/Linux-Image für den ausgewählten virtuellen Computer.|

Jede der Ressourcen, die über die Vorlage bereitgestellt werden, wird mit mehreren Standardeinstellungen konfiguriert. Sie können diese Einstellungen mit den beiden folgenden Methoden anzeigen:

- **Anzeigen der Vorlage auf GitHub:** Wenn Sie mit Vorlagen vertraut sind, können Sie sich die Einstellungen in der [Vorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json) ansehen. 
- **Anzeigen der Einstellungen nach der Bereitstellung:** Wenn Sie nicht mit Vorlagen vertraut sind, können Sie die Vorlage bereitstellen, indem Sie die Schritte in einem der folgenden Abschnitte ausführen, und sich dann die Einstellungen nach der Bereitstellung ansehen.

Sie können das Azure-Portal, PowerShell oder die Azure-Befehlszeilenschnittstelle (CLI) verwenden, um die Vorlage bereitzustellen. Alle Methoden führen zu selben Ergebnis. Um die Vorlage bereitzustellen, führen Sie die Schritte in einem der folgenden Abschnitte aus:

## <a name="deploy-using-the-azure-portal"></a>Bereitstellen über das Azure-Portal

Wenn Sie die Vorlage über das Azure-Portal bereitstellen möchten, führen Sie die folgenden Schritte aus:

1. Registrieren Sie sich für die Vorschau, indem Sie in PowerShell die folgenden Befehle ausführen, nachdem Sie sich angemeldet und das entsprechende Abonnement ausgewählt haben:
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

2. Ändern Sie ggf. die Vorlage. Über die Vorlage werden die Ressourcen und Einstellungen bereitgestellt, die im [Ressourcen](#resources)-Abschnitt dieses Artikels aufgeführt sind. Weitere Informationen zu Vorlagen sowie zum Schreiben von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).
3. Stellen Sie die Vorlage mit einer der folgenden Methoden bereit:
    - **Auswählen der Vorlage im Portal:** Führen die Schritte im Artikel [Bereitstellen von Ressourcen mithilfe einer benutzerdefinierten Vorlage](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template) aus. Wählen Sie die bereits vorhandene Vorlage *101-vm-multiple-ipconfig* aus.
    - **Direkt:** Klicken Sie auf die folgende Schaltfläche, um die Vorlage direkt im Portal zu öffnen: <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-vm-multiple-ipconfig%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

Unabhängig von der Methode, für die Sie sich entscheiden, müssen Sie Werte für die [Parameter](#parameters) angeben, die in diesem Artikel weiter oben aufgeführt sind. Nachdem der virtuelle Computer bereitgestellt ist, stellen Sie eine Verbindung mit dem virtuellen Computer her, und fügen Sie dem von Ihnen bereitgestellten Betriebssystem die privaten IP-Adressen hinzu. Führen Sie dazu die Schritte im Abschnitt [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](#os-config) in diesem Artikel aus. Fügen Sie dem Betriebssystem nicht die öffentlichen IP-Adressen hinzu.

## <a name="deploy-using-powershell"></a>Bereitstellen mit PowerShell

Wenn Sie die Vorlage mit PowerShell bereitstellen möchten, führen Sie die folgenden Schritte aus:

1. Registrieren Sie sich für die Vorschau, indem Sie in PowerShell die folgenden Befehle ausführen, nachdem Sie sich angemeldet und das entsprechende Abonnement ausgewählt haben:
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

2. Stellen Sie die Vorlage bereit, indem Sie die Schritte im Artikel [Bereitstellen einer Vorlage mit PowerShell](../azure-resource-manager/resource-group-template-deploy-cli.md#deploy) ausführen. In diesem Artikel sind mehrere Optionen für ein Bereitstellen einer Vorlage beschrieben. Wenn Sie das Bereitstellen über den `-TemplateUri parameter`, ausführen möchten, hat diese Vorlage den URI *https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json*. Möchten Sie das Bereitstellen über den Parameter `-TemplateFile` ausführen, kopieren Sie den Inhalt der [Vorlagendatei](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json) aus GitHub in eine neue Datei auf Ihrem Computer. Ändern Sie ggf. den Inhalt der Vorlage. Über die Vorlage werden die Ressourcen und Einstellungen bereitgestellt, die im [Ressourcen](#resources)-Abschnitt dieses Artikels aufgeführt sind. Weitere Informationen zu Vorlagen sowie zum Schreiben von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).

    Unabhängig von der Art und Weise, wie Sie die Vorlage bereitstellen, müssen Sie Werte für die Parameter angeben, die im [Parameter](#parameters)-Abschnitt dieses Artikels aufgeführt sind. Wenn Sie die Parameter mithilfe einer Parameterdatei bereitstellen möchten, kopieren Sie den Inhalt der [Parameterdatei](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.parameters.json) aus GitHub in eine neue Datei auf Ihrem Computer. Ändern Sie die Werte in der Datei. Verwenden Sie den Namen der Datei, die Sie erstellt haben, als Wert für den Parameter `-TemplateParameterFile`.
    
    Um gültige Werte für die Parameter „OSVersion“, „ImagePublisher“ und „imageOffer“ zu ermitteln, führen Sie die Schritte im Artikel [Navigieren zu und Auswählen von Images virtueller Windows-Computer](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md#powershell) aus.

    >[!TIP]
    >Wenn Sie nicht sicher sind, ob ein „dnslabelprefix“ verfügbar ist, geben Sie den Befehl `Test-AzureRmDnsAvailability -DomainNameLabel <name-you-want-to-use> -Location <location>` ein, um dies festzustellen. Ist das Präfix verfügbar, gibt der Befehl den Wert `True` zurück.

3. Nachdem der virtuelle Computer bereitgestellt ist, stellen Sie eine Verbindung mit dem virtuellen Computer her, und fügen Sie dem von Ihnen bereitgestellten Betriebssystem die privaten IP-Adressen hinzu. Führen Sie dazu die Schritte im Abschnitt [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](#os-config) in diesem Artikel aus. Fügen Sie dem Betriebssystem nicht die öffentlichen IP-Adressen hinzu.

## <a name="deploy-using-the-azure-cli"></a>Bereitstellen über die Azure-Befehlszeilenschnittstelle

Wenn Sie die Vorlage über die Azure-Befehlszeilenschnittstelle 1.0 bereitstellen möchten, führen Sie die folgenden Schritte aus:

1. Registrieren Sie sich für die Vorschau, indem Sie in PowerShell die folgenden Befehle ausführen, nachdem Sie sich angemeldet und das entsprechende Abonnement ausgewählt haben:
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

2. Stellen Sie die Vorlage bereit, indem Sie die Schritte im Artikel [Bereitstellen einer Vorlage mit der Azure-Befehlszeilenschnittstelle](../azure-resource-manager/resource-group-template-deploy-cli.md#deploy) ausführen. In diesem Artikel sind mehrere Optionen für ein Bereitstellen der Vorlage beschrieben. Wenn Sie das Bereitstellen über `--template-uri` (-f) ausführen möchten, hat diese Vorlage den URI *https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json*. Möchten Sie das Bereitstellen über den Parameter `--template-file` (-f) ausführen, kopieren Sie den Inhalt der [Vorlagendatei](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json) aus GitHub in eine neue Datei auf Ihrem Computer. Ändern Sie ggf. den Inhalt der Vorlage. Über die Vorlage werden die Ressourcen und Einstellungen bereitgestellt, die im [Ressourcen](#resources)-Abschnitt dieses Artikels aufgeführt sind. Weitere Informationen zu Vorlagen sowie zum Schreiben von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).

    Unabhängig von der Art und Weise, wie Sie die Vorlage bereitstellen, müssen Sie Werte für die Parameter angeben, die im [Parameter](#parameters)-Abschnitt dieses Artikels aufgeführt sind. Wenn Sie die Parameter mithilfe einer Parameterdatei bereitstellen möchten, kopieren Sie den Inhalt der [Parameterdatei](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.parameters.json) aus GitHub in eine neue Datei auf Ihrem Computer. Ändern Sie die Werte in der Datei. Verwenden Sie den Namen der Datei, die Sie erstellt haben, als Wert für den Parameter `--parameters-file` (-e).
    
    Um gültige Werte für die Parameter „OSVersion“, „ImagePublisher“ und „imageOffer“ zu ermitteln, führen Sie die Schritte im Artikel [Navigieren zu und Auswählen von Images virtueller Windows-Computer](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md#azure-cli-10) aus.

3. Nachdem der virtuelle Computer bereitgestellt ist, stellen Sie eine Verbindung mit dem virtuellen Computer her, und fügen Sie dem von Ihnen bereitgestellten Betriebssystem die privaten IP-Adressen hinzu. Führen Sie dazu die Schritte im Abschnitt [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](#os-config) in diesem Artikel aus. Fügen Sie dem Betriebssystem nicht die öffentlichen IP-Adressen hinzu.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]



<!--HONumber=Feb17_HO4-->


