<properties
   pageTitle="Erstellen einer pfadbasierten Regel für ein Application Gateway über das Portal | Microsoft Azure"
   description="Erfahren Sie, wie Sie eine pfadbasierte Regel für ein Application Gateway über das Portal erstellen."
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
   ms.date="08/18/2016"
   ms.author="gwallace" />

# Erstellen einer pfadbasierten Regel für ein Application Gateway über das Portal

> [AZURE.SELECTOR]
- [Azure-Portal](application-gateway-create-url-route-portal.md)
- [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)

Mit Routing auf URL-Pfadbasis können Sie Routen basierend auf dem URL-Pfad der Http-Anforderung zuordnen. Es wird überprüft, ob eine Route zu einem Backend-Pool für die URL-Listen im Application Gateway konfiguriert ist, und der Netzwerkverkehr wird an den definierten Back-End-Pool gesendet. Ein gängiges Szenario für URL-basiertes Routing ist der Lastenausgleich von Anforderungen für verschiedene Inhaltstypen auf verschiedene Back-End-Serverpools.

Mit Routing auf URL-Basis wird ein neuer Regeltyp für das Application Gateway eingeführt. Application Gateways verfügen über zwei Regeltypen: Basisregeln und pfadbasierte Regeln. Der Basisregeltyp bietet einen Roundrobin-Dienst für die Back-End-Pools, während pfadbasierte Regeln neben der Roundrobin-Verteilung auch Pfadmuster der Anforderungs-URL beim Auswählen des Back-End-Pools berücksichtigen.



## Szenario

Das folgende Szenario führt Sie durch die Erstellung einer pfadbasierten Regel in einem vorhandenen Application Gateway. Für dieses Szenario wird davon ausgegangen, dass Sie die Schritte unter [Erstellen eines Anwendungsgateways](application-gateway-create-gateway-portal.md) bereits durchgeführt haben.

![URL-Route][scenario]

## <a name="createrule"></a>Erstellen der pfadbasierten Regel

Eine pfadbasierende Regel erfordert einen eigenen Listener. Stellen Sie daher vor dem Erstellen der Regel sicher, dass Ihnen ein Listener zur Verfügung steht, den Sie verwenden können.

### Schritt 1

Navigieren Sie zu http://portal.azure.com, und wählen Sie ein vorhandenes Anwendungsgateway aus. Klicken Sie auf **Regeln**.

![Übersicht über Application Gateway][1]

### Schritt 2

Klicken Sie auf **Pfadbasiert**, um eine neue pfadbasierte Regel hinzuzufügen.

### Schritt 3

Das Blatt **Add path-based rule** (Pfadbasierte Regel hinzufügen) verfügt über zwei Abschnitte. Im ersten Abschnitt haben Sie den Listener, den Namen der Regel und die Standardpfadeinstellungen festgelegt. Die Standardpfadeinstellungen gelten für Routen, die nicht unter benutzerdefinierte, pfadbasierte Regeln fallen. Im zweiten Abschnitt des Blatts **Add path-based rule** (Pfadbasierte Regel hinzufügen) definierten Sie die pfadbasierten Regeln.

**Basic Settings**

- **Name**: Dies ist der Anzeigename für die Regel, auf die Sie über das Portal zugreifen können.
- **Listener**: Dies ist der Listener, der für die Regel verwendet wird.
- **Default backend pool** (Standard-Back-End-Pool): Dies ist die Einstellung, die das Back-End für die Standardregel festlegt.
- **Default HTTP settings** (Standard-HTTP-Einstellungen): Dies ist die Einstellung, die die HTTP-Einstellungen für die Standardregel festlegt.

**Pfadbasierte Regeln**

- **Name**: Dies ist der Anzeigename der pfadbasierten Regel.
- **Pfade**: Diese Einstellung legt den Pfad fest, nach dem die Regel beim Weiterleiten von Datenverkehr sucht.
- **Back-End-Pool**: Dies ist die Einstellung, die das Back-End für die Regel festlegt.
- **HTTP-Einstellung**: Dies ist die Einstellung, die die HTTP-Einstellungen für die Regel festlegt.

>[AZURE.IMPORTANT] Pfade: Die Liste der abzustimmenden Pfadmuster. Jedes muss mit „/“ beginnen, und ein „*“ ist nur am Ende zulässig. Gültige Beispiele sind „/xyz“, „/xyz*“ oder „/xyz/*“.

![Hinzufügen eines Blatts mit pfadbasierten Regeln mit bereitgestellten Informationen][2]

Das Hinzufügen einer pfadbasierten Regel zu einem bestehenden Application Gateway ist über das Portal ein einfacher Prozess. Nachdem eine pfadbasierte Regel erstellt wurde, kann sie bearbeitet werden, damit zusätzliche Regeln einfach hinzugefügt werden können.

![Hinzufügen zusätzlicher pfadbasierter Regeln][3]

## Nächste Schritte

Informationen zum Konfigurieren der SSL-Auslagerung mit Azure Application Gateway finden Sie unter [Konfigurieren der SSL-Auslagerung](application-gateway-ssl-portal.md).

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png

<!---HONumber=AcomDC_0824_2016-->