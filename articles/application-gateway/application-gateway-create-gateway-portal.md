---
title: "Erstellen eines Anwendungsgateways – Azure-Portal | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie ein Anwendungsgateway über das Portal erstellen"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 54dffe95-d802-4f86-9e2e-293f49bd1e06
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: d3c39cfe3159cd4059a81f966fb551175188278b
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="create-an-application-gateway-with-the-portal"></a>Erstellen eines Anwendungsgateways über das Portal

[Application Gateway](application-gateway-introduction.md) ist ein dediziertes virtuelles Gerät, das einen ADC (Application Delivery Controller) als Dienst bereitstellt und für Ihre Anwendung verschiedene Lastenausgleichsfunktionen der Ebene 7 bietet. In diesem Artikel erfahren Sie, wie Sie über das Azure-Portal ein Anwendungsgateway erstellen und einen vorhandenen Server als Back-End- Mitglied hinzufügen.

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich unter [http://portal.azure.com](http://portal.azure.com) beim Azure-Portal an.

## <a name="create-application-gateway"></a>Erstellen eines Anwendungsgateways

Führen Sie zum Erstellen eines Anwendungsgateways die folgenden Schritte aus. Für das Anwendungsgateway ist ein eigenes Subnetz erforderlich. Stellen Sie beim Erstellen eines virtuellen Netzwerks sicher, dass der Adressbereich für mehrere Subnetze ausreicht. Nachdem das Anwendungsgateway in einem Subnetz bereitgestellt wurde, können ihm nur noch andere Anwendungsgateways hinzugefügt werden.

1. Klicken Sie im Portalbereich „Favoriten“ auf **Neu**.
1. Klicken Sie auf dem Blatt **Neu** auf **Netzwerk**. Klicken Sie auf dem Blatt **Netzwerk** auf **Anwendungsgateway**, wie in der folgenden Abbildung zu sehen:

    ![Erstellen eines Anwendungsgateways][1]

1. Geben Sie auf dem daraufhin angezeigten Blatt **Grundlagen** die folgenden Werte ein, und klicken Sie anschließend auf **OK**:

   | **Einstellung** | **Wert** | **Details**|
   |---|---|---|
   |**Name**|AdatumAppGateway|Der Name des Anwendungsgateways|
   |**Ebene**|Standard|Verfügbare Werte sind „Standard“ oder „WAF“. Besuchen Sie [Web Application Firewall (WAF)](application-gateway-web-application-firewall-overview.md), um weitere Informationen über WAF zu erhalten.|
   |**SKU-Größe**|Mittel|Bei Auswahl der Ebene „Standard“ stehen „Klein“, „Mittel“ und „Groß“ zur Auswahl. Bei Auswahl der Ebene „WAF“ stehen nur „Mittel“ und „Groß“ zur Auswahl.|
   |**Anzahl der Instanzen**|2|Die Anzahl der Instanzen des Anwendungsgateways für hohe Verfügbarkeit. Eine Instanzenzahl von 1 sollte nur zu Testzwecken verwendet werden.|
   |**Abonnement**|[Ihr Abonnement]|Wählen Sie ein Abonnement aus, in dem Sie das Anwendungsgateway erstellen möchten.|
   |**Ressourcengruppe**|**Neu erstellen**: AdatumAppGatewayRG|Erstellen Sie eine Ressourcengruppe. Der Name der Ressourcengruppe muss innerhalb des ausgewählten Abonnements eindeutig sein. Weitere Informationen zu Ressourcengruppen finden Sie in der [Übersicht über Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups).|
   |**Location**|USA (West)||

1. Klicken Sie auf dem Blatt **Einstellungen**, das unter **Virtuelles Netzwerk** angezeigt wird, auf **Virtuelles Netzwerk auswählen**. Das Blatt **Virtuelles Netzwerk auswählen** wird geöffnet.  Klicken Sie auf **Neu erstellen**, um das Blatt **Virtuelles Netzwerk erstellen** zu öffnen.

   ![Wählen eines virtuellen Netzwerks][2]

1. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** die folgenden Werte ein, und klicken Sie anschließend auf **OK**. Die Blätter **Virtuelles Netzwerk erstellen** und **Virtuelles Netzwerk auswählen** werden geschlossen. Außerdem wird das Feld **Subnetz** auf dem Blatt **Einstellungen** mit dem ausgewählten Subnetz ausgefüllt.

   | **Einstellung** | **Wert** | **Details**|
   |---|---|---|
   |**Name**|AdatumAppGatewayVNET|Name des Anwendungsgateways.|
   |**Adressraum**|10.0.0.0/16|Dies ist der Adressraum für das virtuelle Netzwerk.|
   |**Subnetzname**|AppGatewaySubnet|Name des Subnetzes für das Anwendungsgateway.|
   |**Subnetzadressbereich**|10.0.0.0/28|Dieses Subnetz ermöglicht weitere Subnetze im virtuellen Netzwerk für Back-End-Pool Mitglieder.|

1. Wählen Sie auf dem Blatt **Einstellungen** unter **Front-End-IP-Konfiguration** für **IP-Adresstyp** die Option **Öffentlich** aus.

1. Klicken Sie auf dem Blatt **Einstellungen** unter **Öffentliche IP-Adresse** auf **Öffentliche IP-Adresse auswählen**. Daraufhin wird das Blatt **Öffentliche IP-Adresse auswählen** geöffnet. Klicken Sie auf **Neu erstellen**.

   ![Öffentliche IP-Adresse auswählen][3]

1. Akzeptieren Sie auf dem Blatt **Öffentliche IP-Adresse erstellen** den Standardwert, und klicken Sie auf **OK**. Das Blatt wird geschlossen, und **Öffentliche IP-Adresse** wird mit der gewählten öffentlichen IP-Adresse ausgefüllt.

1. Klicken Sie auf dem Blatt **Einstellungen** unter **Listenerkonfiguration** auf **HTTP** (unter **Protokoll**). Geben Sie im Feld **Port** den gewünschten Port ein.

2. Klicken Sie auf dem Blatt **Einstellungen** auf **OK**, um den Vorgang fortzusetzen.

1. Überprüfen Sie die Einstellungen auf dem Blatt **Zusammenfassung**, und klicken Sie auf **OK**, um das Erstellen des Anwendungsgateways zu starten. Das Erstellen eines Anwendungsgateways ist eine Aufgabe mit langer Ausführungszeit und dauert eine Weile.

## <a name="add-servers-to-backend-pools"></a>Hinzufügen von Servern zu Back-End-Pools

Nach der Erstellung des Anwendungsgateways müssen die Systeme, die die Anwendung hosten, für die ein Lastenausgleich verwendet werden soll, noch dem Anwendungsgateway hinzugefügt werden. IP-Adressen, FQDN oder NICs dieser Server werden den Back-End-Adresspools hinzugefügt.

### <a name="ip-address-or-fqdn"></a>IP-Adresse oder FQDN

1. Klicken Sie nach der Erstellung des Anwendungsgateways im Bereich **Favoriten** des Azure-Portals auf **Alle Ressourcen**. Klicken Sie im Blatt „Alle Ressourcen“ auf das Anwendungsgateway **AdatumAppGateway**. Falls das ausgewählte Abonnement bereits mehrere Ressourcen enthält, können Sie **AdatumAppGateway** in das Feld **Nach Name filtern...** eingeben. eingeben und komfortabel auf das Anwendungsgateway zugreifen.

1. Das erstellte Anwendungsgateway wird angezeigt. Klicken Sie auf **Back-End-Pools**, und wählen Sie den aktuellen Back-End-Pool **appGatewayBackendPool** aus. Daraufhin wird das Blatt **appGatewayBackendPool** geöffnet.

   ![Application Gateway-Back-End-Pools][4]

1. Klicken Sie auf **Ziel hinzufügen**, um IP-Adressen von FQDN-Werten hinzuzufügen. Wählen Sie **IP-Adresse oder FQDN** als **Typ**, und geben Sie Ihre IP-Adresse oder Ihren FQDN in das Feld ein. Wiederholen Sie diesen Schritt für zusätzliche Back-End-Pool-Mitglieder. Klicken Sie anschließend auf **Speichern**.

### <a name="virtual-machine-and-nic"></a>Virtueller Computer und NIC

Sie können auch Netzwerkkarten virtueller Computer als Back-End-Poolmitglieder hinzufügen. In der Dropdownliste stehen nur virtuelle Computer im gleichen virtuellen Netzwerk wie das Anwendungsgateway zur Auswahl.

1. Klicken Sie nach der Erstellung des Anwendungsgateways im Bereich **Favoriten** des Azure-Portals auf **Alle Ressourcen**. Klicken Sie im Blatt „Alle Ressourcen“ auf das Anwendungsgateway **AdatumAppGateway**. Falls das ausgewählte Abonnement bereits mehrere Ressourcen enthält, können Sie **AdatumAppGateway** in das Feld **Nach Name filtern...** eingeben. eingeben und komfortabel auf das Anwendungsgateway zugreifen.

1. Das erstellte Anwendungsgateway wird angezeigt. Klicken Sie auf **Back-End-Pools**, und wählen Sie den aktuellen Back-End-Pool **appGatewayBackendPool** aus. Daraufhin wird das Blatt **appGatewayBackendPool** geöffnet.

   ![Application Gateway-Back-End-Pools][5]

1. Klicken Sie auf **Ziel hinzufügen**, um IP-Adressen von FQDN-Werten hinzuzufügen. Wählen Sie **Virtueller Computer** als **Typ**, und wählen Sie die zu verwendende VM und NIC aus. Klicken Sie anschließend auf **Speichern**.

   > [!NOTE]
   > In der Dropdownliste stehen nur virtuelle Computer im gleichen virtuellen Netzwerk wie das Anwendungsgateway zur Auswahl.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe, das Anwendungsgateway und alle dazugehörigen Ressourcen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu auf dem Blatt für das Anwendungsgateway die Ressourcengruppe aus, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Szenario haben Sie ein Anwendungsgateway bereitgestellt und dem Back-End einen Server hinzugefügt. In den nächsten Schritten wird das Anwendungsgateway konfiguriert, indem Einstellungen geändert und Regeln im Gateway angepasst werden. Diese Schritte werden in den folgenden Artikeln behandelt:

Unter [Erstellen eines benutzerdefinierten Integritätstests](application-gateway-create-probe-portal.md)

Unter [Konfigurieren der SSL-Auslagerung](application-gateway-ssl-portal.md)

Informieren Sie sich über das Schützen von Anwendungen per [Web Application Firewall](application-gateway-webapplicationfirewall-overview.md), einem Feature des Anwendungsgateways.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png

