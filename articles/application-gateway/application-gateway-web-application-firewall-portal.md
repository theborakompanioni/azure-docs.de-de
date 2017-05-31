---
title: Erstellen oder Aktualisieren einer Azure Application Gateway-Instanz mit der Web Application Firewall | Microsoft-Dokumentation
description: "Informationen zum Erstellen eines Anwendungsgateways mit der Web Application Firewall über das Portal"
services: application-gateway
documentationcenter: na
author: georgewallace
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
ms.author: gwallace
ms.translationtype: Human Translation
ms.sourcegitcommit: 7c4d5e161c9f7af33609be53e7b82f156bb0e33f
ms.openlocfilehash: 224aa0db2feb7a83bec5b4ec46140046d10f012e
ms.contentlocale: de-de
ms.lasthandoff: 05/04/2017


---

# <a name="create-an-application-gateway-with-web-application-firewall-by-using-the-portal"></a>Erstellen eines Anwendungsgateways mit der Web Application Firewall über das Portal

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-web-application-firewall-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-web-application-firewall-powershell.md)

In diesem Artikel wird erläutert, wie ein Anwendungsgateway mit der Web Application Firewall erstellt wird.

Die Web Application Firewall (WAF) in Azure Application Gateway schützt Webanwendungen vor gängigen webbasierten Angriffen wie der Einschleusung von SQL-Code, Angriffen durch websiteübergreifende Skripts und der Übernahme von Sitzungen. Ihr Webanwendung ist vor vielen der (laut OWASP) 10 häufigsten Websicherheitslücken geschützt.

## <a name="scenarios"></a>Szenarios

In diesem Artikel gibt es zwei Szenarios:

Im ersten Szenario wird das [Erstellen eines Anwendungsgateways mit der Web Application Firewall](#create-an-application-gateway-with-web-application-firewall) beschrieben.

Im zweiten Szenario wird das [Hinzufügen der Web Application Firewall zu einem vorhandenen Anwendungsgateway](#add-web-application-firewall-to-an-existing-application-gateway) erläutert.

![Beispielszenario][scenario]

> [!NOTE]
> Zusätzliche Konfigurationsschritte für das Anwendungsgateway (u.a. benutzerdefinierte Integritätstests, Back-End-Pool-Adressen und zusätzlichen Regeln) werden nicht während der Erstbereitstellung, sondern nach der Konfiguration des Anwendungsgateways ausgeführt.

## <a name="before-you-begin"></a>Voraussetzungen

Für Azure Application Gateway ist ein eigenes Subnetz erforderlich. Stellen Sie beim Erstellen eines virtuellen Netzwerks sicher, dass der Adressbereich für mehrere Subnetze ausreicht. Sobald Sie ein Anwendungsgateway in einem Subnetz bereitstellen, können nur zusätzliche Anwendungsgateways zum Subnetz hinzugefügt werden.

##<a name="add-web-application-firewall-to-an-existing-application-gateway"></a> Hinzufügen der Web Application Firewall zu einem vorhandenen Anwendungsgateway

In diesem Beispiel wird ein vorhandenes Anwendungsgateway aktualisiert, sodass es die Web Application Firewall im Schutzmodus unterstützt.

1. Klicken Sie im Azure-Portal im Bereich **Favoriten** auf **Alle Ressourcen**. Klicken Sie im Blatt **Alle Ressourcen** auf das vorhandene Anwendungsgateway. Falls das ausgewählte Abonnement bereits mehrere Ressourcen enthält, können Sie den Namen in das Feld **Nach Name filtern...** eingeben. eingeben und komfortabel auf die DNS-Zone zugreifen.

   ![Erstellen eines Anwendungsgateways][1]

1. Klicken Sie auf **Web Application Gateway**, und ändern Sie die Einstellungen des Anwendungsgateways. Wenn Sie fertig sind, klicken Sie auf **Speichern**

    Folgende Einstellungen dienen zum Aktualisieren eines vorhandenen Anwendungsgateways für die Unterstützung der Web Application Firewall:

   | **Einstellung** | **Wert** | **Details**
   |---|---|---|
   |**Upgrade auf WAF-Ebene**| Aktiviert | Dadurch wird die Ebene des Anwendungsgateways auf die WAF-Ebene festgelegt.|
   |**Firewallstatus**| Aktiviert | Aktiviert | Mit dieser Einstellung aktivieren Sie die Firewall auf der WAF.|
   |**Firewallmodus** | Prävention | Diese Einstellung gibt an, wie die Web Application Firewall böswilligen Datenverkehr behandelt. Im **Erkennungsmodus** werden die Ereignisse nur protokolliert. Im **Schutzmodus** werden die Ereignisse protokolliert, und der böswillige Datenverkehr wird beendet.|
   |**Regelsatz**|3.0|Diese Einstellung bestimmt den [Kernregelsatz](application-gateway-web-application-firewall-overview.md#core-rule-sets), der für den Schutz der Mitglieder des Back-End-Pools verwendet wird.|
   |**Deaktivierte Regeln konfigurieren**|Variiert|Zur Verhinderung möglicher falsch positiver Ergebnisse ermöglicht diese Einstellung die Deaktivierung bestimmter [Regeln und Regelgruppen](application-gateway-crs-rulegroups-rules.md).|

    >[!NOTE]
    > Bei der Aktualisierung eines vorhandenen Anwendungsgateways auf die WAF-SKU wird die SKU-Größe in **Mittel** geändert. Dies kann nach Abschluss der Konfiguration neu konfiguriert werden.

    ![Blatt mit Grundeinstellungen][2-1]

    > [!NOTE]
    > Um die Web Application Firewall-Protokolle anzuzeigen, muss die Diagnose aktiviert und ApplicationGatewayFirewallLog ausgewählt sein. Zu Testzwecken kann für die Anzahl von Instanzen der Wert 1 ausgewählt werden. Beachten Sie, dass eine Anzahl von weniger als zwei Instanzen nicht durch die SLA abgedeckt ist und daher nicht empfohlen wird. Kleine Gateways sind nicht verfügbar, wenn Sie die Web Application Firewall verwenden.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Erstellen eines Anwendungsgateways mit der Web Application Firewall

Dieses Szenario umfasst Folgendes:

* Sie erstellen ein mittelgroßes Anwendungsgateway mit zwei Instanzen mit der Web Application Firewall.
* Sie erstellen ein virtuelles Netzwerk mit dem Namen AdatumAppGatewayVNET und dem reservierten CIDR-Block 10.0.0.0/16.
* Sie erstellen ein Subnetz mit dem Namen Appgatewaysubnet, für das 10.0.0.0/28 als CIDR-Block verwendet wird.
* Sie konfigurieren ein Zertifikat für die SSL-Auslagerung.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose einmonatige Testversion](https://azure.microsoft.com/free) registrieren.
1. Klicken Sie im Portalbereich „Favoriten“ auf **Neu**.
1. Klicken Sie auf dem Blatt **Neu** auf **Netzwerk**. Klicken Sie auf dem Blatt **Netzwerk** auf **Anwendungsgateway**, wie in der folgenden Abbildung zu sehen:
1. Navigieren Sie zum Azure-Portal, klicken Sie auf **Neu** > **Netzwerk** >  **Application Gateway**.

    ![Erstellen eines Anwendungsgateways][1]

1. Geben Sie auf dem daraufhin angezeigten Blatt **Grundlagen** die folgenden Werte ein, und klicken Sie anschließend auf **OK**:

   | **Einstellung** | **Wert** | **Details**
   |---|---|---|
   |**Name**|AdatumAppGateway|Der Name des Anwendungsgateways|
   |**Ebene**|WAF|Verfügbare Werte sind „Standard“ und „WAF“. Besuchen Sie [Web Application Firewall (WAF)](application-gateway-web-application-firewall-overview.md), um weitere Informationen über WAF zu erhalten.|
   |**SKU-Größe**|Mittel|Bei Auswahl der Ebene „Standard“ stehen „Klein“, „Mittel“ und „Groß“ zur Auswahl. Bei Auswahl der Ebene „WAF“ stehen nur „Mittel“ und „Groß“ zur Auswahl.|
   |**Anzahl der Instanzen**|2|Die Anzahl der Instanzen des Anwendungsgateways für hohe Verfügbarkeit. Eine Instanzenzahl von 1 sollte nur zu Testzwecken verwendet werden.|
   |**Abonnement**|[Ihr Abonnement]|Wählen Sie ein Abonnement aus, in dem Sie das Anwendungsgateway erstellen möchten.|
   |**Ressourcengruppe**|**Neu erstellen**: AdatumAppGatewayRG|Erstellen Sie eine Ressourcengruppe. Der Name der Ressourcengruppe muss innerhalb des ausgewählten Abonnements eindeutig sein. Weitere Informationen zu Ressourcengruppen finden Sie in der [Übersicht über Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups).|
   |**Location**|USA (West)||

   ![Blatt mit Grundeinstellungen][2-2]

1. Klicken Sie auf dem Blatt **Einstellungen**, das unter **Virtuelles Netzwerk** angezeigt wird, auf **Virtuelles Netzwerk auswählen**. Damit wird das Blatt **Virtuelles Netzwerk auswählen** geöffnet.  Klicken Sie auf **Neu erstellen**, um das Blatt **Virtuelles Netzwerk erstellen** zu öffnen.

   ![Wählen eines virtuellen Netzwerks][2]

1. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** die folgenden Werte ein, und klicken Sie anschließend auf **OK**. Damit werden die Blätter **Virtuelles Netzwerk erstellen** und **Virtuelles Netzwerk auswählen** geschlossen. Auch das Feld **Subnetz** auf dem Blatt **Einstellungen** mit dem ausgewählten Subnetz wird geschlossen.

   |**Einstellung** | **Wert** | **Details** |
   |---|---|---|
   |**Name**|AdatumAppGatewayVNET|Name des Anwendungsgateways.|
   |**Adressraum**|10.0.0.0/16| Dies ist der Adressraum für das virtuelle Netzwerk.|
   |**Subnetzname**|AppGatewaySubnet|Name des Subnetzes für das Anwendungsgateway.|
   |**Subnetzadressbereich**|10.0.0.0/28| Dieses Subnetz ermöglicht weitere Subnetze im virtuellen Netzwerk für Back-End-Pool Mitglieder.|

1. Wählen Sie auf dem Blatt **Einstellungen** unter **Frontend-IP-Konfiguration** den **IP-Adresstyp** **Öffentlich** aus.

1. Klicken Sie auf dem Blatt **Einstellungen** unter **Öffentliche IP-Adresse** auf **Öffentliche IP-Adresse auswählen**. Daraufhin wird das Blatt **Öffentliche IP-Adresse auswählen** geöffnet. Klicken Sie auf **Neu erstellen**.

   ![Öffentliche IP-Adresse auswählen][3]

1. Akzeptieren Sie auf dem Blatt **Öffentliche IP-Adresse erstellen** den Standardwert, und klicken Sie auf **OK**. Daraufhin werden die Blätter **Öffentliche IP-Adresse auswählen** und **Öffentliche IP-Adresse erstellen** geschlossen, und **Öffentliche IP-Adresse** wird mit der ausgewählten öffentlichen IP-Adresse gefüllt.

1. Klicken Sie auf dem Blatt **Einstellungen** unter **Listenerkonfiguration** unter **Protokoll** auf **HTTP**. Für die Verwendung von **HTTPS**ist ein Zertifikat erforderlich. Da der private Schlüssel des Zertifikats benötigt wird, müssen ein PFX-Export des Zertifikats und das Dateikennwort bereitgestellt werden.

1. Konfigurieren Sie die **WAF** -spezifischen Einstellungen.

   |**Einstellung** | **Wert** | **Details** |
   |---|---|---|
   |**Firewallstatus**| Aktiviert| Diese Einstellung aktiviert oder deaktiviert WAF.|
   |**Firewallmodus** | Prävention| Diese Einstellung bestimmt die Aktionen, die WAF bei böswilligem Datenverkehr unternimmt. Wenn Sie **Erkennung** auswählen, wird der Datenverkehr nur protokolliert.  Wenn Sie **Schutz** auswählen, wird der Datenverkehr protokolliert und mit der Meldung 403 (nicht autorisiert) beendet.|


1. Überprüfen Sie die Zusammenfassungsseite, und klicken Sie auf **OK**.  Das Anwendungsgateway wird in der Warteschlange platziert und erstellt.

1. Nachdem das Anwendungsgateway erstellt wurde, navigieren Sie im Portal zu diesem Gateway, um die Konfiguration fortzusetzen.

    ![Application Gateway – Ressourcenansicht][10]

Mit diesen Schritten wird ein einfaches Anwendungsgateway mit Standardeinstellungen für Listener, Back-End-Pool, Back-End-HTTP-Einstellungen und Regeln erstellt. Nach der erfolgreichen Bereitstellung können Sie diese Einstallungen an Ihre Anforderungen anpassen.

> [!NOTE]
> Anwendungsgateways, die mit der Basiskonfiguration der Web Application Firewall erstellt wurden, werden zum Schutz mit CRS 3.0 konfiguriert.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren der Diagnoseprotokollierung zum Protokollieren von Ereignissen, die mit der Web Application Firewall erkannt oder verhindert werden, finden Sie unter [Application Gateway-Diagnose](application-gateway-diagnostics.md).

Unter [Erstellen eines benutzerdefinierten Integritätstests](application-gateway-create-probe-portal.md)

Unter [Konfigurieren der SSL-Auslagerung](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[2-1]: ./media/application-gateway-web-application-firewall-portal/figure2-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png

