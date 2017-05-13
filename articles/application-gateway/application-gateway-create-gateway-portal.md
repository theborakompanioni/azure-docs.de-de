---
title: "Erstellen eines Anwendungsgateways über das Portal | Microsoft Docs"
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
ms.date: 04/05/2017
ms.author: gwallace
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 2195eb4ce0e22c8c7c72ac0638ab927f13c4d454
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---
# <a name="create-an-application-gateway-by-using-the-portal"></a>Erstellen eines Anwendungsgateways über das Portal

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
> * [Klassische Azure PowerShell](application-gateway-create-gateway.md)
> * [Azure Resource Manager-Vorlage](application-gateway-create-gateway-arm-template.md)
> * [Azure-Befehlszeilenschnittstelle](application-gateway-create-gateway-cli.md)

Erfahren Sie, wie Sie ein Anwendungsgateway mithilfe von SSL-Auslagerung erstellen.

![Beispielszenario][scenario]

Application Gateway ist ein dediziertes virtuelles Gerät mit einem ADC (Application Delivery Controller) als Dienst und bietet verschiedene Lastenausgleichsfunktionen der Ebene 7 für Ihre Anwendung.

Dieses Szenario umfasst Folgendes:

1. [Erstellen eines mittleren Anwendungsgateways](#create-an-application-gateway) mit SSL-Auslagerung mit zwei Instanzen in eigenem Subnetz.
1. [Hinzufügen von Servern zum Back-End-Pool](#add-servers-to-backend-pools)
1. [Löschen aller Ressourcen](#delete-all-resources) Für manche der Ressourcen, die in dieser Übung erstellt werden, fallen Bereitstellungsgebühren an. Führen Sie daher nach Abschluss der Übung unbedingt die Schritte in diesem Abschnitt aus, um die erstellten Ressourcen zu löschen und die Gebühren möglichst gering zu halten.



> [!IMPORTANT]
> Zusätzliche Konfigurationsschritte für das Anwendungsgateway (u.a. benutzerdefinierte Integritätstests, Back-End-Pool-Adressen und zusätzlichen Regeln) werden nicht während der Erstbereitstellung, sondern nach der Konfiguration des Anwendungsgateways ausgeführt.

## <a name="create-an-application-gateway"></a>Erstellen eines Anwendungsgateways

Führen Sie zum Erstellen eines Anwendungsgateways die folgenden Schritte aus. Für das Anwendungsgateway ist ein eigenes Subnetz erforderlich. Stellen Sie beim Erstellen eines virtuellen Netzwerks sicher, dass der Adressbereich für mehrere Subnetze ausreicht. Sobald Sie ein Anwendungsgateway in einem Subnetz bereitstellen, können nur zusätzliche Anwendungsgateways zum Subnetz hinzugefügt werden.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose einmonatige Testversion](https://azure.microsoft.com/free) registrieren.
1. Klicken Sie im Portalbereich „Favoriten“ auf **Neu**.
1. Klicken Sie auf dem Blatt **Neu** auf **Netzwerk**. Klicken Sie auf dem Blatt **Netzwerk** auf **Anwendungsgateway**, wie in der folgenden Abbildung zu sehen:

    ![Erstellen eines Anwendungsgateways][1]

1. Geben Sie auf dem daraufhin angezeigten Blatt **Grundlagen** die folgenden Werte ein, und klicken Sie anschließend auf **OK**:

   | **Einstellung** | **Wert** | **Details**
   |---|---|---|
   |**Name**|AdatumAppGateway|Der Name des Anwendungsgateways|
   |**Ebene**|Standard|Verfügbare Werte sind „Standard“ oder „WAF“. Besuchen Sie [Web Application Firewall (WAF)](application-gateway-web-application-firewall-overview.md), um weitere Informationen über WAF zu erhalten.|
   |**SKU-Größe**|Mittel|Bei Auswahl der Ebene „Standard“ stehen „Klein“, „Mittel“ und „Groß“ zur Auswahl. Bei Auswahl der Ebene „WAF“ stehen nur „Mittel“ und „Groß“ zur Auswahl.|
   |**Anzahl der Instanzen**|2|Die Anzahl der Instanzen des Anwendungsgateways für hohe Verfügbarkeit. Eine Instanzenzahl von 1 sollte nur zu Testzwecken verwendet werden.|
   |**Abonnement**|[Ihr Abonnement]|Wählen Sie ein Abonnement aus, in dem Sie das Anwendungsgateway erstellen möchten.|
   |**Ressourcengruppe**|**Neu erstellen**: AdatumAppGatewayRG|Erstellen Sie eine Ressourcengruppe. Der Name der Ressourcengruppe muss innerhalb des ausgewählten Abonnements eindeutig sein. Weitere Informationen zu Ressourcengruppen finden Sie in der [Übersicht über Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups).|
   |**Location**|USA (West)||

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

1. Klicken Sie auf dem Blatt **Einstellungen** unter **Listenerkonfiguration** unter **Protokoll** auf **HTTPS**. Dadurch werden zusätzliche Felder hinzugefügt. Klicken Sie auf das Ordnersymbol für das Feld **PFX-Zertifikat hochladen**, und wählen Sie das entsprechende PFX-Zertifikat aus. Geben Sie die folgenden Informationen in die zusätzlichen **Listenerkonfiguration**-Felder ein:

   |**Einstellung** | **Wert** | **Details** |
   |---|---|---|
   |Name|Name des Zertifikats|Dieser Wert ist ein Anzeigename, mit dem auf das Zertifikat verwiesen wird.|
   |Kennwort|Kennwort für die PFX-Datei| Dieses Kennwort wird für den privaten Schlüssel verwendet.|

1. Klicken Sie auf dem Blatt **Einstellungen** auf **OK**, um den Vorgang fortzusetzen.

1. Überprüfen Sie die Einstellungen auf dem Blatt **Zusammenfassung**, und klicken Sie auf **OK**, um das Erstellen des Anwendungsgateways zu starten. Das Erstellen eines Anwendungsgateways ist eine Aufgabe mit langer Ausführungszeit.

## <a name="add-servers-to-backend-pools"></a>Hinzufügen von Servern zu Back-End-Pools

Nachdem das Anwendungsgateway erstellt wurde, müssen die Systeme, die die mit einem Lastenausgleich zu versehende Anwendung hosten, weiter dem Anwendungsgateway hinzugefügt werden. IP-Adressen, FQDN oder NICs dieser Server werden den Back-End-Adresspools hinzugefügt.

### <a name="ip-address-or-fqdn"></a>IP-Adresse oder FQDN

1. Klicken Sie nach der Erstellung des Anwendungsgateways im Bereich **Favoriten** des Azure-Portals auf **Alle Ressourcen**. Klicken Sie im Blatt „Alle Ressourcen“ auf das Anwendungsgateway **AdatumAppGateway**. Falls das ausgewählte Abonnement bereits mehrere Ressourcen enthält, können Sie **AdatumAppGateway** in das Feld **Nach Name filtern...** eingeben. eingeben und komfortabel auf das Anwendungsgateway zugreifen.

1. Das erstellte Anwendungsgateway wird angezeigt. Klicken Sie auf **Back-End-Pools**, und wählen Sie den aktuellen Back-End-Pool **appGatewayBackendPool**. Damit wird das Blatt **appGatewayBackendPool** geöffnet.

   ![Application Gateway-Back-End-Pools][4]

1. Klicken Sie auf **Ziel hinzufügen**, um IP-Adressen von FQDN-Werten hinzuzufügen. Wählen Sie **IP-Adresse oder FQDN** als **Typ**, und geben Sie Ihre IP-Adresse oder Ihren FQDN in das Feld ein. Wiederholen Sie diesen Schritt für zusätzliche Back-End-Pool-Mitglieder. Klicken Sie anschließend auf **Speichern**.

### <a name="virtual-machine-and-nic"></a>Virtueller Computer und NIC

Sie können auch Netzwerkkarten virtueller Computer als Back-End-Poolmitglieder hinzufügen. In der Dropdownliste stehen nur virtuelle Computer im gleichen virtuellen Netzwerk wie das Anwendungsgateway zur Auswahl.

1. Klicken Sie nach der Erstellung des Anwendungsgateways im Bereich **Favoriten** des Azure-Portals auf **Alle Ressourcen**. Klicken Sie im Blatt „Alle Ressourcen“ auf das Anwendungsgateway **AdatumAppGateway**. Falls das ausgewählte Abonnement bereits mehrere Ressourcen enthält, können Sie **AdatumAppGateway** in das Feld **Nach Name filtern...** eingeben. eingeben und komfortabel auf das Anwendungsgateway zugreifen.

1. Das erstellte Anwendungsgateway wird angezeigt. Klicken Sie auf **Back-End-Pools**, und wählen Sie den aktuellen Back-End-Pool **appGatewayBackendPool**. Damit wird das Blatt **appGatewayBackendPool** geöffnet.

   ![Application Gateway-Back-End-Pools][5]

1. Klicken Sie auf **Ziel hinzufügen**, um IP-Adressen von FQDN-Werten hinzuzufügen. Wählen Sie **Virtueller Computer** als **Typ**, und wählen Sie die zu verwendende VM und NIC aus. Klicken Sie anschließend auf **Speichern**.

   > [!NOTE]
   > In der Dropdownliste stehen nur virtuelle Computer im gleichen virtuellen Netzwerk wie das Anwendungsgateway zur Auswahl.

## <a name="delete-all-resources"></a>Löschen aller Ressourcen

Führen Sie die folgenden Schritte aus, um alle Ressourcen zu löschen, die in diesem Artikel erstellt wurden:

1. Klicken Sie im Azure-Portal im Bereich **Favoriten** auf **Alle Ressourcen**. Klicken Sie auf dem Blatt „Alle Ressourcen“ auf die Ressourcengruppe **AdatumAppGatewayRG**. Falls das ausgewählte Abonnement bereits mehrere Ressourcen enthält, können Sie **AdatumAppGatewayRG** in das Feld **Nach Name filtern...** eingeben. eingeben und komfortabel auf die Ressourcengruppe zugreifen.
1. Klicken Sie auf dem Blatt **AdatumAppGatewayRG** auf die Schaltfläche **Löschen**.
1. Zur Bestätigung des Löschvorgangs werden Sie vom Portal zur Eingabe des Ressourcengruppennamens aufgefordert. Klicken Sie auf **Löschen**, geben Sie den Ressourcengruppennamen AdatumAppGateway ein, und klicken Sie anschließend auf **Löschen**. Da beim Löschen einer Ressourcengruppe alle Ressourcen innerhalb der Ressourcengruppe gelöscht werden, überprüfen Sie vor dem Löschen immer den Inhalt der Ressourcengruppe. Das Portal löscht zuerst alle in der Ressourcengruppe enthaltenen Ressourcen und anschließend die Ressourcengruppe selbst. Dieser Vorgang dauert einige Minuten.

## <a name="next-steps"></a>Nächste Schritte

In diesem Szenario wird ein Standardanwendungsgateway erstellt. In den nächsten Schritten wird das Anwendungsgateway konfiguriert, indem Einstellungen geändert und Regeln im Gateway angepasst werden. Diese Schritte werden in den folgenden Artikeln behandelt:

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

