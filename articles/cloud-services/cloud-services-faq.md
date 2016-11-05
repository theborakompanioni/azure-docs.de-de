---
title: Häufig gestellte Fragen zu Cloud Services | Microsoft Docs
description: Häufig gestellte Fragen zu Cloud Services
services: cloud-services
documentationcenter: ''
author: Thraka
manager: timlt
editor: ''

ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: adegeo

---
# Häufig gestellte Fragen zu Cloud Services
In diesem Artikel werden einige häufig gestellte Fragen zu Microsoft Azure Cloud Services beantwortet. Sie können auch [Häufig gestellte Fragen zum Azure-Support](http://go.microsoft.com/fwlink/?LinkID=185083) aufrufen, wenn Sie nach allgemeinen Informationen zu Azure-Preisen und zum Support suchen. Informationen zur Größe finden sie auch unter [Größen für Clouddienste](cloud-services-sizes-specs.md).

## Zertifikate
### Wo soll ich mein Zertifikat installieren?
* **My** Anwendungszertifikat mit privatem Schlüssel (*.pfx, *.p12)
* **CA** Alle Zwischenzertifikate werden in diesem Speicher gespeichert (Richtlinien und Sub-Zertifizierungsstellen)
* **ROOT** Ihre Hauptzertifikate der Stammzertifizierungsstelle werden hier im Speicher für Zertifikate der Stammzertifizierungsstelle gespeichert

### Ich kann ein abgelaufenes Zertifikat nicht entfernen.
Azure verhindert, dass Sie ein Zertifikat entfernen, während es verwendet wird. Sie müssen entweder die Bereitstellung löschen, die das Zertifikat verwendet, oder die Bereitstellung mit einem anderen oder einem erneuerten Zertifikat aktualisieren.

### Löschen eines abgelaufenen Zertifikats
Solange das Zertifikat nicht verwendet wird, können Sie das PowerShell-Cmdlet [Remove-AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) zum Entfernen eines Zertifikats verwendet.

### Ich verfüge über abgelaufene Zertifikate namens „Windows Azure Service Management for Extensions“.
Diese Zertifikate werden erstellt, wenn dem Clouddienst eine Erweiterung hinzugefügt wird, wie etwa die Remotedesktoperweiterung. Diese Zertifikate werden nur zum Verschlüsseln und Entschlüsseln der privaten Konfiguration der Erweiterung verwendet. Es spielt keine Rolle, ob diese Zertifikate ablaufen. Das Ablaufdatum wird nicht geprüft.

### Zertifikate, die ich gelöscht haben, werden wieder angezeigt.
Das liegt wahrscheinlich an einem von Ihnen verwendeten Tool wie etwa Visual Studio. Jedes Mal, wenn Sie mit einem Tool, das ein Zertifikat verwendet, eine Verbindung herstellen, wird dieses Zertifikat erneut auf Azure hochgeladen.

### Meine Zertifikate verschwinden.
Wenn die Instanz des virtuellen Computers zyklisch ausgeführt wird, gehen alle lokalen Änderungen verloren. Verwenden Sie eine [Startaufgabe](cloud-services-startup-tasks.md), um Zertifikate bei jedem Start der Rolle auf dem virtuellen Computer zu installieren.

### Ich kann meine Verwaltungszertifikate im Portal nicht finden.
[Verwaltungszertifikate](../azure-api-management-certs.md) sind nur im klassischen Azure-Portal verfügbar. Das aktuelle Azure-Portal verwendet keine Verwaltungszertifikate.

### Wie kann ich ein Verwaltungszertifikat deaktivieren?
[Verwaltungszertifikate](../azure-api-management-certs.md) können nicht deaktiviert werden. Sie löschen sie über das klassische Azure-Portal, wenn Sie nicht möchten, dass weiterhin verwendet werden.

### Wie erstelle ich ein SSL-Zertifikat für eine bestimmte IP-Adresse?
Befolgen Sie die Anweisungen im [Tutorial zum Erstellen eines Zertifikats](cloud-services-certs-create.md). Verwenden Sie die IP-Adresse als DNS-Namen.

## Problembehandlung
### Ich kann keine IP in einem Clouddienst mit mehreren VIPs reservieren.
Stellen Sie zunächst sicher, dass die Instanz des virtuellen Computers, für den Sie die IP reservieren möchten, eingeschaltet ist. Vergewissern Sie sich anschließend, dass Sie reservierte IPs für Staging- und Produktionsbereitstellungen verwenden. Ändern Sie die Einstellungen **nicht**, während ein Upgrade der Bereitstellung durchgeführt wird.

<!---HONumber=AcomDC_0914_2016-->