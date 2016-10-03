<properties
   pageTitle="Erstellen eines Anwendungsgateways mithilfe der Azure-CLI in Resource Manager | Microsoft Azure"
   description="Erfahren Sie, wie Sie ein Anwendungsgateway mit der Azure-CLI in Resource Manager erstellen."
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace" />

# Erstellen eines Anwendungsgateways mithilfe der Azure-CLI

Für das Azure Application Gateway wird ein Load Balancer auf Schicht 7 (Anwendungsschicht) verwendet. Das Application Gateway ermöglicht ein Failover sowie schnelles Routing von HTTP-Anforderungen zwischen verschiedenen Servern in der Cloud und der lokalen Umgebung. Application Gateway bietet folgende Funktionen für die Anwendungsbereitstellung: HTTP-Lastenausgleich, cookiebasierte Sitzungsaffinität, SSL-Auslagerung (Secure Sockets Layer), benutzerdefinierte Integritätstests und Unterstützung für mehrere Websites.

> [AZURE.SELECTOR]
- [Azure-Portal](application-gateway-create-gateway-portal.md)
- [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
- [Klassische Azure PowerShell](application-gateway-create-gateway.md)
- [Azure Resource Manager-Vorlage](application-gateway-create-gateway-arm-template.md)
- [Azure-Befehlszeilenschnittstelle](application-gateway-create-gateway-cli.md)

## Voraussetzung: Installieren der Azure-Befehlszeilenschnittstelle

Um die Schritte in diesem Artikel ausführen zu können, müssen Sie [die Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows (Azure CLI) installieren](../xplat-cli-install.md) und sich [bei Azure anmelden](../xplat-cli-connect.md).

> [AZURE.NOTE] Wenn Sie über kein Azure-Konto verfügen, müssen Sie eines erstellen. Sie können sich [hier](../active-directory/sign-up-organization.md) für eine kostenlose Testversion registrieren.

## Szenario

In diesem Szenario erfahren Sie, wie Sie ein Anwendungsgateway über das Azure-Portal erstellen.

Dieses Szenario umfasst Folgendes:

- Sie erstellen ein Anwendungsgateway für mittlere Nutzungsdauer mit zwei Instanzen.
- Sie erstellen ein virtuelles Netzwerk mit dem Namen AdatumAppGatewayVNET und dem reservierten CIDR-Block 10.0.0.0/16.
- Sie erstellen ein Subnetz mit dem Namen Appgatewaysubnet, für das 10.0.0.0/28 als CIDR-Block verwendet wird.
- Sie konfigurieren ein Zertifikat für die SSL-Auslagerung.

![Beispielszenario][scenario]

>[AZURE.NOTE] Zusätzliche Konfigurationsschritte für das Anwendungsgateway (u.a. benutzerdefinierte Integritätstests, Back-End-Pool-Adressen und zusätzlichen Regeln) werden nicht während der Erstbereitstellung, sondern nach der Konfiguration des Anwendungsgateways ausgeführt.

## Voraussetzungen

Für Azure Application Gateway ist ein eigenes Subnetz erforderlich. Stellen Sie beim Erstellen eines virtuellen Netzwerks sicher, dass der Adressbereich für mehrere Subnetze ausreicht. Sobald Sie ein Anwendungsgateway in einem Subnetz bereitstellen, können nur zusätzliche Anwendungsgateways zum Subnetz hinzugefügt werden.

## Anmelden an Azure

Öffnen Sie die **Microsoft Azure-Eingabeaufforderung**, und melden Sie sich an.

    azure login

Wenn Sie das vorherige Beispiel eingeben, erhalten Sie einen Code. Navigieren Sie in einem Browser zu https://aka.ms/devicelogin, und setzen Sie den Anmeldeprozess fort.

![CMD mit Geräteanmeldung][1]

Geben Sie im Browser den Code ein, den Sie erhalten haben. Sie werden zu einer Anmeldeseite umgeleitet.

![Browser zur Eingabe des Codes][2]

Wenn Sie den Code eingegeben haben, sind Sie angemeldet. Schließen Sie dann den Browser, um mit dem Szenario fortzufahren.

![Erfolgreich angemeldet][3]

## Wechseln in den Ressource Manager-Modus

    azure config mode arm

## Ressourcengruppe erstellen

Vor dem Erstellen des Anwendungsgateways wird eine Ressourcengruppe erstellt, die das Anwendungsgateway enthält. Hier sehen Sie den Befehl:

    azure group create -n AdatumAppGatewayRG -l eastus

## Erstellen eines virtuellen Netzwerks

Sobald die Ressourcengruppe erstellt wurde, wird ein virtuelles Netzwerk für das Anwendungsgateway erstellt. Im folgenden Beispiel lautet der Adressraum 10.0.0.0/16, wie in den obigen Szenariohinweisen angegeben.

    azure network vnet create -n AdatumAppGatewayVNET -a 10.0.0.0/16 -g AdatumAppGatewayRG -l eastus

## Erstellen eines Subnetzes

Nach der Erstellung des virtuellen Netzwerks wird ein Subnetz für das Anwendungsgateway hinzugefügt. Wenn Sie ein Anwendungsgateway mit einer Web-App verwenden, die im selben virtuellen Netzwerk wie das Anwendungsgateway gehostet wird, lassen Sie ausreichend Platz für ein weiteres Subnetz.

    azure network vnet subnet create -g AdatumAppGatewayRG -n Appgatewaysubnet -v AdatumAppGatewayVNET -a 10.0.0.0/28 

## Erstellen des Anwendungsgateways

Sobald das virtuelle Netzwerk und das Subnetz erstellt wurden, sind die Voraussetzungen für das Anwendungsgateway erfüllt. Für den folgenden Schritt sind darüber hinaus ein zuvor exportiertes PFX-Zertifikat und das Kennwort für das Zertifikat erforderlich: Bei den für das Back-End verwendeten IP-Adressen handelt es sich um die IP-Adressen für Ihren Back-End-Server. Diese Werte können entweder private IPs im virtuellen Netzwerk, öffentliche IPs oder vollqualifizierte Domänennamen für Ihre Back-End-Server sein.

    azure network application-gateway create -n AdatumAppGateway -l eastus -g AdatumAppGatewayRG -e AdatumAppGatewayVNET -m Appgatewaysubnet -r 134.170.185.46,134.170.188.221,134.170.185.50 -y c:\AdatumAppGateway\adatumcert.pfx -x P@ssw0rd -z 2 -a Standard_Medium -w Basic -j 443 -f Enabled -o 80 -i http -b https -u Standard



Mit diesem Beispiel wird ein einfaches Anwendungsgateway mit Standardeinstellungen für Listener, Back-End-Pool, Back-End-HTTP-Einstellungen und Regeln erstellt. Darüber hinaus wird die SSL-Auslagerung konfiguriert. Nach der erfolgreichen Bereitstellung können Sie diese Einstellungen an Ihre Anforderungen anpassen. Wenn Sie Ihre Webanwendung bereits mit dem Back-End-Pool in den vorherigen Schritten definiert haben, beginnt nach dem Erstellen der Lastenausgleich.

## Nächste Schritte

Unter [Erstellen eines benutzerdefinierten Integritätstests](application-gateway-create-probe-portal.md) erfahren Sie, wie benutzerdefinierte Integritätstests erstellt werden.

Unter [Konfigurieren der SSL-Auslagerung](application-gateway-ssl-arm.md) wird beschrieben, wie Sie die SSL-Auslagerung konfigurieren, damit Ihre Webserver nicht länger eine kostenintensive SSL-Entschlüsselung durchführen müssen.

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png

<!---HONumber=AcomDC_0921_2016-->