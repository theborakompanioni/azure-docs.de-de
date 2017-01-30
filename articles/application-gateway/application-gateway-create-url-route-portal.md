---
title: "Erstellen einer pfadbasierten Regel für ein Application Gateway über das Portal | Microsoft Docs"
description: "Erfahren Sie, wie Sie eine pfadbasierte Regel für ein Application Gateway über das Portal erstellen."
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 87bd93bc-e1a6-45db-a226-555948f1feb7
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/13/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 09aeb63d4c2e68f22ec02f8c08f5a30c32d879dc
ms.openlocfilehash: 2889716d6b5b6079c311d6a7f1eb97b001098b45


---
# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-portal"></a>Erstellen einer pfadbasierten Regel für ein Application Gateway über das Portal

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)

Mit Routing auf URL-Pfadbasis können Sie Routen basierend auf dem URL-Pfad der Http-Anforderung zuordnen. Es wird überprüft, ob eine Route zu einem Backend-Pool für die URL-Listen im Application Gateway konfiguriert ist, und der Netzwerkverkehr wird an den definierten Back-End-Pool gesendet. Ein gängiges Szenario für URL-basiertes Routing ist der Lastenausgleich von Anforderungen für verschiedene Inhaltstypen auf verschiedene Back-End-Serverpools.

Mit Routing auf URL-Basis wird ein neuer Regeltyp für das Application Gateway eingeführt. Application Gateways verfügen über zwei Regeltypen: Basisregeln und pfadbasierte Regeln. Der Basisregeltyp bietet einen Roundrobin-Dienst für die Back-End-Pools, während pfadbasierte Regeln neben der Roundrobin-Verteilung auch Pfadmuster der Anforderungs-URL beim Auswählen des Back-End-Pools berücksichtigen.

## <a name="scenario"></a>Szenario

Das folgende Szenario führt Sie durch die Erstellung einer pfadbasierten Regel in einem vorhandenen Application Gateway.
Für dieses Szenario wird davon ausgegangen, dass Sie die Schritte unter [Erstellen eines Anwendungsgateways](application-gateway-create-gateway-portal.md)bereits durchgeführt haben.

![URL-Route][scenario]

## <a name="a-namecreateruleacreate-the-path-based-rule"></a><a name="createrule"></a>Erstellen der pfadbasierten Regel

Eine pfadbasierende Regel erfordert einen eigenen Listener. Stellen Sie daher vor dem Erstellen der Regel sicher, dass Ihnen ein Listener zur Verfügung steht, den Sie verwenden können.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zum [Azure-Portal](http://portal.azure.com), und wählen Sie ein vorhandenes Anwendungsgateway aus. Klicken Sie auf **Regeln**

![Übersicht über Application Gateway][1]

### <a name="step-2"></a>Schritt 2

Klicken Sie auf **Pfadbasiert** , um eine neue pfadbasierte Regel hinzuzufügen.

### <a name="step-3"></a>Schritt 3

Das Blatt **Add path-based rule** (Pfadbasierte Regel hinzufügen) verfügt über zwei Abschnitte. Im ersten Abschnitt haben Sie den Listener, den Namen der Regel und die Standardpfadeinstellungen festgelegt. Die Standardpfadeinstellungen gelten für Routen, die nicht unter benutzerdefinierte, pfadbasierte Regeln fallen. Im zweiten Abschnitt des Blatts **Add path-based rule** (Pfadbasierte Regel hinzufügen) definierten Sie die pfadbasierten Regeln.

**Basic Settings**

* **Name** : Dies ist der Anzeigename für die Regel, auf die Sie über das Portal zugreifen können.
* **Listener** : Dies ist der Listener, der für die Regel verwendet wird.
* **Default backend pool** (Standard-Back-End-Pool): Dies ist die Einstellung, die das Back-End für die Standardregel festlegt.
* **Default HTTP settings** (HTTP-Standardeinstellungen): Dies ist die Einstellung, die die HTTP-Einstellungen für die Standardregel festlegt.

**Pfadbasierte Regeln**

* **Name** : Dies ist der Anzeigename der pfadbasierten Regel.
* **Pfade** : Diese Einstellung legt den Pfad fest, nach dem die Regel beim Weiterleiten von Datenverkehr sucht.
* **Back-End-Pool** : Dies ist die Einstellung, die das Back-End für die Regel festlegt.
* **HTTP-Einstellung** : Dies ist die Einstellung, die die HTTP-Einstellungen für die Regel festlegt.

> [!IMPORTANT]
> Pfade: Die Liste der abzustimmenden Pfadmuster. Jedes muss mit „/“ beginnen, und ein „\*“ ist nur am Ende zulässig. Gültige Beispiele sind „/xyz“, „/xyz*“ oder „/xyz/“*.  

![Hinzufügen eines Blatts mit pfadbasierten Regeln mit bereitgestellten Informationen][2]

Das Hinzufügen einer pfadbasierten Regel zu einem bestehenden Application Gateway ist über das Portal ein einfacher Prozess. Nachdem eine pfadbasierte Regel erstellt wurde, kann sie bearbeitet werden, damit zusätzliche Regeln einfach hinzugefügt werden können. 

![Hinzufügen zusätzlicher pfadbasierter Regeln][3]

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren der SSL-Auslagerung mit Azure Application Gateway finden Sie unter [Konfigurieren der SSL-Auslagerung](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png



<!--HONumber=Dec16_HO3-->


