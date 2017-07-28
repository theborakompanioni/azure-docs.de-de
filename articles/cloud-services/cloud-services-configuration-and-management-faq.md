---
title: "Probleme mit der Konfiguration und Verwaltung von Microsoft Azure Cloud Services – Häufig gestellte Fragen | Microsoft-Dokumentation"
description: "Dieser Artikel behandelt häufig gestellte Fragen zur Konfiguration und Verwaltung von Microsoft Azure Cloud Services."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 7d7676be26a8b68ab9fda18388e2b8cd5ed5f60b
ms.contentlocale: de-de
ms.lasthandoff: 06/15/2017


---
# <a name="configuration-and-management-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Probleme mit der Konfiguration und Verwaltung von Microsoft Azure Cloud Services – Häufig gestellte Fragen (FAQs)

Dieser Artikel enthält häufig gestellte Fragen zur Konfiguration und Verwaltung von [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Informationen zur Größe finden sie auch unter [Größen für Clouddienste](cloud-services-sizes-specs.md) .

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="how-do-i-add-nosniff-to-my-website"></a>Wie füge ich meiner Website „nosniff“ hinzu?
Um zu verhindern, dass Clients die MIME-Typen abfangen, fügen Sie eine Einstellung in Ihre Datei *web.config* ein.

```xml
<configuration>
   <system.webServer>
      <httpProtocol>
         <customHeaders>
            <add name="X-Content-Type-Options" value="nosniff" />
         </customHeaders>
      </httpProtocol>
   </system.webServer>
</configuration>
```

Sie können dies auch als Einstellung in IIS hinzufügen. Verwenden Sie den folgenden Befehl und den Artikel [Gängige Starttasks](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe).

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

## <a name="how-do-i-customize-iis-for-a-web-role"></a>Wie passe ich IIS für eine Webrolle an?
Verwenden Sie das IIS-Startskript aus dem Artikel [Gängige Starttasks](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe).

## <a name="i-cannot-scale-beyond-x-instances"></a>Ich kann nicht über X Instanzen hinaus skalieren
Für Ihr Azure-Abonnement gilt ein Limit hinsichtlich der Anzahl von Kernen, die Sie verwenden können. Wenn Sie alle verfügbaren Kerne verwendet haben, können Sie nicht höher skalieren. Ein Beispiel: Wenn das Limit Ihres Abonnements bei 100 Kernen liegt, können Sie für Ihren Clouddienst 100 virtuelle Computerinstanzen der Größe A1 oder 50 virtuelle Computerinstanzen der Größe A2 einrichten.

## <a name="how-can-i-implement-role-based-access-for-cloud-services"></a>Wie implementiere ich rollenbasierten Zugriff für Cloud Services?
Das rollenbasierte Zugriffsmodell (Role-Based Access Control, RBAC) wird von Cloud Services nicht unterstützt, da es sich nicht um einen Azure Resource Manager-basierten Dienst handelt.

Weitere Informationen finden Sie unter [Azure RBAC im Vergleich zu klassischen Administratoren für Abonnements](../active-directory/role-based-access-control-what-is.md#azure-rbac-vs-classic-subscription-administrators).

## <a name="how-do-i-set-the-idle-timeout-for-azure-load-balancer"></a>Wie richte ich das Leerlauftimeout für Azure Load Balancer ein?
Sie können das Timeout in der Dienstdefinitionsdatei (CSDEF) wie folgt angeben:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="mgVS2015Worker" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10100"   idleTimeoutInMinutes="30" />
    </Endpoints>
  </WorkerRole>
```
Weitere Informationen finden Sie unter [New: Configurable Idle Timeout for Azure Load Balancer (Neu: Konfigurierbares Leerlauftimeout für Azure Load Balancer)](https://azure.microsoft.com/blog/new-configurable-idle-timeout-for-azure-load-balancer/).

## <a name="can-microsoft-internal-engineers-rdp-to-cloud-service-instances-without-permission"></a>Können interne Microsoft-Techniker ohne Berechtigung eine RDP-Verbindung zu Clouddienstinstanzen herstellen?
Microsoft folgt einem strengen Kurs, der internen Technikern nicht erlaubt, eine RDP-Verbindung zu einem Clouddienst ohne schriftliche Einverständnis (E-Mail oder schriftliche Kommunikation) des Besitzers oder Bevollmächtigten herzustellen.

## <a name="why-is-the-certificate-chain-of-my-cloud-service-ssl-certificate-incomplete"></a>Warum ist die Zertifikatskette meines Clouddienst-SSL-Zertifikats unvollständig?
Wir empfehlen, dass Kunden die vollständige Zertifikatskette (untergeordnetes Zertifikat, Zwischenzertifikate und Stammzertifikate) installieren und nicht nur das untergeordnete Zertifikat. Wenn Sie nur das untergeordnete Zertifikat installieren, sind Sie davon abhängig, dass Windows die Zertifikatskette durch durchlaufen der CTL erstellt. Wenn zeitweilige Netzwerk- oder DNS-Probleme in Azure oder Windows Update auftreten, während Windows das Zertifikat überprüft, wird das Zertifikat möglicherweise als ungültig behandelt. Durch Installation der vollständigen Zertifikatskette kann dieses Problem vermieden werden. Der Blogbeitrag [How to install a chained SSL certificate (Installieren eines SSL-Kettenzertifikats)](https://blogs.msdn.microsoft.com/azuredevsupport/2010/02/24/how-to-install-a-chained-ssl-certificate/) veranschaulicht diese Vorgehensweise.

## <a name="how-do-i-associate-a-static-ip-address-to-my-cloud-service"></a>Wie verknüpfe ich eine statische IP-Adresse mit meinem Clouddienst?
Sie müssen für das Erstellen einer reservierten IP-Adresse eine statische IP-Adresse festlegen. Diese reservierte IP-Adresse kann einem neuen Clouddienst oder einer vorhandenen Bereitstellung zugeordnet werden. Weitere Informationen finden Sie in den folgenden Dokumenten:
* [Verwalten von reservierten IPs](../virtual-network/virtual-networks-reserved-public-ip.md#manage-reserved-vips)
* [Reservieren der IP-Adresse eines vorhandenen Clouddiensts](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
* [Zuordnen einer reservierten IP zu einem neuen Clouddienst](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-new-cloud-service)
* [Zuordnen einer reservierten IP zu einer aktiven Bereitstellung](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-running-deployment)
* [Zuordnen einer reservierten IP zu einem Clouddienst mit einer Dienstkonfigurationsdatei](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file)

## <a name="what-is-the-quota-limit-for-my-cloud-service"></a>Wie hoch ist die Kontingentgrenze für meinen Clouddienst?
Weitere Informationen finden Sie unter [Dienstspezifische Grenzwerte](../azure-subscription-service-limits.md#subscription-limits).

## <a name="why-does-the-drive-on-my-cloud-service-vm-show-very-little-free-disk-space"></a>Warum wird auf dem Laufwerk meines virtuellen Clouddienstcomputers nur sehr wenig freier Speicherplatz auf dem Datenträger angezeigt?
Hierbei handelt es sich um erwartetes Verhalten, das kein Problem bei Ihrer Anwendung verursachen sollte. Für das Laufwerk %aproot% auf virtuellen Azure PaaS-Computern ist Journaling aktiviert, was ungefähr doppelt so viel Speicherplatz verbraucht wie Dateien normalerweise belegen. Es gibt jedoch einige Punkte zu beachten, durch die dieses Verhalten nicht zu einem Problem wird.

Die Größe des Laufwerks %aproot% beträgt 1,5 GB oder wird als <Größe von CSPKG + max. Journalgröße + Rand mit freiem Speicherplatz> berechnet, je nachdem welcher Wert größer ist. Die Größe Ihres virtuellen Computers hat keinen Einfluss auf diese Berechnung. (Die Größe des virtuellen Computers betrifft nur die Größe des temporären Laufwerks „C:“) 

Schreibvorgänge in das Laufwerk %aproot% werden nicht unterstützt. Wenn Sie einen Schreibvorgang in den virtuellen Azure-Computer vornehmen, müssen Sie diesen in einer temporären LocalStorage-Ressource (oder einer anderen Option, z.B. Blob-Speicher, Azure Storage usw.) ausführen. Der freie Speicherplatz im Ordner %approot% ist also nicht von Bedeutung. Wenn Sie sich unsicher sind, ob Ihre Anwendung in das Laufwerk %aproot% schreibt, können Sie Ihren Dienst immer ein paar Tage lang ausführen lassen und anschließend die „Vorher/Nachher“-Größe vergleichen. 

Azure wird nichts in das Laufwerk %approot% schreiben. Sobald die VHD-Datei aus CSPKG erstellt und in den virtuellen Azure-Computer eingebunden wurde, kann nur noch die Anwendung in dieses Laufwerk schreiben. 

Die Einstellungen für Journaling sind nicht konfigurierbar, also lässt es sich nicht deaktiveren.

## <a name="what-are-the-features-and-capabilities-that-azure-basic-ipsids-and-ddos-provides"></a>Welche Features und Funktionen bieten Azure Basic-IPS/IDS und DDoS?
Azure verfügt über IPS/IDS in physischen Servern des Datencenters, um Bedrohungen abzuwehren. Darüber hinaus können Kunden Sicherheitslösungen von Drittanbietern bereitstellen, z.B. Web Application Firewalls, Netzwerkfirewalls, Antischadsoftware, Angriffserkennungs- und Eindringschutzsysteme (IDS/IPS) und weitere. Weitere Informationen finden Sie unter [Protect your data and assets and comply with global security standards (Schutz Ihrer Daten und Objekte in Übereinstimmung mit globalen Sicherheitsstandards)](https://www.microsoft.com/en-us/trustcenter/Security/AzureSecurity).

Microsoft überwacht durchgehend Server, Netzwerke und Anwendungen, um Bedrohungen zu erkennen. Azure's mehrgleisiger Ansatz zur Verwaltung von Bedrohungen verwendet Angriffserkennung, Verhinderung von verteilten Denial-of-Service-Angriffen (DDoS), Penetrationstests, Verhaltensanalyse, Anomalieerkennung und Machine Learning, um den Schutz stetig zu verbessern und Risiken zu reduzieren. Microsoft Antimalware für Azure schützt Azure Cloud Services und virtuelle Computer. Darüber hinaus können Sie zusätzlich Sicherheitslösungen von Drittanbietern bereitstellen, z.B. Web Application Firewalls, Netzwerkfirewalls, Antischadsoftware, Angriffserkennungs- und Eindringschutzsysteme (IDS/IPS) und weitere.

## <a name="why-does-iis-stop-writing-to-the-log-directory"></a>Warum beendet IIS das Schreiben in das Protokollverzeichnis?
Sie haben das lokale Speicherkontingent für das Schreiben in das Protokollverzeichnis ausgeschöpft. Es gibt drei Möglichkeiten, dies zu korrigieren:
* Aktivieren Sie die Diagnose für IIS und lassen Sie die Diagnose regelmäßig in einen Blob-Speicher verschieben.
* Entfernen Sie Protokolldateien manuell aus dem Protokollierungsverzeichnis.
* Erhöhen Sie die Kontingentgrenze für lokale Ressourcen.

Weitere Informationen finden Sie in den folgenden Dokumenten:
* [Speichern und Anzeigen von Diagnosedaten in Azure Storage](cloud-services-dotnet-diagnostics-storage.md)
* [IIS-Protokolle werden nicht mehr in Cloud Services geschrieben](https://blogs.msdn.microsoft.com/cie/2013/12/21/iis-logs-stops-writing-in-cloud-service/)

## <a name="what-is-the-purpose-of-the-windows-azure-tools-encryption-certificate-for-extensions"></a>Welchen Zweck hat das „Windows Azure Tools Encryption Certificate for Extensions“ (Windows Azure Tools-Verschlüsselungszertifikat für Erweiterungen)?
Diese Zertifikate werden automatisch erstellt, wenn dem Clouddienst eine Erweiterung hinzugefügt wird. In den meisten Fällen handelt es sich hierbei um die WAD- oder RDP-Erweiterung. Es kann sich aber auch um die Antimalware- oder Log Collector-Erweiterung handeln. Diese Zertifikate werden nur zum Verschlüsseln und Entschlüsseln der privaten Konfiguration der Erweiterung verwendet. Das Ablaufdatum wird nicht überprüft, sodass es keine Rolle spielt, wenn das Zertifikat abgelaufen ist. 

Sie können diese Zertifikate ignorieren. Wenn Sie die Zertifikate bereinigen möchten, können Sie versuchen, sie alle zu löschen. Azure löst einen Fehler aus, wenn Sie versuchen ein Zertifikat zu löschen, das verwendet wird.

## <a name="how-can-i-generate-a-certificate-signing-request-csr-without-rdp-ing-in-to-the-instance"></a>Wie kann ich eine Zertifikatsignieranforderung (Certficate Signing Request, CSR) generieren, ohne eine RDP-Verbindung mit der Instanz herzustellen?
Weitere Informationen finden Sie im folgenden Richtliniendokument:

>[Obtaining a certificate for use with Windows Azure Web Sites (WAWS) (Abrufen eines Zertifikats für die Verwendung in Windows Azure-Websites (WAWS))](https://azure.microsoft.com/blog/obtaining-a-certificate-for-use-with-windows-azure-web-sites-waws/)

Beachten Sie, dass es sich bei CSR nur um eine Textdatei handelt. Sie muss NICHT von dem Computer aus erstellt werden, auf dem das Zertifikat letztlich verwendet wird. Obwohl dieses Dokument für App Service geschrieben ist, ist das Erstellen einer CSR generisch und gilt auch für Cloud Services.

