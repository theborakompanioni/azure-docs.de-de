---
title: "Erstellen einer pfadbasierten Regel für ein Anwendungsgateway (Azure-Portal) | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie eine pfadbasierte Regel für ein Anwendungsgateway über das Azure-Portal erstellen."
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 87bd93bc-e1a6-45db-a226-555948f1feb7
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/03/2017
ms.author: davidmu
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: f90813c0150d17f301f243c534d77ffd3da297b1
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-azure-portal"></a>Erstellen einer pfadbasierten Regel für ein Anwendungsgateway über das Azure-Portal

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-create-url-route-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-url-route-arm-ps.md)
> * [Azure CLI 2.0](application-gateway-create-url-route-cli.md)

Mit Routing auf URL-Pfadbasis ordnen Sie Routen basierend auf dem URL-Pfad der HTTP-Anforderung zu. Es wird überprüft, ob eine Route zu einem Back-End-Serverpool für die URL konfiguriert ist, die im Anwendungsgateway aufgeführt wird, und der Netzwerkdatenverkehr wird dann an den definierten Pool gesendet. Ein gängiges Szenario für Routing auf URL-Pfadbasis ist der Lastenausgleich von Anforderungen für verschiedene Inhaltstypen auf verschiedene Back-End-Serverpools.

Anwendungsgateways verfügen über zwei Regeltypen: Basisregeln und Regeln auf URL-Pfadbasis. Der Basisregeltyp stellt einen Roundrobin-Dienst für die Back-End-Pools bereit. Pfadbasierte Regeln nutzen bei der Auswahl des entsprechenden Back-End-Pools zusätzlich zur Roundrobin-Verteilung das Pfadmuster der Anforderungs-URL.

## <a name="scenario"></a>Szenario

Das folgende Szenario erstellt eine pfadbasierte Regel in einem vorhandenen Anwendungsgateway.
Für dieses Szenario wird davon ausgegangen, dass Sie die Schritte unter [Erstellen eines Anwendungsgateways über das Portal](application-gateway-create-gateway-portal.md) bereits ausgeführt haben.

![URL-Route][scenario]

## <a name="createrule"></a>Erstellen der pfadbasierten Regel

Für eine pfadbasierte Regel ist ein eigener Listener erforderlich. Stellen Sie vor dem Erstellen der Regel sicher, dass ein Listener zur Verfügung steht.

### <a name="step-1"></a>Schritt 1

Wechseln Sie zum [Azure-Portal](http://portal.azure.com), und wählen Sie ein vorhandenes Anwendungsgateway aus. Klicken Sie auf **Regeln**.

![Übersicht über Application Gateway][1]

### <a name="step-2"></a>Schritt 2

Klicken Sie auf **Pfadbasiert**, um eine neue pfadbasierte Regel hinzuzufügen.

### <a name="step-3"></a>Schritt 3

Das Blatt **Add path-based rule** (Pfadbasierte Regel hinzufügen) verfügt über zwei Abschnitte. Im ersten Abschnitt haben Sie den Listener, den Namen der Regel und die Standardpfadeinstellungen festgelegt. Die Standardpfadeinstellungen gelten für Routen, die nicht unter benutzerdefinierte pfadbasierte Regeln fallen. Im zweiten Abschnitt des Blatts **Add path-based rule** (Pfadbasierte Regel hinzufügen) definierten Sie die pfadbasierten Regeln.

**Einstellungen für Basisregeln**

* **Name**: Ein Anzeigename für die Regel, auf die Sie über das Portal zugreifen können
* **Listener**: Der Listener, der für die Regel verwendet wird
* **Standard-Back-End-Pool**: Das Back-End, das für die Standardregel verwendet werden soll
* **HTTP-Standardeinstellungen**: Die HTTP-Einstellungen, die für die Standardregel verwendet werden sollen

**Einstellungen für pfadbasierte Regeln**

* **Name**: Ein Anzeigename für die pfadbasierte Regel
* **Pfade**: Der Pfad, nach dem die Regel beim Weiterleiten von Datenverkehr sucht
* **Back-End-Pool**: Das Back-End, das für die Regel verwendet werden soll
* **HTTP-Einstellung**: Die HTTP-Einstellungen, die für die Regel verwendet werden sollen

> [!IMPORTANT]
> Die Einstellung für **Pfade** ist die Liste der abzustimmenden Pfadmuster. Jeder Pfad muss mit einem Schrägstrich beginnen, und ein Sternchen ist nur am Ende zulässig. Zulässige Beispiele: /xyz, /xyz und /xyz/**.  

![Hinzufügen eines Blatts mit pfadbasierten Regeln mit bereitgestellten Informationen][2]

Über das Azure-Portal können Sie einem bestehenden Anwendungsgateway ganz einfach eine pfadbasierte Regel hinzufügen. Nach der Erstellung einer pfadbasierten Regel können Sie sie bearbeiten, um weitere Regeln hinzuzufügen. 

![Hinzufügen zusätzlicher pfadbasierter Regeln][3]

Mit diesem Schritt wird eine pfadbasierte Route konfiguriert. Beachten Sie unbedingt, dass Anforderungen nicht neu geschrieben werden. Eingehende Anforderungen werden vom Anwendungsgateway untersucht und basierend auf dem URL-Muster an den entsprechenden Back-End-Pool gesendet.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren der SSL-Auslagerung mit Azure Application Gateway finden Sie unter [Konfigurieren eines Anwendungsgateways für die SSL-Auslagerung über das Azure-Portal](application-gateway-ssl-portal.md).

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png

