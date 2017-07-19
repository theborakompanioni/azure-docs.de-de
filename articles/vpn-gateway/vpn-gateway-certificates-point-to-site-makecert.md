---
title: "Generieren und Exportieren von Zertifikaten für Point-to-Site: MakeCert: Azure | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Schritte zum Erstellen eines selbstsignierten Stammzertifikats, zum Exportieren des öffentlichen Schlüssels und zum Generieren von Clientzertifikaten mit MakeCert."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: bb61222ae01d1613ec27bb016ff1f94bcdaf8935
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-makecert"></a>Generieren und Exportieren von Zertifikaten für Point-to-Site-Verbindungen mithilfe von MakeCert

> [!NOTE]
> Verwenden Sie die Anweisungen in diesem Artikel nur dann zum Generieren von Zertifikaten, wenn Sie keinen Zugriff auf einem Computer unter Windows 10 haben. Verwenden Sie andernfalls den Artikel [Generieren und Exportieren von Zertifikaten für Punkt-zu-Standort-Verbindungen mithilfe von PowerShell](vpn-gateway-certificates-point-to-site.md).
>

Punkt-zu-Standort-Verbindungen verwenden Zertifikate zur Authentifizierung. In diesem Artikel erfahren Sie, wie Sie mithilfe von MakeCert ein selbstsigniertes Stammzertifikat erstellen und Clientzertifikate generieren. Wenn Sie Informationen zu einzelnen Point-to-Site-Konfigurationsschritten benötigen (beispielsweise zum Hochladen von Stammzertifikaten), wählen Sie in der folgenden Liste einen der Artikel zum Konfigurieren einer Point-to-Site-Verbindung aus:

> [!div class="op_single_selector"]
> * [Erstellen selbstsignierter Zertifikate – PowerShell](vpn-gateway-certificates-point-to-site.md)
> * [Erstellen selbstsignierter Zertifikate – MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)
> * [Konfigurieren einer Punkt-zu-Standort-Verbindung – Resource Manager – Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Konfigurieren einer Point-to-Site-Verbindung – Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Konfigurieren einer Punkt-zu-Standort-Verbindung – klassisch – Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Wir empfehlen, Zertifikate unter Verwendung der [Windows 10-PowerShell-Schritte](vpn-gateway-certificates-point-to-site.md) zu erstellen. Diese MakeCert-Anweisungen werden lediglich als optionale Methode bereitgestellt. Die Zertifikate können jedoch unabhängig von der verwendeten Generierungsmethode unter jedem [unterstützten Clientbetriebssystem](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) installiert werden. Für MakeCert gelten allerdings folgende Einschränkungen:

* Mit MakeCert können nur SHA-1-Zertifikate, aber keine SHA-2-Zertifikate generiert werden. SHA-1-Zertifikate sind zwar für Point-to-Point-Verbindungen noch gültig, SHA-1 verwendet jedoch einen weniger sicheren Verschlüsselungshash als SHA-2.
* MakeCert ist veraltet. Das bedeutet, dass dieses Tool jederzeit entfernt werden kann. Auf Zertifikate, die Sie bereits mithilfe von MakeCert generiert haben, hat die Entfernung von MakeCert keine Auswirkungen. MakeCert wird nur zum Generieren der Zertifikate verwendet, nicht als Überprüfungsmechanismus.

## <a name="rootcert"></a>Erstellen eines selbstsignierten Stammzertifikats

In den folgenden Schritten wird das Erstellen eines selbstsignierten Zertifikats mit MakeCert beschrieben. Diese Schritte gelten nicht spezifisch für ein Bereitstellungsmodell. Sie sind für das Ressourcen-Manager-Modell und das klassische Modell gültig.

1. Laden Sie [MakeCert](https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx) herunter, und installieren Sie es.
2. Nach der Installation befindet sich das Hilfsprogramm „makecert.exe“ üblicherweise im Verzeichnis „C:\Programme (x86)\Windows Kits\10\bin\<arch>“. Es kann jedoch auch an einem anderen Speicherort installiert worden sein. Öffnen Sie eine Eingabeaufforderung als Administrator, und navigieren Sie zum Speicherort des Hilfsprogramms MakeCert. Sie können das folgende Beispiel verwenden (muss an den korrekten Speicherort angepasst werden):

  ```cmd
  cd C:\Program Files (x86)\Windows Kits\10\bin\x64
  ```
3. Erstellen und installieren Sie ein Zertifikat im Speicher für persönliche Zertifikate auf dem Computer. Das folgende Beispiel erstellt eine entsprechende *CER* -Datei, die Sie beim Konfigurieren der Punkt-zu-Standort-Verbindung in Azure hochladen. Ersetzen Sie „P2SRootCert“ und „P2SRootCert.cer“ durch den Namen, den Sie für das Zertifikat verwenden möchten. Das Zertifikat befindet sich unter „Zertifikate - Aktueller Benutzer\Eigene Zertifikate\Zertifikate“.

  ```cmd
  makecert -sky exchange -r -n "CN=P2SRootCert" -pe -a sha1 -len 2048 -ss My
  ```

## <a name="cer"></a>Exportieren des öffentlichen Schlüssels (CER-Datei)

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

Die exportierte CER-Datei muss in Azure hochgeladen werden. Entsprechende Anweisungen finden Sie unter [Konfigurieren einer Punkt-zu-Standort-Verbindung](vpn-gateway-howto-point-to-site-resource-manager-portal.md#uploadfile). Informationen zum Hinzufügen eines zusätzlichen vertrauenswürdigen Stammzertifikats finden Sie in [diesem Abschnitt](vpn-gateway-howto-point-to-site-resource-manager-portal.md#add) des Artikels.

### <a name="export-the-self-signed-certificate-and-private-key-to-store-it-optional"></a>Exportieren des selbstsignierten Zertifikats und des privaten Schlüssels zum Speichern (optional)

Möglicherweise möchten Sie das selbstsignierte Stammzertifikat exportieren und sicher speichern. Bei Bedarf können Sie es später auf einem anderen Computer installieren und weitere Clientzertifikate generieren oder eine andere CER-Datei exportieren. Um das selbstsignierte Stammzertifikat als PFX-Datei zu exportieren, wählen Sie das Stammzertifikat aus, und verwenden Sie die gleichen Schritte wie zum [Exportieren eines Clientzertifikats](#clientexport).

## <a name="create-and-install-client-certificates"></a>Erstellen und Installieren von Clientzertifikaten

Das selbstsignierte Zertifikat wird nicht direkt auf dem Clientcomputer installiert. Sie müssen aus einem selbstsignierten Zertifikat ein Clientzertifikat generieren. Dieses Clientzertifikat exportieren und installieren Sie auf den Clientcomputern. Die folgenden Schritte gelten nicht für ein spezifisches Bereitstellungsmodell. Sie sind für das Ressourcen-Manager-Modell und das klassische Modell gültig.

### <a name="clientcert"></a>Generieren eines Clientzertifikats

Auf jedem Clientcomputer, der per Punkt-zu-Standort eine Verbindung mit einem VNet herstellt, muss ein Clientzertifikat installiert sein. Sie generieren ein Clientzertifikat aus dem selbstsignierten Stammzertifikat und exportieren und installieren es anschließend. Wenn das Clientzertifikat nicht installiert ist, tritt bei der Authentifizierung ein Fehler auf. 

Die folgenden Schritte führen Sie durch das Generieren eines Clientzertifikats aus einem selbstsignierten Stammzertifikat. Sie können mehrere Clientzertifikate aus demselben Stammzertifikat generieren. Wenn Sie mithilfe der folgenden Schritte Clientzertifikate generieren, wird das Clientzertifikat automatisch auf dem Computer installiert, mit dem Sie das Zertifikat generiert haben. Falls Sie ein Clientzertifikat auf einem anderen Clientcomputer installieren möchten, können Sie es exportieren.
 
1. Öffnen Sie auf dem Computer, den Sie zum Erstellen des selbstsignierten Zertifikats verwendet haben, eine Eingabeaufforderung als Administrator.
2. Ändern Sie das Beispiel, und führen Sie es aus, um ein Clientzertifikat zu generieren.
  * Ändern Sie *P2SRootCert* in den Namen des selbstsignierten Stammzertifikats, anhand dessen Sie das Clientzertifikat generieren. Stellen Sie sicher, dass Sie den Namen des Stammzertifikats verwenden. Dieses entspricht Ihrer Angabe für den Wert „CN=“ beim Erstellen des selbstsignierten Stammzertifikats.
  * Ändern Sie *P2SChildCert* in den Namen, den das Clientzertifikat bekommen soll, das Sie generieren möchten.

  Wenn Sie das folgende Beispiel ausführen, ohne es zu ändern, wird in Ihrem persönlichen Zertifikatspeicher ein Clientzertifikat mit dem Namen „P2SChildcert“ anhand des Stammzertifikats „P2SRootCert“ generiert.

  ```cmd
  makecert.exe -n "CN=P2SChildCert" -pe -sky exchange -m 96 -ss My -in "P2SRootCert" -is my -a sha1
  ```

### <a name="clientexport"></a>Exportieren eines Clientzertifikats

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

### <a name="install"></a>Installieren eines exportierten Clientzertifikats

[!INCLUDE [Install client certificate](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Setzen Sie die Punkt-zu-Standort-Konfiguration fort. 

* Schritte für das Bereitstellungsmodell **Resource Manager** finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung mit einem VNet über das Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
* Schritte für das **klassische** Bereitstellungsmodell finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung mit einem VNet über das Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md) (klassisch).

