<properties
   pageTitle="Erstellen eines Anwendungsgateways mit der Web Application Firewall über das Portal | Microsoft Azure"
   description="Informationen zum Erstellen eines Anwendungsgateways mit der Web Application Firewall über das Portal"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="gwallace" />


# <a name="create-an-application-gateway-with-web-application-firewall-by-using-the-portal"></a>Erstellen eines Anwendungsgateways mit der Web Application Firewall über das Portal

> [AZURE.SELECTOR]
- [Azure-Portal](application-gateway-web-application-firewall-portal.md)
- [Azure Resource Manager PowerShell](application-gateway-web-application-firewall-powershell.md)

Das Azure-Anwendungsgateway verwendet einen Load Balancer auf der Schicht 7 (Anwendungsschicht). Das Application Gateway ermöglicht ein Failover sowie schnelles Routing von HTTP-Anforderungen zwischen verschiedenen Servern in der Cloud und der lokalen Umgebung.
Application Gateway bietet zahlreiche Application Delivery Controller-Funktionen (ADC), u.a. HTTP-Lastenausgleich, cookiebasierte Sitzungsaffinität, SSL-Auslagerung (Secure Sockets Layer), benutzerdefinierte Integritätstests und Unterstützung für mehrere Websites.
Eine vollständige Liste der unterstützten Features finden Sie unter [Übersicht über Application Gateway](application-gateway-introduction.md).

Die Web Application Firewall (WAF) in Azure Application Gateway schützt Webanwendungen vor gängigen webbasierten Angriffen wie der Einschleusung von SQL-Code, Angriffen durch websiteübergreifende Skripts und der Übernahme von Sitzungen.

## <a name="scenarios"></a>Szenarios

In diesem Artikel gibt es zwei Szenarios:

Im ersten Szenario erlernen Sie das [Hinzufügen der Web Application Firewall zu einem vorhandenen Anwendungsgateway](#add-web-application-firewall-to-an-existing-application-gateway).

Im zweiten Szenario erlernen Sie das [Erstellen eines Anwendungsgateways mit der Web Application Firewall](#create-an-application-gateway-with-web-application-firewall)

![Beispielszenario][scenario]

>[AZURE.NOTE] Zusätzliche Konfigurationsschritte für das Anwendungsgateway (u.a. benutzerdefinierte Integritätstests, Back-End-Pool-Adressen und zusätzlichen Regeln) werden nicht während der Erstbereitstellung, sondern nach der Konfiguration des Anwendungsgateways ausgeführt.

## <a name="before-you-begin"></a>Voraussetzungen

Für Azure Application Gateway ist ein eigenes Subnetz erforderlich. Stellen Sie beim Erstellen eines virtuellen Netzwerks sicher, dass der Adressbereich für mehrere Subnetze ausreicht. Sobald Sie ein Anwendungsgateway in einem Subnetz bereitstellen, können nur zusätzliche Anwendungsgateways zum Subnetz hinzugefügt werden.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Hinzufügen der Web Application Firewall zu einem vorhandenen Anwendungsgateway

In diesem Szenario wird ein vorhandenes Anwendungsgateway aktualisiert, sodass es die Web Application Firewall im Schutzmodus unterstützt.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zum Azure-Portal, und wählen Sie ein vorhandenes Anwendungsgateway aus.

![Erstellen eines Anwendungsgateways][1]

### <a name="step-2"></a>Schritt 2

Klicken Sie auf **Konfiguration** , und aktualisieren Sie die Einstellungen des Anwendungsgateways. Wenn Sie fertig sind, klicken Sie auf **Speichern**

Folgende Einstellungen dienen zum Aktualisieren eines vorhandenen Anwendungsgateways für die Unterstützung der Web Application Firewall:

- **Tarif**: Als Tarif muss **WAF** ausgewählt werden, damit die Web Application Firewall unterstützt wird.
- **SKU-Größe**: Diese Einstellung gibt die Größe des Anwendungsgateways mit der Web Application Firewall an. Sie haben die Wahl zwischen **Mittel** und **Groß**.
- **Firewallstatus**: Diese Einstellung aktiviert bzw. deaktiviert die Web Application Firewall.
- **Firewallmodus**: Diese Einstellung gibt an, wie die Web Application Firewall böswilligen Datenverkehr behandelt. Im **Erkennungsmodus** werden die Ereignisse nur protokolliert. Im **Schutzmodus** werden die Ereignisse protokolliert, und der böswillige Datenverkehr wird beendet.

![Blatt mit Grundeinstellungen][2]

>[AZURE.NOTE] Um die Web Application Firewall-Protokolle anzuzeigen, muss die Diagnose aktiviert und ApplicationGatewayFirewallLog ausgewählt sein. Zu Testzwecken kann für die Anzahl von Instanzen der Wert 1 ausgewählt werden. Beachten Sie, dass eine Anzahl von weniger als zwei Instanzen nicht durch die SLA abgedeckt ist und daher nicht empfohlen wird. Kleine Gateways sind nicht verfügbar, wenn Sie die Web Application Firewall verwenden.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Erstellen eines Anwendungsgateways mit der Web Application Firewall

Dieses Szenario umfasst Folgendes:

- Sie erstellen ein mittelgroßes Anwendungsgateway mit zwei Instanzen mit der Web Application Firewall.
- Sie erstellen ein virtuelles Netzwerk mit dem Namen AdatumAppGatewayVNET und dem reservierten CIDR-Block 10.0.0.0/16.
- Sie erstellen ein Subnetz mit dem Namen Appgatewaysubnet, für das 10.0.0.0/28 als CIDR-Block verwendet wird.
- Sie konfigurieren ein Zertifikat für die SSL-Auslagerung.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zum Azure-Portal, klicken Sie auf **Neu** > **Netzwerk** > ** Application Gateway**.

![Erstellen eines Anwendungsgateways][1-1]

### <a name="step-2"></a>Schritt 2

Geben Sie anschließend die allgemeinen Informationen zum Anwendungsgateway an. Wählen Sie unbedingt **WAF** als Tarif aus. Wenn Sie fertig sind, klicken Sie auf **OK**

Folgende Informationen werden für die Grundeinstellungen benötigt:

- **Name** : der Name für das Anwendungsgateway.
- **Tarif**: Der Tarif des Anwendungsgateways. Verfügbare Optionen sind **Standard** und **WAF**. Die Web Application Firewall ist nur im WAF-Tarif verfügbar.
- **SKU-Größe**: Diese Einstellung gibt die Größe des Anwendungsgateways an. Verfügbare Optionen sind **Mittel** und **Groß**.
- **Anzahl von Instanzen**: Die Anzahl von Instanzen. Dieser Wert muss eine Zahl zwischen **2** und **10** sein.
- **Ressourcengruppe**: die Ressourcengruppe, zu der das Anwendungsgateway gehört. Hierbei kann es sich um eine vorhandene oder um eine neue Ressourcengruppe handeln.
- **Standort**: Die Region für das Anwendungsgateway. Dieser Standort ist mit dem Standort der Ressourcengruppe identisch. *Der Standort ist wichtig, da das virtuelle Netzwerk und die öffentliche IP den gleichen Standort wie das Gateway aufweisen müssen.*

![Blatt mit Grundeinstellungen][2-2]

>[AZURE.NOTE] Zu Testzwecken kann für die Anzahl von Instanzen der Wert 1 ausgewählt werden. Beachten Sie, dass eine Anzahl von weniger als zwei Instanzen nicht durch die SLA abgedeckt ist und daher nicht empfohlen wird. Kleine Gateways werden für Web Application Firewall-Szenarios nicht unterstützt.

### <a name="step-3"></a>Schritt 3

Nachdem die Grundeinstellungen definiert wurden, muss im nächsten Schritt das zu verwendende virtuelle Netzwerk definiert werden. In diesem virtuellen Netzwerk befindet sich die Anwendung, für die das Anwendungsgateway den Lastenausgleich durchführt.

Klicken Sie auf **Virtuelles Netzwerk auswählen** , um das virtuelle Netzwerk zu konfigurieren.

![Blatt mit Einstellungen für das Anwendungsgateway][3]

### <a name="step-4"></a>Schritt 4

Klicken Sie auf dem Blatt **Virtuelles Netzwerk auswählen** auf **Neu erstellen**

An dieser Stelle kann ein vorhandenes virtuelles Netzwerk ausgewählt werden. Auf diese Vorgehensweise wird in diesem Szenario jedoch nicht näher eingegangen.  Bei Verwendung eines vorhandenen virtuellen Netzwerks ist es wichtig zu wissen, dass das virtuelle Netzwerk ein leeres Subnetz oder ein Subnetz nur mit Anwendungsgatewayressourcen benötigt, damit es verwendet werden kann.

![Blatt „Virtuelles Netzwerk auswählen“][4]

### <a name="step-5"></a>Schritt 5

Füllen Sie die Netzwerkinformationen auf dem Blatt **Virtuelles Netzwerk erstellen** wie in der obigen [Szenariobeschreibung](#scenario) erläutert aus.

![Blatt „Virtuelles Netzwerk erstellen“ mit eingegebenen Informationen][5]

### <a name="step-6"></a>Schritt 6

Nachdem das virtuelle Netzwerk erstellt wurde, wird im nächsten Schritt die Front-End-IP für das Anwendungsgateway definiert. An dieser Stelle können Sie zwischen einer öffentlichen und einer privaten IP-Adresse für das Front-End wählen. Die Auswahl ist davon abhängig, ob die Anwendung mit dem Internet verbunden ist oder nur intern verwendet wird. Bei diesem Szenario wird von einer öffentlichen IP-Adresse ausgegangen. Klicken Sie auf die Schaltfläche **Privat** , um eine private IP-Adresse auszuwählen. Es wird entweder eine automatisch zugewiesene IP-Adresse ausgewählt, oder Sie können das Kontrollkästchen **Bestimmte private IP-Adresse auswählen** aktivieren, um manuell eine IP-Adresse einzugeben.

Klicken Sie auf **Öffentliche IP-Adresse auswählen**. Wenn eine vorhandene öffentliche IP-Adresse verfügbar ist, kann sie an dieser Stelle ausgewählt werden. In diesem Szenario erstellen Sie eine neue öffentliche IP-Adresse. Klicken Sie auf **Create new**

![Blatt „Öffentliche IP-Adresse auswählen“][6]

### <a name="step-7"></a>Schritt 7

Als Nächstes wählen Sie einen Anzeigenamen für die öffentliche IP-Adresse und klicken auf **OK**

![Blatt „Öffentliche IP-Adresse erstellen“][7]

### <a name="step-8"></a>Schritt 8

Als Nächstes richten Sie die Listenerkonfiguration ein.  Wenn **HTTP** verwendet wird, ist die Konfiguration vollständig, und Sie können auf **OK** klicken. Wenn **HTTPS** verwendet werden soll, sind weitere Konfigurationsschritte erforderlich.

Für die Verwendung von **HTTPS**ist ein Zertifikat erforderlich. Da der private Schlüssel des Zertifikats benötigt wird, müssen ein PFX-Export des Zertifikats und das Dateikennwort bereitgestellt werden.

Klicken Sie auf **HTTPS** und dann auf das **Ordnersymbol** neben dem Textfeld **PFX-Zertifikat hochladen**.
Navigieren Sie zur PFX-Zertifikatdatei in Ihrem Dateisystem. Wählen Sie das Zertifikat aus, legen Sie einen Anzeigenamen fest, und geben Sie das Kennwort für die PFX-Datei ein.

Klicken Sie anschließend auf **OK** , um die Einstellungen für das Anwendungsgateway zu überprüfen.

![Abschnitt „Listenerkonfiguration“ auf dem Blatt „Einstellungen“][8]

### <a name="step-9"></a>Schritt 9

Konfigurieren Sie die **WAF** -spezifischen Einstellungen.

- **Firewallstatus** : Diese Einstellung aktiviert oder deaktiviert die WAF.
- **Firewallmodus** : Diese Einstellung bestimmt die Aktionen, die WAF bei böswilligem Datenverkehr unternimmt. Wenn Sie **Erkennung** auswählen, wird der Datenverkehr nur protokolliert.  Wenn Sie **Schutz** auswählen, wird der Datenverkehr protokolliert und mit einem Meldung 403 (nicht autorisiert) beendet.

![Web Application Firewall-Einstellungen][9]

### <a name="step-10"></a>Schritt 10

Überprüfen Sie die Zusammenfassungsseite, und klicken Sie auf **OK**.  Das Anwendungsgateway wird in der Warteschlange platziert und erstellt.

### <a name="step-12"></a>Schritt 12

Nachdem das Anwendungsgateway erstellt wurde, navigieren Sie im Portal zu diesem Gateway, um die Konfiguration fortzusetzen.

![Application Gateway – Ressourcenansicht][10]

Mit diesen Schritten wird ein einfaches Anwendungsgateway mit Standardeinstellungen für Listener, Back-End-Pool, Back-End-HTTP-Einstellungen und Regeln erstellt. Nach der erfolgreichen Bereitstellung können Sie diese Einstallungen an Ihre Anforderungen anpassen.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren der Diagnoseprotokollierung zum Protokollieren von Ereignissen, die mit der Web Application Firewall erkannt oder verhindert werden, finden Sie unter [Application Gateway-Diagnose](application-gateway-diagnostics.md)

Unter [Erstellen eines benutzerdefinierten Integritätstests](application-gateway-create-probe-portal.md)

Unter [Konfigurieren der SSL-Auslagerung](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[1-1]: ./media/application-gateway-web-application-firewall-portal/figure1-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[4]: ./media/application-gateway-web-application-firewall-portal/figure4.png
[5]: ./media/application-gateway-web-application-firewall-portal/figure5.png
[6]: ./media/application-gateway-web-application-firewall-portal/figure6.png
[7]: ./media/application-gateway-web-application-firewall-portal/figure7.png
[8]: ./media/application-gateway-web-application-firewall-portal/figure8.png
[9]: ./media/application-gateway-web-application-firewall-portal/figure9.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png


<!--HONumber=Oct16_HO2-->


