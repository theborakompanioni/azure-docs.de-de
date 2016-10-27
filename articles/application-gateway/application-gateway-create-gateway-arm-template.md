
<properties
   pageTitle="Erstellen eines Anwendungsgateways mit Azure-Ressourcen-Manager-Vorlagen | Microsoft Azure"
   description="Diese Seite enthält Anweisungen zum Erstellen eines Azure-Anwendungsgateways mit einer Azure Resource Manager-Vorlage."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="gwallace"/>



# <a name="create-an-application-gateway-by-using-the-azure-resource-manager-template"></a>Erstellen eines Anwendungsgateways mit der Azure-Ressourcen-Manager-Vorlage

Für das Azure Application Gateway wird ein Load Balancer auf Schicht 7 (Anwendungsschicht) verwendet. Das Anwendungsgateway ermöglicht ein Failover sowie schnelles Routing von HTTP-Anforderungen zwischen verschiedenen Servern in der Cloud und der lokalen Umgebung. Anwendungsgateways weisen folgende Anwendungsbereitstellungsfunktionen auf: HTTP-Lastenausgleich, cookiebasierte Sitzungsaffinität und Secure Sockets Layer-Auslagerung (SSL).

> [AZURE.SELECTOR]
- [Azure-Portal](application-gateway-create-gateway-portal.md)
- [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
- [Klassische Azure PowerShell](application-gateway-create-gateway.md)
- [Azure Resource Manager-Vorlage](application-gateway-create-gateway-arm-template.md)
- [Azure-Befehlszeilenschnittstelle](application-gateway-create-gateway-cli.md)

Sie erfahren, wie Sie eine vorhandene Azure Resource Manager-Vorlage von GitHub herunterladen und ändern und wie Sie die Vorlage über GitHub, PowerShell und die Azure-Befehlszeilenschnittstelle bereitstellen.

Wenn Sie die Azure-Ressourcen-Manager-Vorlage ohne Änderungen einfach direkt aus GitHub bereitstellen möchten, sollten Sie den Abschnitt zum Bereitstellen einer Vorlage aus GitHub lesen.

## <a name="scenario"></a>Szenario

In diesem Szenario führen Sie Folgendes durch:

- Sie erstellen ein Anwendungsgateway mit zwei Instanzen.
- Sie erstellen ein virtuelles Netzwerk mit dem Namen VirtualNetwork1 und dem reservierten CIDR-Block 10.0.0.0/16.
- Sie erstellen ein Subnetz mit dem Namen Appgatewaysubnet, für das 10.0.0.0/28 als CIDR-Block verwendet wird.
- Sie richten zwei zuvor konfigurierte Back-End-IP-Adressen für die Webserver ein, die zum Durchführen des Lastenausgleichs verwendet werden sollen. In diesem Vorlagenbeispiel werden die Back-End-IP-Adressen 10.0.1.10 und 10.0.1.11 verwendet.

>[AZURE.NOTE] Diese Einstellungen sind die Parameter für diese Vorlage. Zum Anpassen der Vorlage können Sie Regeln, den Listener und das SSL-Element ändern, mit dem „azuredeploy.json“ geöffnet wird.

![Szenario](./media/application-gateway-create-gateway-arm-template/scenario-arm.png)

## <a name="download-and-understand-the-azure-resource-manager-template"></a>Herunterladen und Verstehen der Azure-Ressourcen-Manager-Vorlage

Sie können die vorhandene Azure-Ressourcen-Manager-Vorlage herunterladen, um ein virtuelles Netzwerk und zwei Subnetze aus GitHub zu erstellen. Anschließend können Sie die gewünschten Änderungen vornehmen und die Vorlage bei Bedarf wiederverwenden. Führen Sie dazu die folgenden Schritte aus:

1. Navigieren Sie zu [Create Application Gateway](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-create)(Erstellen eines Anwendungsgateways).
2. Klicken Sie auf **azuredeploy.json**, und klicken Sie dann auf **RAW**.
3. Speichern Sie die Datei in einem lokalen Ordner auf Ihrem Computer.
4. Fahren Sie mit Schritt 7 fort, wenn Sie mit Azure-Ressourcen-Manager-Vorlagen bereits vertraut sind.
5. Öffnen Sie die Datei, die Sie gespeichert haben, und sehen Sie sich den Inhalt unter **parameters** in Zeile 5 an. Die Parameter der Azure-Ressourcen-Manager-Vorlage stellen Platzhalter für Werte dar, die während der Bereitstellung ausgefüllt werden können.

  	| Parameter | Beschreibung |
  	|---|---|
  	| **location** | Azure-Region, in der das Anwendungsgateway erstellt wird |
  	| **VirtualNetwork1** | Name für das neue virtuelle Netzwerk |
  	| **addressPrefix** | Adressraum für das virtuelle Netzwerk im CIDR-Format |
  	| **ApplicationGatewaysubnet** | Name für das Anwendungsgateway-Subnetz |
  	| **subnetPrefix** | CIDR-Block für das Anwendungsgateway-Subnetz |
  	| **skuname** | Größe der SKU-Instanz |
  	| **Kapazität** | Anzahl von Instanzen |
  	| **backendaddress1** | IP-Adresse des ersten Webservers |
  	| **backendaddress2** | IP-Adresse des zweiten Webservers |


    >[AZURE.IMPORTANT] Azure-Ressourcen-Manager-Vorlagen, die in GitHub verwaltet werden, können sich im Laufe der Zeit ändern. Überprüfen Sie die Vorlage stets, bevor Sie sie verwenden.

6. Überprüfen Sie den Inhalt unter **resources** , und beachten Sie Folgendes:

    - **type**. Typ der Ressource, die von der Vorlage erstellt wird. In diesem Fall lautet der Typ **Microsoft.Network/applicationGateways**, der für ein Anwendungsgateway steht.
    - **name**. Name der Ressource. Beachten Sie die Verwendung von **[parameters('applicationGatewayName')]**. Dies bedeutet, dass der Name während der Bereitstellung durch Sie oder eine Parameterdatei eingegeben wird.
    - **properties**. Liste der Eigenschaften für die Ressource. Diese Vorlage verwendet während der Erstellung des Anwendungsgateways das virtuelle Netzwerk und die öffentliche IP-Adresse.

7. Navigieren Sie zurück zu [https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-create/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-create).
8. Klicken Sie auf **azuredeploy-paremeters.json** und anschließend auf **RAW**.
9. Speichern Sie die Datei in einem lokalen Ordner auf Ihrem Computer.
10. Öffnen Sie die Datei, die Sie gespeichert haben, und bearbeiten Sie die Parameterwerte. Verwenden Sie die folgenden Werte, um das Anwendungsgateway wie in unserem Szenario beschrieben bereitzustellen.

        {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        {
        "location" : {
        "value" : "West US"
        },
        "addressPrefix": {
        "value": "10.0.0.0/16"
        },
        "subnetPrefix": {
        "value": "10.0.0.0/24"
        },
        "skuName": {
        "value": "Standard_Small"
        },
        "capacity": {
        "value": 2
        },
        "backendIpAddress1": {
        "value": "10.0.1.10"
        },
        "backendIpAddress2": {
        "value": "10.0.1.11"
        }
        }

11. Speichern Sie die Datei. Sie können die JSON-Vorlage und die Parametervorlage mithilfe von online verfügbaren JSON-Validierungstools wie [JSlint.com](http://www.jslint.com/)testen.

## <a name="deploy-the-azure-resource-manager-template-by-using-powershell"></a>Bereitstellen der Azure-Ressourcen-Manager-Vorlage mit PowerShell

Wenn Sie Azure PowerShell zuvor noch nicht verwendet haben, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) , und befolgen Sie die Anweisungen, um sich bei Azure anzumelden und Ihr Abonnement auszuwählen.

### <a name="step-1"></a>Schritt 1

    Login-AzureRmAccount

### <a name="step-2"></a>Schritt 2

Überprüfen Sie die Abonnements für das Konto.

    Get-AzureRmSubscription

Sie werden zur Authentifizierung mit Ihren Anmeldeinformationen aufgefordert.<BR>

### <a name="step-3"></a>Schritt 3

Wählen Sie aus, welches Azure-Abonnement Sie verwenden möchten. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Schritt 4


Erstellen Sie bei Bedarf eine Ressourcengruppe mit dem Cmdlet **New-AzureResourceGroup** . Im folgenden Beispiel erstellen Sie eine Ressourcengruppe mit dem Namen AppgatewayRG in der Region „USA, Osten“.

    New-AzureRmResourceGroup -Name AppgatewayRG -Location "East US"

Führen Sie das Cmdlet **New-AzureRmResourceGroupDeployment** aus, um das neue virtuelle Netzwerk mit der zuvor heruntergeladenen und geänderten Vorlage und den Parameterdateien bereitzustellen.

    New-AzureRmResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
        -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json

## <a name="deploy-the-azure-resource-manager-template-by-using-the-azure-cli"></a>Bereitstellen der Azure-Ressourcen-Manager-Vorlage mit der Azure-Befehlszeilenschnittstelle

Führen Sie die folgenden Schritte aus, um die heruntergeladene Azure-Ressourcen-Manager-Vorlage mit der Azure-Befehlszeilenschnittstelle bereitzustellen:

### <a name="step-1"></a>Schritt 1

Wenn Sie die Azure-CLI noch nie verwendet haben, lesen Sie den Artikel [Installieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) , und befolgen Sie die Anweisungen bis zu dem Punkt, an dem Sie Ihr Azure-Konto und Ihr Abonnement auswählen.
### <a name="step-2"></a>Schritt 2

Führen Sie den Befehl **azure config mode** aus, um in den Ressourcen-Manager-Modus zu wechseln, wie unten dargestellt.

    azure config mode arm

Hier ist die erwartete Ausgabe des obigen Befehls:

    info:   New mode is arm

### <a name="step-3"></a>Schritt 3

Führen Sie bei Bedarf den Befehl **azure group create** aus, um wie unten dargestellt eine neue Ressourcengruppe zu erstellen. Beachten Sie die Ausgabe des Befehls. In der nach der Ausgabe angezeigten Liste werden die verwendeten Parameter erläutert. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über Azure Resource Manager](../resource-group-overview.md).

    azure group create -n appgatewayRG -l eastus

**-n (oder --name)**. Name für die neue Ressourcengruppe. In unserem Fall ist dies *appgatewayRG*.

**-l (oder --location)**. Azure-Region, in der die neue Ressourcengruppe erstellt wird. In unserem Fall ist dies *eastus*.

### <a name="step-4"></a>Schritt 4

Führen Sie das Cmdlet **azure group deployment create** aus, um das neue virtuelle Netzwerk mithilfe der oben heruntergeladenen und geänderten Vorlage und Parameterdateien bereitzustellen. In der nach der Ausgabe angezeigten Liste werden die verwendeten Parameter erläutert.

    azure group deployment create -g appgatewayRG -n TestAppgatewayDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

## <a name="deploy-the-azure-resource-manager-template-by-using-click-to-deploy"></a>Bereitstellen der Azure-Ressourcen-Manager-Vorlage per Click-to-Deploy

Click-to-Deploy ist eine weitere Option zum Verwenden von Azure-Ressourcen-Manager-Vorlagen. Diese Art der Bereitstellung bietet eine einfache Möglichkeit, Vorlagen im Azure-Portal zu verwenden.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu [Create an Application Gateway with Public IP](https://azure.microsoft.com/documentation/templates/101-application-gateway-public-ip/)(Erstellen eines Anwendungsgateways mit öffentlicher IP).

### <a name="step-2"></a>Schritt 2

Klicken Sie auf Schaltfläche zum **Bereitstellen in Azure**.

![Bereitstellen in Azure](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)

### <a name="step-3"></a>Schritt 3

Füllen Sie im Portal die Parameter für die Bereitstellungsvorlage aus, und klicken Sie auf **OK**.

![parameters](./media/application-gateway-create-gateway-arm-template/ibiza1.png)

### <a name="step-4"></a>Schritt 4

Wählen Sie **Rechtliche Bedingungen** aus, und klicken Sie auf **Kaufen**.

### <a name="step-5"></a>Schritt 5

Klicken Sie auf dem Blatt „Benutzerdefinierte Bereitstellung“ auf **Erstellen**.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie die SSL-Auslagerung konfigurieren möchten, finden Sie weitere Informationen im Abschnitt [Konfigurieren eines Application Gateways für die SSL-Auslagerung](application-gateway-ssl.md).

Wenn Sie ein Anwendungsgateway für die Verwendung mit einem internen Load Balancer konfigurieren möchten, ist es ratsam, den Abschnitt [Erstellen eines Application Gateways mit einem internen Lastenausgleich (ILB)](application-gateway-ilb.md)zu lesen.

Weitere Informationen zu Lastenausgleichsoptionen im Allgemeinen finden Sie unter:

- [Azure-Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)



<!--HONumber=Oct16_HO2-->


