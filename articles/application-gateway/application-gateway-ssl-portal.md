---
title: "Konfigurieren der SSL-Auslagerung – Azure Application Gateway (Azure-Portal) | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Anweisungen zum Erstellen eines Anwendungsgateways mit SSL-Auslagerung über das Azure-Portal."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 8373379a-a26a-45d2-aa62-dd282298eff3
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: davidmu
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 23b5a529e7ee7db5615340352fb68b2e64e45972
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-azure-portal"></a>Konfigurieren eines Anwendungsgateways für die SSL-Auslagerung über das Azure-Portal

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Klassische Azure PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Azure Application Gateway kann so konfiguriert werden, dass damit die Secure Sockets Layer-Sitzung (SSL) auf dem Gateway beendet wird. Auf diese Weise wird die aufwändige SSL-Entschlüsselung in der Webfarm vermieden. Die SSL-Auslagerung vereinfacht zudem die Einrichtung und Verwaltung der Webanwendung auf dem Front-End-Server.

## <a name="scenario"></a>Szenario

Das folgende Szenario führt Sie durch die Konfiguration der SSL-Auslagerung in einem vorhandenen Anwendungsgateway. Für dieses Szenario wird davon ausgegangen, dass Sie die Schritte unter [Erstellen eines Anwendungsgateways](application-gateway-create-gateway-portal.md) bereits durchgeführt haben.

## <a name="before-you-begin"></a>Voraussetzungen

Zum Konfigurieren der SSL-Auslagerung mit einem Anwendungsgateway ist ein Zertifikat erforderlich. Dieses Zertifikat wird in das Anwendungsgateway geladen und zum Ver- und Entschlüsseln des über SSL gesendeten Datenverkehrs verwendet. Das Zertifikat muss im PFX-Format (privater Informationsaustausch) vorliegen. In diesem Dateiformat können Sie den privaten Schlüssel exportieren. Das ist erforderlich, damit das Anwendungsgateway die Ver- und Entschlüsselung des Datenverkehrs durchführen kann.

## <a name="add-an-https-listener"></a>Hinzufügen eines HTTPS-Listeners

Der HTTPS-Listener sucht basierend auf seiner Konfiguration nach Datenverkehr und hilft dabei, den Datenverkehr an die Back-End-Pools zu leiten. Gehen Sie folgendermaßen vor, um einen HTTPS-Listener hinzuzufügen:

   1. Wechseln Sie zum Azure-Portal, und wählen Sie ein vorhandenes Anwendungsgateway aus.

   2. Wählen Sie **Listener** und dann die Schaltfläche **Hinzufügen** aus, um einen Listener hinzuzufügen.

   ![Bereich mit der Übersicht über Application Gateway][1]


   3. Stellen Sie die folgenden erforderlichen Informationen für den Listener bereit, und laden Sie das PFX-Zertifikat hoch:
      - **Name**: Der Anzeigename des Listeners.

      - **Front-End-IP-Konfiguration**: Die IP-Konfiguration des Front-Ends, die für den Listener verwendet wird.

      - **Front-End-Port (Name/Port)**: Der im Front-End des Anwendungsgateways verwendete Anzeigename für den Port und der tatsächlich verwendete Port.

      - **Protokoll**: Ein Schalter, mit dem bestimmt wird, ob für das Front-End HTTP oder HTTPS verwendet werden soll.

      - **Zertifikat (Name/Kennwort)**: Wenn die SSL-Auslagerung verwendet wird, ist für diese Einstellung ein PFX-Zertifikat erforderlich. Ein Anzeigename und ein Kennwort sind ebenfalls erforderlich.

   4. Klicken Sie auf **OK**.

![Bereich zum Hinzufügen eines Listeners][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Erstellen einer Regel und Verknüpfen der Regel mit dem Listener

Der Listener wurde erstellt. Als Nächstes erstellen Sie eine Regel, um den Datenverkehr vom Listener zu verarbeiten. Regeln definieren, wie Datenverkehr basierend auf mehreren Konfigurationseinstellungen an die Back-End-Adresspools weitergeleitet wird. Diese Einstellungen umfassen das Protokoll, den Port und die Integritätstests sowie die Angabe, ob cookiebasierte Sitzungsaffinität verwendet wird. Gehen Sie folgendermaßen vor, um eine Regel zu erstellen und dem Listener zuzuordnen:


   1. Wählen Sie für das Anwendungsgateway **Regeln** und dann **Hinzufügen** aus.

   ![Bereich mit Regeln für das Anwendungsgateway][3]


   2. Geben Sie unter **Einfache Regel hinzufügen** einen Anzeigenamen für die Regel in das Feld **Name** ein, und wählen Sie dann den **Listener** aus, den Sie im vorherigen Schritt erstellt haben. Wählen Sie den geeigneten **Back-End-Pool** sowie die geeignete **HTTP-Einstellung** und dann **OK** aus.

   ![Fenster mit HTTPS-Einstellungen][4]

Die Einstellungen sind jetzt im Anwendungsgateway gespeichert. Es kann eine Weile dauern, bis die Einstellungen gespeichert sind und im Portal oder über PowerShell angezeigt werden können. Nach dem Speichern übernimmt das Anwendungsgateway die Ver- und Entschlüsselung des Datenverkehrs. Sämtlicher Datenverkehr zwischen dem Anwendungsgateway und den Back-End-Webservern werden über HTTP verarbeitet. Jegliche Kommunikation an den Client wird, sofern sie über HTTPS initiiert wurde, verschlüsselt an den Client zurückgegeben.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren eines benutzerdefinierten Integritätstests mit Azure Application Gateway finden Sie unter [Erstellen eines benutzerdefinierten Integritätstests](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png


