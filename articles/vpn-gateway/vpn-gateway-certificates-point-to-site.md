---
title: "Generieren und Exportieren von Zertifikaten für Punkt-zu-Site-Verbindungen: PowerShell: Azure | Microsoft-Dokumentation"
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
ms.date: 05/23/2017
ms.author: cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: a7594c37a5d8b92144a1984d58ededd04927d189
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017


---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-powershell-on-windows-10"></a>Generieren und Exportieren von Zertifikaten für Punkt-zu-Site-Verbindungen mithilfe von PowerShell unter Windows 10

Punkt-zu-Standort-Verbindungen verwenden Zertifikate zur Authentifizierung. In diesem Artikel wird beschrieben, wie Sie mithilfe von PowerShell unter Windows 10 ein selbstsigniertes Stammzertifikat erstellen und Clientzertifikate generieren. Wenn Sie Informationen zu einzelnen Schritten für die Punkt-zu-Site-Konfiguration benötigen, z.B. Hochladen von Stammzertifikaten, wählen Sie in der folgenden Liste einen der Artikel zum Konfigurieren einer Punkt-zu-Site-Verbindung aus.

> [!div class="op_single_selector"]
> * [Erstellen selbstsignierter Zertifikate – PowerShell](vpn-gateway-certificates-point-to-site.md)
> * [Erstellen selbstsignierter Zertifikate – MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)
> * [Konfigurieren einer Punkt-zu-Standort-Verbindung – Resource Manager – Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Konfigurieren einer Point-to-Site-Verbindung – Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Konfigurieren einer Punkt-zu-Standort-Verbindung – klassisch – Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 


Sie müssen die in diesem Artikel beschriebenen Schritte auf einem Computer unter Windows 10 ausführen. Die zum Generieren von Zertifikaten verwendeten PowerShell-Cmdlets sind Teil des Windows 10-Betriebssystems und können in anderen Versionen von Windows nicht ausgeführt werden. Der Windows 10-Computer wird lediglich zum Generieren der Zertifikate benötigt. Nachdem die Zertifikate generiert wurden, können sie hochgeladen oder unter jedem unterstützten Clientbetriebssystem installiert werden. 

Wenn Sie keinen Zugriff auf einen Windows 10-Computer haben, können Sie Zertifikate mit [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) generieren. Mit MakeCert können jedoch keine SHA-2-Zertifikate, sondern lediglich SHA-1-Zertifikate generiert werden. SHA-1-Zertifikate sind für Punkt-zu-Site-Verbindungen noch gültig, SHA-1 verwendet jedoch einen Verschlüsselungshash, der nicht so stark ist wie SHA-2. Aus diesem Grund empfehlen wir die Ausführung dieser PowerShell-Schritte, sofern möglich. Unabhängig davon, mit welcher Methode sie generiert wurden, können die Zertifikate unter jedem [unterstützten](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) Clientbetriebssystem installiert werden.

## <a name="rootcert"></a>Erstellen eines selbstsignierten Stammzertifikats

Verwenden Sie das Cmdlet „New-SelfSignedCertificate“, um ein selbstsigniertes Stammzertifikat zu erstellen. Zusätzliche Informationen zu den Parametern finden Sie in der Beschreibung zu [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

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

Die exportierte CER-Datei muss in Azure hochgeladen werden. Entsprechende Anweisungen finden Sie unter [Konfigurieren einer Punkt-zu-Standort-Verbindung](vpn-gateway-howto-point-to-site-rm-ps.md#upload). Informationen zum Hinzufügen eines zusätzlichen vertrauenswürdigen Stammzertifikats finden Sie in [diesem Abschnitt](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) des Artikels.

### <a name="export-the-self-signed-root-certificate-and-public-key-to-store-it-optional"></a>Exportieren des selbstsignierten Stammzertifikats und des öffentlichen Schlüssels zum Speichern (optional)

Möglicherweise möchten Sie das selbstsignierte Stammzertifikat exportieren und sicher speichern. Bei Bedarf können Sie es später auf einem anderen Computer installieren und weitere Clientzertifikate generieren oder eine andere CER-Datei exportieren. Um das selbstsignierte Stammzertifikat als PFX-Datei zu exportieren, wählen Sie das Stammzertifikat aus, und verwenden Sie die gleichen Schritte wie zum [Exportieren eines Clientzertifikats](#clientexport).

## <a name="clientcert"></a>Generieren eines Clientzertifikats

Auf jedem Clientcomputer, der per Punkt-zu-Standort eine Verbindung mit einem VNet herstellt, muss ein Clientzertifikat installiert sein. Sie generieren ein Clientzertifikat aus dem selbstsignierten Stammzertifikat und exportieren und installieren es anschließend. Wenn das Clientzertifikat nicht installiert ist, tritt bei der Authentifizierung ein Fehler auf. 

Die folgenden Schritte führen Sie durch das Generieren eines Clientzertifikats aus einem selbstsignierten Stammzertifikat. Sie können mehrere Clientzertifikate aus demselben Stammzertifikat generieren. Wenn Sie mithilfe der folgenden Schritte Clientzertifikate generieren, wird das Clientzertifikat automatisch auf dem Computer installiert, mit dem Sie das Zertifikat generiert haben. Falls Sie ein Clientzertifikat auf einem anderen Clientcomputer installieren möchten, können Sie es exportieren.

In den Beispielen wird das Cmdlet „New-SelfSignedCertificate“ zum Generieren eines Clientzertifikats verwendet, das in einem Jahr abläuft. Zusätzliche Informationen zu den Parametern, z.B. Festlegen eines anderen Ablaufwerts für das Clientzertifikat, finden Sie in der Beschreibung zu [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

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

  ```
  Thumbprint                                Subject
  
  AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
  7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert
  ```
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
