---
title: Erstellen oder Aktualisieren eines Anwendungsgateways mit einer Web Application Firewall | Microsoft-Dokumentation
description: "Informationen zum Erstellen eines Anwendungsgateways mit einer Web Application Firewall über das Portal"
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: b561a210-ed99-4ab4-be06-b49215e3255a
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: davidmu
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: e747d4777bdf515e8b7bd54f44ff48b8539ed765
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---

# <a name="create-an-application-gateway-with-a-web-application-firewall-by-using-the-portal"></a>Erstellen eines Anwendungsgateways mit einer Web Application Firewall über das Portal

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure-CLI](application-gateway-web-application-firewall-cli.md)

In diesem Artikel wird erläutert, wie ein Anwendungsgateway, für das eine Web Application Firewall (WAF) aktiviert ist, erstellt wird.

WAF in Azure Application Gateway schützt Webanwendungen vor gängigen webbasierten Angriffen wie der Einschleusung von SQL-Befehlen, Angriffen durch websiteübergreifende Skripts und der Übernahme von Sitzungen. WAF schützt vor vielen der (laut OWASP) 10 häufigsten Websicherheitslücken.

## <a name="scenarios"></a>Szenarien

Dieser Artikel enthält zwei Szenarien. Im ersten Szenario wird das [Erstellen eines Anwendungsgateways mit WAF](#create-an-application-gateway-with-web-application-firewall) beschrieben. Im zweiten Szenario wird das [Hinzufügen von WAF zu einem vorhandenen Anwendungsgateway](#add-web-application-firewall-to-an-existing-application-gateway) erläutert.

![Beispielszenario][scenario]

> [!NOTE]
> Sie können dem Anwendungsgateway benutzerdefinierte Integritätstests, Back-End-Pooladressen und zusätzliche Regeln hinzufügen. Diese Anwendungen werden konfiguriert, nachdem das Anwendungsgateway konfiguriert wurde, nicht während der ersten Bereitstellung.

## <a name="before-you-begin"></a>Voraussetzungen

 Für ein Anwendungsgateway ist ein eigenes Subnetz erforderlich. Stellen Sie beim Erstellen eines virtuellen Netzwerks sicher, dass der Adressraum für mehrere Subnetze ausreicht. Nachdem Sie ein Anwendungsgateway in einem Subnetz bereitgestellt haben, können nur zusätzliche Anwendungsgateways zum Subnetz hinzugefügt werden.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Hinzufügen einer Web Application Firewall zu einem vorhandenen Anwendungsgateway

In diesem Beispiel wird ein vorhandenes Anwendungsgateway aktualisiert, sodass es WAF im **Schutzmodus** unterstützt.

1. Wählen Sie im Azure-Portal im Bereich **Favoriten** die Option **Alle Ressourcen** aus. Wählen Sie auf dem Blatt **Alle Ressourcen** das vorhandene Anwendungsgateway aus. Falls das ausgewählte Abonnement bereits mehrere Ressourcen enthält, können Sie den Namen in das Feld **Nach Name filtern** eingeben, um ganz einfach auf die DNS-Zone zuzugreifen.

   ![Auswahl eines vorhandenen Anwendungsgateways][1]

2. Wählen Sie **Web Application Gateway** aus, und aktualisieren Sie die Einstellungen des Anwendungsgateways. Wenn der Aktualisierungsvorgang abgeschlossen ist, wählen Sie **Speichern** aus. 

3. Verwenden Sie die folgenden Einstellungen, um ein vorhandenes Anwendungsgateway für die WAF-Unterstützung zu aktualisieren:

   | **Einstellung** | **Wert** | **Details**
   |---|---|---|
   |**Upgrade auf WAF-Ebene**| Aktiviert | Mit dieser Option wird die Ebene des Anwendungsgateways auf die WAF-Ebene festgelegt.|
   |**Firewallstatus**| Aktiviert | Mit dieser Einstellung aktivieren Sie die Firewall auf der WAF.|
   |**Firewallmodus** | Prävention | Diese Einstellung gibt an, wie WAF schädlichen Datenverkehr behandelt. Im **Erkennungsmodus** werden die Ereignisse nur protokolliert. Im **Schutzmodus** werden die Ereignisse protokolliert, und der schädliche Datenverkehr wird beendet.|
   |**Regelsatz**|3.0|Diese Einstellung bestimmt den [Kernregelsatz](application-gateway-web-application-firewall-overview.md#core-rule-sets), der für den Schutz der Mitglieder des Back-End-Pools verwendet wird.|
   |**Deaktivierte Regeln konfigurieren**|Variabel|Zur Verhinderung möglicher falsch positiver Ergebnisse können Sie mit dieser Einstellung bestimmte [Regeln und Regelgruppen](application-gateway-crs-rulegroups-rules.md) deaktivieren.|

    >[!NOTE]
    > Wenn Sie ein vorhandenes Anwendungsgateway auf die WAF-SKU aktualisieren, wird die SKU-Größe in **Mittel** geändert. Wenn die Konfiguration abgeschlossen ist, können Sie diese Einstellung neu konfigurieren.

    ![Grundlegende Einstellungen][2-1]

    > [!NOTE]
    > Um WAF-Protokolle anzuzeigen, aktivieren Sie die Diagnose, und wählen Sie **ApplicationGatewayFirewallLog** aus. Wählen Sie nur zu Testzwecken als Instanzenanzahl **1** aus. Eine Instanzenanzahl unter **2** ist nicht empfehlenswert, da dies nicht durch die SLA abgedeckt wird. Kleine Gateways sind nicht verfügbar, wenn Sie WAF verwenden.

## <a name="create-an-application-gateway-with-a-web-application-firewall"></a>Erstellen eines Anwendungsgateways mit einer Web Application Firewall

Dieses Szenario umfasst Folgendes:

* Erstellen Sie ein mittleres WAF-Anwendungsgateway mit zwei Instanzen.
* Sie erstellen ein virtuelles Netzwerk mit dem Namen AdatumAppGatewayVNET und dem reservierten CIDR-Block 10.0.0.0/16.
* Sie erstellen ein Subnetz mit dem Namen Appgatewaysubnet, für das 10.0.0.0/28 als CIDR-Block verwendet wird.
* Sie konfigurieren ein Zertifikat für die SSL-Auslagerung.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an. Falls Sie noch nicht über ein Konto verfügen, können Sie sich für eine [kostenlose einmonatige Testversion](https://azure.microsoft.com/free) registrieren.

2. Wählen Sie im Portalbereich **Favoriten** die Option **Neu** aus.

3. Wählen Sie im auf dem Blatt **Neu** die Option **Netzwerk** aus. Wählen Sie auf dem Blatt **Netzwerk** die Option **Application Gateway** aus, wie in der folgenden Abbildung gezeigt:

    ![Erstellung eines Anwendungsgateways][1]

4. Geben Sie auf dem daraufhin angezeigten Blatt **Grundlagen** die folgenden Werte ein, und wählen Sie anschließend **OK** aus:

   | **Einstellung** | **Wert** | **Details**
   |---|---|---|
   |**Name**|AdatumAppGateway|Der Name des Anwendungsgateways.|
   |**Tier**|WAF|Verfügbare Werte sind „Standard“ und „WAF“. Weitere Informationen über WAF finden Sie unter [Web Application Firewall](application-gateway-web-application-firewall-overview.md).|
   |**SKU-Größe**|Mittel|Standardoptionen für die Ebene sind **Klein**, **Mittel** und **Groß**. Optionen für die WAF-Ebene sind nur **Mittel** und **Groß**.|
   |**Anzahl der Instanzen**|2|Die Anzahl der Instanzen des Anwendungsgateways für hohe Verfügbarkeit. Verwenden Sie nur zu Testzwecken als Instanzenanzahl „1“.|
   |**Abonnement**|[Ihr Abonnement]|Wählen Sie ein Abonnement aus, das für die Erstellung des Anwendungsgateways verwendet werden soll.|
   |**Ressourcengruppe**|**Neu erstellen**: AdatumAppGatewayRG|Erstellen Sie eine Ressourcengruppe. Der Name der Ressourcengruppe muss innerhalb des ausgewählten Abonnements eindeutig sein. Weitere Informationen zu Ressourcengruppen finden Sie in der [Übersicht über Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups).|
   |**Location**|USA (Westen)||

   ![Konfiguration von grundlegenden Einstellungen][2-2]

5. Wählen Sie auf dem Blatt **Einstellungen**, das unter **Virtuelles Netzwerk** angezeigt wird, **Virtuelles Netzwerk auswählen** aus. Wählen Sie auf dem Blatt **Virtuelles Netzwerk auswählen** die Option **Neu erstellen** aus.

   ![Auswahl des virtuellen Netzwerks][2]

6. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** die folgenden Werte ein, und wählen Sie anschließend **OK** aus. Dadurch wird das Feld **Subnetz** auf dem Blatt **Einstellungen** mit dem ausgewählten Subnetz ausgefüllt.

   |**Einstellung** | **Wert** | **Details** |
   |---|---|---|
   |**Name**|AdatumAppGatewayVNET|Der Name des Anwendungsgateways.|
   |**Adressraum**|10.0.0.0/16| Dieser Wert ist der Adressraum für das virtuelle Netzwerk.|
   |**Subnetzname**|AppGatewaySubnet|Der Name des Subnetzes für das Anwendungsgateway.|
   |**Subnetzadressbereich**|10.0.0.0/28 | Dieses Subnetz ermöglicht weitere Subnetze im virtuellen Netzwerk für Mitglieder des Back-End-Pools.|

7. Wählen Sie auf dem Blatt **Einstellungen** unter **Front-End-IP-Konfiguration** für **IP-Adresstyp** die Option **Öffentlich** aus.

8. Wählen Sie auf dem Blatt **Einstellungen** unter **Öffentliche IP-Adresse** die Option **Öffentliche IP-Adresse auswählen** aus. Wählen Sie auf dem Blatt **Öffentliche IP-Adresse auswählen** die Option **Neu erstellen** aus.

   ![Auswahl der öffentlichen IP-Adresse][3]

9. Akzeptieren Sie auf dem Blatt **Öffentliche IP-Adresse erstellen** den Standardwert, und wählen Sie **OK** aus. Das Feld **Öffentliche IP-Adresse** wird mit der öffentlichen IP-Adresse ausgefüllt, die Sie ausgewählt haben.

10. Wählen Sie auf dem Blatt **Einstellungen** unter **Listenerkonfiguration** die Option **HTTP** (unter **Protokoll**) aus. Für die Verwendung von **HTTPS** ist ein Zertifikat erforderlich. Der private Schlüssel für das Zertifikat ist erforderlich. Geben Sie einen PFX-Export des Zertifikats an, und geben Sie das Kennwort für die Datei ein.

11. Konfigurieren Sie die Einstellungen für **WAF**.

   |**Einstellung** | **Wert** | **Details** |
   |---|---|---|
   |**Firewallstatus**| Aktiviert| Diese Einstellung aktiviert oder deaktiviert WAF.|
   |**Firewallmodus** | Prävention| Diese Einstellung bestimmt die Aktionen, die WAF bei schädlichem Datenverkehr durchführt. Im **Erkennungsmodus** wird Datenverkehr nur protokolliert. Im **Schutzmodus** wird Datenverkehr protokolliert und mit der Meldung 403 (nicht autorisiert) beendet.|


12. Überprüfen Sie die Seite **Zusammenfassung**, und wählen Sie **OK** aus. Das Anwendungsgateway wird in der Warteschlange platziert und erstellt.

13. Nachdem das Anwendungsgateway erstellt wurde, navigieren Sie im Portal zu diesem Anwendungsgateway, um die Konfiguration fortzusetzen.

    ![Ressourcenansicht des Anwendungsgateways][10]

Mit diesen Schritten wird ein einfaches Anwendungsgateway mit Standardeinstellungen für Listener, Back-End-Pool, Back-End-HTTP-Einstellungen und Regeln erstellt. Nach der erfolgreichen Bereitstellung können Sie diese Einstellungen an Ihre Bereitstellung anpassen.

> [!NOTE]
> Anwendungsgateways, die mit der WAF-Basiskonfiguration erstellt wurden, werden zum Schutz mit CRS 3.0 konfiguriert.

## <a name="next-steps"></a>Nächste Schritte

Um einen benutzerdefinierten Domänenalias für die [öffentliche IP-Adresse](../dns/dns-custom-domain.md#public-ip-address) zu konfigurieren, können Sie Azure DNS oder einen anderen DNS-Anbieter verwenden.

Informationen zum Konfigurieren der Diagnoseprotokollierung zum Protokollieren von Ereignissen, die mit WAF erkannt oder verhindert werden, finden Sie unter [Application Gateway-Diagnose](application-gateway-diagnostics.md).

Informationen zum Erstellen benutzerdefinierter Integritätstests finden Sie unter [Erstellen eines benutzerdefinierten Integritätstests](application-gateway-create-probe-portal.md).

Informationen zum Konfigurieren der SSL-Auslagerung und zum Entfernen kostspieliger SSL-Abonnements von Ihren Webservern finden Sie unter [Konfigurieren der SSL-Auslagerung](application-gateway-ssl-portal.md).

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[2-1]: ./media/application-gateway-web-application-firewall-portal/figure2-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png

