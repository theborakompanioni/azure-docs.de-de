<properties
   pageTitle="Konfigurieren eines Anwendungsgateways für die SSL-Auslagerung über das Portal | Microsoft Azure"
   description="Diese Seite enthält Anweisungen zum Erstellen eines Anwendungsgateways mit SSL-Auslagerung über das Portal."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# Konfigurieren eines Anwendungsgateways für die SSL-Auslagerung über das Portal

> [AZURE.SELECTOR]
-[Azure portal](application-gateway-ssl-portal.md)
-[Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
-[Azure Classic PowerShell](application-gateway-ssl.md)

Azure Application Gateway kann so konfiguriert werden, dass damit die Secure Sockets Layer-Sitzung (SSL) auf dem Gateway beendet wird. Auf diese Weise wird die aufwändige SSL-Entschlüsselung in der Webfarm vermieden. Die SSL-Auslagerung vereinfacht zudem die Einrichtung und Verwaltung der Webanwendung auf dem Front-End-Server.

## Szenario

Das folgende Szenario führt Sie durch die Konfiguration der SSL-Auslagerung in einem vorhandenen Anwendungsgateway. Für dieses Szenario wird davon ausgegangen, dass Sie die Schritte unter [Erstellen eines Anwendungsgateways](application-gateway-create-gateway-portal.md) bereits durchgeführt haben.

## Voraussetzungen

Zum Konfigurieren der SSL-Auslagerung mit einem Anwendungsgateway ist ein Zertifikat erforderlich. Dieses Zertifikat wird in das Anwendungsgateway geladen und zum Ver- und Entschlüsseln des über SSL gesendeten Datenverkehrs verwendet. Das Zertifikat muss im PFX-Format (privater Informationsaustausch) vorliegen. In diesem Dateiformat kann der private Schlüssel exportiert werden, was erforderlich ist, damit das Anwendungsgateway die Ver- und Entschlüsselung des Datenverkehrs durchführen kann.

## Hinzufügen eines HTTPS-Listeners

Der HTTPS-Listener sucht basierend auf seiner Konfiguration nach Datenverkehr und hilft dabei, den Datenverkehr an die Back-End-Pools zu leiten.

### Schritt 1

Navigieren Sie zum Azure-Portal, und wählen Sie ein vorhandenes Anwendungsgateway aus.

![Übersichtsblatt für Anwendungsgateway][1]

### Schritt 2

Klicken Sie auf „Listener“ und dann auf die Schaltfläche „Hinzufügen“, um einen Listener hinzuzufügen.

### Schritt 3

Stellen Sie die erforderlichen Informationen für den Listener bereit, und laden Sie das PFX-Zertifikat hoch. Wenn Sie fertig sind, klicken Sie auf „OK“.

**Name**: Dieser Wert ist der Anzeigename des Listeners.

**Front-End-IP-Konfiguration**: Dieser Wert gibt die IP-Konfiguration des Front-Ends an, die für den Listener verwendet wird.

**Front-End-Port (Name/Port)**: Der im Front-End des Anwendungsgateways verwendete Anzeigename für den Port und der tatsächlich verwendete Port.

**Protokoll**: Ein Schalter, mit dem bestimmt wird, ob für das Front-End HTTP oder HTTPS verwendet werden soll.

**Zertifikat (Name/Kennwort)**: Wenn die SSL-Auslagerung verwendet wird, ist für diese Einstellung ein PFX-Zertifikat sowie der zugehörige Anzeigename und das zugehörige Kennwort erforderlich.

![Blatt zum Hinzufügen eines Listeners][2]

## Erstellen einer Regel und Verknüpfen der Regel mit dem Listener

Der Listener wurde erstellt. Jetzt muss eine Regel erstellt werden, um den Datenverkehr vom Listener zu verarbeiten.

### Schritt 1

Klicken Sie für das Anwendungsgateway auf **Regeln**, und klicken Sie dann auf „Hinzufügen“.

![Blatt mit Regeln des Anwendungsgateways][3]

### Schritt 2

Geben Sie auf dem Blatt **Einfache Regel hinzufügen** den Anzeigenamen für die Regel ein, und fügen Sie den im vorherigen Schritt erstellten Listener hinzu. Wählen Sie den geeigneten Back-End-Pool und die geeigneten HTTP-Einstellungen aus, und klicken Sie auf **OK**.

![Fenster mit HTTPS-Einstellungen][4]

Die Einstellungen sind jetzt im Anwendungsgateway gespeichert. Es kann eine Weile dauern, bis die Einstellungen gespeichert sind und im Portal oder über PowerShell angezeigt werden können. Sobald die Speicherung abgeschlossen ist, übernimmt das Anwendungsgateway die Ver- und Entschlüsselung des Datenverkehrs. Sämtlicher Datenverkehr zwischen dem Anwendungsgateway und den Back-End-Webservern werden über HTTP verarbeitet. Jegliche Kommunikation an den Client wird, sofern sie über HTTPS initiiert wurde, verschlüsselt an den Client zurückgegeben.

## Nächste Schritte

Informationen zum Konfigurieren eines benutzerdefinierten Integritätstests mit Azure Application Gateway finden Sie unter [Erstellen eines benutzerdefinierten Integritätstests](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png

<!---HONumber=AcomDC_0921_2016-->