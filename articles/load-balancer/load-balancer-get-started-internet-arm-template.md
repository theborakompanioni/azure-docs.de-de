---
title: Erstellen eines Load Balancers mit Internetzugriff im Ressourcen-Manager unter Verwendung einer Vorlage | Microsoft Docs
description: Erfahren Sie, wie Sie einen Load Balancer mit Internetzugriff im Resource Manager mithilfe einer Vorlage erstellen.
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: ''
tags: azure-resource-manager

ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2016
ms.author: sewhee

---
# Erstellen eines Load Balancers mit Internetzugriff mithilfe einer Vorlage
[!INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dieser Artikel gilt für das Ressourcen-Manager-Bereitstellungsmodell. Sie können auch erfahren, wie Sie [mithilfe des klassischen Bereitstellungsmodells einen Load Balancer mit Internetzugriff erstellen](load-balancer-get-started-internet-classic-portal.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## Bereitstellen der Vorlage per Klick
In der verfügbaren Beispielvorlage im öffentlichen Repository wird eine Parameterdatei verwendet, die die Standardwerte zum Generieren des oben beschriebenen Szenarios enthält. Öffnen Sie zum Bereitstellen dieser Vorlage per Klick [diesen Link](http://go.microsoft.com/fwlink/?LinkId=544801), klicken Sie auf **Deploy to Azure**, ersetzen Sie ggf. die Werte der Standardparameter, und führen Sie die Schritte im Portal aus.

## Bereitstellen der Vorlage mithilfe von PowerShell
Führen Sie zum Bereitstellen der mithilfe von PowerShell heruntergeladenen Vorlage die unten beschriebenen Schritte aus.

1. Wenn Sie Azure PowerShell zuvor noch nicht verwendet haben, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md), und befolgen Sie die komplette Anleitung, um sich bei Azure anzumelden und Ihr Abonnement auszuwählen.
2. Führen Sie das Cmdlet **New-AzureRmResourceGroupDeployment** aus, um eine Ressourcengruppe mit der Vorlage zu erstellen.
   
        New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'

## Bereitstellen der Vorlage mithilfe der Azure-Befehlszeilenschnittstelle
Führen Sie zum Bereitstellen der Vorlage mithilfe der Azure-Befehlszeilenschnittstelle die unten beschriebenen Schritte aus.

1. Wenn Sie die Azure-Befehlszeilenschnittstelle noch nie verwendet haben, ziehen Sie [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) zurate, und folgen Sie den Anweisungen bis zu dem Punkt, an dem Sie Ihr Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie den Befehl **azure config mode** aus, um in den Ressourcen-Manager-Modus zu wechseln, wie unten dargestellt.
   
        azure config mode arm
   
    Hier ist die erwartete Ausgabe des obigen Befehls:
   
        info:    New mode is arm
3. Navigieren Sie im Browser zu **https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json**, kopieren Sie den Inhalt der JSON-Datei, und fügen Sie ihn in eine neue Datei auf Ihrem Computer ein. Für dieses Szenario kopieren Sie die unten angegebenen Werte in eine Datei mit dem Namen **c:\\lb\\azuredeploy.parameters.json**.
4. Führen Sie das Cmdlet **azure group deployment create** aus, um den neuen Load Balancer mithilfe der heruntergeladenen und geänderten Vorlage und Parameterdateien bereitzustellen. In der nach der Ausgabe angezeigten Liste werden die verwendeten Parameter erläutert.
   
        azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' -e 'c:\lb\azuredeploy.parameters.json'

## Nächste Schritte
[Erste Schritte zum Konfigurieren des internen Lastenausgleichs](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurieren eines Lastenausgleichs-Verteilungsmodus](load-balancer-distribution-mode.md)

[Konfigurieren von TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md)

<!---HONumber=AcomDC_0928_2016-->