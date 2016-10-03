<properties
   pageTitle="Erstellen eines Anwendungsgateways über das Portal | Microsoft Azure"
   description="Erfahren Sie, wie Sie ein Anwendungsgateway über das Portal erstellen"
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

# Erstellen eines Anwendungsgateways über das Portal

Azure Application Gateway verwendet einen Load Balancer auf der Schicht 7 (Anwendungsschicht). Das Application Gateway ermöglicht ein Failover sowie schnelles Routing von HTTP-Anforderungen zwischen verschiedenen Servern in der Cloud und der lokalen Umgebung. Application Gateway bietet zahlreiche Application Delivery Controller-Funktionen (ADC), u.a. HTTP-Lastenausgleich, cookiebasierte Sitzungsaffinität, SSL-Auslagerung (Secure Sockets Layer), benutzerdefinierte Integritätstests und Unterstützung für mehrere Websites. Eine vollständige Liste der unterstützten Features finden Sie unter [Übersicht über Application Gateway](application-gateway-introduction.md).

> [AZURE.SELECTOR]
- [Azure-Portal](application-gateway-create-gateway-portal.md)
- [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
- [Klassische Azure PowerShell](application-gateway-create-gateway.md)
- [Azure Resource Manager-Vorlage](application-gateway-create-gateway-arm-template.md)
- [Azure-Befehlszeilenschnittstelle](application-gateway-create-gateway-cli.md)

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

## Erstellen des Anwendungsgateways

### Schritt 1

Navigieren Sie zum Azure-Portal, klicken Sie auf **Neu** > **Netzwerk** > **Application Gateway**.

![Erstellen eines Anwendungsgateways][1]

### Schritt 2

Geben Sie anschließend die allgemeinen Informationen zum Anwendungsgateway an. Wenn Sie fertig sind, klicken Sie auf **OK**.

Folgende Informationen werden für die Grundeinstellungen benötigt:

- **Name**: der Name für das Anwendungsgateway.
- **SKU-Größe:** Diese Einstellung gibt die Größe des Anwendungsgateways an. Sie haben die Wahl zwischen drei Größen: klein, mittel und groß.
- **Anzahl von Instanzen**: die Anzahl von Instanzen. Dieser Wert sollte zwischen 2 und 10 liegen.
- **Ressourcengruppe:** die Ressourcengruppe, zu der das Anwendungsgateway gehört. Hierbei kann es sich um eine vorhandene oder um eine neue Ressourcengruppe handeln.
- **Standort:** Die Region für das Anwendungsgateway. Dieser Standort ist mit dem Standort der Ressourcengruppe identisch. *Der Standort ist wichtig, da das virtuelle Netzwerk und die öffentliche IP den gleichen Standort wie das Gateway aufweisen müssen.*

![Blatt mit Grundeinstellungen][2]

>[AZURE.NOTE] Zu Testzwecken kann für die Anzahl von Instanzen der Wert 1 ausgewählt werden. Beachten Sie, dass eine Anzahl von weniger als zwei Instanzen nicht durch die SLA abgedeckt ist und daher nicht empfohlen wird. Gateways mit niedriger Nutzungsdauer sind für Dev/Test-Umgebungen vorgesehen und sollten nicht zu Produktionszwecken verwendet werden.

### Schritt 3

Nachdem die Grundeinstellungen definiert wurden, muss im nächsten Schritt das zu verwendende virtuelle Netzwerk definiert werden. In diesem virtuellen Netzwerk befindet sich die Anwendung, für die das Anwendungsgateway den Lastenausgleich durchführt.

Klicken Sie auf **Virtuelles Netzwerk auswählen**, um das virtuelle Netzwerk zu konfigurieren.

![Blatt mit Einstellungen für das Anwendungsgateway][3]

### Schritt 4

Klicken Sie auf dem Blatt *Virtuelles Netzwerk auswählen* auf **Neu erstellen**.

An dieser Stelle kann ein vorhandenes virtuelles Netzwerk ausgewählt werden. Auf diese Vorgehensweise wird in diesem Szenario jedoch nicht näher eingegangen. Bei Verwendung eines vorhandenen virtuellen Netzwerks ist es wichtig zu wissen, dass das virtuelle Netzwerk ein leeres Subnetz oder ein Subnetz nur mit Anwendungsgatewayressourcen benötigt, damit es verwendet werden kann.

![Blatt „Virtuelles Netzwerk auswählen“][4]

### Schritt 5

Füllen Sie die Netzwerkinformationen auf dem Blatt **Virtuelles Netzwerk erstellen** wie in der obigen [Szenariobeschreibung](#scenario) erläutert aus.

![Blatt „Virtuelles Netzwerk erstellen“ mit eingegebenen Informationen][5]

### Schritt 6

Nachdem das virtuelle Netzwerk erstellt wurde, wird im nächsten Schritt die Front-End-IP für das Anwendungsgateway definiert. An dieser Stelle können Sie zwischen einer öffentlichen und einer privaten IP-Adresse für das Front-End wählen. Die Auswahl ist davon abhängig, ob die Anwendung mit dem Internet verbunden ist oder nur intern verwendet wird. Bei diesem Szenario wird von einer öffentlichen IP-Adresse ausgegangen. Klicken Sie auf die Schaltfläche **Privat**, um eine private IP-Adresse auszuwählen. Es wird entweder eine automatisch zugewiesene IP-Adresse ausgewählt, oder Sie können das Kontrollkästchen **Bestimmte private IP-Adresse auswählen** aktivieren, um manuell eine IP-Adresse einzugeben.

### Schritt 7

Klicken Sie auf **Öffentliche IP-Adresse auswählen**. Wenn eine vorhandene öffentliche IP-Adresse verfügbar ist, kann sie an dieser Stelle ausgewählt werden. In diesem Szenario erstellen Sie eine neue öffentliche IP-Adresse. Klicken Sie auf **Neu erstellen**.

![Blatt „Öffentliche IP-Adresse auswählen“][6]

### Schritt 8

Als Nächstes wählen Sie einen Anzeigenamen für die öffentliche IP-Adresse und klicken auf **OK**.

![Blatt „Öffentliche IP-Adresse erstellen“][7]

### Schritt 9

Die letzte Einstellung, die beim Erstellen eines Anwendungsgateways konfiguriert werden muss, ist die Listenerkonfiguration. Wenn **HTTP** verwendet wird, ist die Konfiguration vollständig, und Sie können auf **OK** klicken. Wenn **HTTPS** verwendet werden soll, sind weitere Konfigurationsschritte erforderlich.

Für die Verwendung von **HTTPS** ist ein Zertifikat erforderlich. Da der private Schlüssel des Zertifikats benötigt wird, müssen ein PFX-Export des Zertifikats und das Dateikennwort bereitgestellt werden.


![Leerer Abschnitt „Listenerkonfiguration“ auf dem Blatt „Einstellungen“][8]

### Schritt 10

Klicken Sie auf **HTTPS** und dann auf das **Ordnersymbol** neben dem Textfeld **PFX-Zertifikat hochladen**. Navigieren Sie zur PFX-Zertifikatdatei in Ihrem Dateisystem. Wählen Sie das Zertifikat aus, legen Sie einen Anzeigenamen fest, und geben Sie das Kennwort für die PFX-Datei ein.

Klicken Sie anschließend auf **OK**, um die Einstellungen für das Anwendungsgateway zu überprüfen.

![Abschnitt „Listenerkonfiguration“ auf dem Blatt „Einstellungen“][9]

### Schritt 11

Überprüfen Sie die Zusammenfassungsseite, und klicken Sie auf **OK**. Das Anwendungsgateway wird in der Warteschlange platziert und erstellt.

### Schritt 12

Nachdem das Anwendungsgateway erstellt wurde, navigieren Sie im Portal zu diesem Gateway, um die Konfiguration fortzusetzen.

![Application Gateway – Ressourcenansicht][10]

Mit diesen Schritten wird ein einfaches Anwendungsgateway mit Standardeinstellungen für Listener, Back-End-Pool, Back-End-HTTP-Einstellungen und Regeln erstellt. Nach der erfolgreichen Bereitstellung können Sie diese Einstallungen an Ihre Anforderungen anpassen.

## Nächste Schritte

Unter [Erstellen eines benutzerdefinierten Integritätstests](application-gateway-create-probe-portal.md) erfahren Sie, wie benutzerdefinierte Integritätstests erstellt werden.

Unter [Konfigurieren der SSL-Auslagerung](application-gateway-ssl-portal.md) wird beschrieben, wie Sie die SSL-Auslagerung konfigurieren, damit Ihre Webserver nicht länger eine kostenintensive SSL-Entschlüsselung durchführen müssen.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[6]: ./media/application-gateway-create-gateway-portal/figure6.png
[7]: ./media/application-gateway-create-gateway-portal/figure7.png
[8]: ./media/application-gateway-create-gateway-portal/figure8.png
[9]: ./media/application-gateway-create-gateway-portal/figure9.png
[10]: ./media/application-gateway-create-gateway-portal/figure10.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png

<!---HONumber=AcomDC_0921_2016-->