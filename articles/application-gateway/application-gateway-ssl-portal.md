---
title: "Konfigurieren der SSL-Auslagerung – Azure Application Gateway (Azure-Portal) | Microsoft-Dokumentation"
description: "Diese Seite enthält Anweisungen zum Erstellen eines Anwendungsgateways mit SSL-Auslagerung über das Portal."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 8373379a-a26a-45d2-aa62-dd282298eff3
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 0bec803e4b49f3ae53f2cc3be6b9cb2d256fe5ea
ms.openlocfilehash: a9cb2d921d1be226661311d91367b2b6f44fa0dc
ms.lasthandoff: 03/24/2017


---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-portal"></a>Konfigurieren eines Anwendungsgateways für die SSL-Auslagerung über das Portal

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Klassische Azure PowerShell](application-gateway-ssl.md)

Azure Application Gateway kann so konfiguriert werden, dass damit die Secure Sockets Layer-Sitzung (SSL) auf dem Gateway beendet wird. Auf diese Weise wird die aufwändige SSL-Entschlüsselung in der Webfarm vermieden. Die SSL-Auslagerung vereinfacht zudem die Einrichtung und Verwaltung der Webanwendung auf dem Front-End-Server.

## <a name="scenario"></a>Szenario

Das folgende Szenario führt Sie durch die Konfiguration der SSL-Auslagerung in einem vorhandenen Anwendungsgateway. Für dieses Szenario wird davon ausgegangen, dass Sie die Schritte unter [Erstellen eines Anwendungsgateways](application-gateway-create-gateway-portal.md)bereits durchgeführt haben.

## <a name="before-you-begin"></a>Voraussetzungen

Zum Konfigurieren der SSL-Auslagerung mit einem Anwendungsgateway ist ein Zertifikat erforderlich. Dieses Zertifikat wird in das Anwendungsgateway geladen und zum Ver- und Entschlüsseln des über SSL gesendeten Datenverkehrs verwendet. Das Zertifikat muss im PFX-Format (privater Informationsaustausch) vorliegen. In diesem Dateiformat kann der private Schlüssel exportiert werden, was erforderlich ist, damit das Anwendungsgateway die Ver- und Entschlüsselung des Datenverkehrs durchführen kann.

## <a name="add-an-https-listener"></a>Hinzufügen eines HTTPS-Listeners

Der HTTPS-Listener sucht basierend auf seiner Konfiguration nach Datenverkehr und hilft dabei, den Datenverkehr an die Back-End-Pools zu leiten.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zum Azure-Portal, und wählen Sie ein vorhandenes Anwendungsgateway aus.

### <a name="step-2"></a>Schritt 2

Klicken Sie auf „Listener“ und dann auf die Schaltfläche „Hinzufügen“, um einen Listener hinzuzufügen.

![Übersichtsblatt für Anwendungsgateway][1]

### <a name="step-3"></a>Schritt 3

Stellen Sie die erforderlichen Informationen für den Listener bereit, und laden Sie das PFX-Zertifikat hoch. Wenn Sie fertig sind, klicken Sie auf „OK“.

**Name** : Dieser Wert ist der Anzeigename des Listeners.

**Front-End-IP-Konfiguration** : Dieser Wert gibt die IP-Konfiguration des Front-Ends an, die für den Listener verwendet wird.

**Front-End-Port (Name/Port)** : Der im Front-End des Anwendungsgateways verwendete Anzeigename für den Port und der tatsächlich verwendete Port.

**Protokoll** : Ein Schalter, mit dem bestimmt wird, ob für das Front-End HTTP oder HTTPS verwendet werden soll.

**Zertifikat (Name/Kennwort)** : Wenn die SSL-Auslagerung verwendet wird, ist für diese Einstellung ein PFX-Zertifikat sowie der zugehörige Anzeigename und das zugehörige Kennwort erforderlich.

![Blatt zum Hinzufügen eines Listeners][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Erstellen einer Regel und Verknüpfen der Regel mit dem Listener

Der Listener wurde erstellt. Jetzt muss eine Regel erstellt werden, um den Datenverkehr vom Listener zu verarbeiten. Regeln definieren, wie Datenverkehr an die Back-End-Pools weitergeleitet wird. Sie basieren auf mehreren Konfigurationseinstellungen, z.B. ob die cookiebasierte Sitzungsaffinität verwendet wird, sowie auf dem Protokoll, dem Port und Integritätstests.

### <a name="step-1"></a>Schritt 1

Klicken Sie für das Anwendungsgateway auf **Regeln** , und klicken Sie dann auf „Hinzufügen“.

![Blatt mit Regeln des Anwendungsgateways][3]

### <a name="step-2"></a>Schritt 2

Geben Sie auf dem Blatt **Einfache Regel hinzufügen** den Anzeigenamen für die Regel ein, und fügen Sie den im vorherigen Schritt erstellten Listener hinzu. Wählen Sie den geeigneten Back-End-Pool und die geeigneten HTTP-Einstellungen aus, und klicken Sie auf **OK**

![Fenster mit HTTPS-Einstellungen][4]

Die Einstellungen sind jetzt im Anwendungsgateway gespeichert. Es kann eine Weile dauern, bis die Einstellungen gespeichert sind und im Portal oder über PowerShell angezeigt werden können. Sobald die Speicherung abgeschlossen ist, übernimmt das Anwendungsgateway die Ver- und Entschlüsselung des Datenverkehrs. Sämtlicher Datenverkehr zwischen dem Anwendungsgateway und den Back-End-Webservern werden über HTTP verarbeitet. Jegliche Kommunikation an den Client wird, sofern sie über HTTPS initiiert wurde, verschlüsselt an den Client zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren eines benutzerdefinierten Integritätstests mit Azure Application Gateway finden Sie unter [Erstellen eines benutzerdefinierten Integritätstests](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png

