---
title: "Erstellen und Exportieren von Zertifikaten für Punkt-zu-Standort-Verbindungen: PowerShell: Azure | Microsoft-Dokumentation"
description: "Dieser Artikel enthält die Schritte zum Erstellen selbstsignierter Stammzertifikate, Exportieren des öffentlichen Schlüssels und Generieren von Clientzertifikaten mit PowerShell unter Windows 10."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 27b99f7c-50dc-4f88-8a6e-d60080819a43
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: 1bfcb8683669770d6dd927cbde53a683bbc1d56e
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-powershell"></a>Generieren und Exportieren von Zertifikaten für Punkt-zu-Standort-Verbindungen mithilfe von PowerShell

In diesem Artikel wird beschrieben, wie Sie ein selbstsigniertes Stammzertifikat erstellen und Clientzertifikate generieren. Der Artikel enthält keine Konfigurationsanleitung für Point-to-Site-Verbindungen und auch keine häufig gestellten Fragen dazu. Diese Informationen finden Sie in einem der folgenden Artikel über das Konfigurieren von Punkt-zu-Standort-Verbindungen:

> [!div class="op_single_selector"]
> * [Erstellen selbstsignierter Zertifikate – PowerShell](vpn-gateway-certificates-point-to-site.md)
> * [Erstellen selbstsignierter Zertifikate – MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)
> * [Konfigurieren einer Punkt-zu-Standort-Verbindung – Resource Manager – Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Konfigurieren einer Point-to-Site-Verbindung – Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Konfigurieren einer Punkt-zu-Standort-Verbindung – klassisch – Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Punkt-zu-Standort-Verbindungen verwenden Zertifikate zur Authentifizierung. Wenn Sie eine Punkt-zu-Standort-Verbindung konfigurieren, müssen Sie den öffentlichen Schlüssel (CER-Datei) eines Stammzertifikats in Azure hochladen. Darüber hinaus müssen Clientzertifikate auf der Grundlage des Stammzertifikats generiert und auf jedem Clientcomputer installiert werden, der eine Verbindung mit dem VNet herstellt. Über das Clientzertifikat kann sich der Client authentifizieren.


> [!NOTE]
> Sie müssen die in diesem Artikel beschriebenen Schritte auf einem Computer unter Windows 10 ausführen. Die zum Generieren von Zertifikaten erforderlichen PowerShell-Cmdlets sind Teil des Windows 10-Betriebssystems und funktionieren nicht in anderen Versionen von Windows. Sie benötigen diese Cmdlets nur zum Generieren der Zertifikate. Nachdem die Zertifikate generiert wurden, können sie unter jedem unterstützten Clientbetriebssystem installiert werden. Wenn Sie keinen Zugriff auf einen Windows 10-Computer haben, können Sie Zertifikate mit MakeCert generieren. Die mit MakeCert generierten Zertifikate sind zwar mit Punkt-zu-Standort-Verbindungen kompatibel, aber keine SHA-2-Zertifikate. MakeCert-spezifische Anweisungen finden Sie unter [Erstellen von Zertifikaten mit MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Die von Ihnen mit PowerShell oder MakeCert erstellten Zertifikate können nicht nur unter dem zum Erstellen der Zertifikate verwendeten Betriebssystem, sondern unter jedem [unterstützten Clientbetriebssystem](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) installiert werden.
> 
>

## <a name="rootcert"></a>Erstellen eines selbstsignierten Stammzertifikats

In den folgenden Schritten wird das Erstellen eines selbstsignierten Stammzertifikats mit PowerShell unter Windows 10 beschrieben. Die in den folgenden Schritten verwendeten Cmdlets und Parameter sind Teil des Windows 10-Betriebssystems, nicht Teil einer PowerShell-Version. Das bedeutet nicht, dass die von Ihnen erstellten Zertifikate nur unter Windows 10 installiert werden können. Sie können auf jedem unterstützten Client installiert werden. Weitere Informationen zu unterstützten Clients finden Sie unter [Point-to-Site – Häufig gestellte Fragen](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq).

1. Öffnen Sie auf einem Computer unter Windows 10 eine Windows PowerShell-Konsole mit erhöhten Rechten.
2. Verwenden Sie das folgende Beispiel, um das selbstsignierte Stammzertifikat zu erstellen. Das folgende Beispiel erstellt ein selbstsigniertes Stammzertifikat mit dem Namen P2SRootCert, das automatisch in „Certificates-Current User\Personal\Certificates“ installiert wird. Sie können das Zertifikat anzeigen, indem Sie *certmgr.msc* öffnen oder die Option *Benutzerzertifikate verwalten* auswählen.

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

### <a name="cer"></a>Exportieren des öffentlichen Schlüssels (CER-Datei)

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

Die exportierte CER-Datei muss in Azure hochgeladen werden. Entsprechende Anweisungen finden Sie unter [Konfigurieren einer Punkt-zu-Standort-Verbindung](vpn-gateway-howto-point-to-site-rm-ps.md#upload).

### <a name="export-the-self-signed-root-certificate-and-public-key-to-store-it-optional"></a>Exportieren des selbstsignierten Stammzertifikats und des öffentlichen Schlüssels zum Speichern (optional)

Möglicherweise möchten Sie das selbstsignierte Stammzertifikat exportieren und sicher speichern. Bei Bedarf können Sie es später auf einem anderen Computer installieren und weitere Clientzertifikate generieren oder eine andere CER-Datei exportieren. Um das selbstsignierte Stammzertifikat als PFX-Datei zu exportieren, wählen Sie das Stammzertifikat aus, und verwenden Sie die gleichen Schritte wie zum [Exportieren eines Clientzertifikats](#clientexport).

## <a name="clientcert"></a>Generieren eines Clientzertifikats

Auf jedem Clientcomputer, der per Punkt-zu-Standort eine Verbindung mit einem VNet herstellt, muss ein Clientzertifikat installiert sein. Sie generieren ein Clientzertifikat aus dem selbstsignierten Stammzertifikat und exportieren und installieren es anschließend. Wenn das Clientzertifikat nicht installiert ist, tritt bei der Authentifizierung ein Fehler auf. 

Die folgenden Schritte führen Sie durch das Generieren eines Clientzertifikats aus einem selbstsignierten Stammzertifikat. Sie können mehrere Clientzertifikate aus demselben Stammzertifikat generieren. Wenn Sie mithilfe der folgenden Schritte Clientzertifikate generieren, wird das Clientzertifikat automatisch auf dem Computer installiert, mit dem Sie das Zertifikat generiert haben. Falls Sie ein Clientzertifikat auf einem anderen Clientcomputer installieren möchten, können Sie es exportieren.

Windows 10 wird benötigt, um Clientzertifikate mithilfe der folgenden PowerShell-Schritte zu generieren. Die in den folgenden Schritten verwendeten Cmdlets und Parameter sind Teil des Windows 10-Betriebssystems, nicht Teil einer PowerShell-Version. Das bedeutet nicht, dass die von Ihnen erstellten Zertifikate nur unter Windows 10 installiert werden können. Weitere Informationen zu unterstützten Clients finden Sie unter [Point-to-Site – Häufig gestellte Fragen](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq).

### <a name="example-1"></a>Beispiel 1

Dieses Beispiel verwendet die deklarierte Variable „$cert“ aus dem vorherigen Abschnitt. Wenn Sie die PowerShell-Konsole nach dem Erstellen des selbstsignierten Stammzertifikats geschlossen haben oder zusätzliche Clientzertifikate in einer neuen PowerShell-Konsolensitzung erstellen, verwenden Sie die Schritte in [Beispiel 2](#ex2).

Ändern Sie das Beispiel, und führen Sie es aus, um ein Clientzertifikat zu generieren. Wenn Sie das folgende Beispiel ausführen, ohne es zu ändern, ist das Ergebnis ein Clientzertifikat mit dem Namen P2SChildCert.  Wenn Sie dem untergeordneten Zertifikat einen anderen Namen geben möchten, ändern Sie den CN-Wert. Ändern Sie die TextExtension nicht, wenn Sie dieses Beispiel ausführen. Das Clientzertifikat, das Sie generieren, wird auf Ihrem Computer automatisch in „Certificates - Current User\Personal\Certificate“ installiert.

```powershell
New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <a name="ex2"></a>Beispiel 2

Wenn Sie zusätzliche Clientzertifikate erstellen oder nicht die gleiche PowerShell-Sitzung verwenden, in der Sie Ihr selbstsigniertes Stammzertifikat erstellt haben, führen Sie folgende Schritte aus:

1. Identifizieren Sie das selbstsignierte Stammzertifikat, das auf dem Computer installiert ist. Dieses Cmdlet gibt eine Liste von Zertifikaten zurück, die auf Ihrem Computer installiert sind.

  ```powershell
  Get-ChildItem -Path “Cert:\CurrentUser\My”
  ```
2. Suchen Sie den Antragstellernamen in der zurückgegebenen Liste, und kopieren Sie den Fingerabdruck, der sich daneben in einer Textdatei befindet. Im folgenden Beispiel sind zwei Zertifikate vorhanden. Der CN-Name ist der Name des selbstsignierten Stammzertifikats, aus dem Sie ein untergeordnetes Zertifikat generieren möchten. In diesem Fall P2SRootCert.

        Thumbprint                                Subject

        AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
        7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert


3. Deklarieren Sie eine Variable für das Stammzertifikat mit dem Fingerabdruck aus dem vorherigen Schritt. Ersetzen Sie THUMBPRINT mit dem Namen des Stammzertifikats, aus dem Sie ein untergeordnetes Zertifikat generieren möchten.

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
  ```

  Bei Verwendung des Fingerabdrucks für P2SRootCert im vorherigen Schritt sieht die Variable z.B. wie folgt aus:

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
  ```    

4.  Ändern Sie das Beispiel, und führen Sie es aus, um ein Clientzertifikat zu generieren. Wenn Sie das folgende Beispiel ausführen, ohne es zu ändern, ist das Ergebnis ein Clientzertifikat mit dem Namen P2SChildCert. Wenn Sie dem untergeordneten Zertifikat einen anderen Namen geben möchten, ändern Sie den CN-Wert. Ändern Sie die TextExtension nicht, wenn Sie dieses Beispiel ausführen. Das Clientzertifikat, das Sie generieren, wird auf Ihrem Computer automatisch in „Certificates - Current User\Personal\Certificate“ installiert.

  ```powershell
  New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <a name="clientexport"></a>Exportieren eines Clientzertifikats   

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]
    

## <a name="install"></a>Installieren eines exportierten Clientzertifikats

[!INCLUDE [Install client certificate](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Setzen Sie die Punkt-zu-Standort-Konfiguration fort. 

* Schritte für das Bereitstellungsmodell **Resource Manager** finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung mit einem VNet über das Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md). 
* Schritte für das **klassische** Bereitstellungsmodell finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung mit einem VNet über das Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md) (klassisch).
