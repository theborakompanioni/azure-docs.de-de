---
title: "Benutzerdefinierte Einstellungen für App Service-Umgebungen"
description: "Benutzerdefinierte Konfigurationseinstellungen für App Service-Umgebungen"
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 1d1d85f3-6cc6-4d57-ae1a-5b37c642d812
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: stefsch
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 687475fae0c90713c15e8abbb92b71059eae81c0
ms.lasthandoff: 03/28/2017


---
# <a name="custom-configuration-settings-for-app-service-environments"></a>Benutzerdefinierte Konfigurationseinstellungen für App Service-Umgebungen
## <a name="overview"></a>Übersicht
Da App Service-Umgebungen isoliert für einen einzelnen Kunden eingerichtet werden, gibt es bestimmte Konfigurationseinstellungen, die exklusiv auf eine App Service-Umgebung angewendet werden können. In diesem Artikel werden die verschiedenen Anpassungen dokumentiert, die für App Service-Umgebungen verfügbar sind.

Informationen zum Erstellen einer App Service-Umgebung finden Sie [hier](app-service-web-how-to-create-an-app-service-environment.md).

Sie können Anpassungen der App Service-Umgebung mithilfe eines Arrays im neuen Attribut **clusterSettings** speichern. Dieses Attribut befindet sich im Wörterbuch „Eigenschaften“ der Azure Resource Manager-Entität *hostingEnvironments* .

Der folgende gekürzte Codeausschnitt aus einer Resource Manager-Vorlage zeigt das Attribut **clusterSettings** :

    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

Das Attribut **clusterSettings** kann in eine Resource Manager-Vorlage eingefügt werden, um die App Service-Umgebung zu aktualisieren.

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a>Aktualisieren einer App Service-Umgebung mit dem Azure-Ressourcen-Explorer
Alternativ dazu können Sie die App Service-Umgebung mit dem [Azure-Ressourcen-Explorer](https://resources.azure.com)aktualisieren.  

1. Navigieren Sie im Ressourcen-Explorer zum Knoten für die App Service-Umgebung (**subscriptions** > **resourceGroups** > **providers** > **Microsoft.Web** > **hostingEnvironments**). Klicken Sie dann auf die App Service-Umgebung, die aktualisiert werden soll.
2. Klicken Sie im rechten Bereich in der oberen Symbolleiste auf **Lesen/Schreiben** , um eine interaktive Bearbeitung im Ressourcen-Explorer zuzulassen.  
3. Klicken Sie dann auf die blaue Schaltfläche **Bearbeiten** , um die Bearbeitung der Resource Manager-Vorlage zu ermöglichen.
4. Scrollen Sie zum Ende des rechten Bereichs. Das Attribut **clusterSettings** wird ganz unten angezeigt. Sie können entweder einen Attributwert eingeben oder den vorhandenen Wert aktualisieren.
5. Geben Sie das Array der Konfigurationswerte ein, die Sie für das Attribut **clusterSettings** verwenden möchten. Alternativ können Sie die Werte auch kopieren und einfügen.  
6. Klicken Sie dann oben im rechten Bereich auf die grüne Schaltfläche **PUT** , um einen Commit für die Änderungen an der App Service-Umgebung auszuführen.

Unabhängig davon, wie Sie die Änderung übermitteln, dauert es ungefähr 30 Minuten, multipliziert mit der Anzahl der Front-Ends in der App Service-Umgebung, bis die Änderung wirksam wird.
Wenn eine App Service-Umgebung beispielsweise über vier Front-Ends verfügt, dauert es etwa zwei Stunden, bis die Konfigurationsaktualisierung abgeschlossen ist. Während die Konfigurationsänderung angewendet wird, können keine weiteren Skalierungsvorgänge oder Konfigurationsänderungen in der App Service-Umgebung ausgeführt werden.

## <a name="disable-tls-10"></a>Deaktivieren von TLS 1.0
Eine Frage, die häufig von Kunden gestellt wird – insbesondere dann, wenn die Kunden PCI-Compliance-Audits durchführen – ist, wie TLS 1.0 für ihre Apps explizit deaktiviert werden kann.

TLS 1.0 kann mit dem folgenden **clusterSettings** -Eintrag deaktiviert werden:

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>Ändern der Reihenfolge der TLS-Verschlüsselungssammlung
Eine weitere Frage von Kunden lautet, ob sie die Liste der Verschlüsselungsverfahren ändern können, die von ihrem Server ausgehandelt wird. Dies ist, wie im Folgenden gezeigt, durch Ändern der Einstellung **clusterSettings** möglich. Die Liste der verfügbaren Verschlüsselungssammlungen kann aus [diesem MSDN-Artikel](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx) abgerufen werden.

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [!WARNING]
> Wenn für die Verschlüsselungssammlung falsche Werte festgelegt werden, die SChannel nicht verstehen kann, funktioniert die TLS-Kommunikation mit dem Server ggf. nicht mehr. In diesem Fall müssen Sie den Eintrag *FrontEndSSLCipherSuiteOrder* aus **clusterSettings** entfernen und die aktualisierte Resource Manager-Vorlage übermitteln, um die Standardeinstellungen der Verschlüsselungssammlung wiederherzustellen.  Verwenden Sie diese Funktion umsichtig.
> 
> 

## <a name="get-started"></a>Erste Schritte
Die Azure-Website mit Resource Manager-Schnellstartvorlagen umfasst eine Vorlage mit der Basisdefinition zum [Erstellen einer App Service-Umgebung](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).

<!-- LINKS -->

<!-- IMAGES -->

