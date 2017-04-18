---
title: Herstellen einer Verbindung zwischen einer Web-App und einem Content Delivery Network | Microsoft-Dokumentation
description: "Stellen Sie eine Verbindung zwischen einer Web-App und einem Content Delivery Network her, um Ihre statischen Dateien von Edgeknoten aus zu übermitteln."
services: app-service
author: syntaxc4
ms.author: cfowler
ms.date: 04/03/2017
ms.topic: hero-article
ms.service: app-service-web
manager: erikre
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: a12eab1f7bc4177f659771d58a58f749507c994c
ms.lasthandoff: 04/12/2017

---
# <a name="connect-a-web-app-to-a-content-delivery-network"></a>Herstellen einer Verbindung zwischen einer Web-App und einem Content Delivery Network

In diesem Tutorial erstellen Sie ein Azure CDN-Profil und einen Azure CDN-Endpunkt, um die statischen Dateien aus Ihrer Web-App über die Azure CDN-POP-Standorte bereitzustellen.

> [!TIP]
> Sehen Sie sich unter [Azure CDN-POP-Standorte](https://docs.microsoft.com/en-us/azure/cdn/cdn-pop-locations) die aktuelle Liste an.
>

## <a name="step-1---login-to-azure-portal"></a>Schritt 1: Anmelden beim Azure-Portal

Navigieren Sie in Ihrem bevorzugten Browser zum [Azure-Portal](https://portal.azure.com).

## <a name="step-2---create-a-cdn-profile"></a>Schritt 2: Erstellen eines CDN-Profils

Klicken Sie im linken Navigationsbereich auf die Schaltfläche `+ New` und anschließend auf **Web und mobil**. Wählen Sie unter der Kategorie „Web und mobil“ die Option **CDN** aus.

Geben Sie **Name**, **Standort**, **Ressourcengruppe** und **Tarif** an, und klicken Sie anschließend auf **Erstellen**.

Öffnen Sie im linken Navigationsbereich den Ressourcengruppenhub, und wählen Sie **myResourceGroup** aus. Wählen Sie in der Ressourcenliste die Option **myCDNProfile** aus.

![azure-cdn-profile-created](media/app-service-web-tutorial-content-delivery-network/azure-cdn-profile-created.png)

## <a name="step-3---create-a-cdn-endpoint"></a>Schritt 3: Erstellen eines CDN-Endpunkts

Klicken Sie in den Befehlen neben dem Suchfeld auf `+ Endpoint`, um das Blatt für die Endpunkterstellung zu öffnen.

Geben Sie **Name**, **Ursprungstyp** und **Ursprungshostname** an, und klicken Sie anschließend auf **Hinzufügen**.

Der Content Delivery Network-Endpunkt wird erstellt. Nach Abschluss dieses Vorgangs wird der Status auf **Wird ausgeführt** aktualisiert.

![azure-cdn-endpoint-created](media/app-service-web-tutorial-content-delivery-network/azure-cdn-endpoint-created.png)

## <a name="step-4---leveraging-cdn"></a>Schritt 4: Verwenden von CDN

Nachdem der Endpunkt nun ausgeführt wird, überprüfen wir als Nächstes, ob der Inhalt auf dem POP-Server verfügbar ist. Hierzu navigieren wir zu einer statischen Datei auf dem Webserver und ändern anschließend den Hostnamen von `http://<app_name>.azurewebsites.net/path/to-static-file` in `http://<endpoint_name>.azureedge.net/path/to-static-file`.

![app-service-web-url-to-cdn-endpoint-url](media/app-service-web-tutorial-content-delivery-network/app-service-web-url-to-cdn-endpoint-url.png)

## <a name="next-steps"></a>Nächste Schritte


