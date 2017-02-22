---
title: "Häufig gestellte Fragen zu Rollen in Azure Cloud Services | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zu Azure Cloud Services. Hier finden Sie Antworten zu einigen häufig gestellten Fragen zu Zertifikaten, Web- und Workerrollen."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: 8dc7ea843ea316fa4659a8e6575adbfd045f7a70
ms.openlocfilehash: c169f9ab2eead732ad0fe5579caaa1b4b015732b


---
# <a name="cloud-services-faq"></a>Häufig gestellte Fragen zu Cloud Services
In diesem Artikel werden einige häufig gestellte Fragen zu Microsoft Azure Cloud Services beantwortet. Sie können auch [Häufig gestellte Fragen zum Azure-Support](http://go.microsoft.com/fwlink/?LinkID=185083) aufrufen, wenn Sie nach allgemeinen Informationen zu Azure-Preisen und zum Support suchen. Informationen zur Größe finden sie auch unter [Größen für Clouddienste](cloud-services-sizes-specs.md) .

## <a name="certificates"></a>Zertifikate
### <a name="where-should-i-install-my-certificate"></a>Wo soll ich mein Zertifikat installieren?
* **My**  
  Anwendungszertifikat mit privatem Schlüssel (\*.pfx, \*.p12).
* **CA**  
  Alle Zwischenzertifikate werden in diesem Speicher gespeichert (Richtlinien und Sub-Zertifizierungsstellen)
* **ROOT**  
  Ihre Hauptzertifikate der Stammzertifizierungsstelle werden hier im Speicher für Zertifikate der Stammzertifizierungsstelle gespeichert

### <a name="i-cant-remove-expired-certificate"></a>Ich kann ein abgelaufenes Zertifikat nicht entfernen.
Azure verhindert, dass Sie ein Zertifikat entfernen, während es verwendet wird. Sie müssen entweder die Bereitstellung löschen, die das Zertifikat verwendet, oder die Bereitstellung mit einem anderen oder einem erneuerten Zertifikat aktualisieren.

### <a name="delete-an-expired-certificate"></a>Löschen eines abgelaufenen Zertifikats
Solange das Zertifikat nicht verwendet wird, können Sie das PowerShell-Cmdlet [Remove-AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) zum Entfernen eines Zertifikats verwendet.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>Ich verfüge über abgelaufene Zertifikate namens „Windows Azure Service Management for Extensions“.
Diese Zertifikate werden erstellt, wenn dem Clouddienst eine Erweiterung hinzugefügt wird, wie etwa die Remotedesktoperweiterung. Diese Zertifikate werden nur zum Verschlüsseln und Entschlüsseln der privaten Konfiguration der Erweiterung verwendet. Es spielt keine Rolle, ob diese Zertifikate ablaufen. Das Ablaufdatum wird nicht geprüft.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>Zertifikate, die ich gelöscht haben, werden wieder angezeigt.
Das liegt wahrscheinlich an einem von Ihnen verwendeten Tool wie etwa Visual Studio. Jedes Mal, wenn Sie mit einem Tool, das ein Zertifikat verwendet, eine Verbindung herstellen, wird dieses Zertifikat erneut auf Azure hochgeladen.

### <a name="my-certificates-keep-disappearing"></a>Meine Zertifikate verschwinden.
Wenn die Instanz des virtuellen Computers zyklisch ausgeführt wird, gehen alle lokalen Änderungen verloren. Verwenden Sie eine [Startaufgabe](cloud-services-startup-tasks.md) , um Zertifikate bei jedem Start der Rolle auf dem virtuellen Computer zu installieren.

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a>Ich kann meine Verwaltungszertifikate im Portal nicht finden.
[Verwaltungszertifikate](../azure-api-management-certs.md) sind nur im klassischen Azure-Portal verfügbar. Das aktuelle Azure-Portal verwendet keine Verwaltungszertifikate. 

### <a name="how-can-i-disable-a-management-certificate"></a>Wie kann ich ein Verwaltungszertifikat deaktivieren?
[Verwaltungszertifikate](../azure-api-management-certs.md) können nicht deaktiviert werden. Sie löschen sie über das klassische Azure-Portal, wenn Sie nicht möchten, dass weiterhin verwendet werden.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Wie erstelle ich ein SSL-Zertifikat für eine bestimmte IP-Adresse?
Befolgen Sie die Anweisungen im [Tutorial zum Erstellen eines Zertifikats](cloud-services-certs-create.md). Verwenden Sie die IP-Adresse als DNS-Namen.

## <a name="security"></a>Sicherheit
### <a name="disable-ssl-30"></a>Deaktivieren von SSL 3.0
Um SSL 3.0 zu deaktivieren und TLS-Sicherheit zu verwenden, erstellen Sie einen Starttask, wie in diesem Blogbeitrag dokumentiert: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/.

## <a name="scale-a-cloud-service"></a>Skalieren eines Clouddiensts
### <a name="i-cannot-scale-beyond-x-instances"></a>Ich kann nicht über X Instanzen hinaus skalieren
Für Ihr Azure-Abonnement gilt ein Limit hinsichtlich der Anzahl von Kernen, die Sie verwenden können. Wenn Sie alle verfügbaren Kerne verwendet haben, können Sie nicht höher skalieren. Ein Beispiel: Wenn das Limit Ihres Abonnements bei 100 Kernen liegt, können Sie für Ihren Clouddienst 100 virtuelle Computerinstanzen der Größe A1 oder 50 virtuelle Computerinstanzen der Größe A2 einrichten.

## <a name="troubleshooting"></a>Problembehandlung
### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Ich kann keine IP in einem Clouddienst mit mehreren VIPs reservieren.
Stellen Sie zunächst sicher, dass die Instanz des virtuellen Computers, für den Sie die IP reservieren möchten, eingeschaltet ist. Vergewissern Sie sich anschließend, dass Sie reservierte IPs für Staging- und Produktionsbereitstellungen verwenden. **nicht** , während ein Upgrade der Bereitstellung durchgeführt wird.




<!--HONumber=Jan17_HO4-->


