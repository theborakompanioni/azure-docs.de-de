---
title: "Erstellen selbstsignierter Zertifikate für Punkt-zu-Standort-Verbindungen: PowerShell: Azure | Microsoft-Dokumentation"
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
ms.date: 04/24/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: 72fc6eb93c77dd5a0a7ce55897f4c06fb98c0721
ms.lasthandoff: 04/25/2017


---
# <a name="create-a-self-signed-root-certificate-for-point-to-site-connections-using-powershell"></a>Erstellen eines selbstsignierten Stammzertifikats für Punkt-zu-Standort-Verbindungen mit PowerShell

Punkt-zu-Standort-Verbindungen verwenden Zertifikate zur Authentifizierung. Wenn Sie eine Punkt-zu-Standort-Verbindung konfigurieren, müssen Sie den öffentlichen Schlüssel (CER-Datei) eines Stammzertifikats in Azure hochladen. Clientzertifikate müssen auf der Grundlage des Stammzertifikats generiert und auf jedem Clientcomputer installiert werden, der eine Verbindung mit dem VNET herstellt. Über das Clientzertifikat kann sich der Client authentifizieren. In diesem Artikel erfahren Sie, wie Sie ein selbstsigniertes Stammzertifikat erstellen, den öffentlichen Schlüssel exportieren und Clientzertifikate generieren. Der Artikel enthält keine Konfigurationsanleitung für Point-to-Site-Verbindungen und auch keine häufig gestellten Fragen dazu. Diese Informationen finden Sie in einem der folgenden Artikel:

> [!div class="op_single_selector"]
> * [Erstellen selbstsignierter Zertifikate](vpn-gateway-certificates-point-to-site.md)
> * [Konfigurieren einer Point-to-Site-Verbindung – Resource Manager – Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Konfigurieren einer Point-to-Site-Verbindung – Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Konfigurieren einer Point-to-Site-Verbindung – klassisch – Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

## <a name="rootcert"></a>Erstellen eines selbstsignierten Stammzertifikats

Die folgenden Schritte führen Sie durch die Erstellung eines selbstsignierten Stammzertifikats mit PowerShell unter Windows 10. Die in den folgenden Schritten verwendeten Cmdlets und Parameter sind Teil des Windows 10-Betriebssystems, nicht Teil einer PowerShell-Version. Das bedeutet nicht, dass die von Ihnen erstellten Zertifikate nur unter Windows 10 installiert werden können. Weitere Informationen zu unterstützten Clients finden Sie unter [Point-to-Site – Häufig gestellte Fragen](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq).

1. Öffnen Sie auf einem Computer unter Windows 10 eine Windows PowerShell-Konsole mit erhöhten Rechten.
2. Verwenden Sie das folgende Beispiel, um das selbstsignierte Stammzertifikat zu erstellen. Das folgende Beispiel erstellt ein selbstsigniertes Stammzertifikat mit dem Namen P2SRootCert, das automatisch in „Certificates-Current User\Personal\Certificates“ installiert wird. Um das Zertifikat anzuzeigen, öffnen Sie *certmgr.msc*.

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

### <a name="cer"></a>So erhalten Sie den öffentlichen Schlüssel

Für Punkt-zu-Standort-Verbindungen muss der öffentliche Schlüssel (CER-Format) in Azure hochgeladen werden. Führen Sie die folgenden Schritte aus, um die CER-Datei für Ihr selbstsigniertes Stammzertifikat zu exportieren:

1. Öffnen Sie **Benutzerzertifikate verwalten**, um eine CER-Datei für das Zertifikat zu erhalten. Suchen Sie das selbstsignierte Stammzertifikat (in der Regel in „Zertifikate – Aktueller Benutzer\Eigene Zertifikate\Zertifikate“), und klicken Sie mit der rechten Maustaste darauf. Klicken Sie auf **Alle Aufgaben** und anschließend auf **Exportieren**. Dadurch wird der **Zertifikatexport-Assistent**geöffnet.
2. Klicken Sie im Assistenten auf **Weiter**. Wählen Sie **Nein, privaten Schlüssel nicht exportieren** aus, und klicken Sie dann auf **Weiter**.
3. Wählen Sie auf der Seite **Dateiformat für den Export** die Option **Base-64-codiert X.509 (.CER)** aus, und klicken Sie dann auf **Weiter**. 
4. Wählen Sie unter **Zu exportierende Datei** die Option **Durchsuchen** aus, um zu dem Speicherort zu wechseln, an den das Zertifikat exportiert werden soll. Geben Sie unter **Dateiname**einen Namen für die Zertifikatdatei ein. Klicken Sie auf **Weiter**.
5. Klicken Sie auf **Fertig stellen** , um das Zertifikat zu exportieren. Die Meldung **Der Export war erfolgreich** wird angezeigt. Klicken Sie auf **OK**, um den Assistenten zu schließen.

### <a name="to-export-a-self-signed-root-certificate-optional"></a>So exportieren Sie ein selbstsigniertes Stammzertifikat (optional)

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

Wenn Sie ein Clientzertifikat generieren, wird es automatisch auf dem Computer installiert, mit dem Sie es generiert haben. Falls Sie das Clientzertifikat auf einem anderen Clientcomputer installieren möchten, müssen Sie das Clientzertifikat exportieren, das Sie generiert haben.                              

1. Wählen Sie zum Exportieren eines Clientzertifikats die Option **Benutzerzertifikate verwalten** aus. Die Clientzertifikate, die Sie generiert haben, befinden sich standardmäßig in „Certificates - Current User\Personal\Certificates“. Klicken Sie mit der rechten Maustaste auf das Clientzertifikat, das Sie exportieren möchten, und klicken Sie dann auf **Alle Aufgaben** und anschließend auf **Exportieren**, um den **Zertifikatexport-Assistenten** zu öffnen.
2. Klicken Sie im Assistenten auf **Weiter**, wählen Sie **Ja, privaten Schlüssel exportieren** aus, und klicken Sie dann auf **Weiter**.
3. Übernehmen Sie auf der Seite **Format der zu exportierenden Datei** die Standardwerte. Stellen Sie sicher, dass die Option **Wenn möglich, alle Zertifikate im Zertifizierungspfad einbeziehen** aktiviert ist. Bei Auswahl dieser Option werden auch die Stammzertifikatinformationen exportiert, die für eine erfolgreiche Authentifizierung erforderlich sind. Klicken Sie auf **Weiter**.
4. Auf der Seite **Sicherheit** müssen Sie den privaten Schlüssel schützen. Wenn Sie ein Kennwort verwenden möchten, müssen Sie sich das für dieses Zertifikat festgelegte Kennwort unbedingt merken oder notieren. Klicken Sie auf **Weiter**.
5. Wählen Sie unter **Zu exportierende Datei** die Option **Durchsuchen** aus, um zu dem Speicherort zu wechseln, an den das Zertifikat exportiert werden soll. Geben Sie unter **Dateiname**einen Namen für die Zertifikatdatei ein. Klicken Sie auf **Weiter**.
6. Klicken Sie auf **Fertig stellen** , um das Zertifikat zu exportieren.    

## <a name="install"></a>Installieren eines exportierten Clientzertifikats

Wenn Sie eine P2S-Verbindung mit einem anderen Clientcomputer als dem für die Generierung der Clientzertifikate verwendeten Computer herstellen möchten, müssen Sie ein Clientzertifikat installieren. Beim Installieren eines Clientzertifikats benötigen Sie das Kennwort, das beim Exportieren des Clientzertifikats erstellt wurde.

1. Suchen Sie die *PFX* -Datei, und kopieren Sie sie auf den Clientcomputer. Doppelklicken Sie auf dem Clientcomputer auf die *PFX* -Datei, um sie zu installieren. Behalten Sie für **Speicherort** die Einstellung **Aktueller Benutzer** bei, und klicken Sie auf **Weiter**.
2. Nehmen Sie auf der Seite **Zu importierende Datei** keine Änderungen vor. Klicken Sie auf **Weiter**.
3. Geben Sie auf der Seite **Schutz für den privaten Schlüssel** das Kennwort für das Zertifikat ein, oder überprüfen Sie die Korrektheit des Sicherheitsprinzipals, und klicken Sie anschließend auf **Weiter**.
4. Behalten Sie auf der Seite **Zertifikatspeicher** den Standardspeicherort bei, und klicken Sie dann auf **Weiter**.
5. Klicken Sie auf **Fertig stellen**. Klicken Sie in der **Sicherheitswarnung** für die Zertifikatinstallation auf **Ja**. Sie können beruhigt auf „Ja“ klicken, da Sie das Zertifikat generiert haben. Das Zertifikat wird nun erfolgreich importiert.

## <a name="next-steps"></a>Nächste Schritte

Setzen Sie die Punkt-zu-Standort-Konfiguration fort. 

* Schritte für das Bereitstellungsmodell **Resource Manager** finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung mit einem VNet über das Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md). 
* Schritte für das **klassische** Bereitstellungsmodell finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung mit einem VNet über das Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md) (klassisch).
